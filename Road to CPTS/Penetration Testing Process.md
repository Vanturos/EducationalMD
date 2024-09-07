В данном разделе описана различная теоретическая часть. 

Таким образом, вот главные фазы пентеста:

![[Git/Road to CPTS/Pasted image 20240715162618.png]]


Представлены документы которые необходимы для проведения пентеста и подготавливаются по большей части на фазе **Pre-Engagement**, а именно:

1. **Non-Disclosure Agreement** - бывает односторонним, двусторонним, многосторонним (очев).
2. **Scoping Questionaire** - анкета которая передается клиенту для выяснения, какие именно проверки он намерен произвести. (внутренние, беспроводные, red team, физические, внешние, социальные, проверки веб приложений etc.)
3. **Scoping Document** - документ который суммирует всю информацию полученную от клиента из анкеты Scoping Questionaire, а также информацуию о сервисах, хостах, ip адресах, CIDR диапазонах и другую в соответствии с тем, был ли выбран black box, grey box или же white box.
4. **Penetration Testing Proposal (Contract/Scope of Work)** - формируем документ при Pre-Engagement Meeting. В него входят все те входные данные которые мы получили. Объясняем клиенту все детали.
5. **Rules of Engagement (ROE)** - документ, который составляется по плану приведенному ниже:

| Checkpoint                                | Contents                                                                                              |
| ----------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| ☐ Introduction                            | Description of this document.                                                                         |
| ☐ Contractor                              | Company name, contractor full name, job title.                                                        |
| ☐ Penetration Testers                     | Company name, pentesters full name.                                                                   |
| ☐ Contact Information                     | Mailing addresses, e-mail addresses, and phone numbers of all client parties and penetration testers. |
| ☐ Purpose                                 | Description of the purpose for the conducted penetration test.                                        |
| ☐ Goals                                   | Description of the goals that should be achieved with the penetration test.                           |
| ☐ Scope                                   | All IPs, domain names, URLs, or CIDR ranges.                                                          |
| ☐ Lines of Communication                  | Online conferences or phone calls or face-to-face meetings, or via e-mail.                            |
| ☐ Time Estimation                         | Start and end dates.                                                                                  |
| ☐ Time of the Day to Test                 | Times of the day to test.                                                                             |
| ☐ Penetration Testing Type                | External/Internal Penetration Test/Vulnerability Assessments/Social Engineering.                      |
| ☐ Penetration Testing Locations           | Description of how the connection to the client network is established.                               |
| ☐ Methodologies                           | OSSTMM, PTES, OWASP, and others.                                                                      |
| ☐ Objectives / Flags                      | Users, specific files, specific information, and others.                                              |
| ☐ Evidence Handling                       | Encryption, secure protocols                                                                          |
| ☐ System Backups                          | Configuration files, databases, and others.                                                           |
| ☐ Information Handling                    | Strong data encryption                                                                                |
| ☐ Incident Handling and Reporting         | Cases for contact, pentest interruptions, type of reports                                             |
| ☐ Status Meetings                         | Frequency of meetings, dates, times, included parties                                                 |
| ☐ Reporting                               | Type, target readers, focus                                                                           |
| ☐ Retesting                               | Start and end dates                                                                                   |
| ☐ Disclaimers and Limitation of Liability | System damage, data loss                                                                              |
| ☐ Permission to Test                      | Signed contract, contractors agreement                                                                |

6. **Contractors Agreement** - нужен для физического пентеста, играет роль "**get out of jail free card**".
7. **Reports** - сами составляем и отправляем клиенту по итогам работы.


На фазе **Vulnerability Assessment** происходит анализ тех данных, которые были получены во время **Information Gathering**. Это аналитическая фаза, основная на том, что было найдено.

Есть несколько типов анализа:
- **Descriptive** - описывает набор данных на основе индивидуальных характеристик. Это помогает обнаружить возможные ошибки при сборе данных или выбросы в наборе данных.
- **Diagnostic** - действует подобно описательному анализу, с той тонкой разницей, что мы пытаемся найти причины событий и развития событий.
- **Predictive** - используя текущие данные и данные из прошлого мы пытаемся предсказать модель для будущий возможностей. То есть мы идентифицируем тренды и определяем девиации на ранней стадии.
- **Prescriptive** - предписательный анализ - нацелен на то чтобы сузить количество мер, которые необходимо предпринять для избежания проблем в будущем.

Например:
- Если TCP port 2121 - TCP означает что сервис ориентированный на соединение.
- Так как порт не стандартный, так как он не среди системных портов 0-1023.
- Порт выглядит знакомым так как порт 21 TCP (FTP) - порт который администраторы обычно маскируют.

**Vulnerability Disclosures** можно найти на:
- https://www.cvedetails.com/
- https://packetstormsecurity.com/
- https://www.exploit-db.com/
- https://nvd.nist.gov/vuln/search?execution=e2s1
- https://vulners.com/

