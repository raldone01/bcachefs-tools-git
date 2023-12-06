# Maintainer: Frederik Schwan <freswa at archlinux dot org>
# Contributor: Jelle van der Waa <jelle@archlinux.org>

pkgname=bcachefs-tools
epoch=3
pkgver=1.3.5
pkgrel=3
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
b2sums=('5599218dea4957d3bca8eeaebb1f9cc16004b24b1af1b6ce7c22a263dd4bedfe1193fe05d9ea38e082eb2af26ff026119e82cd946ab979ad8902bb752eeb0bbc')

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
