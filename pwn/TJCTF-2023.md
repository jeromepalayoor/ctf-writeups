# Pwn challenges in TJCTF 2023
- [flip-out](#flip-out)

-----

## flip-out

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/8aa19364-753d-4b92-93af-6a2332112ae8)

Looking through ghridra:

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/632022ac-82bd-4fac-bad7-3eb880846713)

It seems that it reads in a positive integer below 129. So instinctively I tried 128 which got me the flag. However, what it does is that it reads the stack based of the input number and it just so happened that the flag buffer was in the 128th position.

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/7b533b50-596d-40e8-888f-4e8e7bc1fcf1)

Flag: `tjctf{chop-c4st-7bndbji}`
