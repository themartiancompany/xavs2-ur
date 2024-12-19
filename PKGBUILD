# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Daniel Bermond <dbermond@archlinux.org>

# NOTE:
# 10-bit depth is not supported currently
# https://github.com/pkuvcl/xavs2/blob/1.4/build/linux/configure#L500

_os="$( \
  uname \
    -o)"
if [[ "${_os}" == "GNU/Linux" ]]; then
  _libc="glibc"
elif [[ "${_os}" == "Android" ]]; then
  _libc="ndk-sysroot"
fi
_pkg=xavs2
pkgname="${_pkg}"
pkgver=1.4
pkgrel=2
arch=(
  'x86_64'
  'i686'
  'arm'
  'armv7l'
  'armv6l'
  'aarch64'
  'mips'
  'powerpc'
  'pentium4'
)
_pkgdesc=(
  'Open-Source encoder of'
  'AVS2-P2/IEEE1857.4 video'
  'coding standard'
)
pkgdesc="${_pkgdesc[*]}"
_http="http://github.com"
_ns="pkuvcl"
url="${_http}/${_ns}/${_pkg}"
license=(
  'GPL-2.0-or-later'
)
depends=(
  "${_libc}"
  'liblsmash.so'
)
makedepends=(
  'nasm'
  'l-smash'
)
provides=(
  "lib${_pkg}=${pkgver}"
)
conflicts=(
  "lib${_pkg}"
)
replaces=(
  "lib${_pkg}"
)
options=(
  '!lto'
)
source=(
  "${url}/archive/${pkgver}/${pkgname}-${pkgver}.tar.gz"
)
sha256sums=(
  '1e6d731cd64cb2a8940a0a3fd24f9c2ac3bb39357d802432a47bc20bad52c6ce'
)

build() {
  local \
    _configure_opts=()
  _configure_opts=(
    --prefix='/usr'
    --enable-shared
    --bit-depth='8'
    --chroma-format='all'
    --enable-pic
    --disable-swscale
    --disable-lavf
    --disable-ffms
    --disable-gpac
  )
  cd \
    "${pkgname}-${pkgver}/build/linux"
  # fix build with gcc 14
  export \
    CFLAGS+=' -Wno-incompatible-pointer-types'
  ./configure \
    "${_configure_opts[@]}"
  make
}

package() {
  make \
    -C \
      "${pkgname}-${pkgver}/build/linux" \
    DESTDIR="${pkgdir}" \
    install-cli \
    install-lib-shared
}

