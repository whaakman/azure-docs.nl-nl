---
title: Intel Edison naar cloud (C) - verbinding maken met Intel Edison met Azure IoT Hub | Microsoft Docs
description: Informatie over het installeren en verbinden van Intel Edison met Azure IoT Hub voor Intel Edison gegevens verzenden naar de Azure-cloudplatform in deze zelfstudie.
author: rangv
manager: ''
keywords: Azure iot intel edison, intel edison iot-hub, intel edison verzenden gegevens naar de cloud, intel edison naar de cloud
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: conceptual
ms.date: 4/11/2018
ms.author: rangv
ms.openlocfilehash: 2e0f8b46f2020db0bb657a8e099158c349af6a89
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39187662"
---
# <a name="connect-intel-edison-to-azure-iot-hub-c"></a>Intel Edison verbinden met Azure IoT Hub (C)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

In deze zelfstudie gaat begint u door te leren van de basisbeginselen van het werken met een Intel Edison. Vervolgens leert u hoe u uw apparaten naadloos verbinding naar de cloud met behulp van [Azure IoT Hub](about-iot-hub.md).

Heb je nog een kit? Start [hier](https://azure.microsoft.com/develop/iot/starter-kits)

## <a name="what-you-do"></a>Wat u allemaal doen

* Intel Edison instellen en en Groove-modules.
* Een IoT-hub maken.
* Registreer een apparaat voor Edison in uw IoT-hub.
* Een voorbeeld-App uitvoeren op Edison om sensorgegevens te verzenden naar uw IoT hub.

Intel Edison verbinden met een IoT-hub die u maakt. Vervolgens voert u een voorbeeld van toepassing op Edison temperatuur en vochtigheid gegevens verzamelen uit een temperatuursensor Groove. Ten slotte, stuurt u de sensorgegevens naar uw IoT-hub.

## <a name="what-you-learn"></a>Wat u leert

* Over het maken van een Azure-IoT-hub en uw nieuwe apparaat-verbindingsreeks ophalen.
* Klik hier voor meer informatie over het Edison verbinden met een temperatuursensor Groove.
* Klik hier voor meer informatie over het verzamelen van sensorgegevens door het uitvoeren van een voorbeeld van toepassing op Edison.
* Klik hier voor meer informatie over het verzenden van gegevens naar uw IoT hub.

## <a name="what-you-need"></a>Wat u nodig hebt

![Wat u nodig hebt](media/iot-hub-intel-edison-kit-c-get-started/0_kit.png)

* Het bord Intel Edison
* Arduino-bord voor uitbreiding
* Een actief Azure-abonnement. Als u een Azure-account geen [Azure een gratis proefaccount maken](https://azure.microsoft.com/free/) in een paar minuten.
* Een Mac- of een PC waarop Windows of Linux wordt uitgevoerd.
* Een internetverbinding.
* Een B Micro van Type A USB-kabel
* Een voeding direct huidige (DC). Het prioriteitsniveau van uw voeding moet als volgt:
  - 7-15V DC
  - Ten minste 1500mA
  - De pincode center/binnenste moet de positieve pool van de voeding

De volgende items zijn optioneel:

* Groove Base Shield V2
* Groove - temperatuursensor
* Groove-kabel
* Een tussenruimte balken of opgenomen in de verpakking, inclusief twee schroeven om te bevestigen van de module voor het bord uitbreiding en vier sets schroeven en plastic spacers schroeven.

> [!NOTE] 
Deze items zijn optioneel, omdat de ondersteuning voor het voorbeeld van code sensorgegevens gesimuleerde.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-intel-edison"></a>Intel Edison instellen

### <a name="assemble-your-board"></a>Samenstellen van het bord

In deze sectie bevat stappen om uw Intel® Edison-module koppelen aan uw bord uitbreiding.

1. Plaats de Intel® Edison-module binnen het witte overzicht op het mededelingenbord uitbreiding uitlijnen in de gaten in de module met de schroeven op het prikbord van uitbreiding.

2. Druk op de module onder de woorden `What will you make?` totdat u denkt een module dat.

   ![bord 2 samenstellen](media/iot-hub-intel-edison-kit-c-get-started/1_assemble_board2.jpg)

3. De twee hexadecimale nuts (opgenomen in het pakket) gebruiken voor het beveiligen van de module voor het bord uitbreiding.

   ![bord 3 samenstellen](media/iot-hub-intel-edison-kit-c-get-started/2_assemble_board3.jpg)

4. Een installatie in een van de vier-corner gaten op het prikbord van uitbreiding invoegen. Aanpassen en webserverbeheerders een van de witte plastic spacers bij de installatie.

   ![bord 4 samenstellen](media/iot-hub-intel-edison-kit-c-get-started/3_assemble_board4.jpg)

5. Herhaal dit voor de andere drie-corner spacers.

   ![bord 5 samen te stellen](media/iot-hub-intel-edison-kit-c-get-started/4_assemble_board5.jpg)

Nu u uw bord wordt samengesteld.

   ![geassembleerde bord](media/iot-hub-intel-edison-kit-c-get-started/5_assembled_board.jpg)

### <a name="connect-the-grove-base-shield-and-the-temperature-sensor"></a>Verbinding maken met de Groove Base Shield en de temperatuursensor

1. Plaats de Groove Base Shield u aan bij uw bord. Zorg ervoor dat alle pincodes nauw zijn aangesloten op het mededelingenbord.
   
   ![Groove Base schild](media/iot-hub-intel-edison-kit-c-get-started/6_grove_base_sheild.jpg)

2. Groove-kabel gebruiken om te verbinden Groove-temperatuursensor naar de Base Groove Shield **A0** poort.

   ![Verbinding maken met-temperatuursensor](media/iot-hub-intel-edison-kit-c-get-started/7_temperature_sensor.jpg)
   
   ![Edison en sensor-verbinding](media/iot-hub-intel-edison-kit-c-get-started/16_edion_sensor.png)

De sensor is nu gereed.

### <a name="power-up-edison"></a>Alles Edison

1. Sluit de voeding.

   ![Plug-in-voeding](media/iot-hub-intel-edison-kit-c-get-started/8_plug_power.jpg)

2. Een groene LED (met de naam DS1 op het bord Arduino * uitbreiding) moet beschikbaar stellen en blijf branden.

3. Wacht een minuut voor het bord dat moet worden opgestart afronden.

   > [!NOTE]
   > Als u een DC-voeding hebt, kunt u nog steeds het bord via een USB-poort voor energiebeheer. Zie `Connect Edison to your computer` sectie voor meer informatie. Levert het vermogen van uw bord op deze manier kan resulteren in onvoorspelbaar gedrag van het bord, met name wanneer met behulp van Wi-Fi of groot motoren.

### <a name="connect-edison-to-your-computer"></a>Edison aansluiten op uw computer

1. Schakel op de microswitch naar de twee micro USB-poorten, zodat Edison in de apparaatmodus. Raadpleeg voor verschillen tussen het apparaat en host-modus, [hier](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).

   ![In-of uitschakelen in de microswitch](media/iot-hub-intel-edison-kit-c-get-started/9_toggle_down_microswitch.jpg)

2. De micro USB-kabel aansluiten op de bovenste micro USB-poort.

   ![Bovenste micro USB-poort](media/iot-hub-intel-edison-kit-c-get-started/10_top_usbport.jpg)

3. Het andere uiteinde van USB-kabel aansluiten op uw computer.

   ![Computer USB](media/iot-hub-intel-edison-kit-c-get-started/11_computer_usb.jpg)

4. U weet dat uw bord volledig is geïnitialiseerd, wanneer de computer een nieuw station (NET koppelt zoals een SD-kaart invoegen in uw computer).

## <a name="download-and-run-the-configuration-tool"></a>Download en voer het configuratiehulpprogramma uit
Ophalen van de meest recente configuratiehulpprogramma uit [deze koppeling](https://software.intel.com/en-us/iot/hardware/edison/downloads) vermeld in de `Installers` kop. Het hulpprogramma uitvoeren en volg de aanwijzingen op het scherm instructies te klikken naast waar nodig

### <a name="flash-firmware"></a>Flash-firmware
1. Op de `Set up options` pagina, klikt u op `Flash Firmware`.
2. Selecteer de afbeelding die moet op het mededelingenbord flash door een van de volgende manieren:
   - Selecteer om te downloaden en uw bord met de meest recente firmware-installatiekopie beschikbaar is via Intel flash, `Download the latest image version xxxx`.
   - Flash-uw bord met een installatiekopie die u al hebt opgeslagen op uw computer, selecteer `Select the local image`. Blader naar en selecteer de installatiekopie die u wilt aan uw bord flash.
3. Het setup-hulpprogramma wordt geprobeerd het mededelingenbord flash. Het hele knipperende proces kan maximaal tien minuten duren.

### <a name="set-password"></a>Wachtwoord instellen
1. Op de `Set up options` pagina, klikt u op `Enable Security`.
2. U kunt een aangepaste naam op voor de Intel® Edison-bord instellen. Dit is optioneel.
3. Typ een wachtwoord voor uw kaart en klik vervolgens op `Set password`.
4. Markeert u het wachtwoord, wordt later gebruikt.

### <a name="connect-wi-fi"></a>Verbinding maken met Wi-Fi
1. Op de `Set up options` pagina, klikt u op `Connect Wi-Fi`. Wacht tot één minuut als uw computer wordt gescand op beschikbare Wi-Fi-netwerken.
2. Uit de `Detected Networks` vervolgkeuzelijst, selecteert u uw netwerk.
3. Uit de `Security` vervolgkeuzelijst, selecteert u het beveiligingstype van het netwerk.
4. Voer uw gegevens bij en het wachtwoord en klik vervolgens op `Configure Wi-Fi`.
5. Markeer in de IP-adres dat wordt later gebruikt.

> [!NOTE]
> Zorg ervoor dat Edison is verbonden met hetzelfde netwerk bevinden als uw computer. Uw computer verbinding maakt met uw Edison met behulp van het IP-adres.

   ![Verbinding maken met-temperatuursensor](media/iot-hub-intel-edison-kit-c-get-started/12_configuration_tool.png)

Gefeliciteerd! U hebt Edison geconfigureerd.

## <a name="run-a-sample-application-on-intel-edison"></a>Een voorbeeld van toepassing op Intel Edison uitvoeren

### <a name="prepare-the-azure-iot-device-sdk"></a>Voorbereiden van de Azure IoT-Device-SDK

1. Gebruik een van de volgende SSH-clients vanaf uw hostcomputer verbinding maken met uw Intel Edison. Het IP-adres uit het configuratiehulpprogramma van en het wachtwoord die u hebt ingesteld in dit hulpprogramma is.
    - [PuTTY](http://www.putty.org/) voor Windows.
    - De ingebouwde SSH-client op Ubuntu- of Mac OS (uitvoeren `ssh root@"the IP address"`).

2. Kloon de voorbeeld-client-app op uw apparaat. 
   
   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-c-intel-edison-client-app.git
   ```

3. Navigeer naar de map van de opslagplaats om uit te voeren van de volgende opdracht om de Azure IoT SDK bouwen

   ```bash
   cd iot-hub-c-intel-edison-client-app
   sed -i -e 's/\r$//' buildSDK.sh
   chmod 755 buildSDK.sh
   ./buildSDK.sh
   ```

### <a name="configure-the-sample-application"></a>De voorbeeld-App configureren

1. Open het configuratiebestand dat door het uitvoeren van de volgende opdrachten:

   ```bash
   nano config.h
   ```

   ![Configuratiebestand](media/iot-hub-intel-edison-kit-c-get-started/13_configure_file.png)

   Er zijn twee macro's in dit bestand kunt u configurate. Ten eerste `INTERVAL`, waarmee het tijdsinterval tussen twee berichten verzenden naar de cloud wordt gedefinieerd. De tweede waarde `SIMULATED_DATA`, dit is een Booleaanse waarde voor of gesimuleerde sensorgegevens of niet.

   Als u **hoeft niet de sensor**, stel de `SIMULATED_DATA` waarde die moet worden `1` om de voorbeeld-App maken en gebruiken van gesimuleerde sensorgegevens.

2. Opslaan en sluiten door te drukken besturingselement-O > Enter > Control-X.

### <a name="build-and-run-the-sample-application"></a>De voorbeeldtoepassing bouwen en uitvoeren

1. De voorbeeldtoepassing met de volgende opdracht:

   ```bash
   cmake . && make
   ```
   ![Uitvoer maken](media/iot-hub-intel-edison-kit-c-get-started/14_build_output.png)

1. De voorbeeldtoepassing uitvoeren met de volgende opdracht:

   ```bash
   sudo ./app '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   Zorg ervoor dat u kopiëren en plakken de apparaatverbindingsreeks in enkele aanhalingstekens.

Hier ziet u de volgende uitvoer ziet u de sensorgegevens en de berichten die worden verzonden naar uw IoT-hub.

![Uitvoer - sensorgegevens uit Intel Edison verzonden naar uw IoT-hub](media/iot-hub-intel-edison-kit-c-get-started/15_message_sent.png)

## <a name="next-steps"></a>Volgende stappen

U kunt een voorbeeld van toepassing voor het verzamelen van gegevens en verzenden naar uw IoT-hub hebt uitgevoerd.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
