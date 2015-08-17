# Contributor: Abdallah Aly <l3thal8 @gmail.com>

pkgname=sphinxbase-svn
pkgver=11620
pkgrel=1
pkgdesc="the basic libraries shared by the CMU Sphinx trainer and all the Sphinx decoders"
arch=(i686 x86_64)
url="http://cmusphinx.sourceforge.net"
license=('CMU')
depends=('gcc')
makedepends=('subversion' 'python2')
provides=(sphinxbase)
conflicts=(sphinxbase)
md5sums=() #generate with 'makepkg -g'

_svntrunk=https://cmusphinx.svn.sourceforge.net/svnroot/cmusphinx/trunk/sphinxbase
_svnmod=sphinxbase



build() {


cd "$srcdir"

  
  msg "getting $_svnmod"
  if [ -d $_svnmod/.svn ]; then
    (cd $_svnmod && svn up -r $pkgver)
  else
    svn co $_svntrunk --config-dir ./ -r $pkgver $_svnmod
  fi  
  
  msg "SVN checkout done or server timeout"
  msg "Starting make..."

cd $srcdir/$_svnmod
./autogen.sh --prefix=/usr

# patch to force using python2
sed -i -e "s|/usr/bin/python|/usr/bin/python2|g" $srcdir/sphinxbase/Makefile
sed -i -e "s|/usr/bin/python|/usr/bin/python2|g" $srcdir/sphinxbase/python/Makefile

make || return 1
#make check || return 1
mkdir -p "$pkgdir/usr/lib/python2.7/site-packages"
export PYTHONPATH=$PYTHONPATH:$pkgdir/usr/lib/python2.7/site-packages || return 1
make DESTDIR="$pkgdir" install || return 1

mkdir -p "$pkgdir/usr/share/licenses/cmu/"
cp $srcdir/$_svnmod/LICENSE $pkgdir/usr/share/licenses/cmu

}

