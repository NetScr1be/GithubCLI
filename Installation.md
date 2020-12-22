As per: https://github.com/cli/cli/blob/trunk/docs/install_linux.md

### Debian

- `sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-key C99B11DEB97541F0`  
    - > Warning: apt-key is deprecated. Manage keyring files in trusted.gpg.d instead (see apt-key(8)).  
Executing: /tmp/apt-key-gpghome.g0eUcKhjmT/gpg.1.sh --keyserver keyserver.ubuntu.com --recv-key C99B11DEB97541F0  
gpg: key C99B11DEB97541F0: public key "Nate Smith <vilmibm@github.com>" imported  
gpg: Total number processed: 1  
gpg:               imported: 1  

After successfully getting the key the installation first fails with:

`sudo apt update`  

> E: The repository 'https://cli.github.com/packages kali-rolling Release' does not have a Release file.  
N: Updating from such a repository can't be done securely, and is therefore disabled by default.  
N: See apt-secure(8) manpage for repository creation and user configuration details.  

The workaround is to add '[trusted=yes]' to the sources.list entry.  

> deb [trusted=yes] https://cli.github.com/packages kali-rolling main  

NOW it fails with;

> Err:6 https://cli.github.com/packages kali-rolling/main amd64 Packages  
  404  Not Found [IP: 185.199.111.153 443]  

> E: Failed to fetch https://cli.github.com/packages/dists/kali-rolling/main/binary-amd64/Packages  
404  Not Found [IP: 185.199.111.153 443]  
E: Some index files failed to download. They have been ignored, or old ones used instead.  

`$ nslookup 185.199.111.153`  
> ** server can't find 153.111.199.185.in-addr.arpa: NXDOMAIN  <<< **N**on-e**X**istent **D**omain  

Even though;

`$ dig 185.199.111.153`

> ; <<>> DiG 9.16.8-Debian <<>> 185.199.111.153  
;; global options: +cmd  
;; Got answer:  
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 56098  
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1  

> ;; OPT PSEUDOSECTION:  
; EDNS: version: 0, flags:; udp: 4096  
;; QUESTION SECTION:  
;185.199.111.153.               IN      A  

> ;; ANSWER SECTION: 
185.199.111.153.        0       IN      A       185.199.111.153  

> ;; Query time: 0 msec  
;; SERVER: 10.153.97.105#53(10.153.97.105)  
;; WHEN: Tue Dec 22 13:07:00 EST 2020  
;; MSG SIZE  rcvd: 60  

Don't try to get cute and shotgun a solution by doing;

> deb [trusted=yes] https://cli.github.com/packages ***debian*** main  

It doesn't work any better.  

Following the [suggestion of @subsr97](https://github.com/cli/cli/issues/1798#issuecomment-716043885) to DL the deb manually and install via dpkg seems to work;  

`$ ls -l ~/Downloads/gh_1.1.0_linux_amd64.deb`  
> \-rw-r--r-- 1 patrick patrick 5810716 Dec 22 13:26 /home/patrick/Downloads/gh_1.1.0_linux_amd64.deb  

`sudo dpkg -i /home/patrick/Downloads/gh_1.1.0_linux_amd64.deb`  
> Selecting previously unselected package gh.  
(Reading database ... 367352 files and directories currently installed.)  
Preparing to unpack .../gh_1.1.0_linux_amd64.deb ...  
Unpacking gh (1.1.0) ...  
Setting up gh (1.1.0) ...   
Processing triggers for kali-menu (2021.1.0) ...  
Processing triggers for man-db (2.9.3-2) ...  

