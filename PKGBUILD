# $Id: PKGBUILD 56021 2009-10-17 23:55:02Z giovanni $
# Maintainer: Hugo Doria <hugo@archlinux.org>

pkgname=mplayer-nas
pkgver=29776
pkgrel=1
pkgdesc="A movie player for linux with nas support"
arch=('i686' 'x86_64')
depends=('libxxf86dga' 'libxv' 'libmad' 'giflib' 'cdparanoia' 'libxinerama'
         'sdl' 'lame' 'libtheora' 'xvidcore' 'zlib' 'libmng' 'libxss' 'live-media'
         'libgl' 'smbclient' 'aalib' 'jack-audio-connection-kit' 'libcaca' 'nas'
         'x264>=20090416' 'faac' 'lirc-utils' 'ttf-dejavu' 'libxvmc' 'libjpeg>=7')
license=('GPL')
url="http://www.mplayerhq.hu/"
makedepends=('unzip' 'live-media' 'libdca' 'mesa')
backup=('etc/mplayer/codecs.conf' 'etc/mplayer/input.conf')
source=(ftp://ftp.archlinux.org/other/mplayer/mplayer-${pkgver}.tar.bz2 liba52_gcc_bug.patch)
md5sums=('3d78ec46b011da9866b4e6cffd092bc0' 'ac53c73dd6f69d91ab0ea0591df4a653')

build() {
  # Custom CFLAGS break the mplayer build
  unset CFLAGS LDFLAGS

  # Needed to compile using gcc 4.4.0 
  patch -p0 < ${srcdir}/liba52_gcc_bug.patch || return 1 

  cd ${srcdir}/mplayer

  ./configure --prefix=/usr --enable-runtime-cpudetection --disable-gui --disable-arts \
      --confdir=/etc/mplayer --disable-liblzo --disable-speex --enable-nas \
      --disable-openal --disable-fribidi --disable-libdv --disable-musepack \
      --language=all --disable-esd --disable-mga --extra-cflags=-fno-strict-aliasing || return 1

  [ "$CARCH" = "i686" ] &&  sed 's|-march=i486|-march=i686|g' -i config.mak

  make || return 1
  make -j1 DESTDIR=${pkgdir} install || return 1
  install -Dm644 etc/{codecs.conf,input.conf,example.conf} ${pkgdir}/etc/mplayer/ || return 1
  install -dm755 ${pkgdir}/usr/share/mplayer/
  ln -s /usr/share/fonts/TTF/DejaVuSans.ttf ${pkgdir}/usr/share/mplayer/subfont.ttf || return 1
  rm -rf ${pkgdir}/usr/share/mplayer/font
}
