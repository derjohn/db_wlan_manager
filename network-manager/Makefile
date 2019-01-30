PACKAGE=network-manager-db-wifi
DEPENDENCIES=-d network-manager -d python3 -d python3-bs4 -d python3-dnspython
DESCRIPTION="This adds DB wifi (Deutsche Bahn, German Railway) authentication to network-manager. No more captive portal!"
VERSION=0.1+$(shell git rev-parse --short HEAD)
# VERSION works for me, but to use it in a real apt repo, it must be strictly monotonically increasing

check:
	@if ! fpm --help | grep -q jordansissel; then echo "Please install fpm. See https://github.com/jordansissel/fpm/"; fi

deb: check
	rm *.deb ||:
	rm -rf packagedir
	mkdir -p packagedir/usr/share/db_wlan_manager/network-manager
	cp network-manager-dbwifi packagedir/usr/share/db_wlan_manager/network-manager/
	cp ../*.py packagedir/usr/share/db_wlan_manager
	cp ../LICENSE packagedir/usr/share/db_wlan_manager
	cp ../README.md packagedir/usr/share/db_wlan_manager
	chmod 700 packagedir/usr/share/db_wlan_manager/network-manager/network-manager-dbwifi
	mkdir -p packagedir/etc/NetworkManager/dispatcher.d
	cd packagedir/etc/NetworkManager/dispatcher.d; ln -sf /usr/share/db_wlan_manager/network-manager/network-manager-dbwifi 10-dbwifi
	fpm \
	-s dir \
	-t deb \
	-n $(PACKAGE) $(DEPENDENCIES) \
	-v $(VERSION) \
	--description $(DESCRIPTION) \
	--prefix / \
	--license "GPL" \
	--url "https://github.com/sistason/db_wlan_manager" \
	-C packagedir .
	rm -rf packagedir

showdeb:
	dpkg -c $(PACKAGE)_$(VERSION)_amd64.deb

install:
	@sudo dpkg -i $(PACKAGE)_$(VERSION)_amd64.deb
