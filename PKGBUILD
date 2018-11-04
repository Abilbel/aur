# Maintainer: Jorge Israel Peña <jorge.israel.p@gmail.com>

pkgname=syncplay-git
pkgver=1.6.0.r3.g6f840e4
pkgrel=1
pkgdesc="synchronize watching movies on mplayer2, vlc, mpv, and mpc-hc on many computers"
arch=('any')
url="http://syncplay.pl/"
license=('custom')
depends=('python' 'python-twisted' 'pyside2' 'qt5-declarative')
makedepends=('git')
provides=('syncplay')
conflicts=('syncplay')
source=("$pkgname"::'git://github.com/Syncplay/syncplay.git'
        syncplay@.service)
sha256sums=('SKIP'
            '2033d40daad02f06eede073d0cee39fba8c70289dd71e8444d429b810438ec3a')

pkgver() {
  cd "$pkgname"

  git describe --long --tags | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

package() {
  cd $srcdir

  # systemd
  install -Dm644 syncplay@.service ${pkgdir}/usr/lib/systemd/system/syncplay@.service

  cd $pkgname

  # actual program
  make DESTDIR="$pkgdir" install
}
