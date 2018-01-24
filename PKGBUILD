# Maintainer: Matt Keeter <matt.j.keeter@gmail.com>

_realname=guile

pkgbase=mingw-i686-${_realname}
pkgname="${MINGW_PACKAGE_PREFIX}-${_realname}"
pkgver=2.2.3
pkgrel=2
pkgdesc="a portable, embeddable Scheme implementation written in C (mingw-i686)"
arch=('any')
url="https://www.gnu.org/software/guile/"
license=("GPL")
makedepends=("${MINGW_PACKAGE_PREFIX}-gcc"
    "${MINGW_PACKAGE_PREFIX}-pkg-config"
    "make"
    "patch"
    "autoconf"
    "automake")
depends=("${MINGW_PACKAGE_PREFIX}-crt"
    "${MINGW_PACKAGE_PREFIX}-gmp"
    "${MINGW_PACKAGE_PREFIX}-libtool"
    "${MINGW_PACKAGE_PREFIX}-ncurses>=5.7"
    "texinfo"
    "${MINGW_PACKAGE_PREFIX}-libunistring"
    "${MINGW_PACKAGE_PREFIX}-gc"
    "${MINGW_PACKAGE_PREFIX}-readline"
    "${MINGW_PACKAGE_PREFIX}-libffi"
    )
options=('strip' 'staticlibs')
source=("https://ftp.gnu.org/pub/gnu/${_realname}/${_realname}-${pkgver}.tar.gz"
    0002-winpthreads-compat.mingw.patch
    0003-winsock-compat.mingw.patch
    0004-start_child.mingw.patch
    0005-mutex-compat.mingw.patch
    0006-timegm-i686.mingw.patch)

sha256sums=('87ee07caef33c97ddc74bf3c29ce7628cfac12061f573e4a29a3a1176754610a'
            '4450dabb24ca108f32844d792a941a4869fb17ac19f3e60d5273b65317d6ada4'
            '81c9881be95b354109e82883d2e5ab2dfcb3bdd69589435f7508e54bed51958e'
            '0b13841c9deaf655ac4071f9251d22b84ed698eb49bd2952090d150628fca3ce'
            '34d22feb67da94efc498e39a9956eafecced4ab49321dd5c3e93d44075786b96'
            '6afc60d6a978f13d1d4756ae964ce7782c0f4d20d9f3c4ce073a52818f327019')


prepare() {
  cd ${srcdir}/${_realname}-${pkgver}
  patch -p1 -i ${srcdir}/0002-winpthreads-compat.mingw.patch
  patch -p1 -i ${srcdir}/0003-winsock-compat.mingw.patch
  patch -p1 -i ${srcdir}/0004-start_child.mingw.patch
  patch -p1 -i ${srcdir}/0005-mutex-compat.mingw.patch
  patch -p1 -i ${srcdir}/0006-timegm-i686.mingw.patch

  autoreconf -fi
}

build() {
  mkdir -p "${srcdir}/build-${MINGW_CHOST}"
  export lt_cv_deplibs_check_method='pass_all'
  cd "${srcdir}/build-${MINGW_CHOST}"
  "${srcdir}"/${_realname}-${pkgver}/configure \
    --prefix=${MINGW_PREFIX} \
    --build=${MINGW_CHOST} \
    --host=${MINGW_CHOST} \
    --enable-shared \
    --enable-static \
    --enable-maintainer-mode \
    --disable-posix \
    --without-threads
  make
}

package() {
  cd "${srcdir}/build-${MINGW_CHOST}"
  make DESTDIR="$pkgdir" install
  find "${pkgdir}${MINGW_PREFIX}" -name '*.def' -o -name '*.exp' | xargs -rtl1 rm

  rm -r "${pkgdir}${MINGW_PREFIX}"/share/info
  rm -r "${pkgdir}${MINGW_PREFIX}"/share/man
}
