# Maintainer: Xavier R.M. (sapphirus at azorium dot net)

_pkgname=pacwrap
pkgname=('pacwrap' 'pacwrap-dist')
pkgver=0.8.0
pkgrel=1
_pkgbase=${pkgname}-${pkgver}
pkgdesc="Facilitates the creation, management, and execution of unprivileged Arch-based bubblewrap containers."
arch=('x86_64')
url="https://pacwrap.sapphirus.org/"
license=('GPLv3-only')
makedepends=('cargo' 'git' 'fakeroot' 'pacman' 'libalpm.so>=14' 'zstd' 'busybox' 'fakechroot')
source=("${_pkgbase}.tar.zst::https://github.com/pacwrap/pacwrap/releases/download/${pkgver}/${_pkgbase}.tar.zst")
sha512sums=('832139cd18e276c46fed275e8d4b6bdf215ce14ab9d8cf39d1ffc93bde952028b2a9cb2c4b635170b3093ed0a6f8e46c1a9dd6874c54a283c0913c27c0df14e9')
options=('!lto')

prepare() {
    cd "${_pkgbase}" 	
    cargo fetch --locked --target "$CARCH-unknown-linux-gnu" \
    && ./dist/tools/prepare.sh release
}

build() {
    cd "${_pkgbase}"
    PACWRAP_SCHEMA_BUILT=1 \
    cargo build --release --frozen \
    && ./dist/tools/package.sh release
}

package_pacwrap() {
	provides=("${_pkgname}")
	conflicts=("${_pkgname}")
	depends=('bash' 'bubblewrap' 'gnupg' 'pacman' 'libalpm.so>=14' 'libseccomp' "pacwrap-dist=$pkgver" 'zstd')
	optdepends=('xdg-dbus-proxy')

  	cd "${_pkgbase}"

	install -d "${pkgdir}/usr/share/${_pkgname}"	
	install -Dm 755 "target/release/${_pkgname}" "${pkgdir}/usr/bin/${_pkgname}"
  	install -Dm 755 "dist/bin/${_pkgname}-key" "${pkgdir}/usr/bin/${_pkgname}-key"
	install -Dm 644 "dist/bin/${_pkgname}.1" "${pkgdir}/usr/share/man/man1/${_pkgname}.1"
	install -Dm 644 "dist/bin/${_pkgname}.yml.2" "${pkgdir}/usr/share/man/man2/${_pkgname}.yml.2"
	install -Dm 644 LICENSE "${pkgdir}/usr/share/licenses/${_pkgname}/LICENSE"
}

package_pacwrap-dist() {
	provides=("${_pkgname}-dist")
	conflicts=("${_pkgname}-dist")

  	cd "${_pkgbase}"

	install -dD 755 "${pkgdir}/usr/share/${_pkgname}/"
	install -Dm 644 LICENSE "${pkgdir}/usr/share/licenses/${_pkgname}-dist/LICENSE"
	install -Dm 644 "dist/bin/filesystem.tar.zst" "${pkgdir}/usr/share/${_pkgname}/filesystem.tar.zst"
	install -Dm 644 "dist/bin/filesystem.dat" "${pkgdir}/usr/share/${_pkgname}/filesystem.dat"

	cp -r "dist/runtime" "${pkgdir}/usr/share/${_pkgname}/"
}
