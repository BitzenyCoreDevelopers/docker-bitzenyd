FROM alpine:3.7 as builder

RUN apk add --no-cache \
    git \
    curl \
    autoconf \
    automake \
    libtool \
    make \
    g++ \
    boost-dev \
    libressl-dev

RUN wget 'http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz' && \
    tar -xzvf db-4.8.30.NC.tar.gz && \
    cd db-4.8.30.NC/build_unix/ && \
    ../dist/configure --disable-shared --enable-cxx --disable-replication --with-pic --prefix=/usr/local/ && \
    make install

RUN git clone https://github.com/BitzenyCoreDevelopers/bitzeny.git

WORKDIR bitzeny

RUN ./autogen.sh && \
    ./configure LDFLAGS=-L/usr/local/lib/ CPPFLAGS=-I/usr/local/include/ --without-gui --disable-tests --with-incompatible-bdb --enable-hardening --without-miniupnpc && \
    make && \
    make install

FROM alpine:3.7

COPY --from=builder /bitzeny/src/bitzenyd bitzenyd

RUN apk add --no-cache \
    libressl-dev \
    boost-dev

VOLUME /root/.bitzeny
EXPOSE 9252 9253

ENTRYPOINT [ "./bitzenyd" ]
