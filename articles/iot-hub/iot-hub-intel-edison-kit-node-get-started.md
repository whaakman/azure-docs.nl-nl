---
title: Intel Edison naar cloud (Node.js) - Intel Edison verbinding maken met Azure IoT Hub | Microsoft Docs
description: Informatie over het instellen en Intel Edison verbinden met Azure IoT Hub voor Intel Edison gegevens verzenden naar het Azure-cloud-platform in deze zelfstudie.
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Azure iot intel edison, intel edison iothub, intel edison verzenden van gegevens naar de cloud, intel edison naar cloud
ms.assetid: a7c9cf2d-c102-41b0-aa45-41285c6877eb
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 6/15/2017
ms.author: xshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5a31efba704045196b5563f7bc467c773bea7805
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="connect-intel-edison-to-azure-iot-hub-nodejs"></a>Intel Edison verbinden met Azure IoT Hub (Node.js)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

In deze zelfstudie maakt beginnen u door te leren over het werken met Intel Edison. Vervolgens leert u hoe u uw apparaten probleemloos verbinding met de cloud via [Azure IoT Hub](iot-hub-what-is-iot-hub.md).

Heb je nog een kit? Start [hier](https://azure.microsoft.com/develop/iot/starter-kits)

## <a name="what-you-do"></a>Wat u doet

* Intel Edison instellen en en Groove modules.
* Een iothub maken.
* Een apparaat registreren voor Edison in uw IoT-hub.
* Een voorbeeld van een toepassing uitvoeren op Edison sensorgegevens verzenden naar uw IoT-hub.

Verbinding maken met Intel Edison naar een IoT-hub die u maakt. Vervolgens voert u een voorbeeld van toepassing op Edison temperatuur en vochtigheid gegevens te verzamelen van een temperatuursensor Groove. U kunt ten slotte de sensorgegevens verzendt naar uw IoT-hub.

## <a name="what-you-learn"></a>Wat u leert

* Het maken van een Azure-IoT-hub en het nieuwe apparaat-verbindingsreeks ophalen.
* Klik hier voor meer informatie over het Edison verbinden met een temperatuursensor Groove.
* Klik hier voor meer informatie over het verzamelen van sensorgegevens met een voorbeeld van toepassing op Edison uitgevoerd.
* Hoe sensorgegevens verzendt naar uw IoT-hub.

## <a name="what-you-need"></a>Wat u nodig hebt

![Wat u nodig hebt](media/iot-hub-intel-edison-kit-node-get-started/0_kit.png)

* De Intel Edison-kaart
* Arduino uitbreiding mededelingenbord
* Een actief Azure-abonnement. Als u een Azure-account geen [maken van een gratis Azure-proefaccount](https://azure.microsoft.com/free/) over een paar minuten.
* Een Mac of een PC die Windows of Linux wordt uitgevoerd.
* Een internetverbinding.
* Een B Micro van Type A USB-kabel
* Een voeding direct huidige (DC). Het prioriteitsniveau van uw voeding moet als volgt:
  - 7 15V DC
  - Ten minste 1500mA
  - De pincode center/interne moet de positieve pool van de voeding

De volgende items zijn optioneel:

* Groove Base schild V2
* Groove - temperatuursensor
* Groove-kabel
* Een tussenruimte balken of schroeven opgenomen in de verpakking, met inbegrip van twee schroeven om de module aan de bestuur uitbreiding en vier sets schroeven en plastic spacers bevestigen.

> [!NOTE] 
Deze items zijn optioneel, omdat de ondersteuning voor het voorbeeld van code sensorgegevens gesimuleerd.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-intel-edison"></a>Intel Edison instellen

### <a name="assemble-your-board"></a>Het mededelingenbord samenstellen

Deze sectie bevat stappen om uw module Intel® Edison koppelen aan het mededelingenbord uitbreiding.

1. Plaats de Intel® Edison module binnen het wit overzicht op het mededelingenbord uitbreiding uitgelijnd in de gaten in de module met de schroeven op het mededelingenbord van de uitbreiding.

2. Druk op de module onder de woorden `What will you make?` totdat u denkt een module dat.

   ![het mededelingenbord 2 samenstellen](media/iot-hub-intel-edison-kit-node-get-started/1_assemble_board2.jpg)

3. De twee hexadecimale nuts (opgenomen in het pakket) gebruiken voor het beveiligen van de module in op het mededelingenbord van de uitbreiding.

   ![het mededelingenbord 3 samenstellen](media/iot-hub-intel-edison-kit-node-get-started/2_assemble_board3.jpg)

4. Voeg een installatie in een van de vier hoek gaten op het mededelingenbord van de uitbreiding. Draai en dat een van de witte plastic spacers op de installatie wordt versterkt.

   ![het mededelingenbord 4 samenstellen](media/iot-hub-intel-edison-kit-node-get-started/3_assemble_board4.jpg)

5. Herhaal voor de andere drie hoek spacers.

   ![het mededelingenbord 5 samenstellen](media/iot-hub-intel-edison-kit-node-get-started/4_assemble_board5.jpg)

Nu is het mededelingenbord samengesteld.

   ![samengesteld mededelingenbord](media/iot-hub-intel-edison-kit-node-get-started/5_assembled_board.jpg)

### <a name="connect-the-grove-base-shield-and-the-temperature-sensor"></a>Verbinding maken met het Groove Base schild en de temperatuursensor

1. Plaats het Groove Base schild doorsturen naar het mededelingenbord. Zorg ervoor dat alle pincodes goed zijn aangesloten op het mededelingenbord.
   
   ![Groove Base schild](media/iot-hub-intel-edison-kit-node-get-started/6_grove_base_sheild.jpg)

2. Verbinding maken Groove-temperatuursensor naar het Groove Base schild via Groove kabel **A0** poort.

   ![Verbinding maken met-temperatuursensor](media/iot-hub-intel-edison-kit-node-get-started/7_temperature_sensor.jpg)

   ![Edison en sensor verbinding](media/iot-hub-intel-edison-kit-node-get-started/16_edion_sensor.png)

Nu is uw sensor gereed.

### <a name="power-up-edison"></a>Geef Edison

1. Sluit de voeding.

   ![Plug-in-voeding](media/iot-hub-intel-edison-kit-node-get-started/8_plug_power.jpg)

2. Een groene LED (met de naam DS1 op het mededelingenbord Arduino * uitbreiding) moet branden en blijf branden.

3. Wacht een minuut voor de kaart te voltooien wordt opgestart.

   > [!NOTE]
   > Als u een DC-voeding niet hebt, kunt u nog steeds het mededelingenbord via een USB-poort voor energiebeheer. Zie `Connect Edison to your computer` sectie voor meer informatie. Dat het mededelingenbord op deze manier kan leiden tot onvoorspelbaar gedrag van het mededelingenbord vooral wanneer met behulp van Wi-Fi of groot motoren.

### <a name="connect-edison-to-your-computer"></a>Edison aansluiten op uw computer

1. Schakelen naar beneden de microswitch naar de twee micro USB-poorten, zodat Edison in de apparaatmodus. Raadpleeg voor de verschillen tussen het apparaat en host-modus, [hier](https://software.intel.com/en-us/node/628233#usb-device-mode-vs-usb-host-mode).

   ![Uitschakelen van de microswitch](media/iot-hub-intel-edison-kit-node-get-started/9_toggle_down_microswitch.jpg)

2. De micro USB-kabel aansluiten op de bovenste micro USB-poort.

   ![Bovenste micro USB-poort](media/iot-hub-intel-edison-kit-node-get-started/10_top_usbport.jpg)

3. Het andere einde van USB-kabel aansluiten op uw computer.

   ![Computer USB](media/iot-hub-intel-edison-kit-node-get-started/11_computer_usb.jpg)

4. Weet u dat het mededelingenbord volledig is geïnitialiseerd, wanneer uw computer koppelt een nieuw station (net zoals een SD-kaart in uw computer invoegen).

## <a name="download-and-run-the-configuration-tool"></a>Downloaden en uitvoeren van het configuratiehulpprogramma
Ophalen van de nieuwste hulpprogramma voor serverconfiguratie van [deze koppeling](https://software.intel.com/en-us/iot/hardware/edison/downloads) vermeld in de `Installers` kop. Het hulpprogramma uitvoeren en volg de aanwijzingen op het scherm instructies en klik vervolgens op waar nodig

### <a name="flash-firmware"></a>Flash-firmware
1. Op de `Set up options` pagina, klikt u op `Flash Firmware`.
2. Selecteer de installatiekopie moet op het mededelingenbord flash op een van de volgende manieren:
   - Als u wilt downloaden en het mededelingenbord met de meest recente firmware-installatiekopie beschikbaar van Intel flash, selecteer `Download the latest image version xxxx`.
   - Selecteer Flash het mededelingenbord met een installatiekopie die u al hebt opgeslagen op uw computer, `Select the local image`. Blader naar en selecteer de installatiekopie die u wilt flash op het mededelingenbord.
3. Het hulpprogramma setup probeert te flash het mededelingenbord. Het hele knipperende proces kan maximaal 10 minuten duren.

### <a name="set-password"></a>Wachtwoord instellen
1. Op de `Set up options` pagina, klikt u op `Enable Security`.
2. U kunt een aangepaste naam voor uw kaart Intel® Edison instellen. Dit is optioneel.
3. Typ een wachtwoord voor uw kaart en klik vervolgens op `Set password`.
4. Markeert u het wachtwoord dat later wordt gebruikt.

### <a name="connect-wi-fi"></a>Wi-Fi-verbinding
1. Op de `Set up options` pagina, klikt u op `Connect Wi-Fi`. Wacht maximaal één minuut als uw computer wordt gescand naar beschikbare Wi-Fi-netwerken.
2. Van de `Detected Networks` vervolgkeuzelijst, selecteert u uw netwerk.
3. Van de `Security` vervolgkeuzelijst Selecteer beveiligingstype van het netwerk.
4. Geef informatie op uw aanmeldingsnaam en het wachtwoord en klik vervolgens op `Configure Wi-Fi`.
5. Markeer de IP-adres, die later wordt gebruikt.

> [!NOTE]
> Zorg ervoor dat Edison is verbonden met hetzelfde netwerk bevindt als uw computer. Uw computer verbinding maakt met uw Edison met behulp van het IP-adres.

   ![Verbinding maken met-temperatuursensor](media/iot-hub-intel-edison-kit-node-get-started/12_configuration_tool.png)

Gefeliciteerd. U hebt Edison geconfigureerd.

## <a name="run-a-sample-application-on-intel-edison"></a>Een voorbeeld van een toepassing uitvoeren op Intel Edison

### <a name="prepare-the-azure-iot-device-sdk"></a>Het apparaat met Azure IoT SDK voorbereiden

1. Gebruik een van de volgende SSH-clients van de hostcomputer verbinding maken met uw Edison Intel. Het IP-adres is van het configuratiehulpprogramma en het wachtwoord is de taak die u hebt ingesteld in dat hulpprogramma.
    - [PuTTY](http://www.putty.org/) voor Windows.
    - De ingebouwde SSH-client op Ubuntu- of Mac OS.

2. Kloon de voorbeeld-client-app op uw apparaat. 
   
   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-intel-edison-client-app
   ```

3. Navigeer naar de map opslagplaats naar de volgende opdracht om alle pakketten te installeren, serval minuten kan duren.
   
   ```bash
   cd iot-hub-node-intel-edison-client-app
   npm install
   ```


### <a name="configure-and-run-the-sample-application"></a>Configureren en uitvoeren van de voorbeeldtoepassing

1. Open het configuratiebestand met de volgende opdrachten:

   ```bash
   nano config.json
   ```

   ![Configuratiebestand](media/iot-hub-intel-edison-kit-node-get-started/13_configure_file.png)

   Er zijn twee macro's in dit bestand kunt u configurate. De eerste is `INTERVAL`, definieert het tijdsinterval tussen twee berichten die naar de cloud verzendt. De tweede waarde `simulatedData`, dit is een Booleaanse waarde voor of gesimuleerde sensorgegevens of niet.

   Als u **hoeft niet de sensor**stelt de `simulatedData` van waarde naar `true` ervoor dat de voorbeeldtoepassing maken en gebruiken van gesimuleerde sensorgegevens.

1. Opslaan en sluiten door te drukken besturingselement-O > Enter > Control X.


1. De voorbeeldtoepassing met de volgende opdracht uitvoeren:

   ```bash
   sudo node index.js '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   Zorg ervoor dat u kopiëren en plakken de apparaat-verbindingsreeks in enkele aanhalingstekens.

Hier ziet u de volgende uitvoer waarin de sensorgegevens en de berichten die worden verzonden naar uw IoT-hub.

![Output - sensorgegevens verzonden van Intel Edison naar uw IoT-hub](media/iot-hub-intel-edison-kit-node-get-started/15_message_sent.png)

## <a name="next-steps"></a>Volgende stappen

U kunt een voorbeeldtoepassing sensorgegevens verzamelen en verzenden naar uw IoT-hub hebt uitgevoerd.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
