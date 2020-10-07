# Official Per Vices libUHD build script to compile libUHD from source.

pkgname=libuhdpv
pkgver=20200807git.tng.v2.5.r16.gc90009d40
_pkgid=git
pkgrel=1
pkgdesc="Alternative USRP Hardware Driver (UHD) userspace driver for Per Vices Crimson."
arch=('x86_64' 'i686')
url="http://www.pervices.com"
license=('GPL')
# depends=('boost-libs' 'orc' 'libusbx' 'python2')
# makedepends=('cmake' 'boost' 'python2-cheetah')
depends=('boost-libs' 'orc' 'libusb')
optdepends=('python: usrp utils')
makedepends=('cmake' 'boost' 'python-mako' 'python-cheetah3')

provides=('libuhd')
conflicts=('libuhd')

source=('uhd::git+https://github.com/pervices/uhd.git#branch=master')
#source=('uhd::git+https://github.com/pervices/uhd.git#branch=master-testing')
#source=('uhd::git+https://github.com/pervices/uhd.git#branch=master-fulltx')
#source=('uhd::git+https://github.com/pervices/uhd.git#branch=ops/vvv/master-testing')
#source=('uhd::git+https://github.com/pervices/uhd.git#commit=06ba790c2f')
#source=('uhd::git+https://github.com/pervices/uhd.git#commit=547549ac')
#source=('uhd::git+https://github.com/pervices/uhd.git#commit=53c696972')
#You can specify #branch=,#commit=,#tag=fragment
md5sums=('SKIP')
sha1sums=('SKIP')

_gitname="uhd"

pkgver() {
    #If we have already downloaded the repo, then use its HEAD for the package version. Otherwise, simply use the current date.
    cd ${srcdir}
    if [ -d ${_gitname} ]; then
        cd ${_gitname} && echo -n `date +%Y%m%d`git. && git describe --long | sed -E 's/([^-]*-g)/r\1/;s/-/./g'
    else
        date --utc +%Y%m%d%H%M 
    fi
}

build() {

#        if [ -d $_gitname ]; then
#                (cd $_gitname && git clean -x -d -f && git reset --hard HEAD && git checkout master && git fetch --all && git checkout $_gitbranch && git pull origin $_gitbranch)
#		(cd $_gitname && git clean -x -d -f && git reset --hard $_gitbranch && git fetch --all && git reset --hard $_gitbranch )
#                msg "Updated the local files."
#        else
#                git clone $_gitroot
#                msg "GIT checkout done or server timeout"
#        fi
        
        cd ${srcdir}/$_gitname/host
        
        #Fix to ensure python2 is always called.
        find -name "*.py" -or -name '*.py.in' | xargs sed -i "s|#!/usr/bin/env python$|#!/usr/bin/env python2|"

        #Make build directory
        mkdir -p build
        cd build
        
#        #Build project using CMAKE
#        cmake \
#           -DCMAKE_INSTALL_PREFIX=/usr/ \
#           -DPYTHON_EXECUTABLE=/usr/bin/python2 \
#           -DENABLE_EXAMPLES=ON \
#           -DENABLE_UTILS=ON \
#           -DENABLE_TESTS=OFF \
#           -DENABLE_E100=OFF \
#	   -DENABLE_USRP1=OFF \
#	   -DENABLE_USER2=OFF \
#	   -DENABLE_B200=OFF \
#	   -DENABLE_X300=OFF \
#	   -DENABLE_OCTOCLOCK=OFF \
#	   -DENABLE_DOXYGEN=OFF \
#	   -DENABLE_USB=OFF \
#           ..
#           
        #Build project using CMAKE
        cmake \
           -DCMAKE_INSTALL_PREFIX=/usr/ \
           -DENABLE_EXAMPLES=ON \
           -DENABLE_UTILS=ON \
           -DENABLE_TESTS=OFF \
           -DENABLE_E100=OFF \
	   -DENABLE_N230=OFF \
	   -DENABLE_N300=OFF \
	   -DENABLE_E320=OFF \
	   -DENABLE_USRP1=OFF \
	   -DENABLE_USER2=OFF \
	   -DENABLE_B200=OFF \
	   -DENABLE_X300=OFF \
       -DENABLE_CRIMSON_TNG=ON \
       -DENABLE_CYAN_16T=OFF \
       -DENABLE_CYAN_64T=OFF \
	   -DENABLE_OCTOCLOCK=OFF \
	   -DENABLE_DOXYGEN=OFF \
	   -DENABLE_USB=OFF \
           ..

        
        make -j4
}

check() {
  cd "$srcdir/$_gitname/host/build"
  make test
}

package() {
  cd "$srcdir/$_gitname/host/build"
  make DESTDIR="$pkgdir" install
  install -Dm644 "../utils/uhd-usrp.rules" "$pkgdir/usr/lib/udev/rules.d/10-uhd-usrp.rules"
} 
