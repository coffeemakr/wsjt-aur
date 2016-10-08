pkgname="wsjt-svn"
pkgver=r7163
pkgrel=1
arch=('x86_64' 'i386')
url="http://www.physics.princeton.edu/pulsar/K1JT/wspr.html"
source=()
makedepends=(
	'gcc-fortran'
	'subversion'
	'python2'
)
depends=(
	'python3'
	'python3-numpy'
	'python2' # required?
)
conflicts=("wsjt")
sha512sums=()
_svnurl="svn://svn.code.sf.net/p/wsjt/wsjt/trunk"
_svnlocal="${pkgname}-trunk"
_svncred=anonsvn

pkgver() {
  cd "${_svnlocal}"
  local version="$(svnversion)"
  msg "Repo reports revision ${version}"
  printf "r%d" "${version//[[:alpha:]]}"
}

prepare() {
  cd "${srcdir}"
  if [[ -d "${_svnlocal}/.svn" ]]; then
    msg "Reusing existing SVN repository at ${_svnlocal}"
    cd "${_svnlocal}"
    svn update --no-auth-cache --username "${_svncred}" --password "${_svncred}"
  else
    msg "No local Subversion repo found, creating new one at ${_svnlocal}"
    svn checkout --no-auth-cache --username "${_svncred}" --password "${_svncred}" \
      "${_svnurl}" "${_svnlocal}"
  fi
  msg "Repo up to date"
}

build() {
  svnroot="${srcdir}/${_svnlocal}"
  cd "${svnroot}"
  msg "Compiling"
  unset LDFLAGS
  ./configure --prefix=/usr --with-python2=/usr/bin/python2 --with-python3=/usr/bin/python3 --with-f2py=/usr/bin/f2py --disable-manpages --disable-docs
  make
}

package() {
  svnroot="${srcdir}/${_svnlocal}"
  cd "${svnroot}"
  make DESTDIR="${pkgdir}" install
}
