# ios-hash-demo
ハッシュ化するサンプルです。

## Settings

①プロジェクト配下にCommonCryptoディレクトリを作る。<br>

②CommonCrypto配下にmodule.mapを作成する。

```:module.map
module CommonCrypto {
    header "/usr/include/CommonCrypto/CommonCrypto.h"
    export *
}
```
③Build Settings -> Swift Compiler – Search Paths -> Import Pathsにフォルダのパスを通す。

```
$(SRCROOT)/CommonCrypto
```

④importする

```
import CommonCrypto
```

[補足] ブリッジヘッダーに定義する方法もある
ブリッジヘッダーでヘッダーファイルをimportする。

```
#import <CommonCrypto/CommonCrypto.h>
```

## Usage
```
let string = "demo"
let hashedString = string.md5
```

## サンプル

```swift:String+Hash.swift
import Foundation
import CommonCrypto

extension String {

    var md5: String {
        var md5String = ""
        let digestLength = Int(CC_MD5_DIGEST_LENGTH)
        let md5Buffer = UnsafeMutablePointer<UInt8>.allocate(capacity: digestLength)

        if let data = self.data(using: .utf8) {
            data.withUnsafeBytes({ (bytes: UnsafePointer<CChar>) -> Void in
                CC_MD5(bytes, CC_LONG(data.count), md5Buffer)
                md5String = (0..<digestLength).reduce("") { $0 + String(format:"%02x", md5Buffer[$1]) }
            })
        }
        
        return md5String
    }

    var sha1: String {

        let str = self.cString(using: String.Encoding.utf8)
        let strLen = CC_LONG(self.lengthOfBytes(using: String.Encoding.utf8))
        let digestLen = Int(CC_SHA1_DIGEST_LENGTH)
        let result = UnsafeMutablePointer<CUnsignedChar>.allocate(capacity: digestLen)

        CC_SHA1(str!, strLen, result)

        let hash = NSMutableString()
        for i in 0..<digestLen {
            hash.appendFormat("%02x", result[i])
        }
        result.deinitialize()

        return String(hash)
    }
}
```
