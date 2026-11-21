# Battery & performance

All numbers are measurements from the Redmi K90 (HyperOS OS4.0.6.0, Android 17) running the
**R8 release build** unless stated otherwise. Nothing here is estimated or extrapolated.

## Method

- Idle cost: `dumpsys battery unplug` (so batterystats accumulates while on the charger),
  `dumpsys batterystats --reset`, screen off (`KEYCODE_SLEEP`) for 10 minutes, then
  `dumpsys batterystats app.hyperkeep` / `--checkin`, process CPU from `/proc/<pid>/stat`
  (utime + stime, 10 ms ticks) and `dumpsys meminfo app.hyperkeep`. `dumpsys battery reset`
  afterwards. Wireless ADB stayed connected during the window, which itself keeps Wi‑Fi active;
  the comparison between modes is what matters.
- Repair cost and latency: HyperKeep logs one line per background pass (`I/HyperKeep … pass=…us`),
  correlated with PowerKeeper's own log line (`SmartPower.PowerPolicy: … ChangedNoRestrictAppsList`).
- Percent-of-battery figures are not reported: over minutes they would be below the gauge's
  resolution and therefore made up. CPU time, wake-ups, wake locks and memory are the drivers and
  are reported directly.

## Results (release build)

### Idle cost

| Mode | Window | HyperKeep CPU | Wake locks | Alarms / jobs run | Memory (PSS) | batterystats estimate |
|---|---|---|---|---|---|---|
| **Smart Guard** (default, no service) | 10 min, screen off, background process (no UI loaded) | **55 ms** (batterystats; 110 ms in `/proc`). All of it was one GC that HyperOS requested on the cached process; HyperKeep code did not run | none | none | 18.7 MB while cached; 0 once the system reclaims it | 0.0056 mAh |

A first Smart Guard run started right after on-screen use measured 135 ms / 0.014 mAh: that was the
one-time cost of the activity being torn down at screen-off plus two GCs of the cached UI heap,
not monitoring work, so it was re-run from the real background state (above).

Smart Guard is effectively free while nothing changes, and it keeps no service and no
notification.

The `--checkin` `pwi` field for HyperKeep read 1.54 mAh, but that field is a since-charged
aggregate that did not reset and includes ~40 minutes of on-screen UI testing; it is not an idle figure.

The debug build is not representative (58 MB PSS, 33 MB of unminified dex).

The update check is a separate JobScheduler job: periodic (about once a day, 6 h flex), only with
a network, persisted. Android runs it in a maintenance window alongside other apps' jobs. One HTTPS
request for `hyperkeep-update.json` (about 1 KB); no service, alarm or wake lock.

### Event cost and latency (PowerKeeper really rebuilding the list)

| Scenario | Removal → repaired | Engine pass | Follow-up pass caused by HyperKeep's own write |
|---|---|---|---|
| Smart Guard, process cached | 1.53 s | 14.2 ms | 3.4 ms (no-op) |
| Smart Guard, process killed (cold start) | 1.84 s | 34.2 ms | 4.9 ms (no-op) |

Each repair is one `Settings.System` read-compare-write-read cycle plus, if Google Play
services was re-added, two explicit heartbeat broadcasts. No wake lock is held by HyperKeep; the
job framework holds its own short `*job*` wake lock only while a job runs (measured 0 ms total in
the idle window, because no job ran).

### UI smoothness (release build, Redmi K90, 120 Hz)

`dumpsys gfxinfo` while flinging the 387-app list up and down 12 times with the frosted
navigation bar and status strip active: 2 608 frames, **1 janky frame (0.04 %)**, 0 missed
vsync, 0 slow UI-thread frames; frame time p50 10 ms / p99 15 ms; GPU 6–8 ms for most frames.
The blur only costs anything while content moves on screen.

### Size

Release APK 2.8 MB (R8 + resource shrinking), no native libraries of its own besides the AndroidX
graphics-path and DataStore helpers.

### Why it is cheap

- No polling, timers, alarms or wake locks in any mode.
- Smart Guard: system_server observes the key; HyperKeep's process only exists for the few
  milliseconds of a pass.
- Background passes never touch the package inventory, icons or UI classes.
- Writes happen only when a protected package is actually missing; the self-triggered re-check
  is a cheap no-op and cannot loop.
- Repeated fights are throttled (RepairPolicy) instead of retried in a storm.
