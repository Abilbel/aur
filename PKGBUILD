# Maintainer: Jesse Spangenberger <azulephoenix@gmail.com>
# Contributor: Tristelune <tristelune@archlinux.info>
pkgname=pdfstudio
pkgver=19.2.1
pkgrel=1
pkgdesc="Review, annotate, and edit PDF Documents"
arch=('i686' 'x86_64')
url="http://www.qoppa.com/pdfstudio/"
license=('custom')
conflicts=('pdfstudio18' 'pdfstudio12' 'pdfstudio11' 'pdfstudio8' 'pdfstudio9' 'pdfstudio10')
provides=('pdfstudio')
makedepends=('pacman>=4.2.0')
depends=('java-runtime>=6' 'gtk-update-icon-cache')

install=${pkgname}.install     
     
sha256sums=('8bbf7d3aaedfdde658e8fe5fc9bf30b92f1643986507b3d90e0eb6ebf1c39a0c'
            '6c73dba3ccc9e4ebc1e3d2318bd7e3cf8ff1d6cdca2243bed6522f99c20aea49'
            '0a3c1c337a22228f3df28412ca65d45d0d8067b508cf7b1cf93810fc17c9b447')
sha256sums_i686=('a8e41cf6c9d34359a790afac451ecfac3a7bb80134910f6bf81e4851ea04fda3')
sha256sums_x86_64=('73197a7caf6dcfe503ee7a4af0737b16c8cb24803b4b363d989bd2f8751c3029')
source_i686=("http://download.qoppa.com/pdfstudio/PDFStudio_linux.deb")
source_x86_64=("http://download.qoppa.com/pdfstudio/PDFStudio_linux64.deb")
source=(${pkgname}.desktop
	${pkgname}.install
	${pkgname}.png)

prepare() {
	bsdtar xf data.tar.gz
	bsdtar xf "opt/pdfstudio2019/lib/pdfstudio.jar" resources/license.html

	rm -rf "opt/pdfstudio2019/jre"
}

package() {

	cd "${srcdir}"
			
	install -dm 755 "${pkgdir}/opt"
	install -Dm644 ${pkgname}.desktop "${pkgdir}/usr/share/applications/${pkgname}.desktop"
	install -Dm644 ${pkgname}.png "${pkgdir}/usr/share/pixmaps/${pkgname}.png"
	install -Dm644 resources/license.html "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE.html"
	
	cp -r opt/ "${pkgdir}"
	mkdir -p "${pkgdir}/usr/bin"
	ln -s /opt/pdfstudio2019/pdfstudio2019 "${pkgdir}/usr/bin/pdfstudio"
}
