---
title: Azure IoT Hub met Azure Eventhubs vergelijken | Microsoft Docs
description: Een vergelijking van de IoT Hub en Event Hubs-Azure-services markering functionele verschillen en use-cases. De vergelijking bevat de ondersteunde protocollen, beheer van apparaten, bewaking, en uploaden van bestanden.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: kgremban
ms.openlocfilehash: 20bb0cb6982bcbea6b18989099322cfd3389b0b0
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54819634"
---
# <a name="connecting-iot-devices-to-azure-iot-hub-and-event-hubs"></a>IoT-apparaten verbinden met Azure: IoT Hub en Eventhubs

Azure biedt speciaal ontwikkeld voor diverse typen connectiviteit en communicatie kunt u uw gegevens verbinden met de kracht van de cloud. Zowel Azure IoT Hub en Azure Event Hubs zijn cloud-services die u kunnen opnemen van grote hoeveelheden gegevens en verwerken of opslaan van die gegevens voor zakelijke inzichten worden verkregen. De twee services zijn vergelijkbaar in dat deze beide ondersteuning bieden voor opname van gegevens met lage latentie en hoge betrouwbaarheid, maar ze zijn ontworpen voor verschillende doeleinden. IoT Hub is specifiek voor de unieke vereisten van een IoT-apparaten, op schaal, verbinding met de Azure-Cloud terwijl Event Hubs is ontworpen voor big data streamen ontwikkeld. Dit is de reden waarom Microsoft raadt het gebruik van Azure IoT Hub IoT-apparaten verbinden met Azure

Azure IoT Hub is de cloudgateway die wordt aangesloten IoT-apparaten voor het verzamelen van gegevens naar schijf zakelijke inzichten en automatisering. IoT Hub bevat bovendien functies die de relatie tussen uw apparaten en uw back-endsystemen verrijken. Bidirectionele communicatie mogelijkheden betekenen dat u ontvangt gegevens van apparaten kunnen ook opdrachten en het beleid naar sturen apparaten, bijvoorbeeld voor het bijwerken van eigenschappen of acties voor apparaat aanroepen.  Deze cloud-naar-apparaat-connectiviteit wordt ook gebruikt door de belangrijkste mogelijkheden van het leveren van intelligentie van de cloud naar edge-apparaten met Azure IoT Edge. De unieke id van op apparaatniveau opgegeven IoT Hub biedt betere beveiliging, helpt uw IoT-oplossing van mogelijke aanvallen. 

[Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) is van de big data. de service van Azure. Het is ontworpen voor hoge gegevensdoorvoer streaming scenario's waarbij klanten miljarden aanvragen per dag verzenden. Eventhubs maakt gebruik van een basis van gepartitioneerd gebruik-model uit uw stream te schalen en is geïntegreerd in de big data- en analyseservices van Azure, met inbegrip van Databricks, Stream Analytics, ADLS en HDInsight. Deze service is ontworpen met functies zoals Event Hubs Capture en automatisch vergroten, ter ondersteuning van uw big data-toepassingen en oplossingen. Bovendien IoT-Hub maakt gebruik van Event Hubs voor het pad van de stroom telemetrie, zodat uw IoT-oplossing ook van de enorme mogelijkheden van Event Hubs profiteren.

Alleen IoT Hub biedt om samen te vatten, terwijl beide oplossingen zijn ontworpen voor opname van gegevens op een zeer grote schaal, de uitgebreide IoT-specifieke mogelijkheden die zijn ontworpen voor u de waarde voor het bedrijf van uw IoT-apparaten verbinden met de Azure-cloud te maximaliseren.  Als uw IoT-traject net begint, basismodel beginnend met IoT Hub ter ondersteuning van uw gegevens opnemen's u hebt direct toegang tot de complete IoT-capaciteiten zodra uw zakelijke en technische behoeften ze nodig hebben.

De volgende tabel bevat informatie over hoe de twee niveaus van IoT-Hub met Event Hubs vergelijken als u ze voor IoT-mogelijkheden evalueren wilt. Zie voor meer informatie over de lagen standard en basic van IoT-Hub, [u bij het kiezen van de juiste IoT Hub-laag](iot-hub-scaling.md).

| IoT-mogelijkheid | Standard-laag van IoT Hub | IoT Hub basic-laag | Event Hubs |
| --- | --- | --- | --- |
| Apparaat-naar-cloud-berichten | ![Selecteren][checkmark] | ![Selecteren][checkmark] | ![Selecteren][checkmark] |
| Protocollen: HTTPS, AMQP, AMQP via webSockets | ![Selecteren][checkmark] | ![Selecteren][checkmark] | ![Selecteren][checkmark] |
| Protocollen: MQTT-, MQTT via webSockets | ![Selecteren][checkmark] | ![Selecteren][checkmark] |  |
| Id per apparaat | ![Selecteren][checkmark] | ![Selecteren][checkmark] |  |
| Bestanden van apparaten uploaden | ![Selecteren][checkmark] | ![Selecteren][checkmark] |  |
| Device Provisioning Service | ![Selecteren][checkmark] | ![Selecteren][checkmark] |  |
| Cloud-naar-apparaat-berichten | ![Selecteren][checkmark] |  |  |
| Dubbele apparaat- en Apparaatbeheer | ![Selecteren][checkmark] |  |  |
| Apparaat-streams (preview) | ![Selecteren][checkmark] |  |  |
| IoT Edge | ![Selecteren][checkmark] |  |  |

Zelfs als de enige use-case opname van apparaat-naar-cloud-gegevens wordt sterk aangeraden met behulp van IoT Hub biedt een service die is ontworpen voor connectiviteit van IoT-apparaten. 

### <a name="next-steps"></a>Volgende stappen

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie de [Ontwikkelaarshandleiding voor IoT Hub](iot-hub-devguide.md).

<!-- This one reference link is used over and over. --robinsh -->
[checkmark]: ./media/iot-hub-compare-event-hubs/ic195031.png
