# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer:  a821
# Contributor: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Contributor: Truocolo <truocolo@aol.com>
# Contributor: Vincent Grande <shoober420@gmail.com>
# Contributor: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Contributor: Jan de Groot <jgc@archlinux.org>
# Contributor: Brice Carpentier <brice@daknet.org>

_pkg="glib"
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
  armv6l
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
  xorgproto
  valgrind
)
_repo="https://gitlab.freedesktop.org"
_ns="${_pkgname}"
_url="${_repo}/${_ns}/${_pkgname}"
_local="file://${HOME}/${_pkgname}"
source=(
  # "git+${_url}.git"
  "git+${_local}"
)
sha256sums=(
  'SKIP')

_flags() {
  _cflags=()
  _ldflags=()
  local \
    _bin \
    _include \
    _lib \
    _usr
  _bin="$( \
    dirname \
      "$( \
        command \
          -v \
          "cc")")"
  if [[ "${_bin}" == "" ]]; then
    _bin="$( \
      dirname \
      "$(command \
          -v \
          "gcc")")"
  fi
  if [[ "${_bin}" != "" ]]; then
    _usr="$( \
      dirname \
        "${_bin}")"
    _include="${_usr}/include"
    _lib="${_usr}/lib"
    _cflags+=" -I${_include}"
    _cflags+=" -I${_include}/${_pkg}-2.0"
    _cflags+=" -I${_lib}/${_pkg}-2.0/include"
    _ldflags+=" -L${_lib}"
  fi
  _cflags+=(
    "${CFLAGS}"
    "-g3")
  _ldflags+="${LDFLAGS}"
}

_meson_options=(
  -D dwrite=disabled
  -D gtk_doc=false
  -D spectre=disabled
  -D symbol-lookup=disabled
  -D tests=disabled
  -D xcb=enabled
  -D xlib=enabled
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
  _flags
  CFLAGS="${_cflags[*]}" \
  CXXFLAGS="${_cflags[*]}" \
  LDFLAGS="${_ldflags[*]}" \
    arch-meson \
      "${_pkgname}" \
      build \
      "${_meson_options[@]}"
  CFLAGS="${_cflags[*]}" \
  CXXFLAGS="${_cflags[*]}" \
  LDFLAGS="${_ldflags[*]}" \
    meson \
      compile \
      -C build
}

package_cairo-git() {
  provides=(
    "lib${_pkgname}-gobject.so=${pkgver}"
    "lib${_pkgname}-script-interpreter.so=${pkgver}"
    "lib${_pkgname}.so=${pkgver}"
    "lib${_pkgname}=${pkgver}"
    "${_pkgname}=${pkgver}")
  conflicts=(
    "${_pkgname}"
    "lib${_pkgname}"
  )
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
    "${_pkgname}=${pkgver}")
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
