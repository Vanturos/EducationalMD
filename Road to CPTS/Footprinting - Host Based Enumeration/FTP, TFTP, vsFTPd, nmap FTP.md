###### **FTP - File Transfer Protocol** 
один из старейших протоколов. Работает на application уровне стэка **TCP/IP**. В FTP соединении открывается 2 канала. Сначала, клиент и сервер настраивают контроль над каналами через **TCP port 21**, и клиент отправляет команды на сервер, в ответ получая коды статуса. Затем оба участника соединения могут передавать данные через **20 порт TCP**.
20 порт используется только для передачи данных и протокол следит за ошибками во время этого процесса.
Есть **active и passive** вариации FTP. В активном варианте клиент устанавливает соединение, как описано выше, через **TCP-порт 21** и, таким образом, сообщает серверу, через какой клиентский порт сервер может передавать свои ответы. Однако, если клиент защищен брандмауэром, сервер не может ответить, поскольку все внешние подключения заблокированы. Для этого был разработан пассивный режим. Здесь сервер объявляет порт, через который клиент может установить канал передачи данных. Поскольку клиент инициирует подключение таким способом, брандмауэр не блокирует передачу.
FTP - **clear-text** протокол = его можно перехватывать. Также есть вероятность присутствия на сервере **anonymous FTP** доступа.

###### **Trivial File Transfer Protocol (TFTP)**
 проще чем FTP и позволяет передавать данные, но **не предоставляет** возможность аутентификации клиента. TFTP использует **UDP** в отличие от FTP, что делает его отчасти ненадежным.

###### **Команды TFTP ниже:**

|**Commands**|**Description**|
|---|---|
|`connect`|Sets the remote host, and optionally the port, for file transfers.|
|`get`|Transfers a file or set of files from the remote host to the local host.|
|`put`|Transfers a file or set of files from the local host onto the remote host.|
|`quit`|Exits tftp.|
|`status`|Shows the current status of tftp, including the current transfer mode (ascii or binary), connection status, time-out value, and so on.|
|`verbose`|Turns verbose mode, which displays additional information during file transfer, on or off.|

###### **Default configuration:**
Один из наиболее используемых серверов FTP для UNIX это **vsFTPd**.
Его стандартную конфигурацию можно найти по пути **/etc/vsftpd.conf**.
Для его установки достаточно **sudo apt install vsftpd**.
Для просмотра конфигурационных файлов достаточно:
**cat /etc/vsftpd.conf | grep -v "#"**

Сами параметры и их описания ниже:

|**Setting**|**Description**|
|---|---|
|`listen=NO`|Run from inetd or as a standalone daemon?|
|`listen_ipv6=YES`|Listen on IPv6 ?|
|`anonymous_enable=NO`|Enable Anonymous access?|
|`local_enable=YES`|Allow local users to login?|
|`dirmessage_enable=YES`|Display active directory messages when users go into certain directories?|
|`use_localtime=YES`|Use local time?|
|`xferlog_enable=YES`|Activate logging of uploads/downloads?|
|`connect_from_port_20=YES`|Connect from port 20?|
|`secure_chroot_dir=/var/run/vsftpd/empty`|Name of an empty directory|
|`pam_service_name=vsftpd`|This string is the name of the PAM service vsftpd will use.|
|`rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem`|The last three options specify the location of the RSA certificate to use for SSL encrypted connections.|
|`rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key`||
|`ssl_enable=NO`|
Кроме того, существует файл под названием **/etc/ftpusers**, на который нам также следует обратить внимание, поскольку этот файл используется для отказа определенным пользователям в доступе к FTP-сервису. 

###### **Dangerous Settings**

Существует множество различных настроек, связанных с безопасностью, которые мы можем настроить на каждом **FTP**-сервере. Они могут использоваться для различных целей, таких как тестирование подключений через брандмауэры, тестирование маршрутов и механизмов аутентификации. Одним из таких механизмов аутентификации является анонимный пользователь. Это часто используется для того, чтобы позволить всем пользователям внутренней сети обмениваться файлами и данными без доступа к компьютерам друг друга. В случае **vsFTPd** дополнительные параметры, которые можно добавить в файл конфигурации для анонимного входа в систему, выглядят следующим образом:

|**Setting**|**Description**|
|---|---|
|`anonymous_enable=YES`|Allowing anonymous login?|
|`anon_upload_enable=YES`|Allowing anonymous to upload files?|
|`anon_mkdir_write_enable=YES`|Allowing anonymous to create new directories?|
|`no_anon_password=YES`|Do not ask anonymous for password?|
|`anon_root=/home/username/ftp`|Directory for anonymous.|
|`write_enable=YES`|Allow the usage of FTP commands: STOR, DELE, RNFR, RNTO, MKD, RMD, APPE, and SITE?|

Однако, чтобы получить первое представление о настройках сервера, мы можем использовать следующую команду:

```
ftp> status

Connected to 10.129.14.136.
No proxy connection.
Connecting using address family: any.
Mode: stream; Type: binary; Form: non-print; Structure: file
Verbose: on; Bell: off; Prompting: on; Globbing: on
Store unique: off; Receive unique: off
Case: off; CR stripping: on
Quote control characters: on
Ntrans: off
Nmap: off
Hash mark printing: off; Use of PORT cmds: on
Tick counter printing: off
```

Некоторые команды следует использовать время от времени, так как это позволит серверу показывать нам больше информации, которую мы можем использовать для наших целей. К таким командам относятся debug и trace.

```
ftp> debug

Debugging on (debug=1).


ftp> trace

Packet tracing on.


ftp> ls

---> PORT 10,10,14,4,188,195
200 PORT command successful. Consider using PASV.
---> LIST
150 Here comes the directory listing.
-rw-rw-r--    1 1002     1002      8138592 Sep 14 16:54 Calender.pptx
drwxrwxr-x    2 1002     1002         4096 Sep 14 17:03 Clients
drwxrwxr-x    2 1002     1002         4096 Sep 14 16:50 Documents
drwxrwxr-x    2 1002     1002         4096 Sep 14 16:50 Employees
-rw-rw-r--    1 1002     1002           41 Sep 14 16:45 Important Notes.txt
226 Directory send OK.
```

|**Setting**|**Description**|
|---|---|
|`dirmessage_enable=YES`|Show a message when they first enter a new directory?|
|`chown_uploads=YES`|Change ownership of anonymously uploaded files?|
|`chown_username=username`|User who is given ownership of anonymously uploaded files.|
|`local_enable=YES`|Enable local users to login?|
|`chroot_local_user=YES`|Place local users into their home directory?|
|`chroot_list_enable=YES`|Use a list of local users that will be placed in their home directory?|

|**Setting**|**Description**|
|---|---|
|`hide_ids=YES`|All user and group information in directory listings will be displayed as "ftp".|
|`ls_recurse_enable=YES`|Allows the use of recurse listings.|
В следующем примере мы можем видеть, что при наличии параметра hide_ids=YES представление UID и GUID-кода службы будет перезаписано, что затруднит нам определение того, с какими правами эти файлы записываются и загружаются.

###### Download All Available Files
```
wget -m --no-passive ftp://anonymous:anonymous@10.129.14.136
```

###### Nmap FTP Scripts
```
vantuzi@htb[/htb]$ sudo nmap --script-updatedb

Starting Nmap 7.80 ( https://nmap.org ) at 2021-09-19 13:49 CEST
NSE: Updating rule database.
NSE: Script Database updated successfully.
Nmap done: 0 IP addresses (0 hosts up) scanned in 0.28 seconds
```
```
vantuzi@htb[/htb]$ find / -type f -name ftp* 2>/dev/null | grep scripts

/usr/share/nmap/scripts/ftp-syst.nse
/usr/share/nmap/scripts/ftp-vsftpd-backdoor.nse
/usr/share/nmap/scripts/ftp-vuln-cve2010-4221.nse
/usr/share/nmap/scripts/ftp-proftpd-backdoor.nse
/usr/share/nmap/scripts/ftp-bounce.nse
/usr/share/nmap/scripts/ftp-libopie.nse
/usr/share/nmap/scripts/ftp-anon.nse
/usr/share/nmap/scripts/ftp-brute.nse
```
###### Nmap Script Trace

```
vantuzi@htb[/htb]$ sudo nmap -sV -p21 -sC -A 10.129.14.136 --script-trace

Starting Nmap 7.80 ( https://nmap.org ) at 2021-09-19 13:54 CEST                                                                                                                                                   
NSOCK INFO [11.4640s] nsock_trace_handler_callback(): Callback: CONNECT SUCCESS for EID 8 [10.129.14.136:21]                                   
NSOCK INFO [11.4640s] nsock_trace_handler_callback(): Callback: CONNECT SUCCESS for EID 16 [10.129.14.136:21]             
NSOCK INFO [11.4640s] nsock_trace_handler_callback(): Callback: CONNECT SUCCESS for EID 24 [10.129.14.136:21]
NSOCK INFO [11.4640s] nsock_trace_handler_callback(): Callback: CONNECT SUCCESS for EID 32 [10.129.14.136:21]
NSOCK INFO [11.4640s] nsock_read(): Read request from IOD #1 [10.129.14.136:21] (timeout: 7000ms) EID 42
NSOCK INFO [11.4640s] nsock_read(): Read request from IOD #2 [10.129.14.136:21] (timeout: 9000ms) EID 50
NSOCK INFO [11.4640s] nsock_read(): Read request from IOD #3 [10.129.14.136:21] (timeout: 7000ms) EID 58
NSOCK INFO [11.4640s] nsock_read(): Read request from IOD #4 [10.129.14.136:21] (timeout: 11000ms) EID 66
NSE: TCP 10.10.14.4:54226 > 10.129.14.136:21 | CONNECT
NSE: TCP 10.10.14.4:54228 > 10.129.14.136:21 | CONNECT
NSE: TCP 10.10.14.4:54230 > 10.129.14.136:21 | CONNECT
NSE: TCP 10.10.14.4:54232 > 10.129.14.136:21 | CONNECT
NSOCK INFO [11.4660s] nsock_trace_handler_callback(): Callback: READ SUCCESS for EID 50 [10.129.14.136:21] (41 bytes): 220 Welcome to HTB-Academy FTP service...
NSOCK INFO [11.4660s] nsock_trace_handler_callback(): Callback: READ SUCCESS for EID 58 [10.129.14.136:21] (41 bytes): 220 Welcome to HTB-Academy FTP service...
NSE: TCP 10.10.14.4:54228 < 10.129.14.136:21 | 220 Welcome to HTB-Academy FTP service.
```
###### Service Interaction
Это выглядит несколько иначе, если FTP-сервер работает с шифрованием TLS/SSL. Потому что тогда нам нужен клиент, который может обрабатывать TLS/SSL. Для этого мы можем использовать клиент openssl и взаимодействовать с FTP-сервером. Преимущество использования openssl заключается в том, что мы можем видеть SSL-сертификат, который также может быть полезен.

```
openssl s_client -connect 10.129.14.136:21 -starttls ftp

CONNECTED(00000003)                                                                                      
Can't use SSL_get_servername                        
depth=0 C = US, ST = California, L = Sacramento, O = Inlanefreight, OU = Dev, CN = master.inlanefreight.htb, emailAddress = admin@inlanefreight.htb
verify error:num=18:self signed certificate
verify return:1

depth=0 C = US, ST = California, L = Sacramento, O = Inlanefreight, OU = Dev, CN = master.inlanefreight.htb, emailAddress = admin@inlanefreight.htb
verify return:1
---                                                 
Certificate chain
 0 s:C = US, ST = California, L = Sacramento, O = Inlanefreight, OU = Dev, CN = master.inlanefreight.htb, emailAddress = admin@inlanefreight.htb
 
 i:C = US, ST = California, L = Sacramento, O = Inlanefreight, OU = Dev, CN = master.inlanefreight.htb, emailAddress = admin@inlanefreight.htb
---
 
Server certificate

-----BEGIN CERTIFICATE-----

MIIENTCCAx2gAwIBAgIUD+SlFZAWzX5yLs2q3ZcfdsRQqMYwDQYJKoZIhvcNAQEL
...SNIP...
```