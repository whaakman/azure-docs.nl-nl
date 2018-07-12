---
title: Azure IoT Hub ontwikkelen voor apparaten met beperkte | Microsoft Docs
description: Handleiding voor ontwikkelaars - richtlijnen voor het ontwikkelen met Azure IoT SDK's voor beperkte apparaten.
services: iot-hub
documentationcenter: c
author: yzhong94
manager: timlt
editor: ''
ms.assetid: 979136db-c92d-4288-870c-f305e8777bdd
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/24/2018
ms.author: yizhon
ms.openlocfilehash: 15fc5794c71428b5fb1036060af3e9c4a6890f4f
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38969058"
---
# <a name="develop-for-constrained-devices-using-azure-iot-sdks"></a>Ontwikkelen voor beperkte apparaten met behulp van Azure IoT SDK 's

## <a name="develop-using-azure-iot-hub-c-sdk"></a>Ontwikkelen met behulp van C-SDK van Azure IoT Hub
Azure IoT Hub C-SDK is geschreven in ANSI C (C99), waardoor het geschikt zijn voor het uitvoeren van een aantal verschillende platformen met weinig ruimte voor schijf en het geheugen.  De aanbevolen hoeveelheid RAM is ten minste 64 KB, maar het geheugengebruik van de exacte is afhankelijk van het protocol dat wordt gebruikt, het aantal geopende verbindingen, evenals het platform dat is gericht.

C-SDK is beschikbaar in de vorm van apt-get, NuGet en MBED pakket.  Als u wilt richten op beperkte apparaten, kunt u de SDK voor uw doelplatform lokaal bouwen. Deze documentatie wordt gedemonstreerd hoe u bepaalde functies voor het verkleinen van het bereik van het gebruik van de C-SDK verwijderen [cmake][lnk-cmake].  Deze documentatie wordt bovendien de aanbevolen procedure programmeermodellen voor het werken met beperkte apparaten beschreven.

### <a name="building-the-c-sdk-for-constrained-devices"></a>Het bouwen van de C-SDK voor beperkte apparaten
#### <a name="prerequisites"></a>Vereisten
Volg deze [installatiehandleiding] [ lnk-devbox-setup] om voor te bereiden uw ontwikkelomgeving voor het bouwen van de C-SDK.  Voordat u met de stap voor het bouwen met cmake krijgt, kunt u cmake-vlaggen als u wilt verwijderen van ongebruikte functies aanroepen.

#### <a name="remove-additional-protocol-libraries"></a>Verwijder aanvullend protocol-bibliotheken
C SDK biedt ondersteuning voor vijf protocollen vandaag: MQTT-, MQTT via WebSocket, AMQPs, AMQP via WebSocket en HTTPS.    De meeste scenario's vereisen een tot twee protocollen die worden uitgevoerd op een client, daarom kunt u het protocol-bibliotheek die u niet gebruikt verwijderen uit de SDK.  Als u meer informatie over het kiezen van het juiste communicatieprotocol voor uw scenario u in deze vindt [document][lnk-choosing-protocol].  MQTT is bijvoorbeeld een lichte protocol dat vaak beter voor beperkte apparaten geschikt is.

AMQP en HTTP-bibliotheken met de volgende cmake-opdracht, kunt u verwijderen:
```
cmake -Duse_amqp=OFF -Duse_http=OFF <Path_to_cmake>
```

#### <a name="remove-sdk-logging-capability"></a>Mogelijkheid tot het vastleggen van de SDK verwijderen
De C-SDK biedt uitgebreide logboekregistratie in om te helpen bij het opsporen van fouten. U kunt de logboekregistratiemogelijkheden voor de productieapparaten met de volgende cmake-opdracht verwijderen:
```
cmake -Dno_logging=OFF <Path_to_cmake>
```

#### <a name="remove-upload-to-blob-capability"></a>Uploaden naar de blobopslag mogelijkheid verwijderen
U kunt grote bestanden uploaden naar Azure Storage met behulp van de ingebouwde mogelijkheden in de SDK.  Azure IoT Hub fungeert als een functie voor berichtverzending naar een gekoppeld Azure Storage-account.  U kunt deze functie gebruiken om media-bestanden, grote telemetrie batches en logboeken te verzenden.  Meer informatie over het uploaden van bestanden met IoT Hub in deze [document][lnk-hub-file-upload].  Als uw toepassing geen deze functionaliteit is vereist, kunt u deze functie met de volgende cmake-opdracht verwijderen:
```
cmake -Ddont_use_uploadtoblob=ON <Path_to_cmake>
```
#### <a name="running-strip-on-linux-environment"></a>Actieve strook/lijn op Linux-omgeving
Als u de binaire uitgevoerd op Linux-systeem, kunt u gebruikmaken van de [opdracht van strook /] [ lnk-strip] om de grootte van de laatste aanvraag na het compileren van te beperken.
```
strip -s <Path_to_executable>
```

### <a name="programming-models-for-constrained-devices"></a>Programmeringsmodellen voor beperkte apparaten
#### <a name="avoid-using-the-serializer"></a>Vermijd het gebruik van de Serializer
De C-SDK is een optionele [serializer][lnk-serializer], waarmee u declaratieve toewijzingstabellen voor het definiëren van methoden en apparaatdubbeleigenschappen gebruiken.  De serializer is ontworpen om ontwikkeling te vereenvoudigen, maar het voegt overhead, die is niet optimaal voor beperkte apparaten.  In dit geval primitieve client-API's kunt u overwegen en json parseren met behulp van een lichtgewicht parser zoals [parson][lnk-parson].

#### <a name="use-the-lower-layer-ll"></a>Gebruik de onderste laag (_LLE_)
De C-SDK biedt ondersteuning voor twee programmeermodellen.  Een set API's met heeft een _LLE_ infix, die staat voor de laag.  Deze reeks API's zijn lichter van gewicht en kan niet instellen werkthreads, wat betekent dat de gebruiker moet handmatig instellen voor het plannen.  Bijvoorbeeld: voor de client van het apparaat, de _LLE_ API's vindt u in deze [headerbestand](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/inc/iothub_device_client_ll.h).  Een andere set API's zonder dat de _LLE_ index heet de laag gemak, waar geen werkthread automatisch wordt genomen.  Bijvoorbeeld, de laag voor uw gemak bedoeld; API's voor de apparaatclient kan worden gevonden in deze [headerbestand](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/inc/iothub_device_client.h).  Voor beperkte apparaten waar elke extra thread kan duren voordat een aanzienlijke hoeveelheid systeembronnen gebruikt, kunt u overwegen _LLE_ API's.

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over de architectuur van Azure IoT C SDK:
-   [Azure IoT C SDK-broncode](https://github.com/Azure/azure-iot-sdk-c/)
-   [Azure IoT device-SDK voor C-Inleiding](https://docs.microsoft.com/azure/iot-hub/iot-hub-device-sdk-c-intro)

------
[lnk-cmake]: https://cmake.org/
[lnk-devbox-setup]:  https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-choosing-protocol]: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-protocols
[lnk-hub-file-upload]: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-file-upload
[lnk-strip]: https://en.wikipedia.org/wiki/Strip_(Unix)
[lnk-serializer]: https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer
[lnk-parson]: https://github.com/kgabis/parson