compile:
	make -C ../mpy-cross -j8
	make -C ../ports/unix deplibs MICROPY_STANDALONE=1 CC="xcrun --sdk iphoneos clang -arch armv7 -mios-version-min=7.0" CROSS_COMPILE=armv7-apple-darwin STRIP=strip SIZE=size
	make -C ../ports/unix -j8 MICROPY_STANDALONE=1 CC="xcrun --sdk iphoneos clang -arch armv7 -mios-version-min=7.0" CROSS_COMPILE=armv7-apple-darwin STRIP=strip SIZE=size
	cp ../ports/unix/micropython mpython-32
	make -C ../ports/unix clean
	make -C ../ports/unix deplibs MICROPY_STANDALONE=1 CC="xcrun --sdk iphoneos clang -arch arm64 -mios-version-min=7.0" CROSS_COMPILE=aarch64-apple-darwin STRIP=strip SIZE=size
	make -C ../ports/unix -j8 MICROPY_STANDALONE=1 CC="xcrun --sdk iphoneos clang -arch arm64 -mios-version-min=7.0" CROSS_COMPILE=aarch64-apple-darwin STRIP=strip SIZE=size
	cp ../ports/unix/micropython mpython-64
	mkdir -p layout/usr/bin
	lipo -create -output layout/usr/bin/mpython mpython-32 mpython-64
	rm -f mpython-*

package:
	ldid -Sent.plist layout/usr/bin/mpython
	mkdir -p layout/DEBIAN
	sed "s/INSTSIZE/$$(du -s -k layout/usr/bin | cut -f1)/" control > layout/DEBIAN/control
	$(THEOS)/bin/dm.pl -bZ lzma layout tmp.deb
	dpkg-name tmp.deb

clean:
	make -C ../mpy-cross clean
	make -C ../ports/unix clean
	rm -rf layout *.deb mpython-*

.PHONY: compile clean package
