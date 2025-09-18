# Reference: <https://postmarketos.org/vendorkernel>
# Kernel config based on: arch/x86_64/configs/xiaomipad2_defconfig

pkgname=linux-xiaomi-latte
pkgver=6.12.0
pkgrel=0
pkgdesc="XiaoMi Pad2 Upstream Kernel Patch"
arch="x86_64"
_carch="x86_64"
_flavor="xiaomi-latte"
url="https://github.com/torvalds/linux"
license="GPL-2.0-only"
options="!strip !check !tracedeps pmb:cross-native"
makedepends="bash bc bison devicepkg-dev flex openssl-dev perl gcompat linux-headers elfutils-dev gawk sbsigntool"

# Source
_config=xiaomipad2_defconfig
_commit="adc218676eef25575469234709c2d87185ca223a"
source="
    $pkgname-$_commit.tar.gz::https://github.com/torvalds/linux/archive/$_commit.tar.gz
    $_config
    latte-audio-fix.patch
    latte-i915-fix.patch
    EFI/boot/bootx64.efi
    EFI/boot/grub.cfg
    EFI/boot/grubx64.efi
    EFI/boot/KeyTool.efi
    EFI/boot/mmx64.efi
    EFI/MOK.crt
    EFI/MOK.key
    MOK.cer
"
builddir="$srcdir/linux-$_commit"
_outdir="out"

prepare() {
    default_prepare
    rm -rf ${builddir}/include/config
    REPLACE_GCCH=0 . downstreamkernel_prepare
}

build() {
    unset LDFLAGS
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
6fb84e3306018ce097cc4715befccd5233e7777671767efdfccc7847d2a0a781c1c6ef51126070c90775f5f0b461a9ad29b6071c4b533a00c8d8e9031815fa68  linux-xiaomi-latte-adc218676eef25575469234709c2d87185ca223a.tar.gz
9a932de6aa5c831268cda53fbff9d895a170fad882c205cfb46d240ea92ae68cd86a0a950e1a3a1fa1239b687aa8793a1852b1ff472c26b1c0b192f5abf778c0  xiaomipad2_defconfig
ca11535c6d2c96798a5884d378b9a1c48e129ae49d275ca4d1ab4509eae1c2770677f31828f6078b3968da99f542d208c3a16d35297596e9353ff9942f815fab  latte-audio-fix.patch
3f71cf97574f22119c203fc28b0b72e402108f021c5bd1b9107e78ac15560dd79a82c013faacc19601b5fe59d854a5d2d5f1e2ded2f2edd07bb17b59992f96e2  latte-i915-fix.patch
bb03f2c6f0c8bc7ca3f9e670ddc9fee05a29fc1708b86a4007498418e5aa17c33f79d383c38c8811a89a93e37bd737aa975db210ced4d64999cd904f84808aaa  bootx64.efi
5dd565e4f68bce6f67c5d72e788eaf9e92fa8eb10e1a6e821abd8d2f36d9bcc38a9d860229c4554eedf5fddd05783d931351e5ea52e3c1d4af981bba5f470dad  grub.cfg
bad3d37fb7451ad28c722698c4c6c3a359cacdf7a58acf0a5ace813217c76462981d0f08e6fc9affcdb4d47bc303fb63b52b085a7697788053b2859387b9ad92  grubx64.efi
0d499348919200c1b8e09bb424677c94a7c3df35b164631f81b5c47b18fbec93e3dbbcdf402407c0e767ec59194d1ef10054671c48d540db9671a769cdc5a6e8  KeyTool.efi
5633221cc5e47da733d499d8f21e99568f15e160e627a250a52b72508776e77130e9e74ce337692dd90a3adab0497dad33327f73e3c06f072b34329a123c37c0  mmx64.efi
fedc27118b43badbcd47f44ac127ff9f02ce4e53ba04350c5bd893d6f6f6ff687c73f8cc7b15e8c71bc0def1ec63247ab675e95b339402edbaa744a3d8acf39d  MOK.crt
1b5d43a851e7dc4085e34368b145752d4f8d00671db097f09b85a0152e1427486e7887f9a7fd876b9f97ca4f22dcd584c496f2d8bd9a7d2e64cc1fecad8ce0f0  MOK.key
5fdeb5fc0c7fdc61d1a948abc69168966c2c7893a18f54e7583a564555b4a8a360fc24999580d9498ba55d7db63493468c3f4f4690a0f40a7105bb408da6d305  MOK.cer
"
