---
title: Selecteren en implementeren van een ASC voor IoT-agent Preview | Microsoft Docs
description: Meer informatie over hoe selecteren en implementeren van ASC voor IoT security agents op IoT-apparaten.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 32a9564d-16fd-4b0d-9618-7d78d614ce76
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2019
ms.author: mlottner
ms.openlocfilehash: 5ce583fe98acb7e0a4aaeb720cb2d5ed5eebb9e3
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541960"
---
# <a name="select-and-deploy-a-security-agent-on-your-iot-device"></a>Selecteren en implementeren van een beveiligingsagent op uw IoT-apparaat

> [!IMPORTANT]
> ASC voor IoT is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.


ASC voor IoT biedt referentiearchitecturen voor beveiliging-agents die bewaken en verzamelen gegevens van IoT-apparaten.

Beveiligingsagenten als open source-projecten zijn ontwikkeld en zijn beschikbaar in twee versies: <br> [C](https://aka.ms/iot-security-github-c), en [ C# ](https://aka.ms/iot-security-github-cs).

## <a name="supported-platforms-for-asc-for-iot-agents"></a>Ondersteunde platforms voor ASC voor IoT-agents

De volgende lijst bevat alle ondersteunde platforms.  

|ASC voor IoT-agent |Besturingssysteem |Architectuur |
|--------------|------------|--------------|
|C|Ubuntu 16.04 |   x64|
|C|Ubuntu 18.04 |   x64|
|C|Debian 9 |   x64, x86|
|C#|Ubuntu 16.04    |x64|
|C#|Ubuntu 18.04    |x64|
|C#|Debian 9    |x64|
|C#|Windows Server 2016|    X64|
|C#|Windows 10 IoT Core build 17763 |x64|


## <a name="which-flavor-should-i-use"></a>Welke versie moet ik gebruiken?

Houd rekening met de volgende vragen met betrekking tot uw IoT-apparaten:

- Gebruikt u _Windows Server_ of _Windows IoT Core_? 

    Gebruik [ASC voor IoT-installatie voor Windows met de C#-security-agent op basis van](tutorial-deploy-windows-cs.md).

- Gebruikt u een Linux-distributie met x86 architectuur? 

    Gebruik [ASC voor IoT-installatie met de beveiliging op basis van de C-agent voor Linux](tutorial-deploy-linux-c.md).
- Gebruikt u een Linux-distributie met x64 architectuur?

    U kunt beide versies. <br>
    [ASC voor IoT-installatie met de beveiliging op basis van de C-agent voor Linux](tutorial-deploy-linux-c.md) en/of [ASC voor IoT-installatie voor Linux met de C#-security-agent op basis van](tutorial-deploy-linux-cs.md).

Beide soorten, zodat ze hebben dezelfde set functies en ondersteuning voor de vergelijkbare configuratieopties. De beveiliging op basis van de C-agent heeft een lagere geheugengebruik.


## <a name="next-steps"></a>Volgende stappen
- [Overzicht](overview.md)
- [Beveiliging-agents](security-agent-architecture.md)
- [Security agent verificatiemethoden](concept-security-agent-authentication-methods.md)
- [ASC voor veelgestelde vragen over IoT](resources-frequently-asked-questions.md)