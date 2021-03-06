# Simpo SDK for iOS

## Installation instructions:
  - [Carthage](#carthage)
  - [CocoaPods](#cocoapods)

### Carthage

1. Install Carthage  
```bash
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
$ brew install carthage
```
2. Create `Cartfile` in the project root dir with the content:  
```bash
github "simpoio/ios-sdk"
```

3. Install the dependency:  
```bash
carthage update --platform iOS
```
4. Add `Simpo.framework` (located in `<project-root>/Carthage/Build/iOS`) to the `Linked Frameworks and Libraries`:  
![picture](http://uploads.simpo.io.s3.amazonaws.com/mobile/carthage1.png)
5. Add a `Run Script` build phase with this content: 
  - shell script (if not exist): /usr/local/bin/carthage copy-frameworks
  - input Files: $(SRCROOT)/Carthage/Build/iOS/Simpo.framework
  - output Files: $(BUILT_PRODUCTS_DIR)/$(FRAMEWORKS_FOLDER_PATH)/Simpo.framework
  
![picture](http://uploads.simpo.io.s3.amazonaws.com/mobile/carthage2.png)
6. Optional if CocoaPods script `[CP] Embed Pods Frameworks` exists.  
Solves the AppStore submission issue.  
Add a `Run Script` build phase with the name `Carthage Strip Architectures` and this content:
```bash
FRAMEWORK="Simpo"
FRAMEWORK_EXECUTABLE_PATH="${BUILT_PRODUCTS_DIR}/${FRAMEWORKS_FOLDER_PATH}/$FRAMEWORK.framework/$FRAMEWORK"
EXTRACTED_ARCHS=()

echo "Stripping the Simpo framework"

for ARCH in $ARCHS
do
lipo -extract "$ARCH" "$FRAMEWORK_EXECUTABLE_PATH" -o "$FRAMEWORK_EXECUTABLE_PATH-$ARCH"
EXTRACTED_ARCHS+=("$FRAMEWORK_EXECUTABLE_PATH-$ARCH")
done
lipo -o "$FRAMEWORK_EXECUTABLE_PATH-merged" -create "${EXTRACTED_ARCHS[@]}"
rm "${EXTRACTED_ARCHS[@]}"
rm "$FRAMEWORK_EXECUTABLE_PATH"
mv "$FRAMEWORK_EXECUTABLE_PATH-merged" "$FRAMEWORK_EXECUTABLE_PATH"
```

### CocoaPods

1. Install CocoaPods  
```bash
$ sudo gem install cocoapods
```
2. If needed (first time) run:
```bash
pod init
```
3. Install the dependency by adding `pod 'SimpoSDK'` into the `Podfile`. Then run:
```bash
$ pod update
```

So the `Podfile` looks like this:  
```bash
platform :ios, '10.0'

target '<Project name>' do
  use_frameworks!
  pod 'SimpoSDK'
end
````
