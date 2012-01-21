# Maintainer: Juergen Hoetzel <juergen@archlinux.org>
# Contributor: Renchi Raju <renchi@green.tam.uiuc.edu>

pkgname=emacs
_majorver=23.3
_minorver=b
# We want something like "23.3.a" so pacman version comparison works, but
# upstream uses "23.3a", which is a bit silly and interpreted as alpha.
pkgver=$_majorver.$_minorver
_realver=$_majorver$_minorver
pkgrel=2
pkgdesc="The extensible, customizable, self-documenting real-time display editor"
arch=('i686' 'x86_64')
url="http://www.gnu.org/software/emacs/emacs.html"
license=('GPL3')
depends=('librsvg' 'gpm' 'giflib' 'libxpm' 'gtk2' 'hicolor-icon-theme' 'gconf' 'desktop-file-utils' 'alsa-lib')
install=emacs.install
source=(ftp://ftp.gnu.org/gnu/emacs/$pkgname-$_realver.tar.bz2{,.sig} emacs-subversion17.patch)
md5sums=('917ce0054ef63773078a6e99b55df1ee'
         '4f987fbb906c0ee73e05904b0f7ce5ec'
         '12e2e7a66df5cc5ded54e1d30083a1fb')

build() {
  cd "$srcdir"/$pkgname-$_majorver
  patch -p1 -i ../emacs-subversion17.patch
  ./configure --prefix=/usr --sysconfdir=/etc --libexecdir=/usr/lib \
    --localstatedir=/var --with-x-toolkit=gtk --with-xft
  make
}

package() {
  cd "$srcdir"/$pkgname-$_majorver
  make DESTDIR="$pkgdir" install

  # remove conflict with ctags package
  mv "$pkgdir"/usr/bin/{ctags,ctags.emacs}
  mv "$pkgdir"/usr/share/man/man1/{ctags.1,ctags.emacs.1}
  # fix all the 777 perms on directories
  find "$pkgdir"/usr/share/emacs/$_majorver -type d -exec chmod 755 {} \;
  # fix user/root permissions on usr/share files
  find "$pkgdir"/usr/share/emacs/$_majorver -exec chown root:root {} \;
  # fix perms on /var/games
  chmod 775 "$pkgdir"/var/games
  chmod 775 "$pkgdir"/var/games/emacs
  chmod 664 "$pkgdir"/var/games/emacs/*
  chown -R root:games "$pkgdir"/var/games
}
