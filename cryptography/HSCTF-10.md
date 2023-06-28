# Cryptography challenges in HSCTF 10
- [double-trouble](#double-trouble)
- [really-small-algorithm](#really-small-algorithm)
- [cupcakes](#cupcakes)
- [trios](#trios)

-----

## double-trouble

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/a1977ff3-2782-45a5-b11b-e61c2a16a768)

There was a given pdf file with the challenge.

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/9ab13d24-f939-434b-89bd-0055ec561e6b)

Thanks to the hint, it is caesar cipher.

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/e89c26bd-e20d-46c3-8612-8470ec2bb6a9)

So decrypting the small part.

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/3c8a160c-8aff-438e-9fd7-6737259d0e01)

Flag: `flag{CaesarCiphersAreCool}`

-----

## really-small-algorithm

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/7521e1df-c4eb-466a-a5d4-cc534eddef7f)

Putting the numbers into a [decoder](https://www.dcode.fr/rsa-cipher){:target="_blank" rel="noopener"} gives flag.

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/c2a7ce90-5314-4b94-b2f0-f4798b2746e5)

Flag: `flag{bigger_is_better}`

-----

## cupcakes

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/f1cb21ba-60cf-404d-afb6-02cf9e5ac2fa)

I tried vigenere cipher which needed a key.

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/c7341e2b-509d-4247-a829-63bfdda2802d)

Flag: `flag{instantbatter}`

-----

## trios

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/e4373924-0df1-4e4d-ac26-0c544e50ffe7)

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/5820117f-ad55-426b-addb-02486290ebd9)

Error in data.txt as it is not a multiple of 3:

```
IIqrBRz → IqrBrz
S4mLtKOIqr2stRbcQHJAPR2svphjHu0 → 4mLtKOIqr2stRbcQHJAPR2svphjHu0
```

Looks like a substitution cipher but each letter is represented by a random group of 3 characters. So, just assign each group of 3 into a letter and use [quipqiup](https://quipqiup.com/){:target="_blank" rel="noopener"}.

```py
g = ""
bad = ",.\n {}"
with open("data.txt", "r") as f:
    for c in f.read():
        if c not in bad:
            g += c

abc = [chr(i) for i in range(97, 123)]
g = [g[i:i+3] for i in range(0, len(g), 3)]
# split into groups of 3
found = []
s = ""
i = 0
for k in g:
    if k not in found:
        found.append(k)
        s += abc[i]
        i += 1
    else:
        d = found.index(k)
        s += abc[d]
# assign 1 letter for each group
print(s)

>>> "abcdebefgchijhekclijmjbnechehoplfieqremmcdesakethjksimbchducdesmijashqcqsaaepehifbcshievijaigemcrebchducdebjhdehjudgijasbbjhemgeeijpmjchqigehkeojuhiigejoouppehoemjaecogbeiiepimsbbedcbijjkhwumijhedushecfsdshksixepbchq"
```

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/64abcd78-ab2c-498a-a821-4019b2d22265)

Flag:`flag{elephant}`
