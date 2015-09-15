# Maintainer: Jorge Israel Peña <jorge.israel.p@gmail.com>

pkgname=syncplay
pkgver=1.3.3
pkgrel=1
pkgdesc="synchronize watching movies on mplayer2, vlc, mpv, and mpc-hc on many computers"
arch=('any')
url="http://syncplay.pl/"
license=('custom')
depends=(python2 python2-pyside python2-twisted)
source=("https://github.com/Syncplay/syncplay/archive/${pkgver}.tar.gz"
        syncplay.service)
sha256sums=('80b41f8329aac75bbce0814ef0a7cd0236b7db1d861e90d2f178024c02c25453'
            '7b0db4ec2731288084d788fc26e11b14166b6886f406b26e9e1c4e67ed9feb0f')

package() {
  cd $srcdir

  # systemd
  install -Dm644 syncplay.service ${pkgdir}/usr/lib/systemd/system/syncplay.service

  cd "$pkgname-${pkgver}${minor}"

  make PREFIX="$pkgdir" install
}
