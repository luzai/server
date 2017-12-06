# Sever 84

Only user `amax` is allowed to use `sudo`
If need to use, contact 3140102282@zju.edu.cn.

## Connect to Internet

Here provide a method to share Internet of your laptop to server.

### Prerequisite:

- openssh-client
  - windows system can try:
    - cygwin and install openssh-client
    - linux subsystem and install openssh-client
    - [Potentially work] some gui manipulations in putty, I do not find the way. Tell me if you are familiar with putty.

### Forward Dynamic Port

In the most case, this works:

``` bash
ssh -gfNTD 1080 username@serverip
proxychains curl ip.gs
```

If 1080 is being used, you can

- use existed 1080
- Start a new port, as below

``` bash
cp /etc/proxychains.conf ./
vim ./proxychains.conf # and modify 1080 to 1081
ssh -gfNTD 1081 username@serverip
proxychains curl ip.gs
```

### Post Work

For convenience, you can add this to `.bashrc` or other `rc` files you are using.

``` bash
alias git='proxychains git'
alias pip='proxychains pip2'
alias pip2='proxychains pip2'
alias pip3='proxychains pip3'
alias wget='proxychains wget -c '
alias conda='proxychains conda'
```
