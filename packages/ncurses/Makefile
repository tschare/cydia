include ./CONFIG
DEPENDS=
PACKAGE_DIR?=${PWD}
PKG=ncurses-6.0

all: root
	sudo rm -rf control data
	${MAKE} clean
	mkdir -p data
	cp -rf root/* data
	${MAKE} control
	${MAKE} deb

thin: root.tar.gz
	rm -rf root
	tar xzf root.tar.gz
	mkdir -p root/usr/bin
	mv root/usr/lib/gettext/* root/usr/bin
	rm -rf root/usr/share
	rm -rf root/usr/bin
	rm -rf root/usr/lib/pkgconfig
	rm -rf root/usr/lib/gettext
	rm -rf root/usr/include
	rm -rf root/usr/lib/*.a
	rm -rf root/usr/lib/gio
	rm -rf root/usr/lib/glib-2.0
	$(MAKE) all PACKAGE=$(PACKAGE)_thin

root.tar.gz: root
	tar czvf root.tar.gz root

root: $(PKG)
	rm -rf root
	mkdir -p root
	cd $(PKG) && \
		export CC="xcrun --sdk iphoneos gcc -arch armv7" ; \
		export CXX="xcrun --sdk iphoneos g++ -arch armv7" ; \
		./configure --prefix=/usr --host=arm-apple-darwin && \
		$(MAKE) -j12 && \
		$(MAKE) install DESTDIR=$(shell pwd)/root

$(PKG):
	wget -c https://ftp.gnu.org/gnu/ncurses/ncurses-6.0.tar.gz
	tar xzvf $(PKG).tar.gz

include ../deb_hand.mak
