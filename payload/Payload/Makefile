IOS := -arch arm64 -mios-version-min=11.0 -isysroot /Applications/Xcode-beta.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS12.1.sdk

#IOS += -Iheaders -framework CoreFoundation -framework Foundation -framework IOKit \
		#-I/Users/User/rsync/mettle/build/aarch64-iphone-darwin/include \
		#-I/Users/User/rsync/mettle/mettle/src \
		#-L/Users/User/rsync/mettle/build/aarch64-iphone-darwin/lib \
		#-lmettle -lsigar -lev -lz -leio -ldnet -lcurl -lmbedx509 -lmbedtls -lmbedcrypto \
		#-framework CoreVideo -framework CoreImage -framework CoreGraphics -framework CoreMedia -framework AVFoundation -framework UIKit

payload.dylib: payload.o loader.o jitcopy.o voucher_swap.o offsets.o kernelSymbolFinder.o lzssdec.o
	clang ${IOS} $^ -shared -o $@ -bind_at_load \
		-fno-stack-protector -fobjc-arc -fmodules -framework IOKit -lc++
	strip -u -r -s exports.txt payload.dylib

jitcopy.o: jitcopy.s
	as ${IOS} $< -o $@

%.o: %.m common.h
	clang -c ${IOS} $< -o $@ -fno-stack-protector -fobjc-arc -fmodules

%.o: %.c
	clang -c ${IOS} $< -o $@ -fno-stack-protector -fmodules

%.o: %.cpp
	clang -c ${IOS} $< -o $@ -fno-stack-protector -fmodules

.PHONY: clean
clean:
	rm -f *.o *.dylib

