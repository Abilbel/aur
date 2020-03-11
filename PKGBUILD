# Maintainer: Caleb Maclennan <caleb@alerque.com>
# Contributor: Xavier Devlamynck <magicrhesus@ouranos.be>
# Contributor: Marti Raudsepp <marti@juffo.org>
# Contributor: Travis Hegner <travis.hegner@gmail.com>

pkgbase=pjproject-git
pkgname=("$pkgbase" "python-$pkgbase")
pkgver=2.10.r38.g98f70c2
pkgrel=1
pkgdesc='Open source SIP stack and media stack'
arch=('x86_64' 'armv7h' 'i686')
url='https://www.pjsip.org/'
license=('GPL')
makedepends=('alsa-lib' 'e2fsprogs' 'ffmpeg' 'libsamplerate' 'libsrtp'
             'openssl' 'opus' 'portaudio' 'speex' 'swig' 'util-linux'
             'python' 'python-setuptools')
source=("$pkgbase::git+https://github.com/pjsip/${pkgbase/-/.}"
        '0001-Don-t-build-Java-bindings.patch'
        'config_site.h')
sha256sums=('SKIP'
            'c6673d97185c2383140b6d915aeaa7e525c9cfb5f51c097472cf4773b4f87ab4'
            '61fa2a76d069aa5c95b6e2c539f7b20e2ccf0b126fc60c18117762541d0a7472')

pkgver() {
    cd "$pkgbase"
    git describe --tags --abbrev=7 HEAD | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
  cd "$pkgbase"
  patch -Np1 < "../${source[1]}"
  cp "../${source[2]}" 'pjlib/include/pj/config_site.h'
}

build() {
  cd "$pkgbase"
  export CXXFLAGS="$CXXFLAGS -fPIC"
  if [ "$CARCH" = "i686" ]; then
    export CXXFLAGS="$CXXFLAGS -march=i686"
    arch_opts=(--disable-libwebrtc)
  fi
  export CFLAGS="$CFLAGS -fPIC -DNDEBUG"
  ./configure \
    --prefix=/usr \
    --with-external-speex \
    --with-external-srtp \
    --with-external-pa \
    --with-external-gsm \
    --enable-shared \
    --disable-opencore-amr \
    "${arch_opts[@]}"

  make -j1 dep
  make -j1

  make -j1 -C pjsip-apps/src/swig

  cd 'pjsip-apps/src/swig/python'
  python setup.py build
}

package_pjproject-git() {
  depends=('openssl' 'portaudio' 'speex' 'alsa-lib' 'libsamplerate' 'util-linux'
           'ffmpeg' 'libsrtp' 'opus')
  optdepends=('e2fsprogs' 'python-pjproject: Python bindings')
  provides=("${pkgname%-git}")
  conflicts=("${pkgname%-git}")
  cd "$pkgbase"
  make -j1 DESTDIR="$pkgdir" install
  install -D -m755 pjsip-apps/bin/pjsua-*gnu* "$pkgdir/usr/bin/pjsua"
}

package_python-pjproject-git() {
  depends=('pjproject' 'python')
  provides=("${pkgname%-git}")
  conflicts=("${pkgname%-git}")
  cd "$pkgbase/pjsip-apps/src/swig/python"
  python setup.py install --root="$pkgdir/" --optimize=1 --skip-build
}
