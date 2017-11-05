FROM alpine:edge

ENV gpp_version 6.4.0-r5
ENV build_packages "git autoconf automake libtool g++=${gpp_version} make"
ENV library_packages "boost-dev libressl-dev db-dev protobuf-dev libqrencode-dev libevent-dev"
ENV JOBS 4

RUN apk --no-cache --update add --virtual .build ${build_packages} && \
	apk --no-cache --update add ${library_packages} && \
	git clone -b z1.2.x --depth 1 https://github.com/onokatio/bitzeny && \
	cd bitzeny && \
  ./autogen.sh && \
	sed -e 's/wait $am_sleep/:/' configure > configure.new && \
	chmod +x configure.new && \
  ./configure.new --prefix=/usr --without-gui --disable-tests --with-incompatible-bdb --enable-hardening --without-miniupnpc && \
  make --jobs="${JOBS}" && \
	make install && \
	apk del --purge .build && \
	cd / && \
	rm -rf bitzeny

VOLUME /root/.bitzeny
EXPOSE 9252 9253

ENTRYPOINT [ "/usr/bin/bitzenyd" ]
