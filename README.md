Shell script opgaver
1.

bruger kommando

#!/bin/bash
date


2.

bruger kommando

#!/bin/bash
echo "Hostname: $(hostname)"
echo "IP-adresse(r):"
hostname -I

3.

bruger kommando

#!/bin/bash
ls /etc/*.conf 2>/dev/null

4.
bruger kommando
#!/bin/bash

FIL="$1"

if [ -z "$FIL" ]; then
  echo "Brug: $0 filnavn"
  exit 1
fi

if [ -f "$FIL" ]; then
  echo "Filen $FIL findes."
else
  echo "Filen $FIL findes ikke."
fi


5.

bruger kommmando
#!/bin/bash
who | wc -l

6.

bruger kommando

echo "=== Processer for bruger ==="
    read -p "Indtast brugernavn (tryk Enter for aktuel bruger): " brugernavn
    if [ -z "$brugernavn" ]; then
        brugernavn=$(whoami)
    fi
    echo "Processer for bruger: $brugernavn"
    ps -u "$brugernavn" -o pid,ppid,%cpu,%mem,cmd 2>/dev/null
    if [ $? -ne 0 ]; then
        echo "Kunne ikke finde processer for bruger '$brugernavn'"
    fi
    echo


7.

bruger kommando

read -p "Indtast portnummer (fx 22): " port
        if [ -z "$port" ]; then
        port=22
    fi
    echo "Tjekker port $port..."
if command -v ss &> /dev/null; then
        if ss -tuln | grep -q ":$port "; then
            echo "✓ Port $port er åben (lytter)"
            ss -tuln | grep ":$port "
        else
            echo "✗ Port $port er ikke åben"
        fi
elif command -v netstat &> /dev/null; then
        if netstat -tuln | grep -q ":$port "; then
            echo "✓ Port $port er åben (lytter)"
            netstat -tuln | grep ":$port "
        else
            echo "✗ Port $port er ikke åben"
        fi
    else
        echo "Hverken ss eller netstat er tilgængelig"
    fi
    echo


8.

bruger kommando

echo "=== Skærmlåsnings-timer ==="
    echo "Starter 5 minutters inaktivitetstimer..."
    echo "Skriv noget for at nulstille timeren, eller vent 5 minutter"
        TIMEOUT=300  # 5 minutter i sekunder
    OLD_TMOUT=$TMOUT
    TMOUT=$TIMEOUT
    while true; do
        read -t $TIMEOUT -p "Timer aktiv (tryk Enter for at nulstille): " input
        if [ $? -ne 0 ]; then
            echo
            echo "5 minutter uden aktivitet - låser skærm..."
            if command -v gnome-screensaver-command &> /dev/null; then
                gnome-screensaver-command -l
            elif command -v xdg-screensaver &> /dev/null; then
                xdg-screensaver lock
            elif command -v loginctl &> /dev/null; then
                loginctl lock-session
            else
                echo "Ingen skærmlåsningskommando fundet"
            fi
            break
        else
            echo "Timer nulstillet"
        fi
    done
    TMOUT=$OLD_TMOUT
    echo


Python scripting opgaver

1.

bruger kommando

from datetime import datetime

now = datetime.now()
print(now.strftime("%d-%m-%Y %H:%M:%S"))



2.

bruger kommando

import socket

hostname = socket.gethostname()
ip = socket.gethostbyname(hostname)

print("Hostname:", hostname)
print("IP-adresse:", ip)











3.

bruger kommando

from pathlib import Path

for file in Path("/etc").iterdir():
    if file.name.endswith(".conf"):
        print(file.name)

4.

bruger kommando
from pathlib import Path

fil = Path("/etc/passwd")

if fil.exists():
    print("Filen findes")
else:
    print("Filen findes ikke")


5.

bruger kommando

import os

for file in os.listdir("."):
    if file.endswith(".txt"):
        os.rename(file, file.replace(".txt", ".md"))

6.

bruger kommando

import psutil

for proc in psutil.process_iter(['pid', 'name']):
    print(proc.info)


7.

bruger kommando

import socket

host = "google.com"
port = 443

s = socket.socket()
s.settimeout(3)

try:
    s.connect((host, port))
    print(f"Port {port} er åben")
except:
    print(f"Port {port} er lukket")
finally:
    s.close()

8.

bruger kommando

import shutil

total, used, free = shutil.disk_usage("/")
free_percent = free / total * 100

if free_percent < 20:
    print("ADVARSEL: Lav diskplads")
else:
    print(f"Ledig plads: {free_percent:.2f}%")

9.

bruger kommando

a = float(input("Tal 1: "))
op = input("Operator (+ - * /): ")
b = float(input("Tal 2: "))

if op == "+":
    print(a + b)
elif op == "-":
    print(a - b)
elif op == "*":
    print(a * b)
elif op == "/":
    print(a / b)
else:
    print("Ugyldig operator")



Powershell opgaver


bruger kommando

Get-Process | Select-Object Name, Id


bruger kommando

$now = Get-Date
Write-Host $now.ToString("dd-MM-yyyy HH:mm:ss")


Kali Linux


Filsystem
pwd && cd ~
mkdir -p ~/kali-ovelser/fs/{data,tmp}
echo "hej kali" > ~/kali-ovelser/fs/data/notes.txt
mv ~/kali-ovelser/fs/data/notes.txt ~/kali-ovelser/fs/tmp/.hidden_notes
Brugere og grupper
id
grep "^$USER:" /etc/passwd
sudo groupadd lab 2>/dev/null || true && sudo usermod -aG lab $USER
Processer
ps -u $USER
echo $$
sleep 60 & jobs
Resurser
top -b -n1 | head -n 10
df -h
time ls / >/dev/null
Netværk
ip a
ping -c 3 kali.org
ss -tulpn (brug sudo hvis krævet)
Systeminfo & environment
uname -r && uname -m
echo "$PATH"
APT
sudo apt update
apt search jq | head -n 10
sudo apt install -y jq && jq --version && sudo apt remove -y jq
Logging
journalctl -n 20 --no-pager
journalctl -u ssh.service -n 20 --no-pager (nogle systemer: sshd.service)
grep -E '^(Start-Date|Commandline):' /var/log/apt/history.log | tail -n 20
sudo tail -f /var/log/auth.log (stop med Ctrl+C; alternativt /var/log/secure)
sudo ls -lhS /var/log | head -n 5
Processer & services
ping -c 10 8.8.8.8 (stop med Ctrl+C)
sleep 120 & kill %1 (eller: find PID og kill <PID>)
systemctl status ssh
Kryptografi
Hash
(cd ~/kali-ovelser/fs/tmp && sha256sum .hidden_notes > notes.sha256)
(cd ~/kali-ovelser/fs/tmp && sha256sum -c notes.sha256)
Kryptering/dekryptering (symmetrisk) – enkel metode med gpg
(cd ~/kali-ovelser/fs/tmp && gpg --symmetric --output notes.gpg .hidden_notes)
(cd ~/kali-ovelser/fs/tmp && gpg --decrypt --output notes.dec notes.gpg && diff -u .hidden_notes notes.dec || true)
Signering & verifikation (gpg)
gpg --quick-generate-key "Lab User" default default never (engang)
(cd ~/kali-ovelser/fs/tmp && gpg --output notes.sig --detach-sign .hidden_notes)
(cd ~/kali-ovelser/fs/tmp && gpg --verify notes.sig .hidden_notes)


Historisk kryptografi
Caesar ROT

 

Vigenére


 
Steganografi
Steganografi (af græsk steganos, "dækket", og gráphein, "at skrive") er et underemne inden for kryptologien, der beskæftiger sig med at skjule beskeder i en eller anden form for kontekst. Steganografi adskiller sig fra kryptering, da man ved kryptering forudsætter, at en opponent kender til beskedtransporten. Ved steganografi prøver man i stedet at skjule, at der overhovedet sker en beskedtransport.



Anvendt kryptografi
Onionshare
OnionShare bruger Tor til anonym og midlertidig fildeling uden behov for en konto, mens Keybase er en kontobaseret tjeneste, der anvender PGP til krypteret kommunikation og bekræftelse af identitet

Pcrypt
Pcrypt er en dansk virksomhed, der arbejder med løsninger inden for kryptografi og sikker kommunikation. I modsætning til almindelige forbrugerværktøjer har Pcrypt fokus på professionelle og erhvervsrettede behov. Derfor kunne virksomheden være en interessant og relevant praktikplads for studerende med interesse for IT-sikkerhed


Open source key management
Bitwarden er et open source-værktøj, der gør det muligt at gemme og dele adgangskoder og andre hemmelige oplysninger sikkert. Det bygger på ende-til-ende kryptering og zero-knowledge-principper, så kun brugeren selv har adgang til dataene

Kryptografi i din software
Web Crypto API er tilgængeligt i alle webbrowsere, og kan bruges til at kryptere og hashe med.  I denne opgave skal du undersøge, hvad Web Crypto API er for noget.
 Web Crypto API er et browser-baseret API, der giver adgang til sikre kryptografiske primitive som kryptering, signering og hashing. Det anvendes til klient-side sikkerhed og end-to-end kryptering i moderne webapplikationer.

Sikker e-mail?
E-mail protokoller blev oprindeligt udviklet uden indbygget sikkerhed, og derfor findes der ikke én enkel løsning på, hvordan man sender en sikker e-mail. I denne opgave skal du undersøge forskellige muligheder, f.eks. . hvordan man kan sende sikker mail via Gmail eller Hotmail, og hvordan man kan gøre det gennem Office 365 med din edu-mail på skolen.
