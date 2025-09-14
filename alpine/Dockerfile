FROM alpine:3.19 AS build

WORKDIR /usr/src

COPY init.sh /usr/src/
RUN chmod +x init.sh

RUN apk update && apk add --no-cache \
    git \
    build-base \
    make \
    automake \
    autoconf \
    libtool \
    pkgconf \
    openssl \
    curl \
    libusb \
    libzip \
    readline \
    openssl-dev \
    curl-dev \
    libusb-dev \
    libzip-dev \
    python3-dev \
    readline-dev \
    libxml2-dev

RUN git clone --depth 1 https://github.com/libimobiledevice/libplist.git && \
    cd libplist && ./autogen.sh && make && make install

RUN git clone --depth 1 https://github.com/libimobiledevice/libtatsu.git && \
    cd libtatsu && ./autogen.sh && make && make install

RUN git clone --depth 1 https://github.com/libimobiledevice/libimobiledevice-glue.git && \
    cd libimobiledevice-glue && ./autogen.sh && make && make install

RUN git clone --depth 1 https://github.com/libimobiledevice/libusbmuxd.git && \
    cd libusbmuxd && ./autogen.sh && make && make install

RUN git clone --depth 1 https://github.com/libimobiledevice/libimobiledevice.git && \
    cd libimobiledevice && ./autogen.sh && make && make install

RUN git clone https://github.com/libimobiledevice/usbmuxd.git && \
    cd usbmuxd && ./autogen.sh && make && make install

RUN git clone --depth 1 https://github.com/libimobiledevice/libirecovery.git && \
    cd libirecovery && ./autogen.sh && make && make install

RUN git clone --depth 1 https://github.com/libimobiledevice/idevicerestore.git && \
    cd idevicerestore && ./autogen.sh && make && make install

RUN git clone --depth 1 https://github.com/libimobiledevice/libideviceactivation.git && \
    cd libideviceactivation && ./autogen.sh && make && make install

FROM alpine:3.19

COPY --from=build /usr/src/init.sh /init.sh
COPY --from=build /usr/local/ /usr/local/

RUN apk update && apk add --no-cache \
    bash \
    openssl \
    libcurl \
    libusb \
    libzip \
    libxml2 \
    readline

ENTRYPOINT ["/bin/bash", "/init.sh"]
CMD ["/bin/bash"]
