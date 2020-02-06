# Maintainer: Caleb Maclennan <caleb@alerque.com>

_rockname=fluent
_project=fluent-lua
pkgname=("lua-$_rockname" "lua52-$_rockname" "lua51-$_rockname")
pkgver=0.0.4
_rockrel=0
pkgrel=2
pkgdesc='Lua implementation of Project Fluent.'
arch=('any')
url="https://github.com/alerque/$_project"
license=('MIT')
makedepends=('luarocks')
source=("${_rockname}-${pkgver}.tar.gz::https://github.com/alerque/$_project/archive/$pkgver.tar.gz")
sha256sums=('90705d64e4bc35d204604601e65094da49ca14c41e43110da778c1e3166346b5')

_package_helper() {
  cd "$_project-$pkgver"
  luarocks --lua-version=$1 --tree="$pkgdir/usr" install --deps-mode=none --no-manifest "$_rockname-scm-$_rockrel.rockspec"
}

package_lua-fluent() {
  depends+=('lua')
  _package_helper 5.3
}

package_lua52-fluent() {
  depends+=('lua52')
  _package_helper 5.2
}

package_lua51-fluent() {
  depends+=('lua51')
  _package_helper 5.1
}
