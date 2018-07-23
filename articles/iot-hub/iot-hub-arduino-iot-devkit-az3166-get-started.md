---
title: IoT DevKit naar de cloud--IoT DevKit AZ3166 verbindt met Azure IoT Hub | Microsoft Docs
description: In deze zelfstudie leert u hoe u instelt en verbindt IoT DevKit AZ3166 met Azure IoT Hub, zodat het verzenden van gegevens naar de Azure-cloud-platform.
author: rangv
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: cf9ee5339c53eb4f9c74f6b5f251a7963555d676
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37928746"
---
# <a name="connect-iot-devkit-az3166-to-azure-iot-hub-in-the-cloud"></a>IoT DevKit AZ3166 verbinden met Azure IoT Hub in de cloud

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

U kunt de [MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) te ontwikkelen en prototype Internet of Things (IoT)-oplossingen die van Microsoft Azure-services profiteren. Het bevat een bord Arduino-compatibel met uitgebreide randapparatuur en sensoren, een open-source-bord-pakket en een groeiend [projecten catalogus](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/).

## <a name="what-you-do"></a>Wat u allemaal doen
Verbinding maken met de [DevKit](https://microsoft.github.io/azure-iot-developer-kit/) naar een Azure-IoT-hub die u maakt, de temperatuur en vochtigheid gegevens van sensoren te verzamelen en verzenden van de gegevens naar de IoT-hub.

Heb je nog een DevKit? Probeer [DevKit simulator](https://azure-samples.github.io/iot-devkit-web-simulator/) of [download er eentje](https://aka.ms/iot-devkit-purchase).

## <a name="what-you-learn"></a>Wat u leert

* Hoe u de IoT DevKit verbinden met een draadloos toegangspunt en uw ontwikkelomgeving voorbereiden.
* Over het maken van een IoT-hub en een apparaat registreren voor de MXChip IoT DevKit.
* Klik hier voor meer informatie over het verzamelen van sensorgegevens door het uitvoeren van een voorbeeld van toepassing op de MXChip IoT DevKit.
* Klik hier voor meer informatie over het verzenden van de sensorgegevens naar uw IoT-hub.

## <a name="what-you-need"></a>Wat u nodig hebt

* Een bord MXChip IoT DevKit met een Micro-USB-kabel. [Nu downloaden](https://aka.ms/iot-devkit-purchase).
* Een computer met Windows 10- of macOS 10.10 +.
* Een actief Azure-abonnement. [Activeren van een gratis 30-daagse proefversie Microsoft Azure-account](https://azureinfo.microsoft.com/us-freetrial.html).
  

## <a name="prepare-your-hardware"></a>Bereid uw hardware

Koppelt de hardware op de computer.

U moet deze hardware:

* DevKit bord
* Micro-USB-kabel

![Vereiste hardware](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/hardware.jpg)

De DevKit verbinden met uw computer:

1. Het einde van de USB-verbinding naar uw computer.
2. Het einde Micro-USB verbinden met de DevKit.
3. De groene LED voor energiebeheer wordt bevestigd dat de verbinding.

![Hardwareverbindingen](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect.jpg)

## <a name="configure-wi-fi"></a>Wi-Fi configureren

IoT-projecten zijn afhankelijk van verbinding met internet. Gebruik de volgende instructies voor het configureren van de DevKit verbinding maken met Wi-Fi.

### <a name="enter-ap-mode"></a>Azië en Stille Oceaan-modus

Houd ingedrukt B, push en release van de knop opnieuw instellen en loslaat B. Uw DevKit in Azië en Stille Oceaan-modus geplaatst voor het configureren van Wi-Fi. Het scherm wordt weergegeven de serviceset-id (SSID) van de DevKit en de IP-adres van de configuratie-portal.

![Knop, knop B en SSID opnieuw instellen](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ap.jpg)

### <a name="connect-to-devkit-ap"></a>Verbinding maken met DevKit Azië en Stille Oceaan

Gebruik een ander apparaat Wi-Fi ingeschakeld (computer of mobiele telefoon) nu verbinding maken met de DevKit SSID (gemarkeerd in de vorige afbeelding). Het wachtwoord leeg laten.

![Informatie over het netwerk en de knop verbinding maken](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect-ssid.png)

### <a name="configure-wi-fi-for-the-devkit"></a>Wi-Fi configureren voor de DevKit

Het IP-adres wordt weergegeven op het scherm DevKit op uw computer of mobiele telefoon browser openen, selecteert u het Wi-Fi-netwerk dat u de DevKit wilt verbinden met en typ het wachtwoord. Selecteer **Verbinden**.

![Wachtwoord en de knop verbinding maken](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-portal.png)

Wanneer de verbinding slaagt, wordt de DevKit opnieuw wordt opgestart in een paar seconden. Vervolgens ziet u de naam Wi-Fi- en IP-adres op het scherm:

![Naam Wi-Fi- en IP-adres](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ip.jpg)

> [!NOTE] 
> Het IP-adres weergegeven in de foto mogelijk niet overeen met het IP-adres toegewezen en wordt weergegeven op het scherm DevKit. Dit is normaal, omdat het Wi-Fi DHCP gebruikt voor het IP-adressen dynamisch toewijzen.

Nadat Wi-Fi is geconfigureerd, wordt uw referenties op het apparaat voor die verbinding behouden, zelfs als het apparaat losgekoppeld wordt. Bijvoorbeeld, als u de DevKit voor Wi-Fi op uw startpagina configureren en vervolgens de DevKit duren voordat het kantoor, moet u Azië en Stille Oceaan-modus (te beginnen bij de stap in de sectie 'Voer Azië en Stille Oceaan modus') als u wilt de DevKit verbinden met uw kantoor Wi-Fi configureren. 

## <a name="start-using-the-devkit"></a>Start met behulp van de DevKit

De standaard-app die wordt uitgevoerd op de DevKit controleert of de nieuwste versie van de firmware en sommige sensorgegevens diagnose wordt voor u.

### <a name="upgrade-to-the-latest-firmware"></a>Een upgrade uitvoert naar de meest recente firmware

> [!NOTE] 
> Sinds v1.1 kan DevKit ST-SAFE in bootloader. U moet firmware bijwerken als u worden uitgevoerd onder v1.1 zodat deze waarschijnlijk werken.

Als u een upgrade nodig, ziet het scherm u de huidige en de meest recente firmware-versies. Als u wilt bijwerken, gaat u als volgt de [firmware bijwerken](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/) handleiding.

![Weergave van de huidige en de meest recente firmware-versies](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/firmware.jpg)

> [!NOTE] 
> Dit is een eenmalige taak. Nadat u een begin met ontwikkelen in de DevKit en uw app downloaden, komen de meest recente firmware met uw app.

### <a name="test-various-sensors"></a>Testen van verschillende sensoren

Druk op de knop B voor het testen van sensoren. Indrukken en loslaten van de knop B om te bladeren door elke sensor blijven.

![Knop B en sensor weergeven](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/sensors.jpg)

## <a name="prepare-the-development-environment"></a>De ontwikkelomgeving voorbereiden

Nu is het tijd om de ontwikkelomgeving instellen: hulpprogramma's en -pakketten voor u om verbluffende IoT-toepassingen te bouwen. U kunt de Windows- of Mac OS-versie kiezen op basis van uw besturingssysteem.

### <a name="windows"></a>Windows

We raden u aan de van een installatiepakket gebruiken om de ontwikkelomgeving voorbereiden. Als u problemen ondervindt, voert u de [handmatige stappen](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/) zodat deze klaar.

#### <a name="download-the-latest-package"></a>De meest recente pakket downloaden

Het ZIP-bestand dat u hebt gedownload, bevat de benodigde hulpprogramma's en pakketten voor de ontwikkeling van DevKit.

> [!div class="button"]
[Downloaden](https://aka.ms/devkit/prod/installpackage/latest)

Het ZIP-bestand bevat de volgende hulpprogramma's en pakketten. Als u al bepaalde onderdelen geïnstalleerd hebt, wordt het script opsporen en ze overslaan.

* Node.js en Yarn: Runtime voor de setup-script en geautomatiseerde taken.
* [Azure CLI 2.0-MSI](https://docs.microsoft.com//cli/azure/install-azure-cli#windows): opdrachtregelervaring van Cross-platform voor het beheren van Azure-resources. Het MSI-bestand bevat de afhankelijke Python en pip.
* [Visual Studio Code](https://code.visualstudio.com/) (VS-Code): lichtgewicht code-editor voor de ontwikkeling van DevKit.
* [Visual Studio Code-extensie voor Arduino](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino): extensie waarmee Arduino-ontwikkeling in Visual Studio Code.
* [Arduino IDE](https://www.arduino.cc/en/Main/Software): hulpprogramma dat afhankelijk van de extensie voor Arduino.
* Pakket voor DevKit bord: Hulpprogramma ketens, bibliotheken en projecten voor de DevKit.
* Hulpprogramma voor ST-koppeling: Essentiële hulpprogramma's en stuurprogramma's.

#### <a name="run-the-installation-script"></a>Voer het script voor installatie

In Windows Verkenner, Ga naar het zipbestand en pak het uit. Zoek `install.cmd`, met de rechtermuisknop en selecteer **als administrator uitvoeren**.

![Verkenner](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/run-admin.png)

Tijdens de installatie ziet u de voortgang van elk programma of het pakket.

![Voortgang van installatie](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install.png)

> [!NOTE] 
> Afhankelijk van uw omgeving, ontvangt soms ook u een fout bij het installeren van Arduino IDE. In dit geval kunt u proberen [Arduino IDE afzonderlijk installeren](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/#windows) en voer install.cmd opnieuw uit. Volg anders de [handmatige stappen](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/#windows) voor het installeren van alle benodigde hulpprogramma's en pakketten.

#### <a name="install-drivers"></a>Stuurprogramma's installeren

De VS Code voor Arduino-extensie is afhankelijk van de Arduino IDE. Als dit de eerste keer dat u de Arduino IDE installeert, wordt u gevraagd relevante stuurprogramma's te installeren:

![ophalen van-aan de slag-stuurprogramma](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/driver.png)

De installatie duurt ongeveer 10 minuten, afhankelijk van de snelheid van uw internet. Nadat de installatie voltooid is, ziet u Visual Studio Code en Arduino IDE snelkoppelingen op het bureaublad.

> [!NOTE] 
> Af en toe wanneer u VS Code start, u wordt gevraagd met een fout niet kan vinden het Arduino IDE of gerelateerde Bord pakket. Op te lossen, VS Code sluiten en opnieuw starten van de Arduino IDE. VS Code moet vervolgens Ga naar het pad met Arduino IDE.

### <a name="macos"></a>macOS

We raden u met één muisklik installatie-ervaring die u kunt de ontwikkelomgeving voorbereiden. Als u problemen ondervindt, voert u de [handmatige stappen](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/) zodat deze klaar.

#### <a name="install-homebrew"></a>Installeer Homebrew

> [!NOTE] 
> Als u Homebrew hebt geïnstalleerd, kunt u deze stap overslaan.

Ga als volgt de [Homebrew-installatie-instructies](https://docs.brew.sh/Installation.html) om deze te installeren.

#### <a name="download-the-latest-package"></a>De meest recente pakket downloaden
Het ZIP-bestand dat u hebt gedownload, bevat de benodigde hulpprogramma's en pakketten voor de ontwikkeling van DevKit.

> [!div class="button"]
[Downloaden](https://aka.ms/devkit/prod/installpackage/mac/latest)

Het ZIP-bestand bevat de volgende hulpprogramma's en pakketten. Als u al bepaalde onderdelen geïnstalleerd hebt, wordt het script opsporen en ze overslaan.

* Node.js en Yarn: Runtime voor de setup-script en geautomatiseerde taken.
* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest#a-namemacosinstall-on-macos): opdrachtregelervaring van Cross-platform voor het beheren van Azure-resources.
* [Visual Studio Code](https://code.visualstudio.com/) (VS-Code): lichtgewicht code-editor voor de ontwikkeling van DevKit.
* [Visual Studio Code-extensie voor Arduino](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino): extensie waarmee Arduino-ontwikkeling in Visual Studio Code.
* [Arduino IDE](https://www.arduino.cc/en/Main/Software): hulpprogramma dat afhankelijk van de extensie voor Arduino.
* Pakket voor DevKit bord: Hulpprogramma ketens, bibliotheken en projecten voor de DevKit.
* Hulpprogramma voor ST-koppeling: Essentiële hulpprogramma's en stuurprogramma's.

#### <a name="run-the-installation-script"></a>Voer het script voor installatie

In de Finder, zoek het ZIP en pak het uit:

![macOS finder](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/mac-finder.png)

Definitieve app starten, Ga naar de map die u ZIP-bestand uitpakken en uitvoeren:

```bash
./install.sh
```

![macOS installeren](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/mac-install-sh.png)

> [!NOTE] 
> Als u voldoet aan de Machtigingsfout Homebrew, voert u `brew doctor` zodat deze vast. Controleer [Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#homebrew-permission-error-on-macos) voor meer informatie.

U hebt nu de benodigde hulpprogramma's en pakketten die zijn geïnstalleerd voor macOS.


## <a name="open-the-project-folder"></a>Open de projectmap

### <a name="start-vs-code"></a>Start VS Code

Zorg ervoor dat uw DevKit niet is verbonden. Start VS Code eerst en de DevKit verbinden met uw computer. VS Code vindt de DevKit automatisch en wordt een introductiepagina geopend:

![Introductiepagina](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/vscode_start.png)

> [!NOTE] 
> Af en toe wanneer u VS Code start, u wordt gevraagd met een fout niet kan vinden het Arduino IDE of gerelateerde Bord pakket. VS Code sluiten en opnieuw starten van de Arduino IDE. VS Code moet vervolgens Ga naar het pad met Arduino IDE.


### <a name="open-the-arduino-examples-folder"></a>Open de map Arduino-voorbeelden

Op de **Arduino voorbeelden** tabblad, blader naar **voorbeelden voor MXCHIP AZ3166** > **AzureIoT**, en selecteer **GetStarted**.

![Tabblad Arduino-voorbeelden](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/vscode_start.png)

Als u het deelvenster sluiten, kunt u deze opnieuw openen. Gebruik `Ctrl+Shift+P` (Mac OS: `Cmd+Shift+P`) om te openen de command palette, typt u **Arduino**, en zoek en selecteer **Arduino: voorbeelden**.

## <a name="provision-azure-services"></a>Azure-services inrichten

In het venster van de oplossing, kunt u uw taak uitvoeren via `Ctrl+P` (Mac OS: `Cmd+P`) door in te voeren `task cloud-provision`.

In de terminal van VS Code en helpt een interactieve vanaf de opdrachtregel u de vereiste Azure-services inrichten:

![Interactieve vanaf de opdrachtregel](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/cloud-provision.png)

## <a name="build-and-upload-the-arduino-sketch"></a>Bouwen en de schets Arduino uploaden

### <a name="windows"></a>Windows

1. Gebruik `Ctrl+P` om uit te voeren `task device-upload`.
2. De terminal vraagt u om in te voeren van de configuratiemodus. Om dit te doen, houdt u ingedrukt A, en vervolgens push- en release van de knop opnieuw instellen. Het scherm wordt weergegeven de DevKit-id en 'Configuration'.

Dit is het instellen van de verbindingsreeks die wordt opgehaald uit `task cloud-provision` stap.

VS Code begint vervolgens te controleren en de schets Arduino uploaden:

![Verificatie en uploaden van de schets Arduino](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/device-upload.png)

De DevKit opnieuw wordt opgestart en de code wordt gestart.

> [!NOTE] 
> In sommige gevallen krijgt u de fout ' fout: AZ3166: onbekend pakket '. Dit wordt veroorzaakt door het bord package index is niet vernieuwd. Schakel dit selectievakje [Veelgestelde vragen over stappen](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) te op te lossen.

### <a name="macos"></a>macOS

1. DevKit in configuratiemodus geplaatst: houd knop A, en vervolgens push- en release de knop opnieuw instellen. Het scherm wordt weergegeven dat 'Configuration'.
2. Gebruik `Cmd+P` om uit te voeren `task device-upload`.

Dit is het instellen van de verbindingsreeks die wordt opgehaald uit `task cloud-provision` stap.

VS Code begint vervolgens te controleren en de schets Arduino uploaden:

![apparaat-upload](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/device-upload.png)

De DevKit opnieuw wordt opgestart en de code wordt gestart.

> [!NOTE] 
> In sommige gevallen krijgt u de fout ' fout: AZ3166: onbekend pakket '. Dit wordt veroorzaakt door het bord package index is niet vernieuwd. Schakel dit selectievakje [Veelgestelde vragen over stappen](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) te op te lossen.


## <a name="test-the-project"></a>Het project testen

Volg deze stappen om te openen en het instellen van de seriële Monitor in VS Code:

1. Klik op de `COM[X]` word op de statusbalk om in te stellen met de juiste COM-poort `STMicroelectronics`: ![com-poort](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/com-port.png)

2. Klik op stroom plug-pictogram op de statusbalk te openen van de seriële Monitor: ![serieel-monitor](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution//connect-iothub/serial-monitor.png)

3. Op de statusbalk weergegeven, klikt u op het getal dat staat voor de baudrate en ingesteld op `115200`: ![baudrate](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/baud-rate.png)

De voorbeeldtoepassing wordt uitgevoerd wanneer u de volgende resultaten ziet:

* De seriële Monitor bevat de dezelfde informatie als de inhoud in de onderstaande schermafbeelding.
* De MXChip IoT DevKit RGB-LED knippert.

![Uiteindelijke uitvoer in VS Code](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/result-serial-output.png)

## <a name="problems-and-feedback"></a>Problemen en feedback

Als u problemen ondervindt, kunt u vinden [Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/). U kunt ons ook feedback geven door een opmerking op deze pagina verlaten.

## <a name="next-steps"></a>Volgende stappen

U hebt een MXChip IoT DevKit is verbonden met uw IoT-hub en u hebt de vastgelegde gegevens verzonden naar uw IoT-hub.

Zie om door te gaan aan de slag met Azure IoT Hub en andere IoT-scenario's verkennen:

- [Berichten op cloudapparaten beheren met iothub-explorer](https://docs.microsoft.com/azure/iot-hub/iot-hub-explorer-cloud-device-messaging)
- [IoT Hub-berichten opslaan in Azure-gegevensopslag](https://docs.microsoft.com//azure/iot-hub/iot-hub-store-data-in-azure-table-storage)
- [Power BI gebruiken voor het visualiseren van realtime-sensorgegevens uit Azure IoT Hub](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-power-bi)
- [Web-Apps gebruiken voor het visualiseren van realtime-sensorgegevens uit Azure IoT Hub](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-web-apps)
- [Weersvoorspelling met behulp van de sensorgegevens uit IoT hub in Azure Machine Learning](https://docs.microsoft.com/azure/iot-hub/iot-hub-weather-forecast-machine-learning)
- [Apparaatbeheer met iothub-explorer](https://docs.microsoft.com/azure/iot-hub/iot-hub-device-management-iothub-explorer)
- [Externe bewaking en meldingen met Logic Apps](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps)
