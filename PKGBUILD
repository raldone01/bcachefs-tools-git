# Maintainer: Frederik Schwan <freswa at archlinux dot org>
# Contributor: Jelle van der Waa <jelle@archlinux.org>

pkgname=bcachefs-tools
epoch=3
pkgver=1.2
pkgrel=2
pkgdesc='BCacheFS filesystem utilities'
arch=('x86_64')
url='https://bcachefs.org/'
license=('GPL2')
depends=(
  bash
  gcc-libs
  libaio.so libaio
  libblkid.so libuuid.so util-linux-libs
  libkeyutils.so keyutils
  libsodium.so libsodium
  liburcu
  libz.so zlib
  libzstd.so zstd
  lz4
  libudev.so systemd-libs
)
makedepends=(
  cargo
  clang
  llvm
  pkgconf
  valgrind
)
source=("${pkgname}-${pkgver}.tar.gz"::https://github.com/koverstreet/bcachefs-tools/archive/refs/tags/v${pkgver}.tar.gz)
b2sums=('3cf7cc36c6e489b460af46ce37920a0bfed450943e886bfa056b9cf4d1051dc2c627617d37068aa6b6436aa779c180428b0910b5871acb248ccee85416a283c2')

build() {
  cd ${pkgname}-${pkgver}
  make
}

package() {
  cd ${pkgname}-${pkgver}
  make DESTDIR="$pkgdir" PREFIX="/usr" ROOT_SBINDIR="/usr/bin" \
       INITRAMFS_DIR="/etc/initcpio" install
}
