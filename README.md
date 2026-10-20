# FristOneRR PanVK Driver for Mali (kbase)

An open-source Vulkan driver (Mesa **PanVK**) for Mali GPUs on Android phones that use
Arm's vendor **kbase** kernel driver. It is built for **Winlator** and **DXVK**, so Windows games
can run through Vulkan without the stock Mali driver.

> **Status: 3.0.** It works well on many Mali phones, but it is not a conformant Vulkan
> implementation. Expect bugs and please report them.

---

## Official download and SHA256

Download only from the [Releases](../../releases) page of this repository.
To check that your file is the original, compare its SHA256 with the table below
(on Android: a file-hash app, or Termux: `sha256sum <file>`).

| File | SHA256 |
|---|---|
| `Panvk-Mali-G57-3.0.zip` (driver) | `6c5f00cb39a7330e1840547a9b4dcb8d3bc9b6d939a2977592769c46908c0ac2` |
| `libvulkan_panfrost.so` (inside the driver zip) | `8761b79bc62bbc41ce2e0f7987f90a856ebd3d1b6351f2dfb628e678d6d9d4ab` |
| `FristOneRR-Wrapper-3.0.zip` ([wrapper](https://github.com/FristOneRR-Admin/FristOneRR-Wrapperv1)) | `6bbfe5b252df2ea038e7bb8b360b59b11107da3465ed1d951e97fba4449e9d41` |
| `FristOneRR-Wrapper-3.0.tzst` (same wrapper, tzst package) | `3d039d0669fdbb869c8493780afd57f9168d2891126755ecbb944d99fc8c63f5` |
| `libvulkan_wrapper.so` (inside the wrapper zip / tzst) | `f9350e6b0c936f383f160b20a379ef714bce8e8e89752472d4f087cca480acfb` |

If the SHA256 is different, the file is not ours. Don't install it.

---

## What's new in 3.0

- **D3D12 works** through vkd3d-proton (with FristOneRR-Wrapper 3.0).
- **Black triangles fixed** in some games.
- **Mali-G52 bug fixes.** DXVK 2.x still doesn't start on Bifrost (G52 / G76 / G72); use DXVK 1.x. This is planned for the next update.
- **Emulators:** the same driver zip now also works in **Switch, PS2, PS3 and PS4 emulators** that can install
  custom GPU drivers. See [Emulators](#emulators) for what works.
- **BC4–BC7 textures are handled inside the driver**, so emulators no longer need BCn hidden. BC4/BC5 textures use less RAM.
- **Crash fixes:** changing the resolution in a game, loading a new level in emulators, and games that use many textures in one draw.
- **New: [FristOneRR-Wrapper 3.0](https://github.com/FristOneRR-Admin/FristOneRR-Wrapperv1)** with D3D12 support and a
  one-click installer (`install.bat`).

---

## Installation (step by step)

**What you need**
- A Mali phone from the [compatibility list](#device-compatibility).
- An app from the [supported apps](#supported-apps) list.

**Steps**
1. Download `Panvk-Mali-G57-3.0.zip` from [Releases](../../releases). **Don't unzip it.**
2. Open Winlator → **Contents / Drivers → Install**, then pick the **zip file itself**.
3. Open your container's settings (**Edit container**) and set the graphics driver to **Wrapper**.
   In the wrapper/driver settings, choose **Panvk-Mali-G57-3.0** as the Vulkan driver.
4. Install **FristOneRR-Wrapper 3.0** (see [Wrapper](#wrapper)) and pick a DXVK version (and vkd3d-proton for D3D12 games).
5. After every driver update, clear the shader cache (DXVK / vkd3d cache files of your games).
6. Start the container and check it's working: open **AIO Graphics Test → GPU Info**. The GPU should show as **Mali-…** with about **130+ extensions**.
   The extension count shown in the driver menu does **not** change; only GPU Info inside a running container shows the real number.

**Troubleshooting**
- *"Error" when installing the zip:* make sure you picked the original zip (not an extracted folder or a re-zipped copy).
- *No install button:* your Winlator build doesn't support custom drivers. Use one of the [supported apps](#supported-apps).
- *A game crashes on start, or the phone freezes / reboots:* set **BCn texture emulation to "none"** in the wrapper settings. BCn → ASTC conversion can freeze the phone.

---

## Supported apps

| App | Status |
|---|---|
| **Winlator Ludashi** | ✅ **Recommended for best performance** |
| Bannerlator | ✅ |
| Winlator Mali 1.2 (adrenotools) | ✅ |
| GameNative | ✅ |

> **Tip (Ludashi):** the Ludashi package gives the highest performance, but the phone can get warm.
> Use a phone cooler (air cooling), or lower the resolution to **540p** and lock the frame rate at **30 FPS**.

Some Winlator forks have no option to install custom drivers. They can't load this driver.

---

## DirectX support

With **FristOneRR-Wrapper 3.0**, DXVK (D3D8–D3D11) and vkd3d-proton (D3D12):

| API | 64-bit games | 32-bit games |
|---|:---:|:---:|
| **D3D8**  | ✅ | ✅ |
| **D3D9**  | ✅ | ✅ |
| **D3D10** | ✅ | ✅ |
| **D3D11** | ✅ | ✅ |
| **D3D12** (vkd3d-proton) | ✅ | ✅ |

**Vulkan test programs** (for example the *Vulkan cube* in AIO Graphics Test) are not a target of
this driver. If they fail, that doesn't mean games won't work. Test with the D3D tests or a real game.

---

## Wrapper

**Use [FristOneRR-Wrapper 3.0](https://github.com/FristOneRR-Admin/FristOneRR-Wrapperv1)** with this driver.
It is made for FristOneRR PanVK and supports 32-bit and 64-bit games and D3D12.

### Easy install (recommended): `install.bat`

1. Download `FristOneRR-Wrapper-3.0.zip` and **extract it** into a folder the container can see
   (for example your phone's `Download` folder, which is usually drive **D:** in Winlator).
   Keep `install.bat`, `restore.bat` and `libvulkan_wrapper.so` together in the same folder.
2. Start the container, open that folder in the Wine file manager and **double-click `install.bat`**.
   - The first time, it saves the app's own wrapper to `Z:\usr\lib\FristOneRR-backup\`.
   - Then it installs FristOneRR-Wrapper 3.0. Wait for **"Done"** and press a key.
3. **Close the container and start it again.** The new wrapper is loaded on the next start.

To go back to the app's own wrapper, double-click **`restore.bat`** and restart the container.
Running `install.bat` again later (for example after `restore.bat`) keeps the first backup; it never overwrites it.

### Other ways to install

- **`FristOneRR-Wrapper-3.0.tzst`:** for apps that install wrappers from a `.tzst` file
  (contains `usr/lib/libvulkan_wrapper.so`).
- **By hand:** copy `libvulkan_wrapper.so` from the zip into the container at **`Z:\usr\lib\`**,
  replacing the existing file (keep a backup).

More details: [FristOneRR-Wrapper](https://github.com/FristOneRR-Admin/FristOneRR-Wrapperv1#install).

Other wrappers (leegao, Ludashi built-in, GameNative, Bannerlator) can also load the driver, but
they are not tuned for it.

---

## Emulators

The driver zip can also be installed in emulators that have a **custom GPU driver** option
(install the zip itself, the same way as in Winlator). This is for people who want to test.
Results on Mali-G57 MC2:

| Emulator | Status |
|---|---|
| **PS2** | ✅ **Recommended.** Plays well; some games may stutter. |
| **Switch** | ⚠️ Partly works. Connects, and some games are playable. |
| **PS3** | ❌ Not recommended. Beyond what Mali-G57 can do. |
| **PS4** | ❌ Not recommended. Beyond what Mali-G57 can do. |

**Tips**
- Clear the emulator's shader cache after installing a new driver.
- Heavy Switch games are limited by the CPU: lowering the resolution doesn't raise the FPS in gameplay.
- Switch emulators can't set environment variables, so the driver uses its default settings there.
- Changing the resolution inside the emulator now works without a crash.

---

## Device compatibility

✅ works · ❓ waiting for a test result · ❌ not supported

| Device | SoC | GPU | Status |
|---|---|---|---|
| Poco M6 Pro 4G *(dev device)* | Helio G99 | Mali-G57 MC2 | ✅ |
| Samsung Galaxy A15 | Helio G99 | Mali-G57 MC2 | ✅ |
| Samsung Galaxy A17 4G | Helio G99 | Mali-G57 MC2 | ✅ |
| Realme 10 4G | Helio G99 | Mali-G57 MC2 | ✅ |
| Infinix Note 50 Pro | Helio G100 Ultimate | Mali-G57 MC2 | ✅ |
| *(unnamed)* | Helio G100 Ultra | Mali-G57 MC2 | ✅ |
| Infinix Note 30 5G | Dimensity 6080 | Mali-G57 MC2 | ✅ |
| *(unnamed)* | Dimensity 6080 | Mali-G57 MC2 | ✅ |
| Samsung Galaxy A15 5G | Dimensity 6100+ | Mali-G57 MC2 | ✅ |
| *(unnamed)* | Unisoc T820 | Mali-G57 | ✅ |
| Tecno Camon 20 Pro 5G | Dimensity 8050 | Mali-G77 MC9 | ✅ |
| *(unnamed)* | Kompanio 1300T | Mali-G77 MC9 | ✅ |
| *(unnamed)* | — | Mali-G78 MP20 | ✅ |
| Redmi Note 12 Pro 5G | Dimensity 1080 | Mali-G68 MC4 | ✅ |
| Samsung Galaxy Tab S9 FE+ | Exynos 1380 | Mali-G68 MP5 | ✅ |
| Redmi Note 10S | Helio G95 | Mali-G76 MC4 | ✅ |
| Poco M5s | Helio G95 | Mali-G76 MC4 | ✅ |
| *(unnamed)* | — | Mali-G76 MC4 | ✅ |
| Oppo A38 | Helio G85 | Mali-G52 MC2 | ✅ |
| Xiaomi Redmi 13C | Helio G85 | Mali-G52 MC2 | ✅ |
| *(unnamed)* | — | Mali-G72 MP3 | ✅ |
| Samsung F07 | — | — | ✅ |
| *(unnamed)* | Unisoc T606 | Mali-G57 | ❓ |
| *(unnamed)* | Exynos 2100 | Mali-G78 | ❓ |
| *(unnamed)* | Exynos 1280 | Mali-G68 | ❓ |
| *(unnamed)* | Dimensity 700 | Mali-G57 | ❓ |

### By GPU family

| Family | GPUs | Status |
|---|---|---|
| Valhall v9 (Job Manager) | G57, G68, G77, G78 | ✅ Main target |
| Bifrost v7 | G52, G76 | ✅ (DXVK 1.x) |
| Bifrost v6 | G71, G72 | ✅ Confirmed on G72 MP3 |
| Valhall 5th gen (CSF) | G610, G710, G615, Immortalis | ❌ Not supported |
| Midgard | T-series | ❌ Not supported |

Your phone isn't listed? Try it and tell us the result in an [Issue](../../issues).

---

## Environment variables (advanced)

| Variable | Default | What it does |
|---|---|---|
| `PANVK_POLY_HEAP_MB` | `16` | Geometry/tessellation scratch heap (4–512). Raise it only if a game draws missing geometry. |
| `PANVK_TILER_HEAP_MB` | `512` | Tiler heap limit (16–2048). Lower values may save RAM in heavy games. |
| `PANVK_HEAP_MB` | 25 % of RAM (1024–3072) | GPU memory reported to games (256–16384). Raise it only if a heavy game says it lacks video memory; lower it if Android kills the app. |
| `PANVK_STENCIL_LATE` | `0` | `1` fixes black triangles / missing light in some games. Leave it off for other games: it can make them darker and slower. |
| `PANVK_TRACE` | off | `1` prints driver debug output to the Wine log. |

> ⚠️ **Only use the variables in this table.** The driver has other internal `PANVK_*` switches
> for development and testing. Setting them can break rendering, lower FPS or crash games.

---

## Known issues

- Some heavy D3D12 games can hang while loading. Use the game's D3D11 mode for now; a fix is planned for 3.5.
- Some games show graphics glitches or wrong / blocky textures.
- Games that use geometry or tessellation shaders may draw incorrectly.
- BCn → ASTC texture conversion can freeze or reboot the phone. Set BCn to "none".
- Bifrost (G52 / G76 / G72): DXVK 2.x doesn't start yet; use DXVK 1.x (next update).
- Switch emulators: after a pause, the picture can look too bright / blue until the next level.
- Bannerlator's **"surfaceflinger"** render option shows wrong colors (blue sky turns red). Use the default renderer.
- DXVK 2.5+: the DXVK HUD text is squashed into one line. Rendering itself is fine.
- Heavy games can still be slower than on the stock driver.

---

## How to report a problem

Open an [Issue](../../issues) and include:

1. Phone model, SoC, GPU and kbase version (shown in AIO Graphics Test → GPU Info).
2. App (Winlator build), wrapper, DXVK version, Box64/FEX version.
3. What happens (black screen, crash, freeze, low FPS…).
4. Logs:
   - **Wine log:** enable Wine debug in the container settings, then attach `wine_debug.log`.
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

---

## License

The driver is built from Mesa (MIT license; see `LICENSE-Mesa.txt` in the zip).
This repository is licensed under the MIT License.
