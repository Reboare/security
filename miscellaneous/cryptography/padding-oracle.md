# Padding Oracle Attack

This applies to a general padding oracle attack, but it most commonly applicable to web-services, making it unclear exactly where this fits.  However, since it is not only web-service applicable and can be applied to a broad range of applications, I feel it's more applicable to be placed in a general cryptography/encryption section.

A padding oracle attack, attacks the general decryption mechanism used by a remote service rather than the encryption itself.  The attack targets a set of ciphers known as block-ciphers which rely on encrypting chunks of data rather than stream based ciphers which encrypt a stream of information at an arbitrary length.

------To Add-----

## Tools

### Padbuster

A tool called [padbuster](https://github.com/GDSSecurity/PadBuster), is incredibly effective at exploiting these vulnerabilities automatically.  The following example will decrypt a base64 encoded ciphertext using the padding oracle attack.

```bash
padbuster <url> <codetodecrypt> <blocksize> -encoding 0  -cookies <cookies>  -veryverbose
```

Once you've decrypted and edited the cookie, you can use the same tool and methods to re-encrypt the cookie.

```bash
padbuster <url> <codetodecrypt> <blocksize> -encoding 0 -plaintext <toencrypt> -cookies <cookies>  -veryverbose
```

### Alternatives

I've yet to properly test these out as I did attempt but didn't completely understand the concept of a padding oracle at the time so my solution wasn't correct, but these tools look interesting as a point of further research.

[https://github.com/mpgn/Padding-oracle-attack](https://github.com/mpgn/Padding-oracle-attack)

[https://github.com/mwielgoszewski/python-paddingoracle](https://github.com/mwielgoszewski/python-paddingoracle)

TODO: Understand the solution correctly, modify these examples and make sure they work.

## References

[https://blog.skullsecurity.org/2013/padding-oracle-attacks-in-depth](https://blog.skullsecurity.org/2013/padding-oracle-attacks-in-depth)

[https://blog.skullsecurity.org/2013/a-padding-oracle-example](https://blog.skullsecurity.org/2013/a-padding-oracle-example)

[https://robertheaton.com/2013/07/29/padding-oracle-attack/](https://robertheaton.com/2013/07/29/padding-oracle-attack/)

[https://cryptopals.com/sets/3/challenges/17](https://cryptopals.com/sets/3/challenges/17)

