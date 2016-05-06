FROM debian:stretch
MAINTAINER debian@olbat.net

RUN mkdir -p /src/crystal/debian
COPY debian/control /src/crystal/debian/
WORKDIR /src/crystal

# Install crystal from upstream APT repository and build deps listed in the
# debian/control file
RUN \
  apt-key adv --keyserver keys.gnupg.net --recv-keys 09617FD37CC06B54 && \
  echo "deb http://dist.crystal-lang.org/apt crystal main" \
    > /etc/apt/sources.list.d/crystal.list && \
  apt-get update && \
  apt-get install -y build-essential debhelper devscripts && \
  mk-build-deps -i -t "apt-get --no-install-recommends -y" debian/control && \
  apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

VOLUME /src/crystal
COPY . /src/crystal

# Move generated files to the working directory (that can be a volume)
CMD sh -c "gbp buildpackage -uc -us --git-ignore-new && mv ../*.* ."
