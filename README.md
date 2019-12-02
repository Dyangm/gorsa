# GoRSA 加解密库


获取扩展包:

```
go get github.com/Dyangm/gorsa
```

具体使用:

```
package main

import (
	"fmt"
	"github.com/Dyangm/gorsa"
)

func main() {
	// 生成私钥
	privateKey, err := gorsa.GetPrivateKey()
	if err != nil {
		panic(err)
	}

	fmt.Println(privateKey)
	//由私钥生成公钥
	pubKey, err := gorsa.GetPublicKey(privateKey)
	if err != nil {
		panic(err)
	}
	fmt.Println(pubKey)

	// 公钥加密私钥解密
	if err := applyPubEPriD(privateKey, pubKey); err != nil {
		panic(err)
	}
	// 公钥解密私钥加密
	if err := applyPriEPubD(privateKey, pubKey); err != nil {
		panic(err)
	}
}

// 公钥加密私钥解密
func applyPubEPriD(privateKey, pubKey string) error {
	pubenctypt, err := gorsa.PublicEncrypt(`hello world`, pubKey)
	if err != nil {
		return err
	}

	pridecrypt, err := gorsa.PriKeyDecrypt(pubenctypt, privateKey)
	if err != nil {
		return err
	}
	if string(pridecrypt) != `hello world` {
		return fmt.Errorf(`解密失败`)
	}
	return nil
}

// 公钥解密私钥加密
func applyPriEPubD(privateKey, pubKey string) error {
	prienctypt, err := gorsa.PriKeyEncrypt(`hello world`, privateKey)
	if err != nil {
		return err
	}

	pubdecrypt, err := gorsa.PublicDecrypt(prienctypt, pubKey)
	if err != nil {
		return err
	}
	if string(pubdecrypt) != `hello world` {
		return fmt.Errorf(`解密失败`)
	}
	return nil
}

```
