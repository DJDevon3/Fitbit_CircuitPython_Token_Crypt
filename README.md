# Fitbit_CircuitPython_Token_Crypt
SHA-256 Base64 URL Encoded Tokenizer

# Example using Circuit Python 8.2
### Required Libraries:
- [Adafruit_CircuitPython_hashlib](https://github.com/adafruit/Adafruit_CircuitPython_hashlib)
- [Adafruit_CircuitPython_binascii](https://github.com/adafruit/Adafruit_CircuitPython_binascii)
- [Community Bundle CircuitPython_Base64](https://github.com/jimbobbennett/CircuitPython_Base64/tree/master)


```py
import adafruit_hashlib as hashlib
import circuitpython_base64 as base64
# SHA-256
print("\n--SHA256--")
m = hashlib.sha256()
# Update the hash object with byte_string
m.update(byte_string)
# Obtain the digest, digest size, and block size
print(
    "Msg Digest: {}\nMsg Digest Size: {}\nMsg Block Size: {}".format(
        m.hexdigest(), m.digest_size, m.block_size
    )
)
sha256 = m.digest()
print("SHA256:", sha256)

print("\n--BASE64--")
bytes_to_encode = b" " + str(sha256) + " "
base64_string = base64.encodebytes(bytes_to_encode)
print("Base64 Encoded : ", base64_string)

print("-- Base64 ASCII <-> Binary Conversions --")
data = sha256
print("Original Binary Data: ", data)
# Convert binary data to a line of ASCII characters in base64 coding.
b64_ascii_data = b2a_base64(data)
print("Base64 ASCII Data: ", b64_ascii_data)
stripped_ascii = repr(b64_ascii_data)[2:-4]
replace_chars1 = stripped_ascii.replace('+', '-')
code_verify = replace_chars1.replace('/', '_')
print(f"Code Verifier: {code_verify}")
print("Fitbit Match : -4cf-Mzo_qg9-uq0F4QwWhRh4AjcAqNx7SbYVsdmyQM")
print(f"If Code Verifier = Fitbit Match you've matched their example's required encryption! :) ")
```
### Example Output:
```py
Fitbit Example String: b'01234567890123456789012345678901234567890123456789'

--SHA256--
Msg Digest: fb871ff8cce8fea83dfaeab41784305a1461e008dc02a371ed26d856c766c903
Msg Digest Size: 32
Msg Block Size: 64
SHA256: b'\xfb\x87\x1f\xf8\xcc\xe8\xfe\xa8=\xfa\xea\xb4\x17\x840Z\x14a\xe0\x08\xdc\x02\xa3q\xed&\xd8V\xc7f\xc9\x03'

--BASE64--
Base64 Encoded :  b'IGInXHhmYlx4ODdceDFmXHhmOFx4Y2NceGU4XHhmZVx4YTg9XHhmYVx4ZWFceGI0XHgxN1x4ODQw\nWlx4MTRhXHhlMFx4MDhceGRjXHgwMlx4YTNxXHhlZCZceGQ4Vlx4YzdmXHhjOVx4MDMnIA==\n'
-- Base64 ASCII <-> Binary Conversions --
Original Binary Data:  b'\xfb\x87\x1f\xf8\xcc\xe8\xfe\xa8=\xfa\xea\xb4\x17\x840Z\x14a\xe0\x08\xdc\x02\xa3q\xed&\xd8V\xc7f\xc9\x03'
Base64 ASCII Data:  b'+4cf+Mzo/qg9+uq0F4QwWhRh4AjcAqNx7SbYVsdmyQM=\n'
Code Verifier: -4cf-Mzo_qg9-uq0F4QwWhRh4AjcAqNx7SbYVsdmyQM
Fitbit Match : -4cf-Mzo_qg9-uq0F4QwWhRh4AjcAqNx7SbYVsdmyQM
If Code Verifier = Fitbit Match you've matched their example's required encryption! :)
```
- As of the creation of this file this just one of many many steps Fitbit requires to create an authorized API token.
- This is essentially all just for creating the "verifier" which in plain english means a salt.
- All this to create just the salt.
- To actually create an automated tokenizer a web server or localhost server is required to passback their randomly generated "Authorization Code".
- This cannot be done by a microcontroller alone easily enough to recommend it as an API example.
- I don't want the work I put into making this go to waste so it will live here.
