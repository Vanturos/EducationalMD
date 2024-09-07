
|Port(s)|Protocol|
|---|---|
|`20`/`21` (TCP)|`FTP`|
|`22` (TCP)|`SSH`|
|`23` (TCP)|`Telnet`|
|`25` (TCP)|`SMTP`|
|`80` (TCP)|`HTTP`|
|`161` (TCP/UDP)|`SNMP`|
|`389` (TCP/UDP)|`LDAP`|
|`443` (TCP)|`SSL`/`TLS` (`HTTPS`)|
|`445` (TCP)|`SMB`|
|`3389` (TCP)|`RDP`|
Существует две категории портов: Transmission Control Protocol (TCP), and User Datagram Protocol (UDP). TCP ориентирован на соединение. Это означает, что перед отправкой данных должно быть установлено соединение между клиентом и сервером. Сервер должен находиться в состоянии прослушивания и ожидать запросов на соединение от клиентов. UDP использует модель связи без установления соединения. «Hanshake» отсутствует и, следовательно, вносит определенную ненадежность, поскольку нет гарантии доставки данных. UDP полезен, когда исправление/проверка ошибок не требуется или выполняется самим приложением.

| PORT NUMBER | SERVICE                    |
| ----------- | -------------------------- |
| 7           | Echo                       |
| 20, 21      | FTP                        |
| 22          | SSH/SCP                    |
| 23          | Telnet                     |
| 25          | SMTP                       |
| 53          | DNS                        |
| 67, 68      | DHCP/BOOTP                 |
| 69          | TFTP                       |
| 80          | HTTP                       |
| 88          | Kerberos                   |
| 110         | POP3                       |
| 123         | NTP                        |
| 137         | NetBIOS                    |
| 143         | IMAP                       |
| 161         | SNMP                       |
| 194         | IRC                        |
| 389         | LDAP                       |
| 443         | HTTPS                      |
| 445         | Microsoft DS SMB           |
| 464         | Kerberos password settings |
| 547         | DHCPv6                     |
| 596         | SMSD                       |
| 636         | LDAP over SSL              |
| 1720        | H.323                      |
| 3389        | RDP                        |
| 5060        | SIP                        |
| 5061        | SIP over TLS               |

###### Cheat Sheet по портам - https://www.stationx.net/common-ports-cheat-sheet/