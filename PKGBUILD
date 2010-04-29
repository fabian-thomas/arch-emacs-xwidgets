# Maintainer: Juergen Hoetzel <juergen@archlinux.org>
# Contributor: Renchi Raju <renchi@green.tam.uiuc.edu>
pkgname=emacs
pkgver=23.1
pkgrel=5
pkgdesc="The Emacs Editor"
arch=(i686 x86_64)
url="http://www.gnu.org/software/emacs/emacs.html"
license=('GPL')
options=(docs)
depends=('dbus-core' 'librsvg' 'gpm'  'giflib' 'libtiff'  'libxpm' 'libjpeg' 'gtk2' 'texinfo' 'hicolor-icon-theme')
source=(ftp://ftp.gnu.org/gnu/emacs/$pkgname-$pkgver.tar.gz emacs.desktop libpng14.patch 0001-configure.in-Invoke-CPP-with-P-when-creating-Makefil.patch)
md5sums=('a620d4452769d04ad8864d662f34f8dd' '8af038d2ba4561271e935bb444ceb4e3'\
         'd3e657091f41504fba7bdb0e96ec9b38' '8afbea5b319862c1449cc6534e0908de')
install=emacs.install

build() {
  cd $startdir/src/$pkgname-$pkgver
  patch -p1 < $startdir/src/libpng14.patch 
  # Patch from Upstream dev: Invoke $CPP with -P when creating Makefile
  patch -p2 -i $startdir/src/0001-configure.in-Invoke-CPP-with-P-when-creating-Makefil.patch
  mandir=/usr/share/man
  ./configure --prefix=/usr --sysconfdir=/etc --libexecdir=/usr/lib \
  --localstatedir=/var --mandir=${mandir} --without-sound -with-x-toolkit=gtk
  make 
  make DESTDIR=$startdir/pkg install 

  # remove conflict with ctags package
  mv $startdir/pkg/usr/bin/{ctags,ctags.emacs} 
  mv $startdir/pkg/usr/bin/{etags,etags.emacs} 
  mv $startdir/pkg${mandir}/man1/{etags.1,etags.emacs.1} 
  mv $startdir/pkg${mandir}/man1/{ctags.1,ctags.emacs.1} 
  # fix all the 777 perms on directories
  find $startdir/pkg/usr/share/emacs/$pkgver -type d -exec chmod 755 {} \;
  # fix user/root permissions on usr/share files
  find $startdir/pkg/usr/share/emacs/$pkgver -exec chown root.root {} \;
  # fix perms on /var/games
  chmod 775 ${startdir}/pkg/var/games
  chmod 775 ${startdir}/pkg/var/games/emacs
  chmod 664 ${startdir}/pkg/var/games/emacs/*
  chown -R root:50 ${startdir}/pkg/var/games


  # fix  FS#9253 
  mkdir -p $startdir/pkg/usr/share/pixmaps ${startdir}/pkg/usr/share/applications
  install -D -m644 ${startdir}/src/${pkgname}.desktop   ${startdir}/pkg/usr/share/applications
  ln -s  ../emacs/${pkgver}/etc/images/icons/hicolor/48x48/apps/emacs.png $startdir/pkg/usr/share/pixmaps/emacs-icon.png  
}
