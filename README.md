# gonePhishing
Et skoleprosjekt som simulerer phishing for å lære om nettverk. Python (Flask), Linux, og Wireshark

I dette prosjektet skal du gjøre følgende:
1. Bruke Flask til å lage en webside med et skjema (form) for å sende inn data
1. Bruke Gunicorn (Linux-basert webserver for Python) til å hoste websiden på en Linux-maskin
1. Endre 'hosts'-filen på en klientmaskin, så et domene (f.eks. vg.no) i stedet leder til din webserver
1. Bruke Wireshark på serveren din til å plukke opp informasjonen som sendes

# 1. Flask

Lag en katalog for prosjektet som har følgende struktur:

    |-- phishing
        |-- app.py
        |-- templates
            |-- form.html
  ```

            
📦phishing
 ┣ 📂client
 ┣ 📂node_modules
 ┣ 📂server
 ┃ ┗ 📜index.js
 ┣ 📜.gitignore
 ┣ 📜package-lock.json
 ┗ 📜package.json

    ```
    |-- phishing
        |-- app.py
        |-- _templates
            |-- form.html
            
