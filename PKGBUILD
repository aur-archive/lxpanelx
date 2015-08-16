# Contributor: Lobashev Vadim < admin at mind-x.org>
pkgname=lxpanelx
pkgver=4
pkgrel=1
pkgdesc="Lightweight X11 desktop panel. Fork by geekless from LOR."
arch=('i686' 'x86_64')
url="http://code.google.com/p/lxpanelx/"
license=('GPL')
depends=('alsa-lib' 'gtk2>=2.12.0' 'lxmenu-data' \
	 'menu-cache' 'startup-notification')
makedepends=('autoconf' 'automake' 'gcc' 'intltool' 'libtool' \
	     'make' 'pkgconfig' 'python' 'git')
options=('!libtool')
provides=('lxpanel')
conflicts=('lxpanel')
groups=('lxde-git')
source=('icon.patch')
md5sums=('1daebaab014dca5e056f3bf3fceac19c')

_svntrunk="http://lxpanelx.googlecode.com/svn/trunk/"
_svnmod="lxpanelx"

build() {
  cd ${srcdir}

  #####
  if [ -d $_svnmod/.svn ]; then
    (cd $_svnmod && svn up -r $pkgver)
  else
    svn co $_svntrunk --config-dir ./ -r $pkgver $_svnmod
  fi
  msg "SVN checkout done or server timeout"
  
  msg "Creating temporary build directory..."
  cp -r $srcdir/$_svnmod $srcdir/$_svnmod-build || return 1
  cd "${_svnmod}-build" || return 1

  msg "Starting make..."
  #####

  cd $srcdir/$_svnmod-build

  # Disable the building of man
  sed -i -e 's:po man:po:' Makefile.am || return 1
  sed -i -e 's:man/Makefile::' configure.ac || return 1
  
  msg "Applying patch for a fixed icon size"
  cd ${srcdir}/$_svnmod-build
  patch -Np1 -i ${srcdir}/icon.patch || return 1
  
  # Generating building system
  ./autogen.sh || return 1

  ./configure --prefix=/usr \
	      --sysconfdir=/etc \
	      --localstatedir=/var \
	      --disable-static

  make || return 1
  make DESTDIR=${pkgdir} install || return 1
  
  msg "Removing build directory..."
  cd ${srcdir}
  rm -Rf ${_svnmod}-build
}
