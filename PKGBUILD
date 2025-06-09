# Maintainer: Alvaro Oliveira <alvaro.oliveira@mail.com>

pkgname=dmenu-xft-emoji
pkgver=5.2
pkgrel=1
pkgdesc="dmenu with color‑font (emoji) and CJK fallback support"
arch=('x86_64')
url="https://github.com/limaon/dmenu-xft-emoji"
license=('MIT')

depends=('libxft' 'libxinerama' 'fontconfig')
optdepends=('terminus-font: bitmap Terminus for X11')

makedepends=('git')

provides=('dmenu')
conflicts=('dmenu' 'dmenu-git')

source=(
  "https://dl.suckless.org/tools/dmenu-${pkgver}.tar.gz"
  "dmenu-allow-color-font-5.2.diff"
  "config.h"
)
sha256sums=('SKIP' 'SKIP' 'SKIP')

prepare() {
  cd "${srcdir}/dmenu-${pkgver}"
  patch -Np1 --silent -i "${srcdir}/dmenu-allow-color-font-5.2.diff"
  cp "${srcdir}/config.h" .
}

build() {
  cd "${srcdir}/${pkgname}"
  make
}

package() {
  cd "${srcdir}/${pkgname}"
  make DESTDIR="${pkgdir}" PREFIX=/usr install
  install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
