# FristOneRR — Vulkan driver for Mali-G57 (Winlator / DXVK)

Vulkan driver for Mali-G57 on Android, for PC games through Winlator + DXVK.
Based on Mesa PanVK, running on the phone's stock Mali kernel driver (kbase). No root needed.

> ⚠️ **Beta.** Expect bugs. Personal project — not affiliated with Arm, Mesa or Collabora.

## Device compatibility

### How it works (short version)
Mali GPUs come in generations. What matters for this driver is **how the GPU receives work from the kernel**:

| Family | Arch | Job interface | Examples | This driver |
|---|---|---|---|---|
| Bifrost | v6 / v7 | JM (Job Manager) | G71, G72, G52, G76 | Built in, untested |
| **Valhall gen 1** | **v9** | **JM (Job Manager)** | **G57**, G68, G77, G78 | **Target** |
| Valhall gen 2+ | v10+ | CSF (Command Stream) | G310, G510, G610, G710, G615, G715, Immortalis | Not supported |

This driver talks to the phone's **stock Mali kernel driver (kbase)** using the JM interface. CSF GPUs use a completely different interface, so they will not work.

### ✅ Tested
| Phone | SoC | GPU | Android | Result |
|---|---|---|---|---|
| POCO M6 Pro | Helio G99 (MT6789) | Mali-G57 MC2 | 16 | Works (Far Cry 3 ~18-25 FPS, Low) |

### 🟢 Likely to work: same GPU (Mali-G57), untested
The same GPU as the tested device. The main risk is a different kbase version or vendor changes to the kernel driver.
- **Helio G99**: Redmi Note 13 Pro 4G, Redmi Note 12S, Galaxy A15 4G, and many Infinix / Tecno / realme phones
- **Helio G96**
- **Dimensity** 700 / 720 / 800U / 810 / 6020 / 6080 / 6100+
- **Unisoc** T616

### 🟡 Might work: same family (Valhall v9 / JM), different GPU, untested
Same architecture, but core count and hardware details differ. Google Tensor and Exynos may also ship modified kbase drivers.
- **Mali-G68**: Dimensity 900 / 920 / 1080 / 7050, Exynos 1280
- **Mali-G77**: Dimensity 1000 / 1100 / 1200
- **Mali-G78**: Exynos 1080 / 2100, Google Tensor G1 (Pixel 6), Kirin 9000

### 🟠 Long shot: Bifrost (JM), untested
Older architecture. Built into this driver and uses the same JM path, but has never been tested on real hardware.
- **Mali-G52**: Helio G80 / G85 / G88, Exynos 850, Kirin 810, Unisoc T618
- **Mali-G76**: Helio G90T / G95, Exynos 9820, Kirin 990
- **Mali-G71 / G72** (v6): Exynos 8890 / 9810, Kirin 960 / 970, Helio P60 / P70 (very old phones, may not run current Winlator)

### ❌ Not supported
- **CSF GPUs** (Mali-G310 / G510 / G610 / G710 / G615 / G715, Immortalis): different job interface
- **Midgard** (Mali-T series): not supported by PanVK
- **Adreno** (Snapdragon): use Turnip instead

## Mali driver version (kbase)
Your phone's stock Mali driver has a version like `r44p1` or `r54p1`. It changes with system updates.

- This driver has only been tested on **r54p1**.
- Very old kbase versions may fail to load or have missing features.
- You can usually see the version in GPU info apps (e.g. AIDA64, Device Info HW) or in a Vulkan info app while using the stock driver (look for something like `v1.r54p1`).

## About extension count
You may notice the number of Vulkan extensions differs from your stock driver (for example 68, 98, 114 or 148 depending on the stock driver version).

**Extension count is not a performance score.**
- Once installed, this driver reports what **PanVK actually supports** on the GPU, not what the stock Arm driver reports.
- All Mali-G57 phones should see the same (or very close) number with this driver, whatever the stock number was.
- Games and DXVK only need specific extensions. If those are present, the total number doesn't matter.
- Extensions are added when real games need them, not to inflate a number. Advertising extensions that don't really work leads to crashes and broken graphics.

## Help us test
If you try this driver on any device, please open an [Issue](../../issues) with:
- Phone model
- SoC and GPU (e.g. Helio G99 / Mali-G57 MC2)
- Android version
- Mali driver version (e.g. r54p1)
- Winlator version and DXVK version
- Game(s) tested, FPS, and whether it works / crashes / has graphics bugs

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
## Vulkan wrapper (Winlator)
| Wrapper | Status |
|---|---|
| [leegao/bionic-vulkan-wrapper](https://github.com/leegao/bionic-vulkan-wrapper) (latest) | ✅ **recommended** — best quality |
| Winlator Ludashi built-in wrapper | ✅ works, lower quality |
| Other wrappers | ✅ several tested and working, but lower quality than leegao's |

## Credits
This driver stands on the work of others — thank you:

- **[Mesa](https://mesa3d.org) / PanVK** — Collabora and the Mesa contributors (the Vulkan driver itself)
- **[leegao/mesa-funnymdzz](https://github.com/leegao/mesa-funnymdzz/tree/ci/src)** — the Mesa PanVK Android tree this driver is built from
- **[Vtgamer998/MESA-KMOD](https://github.com/Vtgamer998/MESA-KMOD)** — the kbase backend that lets Mesa run on the stock Mali kernel driver
- **[leegao/bionic-vulkan-wrapper](https://github.com/leegao/bionic-vulkan-wrapper)** — the Vulkan wrapper used for testing
  
## License
MIT (see `LICENSE`). Built on [Mesa](https://mesa3d.org) — Mesa's own license applies to
its code and is included in eevery zip (`LICENSE-Mesa.txt`).
