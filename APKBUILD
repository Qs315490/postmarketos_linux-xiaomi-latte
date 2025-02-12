# Reference: <https://postmarketos.org/vendorkernel>
# Kernel config based on: arch/x86_64/configs/xiaomipad2_defconfig

pkgname=linux-xiaomi-latte
pkgver=6.10.0
pkgrel=0
pkgdesc="XiaoMi Pad2 kernel fork"
arch="x86_64"
_carch="x86_64"
_flavor="xiaomi-latte"
url="https://github.com/Qs315490/latte_kernel_6.10"
license="GPL-2.0-only"
options="!strip !check !tracedeps pmb:cross-native"
makedepends="bash bc bison devicepkg-dev flex openssl-dev perl gcompat"

# Source
_repository="latte_kernel_6.10"
_commit="8efe3a75dadc48924518e35ac353f917600dd0dc"
_config="config-$_flavor.$arch"
source="
    $pkgname-$_commit.tar.gz::https://github.com/Qs315490/$_repository/archive/$_commit.tar.gz
    $_config
    EFI/boot/bootx64.efi
    EFI/boot/grub.cfg
    EFI/boot/grubx64.efi
    EFI/boot/KeyTool.efi
    EFI/boot/mmx64.efi
    EFI/MOK.crt
    EFI/MOK.key
    MOK.cer
"
builddir="$srcdir/$_repository-$_commit"
_outdir="out"

prepare() {
    default_prepare
    REPLACE_GCCH=0 . downstreamkernel_prepare
}

build() {
    unset LDFLAGS
    make xiaomipad2_defconfig
    make O="$_outdir" ARCH="$_carch" CC="${CC:-gcc}" LOCALVERSION= \
            KBUILD_BUILD_VERSION="$((pkgrel + 1 ))-postmarketOS"
}

package() {
    downstreamkernel_package "$builddir" "$pkgdir" "$_carch" "$_flavor" "$_outdir"
    cp -r ${startdir}/EFI $pkgdir/boot/
    cp ${startdir}/MOK.cer $pkgdir/boot/
    sbsign --key ${startdir}/EFI/MOK.key --cert ${startdir}/EFI/MOK.crt --output $pkgdir/boot/vmlinuz-$pkgver $pkgdir/boot/vmlinuz-*
}

sha512sums="
d14bd852eaf2fecfc050febb56dc95b12460cc6d5b9696e8ffb432bbd75960bbfc4cfc6bfe8d6ad77ea40d16d5c8fbd6bd9a7ce048cf3b33e16da3784cba0a5f  linux-xiaomi-latte-8efe3a75dadc48924518e35ac353f917600dd0dc.tar.gz
342d724f86320842a4939144b21403e49639e32d3eb5163b206964705e283b0a1c21d8a26ad4f260a3708e889e3cd391b8eea0703309e87a104f05ec1a9aed74  config-xiaomi-latte.x86_64
bb03f2c6f0c8bc7ca3f9e670ddc9fee05a29fc1708b86a4007498418e5aa17c33f79d383c38c8811a89a93e37bd737aa975db210ced4d64999cd904f84808aaa  bootx64.efi
5dd565e4f68bce6f67c5d72e788eaf9e92fa8eb10e1a6e821abd8d2f36d9bcc38a9d860229c4554eedf5fddd05783d931351e5ea52e3c1d4af981bba5f470dad  grub.cfg
bad3d37fb7451ad28c722698c4c6c3a359cacdf7a58acf0a5ace813217c76462981d0f08e6fc9affcdb4d47bc303fb63b52b085a7697788053b2859387b9ad92  grubx64.efi
0d499348919200c1b8e09bb424677c94a7c3df35b164631f81b5c47b18fbec93e3dbbcdf402407c0e767ec59194d1ef10054671c48d540db9671a769cdc5a6e8  KeyTool.efi
5633221cc5e47da733d499d8f21e99568f15e160e627a250a52b72508776e77130e9e74ce337692dd90a3adab0497dad33327f73e3c06f072b34329a123c37c0  mmx64.efi
fedc27118b43badbcd47f44ac127ff9f02ce4e53ba04350c5bd893d6f6f6ff687c73f8cc7b15e8c71bc0def1ec63247ab675e95b339402edbaa744a3d8acf39d  MOK.crt
1b5d43a851e7dc4085e34368b145752d4f8d00671db097f09b85a0152e1427486e7887f9a7fd876b9f97ca4f22dcd584c496f2d8bd9a7d2e64cc1fecad8ce0f0  MOK.key
5fdeb5fc0c7fdc61d1a948abc69168966c2c7893a18f54e7583a564555b4a8a360fc24999580d9498ba55d7db63493468c3f4f4690a0f40a7105bb408da6d305  MOK.cer
"
