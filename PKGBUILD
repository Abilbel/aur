# Maintainer: Giovanni Santini <giovannisantini93@yahoo.it>
# Previous maintainer: Daniel Apolinario <dapolinario@gmail.com>
# Contributor: Roman Timushev <romikt@gmail.com>
pkgname=gnome-defaults-list
pkgver=3.36.0
pkgrel=1
pkgdesc="Default file associations for GNOME environment"
#_ubuntuver=0ubuntu5
_pkgrel=2
arch=(any)
#url="http://packages.ubuntu.com/source/gnome-session"
url="https://tracker.debian.org/pkg/gnome-session"
license=(GPL LGPL)
#source=(http://archive.ubuntu.com/ubuntu/pool/main/g/gnome-session/gnome-session_${pkgver}-${_ubuntuver}.debian.tar.xz)
source=(https://deb.debian.org/debian/pool/main/g/gnome-session/gnome-session_$pkgver-$_pkgrel.debian.tar.xz)
sha256sums=('46a7a27af40f8db27a55c964da2a31302f635e44b5fc9d80c0147aaf44dc31c3')

package() {
	#install -d "$pkgdir/etc/gnome"
	install -d "$pkgdir/usr/share/applications"
	# sed -i "s/libreoffice\-//g" $pkgdir/etc/gnome/defaults.list
	cp "$srcdir/debian/gnome-mimeapps.list" "$pkgdir/usr/share/applications/"
	#ln -s "$pkgdir/usr/share/applications/gnome-mimeapps.list" "/etc/gnome/defaults.list"
}

