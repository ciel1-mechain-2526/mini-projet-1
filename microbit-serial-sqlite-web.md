# Projet : Station de mesure et visualisation de température

## Objectif
Créer un système simple permettant :
- de **mesurer la température** à l'aide d'une carte *Micro:bit*,
- de **transmettre les données** à un PC via le port série,
- de **stocker les valeurs** dans une base SQLite,
- de **consulter les mesures récentes** à travers une interface web minimale (*Flask*).

---

## Matériel requis
- 1 micro:bit
- 1 câble micro-USB/USB pour la connexion au PC
- 1 PC sous Linux

---

## Programme *Micro:bit*

```python
from microbit import *
import utime

while True:
    print(temperature()+1)
    utime.sleep(10)
```

Ce script en **MicroPython** :
- lit la température du capteur interne de la *Micro:bit*,
- l’envoie régulièrement (ici toutes les 10 secondes) sur le port série.

---

## Programme sur PC 

```python
import threading
import sqlite3
from flask import Flask, Response
import serial


SERIAL_PORT = "/dev/ttyACM0"
BAUDRATE = 115200
DB_PATH = "temperature.db"


def serial_loop():
    with serial.Serial(SERIAL_PORT, BAUDRATE, timeout=1) as ser, sqlite3.connect(DB_PATH) as conn:
        while True:
            line = ser.readline().strip()
            try:
                v = int(line)
                conn.execute("INSERT INTO temperature(value) VALUES (?)", (v,))
                conn.commit()
            except ValueError:
                continue


app = Flask(__name__)

@app.route('/')
def index():
    rows = sqlite3.connect(DB_PATH).execute(
        'SELECT timestamp, value FROM temperature ORDER BY timestamp DESC LIMIT 30'
    ).fetchall()
    return Response('\n'.join(f"{ts}   {v}°C" for ts, v in rows), mimetype="text/plain")


if __name__ == "__main__":
    sqlite3.connect(DB_PATH).execute(
        """
        CREATE TABLE IF NOT EXISTS temperature (
            timestamp TEXT PRIMARY KEY DEFAULT (datetime('now','localtime')),
            value INT
        )
        """
    ).connection.commit()

    threading.Thread(target=serial_loop, daemon=True).start()

    app.run(host="0.0.0.0", port=8000)
```

- **Lecture série** : un thread Python lit les valeurs envoyées par la *Micro:bit* sur `/dev/ttyACM0`.
- **Stockage** : chaque valeur est insérée dans la base SQLite avec un horodatage automatique.
- **Serveur web** :  À chaque requète HTTP sur le port 8000 du PC, un petit serveur *Flask* retourne les 30 dernières mesures en texte brut.

Exemple de sortie :

    2025-09-20 10:15:23   24°C
    2025-09-20 10:14:53   23°C
    ...


---

## Base de données SQLite

- La table est créée automatiquement au premier lancement du programme PC

- Chaque insertion ne fournit que `value`. Le `timestamp` est géré par SQLite.
