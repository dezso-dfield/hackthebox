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

```

### Brute-forcing

```shell
ffuf -w /opt/useful/seclists/Usernames/xato-net-10-million-usernames.txt -u http://94.237.54.176:58188/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=FUZZ&password=123" -fr "Unknown user."

ffuf -w ./custom_wordlist.txt -u http://172.17.0.2/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=admin&password=FUZZ" -fr "Invalid username"
```

