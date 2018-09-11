# Maintainer: Simon Legner <Simon.Legner@gmail.com>
# Contributor: Pieter Goetschalckx <3.14.e.ter at gmail dot com>

pkgname=luacheck
pkgver=0.22.1
pkgrel=1
pkgdesc="A tool for linting and static analysis of Lua code."
arch=('any')
url="https://github.com/mpeterv/luacheck"
license=('MIT')
depends=('lua' 'lua-filesystem')
optdepends=('lua-lanes: for parallel checking')
source=("https://github.com/mpeterv/$pkgname/archive/$pkgver.tar.gz")
sha256sums=('46fef6860f5cd1c432cb4ae760f231f09aa286193271f0c550d9323e451da3af')

package() {
  cd "$pkgname-$pkgver"
  mkdir -p "$pkgdir/usr/share/lua/5.3/luacheck"
  install -Dm644 $(find "src/luacheck" -type f) "$pkgdir/usr/share/lua/5.3/luacheck"
  install -Dm755 "bin/luacheck.lua" "$pkgdir/usr/bin/luacheck"
  install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
