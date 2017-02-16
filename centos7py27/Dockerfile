FROM centos:7

RUN yum groupinstall -y 'development tools'
RUN yum -y install ruby-devel gcc curl libyaml-devel rpm-build which
RUN gpg2 --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
RUN curl -L get.rvm.io | bash -s stable
RUN bash -c "source /etc/profile.d/rvm.sh;rvm requirements"
RUN bash -c "source /etc/profile.d/rvm.sh;rvm install 1.9.3"
RUN bash -c "source /etc/profile.d/rvm.sh;rvm use 1.9.3 --default"
RUN bash -c "source /etc/profile.d/rvm.sh;rvm rubygems current"
RUN bash -c "source /etc/profile.d/rvm.sh;gem install --no-ri --no-rdoc fpm"
RUN yum -y install openssl-devel readline-devel\
    bzip2-devel sqlite-devel zlib-devel ncurses-devel\
    db4-devel expat-devel gdbm-devel
ENV BUILD_VER=2.7.13
ENV DLURL=http://python.org/ftp/python/${BUILD_VER}/Python-${BUILD_VER}.tgz
RUN mkdir -p /build; cd /build\
    && curl -L ${DLURL} | tar xz
RUN cd /build/Python-${BUILD_VER}\
    && ./configure --prefix=/usr/local\
        --enable-unicode=ucs4 --enable-ipv6 \
    && make -j2 && make install DESTDIR=/tmp/installdir
RUN echo 'echo /usr/local/lib > /etc/ld.so.conf.d/usr-local-lib.conf' > /tmp/installdir/run-ldconfig.sh
RUN echo '/sbin/ldconfig' >> /tmp/installdir/run-ldconfig.sh
RUN bash -c "source /etc/profile.d/rvm.sh;\
    fpm -s dir -t rpm -n python27 -v ${BUILD_VER} -C /tmp/installdir \
    --after-install /tmp/installdir/run-ldconfig.sh \
    -d 'openssl' \
    -d 'bzip2' \
    -d 'zlib' \
    -d 'expat' \
    -d 'sqlite' \
    -d 'ncurses' \
    -d 'readline' \
    -d 'gdbm' \
    --directories=/usr/local/lib/python2.7/ \
    --directories=/usr/local/include/python2.7/ \
    usr/local"
