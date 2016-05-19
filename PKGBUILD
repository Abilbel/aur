# Maintainer: Jan Was <janek.jan(at)gmail(dot)com>

pkgname=mattermost-desktop
pkgdesc="Mattermost chat desktop client for Linux"
_pkgname=desktop
pkgver=1.2.0
pkgrel=1
pkgdesc="Mattermost Desktop (Beta) for Linux"
arch=('i686' 'x86_64')
url="https://github.com/mattermost/desktop"
license=('MIT')
makedepends=('npm' 'gendesk')
depends=('gconf')
optdepends=()

source=("https://github.com/mattermost/desktop/archive/v${pkgver}.tar.gz")

sha256sums=('f8b22582d585781f7323db2c17a084f78eed4488b6c4053dd9b9237577744cc9')

build() {
    cd "${srcdir}/${_pkgname}-${pkgver}"
    npm install
    npm run package
}

prepare() {
    gendesk --pkgname "$pkgname" --pkgdesc "$pkgdesc" \
        --exec "/usr/lib/mattermost/Mattermost" \
        --categories "GNOME;GTK;Network;InstantMessaging;"
}

package() {
    cd "${srcdir}/${_pkgname}-${pkgver}"

    install -d -m 755 "${pkgdir}"/usr/lib
    case "$CARCH" in
        i686) _release="Mattermost-linux-ia32"
            ;;
        x86_64) _release="Mattermost-linux-x64"
            ;;
    esac

    cp -r release/${_release} "$pkgdir/usr/lib/mattermost"

    install -d -m 755 "$pkgdir/usr/bin"
    ln -s /usr/lib/mattermost/Mattermost "$pkgdir/usr/bin/mattermost"

    install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname"

    install -Dm644 "$srcdir/$pkgname.desktop" "$pkgdir/usr/share/applications/$pkgname.desktop"
    install -Dm644 "$pkgdir/usr/lib/mattermost/resources/app/resources/appicon.png" "$pkgdir/usr/share/pixmaps/$pkgname.png"
}

