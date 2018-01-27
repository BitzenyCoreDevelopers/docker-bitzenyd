FROM alpine AS builder

ENV gpp_version 6.4.0-r5
ENV build_packages "git curl autoconf automake libtool make g++"
ENV library_packages "boost-dev libressl-dev protobuf-dev libqrencode-dev libevent-dev"
ENV JOBS 4

ARG REF=master

RUN apk --no-cache --update add --virtual .build ${build_packages} && \
    apk --no-cache --update add ${library_packages} && \
    curl -L http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz -o db-4.8.30.NC.tar.gz && \
    tar zxvf db-4.8.30.NC.tar.gz && \
    cd db-4.8.30.NC && \
    cd build_unix && \
    ../dist/configure --disable-shared --enable-cxx --disable-replication --with-pic --prefix=/usr/local/ && \
    make libdb_cxx-4.8.a libdb-4.8.a && \
    make install_lib install_include && \
    cd ../.. && \
    rm -rf db-4.8.30.NC && \
    git clone https://github.com/BitzenyCoreDevelopers/bitzeny.git bitzeny && \
    cd bitzeny && \
    git checkout "$REF" && \
    ./autogen.sh && \
    sed -e 's/wait $am_sleep/:/' configure > configure.new && \
    chmod +x configure.new && \
    ./configure.new LDFLAGS=-L/usr/local/lib/ CPPFLAGS=-I/usr/local/include/ --prefix=/usr --without-gui --disable-tests --with-incompatible-bdb --enable-hardening --without-miniupnpc && \
    make --jobs="${JOBS}"

FROM alpine

RUN apk --no-cache --update add ${library_packages}
COPY --from=build /bitzeny/src/bitzenyd /usr/bin/bitzenyd

VOLUME /root/.bitzeny
EXPOSE 9252 9253

ENTRYPOINT [ "/usr/bin/bitzenyd" ]
