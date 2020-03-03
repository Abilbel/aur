# Maintainer: Caleb Maclennan <caleb@alerque.com>

pkgname=ezra-project
pkgver=0.11.1
pkgrel=4
pkgdesc="Bible study software focussing on topical study based on keywords/tags"
arch=('x86_64')
url="https://github.com/tobias-klein/$pkgname"
license=('GPL3')
depends=('curl'
         'electron'
         'icu'
         'nodejs'
         'python2'
         'sqlite')
makedepends=('cmake' 'gendesk' 'npm' 'sword')
source=("https://github.com/tobias-klein/$pkgname/archive/$pkgver.tar.gz"
        'ezra-project.sh')
sha256sums=('cf662fc55d658d5e03ed95025428b81b1d12cb6d3f0aed9b6e41278f80b65ecd'
            '0a36167bce248b6082045163cf60b143d02ca1e447a791cf0c88e960a7fdc618')
_electron="$(electron --version | sed 's/^v//')"

prepare() {
    cd "$pkgname-$pkgver"
    npm uninstall --no-audit -D electron
    npm install --no-audit electron@"$_electron"
    gendesk -f -n --pkgname "$pkgname" --pkgdesc "$pkgdesc" --name "Ezra Project"
}

build() {
    cd "$pkgname-$pkgver"
    npm run compile-pug
    npm run install-node-prune
    "$(npm bin)"/electron-rebuild -f -w node-sword-interface -v "$_electron"
    npm run prune-node-modules
    npm run purge-build-artifacts
    npm run cleanup-gyp-shebang
}

package() {
    cd "$pkgname-$pkgver"
    install -Dm644 -t "$pkgdir/usr/share/applications/" "$pkgname.desktop"
    install -Dm755 "$srcdir/$pkgname.sh" "$pkgdir/usr/bin/$pkgname"
    "$(npm bin)"/electron-packager . "$pkgname" --overwrite --asar --platform=linux --arch=x64 --prune=true --out=release --electron-version="$_electron"
    rm release/ezra-project-linux-x64/"$pkgname"
    mkdir -p "$pkgdir/usr/lib/"
    cp -a release/ezra-project-linux-x64 "$pkgdir/usr/lib/$pkgname"
}
