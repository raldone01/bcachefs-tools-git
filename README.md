# bcachefs-tools-git

This is an AUR package for building and installing the latest development version of bcachefs-tools and bcachefs-dkms from the official bcachefs Git repository.

## Installing

```bash
git clone https://aur.archlinux.org/bcachefs-tools-git.git
cd bcachefs-tools-git
makepkg -sCf
```

## Installing a custom commit, branch or tag

Edit the `PGKBUILD` file and set one of the following variables:
```bash
# Leaving the variables empty will build the latest master
_tag= # for example v1.34.0
_branch=
_commit=
```

# Notes

This is a fork of the `bcachefs-tools` package from arch: https://gitlab.archlinux.org/archlinux/packaging/packages/bcachefs-tools
