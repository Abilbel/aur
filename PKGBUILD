# Maintainer: John D Jones III AKA jnbek <jnbek1972 -_AT_- g m a i l -_Dot_- com>
_npmname=jshint
_npmver=2.8.0
pkgname=nodejs-jshint # All lowercase
pkgver=2.8.0
pkgrel=1
pkgdesc="Static analysis tool for JavaScript"
arch=(any)
url="http://jshint.com/"
license=(MIT)
depends=('nodejs' 'npm')
optdepends=()
source=(http://registry.npmjs.org/$_npmname/-/$_npmname-$_npmver.tgz)
noextract=($_npmname-$_npmver.tgz)
sha1sums=('1d09a3bd913c4cadfa81bf18d582bd85bffe0d44')

package() {
  cd "$srcdir"
  local _npmdir="$pkgdir/usr/lib/node_modules/"
  mkdir -p "$_npmdir"
  cd "$_npmdir"
  npm install --user root -g --prefix "$pkgdir/usr" $_npmname@$_npmver
}
