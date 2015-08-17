# PACKAGER : Piotr Rogoża <rogoza dot piotr at gmail dot com>"

pkgname=nvidia-vs
pkgver=270.41.06
_kernver='2.6.36-vs-ARCH'
pkgrel=1
pkgdesc="NVIDIA drivers for kernel26-vserver."
arch=('i686' 'x86_64')
url="http://www.nvidia.com/"
depends=('kernel26-vserver>=2.6.36' 'kernel26-vserver<2.6.37' "nvidia-utils=${pkgver}")
conflicts=('nvidia-96xx' 'nvidia-173xx')
license=('custom')
install=nvidia.install
options=(!strip)

if [ "$CARCH" = "i686" ]; then
	_arch='x86'
	_pkg="NVIDIA-Linux-${_arch}-${pkgver}"
        source=("ftp://download.nvidia.com/XFree86/Linux-${_arch}/${pkgver}/${_pkg}.run")
        md5sums=('702868c77b13bba46f0fce26ed7a1fb7')
elif [ "$CARCH" = "x86_64" ]; then
	_arch='x86_64'
	_pkg="NVIDIA-Linux-${_arch}-${pkgver}-no-compat32"
        source=("ftp://download.nvidia.com/XFree86/Linux-${_arch}/${pkgver}/${_pkg}.run")
        md5sums=('d62243b91d0dc8f2c4b2ab646b72ca65')
fi

build() {
	cd $srcdir
	sh ${_pkg}.run --extract-only
	cd ${_pkg}/kernel
 	make SYSSRC=/lib/modules/${_kernver}/build module
}

package() {
	install -D -m644 $srcdir/${_pkg}/kernel/nvidia.ko \
		$pkgdir/lib/modules/${_kernver}/kernel/drivers/video/nvidia.ko
        install -d -m755 $pkgdir/etc/modprobe.d
        echo "blacklist nouveau" >> $pkgdir/etc/modprobe.d/nouveau_blacklist.conf
	sed -i -e "s/KERNEL_VERSION='.*'/KERNEL_VERSION='${_kernver}'/" $startdir/nvidia.install
        # gzip -9 all modules to safe 100MB of space
        find "$pkgdir" -name '*.ko' -exec gzip -9 {} \;
}
