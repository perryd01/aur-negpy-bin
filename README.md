# aur-negpy-bin

Arch package for [NegPy](https://github.com/marcinz606/NegPy), a tool for processing film negatives with GPU acceleration.

This package repackages the upstream AppImage and installs its extracted contents to `/opt/negpy`, with a launcher at `/usr/bin/negpy`.

## Installation

```bash
git clone https://aur.archlinux.org/negpy-bin.git
cd negpy-bin
makepkg -si
```

## Dependencies

The AppImage deliberately does not bundle a set of host libraries, so this package declares them. Camera and SANE libraries are bundled; only their host udev rules and backends are needed.

Runtime essentials: `sane`, `libgphoto2`, `vulkan-icd-loader`, plus the Qt/X11/Wayland libraries listed in the PKGBUILD.

Optional:
- `vulkan-radeon` — AMD GPU acceleration
- `vulkan-intel` — Intel GPU acceleration
- `nvidia-utils` — NVIDIA GPU acceleration
- `sane-airscan` — network eSCL/AirScan scanner support

## Updating

For a new upstream release:

1. Update `pkgver` in the PKGBUILD.
2. Get the new AppImage hash:
   ```bash
   curl -L -o app.AppImage \
     "https://github.com/marcinz606/NegPy/releases/download/<ver>/NegPy-<ver>-x86_64.AppImage"
   sha256sum app.AppImage
   ```
3. Update the LICENSE hash the same way from
   `https://raw.githubusercontent.com/marcinz606/NegPy/<ver>/LICENSE`.
4. Regenerate metadata and build:
   ```bash
   makepkg --printsrcinfo > .SRCINFO
   makepkg -si
   ```

## Notes

- `negpy-bin` provides `negpy` and conflicts with `negpy` and `negpy-git`.
- The AppImage's own self-updater is not used; updates come through `pacman`.

## License

GPL-3.0-only. See https://github.com/marcinz606/NegPy
