# Maintainer: André van Delden <andre.van.deldenX@Xuni-bremen.de>

_hkgname=not-gloss
pkgname=haskell-not-gloss
pkgver=0.6.0.0
pkgrel=1

pkgdesc="This package intends to make it relatively easy to do simple 3d graphics using high-level primitives. It is inspired by gloss and attempts to emulate it. This is an early release and the api will certainly change."

url="http://hackage.haskell.org/package/${_hkgname}"
license=('BSD3')
arch=('i686' 'x86_64')
depends=('ghc' 'haskell-glut>=2.3.0.0' 'haskell-glut<5'
         'haskell-openglraw>=1.2.0.0' 'haskell-openglraw<1.5'
         'haskell-spatial-math>=0.2.0' 'haskell-spatial-math<0.3')
options=('strip' 'staticlibs')
source=(http://hackage.haskell.org/packages/archive/${_hkgname}/${pkgver}/${_hkgname}-${pkgver}.tar.gz)
install="${pkgname}.install"
sha512sums=('3e73bd158498167798a082221f398736b615776d3dbd9ed5e8267c98093097e84b3a1c4ee42fc9ac4e447e4c5ff8d2237ecdccba72d4d57c6fcad199a9bf0342')

build() {
    cd ${srcdir}/${_hkgname}-${pkgver}
    runhaskell Setup configure -O ${PKGBUILD_HASKELL_ENABLE_PROFILING:+-p } --enable-split-objs --enable-shared \
       --prefix=/usr --docdir=/usr/share/doc/${pkgname} --libsubdir=\$compiler/site-local/\$pkgid
    runhaskell Setup build
    runhaskell Setup haddock
    runhaskell Setup register   --gen-script
    runhaskell Setup unregister --gen-script
    sed -i -r -e "s|ghc-pkg.*unregister[^ ]* |&'--force' |" unregister.sh
}

package() {
    cd ${srcdir}/${_hkgname}-${pkgver}
    install -D -m744 register.sh   ${pkgdir}/usr/share/haskell/${pkgname}/register.sh
    install    -m744 unregister.sh ${pkgdir}/usr/share/haskell/${pkgname}/unregister.sh
    install -d -m755 ${pkgdir}/usr/share/doc/ghc/html/libraries
    ln -s /usr/share/doc/${pkgname}/html ${pkgdir}/usr/share/doc/ghc/html/libraries/${_hkgname}
    runhaskell Setup copy --destdir=${pkgdir}
    install -D -m644 LICENSE ${pkgdir}/usr/share/licenses/${pkgname}/LICENSE
    rm -f ${pkgdir}/usr/share/doc/${pkgname}/LICENSE
}
