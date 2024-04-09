FROM ubuntu:jammy

# you can also pass a file on the local fs here
ARG SDK_SOURCE=https://github.com/shekhar-infinity/realtek-mips-sdks/raw/master/rtl819x/rsdk-4.4.7-4181-EB-2.6.30-0.9.30-m32u-140129.tar.bz2
# just make sure it matches the checksum, otherwise you're on your own
ARG CHECKSUM=63af001fcbebbf32e6235a66efb117d8abd13e088c8161abece7d578b55b56d2

ADD ${SDK_SOURCE} /tmp/sdk.tar.bz2

RUN bash -c 'cat /tmp/sdk.tar.bz2 | sha256sum -c <(echo "${CHECKSUM}  -")'

# shotgun approach, the sdk is x86 so it needs some i386 libs.
# bzip2 is just for decompressing the tar
RUN dpkg --add-architecture i386 && \
    apt update && \
    DEBIAN_FRONTEND=noninteractive apt install -y gcc-multilib bzip2 zlib1g:i386

RUN tar -C /tmp/ -xjf /tmp/sdk.tar.bz2 && \
    rm /tmp/sdk.tar.bz2 && \
    (cd /tmp/ && echo * > /sdk_version && mv ./*/ /sdk)

WORKDIR /sdk
