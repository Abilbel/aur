# Maintainer:  Caleb Maclennan <caleb@alerque.com>

pkgname=otf-iwona
pkgver=0.995
pkgrel=1
pkgdesc='Open Type Iwona font'
arch=('any')
url="http://jmn.pl/kurier-i-iwona/"
license=('CUSTOM')
depends=('fontconfig' 'xorg-font-utils')
source=("$pkgname-$pkgver.zip::http://jmn.pl/pliki/Iwona-otf-0_995.zip")
sha256sums=('2740d5c2a5efd29fc01eaa9ba29b4bd06295c5305092f9c1cfd4366e27502caa')

package() {
    cd "otf"
    install -Dm644 -t "$pkgdir/usr/share/fonts/OTF/{}" *.otf
}

