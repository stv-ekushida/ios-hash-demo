# ios-hash-demo
ハッシュ化するサンプル

## Settings

①CommonCryptoディレクトリを作る
②CommonCrypto配下にmodule.mapを作成する。

```:module.map
module CommonCrypto {
    header "/usr/include/CommonCrypto/CommonCrypto.h"
    export *
}
```
③Build Settings -> Swift Compiler – Search Paths -> Import Pathsにフォルダのパスを通す

```
$(SRCROOT)/CommonCrypto
```

④ブリッジヘッダーでヘッダーファイルをimportする

```
#import <CommonCrypto/CommonCrypto.h>
```

## Usage
```
let string = "demo"
let hashedString = string.md5
```
