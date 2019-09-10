# Maintainer: Caleb Maclennan <caleb@alerque.com>
# Contributor: Martin C. Doege <mdoege at compuserve dot com>
# Contributor: Hilton Medeiros <medeiros.hilton at gmail.com>

pkgname=otf-crimson-text
pkgver=2019.07
_ver=f8255058575d26ddf35ab0b2d5ab7832c40f7a1a
pkgrel=1
epoch=1
pkgdesc="A font family for book production in the tradition of beautiful oldstyle typefaces"
arch=('any')
url="https://github.com/skosch/Crimson/"
license=('OFL')
source=("$pkgname.zip::https://github.com/skosch/Crimson/archive/$_ver.zip")
sha256sums=('751e1922bc3fc89bd545e7a6c28a6b8c0f28e266dce83f67fa6bd546672b6cad')

package() {
  cd "$srcdir/Crimson-$_ver/Desktop Fonts"
  install -d "$pkgdir/usr/share/fonts/OTF"
  install -m644 OTF/*.otf "$pkgdir/usr/share/fonts/OTF/"
}
