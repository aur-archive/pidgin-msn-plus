pkgname=pidgin-msn-plus
pkgver=2.5.8
plusver=0.0.2
pkgrel=1
pkgdesc="Pidgin patched version that include MSN plus and supports new plugins for MSN protocol"
arch=('i686' 'x86_64')
url="http://pidgin.im/"
license=('GPL')
depends=('startup-notification' 'gtkspell' 'gstreamer0.10' 'libxss' 'nss' 'hicolor-icon-theme')
makedepends=('avahi' 'ca-certificates' 'perlxml' 'python' 'gettext' 'intltool')
optdepends=('avahi: Bonjour protocol support'
            'tk: Tcl/Tk scripting support'
            'ca-certificates: SSL CA certificates')
provides=('pidgin')
options=('!libtool')
conflicts=('pidgin')
install=pidgin-msn.install
source=(http://heanet.dl.sourceforge.net/sourceforge/pidgin/pidgin-$pkgver.tar.bz2 
 http://developer.pidgin.im/raw-attachment/ticket/4598/bug4598-plus_colour-cleaned.patch
http://developer.pidgin.im/raw-attachment/ticket/4598/pidgin-plus-"$plusver"_new.tar.gz
http://developer.pidgin.im/raw-attachment/ticket/4598/bug4598-plus-long_closing_tags.patch)

md5sums=('c207407dca71c6357c82135875e472f0'
         'b55d7d97fe21b3469b263f16f786be0d'
         '93851412493410f54c72b2e6b8dc03c4'
         '69576d73012dce819bff1706f5034209')

build() {

cd "$srcdir/pidgin-$pkgver"

#  patch pidgin with plus_colour-cleaned
	patch -Np0 -i ../bug4598-plus_colour-cleaned.patch

#  configure, make and install pidgin
  ./configure --prefix=/usr --sysconfdir=/etc --disable-schemas-install --disable-meanwhile \
    --disable-nm --disable-perl --disable-gnutls --disable-doxygen --disable-tcl --disable-tk \
    --with-system-ssl-certs=/etc/ssl/certs || return 1
  make || return 1
  make DESTDIR="$pkgdir" install || return 1

  # Remove GConf schema file
  rm -rf "$pkgdir/etc" || return 1

# compiling plus plugin

  cd $startdir/src/pidgin-plus-$plusver/plus

#  patch pidgin with plus-long_closing_tags
	patch -Np0 -i ../../bug4598-plus-long_closing_tags.patch


cd $startdir/src/pidgin-plus-$plusver
  ./configure --prefix=/usr

  make || return 1
  make DESTDIR=$startdir/pkg install

}

