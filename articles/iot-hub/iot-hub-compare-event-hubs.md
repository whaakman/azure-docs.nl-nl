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
ms.openlocfilehash: b86132b42aef981e6218b27e271e6db645d14071
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="connecting-iot-devices-to-azure-iot-hub-and-event-hubs"></a>IoT-apparaten verbinding laten maken naar Azure: IoT-Hub en Event Hubs

Azure biedt services die specifiek zijn ontwikkeld voor diverse typen connectiviteit en communicatie voor hulp bij het verbinding maken met uw gegevens tot de macht van de cloud. Zowel Azure IoT Hub en Azure Event Hubs zijn cloudservices die kunnen grote hoeveelheden gegevens en het proces voor opnemen of die gegevens voor zakelijke inzichten opslaan. De twee services zijn vergelijkbaar in dat ze beide opname van gegevens met weinig latentie en hoge betrouwbaarheid ondersteunen, maar ze zijn ontworpen voor verschillende doeleinden. IoT Hub is speciaal ontworpen houden met de unieke vereisten van het verbinden van IoT-apparaten, op schaal naar de Cloud met Azure Event Hubs is ontworpen voor big data streaming. Dit is de reden waarom Microsoft raadt het gebruik van Azure IoT Hub IoT-apparaten verbinding te maken met Azure

Azure IoT Hub wordt de cloudgateway die verbinding IoT-apparaten maakt voor het verzamelen van gegevens naar schijf zakelijke inzichten en automatisering. IoT-Hub bevat bovendien functies die de relatie tussen uw apparaten en uw back endsystemen aanvullen. Bidirectionele communicatie mogelijkheden beschikken ontvangen van gegevens van apparaten kunnen ook versturen opdrachten en het beleid terug naar apparaten, bijvoorbeeld eigenschappen bijwerken of acties voor apparaat worden aangeroepen.  Deze connectiviteit cloud-naar-apparaat wordt ook de belangrijkste mogelijkheden van de cloud intelligence bij uw randapparaten af te leveren met Azure IoT rand. De unieke id van de op apparaatniveau geleverd IoT Hub biedt betere beveiliging, helpt uw IoT-oplossing van mogelijke aanvallen. 

[Azure Event Hubs] [ Azure Event Hubs] heeft de big data streaming-service van Azure. Het is ontworpen voor hoge doorvoersnelheid gegevens streaming scenario's waarin klanten miljarden aanvragen per dag kunnen verzenden. Event Hubs maakt gebruik van een gepartitioneerde consumer uit uw stream te schalen en is geïntegreerd in de big data en analytics-services van Azure, met inbegrip van Databricks, Stream Analytics, ADLS en HDInsight. Met functies zoals Event Hubs vastleggen en automatisch vergroten, wordt deze service ontworpen ter ondersteuning van uw big data-apps en -oplossingen. Bovendien IoT-Hub maakt gebruik van Event Hubs voor het pad van de stroom telemetrie zodat uw IoT-oplossing ook van de enorme mogelijkheden van Event Hubs gebruikmaakt.

IoT-Hub biedt om samen te vatten, terwijl beide oplossingen zijn ontworpen voor gegevensopname een grootschalige, de uitgebreide IoT-specifieke mogelijkheden die zijn ontworpen om te maximaliseren, de zakelijke voordelen van uw IoT-apparaten verbinding met de Azure-cloud.  Als uw IoT-reis net begint, wordt beginnen met IoT Hub ter ondersteuning van uw gegevens opname scenario's zorgen dat u direct toegang tot de IoT uitgebreide mogelijkheden hebben wanneer ze nodig zijn voor uw technische en zakelijke behoeften.

De volgende tabel bevat informatie over hoe de twee lagen van IoT Hub met Event Hubs vergelijken wanneer u ze voor IoT-mogelijkheden evalueren wilt. Zie voor meer informatie over de lagen standard en basic van IoT Hub [het kiezen van de juiste laag van de IoT Hub][lnk-scaling].

| IoT-functionaliteit | Standard-laag van IoT-Hub | De basisstaffel IoT-Hub | Event Hubs |
| --- | --- | --- | --- |
| Apparaat-naar-cloud-berichten | ![Selecteren][1] | ![Selecteren][1] | ![Selecteren][1] |
| Protocollen: HTTPS, AMQP, AMQP via webSockets | ![Selecteren][1] | ![Selecteren][1] | ![Selecteren][1] |
| Protocollen: MQTT, MQTT via webSockets | ![Selecteren][1] | ![Selecteren][1] |  |
| Id per apparaat | ![Selecteren][1] | ![Selecteren][1] |  |
| Bestanden van apparaten uploaden | ![Selecteren][1] | ![Selecteren][1] |  |
| Device Provisioning Service | ![Selecteren][1] | ![Selecteren][1] |  |
| Cloud-naar-apparaat-berichten | ![Selecteren][1] |  |  |
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
