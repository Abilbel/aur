# Maintainer: Massimiliano Torromeo <massimiliano.torromeo@gmail.com>

pkgname=mattermost
pkgver=3.7.0
_pkgver=${pkgver/rc/-rc}
pkgrel=1
pkgdesc="Open source Slack-alternative in Golang and React"
arch=('i686' 'x86_64')
url="http://mattermost.org"
license=('MIT')
depends=('glibc')
makedepends=('go' 'npm' 'python2' 'git' 'mercurial' 'libpng12')
backup=('etc/webapps/mattermost/config.json')
optdepends=('mariadb: SQL server storage'
            'percona-server: SQL server storage'
            'postgresql: SQL server storage')
source=(https://github.com/mattermost/platform/archive/v$_pkgver/$pkgname-$_pkgver.tar.gz
        mattermost.service
        user.conf
        tmpfile.conf
        mattermost.sh)
sha256sums=('037ecdd907707fcc6603482b2ae08b87437c78097199bcc8d8d3234d17fcfd1c'
            'b3fbb2d04e72396677b2c8e34df089ff135796f7a0e8a42d45e989773d6d5b07'
            '7cd154ed034a09f6671cab68bc9c30a7fd84e777e801e2aaf93a567cfa0dccfd'
            '42277f740be74081126e5ac20a90bdf11cc9588f9b16e6bc1e2f6f106bedb8a6'
            '7f4993798d1a2ae9a78fed5fc3fe88d44a7a669e7ffefda7fa6a36c27c6c5840')

prepare() {
  mkdir -p src/github.com/mattermost
  cd src/github.com/mattermost
  rm -f platform
  ln -s "$srcdir"/platform-$_pkgver platform
  cd platform

  sed -n '1,/cp README.md/p;/^run-server:/,$p' -i Makefile
  sed -r -i Makefile \
    -e 's/^package: build build-client/package: build-linux build-client/' \
    -e 's/GOARCH=amd64//' \
    -e 's/^(\s*)BUILD_HASH(_ENTERPRISE)? =.*/\1BUILD_HASH\2 = none/'
}

build() {
  cd "$srcdir"/src/github.com/mattermost/platform
  GOPATH="$srcdir" BUILD_NUMBER=$_pkgver-$pkgrel make package
}

package() {
  cd "$srcdir"/src/github.com/mattermost/platform

  install -dm755 "$pkgdir"/usr/share/webapps \
                 "$pkgdir"/var/log/$pkgname \
                 "$pkgdir"/etc/webapps \
                 "$pkgdir"/usr/share/{licenses,doc}/$pkgname

  cp -a dist/mattermost "$pkgdir"/usr/share/webapps/$pkgname

  cd "$pkgdir"/usr/share/webapps/$pkgname
  rm -rf logs
  ln -s /var/log/$pkgname logs

  mv config "$pkgdir"/etc/webapps/$pkgname
  ln -s /etc/webapps/$pkgname config

  sed -e 's@"Directory": ".*"@"Directory": "/var/lib/mattermost/"@g' \
      -e 's@tcp(dockerhost:3306)@unix(/run/mysqld/mysqld.sock)@g' \
      -i "$pkgdir"/etc/webapps/$pkgname/config.json

  mv MIT-COMPILED-LICENSE.md "$pkgdir"/usr/share/licenses/$pkgname
  mv NOTICE.txt README.md "$pkgdir"/usr/share/doc/$pkgname

  cd "$srcdir"
  install -Dm755 bin/platform "$pkgdir"/usr/share/webapps/$pkgname/bin/platform
  install -Dm755 mattermost.sh "$pkgdir"/usr/bin/mattermost
  install -Dm644 mattermost.service "$pkgdir"/usr/lib/systemd/system/mattermost.service
  install -Dm644 user.conf "$pkgdir"/usr/lib/sysusers.d/mattermost.conf
  install -Dm644 tmpfile.conf "$pkgdir"/usr/lib/tmpfiles.d/mattermost.conf
}
