# Домашнее задание к занятию "`Защита сети`" - `Осаковская Анна`

---

### Задание 1

`Определение сетевых служб, запущенных на защищаемой системе`

**sudo nmap -sA < ip-адрес >**
  
<img width="891" height="257" alt="Снимок экрана 2026-02-27 в 22 32 29" src="https://github.com/user-attachments/assets/f8415205-33e7-40b1-aa69-596f2262e174" />

**sudo nmap -sT < ip-адрес >**

<img width="891" height="384" alt="Снимок экрана 2026-02-27 в 22 34 56" src="https://github.com/user-attachments/assets/fca24d6e-80f0-4a3e-a2c1-8c7335227f45" />

**sudo nmap -sS < ip-адрес >**

<img width="886" height="378" alt="Снимок экрана 2026-02-27 в 22 37 53" src="https://github.com/user-attachments/assets/f35efc4f-122c-44f7-8c40-5c202e882189" />

**sudo nmap -sV < ip-адрес >**

<img width="889" height="481" alt="Снимок экрана 2026-02-27 в 22 41 14" src="https://github.com/user-attachments/assets/8c96aaa9-d7e9-4bd3-870c-d71877ce4f26" />

`Cобытия, которые попали в логи Suricata`

<img width="1428" height="879" alt="Снимок экрана 2026-02-27 в 22 00 14" src="https://github.com/user-attachments/assets/e25b37b6-8eda-441f-b82c-b5d7ba1f4b8d" />

`Cобытия, которые попали в логи Fail2Ban`

<img width="1461" height="725" alt="Снимок экрана 2026-02-27 в 22 23 11" src="https://github.com/user-attachments/assets/2191144f-4dc1-4254-a271-84080ffae397" />

`Вывод`

**Suricata** успешно обнаружила все этапы атаки:

- разведку сети (сканирования портов);

- попытки подключения к SSH (с IP 192.168.1.218 и 4 с IP 192.168.1.109).

**Fail2Ban** не сработал, так как в логах отсутствуют записи о неудачных попытках аутентификации. Для срабатывания Fail2Ban нужны именно неудачные попытки входа с неверными паролями, а не просто TCP-соединения с 22 портом. В ходе выполнения задания nmap только сканировал порты, но не пытался подобрать пароли (например, через hydra).

---

### Задание 2

`Атака на подбор пароля для службы SSH`

<img width="1154" height="437" alt="Снимок экрана 2026-02-28 в 12 14 00" src="https://github.com/user-attachments/assets/e8df8da8-7889-459e-a1ee-f767b33c6075" />

`Конфигурационный файл Fail2Ban`

<img width="915" height="360" alt="Снимок экрана 2026-02-28 в 13 46 16" src="https://github.com/user-attachments/assets/c064b709-de17-4c5f-a477-839c8ea0127b" />

`События, которые попали в логи Suricata и Fail2Ban`

**лог Suricata**

<img width="1306" height="802" alt="Снимок экрана 2026-02-28 в 12 18 24" src="https://github.com/user-attachments/assets/58b6c62f-477a-4c81-8c12-3530bd6fc1a2" />

**лог Fail2Ban**

<img width="1265" height="431" alt="Снимок экрана 2026-02-28 в 12 17 18" src="https://github.com/user-attachments/assets/d60869f9-387a-4bc9-980a-288da5ac0393" />

`Вывод`

- **Suricata** обнаружила попытки подключения к SSH с IP 192.168.1.218, а также сопутствующее сканирование портов SYN scan.
  
- **Fail2Ban** зафиксировал эти попытки в логах /var/log/auth.log и после 3-й неудачной попытки (согласно maxretry = 3) заблокировал IP-адрес атакующего.

- **Fail2Ban** успешно выполнил задачу по защите SSH от брутфорс-атак (заблокировал атакующий IP). **Suricata** обеспечила полную видимость атаки, включая момент блокировки (ICMP-пакеты), что позволяет администратору отслеживать и анализировать события информационной безопасности
