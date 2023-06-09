# Web challenges in HSCTF 10
- [th3-w3bsite](#th3-w3bsite)
- [an-inaccessible-admin-panel](#an-inaccessible-admin-panel)

-----

## th3-w3bsite

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/c85cfb18-3b56-426a-b8f2-0f21918e639f)

Looking at the webpage shows nothing interesting, looking at source gives flag:

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/d4395078-e656-4048-b379-a5fd79b43601)

Flag: `flag{1434}`

-----

## an-inaccessible-admin-panel

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/7a1baa18-3e01-4d50-822f-c08033809ac8)

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/8813c221-f080-4981-943e-764249254efc)

Looking at source shows something. However, the real authentication is in the login.js

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/bcba515d-2558-4917-b3dd-95456aaf38d0)

```js
window.onload = function() {
    var loginForm = document.getElementById("loginForm");
    loginForm.addEventListener("submit", function(event) {
        event.preventDefault(); 
    
        var username = document.getElementById("username").value;
        var password = document.getElementById("password").value;
    
        function fii(num){
            return num / 2 + fee(num);
        }
        function fee(num){
            return foo(num * 5, square(num));
        }
        function foo(x, y){
            return x*x + y*y + 2*x*y;
        }
        function square(num){
            return num * num;
        }

        var key = [32421672.5, 160022555, 197009354, 184036413, 165791431.5, 110250050, 203747134.5, 106007665.5, 114618486.5, 1401872, 20702532.5, 1401872, 37896374, 133402552.5, 197009354, 197009354, 148937670, 114618486.5, 1401872, 20702532.5, 160022555, 97891284.5, 184036413, 106007665.5, 128504948, 232440576.5, 4648358, 1401872, 58522542.5, 171714872, 190440057.5, 114618486.5, 197009354, 1401872, 55890618, 128504948, 114618486.5, 1401872, 26071270.5, 190440057.5, 197009354, 97891284.5, 101888885, 148937670, 133402552.5, 190440057.5, 128504948, 114618486.5, 110250050, 1401872, 44036535.5, 184036413, 110250050, 114618486.5, 184036413, 4648358, 1401872, 20702532.5, 160022555, 110250050, 1401872, 26071270.5, 210656255, 114618486.5, 184036413, 232440576.5, 197009354, 128504948, 133402552.5, 160022555, 123743427.5, 1401872, 21958629, 114618486.5, 106007665.5, 165791431.5, 154405530.5, 114618486.5, 190440057.5, 1401872, 23271009.5, 128504948, 97891284.5, 165791431.5, 190440057.5, 1572532.5, 1572532.5];

        function validatePassword(password){
            var encryption = password.split('').map(function(char) {
                return char.charCodeAt(0);
            });
            var checker = [];
            for (var i = 0; i < encryption.length; i++) {
                var a = encryption[i];
                var b = fii(a);
                checker.push(b);
            }
            console.log(checker);
            
            if (key.length !== checker.length) {
                return false;
            }
            
            for (var i = 0; i < key.length; i++) {
                if (key[i] !== checker[i]) {
                    return false;
                }
            }
            return true;
        }


        if (username === "Admin" && validatePassword(password)) {
            alert("Login successful. Redirecting to admin panel...");
            window.location.href = "admin_panel.html";
        }
        else if (username === "default" && password === "password123") {
            var websiteNames = ["Google", "YouTube", "Minecraft", "Discord", "Twitter"];
            var websiteURLs = ["https://www.google.com", "https://www.youtube.com", "https://www.minecraft.net", "https://www.discord.com", "https://www.twitter.com"];
            var randomNum = Math.floor(Math.random() * websiteNames.length);
            alert("Login successful. Redirecting to " + websiteNames[randomNum] + "...");
            window.location.href = websiteURLs[randomNum];
        } else {
            alert("Invalid credentials. Please try again.");
        }
    });
  };
  ```
  
So it checks each value of the key as `a`, with `k` being charcodeat of that character, `k^4 + 10k^3 + 25k^2 + 0.5k - a = 0`. So I wrote a script to bruteforce all characters to get the flag.
  
```py
s = "_0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ!\"#$%&'()*+,-./:;<=>?@[\]^`{|} ~"

key = [32421672.5, 160022555, 197009354, 184036413, 165791431.5, 110250050, 203747134.5, 106007665.5, 114618486.5, 1401872, 20702532.5, 1401872, 37896374, 133402552.5, 197009354, 197009354, 148937670, 114618486.5, 1401872, 20702532.5, 160022555, 97891284.5, 184036413, 106007665.5, 128504948, 232440576.5, 4648358, 1401872, 58522542.5, 171714872, 190440057.5, 114618486.5, 197009354, 1401872, 55890618, 128504948, 114618486.5, 1401872, 26071270.5, 190440057.5, 197009354, 97891284.5, 101888885, 148937670, 133402552.5, 190440057.5, 128504948, 114618486.5, 110250050, 1401872, 44036535.5, 184036413, 110250050, 114618486.5, 184036413, 4648358, 1401872, 20702532.5, 160022555, 110250050, 1401872, 26071270.5, 210656255, 114618486.5, 184036413, 232440576.5, 197009354, 128504948, 133402552.5, 160022555, 123743427.5, 1401872, 21958629, 114618486.5, 106007665.5, 165791431.5, 154405530.5, 114618486.5, 190440057.5, 1401872, 23271009.5, 128504948, 97891284.5, 165791431.5, 190440057.5, 1572532.5, 1572532.5]

flag = ""

for k in key:
    for a in s:
        n = ord(a)
        d = n**4 + 10*n**3 + 25*n**2 + 0.5*n
        if d == k:
            flag += a
            break

print(flag)

>>> "Introduce A Little Anarchy, Upset The Established Order, And Everything Becomes Chaos!!"
```

Looking admin_panel.html:

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/57e72bd0-b8d4-4dfa-96e9-c968e2b2df8a)

Flag: `flag{Admin, Introduce A Little Anarchy, Upset The Established Order, And Everything Becomes Chaos!!}`
