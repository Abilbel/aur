# Maintainer: Caleb Maclennan <caleb@alerque.com>

_npmname=csscomb
pkgname=nodejs-"$_npmname"
pkgver=4.2.0
pkgrel=4
pkgdesc='Coding style formatter for CSS'
arch=('any')
url="https://github.com/$_npmname/$_npmname.js"
license=('MIT')
depends=('nodejs')
makedepends=('nodejs' 'npm')
source=("https://registry.npmjs.org/$_npmname/-/$_npmname-$pkgver.tgz")
sha256sums=('fcb06b9def17349aa4e57217d6ec964144c08e11cab7642920637a67b74e50c8')
noextract=("${source[@]##*/}")

package() {
    npm install -g --user root --cache "${srcdir}/npm-cache" --prefix "$pkgdir/usr" "${source[@]##*/}"
    find "${pkgdir}"/usr -type d -exec chmod 755 {} +
    find "${pkgdir}" -type f -name package.json -exec sed -i -e '/_where/d' {} \;
    chown -R root:root $pkgdir
}
