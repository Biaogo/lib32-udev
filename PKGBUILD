# Maintainer: Christian Hesse <mail@eworm.de>
# Maintainer: Dave Reisner <dreisner@archlinux.org>
# Maintainer: Tom Gundersen <teg@jklm.no>

_pkgbase=systemd-stable

pkgname=lib32-udev
_tag='2d7670ddc4473e18c92b15b4a4522e4f12a59d33' # git rev-parse v${_tag_name}
_tag_name=254.4
pkgver="${_tag_name/-/}"
pkgrel=1
pkgdesc='Userspace device file manager (32-bit)'
arch=('x86_64')
url='https://www.github.com/systemd/systemd'
license=('GPL2' 'LGPL2.1')
provides=("lib32-udev=${pkgver}" 'lib32-eudev')
replaces=('lib32-eudev')
depends=('lib32-gcc-libs' 'udev' 'lib32-libcap' 'lib32-glibc')
makedepends=('git' 'gperf' 'intltool' 'lib32-acl'
             'lib32-glib2' 'lib32-gnutls' 'lib32-libelf'
             'libxslt' 'meson' 'python-jinja' 'python-lxml' 'lib32-libgcrypt')
options=('strip')
validpgpkeys=('63CDA1E5D3FC22B998D20DD6327F26951A015CC4'  # Lennart Poettering <lennart@poettering.net>
              'A9EA9081724FFAE0484C35A1A81CEA22BC8C7E2E'  # Luca Boccassi <luca.boccassi@gmail.com>
              '9A774DB5DB996C154EBBFBFDA0099A18E29326E1'  # Yu Watanabe <watanabe.yu+github@gmail.com>
              '5C251B5FC54EB2F80F407AAAC54CA336CFEB557E') # Zbigniew Jędrzejewski-Szmek <zbyszek@in.waw.pl>
source=("git+https://github.com/systemd/systemd-stable#tag=${_tag}" #?signed
        "git+https://github.com/systemd/systemd#tag=v${_tag_name%.*}" #?signed
        0001-artix-standalone-install.patch)
sha512sums=('SKIP'
            'SKIP'
            '3d985204cda5faadc21188874127a03d1543072e16c11eca871ea000d273c226ffba3b458d06606fdef4334326bd6fed727fe1d781c763871ff4bdaa8fb42d66')

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

        -Dversion-tag="${_tag_name/-/\~}-${pkgrel}-artix"
        -Dshared-lib-tag="${pkgver}-${pkgrel}"
        -Dmode=release

        # features
        -Daudit=false
        -Dblkid=false
        #-Dgnu-efi=false
        -Dima=false
        -Dkmod=false
        -Dlibcryptsetup=false
        -Dlibidn2=false
        -Dlibiptc=false
        -Dlz4=false
        -Dmicrohttpd=false
        -Dpam=false
        -Dseccomp=false

        -Dlink-udev-shared=false

        # components
        -Dutmp=false
        -Dhibernate=false
        -Dldconfig=false
        -Dresolve=false
        -Defi=false
        -Dtpm=false
        -Denvironment-d=false
        -Dbinfmt=false
        -Drepart=false
        -Dcoredump=false
        -Dpstore=false
        -Doomd=false
        -Dlogind=false
        -Dhostnamed=false
        -Dlocaled=false
        -Dmachined=false
        -Dportabled=false
        -Dsysext=false
        -Duserdb=false
        -Dhomed=false
        -Dnetworkd=false
        -Dtimedated=false
        -Dtimesyncd=false
        -Dremote=false
        -Dcreate-log-dirs=false
        -Dnss-myhostname=false
        -Dnss-mymachines=false
        -Dnss-resolve=false
        -Dnss-systemd=false
        -Dfirstboot=false
        -Drandomseed=false
        -Dbacklight=false
        -Dvconsole=false
        -Dquotacheck=false
        -Dsysusers=false
        -Dtmpfiles=false
        -Dimportd=false
        -Dhwdb=false
        -Drfkill=false
        -Dxdg-autostart=false
        -Dman=false
        -Dhtml=false
        -Dtranslations=false

        -Ddbuspolicydir=/usr/share/dbus-1/system.d
        -Ddefault-hierarchy=unified
        -Ddefault-kill-user-processes=false
        -Ddefault-locale=C
        -Dfallback-hostname='artixlinux'
        -Dnologin-path=/usr/bin/nologin
        -Dntp-servers=
        -Ddns-servers=
        -Drpmmacrosdir=no
        -Dsysvinit-path=
        -Dsysvrcnd-path=
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
}
