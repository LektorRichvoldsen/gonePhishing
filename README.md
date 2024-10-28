# gonePhishing
Et skoleprosjekt som simulerer phishing for å lære om nettverk. Python (Flask), Linux, og Wireshark

I dette prosjektet skal du gjøre følgende:
1. Bruke Flask til å lage en webside med et skjema (form) for å sende inn data
1. Bruke Gunicorn (Linux-basert webserver for Python) til å hoste websiden på en Linux-maskin
1. Endre 'hosts'-filen på en klientmaskin, så et domene (f.eks. vg.no) i stedet leder til din webserver
1. Bruke Wireshark på serveren din til å plukke opp informasjonen som sendes

# 1. Flask

Installer Flask:
```terminal
pip install flask
```

Lag en katalog for prosjektet som har følgende struktur:
```     
📂phishing
 ┣ 📜app.py
 ┗ 📂templates
    ┗ 📜form.html
```
app.py:
```python
from flask import Flask, request, render_template

app = Flask(__name__)


@app.route('/')
def hello_world():
    return 'Hello World'


@app.route('/form')
def form():
    return render_template('form.html')


@app.route('/hack', methods=['GET', 'POST'])
def hack():
    navn = request.form['name']
    print(navn)
    return 'Funka'


if __name__ == '__main__':
    app.run(debug=True)
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
     <form action="\hack" method="post">
        <h1>VG.NO</h1>
        <label for="name">Enter your name: </label>
        <input type="text" name="name" id="name" required />
    
        <label for="password">Enter your password: </label>
        <input type="text" name="email" id="password" required />
    
        <input type="submit" value="Klikk" />
    </form>
</body>
</html>
```

Test websiden ved å kjøre app.py, og så gå til en nettleser og åpne localhost:5000

# 2. Gunicorn

OBS, Gunicorn fungerer bare på Linux!

Installeres med 
```console
pip install gunicorn
```
Lag en fil i prosjektmappa som heter gunicorn_config.py

```     
📂phishing
 ┣ 📜app.py
 ┣ 📜gunicorn_config.py
 ┗ 📂templates
    ┗ 📜form.html
```

```python
import os

workers = int(os.environ.get('GUNICORN_PROCESSES', '2'))

threads = int(os.environ.get('GUNICORN_THREADS', '4'))

# timeout = int(os.environ.get('GUNICORN_TIMEOUT', '120'))

bind = os.environ.get('GUNICORN_BIND', '0.0.0.0:8080')

forwarded_allow_ips = '*'
```

Start Gunicorn:
```terminal
gunicorn --config gunicorn_config.py app:app
```
Den første 'app'-en er navnet på python-filen din.

Test ved å åpne nettleser på en annen maskin og gå til [din ip]:8080 (mulig port 8080 må åpnes i brannmur)

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
Flush DNS for å få inn endringene:
```
ipconfig /flushdns
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
Oppdater DNS
```
sudo resolvectl flush-caches
```
## Test med ping
Ping vg.no for å se som du sendes til den din egen server. Prøv også å gå til [navnet ditt].com i en nettleser og se om den tar deg til VG.

# 4. Wireshark

Når data sendes over et nettverk, blir de brutt opp i 'pakker'. Wireshark er et program som overvåker et nettverkskort, og kan registrere alle pakker som sendes og mottas på det kortet.

Du overvåker altså Ethernet ELLER Wi-Fi, ikke all trafikken på en maskin.

Installer Wireshark på maskinen du kjører Flask-serveren (den med IP-adressen du pekte til i hosts-fila).
