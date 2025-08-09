## What's the password?
```
// basic reading
```
## ... and again!
```
// basic reading
```
## Newline Troubles
```
Ctrl + D to terminate without newline
```
## Reasoning about files
```
echo -n 'password' > file
```
## Specifying Filenames
```
echo -n 'password' > file
/challenge/runme file
```
## Decoding Practice
```
decode b'[binary string]' into [hex]
echo -ne '\x[hex]\x[hex]\x[hex]\x[hex]\x[hex]' | /challenge/runme
```
## Encoding Practice
```
echo -n '[binary string]' | /challenge/runme
```
## Hex-encoding ASCII
```
echo -n '[hex string]' > file
/challenge/runme file
```
## Nested Encoding
```
quad hex encode password string
```
## Hex-encoding UTF-8
```
hex encode emojis
```
## UTF Mixups
```
echo [hex string] > jiwc
/challenge/runme
```
## Modifying Encoded Data
```
convert correct_password to hex
enter hex backwards
```
## Decoding Base64
```
echo -n '[password]' | base64 -d | /challenge/runme
```
## Encoding Base64
```
echo -n '[hex string]' | base64 | /challenge/runme
```
## Dealing with Obfuscation
```
correct and entered are switched
just change it to stdout.write(correct_password)
exploit | /challenge/runme
```
## Dealing with Obfuscation 2
```
i don't really understand this but i feel like this is a python issue
```
