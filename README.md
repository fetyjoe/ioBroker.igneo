ioBroker-Adapter für Estyma Igneo

Dieser inoffizielle ioBroker-Adapter liest Messwerte und Betriebszustände einer Estyma-Igneo-Heizungssteuerung über das Portal igneo.pl aus. Er stellt die Werte als schreibgeschützte ioBroker-Datenpunkte bereit. Damit kannst du beispielsweise die Kesseltemperatur in VIS anzeigen, den Pelletverbrauch aufzeichnen oder bei einer Störung eine ioBroker-Benachrichtigung auslösen.

Funktionsumfang

Kessel: Ist- und Solltemperaturen, Rücklauf, Abgastemperatur, Außentemperatur und Betriebsmodus.

Brenner und Pellets: Brennerstatus, Brennerfreigabe, Pelletvorrat sowie Tages- und Gesamtverbrauch.

Heizkreis 1: Vorlauf, Raumtemperatur, Komfort- und Spartemperatur sowie Betriebsmodus.

Verbindung: Gerätestatus, CAN-Statuscode, Alarmanzahl und Zeitpunkte des letzten erfolgreichen Abrufs und der letzten Messwertübernahme.

Die Version 0.1.1 legt pro Instanz 25 Datenpunkte an. Welche Messwerte befüllt werden, hängt von den im Portal verfügbaren Daten der Anlage ab. Der Adapter unterstützt in dieser Version eine Gerätenummer pro Instanz und liest die Werte des ersten Heizkreises. Alle Datenpunkte sind schreibgeschützt.

Voraussetzungen

Eine Estyma-Igneo-Steuerung, deren Werte im Portal igneo.pl verfügbar sind.

Ein Igneo-Konto mit Zugriff auf das gewünschte Gerät.

HTTPS-Zugriff auf igneo.pl vom ioBroker-Host aus.

Node.js ab Version 20, ioBroker js-controller ab Version 5.0.19 und ioBroker Admin ab Version 6.0.0. Für die grafische Installation muss deine Admin-Version den Reiter Aus Datei anbieten.

Installation

Über ioBroker Admin

Lade das Adapterpaket iobroker.igneo-0.1.1.tgz aus dem GitHub-Release herunter. Wenn du das Paket als ZIP erhalten hast, entpacke die ZIP einmal und verwende die darin enthaltene .tgz.

Öffne in ioBroker Adapter → Aus eigener Quelle installieren → Aus Datei. Aktiviere bei Bedarf den Expertenmodus, damit die Schaltfläche angezeigt wird.

Wähle die .tgz und starte die Installation. Lass Instanz erstellen aktiviert.

Öffne unter Instanzen die Einstellungen von igneo.0.

Über die Konsole

Kopiere die .tgz in das Home-Verzeichnis auf dem ioBroker-Host. Die folgenden Befehle gelten für eine Linux-Installation in /opt/iobroker. Passe den Pfad bei Bedarf an. Führe iobroker add igneo bei einer Erstinstallation aus, um die Instanz anzulegen.

cd /opt/iobroker
npm install "$HOME/iobroker.igneo-0.1.1.tgz"
iobroker add igneo

Die Installationswege sind in der ioBroker-Admin-Dokumentation und in der ioBroker-CLI-Dokumentation beschrieben.

Einrichtung

Einstellung

Eingabe

Igneo-Benutzername oder E-Mail

Der Benutzername oder die E-Mail-Adresse, mit der du dich auf igneo.pl anmeldest

Igneo-Passwort

Das Passwort für dein Igneo-Konto. ioBroker speichert es verschlüsselt.

Gerätenummer

Der numerische Teil der URL hinter /device/ auf der Geräteseite im Igneo-Portal

Abrufintervall

Standardmäßig 60 Sekunden. Einstellbar von 30 bis 3.600 Sekunden.

Speichere die Einstellungen und aktiviere die Instanz. Unter Objekte → igneo → 0 findest du die Datenpunkte der ersten Instanz. igneo.0.info.connection zeigt nach einem erfolgreichen Abruf true an. Unter igneo.0.info.lastTelemetry siehst du, ob bereits ein Heizungswert übernommen wurde.

Datenpunkte

Die vollständige ID beginnt bei der ersten Instanz mit igneo.0.. Bei weiteren Instanzen steht anstelle der 0 die jeweilige Instanznummer.

Kessel

Datenpunkt

Bedeutung

Einheit oder Typ

boiler.actualTemperature

Gemessene Kesseltemperatur

°C

boiler.targetTemperature

Berechnete Kessel-Solltemperatur

°C

boiler.manualTargetTemperature

Manuell eingestellte Kessel-Solltemperatur

°C

boiler.returnTemperature

Kesselrücklauftemperatur

°C

boiler.exhaustTemperature

Abgastemperatur

°C

boiler.outdoorTemperature

Temperatur des Außenfühlers der Heizung

°C

boiler.controllerOn

Regler eingeschaltet

Boolean

boiler.mode

Betriebsmodus des Kessels

Text

Brenner und Pellets

Datenpunkt

Bedeutung

Einheit oder Typ

burner.status

Brennerstatus laut Portal

Text

burner.enabled

Brennerfreigabe

Boolean

pellets.tank

Pelletvorrat im Behälter

kg

pellets.today

Pelletverbrauch seit Tagesbeginn laut Portal

kg

pellets.total

Gesamtverbrauch laut Zähler der Steuerung

kg

Der Tagesverbrauch ergibt sich aus der Differenz zwischen dem aktuellen Gesamtzähler und dem Tagesanfangswert aus dem Portal. Beide Werte müssen verfügbar sein.

Heizkreis 1

Datenpunkt

Bedeutung

Einheit oder Typ

heatingCircuit1.flowTemperature

Gemessene Vorlauftemperatur

°C

heatingCircuit1.roomTemperature

Gemessene Raumtemperatur

°C

heatingCircuit1.comfortTemperature

Komfort-Solltemperatur

°C

heatingCircuit1.economyTemperature

Spar-Solltemperatur

°C

heatingCircuit1.mode

Betriebsmodus des Heizkreises

Text

Geräteverbindung und Diagnose

Datenpunkt

Bedeutung

Einheit oder Typ

device.online

Gerät online laut Portal. Ein verfügbarer CAN-Status fließt mit ein.

Boolean

device.canStatus

CAN-Statuscode aus dem Portal

Zahl

device.alarms

Anzahl der vom Portal gemeldeten Alarme

Zahl

info.connection

Portalabruf erfolgreich

Boolean

info.lastSuccess

Zeitpunkt des letzten erfolgreichen Portalabrufs

Unix-Zeit in ms

info.lastTelemetry

Zeitpunkt der letzten Übernahme mindestens eines Heizungswerts

Unix-Zeit in ms

info.error

Meldung zum letzten Abruffehler. Nach einem erfolgreichen Abruf ist der Wert leer.

Text

Verwendung in VIS 1

Die Datenpunkte lassen sich direkt in einem VIS-1-HTML-Widget verwenden.

<div>Kessel: {igneo.0.boiler.actualTemperature} °C</div>
<div>Vorlauf: {igneo.0.heatingCircuit1.flowTemperature} °C</div>
<div>Pelletvorrat: {igneo.0.pellets.tank} kg</div>
<div>Verbrauch heute: {igneo.0.pellets.today} kg</div>
<div>Brenner: {igneo.0.burner.status}</div>

Für Diagramme kannst du die gewünschten Datenpunkte zusätzlich mit einem ioBroker-History- oder Datenbankadapter aufzeichnen.

Anmeldung und Fehlerdiagnose

Der Adapter verwendet den regulären Login von igneo.pl und hält die Sitzung im Arbeitsspeicher. Wenn er beim Abruf einen Sitzungsablauf erkennt, meldet er sich erneut an und wiederholt den Datenabruf. Bei Fehlern verlängert er die Wartezeit bis zum nächsten Versuch vorübergehend. Nach einem fehlgeschlagenen Login wartet er mindestens fünf Minuten.

Beobachtung

Prüfung

info.connection ist false

Lies info.error und das ioBroker-Log. Prüfe den Portalzugriff vom ioBroker-Host sowie die Zugangsdaten und Gerätenummer.

info.connection ist true, device.online ist false

Das Portal antwortet. Prüfe den Gerätestatus auf igneo.pl und den Wert von device.canStatus.

Ein einzelner Messwert ist leer oder veraltet

Prüfe, ob die Geräteseite den zugehörigen Sensorwert liefert. Fehlende Messwerte lassen den letzten empfangenen Wert bestehen.

Die Heizungswerte werden nicht mehr übernommen

Vergleiche info.lastSuccess mit info.lastTelemetry und prüfe device.online.

Der Adapter hängt vom Estyma-Igneo-Portal ab. Änderungen am Login oder an der Datenantwort können ein Adapter-Update erforderlich machen. Bei einer Fehlermeldung im GitHub-Issue helfen die Adapterversion, die Versionen von Admin und js-controller, das Steuerungsmodell und der relevante Log-Auszug. Entferne vor dem Posten Zugangsdaten und die persönliche Gerätenummer.
