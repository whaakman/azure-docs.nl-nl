---
title: Inzicht krijgen in ASC voor IoT-oplossingsarchitectuur Preview | Microsoft Docs
description: Meer informatie over de stroom van gegevens in de ASC voor IoT-service.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 2cf6a49b-5d35-491f-abc3-63ec24eb4bc2
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2019
ms.author: mlottner
ms.openlocfilehash: ee81d9543525ba1187fc6c078391559929b9bf96
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541900"
---
# <a name="asc-for-iot-architecture"></a>ASC voor IoT-architectuur

In dit artikel wordt uitgelegd dat de functionele systeemarchitectuur van de Azure Security Center voor IoT-oplossing. 

> [!IMPORTANT]
> ASC voor IoT is momenteel in openbare preview.
> Deze preview-versie wordt geleverd zonder een service level agreement, en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="asc-for-iot-components"></a>ASC voor IoT-onderdelen

ASC voor IoT bestaat uit de volgende onderdelen:
- Apparaat-agents
- Bericht SDK verzenden
- IoT Hub-integratie
- Analytics-pijplijn
 
### <a name="asc-for-iot-workflow"></a>ASC voor IoT-werkstroom

ASC voor IoT-apparaat agents kunt u eenvoudig onbewerkte beveiligingsgebeurtenissen verzamelen van uw apparaten. Onbewerkte beveiligingsgebeurtenissen kunnen opnemen IP-verbindingen, maken van het proces, aanmeldingen en andere beveiliging relevante informatie. ASC voor IoT-apparaat agents worden ook gebeurtenis aggregatie om te voorkomen dat met hoge netwerkdoorvoer verwerkt. De agents zijn hoge mate aanpasbaar, zodat u kunt deze gebruiken voor specifieke taken, zoals het verzenden van alleen belangrijke informatie in de snelste SLA, of voor het verzamelen van gegevens van de uitgebreide beveiliging en -context in grotere segmenten hogere servicekosten voor de te voorkomen.
 
Apparaat-agents en het gebruik van andere toepassingen de **Azure ASC verzenden bericht SDK** voor het verzenden van informatie over beveiliging in Azure IoT Hub. IoT Hub neemt deze informatie en stuurt deze door aan de ASC voor IoT-service.

Zodra de ASC voor IoT-service is ingeschakeld, naast de doorgestuurde gegevens verzendt IoT Hub ook alle interne gegevens voor analyse door ASC voor IoT. Deze gegevens omvatten de apparaat-cloud-logboeken voor bewerkingen, apparaat-id's en de configuratie van de Hub. Al deze informatie helpt bij het maken van de ASC van IoT analytics-pijplijn.
 
ASC voor pijplijn van IoT analytics ontvangt ook meer threat intelligence streams uit verschillende bronnen binnen Microsoft en Microsoft partners. De ASC voor de hele analysepijplijn IoT werkt met de configuratie van elke klant aangebracht op de service (zoals aangepaste waarschuwingen en het gebruik van de beveiliging van het verzonden bericht SDK).
 
ASC voor IoT combineert met behulp van de analytics-pijplijn, alle van de stromen die gegevens voor de praktijk toepasbare aanbevelingen en waarschuwingen te genereren. De pijplijn bevat beide aangepaste regels die zijn gemaakt door beveiligingsonderzoekers en experts, evenals machine learning-modellen te zoeken naar afwijkingen van analyse voor het gedrag en het risico van standard-apparaat.
 
ASC voor IoT-aanbevelingen en waarschuwingen (uitvoer analysepijplijn) wordt geschreven naar de Log Analytics-werkruimte van elke klant. Met inbegrip van de ruwe gebeurtenissen in de werkruimte, evenals de waarschuwingen en aanbevelingen, kunt u gedetailleerde informatie over onderzoek en query's met behulp van de exacte details van de verdachte activiteiten gedetecteerd.  

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over de basisarchitectuur en de werkstroom van ASC voor IoT-oplossing. Voor meer informatie over vereisten, hoe u aan de slag en inschakelen van uw oplossing van IoT-Hub, Zie de volgende artikelen:

- [Service-vereisten](service-prerequisites.md)
- [Aan de slag](getting-started.md)
- [Uw oplossing configureren](quickstart-configure-your-solution.md)
- [Beveiliging van IoT-Hub inschakelen](quickstart-onboard-iot-hub.md)
- [ASC voor veelgestelde vragen over IoT](resources-frequently-asked-questions.md)
- [ASC voor IoT-beveiligingswaarschuwingen](concept-security-alerts.md)

