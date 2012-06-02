# Contributor: Xavion <Xavion (dot) 0 (at) Gmail (dot) com>

realname=p7zip
pkgname=${realname}-gui
pkgver=9.20.1
pkgrel=2
pkgdesc="The GUI component of the P7Zip compression utility"
arch=("i686" "x86_64")
license=("GPL")
url="http://${realname}.sourceforge.net"
depends=("wxgtk" "${realname}=${pkgver}")
makedepends=("make" "yasm" "nasm")
optdepends=("j7z: An alternative 7-Zip GUI")
options=(!emptydirs)
source=(http://downloads.sourceforge.net/sourceforge/${realname}/${realname}_${pkgver}_src_all.tar.bz2)

build() {
	cd "${srcdir}"/${realname}_${pkgver}

	[[ $CARCH = x86_64 ]] \
	&& cp makefile.linux_amd64_asm makefile.machine \
	|| cp makefile.linux_x86_asm_gcc_4.X makefile.machine
	sed -i "s|usr/local|usr|g" makefile

	make 7zG 7zFM OPTFLAGS="${CXXFLAGS}"
}

package() {
	make -C "${srcdir}/${realname}_${pkgver}" install \
		DEST_HOME=/usr DEST_DIR="${pkgdir}" \
		DEST_MAN='$(DEST_HOME)/share/man'
	
	# Remove files belonging in the base package
	cd "${pkgdir}/usr/lib/${realname}"
	rm -r 7z.so Codecs
	cd "${pkgdir}/usr/share/man/man1"
	rm 7z.1 7za.1 7zr.1
	
	# Fix odd permissions on bin when 7zG installed
	chmod +w "${pkgdir}/usr/bin"

	cd "${srcdir}"/${realname}_${pkgver}

	install -m755 -D GUI/${realname}_32.png "${pkgdir}"/usr/share/icons/hicolor/32x32/apps/${realname}.png
	ln -s /usr/lib/${realname}/7zCon.sfx "${pkgdir}"/usr/lib/${realname}/7z.sfx

	# Desktop
	install -d "${pkgdir}"/usr/share/applications/
	cp "${startdir}"/7zFM.desktop "${pkgdir}"/usr/share/applications/

	# KDE
	install -d "${pkgdir}"/usr/share/kde4/services/ServiceMenus/
	cp GUI/kde4/* "${pkgdir}"/usr/share/kde4/services/ServiceMenus/

	# Help
	find "GUI/help" -type d -exec chmod 755 {} ';'
	find "GUI/help" -type f -exec chmod 644 {} ';'
	cp -r GUI/help "${pkgdir}"/usr/lib/${realname}/
}

sha1sums=('1cd567e043ee054bf08244ce15f32cb3258306b7')
