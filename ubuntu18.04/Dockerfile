FROM ubuntu:18.04

RUN apt update && DEBIAN_FRONTEND=noninteractive apt install -y build-essential git libpcre3 libpcre3-dev libssl-dev libtool autoconf libxml2-dev libcurl4-openssl-dev apache2-dev libpcre3-dev libxml2-dev zlib1g-dev libssl-dev libgd-dev libgeoip-dev libxslt1-dev libxml2-dev libhiredis-dev libluajit-5.1-dev libmaxminddb-dev libmhash-dev libpam0g-dev libperl-dev quilt dwz debhelper=11.1.6ubuntu1 dh-systemd software-properties-common libyajl-dev

RUN curl https://nginx.org/keys/nginx_signing.key | apt-key add && echo "deb https://nginx.org/packages/ubuntu/ bionic nginx" > /etc/apt/sources.list.d/nginx.list && echo "deb-src https://nginx.org/packages/ubuntu/ bionic nginx" >> /etc/apt/sources.list.d/nginx.list

RUN apt update && apt-cache madison nginx

RUN mkdir -p /opt/nginx/build

WORKDIR /opt/nginx/build

ARG PACKAGE_VERSION="1.0.1"
ARG NGINX_VERSION="1.18.0"
ARG PACKAGE_NAME="libnginx-mod-http-modsecurity"

#RUN mkdir -p /opt/nginx/build/${PACKAGE_NAME}-${PACKAGE_VERSION}

RUN git clone -b v1.0.1 https://github.com/SpiderLabs/ModSecurity-nginx.git ${PACKAGE_NAME}-${PACKAGE_VERSION} && rm -rf ${PACKAGE_NAME}-${PACKAGE_VERSION}/.git

RUN DEBIAN_FRONTEND=noninteractive apt-get source nginx

COPY *.deb /opt/

RUN apt install -y libcurl4-openssl-dev libfuzzy2

RUN dpkg -i /opt/*.deb

RUN mv nginx-${NGINX_VERSION} ${PACKAGE_NAME}-${PACKAGE_VERSION}/nginx

RUN tar -c ${PACKAGE_NAME}-${PACKAGE_VERSION} | xz -zT 0 - > ${PACKAGE_NAME}_${PACKAGE_VERSION}.orig.tar.xz

COPY debian /opt/nginx/build/debian

RUN cp -dpR debian ${PACKAGE_NAME}-${PACKAGE_VERSION}/debian

RUN cd ${PACKAGE_NAME}-${PACKAGE_VERSION} && dpkg-buildpackage -S -us -uc -jauto

RUN cd ${PACKAGE_NAME}-${PACKAGE_VERSION} && dpkg-buildpackage -b -us -uc -jauto
