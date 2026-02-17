# Certificates and Priv/Pub keys with openssl cheatsheet

## How to get the Certificate's site     
- With openssl:  
  Display digital certificates from Google together with information about the secure session, the cipher suite in play, and related items.  
  The encoding for the certificates is base64.
  ```
  # openssl s_client -connect google.com:443 -showcerts
  ```

  Check the content:
  ```
  $ openssl x509 -inform [der,pem,etc] -in stackexchange.com.cer -text
  ```

  Download cert and create .pem (i.e. for import or be used in Python requests -verify='path_to.pem'):  
  *Warning: check if full chain is needed, or if have to strip everything but BEGIN/END CERT blocks*
  ```
  # openssl s_client -connect google.com:443 -showcerts | openssl x509 -outform PEM > mycert.pem
  ```

- With Chrome:  
`Click on ssl padlock -> Certificates -> Select the cert to export (root, intermediate or cert) -> drag the certifitate icon to Finder`  

---

## How to import the site certificate into the OS to trust
(Chrome uses the OS's cert store)  
MacOS: Open Keychain Access app -> Add .cer file -> Trust  
Linux: see https://tarunlalwani.com/post/self-signed-certificates-trusting-them/

---

## Public and Private key pair: 
> When using OpenSSL to create these keys, there are two separate commands: one to create a private key, and another to extract the matching public key from the private one. These key pairs are encoded in **base64**, and their sizes can be specified during this process.  
The private key consists of numeric values, two of which (a modulus and an exponent) make up the public key. Although the private key file contains the public key, the extracted public key does not reveal the value of the corresponding private key.  
The resulting file with the private key thus contains the full key pair. Extracting the public key into its own file is practical because the two keys have distinct uses, but this extraction also minimizes the danger that the private key might be publicized by accident.  

1. Create Priv Key  
   Example: generate a 2048-bit RSA key pair with OpenSSL:
   ```
   # openssl genpkey -out privkey.pem -algorithm rsa 2048
   ```

   Resulting privkey.pem file, which is in base64:
   ```
   -----BEGIN PRIVATE KEY-----
   MIICdgIBADANBgkqhkiG9w0BAQEFAASCAmAwggJcAgEAAoGBANnlAh4jSKgcNj/Z
   JF4J4WdhkljP2R+TXVGuKVRtPkGAiLWE4BDbgsyKVLfs2EdjKL1U+/qtfhYsqhkK
   …
   -----END PRIVATE KEY-----
   ```

2. Extract the pair’s Public key from the Private:
   ```
   # openssl rsa -in privkey.pem -outform PEM -pubout -out pubkey.pem
   ```

   Resulting pubkey.pem:  
   ```
   -----BEGIN PUBLIC KEY-----
   MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDZ5QIeI0ioHDY/2SReCeFnYZJY
   z9kfk11RrilUbT5BgIi1hOAQ24LMilS37NhHYyi9VPv6rX4WLKoZCmkeYaWk/TR5
   4nbH1E/AkniwRoXpeh5VncwWMuMsL5qPWGY8fuuTE27GhwqBiKQGBOmU+MYlZonO
   O0xnAKpAvysMy7G7qQIDAQAB
   -----END PUBLIC KEY-----
   ```  

### Create Priv/Pub key pair with ssh-keygen
Obs: ssh-keygen uses openssl lib. ssh-keygen simplifies the process of key pair creation (for ssh key access, for example).
```
# ssh-keygen -t rsa -b 4096
```

It saves the priv/pub files to the default location in the .ssh directory of your home directory.  
Then you can copy the public key into the server’s authorized_keys file manually, or with the ssh-copy-id command.

---

## Digital Certificates:
1. Create the certificate signing request (CSR).  
   A CSR consists mainly of the public key of a key pair, and some additional information.  
   The CSR that is generated can be sent to a CA to request the issuance of a CA-signed SSL certificate.  

   ```
   # openssl req -new -newkey rsa:2048 -nodes -keyout privkeyDC.pem -out myserver.csr
   ```

   Option: Generate a CSR from an existing Private Key:
   ```
   # openssl req -key domain.key -new -out domain.csr
   ```

   Check out the csr content:
   ```
   # openssl req -text -in myserver.csr -noout -verify
   ```

   ### **Option**: create a self-signed certificate:
   ```
   # openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:4096 -keyout myserver.pem -out myserver.crt
   ```

   Check out the crt content:
   ```
   # openssl x509 -in myserver.crt -text -noout
   ```

## Convert Certificate Formats
Normally X.509 certificates are ASCII PEM encoded

- Convert PEM (.crt) to DER
   ```
   # openssl x509 -in domain.crt -outform der -out domain.der
   ```
   The DER format is typically used with Java.

- Convert DER to PEM
   ```
   # openssl x509 -inform der -in domain.der -out domain.crt
   ```

## How to verify Modulus
Useful to ensure the correct key is used with a certificate. Modulus must match between priv key, CSR and Cert.

- Private key
   ```
   # openssl rsa -noout -modulus -in private.key
   ```
- CSR
   ```
   # openssl req -noout -modulus -in csr.csr
   ```
- Certificate
   ```
   # openssl x509 -noout -modulus -in certificate.crt
   ```
  - Or getting the cert directly from the site
     ```
     # openssl s_client -connect site.com:443 | openssl x509 -noout -modulus -dates
     ```

---

## PKI Concepts
1. A requestor generates a CSR and submits it to the CA.
2. The CA issues a certificate based on the CSR and returns it to the requestor.
3. Should the certificate at some point be revoked, the CA adds it to its CRL.

### Components:  
- Public Key Infrastructure (PKI): Security architecture where trust is conveyed through the signature of a trusted CA.
- Certificate Authority (CA): Entity issuing certificates and CRLs.
- Registration Authority (RA): Entity handling PKI enrollment. May be identical with the CA.
- Certificate: Public key and ID bound by a CA signature.
- Certificate Signing Request (CSR): Request for certification. Contains public key and ID to be certified.
- Certificate Revocation List (CRL): List of revoked certificates. Issued by a CA at regular intervals.
- Certification Practice Statement (CPS): Document describing structure and processes of a CA.

### CA Types:
- Root CA: CA at the root of a PKI hierarchy. Issues only CA certificates.
- Intermediate CA: CA below the root CA but not a signing CA. Issues only CA certificates.
- Signing CA: CA at the bottom of a PKI hierarchy. Issues only **user certificates**.

### Certificate Types:
- CA Certificate: Certificate of a CA. Used to sign certificates and CRLs.
- Root Certificate: Self-signed CA certificate at the root of a PKI hierarchy. Serves as the PKI’s trust anchor.
- Cross Certificate: CA certificate issued by a CA external to the primary PKI hierarchy. Used to connect two PKIs and thus usually comes in pairs.
- User Certificate: End-user certificate issued for one or more purposes: email-protection, server-auth, client-auth, code-signing, etc. A user certificate cannot sign other certificates.

### File Formats:
- Privacy Enhanced Mail (PEM): Text format. Base-64 encoded data with header and footer lines. Preferred format in OpenSSL and most software based on it (e.g. Apache mod_ssl, stunnel).
- Distinguished Encoding Rules (DER): Binary format. Preferred format in Windows environments. Also the official format for Internet download of certificates and CRLs.

---

## References
https://opensource.com/article/19/6/cryptography-basics-openssl-part-1
https://opensource.com/article/19/6/cryptography-basics-openssl-part-2
https://www.digitalocean.com/community/tutorials/openssl-essentials-working-with-ssl-certificates-private-keys-and-csrs

