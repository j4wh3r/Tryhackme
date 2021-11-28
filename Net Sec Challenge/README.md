
![Capture](https://user-images.githubusercontent.com/90579213/143771346-c45c772a-8a31-4727-998a-9d9c82409513.JPG)



### before answering the questions and instead doing many nmap scans(specific options for each task), i'll provide you with an all in one nmap scan, but you may fall asleep waiting for all requested output to answer the quests xD, so i'll not do that:

`nmap -sV -p- $ip -T4 -v -oN nmap.scan` 

`$ip -> change it to the ip address of the machine`

#### 1.What is the highest port number being open less than 10,000?

`nmap $ip `

![8080](https://user-images.githubusercontent.com/90579213/143772018-836093c4-c92c-4319-aae1-29b03d6525c2.JPG)


#### 2.There is an open port outside the common 1000 ports; it is above 10,000. What is it?

if we don't provide the port range to nmap, this tool will scan only for the common 1000 ports, so in this case, the port is outside this list, sooo we i'll scan for all 65535 ports using the `-p-` option 

`nmap -p- $ip -v -T4`


15 minutes later...

![10021](https://user-images.githubusercontent.com/90579213/143772394-16316115-a5bf-4f47-8442-3ca052458527.JPG)

![Capture](https://user-images.githubusercontent.com/90579213/143772510-c92feeb4-1b23-4965-98a1-bbeef8142d38.JPG)



#### 3.How many TCP ports are open?

using the same previus scan result, we found that we have 6 open tcp ports

![image](https://user-images.githubusercontent.com/90579213/143772588-e0bd05f2-bac2-45f6-aa1d-3c209ecea286.png)



#### 4.What is the flag hidden in the HTTP server header?

here we need to use telnet to connect to the specified ports(we can use netcat too..)

`telnet $ip 80`

after that, i'll make a custom http request header and we'll find the flag in the response header:

![image](https://user-images.githubusercontent.com/90579213/143772895-f81eb88e-83f4-4212-9b08-fe6967db1740.png)


![image](https://user-images.githubusercontent.com/90579213/143772931-5642b914-0af6-4be5-9d16-0e42ad325078.png)



#### 5.What is the flag hidden in the SSH server header?

`telnet $ip 22`

![image](https://user-images.githubusercontent.com/90579213/143772976-bf4af50c-e34f-49d5-ab6c-719d2a24c512.png)


#### 6.We have an FTP server listening on a nonstandard port. What is the version of the FTP server? (we found already a nonstandard port '10021' but we don't know what is it exactly)

`nmap -sV -p 10021 $ip`

![image](https://user-images.githubusercontent.com/90579213/143773040-363097b3-6c18-46f0-85af-85b08c17e601.png)


#### 7.We learned two usernames using social engineering: eddie and quinn. What is the flag hidden in one of these two account files and accessible via FTP?

we'll brute force the user eddie's ftp login using hydra tool, but hey don't forget, we're not using the default ftp port number!

`hydra -l eddie -P /usr/share/wordlists/rockyou.txt  ftp://10.10.223.133  -vV  -I -s 10021`

and we found his password: jordan, then we need to login with those credentials:

`ftp $ip 10021`

`dir` -> to list the content of the directory, but we found noth!!

lets move to the next user, quinn, and by doing the same steps we found an interesting file called 'ftp_flag.txt'

`get ftp_flag.txt` : to get the file and see its content(type 'exit' in the ftp prompt and the 'cat ftp_flag.txt')

![image](https://user-images.githubusercontent.com/90579213/143773689-c4781a1a-0e13-4ae4-91fb-17ff1799c2e4.png)


#### 8.Browsing to http://10.10.223.133:8080 displays a small challenge that will give you a flag once you solve it. What is the flag?

so here the challenge is to make an undetectable nmap scan, honestly i've tried many options, and the right one is the TCP NULL scan

`nmap -sN $ip`

![image](https://user-images.githubusercontent.com/90579213/143773839-85eefe05-2c57-43a4-99eb-b9c53f19e2af.png)














