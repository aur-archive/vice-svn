# Maintainer:Lobotomius <lobotomius at gmail dot com>
# Contributor:grimi <grimi at poczta dot fm>
# Contributor:Sebastian Wolf <fatmike303 at gmail dot com>
# Last modified: 18/03/2014 00:24:27 +0000 PKGBUILD

pkgname=vice-svn
pkgver=28862
pkgrel=1
pkgdesc="A Versatile Commodore Emulator, development version, GTK3 and FFMPEG enabled"
arch=('i686' 'x86_64')
license=('GPL')
url="http://vice-emu.sourceforge.net"
depends=('vte3' 'giflib' 'ffmpeg')
makedepends=('xorg-bdftopcf' 'xorg-mkfontdir' 'subversion')
provides=("vice=$pkgver")
conflicts=('vice' 'vice-gnomeui' 'vice-gtkglext' 'vice-sdl')
source=(
		'svn+https://vice-emu.svn.sourceforge.net/svnroot/vice-emu/trunk'
		'CBM_Logo.svg'
		'x64.desktop'
		'uifilechooser.h.patch'
		'filechooser.c.patch'
	)
md5sums=(
		'SKIP'
         '7b3c3d3a220b747a00d9f507adb7ef0d'
         '92381be443259ed06ef264596d0f09e4'
         'e3b1458620f7a5edda84faaa3d277874'
         '2a0f30763044e6e686518dc5f06ab1d0'
         )

pkgver() {
	cd trunk; svnversion | tr -d [A-z]
}

build() {
	cd "$srcdir/trunk/vice"

	./autogen.sh

	patch -p1 src/arch/unix/x11/gnome/uifilechooser.c < "$srcdir/filechooser.c.patch"
	patch -p1 src/arch/unix/x11/gnome/uifilechooser.h < "$srcdir/uifilechooser.h.patch"

	./configure \
		--enable-arch=unix \
		--prefix=/usr \
		--libexecdir=/usr/lib \
		--enable-fullscreen \
		--without-oss \
		--without-arts \
		--with-png \
		--without-x \
		--enable-memmap \
		--enable-ethernet \
		--enable-ffmpeg \
		--enable-gnomeui3

	make
}

package() {
	cd "$srcdir/trunk/vice"
	make DESTDIR="$pkgdir" install
	mkdir -p "$pkgdir/usr/share/icons"
	mkdir -p "$pkgdir/usr/share/applications"
	install -m744 "$srcdir/CBM_Logo.svg" "$pkgdir/usr/share/icons/CBM_Logo.svg"
	install -m744 "$srcdir/x64.desktop" "$pkgdir/usr/share/applications/x64.desktop"
}

