## <a name="prepare-your-raspberry-pi"></a>Uw Raspberry Pi voorbereiden

### <a name="install-raspbian"></a>Raspbian installeren

Als dit de eerste keer dat u uw Pi frambozen gebruikt, moet u het Raspbian-besturingssysteem met behulp van NOOBS op de SD-kaart die is opgenomen in de kit installeren. De [frambozen Pi Software handleiding] [ lnk-install-raspbian] wordt beschreven hoe u een besturingssysteem installeren op uw frambozen Pi. Deze zelfstudie wordt ervan uitgegaan dat u het besturingssysteem Raspbian op uw Pi frambozen hebt geïnstalleerd.

> [!NOTE]
> De SD-kaart die is opgenomen in de [Microsoft Azure IoT Starter Kit voor frambozen Pi 3] [ lnk-starter-kits] al NOOBS geïnstalleerd. U kunt de Pi frambozen van deze kaart opstart en installeren van het besturingssysteem Raspbian.

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

[lnk-install-raspbian]: https://www.raspberrypi.org/learning/software-guide/quickstart/
[lnk-pi-wireless]: https://www.raspberrypi.org/documentation/configuration/wireless/README.md
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/