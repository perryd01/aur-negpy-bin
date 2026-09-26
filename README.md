# aur-negpy-bin

Arch package for [NegPy](https://github.com/marcinz606/NegPy), a tool for processing film negatives with GPU acceleration.

This package repackages the upstream AppImage and installs its extracted contents to `/opt/negpy`, with a launcher at `/usr/bin/negpy`.

## Installation

### From the AUR

`makepkg` needs the `base-devel` group:

```bash
sudo pacman -S --needed base-devel
git clone https://aur.archlinux.org/negpy-bin.git
cd negpy-bin
makepkg -si
```

`makepkg -si` builds the package, installs missing dependencies with `pacman`, and installs the result.

### Build and install a local package

`makepkg` only builds a package file; it does not install anything. `pacman -U` installs that file, resolving its `depends` from your mirrors.

```bash
cd negpy-bin
makepkg                                  # -> negpy-bin-<ver>-<rel>-x86_64.pkg.tar.zst
ls negpy-bin-*.pkg.tar.zst               # confirm the file exists
sudo pacman -U negpy-bin-<ver>-<rel>-x86_64.pkg.tar.zst
```

Then launch it:

```bash
negpy
```

### Build without dependency checks

`makepkg -si` runs `pacman` through `sudo`, which fails in a non-interactive shell. When the build dependencies are already present, skip the checks and install the resulting package file yourself:

```bash
makepkg -d                                    # -d = skip dependency checks
sudo pacman -U negpy-bin-<ver>-<rel>-x86_64.pkg.tar.zst
```

### Clean up build files

`makepkg` leaves `src/` (unpacked source) and `pkg/` (staging tree) behind.

```bash
makepkg -c            # remove work files after a successful build
makepkg -C            # remove src/ before building
rm -rf src/ pkg/      # remove manually
```

### Uninstall

```bash
sudo pacman -R negpy-bin
```

## Dependencies

The AppImage deliberately does not bundle a set of host libraries, so this package declares them. `icu`, `libxml2`, `systemd-libs` and `libusb` are bundled and therefore not host dependencies. Camera and SANE libraries are bundled; only their host udev rules and backends are needed.

Runtime essentials: `sane`, `vulkan-icd-loader`, plus the Qt/X11/Wayland libraries listed in the PKGBUILD.

Optional:
- `vulkan-radeon` — AMD GPU acceleration
- `vulkan-intel` — Intel GPU acceleration
- `nvidia-utils` — NVIDIA GPU acceleration
- `libgphoto2` — camera udev rules for camera scanning
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
