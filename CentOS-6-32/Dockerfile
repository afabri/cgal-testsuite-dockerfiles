FROM centos:centos6
MAINTAINER Philipp Moeller <bootsarehax@gmail.com>

RUN yum -y update; yum clean all
RUN yum -y install epel-release.noarch
RUN yum -y update; yum -y install \
                   cmake \
                   eigen3-devel.noarch \
                   gcc-c++ \
                   glew-devel.i686 \
                   glibc-devel.i686 \
                   glibc-devel.x86_64 \
                   gmp-devel.i686 \
                   libstdc++-devel.i686 \
                   libstdc++-devel.x86_64 \
                   make \
                   mpfr-devel.i686 \
                   ntl-devel.i686 \
                   qt-devel.i686 \
                   mesa-libGL-devel.i686 \
                   qt5-qtsvg-devel.i686 \
                   tar \
                   zlib-devel.i686
RUN yum clean all

ENV BOOST_MAJOR=1 BOOST_MINOR=48 BOOST_PATCH=0
RUN curl -s -SL http://sourceforge.net/projects/boost/files/boost/${BOOST_MAJOR}.${BOOST_MINOR}.${BOOST_PATCH}/boost_${BOOST_MAJOR}_${BOOST_MINOR}_${BOOST_PATCH}.tar.gz | tar xz
RUN cd boost_${BOOST_MAJOR}_${BOOST_MINOR}_${BOOST_PATCH} && \
    ./bootstrap.sh --without-libraries=filesystem,graph_parallel,python,iostreams,locale,mpi,serialization,signals,wave && \
    ./b2 cflags=-m32 \
         cxxflags=-m32 \
         address-model=32 \
         threading=multi \
         architecture=x86 \
         instruction-set=i686 \
         --prefix=/usr/local/ install

ENV QGL_REPO_32="\n[rineau-libQGLViewer-qt5-i386]\n\
name=Copr repo for libQGLViewer-qt5 owned by rineau (i386)\n\
baseurl=https://copr-be.cloud.fedoraproject.org/results/rineau/libQGLViewer-qt5/epel-6-i386/\n\
skip_if_unavailable=True\n\
gpgcheck=1\n\
gpgkey=https://copr-be.cloud.fedoraproject.org/results/rineau/libQGLViewer-qt5/pubkey.gpg\n\
enabled=1\n\
enabled_metadata=1"

RUN cd /etc/yum.repos.d/ && \
    curl -s -SLO https://copr.fedoraproject.org/coprs/rineau/libQGLViewer-qt5/repo/epel-6/rineau-libQGLViewer-qt5-epel-6.repo && \
    rpm --import https://copr-be.cloud.fedoraproject.org/results/rineau/libQGLViewer-qt5/pubkey.gpg && \
    echo -e "$QGL_REPO_32" >> rineau-libQGLViewer-qt5-epel-6.repo && \
    yum -y install libQGLViewer-qt5-devel.i686 libQGLViewer-devel.i686

ENV CC="gcc -m32" CXX="g++ -m32"
ENV CGAL_TEST_PLATFORM="CentOS6-32"
ENV CGAL_CMAKE_FLAGS="(\"-DWITH_CGAL_Qt3:BOOL=OFF\")"
