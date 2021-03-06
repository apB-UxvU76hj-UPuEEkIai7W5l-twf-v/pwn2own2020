# Copy flags from WebKit
SBX_INCLUDES := -Ithreadexec/include \
									-IWebKit/Source/WebCore/platform/network/cf \
									-IWebKit/Source/WebCore/platform/network \
									-IWebKit/Source/WebKit/Shared/API \
									-IWebKit/WebKitBuild/Release/include \
									-IWebKit/WebKitBuild/Release/usr/local/include \
									-IWebKit/WebKitBuild/WTF.build/Release/WTF.build/DerivedSources \

SBX_WEBKIT_FLAGS := -fmessage-length=0 -fdiagnostics-show-note-include-stack -fmacro-backtrace-limit=0 -std=gnu++1z -stdlib=libc++ -Wno-trigraphs -fno-exceptions -fno-rtti -fno-sanitize=vptr -fpascal-strings -O3 -fno-common -Wno-missing-field-initializers -Wunreachable-code -Wnon-virtual-dtor -Wno-overloaded-virtual -Wno-exit-time-destructors -Wno-missing-braces -Wparentheses -Wswitch -Wunused-function -Wno-unused-label -Wno-unused-parameter -Wunused-value -Wempty-body -Wuninitialized -Wno-unknown-pragmas -Wno-shadow -Wno-four-char-constants -Wno-conversion -Wconstant-conversion -Wint-conversion -Wbool-conversion -Wenum-conversion -Wno-float-conversion -Wnon-literal-null-conversion -Wobjc-literal-conversion -Wsign-compare -Wno-shorten-64-to-32 -Wnewline-eof -Wno-c++11-extensions -DNDEBUG -DU_DISABLE_RENAMING=1 -DU_SHOW_CPLUSPLUS_API=0 -isysroot /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.15.sdk -fasm-blocks -fstrict-aliasing -Wdeprecated-declarations -Winvalid-offsetof -g -fvisibility=hidden -fvisibility-inlines-hidden -fno-threadsafe-statics -Wno-sign-conversion -Winfinite-recursion -Wmove -Wcomma -Wblock-capture-autoreleasing -Wstrict-prototypes -Wrange-loop-analysis -Wno-semicolon-before-method-body -isystem /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.15.sdk/usr/local/include

SBX_CXXFLAGS = $(SBX_INCLUDES) $(SBX_WEBKIT_FLAGS) \
							 -DCURRENT_DIR=\"$(CURDIR)\" \

SBX_LDFLAGS = -FWebKit/WebKitBuild/Release \
							 -target x86_64-apple-macos10.15 \
							 -framework JavaScriptCore \
							 -framework CoreFoundation \
							 -framework Foundation \
							 -Lthreadexec/lib \
							 -lthreadexec

all: cvm_side sbx

cvm_side: cvm_side.cc
	make -C root
	./embed.py root/app > bundle.hh
	$(CXX) -o $@ $<

sbx: safari.mm cvm.cc
	./build-threadexec.sh
	./build-webkit.sh
	$(CXX) $(SBX_CXXFLAGS) -o $@ $^ $(SBX_LDFLAGS)

clean:
	make clean -C root
	rm -f cvm_side sbx

.PHONY: all clean
