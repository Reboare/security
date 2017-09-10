# Cryptography

## Modern

### RSA

Use [RSACtfTool ](https://github.com/Ganapati/RsaCtfTool)for any RSA keys which appear to be obviously weak.  It runs a full suite of tests so it can be used to rule out anything obvious.

```bash
./RsaCtfTool.py --publickey ./key.pub --uncipher ./ciphered\_file
```

### Elliptic-Curve Cryptography

When trying to decrypt or encrypt with elliptic-curve cryptography the recommended tool is [seccure](http://point-at-infinity.org/seccure/) or [python-seccure](https://pypi.python.org/pypi/seccure).

```py
seccure.decrypt(ciphertext, b'my private key')
```

## Classical Ciphers

The most difficult element of cracking a cipher is identifying it's type.  There are a number of markers however that can help in reducing the search space.  A good resource for this is [Practical Cryptography's Guide](http://practicalcryptography.com/cryptanalysis/text-characterisation/identifying-unknown-ciphers/).

A critical skill in cracking a cipher is identifying the type of cipher it has been encrypted with, then practical cryptography has a good [guide](http://practicalcryptography.com/cryptanalysis/text-characterisation/identifying-unknown-ciphers/) to allowing you to begin initial analysis.  

