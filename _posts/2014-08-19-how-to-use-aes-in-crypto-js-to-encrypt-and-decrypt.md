---
layout: post
title:  "如何使用CryptoJS的AES方法进行加密和解密"
date:   2014-08-19 15:56:40
categories: JavaScript CryptoJS AES
---
最近在调用一个serve接口的时候需要先对数据进行加密后再传输，使用的是`AES/ECB/PKCS5Padding`，前端选择了CryptoJS，但第一次用此加密库踩了不少坑。  
那么现在就将整个加密和解密的过程中踩过那些坑记录下来：  
首先准备一份明文和秘钥：  
```
var plaintText = 'aaaaaaaaaaaaaaaa'; // 明文
var keyStr = 'bbbbbbbbbbbbbbbb'; // 一般key为一个字符串
```

参看官网文档，AES方法是支持AES-128、AES-192和AES-256的，加密过程中使用哪种加密方式取决于传入key的类型，否则就会按照AES-256的方式加密。  

> CryptoJS supports AES-128, AES-192, and AES-256. It will pick the variant by the size of the key you pass in. If you use a passphrase, then it will generate a 256-bit key.  

由于Java就是按照128bit给的，但是由于是一个字符串，需要先在前端将其转为128bit的才行。  
最开始以为使用`CryptoJS.enc.Hex.parse`就可以正确地将其转为128bit的key。但是不然...  
经过多次尝试，需要使用`CryptoJS.enc.Utf8.parse`方法才可以将key转为128bit的。好吧，既然说了是多次尝试，那么就不知道原因了，后期再对其进行更深入的研究。  
```
// 字符串类型的key用之前需要用uft8先parse一下才能用
var key = CryptoJS.enc.Utf8.parse(keyStr);
```

由于后端使用的是`PKCS5Padding`，但是在使用CryptoJS的时候发现根本没有这个偏移，查询后发现`PKCS5Padding`和`PKCS7Padding`是一样的东东，使用时默认就是按照`PKCS7Padding`进行偏移的。
```
// 加密
var encryptedData = CryptoJS.AES.encrypt(plaintText, key, {
    mode: CryptoJS.mode.ECB,
    padding: CryptoJS.pad.Pkcs7
});
```

由于CryptoJS生成的密文是一个对象，如果直接将其转为字符串是一个Base64编码过的，在`encryptedData.ciphertext`上的属性转为字符串才是后端需要的格式。
```
var encryptedBase64Str = encryptedData.toString();
// 输出：'RJcecVhTqCHHnlibzTypzuDvG8kjWC+ot8JuxWVdLgY='
console.log(encryptedBase64Str);

// 需要读取encryptedData上的ciphertext.toString()才能拿到跟Java一样的密文
var encryptedStr = encryptedData.ciphertext.toString();
// 输出：'44971e715853a821c79e589bcd3ca9cee0ef1bc923582fa8b7c26ec5655d2e06'
console.log(encryptedStr);
```

由于加密后的密文为128位的字符串，那么解密时，需要将其转为Base64编码的格式。  
那么就需要先使用方法`CryptoJS.enc.Hex.parse`转为十六进制，再使用`CryptoJS.enc.Base64.stringify`将其变为Base64编码的字符串，此时才可以传入`CryptoJS.AES.decrypt`方法中对其进行解密。  
```
// 拿到字符串类型的密文需要先将其用Hex方法parse一下
var encryptedHexStr = CryptoJS.enc.Hex.parse(encryptedStr);

// 将密文转为Base64的字符串
// 只有Base64类型的字符串密文才能对其进行解密
var encryptedBase64Str = CryptoJS.enc.Base64.stringify(encryptedHexStr);
```

使用转为Base64编码后的字符串即可传入`CryptoJS.AES.decrypt`方法中进行解密操作。
```
// 解密
var decryptedData = CryptoJS.AES.decrypt(encryptedBase64Str, key, {
    mode: CryptoJS.mode.ECB,
    padding: CryptoJS.pad.Pkcs7
});
```

经过CryptoJS解密后，依然是一个对象，将其变成明文就需要按照Utf8格式转为字符串。  
```
// 解密后，需要按照Utf8的方式将明文转位字符串
var decryptedStr = decryptedData.toString(CryptoJS.enc.Utf8);
console.log(decryptedStr); // 'aaaaaaaaaaaaaaaa'
```

#### 参考资料：
 * [CryptoJS](https://code.google.com/p/crypto-js/)
 * [http://stackoverflow.com/questions/14382883/javascript-aes-in-ecb-mode-with-pkcs5-padding](http://stackoverflow.com/questions/14382883/javascript-aes-in-ecb-mode-with-pkcs5-padding)
    