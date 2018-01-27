FROM ubuntu AS build

ENV DEBIAN_FRONTEND=noninteractive
ARG REF=master

RUN apt-get update && \
  apt-get upgrade -y && \
  apt-get install -y build-essential \
    libtool autotools-dev autoconf \
    libssl-dev \
    libboost-all-dev \
    libevent-dev \
    pkg-config \
    software-properties-common \
    git && \
  add-apt-repository -y ppa:bitcoin/bitcoin && \
  apt-get update && \
  apt-get install -y libdb4.8-dev libdb4.8++-dev && \
  git clone https://github.com/BitzenyCoreDevelopers/bitzeny.git /bitzeny && \
  cd /bitzeny && \
  git checkout "$REF" && \
  ./autogen.sh && \
  ./configure --prefix=/usr --without-miniupnpc --without-gui --disable-tests && \
  make

FROM ubuntu

RUN apt-get update && \
  apt-get upgrade -y && \
  apt-get install -y \
    libssl-dev \
    libboost-all-dev \
    libevent-dev \
    software-properties-common && \
  add-apt-repository -y ppa:bitcoin/bitcoin && \
  apt-get update && \
  apt-get install -y libdb4.8-dev libdb4.8++-dev && \
  apt-get autoremove -y software-properties-common && \
  apt-get clean

COPY --from=build /bitzeny/src/bitzenyd /usr/bin/bitzenyd

VOLUME /root/.bitzeny
EXPOSE 9252 9253

ENTRYPOINT [ "/usr/bin/bitzenyd" ]
