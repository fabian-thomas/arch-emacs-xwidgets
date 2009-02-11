# Maintainer: Juergen Hoetzel <juergen@archlinux.org>
# Contributor: Renchi Raju <renchi@green.tam.uiuc.edu>
pkgname=emacs
pkgver=22.3
pkgrel=1
pkgdesc="The Emacs Editor"
arch=(i686 x86_64)
url="http://www.gnu.org/software/emacs/emacs.html"
license=('GPL')
options=(docs)
depends=('ncurses' 'libpng' 'libtiff' 'giflib' 'libxpm' 'gtk2' 'texinfo')
source=(ftp://ftp.gnu.org/gnu/emacs/$pkgname-$pkgver.tar.gz emacs.desktop)
md5sums=('aa8ba34f548cd78b35914ae5a7bb87eb' 
         '8af038d2ba4561271e935bb444ceb4e3')
install=emacs.install

build() {
  cd $startdir/src/$pkgname-$pkgver
  mandir=/usr/share/man
  ./configure --prefix=/usr --sysconfdir=/etc --libexecdir=/usr/lib \
  --localstatedir=/var --mandir=${mandir} --without-sound -with-x-toolkit=gtk
  make || return 1
  make DESTDIR=$startdir/pkg install || return 1

  # remove conflict with ctags package
  mv $startdir/pkg/usr/bin/{ctags,ctags.emacs} || return 1
  mv $startdir/pkg/usr/bin/{etags,etags.emacs} || return 1
  mv $startdir/pkg${mandir}/man1/{etags.1,etags.emacs.1} || return 1
  mv $startdir/pkg${mandir}/man1/{ctags.1,ctags.emacs.1} || return 1
  # fix all the 777 perms on directories
  find $startdir/pkg/usr/share/emacs/$pkgver -type d -exec chmod 755 {} \;
  # fix user/root permissions on usr/share files
  find $startdir/pkg/usr/share/emacs/$pkgver -exec chown root.root {} \;
  # fix perms on /var/games
  chmod 775 ${startdir}/pkg/var/games
  chmod 775 ${startdir}/pkg/var/games/emacs
  chmod 664 ${startdir}/pkg/var/games/emacs/*
  chown -R root:50 ${startdir}/pkg/var/games


  # remove info dir
  rm $startdir/pkg/usr/share/info/dir
  gzip -9nf $startdir/pkg/usr/share/info/*

  # fix  FS#9253 
  mkdir -p $startdir/pkg/usr/share/pixmaps ${startdir}/pkg/usr/share/applications
  install -D -m644 ${startdir}/src/${pkgname}.desktop   ${startdir}/pkg/usr/share/applications
  ln -s ../emacs/${pkgver}/etc/images/icons/emacs_48.png  $startdir/pkg/usr/share/pixmaps/emacs-icon.png
}