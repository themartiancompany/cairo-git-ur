# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer:  a821
# Contributor: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Contributor: Vincent Grande <shoober420@gmail.com>
# Contributor: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Contributor: Jan de Groot <jgc@archlinux.org>
# Contributor: Brice Carpentier <brice@daknet.org>
# Contributor: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>

_pkgname="cairo"
pkgbase="${_pkgname}-git"
pkgname=(
  "${pkgbase}"
  "${_pkgname}-docs-git")
pkgver=1.18.0.r10.gf9de19ad7
pkgrel=1
pkgdesc="2D graphics library with support for multiple output devices"
url="https://${_pkgname}graphics.org/"
arch=(
  x86_64
  arm
  armv7h
  aarch64
  i686)
license=(
  LGPL
  MPL
)
depends=(
  fontconfig
  freetype2
  glib2
  libpng
  libx11
  libxcb
  libxext
  libxrender
  lzo
  pixman
  zlib
)
makedepends=(
  git
  gtk-doc
  meson
  xsltproc
  valgrind
)
_repo="https://gitlab.freedesktop.org"
_ns="${_pkgname}"
_url="${_repo}/${_ns}/${_pkgname}"
source=(
  "git+${_url}.git")
sha256sums=(
  'SKIP')

_meson_options=(
  -D dwrite=disabled
  -D gtk_doc=false
  -D spectre=disabled
  -D symbol-lookup=disabled
  -D tests=disabled
)

pkgver() {
  cd \
    "${_pkgname}"
  git \
    describe \
    --tags | \
    sed \
      's/[^-]*-g/r&/;s/-/./g'
}

build() {
  arch-meson \
    "${_pkgname}" \
    build \
    "${_meson_options[@]}"
  meson \
    compile \
    -C build
}

package_cairo-git() {
  provides=(
    libcairo-gobject.so
    libcairo-script-interpreter.so
    libcairo.so
    "${_pkgname}")
  conflicts=(
    "${_pkgname}")
  meson \
    install \
    -C build \
    --destdir "${pkgdir}"
  if [[ " ${_meson_options[*]} " =~ \
        ' gtk_doc=true ' ]] then
    mkdir \
      -p \
      doc/usr/share
    mv \
      {"$pkgdir",doc}"/usr/share/gtk-doc"
  fi
}

package_cairo-docs-git() {
  pkgdesc+=" (documentation)"
  depends=()
  provides=(
    "${_pkgname}")
  conflicts=(
    "${_pkgname}")
  if [[ " ${_meson_options[*]} " =~ \
        ' gtk_doc=true ' ]] then
    mv \
      doc/* \
      "${pkgdir}"
  fi
}

# vim:set sw=2 sts=-1 et:
