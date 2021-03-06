title: WLAN-Sicherheit
author:
    name: Daniel Seiller
    email: seiller@kph.uni-mainz.de
controls: false
theme: sjaakvandenberg/cleaver-dark
output: WLAN-Sicherheit.html
--

#WLAN-Sicherheit
--

###Inhalt
1. Einführung
    1. Funktionsweise von WLAN
    2. Möglichkeiten zur Verschlüsselung
2. Sicherheit von WEP
    1. Funktionsweise
    2. Angriffe
3. Sicherheit von WPA
    1. Funktionsweise
    2. Angriffe
--

### Funktionsweise von WLAN
* Ein WLAN-Netzwerk besteht aus einer (oder mehreren) *Basisstation/-en* (Access-Point)
 und mehreren *Clients* (Laptop, Smarthphone, Tablet, etc)
* Clients bauen eine Verbindung zum Access-Point auf und authentifizieren sich ggf.
* Nach erfolgreicher Authentifizierung ist der Client am Access-Point angemeldet und kann über das Netzwerk (TCP/IP) kommunizieren
--

### Möglichkeiten zur Verschlüsselung

* Open (keine Authentifizierung)
    * Wird meistens mit Portal-Seite/Captive-Portal kombiniert

* WEP (Wired Equivalent Privacy)
    * Benutzt RC4 als Verschlüsselungs-Algorithmus
    * Unsicher
--

### Möglichkeiten zur Verschlüsselung (Fortsetzung)

* WPA/WPA2 (Wi-Fi Protected Access)
    * Benutzt auch RC4
    * Schlüssel wird in regelmäßigen Abständen neu erzeugt
    * Integrität von Paketen wird geprüft

* WPA2 (Wi-Fi Protected Access II)
    * Verbesserung von WPA
    * Benutzt AES mit wechselndem Schlüssel (TKIP) oder AES

* WPA2-Enterprise
    * Verwendet einen RADIUS-Server zur Authentifizierung mit Username und Passwort
--

### Zusätzliche Sicherungsmöglichkeiten

* MAC-Adressfilter
    * Blacklist
    * Whitelist

* ESSID verstecken
    * macht es schwieriger das WLAN zu finden
--

### Details zu WEP

* Mögliche Shlüssellängen (24-bit IV + Schlüssel)
    * 64-bit (40-bit)
    * 128-bit (104-bit)
    * 152-bit (128-bit) (manche Hersteller)
    * 256-bit (232-bit) (manche Hersteller)

* Authentifizierungs-Modi
    * Open-System (keine Authentifizierung nur Verschlüsselung)
    * Shared Key (Challenge-Response mit WEP-Schlüssel)
--

### Sicherheit von WEP

<img src=wep.gif style="width: 40%; height: 40%">
--

### Angriffe auf WEP

* Shared Key Authentifizierung
    * XOR-Angriff
        * Access-Point schickt Challenge1
        * Client berechnet Ciphertext1=E(IV1,Challenge1)=Challenge1 &#8853; Keystream1
        * Client schickt IV1+Ciphertext1 zurück
        * Angreifer berechnet Challenge1 &#8853; Ciphertext1 = Keystream1
--

### Angriffe auf RC4 PRNG

* ARP-Replay Angriff
    * abgehörtes ARP-Paket wird an Access-Point zurückgesendet
    * Access-Point antwortet mit neue ARP-Paket und neuem IV
    * Solange wiederholen biss ausreichen IVs gesammelt wurden

* Nachdem ausreichend Pakete gesammelt wurden (500.000 - 6.000.000)
    * PTW Angriff auf 40-bit und 104-bit Schlüssel
    * FMS/KoReK Angriff auf alles andere
    * beide Angriffe basieren auf statistischer Analyse des Schlüsselstroms
--

### Sicherheit von WPA2

* Verschlüsselung baut auf AES auf
    * TKIP wird verwendet um den Schlüssel regelmäßigen zu ändern
    * PBKDF2 wird verwendet um den "Master-Key-Hash" aus Password und ESSID abzuleiten

* 4-Way-Handshake
    * Wird zum Schlüsselaustausch verwendet (nur der mit PBKDF2 berechnete "Master-Key-Hash" wird übertragen)

* WPS
    * Anmeldung im WLAN mit 7 oder 8 stelligem PIN
--

### Sicherheit von WPA/WPA2 mit WPS

<img src=wpa_wps.png style="width: 50%; height: 50%">
--

### Angriffe auf WPA2

* Deauth Angriff
    * Authentifizierte Client könne durch Paket-Injektion vom Access-Point getrennt werden
    * beim Wiederverbinden kann der 4-Way-Handshake abgefangen werden
    * Knacken des Master-Key-Hashes mittels Bruteforce (kann sehr lange dauern)

* WPS
    * WPS-PIN mit Bruteforce knacken (2x10000 an stelle von 10000x10000 möglichen PINS)
    * Pixiedust Angriff (funktioniert nicht immer)
    * Wenn Angriff erfolgreich ist kann der WPA-PSK ausgelesen werden
--

### Angriffe auf WPA2 (Fortsetzung)

* Evil-Twin-Angriff
    * Eigenen Access-Point mit den selben Einstellungen wie der zu knackende AP erstellen
    * ggf Fake-Router-Konfigurations-Seite erstellen, welche nach WLAN-Password fragt
    * bei WPA2 Enterprise eigenen FreeRADIUS WPE (Wireless Pwnage Edition) Server aufsetzen
    * Dann Challenge-Response Abfangen und per Bruteforce knacken
--

### Angriffe auf MAC-Filter

* MAC-Adress-Spoofing
    * MAC-Adresse eines legitimen Clients kopieren
    * Eigene MAC-Adresse zu der kopierten ändern

* MAC-Adress-Bruteforce
    * So lange MAC-Adressen ausprobieren bis man eine nicht blockierte Adresse gefunden hat
--

### Wie kann man sich schützen?

* WPA2 benutzen
* WPS abschalten
* Ein starkes Passwort verwenden
* Schauen ob man mit dem richtigen Access-Point verbunden ist

--

# Live Demo
## Angriff auf WEP-Netzwerk via ARP-Replay und PTW-Angriff
--

#Fragen?
--

# Quellen

* https://de.wikipedia.org/wiki/Wired_Equivalent_Privacy
* https://en.wikipedia.org/wiki/Wi-Fi_Protected_Access
* http://aircrack-ng.org/doku.php?id=aircrack-ng
* https://de.wikipedia.org/wiki/Wi-Fi_Protected_Setup
* http://phreaklets.blogspot.de/2013/06/cracking-wireless-networks-protected.html
