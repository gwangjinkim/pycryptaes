
# Installation

```
pip install pycryptaes
```

# Use `pycryptaes` for encryption and decryption using the AES algorithm

```

from pycryptaes import AES

# cd first to the pycryptaes package or do `$ mkdir -p _test` where you are! 

# initialize a AES Object for encryption and decryption with the AES method.     
ca = AES()

# to be encoded message
message = "Hello, World! This is an encrypted message!"

# generate random key and capture than random nonce
key, nonce = ca.generate_random_key_tuple()

# encode the message - capture tag
ciphertext, tag = ca.encrypt(message.encode('utf-16'))

# write the encoded message with the other necessary variables into a file:
ca.write_ciphertext("./_test/_ciphertext", ciphertext, tag, nonce)

# read encoded message (and the additional variables) from file
ciphertext, tag, nonce = ca.read_ciphertext("./_test/_ciphertext")

# decode the message - you need key, nonce pair and the tag - to make it human readable, set `to_text=True`
# - otherwise the output will be of the type `binary`
message == ca.decrypt(ciphertext, tag, key, nonce, to_text=True)

```

# Use `pycryptaes` to save credentials in encrypted form (and not plain text)

```
from pycryptaes import AES
    
# initialize a AES Object for encryption and decryption with the AES method.     
ca = AES()

# cd first to the pycryptaes package or do `$ mkdir -p _test` where you are!   
ca.generate_key_user_pass("./_test/_key", "./_test/_user", "./_test/_pass")
# you are prompted to enter a username and a password
# they will be saved together with the randomly generated key in the corresponding files whichyou specified.
# let's enter `user123` and `pass123` as username and password.

# to use the files, you read-in the encrypted content of the files
# into a credential object `co` (which is itself a CryptoAES object loaded with the information in the encrypted files)
co = ca.read_key_user_pass("./_test/_key", "./_test/_user", "./_test/_pass")

# the credential object has the property-decoratoraed methods `username` and `password`
# which - when invoked - dynamically decrypt the username and password 
# the nice feature of these property decorator function is - that no variable in the script
# holds the actual username and password in plain text format.
co.username  ## "user123"
co.password  ## "pass123"
# so if credentials are required, we can overgive them to the functions which need them
# by calling these property methods as pseudo properties.
```
