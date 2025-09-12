# Maintainer: Frederik Schwan <freswa at archlinux dot org>
# Contributor: Jelle van der Waa <jelle@archlinux.org>

pkgbase=bcachefs-tools
pkgname=(bcachefs-tools bcachefs-dkms)
epoch=3
pkgver=1.25.3
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
  https://github.com/koverstreet/bcachefs-tools/pull/404.patch
)
b2sums=('cbd14c4466c90834442eb5a2a9a14f81faa66e62169635e1a13671dc3df448ef1494fa69e7a109bcc1f6b0ba9d57b0a979efd2138dc3657cfa04f106c909f402'
        '88f44e47c38f3bbd5bf23adaf4b1a6f1580bc9f419c16df81c1ea3d7f251a653f80122546378abbdbca1f5f5eaac40f7ed726708ab1424009b5859248294f8b9')

prepare() {
  cd ${pkgname}-${pkgver}
  patch -Np1 < ../404.patch
}

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

package_bcachefs-tools() {
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

package_bcachefs-dkms() {
  depends=()
  cd ${pkgbase}-${pkgver}

  make \
    PREFIX="/usr" \
    LIBEXECDIR=/usr/lib \
    DESTDIR="${pkgdir}" \
    ROOT_SBINDIR="/usr/bin" \
    INITRAMFS_DIR="/usr/lib/initcpio/" \
    install_dkms
}
