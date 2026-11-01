<p align="center"><img src="https://github.com/user-attachments/assets/ddd094ce-92c3-4bd4-82c2-18fc3a159020" alt="FristOneRR PanVK" width="220"></p>

<h1 align="center">FristOneRR PanVK</h1>

<p align="center"><b>An open-source Vulkan driver for Mali GPUs on Android (kbase),<br>
built for Winlator, DXVK and emulators.</b></p>

<p align="center">
<a href="../../releases">Download</a> |
<a href="#installation-step-by-step">Install</a> |
<a href="#device-compatibility">Devices</a> |
<a href="https://github.com/FristOneRR-Admin/FristOneRR-Wrapperv1">Wrapper</a> |
<a href="../../issues">Report a problem</a> |
<a href="https://t.me/FirstOneRR">Telegram</a>
</p>

> Download only from the [Releases](../../releases) page of this repository and check the
> [SHA256](#official-download-and-sha256). Files from other places are not ours.

<p align="center">
<img alt="latest release v3.6.0" src="https://img.shields.io/badge/latest%20release-v3.6.0-4c1">
<img alt="license MIT" src="https://img.shields.io/badge/license-MIT-007ec6">
<img alt="GPU Mali (kbase)" src="https://img.shields.io/badge/GPU-Mali%20%28kbase%29-e05d44">
<img alt="wrapper 3.1.0" src="https://img.shields.io/badge/wrapper-3.1.0-8a2be2">
<img alt="dev FristOneRR" src="https://img.shields.io/badge/dev-FristOneRR-555">
</p>

<p align="center">
<a href="#about">About</a> |
<a href="#whats-new-in-360">What's new</a> |
<a href="#supported-apps">Apps</a> |
<a href="#directx-support">DirectX</a> |
<a href="#emulators">Emulators</a> |
<a href="#known-issues">Known issues</a> |
<a href="#credits">Credits</a> |
<a href="#license">License</a>
</p>

---

## About

An open-source Vulkan driver (Mesa **PanVK**) for Mali GPUs on Android phones that use
Arm's vendor **kbase** kernel driver. Windows games run through Vulkan in **Winlator** with
**DXVK** and **vkd3d-proton**, and emulators with a custom GPU driver option can use it too,
without the stock Mali driver.

> **Status: 3.6.0.** It works well on many Mali phones, but it is not a conformant Vulkan
> implementation. Expect bugs and please report them.

---

## Official download and SHA256

Download only from the [Releases](../../releases) page of this repository.
To check that your file is the original, compare its SHA256 with the table below
(on Android: a file-hash app, or Termux: `sha256sum <file>`).

| File | SHA256 |
|---|---|
| `PanVK-FristOneRR-V.3.6.0.zip` (driver) | `b963d57e09aa53a5a43c2608c7df2c85566bcb6b21fcffcba941bda173ab1665` |
| `libvulkan_panfrost.so` (inside the driver zip) | `4e249068ea774fe244a94519c83e186f9bff3d7884ca3379d75d07613d080fce` |
| `FristOneRR-Wrapper.zip` ([wrapper 3.1.0](https://github.com/FristOneRR-Admin/FristOneRR-Wrapperv1)) | `006fc3d4a48f765b1442e7542f47c58a8ccbb2f63761bed1f943d260a0d6f063` |
| `wrapper-fristonerr.tzst` (same wrapper, tzst package) | `b61cab354a3400cab5ffcc6c4d9312062bc096777d1f9c08e851456a1355b69a` |
| `libvulkan_wrapper.so` (inside the wrapper zip / tzst) | `d4aaa03f0f354c3dcbcbc52cd2dfa6899770ba839f3129267eb9894dd1885298` |

If the SHA256 is different, the file is not ours. Don't install it.

---

## What's new in 3.6.0

> **Use this driver with [FristOneRR-wrapper 3.1.0](https://github.com/FristOneRR-Admin/FristOneRR-Wrapperv1/releases).**
> DXVK 2.7 and newer (including DXVK 3.x) need wrapper 3.1.0; older wrappers stop them from starting.

- **Vulkan 1.1 to 1.4 supported.** Apps and games that need Vulkan 1.1, 1.2, 1.3 or 1.4 can use the driver.
- **DXVK 3.x works.** DXVK 2.x and 3.x start and render on Valhall phones (G57, G68, G77, G78),
  including the gplasync, async, Sarek, GPLALL and ARM64EC builds.
- **DXVK 2.x now also works on Bifrost** (G52, G72, G76), and DXVK 2.7+ / 3.x should work too with wrapper 3.1.0 (not tested yet). Before, these GPUs needed DXVK 1.x.
- **Games load faster.** The long wait before a game starts (and before the extension list shows in driver apps) is gone.
- **Less shader stutter with DXVK 2.x/3.x.** The driver now supports graphics pipeline libraries by default, so you no longer need `DXVK_ASYNC=1`.
- **32-bit games on more phones:** phones without a special memory heap (for example Dimensity 700) no longer crash in 32-bit games.
- **More games draw correctly.** Better support for effects that D3D10/D3D11 games use
  (geometry shaders, stream output, wireframe and more), and textures that stayed missing in some games now show up.
- **DXVK HUD fixed.** The text is no longer squashed into one line, and the frame-time graph works.
- **Games that measure GPU time** (DXVK HUD, built-in benchmarks) now get real values instead of 0.
- Everything from 3.5 is included.

---

## Installation (step by step)

**What you need**
- A Mali phone from the [compatibility list](#device-compatibility).
- An app from the [supported apps](#supported-apps) list.

**Steps**
1. Download `PanVK-FristOneRR-V.3.6.0.zip` from [Releases](../../releases). **Don't unzip it.**
2. Open Winlator → **Contents / Drivers → Install**, then pick the **zip file itself**.
3. Open your container's settings (**Edit container**) and set the graphics driver to **Wrapper**.
   In the wrapper/driver settings, choose **PanVK-FristOneRR-V.3.6.0** as the Vulkan driver.
4. Install **FristOneRR-wrapper 3.1.0** (see [Wrapper](#wrapper)) and pick a DXVK version (and vkd3d-proton for D3D12 games).
5. After every driver update, clear the shader cache (DXVK / vkd3d cache files of your games).
6. Start the container and check it's working: open **AIO Graphics Test → GPU Info**. The GPU should show as **Mali-…** with about **130+ extensions**.
   The extension count shown in the driver menu does **not** change; only GPU Info inside a running container shows the real number.

**Troubleshooting**
- *"Error" when installing the zip:* make sure you picked the original zip (not an extracted folder or a re-zipped copy).
- *No install button:* your Winlator build doesn't support custom drivers. Use one of the [supported apps](#supported-apps).
- **Always set BCn texture emulation to "none".** It works best with FristOneRR PanVK.
  BCn → ASTC / ETC can cause wrong colors, high RAM use or crashes; it will be improved in the next update.
  (In Bannerlator and Winlator Mali, "auto" also needs the ASTC box ticked, otherwise the picture is wrong.)

---

## Supported apps

| App | Status | BC → ASTC |
|---|---|:---:|
| **Winlator Ludashi** | ✅ **Recommended for best performance** | — |
| Bannerlator | ✅ | ✅ |
| Winlator Mali 1.2+ (adrenotools) | ✅ | ✅ |
| GameNative | ✅ | — |

> **Tip (Ludashi):** the Ludashi package gives the highest performance, but the phone can get warm.
> Use a phone cooler (air cooling), or lower the resolution to **540p** and lock the frame rate at **30 FPS**.

Set BCn texture emulation to **"none"** in every app (see [Troubleshooting](#installation-step-by-step)). BCn → ASTC / ETC will be improved in the next update.
Some Winlator forks have no option to install custom drivers. They can't load this driver.

---

## DirectX support

With **FristOneRR-wrapper 3.1.0**, DXVK (D3D8–D3D11) and vkd3d-proton (D3D12):

| API | 64-bit games | 32-bit games |
|---|:---:|:---:|
| **D3D8**  | ✅ | ✅ |
| **D3D9**  | ✅ | ✅ |
| **D3D10** | ✅ | ✅ |
| **D3D11** | ✅ | ✅ |
| **D3D12** (vkd3d-proton) | ✅ | ✅ |

### Which DXVK to use

| DXVK | Valhall (G57, G68, G77, G78) | Bifrost (G52, G72, G76) |
|---|:---:|:---:|
| DXVK 1.x, async, Sarek | ✅ | ✅ |
| DXVK 2.0 – 2.6 (incl. gplasync, GPLALL) | ✅ | ✅ |
| DXVK 2.7 – 3.x (incl. gplasync, ARM64EC) | ✅ with wrapper 3.1.0 | ⚠️ should work with wrapper 3.1.0 (not tested yet) |

On Bifrost, D3D11 games run at feature level 10_1 at most: games that need 11_0 won't start.
D3D10 games: use DXVK 1.x (DXVK 2.x and newer no longer include their own D3D10).

**Vulkan test programs** (for example the *Vulkan cube* in AIO Graphics Test) are not a target of
this driver. If they fail, that doesn't mean games won't work. Test with the D3D tests or a real game.

---

## Wrapper

**Use [FristOneRR-wrapper 3.1.0](https://github.com/FristOneRR-Admin/FristOneRR-Wrapperv1)** with this driver.
It is made for FristOneRR PanVK, supports 32-bit and 64-bit games, D3D12, and DXVK 2.7+ / 3.x.

- **`FristOneRR-Wrapper.zip`:** extract it, then double-click **`install.bat`** inside the container
  (it backs up the app's own wrapper first; `restore.bat` puts it back). Restart the container afterwards.
- **`wrapper-fristonerr.tzst`:** for apps that import wrappers from a `.tzst` file (Bannerlator and others).
  Import it and select it; nothing else to tick. After switching wrappers, **close the app completely** before starting it again.

Full steps: [FristOneRR-wrapper](https://github.com/FristOneRR-Admin/FristOneRR-Wrapperv1#install).

Other wrappers (leegao, Ludashi built-in, GameNative, Bannerlator) can also load the driver, but
they are not tuned for it, and some of them stop DXVK 2.7+ / 3.x from starting.

---

## Emulators

The driver zip can also be installed in emulators that have a **custom GPU driver** option
(install the zip itself, the same way as in Winlator). This is for people who want to test.
Results on Mali-G57 MC2:

| Emulator | Status |
|---|---|
| **PS2** | ✅ **Recommended.** Plays well; some games may stutter. |
| **Xbox ([XboxMali](https://github.com/msx94302-ops/Xbox-Mali/releases/tag/XBOX-MALI-v0.0.5))** | ✅ **Recommended.** Need for Speed Most Wanted (Black Edition): 720p, resolution scale 2x, steady 30 FPS. |
| **Switch (Eden Nightly)** | ✅ Optimized since 3.5. Many games playable; pause/resume no longer crashes. |
| **PS3** | ❌ Not recommended. Beyond what Mali-G57 can do. |
| **PS4** | ❌ Not recommended. Beyond what Mali-G57 can do. |

**Tips**
- Clear the emulator's shader cache after installing a new driver.
- The first start of a Switch game compiles shaders; the second start is much faster.
- Heavy Switch games are limited by the CPU: lowering the resolution doesn't raise the FPS in gameplay.
- Close other apps (especially Winlator) before playing: Switch games need a lot of free RAM.
- XboxMali: turn **Hack 60 FPS off**. Original Xbox games are made for 30 FPS; above that the game can look smoother, but the picture or game speed can go wrong.
- XboxMali: resolution scale 1x reaches 30 FPS in every resolution, up to 1080p. Scale 2x looks much sharper but is heavy: if a game stutters at 2x, go back to 1x and play at 1080p (30 FPS).
- XboxMali download: [XBOX-MALI-v0.0.5](https://github.com/msx94302-ops/Xbox-Mali/releases/tag/XBOX-MALI-v0.0.5). *I'm not involved in developing XboxMali; I was just impressed by how well it runs with the PanVK driver I'm developing.*

### Native ports

| Port | Status |
|---|---|
| **Skyrim (2011) – [Skyrim_Android](https://github.com/ttom795/Skyrim_Android/releases)** | ✅ Picture correct. 960x540, Low: 20–30 FPS on Mali-G57 MC2. Install the driver zip in the launcher's **GPU driver** section. Set **Frame rate cap** to 30. |

---

## Device compatibility

✅ works · ⚠️ works with limits · ❓ waiting for a test result · ❌ not supported

| Device | SoC | GPU | Status |
|---|---|---|---|
| Poco M6 Pro 4G *(dev device)* | Helio G99 | Mali-G57 MC2 | ✅ |
| Samsung Galaxy A15 | Helio G99 | Mali-G57 MC2 | ✅ |
| Samsung Galaxy A17 4G | Helio G99 | Mali-G57 MC2 | ✅ |
| Realme 10 4G | Helio G99 | Mali-G57 MC2 | ✅ |
| Poco M5 | Helio G99 | Mali-G57 MC2 | ✅ |
| Infinix Note 50 Pro | Helio G100 Ultimate | Mali-G57 MC2 | ✅ |
| *(unnamed)* | Helio G100 Ultra | Mali-G57 MC2 | ✅ |
| Infinix Note 30 5G | Dimensity 6080 | Mali-G57 MC2 | ✅ |
| *(unnamed)* | Dimensity 6080 | Mali-G57 MC2 | ✅ |
| Samsung Galaxy A15 5G | Dimensity 6100+ | Mali-G57 MC2 | ✅ |
| Vivo Y29s | Dimensity 6300 | Mali-G57 MC2 | ✅ |
| Redmi Note 11 SE 5G | Dimensity 700 | Mali-G57 MC2 | ✅ |
| *(unnamed)* | Unisoc T820 | Mali-G57 | ✅ |
| Tecno Spark 10C | Unisoc T606 | Mali-G57 MP1 | ✅ |
| Tecno Camon 20 Pro 5G | Dimensity 8050 | Mali-G77 MC9 | ✅ |
| POCO X3 GT | Dimensity 1100 | Mali-G77 MC9 | ✅ |
| *(unnamed)* | Kompanio 1300T | Mali-G77 MC9 | ✅ |
| Samsung Galaxy S20 | Exynos 990 | Mali-G77 MP11 | ❓ |
| *(unnamed)* | — | Mali-G78 MP20 | ✅ |
| Samsung Galaxy S21 | Exynos 2100 | Mali-G78 MP14 | ✅ (`PANVK_ATOM_STRIDE=56`) |
| Redmi Note 12 Pro 5G | Dimensity 1080 | Mali-G68 MC4 | ✅ |
| Samsung Galaxy M33 5G | Exynos 1280 | Mali-G68 MP4 | ✅ |
| Samsung Galaxy Tab S9 FE+ | Exynos 1380 | Mali-G68 MP5 | ✅ |
| Samsung Galaxy A54 | Exynos 1380 | Mali-G68 MP5 | ✅ (`PANVK_ATOM_STRIDE=56`) |
| Samsung Galaxy A26 5G | Exynos 1380 | Mali-G68 MP5 | ✅ |
| Redmi Note 10S | Helio G95 | Mali-G76 MC4 | ✅ |
| Poco M5s | Helio G95 | Mali-G76 MC4 | ✅ |
| Tecno Camon 17 Pro | Helio G95 | Mali-G76 MC4 | ✅ (DXVK 1.x) |
| Huawei P40 Pro | Kirin 990 | Mali-G76 MP16 | ❓ |
| Oppo A38 | Helio G85 | Mali-G52 MC2 | ✅ |
| Xiaomi Redmi 13C | Helio G85 | Mali-G52 MC2 | ✅ |
| RGVITA (tablet) | Unisoc T618 | Mali-G52 MC2 | ✅ |
| *(unnamed)* | — | Mali-G72 MP3 | ✅ |
| Samsung Galaxy A50 | Exynos 9610 | Mali-G72 MP3 | ❓ |
| Samsung F07 | — | — | ✅ |

### By GPU family

| Family | GPUs | Status |
|---|---|---|
| Valhall v9 (Job Manager) | G57, G68, G77, G78 | ✅ Main target |
| Bifrost v7 | G52, G76 | ✅ (DXVK 1.x and 2.x; 3.x not tested yet) |
| Bifrost v6 | G71, G72 | ✅ Confirmed on G72 MP3 |
| Valhall 5th gen (CSF) | G610, G615, G710, G720, Immortalis | ❌ Not supported yet. Planned once I have a device to test on. |
| Midgard | T-series | ❌ Not supported |

Your phone isn't listed? Try it and tell us the result in an [Issue](../../issues).

---

## Environment variables (advanced)

| Variable | Default | What it does |
|---|---|---|
| `FRR_LOG` | `0` | `1` writes the driver's log messages. Turn it on when you report a bug. |
| `PANVK_HEAP_MB` | 25 % of RAM (1024–3072) | GPU memory reported to games (256–16384). On 4 GB phones, `2048` can fix missing or broken textures in heavy games; lower it if Android kills the app. |
| `PANVK_POLY_HEAP_MB` | `16` | Geometry/tessellation scratch heap (4–512). Raise it only if a game draws missing geometry. |
| `PANVK_TILER_HEAP_MB` | `512` | Tiler heap limit (16–2048). Lower values may save RAM in heavy games. |
| `PANVK_STENCIL_LATE` | `0` | `1` fixes black triangles / missing light in some games. Leave it off for other games: it can make them darker and slower. |
| `PANVK_ATOM_STRIDE` | auto | Set `56` only if the driver doesn't start on your phone (some Exynos and G52 phones). |

> ⚠️ **Only use the variables in this table.** The driver has other internal switches
> for development and testing. Setting them can break rendering, lower FPS or crash games.

---

## Known issues

- Some heavy D3D12 games (for example *Resident Evil 2* and *Stray*) don't start or crash while loading. Use the game's D3D11 mode if it has one.
- Bifrost (G52 / G76 / G72): D3D11 games that need feature level 11_0 don't start (the GPU has no tessellation or geometry shaders).
- Some games show a see-through triangle on the right side of menus.
- Games that ask for 2× MSAA can't create textures (Mali has no 2× mode). Turn off anti-aliasing or use 4×.
- Heavy games can still be slower than on the stock driver.
- Bannerlator's **"surfaceflinger"** render option shows wrong colors (blue sky turns red). Use the default renderer.

---

## How to report a problem

Open an [Issue](../../issues) and include:

1. Phone model, SoC, GPU and kbase version (shown in AIO Graphics Test → GPU Info).
2. App (Winlator build), wrapper, DXVK version, Box64/FEX version.
3. What happens (black screen, crash, freeze, low FPS…).
4. Logs:
   - **Wine log:** enable Wine debug in the container settings, add `FRR_LOG=1` to the environment variables, then attach `wine_debug.log`.
   - **DXVK log:** set `DXVK_LOG_LEVEL=info` **and** `DXVK_LOG_PATH=C:\` (without `DXVK_LOG_PATH` the file may not be written).

Use an existing issue for the same device or game instead of opening a new one. Videos can't be attached directly, so upload them (YouTube, Google Drive…) and post the link.

---

## Credits

This driver stands on the work of many people. Thank you!

| Who | Contribution |
|---|---|
| **Mesa / PanVK developers** (Collabora, Arm and contributors) | The PanVK Vulkan driver this project is built on |
| **funnymdzz** (mesa fork) & **leegao** | The Mesa source used as our base; [bionic-vulkan-wrapper](https://github.com/leegao/bionic-vulkan-wrapper), which FristOneRR-Wrapper is built on; advice throughout the project |
| **Noysz** / [panvk-g99-jm](https://github.com/Noysz/panvk-g99-jm) | Valhall v9 / Job Manager groundwork (included in the base we started from); also our reference for the v9 draw path |
| **Vtgamer998** (MESA-KMOD) | kbase kernel-interface work |
| **mexicanbr0auth** / [mesa-panvk-g57](https://github.com/mexicanbr0auth/mesa-panvk-g57) | PanVK/kbase work for Mali-G57; parts of our code came from this project |
| **wonderkast02** / [panvk-g720-kbase-csf](https://github.com/wonderkast02/panvk-g720-kbase-csf) | Community PanVK-over-kbase work (Mali-G720, CSF) |
| **BossDrk** / [0x8055/panvk-g52-oppo-a38](https://github.com/0x8055/panvk-g52-oppo-a38) | Mali-G52 on Oppo A38 research and testing: found and verified the 56-byte stride fix |
| **Claude** (AI assistant by Anthropic) | Development help, debugging and code review; audited the code origin and helped write this credit list |
| **All testers** who opened issues and sent logs | Device reports and game results |

**Code origin (measured at 1.1.0):** of the 1,669 lines we added on top of Mesa, **67 %** don't appear anywhere else, **3.6 %** match mexicanbr0auth, **2.4 %** match Noysz and **0.7 %** match BossDrk's 0x8055 repository; the rest is Mesa code we moved or reused. Full table, per-file details and a script to re-check it yourself are in the [source repository](https://github.com/FristOneRR-Admin/FristOneRR-Panvk-Source).

## Support

☕ Support my work

If you enjoy my projects and would like to support my work, you can buy me a coffee on Ko-fi! Every project I create is and will always remain free, but any support is greatly appreciated and helps me keep working on new projects and improvements.

Thank you for your support!

[Support on Ko-fi](https://ko-fi.com/fristonerr)


## License

The driver is built from Mesa (MIT license; see `LICENSE-Mesa.txt` in the zip).
This repository is licensed under the MIT License.
