# Einrichtung eines Raspberry Pi Servers mit Todo-Listen-Verwaltung

## Zugriff auf den Raspberry Pi über SSH

1. SD-Karte in den Raspberry Pi stecken und starten.
2. Windows PowerShell auf deinem Computer öffnen.
3. Verbinden über SSH:
    ```sh
    ssh <benutzer>@<ipaddresse>
    ```
4. Updates zur Sicherheit:
    ```sh
    sudo apt-get update
    sudo apt-get upgrade
    ```

## Einrichten einer statischen IP-Adresse mit nmcli

1. Status der Netzwerkgeräte anzeigen:
    ```sh
    nmcli dev status
    ```
2. Netzwerkverbindung ändern (ersetze `<name>`, `<adresse>`, `<gateway adresse>` und `<dns>` mit den entsprechenden Werten):
    ```sh
    nmcli con mod "<name>" ipv4.addresses <adresse>
    nmcli con mod "<name>" ipv4.gateway <gateway adresse>
    nmcli con mod "<name>" ipv4.dns <dns>
    nmcli con mod "<name>" ipv4.method manual
    nmcli con up "<name>"
    ```

## Einrichten der Benutzer

1. Benutzer `willi` erstellen:
    ```sh
    sudo adduser willi
    ```
2. Benutzer `fernzugriff` erstellen:
    ```sh
    sudo adduser fernzugriff
    ```
3. Sudo-Rechte für `fernzugriff`:
    ```sh
    sudo usermod -aG sudo fernzugriff
    ```

## Einrichten von Docker

1. Docker installieren:
    ```sh
    curl -fsSL https://get.docker.com/ -o get-docker.sh
    sudo sh get-docker.sh
    ```
2. `fernzugriff` zur Docker-Gruppe hinzufügen:
    ```sh
    sudo usermod -aG docker fernzugriff
    ```
3. Docker-Dienst aktivieren und starten:
    ```sh
    sudo systemctl enable docker
    sudo systemctl start docker
    ```

## Deployment der Todo-Listen-Web-App

1. Verzeichnis für die Web-App erstellen und hinein navigieren:
    ```sh
    mkdir ~/todo-liste-app
    cd ~/todo-liste-app
    ```
2. Dockerfile und docker-compose in Github

3. Docker-Image bauen:
    ```sh
    sudo docker build -t todo-list-app .
    ```
4. Docker-Container starten:
    ```sh
    sudo docker run -d -p 80:5000 todo-list-app
    ```
