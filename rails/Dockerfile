FROM ruby:3.1.0

ENV LANG C.UTF-8 \
    HOME="/app" \
    TZ=Asia/Tokyo

RUN apt-get update -qq && \
    apt-get install -y build-essential libpq-dev nodejs
RUN curl -fsSL https://deb.nodesource.com/setup_17.x | bash - && \
    apt-get install -y nodejs vim sudo

WORKDIR /tmp
RUN wget https://github.com/libvips/libvips/releases/download/v8.12.1/vips-8.12.1.tar.gz && \
    tar zxf vips-8.12.1.tar.gz
WORKDIR /tmp/vips-8.12.1
RUN ./configure && \
    make && \
    make install && \
    ldconfig

WORKDIR /../../app
ADD Gemfile ${HOME}/Gemfile
ADD Gemfile.lock ${HOME}/Gemfile.lock

RUN bundle install -j4 && \
    rm -rf /usr/local/bundle/cache/* \
    /usr/local/share/.cache/* \
    /var/cache/* \
    /tmp/* \
    /usr/lib/mysqld* \
    /usr/bin/mysql*

ADD . ${HOME}

EXPOSE 3000