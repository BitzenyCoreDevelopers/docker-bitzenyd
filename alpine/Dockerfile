FROM alpine:edge

ENV gpp_version 6.4.0-r5
ENV depend_packages "git autoconf automake libtool g++=${gpp_version} make boost-dev libressl-dev db-dev miniupnpc-dev protobuf-dev libqrencode-dev libevent-dev miniupnpc-dev"

RUN apk --no-cache --update add ${depend_packages}
RUN git clone -b fix-boost --depth 1 https://github.com/onokatio/bitzeny
RUN cd bitzeny && \
  ./autogen.sh && \
	sed -e 's/wait $am_sleep/:/' configure > configure.new && \
	chmod +x configure.new && \
  ./configure.new --prefix=/usr --without-gui --disable-tests --with-incompatible-bdb --enable-hardening --without-miniupnpc
RUN cd bitzeny && \
  make -j4 && make install

VOLUME /root/.bitzeny
EXPOSE 9252 9253

#ENTRYPOINT [ "/usr/bin/bitzenyd" ]
CMD sh
