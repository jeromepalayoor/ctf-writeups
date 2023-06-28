# OSINT challenges from aupCTF
- [Git](#git)
- [Ash](#ash)
- [Records](#records)
- [Wisdom](#wisdom)

4/7 solved

-----

## Git

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/7fa72b3f-0a86-452f-81d2-f9be09f3adfa)

Link: [https://github.com/asadse7en/fypCTF](https://github.com/asadse7en/fypCTF)

The flag is in the tags:

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/61b8a2a0-fcc7-4d79-ac30-282b72d3f079)

Flag: `aupCTF{5t4r-th3-r3p0}`

-----

## Ash

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/4a9d672a-997c-4560-b70e-5d02c9f46fb0)

After looking up his name Ash in Google with Combo Breaker Champion I find an esports player and found that he won 19 tournaments.

Flag: `aupCTF{19}`

-----

## Records

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/e901ac20-2d22-4fe8-a959-50946f538fc5)

Link: [https://iasad.me/](https://iasad.me/)

I use a [DNS records lookup service](https://dnschecker.org/all-dns-records-of-domain.php?query=iasad.me&rtype=TXT&dns=google) to find the TXT record.

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/79ff5a65-5e76-449e-bc64-d0f27445d0e8)

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/d775d4b4-5ef0-4ce2-a9a4-c6b6ff3cb0cd)

Flag: `aupCTF{st0p-l00k1ing-my-dns-r3c0rds}`

-----

## Wisdom

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/e5081041-ab11-4fce-aea3-745700f2504c)

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/0cb32a2a-3eb7-4738-96a9-cdb9d3ca6d37)

Putting it into Google image search, I find that it is [A university library](http://www.aup.edu.pk/library/Gallery/images).
I found [this part of their website](http://www.aup.edu.pk/library/Page/v/Collection) which shows the number of books, which is 113600.

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/fc99d5d2-4b61-424d-a7b3-04672a2c4d5e)

However, there is no number of journals. I went to web archive and found [this](https://web.archive.org/web/20191120155521/http://www.aup.edu.pk/library/Page/v/Collection).
Number of journals was: 102+13+11490 = 11605

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/62544ff3-6902-42f0-afc8-a09a1e20de92)

Flag: `aupCTF{113600-11605}`
