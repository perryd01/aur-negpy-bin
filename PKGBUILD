pkgname=negpy-bin
pkgver=0.62.0
pkgrel=1
pkgdesc="A tool for processing film negatives with GPU acceleration (AppImage)"
arch=('x86_64')
url="https://github.com/marcinz606/NegPy"
license=('GPL-3.0-only')

# Host libraries the AppImage deliberately does not bundle (see build.py
# libs_to_remove). Camera and SANE libraries are bundled; only their host
# udev rules / backends are needed, hence sane and libgphoto2.
depends=(
  'glibc'
  'gcc-libs'
  'dbus'
  'libdrm'
  'libglvnd'
  'expat'
  'fontconfig'
  'freetype2'
  'icu'
  'libjpeg-turbo'
  'libxml2'
  'onetbb'
  'systemd-libs'
  'libusb'
  'wayland'
  'libx11'
  'libxcb'
  'xcb-util-wm'
  'xcb-util-keysyms'
  'libxext'
  'libxfixes'
  'libxrender'
  'zlib'
  'sane'
  'libgphoto2'
  'vulkan-icd-loader'
)

optdepends=(
  'vulkan-radeon: AMD GPU acceleration'
  'vulkan-intel: Intel GPU acceleration'
  'nvidia-utils: NVIDIA GPU acceleration'
  'sane-airscan: network eSCL/AirScan scanner support'
)

provides=('negpy')
conflicts=('negpy' 'negpy-git')

source=(
  "${pkgname}-${pkgver}.AppImage::https://github.com/marcinz606/NegPy/releases/download/${pkgver}/NegPy-${pkgver}-x86_64.AppImage"
  "LICENSE::https://raw.githubusercontent.com/marcinz606/NegPy/${pkgver}/LICENSE"
)
sha256sums=(
  'ae3223b7d77440cf989d3191a03c2dbda2c12662ddbaabc00755aa2604f8829b'
  '3972dc9744f6499f0f9b2dbf76696f2ae7ad8af9b23dde66d6af86c9dfb36986'
)

build() {
  cd "$srcdir"
  chmod +x "${pkgname}-${pkgver}.AppImage"
  ./"${pkgname}-${pkgver}.AppImage" --appimage-extract >/dev/null
}

package() {
  cd "$srcdir"

  # The extracted AppDir keeps its internal layout so AppRun's rpath holds.
  install -d "$pkgdir/opt/negpy"
  cp -a squashfs-root/. "$pkgdir/opt/negpy/"

  install -Dm755 /dev/stdin "$pkgdir/usr/bin/negpy" <<'EOF'
#!/bin/sh
exec /opt/negpy/AppRun "$@"
EOF

  install -Dm644 squashfs-root/usr/share/applications/NegPy.desktop \
    "$pkgdir/usr/share/applications/negpy.desktop"
  sed -i 's/^Exec=.*/Exec=negpy/' "$pkgdir/usr/share/applications/negpy.desktop"

  install -Dm644 squashfs-root/negpy.png \
    "$pkgdir/usr/share/icons/hicolor/512x512/apps/negpy.png"
  install -Dm644 squashfs-root/usr/share/icons/hicolor/scalable/apps/negpy.svg \
    "$pkgdir/usr/share/icons/hicolor/scalable/apps/negpy.svg"
  install -Dm644 squashfs-root/usr/share/icons/hicolor/48x48/apps/negpy.png \
    "$pkgdir/usr/share/icons/hicolor/48x48/apps/negpy.png"

  install -Dm644 "$srcdir/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
