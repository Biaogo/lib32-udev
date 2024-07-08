# Maintainer: Christian Hesse <mail@eworm.de>
# Maintainer: Dave Reisner <dreisner@archlinux.org>
# Maintainer: Tom Gundersen <teg@jklm.no>

_pkgbase=systemd-stable
_tag=255.6

pkgname=lib32-udev
pkgver="${_tag/~/}"
pkgrel=1
pkgdesc='Userspace device file manager (32-bit)'
arch=('x86_64')
url='https://www.github.com/systemd/systemd'
license=(
    'GPL-2.0-or-later'
    'LGPL-2.1-or-later'
)
provides=(
    'libudev.so'
)
depends=(
    'lib32-gcc-libs'
    'libudev'
    'lib32-libcap'
    'lib32-glibc'
)
makedepends=(
    'git'
    'gperf'
    'intltool'
    'lib32-acl'
    'lib32-glib2'
    'lib32-gnutls'
    'lib32-libelf'
    'libxslt'
    'meson'
    'python-jinja'
    'python-lxml'
    'lib32-libgcrypt'
)
checkdepends=('udev')
options=('strip')
validpgpkeys=('63CDA1E5D3FC22B998D20DD6327F26951A015CC4'  # Lennart Poettering <lennart@poettering.net>
              'A9EA9081724FFAE0484C35A1A81CEA22BC8C7E2E'  # Luca Boccassi <luca.boccassi@gmail.com>
              '9A774DB5DB996C154EBBFBFDA0099A18E29326E1'  # Yu Watanabe <watanabe.yu+github@gmail.com>
              '5C251B5FC54EB2F80F407AAAC54CA336CFEB557E') # Zbigniew Jędrzejewski-Szmek <zbyszek@in.waw.pl>
source=("git+https://github.com/systemd/systemd-stable#tag=v${_tag}" #?signed
        "git+https://github.com/systemd/systemd#tag=v${_tag%.*}" #?signed
        0001-artix-standalone-install.patch)
sha512sums=('c1de1eb0d0ef6d8da81a105cdfcb86634bed6f46ab1038de9ab786fd85f59524e7eb30fe1d02dbf2c3b3a29dc66d04a102b2274a09ad3d2c18953c380099aa0e'
            'd430427987309483c99062adb02741d25239ba5fbb97053ef817c0c5a0a935328af9c8b651de2b119b0e851dcf6623f01343859735ff81d7013ab0133e67c7ea'
            '28882b12d640938ac36b3a8a6ede1aa58185b125beb10a58c63f8ee9a18096da29b0afe31e8db04e5c37d7bd315effb75425320d96fe2b7252292b1d3e955cae')

_backports=(
)

_reverts=(
)

prepare() {
    cd "$_pkgbase"

    # add upstream repository for cherry-picking
    git remote add -f upstream ../systemd

    local _c
    for _c in "${_backports[@]}"; do
        git cherry-pick -n "${_c}"
    done
    for _c in "${_reverts[@]}"; do
        git revert -n "${_c}"
    done
    patch -Np1 -i ../0001-artix-standalone-install.patch
}

build() {
    export CC="gcc -m32"
    export CXX="g++ -m32"
    export PKG_CONFIG_PATH="/usr/lib32/pkgconfig"

    local _meson_options=(
        --libexecdir	/usr/lib32
        --libdir		/usr/lib32

        -Dversion-tag="${pkgver}-${pkgrel}-artix"
        -Dshared-lib-tag="${pkgver}-${pkgrel}"
        -Dmode=release

        -Dstandalone-binaries=false
        -Dsysusers=false
        -Dtmpfiles=false

        -Dhwdb=false

        -Dblkid=disabled

        -Dkmod=disabled

        -Dgshadow=false

        -Dsbat-distro='artix'
        -Dsbat-distro-summary='Artix Linux'
        -Dsbat-distro-pkgname="${pkgname}"
        -Dsbat-distro-version="${pkgver}"

        -Dtests=false

        -Dlink-udev-shared=false
        #-Dlink-boot-shared=false

        -Ddefault-keymap='us'

        -Dman=disabled

        -Dhtml=disabled

        -Ddns-servers=''
        -Dntp-servers=''
        -Defi=false

        -Dsysvinit-path=
        -Dsysvrcnd-path=
        -Ddefault-dnssec=no

        -Ddefault-llmnr=no
        -Ddefault-mdns=no

        -Dadm-group=false
        -Danalyze=false

        -Dapparmor=disabled
        -Daudit=disabled
        -Dbacklight=false
        -Dbinfmt=false
        -Dbootloader=disabled
        -Dbzip2=disabled
        -Dcoredump=false
        -Ddbus=disabled
        -Delfutils=disabled
        -Denvironment-d=false
        -Dfdisk=disabled
        -Dgcrypt=disabled
        -Dglib=disabled
        -Dgnutls=disabled
        -Dhibernate=false
        -Dhostnamed=false
        -Didn=false
        -Dima=false
        -Dinitrd=false
        -Dfirstboot=false
        -Dkernel-install=false
        -Dldconfig=false
        -Dlibcryptsetup=disabled
        -Dlibcurl=disabled
        -Dlibfido2=disabled
        -Dlibidn=disabled
        -Dlibidn2=disabled
        -Dlibiptc=disabled
        -Dlocaled=false
        -Dlogind=false
        -Dlz4=disabled
        -Dmachined=false
        -Dmicrohttpd=disabled
        -Dnetworkd=false
        -Dnscd=false
        -Dnss-myhostname=false
        -Dnss-resolve=disabled
        -Dnss-systemd=false
        -Doomd=false
        -Dopenssl=disabled
        -Dp11kit=disabled
        -Dpam=disabled
        -Dpcre2=disabled
        -Dpolkit=disabled
        -Dportabled=false
        -Dpstore=false
        -Dpwquality=disabled
        -Drandomseed=false
        -Dresolve=false
        -Drfkill=false
        -Dseccomp=disabled
        -Dsmack=false
        -Dsysext=false
        -Dtimedated=false
        -Dtimesyncd=false
        -Dtpm=false
        -Dqrencode=disabled
        -Dquotacheck=false
        -Duserdb=false
        -Dutmp=false
        -Dvconsole=false
        -Dwheel-group=false
        -Dxdg-autostart=false
        -Dxkbcommon=disabled
        -Dxz=disabled
        -Dzlib=disabled
        -Dzstd=disabled

        -Dbpf-framework=disabled
        -Dpasswdqc=disabled
        -Dselinux=disabled
        -Dxenctrl=disabled
        -Dlibcryptsetup-plugins=disabled
        -Drepart=false
        -Dsysupdate=false
        -Dimportd=false
        -Dhomed=false
        -Dremote=false
        -Dnss-mymachines=false
        -Dukify=false
        -Dtpm2=false
    )

    artix-meson "$_pkgbase" build "${_meson_options[@]}"

    local _targets=()
    _targets+=(
        udev:shared_library
        src/libudev/libudev.pc
    )

    meson compile -C build "${_targets[@]}"
}

check() {
    local tests=()
    tests+=(
        test-libudev
        test-libudev-sym
    )
    meson test -C build --print-errorlogs "${tests[@]}"
}

package() {
    meson install -C build --destdir="$pkgdir" --no-rebuild --tags libudev,libudev-devel

    rm -rf "$pkgdir"/usr/include
}
