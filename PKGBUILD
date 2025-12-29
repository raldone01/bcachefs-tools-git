# Maintainer: raldone01 <raldone01 at gmail dot com>
# Contributor: Frederik Schwan <freswa at archlinux dot org>; Jelle van der Waa <jelle@archlinux.org>

_pkgname=bcachefs-tools
pkgbase=bcachefs-tools-git
pkgname=(bcachefs-tools-git bcachefs-dkms-git)
pkgver=1.34.0.r1.g572c71f5
pkgrel=1
pkgdesc='BCacheFS filesystem utilities (Git version)'
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
  git
  jq
  llvm
  pkgconf
  valgrind
)
options=(!lto)

# Configuration:
# Set exactly one of the following variables to pin a version.
# Leave all empty to build the latest master.
_tag= # for example v1.34.0
_branch=
_commit=

_url="git+https://evilpiepirate.org/git/bcachefs-tools.git"

if [[ -n "$_tag" && -z "$_branch" && -z "$_commit" ]]; then
    source=("${_url}#tag=${_tag}")
elif [[ -z "$_tag" && -n "$_branch" && -z "$_commit" ]]; then
    source=("${_url}#branch=${_branch}")
elif [[ -z "$_tag" && -z "$_branch" && -n "$_commit" ]]; then
    source=("${_url}#commit=${_commit}")
elif [[ -z "$_tag" && -z "$_branch" && -z "$_commit" ]]; then
    source=("${_url}")
else
    error "Configuration Error: Only one of _tag, _branch, or _commit may be set at a time."
    exit 1
fi

b2sums=('SKIP')

pkgver() {
  cd "${_pkgname}"
  # Generate version based on git tags (e.g., 1.3.3.r10.g123456)
  git describe --long --tags 2>/dev/null | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
  cd "${_pkgname}"
  # If you need to apply patches, do it here
}

build() {
  cd "${_pkgname}"

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

package_bcachefs-tools-git() {
  provides=("${_pkgname}=${pkgver}")
  conflicts=("${_pkgname}")

  cd "${_pkgname}"

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

  # remove dkms module from the main package
  rm -rf "${pkgdir}"/usr/src

  # package completions
  install -dm755 "${pkgdir}"/usr/share/{bash-completion/completions,fish/vendor_completions.d,zsh/site-functions}
  "${pkgdir}"/usr/bin/bcachefs completions bash > "${pkgdir}"/usr/share/bash-completion/completions/bcachefs
  "${pkgdir}"/usr/bin/bcachefs completions fish > "${pkgdir}"/usr/share/fish/vendor_completions.d/bcachefs.fish
  "${pkgdir}"/usr/bin/bcachefs completions zsh > "${pkgdir}"/usr/share/zsh/site-functions/_bcachefs
}

package_bcachefs-dkms-git() {
  pkgdesc="BCacheFS filesystem kernel module (DKMS) (Git version)"
  depends=(dkms)
  provides=("bcachefs-dkms=${pkgver}")
  conflicts=("bcachefs-dkms")

  cd "${_pkgname}"

  make \
    PREFIX="/usr" \
    LIBEXECDIR=/usr/lib \
    DESTDIR="${pkgdir}" \
    ROOT_SBINDIR="/usr/bin" \
    INITRAMFS_DIR="/usr/lib/initcpio/" \
    install_dkms
}
