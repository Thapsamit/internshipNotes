## addon.cpp
```cpp
// addon.cpp

#include <node.h>
#import <Foundation/Foundation.h>

using namespace v8;

void HideElectronWindow(const FunctionCallbackInfo<Value>& args) {
    Isolate* isolate = args.GetIsolate();

    // Extract the window ID from JavaScript arguments
    if (args.Length() < 1 || !args[0]->IsString()) {
        isolate->ThrowException(Exception::TypeError(
            String::NewFromUtf8(isolate, "Invalid arguments. Expected a window ID as a string.")));
        return;
    }

    String::Utf8Value windowIdArg(args[0]->ToString());
    const char* windowId = *windowIdArg;

    // Load the Swift module
    NSBundle *bundle = [NSBundle mainBundle];
    NSString *swiftModulePath = [bundle pathForResource:@"ScreenShareHelper" ofType:@"xcframework"];
    NSBundle *swiftModuleBundle = [NSBundle bundleWithPath:swiftModulePath];
    [swiftModuleBundle load];

    // Call the Swift function with the window ID
    Class ScreenShareHelperClass = NSClassFromString(@"ScreenShareHelper");
    id screenShareHelperInstance = [[ScreenShareHelperClass alloc] init];

    SEL hideWindowSelector = NSSelectorFromString(@"hideElectronWindow:");
    NSInvocation *invocation = [NSInvocation invocationWithMethodSignature:
                                [screenShareHelperInstance methodSignatureForSelector:hideWindowSelector]];
    [invocation setSelector:hideWindowSelector];
    [invocation setTarget:screenShareHelperInstance];
    [invocation setArgument:&windowId atIndex:2];  // 0 and 1 are reserved for target and selector

    [invocation invoke];

    // Return success to JavaScript
    args.GetReturnValue().Set(String::NewFromUtf8(isolate, "Electron window hidden during screen share."));
}

void Initialize(Local<Object> exports) {
    NODE_SET_METHOD(exports, "hideElectronWindow", HideElectronWindow);
}

NODE_MODULE(NODE_GYP_MODULE_NAME, Initialize)
```

- create swift module

// ScreenShareHelper.swift
```swift
import Cocoa

public class ScreenShareHelper {
    public init() {}

    public func hideElectronWindow(windowId: String) {
        print("Hiding Electron window with ID: \(windowId)")

        // Assuming 'windowId' is the native window handle or identifier
        if let window = NSApp.window(withWindowNumber: Int(windowId)) {
            window.orderOut(nil)
        }
    }
}
```


- swift build --product ScreenShareHelper -c release



-
```cpp
import AppKit

// Assuming window is your NSWindow instance
if let window = yourWindow {
    window.sharingType = NSWindowSharingType(rawValue: 0)
}
```




```swift
// Package.swift

// swift-tools-version:5.3
import PackageDescription

let package = Package(
    name: "ScreenShareHelper",
    platforms: [
        .macOS(.v10_12),
    ],
    products: [
        .library(
            name: "ScreenShareHelper",
            targets: ["ScreenShareHelper"]),
    ],
    targets: [
        .target(
            name: "ScreenShareHelper",
            dependencies: []),
    ]
)


```


```

project-root/
|-- electron-app/
|   |-- main.js
|   |-- index.html
|   |-- package.json
|   |-- node_modules/
|   |   |-- ... (Electron dependencies)
|-- node-addon/
|   |-- binding.gyp
|   |-- addon.cpp
|   |-- build/
|   |   |-- Release/
|   |       |-- node-addon.node
|   |-- ScreenShareHelper.xcframework/
|       |-- ... (Swift module content)
|-- swift-package/
|   |-- Package.swift
|   |-- Sources/
|   |   |-- ScreenShareHelper/
|   |       |-- ScreenShareHelper.swift
|-- build/
|   |-- ... (Build artifacts, generated automatically)


```

