![iOS](https://img.shields.io/badge/os-iOS-green.svg?style=flat)
[![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage)
[![GitHub license](https://img.shields.io/badge/license-MIT-lightgrey.svg)](LICENSE.txt)

# CommonCrypto Wrapper Framework
A simple wrapper framework for the [CommonCrypto][CommonCrypto-Source] C library which can be used in Swift applications and frameworks.

The framework builds with the CommonCrypto version of the selected target SDK of your project. It is compatible with iOS Simulator and Device SDKs.
The framework uses [LLVM modules][LLVM-Modules] to make the CommonCrypto interfaces available to Swift.


You should be aware that CommonCrypto is not designed to be used with Swift and using the C based funtions can be quite challenging. It is recommended to wrap the CommonCrypto functionality inside a separate Swift framework using CommonCrypto in order to use it in Swift applications.


## Integration with Carthage
Cou can add the framework to your [Cartfile][Carthage-Cartfile] just like any other Carthage compatible framework:
```
github "wiedem/CommonCrypto"
```

Follow the other common steps as described on the [Carthage website][Carthage] for the integration into your project.
No further steps are required, you don't need to create or add any modulemap files.


## Using CommonCrypto in Swift
You can use CommonCrypt after integrating the wrapper framework into your project just like any other Swift framework.

**Example:**
```Swift
import CommonCrypto

open class CryptoTest {
  open class func hashString(_ string: String) -> Data {
    let ctx = UnsafeMutablePointer<CC_SHA1_CTX>.allocate(capacity: 1)
    defer { ctx.deallocate(capacity: 1) }

    let data: Data = string.data(using: .utf8)!
    CC_SHA1_Init(ctx)

    data.withUnsafeBytes { (rawPtr: UnsafePointer<UInt8>) -> Void in
      CC_SHA1_Update(ctx, rawPtr, UInt32(data.count))
    }

    var hashData = Data(count: Int(CC_SHA1_DIGEST_LENGTH))
    hashData.withUnsafeMutableBytes { (rawPtr: UnsafeMutablePointer<UInt8>) -> Void in
      CC_SHA1_Final(rawPtr, ctx)
    }

    return hashData
  }
}
```


## License
The CommonCrypto wrapper framework is released under the the [MIT license](LICENSE.txt).


[CommonCrypto-Source]: https://opensource.apple.com/source/CommonCrypto/ "CommonCrypto Sources"
[Security-Apple-Developer]: https://developer.apple.com/security/ "Security - Appler Developer"
[LLVM-Modules]: http://clang.llvm.org/docs/Modules.html "LLVM - Modules"
[Carthage]: https://github.com/Carthage/Carthage "Carthage"
[Carthage-Cartfile]: https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#cartfile "Carthage - Cartfile"
