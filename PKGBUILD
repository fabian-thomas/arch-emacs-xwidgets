# Maintainer: Juergen Hoetzel <juergen@archlinux.org>
# Maintainer: Frederik Schwan <freswa at archlinux dot org>
# Contributor: Jaroslav Lichtblau <svetlemodry@archlinux.org>
# Contributor: Renchi Raju <renchi@green.tam.uiuc.edu>

pkgbase=emacs
pkgname=(emacs-nativecomp)
pkgver=29.1
pkgrel=4
arch=('x86_64')
url='https://www.gnu.org/software/emacs/emacs.html'
license=('GPL3')
depends=(
  gmp
  gnutls
  jansson
  lcms2
  libacl.so
  libasound.so
  libdbus-1.so
  libfontconfig.so
  libfreetype.so
  libgdk-3.so
  libgdk_pixbuf-2.0.so
  libgif.so
  libgio-2.0.so
  libglib-2.0.so
  libgobject-2.0.so
  libgpm.so
  libgtk-3.so
  libharfbuzz.so
  libice
  libjpeg.so
  libncursesw.so
  libotf
  libpango-1.0.so
  libpng
  librsvg-2.so
  libsm
  sqlite libsqlite3.so
  libsystemd.so
  libtiff.so
  libtree-sitter.so
  libwebp.so
  libwebpdemux.so
  libxfixes
  libxml2.so
  m17n-lib
  zlib
)
makedepends=(libgccjit)
source=(https://ftp.gnu.org/gnu/emacs/${pkgbase}-${pkgver}.tar.xz{,.sig})
b2sums=('5bec8fd7c63c04b93b2ad87c12c48373930c1b3c6984d139938ad1eb692af76417dc5494057225a04f77ce4797958056aa3522f50e3b0565ef5f060bb15f5402'
        'SKIP')
validpgpkeys=('17E90D521672C04631B1183EE78DAE0F3115E06B'  # Eli Zaretskii <eliz@gnu.org>
              'CEA1DE21AB108493CC9C65742E82323B8F4353EE') # Stefan Kangas <stefankangas@gmail.com>

prepare() {
  cp --reflink=auto -ar ${pkgbase}-${pkgver} ${pkgbase}-${pkgver}-nativecomp
}

build() {
  local _confflags="--sysconfdir=/etc \
    --prefix=/usr \
    --libexecdir=/usr/lib \
    --with-tree-sitter \
    --localstatedir=/var \
    --with-cairo \
    --disable-build-details \
    --with-harfbuzz \
    --with-libsystemd \
    --with-modules"

  export ac_cv_lib_gif_EGifPutExtensionLast=yes

  cd ${pkgbase}-${pkgver}-nativecomp
  ./configure \
    --with-x-toolkit=gtk3 \
    --with-xwidgets \
    --with-native-compilation=aot \
    $_confflags
  make bootstrap
}

package_emacs-nativecomp() {
  pkgdesc='The extensible, customizable, self-documenting real-time display editor with native compilation enabled'
  depends+=(libgccjit)
  provides=(emacs)
  conflicts=(emacs)

  cd ${pkgbase}-${pkgver}-nativecomp
  make DESTDIR="${pkgdir}" install

  # remove conflict with ctags package
  mv "${pkgdir}"/usr/bin/{ctags,ctags.emacs}
  mv "${pkgdir}"/usr/share/man/man1/{ctags.1.gz,ctags.emacs.1}

  # fix user/root permissions on usr/share files
  find "${pkgdir}"/usr/share/emacs/${pkgver} -exec chown root:root {} \;
}
