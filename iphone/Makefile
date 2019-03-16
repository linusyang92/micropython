compile:
	make -C ../mpy-cross
	make -C ../ports/unix deplibs MICROPY_STANDALONE=1 CC="xcrun --sdk iphoneos clang -arch armv7 -mios-version-min=7.0" CROSS_COMPILE=armv7-apple-darwin STRIP=strip SIZE=size
	make -C ../ports/unix MICROPY_STANDALONE=1 CC="xcrun --sdk iphoneos clang -arch armv7 -mios-version-min=7.0" CROSS_COMPILE=armv7-apple-darwin STRIP=strip SIZE=size

package: compile
	mkdir -p layout/usr/bin
	cp ../ports/unix/micropython layout/usr/bin/mpython
	ldid -Sent.plist layout/usr/bin/mpython
	mkdir -p layout/DEBIAN
	sed "s/INSTSIZE/$$(du -s -k layout/usr/bin | cut -f1)/" control > layout/DEBIAN/control
	$(THEOS)/bin/dm.pl -bZ lzma layout tmp.deb
	dpkg-name tmp.deb

clean:
	make -C ../mpy-cross clean
	make -C ../ports/unix clean
	rm -rf layout *.deb

.PHONY: compile clean package
