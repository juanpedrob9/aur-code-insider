# Maintainer: Jean-Pier Brochu <jeanpier.brochu@gmail.com>
# This is a modified PKGBUILD of the standard VSCode created by: Mateusz Paluszkiewicz <aifam96@gmail.com>
# Added pkgver() function in order to build the daily release of VSCode insider edition
# *Timestamp is a little off but it work!*

pkgname=code-insider
pkgver=1.8.0r1480065238.0b84799
pkgrel=1
pkgdesc="Code editing. Redefined. Visual Studio Code Insider."
arch=('x86_64')
url="https://code.visualstudio.com/"
license=('MIT')
depends=('gconf' 'gtk2' 'libnotify' 'nss')
provides=('vscode-insider' 'visualstudiocodeinsider' 'visual-studio-code-insider' 'code-insider')
conflicts=('visual-studio-code-insider' 'visual-studio-code-insider-oss' 'visual-studio-code-insider-git')
replaces=('visual-studio-code-insider')

source=(${pkgname}-x64-$(date +%Y%m%d).tar.gz::https://vscode-update.azurewebsites.net/latest/linux-x64/insider
               ${pkgname}.desktop)

sha256sums=('8bc8a0d6d9640928ebb0abe3640e9156cc1fa715943026f44ce8658df81c2945'
            '10c4b7abca633c82e39cf92f4ab50ad932e06b115f86d467c364ecc5149acb9a')

pkgver() {
	cd "${srcdir}/VSCode-linux-x64/resources/app/"
	printf "%sr%s.%s" \
		"$(sed -n 's/\"version\"\://p' package.json | sed 's/^[ \t\"\,]*//;s/[ \t\"\,]*$//' | cut -d- -f1)" \
		"$(date -d $(sed -n 's/\"date\"\://p' product.json | sed 's/^[ \t\"\,]*//;s/[ \t\"\,]*$//') +%s)" \
		"$(sed -n 's/\"commit\"\://p' product.json | sed 's/^[ \t\"\,]*//;s/[ \t\"\,]*$//' | cut -c 1-7)"
}

package() {
	_pkg=VSCode-linux-x64

	# Creating directories
	install -dm755 "$pkgdir"/{,usr/{bin,share/{pixmaps,applications,licenses/$pkgname}}}

	# Read Write for all users and groups, that repairs problems with icon extension
	install -dm777 "$pkgdir"/usr/share/$pkgname

	# Installing launcher
	install -m644 "$srcdir"/$pkgname.desktop "$pkgdir"/usr/share/applications/

	# Copying all files
	cp -a "$srcdir"/$_pkg/* "$pkgdir"/usr/share/$pkgname/
	chmod -R 777 "$pkgdir"/usr/share/$pkgname/

 	# Installing icons
	install -m644 "$srcdir"/$_pkg/resources/app/resources/linux/code.png "$pkgdir"/usr/share/pixmaps/$pkgname.png

	# Installing license
	install -m644 "$srcdir"/$_pkg/resources/app/LICENSE.txt "$pkgdir"/usr/share/licenses/$pkgname/

	# Link binary to /usr/bin
	ln -s /usr/share/$pkgname/code-insiders "$pkgdir"/usr/bin/code-insider
}
