FROM ubuntu:latest

ARG mediainforepo="https://mediaarea.net/repo/deb/repo-mediaarea_1.0-21_all.deb"
ARG dovitoollink="https://github.com/quietvoid/dovi_tool/releases/download/2.1.0/dovi_tool-2.1.0-x86_64-unknown-linux-musl.tar.gz"
ARG hdr10plustoollink="https://github.com/quietvoid/hdr10plus_tool/releases/download/1.6.0/hdr10plus_tool-1.6.0-x86_64-unknown-linux-musl.tar.gz"
ARG mp4boxlink="https://github.com/gpac/gpac.git"
ARG mp4boxtag="v2.2.1"

COPY /dvmkv2mp4 /usr/local/bin

RUN chmod a+x /usr/local/bin/dvmkv2mp4

# Install MEDIAINFO MKVTOOLNIX FFMPEG WGET
RUN \
  printf "\n---Install mediainfo mkvtoolnix ffmpeg and wget---\n\n" && \
  apt-get update && \
  apt-get dist-upgrade -y && \
  apt-get install -y software-properties-common wget && \
  add-apt-repository ppa:savoury1/ffmpeg4 && \
  wget -O /usr/share/keyrings/gpg-pub-moritzbunkus.gpg https://mkvtoolnix.download/gpg-pub-moritzbunkus.gpg && \
  echo "deb [signed-by=/usr/share/keyrings/gpg-pub-moritzbunkus.gpg] https://mkvtoolnix.download/ubuntu/ jammy main" | tee -a /etc/apt/sources.list && \
  wget ${mediainforepo} -O temp.deb && \
  dpkg -i temp.deb && \
  rm temp.deb && \
  apt-get update && \
  apt-get install -y \
    ffmpeg \
    mediainfo \
    mkvtoolnix \
    jq \
    bc

# DOVI_TOOL
RUN \
  printf "\n---Download dovi_tool---\n\n" && \
  wget -O - ${dovitoollink} | \
  tar -zx -C /usr/local/bin/

# HDR10PLUS_TOOL
RUN \
  printf "\n---Download hdr10plus_tool---\n\n" && \
  wget -O - ${hdr10plustoollink} | \
  tar -zx -C /usr/local/bin/

# MP4BOX
RUN \
  printf "\n---Download build utils---\n\n" && \
  apt-get install -y build-essential pkg-config git && \
  apt-get install -y zlib1g-dev && \
  printf "\n---Build mp4box---\n\n" && \
  mkdir mp4box && \
  cd mp4box && \
  git clone --depth 1 --branch ${mp4boxtag} ${mp4boxlink} gpac_public && \
  cd gpac_public && \
  ./configure --static-bin && \
  make -j $(nproc) && \
  make install && \
  MP4Box -version && \
  cd ../.. && \
  rm -Rf mp4box


RUN \
  printf "\n---Create \"convert\" directory for volume attachment---\n\n" && \
  mkdir /convert

VOLUME ["/convert"]

RUN \
  printf "\n---Create \".gpac\" directory for volume attachment---\n\n" && \
  mkdir /.gpac

VOLUME ["./gpac"]

RUN \
  printf "\n---Create \"cache\" directory for volume attachment---\n\n" && \
  mkdir /cache

VOLUME ["cache"]


WORKDIR /convert

ENTRYPOINT ["dvmkv2mp4"]
