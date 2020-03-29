# Arduino Nano

Der Arduino Nano ist auf der Extension Platine für das Autosquaring und Auslesen der Temperatur Sensoren verantwortlich. 

{% hint style="info" %}
Braucht man weder Autosquaring noch Temperatur Fühler kann der Arduino Nano auch weggelassen werden. Das Board mit Bedienpanel funktioniert trotzdem.
{% endhint %}

## Firmware

Die Firmware kann hier heruntergeladen werden. Diese muss dann mit der Arduino Software aufgespielt werden. Danach kann der Arduino bedenkenlos in die Extension Platine eingesetzt werden.

Benötige Libraries\(Können in der Arduino IDE unter Sketch → Bibliothek einbinden → Bibliotheken verwalten hinzugefügt werden\):

* [U8X8 von Olikraus](https://github.com/olikraus/u8g2/)

[Firmware](https://github.com/taltholtmann/documentation/blob/master/Firmware.ino)\(Einfach in die Arduino IDE kopieren\)

### Anpassungen / Einstellungen

In den ersten Zeilen der Firmware können noch Einstellungen vorgenommen werden, sofern das gewünscht ist. Hier ist eigentlich alles mit Kommentaren erklärt.

```text
// Programm configuration
uint16_t stepDelay = 0; // delay between steps in microseconds if no poti is connected - set to 0 to use a poti for setting speed
uint16_t startDelay = 1000; // delay betwenn pressing the autosquare button and starting autosquare in miliseconds, should be at least 500 
byte speedPotiSamples = 10; // Samples of every speed poti reading
uint16_t speedPotiMin = 10;  // min delay between steps in microseconds
uint16_t speedPotiMax = 5000; // min delay between steps in microseconds
```



## Autosquaring

### Erklärung

Der Arduino Nano übernimmt die Kontrolle über die Stepper Motoren, wenn der Autosquaring Knopf/Taster gedrückt wird. Das Ganze hat eine Zeitverzögerung, damit man nicht direkt alle Achseneinstellungen unbrauchbar macht, wenn man da zufällig mal dran kommt. Bedeutet man drückt den Taster, wartet kurz und dann beginnt das Autosquaring. Für welche Achsen Autosquaring durchgeführt werden soll und die Drehrichtung lassen sich über Jumper konfigurieren. Man muss den Taster so lange gedrückt halten, bis alle Endstopps angefahren wurden.  
Der aktuelle Status wird dabei über das Display ausgegeben.

### Anschluss der Endstopps

Für das Autosquaring braucht jeder Stepper Motor einen eigenen Endstopp Schalter. Diese müssen wie folgt angeschlossen werden:

| Eingang | Achse |
| :--- | :--- |
| IN5 | X1 |
| IN6 | X2 |
| IN7 | Y1 |
| IN8 | Y2 |

### Estlcam

Damit Estlcam nicht gleichzeitig versucht die Motoren zu steuern\(passiert eigentlich nur durch Bedienfehler\) kann der Eingang IN9 genutzt werden. Dazu muss der entsprechende Jumper auf der Platine gesetzt sein - JP8. Danach kann man diesen in Estlcam bei den Eingängen z.B. als Endstopp konfigurieren. Wird Autosquare nun gestartet löst der Eingang aus und Estlcam stoppt die Steuerung, da es denkt, ein Endstopp wurde ausgelöst.



