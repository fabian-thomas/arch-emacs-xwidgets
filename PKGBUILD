# Maintainer: Juergen Hoetzel <juergen@archlinux.org>
# Contributor: Renchi Raju <renchi@green.tam.uiuc.edu>

pkgname=emacs
pkgver=25.1
pkgrel=4
pkgdesc="The extensible, customizable, self-documenting real-time display editor"
arch=('i686' 'x86_64')
url="http://www.gnu.org/software/emacs/emacs.html"
license=('GPL3')
depends=('librsvg' 'gpm' 'giflib' 'libxpm' 'libotf' 'm17n-lib' 'gtk3' 'hicolor-icon-theme' 'gconf' 'desktop-file-utils' 'alsa-lib' 'imagemagick' 'gnutls')
validpgpkeys=('B29426DEFB07724C3C35E5D36592E9A3A0B0F199' '28D3BED851FDF3AB57FEF93C233587A47C207910')
source=(ftp://ftp.gnu.org/gnu/emacs/$pkgname-$pkgver.tar.xz{,.sig})
md5sums=('4f3d42fb22823a659e16bfa89078a74c'
         'SKIP')

build() {
  cd "$srcdir"/$pkgname-$pkgver
  export REL_ALLOC=no
  ac_cv_lib_gif_EGifPutExtensionLast=yes ./configure --prefix=/usr --sysconfdir=/etc --libexecdir=/usr/lib \
    --localstatedir=/var --with-x-toolkit=gtk3 --with-xft
  make
}

package() {
  cd "$srcdir"/$pkgname-$pkgver
  make DESTDIR="$pkgdir" install

  # remove conflict with ctags package
  mv "$pkgdir"/usr/bin/{ctags,ctags.emacs}
  mv "$pkgdir"/usr/share/man/man1/{ctags.1.gz,ctags.emacs.1}

  # fix user/root permissions on usr/share files
  find "$pkgdir"/usr/share/emacs/$pkgver -exec chown root:root {} \;
  # fix perms on /var/games
  chmod 775 "$pkgdir"/var/games
  chmod 775 "$pkgdir"/var/games/emacs
  chmod 664 "$pkgdir"/var/games/emacs/*
  chown -R root:games "$pkgdir"/var/games
}
