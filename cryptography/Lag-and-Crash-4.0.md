# Cryptography challenges in Lag and Crash 4.0
- [Bit Collider](#bit-collider)
- [Viva la Revolution!](#viva-la-revolution)
- [Pattern Recognition](#pattern-recognition)
- [deserving employee](#deserving-employee)
- [Resistance Fighters](#resistance-fighters)

-----

## Bit-Collider

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/387c24e5-58d7-4888-a3f1-7919006cee78)

```py
from Crypto.Util.number import bytes_to_long
from secret import flag

def hash(x):
    return (bytes_to_long(x) ^ 0x1337deadbeef) & 0xffffbfffffff

print("Get a hash collision to grab the flag!")
m1 = bytes.fromhex(input(f"Enter m1: "))[:6]
m2 = bytes.fromhex(input(f"Enter m2: "))[:6]

h1 = hash(m1)
h2 = hash(m2)
if m1 != m2 and h1 == h2:
    print("Congrats!")
    print(flag)
```

I realised inputting `00` and `0000` would result in the same hash without the same original string so I used that to get the flag.

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/d6df0a4f-fa85-4911-93a5-200798fcff04)

Flag: `LNC24{b1twI5e_aNd_i5_jU5t_b1tm4sKiNg_98bd61c32def}`

-----

## Viva-la-Revolution

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/e46026a7-f310-49ce-9837-6dd78b66b253)

```py
import random
from Crypto.Util.number import bytes_to_long, isPrime

VQTR = b"LNC24{answer}"

def X9sm_4sF(zWQ):
    p, q = 0, 0
    while not isPrime(p):
        p = random.getrandbits(zWQ) + random.getrandbits(zWQ // 20)
    while not isPrime(q):
        q = random.getrandbits(zWQ) + random.getrandbits(zWQ // 10)
    return p, q

dL = bytes_to_long(VQTR)
z, Z = X9sm_4sF(2048)
n = z * Z
e = 0x10001
c = pow(dL, e, n)

print(f"{n}")
print(f"{e}")
print(f"{c}")
```

```
n = 709872443186761582125747585668724501268558458558798673014673483766300964836479167241315660053878650421761726639872089885502004902487471946410918420927682586362111137364814638033425428214041019139158018673749256694555341525164012369589067354955298579131735466795918522816127398340465761406719060284098094643289390016311668316687808837563589124091867773655044913003668590954899705366787080923717270827184222673706856184434629431186284270269532605221507485774898673802583974291853116198037970076073697225047098901414637433392658500670740996008799860530032515716031449787089371403485205810795880416920642186451022374989891611943906891139047764042051071647203057520104267427832746020858026150611650447823314079076243582616371718150121483335889885277291312834083234087660399534665835291621232056473843224515909023120834377664505788329527517932160909013410933312572810208043849529655209420055180680775718614088521014772491776654380478948591063486615023605584483338460667397264724871221133652955371027085804223956104532604113969119716485142424996255737376464834315527822566017923598626634438066724763559943441023574575168924010274261376863202598353430010875182947485101076308406061724505065886990350185188453776162319552566614214624361251463
e = 65537
c = 629271983449087386446961777226000321301965870312268398599537506669889858431379374906494296262823402090165227192809111893224606970248911616802028035824987039758150234145646950316322005917474636606496760673729672877002608909114340000560089382147053953497890425960082938638440486160548358091173635652928750306045154085730654646167452781305825773417471803769668239857212901719807563350263208536312605971444393234367833720622959963977050546213067508180350287625419333696057738845791937756157824897000414535803260716698665471511297275350084086399088067700298956804368564280578335182224349924975014694277663995364772869936436502300717488901891536212232968857135985390616783859464598493974870234747547813904771647692801165365159558462391151691581705494934125275439820607134163339059648948867915041128078971358771970872214113376374091958948592191250934718456852476334122838759940448929560767338089319006528956581832032011095329631491681996100670689605557676862157732327246519546078814025082400486986107558240662048291393341276255527289862573558742702810857731050943492472716395524546584287344269852366782811711970063630718331036466657419817862510398563073262391748024443696817977266765498844400643169098599629738131271505532820548939256243247
```

To be fair the challenge was too easy as using [https://www.dcode.fr/rsa-cipher](https://www.dcode.fr/rsa-cipher) solves the RSA instantly.

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/a457d88f-ae43-49f4-a25f-8be4fd1e1e4b)

Flag: `LNC24{pR1m3s_4eva!}`

-----

## Pattern-Recognition

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/ed8bd352-5bcf-4e7c-bb02-f85d7bed822c)

```
The following 3 ciphertexts were generated from the same script and using the same plaintext

1. k× dÒ aÄ $V (\ lç iÛ [À jà 1d qã nâ +\ lÚ `Ç R± qã %Y bÐ X¼ /_ ]Ê P¯ hÚ (X dØ V· 6m fÏ `Ï lÚ oâ |ù
2. dÐ cÑ bÅ 0b &Z oê bÔ ]Â hÞ -` lÞ dØ #T _Í ]Ä T³ pâ -a _Í cÇ /_ dÑ ]¼ kÝ $T dØ U¶ 1h hÑ kÚ lÚ kÞ {ø
3. eÑ kÙ Z½ )[ )] rí fØ `Å mã -` qã má #T kÙ _Æ Q° bÔ .b `Î Y½ /_ hÕ S² pâ *Z sç [¼ 0g YÂ jÙ _Í lß z÷

The plaintext is all in lowercase
The flag is the plaintext converted all to uppercase
(No key is required!)

Let's see how good you are at pattern recognition >:)
```

I immediately saw the pattern as the ordinal number of the second character minus the ordinal number of the first character in each character pair gives the ordinal number of the character in the flag. I quikly wrote a script to decode it.

```py
a = 'k× dÒ aÄ $V (\ lç iÛ [À jà 1d qã nâ +\ lÚ `Ç R± qã %Y bÐ X¼ /_ ]Ê P¯ hÚ (X dØ V· 6m fÏ `Ï lÚ oâ |ù'
b = 'dÐ cÑ bÅ 0b &Z oê bÔ ]Â hÞ -` lÞ dØ #T _Í ]Ä T³ pâ -a _Í cÇ /_ dÑ ]¼ kÝ $T dØ U¶ 1h hÑ kÚ lÚ kÞ {ø'
c = 'eÑ kÙ Z½ )[ )] rí fØ `Å mã -` qã má #T kÙ _Æ Q° bÔ .b `Î Y½ /_ hÕ S² pâ *Z sç [¼ 0g YÂ jÙ _Í lß z÷'

def x(a):
    a = a.split(' ')
    b = []
    for i in a:
        b.append(chr(ord(i[1])-ord(i[0])))

    return ''.join(b)

print(x(a).upper())
print(x(b).upper())
print(x(c).upper())
```

Flag: `LNC24{REV3RT1NG_R4ND0M_R0TA7IONS}`

-----

## deserving-employee

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/f281a15a-1e15-4b17-af26-3125e1ff8548)

challenge.txt:

```
(pls use a monospace font for the challenge, for readability, as this challenge is created and formatted as such)(courier new, consolas, etc)

----------
QUESTION 1
----------
Story:
your boss has fired you for an unknown reason. he keeps these records in an encrypted zip file. you, as a secretly expert cryptographer, has gained access and decided to take the venture to crack the file. he left behind hints on a note with the file

Ciphertext hex:
97 9E 97 9E 97 9E
92 9E 8C 97   8B 97 9A 92   96 91   9E   9D 9E 98
96 91   8B 97 9A   93 90 9E 9B
90 8A 8B   8B 97 9A   9C 90 9B 9A

93 9E 98 A0 9C 8D 9E 8C 97 A0 99 90 8D 9A 89 9A 8D

Details (for nerds):
- this is a substitution cipher. no transposition has occurred
- the cipher has both a block size and key length of 1 hex digit
- the cipher uses the electronic code block (ecb) mode of encryption
- there is no padding and iv. the plaintext's and ciphertext's sizes are the same
- all characters in the plaintext are found on the keyboard

Details (for starters):
- this algorithm works by replacing every character in the original text with a different one only, to form the gibberish
- you do not need to know the algorithm
- every character has been converted to its hex equivalent (in sets of 2 hex digits each)
- if a set of '7A' represents the character 'b', then all sets of '7A' can be replaced by 'b'
- this is a guessing game. you are to guess what a set of 2 hex digits maps to which character
- all characters in the original text are found on the keyboard

Requirement:
decrypt the cipher. then find the secret code (on the last line). the secret code only contains proper words and '_'

Hint:
notice any patterns? the natural language pattern has not been obscured in this cipher. you can create a small map of commonly used character sequences and map it to similar sequences used in English. if you found a suitable character to represent a set of 2 hex digits, it will apply to all of the sets of the same 2 hex digits. to start, the hex characters 'DF','97','9E','A0' has a plaintext equivalent of <a space>,'h','a','_' respectively




----------
QUESTION 2
----------
Story:
your boss is sending a retrenchment package with an instruction to a teller at the bank to transfer $2000 to your account. you intercepted the encrypted message and decide to change it to the highest 4-digit number ($9999) to teach him a lesson

Encrypted hex:
9D A2 BC A0 C1 CF BC 32 68 EF E4 49 52 E7 38 61 E4 46 5C A2 4C 5A E8 FD 12 E8 4E E6 FC 4D F7 08 46 5D A8 55 4D CE

Details (for nerds):
- this ciphertext undergone an XOR cipher with an unknown key
- you are not required to find out the key
- you are not required to decrypt the ciphertext
- there is no padding and iv. the plaintext's and ciphertext's sizes are the same
- the natural language pattern has been obscured very well as the key chosen has a very high key length (same as plaintext length). thus it is not very possible to use that attack
- all characters in the plaintext are found on the keyboard
- you know that the first 4 bytes(9D A2 BC A0) of the cipher has a plaintext equivalent of '2000'
- note: each of the char in '2000' is encrypted separately, not as a whole. Thus, '2000' = '32 30 30 30' [in hex]

Details (for starters):
- you know that the first 4 bytes(9D A2 BC A0) has the original text equivalent of '2000'
- note: each of the '2000' is encrypted separately, not as a whole. Thus, '2000' = '32 30 30 30' [in hex]
- think about this:
data XOR password XOR data
similar to
3 X 5 X 1/3 == 1 X 5 == 5

- by using the same data acting twice in the equation, you are creating an inverse to the data, cancelling out the original data. this then reveals the password, which is '5'
- 2 0 0 0 = 32 30 30 30 [in hex]
- 9 9 9 9 = 39 39 39 39 [in hex]
- use an online XOR tool for the similar example above

Requirement:
you are to change the first 4 bytes only(9D A2 BC A0), to a plaintext equivalent of '9999' through the ciphertext without decryption. the whole message will be the answer

Hint:
- any data that is XORed with itself will result in 0s the length of the data
- any 0s that is XORed with data of the same length will result in the data
- except in this case, the 0s are replaced by the actual portions of the key
- the first byte of the answer is '96'. if you don't get this, try again!
- below is an example diagram for the above explanation:

[data]	01101010		00000000
	XOR			XOR
[key]	11010100	==	11010100	==	11010100 [the original key!]
	XOR
[data]	01101010


---------------------
BREAKING THE PASSWORD
---------------------
congratulations! you solved both questions. now, refer to this section to find the secret password

---------------
----EXAMPLE----

for example, if your answer to first question is:
i_love_lag_and_crash_2024

and if your answer to second question is:
AA AA AA AA C1 CF BC 32 68 EF E4 49 52 E7 38 61 E4 46 5C A2 4C 5A E8 FD 12 E8 4E E6 FC 4D F7 08 46 5D A8 55 4D CE

then you need to concatenate both the answers together, like this (note the spaces!):
i_love_lag_and_crash_2024AA AA AA AA C1 CF BC 32 68 EF E4 49 52 E7 38 61 E4 46 5C A2 4C 5A E8 FD 12 E8 4E E6 FC 4D F7 08 46 5D A8 55 4D CE

run it through a md5 checksum. you should get this(which is the zip file's password):
f3bef903d6c81cef37b0aadfe2455829

--END EXAMPLE--
---------------

you can use the following websites, or any md5 hashing program on your computer to get the checksum:
- https://cyberchef.org/#recipe=MD5()&input=W0pVU1QgUkVQTEFDRSBUSElTIFdJVEggWU9VUiBBTlNXRVIgQU5EIEdFVCBUSEUgT1VUUFVUIEJFTE9XXQ&ienc=20127&oenc=20127

- https://codebeautify.org/md5-hash-generator?input=[JUST%20REPLACE%20THIS%20WITH%20YOUR%20ANSWER%20AND%20GET%20THE%20OUTPUT%20BELOW]
```

This is a puzzle with 2 parts. First part I solved using [https://www.quipqiup.com/](https://www.quipqiup.com/) which helped in the word detection using frequency analysis. First part: `lag_crash_forever`

Second part is XOR 2000 in hex with first 4 bytes getting the first 4 bytes of the key which I XOR with 9999 in hex and replaced that result for the first 4 bytes of the given encoded hex to get: `96 AB B5 A9 C1 CF BC 32 68 EF E4 49 52 E7 38 61 E4 46 5C A2 4C 5A E8 FD 12 E8 4E E6 FC 4D F7 08 46 5D A8 55 4D CE`. Script I used:

```py
c = "9D A2 BC A0"
c = c.split(" ")
c = [int(i, 16) for i in c]
known = "32 30 30 30"
known = known.split(" ")
known = [int(i, 16) for i in known]
for i in range(4):
    c[i] = c[i] ^ known[i]
x = "39 39 39 39"
x = x.split(" ")
x = [int(i, 16) for i in x]
for i in range(4):
    c[i] = c[i] ^ x[i]
for i in c:
    print(hex(i)[2:].upper(), end=" ")
```

Then I md5sum `lag_crash_forever96 AB B5 A9 C1 CF BC 32 68 EF E4 49 52 E7 38 61 E4 46 5C A2 4C 5A E8 FD 12 E8 4E E6 FC 4D F7 08 46 5D A8 55 4D CE` to get `0c019503a833675624b7b868ea6c1839` which is the password for the zip file. In the zip file:

```
why did i fire you?
i found your replacement!

LNC24{i_pwned_my_boss}
```

Flag: `LNC24{i_pwned_my_boss}`

-----

## Resistance-Fighters

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/f69a0399-0911-40ea-9677-abb94e692b14)

```py
from Crypto.Util.number import *

zxyyzx = b"LNC24{flag!}"

pqrs_1 = getPrime(1024)
pqrs_2 = getPrime(1024)
fjkdasl = pqrs_1 * pqrs_2
jkl = (pqrs_1 - 1) * (pqrs_2 - 1)
ufdiaskop = 65537
xzyx = inverse(ufdiaskop, jkl)
xnclkvha = (fjkdasl, xzyx)
fdasfdasf = [(fjkdasl, getPrime(512)) for _ in range(5)]
ezxyyzx = bytes_to_long(zxyyzx)
for x in fdasfdasf:
    ezxyyzx = pow(ezxyyzx, x[1], x[0])

print(f"My private key pair: {xnclkvha}")
print(f"The resistance's public keys: {fdasfdasf}")
print(f"Encrypted message: {ezxyyzx}")
```

Used chatGPT to rename the variables to make it more readable:

```py
from Crypto.Util.number import *

message = b"LNC24{flag!}"

prime_1 = getPrime(1024)
prime_2 = getPrime(1024)

modulus = prime_1 * prime_2
phi = (prime_1 - 1) * (prime_2 - 1)

public_exponent = 65537

private_exponent = inverse(public_exponent, phi)
private_key = (modulus, private_exponent)
public_keys = [(modulus, getPrime(512)) for _ in range(5)]

encrypted_message = bytes_to_long(message)

for public_key in public_keys:
    encrypted_message = pow(encrypted_message, public_key[1], public_key[0])

print(f"My private key pair: {private_key}")
print(f"The resistance's public keys: {public_keys}")
print(f"Encrypted message: {encrypted_message}")
```

I used the n, e and d to get p and q using the algorithm from [here](https://crypto.stackexchange.com/a/11510/108953), it took about 4 different runs until it output the corrrect p and q since it outputs p=1 sometimes

```py
import random
from math import gcd
from Crypto.Util.number import *

modulus = 11662469740220416758274554731829893292764548399752595026883254037619728377036866329964118325842123999166726481368698310416207040969275735997153780656023663777915067417635940067290018297682184926229385600499311881990981592227301880699854802525580443256199250660087945557731597993590001816631977745961903133222802507479353329089739966060066421077360413446857911528790514230131354519975285843427848066346937605596389334567341355314695461406272051353291606911005651484925566882499233095236192736624998343054256852350721842113215505383491277680291967544085277634690081716358579999967114020874441850936269255692719356240959
d = 2715554392721112649820249709442363422915101524028023866064031869235348811107963138307405673930006137407636085046406399697137791555779906485139183862717565791094861357601422790589219512987017134965903600464157641014730291245992747600283569381271000565933755971023117463585214235961112466573141590298284050429172191201004314571399356681054138205959806322768268270713702304016625686067735709834304468371400915158296181944395897554385284034850412479965091231846890805063225375785929222145417180201607069470811763536581976716414475142627354817538782926634168355493485538826143652199791154893513809633767817638110183613473
e1 = 65537

list_e = [12198414081283325651093858337945937870726015515693821217139203071862226378373088290526843085558058773885598685053072672409427788464708692820638455371427557,
          12518437855122426602228773606106930462791551070982771730615065514938879529560491807682709819531824636662209964795031096194901241258547844344336391792452351,
          8837085328502810057743811925761568995073265711835540464279743221583784520410968080389219302132539854490989609787910004454673811861008236816916140896467889,
          12104420783101630768899712255315982964673837455735753225430973286804347393831472963943867663171797514980749204611896821519318145698123232798158278713563737,
          7692096159664623052440431586762039808421829529254731005279647000696610750198349607227575609838050879026572584841551833407098609632669330295184935632473369]

c = 2204611413532595234932163517501823165954643129031788640195381425574379641838490949229118023964812194738568123508172524818926057059648511530362765428737776421598340956050912574912303965296475909936070863060263305995399658184111217572427215728713044607737391326172538205499459786229612418787235253647331265909067320430104023655823261877918528667439338180288022440384937742650643119749775591868417332925400462363618076500240647074277521854262652679502093303541534385285625930285687446586649753550872417369028638732744350322354429391594697329714173646010225742387737504354807704407848187669455357141905147763322300207850

f = e1 * d - 1
# write f = 2^s * t
t = f
s = 0
while t % 2 == 0:
    t = t // 2
    s += 1

a = random.randint(2, modulus - 1)
while True:
    if pow(a, t, modulus) != 1:
        break
    a = random.randint(2, modulus - 1)

# write a^t = 1 + 2^s * b
b = pow(a, t, modulus)
if b == 1 or b == -1:
    print("fail")
    exit()

# find p and q
p = gcd(b - 1, modulus)un
q = modulus // p
print(p)
print(q)

# p = 117786214627767885763307181434309985065630636997552228326356991302577419312649524722293853277427253531907520001936040286806009774243573406010447894139685544709446338354561259593653921278482083304040981272697036212635144584158637859140178563807257470947489443481960578654453030736116028522370540038561013183501
# q = 99013876768826992587429599061661839084571343591421052904268123603508582456791284930271647954405386354831953746933890517945260295724973183226625330001635488615294760862729277861136080164817536820426189105746102881992191898784215661450660758598892279471520073790841142530373118692964450069977256640473736014459
```

Wrote a script to decrypt the different public key pairs

```py
from Crypto.Util.number import *

modulus = 11662469740220416758274554731829893292764548399752595026883254037619728377036866329964118325842123999166726481368698310416207040969275735997153780656023663777915067417635940067290018297682184926229385600499311881990981592227301880699854802525580443256199250660087945557731597993590001816631977745961903133222802507479353329089739966060066421077360413446857911528790514230131354519975285843427848066346937605596389334567341355314695461406272051353291606911005651484925566882499233095236192736624998343054256852350721842113215505383491277680291967544085277634690081716358579999967114020874441850936269255692719356240959
list_e = [12198414081283325651093858337945937870726015515693821217139203071862226378373088290526843085558058773885598685053072672409427788464708692820638455371427557,
          12518437855122426602228773606106930462791551070982771730615065514938879529560491807682709819531824636662209964795031096194901241258547844344336391792452351,
          8837085328502810057743811925761568995073265711835540464279743221583784520410968080389219302132539854490989609787910004454673811861008236816916140896467889,
          12104420783101630768899712255315982964673837455735753225430973286804347393831472963943867663171797514980749204611896821519318145698123232798158278713563737,
          7692096159664623052440431586762039808421829529254731005279647000696610750198349607227575609838050879026572584841551833407098609632669330295184935632473369]
c = 2204611413532595234932163517501823165954643129031788640195381425574379641838490949229118023964812194738568123508172524818926057059648511530362765428737776421598340956050912574912303965296475909936070863060263305995399658184111217572427215728713044607737391326172538205499459786229612418787235253647331265909067320430104023655823261877918528667439338180288022440384937742650643119749775591868417332925400462363618076500240647074277521854262652679502093303541534385285625930285687446586649753550872417369028638732744350322354429391594697329714173646010225742387737504354807704407848187669455357141905147763322300207850
p = 117786214627767885763307181434309985065630636997552228326356991302577419312649524722293853277427253531907520001936040286806009774243573406010447894139685544709446338354561259593653921278482083304040981272697036212635144584158637859140178563807257470947489443481960578654453030736116028522370540038561013183501
q = 99013876768826992587429599061661839084571343591421052904268123603508582456791284930271647954405386354831953746933890517945260295724973183226625330001635488615294760862729277861136080164817536820426189105746102881992191898784215661450660758598892279471520073790841142530373118692964450069977256640473736014459

for e in list_e[::-1]:
    phi = (p - 1) * (q - 1)
    d = inverse(e, phi)
    c = pow(c, d, modulus)

print(long_to_bytes(c))
```

Flag: `LNC24{1_n33d_m0r3_buLL3ts}`
