SRC_NAME = vim-8.2
SRC_URL = http://ftp.vim.org/pub/vim/unix/vim-8.2.tar.bz2
THISDIR = $(shell pwd)

all: download_test extract_test config_test
	$(MAKE) -j$(HOST_NCPU) -C $(SRC_NAME)

download_test:
	( if [ ! -f $(SRC_NAME).tar.bz2 ]; then \
		wget -t5 --timeout=20 --no-check-certificate -O $(SRC_NAME).tar.gz $(SRC_URL); \
	fi )

extract_test:
	( if [ ! -d $(SRC_NAME) ]; then \
		tar xf $(SRC_NAME).tar.bz2; \
	fi )

config_test:
	( if [ -f ./config_done ]; then \
		echo "the same configuration"; \
	else \
		make configure && touch config_done; \
	fi )

configure:
	( cd $(SRC_NAME) ; \
	./configure \
		--prefix=/usr \
		--with-features=normal \
		--disable-gui \
		--disable-gtktest \
		--disable-xim \
		--without-x \
		--disable-netbeans \
		--disable-cscope \
		--disable-gpm \
		--disable-acl \
		--disable-selinux \
		--without-cxx \
		--without-cxx-binding \
		--without-ada \
		--without-debug \
		--without-manpages \
		--without-profile \
		--without-tests \
		--host=$(HOST_TARGET) \
		--build=$(HOST_BUILD) ; \
	)

clean:
	if [ -f $(SRC_NAME)/Makefile ] ; then \
		$(MAKE) -C $(SRC_NAME) distclean ; \
	fi ; \
	rm -f config_done

romfs:
	$(ROMFSINST) -p +x $(THISDIR)/$(SRC_NAME)/vim /usr/bin/vim
