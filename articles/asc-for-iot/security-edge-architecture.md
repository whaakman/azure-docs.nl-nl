---
title: Inzicht in Azure Security Center voor IoT-beveiliging-module voor IoT Edge | Microsoft Docs
description: Meer informatie over de architectuur en capaciteiten van Azure Security Center voor IoT-beveiliging-module voor IoT Edge.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: e78523ae-d70a-456a-818d-f8b1b025d7cb
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/25/2019
ms.author: mlottner
ms.openlocfilehash: 847d6238bd42dc7da723dcc9acd47ed09c16dbf9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65200587"
---
# <a name="azure-iot-edge-security-module"></a>Azure IoT Edge-module voor beveiliging

> [!IMPORTANT]
> Azure Security Center voor IoT is momenteel in openbare preview.
> Deze preview-versie wordt geleverd zonder een service level agreement, en wordt niet aanbevolen voor productie worklo§1ads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

[Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/) biedt krachtige mogelijkheden om te beheren en zakelijke werkstromen aan de rand uitvoeren.
Het belangrijkste onderdeel dat IoT Edge speelt in IoT-omgevingen maken het met name geschikt voor kwaadwillende actoren.

Azure Security Center (ASC) voor IoT-beveiliging-module biedt een uitgebreide oplossing voor uw IoT-Edge apparaten.
ASC voor IoT-module worden verzameld, verzamelt en analyseert onbewerkte beveiligingsgegevens van uw besturingssysteem en container in de praktijk toepasbare aanbevelingen en waarschuwingen.

Net als bij ASC voor IoT-beveiliging-agents voor IoT-apparaten, de ASC voor IoT Edge-module is hoge mate aanpasbaar zijn via de moduledubbel.
Zie [configureren van uw agent](how-to-agent-configuration.md) voor meer informatie.

ASC voor IoT-beveiliging-module voor IoT Edge biedt de volgende functies:

- Verzamelt onbewerkte beveiligingsgebeurtenissen van het onderliggende besturingssysteem (Linux) en de IoT Edge-Container-systemen.
  
  Zie [ASC voor de configuratie van de agent IoT](how-to-agent-configuration.md) voor meer informatie over beschikbare beveiligingsupdates gegevens collectors.

- Analyse van de manifesten voor IoT Edge-implementatie.

- Onbewerkte beveiligingsgebeurtenissen samen tot berichten die worden verzonden via [IoT Edge Hub](https://docs.microsoft.com/azure/iot-edge/iot-edge-runtime#iot-edge-hub).

- Configuratie door gebruik te maken van de moduledubbel beveiliging verwijderen.

  Zie [een ASC voor IoT-agent configureren](how-to-agent-configuration.md) voor meer informatie.

ASC voor IoT-beveiliging-module voor IoT Edge wordt uitgevoerd in een beschermde modus onder IoT Edge.
Beschermde modus is vereist voor de module voor het bewaken van het besturingssysteem en andere IoT Edge-modules.

## <a name="agent-supported-platforms"></a>Agent die wordt ondersteund platforms

ASC voor IoT-beveiliging-module voor IoT Edge is momenteel alleen beschikbaar voor Linux.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over de architectuur en capaciteiten van ASC voor IoT-beveiliging-module voor IoT Edge.

Gebruik de volgende artikelen om door te gaan aan de slag met ASC voor IoT-implementatie:

- Implementeer [security-module voor IoT Edge](how-to-deploy-edge.md)
- Meer informatie over het [uw beveiligingsmodule configureren](how-to-agent-configuration.md)
- Bekijk de ASC voor IoT [vereisten-Service](service-prerequisites.md)
- Meer informatie over het [ASC inschakelen voor IoT-service in uw IoT-Hub](quickstart-onboard-iot-hub.md)
- Meer informatie over de service van de [ASC voor veelgestelde vragen over IoT](resources-frequently-asked-questions.md)