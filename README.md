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
      DevnagriSdk.shared().initSdk(apiKey:String)
     }
     
# Change Language
In case you don't want to use the system language, you can set a different language in the updateAppLocale method. The language code (locale) needs to be present in a release from Devnagri.

      DavnagriSdk.shared().updateAppLocale(code: "en")
      
# Get Current language code
In case you want to know which language code currently used by application.

     let currentLanguageCode = DevnagriSdk.shared().getCurrentApplicationLanguageCode()

# Get Supported Languages
You can get supported languages for the SDK using this method. This will return hashmap of language name and language code

      DevnagriSdk.shared().getAllSupportableLanguage{ arrOfLanguage in
            //do here…
      } 
 
# Translate String, List and Dictionary on runtime
You can use these methods anywhere in your project and these will provide translation for current active locale in callback method.

Get Translation of a Specific String

      DevnagriSdk.shared().getTranslationOfString(sentance : ”Hello ”){ translatedString in
       //Do here...
      }

Get Translations of an Array of Strings.

      let arrayOfStrings:[String] = ["SampleText1","SampleText2","SampleText3"];
      DevnagriSdk.shared().getTranslationsOfStrings(sentances : arrayOfStrings){translatedStrings
      //Do here...
      }

Get Translations Of Dictionary

       let dicOfStrings:[String:String] = ["A":"SampleText1", "B":"SampleText2", "C":"SampleText3"];
       DevnagriSdk.shared().getTranslationOfDictionary(dictionary:[“Key1”:”Value1”, “Key      2”:”Value 2”]){translatedDic in
        //Do here....
        }

# Usage

Select project target and click general select in framework,libraries,and Embedded Content and select *DevnagriSdk.framwork* if not selected 
than choose Embed -> Embed & Sign.

      
     
