# Maintainer: Matt Keeter <matt.j.keeter@gmail.com>

_realname=guile

pkgbase=mingw-w64-${_realname}
pkgname="${MINGW_PACKAGE_PREFIX}-${_realname}"
pkgver=2.2.0
pkgrel=2
pkgdesc="a portable, embeddable Scheme implementation written in C (mingw-w64)"
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
    0004-start_child.mingw.patch)
sha256sums=('ef1e9544631f18029b113911350bffd5064955c208a975bfe0d27a4003d6d86b'
            '4450dabb24ca108f32844d792a941a4869fb17ac19f3e60d5273b65317d6ada4'
            '81c9881be95b354109e82883d2e5ab2dfcb3bdd69589435f7508e54bed51958e'
            '0b13841c9deaf655ac4071f9251d22b84ed698eb49bd2952090d150628fca3ce')

prepare() {
  cd ${srcdir}/${_realname}-${pkgver}
  patch -p1 -i ${srcdir}/0002-winpthreads-compat.mingw.patch
  patch -p1 -i ${srcdir}/0003-winsock-compat.mingw.patch
  patch -p1 -i ${srcdir}/0004-start_child.mingw.patch

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
