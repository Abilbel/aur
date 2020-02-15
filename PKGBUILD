# Maintainer: Caleb Maclennan <caleb@alerque.com>

_pkgname=alkalami
pkgname=ttf-sil-$_pkgname
_fname=${_pkgname^}
pkgver=1.200
pkgrel=1
pkgdesc='Unicode font for Arabic-based writing systems in the Kango region of Nigeria and Niger'
arch=('any')
url="https://software.sil.org/$_pkgname"
license=('custom:OFL')
depends=('fontconfig' 'xorg-font-utils')
source=("http://software.sil.org/downloads/r/$_pkgname/$_fname-$pkgver.zip")
sha256sums=('f294789b09d1aaaa56f70b198ab842b88803ae649ce3e4e74df8e9f1ac4662d5')

package() {
    cd "$_fname-$pkgver"
    find -type f -iname "$_fname*.ttf" -execdir \
        install -Dm644 {} -t "$pkgdir/usr/share/fonts/TTF" \;
    install -Dm644 OFL.txt -t "$pkgdir/usr/share/licenses/$pkgname"
}
