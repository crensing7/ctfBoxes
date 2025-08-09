# Level 0
- ssh -p \[port] user@host
# Level 1
- cat readme
# Level 2
- cat ./-
# Level 3
- cat spaces\ in\ this\ filename\
# Level 4
- cd inhere
- ls -a
- cat .hidden
# Level 5
- cd inhere
- file ./ *
- cat ./-file07
# Level 6
- cd inhere
- find -type f -size 1033c
# Level 7
- cd inhere
- find -type f -size 33c -exec stat --format='%n %G %U' {} \; 2>/dev/null | grep 'bandit6 bandit7'
# Level 8
- cat data.txt | grep millionth
# Level 9
- cat data.txt | sort | uniq -u
# Level 10
- strings data.txt
# Level 11
- base64 -d data.txt
# Level 12
- rot13 decrypt data.txt using python script
# Level 13
- Use combination of gzip, bzip2, tar extraction to get ascii file
# Level 14
- ssh bandit14@localhost -p 2220 -i sshkey.private
# Level 15
- telnet localhost 30000
# Level 16
- nmap localhost -sV
- openssl s_client localhost:port
- chmod 600 level17
# Level 17
- diff passwords.old passwords.new
# Level 18
- ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme
# Level 19
- cd /etc/bandit_pass
- /home/bandit20-do cat bandit_20
# Level 20
- cat level20 | nc -l localhost 6969
- suconnect 6969
# Level 21
- cd /etc/cron.d
- cat cronjob_bandit22
- cat /usr/bin/cronjob_bandit22.sh
- cat /tmp/\[string]
# Level 22
- cat /tmp/$(echo I am user bandit23 | md5sum | cut -d ' ' -f 1)
# Level 23
- mkdir /tmp/rand
- nano script.sh
	+ cat /etc/bandit_pass/bandit24 > /tmp/rand/password
- chmod 777 /tmp/rand
# Level 24
- Bash script to spam password and all pin combinations
- Maybe use sleep if its lagging
# Level 25
- Minimize window to get into more
- Vim from more, set shell, execute shell
- bandit27-do cat /etc/bandit_pass/bandit27
# Level 26
- Passthrough from previous level
# Level 27
- git clone ssh://bandit27-git@localhosti:2220/home/bandit27-git/repo pass
- cd pass, cat README
# Level 28
- Same as 27 except git checkout the commit with password
# Level 29
- Same as 27 except git checkout remote branch origin/dev into new branch
# Level 30
- Same as 27 except git tag
- git show secret **lol**
# Level 31
- remove .gitignore
- echo 'May I come in?' > key.txt
- git add .
- git commit -m 'bruh'
- git push origin master
# Level 32
- Literally just type '$0'
- Kinda boring way to end things, but I had a lot of fun doing this!

