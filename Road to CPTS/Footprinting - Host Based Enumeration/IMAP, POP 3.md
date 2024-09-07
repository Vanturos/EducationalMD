#### Основные различия IMAP и POP3

**IMAP (Internet Message Access Protocol)** — это сетевой протокол, который позволяет управлять электронной почтой непосредственно на удалённом сервере. IMAP поддерживает работу с папками и синхронизацию между несколькими клиентами, создавая единое хранилище для всех устройств. Это позволяет просматривать и управлять письмами, не загружая их локально. Письма остаются на сервере до их удаления.

**POP3 (Post Office Protocol)**, в отличие от IMAP, предоставляет только базовые функции: получение и удаление писем с сервера. POP3 не поддерживает синхронизацию или управление письмами на сервере, что делает его менее гибким по сравнению с IMAP.

#### Основные функции IMAP

- **Поддержка структуры папок**: создание, переименование, удаление папок.
- **Онлайн-управление письмами**: доступ к письмам и папкам прямо на сервере.
- **Синхронизация**: синхронизация изменений между всеми подключенными клиентами.
- **Многочисленные почтовые ящики**: возможность работы с несколькими почтовыми ящиками одновременно.
- **Просмотр писем на сервере**: просмотр заголовков и содержимого писем до их загрузки.

#### Команды IMAP

- `LOGIN username password` — вход пользователя.
- `LIST "" *` — список всех папок.
- `CREATE "INBOX"` — создание новой папки.
- `DELETE "INBOX"` — удаление папки.
- `RENAME "OldName" "NewName"` — переименование папки.
- `SELECT INBOX` — выбор папки для работы.
- `UNSELECT INBOX` — выход из выбранной папки.
- `FETCH <ID> all` — получение данных о сообщении.
- `CLOSE` — удаление всех писем с флагом `Deleted`.
- `LOGOUT` — завершение сеанса с сервером.

#### Команды POP3

- `USER username` — указание имени пользователя.
- `PASS password` — аутентификация пользователя.
- `STAT` — запрос количества сохранённых писем.
- `LIST` — запрос списка писем и их размера.
- `RETR id` — получение письма по ID.
- `DELE id` — удаление письма по ID.
- `CAPA` — запрос возможностей сервера.
- `RSET` — сброс состояния сессии.
- `QUIT` — завершение сеанса с сервером.

#### Настройки безопасности и диагностики

IMAP передаёт данные в открытом виде, что небезопасно. Для защиты данных рекомендуется использовать зашифрованные соединения с использованием SSL/TLS. По умолчанию IMAP использует порт 143, но для зашифрованных соединений используется порт 993.

**Опасные настройки:**
- **auth_debug** — логгирование всех процессов аутентификации.
- **auth_debug_passwords** — логгирование паролей, используемых при аутентификации.
- **auth_verbose** — логгирование неудачных попыток входа и причин отказа.
- **auth_verbose_passwords** — логгирование используемых паролей с возможностью их обрезки.
- **auth_anonymous_username** — задание имени пользователя для анонимного входа.

#### Анализ сервисов IMAP/POP3 с помощью Nmap

Можно использовать Nmap для сканирования серверов на открытые порты IMAP (143, 993) и POP3 (110, 995) и для получения информации о сервере, включая сертификаты SSL и поддерживаемые команды.

```bash
sudo nmap <IP-адрес> -sV -p110,143,993,995 -sC
```

#### Работа с IMAP/POP3 через cURL и OpenSSL

Для взаимодействия с серверами IMAP/POP3 через SSL можно использовать cURL или OpenSSL. Например, с помощью cURL можно подключиться к серверу IMAP:

```bash
curl -k 'imaps://<IP-адрес>' --user user:password
```

С помощью OpenSSL можно установить зашифрованное соединение:

```bash
openssl s_client -connect <IP-адрес>:imaps
```

```
┌─[eu-academy-6]─[10.10.14.248]─[htb-ac-807486@htb-minoqm0tib]─[~]
└──╼ [★]$ locate *.nse | grep "imap"
/usr/share/nmap/scripts/imap-brute.nse
--SNIP--
```

```
┌─[eu-academy-6]─[10.10.14.248]─[htb-ac-807486@htb-minoqm0tib]─[~]
└──╼ [★]$ nc -nv 10.129.91.8 110
(UNKNOWN) [10.129.91.8] 110 (pop3) open
+OK InFreight POP3 v9.188
```

### IMAP FETCH

This command allows a client to fetch messages from a folder, and it is one of the most widely used IMAP commands.

You may specify which messages you wish to fetch, and even which messages parts; you do not have to fetch the entire message. This one command is probably the “killer feature” of IMAP, one of the main reasons it exists; it allows clients to interact in a sophisticated way with the IMAP server.

Here is a list of all the message parts (and defined macros representing pre-defined messages parts) you can fetch:

- ALL: Macro equivalent to: (FLAGS INTERNALDATE RFC822.SIZE ENVELOPE)
- FAST: Macro equivalent to: (FLAGS INTERNALDATE RFC822.SIZE)
- FULL: Macro equivalent to: (FLAGS INTERNALDATE RFC822.SIZE ENVELOPE BODY)
- BODY: Non-extensible form of BODYSTRUCTURE.
- BODY[<section>]<<partial>>: The text of a particular body section. The section specification is a set of zero or more part specifiers delimited by periods. A part specifier is either a part number or one of the following: HEADER, HEADER.FIELDS, HEADER.FIELDS.NOT, MIME, and TEXT. An empty section specification refers to the entire message, including the header. You may even select only parts of a multipart MIME message and even specific octets within that part, see [RFC 3501#section-6.4.5](https://tools.ietf.org/html/rfc3501#section-6.4.5) for more details.
- BODY.PEEK[<section>]<<partial>>: An alternate form of BODY[<section>] that does not implicitly set the \Seen flag.
- BODYSTRUCTURE: The MIME body structure of the message. This is computed by the server by parsing the MIME header fields in the header and body MIME headers.
- ENVELOPE: The envelope structure of the message. This is computed by the server by parsing the message header into the component parts, defaulting various fields as necessary.
- FLAGS: The flags that are set for this message.
- INTERNALDATE: The internal date of the message.
- RFC822: Functionally equivalent to BODY[], differing in the syntax of the resulting untagged FETCH data in that the full RFC822 message is returned.
- RFC822.HEADER: Functionally equivalent to BODY.PEEK[HEADER], with RFC822 header syntax returned.
- RFC822.SIZE: The size of the message.
- RFC822.TEXT: Functionally equivalent to BODY[TEXT], differing in the syntax of the resulting untagged FETCH data as RFC822.TEXT is returned.
- UID: The unique identifier for the message.