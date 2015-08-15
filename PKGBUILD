# Maintainer: Peter Marheine <peter@taricorp.net>
# based on 'cross-arm-elf-gcc-base' by Sergej Pupykin <pupykin.s+arch@gmail.com>

pkgname=cross-rx-elf-gcc-base
pkgver=4.5.0
pkgrel=1
pkgdesc="The GNU Compiler Collection"
arch=(i686 x86_64)
license=('GPL' 'LGPL')
url="http://gcc.gnu.org"
depends=('cross-rx-elf-binutils' 'libmpc' 'libelf' 'cloog-ppl')
options=(!libtool !emptydirs zipman docs !strip)
source=(ftp://gcc.gnu.org/pub/gcc/releases/gcc-${pkgver}/gcc-core-${pkgver}.tar.bz2)
md5sums=('58eda33c3184303628f91c42a7ab15b5')

build() {
  cd $srcdir/gcc-$pkgver

  export CFLAGS="-O2 -pipe"
  export CXXFLAGS="-O2 -pipe"

  [ $NOEXTRACT -eq 1 ] || rm -rf build
  mkdir build
  cd build

  [ $NOEXTRACT -eq 1 ] || ../configure --prefix=/usr \
	--target=rx-elf \
	--host=$CHOST \
	--build=$CHOST \
	--enable-shared --disable-nls --enable-languages=c --enable-multilib \
	--with-local-prefix=/usr/lib/cross-rx \
	--with-as=/usr/bin/rx-elf-as --with-ld=/usr/bin/rx-elf-ld \
	--with-newlib \
	--with-sysroot=/usr/$CHOST/rx-elf

  make all-gcc all-target-libgcc || return 1
  make DESTDIR=$pkgdir install-gcc install-target-libgcc || return 1

  rm -f $pkgdir/usr/share/man/man7/fsf-funding.7*
  rm -f $pkgdir/usr/share/man/man7/gfdl.7*
  rm -f $pkgdir/usr/share/man/man7/gpl.7*
  rm -rf $pkgdir/usr/share/info

  cp -r $pkgdir/usr/libexec/* $pkgdir/usr/lib/ && \
  rm -rf $pkgdir/usr/libexec

  # strip it manually
  strip $pkgdir/usr/bin/* 2>/dev/null || true
  find $pkgdir/usr/lib -type f -exec rx-elf-strip {} \; 2>/dev/null || true
}
