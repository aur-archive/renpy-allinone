# Maintainer: Cravix < dr dot neemous at gmail dot com >
# combination of renpy and python-renpy PKGBUILD written by AlexanderR <rvacheva at nxt dot ru>
# the motivation is simple: i don't want to download the same file twice.
pkgbase=renpy-allinone
pkgname=(renpy-aio python-renpy-aio renpy-allinone)
_pkgname=renpy
pkgver=6.99.4
pkgrel=1
# It used to build fine without this line, what the heck
arch=('i686' 'x86_64')
license=('MIT')
url='http://www.renpy.org'
makedepends=('cython2' "python2-pygame-sdl2>=$pkgver")
pkgdesc="Visual novel engine Ren'Py along with its platdeps libs (release channel)"
source=("http://www.renpy.org/dl/$pkgver/$_pkgname-$pkgver-source.tar.bz2"
        "${_pkgname}.desktop"
        "${_pkgname}."{sh,csh}
        "${_pkgname}-launcher.sh")
md5sums=('2e2e5c08aa1613abb67aaad61e9c6f7f'
         'a9beb35fa6c6d8af7ba5d2a764c33158'
         'd206d24b78e207a2c3b603fef14ac47f'
         '8b9922e26e567248a2a5adc1d0cdfdd4'
         'dfa92cdecc15e5c1ddee387fbbbb2d9c')

build() {
  cd "$srcdir/renpy-$pkgver-source"
  find . -name "*.py" -exec sed -i 's/env python$/env python2/' {} \;
  RENPY_CYTHON=/usr/bin/cython2 python2 module/setup.py build
}

package_renpy-aio(){
  pkgdesc="A visual novel engine. Both player and development tools included."
  # In fact the renpy data file is not arch-dependent, but heck, makepkg can't 
  # handle PKGBUILDs contains packages builded for different arch types, so
  # have to stay this way for a while 'til next pacman release. 
  #arch=('any')
  arch=('i686' 'x86_64')
  depends=('ttf-dejavu' 'python-renpy')
  provides=("renpy=$pkgver")
  conflicts=('renpy')
  replaces=('renpy')

  mkdir -p "$pkgdir/"{usr/share/{${pkgname%-*},doc/${pkgname%-*}},etc/profile.d}

  cd "$srcdir"

  install -m755 ${pkgname%-*}.{sh,csh} "$pkgdir/etc/profile.d"
  install -D -m755 ${pkgname%-*}-launcher.sh "$pkgdir/usr/bin/${pkgname%-*}"
  install -D -m644 ${pkgname%-*}.desktop "$pkgdir/usr/share/applications/${pkgname%-*}.desktop"

  cd ${pkgname%-*}-$pkgver-source

  cp -r launcher renpy renpy.py  templates the_question tutorial "$pkgdir/usr/share/${pkgname%-*}"
  cp -r doc/* "$pkgdir/usr/share/doc/${pkgname%-*}"
  install -D -m644 launcher/game/images/logo.png "$pkgdir/usr/share/pixmaps/${pkgname%-*}.png"
  install -D -m644 'LICENSE.txt' "$pkgdir/usr/share/licenses/${pkgname%-*}/LICENSE"

  chgrp -R games "$pkgdir"/usr/share/renpy/{the_question,tutorial}
  chmod g+w "$pkgdir"/usr/share/renpy/{the_question,tutorial}
}

package_python-renpy-aio() 
{
  pkgdesc="Platform-dependant Ren'Py libraries."
  arch=('i686' 'x86_64')
  depends=('ffmpeg' 'fribidi' 'glew' "python2-pygame-sdl2>=$pkgver")
  optdepends=('tk: renpy project directory selection dialog')
  provides=("python-renpy=$pkgver")
  conflicts=('python-renpy')
  replaces=('python-renpy')

  cd "$srcdir"/$_pkgname-${pkgver}-source

  RENPY_CYTHON=/usr/bin/cython2 python2 module/setup.py install --root="$pkgdir/" --prefix=/usr
  install -D -m644 'LICENSE.txt' "$pkgdir/usr/share/licenses/${pkgname%-*}/LICENSE"
}

package_renpy-allinone()
{
  pkgdesc="Meta package for you to make it much easier to update."
  depends=("renpy>=$pkgver" "python-renpy>=$pkgver")
  arch=('i686' 'x86_64')
  conflicts=('renpy64' 'renpy-bin' )
  replaces=('renpy64' 'renpy-bin' )
  install=${_pkgname}.install
}
