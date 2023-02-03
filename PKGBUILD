# Maintainer: Charles Leclerc <charles@la-mouette.net>
# Co-Maintainer: Polarian <polarian@polarian.dev>

pkgname=reposilite
pkgver=3.3.0
pkgrel=3
pkgdesc="Reposilite (formerly NanoMaven) - lightweight repository manager for Maven artifacts. It is a simple solution to replace managers like Nexus, Archiva or Artifactory."
arch=(any)
url="https://github.com/dzikoysk/$pkgname"
license=('Apache')
depends=('jre11-openjdk-headless')
makedepends=('jdk11-openjdk' 'nodejs' 'npm')
source=("$pkgname-$pkgver.tar.gz::$url/archive/$pkgver.tar.gz"
        "$pkgname.service"
        "$pkgname.sysusers"
        "$pkgname.tmpfiles"
        "$pkgname.env"
        "$pkgname.wrapper")
sha256sums=('79e2cf65a507c33216b11358ec746459331c8157d4089606021f2ed51e66f29c'
            '1e543e7ef39d64ae683156aaa6aad8f164f30de999d15717416410e1750b9a8e'
            '92ccfeff429aa4757ef353677dd99ad7aebe7483d4824706a27250e81efd6323'
            '9587fa49dd66d5f31dee33aa1a9da269a34666b63f62e2550a66c3bc1d397aa7'
            '7affcf3ef54c9c05326281c3496a8744221be312675fd7d4ab17fd50eb320521'
            'f36b16f87dd0cee15d0ec5c11ba28596f4c09df5cf2739f2dce4398737337d3c')
backup=('etc/reposilite/configuration.cdn'
        'etc/reposilite/wrapper.env')

build() {
  cd "$pkgname-$pkgver"
  sed -i -e "s/version\\s*=.*/version = \"$pkgver\"/" build.gradle.kts
  chmod a+x gradlew
  JAVA_HOME="/usr/lib/jvm/java-11-openjdk" ./gradlew :reposilite-backend:shadowJar --no-daemon --stacktrace
}

package() {
  install -Dm 644 $pkgname.service -t "${pkgdir}/usr/lib/systemd/system"
  install -Dm 644 $pkgname.sysusers -t "${pkgdir}/usr/lib/sysusers.d/$pkgname.conf"
  install -Dm 644 $pkgname.tmpfiles -t "${pkgdir}/usr/lib/tmpfiles.d/$pkgname.conf"
  install -Dm 644 $pkgname-$pkgver/reposilite-backend/build/libs/$pkgname-$pkgver.jar -t "$pkgdir/usr/share/java/$pkgname/$pkgname.jar"
  install -Dm 644 $pkgname.env -t "${pkgdir}/etc/reposilite/wrapper.env"
  install -Dm 755 $pkgname.wrapper -t "${pkgdir}/usr/bin/reposilite"
}
