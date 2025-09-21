# Reference: <https://postmarketos.org/vendorkernel>
# Kernel config based on: arch/x86_64/configs/xiaomipad2_defconfig

pkgname=linux-xiaomi-latte
pkgver=6.14.0
pkgrel=0
pkgdesc="XiaoMi Pad2 kernel fork"
arch="x86_64"
_carch="x86_64"
_flavor="xiaomi-latte"
url="https://github.com/Qs315490/linux_latte"
license="GPL-2.0-only"
options="!strip !check !tracedeps pmb:cross-native"
makedepends="binutils xz sbsigntool"

# Source
source="
	linux-image-6.14.0_6.14.0-ge6fbd0997ea2-35_amd64.deb
	MOK.cer
	EFI/boot/bootx64.efi
	EFI/boot/grub.cfg
	EFI/boot/grubx64.efi
	EFI/boot/mmx64.efi
	EFI/boot/KeyTool.efi
	EFI/boot/setup_var.efi
	EFI/boot/linux-usb-daget_ssh_123456.efi
	EFI/MOK.key
	EFI/MOK.crt
"

package() {
	# echo $startdir $srcdir
	cd "$srcdir"
    echo "decompressing linux-image-${pkgver}"
	ar -x "linux-image-${pkgver}"*"_amd64.deb"
	# ls -al $srcdir
	mkdir -p $pkgdir/usr/share/kernel/$_flavor
	echo "$pkgver" > $pkgdir/usr/share/kernel/$_flavor/kernel.release
	# 解压data.tar.xz到pkgdir 排除 etc usr
    echo "extracting data.tar.xz"
	tar -xJf data.tar.xz -C "$pkgdir" --exclude etc --exclude usr
    echo "extract done"
	# grub-install --target=x86_64-efi --efi-directory=$pkgdir/boot --boot-directory=$pkgdir/boot
	cp -r ${startdir}/EFI $pkgdir/boot/
	cp ${startdir}/MOK.cer $pkgdir/boot/
    echo "signing kernel"
	sbsign --key ${startdir}/EFI/MOK.key --cert ${startdir}/EFI/MOK.crt --output $pkgdir/boot/vmlinuz-$pkgver $pkgdir/boot/vmlinuz-*
}

sha512sums="
31817fb8568cf122edb20bdf59319ebc429ae8cad34ce99ff8c6462160850b9571edff15dded1bdb0cd537602af14831624d3a026f286aa783702c767166bc24  linux-image-6.14.0_6.14.0-ge6fbd0997ea2-35_amd64.deb
5fdeb5fc0c7fdc61d1a948abc69168966c2c7893a18f54e7583a564555b4a8a360fc24999580d9498ba55d7db63493468c3f4f4690a0f40a7105bb408da6d305  MOK.cer
bb03f2c6f0c8bc7ca3f9e670ddc9fee05a29fc1708b86a4007498418e5aa17c33f79d383c38c8811a89a93e37bd737aa975db210ced4d64999cd904f84808aaa  bootx64.efi
b19787a0aed0c6041613a3e2a7078a6a6ff4c9e373fd9f502d7d718e45a7d6228015a5c8e8afff83e6e030836885a9d37374783173dd5ee65ab1193041ff37ce  grub.cfg
bad3d37fb7451ad28c722698c4c6c3a359cacdf7a58acf0a5ace813217c76462981d0f08e6fc9affcdb4d47bc303fb63b52b085a7697788053b2859387b9ad92  grubx64.efi
5633221cc5e47da733d499d8f21e99568f15e160e627a250a52b72508776e77130e9e74ce337692dd90a3adab0497dad33327f73e3c06f072b34329a123c37c0  mmx64.efi
0d499348919200c1b8e09bb424677c94a7c3df35b164631f81b5c47b18fbec93e3dbbcdf402407c0e767ec59194d1ef10054671c48d540db9671a769cdc5a6e8  KeyTool.efi
cf895f38d588d7c24a644c8a222d00cd8f703892a9a5681378ca741396ae5f3dbac36defa9f512af70742310144095b4f6630aca7575c36826148a0190dcccff  setup_var.efi
4e834728aa1b353824dafc8eacf81f7a11aad248dbaf75e842d5166a9a9f8003b6418981b93de32fe928f9a84ac39c61a4d0442bc77dbf7a076f85e95ba893ea  linux-usb-daget_ssh_123456.efi
1b5d43a851e7dc4085e34368b145752d4f8d00671db097f09b85a0152e1427486e7887f9a7fd876b9f97ca4f22dcd584c496f2d8bd9a7d2e64cc1fecad8ce0f0  MOK.key
fedc27118b43badbcd47f44ac127ff9f02ce4e53ba04350c5bd893d6f6f6ff687c73f8cc7b15e8c71bc0def1ec63247ab675e95b339402edbaa744a3d8acf39d  MOK.crt
"
