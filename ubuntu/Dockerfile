FROM ubuntu:16.04 as builder

RUN apt-get update -y && \
    apt-get upgrade -y && \
    apt-get install -y build-essential \
    libtool autotools-dev autoconf \
    libssl-dev \
    libboost-all-dev \
    git \
    pkg-config

RUN git clone https://github.com/BitzenyCoreDevelopers/bitzeny.git

# install libdb
RUN apt-get install -y software-properties-common && \
    add-apt-repository ppa:bitcoin/bitcoin && \
    apt-get update -y && \
    apt-get install -y \
    libdb4.8-dev \
    libdb4.8++-dev

WORKDIR bitzeny

RUN ./autogen.sh && \
    ./configure --without-miniupnpc --without-gui --disable-tests && \
    make && \
    make install

FROM ubuntu:16.04

COPY --from=builder /bitzeny/src/bitzenyd bitzenyd

RUN apt-get update -y && \
    apt-get upgrade -y && \
    apt-get install -y libssl-dev \
    libboost-all-dev

# install libdb
RUN apt-get install -y software-properties-common && \
    add-apt-repository ppa:bitcoin/bitcoin && \
    apt-get update -y && \
    apt-get install -y \
    libdb4.8-dev \
    libdb4.8++-dev

# clean up
RUN apt-get autoremove -y software-properties-common && \
    apt-get clean

VOLUME /root/.bitzeny

EXPOSE 9252 9253

ENTRYPOINT [ "./bitzenyd" ]