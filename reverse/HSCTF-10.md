# Reversing challenges in HSCTF 10
- [back-to-basics](#back-to-basics)
- [brain-hurt](#brain-hurt)
- [mystery-methods](#mystery-methods)

-----

## back-to-basics

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/6e036a6d-dbc8-4d5d-a431-4d13db6832b8)

```java
import java.util.Scanner;

class ReverseEngineeringChallenge {
    public static void main(String args[]) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter password: ");
        String userInput = scanner.next();
        if (checkPassword(userInput)) {
            System.out.println("Access granted.");
        } else {
            System.out.println("Access denied!");
        }
    }

    public static boolean checkPassword(String password) {
        return password.length() == 20 &&
                password.charAt(0) == 'f' &&
                password.charAt(11) == '_' &&
                password.charAt(1) == 'l' &&
                password.charAt(6) == '0' &&
                password.charAt(3) == 'g' &&
                password.charAt(8) == '1' &&
                password.charAt(4) == '{' &&
                password.charAt(9) == 'n' &&
                password.charAt(7) == 'd' &&
                password.charAt(10) == 'g' &&
                password.charAt(2) == 'a' &&
                password.charAt(12) == 'i' &&
                password.charAt(5) == 'c' &&
                password.charAt(17) == 'r' &&
                password.charAt(14) == '_' &&
                password.charAt(18) == 'd' &&
                password.charAt(16) == '4' &&
                password.charAt(19) == '}' &&
                password.charAt(15) == 'h' &&
                password.charAt(13) == '5';
    }
}
```

Common sense, just put each character in the correct order!

Flag: `flag{c0d1ng_i5_h4rd}`

-----

## brain-hurt

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/e76b5e0b-5ac8-4595-9e42-bdf7733b5090)

```py
def validate_flag(flag):
    encoded_flag = encode_flag(flag)
    expected_flag = 'ZT_YE\\0|akaY.LaLx0,aQR{"C'
    if encoded_flag == expected_flag:
        return True
    else:
        return False

def encode_flag(flag):
    encoded_flag = ""
    for c in flag:
        encoded_char = chr((ord(c) ^ 0xFF) % 95 + 32)
        encoded_flag += encoded_char
    return encoded_flag
```

I wrote a decoder function which reverses it (thanks to @hollowcrust).

```py
def decode_flag(flag):
    decoded_flag = ""
    for c in flag:
        tmp = (ord(c)-32 + 95) ^ 0xFF
        if(tmp > 122) or (tmp < 48):
            tmp = (ord(c)-32 + 190) ^ 0xFF
        decoded_char = chr(tmp)
        decoded_flag += decoded_char
    return decoded_flag
    
>>> "flagd1D_U_g3t_tH15_onE?"
```

Flag: `flag{d1D_U_g3t_tH15_onE?}`

-----

## mystery-methods

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/b37cf63f-c8ca-431d-b0e0-a19f97ae1154)

```java
import java.util.Base64;
import java.util.Scanner;

public class mysteryMethods{
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Flag: ");
        String userInput = scanner.nextLine();
        String encryptedInput = encryptInput(userInput);

        if (checkFlag(encryptedInput)) {
            System.out.println("Correct flag! Congratulations!");
        } else {
            System.out.println("Incorrect flag! Please try again.");
        }
    }

    public static String encryptInput(String input) {
        String flag = input;
        flag = unknown2(flag, 345345345);
        flag = unknown1(flag);
        flag = unknown2(flag, 00000);
        flag = unknown(flag, 25);
        return flag;
    }

    public static boolean checkFlag(String encryptedInput) {
        return encryptedInput.equals("OS1QYj9VaEolaDgTSTXxSWj5Uj5JNVwRUT4vX290L1ondF1z");
    }

    public static String unknown(String input, int something) {
        StringBuilder result = new StringBuilder();
        for (char c : input.toCharArray()) {
            if (Character.isLetter(c)) {
                char base = Character.isUpperCase(c) ? 'A' : 'a';
                int offset = (c - base + something) % 26;
                if (offset < 0) {
                    offset += 26;
                }
                c = (char) (base + offset);
            }
            result.append(c);
        }
        return result.toString();
    }

    public static String unknown1(String xyz) {
        return new StringBuilder(xyz).reverse().toString();
    }

    public static String unknown2(String xyz, int integer) {
        return Base64.getEncoder().encodeToString(xyz.getBytes());
    }
}
```

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/6234214b-bfb4-447d-9679-1802138b66bb)

`unknown` looks like caesar cipher shift.

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/fb7f5521-6354-4536-89dd-7c2f5f51ff29)

`unknown1` looks like it reverses the string

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/bd278258-59cc-41a9-aaaa-03bd5aff8a7a)

`unknown2` looks like a base64 encoder

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/6d821d2e-7c9b-47d9-b646-45a34c20b4d0)

Reverse the encoded string using [cyberchef](https://gchq.github.io/CyberChef/#recipe=ROT13(true,true,false,27)From_Base64('A-Za-z0-9%2B/%3D',true,false)Reverse('Character')From_Base64('A-Za-z0-9%2B/%3D',true,false)&input=T1MxUVlqOVZhRW9sYURnVFNUWHhTV2o1VWo1Sk5Wd1JVVDR2WDI5MEwxb25kRjF6){:target="_blank" rel="noopener"}

![image](https://github.com/jeromepalayoor/ctf-archive-hub/assets/63996033/1c002b36-f732-470a-91ef-bef87f6ce42f)

Flag: `flag{hsCTF_I5_r3aLLy_fUN}`
