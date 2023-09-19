pkgname=bcachefs-tools
pkgver=1.2
pkgrel=1
pkgdesc="BCacheFS filesystem utilities"
url="https://github.com/koverstreet/bcachefs-tools"
arch=("x86_64")
license=("GPL2")
depends=(util-linux)
makedepends=(cargo git pkgconf libsodium libaio util-linux-libs
             lz4 liburcu zstd keyutils valgrind llvm clang)
source=("${pkgname}-${pkgver}.tar.gz"::https://github.com/koverstreet/bcachefs-tools/archive/refs/tags/v${pkgver}.tar.gz)
sha256sums=('2f7b68576303bcbb80ea6c4042aa27b1b1027739f3de68106ca9166963e161dc')

build() {
  cd "${pkgname}-${pkgver}"
  make
}

package() {
  cd "${pkgname}-${pkgver}"
  make DESTDIR="$pkgdir" PREFIX="/usr" ROOT_SBINDIR="/usr/bin" \
       INITRAMFS_DIR="/etc/initcpio" install
}
