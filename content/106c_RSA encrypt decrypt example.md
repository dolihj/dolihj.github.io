# Source: 
[open tutorial](https://opentutorials.org/course/228/4894) Public Key, SSL,

# Related Docs
[[106_RSA algorithm with Proof and Digial Cert]], [[106d_Digital Cert]]

# Openssl Encrypt Decrypt

echo 'my bankaccount password is SUPERRICH' > 001_plaintopsecret.txt

openssl enc -e  -aes256 -in 001_plaintopsecret.txt -out 002_ciphered_topsecret.txt

openssl enc -d  -aes256 -in 002_ciphered_topsecret.txt -out 003_decipher.txt



# Generate RSA Key


openssl genrsa -out 01private.pem

e : 65537


## Private Key generate
openssl genrsa -out private.pem 1024;

## Public Key generate
openssl rsa -in private.pem -out public.pem -outform PEM -pubout;

> Other Option : Using ssh-keygen

ssh-keygen -t rsa -b 2048 -m PEM -f ./hjid_rsa
ssh-keygen -f hjid_rsa.pub -m 'PEM' -e > hj_pub.pem

```
Your identification has been saved in ./hj_rsa_key
Your public key has been saved in ./hj_rsa_key.pub
The key fingerprint is:
SHA256:clLi82Z23XIL1d9s1A80NbDtYlO2sdFCL/HQbU+npxk parkhj1@hjubuntu
The key's randomart image is:
+---[RSA 2048]----+
|             .=oo|
|             .o*O|
|      . .    .=X*|
|     . o     .E=O|
|      = S    +oO+|
|       *   ..o=++|
|        = . + o *|
|       + .   + o |
|              .  |
+----[SHA256]-----+
```




## Encrypt with public key 

echo 'My name is Bob and I want to talk to Alice' > file.txt


openssl rsautl -encrypt -inkey 02_public.pem -pubin -in file.txt -out file.ssl
openssl rsautl -decrypt -inkey 01private.pem -in file.ssl -out decrypted.txt



## Decrypt with private key
openssl rsautl -decrypt -inkey hj_prv.pem -in 02enc -out 03dec
