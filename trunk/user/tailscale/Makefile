include $(ROOTDIR)/coustom
THISDIR = $(shell pwd)
#export GO111MODULE=on
#export GOPROXY=https://goproxy.bj.bcebos.com/
PKG_NAME:=tailscale
PKG_VERSION:=1.22.2
PKG_RELEASE:=1
PKG_GOGET:=github.com/tailscale/tailscale

PKG_SOURCE:=$(GOPKG_SOURCE)
PKG_COMMIT:=6f700925cef22d8b2a100840c8d9eb084dadfece

all:download build

download:
	git clone https://$(PKG_GOGET)
	cd $(PKG_NAME) && git checkout $(PKG_COMMIT)

build:
	( if [ $(GOPROXY_ON) = "y" ]; then \
		go env -w GOPROXY=https://goproxy.cn,direct ; \
		fi ; \
		cd $(PKG_NAME) && \
		CGO_ENABLED=0 GO111MODULE=on GOOS=linux GOARCH=mipsle go build -ldflags "-w -s" -x -v tailscale.com/cmd/tailscale; \
		CGO_ENABLED=0 GO111MODULE=on GOOS=linux GOARCH=mipsle go build -ldflags "-w -s" -x -v tailscale.com/cmd/tailscaled; \
		)

clean:
	rm -rf $(THISDIR)/$(PKG_NAME)

romfs:
	$(ROMFSINST) -p +x $(THISDIR)/$(PKG_NAME)/tailscale /usr/bin/tailscale
	$(ROMFSINST) -p +x $(THISDIR)/$(PKG_NAME)/tailscaled /usr/bin/tailscaled
