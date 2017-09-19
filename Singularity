# -*- mode: rpm-spec -*-

# Released into the Public Domain:
# https://creativecommons.org/publicdomain/zero/1.0/legalcode

Bootstrap: docker
From: centos:7

%post

# Install OS packages.
debug="strace"
dev="tar gzip gcc gcc-c++ make"
build_deps="wxGTK3-devel fftw-devel libtiff-devel"
runtime_deps="pdftk gnuplot"
yum -y install epel-release	# for wxGTK3-devel
yum -y install $dev $build_deps $runtime_deps $debug
# RedHat Bug 1077718
update-alternatives --install /bin/wx-config wx-config /bin/wx-config-3.0 10

# Install ctffind.
pn=ctffind
v=4.1.8
url="http://grigoriefflab.janelia.org/sites/default/files/${pn}-${v}.tar.gz"
tarball=$(basename $url)
tardir=${tarball%.tar.*}
test -f $tarball || curl -o $tarball $url
tar -xf $tarball
cd $tardir
prefix=/usr
./configure --prefix=$prefix
make -j $(nproc) install
rm -f $prefix/bin/{install-sh,config.sub,config.guess,ltmain.sh,depcomp}

%test
which ctffind
which ctffind_plot_results.sh
echo | ctffind | head -6

%runscript
exec ctffind
