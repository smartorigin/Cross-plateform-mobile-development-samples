# we specify a root target for android to prevent all of the targets from spidering out
./build_ios/libhelloworld.xcodeproj: libhelloworld.gyp ./deps/djinni/support-lib/support_lib.gyp helloworld.djinni
	sh ./run_djinni.sh
	deps/gyp/gyp --depth=. -f xcode -DOS=ios --generator-output ./build_ios -Ideps/djinni/common.gypi ./libhelloworld.gyp
ios: ./build_ios/libhelloworld.xcodeproj
	xcodebuild -project ios/Djinnius.xcodeproj \
           -scheme Djinnius \
           -configuration 'Debug' \
           -sdk iphonesimulator
# we specify a root target for android to prevent all of the targets from spidering out
GypAndroid.mk: libhelloworld.gyp ./deps/djinni/support-lib/support_lib.gyp helloworld.djinni
	sh ./run_djinni.sh
	ANDROID_BUILD_TOP=/Applications/android-ndk-r16/ deps/gyp/gyp --depth=. -f android -DOS=android -Ideps/djinni/common.gypi ./libhelloworld.gyp --root-target=libhelloworld_jni
# this target implicitly depends on GypAndroid.mk since gradle will try to make it
android: GypAndroid.mk
cd android/ && ./gradlew app:assembleDebug
	@echo "Apks produced at:"
	@python deps/djinni/example/glob.py ./ '*.apk'
