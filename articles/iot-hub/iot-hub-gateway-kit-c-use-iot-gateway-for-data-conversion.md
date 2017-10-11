---
title: Gegevensconversie op IoT gateway met Azure IoT rand | Microsoft Docs
description: IoT gateway gebruiken voor het converteren van de indeling van sensorgegevens via een aangepaste module vanaf Azure IoT rand.
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: conversie van IOT gateway, iot gateway gegevenstransformatie
ms.assetid: 75f2573d-500b-4405-bff7-61021c4c3500
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/25/2017
ms.author: xshi
ms.openlocfilehash: d5c735a4adbc59e9526ec4fd40720c5ec136d63d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="use-iot-gateway-for-sensor-data-transformation-with-azure-iot-edge"></a>Gebruik IoT gateway voor gegevenstransformatie sensor met Azure IoT rand

> [!NOTE]
> Voordat u deze zelfstudie begint, zorg er dan voor dat u hebt de volgende uitkomsten voltooid in volgorde:
> * [Intel NUC instellen als IoT-gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
> * [Gebruik IoT gateway dingen verbinding met de cloud - SensorTag met Azure IoT Hub](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)

Een doel van een Iot-gateway is verzamelde gegevens verwerken voordat deze naar de cloud verzonden. Azure IoT-rand introduceert modules die kunnen worden gemaakt en om de werkstroom gegevensverwerking samengesteld. Een module een bericht ontvangt, voert een bepaalde actie op deze en vervolgens voor andere modules verwerken op verplaatsen.

## <a name="what-you-learn"></a>Wat u leert

U informatie over het maken van een module berichten uit de SensorTag converteren naar een andere indeling.

## <a name="what-you-do"></a>Wat u doet

* Maakt een module voor het converteren van een ontvangen bericht naar de .json-indeling.
* De module worden gecompileerd.
* De module toevoegen aan de voorbeeldtoepassing uitschakelen vanaf Azure IoT rand.
* De voorbeeldtoepassing uitvoert.

## <a name="what-you-need"></a>Wat u nodig hebt

* De volgende zelfstudies in de reeks voltooid:
  * [Intel NUC instellen als IoT-gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
  * [Gebruik IoT gateway dingen verbinding met de cloud - SensorTag met Azure IoT Hub](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)
* Een SSH-client die wordt uitgevoerd op de hostcomputer. PuTTY wordt aanbevolen voor Windows. Linux- en Mac OS al worden geleverd met een SSH-client.
* Het IP-adres en de gebruikersnaam en wachtwoord voor toegang tot de gateway van de SSH-client.
* Een internetverbinding.

## <a name="create-a-module"></a>Een module maken

1. De SSH-client wordt uitgevoerd op de hostcomputer en maak verbinding met de IoT-gateway.
1. Kloon de bronbestanden van de module conversie vanuit GitHub naar de basismap van de IoT-gateway met de volgende opdrachten:

   ```bash
   cd ~
   git clone https://github.com/Azure-Samples/iot-hub-c-intel-nuc-gateway-customized-module.git
   ```

   Dit is een systeemeigen Azure Edge-module, geschreven in de programmeertaal. De module wordt de indeling van ontvangen berichten geconverteerd naar het volgende:

   ```json
   {"deviceId": "Intel NUC Gateway", "messageId": 0, "temperature": 0.0}
   ```

## <a name="compile-the-module"></a>De module niet compileren

Samengesteld op basis van de module, voer de volgende opdrachten:

```bash
cd iot-hub-c-intel-nuc-gateway-customized-module/my_module
# change the build script runnable
chmod 777 build.sh
# remove the invalid windows character
sed -i -e "s/\r$//" build.sh
# run the build shell script
./build.sh
```

U krijgt een `libmy_module.so` bestand nadat het compileren is voltooid. Noteer het absolute pad van dit bestand.

## <a name="add-the-module-to-the-ble-sample-application"></a>De module toevoegen aan de voorbeeldtoepassing uitschakelen

1. Ga naar de map samples met de volgende opdracht:

   ```bash
   cd /usr/share/azureiotgatewaysdk/samples
   ```

1. Open het configuratiebestand met de volgende opdracht:

   ```bash
   vi ble_gateway.json
   ```

1. Een module toevoegen door het invoegen van de volgende code naar de `modules` sectie.

   ```json
   {
       "name": "MyModule",
       "loader": {
           "name": "native",
           "entrypoint":{
               "module.path": "[Your libmy_module.so path]"
               }
            },
        "args": null
    },
    ```

1. Vervang `[Your libmy_module.so path]` in de code met het absolute pad van de libmy_module.so' bestand.
1. Vervang de code in de `links` sectie met de volgende:

   ```json
   {
       "source": "SensorTag",
       "sink": "MyModule"
   },
   {
       "source": "MyModule",
       "sink": "mapping"
   }
   ```

1. Druk op `ESC`, en typ vervolgens `:wq` het bestand wilt opslaan.

## <a name="run-the-sample-application"></a>De voorbeeldtoepassing uitvoeren

1. Zet de SensorTag.
1. De omgevingsvariabele SSL_CERT_FILE instellen met de volgende opdracht:

   ```bash
   export SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt
   ```

1. Voer de voorbeeldtoepassing met de toegevoegde module met de volgende opdracht:

   ```bash
   ./ble_gateway ble_gateway.json
   ```

## <a name="next-steps"></a>Volgende stappen

U hebt de IoT-gateway is het bericht uit de SensorTag converteren naar de .json-indeling gebruiken.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
