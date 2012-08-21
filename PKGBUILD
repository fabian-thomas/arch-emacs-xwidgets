# Maintainer: Juergen Hoetzel <juergen@archlinux.org>
# Contributor: Renchi Raju <renchi@green.tam.uiuc.edu>

pkgname=emacs
pkgver=24.1
pkgrel=2
pkgdesc="The extensible, customizable, self-documenting real-time display editor"
arch=('i686' 'x86_64')
url="http://www.gnu.org/software/emacs/emacs.html"
license=('GPL3')
depends=('librsvg' 'gpm' 'giflib' 'libxpm' 'libotf' 'm17n-lib' 'gtk3' 'hicolor-icon-theme' 'gconf' 'desktop-file-utils' 'alsa-lib' 'imagemagick')
install=emacs.install
source=(ftp://ftp.gnu.org/gnu/emacs/$pkgname-$pkgver.tar.bz2{,.sig})
md5sums=('8ba0932c498bc8fb10d7ddba52227e5b' 'c303d5c0bd0f7ea28acd14c9738faff4')


build() {
  cd "$srcdir"/$pkgname-$pkgver
  ./configure --prefix=/usr --sysconfdir=/etc --libexecdir=/usr/lib \
    --localstatedir=/var --with-x-toolkit=gtk3 --with-xft
  make
}

package() {
  cd "$srcdir"/$pkgname-$pkgver
  make DESTDIR="$pkgdir" install

  # remove conflict with ctags package
  mv "$pkgdir"/usr/bin/{ctags,ctags.emacs}
  mv "$pkgdir"/usr/share/man/man1/{ctags.1.gz,ctags.emacs.1}
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
