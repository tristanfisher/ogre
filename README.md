ogre
----

throw connections at a host to try to figure out if services are listening.

optionally brute force your way in.


####why?

because i found a computer in my closet, i don't have a usb keyboard, and i forgot the password i had set.

to complicate the matter, i think i set up iptables well enough that it's filtering connections from nmap.  hooray.


####other notes

###### malicious use 

depending on where you live, it's probably illegal to use this tool to break your way into a server you don't own.  regardless of where you live, it's probably immoral. 

###### password_dictionary.txt

the example password file is a concatenation of other passwords dictionaries.  i didn't preserve relative frequency on merge of files, but it is a uniq series of files redirected onto each other.

e.g. 

	for i in ls; do cat $i >> password.txt; done | awk ' !x[$0]++' password.txt > uniq_password.txt

for what it's worth, the first file that was redirected was 10k passwords in order of frequency.  

please don't open pull requests or issues about the example dictionary, as it's a sample file that is already abrasively large for git.

if you do know of any publically-available, open-source lists, i am willing to add links to the readme. 