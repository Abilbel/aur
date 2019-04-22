# Maintainer:  Caleb Maclennan <caleb@alerque.com>
# Contributor: Adrián Pérez de Castro <aperez@igalia.com>

pkgname=sile-git
pkgdesc='Modern typesetting system inspired by TeX'
pkgver=0.9.5.1.r33.g1d3b28c
_branch='testing'
pkgrel=1
arch=(any)
url='http://www.sile-typesetter.org/'
license=('MIT')
provides=("${pkgname%-git}")
conflicts=("${pkgname%-git}")
depends=('fontconfig'
         'harfbuzz>=1.2.6'
         'icu'
         'lua-cosmo'
         'lua-expat'
         'lua-filesystem'
         'lua-lpeg'
         'lua-penlight'
         'lua-sec'
         'lua-socket'
         'lua-zlib'
         'ttf-gentium-plus')
makedepends=('git')
source=("git://github.com/alerque/${pkgname%-git}.git#branch=$_branch")
sha512sums=('SKIP')

pkgver() {
    cd "$srcdir/${pkgname%-git}"
    git describe --long --tags | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare () {
    cd "$srcdir/${pkgname%-git}"
    ./bootstrap.sh
}

build () {
    cd "$srcdir/${pkgname%-git}"
    ./configure --prefix=/usr
    make
}

package () {
    cd "$srcdir/${pkgname%-git}"
    make install DESTDIR="$pkgdir/"
}
