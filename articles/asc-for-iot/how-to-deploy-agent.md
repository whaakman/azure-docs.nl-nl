---
title: Selecteren en implementeren van Azure Security Center voor IoT-agent Preview | Microsoft Docs
description: Meer informatie over hoe selecteren en implementeren van Azure Security Center voor IoT security agents op IoT-apparaten.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 32a9564d-16fd-4b0d-9618-7d78d614ce76
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2019
ms.author: mlottner
ms.openlocfilehash: 8f5a6187b0a651da9dd8de1cb5670a8faffded1a
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58862417"
---
# <a name="select-and-deploy-a-security-agent-on-your-iot-device"></a>Selecteren en implementeren van een beveiligingsagent op uw IoT-apparaat

> [!IMPORTANT]
> Azure Security Center voor IoT is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Azure Security Center (ASC) voor IoT biedt referentiearchitecturen voor beveiliging-agents die bewaken en verzamelen gegevens van IoT-apparaten.
Zie [Security agent referentiearchitectuur](security-agent-architecture.md) voor meer informatie.

Agents als open source-projecten zijn ontwikkeld en zijn beschikbaar in twee versies: <br> [C](https://aka.ms/iot-security-github-c), en [ C# ](https://aka.ms/iot-security-github-cs).

In dit artikel leert u het volgende: 
> [!div class="checklist"]
> * Vergelijk security agent-versies
> * Platforms van ondersteunde agents detecteren
> * Kies de juiste agent-versie voor uw oplossing

## <a name="understand-security-agent-options"></a>Agent beveiligingsopties begrijpen

Elke ASC voor IoT security agent smaak biedt dezelfde set functies en biedt ondersteuning voor vergelijkbare configuratieopties. 

De beveiliging op basis van de C-agent heeft een lagere geheugengebruik en is de ideale keuze voor apparaten met minder beschikbare bronnen. 

|     | Beveiliging op basis van de C agent | C#-op basis van de security-agent |
| --- | ----------- | --------- |
| Open source | Beschikbaar onder [MIT-licentie](https://en.wikipedia.org/wiki/MIT_License) in [Github](https://aka.ms/iot-security-github-cs) | Beschikbaar onder [MIT-licentie](https://en.wikipedia.org/wiki/MIT_License) in [Github](https://aka.ms/iot-security-github-c) |
| Talen    | C | C# |
| Ondersteunde Windows-platforms? | Nee | Ja |
| Windows-vereisten | --- | [WMI](https://docs.microsoft.com/en-us/windows/desktop/wmisdk/) |
| Ondersteunde Linux-platform? | Ja, x64 en x86 | Ja, alleen x64 |
| Linux-vereisten | libunwind8, libcurl3, uuid-runtime, auditd, audispd-invoegtoepassingen | libunwind8, libcurl3, uuid-runtime, auditd, audispd-plugins, sudo, netstat, iptables |
| Ingenomen schijfruimte | 10,5 MB | 90MB |
| Geheugengebruik (gemiddeld) | 5.5 MB | 33MB |
| [Verificatie](concept-security-agent-authentication-methods.md) naar IoT Hub | Ja | Ja |
| Beveiligingsgegevens [verzameling](how-to-agent-configuration.md#supported-security-events) | Ja | Ja |
| Aggregatie van gebeurtenis | Ja | Ja |
| Externe configuratie via [security moduledubbel](concept-security-module.md) | Ja | Ja |


## <a name="choose-an-agent-flavor"></a>Kies de smaak van een agent 

Beantwoord de volgende vragen over uw IoT-apparaten te selecteren van de juiste agent:

- Gebruikt u _Windows Server_ of _Windows IoT Core_? 

    [Implementeer een C#-security-agent voor Windows op basis van](how-to-deploy-windows-cs.md).

- Gebruikt u een Linux-distributie met x86 architectuur? 

    [Implementeren van een beveiliging op basis van de C-agent voor Linux](how-to-deploy-linux-c.md).

- Gebruikt u een Linux-distributie met x64 architectuur?

    U kunt een van beide smaak agent gebruiken. <br>
    [Implementeren van een beveiliging op basis van de C-agent voor Linux](how-to-deploy-linux-c.md) en/of [implementeren een C#-security-agent voor Linux op basis van](how-to-deploy-linux-cs.md).

Beide agent-versies bieden dezelfde set functies en ondersteuning voor vergelijkbare configuratieopties.
Zie [Security agent vergelijking](how-to-deploy-agent.md#understand-security-agent-options) voor meer informatie.

## <a name="supported-platforms"></a>Ondersteunde platforms

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

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over configuratie-opties, blijven de handleiding voor de configuratie van agent. 
> [!div class="nextstepaction"]
> [De configuratie van agent hoe u met betrekking tot](./how-to-agent-configuration.md)
