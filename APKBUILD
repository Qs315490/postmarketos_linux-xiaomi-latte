# Reference: <https://postmarketos.org/vendorkernel>
# Kernel config based on: arch/x86_64/configs/xiaomipad2_defconfig
# bootx64.efi is shimx64.efi from fedora

pkgname=linux-xiaomi-latte
pkgver=6.16.0
pkgrel=0
pkgdesc="Upstream Kernel For XiaoMi Pad2"
arch="x86_64"
_carch="x86_64"
_flavor="xiaomi-latte"
url="https://github.com/torvalds/linux"
license="GPL-2.0-only"
options="!strip !check !tracedeps pmb:kconfigcheck-uefi"
makedepends="bash bc bison devicepkg-dev flex openssl-dev perl gcompat linux-headers elfutils-dev gawk sbsigntool"

# Source
_config=xiaomipad2_defconfig
_commit="038d61fd642278bab63ee8ef722c50d10ab01e8f"
source="
	$pkgname-$_commit.tar.gz::https://github.com/torvalds/linux/archive/$_commit.tar.gz
	$_config
	latte-audio-fix.patch
	latte-i915-fix.patch
	EFI/boot/bootx64.efi
	EFI/boot/grub.cfg
	EFI/boot/grubx64.efi
	EFI/boot/mmx64.efi
	MOK.crt
	MOK.key
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
	sbsign --key ${startdir}/MOK.key --cert ${startdir}/MOK.crt --output $pkgdir/boot/vmlinuz $pkgdir/boot/vmlinuz
}

sha512sums="
9cfd1b774fce4237966f32c1880e087490885b8464cefb468c777b6ff022e9ec22a0ec0c051940fff86813c5f535e7c8e291f8998fd5ae8eaebcd546729b1ee6  linux-xiaomi-latte-038d61fd642278bab63ee8ef722c50d10ab01e8f.tar.gz
450751cabdb1fcc8279dcab746d721998f0d066691a563f52e2d34720a42925e8532bb9ea6c7876e434fa554e969489d34fe7cada561460815f558c73db46cba  xiaomipad2_defconfig
7e3eeb8d54e2bf13bd9b126259daf2dd38cf698c3e66401a953d2ff235a87cfab33ea3f45575ec21cec3987f8cc0fd07f2aabc0c419753e215a851e219502025  latte-audio-fix.patch
7a311bb9ff309349fb5dc11a8a9b18f6595abb28108da60a67cd95dd9c713f150f29068c001400f47010fab1809f092ef2cc4393bab98a64592334ca8650519a  latte-i915-fix.patch
bb03f2c6f0c8bc7ca3f9e670ddc9fee05a29fc1708b86a4007498418e5aa17c33f79d383c38c8811a89a93e37bd737aa975db210ced4d64999cd904f84808aaa  bootx64.efi
5dd565e4f68bce6f67c5d72e788eaf9e92fa8eb10e1a6e821abd8d2f36d9bcc38a9d860229c4554eedf5fddd05783d931351e5ea52e3c1d4af981bba5f470dad  grub.cfg
bad3d37fb7451ad28c722698c4c6c3a359cacdf7a58acf0a5ace813217c76462981d0f08e6fc9affcdb4d47bc303fb63b52b085a7697788053b2859387b9ad92  grubx64.efi
5633221cc5e47da733d499d8f21e99568f15e160e627a250a52b72508776e77130e9e74ce337692dd90a3adab0497dad33327f73e3c06f072b34329a123c37c0  mmx64.efi
fedc27118b43badbcd47f44ac127ff9f02ce4e53ba04350c5bd893d6f6f6ff687c73f8cc7b15e8c71bc0def1ec63247ab675e95b339402edbaa744a3d8acf39d  MOK.crt
1b5d43a851e7dc4085e34368b145752d4f8d00671db097f09b85a0152e1427486e7887f9a7fd876b9f97ca4f22dcd584c496f2d8bd9a7d2e64cc1fecad8ce0f0  MOK.key
5fdeb5fc0c7fdc61d1a948abc69168966c2c7893a18f54e7583a564555b4a8a360fc24999580d9498ba55d7db63493468c3f4f4690a0f40a7105bb408da6d305  MOK.cer
"
