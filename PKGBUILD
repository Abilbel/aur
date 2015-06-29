# Maintainer: Caleb Maclennan <caleb@alerque.com>
# Contributor: Adrián Pérez de Castro <aperez@igalia.com>

pkgname='sile-git'
pkgdesc='Modern typesetting system inspired by TeX'
pkgver=0.9.2_132_g4bb5a58
_branch='master'
pkgrel=1
arch='any'
url='http://www.sile-typesetter.org/'
license='MIT'
provides=("${pkgname%-git}")
conflicts=("${pkgname%-git}")
source=("git://github.com/simoncozens/${pkgname%-git}.git#branch=${_branch}")
sha512sums=('SKIP')
depends=('lua-lpeg'
         'lua-expat'
         'lua-luafilesystem'
         'harfbuzz')

pkgver() {
  cd "${pkgname%-git}"
  git describe --long --tags | sed 's/^v//;s/-/_/g'
}

prepare () {
    cd "${pkgname%-git}"
    ./bootstrap.sh
}

build () {
    cd "${pkgname%-git}"
    ./configure
    make
}

package () {
    cd "${pkgname%-git}"
    make install DESTDIR="${pkgdir}/"
}
