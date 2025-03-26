# Maintainer: Frederik Schwan <freswa at archlinux dot org>
# Contributor: Jelle van der Waa <jelle@archlinux.org>

pkgname=bcachefs-tools
epoch=3
pkgver=1.25.0
pkgrel=1
pkgdesc='BCacheFS filesystem utilities'
arch=('x86_64')
url='https://bcachefs.org/'
license=('GPL-2.0-only')
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
options=(!lto)
source=(
  "${pkgname}-${pkgver}.tar.gz"::https://github.com/koverstreet/bcachefs-tools/archive/refs/tags/v${pkgver}.tar.gz
)
b2sums=('c57e8f53603819ea85e74e68c5047934b7b3db32d52b717b69117c7acb85515f09a6aa2a153daa8d8d99a987fac146ad222a69e65b0e219e449ebafc87259f9d')

build() {
  cd ${pkgname}-${pkgver}

  # this uses malloc_usable_size, which is incompatible with fortification level 3
  # https://github.com/koverstreet/bcachefs-tools/issues/237
  export CFLAGS="${CFLAGS/_FORTIFY_SOURCE=3/_FORTIFY_SOURCE=2}"
  export CXXFLAGS="${CXXFLAGS/_FORTIFY_SOURCE=3/_FORTIFY_SOURCE=2}"

  make \
    LIBEXECDIR=/usr/lib \
    DESTDIR="${pkgdir}" \
    ROOT_SBINDIR="/usr/bin" \
    INITRAMFS_DIR="/usr/lib/initcpio/"
}

package() {
  cd ${pkgname}-${pkgver}

  # this uses malloc_usable_size, which is incompatible with fortification level 3
  # https://github.com/koverstreet/bcachefs-tools/issues/237
  export CFLAGS="${CFLAGS/_FORTIFY_SOURCE=3/_FORTIFY_SOURCE=2}"
  export CXXFLAGS="${CXXFLAGS/_FORTIFY_SOURCE=3/_FORTIFY_SOURCE=2}"

  make \
    PREFIX="/usr" \
    LIBEXECDIR=/usr/lib \
    DESTDIR="${pkgdir}" \
    ROOT_SBINDIR="/usr/bin" \
    INITRAMFS_DIR="/usr/lib/initcpio/" \
    install

  # replace incompatible initcpio hooks
  rm -rf "${pkgdir}"/usr/lib/initcpio/*
  install -dm755 "${pkgdir}"/usr/lib/initcpio/{hooks,install}
  install -Dm644 arch/etc/initcpio/hooks/bcachefs "${pkgdir}"/usr/lib/initcpio/hooks/
  install -Dm644 arch/etc/initcpio/install/bcachefs "${pkgdir}"/usr/lib/initcpio/install/

  # package completions
  install -dm755 "${pkgdir}"/usr/share/{bash-completion/completions,fish/vendor_completions.d,zsh/site-functions}
  "${pkgdir}"/usr/bin/bcachefs completions bash > "${pkgdir}"/usr/share/bash-completion/completions/bcachefs
  "${pkgdir}"/usr/bin/bcachefs completions fish > "${pkgdir}"/usr/share/fish/vendor_completions.d/bcachefs.fish
  "${pkgdir}"/usr/bin/bcachefs completions zsh > "${pkgdir}"/usr/share/zsh/site-functions/_bcachefs
}
