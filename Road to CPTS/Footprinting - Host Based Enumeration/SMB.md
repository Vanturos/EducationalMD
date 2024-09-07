SMB - server message block - это клиент-серверный протокол, который регулирует доступ к файлам и целым каталогам, а также к другим сетевым ресурсам, таким как принтеры, маршрутизаторы или интерфейсы, предоставляемые для работы в сети. Обмен информацией между различными системными процессами также может осуществляться на основе протокола SMB. Впервые SMB стал доступен более широкой публике, например, как часть сетевой операционной системы OS/2 LAN Manager и LAN Server. С тех пор основной областью применения протокола стала, в частности, серия операционных систем Windows, сетевые службы которых поддерживают SMB в режиме понижающей совместимости, что означает, что устройства с более новыми версиями могут легко взаимодействовать с устройствами, на которых установлена более старая операционная система Microsoft. В рамках проекта свободного программного обеспечения Samba также существует решение, позволяющее использовать SMB в дистрибутивах Linux и Unix и, таким образом, осуществлять кросс-платформенную коммуникацию через SMB.

Протокол SMB позволяет клиенту взаимодействовать с другими участниками той же сети для доступа к файлам или службам, совместно используемым с ним в сети. Другая система также должна была реализовать сетевой протокол и получать и обрабатывать запрос клиента с помощью серверного приложения SMB. Однако перед этим обе стороны должны установить соединение, поэтому они сначала обмениваются соответствующими сообщениями.

В IP-сетях SMB для этой цели используется протокол TCP, который обеспечивает трехстороннее подтверждение связи между клиентом и сервером перед окончательным установлением соединения. Спецификации протокола TCP также регулируют последующую передачу данных. Здесь мы можем рассмотреть несколько примеров.

SMB-сервер может предоставлять произвольные части своей локальной файловой системы в качестве общих ресурсов. Таким образом, иерархия, видимая клиенту, частично не зависит от структуры на сервере. Права доступа определяются списками контроля доступа (ACL). Ими можно управлять детально на основе таких атрибутов, как выполнение, чтение и полный доступ для отдельных пользователей или групп пользователей. Списки контроля доступа определяются на основе общих ресурсов и, следовательно, не соответствуют правам, назначенным локально на сервере.

###### **Samba**

Как упоминалось ранее, существует альтернативный сервер SMB, называемый Samba, разработанный для операционной системы на базе Unix. Samba реализует сетевой протокол **Common Internet File System (CIFS)**. CIFS - это "диалект" SMB. Другими словами, **CIFS** - это очень специфическая реализация протокола SMB, который, в свою очередь, был создан Microsoft. Это позволяет Samba взаимодействовать с более новыми системами Windows. Поэтому его обычно называют **SMB / CIFS**. Однако CIFS является расширением протокола SMB. Поэтому, когда мы передаем SMB-команды через Samba более старой службе NetBIOS, она обычно подключается к серверу Samba через TCP-порты 137, 138, 139, но CIFS использует только **TCP-порт 445**. Существует несколько версий SMB, включая устаревшие версии, которые все еще используются в определенных инфраструктурах.

|**SMB Version**|**Supported**|**Features**|
|---|---|---|
|CIFS|Windows NT 4.0|Communication via NetBIOS interface|
|SMB 1.0|Windows 2000|Direct connection via TCP|
|SMB 2.0|Windows Vista, Windows Server 2008|Performance upgrades, improved message signing, caching feature|
|SMB 2.1|Windows 7, Windows Server 2008 R2|Locking mechanisms|
|SMB 3.0|Windows 8, Windows Server 2012|Multichannel connections, end-to-end encryption, remote storage access|
|SMB 3.0.2|Windows 8.1, Windows Server 2012 R2||
|SMB 3.1.1|Windows 10, Windows Server 2016|Integrity checking, AES-128 encryption|

###### **Default Configuration**

```
vantuzi@htb[/htb]$ cat /etc/samba/smb.conf | grep -v "#\|\;" 

[global]
   workgroup = DEV.INFREIGHT.HTB
   server string = DEVSMB
   log file = /var/log/samba/log.%m
   max log size = 1000
   logging = file
   panic action = /usr/share/samba/panic-action %d

   server role = standalone server
   obey pam restrictions = yes
   unix password sync = yes

   passwd program = /usr/bin/passwd %u
   passwd chat = *Enter\snew\s*\spassword:* %n\n *Retype\snew\s*\spassword:* %n\n *password\supdated\ssuccessfully* .

   pam password change = yes
   map to guest = bad user
   usershare allow guests = yes

[printers]
   comment = All Printers
   browseable = no
   path = /var/spool/samba
   printable = yes
   guest ok = no
   read only = yes
   create mask = 0700

[print$]
   comment = Printer Drivers
   path = /var/lib/samba/printers
   browseable = yes
   read only = yes
   guest ok = no
```

|**Setting**|**Description**|
|---|---|
|`[sharename]`|The name of the network share.|
|`workgroup = WORKGROUP/DOMAIN`|Workgroup that will appear when clients query.|
|`path = /path/here/`|The directory to which user is to be given access.|
|`server string = STRING`|The string that will show up when a connection is initiated.|
|`unix password sync = yes`|Synchronize the UNIX password with the SMB password?|
|`usershare allow guests = yes`|Allow non-authenticated users to access defined share?|
|`map to guest = bad user`|What to do when a user login request doesn't match a valid UNIX user?|
|`browseable = yes`|Should this share be shown in the list of available shares?|
|`guest ok = yes`|Allow connecting to the service without using a password?|
|`read only = yes`|Allow users to read files only?|
|`create mask = 0700`|What permissions need to be set for newly created files?|

###### **Dangerous Settings**

|**Setting**|**Description**|
|---|---|
|`browseable = yes`|Allow listing available shares in the current share?|
|`read only = no`|Forbid the creation and modification of files?|
|`writable = yes`|Allow users to create and modify files?|
|`guest ok = yes`|Allow connecting to the service without using a password?|
|`enable privileges = yes`|Honor privileges assigned to specific SID?|
|`create mask = 0777`|What permissions must be assigned to the newly created files?|
|`directory mask = 0777`|What permissions must be assigned to the newly created directories?|
|`logon script = script.sh`|What script needs to be executed on the user's login?|
|`magic script = script.sh`|Which script should be executed when the script gets closed?|
|`magic output = script.out`|Where the output of the magic script needs to be stored?|

###### **Restart Samba**

```
root@samba:~# sudo systemctl restart smbd
```
Now we can display a list (`-L`) of the server's shares with the `smbclient` command from our host. We use the so-called `null session` (`-N`), which is `anonymous` access without the input of existing users or valid passwords.
```
vantuzi@htb[/htb]$ smbclient -N -L //10.129.14.128

        Sharename       Type      Comment
        ---------       ----      -------
        print$          Disk      Printer Drivers
        home            Disk      INFREIGHT Samba
        dev             Disk      DEVenv
        notes           Disk      CheckIT
        IPC$            IPC       IPC Service (DEVSM)
SMB1 disabled -- no workgroup available
```

```
root@samba:~# smbstatus

Samba version 4.11.6-Ubuntu
PID     Username     Group        Machine                                   Protocol Version  Encryption           Signing              
----------------------------------------------------------------------------------------------------------------------------------------
75691   sambauser    samba        10.10.14.4 (ipv4:10.10.14.4:45564)      SMB3_11           -                    -                    

Service      pid     Machine       Connected at                     Encryption   Signing     
---------------------------------------------------------------------------------------------
notes        75691   10.10.14.4   Do Sep 23 00:12:06 2021 CEST     -            -           

No locked files
```

###### **RPCclient**

```
vantuzi@htb[/htb]$ rpcclient -U "" 10.129.14.128

Enter WORKGROUP\'s password:
rpcclient $> 
```

|**Query**|**Description**|
|---|---|
|`srvinfo`|Server information.|
|`enumdomains`|Enumerate all domains that are deployed in the network.|
|`querydominfo`|Provides domain, server, and user information of deployed domains.|
|`netshareenumall`|Enumerates all available shares.|
|`netsharegetinfo <share>`|Provides information about a specific share.|
|`enumdomusers`|Enumerates all domain users.|
|`queryuser <RID>`|Provides information about a specific user.|

###### **RPCclient - Enumeration**

```
rpcclient $> srvinfo

        DEVSMB         Wk Sv PrQ Unx NT SNT DEVSM
        platform_id     :       500
        os version      :       6.1
        server type     :       0x809a03
		
		
rpcclient $> enumdomains

name:[DEVSMB] idx:[0x0]
name:[Builtin] idx:[0x1]


rpcclient $> querydominfo

Domain:         DEVOPS
Server:         DEVSMB
Comment:        DEVSM
Total Users:    2
Total Groups:   0
Total Aliases:  0
Sequence No:    1632361158
Force Logoff:   -1
Domain Server State:    0x1
Server Role:    ROLE_DOMAIN_PDC
Unknown 3:      0x1


rpcclient $> netshareenumall

netname: print$
        remark: Printer Drivers
        path:   C:\var\lib\samba\printers
        password:
netname: home
        remark: INFREIGHT Samba
        path:   C:\home\
        password:
netname: dev
        remark: DEVenv
        path:   C:\home\sambauser\dev\
        password:
netname: notes
        remark: CheckIT
        path:   C:\mnt\notes\
        password:
netname: IPC$
        remark: IPC Service (DEVSM)
        path:   C:\tmp
        password:
		
		
rpcclient $> netsharegetinfo notes

netname: notes
        remark: CheckIT
        path:   C:\mnt\notes\
        password:
        type:   0x0
        perms:  0
        max_uses:       -1
        num_uses:       1
revision: 1
type: 0x8004: SEC_DESC_DACL_PRESENT SEC_DESC_SELF_RELATIVE 
DACL
        ACL     Num ACEs:       1       revision:       2
        ---
        ACE
                type: ACCESS ALLOWED (0) flags: 0x00 
                Specific bits: 0x1ff
                Permissions: 0x101f01ff: Generic all access SYNCHRONIZE_ACCESS WRITE_OWNER_ACCESS WRITE_DAC_ACCESS READ_CONTROL_ACCESS DELETE_ACCESS 
                SID: S-1-1-0
```

###### **Rpcclient - User Enumeration**

```
rpcclient $> enumdomusers

user:[mrb3n] rid:[0x3e8]
user:[cry0l1t3] rid:[0x3e9]


rpcclient $> queryuser 0x3e9
```

###### **Rpcclient - Group Information**

```
rpcclient $> querygroup 0x201

        Group Name:     None
        Description:    Ordinary Users
        Group Attribute:7
        Num Members:2
```

###### **Brute Forcing User RIDs**

```
vantuzi@htb[/htb]$ for i in $(seq 500 1100);do rpcclient -N -U "" 10.129.14.128 -c "queryuser 0x$(printf '%x\n' $i)" | grep "User Name\|user_rid\|group_rid" && echo "";done

        User Name   :   sambauser
        user_rid :      0x1f5
        group_rid:      0x201
		
        User Name   :   mrb3n
        user_rid :      0x3e8
        group_rid:      0x201
		
        User Name   :   cry0l1t3
        user_rid :      0x3e9
        group_rid:      0x201
```

###### **Impacket - Samrdump.py**

```
vantuzi@htb[/htb]$ samrdump.py 10.129.14.128
```

###### **SMBmap**

```
vantuzi@htb[/htb]$ smbmap -H 10.129.14.128
```

###### **CrackMapExec**

```
vantuzi@htb[/htb]$ crackmapexec smb 10.129.14.128 --shares -u '' -p ''
```

###### **Enum4Linux-ng - Installation**

```
vantuzi@htb[/htb]$ git clone https://github.com/cddmp/enum4linux-ng.git
vantuzi@htb[/htb]$ cd enum4linux-ng
vantuzi@htb[/htb]$ pip3 install -r requirements.txt
```

###### **Enum4Linux-ng - Enumeration**

```
vantuzi@htb[/htb]$ ./enum4linux-ng.py 10.129.14.128 -A
```

