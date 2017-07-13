FROM debian:jessie

ENV KERNEL_VERSION 4.12
ENV VBOX_VERSION 5.1.22

RUN apt-get update
RUN apt-get install -y curl git gcc make p7zip p7zip-full bzip2

WORKDIR /opt
RUN git clone --single-branch -b v${KERNEL_VERSION}-coreos https://github.com/coreos/linux.git

WORKDIR /opt/linux
RUN apt-get install bc
# RUN curl -L -o .config https://raw.githubusercontent.com/coreos/coreos-overlay/master/sys-kernel/coreos-kernel/files/x86_64_defconfig-${KERNEL_VERSION}
RUN curl -L -o commonconfig https://raw.githubusercontent.com/coreos/coreos-overlay/master/sys-kernel/coreos-modules/files/commonconfig-4.12
RUN curl -L -o amd64_defconfig https://raw.githubusercontent.com/coreos/coreos-overlay/master/sys-kernel/coreos-modules/files/arm64_defconfig-4.12
RUN make oldconfig
RUN make prepare
RUN make scripts

RUN mkdir /opt/vbguest
WORKDIR /opt/vbguest

RUN curl -L -o vboxguest.iso http://download.virtualbox.org/virtualbox/${VBOX_VERSION}/VBoxGuestAdditions_${VBOX_VERSION}.iso
RUN 7z x vboxguest.iso -ir'!VBoxLinuxAdditions.run'
RUN sh VBoxLinuxAdditions.run --noexec --target .
RUN mkdir amd64
RUN tar -C amd64 -xjf VBoxGuestAdditions-amd64.tar.bz2
RUN ls amd64

# RUN src/vboxguest-5.1.22/vboxguest

ADD vbox.patch amd64/src
# RUN cd amd64/src; patch -Np0 < vbox.patch
RUN ls -la amd64/src

RUN KERN_DIR=/opt/linux make -C amd64/src/vboxguest-${VBOX_VERSION}/vboxsf -j1

# CMD sleep 1000
CMD cp /opt/vbguest/amd64/src/vboxguest-${VBOX_VERSION}/vboxsf/vboxsf.ko /opt/vbresult/vboxsf.ko
