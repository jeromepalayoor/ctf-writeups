# Crypto challenges for TJCTF 2023
- [baby-rsa](#baby-rsa)
- [ezdlp](#ezdlp)

-----

## baby-rsa

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/fb862b6a-a1dc-40d7-9518-41d08844ac35)

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/d0da0bc2-3dc8-4a3e-a873-3942c6317f49)

It seems that the ciphertext is comparitively way shorter than the modulus and since e is just 3, taking the [cube root](https://www.dcode.fr/cube-root) of the ciphertext gives the message since the modulus is useless: `m^e mod n = c => m^e = c`

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/e3130f9d-3e5c-4a58-9347-1548b2c1334e)

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/5e6ccef5-4c07-4962-9c98-00fc69b6a6e5)

Flag: `tjctf{thr33s_4r3_s0_fun_fb23d5ed}`

-----

## ezdlp

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/bd74a4e6-59f0-4c54-992a-19fe618328b6)

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/97e5f8ca-aa91-4d5f-9c80-261c24d14a87)

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/ac505536-a9b5-4e07-b6f9-fb63bfada965)

Flag: `tjctf{26104478854569770948763268629079094351020764258425704346666185171631094713742516526074910325202612575130356252792856014835908436517926646322189289728462011794148513926930343382081388714077889318297349665740061482743137948635476088264751212120906948450722431680198753238856720828205708702161666784517}`
