Week 5 Midterm Lab – Applied Cryptography  
Author: Giorgi Kordzaia  
Tools used: Windows CMD, OpenSSL, Git

Task 1 – AES Encryption and Decryption

I created a file called secret.txt with a secret message.  
I encrypted the file using AES-128-CBC with the -pbkdf2 option to avoid the OpenSSL warning.  
Then I decrypted the file and checked that the content matched the original.

Commands used:

echo This file contains top secret information. > secret.txt  
openssl enc -aes-128-cbc -salt -pbkdf2 -in secret.txt -out secret.enc -pass pass:MyStrongPass  
openssl enc -d -aes-128-cbc -pbkdf2 -in secret.enc -out decrypted.txt -pass pass:MyStrongPass

The decrypted file matched the original text. The warning was resolved by using -pbkdf2.

Task 2 – ECC Signature and Verification

I generated ECC private and public keys using the prime256v1 curve.  
I created a message file and signed it using my private key.  
I verified the signature using the public key and the result was successful.

Commands used:

openssl ecparam -name prime256v1 -genkey -noout -out ecc_private.pem  
openssl ec -in ecc_private.pem -pubout -out ecc_public.pem  
echo Elliptic Curves are efficient. > ecc.txt  
openssl dgst -sha256 -sign ecc_private.pem -out ecc.sig ecc.txt  
openssl dgst -sha256 -verify ecc_public.pem -signature ecc.sig ecc.txt

The signature verification was successful. Output was: Verified OK

Task 3 – Hashing and HMAC

Task 3A – I created a file and generated its SHA-256 hash.

Commands used:

echo Never trust, always verify. > data.txt  
certutil -hashfile data.txt SHA256

Task 3B – I created an HMAC using the key "secretkey123" and SHA-256.

Command used:

openssl dgst -sha256 -hmac "secretkey123" data.txt

Task 3C – I changed one letter in the data.txt file and generated a new HMAC.  
The HMAC changed completely, showing that the file was not the same.  
This proves that HMAC helps detect any changes in the file.  
I explained this in the file hmac_explanation.txt

Task 4 – Diffie-Hellman Key Exchange

I generated Diffie-Hellman parameters and created private keys for Alice and Bob.  
Each of them derived a shared secret using the other’s public key.  
I hashed the secrets to check if they were the same, and they matched.

Commands used:

openssl dhparam -out dhparam.pem 2048  
openssl genpkey -paramfile dhparam.pem -out alice_private.pem  
openssl genpkey -paramfile dhparam.pem -out bob_private.pem  
openssl pkey -in alice_private.pem -pubout -out alice_public.pem  
openssl pkey -in bob_private.pem -pubout -out bob_public.pem  
openssl pkeyutl -derive -inkey alice_private.pem -peerkey bob_public.pem -out alice_secret.bin  
openssl pkeyutl -derive -inkey bob_private.pem -peerkey alice_public.pem -out bob_secret.bin  
certutil -hashfile alice_secret.bin SHA256  
certutil -hashfile bob_secret.bin SHA256

The SHA-256 hashes of the shared secrets were identical.  
This shows that Diffie-Hellman key exchange worked successfully.

In the file dh_usage.txt I explained how this algorithm is used in real life, such as in TLS and secure messaging.

All tasks were completed using command-line tools only.
