## <a name="prepare-your-raspberry-pi"></a>Uw Raspberry Pi voorbereiden

### <a name="install-raspbian"></a>Raspbian installeren

Als dit de eerste keer dat u uw Pi frambozen gebruikt, moet u het Raspbian-besturingssysteem met behulp van NOOBS op de SD-kaart die is opgenomen in de kit installeren. De [frambozen Pi Software handleiding] [ lnk-install-raspbian] wordt beschreven hoe u een besturingssysteem installeren op uw frambozen Pi. Deze zelfstudie wordt ervan uitgegaan dat u het besturingssysteem Raspbian op uw Pi frambozen hebt geïnstalleerd.

> [!NOTE]
> De SD-kaart die is opgenomen in de [Microsoft Azure IoT Starter Kit voor frambozen Pi 3] [ lnk-starter-kits] al NOOBS geïnstalleerd. U kunt de Pi frambozen van deze kaart opstart en installeren van het besturingssysteem Raspbian.

### <a name="set-up-the-hardware"></a>De hardware instellen

Deze zelfstudie wordt gebruikgemaakt van de sensor BME280 is opgenomen in de [Microsoft Azure IoT Starter Kit voor frambozen Pi 3] [ lnk-starter-kits] telemetrische gegevens genereren. Een LED wordt gebruikt om aan te geven wanneer de Pi frambozen verwerkt een methodeaanroep vanuit het dashboard van oplossing.

De onderdelen op het mededelingenbord brood zijn:

- Rode LED
- 220 Ohm weerstand (rood, rood, bruine)
- BME280-temperatuursensor

Het volgende diagram toont hoe u verbinding maakt van uw hardware:

![Hardware-instellingen voor frambozen Pi][img-connection-diagram]

De volgende tabel geeft een overzicht van de verbindingen van de Pi frambozen aan de onderdelen op de breadboard:

| Raspberry Pi            | Breadboard             |Kleur         |
| ----------------------- | ---------------------- | ------------- |
| GND (Pin 14)            | LED - ve pincode (18 bis)      | Paars          |
| GPCLK0 (pincode 7)          | Weerstand (25 bis)         | Orange          |
| SPI_CE0 (Pin 24)        | CS (39A)               | Blauw          |
| SPI_SCLK (Pin 23)       | SCK (36A)              | Geel        |
| SPI_MISO (Pin 21)       | SDO (37 BIS)              | Wit         |
| SPI_MOSI (Pin 19)       | SDI (38 BIS)              | Groen         |
| GND (pincode 6)             | GND (35A)              | Zwart         |
| 3.3 V (pincode 1)           | 3Vo (34 bis)              | Rood           |

U voltooit de installatie van de hardware, moet u:

- Verbinding maken met uw frambozen-Pi de voeding die is opgenomen in het pakket.
- Verbinding maken met uw Pi frambozen via de Ethernet-kabel in uw kit opgenomen met het netwerk. U kunt ook kunt u instellen [draadloze netwerkverbinding] [ lnk-pi-wireless] voor uw frambozen Pi.

U hebt nu de hardware-instellingen van uw Pi frambozen voltooid.

### <a name="sign-in-and-access-the-terminal"></a>Aanmelden en toegang tot de terminal

U hebt twee opties voor toegang tot een terminal omgeving op uw Pi frambozen:

- Als u een toetsenbord en de monitor die zijn verbonden met uw Pi frambozen hebt, kunt u de gebruikersinterface van Raspbian voor toegang tot een terminalvenster.

- Toegang tot de opdrachtregel op uw frambozen Pi gebruik van SSH op uw computer.

#### <a name="use-a-terminal-window-in-the-gui"></a>Gebruik een terminalvenster in de gebruikersinterface

De standaardreferenties voor Raspbian zijn gebruikersnaam **pi** en het wachtwoord **frambozen**. U kunt starten in de taakbalk in de gebruikersinterface van de **Terminal** hulpprogramma met het pictogram dat op een monitor lijkt.

#### <a name="sign-in-with-ssh"></a>Meld u aan met SSH

U kunt SSH gebruiken voor vanaf de opdrachtregel toegang tot uw frambozen Pi. Het artikel [SSH (Secure Shell)] [ lnk-pi-ssh] wordt beschreven hoe SSH configureren op uw Pi frambozen en verbinding maken van [Windows] [ lnk-ssh-windows] of [Linux en Mac OS][lnk-ssh-linux].

Aanmelden met gebruikersnaam **pi** en het wachtwoord **frambozen**.

#### <a name="optional-share-a-folder-on-your-raspberry-pi"></a>Optioneel: Een map op uw Pi frambozen delen

U kunt desgewenst een map op uw Pi frambozen delen met uw bureaublad omgeving. Delen van een map, kunt u gebruikmaken van uw voorkeur bureaublad teksteditor (zoals [Visual Studio Code](https://code.visualstudio.com/) of [Sublime Text](http://www.sublimetext.com/)) voor het bewerken van bestanden op uw Pi frambozen in plaats van `nano` of `vi`.

Als u wilt delen een map met Windows, configureert u een Samba-server op de frambozen Pi. U kunt ook gebruik van de ingebouwde [SFTP](https://www.raspberrypi.org/documentation/remote-access/) server met een SFTP-client op het bureaublad.

### <a name="enable-spi"></a>SPI inschakelen

Voordat u de voorbeeldtoepassing uitvoeren kunt, moet u de seriële randapparatuur Interface (SPI)-bus van de Pi frambozen inschakelen. De Pi frambozen communiceert met het apparaat van de sensor BME280 via de SPI-bus. Gebruik de volgende opdracht in het configuratiebestand bewerken:

```sh
sudo nano /boot/config.txt
```

Zoek de regel:

`#dtparam=spi=on`

- Om de opmerkingen bij de regel, verwijder de `#` aan het begin.
- Sla de wijzigingen (**Ctrl-O**, **Enter**) en sluit de editor af (**Ctrl X**).
- Start opnieuw op de frambozen Pi zodat SPI. Opnieuw wordt opgestart nadat de verbinding verbreekt de terminal, moet u aan te melden opnieuw wanneer de Pi frambozen opnieuw wordt gestart:

  ```sh
  sudo reboot
  ```


[img-connection-diagram]: media/iot-suite-v1-raspberry-pi-kit-prepare-pi/rpi2_remote_monitoring.png

[lnk-install-raspbian]: https://www.raspberrypi.org/learning/software-guide/quickstart/
[lnk-pi-wireless]: https://www.raspberrypi.org/documentation/configuration/wireless/README.md
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/