# Frontend Diagnostics

[Back to the Scientific Method](README.md)

The frontend is where users actually feel problems, and it's the layer with the most uncontrolled variables: browsers, devices, network conditions, extensions, screen readers, and user behavior. Good frontend diagnosis is mostly about pinning those variables down.

## Typical symptoms

- Slow first load, or poor Core Web Vitals (LCP, INP, CLS)
- Janky scrolling, typing lag, frozen UI
- Works in one browser or device but not another
- The UI shows stale or wrong data after a user action
- Memory climbs the longer the tab stays open
- A screen reader or keyboard user can't complete a flow
- Errors that show up in error tracking but nobody can reproduce

## Observation tooling

| Need | Tools |
|---|---|
| Network timing, payloads, caching headers | Browser DevTools **Network** panel (throttling, disable cache, HAR export) |
| Rendering, scripting, and layout cost | DevTools **Performance** panel (flame chart, long tasks, layout shifts); Firefox Profiler |
| Page-level audits | **Lighthouse** (DevTools or Lighthouse CI), **PageSpeed Insights** (lab + field CrUX data), **WebPageTest** (filmstrip, multi-location, repeat view) |
| Real-user metrics | The `web-vitals` library feeding your analytics or RUM tool (Sentry, Datadog RUM, New Relic Browser, and similar) |
| Memory leaks | DevTools **Memory** panel: heap snapshots, allocation timelines, detached DOM nodes |
| Framework state and change detection | **Angular DevTools** (component tree, change-detection profiler), React DevTools Profiler, Vue DevTools |
| Bundle composition | `source-map-explorer`, `webpack-bundle-analyzer`, `ng build --stats-json` with an analyzer, Vite/Rollup visualizer |
| Errors in the wild | Sentry or similar, with **source maps** uploaded so stack traces point at real code |
| Accessibility | **axe DevTools** / `axe-core`, Lighthouse accessibility audit, WAVE, and a real screen reader (NVDA, JAWS, VoiceOver) |
| Cross-browser and device | Playwright (Chromium, Firefox, WebKit), BrowserStack or Sauce Labs, real devices over remote debugging |
| Reproducible user flows | Playwright or Cypress scripts, Playwright **trace viewer**, session replay tools (with privacy masking) |

## Common hypotheses and how to tell them apart

| Symptom | Candidate hypotheses | Discriminating experiment |
|---|---|---|
| Slow LCP | (a) Slow server response (TTFB) (b) Render-blocking JS/CSS (c) Large, unoptimized hero image (d) Hero image discovered late | Read the Network waterfall. If TTFB is most of LCP, it's (a); go to the [backend playbook](backend.md). Otherwise check when the LCP resource *starts* downloading. A late start points to (d), a long download to (c). Block the suspect script in DevTools and re-measure for (b). |
| Poor INP / typing lag | (a) Long task in an event handler (b) Excessive re-rendering or change detection (c) Third-party script on the main thread | Record in the Performance panel while reproducing, then find the long task under the interaction. Profile with Angular or React DevTools to count renders for (b). Block third-party domains and re-test for (c). |
| Layout shift (CLS) | (a) Images without dimensions (b) Late-injected banners or ads (c) Web font swap | Enable the Layout Shift Regions overlay and see which element moves. Reserve space for each suspect in turn. |
| Stale data after a save | (a) HTTP cache serving an old response (b) Client-side cache or store not invalidated (c) Race: an older response arrives after a newer one (d) Backend eventual consistency | Check Network for `(disk cache)` / `304` for (a). Log store state transitions for (b). Add artificial latency to one request with DevTools request blocking or a proxy, then look for out-of-order resolution for (c). Query the backend directly after the save for (d). |
| Memory grows over time | (a) Unremoved event listeners or subscriptions (b) Detached DOM kept alive by references (c) Unbounded client cache | Take heap snapshot → repeat the action 10 times → take another snapshot → compare. Filter by "Detached" for (b). Look at retainer chains to find the owning component. |
| Fails only in Safari | (a) Unsupported API or CSS feature (b) Date parsing differences (c) Stricter cookie or storage policy | Run the flow in Playwright WebKit with the console open. Check the API on caniuse / MDN compatibility tables. Log the parsed values of date strings. |
| Screen reader can't complete checkout | (a) Custom control missing role or name (b) Focus lost after a dynamic update (c) Error messages not announced | Run axe for (a). Tab through with the keyboard only and watch `document.activeElement` for (b). Use NVDA or VoiceOver to hear what's announced on error for (c). |

## Worked example: LCP regression after a release

1. **Observe.** RUM shows mobile p75 LCP rose from 2.1 s to 3.8 s starting the day of release 4.12. Desktop barely changed. Error rates are flat.
2. **Question.** Why did mobile p75 LCP on the product page rise by ~1.7 s after release 4.12?
3. **Research.** The 4.12 diff includes a new carousel component, an analytics SDK upgrade, and a new web font. TTFB in RUM is unchanged.
4. **Hypothesize.**
   - H1: The carousel lazy-loads the hero image, so the browser discovers it late.
   - H2: The analytics SDK is now render-blocking.
   - H3: The web font delays text rendering, and text is now the LCP element.
5. **Predict.** If H1 is true, the waterfall will show the hero image request starting after the carousel JS runs, and adding `fetchpriority="high"` with eager loading will restore LCP. If H2 is true, blocking the SDK domain will restore LCP. If H3 is true, the LCP element in the Performance panel will be a text node.
6. **Experiment.** Using WebPageTest on a Moto G-class profile over 4G, 5 runs each: baseline 4.12, then 4.12 with the SDK domain blocked, then a local build with the hero set to `loading="eager" fetchpriority="high"`.
7. **Analyze.** LCP element is the hero image, so H3 is refuted. Blocking the SDK saved 80 ms, which is noise, so H2 is refuted. The hero request started at 2.4 s in the baseline and at 0.6 s with eager loading, and median LCP dropped to 2.2 s. H1 is supported.
8. **Conclude.** The carousel applied `loading="lazy"` to every slide, including the first. The fix is eager loading and high priority for slide one. Lighthouse CI now asserts an LCP budget on the product page.

## Experiment techniques

- **Throttle deliberately.** Use DevTools CPU throttling (4x–6x) and network throttling to approximate the devices your users actually have. Your laptop is not your user's phone.
- **Use repeat runs and medians.** Web performance is noisy. Run each variant at least 3–5 times and compare medians.
- **Separate lab and field data.** Lab tools (Lighthouse, WebPageTest) are for controlled experiments. Field data (RUM, CrUX) is for deciding whether real users are affected.
- **Use request blocking and local overrides.** DevTools can block a URL or override a response file locally. That lets you test "what if this script or response were different" without a deploy.
- **Use feature flags as the variable.** Toggle the suspect feature for a slice of users and compare RUM between cohorts.
- **Keep incognito as a control.** Extensions and cached state are common hidden variables.

## AI prompts that help

- "Here is a Performance panel long-task summary and the component code for the handler. List five hypotheses for why this interaction takes 400 ms, ranked, with what I'd measure to disprove each."
- "Here's a heap snapshot comparison showing these retainers. Which references in this component could be keeping the detached nodes alive?"
- "Write a Playwright script that repeats this flow 20 times and records `performance.memory` (Chromium) after each run."

**Pitfall:** AI will often suggest generic fixes ("add lazy loading," "use `OnPush`," "memoize everything"). Some of those make things worse. Lazy loading the LCP image is exactly what caused the worked example above. Measure first.

## Theory behind the playbook

- [CS23 Human-Computer Interaction](../computer-science-in-ai-curriculum/talks/23-human-computer-interaction.md): perceived performance, accessibility, and usability
- [CS09 Computer Networks](../computer-science-in-ai-curriculum/talks/09-computer-networks.md): HTTP, caching, and latency
- [SDA20 Caching Strategies](../computer-science-software-design-and-architecture/curriculum/part-5/part-5-20--caching-strategies/README.md): client and CDN caching layers
- [SDA23 Performance Antipatterns](../computer-science-software-design-and-architecture/curriculum/part-6/part-6-23--performance-antipatterns/README.md): busy frontend, chatty I/O, extraneous fetching
- [SDA16 DNS, CDNs & Load Balancers](../computer-science-software-design-and-architecture/curriculum/part-4/part-4-16--dns-cdns-load-balancers/README.md)
