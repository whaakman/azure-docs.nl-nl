---
title: Inzicht in Azure Security Center voor IoT-agent beveiligingsarchitectuur Preview | Microsoft Docs
description: Meer informatie over de beveiligingsarchitectuur van agent voor agents die worden gebruikt in de Azure Security Center voor IoT-service.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: e78523ae-d70a-456a-818d-f8b1b025d7cb
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/25/2019
ms.author: mlottner
ms.openlocfilehash: e10cd3f60c3b12c6d5115ff34f4cbde2ef19d9fd
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58862808"
---
# <a name="security-agent-reference-architecture"></a>Security agent-referentiearchitectuur

> [!IMPORTANT]
> Azure Security Center voor IoT is momenteel in openbare preview.
> Deze preview-versie wordt geleverd zonder een service level agreement, en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.


Azure Security Center (ASC) voor IoT biedt referentiearchitectuur voor beveiliging-agents die zich, verwerken, samenvoegen en verzenden van beveiligingsgegevens via IoT hub.

Beveiligingsagenten zijn ontworpen voor gebruik in een omgeving met beperkte IoT en hoge mate aanpasbaar in termen van waarden die ze bieden in vergelijking tot de bronnen die waarvan ze gebruikmaken.

Beveiligingsagenten ondersteunen de volgende functies:

- Onbewerkte beveiligingsgebeurtenissen verzamelen van het onderliggende besturingssysteem (Linux, Windows). Zie voor meer informatie over beschikbare beveiligingsupdates gegevens collectors, [ASC voor de configuratie van de agent IoT](how-to-agent-configuration.md).

- Cumulatieve onbewerkte beveiligingsgebeurtenissen in berichten die worden verzonden via IoT hub.

- Verifiëren met het bestaande apparaat-id of een specifieke module-identiteit. Zie [Security agent verificatiemethoden](concept-security-agent-authentication-methods.md) voor meer informatie.

- Op afstand configureren door het gebruik van de **azureiotsecurity** moduledubbel. Zie voor meer informatie, [een ASC voor IoT-agent configureren](how-to-agent-configuration.md).

ASC voor IoT-beveiliging agents als open-source-projecten zijn ontwikkeld en zijn beschikbaar via GitHub: 

- [ASC op basis van IoT C-agent](https://github.com/Azure/Azure-IoT-Security-Agent-C) 
- [ASC voor IoT C#-op basis van agent](https://github.com/Azure/Azure-IoT-Security-Agent-CS)

## <a name="agent-supported-platforms"></a>Agent die wordt ondersteund platforms

ASC voor IoT biedt verschillende installer-agents voor 32-bits en 64-bits Windows- en hetzelfde voor 32-bits en 64-bits Linux. Zorg ervoor dat u het juiste agent-installatieprogramma voor elk van uw apparaten op basis van de volgende tabel:

| 32- of 64-bits | Linux | Windows |    Details|
|----------|----------------------------------------------|-------------|-------------------------------------------|
| 32-bits  | C  | C#  ||
| 64-bits  | C#of C           | C#      | De C-agent gebruiken voor apparaten met minimale resources|

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over ASC voor IoT-beveiligingsarchitectuur agent en installatieprogramma's van het beschikbaar.

Gebruik de volgende artikelen om door te gaan aan de slag met ASC voor IoT-implementatie:

- Inzicht in [Security agent verificatiemethoden](concept-security-agent-authentication-methods.md)
- Selecteer en implementeer een [security-agent](how-to-deploy-agent.md)
- Bekijk de ASC voor IoT [vereisten-Service](service-prerequisites.md)
- Meer informatie over het [ASC inschakelen voor IoT-service in uw IoT-Hub](quickstart-onboard-iot-hub.md)
- Meer informatie over de service van de [ASC voor veelgestelde vragen over IoT](resources-frequently-asked-questions.md)
