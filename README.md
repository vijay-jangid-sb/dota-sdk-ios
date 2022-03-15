# iOS SDK Installation Instructions
Learn how to install the Devnagri SDK for iOS.

# Introduction
Devnagri Over the Air for iOS lets you update translations in your iOS app without having to release it every single time.

By including our SDK, your app will check for updated translations in Devnagri regularly and download them in the background.

You can install the SDK manually.

# Manual Installation
Download the latest release from https://github.com/DevnagriAI/dota-sdk-ios/releases 
Add DevnagriSDK.framework in Xcode as linked binary to your target.
The Apple store will reject your app if it includes simulator binaries. Therefore, a script to strip the extra binaries needs to be run before you upload the app. To do this, go to Build Phases and add a Run Script section by clicking the + symbol. Copy and paste the following script:

      FRAMEWORK="DevnagriSDK"
      FRAMEWORK_EXECUTABLE_PATH="${BUILT_PRODUCTS_DIR}/${FRAMEWORKS_FOLDER_PATH}/$FRAMEWORK.framework/$FRAMEWORK"
      EXTRACTED_ARCHS=()
      for ARCH in $ARCHS
      do
         lipo -extract "$ARCH" "$FRAMEWORK_EXECUTABLE_PATH" -o "$FRAMEWORK_EXECUTABLE_PATH-$ARCH"
         EXTRACTED_ARCHS+=("$FRAMEWORK_EXECUTABLE_PATH-$ARCH")
      done
      lipo -o "$FRAMEWORK_EXECUTABLE_PATH-merged" -create "${EXTRACTED_ARCHS[@]}"
      rm "${EXTRACTED_ARCHS[@]}"
      rm "$FRAMEWORK_EXECUTABLE_PATH"
      mv "$FRAMEWORK_EXECUTABLE_PATH-merged" "$FRAMEWORK_EXECUTABLE_PATH"

# Configuration

  import DevnagriSDK

Initialize the SDK by calling the following code in your didFinishLaunchingWithOptions Method and you can get API_KEY from devnagri

     func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
         DevnagriSDK.shared.init(
           api_key: <API_KEY>
         )
     }
