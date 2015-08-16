pkgname=kdevelop-extra-plugins-python-git
pkgver=20120301
pkgrel=1
pkgdesc="A Python plugin for KDevelop - GIT build"
arch=('i686' 'x86_64')
url="https://projects.kde.org/projects/playground/devtools/plugins/kdev-python"
license=('GPL')
groups=('kde' 'kdevelop-extra-plugins')
depends=('kdevelop-git')
makedepends=('cmake' 'automoc4' 'git')
provides=('kdevelop-python')
conflicts=('kdevelop-python')
source=()
md5sums=()

_gitroot="git://anongit.kde.org/kdev-python"
_gitname="python"

build() {
    cd "${srcdir}"
    msg "Connecting to GIT server...."

    if [ -d "${_gitname}" ] ; then
        cd "${_gitname}"

	# Change remote url to anongit
        if [ -z $(git branch -v | grep anongit) ] ; then
            git remote set-url origin ${_gitroot}
        fi
	
	git pull origin
        msg "The local files are updated."
    else
        git clone "${_gitroot}" "${_gitname}"
    fi

    cd "${srcdir}/${_gitname}"
    sed -i 's|#!.*python$|&2|' python-src/Parser/asdl_c.py

    msg "GIT checkout done or server timeout"
    msg "Starting make..."

    mkdir -p "${srcdir}/build"
    cd "${srcdir}/build"

    cmake "../${_gitname}" \
        -DCMAKE_SKIP_RPATH=ON \
        -DCMAKE_BUILD_TYPE=RELWITHDEBINFO \
        -DCMAKE_{SHARED,MODULE,EXE}_LINKER_FLAGS='-Wl,--no-undefined -Wl,--as-needed' \
        -DCMAKE_INSTALL_PREFIX=/usr

    make parser
    make
}

package() {
    cd "${srcdir}/build"
    make DESTDIR="${pkgdir}" install
}
