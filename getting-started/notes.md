## Getting started 

# SSH

```shell
  ssh <username>@<ip_address>
```


# Netcat

```shell
  netcat <ip_address> <port>
  nc <ip_address> <port>

  //banner grabbing
  nc -nv 10.129.42.253 21
```

# Tmux

```shell
  tmux -y
```

# Vim

```shell
  vim /etc/hosts
```

# Nmap 

```shell
  nmap <ip>
  nmap -sC -sV -p- <ip>
  nmap -A -p445 <ip>
  nmap --script smb-os-discovery.nse -p445 <ip>
```

# SmbClient 

```shell
  smbclient -L \\\\<ip>
  smbclient -U <username> \\\\<ip>//<share_name>
```

# Nmap 

```shell
  nmap <ip>
  nmap -sC -sV -p- <ip>
```

# SnmpWalk 

```shell
  snmpwalk -v 2c -c public <ip> 1.3.6.1.2.1.1.5.0
  snmpwalk -v 2c -c private  <ip>
```

# Curl

```
  curl -IL <full_url>
```

# GoBuster 

```shell
  gobuster dir -u <url> -w /usr/share/seclists/Discovery/Web-Content/common.txt
  gobuster dns -d <domain> -w /usr/share/SecLists/Discovery/DNS/namelist.txt
```


# Whatweb
```shell
  whatweb <ip>
  whatweb --no-errors <ip>
```

# SearchSploit
```shell
  searchsploit <name_of_service>
```

# Msfconsole
```shell
  msfconsole
  search exploit <exploit_name>
  use <exploit_path>
  set RHOSTS=<ip> set LHOST=<local_ip/network_interface>
  check
  exploit
```
