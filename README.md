## Библиотека обеспечения безопасности данных на основе эллиптических кривых

Пакет cryptoswitch реализует схему ECIES с выбором алгоритма шифрования (на вход подаются симметричные алгоритмы шифрования, такие как AES, TwoFish и т.д., и режим шифрования )

### ECIES

![Encryption](https://github.com/elizarpif/diploma-elliptic/blob/develop/assets/ecies_encryption.png)

![Decryption](https://github.com/elizarpif/diploma-elliptic/blob/develop/assets/ecies_decryption.png)

### Возможные алгоритмы (курсивом - не реализовано):
- **AES**
- **DES**
- _3DES (TripleDES)_
- _RC5_
- _Blowfish_
- **Twofish**
- **Camellia**
- _RC4_
- _SEAl_

```go
privKey, err := cryptoswitch.GenerateKey()
if err != nil {
    log.WithError(err).Error("can't generate private key")
    return err
}

cw := cryptoswitch.NewCryptoSwitch(cryptoswitch.AES, cryptoswitch.GCM)

encrypt, err := cw.Encrypt(privKey.PublicKey, []byte(testingMessage))
if err != nil {
    log.WithError(err).Error("can't encrypt aes")
    return err
}
```

### Возможные режимы шифрования:
- CBC
- GCM (работает только с шифрованием ключа 128 бит)

GCM – **Galois/Counter Mode** – режим счётчика с аутентификацией Галуа: это режим аутентифицированного шифрования, который, к тому же, поддерживает аутентификацию дополнительных данных (передаются в открытом виде). В англоязычной литературе это называется AEAD – Authenticated Encryption with Associated Data. В ГОСТовой криптографии такого режима как раз не хватает. Аутентифицированное шифрование позволяет обнаружить изменения сообщения до его расшифрования, для этого сообщение снабжается специальным кодом аутентификации (в русскоязычной традиции также называется имитовставкой). GCM позволяет защитить кодом аутентификации не только шифрованную часть сообщения, но и произвольные прикреплённые данные – это полезно, потому что в этих данных может быть записан, например, адрес получателя или другая открытая информация, которую, вместе с тем, требуется защитить от искажений/подмены.