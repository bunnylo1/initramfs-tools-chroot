# Maintainer bunnylo1 <bunnylo12@yahoo.com>

pkgbase='initramfs-tools-chroot'
_pkgmain='initramfs-tools'
_pkgcore="${_pkgmain}-core"
_pkgbin="${_pkgmain}-bin"
pkgname=(
"${_pkgmain}"
"${_pkgcore}"
"${_pkgbin}"
)
pkgver='0.143'
pkgrel='mpr'
arch=(any)
depends=()
makedepends=(
    'patch'
    "make"
    "c-compiler"
    'coreutils'
    'pkg-config' 
    'libudev-dev'
)


source=(
    git+"https://git.launchpad.net/ubuntu/+source/initramfs-tools"
    'chroot_patch.patch'
)

sha256sums=( 
    'SKIP'
    'b2fecc07c9b8a4b49bd997faf8e99bd97c5d62ecf64492607860f662e80f85e8'
)

prepare(){
    cd initramfs-tools
    patch -p1 -t -i ../chroot_patch.patch || :
}

shopt -sq expand_aliases

alias _pkg_main="package_${_pkgmain}"

alias _pkg_bin="package_${_pkgbin}"

alias _pkg_core="package_${_pkgcore}"

_copy(){
    rm -R "$2" || echo
    mkdir -p $(dirname "$2") || echo
    rsync -axHAX --mkpath "$1" "$2"
}

build(){
    cd "${srcdir}/${_pkgmain}/src"
    make 
}

_pkg_bin(){
    for i in postinst preinst postrm prerm; do
        install -Dm755 "${srcdir}/${_pkgmain}/debian/${pkgname}.$i" "${pkgdir}/DEBIAN/$i" || :
    done
    pkgdesc="binaries used by initramfs-tools
This package contains binaries used inside the initramfs images generated
by initramfs-tools."
    depends=(
    "libc6"
    "libgcc-s1" 
    "libudev1"
    )

    src="${srcdir}/${_pkgmain}"   
    _copy "${src}/src/wait-for-root" "${pkgdir}/usr/lib/initramfs-tools/bin/src/wait-for-root"
    _copy "${src}/src/gcc_s1-stub" "${pkgdir}/usr/lib/initramfs-tools/bin/src/gcc_s1-stub"
}

_pkg_core(){
    for i in postinst preinst postrm prerm; do
        install -Dm755 "${srcdir}/${_pkgmain}/debian/${pkgname}.$i" "${pkgdir}/DEBIAN/$i" || :
    done
    pkgarch=all
    multiarch=foreign
    pkgdesc="generic modular initramfs generator (core tools)
This package contains the mkinitramfs program that can be used to
create a bootable initramfs for a Linux kernel.  The initramfs should
be loaded along with the kernel and is then responsible for mounting
the root filesystem and starting the main init system."
    
    depends=(
    "klibc-utils" 
    "cpio" 
    "kmod" 
    "udev" 
    "coreutils" 
    "logsave|e2fsprogs" 
    )
 src="${srcdir}/${_pkgmain}"   

 _copy "${src}/lsinitramfs" "${pkgdir}/usr/bin/lsinitramfs"
 _copy "${src}/unmkinitramfs" "${pkgdir}/usr/bin/unmkinitramfs"	
 _copy "${src}/init"	"${pkgdir}/usr/share/initramfs-tools/init"
 _copy "${src}/scripts"	"${pkgdir}/usr/share/initramfs-tools/scripts"
 _copy "${src}/conf/initramfs.conf" "${pkgdir}/etc/initramfs-tools/conf/initramfs.conf"
 _copy "${src}/hooks" "${pkgdir}/usr/share/initramfs-tools/hooks"
 _copy "${src}/hook-functions" "${pkgdir}/usr/share/initramfs-tools/hook-functions"
 _copy "${src}/conf/modules" "${pkgdir}/usr/share/initramfs-tools/conf/modules"
    
}

_pkg_main(){
    for i in postinst preinst postrm prerm; do
        install -Dm755 "${srcdir}/${_pkgmain}/debian/${pkgname}.$i" "${_pkgmain}/DEBIAN/$i" || :
    done
    pkgarch=all
    multiarch=foreign
    #cp "${srcdir}/${pkgbase}/debian/${pkgbase}".* "${pkgdir}/DEBIAN/"
    depends=(
    "initramfs-tools-core"
    "linux-base"
    )

    pkgdesc="generic modular initramfs generator (automation)
This package builds a bootable initramfs for Linux kernel packages. The
initramfs is loaded along with the kernel and is responsible for
mounting the root filesystem and starting the main init system."

 src="${srcdir}/${pkgbase}"   
 _copy "${src}/conf/update-initramfs.conf" "${pkgdir}/etc/initramfs-tools/conf/update-initramfs.conf"
 _copy "${src}/update-initramfs" "${pkgdir}/usr/sbin/update-initramfs"
 _copy "${src}/debian/script" "${pkgdir}/usr/share/bug/initramfs-tools/debian/script"
 _copy "${src}/kernel" "${pkgdir}/etc/kernel"

}
