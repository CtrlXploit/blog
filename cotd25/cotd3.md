# COTD 3

First we extract the chall.zip and we get chall.jpg.
For any forensics/steganography challenge in a CTF, it is quite useful to run the following commands -
```bash
file chall.jpg 
chall.jpg: JPEG image data, JFIF standard 1.01, resolution (DPI), density 96x96, segment length 16, comment: "flag{check_the_strings}", baseline, precision 8, 543x309, components 3

exiftool chall.jpg
Comment : flag{check_the_strings}
```
This is a hint that tells us to use the command `strings`.

```bash
strings chall.jpg
pass: g00dp4ssw0rd
```

Now, we do `binwalk` because of the hint "taking a walk down the binary path" -
```bash
binwalk chall.jpg
DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             JPEG image data, JFIF standard 1.01
5033          0x13A9          JPEG image data, JFIF standard 1.01
```

As we can see, there is another image appended to the original chall.jpg.

We can extract it using `binwalk` only -

```bash
binwalk --dd='.*' chall.jpg
```

Now there is a folder in our current working directory, that contains the appended image.

Now the last hint is suggesting the use of `steghide` command and we already have the password for it.

```bash
steghide extract -sf _chall.jpg.extracted/13A9 -p g00dp4ssw0rd
wrote extracted data to "hidden.zip".
```
After unzipping the hidden.zip, we get flag.txt which contains our flag.

`cotd{f0rens1cs_1s_c00l}`
