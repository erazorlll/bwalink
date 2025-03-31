ARG BUILD_FROM
FROM $BUILD_FROM

RUN apk update
RUN apk --no-cache add bash curl curl-dev ruby-dev build-base
RUN apk --no-cache add ruby ruby-io-console ruby-irb \
  ruby-json ruby-etc ruby-bigdecimal ruby-rdoc \
  libffi-dev zlib-dev ruby-bundler
RUN bundle config set deployment 'true'

## DON'T install public version as in original code
#RUN gem install balboa_worldwide_app

## CUSTOM CODE to build second balboa app with device name bwa2
RUN mkdir /usr/bin/balboagem
RUN cd /usr/bin/balboagem/
RUN curl -k -L -o balboa_source.tar.gz https://github.com/erazorlll/balboa_worldwide_app/archive/refs/tags/v0.1.tar.gz
RUN tar -xzvf balboa_source.tar.gz
WORKDIR "/balboa_worldwide_app-0.1"
RUN ls -ls
RUN apk update && apk add --no-cache build-base && \
    bundle config set deployment 'true' && \
    apk add --no-cache socat tzdata
RUN gem build balboa_worldwide_app.gemspec
RUN gem install ./balboa_worldwide_app
## CUSTOM CODE END

RUN apk del build-base
RUN rm -rf /var/cache/apk/*

ADD docker-entrypoint.sh /
RUN chmod +x /docker-entrypoint.sh

CMD [ "/docker-entrypoint.sh" ]
