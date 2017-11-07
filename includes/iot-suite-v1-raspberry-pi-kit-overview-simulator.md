## <a name="overview"></a>Overzicht

In deze zelfstudie maakt uitvoeren u de volgende stappen:

- Implementeer een exemplaar van de vooraf geconfigureerde oplossing voor externe controle op uw Azure-abonnement. Deze stap implementeert automatisch en meerdere Azure-services configureert.
- Instellen van uw apparaat om te communiceren met uw computer en de oplossing voor externe controle.
- Werk de voorbeeldcode van de apparaten verbinding maken met de oplossing voor externe controle en verzenden van de gesimuleerde telemetrie die u op het dashboard van de oplossing weergeven kunt.

## <a name="prerequisites"></a>Vereisten

U hebt een actief Azure-abonnement nodig om deze zelfstudie te voltooien.

> [!NOTE]
> Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure][lnk-free-trial] voor meer informatie.

### <a name="required-software"></a>Vereiste software

SSH-client moet u op de computer waarmee u kunt extern toegang tot de opdrachtregel op de frambozen Pi.

- Windows bevat geen een SSH-client. Wordt u aangeraden [PuTTY](http://www.putty.org/).
- De meeste Linux-distributies en Mac OS omvatten het SSH-opdrachtregelprogramma. Zie voor meer informatie [SSH met behulp van Linux- of Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).

### <a name="required-hardware"></a>Vereiste hardware

Een desktopcomputer zodat u kunt extern verbinding maken met de opdrachtregel op de frambozen Pi.

[Microsoft IoT Starter Kit voor frambozen Pi 3] [ lnk-starter-kits] of gelijkwaardige onderdelen. Deze zelfstudie maakt gebruik van de volgende items van de kit:

- Raspberry Pi 3
- MicroSD-kaart (met NOOBS)
- Een Mini USB-kabel
- Een Ethernet-kabel

[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/