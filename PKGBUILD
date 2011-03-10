# Maintainer: Juergen Hoetzel <juergen@archlinux.org>
# Contributor: Renchi Raju <renchi@green.tam.uiuc.edu>
pkgname=emacs
pkgver=23.3
pkgrel=1
pkgdesc="The Emacs Editor"
arch=(i686 x86_64)
url="http://www.gnu.org/software/emacs/emacs.html"
license=('GPL')
options=(docs)
replaces=(cedet)
depends=('dbus-core' 'librsvg' 'gpm'  'giflib' 'libtiff'  'libxpm' 'libjpeg' 'gtk2' 'texinfo' 'hicolor-icon-theme' 'gconf')
source=(ftp://ftp.gnu.org/gnu/emacs/$pkgname-$pkgver.tar.gz emacs.desktop)
md5sums=('bf07c01ef473d8540c9c39f94506b1e6'
         '8af038d2ba4561271e935bb444ceb4e3')
install=emacs.install

build() {
  cd $startdir/src/$pkgname-$pkgver
  mandir=/usr/share/man
  
  # gcc 4.5 Workaround: http://gcc.gnu.org/bugzilla/show_bug.cgi?id=43904
  CFLAGS="${CFLAGS} -fno-optimize-sibling-calls"\
    ./configure --prefix=/usr --sysconfdir=/etc --libexecdir=/usr/lib \
    --localstatedir=/var --mandir=${mandir} --without-sound --with-x-toolkit=gtk
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
