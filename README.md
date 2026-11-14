# santa-secrets

We removed the plain secret, so we are now again secure.
Now, we will save it in a secure format using RSA encryption.
RSA is only as strong as the primes behind it.

## Manuals for Santa: How to encrypt text using RSA keys
For encryption use public key (public.pem) that is known to all. So everybody can encrypt.
For decryption use private key (private.pem) so only owner can decrypt and read secrets.

### Encryption
What we need:
- secret.txt
- public.pem

```
openssl pkeyutl \
  -encrypt \
  -pubin \
  -inkey public.pem \
  -in secret.txt \
  -pkeyopt rsa_padding_mode:oaep \
  -pkeyopt rsa_oaep_md:sha256 \
  | base64
```

### Decryption
What we need:
- cipher.txt
- private.pem

```
cat cipher.txt | base64 -d | openssl pkeyutl \
  -decrypt \
  -inkey private.pem \
  -in /dev/stdin \
  -pkeyopt rsa_padding_mode:oaep \
  -pkeyopt rsa_oaep_md:sha256
```
