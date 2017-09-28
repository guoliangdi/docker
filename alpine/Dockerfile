#
# guoliangdi Alpine Linux image
# from base gliderlabs
#

FROM gliderlabs/alpine:3.4
MAINTAINER guoliangdi Devops Team <github.com/guoliangD>

# Install root filesystem
ADD ./rootfs /

# Install base packages
RUN apk update && apk upgrade && \
    apk-install curl wget bash tree && \
    echo -ne "Alpine Linux 3.4 image. (`uname -rsv`)\n" >> /root/.built

# Define bash as default command
CMD ["/bin/bash"]
