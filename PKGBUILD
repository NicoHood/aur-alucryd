# Maintainer: PitBall

pkgname=lib32-libcurl-compat
_pkgname=curl
pkgver=7.46.0
pkgrel=1
pkgdesc="An URL retrieval library (32bit version)"
arch=('x86_64')
url="http://curl.haxx.se"
license=('MIT')
depends=('ca-certificates' 'lib32-gnutls' 'lib32-openssl' 'lib32-zlib')
makedepends=('gcc-multilib')
provides=('lib32-libcurl' 'lib32-libcurl-gnutls')
replaces=('lib32-libcurl' 'lib32-libcurl-gnutls')
conflicts=('lib32-libcurl' 'lib32-libcurl-gnutls')
options=('strip')
source=("http://curl.haxx.se/download/${_pkgname}-$pkgver.tar.gz")
md5sums=('230e682d59bf8ab6eca36da1d39ebd75')
install=curl-compat.install

build() {
  config=" ./configure \
      --libdir=/usr/lib32
      --prefix=/usr \
      --disable-ldap \
      --disable-ldaps \
      --enable-ipv6 \
      --disable-manual \
      --disable-versioned-symbols \
      --enable-threaded-resolver \
      --without-gssapi \
      --without-libidn \
      --without-libssh2 \
      --with-random=/dev/urandom \
      --with-ca-bundle=/etc/ssl/certs/ca-certificates.crt"

  cp -a ${_pkgname}-$pkgver{,-gnutls}
  export CC="gcc -m32"
  export CXX="g++ -m32"
  export PKG_CONFIG_PATH="/usr/lib32/pkgconfig"

  cd "${_pkgname}-$pkgver-gnutls"
  $config --without-ssl --with-gnutls
  make

  cd "../${_pkgname}-$pkgver"
  $config
  make
}

package() {
  cd "${_pkgname}-$pkgver-gnutls"
  make DESTDIR="$pkgdir" install
  mv $pkgdir/usr/lib32/libcurl{,-gnutls}.so.4.4.0
  cd "../${_pkgname}-$pkgver"
  make DESTDIR="$pkgdir" install
  rm -rf "${pkgdir}"/usr/{share,bin}
  ln -s libcurl.so.4.4.0  "$pkgdir"/usr/lib32/libcurl.so.4.2.0
  ln -s libcurl.so.4.4.0  "$pkgdir"/usr/lib32/libcurl.so.3
  ln -s libcurl-gnutls.so.4.4.0  "$pkgdir"/usr/lib32/libcurl-gnutls.so.4.2.0
  ln -s libcurl-gnutls.so.4.4.0  "$pkgdir"/usr/lib32/libcurl-gnutls.so.4
  ln -s libcurl-gnutls.so.4.4.0  "$pkgdir"/usr/lib32/libcurl-gnutls.so.3

  find "${pkgdir}"/usr/include/curl -type f -not -name curlbuild.h -delete
  mv "$pkgdir"/usr/include/curl/curlbuild.h "$pkgdir"/usr/include/"$_pkgname"/curlbuild-32.h

  # license
  install -d "$pkgdir"/usr/share/licenses
  ln -s "$_pkgname" "$pkgdir"/usr/share/licenses/"$pkgname"
}
