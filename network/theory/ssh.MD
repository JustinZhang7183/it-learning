# SSH
### What is SSH
- Secure Shell Protocol
- It is a remote administration protocol
- It allows users to **access, control and modify their remote servers** over the internet
- It was a replacement for the unencrypted Telnet
- Using cryptographic techniques to ensure that all communication to and from the remote server happens in an encrypted manner
### How to generate an SSH key pair
- it is recommended that you specify a passphrase to protect your private key against unauthorized use.
```
# use -t to specify the type of key to create
ssh-keygen -t rsa
# use -b to specify the length (bit size)
ssh-keygen -b 2048 -t rsa
```
### How Does SSH Work
###### ssh {user}@{host}
- {user} represents the account you want to access
- {host} refers to the computer you want to access (IP address or domain name)
### Different Encryption Techniques
###### Symmetrical Encryption
- A secret key is used for both encryption and decryption of a message by both client and the host
- There is usually only one key, or sometimes a pair of keys (one key can easily be calculated using the other key)
- The process of creating a symmetric key in ssh is carried out by a **key exchange algorithm - Diffie-Hellman key exchange** (Two computer share public pieces of data and then manipulate it with disignated ciphers to independently calculate the secret key)
- Symmetrical encryption ciphers including AES (Advanced Encryption Standard), CAST128, Blowfish, etc
###### Asymmetrical Encryption
- Using two separate keys for encryption and decryption (public key and private key)
- Public key can be used by any individual to encrypt a message, the message can only be decrypted by the recipient who possesses their particular private key, and vice versa
###### Hashing
- One-way-hash, can not be decrypted
- Generate a unique value of a fixed length for each input
### What Does SSH Work With These Encryption Techniques
1. SSH operates on TCP port 22 by default
2. A client tries to connect to a server
3. Server presents the encryption protocols and respective versions that it supports.
4. If the client has a similar matching pair of a protocol and version, an agreement is reaced and connection is started with the accepted protocol.
5. Client compares the host public key with server private key, if the keys match, client and server agree to use symmetric encryption to communicate during the SSH session (If it's the first time, client will save the host public key)
6. Using Diffie-Hellman key exchange algorithm to generate a shared secret key
7. Using the shared secret key to communicate.
