---
title: Azure IoT Hub naar Azure Event Hubs vergelijken | Microsoft Docs
description: Een vergelijking van de markering functionele verschillen en gebruiksvoorbeelden IoT Hub en Event Hubs Azure-services. De vergelijking omvat ondersteunde protocollen, Apparaatbeheer, bewaking, en bestanden kunnen uploaden.
services: iot-hub
documentationcenter: ''
author: kgremban
manager: timlt
editor: ''
ms.assetid: aeddea62-8302-48e2-9aad-c5a0e5f5abe9
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/01/2018
ms.author: kgremban
ms.openlocfilehash: 303a2bde0a1e0b25ca6eb145e7b0cd6c91fff351
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="comparison-of-azure-iot-hub-and-azure-event-hubs"></a>Vergelijking van Azure IoT Hub en Azure Event Hubs

Zowel Azure IoT Hub en Azure Event Hubs zijn cloudservices die kunnen grote hoeveelheden gegevens en het proces voor opnemen of die gegevens voor zakelijke inzichten opslaan. De twee services zijn vergelijkbaar in dat ze beide ondersteunen gebeurtenissen en telemetriegegevens gegevens verwerken met weinig latentie en hoge betrouwbaarheid. Echter is alleen IoT Hub ontwikkeld met de specifieke mogelijkheden die nodig zijn ter ondersteuning van geschaald internet der dingen scenario's. 

Azure IoT Hub wordt de cloudgateway die is verbonden apparaten en verzamelt gegevens voor zakelijke inzichten en automatisering. Maakt het gemakkelijk om te streamen naar de cloud en beheren van uw apparaten op schaal. Een belangrijk onderscheid tussen IoT Hub en andere gegevens opname-services is de IoT-Hub bevat functies die de relatie tussen uw apparaten en uw back endsystemen aanvullen. Bidirectionele communicatiemogelijkheden beschikken dat tijdens het ontvangen van gegevens van apparaten kunt u ook verzenden berichten terug naar apparaten eigenschappen bijwerken of waarvoor een actie wilt aanroepen. Identiteit op apparaatniveau helpt bij het beveiligen van uw systeem. Gedistribueerde computing verplaatst cloud service logica op de edge-apparaten.

[Azure Event Hubs] [ Azure Event Hubs] is een gebeurtenis opname-service die kunt verwerken en opslaan van grote hoeveelheden gegevens en telemetrie. Event Hubs is ontworpen voor opname van de gebeurtenis grootschalige, zowel in de context van inter datacenter en intra-datacenter scenario's, maar geen biedt de uitgebreide IoT-specifieke mogelijkheden die beschikbaar met IoT Hub zijn. Om die reden raadzaam niet Event Hubs voor uw IoT-oplossingen. 

De volgende tabel bevat informatie over hoe de twee lagen van IoT Hub met Event Hubs vergelijken wanneer u ze voor IoT-mogelijkheden evalueren wilt. Zie voor meer informatie over de lagen standard en basic van IoT Hub [het kiezen van de juiste laag van de IoT Hub][lnk-scaling].

| IoT-functionaliteit | Standard-laag van IoT-Hub | De basisstaffel IoT-Hub | Event Hubs |
| --- | --- | --- | --- |
| Apparaat-naar-cloud-berichten | ![Selecteren][1] | ![Selecteren][1] | ![Selecteren][1] |
| Protocollen: HTTPS, AMQP, AMQP via websockets | ![Selecteren][1] | ![Selecteren][1] | ![Selecteren][1] |
| Protocollen: MQTT, MQTT via websockets | ![Selecteren][1] | ![Selecteren][1] |  |
| Per apparaat-id | ![Selecteren][1] | ![Selecteren][1] |  |
| Bestanden van apparaten uploaden | ![Selecteren][1] | ![Selecteren][1] |  |
| Device Provisioning Service | ![Selecteren][1] | ![Selecteren][1] |  |
| Messaging-cloud-naar-apparaat | ![Selecteren][1] |  |  |
| Dubbele apparaat- en Apparaatbeheer | ![Selecteren][1] |  |  |
| IoT Edge | ![Selecteren][1] |  |  |

Zelfs als het enige gebruiksvoorbeeld gegevensopname apparaat-naar-cloud is, sterk aangeraden gebruik van IoT Hub, aangezien deze biedt een service die is ontworpen voor connectiviteit van IoT-apparaten. 

### <a name="next-steps"></a>Volgende stappen

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie de [Ontwikkelaarshandleiding voor IoT Hub][lnk-devguide]


[Azure Event Hubs]: ../event-hubs/event-hubs-what-is-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md

<!--Image references-->
[1]: ./media/iot-hub-compare-event-hubs/ic195031.png