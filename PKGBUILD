# Maintainer: David Manouchehri <d@32t.ca>
# Former Maintainer: Alexander Fehr <pizzapunk gmail com>
# Contributor: Thomas Jost <schnouki schnouki net>
# Contributor: Hinrich Harms <arch hinrich de>

pkgname=enigmail
pkgver=1.5.1
pkgrel=1
_tbver=17.0.6
pkgdesc="Thunderbird extension that enables sending and receiving signed and encrypted e-mail messages"
arch=('i686' 'x86_64')
url="http://enigmail.mozdev.org/"
license=('MPL' 'GPL')
depends=('thunderbird' 'gnupg')
makedepends=('zip' 'unzip' 'python2')
source=(http://www.enigmail.net/download/source/enigmail-$pkgver.tar.gz
        http://releases.mozilla.org/pub/mozilla.org/thunderbird/releases/$_tbver/source/thunderbird-$_tbver.source.tar.bz2
        mozconfig)
md5sums=('3e71f84ed2c11471282412ebe4f5eb2d'
         'e3be03513d038bbbcf8cca8a3652170b'
         'b3991092bd3d4692ec91374cb5e6c361')

build() {
  # Compile Thunderbird
  cd "$srcdir"/comm-release
  cp "$srcdir"/mozconfig .mozconfig
  export PYTHON=/usr/bin/python2
  make -f client.mk build MOZ_MAKE_FLAGS="$MAKEFLAGS"

  # Compile Enigmail
  mv "$srcdir"/enigmail "$srcdir"/comm-release/mailnews/extensions
  cd "$srcdir"/comm-release/mailnews/extensions/enigmail
  ./makemake -r -o "$srcdir"/comm-release/obj-enigmail
  cd "$srcdir"/comm-release/obj-enigmail/mailnews/extensions/enigmail
  make

  # Create the Enigmail XPI
  make xpi
}

package() {
  cd "$srcdir"/comm-release

  emid=$(sed -n '/.*<em:id>\(.*\)<\/em:id>.*/{s//\1/p;q}' mailnews/extensions/enigmail/package/install.rdf)

  install -d -m755 "$pkgdir"/usr/lib/thunderbird/extensions/$emid
  cd "$pkgdir"/usr/lib/thunderbird/extensions/$emid

  unzip "$srcdir"/comm-release/obj-enigmail/mozilla/dist/bin/enigmail-*.xpi
}
