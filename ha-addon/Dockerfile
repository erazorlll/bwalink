ARG BUILD_FROM="alpine"
FROM $BUILD_FROM

RUN apk update
RUN apk --no-cache add bash curl curl-dev ruby-dev build-base
RUN apk --no-cache add ruby ruby-io-console ruby-irb \
  ruby-json ruby-etc ruby-bigdecimal ruby-rdoc \
  libffi-dev zlib-dev ruby-bundler wget 
RUN bundle config set deployment 'true'

## DON'T install public version as in original code
#RUN gem install balboa_worldwide_app

## CUSTOM CODE to build second balboa app with device name bwa2
RUN mkdir /usr/bin/balboagem
RUN cd /usr/bin/balboagem/
#RUN curl -k -L -o balboa_source.tar.gz https://github.com/erazorlll/balboa_worldwide_app/archive/refs/tags/v0.2.tar.gz
RUN wget -O balboa_source.tar.gz https://github.com/erazorlll/balboa_worldwide_app/archive/refs/tags/v0.2.tar.gz
RUN tar -xzvf balboa_source.tar.gz
WORKDIR "/balboa_worldwide_app-0.2"
RUN ls -ls
RUN apk update && apk add --no-cache build-base && \
    bundle config set deployment 'true' && \
    apk add --no-cache socat tzdata
RUN gem install sd_notify
RUN gem install net-telnet-rfc2217
RUN gem install mqtt-homie-homeassistant
RUN gem install mqtt-homeassistant
RUN gem install digest-crc
RUN gem install ccutrer-serialport
RUN gem build balboa_worldwide_app.gemspec
RUN echo ls -ls
RUN gem install --local balboa_worldwide_app-2.3.1.gem
#RUN gem install --local balboa_worldwide_app
## CUSTOM CODE END

RUN apk del build-base
RUN rm -rf /var/cache/apk/*

ADD docker-entrypoint.sh /
RUN chmod +x /docker-entrypoint.sh

CMD [ "/docker-entrypoint.sh" ]
