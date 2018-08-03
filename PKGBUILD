# Maintainer: Caleb Maclennan <caleb@alerque.com>

pkgname=ufo2otf
pkgver=0.2.2
pkgrel=1
pkgdesc="Tools for converting Bibles from USFM to OSIS XML"
arch=('any')
url="https://github.com/chrislit/usfm2osis"
license=('BSD')
conflicts=("${pkgname}-git")
depends=('python'
         'fontforge'
         'afdko')
makedepends=('python-setuptools')
options=(!emptydirs)
source=("https://pypi.python.org/packages/source/u/${pkgname}/${pkgname}-${pkgver}.tar.gz"
        'python3.patch')
sha256sums=('180b4e1bbacb5f94338e87c160bf68a0919956c0e31310ad0195d0e6c25d96ca'
            '152396362fb91765a7a34958d24ca521a804df3ade5f0cce27135dfb1c712d62')

prepare () {
    cd "${pkgname}-${pkgver}"
    patch -p1 < ${srcdir}/python3.patch
}

package() {
    cd "${pkgname}-${pkgver}"
    python setup.py -q install --root="$pkgdir" --optimize=1
    install -Dm644 LICENSE.txt $pkgdir/usr/share/licenses/${pkgname}/LICENSE
}
