---
title: 'SensorTag apparaat & Azure IoT Gateway - les 1: Intel NUC instellen | Microsoft Docs'
description: Intel NUC instellen om te werken als een IoT-gateway tussen een sensor en Azure IoT Hub sensor gegevens kunt verzamelen en verzenden naar IoT Hub.
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: IOT-gateway, intel-nuc nuc computer, DE3815TYKE
ms.assetid: 917090d6-35c2-495b-a620-ca6f9c02b317
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 1a3a92ab8d08c6ed6f047208217c46022027157e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="set-up-intel-nuc-as-an-iot-gateway"></a>Intel NUC instellen als een IoT-gateway
[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

## <a name="what-you-will-do"></a>Wat u doet

- Intel NUC instellen als een IoT-gateway.
- De Azure IoT Edge-pakket installeren op de Intel NUC.
- Een voorbeeldtoepassing 'hello_world' uitgevoerd op de Intel NUC om te controleren of de functionaliteit van de gateway.

  > Als u problemen hebt, moet u uitkijken voor oplossingen op de [probleemoplossing pagina](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Wat u leert

In deze les leert u het:

- Klik hier voor meer informatie over het Intel NUC verbinding met de randapparaten.
- Informatie over het installeren en bijwerken van de vereiste pakketten op Intel NUC met behulp van de smartcard Package Manager.
- Het uitvoeren van de voorbeeldtoepassing 'hello_world' om te controleren of de functionaliteit van de gateway.

## <a name="what-you-need"></a>Wat u nodig hebt

- Een Intel NUC Kit DE3815TYKE met Intel IoT Gateway Software Suite (o de Linux * 7.0.0.13) vooraf is geïnstalleerd. [Klik hier om aan te schaffen Groove IoT commerciële Gateway Kit](https://www.seeedstudio.com/Grove-IoT-Commercial-Gateway-Kit-p-2724.html).
- Een Ethernet-kabel.
- Een toetsenbord.
- Een HDMI of VGA-kabel.
- Een monitor met een HDMI of VGA-poort.
- Optioneel: [Texas instrumenten Sensor Tag (CC2650STK)](http://www.ti.com/tool/cc2650stk)

![Gateway-pakket](media/iot-hub-gateway-kit-lessons/lesson1/kit.png)

## <a name="connect-intel-nuc-with-the-peripherals"></a>Intel NUC verbinden met de randapparaten

De onderstaande afbeelding is een voorbeeld van Intel NUC die is verbonden met verschillende randapparatuur:

1. Verbonden met een toetsenbord.
2. Verbonden met een monitor met een VGA-kabel of HDMI-kabel.
3. Verbonden met een bekabeld netwerk met een Ethernet-kabel.
4. Verbonden met een voeding met een power-kabel.

![Intel NUC verbonden met de randapparaten](media/iot-hub-gateway-kit-lessons/lesson1/nuc.png)

## <a name="connect-to-the-intel-nuc-system-from-host-computer-via-secure-shell-ssh"></a>Verbinding maken met de Intel NUC-systeem van de hostcomputer via Secure Shell (SSH)

U moet een toetsenbord en een monitor voor het ophalen van het IP-adres van uw apparaat Intel NUC. Als u al het IP-adres, kunt u gaat u verder met stap 3 in deze sectie.

1. Schakel de Intel NUC door op de knop power en meld u vervolgens aan.

   De standaard-gebruikersnaam en wachtwoord zijn beide `root`.

       > Hit the enter key on your keyboard if you see either of the following errors when you boot: 'A TPM error (7) occurred attempting to read a pcr value.' or 'Timeout, No TPM chip found, activating TPM-bypass!'

2. Het IP-adres van de Intel NUC ophalen door het uitvoeren van de `ifconfig` opdracht op het apparaat Intel NUC.

   Hier volgt een voorbeeld van uitvoer van de opdracht.

   ![ifconfig-uitvoer met Intel NUC IP](media/iot-hub-gateway-kit-lessons/lesson1/ifconfig.png)

   In dit voorbeeld wordt de waarde die volgt `inet addr:` het IP-adres die u nodig hebt wanneer een verbinding maakt met de Intel NUC van een hostcomputer.

3. Gebruik een van de volgende SSH-clients van de hostcomputer verbinding maken met de Intel NUC.

    - [PuTTY](http://www.putty.org/) voor Windows.
    - De ingebouwde SSH-client op Ubuntu- of Mac OS.

   Het is efficiënter en productiever een Intel NUC van een hostcomputer te werken. U moet de Intel-NUC IP-adres, gebruikersnaam en wachtwoord verbinding kunnen maken via een SSH-client. Hier volgt een voorbeeld dat gebruikmaakt van een SSH-client op Mac OS.
   ![SSH-client op Mac OS uitgevoerd](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)

## <a name="install-the-azure-iot-edge-package"></a>Installeer het Azure IoT Edge-pakket

Het Azure-IoT-Edge-pakket bevat de vooraf gecompileerde binaire bestanden van de IoT-rand en de bijbehorende afhankelijkheden. Deze binaire bestanden zijn Azure IoT Edge, de Azure IoT SDK en de bijbehorende hulpprogramma's. Het pakket bevat ook een 'hello_world' voorbeeldtoepassing wordt gebruikt voor het valideren van de functionaliteit van de gateway. IoT-rand is het belangrijkste deel van de gateway. 

Volg deze stappen om het pakket te installeren.

1. De Cloud van de IoT-opslagplaats met de volgende opdrachten in een terminalvenster toevoegen:

   ```bash
   rpm --import https://iotdk.intel.com/misc/iot_pub2.key
   smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
   smart channel --add WR_Repo type=rpm-md baseurl=https://distro.windriver.com/release/idp-3-xt/public_feeds/WR-IDP-3-XT-Intel-Baytrail-public-repo/RCPL13/corei7_64/
   ```

   > Voer 'y' als 'Voeg dit kanaal?' moet u
   
   Als u krijgt een `import read failed(-1)` fout, gebruik de volgende opdrachten om het probleem te verhelpen:
   ```bash
   wget http://iotdk.intel.com/misc/iot_pub2.key 
   rpm --import iot_pub2.key  
   ```

   De `rpm` opdracht wordt de sleutel rpm geïmporteerd. De `smart channel` opdracht voegt het rpm-kanaal naar de smartcard Package Manager. Voordat u de `smart update` uitvoert, ziet u uitvoer zoals hieronder.

   ![rpm en slimme kanaal opdrachten uitvoer](media/iot-hub-gateway-kit-lessons/lesson1/rpm_smart_channel.png)

2. De opdracht smart update niet uitvoeren:

   ```bash
   smart update
   ```

3. Installeer het Azure IoT Gateway-pakket met de volgende opdracht:

   ```bash
   smart install packagegroup-cloud-azure -y
   ```

   `packagegroup-cloud-azure`is de naam van het pakket. De `smart install` opdracht wordt gebruikt om het pakket te installeren.

    > Voer de volgende opdracht als deze fout wordt weergegeven: 'openbare sleutel niet beschikbaar'

    ```bash
    smart config --set rpm-check-signatures=false
    smart install packagegroup-cloud-azure -y
    ```
    > Opnieuw opstarten van de Intel NUC als u deze fout ziet: 'Er is geen pakket biedt util-linux-dev'

   Nadat het pakket is geïnstalleerd, is Intel NUC gereed om te fungeren als een gateway.

## <a name="run-the-azure-iot-edge-helloworld-sample-application"></a>De voorbeeldtoepassing voor Azure IoT Edge 'hello_world' uitvoeren

Het volgende voorbeeld van een toepassing maakt een gateway vanuit een `hello_world.json` bestand en de fundamentele onderdelen van de rand van Azure IoT-architectuur gebruikt voor het vastleggen van een Hallo wereld-bericht in een bestand (log.txt) om de vijf seconden.

U kunt het Hallo wereld-voorbeeld uitvoeren door het uitvoeren van de volgende opdrachten:

```bash
cd /usr/share/azureiotgatewaysdk/samples/hello_world/
./hello_world hello_world.json
```

Laat de toepassing Hello World uitvoeren voor een paar minuten en klik vervolgens op Enter drukken om te stoppen.
![uitvoer van de toepassing](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)

> U kunt negeren 'ongeldig argument handle(NULL)' fouten die worden weergegeven nadat u druk op Enter.

U kunt controleren of de gateway is uitgevoerd door het openen van het bestand log.txt die nu in de map hello_world ![log.txt directory weergeven](media/iot-hub-gateway-kit-lessons/lesson1/logtxtdir.png)

Open log.txt met de volgende opdracht:

```bash
vim log.txt
```

Vervolgens ziet u de inhoud van log.txt die als een JSON-indeling uitvoer van de logboekregistratie van berichten die elke vijf seconden zijn geschreven door de gateway Hallo wereld-module.
![log.txt directory weergeven](media/iot-hub-gateway-kit-lessons/lesson1/logtxtview.png)

Als u problemen hebt, moet u uitkijken voor oplossingen op de [probleemoplossing pagina](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="summary"></a>Samenvatting

Gefeliciteerd. U klaar bent met het instellen van Intel NUC als een gateway. Nu u klaar bent om door te gaan met de volgende les voor het instellen van de hostcomputer, een Azure-IoT-Hub maken en registreren van uw logische Azure IoT Hub-apparaat.

## <a name="next-steps"></a>Volgende stappen
[Een IoT-gateway gebruiken voor verbinding van een apparaat met Azure IoT Hub](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)

