# Maintainer: Frederik Schwan <freswa at archlinux dot org>
# Contributor: Jelle van der Waa <jelle@archlinux.org>

pkgname=bcachefs-tools
epoch=3
pkgver=1.3.1
pkgrel=1
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
b2sums=('a1d54feefecc6fb0fcae73e81dd7afbd65302573918e7fdad7a2a74566590fee666dbb7c99af5de069ff5fcfe746baf74f4889bbebf547f6b29b1944e0253993')

build() {
  cd ${pkgname}-${pkgver}
  make
}

package() {
  cd ${pkgname}-${pkgver}
  make DESTDIR="$pkgdir" PREFIX="/usr" ROOT_SBINDIR="/usr/bin" \
       INITRAMFS_DIR="/etc/initcpio" install
  # remove initcpio hooks that seems incompatible with mkinitcpio
  rm -rf "${pkgdir}"/etc/initcpio
}
