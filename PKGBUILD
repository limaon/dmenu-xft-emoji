pkgname=dmenu-xft-emoji
pkgver=5.2
pkgrel=2
pkgdesc="dmenu with color-font (emoji) and CJK fallback support"
arch=('x86_64')
url="https://tools.suckless.org/dmenu/"
license=('MIT')

depends=('libxft' 'libxinerama' 'fontconfig')
optdepends=('terminus-font: bitmap Terminus for X11')

source=(
  "https://dl.suckless.org/tools/dmenu-${pkgver}.tar.gz"
  "dmenu-allow-color-font-5.2.diff"
  "config.h"
)
sha256sums=('d4d4ca77b59140f272272db537e05bb91a5914f56802652dc57e61a773d43792'
            '5d0ad90d272b7a11dce9da914e2777e6e168b38070c02ed057f1d26532e60500'
            '61d81dce7b68fe6fb3b7581a2007ef4e44251e42fbcce7ad1d7b771ea47b6454')

prepare() {
  cd "${srcdir}/dmenu-${pkgver}"
  patch -Np1 --silent --forward -i "${srcdir}/dmenu-allow-color-font-5.2.diff" || true
  cp "${srcdir}/config.h" .
}

build() {
  cd "${srcdir}/dmenu-${pkgver}"
  make
}

package() {
  cd "${srcdir}/dmenu-${pkgver}"
  make DESTDIR="${pkgdir}" PREFIX=/usr install
  install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
