FROM mkaczanowski/archlinux-golang
MAINTAINER Mateusz Kaczanowski, kaczanowski.mateusz@gmail.com

ENV ETCDI_VERSION 2.0.4
RUN pacman --noconfirm -S patch
RUN curl -sSL -k https://github.com/coreos/etcd/archive/v$ETCDI_VERSION.tar.gz \
		| tar --touch -v -C /usr/src -xz

COPY patches/* /usr/src/etcd-$ETCDI_VERSION/
RUN cd /usr/src/etcd-$ETCDI_VERSION && patch etcdserver/server.go < server.go.patch
RUN cd /usr/src/etcd-$ETCDI_VERSION && patch etcdserver/raft.go < raft.go.patch
RUN cd /usr/src/etcd-$ETCDI_VERSION && ./build 2>&1
ENV PATH /usr/src/etcd-$ETCDI_VERSION/bin:$PATH

RUN pacman --noconfirm -R patch; pacman -Scc
EXPOSE 4001 7001 2379 2380
CMD ["/usr/lib/systemd/systemd"]