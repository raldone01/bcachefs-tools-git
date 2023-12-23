# Maintainer: Frederik Schwan <freswa at archlinux dot org>
# Contributor: Jelle van der Waa <jelle@archlinux.org>

pkgname=bcachefs-tools
epoch=3
pkgver=1.3.6
pkgrel=1
pkgdesc='BCacheFS filesystem utilities'
arch=('x86_64')
url='https://bcachefs.org/'
license=('GPL2')
depends=(
  bash
  fuse3
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
  udev
)
makedepends=(
  cargo
  clang
  llvm
  pkgconf
  valgrind
)
source=(
  "${pkgname}-${pkgver}.tar.gz"::https://github.com/koverstreet/bcachefs-tools/archive/refs/tags/v${pkgver}.tar.gz
)
b2sums=('9a005e79b3956f745958d56e5053f1085fef0c30bac3e3be2c1cf79a24bfe43b1bdf14ce9ec036c88c1b4e85da7b07e6b08ebc7e08cf8db11b15e0a88aa53065')

build() {
  cd ${pkgname}-${pkgver}
  BCACHEFS_FUSE=1 make
}

package() {
  cd ${pkgname}-${pkgver}
  make DESTDIR="${pkgdir}" PREFIX="/usr" ROOT_SBINDIR="/usr/bin" \
       INITRAMFS_DIR="/etc/initcpio" install
  # remove initcpio hooks that seems incompatible with mkinitcpio
  rm -rf "${pkgdir}"/etc

  # package completions
  install -dm755 "${pkgdir}"/usr/share/{bash-completion/completions,fish/vendor_completions.d,zsh/site-functions}
  "${pkgdir}"/usr/bin/bcachefs completions bash > "${pkgdir}"/usr/share/bash-completion/completions/bcachefs
  "${pkgdir}"/usr/bin/bcachefs completions fish > "${pkgdir}"/usr/share/fish/vendor_completions.d/bcachefs.fish
  "${pkgdir}"/usr/bin/bcachefs completions zsh > "${pkgdir}"/usr/share/zsh/site-functions/_bcachefs
}
