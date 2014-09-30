ogre
----

throw connections at a host to try to figure out if services are listening.

optionally brute force your way in.

####how to use

Type `./ogre --help` to get usage information:
	
	usage: ogre [-h] [--scan] [--crack] [--version] [--verbose] --host HOST
	            [--ports [PORTS [PORTS ...]]]
	            [--scan_receive_bytes SCAN_RECEIVE_BYTES]
	            [--timeout TIMEOUT SECONDS] [--log_file /PATH/TO/LOGFILE]
	            [--debug] [--show_failures] [--username USERNAME]
	            [--password PASSWORD] [--password_file PASSWORD_FILE_NAME]
	            [--stop_on_success]
	            
	            

Or if you prefer, check out the following:

#####--scan

Specify a `--host` and a comma separated set of ports:

	./ogre --host tristanfisher.com --scan --port 22,80,443

Or a range of ports:

	./ogre --host tristanfisher.com --scan --port 22-1024
	
Or just one port:

	./ogre --host tristanfisher.com --scan --port 22

#####--crack

Specify a password dictionary (*ogre* can read gzip or plaintext files, no need to decompress first):

	./ogre --ip 127.0.0.1 --port 22 --username 'tristan' --crack --password_file password_dictionary.txt.gz --timeout 1


Or, if you know the password, specify it:

	./ogre --ip 127.0.0.1 --port 22 --username 'tristan' --crack --password 'password'

#####--scan --crack

You can chain the options to acheive a sort of "autopilot" mode in which ogre will attempt to brute force ports that look like SSH from a scan:

./ogre --host 127.0.0.1 --port 2-1024 --timeout .5 --password_file password_dictionary.txt.gz --username 'tristan' --scan --crack

####why?

because i found a computer in my closet, i don't have a usb keyboard, and i forgot the password i had set.

to complicate the matter, i think i set up iptables well enough that it's filtering connections from nmap.  hooray.


####other notes

###### malicious use 

depending on where you live, it's probably illegal to use this tool to break your way into a server you don't own.  regardless of where you live, it's probably immoral.  i disclaim any liability for your actions.

###### password_dictionary.txt

the example password file is a concatenation of other passwords dictionaries.  i didn't preserve relative frequency on merge of files, but it is a uniq series of files redirected onto each other.

e.g. 

	for i in ls; do cat $i >> password.txt; done | awk ' !x[$0]++' password.txt > uniq_password.txt

for what it's worth, the first file that was redirected was 10k passwords in order of frequency.  

please don't open pull requests or issues about the example dictionary, as it's a sample file that is already abrasively large for git.

if you do know of any publically-available, open-source lists, i am willing to add links to the readme.

###### securing against dictionary attacks

while by no means an exhaustive list, the following will help:

for all traffic: 

- consider tracking and limiting malicious users from making connections (e.g. fail2ban, session database)

- lock accounts and notify users if N-login attempts have failed

- set your firewall to only allow administrative traffic from some networks (if you always ssh from 74.1.x.x/16...)


for ssh (configured via sshd_config): 

- if you can, disallow password-based SSH access that does not also require a key-exchange (`PasswordAuthentication no`)
 
- don't allow root to log in over ssh (username is predictable -- not as big of a deal if you can disallow password-only auth `PermitRootLogin no`)

- keep in mind that not all accounts on the system need SSH access (`AllowUsers xUser yUser zUser`)

 
###### license

this software is licensed under the [gnu affero gpl v3](LICENSE)