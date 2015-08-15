# Maintainer: Mauro Fruet <maurofruet@gmail.com>
# Contributor: oke3 < Sekereg [at] gmx [dot] com >
# Contributor: neuromante <lorenzo.nizzi.grifi@gmail.com>

pkgname=banshee-community-extensions-git
pkgver=20130110
pkgrel=1
pkgdesc="Extensions to the Banshee media player that are community contributed and maintained"
arch=('i686' 'x86_64')
url="http://gitorious.org/banshee-community-extensions"
license=('MIT' 'GPL2')
install=$pkgname.install
depends=('banshee-git' 'lirc-utils' 'fftw' 'libsamplerate' 'xdg-utils' 'desktop-file-utils')
makedepends=('gnome-doc-utils' 'intltool' 'git')
provides=('banshee-community-extensions')
conflicts=('banshee-community-extensions')

_gitroot="git://gitorious.org/banshee-community-extensions/banshee-community-extensions.git"
_gitname="banshee-community-extensions"

build() {
  cd "$srcdir"
  msg "Connecting to GIT server...."

  if [[ -d "$_gitname" ]]; then
    cd "$_gitname" && git pull origin
    msg "The local files are updated."
  else
    git clone "$_gitroot" "$_gitname"
  fi

  msg "GIT checkout done or server timeout"
  msg "Starting make..."

  rm -rf "$srcdir/$_gitname-build"
  git clone "$srcdir/$_gitname" "$srcdir/$_gitname-build"
  cd "$srcdir/$_gitname-build"

  sed -i 's/AM_CONFIG_HEADER/AC_CONFIG_HEADERS/' configure.ac

  ./autogen.sh --prefix=/usr --sysconfdir=/etc --localstatedir=/var \
               --with-vendor-build-id=ArchLinux

  make
}

package() {
  cd "$srcdir/$_gitname-build"

  make DESTDIR="$pkgdir" install
}
