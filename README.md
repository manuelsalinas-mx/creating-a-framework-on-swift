# CREATING A FRAMEWORK ON SWIFT
![1*TFJsgX7UUToFVL00qV9cdA](https://user-images.githubusercontent.com/110424672/203581211-5621e99b-c6b5-4939-9e7a-0064e6b2e6ca.png)


Learn how to build an iOS framework, which lets you share code between apps to modularize your code.

## What is a Framework?
**Frameworks** are self-contained, reusable chunks of code and resources you can import into many apps. You can even share them across iOS, tvOS, watchOS and macOS apps. When combined with Swift’s access control, frameworks help define strong, testable interfaces between code modules.

**Frameworks have three major purposes:**
- Encapsulate code
- Modularize code
- Reuse code

## Creating a Framework

### STEP 1:
In Xcode, select **File ▸ New ▸ Project….** Then choose **iOS ▸ Framework & Library ▸ Framework.**
<img width="1512" alt="Screenshot 2022-11-23 at 9 14 35 AM" src="https://user-images.githubusercontent.com/110424672/203581921-830d292a-6b83-4ced-9afc-0703391b0628.png">

### STEP 2:
Name your project
<img width="1512" alt="Screenshot 2022-11-23 at 9 17 24 AM" src="https://user-images.githubusercontent.com/110424672/203582513-0433d457-491d-401b-ab05-4b88ab29f3bf.png">

### STEP 3: 
Add your classes to the Framework including the code. In my case, I will add `Logger` to print a simple log once the main app starts

```
public class Logger {
    public init() {}
    
    public func log(message: String) {
        print("Log message: ", message)
    }
}
```
<img width="1072" alt="Screenshot 2022-11-23 at 9 25 46 AM" src="https://user-images.githubusercontent.com/110424672/203584384-fb6f41b3-0b1d-4a49-a26e-b61a99c5655e.png">


### STEP 4: 
Time to export our project to `xcframework` file using an old friend... **The Terminal**

Open your terminal and navigate to the framework folder. Alternatively, you could drag your project folder to your terminal after the cd command:

<img width="785" alt="Screenshot 2022-11-23 at 9 58 51 AM" src="https://user-images.githubusercontent.com/110424672/203592312-d67130c1-9604-4d2a-b6a4-ffda122b7d06.png">

Next, start **archiving** your framework for the following targets:
- iOS
- Simulator
- MacOS


![1_w4TiFoJsB3gz8C01Qt1OqA](https://user-images.githubusercontent.com/110424672/203831511-aa9c70a0-ca22-4cc7-8992-49cc5a23bf2e.jpg)


### Commands for Terminal 
<details><summary>Archive for iOS</summary>
<p>

```
xcodebuild archive \
-scheme FrameworkName \
-configuration Release \
-destination 'generic/platform=iOS' \
-archivePath './build/FrameworkName.framework-iphoneos.xcarchive' \
SKIP_INSTALL=NO \
BUILD_LIBRARY_FOR_DISTRIBUTION=YES
```

</p>
</details>

<details><summary>Archive for Simulator</summary>
<p>

```
xcodebuild archive \
-scheme FrameworkName \
-configuration Release \
-destination 'generic/platform=iOS Simulator' \
-archivePath './build/FrameworkName.framework-iphonesimulator.xcarchive' \
SKIP_INSTALL=NO \
BUILD_LIBRARY_FOR_DISTRIBUTION=YES
```

</p>
</details>

<details><summary>Achieve for MacOS</summary>
<p>

```
xcodebuild archive \
-scheme FrameworkName \
-configuration Release \
-destination 'platform=macOS,arch=x86_64,variant=Mac Catalyst' \
-archivePath './build/FrameworkName.framework-catalyst.xcarchive' \
SKIP_INSTALL=NO \
BUILD_LIBRARY_FOR_DISTRIBUTION=YES
```

</p>
</details>

<details><summary>Create Framework</summary>
<p>

```
xcodebuild -create-xcframework \
-framework './build/FrameworkName.framework-iphonesimulator.xcarchive/Products/Library/Frameworks/FrameworkName.framework' \
-framework './build/FrameworkName.framework-iphoneos.xcarchive/Products/Library/Frameworks/FrameworkName.framework' \
-framework './build/FrameworkName.framework-catalyst.xcarchive/Products/Library/Frameworks/FrameworkName.framework' \
-output './build/FrameworkName.xcframework'

```

</p>
</details>

<details><summary>All in One</summary>
<p>

```
xcodebuild archive \
-scheme FrameworkName \
-configuration Release \
-destination 'generic/platform=iOS' \
-archivePath './build/FrameworkName.framework-iphoneos.xcarchive' \
SKIP_INSTALL=NO \
BUILD_LIBRARY_FOR_DISTRIBUTION=YES
xcodebuild archive \
-scheme FrameworkName \
-configuration Release \
-destination 'generic/platform=iOS Simulator' \
-archivePath './build/FrameworkName.framework-iphonesimulator.xcarchive' \
SKIP_INSTALL=NO \
BUILD_LIBRARY_FOR_DISTRIBUTION=YES
xcodebuild archive \
-scheme FrameworkName \
-configuration Release \
-destination 'platform=macOS,arch=x86_64,variant=Mac Catalyst' \
-archivePath './build/FrameworkName.framework-catalyst.xcarchive' \
SKIP_INSTALL=NO \
BUILD_LIBRARY_FOR_DISTRIBUTION=YES
xcodebuild -create-xcframework \
-framework './build/FrameworkName.framework-iphonesimulator.xcarchive/Products/Library/Frameworks/FrameworkName.framework' \
-framework './build/FrameworkName.framework-iphoneos.xcarchive/Products/Library/Frameworks/FrameworkName.framework' \
-framework './build/FrameworkName.framework-catalyst.xcarchive/Products/Library/Frameworks/FrameworkName.framework' \
-output './build/FrameworkName.xcframework'

```

</p>
</details>

For being fast we are going to take the **All in One** command
<img width="1625" alt="Screenshot 2022-11-23 at 10 10 28 AM" src="https://user-images.githubusercontent.com/110424672/203594588-93d394cb-173a-488b-a029-1f7410c402e2.png">

Once Terminal finishes the process. You will be able to see in your finder and the following screenshot, you generate three different archives files from your framework.

<img width="775" alt="Screenshot 2022-11-23 at 10 16 49 AM" src="https://user-images.githubusercontent.com/110424672/203595907-42ec3ad8-3c79-45bd-ba03-586157ee0c29.png">

You’ve now made your first **XCFramework.**


### FOR YOUR INFORMATION 
This command will generate an archive of your framework by using the following list as inputs :
- **-scheme CalendarControl:** It’ll use this scheme for archiving.
- **-configuration Release:** It’ll use the release configuration for building.
- **-destination ‘generic/platform=iOS’:** This is the architecture type.
- **-archivePath:** It saves archives into this folder path with the given name.
- **SKIP_INSTALL:** Set NO to install the framework to the archive.
- **BUILD_LIBRARIES_FOR_DISTRIBUTION:** Ensures your libraries are built for distribution and creates the interface file.
- **-destination ‘generic/platform=iOS Simulator’:** This is where you set the architecture type.
- **-archivePath:** This generates the archive into the folder path with the given name.

## Integrating XCFramework Into Your Project
Drag _XCFramework_ to the Frameworks, Libraries and Embedded Content section of your project target:

<img width="1512" alt="Screenshot 2022-11-23 at 10 20 03 AM" src="https://user-images.githubusercontent.com/110424672/203596774-9408e83d-1636-44b8-ad7a-a324088fb153.png">

After that, you will have your framework added on your main app project.  
<img width="1084" alt="Clientazo_—_Clientazo_xcodeproj" src="https://user-images.githubusercontent.com/110424672/203596872-1728017f-e196-44d4-89a7-713893d1e060.png">

### Time to test it 

**Importing**

<img width="847" alt="Screenshot 2022-11-23 at 10 24 21 AM" src="https://user-images.githubusercontent.com/110424672/203597824-b1f7e5fd-8593-49ff-9095-e01fb9fc7dc8.png">

**Running**

<img width="1324" alt="Screenshot 2022-11-23 at 10 25 57 AM" src="https://user-images.githubusercontent.com/110424672/203597995-a6ac2491-310b-4d3b-8415-07adeeb5a947.png">

# That's it, Enjoy!





