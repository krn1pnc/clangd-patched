# Maintainer: krn1pnc <krn1pnc@outlook.com>

pkgname=clangd-patched
pkgver=18.r16371.g0ebe97115dc7
pkgrel=1
pkgdesc="Trunk version of standalone clangd binary, with a patch"
arch=("x86_64")
url="https://llvm.org/"
license=("custom:Apache 2.0 with LLVM Exception")
depends=("llvm-libs" "gcc" "compiler-rt")
makedepends=("git" "cmake" "ninja" "python")
source=("git+https://github.com/llvm/llvm-project.git"
        "prevent-oom.patch")
sha256sums=("SKIP"
            "340fb5b9941b040297174d4623f2e641f59eb79adba2f571f5df6b40c2e79caf")

pkgver() {
    cd "${srcdir}/llvm-project"

    git describe --long --match llvmorg-\* | sed -r "s/llvmorg-([^-]*)-(init|rc[0-9]+)-(.*)/\1-r\3/;s/-/./g"
}

prepare() {
    cd "${srcdir}/llvm-project"

    patch -p1 -i "${srcdir}/prevent-oom.patch"
}

build() {
    cd "${srcdir}/llvm-project"

    cmake -B build -S llvm \
        -G Ninja \
        -DCMAKE_INSTALL_PREFIX=/opt/clangd \
        -DCMAKE_BUILD_TYPE=Release \
        -DLLVM_ENABLE_PROJECTS="clang;clang-tools-extra"
    cmake --build build --target clangd
}

package() {
    cd "${srcdir}/llvm-project/build"

    cmake --install . --prefix "${pkgdir}/opt/clangd" --component clangd
    mkdir "${pkgdir}/opt/clangd/lib"
    cp -r ./lib/clang "${pkgdir}/opt/clangd/lib/clang"
}
