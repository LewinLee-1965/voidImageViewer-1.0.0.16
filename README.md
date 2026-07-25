# void Image Viewer

**Version 1.0.0.16** — a lightweight Windows image viewer with animated GIF/WEBP support, extended for **machine-vision workflows**: fast opening of very large images, pixel-level zoom/pan, and live **pixel coordinates + pixel values** in the status bar.

> **Note:** This build is based on [voidImageViewer 1.0.0.15](https://github.com/voidtools/voidImageViewer/releases/tag/1.0.0.15) (by [voidtools](https://www.voidtools.com/) / David Carpenter) and extends it with features aimed at inspecting high-resolution images in machine-vision and industrial imaging scenarios.

Opens and displays **BMP, GIF, ICO, PNG, JPG, TIF, and WEBP** images as fast as possible.  
Animates GIF/WEBP files as accurately as possible.

[Features](#features) · [Machine vision](#machine-vision--pixel-inspection) · [Screenshots](#screenshots) · [What's new](#whats-new-in-10016) · [Build](#build) · [Download](#download)

---

## Why this fork / extension?

Upstream void Image Viewer is already excellent for everyday browsing. Version **1.0.0.16** keeps that DNA and adds tooling that vision engineers commonly need when reviewing camera dumps, AOI captures, or tiled high-res assets:


| Need                                | How 1.0.0.16 helps                                                    |
| ----------------------------------- | --------------------------------------------------------------------- |
| Inspect huge frames without waiting | Fast open path, mipmaps, preload / cache                              |
| Zoom to true source pixels          | Discrete zoom presets + 1:1 / Best Fit / Fill Window                  |
| Read exact sample under the cursor  | Status bar shows `POS: x,y` and `RGB` / `Gray`                        |
| Stay in Explorer workflows          | Optional context-menu entry (does not change the default association) |


---

## Features

### Core viewer (from the 1.0.0.15 line)

- **Fast open & display** for BMP, GIF, ICO, PNG, JPG, TIF, WEBP
- **Accurate GIF / animated WebP** playback with play, pause, frame step, and rate control
- **View modes:** 1:1, Best Fit, Fill Window, Pan/Scan, fullscreen, customizable zoom
- **Navigation:** previous / next, playlist, natural sort, shuffle, Jump To
- **File tools:** copy / move / delete / rename / print / set wallpaper / properties
- **EXIF orientation** via `System.Photo.Orientation`
- **Everything** search integration ([voidtools Everything](https://www.voidtools.com/))
- **Performance:** mipmap shrinking, preload next image, cache last image
- **Portable-friendly** settings via `voidImageViewer.ini`
- **Customizable** keyboard & mouse actions

### Extended for machine vision (1.0.0.16)

- **Live pixel probe** in the status bar (enabled by default)
  - Color images: `RGB: r,g,b`
  - Grayscale images: `Gray: n`
  - Source coordinates: `POS: x,y` (image space, not window space)
- **Large-image reliability** fixes for extreme widths/heights and preview rendering
- **Explorer context menu** option to open supported images without replacing the default viewer

Toggle pixel info with INI key `statusbar_pixel_info` (or the corresponding UI option). Default is **on**.

---

## Machine vision & pixel inspection

When you move the mouse over the image, the status bar’s **bottom-left** parts update in real time:

Status bar pixel probe layout

**Example readings**


| Image type | Status parts                         |
| ---------- | ------------------------------------ |
| Color      | `RGB: 128,64,200` · `POS: 3840,2160` |
| Grayscale  | `Gray: 180` · `POS: 1024,768`        |


Coordinates are **source-pixel** positions after accounting for zoom, pan, and fit modes — suitable for correlating with algorithm ROIs, defect lists, or calibration points.

Pixel-level zoom for high-resolution inspection

Live RGB / Gray and POS on the status bar

**Typical workflow**

1. Open a high-resolution capture (drag-drop, Open File, or Explorer context menu).
2. Use **Zoom In** / mouse wheel (as configured) or switch to **1:1** for true pixel scale.
3. Pan with middle-mouse / configured left-click action / numpad.
4. Read `POS` + `RGB`/`Gray` on the status bar while hovering defect candidates or ROI corners.

---

## Screenshots

### Main window (upstream UI)

![Main window](docs\screenshots\main.png)

### Options — General

![Options](docs\screenshots\options.png)

In **1.0.0.16**, General options also include **Add VoidImageViewer to Explorer context menu** (per-user, no admin required; does **not** change the default open program).

### View huge frame & Source pixels

![View huge frame](docs\screenshots\hugeImageView.png)

## What's new in 1.0.0.16

Based on **1.0.0.15**, with the following highlights (see `[Changes.txt](Changes.txt)` for the full log):

- Improved status-bar pixel info: **Gray** for grayscale images, **RGB** for color; shown at the bottom-left; **enabled by default**
- Option to add VoidImageViewer to the **Explorer context menu** for supported image types
- Fixed preview rendering black for images with width or height of **100000** or more
- Fixed mipmap level selection incorrectly comparing height against width
- Resource script includes `winres.h` instead of `afxres.h` for broader Visual Studio builds

**Inherited from recent upstream releases (selected):** large-image scrolling fixes, WebP improvements, mipmaps, preload, cache-last, title-bar format INI, EXIF orientation, slideshow/animation sleep prevention, and more — see `Changes.txt`.

---

## Requirements

- **OS:** Windows (Win32 / x64 / ARM / ARM64 supported by the project)
- **Runtime:** GDI+ (`gdiplus.dll`, normally present on modern Windows)
- **Optional:** [Everything](https://www.voidtools.com/) for IPC search features

---

## Build

Recommended toolchain: **Visual Studio 2019** (v142) + Windows 10 SDK.

1. Open `[vs2019/voidImageViewer.sln](vs2019/voidImageViewer.sln)`
2. Select **Release** | **x64** (also verify Win32 if needed)
3. Build — the solution compiles the app sources and the required **libwebp** units (no separate top-level libwebp build required)

Legacy VS2005 projects live under `[vs2005/](vs2005/)`. Installer scripts are under `[nsis/](nsis/)`.

Developer docs (Chinese):

- `[docs/程序基本架构说明书.md](docs/程序基本架构说明书.md)`
- `[docs/升级开发指导建议书.md](docs/升级开发指导建议书.md)`

---

## Command-line (selected)

```text
voidImageViewer.exe [options] [file|folder ...]
```

Useful switches include `/slideshow`, `/fullscreen`, `/shuffle`, `/rate`, window geometry (`/x` `/y` `/width` `/height`), `/minimal`, `/compact`, and Everything-related options. From **1.0.0.14+**, `-switch` style is also accepted (escape with quotes or include a `.` in the token).

Run with `/?` (or see `_viv_command_line_options` in `src/viv.c`) for the complete list.

---

## Configuration

Settings are stored in `voidImageViewer.ini`. Pixel probe:


| Key                    | Default | Meaning                                         |
| ---------------------- | ------- | ----------------------------------------------- |
| `statusbar_pixel_info` | `1`     | Show live pixel value + `POS` on the status bar |


---

## Download

- Upstream releases: [https://github.com/voidtools/voidImageViewer/releases](https://github.com/voidtools/voidImageViewer/releases)  
- Discussion / support forum: [https://www.voidtools.com/forum/viewtopic.php?t=5623](https://www.voidtools.com/forum/viewtopic.php?t=5623)

This tree is **1.0.0.16** — build from source with the steps above, or package with the NSIS scripts as needed.

---

## License

- Application: **MIT** — see `[LICENSE](LICENSE)` (voidtools / David Carpenter)
- Embedded **libwebp**: Google’s BSD-style license (also in `LICENSE`)

---

## See also

- Upstream project: [https://github.com/voidtools/voidImageViewer](https://github.com/voidtools/voidImageViewer)  
- voidtools forum topic: [https://www.voidtools.com/forum/viewtopic.php?t=5623](https://www.voidtools.com/forum/viewtopic.php?t=5623)  
- Changelog: `[Changes.txt](Changes.txt)`