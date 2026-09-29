# FristOneRR PanVK Driver for Mali (kbase)

An open-source Vulkan driver (Mesa **PanVK**) for Mali GPUs on Android phones that use
Arm's vendor **kbase** kernel driver. It is built for **Winlator** and **DXVK**, so Windows games
can run through Vulkan without the stock Mali driver.

> **Status: beta.** It works well on many Helio G99 / Mali-G57 devices, but it is not a
> conformant Vulkan implementation. Expect bugs and please report them.

**Source code:** [FristOneRR-Admin/FristOneRR-Panvk-Source](https://github.com/FristOneRR-Admin/FristOneRR-Panvk-Source)

---

## What's new in beta 1.1.0

- **DXVK 2.x and 3.x now work for D3D9** (tested 2.0 → 3.1.1). Needs the app's built-in *Wrapper original*; see [Wrappers](#wrappers).
- **Mali-G52 on older kernels (e.g. Oppo A38, kbase r49):** the driver now detects the job-submit layout by itself. The special s56 build is no longer needed.
- **Mali-G52 r1** is now shown by name instead of "Mali unknown 0x74021000".
- **Lower RAM use:** about **112 MB less per Vulkan device**, a big help on 4–6 GB phones.
- G68 (Dimensity 1080) and G77 / G78 are recognized by name.

---

## Installation (step by step)

**What you need**
- A Mali phone from the [compatibility list](#device-compatibility).
- A Winlator build that can **install custom Vulkan drivers** (adrenotools), for example **Winlator Ludashi** or **Bannerlator**.
  Some forks (e.g. some "Winlator Mali" builds) have no driver install option. They can't load this driver.

**Steps**
1. Download `Panvk-Mali-G57-beta_1.1.0.zip` from [Releases](../../releases). **Don't unzip it.**
2. Open Winlator → **Contents / Drivers → Install**, then pick the **zip file itself**.
3. Open your container's settings (**Edit container**) and set the graphics driver to **Wrapper**.
   In the wrapper/driver settings, choose **Panvk-Mali-G57-beta_1.1.0** as the Vulkan driver.
4. Pick the wrapper and DXVK version from the [DirectX table](#directx-support) below.
5. Start the container and check it's working: open **AIO Graphics Test → GPU Info**. The GPU should show as **Mali-…** with about **130+ extensions**.
   The extension count shown in the driver menu does **not** change; only GPU Info inside a running container shows the real number.

**Troubleshooting**
- *"Error" when installing the zip:* make sure you picked the original zip (not an extracted folder or a re-zipped copy).
- *No install button:* your Winlator build doesn't support custom drivers. Use Ludashi or Bannerlator.
- *A game crashes on start:* try turning **off BCn texture emulation** in the wrapper settings.

---

## DirectX support

| API | DXVK 1.10.3 + leegao wrapper | DXVK 2.0 – 3.1.1 + Wrapper original | vkd3d-proton |
|---|:---:|:---:|:---:|
| **D3D9**  | ✅ | ✅ | — |
| **D3D10** | ❔ untested | ❌ | — |
| **D3D11** | ✅ | ❌ | — |
| **D3D12** | — | — | ❌ |

**Tested DXVK builds for D3D9:** 2.0, 2.3-gplasync, 2.3.1, 2.4.1, 2.5.3, 2.6.2,
2.6.2-gplasync, 2.6.2-arm64ec-gplasync, 2.7.1, 3.1.1

**Why D3D10/11 still need DXVK 1.10.3:** DXVK 2.x requires tessellation, transform feedback
and geometry shaders. Mali Valhall v9 (G57/G68/G77/G78) doesn't have these in hardware yet,
and DXVK 1.10.3 doesn't need them.

**Unity games (D3D11):** if you see *"Failed to initialize player"*, try **DXVK-Sarek**.

---

## Wrappers

> ⚠️ **Important**
> - **D3D9 with DXVK 2.x / 3.x → use the app's built-in "Wrapper original".**
>   **Do NOT use the leegao wrapper with DXVK 2.x+.** It crashes on the first frame: the FPS counter flashes, then the app closes.
> - **D3D11 → use DXVK 1.10.3 + the leegao wrapper.** Wrapper original doesn't support D3D11 yet.
> - **Other wrappers:** users report that DXVK 2.x/3.x also works (slower, with some bugs) on **Bannerlator**, **Ludashi** and the **GameNative** wrapper. Pipetto-crypto's wrapper is untested.

| Wrapper | Recommended for |
|---|---|
| **leegao** (bionic-vulkan-wrapper) | DXVK 1.10.3: D3D9 / D3D11 |
| **Wrapper original** (built into the app) | DXVK 2.x / 3.x: D3D9 |
| Bannerlator | Fallback (ships the leegao wrapper) |

---

## Device compatibility

### Confirmed by real users

| Device | SoC | GPU | kbase | Result |
|---|---|---|---|---|
| Poco M6 Pro 4G *(dev device)* | Helio G99 | Mali-G57 MC2 | — | ✅ Full test suite; NFS Most Wanted 50–70 FPS |
| Samsung Galaxy A15 | Helio G99 | Mali-G57 MC2 | r54p1 | ✅ Tomb Raider Anniversary faster than stock; Tomb Raider 2013 slow |
| Realme 10 4G | Helio G99 | Mali-G57 MC2 | — | ✅ NFS Hot Pursuit 2010 at 25–30 FPS |
| Infinix Note 50 Pro | Helio G100 Ultimate | Mali-G57 MC2 | — | ✅ 66 → 144 extensions |
| *(unnamed)* | Helio G100 Ultra | Mali-G57 MC2 | r32p | ✅ DXVK 1.11 / 1.12 work |
| *(unnamed)* | Dimensity 6080 | Mali-G57 MC2 | r32p1 | ✅ Works (Hades reboots the device) |
| Oppo A38 | Helio G85 | Mali-G52 r1 MC2 | r49.1 | ✅ D3D9/10/11 + Zink (automatic stride detection in 1.1.0) |
| *(unnamed)* | Kompanio 1300T | Mali-G77 MC9 | r32p1 | ✅ Prince of Persia (2008) with DXVK 2.3.1-gplasync ~30 FPS |
| Redmi Note 10S | Helio G95 | Mali-G76 MC4 | r26p0 | ✅ NFS Underground 2 runs (~15 FPS, stock ~60); GTA III (re3) freezes on loading; 57 → 135 extensions |

### Partly working / in progress

| Device | SoC | GPU | Status |
|---|---|---|---|
| Redmi Note 12 Pro 5G | Dimensity 1080 | Mali-G68 MC4 | ⚠️ Recognized since 1.1.0; Black Mesa stuck on loading |
| Poco M5s | Helio G95 | Mali-G76 MC4 | ⚠️ Loads, freezes on GPU info |
| Samsung Galaxy A15 5G | Dimensity 6100+ | Mali-G57 MC2 | ⚠️ Works on Bannerlator; Resident Evil 4 renders incorrectly |
| Xiaomi Redmi 13C | Helio G85 | Mali-G52 MC2 | ⚠️ Hangs on GPU Info with 1.0.0 (1.1.0 should fix it: automatic stride detection) |
| *(unnamed)* | — | Mali-G76 MC4 | ⚠️ Loads (61 → 137 extensions), no game results yet |
| Samsung F07 | — | — | ⚠️ Dark Souls crashes during shader compile (log pending) |

### Not working

| Device | SoC | GPU | Note |
|---|---|---|---|
| Samsung Galaxy Tab S9 FE+ | **Exynos 1380** | Mali-G68 MP5 | ❌ Does not load |

> **Samsung phones:** Samsung devices with **MediaTek** chips (e.g. Galaxy A15) work.
> **Samsung Exynos** chips are **not supported yet** (tested: Exynos 1380).

### By GPU family

| Family | GPUs | Status |
|---|---|---|
| Valhall v9 (Job Manager) | G57, G68, G77, G78 | ✅ Main target |
| Bifrost v7 | G52, G76 | ✅ Confirmed on G52 r1 and G76 MC4 (slower than Valhall, DXVK 1.x only) |
| Bifrost v6 | G71, G72 | ❔ Untested |
| Valhall 5th gen (CSF) | G610, G710, G615, Immortalis | ❌ Not supported |
| Midgard | T-series | ❌ Not supported |
| Samsung Exynos | any | ❌ Not supported yet |

---

## Environment variables (advanced)

| Variable | Default | What it does |
|---|---|---|
| `PANVK_ATOM_STRIDE` | auto | Force the job-submit stride: `64` or `56`. Only set this if auto-detection fails (you see `[KFAULT]` in the log). |
| `PANVK_POLY_HEAP_MB` | `16` | Geometry/tessellation scratch heap (4–512). Raise it only if a game draws missing geometry. |
| `PANVK_TILER_HEAP_MB` | `512` | Tiler heap limit (16–2048). Lower values may save RAM in heavy games. |
| `PANVK_TRACE` | off | `1` prints driver debug output to the Wine log. |

---

## Known issues

- DXVK 2.x on Mali-G57: some games show graphics glitches (e.g. speckled textures).
- D3D11 doesn't work with *Wrapper original* (use DXVK 1.10.3 + leegao).
- DXVK 2.x + leegao wrapper crashes on the first frame (use *Wrapper original* for D3D9).
- DXVK 2.5+: the HUD text is squashed into one line. Rendering itself is fine.
- D3D12 (vkd3d-proton) runs but draws nothing, then closes.
- Tomb Raider 2013 is much slower than on the stock driver.
- Some Mali-G76 devices (Helio G95) freeze on start.
- Bifrost (G52/G76): DXVK 2.x doesn't start yet (robustness2 is only enabled on Valhall v9 so far).

---

## How to report a problem

Open an [Issue](../../issues) and include:

1. Phone model, SoC, GPU and kbase version (shown in AIO Graphics Test → GPU Info).
2. Winlator build, wrapper, DXVK version, Box64/FEX version.
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
| **funnymdzz** (mesa fork) & **leegao** | The Mesa source used as our base; bionic Vulkan wrapper; advice throughout the project |
| **Noysz** / [panvk-g99-jm](https://github.com/Noysz/panvk-g99-jm) | Valhall v9 / Job Manager groundwork (included in the base we started from); also our reference for the v9 draw path |
| **Vtgamer998** (MESA-KMOD) | kbase kernel-interface work |
| **mexicanbr0auth** / [mesa-panvk-g57](https://github.com/mexicanbr0auth/mesa-panvk-g57) | PanVK/kbase work for Mali-G57; parts of our code came from this project |
| **wonderkast02** / [panvk-g720-kbase-csf](https://github.com/wonderkast02/panvk-g720-kbase-csf) | Community PanVK-over-kbase work (Mali-G720, CSF) |
| **LukeValen** / [0x8055/panvk-g52-oppo-a38](https://github.com/0x8055/panvk-g52-oppo-a38) | Mali-G52 on Oppo A38 research |
| **BossDrk** | Mali-G52 (Oppo A38) testing: found and verified the 56-byte stride fix |
| **Claude** (AI assistant by Anthropic) | Development help, debugging and code review; audited the code origin and helped write this credit list |
| **All testers** who opened issues and sent logs | Device reports and game results |

**Code origin (measured at 1.1.0):** of the 1,669 lines we added on top of Mesa, **67 %** don't appear anywhere else, **3.6 %** match mexicanbr0auth, **2.4 %** match Noysz and **0.7 %** match LukeValen; the rest is Mesa code we moved or reused. Full table, per-file details, an apology for the missing credits in 1.0.0, and a script to re-check it yourself are in the [source repository](https://github.com/FristOneRR-Admin/FristOneRR-Panvk-Source).

> **Credits update (1.1.0):** beta 1.0.0 was released without these credits. That was my mistake as a first-time GitHub user, and I'm sorry. The credits above were added on the day the source code was published.

---

## License

The driver is built from Mesa (MIT license; see `LICENSE-Mesa.txt` in the zip).
This repository is licensed under the MIT License.
