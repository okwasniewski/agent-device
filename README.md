# Live Limrun iOS evidence: long press and double tap

Recorded 2026-09-16 on Limrun iOS simulators (region eu-north1) through the Limrun SDK's
server-side recorder, while `agent-device` (local build of `fbad6861` + `bce6c4c5`, i.e.
callstack/agent-device#2642 + #2641 merged locally as `417df160`) drove the device over
`connect limrun --platform ios`.

App under test: the repo's `examples/test-app` Release build from the CI fixture artifact
`fingerprint.2ac5ab6fd3cb42b949511267c1474b7b66aee9f0.ios` (`com.callstack.agentdevicelab`).

## long-press.mp4 / long-press.gif (PR #2642)

Instance `ios_euna_01m2mkgemffvyty5wvbagsxsg0`. Automation lab screen, "Input canaries" section.

| step | command | canary text after |
| --- | --- | --- |
| baseline | `snapshot` | `Last input: none`, `Long presses: 0` |
| gesture | `longpress 'label="Long press canary"' 1000` | `Last input: longpress`, `Long presses: 1` |
| control | `press 'label="Long press canary"'` | `Last input: tap`, `Long presses: 1` |

Timeline: `timeline2.txt`. Still after the gesture: `long-press-after.jpg`.

## double-tap.mp4 / double-tap.gif (PR #2641)

Instance `ios_euna_01m2mkk4kmec8ba79fz359axnp`. Form screen, "Full name" field.

| step | command | edit menu in the accessibility tree |
| --- | --- | --- |
| fill | `fill @e8 "hello world"` | none |
| control | `press @e8` | `Select`, `Select All`, `AutoFill` (caret only, nothing selected) |
| gesture | `press @e8 --double-tap` | `Cut`, `Copy`, `AutoFill` (word selected) |

Timeline: `timeline3.txt`. Stills: `double-tap-single-press.jpg`, `double-tap-after.jpg`.

Both sessions were closed with `close`; the instances show `terminated` in the Limrun API.

## followups/ (PR #2645 screenshot, PR #2646 record)

Recorded 2026-09-16 on Limrun iOS instance `ios_euna_01m2mr0avjeebt7v4tbbwf950p` (eu-north1) by
`agent-device` built from a local merge of `feat/limrun-recording` (`6065c1f5c`) and
`fix/limrun-ios-screenshot-png` (`3c0912504`) on main `6dd57d9ae`. Same test app as above.

| step | command | result |
| --- | --- | --- |
| screenshot | `screenshot limrun-screenshot.png` | success, `file` reads `PNG image data, 402 x 874, 8-bit/color RGBA` |
| refused option | `record start x.mp4 --fps 30` | `INVALID_ARGS`: "Limrun recordings do not support --fps" |
| record | `record start limrun-record.mp4` … `longpress` … `press` … `record stop` | success, `recorder: confirmed`, `durationMs: 11472`; `ffprobe`: h264 602x1310, 10.87 s |

`limrun-record.mp4` is the file `record stop` wrote (no sidecar this time); `limrun-record.gif` is a
down-scaled preview of it, `limrun-record-frame.jpg` a frame at 6 s. Timeline: `timeline.txt`.
