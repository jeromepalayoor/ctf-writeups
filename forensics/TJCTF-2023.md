# Forensics challenges in TJCTF 2023
- [beep-boop-robot](#beep-boop-robot)
- [nothing-to-see](#nothing-to-see)
- [neofeudalism](#neofeudalism)

-----

## beep-boop-robot

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/cef4d75d-218b-4d47-bbc8-4224c7ec6c65)

The given audio file was just a audio morse code which can put inside [a online decoder](https://databorder.com/transfer/morse-sound-receiver/){:target="_blank" rel="noopener"} to get the flag.

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/9e28e79b-06d9-442e-bddb-807b3ef4bcba)

Flag: `tjctf{thisisallonewordlmao}`

-----

## nothing-to-see

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/00375090-1376-41c2-8eef-64809d249eae)

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/aae00eec-0e82-453b-9247-4d814899c451)

There is a zip file hidden in the image.

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/31b234e2-8586-45ec-a88d-dd318d50bf5b)

It requires a password. Doing strings reveal a possilbe password.

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/53e4cf5c-880d-4fc5-8c26-423be0e3d4c8)

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/0d048078-a9d9-4fbf-8c8d-3b1a5b2df510)

Flag: `tjctf{the_end_is_not_the_end_4c261b91}`

-----

## neofeudalism

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/1d3e8021-fe99-440e-87f1-81f4f6663a96)

I tried binwalk, exiftool and strings on it but nothing shows. Therefore I tried zsteg.

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/fa216898-2e9e-4195-aea6-352b8d06a095)

Flag: `tjctf{feudalism_still_bad_ea31e43b}`
