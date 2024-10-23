# gonePhishing
Et skoleprosjekt som simulerer phishing for å lære om nettverk. Python (Flask), Linux, og Wireshark

I dette prosjektet skal du gjøre følgende:
1. Bruke Flask til å lage en webside med et skjema (form) for å sende inn data
1. Bruke Gunicorn (Linux-basert webserver for Python) til å hoste websiden på en Linux-maskin
1. Endre 'hosts'-filen på en klientmaskin, så et domene (f.eks. vg.no) i stedet leder til din webserver
1. Bruke Wireshark på serveren din til å plukke opp informasjonen som sendes

# 1. Flask

Lag en katalog for prosjektet som har følgende struktur:
```     
📂phishing
 ┣ 📜app.py
 ┣ 📂templates
 ┃ ┗ 📜form.html
```
app.py:
```python
import Flask, request, render_page
ting = tang 
```

form.html:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Haxxing</title>
</head>
<body>
    <form action="" method="post">
        
    </form>
</body>
</html>
```
# 2. Gunicorn

Installeres med 
```console
pip install gunicorn
```

Putt inn instrukt fra webside
# 3. Endre hosts-fil
DNS er en slags telefonkatalog som gjør at du kan skrive 'vg.no' i stedet for '195.88.54.16' når du vil lese clickbait om kjendiser.

MEN, lokalt på de fleste maskiner finnes det en 'hosts'-fil. Dersom domenet man leter etter ligger i hosts, vil du sendes til IP-adressen som er registrert der.

Hosts-fila vil i de fleste tilfeller overstyre DNS.

La oss kapre vg.no på en klient-maskin.

Finn IP-adressen til vg.no med ping:

```console
C:\Users\Erik>ping vg.no

Pinging vg.no [195.88.54.16] with 32 bytes of data:
```
Endre hosts-fila så vg.no peker til IP-adressen du kjører Flask-prosjektet på. Du kan også legg til [navnet ditt].com til den originale vg-adressen:

## Windows

```
C:\Windows\System32\drivers\etc
```
```
# localhost name resolution is handled within DNS itself.
#	::1             localhost
127.0.0.1		localhost
195.88.55.16            erik.com
192.168.02.13		vg.no
```
## Linux
```console
/etc/hosts
```
```
127.0.0.1       localhost
127.0.1.1       erik-Virtual-Machine

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters

10.2.1.52 vg.no
```

