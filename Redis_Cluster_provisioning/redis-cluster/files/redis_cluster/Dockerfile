FROM redis:3.2

MAINTAINER Daniel Fiott

# Dependencies
RUN apt-get update -qq && \
    apt-get install --no-install-recommends -yqq \
      net-tools supervisor ruby rubygems locales gettext-base wget && \
    apt-get clean -yqq

# ENV setup
RUN locale-gen en_US.UTF-8
ENV LANG       en_US.UTF-8
ENV LC_ALL     en_US.UTF-8
ENV HOME /root
ENV DEBIAN_FRONTEND noninteractive

RUN gem install redis -v 3.3.3

RUN apt-get install -y gcc make g++ build-essential libc6-dev tcl git supervisor ruby

RUN wget -qO redis.tar.gz http://download.redis.io/releases/redis-3.2.9.tar.gz \
    && tar xfz redis.tar.gz -C / \
    && mv /redis-3.2.9 /redis

RUN (cd /redis && make)

# redis Config folders
RUN mkdir /redis-conf
RUN mkdir /redis-data

# startup script
COPY ./startup-script.sh /startup-script.sh
RUN chmod 755 /startup-script.sh

EXPOSE 7000 7001 7002

ENTRYPOINT ["/startup-script.sh"]
CMD ["redis-cluster"]
