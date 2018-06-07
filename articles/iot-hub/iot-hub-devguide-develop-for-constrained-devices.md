---
title: Azure IoT Hub voor beperkte apparaten ontwikkelen | Microsoft Docs
description: Handleiding voor ontwikkelaars - instructies voor het ontwikkelen met Azure IoT SDK's voor beperkte apparaten.
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
ms.openlocfilehash: 62065b78e3f8191c6423ba9dba4a8f7d16fad114
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34654969"
---
# <a name="develop-for-constrained-devices-using-azure-iot-sdks"></a>Ontwikkelen voor beperkte apparaten met behulp van Azure IoT SDK 's

## <a name="develop-using-azure-iot-hub-c-sdk"></a>Ontwikkelen met Azure IoT Hub C SDK
Azure IoT Hub C SDK is geschreven in ANSI-C (C99), waardoor het geschikt is voor een aantal verschillende platformen met kleine footprint van schijf en geheugen gebruiken.  De aanbevolen hoeveelheid RAM is ten minste 64 KB, maar het exacte geheugengebruik is afhankelijk van het protocol dat wordt gebruikt, het aantal verbindingen dat is geopend, evenals het platform die zijn gericht.

C-SDK is beschikbaar in de vorm van apt get, NuGet en MBED pakket.  Om beperkte doelapparaten, kunt u de SDK voor uw doelplatform lokaal bouwen. Deze documentatie laat zien hoe u bepaalde functies verwijderen voor de voetafdruk van de C-SDK met verkleinen [cmake][lnk-cmake].  Deze documentatie wordt bovendien de aanbevolen procedure voor het programmeren van modellen voor het werken met beperkte apparaten beschreven.

### <a name="building-the-c-sdk-for-constrained-devices"></a>De C-SDK voor beperkte apparaten bouwen
#### <a name="prerequisites"></a>Vereisten
Volg dit [installatiehandleiding] [ lnk-devbox-setup] voor het voorbereiden van uw ontwikkelomgeving voor het bouwen van de C-SDK.  Voordat u met de stap voor het bouwen met cmake krijgt, kunt u cmake vlaggen voor het verwijderen van ongebruikte functies kunt aanroepen.

#### <a name="remove-additional-protocol-libraries"></a>Verwijder aanvullend protocol bibliotheken
C SDK biedt ondersteuning voor vijf protocollen vandaag: MQTT, MQTT over WebSocket, AMQPs, AMQP via het WebSocket- en HTTPS.    De meeste scenario's moet één tot twee protocollen uitgevoerd op een client, daarom kunt u de protocol-bibliotheek die u niet met verwijderen van de SDK.  Als u meer informatie over het kiezen van het juiste communicatieprotocol voor uw scenario u in deze vindt [document][lnk-choosing-protocol].  Bijvoorbeeld is MQTT een lichtgewicht protocol dat vaak beter voor beperkte apparaten geschikt is.

U kunt AMQP en HTTP-bibliotheken met de volgende opdracht in de cmake verwijderen:
```
cmake -Duse_amqp=OFF -Duse_http=OFF <Path_to_cmake>
```

#### <a name="remove-sdk-logging-capability"></a>Mogelijkheid tot het vastleggen van de SDK verwijderen
De C-SDK biedt uitgebreide logboekregistratie in om te helpen bij foutopsporing. U kunt de logboekregistratie kunnen voor productieapparaten met de volgende opdracht in de cmake verwijderen:
```
cmake -Dno_logging=OFF <Path_to_cmake>
```

#### <a name="remove-upload-to-blob-capability"></a>Uploaden naar de blobopslag mogelijkheid verwijderen
U kunt grote bestanden uploaden naar Azure Storage met de ingebouwde mogelijkheden in de SDK.  Azure IoT Hub fungeert als een dispatcher op een gekoppelde Azure Storage-account.  Deze functie kunt u mediabestanden, grote telemetrie batches en logboeken te verzenden.  Meer informatie over het uploaden van bestanden met IoT Hub in deze [document][lnk-hub-file-upload].  Als uw toepassing deze functionaliteit niet vereist, kunt u deze functie met de volgende opdracht uit cmake verwijderen:
```
cmake -Ddont_use_uploadtoblob=ON <Path_to_cmake>
```
#### <a name="running-strip-on-linux-environment"></a>Actieve strook op Linux-omgeving
Als u de binaire op Linux-systeem uitgevoerd, kunt u gebruikmaken van de [opdracht van strook /] [ lnk-strip] om de grootte van de laatste toepassing na het compileren van te beperken.
```
strip -s <Path_to_executable>
```

### <a name="programming-models-for-constrained-devices"></a>Programmering modellen voor beperkte apparaten
#### <a name="avoid-using-the-serializer"></a>Vermijd het gebruik van de Serializer
De C-SDK is een optionele [serialisatiefunctie][lnk-serializer], waarmee u declaratieve om toewijzingstabellen te gebruiken methoden en apparaateigenschappen twin definiëren.  De serialisatiefunctie is ontworpen om u te vereenvoudigen, maar wordt er overhead, die niet optimaal voor beperkte apparaten is.  In dit geval Overweeg het gebruik van primitieve client-API's en json parseren met behulp van een lichtgewicht parser zoals [parson][lnk-parson].

#### <a name="use-the-lower-layer-ll"></a>Gebruik de onderste laag (_LLE_)
De C-SDK ondersteunt twee programmeermodellen.  Een set API's met heeft een _LLE_ infix, die staat voor de onderste laag.  Deze reeks API's zijn lichter gewicht en niet doen draaien up werkthreads, wat betekent dat de gebruiker moet handmatig instellen voor het plannen.  Bijvoorbeeld: voor de client van het apparaat de _LLE_ API's vindt u in dit [headerbestand](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/inc/iothub_device_client_ll.h).  Een andere set API's zonder de _LLE_ index heet de laag gemak waar een werkthread automatisch wordt geactiveerd.  Bijvoorbeeld, de laag gemak API's voor client van het apparaat vindt u in dit [headerbestand](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/inc/iothub_device_client.h).  Voor beperkte apparaten waar elke extra thread kan duren voordat een aanzienlijke percentage van de systeembronnen gebruikt, kunt u overwegen _LLE_ API's.

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over de architectuur van Azure IoT C SDK:
-   [Azure IoT C SDK broncode](https://github.com/Azure/azure-iot-sdk-c/)
-   [Azure IoT-apparaat SDK voor C Inleiding](https://docs.microsoft.com/azure/iot-hub/iot-hub-device-sdk-c-intro)

------
[lnk-cmake]: https://cmake.org/
[lnk-devbox-setup]:  https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-choosing-protocol]: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-protocols
[lnk-hub-file-upload]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-file-upload
[lnk-strip]: https://en.wikipedia.org/wiki/Strip_(Unix)
[lnk-serializer]: https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer
[lnk-parson]: https://github.com/kgabis/parson