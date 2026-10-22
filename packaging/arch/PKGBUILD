# Maintainer: IPedrax
# Based on CachyOS's wine-cachyos PKGBUILD (loathingkernel, Peter Jung).

# shortcut: packages the tree already built in $_builddir (configured with
# --prefix=/usr) instead of building from git. Run make there first; switch to
# a git source and a build() step before sharing this package.
_builddir="$HOME/build/winguin"

pkgname=winguin
pkgver=11.0.20261014.g686ab11
pkgrel=2
_geckover=2.47.4
_monover=10.4.1
pkgdesc="Wine with CachyOS and Proton patches, using your own user name and home folders"
url="https://github.com/IPedrax/Winguin"
arch=(x86_64)
license=(LGPL-2.1-or-later)
options=(staticlibs !strip !debug !lto)
depends=(
  attr
  desktop-file-utils
  fontconfig
  freetype2
  libgcc
  gettext
  glib2
  glibc
  libpcap
  libunwind
  libx11
  libxcursor
  libxext
  libxkbcommon
  libxi
  libxrandr
  systemd-libs
  wayland
)
provides=("wine=11.0")
conflicts=(wine wine-mono wine-gecko)
backup=(etc/xdg/mimeapps.list)
source=(https://dl.winehq.org/wine/wine-gecko/${_geckover}/wine-gecko-${_geckover}-x86{,_64}.tar.xz
        https://github.com/madewokherd/wine-mono/releases/download/wine-mono-${_monover}/wine-mono-${_monover}-x86.tar.xz
        30-win32-aliases.conf
        mimeapps.list)
b2sums=('2a73c12585b502ae11188482cbc9fb1f45f95bfe4383a7615011104b132f4845f9813d01fb40277e1934fab5f1b35ab40b4f4a66a9967463dd1d666a666904e9'
        '62856a88266b4757602c0646e024f832974a93f03b9df253fd4895d4f11a41b435840ad8f7003ec85a0d8087dec15f2e096dbfb4b01ebe4d365521e48fd0c5c0'
        '9ca53dee272470806432c61587080e6dc04fd9eaafde4f55f5d57d5557ec6859d77a74b74c9e3f472da04b8ace9609f0927573faab368a25249c76b3e37e65c1'
        '45db34fb35a679dc191b4119603eba37b8008326bd4f7d6bd422fbbb2a74b675bdbc9f0cc6995ed0c564cf088b7ecd9fbe2d06d42ff8a4464828f3c4f188075b'
        'd255ba2d33ae36c563548f4316a668452095c475ab57fb10386fe4a676cd911f403061e3b6e2324852ade401bfb1277bdffd94c8cd9a5a23b4a292e673247173')

package() {
  make -C "$_builddir" prefix="$pkgdir/usr" libdir="$pkgdir/usr/lib" \
    dlldir="$pkgdir/usr/lib/wine" install

  install -d "$pkgdir"/usr/share/fontconfig/conf.{avail,default}
  install -m644 30-win32-aliases.conf "$pkgdir/usr/share/fontconfig/conf.avail"
  ln -s ../conf.avail/30-win32-aliases.conf "$pkgdir/usr/share/fontconfig/conf.default/30-win32-aliases.conf"

  # Runs Windows executables from a terminal: ./app.exe
  install -d "$pkgdir/usr/lib/binfmt.d"
  echo ':DOSWin:M::MZ::/usr/bin/wine:' > "$pkgdir/usr/lib/binfmt.d/wine.conf"

  # Double-clicking .exe files runs them, for every user without their own default
  install -Dm644 mimeapps.list "$pkgdir/etc/xdg/mimeapps.list"

  install -d "$pkgdir"/usr/share/wine/{gecko,mono}
  cp -dr --no-preserve=ownership wine-gecko-${_geckover}-x86{,_64} "$pkgdir/usr/share/wine/gecko/"
  cp -dr --no-preserve=ownership wine-mono-${_monover} "$pkgdir/usr/share/wine/mono/"
}
