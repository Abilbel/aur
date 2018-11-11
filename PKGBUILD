# Maintainer: William Gathoye <william + aur at gathoye dot be>

pkgname=mattermost-git
_pkgname="${pkgname%-git}"
pkgver=5.1.0.r358.g7a758eae7
pkgrel=1
pkgdesc="Open source Slack-alternative in Golang and React"
arch=('i686' 'x86_64' 'arm' 'armv6h' 'armv7h' 'aarch64')
url="https://mattermost.com"
license=('AGPL' 'Apache')

makedepends=('git' 'go-pie' 'libpng12' 'npm')
# Experiencing issues with gifsicle. Using system tool instead.
if [ "$CARCH" != 'x86_64' ]; then
    makedepends+=('gifsicle')
fi
provides=('mattermost')
conflicts=('mattermost')
backup=("etc/webapps/${_pkgname}/config.json")
optdepends=(
    'mariadb: SQL server storage'
    'percona-server: SQL server storage'
    'postgresql: SQL server storage')

source=(
    # The mattermost repo is quite huge. Consider manually cloning the
    # repository first, either a full clone or with the --dept argument. You
    # can also specify the --depth git argument in your in makepkg.conf file.
    # For local tests, simply replace this git URL by
    #'mattermost-server::git+file:///home/user/whatever/mattermost-server#branch=release-4.2'
    # For the URL syntax, please check this link:
    # https://wiki.archlinux.org/index.php/VCS_package_guidelines#VCS_sources
    "git+https://github.com/${_pkgname}/${_pkgname}-server"
    "git+https://github.com/${_pkgname}/${_pkgname}-webapp"
    "mattermost-ldflags.patch"
    "${_pkgname}.service"
    "${_pkgname}.sysusers"
    "${_pkgname}.tmpfiles")
sha512sums=(
    'SKIP'
    'SKIP'
    'ac952eae873aa09ba7bdf1e7abc618f0dc6982fa85df298261ab71ccf71f66c95846dade400e05d731f2c5ee2c6f4332d6f78d737026c9f098f1e03f419bee00'
    'cd02b3da86869117554c3c53a657a4b46989ea533b7b47c24fb642ffbd182ce6ecfb16a8ddde3af4d5e8cff0ab41a932753129662e126994e1ad5912545e6eb4'
    'b95bf2c0d840d0e85baebc1051c872056fa4990d263334fecc7b11d96085cb65a69dd866f18889e209336028f17c02152c13a92d2be1c21848939f22203439f0'
    'e3ffcf4b86e2ecc7166c1abf92cd4de23d81bad405db0121e513a8d81fea05eec9dd508141b14b208c4c13fbc347c56f01ed91326faa01e872ecdedcc18718f9')

# Using the most recent un-annotated tag reachable from the last commit
# src.: https://wiki.archlinux.org/index.php/VCS_package_guidelines#Git
# Remove the v prefix:
# src.: http://stackoverflow.com/a/7979255/3514658
pkgver() {
    cd "${srcdir}"/src/github.com/${_pkgname}/${_pkgname}-server
    git describe --long --tags | \
        sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {

    # If you want to test languages and pull requests, simply clone this
    # repositiory and init the submodule with the following command:
    # git submodule update --init --recursive
    # then, use the command mattermost_prepare_pkgbuild.sh. See its help
    # available here: https://github.com/wget/mattermost-prepare-pkgbuild

    mkdir -p src/github.com/${_pkgname}
    cd src/github.com/${_pkgname}

    # Remove previous platform folder if any previous clone was effective
    rm -f ${_pkgname}-server
    rm -f ${_pkgname}-webapp

    # Create the directory structure to match Go namespaces
    ln -s "$srcdir"/${_pkgname}-server ${_pkgname}-server
    ln -s "$srcdir"/${_pkgname}-webapp ${_pkgname}-webapp
    cd ${_pkgname}-server

    # Pass Arch Linux's Go compilation flags to Mattermost in order to take
    # into account advanced features like PIE.
    patch < "${srcdir}"/mattermost-ldflags.patch

    # We are not using docker, no need to stop it.
    sed -r -i Makefile \
        -e 's/^clean: stop-docker/clean:/'

    # The configuration isn't available at this time yet, modify the default.
    sed -r -i build/release.mk \
        -e 's/\$\(DIST_PATH\)\/config\/config.json/\$\(DIST_PATH\)\/config\/default.json/'

    # The Go programming language only supports 8 instruction sets, therefore
    # we cannot rely on ${CARCH} and need to cast manually.
    # src.: https://golang.org/doc/install/source#introduction
    case "${CARCH}" in
        i686)
            sed -r -i build/release.mk \
                -e "5,7s/amd64/386/"
            ;;
        arm*64*|*arch*64*)
            sed -r -i build/release.mk \
                -e "5,7s/amd64/arm64/"
            ;;
        arm*)
            sed -r -i build/release.mk \
                -e "5,7s/amd64/arm/"
            ;;
    esac

    # Remove platform specific lines from the Makefile from the line beginning
    # with that statement to the end of file (we do not care of the additional
    # file copy, nor the tar compression defined below the file).
    sed '/# ----- PLATFORM SPECIFIC -----/,//d' -i ./build/release.mk

    # Enforce build hash to Arch Linux (Enterprise hash is already set to
    # none), instead of the official git hash value.
    sed -r -i Makefile \
        -e "s/^(\s*)BUILD_HASH(_ENTERPRISE)? =.*/\1BUILD_HASH\2 = ${pkgver}-${pkgrel} Arch Linux \(${CARCH}\)/" \
        -e 's/-X (.*)(\$\(BUILD_HASH(_ENTERPRISE)?\))(.*)/-X '\''\1\2'\''\4/'
    cd "${srcdir}/${_pkgname}-webapp"
    sed -r -i webpack.config.js \
        -e "s/^(\s*)COMMIT_HASH:(.*),$/\1COMMIT_HASH: JSON.stringify\(\"${pkgver}-${pkgrel} Arch Linux \(${CARCH}\)\"\),/"

    # Link against system gifsicle
    if [ "$CARCH" != 'x86_64' ]; then
        gifsicleNpm="${srcdir}/${_pkgname}-webapp/node_modules/gifsicle/vendor/gifsicle"
        gifsicleNpm="${gifsicleNpm//\//\\/}"
        gifsicleSystem="$(which gifsicle)"
        gifsicleSystem="${gifsicleSystem//\//\\/}"
        sed -r -i Makefile \
            -e "s/(\t*)npm install(.*)/\0\n\1rm \"$gifsicleNpm\"\n\tln -s \"$gifsicleSystem\" \"$gifsicleNpm\"/"
    fi
}

build() {
    # No need to build mattermost-webapp as the server is taking care of this
    # step via its build-client make instruction.

    cd "${srcdir}"/src/github.com/${_pkgname}/${_pkgname}-server
    # Prevent the build to crash when some dependencies are not met or
    # outdated. This clean the webapp as well (cf. mattermost-server/Makefile,
    # clean target).
    make clean
    GOPATH="${srcdir}" BUILD_NUMBER=${pkgver}-${pkgrel} make build-linux
    GOPATH="${srcdir}" BUILD_NUMBER=${pkgver}-${pkgrel} make build-client
    GOPATH="${srcdir}" BUILD_NUMBER=${pkgver}-${pkgrel} make package
}

package() {
    cd "${srcdir}"/src/github.com/${_pkgname}/${_pkgname}-server

    install -dm755 \
        "${pkgdir}"/usr/bin \
        "${pkgdir}"/usr/share/webapps \
        "${pkgdir}"/var/log/${_pkgname} \
        "${pkgdir}"/etc/webapps \
        "${pkgdir}"/usr/share/doc/${_pkgname}

    cp -a dist/${_pkgname} "${pkgdir}"/usr/share/webapps/

    cd "${pkgdir}"/usr/share/webapps/${_pkgname}
    install -dm755 client/plugins

    rm -rf logs
    ln -s /var/log/${_pkgname} logs

    cp config/default.json config/config.json
    mv config "${pkgdir}"/etc/webapps/${_pkgname}
    ln -s /etc/webapps/${_pkgname} config

    sed -e 's@"Directory": ".*"@"Directory": "/var/lib/mattermost/"@g' \
        -e 's@tcp(dockerhost:3306)@unix(/run/mysqld/mysqld.sock)@g' \
        -i "${pkgdir}"/etc/webapps/${_pkgname}/config.json

    mv NOTICE.txt README.md "${pkgdir}"/usr/share/doc/${_pkgname}

    cd "${srcdir}"
    install -Dm755 "bin/${_pkgname}" "${pkgdir}/usr/share/webapps/${_pkgname}/bin/${_pkgname}"
    ln -s "/usr/share/webapps/${_pkgname}/bin/${_pkgname}" "${pkgdir}/usr/bin/${_pkgname}"
    install -Dm644 ${_pkgname}.service -t "${pkgdir}"/usr/lib/systemd/system/
    install -Dm644 ${_pkgname}.sysusers "${pkgdir}"/usr/lib/sysusers.d/${_pkgname}.conf
    install -Dm644 ${_pkgname}.tmpfiles "${pkgdir}"/usr/lib/tmpfiles.d/${_pkgname}.conf
}
