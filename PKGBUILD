# Maintainer: Xavier Devlamynck <magicrhesus@ouranos.be>
# Contributor: Marti Raudsepp <marti@juffo.org>
#Contributor: Travis Hegner <travis.hegner@gmail.com>

pkgname=pjproject
pkgver=2.5.5
pkgrel=1
pkgdesc="Open source SIP stack and media stack"
arch=('i686' 'x86_64')
url="http://www.pjsip.org/"
license=('GPL')
depends=('openssl' 'portaudio' 'speex' 'alsa-lib' 'libsamplerate' 'util-linux-ng' 'ffmpeg' 'libsrtp' 'opus')
makedepends=('e2fsprogs' 'python')
install=pjproject.install
source=(http://www.pjsip.org/release/${pkgver}/pjproject-${pkgver}.tar.bz2)
sha256sums=('ab39207b761d3485199cd881410afeb2d171dff7c2bf75e8caae91c6dca508f3')

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  export CXXFLAGS="${CXXFLAGS} -fPIC"
  export CFLAGS="${CXXFLAGS}"
  ./configure --prefix=/usr --with-external-speex --with-external-srtp --with-external-pa --with-external-gsm --disable-oss --enable-shared --disable-opencore-amr --disable-v4l2 --disable-video --disable-sound
  echo "#define PJ_HAS_IPV6 1" >> "${srcdir}/${pkgname}-${pkgver}/pjlib/include/pj/config_site.h"
  make -j1 dep 
  make -j1 
}

package() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  make -j1 DESTDIR=${pkgdir} install
  install -D -m755 pjsip-apps/bin/pjsua-*gnu ${pkgdir}/usr/bin/pjsua
}
