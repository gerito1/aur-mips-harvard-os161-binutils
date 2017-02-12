# Contributor: Alexander 'hatred' Drozdov <adrozdoff@gmail.com>
# Contributor: toha257 <toha257@gmail.com>
# Contributor: Allan McRae <allan@archlinux.org>
# Contributor: Kevin Mihelich <kevin@archlinuxarm.org>
# Contributor: Tavian Barnes <tavianator@tavianator.com>
# Maintainer: Geronimo Bareiro <gero[dot]bare[at]gmail.com>

_target="mips-harvard-os161"
pkgname=${_target}-binutils
_pkgver=2.24+os161-2.1
pkgver=${_pkgver/os161-/os161_}
pkgrel=1
pkgdesc="A set of programs to assemble and manipulate binary and object files (${_target})"
arch=('i686' 'x86_64')
url="http://os161.eecs.harvard.edu/"
license=('GPL')
depends=('glibc>=2.24' 'zlib')
options=('staticlibs' '!distcc' '!ccache')
source=(http://os161.eecs.harvard.edu/download/binutils-${_pkgver}.tar.gz
#binutils.patch
)
md5sums=('89fb5251c8fac189551f30550e6c4d0b'
#'851b1dc363d8dc5ee6454c3585991bb0'
)

prepare() {
  
  cd binutils-${_pkgver}

  #patch -p1 -i ${srcdir}/binutils.patch

  # hack! - libiberty configure tests for header files using "$CPP $CPPFLAGS"
  sed -i "/ac_cpp=/s/\$CPPFLAGS/\$CPPFLAGS -O2/" libiberty/configure

  mkdir ${srcdir}/binutils-build
}

build() {
  cd binutils-build

  ../binutils-${_pkgver}/configure --prefix=/usr \
      --program-prefix=${_target}- \
      --with-lib-path=/usr/lib/binutils/${_target} \
      --with-local-prefix=/usr/lib/${_target} \
      --with-sysroot=/usr/${_target} \
      --target=${_target} --host=${CHOST} --build=${CHOST} \
      --disable-nls --nfp \
      --disable-shared --disable-threads \
      --disable-werror
#      --enable-ld=default --enable-gold --enable-plugins \
#      --enable-deterministic-archives \
#      --disable-werror #--disable-gdb --disable-sim

#  ../binutils-${pkgver}/configure --prefix=/usr \
#      --program-prefix=${_target}- \
#      --with-lib-path=/usr/lib/binutils/${_target} \
#      --with-local-prefix=/usr/lib/${_target} \
#      --with-sysroot=/usr/${_target} \
#      --target=${_target} --host=${CHOST} --build=${CHOST} \
#      --disable-nls \
#      --enable-threads --enable-shared --with-pic \
#      --enable-ld=default --enable-gold --enable-plugins \
#      --enable-deterministic-archives \
#      --disable-werror --disable-gdb --disable-sim

  make configure-host

  make tooldir=/usr
}

check() {
  cd binutils-build
  
  # unset LDFLAGS as testsuite makes assumptions about which ones are active
  # ignore failures in gold testsuite...
  make -k LDFLAGS="" check || true
}

package() {
  cd binutils-build
  make prefix=${pkgdir}/usr tooldir=${pkgdir}/usr install

  # Remove unwanted files
  rm -rf ${pkgdir}/usr/share
  rm -f ${pkgdir}/usr/bin/{ar,as,ld,nm,objdump,ranlib,readelf,strip,objcopy}
}

