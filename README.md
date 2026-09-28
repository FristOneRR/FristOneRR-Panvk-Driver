# FristOneRR — Vulkan driver for Mali-G57 (Winlator / DXVK)

Vulkan driver for Mali-G57 on Android, for PC games through Winlator + DXVK.
Based on Mesa PanVK, running on the phone's stock Mali kernel driver (kbase). No root needed.

> ⚠️ **Beta.** Expect bugs. Personal project — not affiliated with Arm, Mesa or Collabora.

## Tested device
| Device | SoC | GPU | Android | Stock driver |
|---|---|---|---|---|
| POCO M6 Pro (2312FPCA6G) | MediaTek Helio G99 (MT6789) | Mali-G57 MC2 | 16 | Mali (kbase, JM) |

## Should work (same GPU, not tested yet)
Phones with a **Mali-G57 MC2 / MP2** GPU:

- **MediaTek Helio:** G96, G99, G100, G200
- **MediaTek Dimensity:** 700, 810, 6020, 6080, 6100+, 6300, 6400
- **UNISOC:** T750, T765 (T8200), T7300, T8300, T9300

Different vendors ship different kbase versions, so please report your result — working or not.

## Install
1. Download the latest `.zip` from **[Releases](../../releases)**.
2. Winlator → install the zip as a Vulkan/graphics driver (same way as Turnip zips).
3. Select **FristOneRR** in the container's graphics driver setting.
4. The first line of the Wine log should read `FristOneRR vXX (Mesa ...)`.

Verify the download: its `sha256sum` must match `SHA256.txt` on the release page.

## DXVK
| Version | Status |
|---|---|
| DXVK 1.5.5 | ✅ works |
| DXVK-Sarek 1.11 | ✅ works (vkcube OK) — Far Cry 3 crashes at start with Sarek, use DXVK 1.10.3 for it |
| DXVK 1.10.3 | ✅ works (recommended) |
| DXVK 2.x | ❓ not tested yet — planned for the next update |

## Tested games
| Game | API | Result |
|---|---|---|
| Far Cry 3 | D3D9 | ✅ playable, ~18–25 FPS (Low settings) |
| Borderlands | D3D9 | ✅ playable |
| Need for Speed: Most Wanted (2005) | D3D9 | ✅ playable |

## Options (environment variables)
| Variable | Default | Effect |
|---|---|---|
| `PANVK_OVERLAP=0` | on | turn off vertex/pixel overlap (if you see glitches) |
| `PANVK_AFBC=1` | off | enable AFBC compression (currently slower) |
| `PANVK_SPILL_NOOPT=0` | on | turn off the fix for GPU hangs in heavy shaders |
| `PANVK_SKIP_FS=1` | off | skip very heavy shaders (last resort for hangs) |
| `PANVK_TILER_HEAP_MB=256` | 512 | smaller GPU heap if RAM is low |

## Report a problem
Open an **[Issue](../../issues)** with: phone model / SoC, driver version (first log line),
Winlator / Wine / DXVK version, game name, what happened, and the log file.

## License
MIT (see `LICENSE`). Built on [Mesa](https://mesa3d.org) — Mesa's own license applies to
its code and is included in eevery zip (`LICENSE-Mesa.txt`).
