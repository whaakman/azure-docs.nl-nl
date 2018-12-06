---
title: Een inleiding op de Azure Stack Development Kit (ASDK) | Microsoft Docs
description: Hierin wordt beschreven wat de ASDK is en algemene scenario's voor het evalueren van Microsoft Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.date: 10/25/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 345d62f4045e7b9279d52acf70cc340d088f5db0
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52961328"
---
# <a name="what-is-the-azure-stack-development-kit"></a>Wat is Azure Stack Development Kit?
[Microsoft Azure Stack-geïntegreerde systemen](../azure-stack-poc.md) in grootte variëren van 4-16 knooppunten en gezamenlijk worden ondersteund door een hardware-partner en Microsoft. Gebruik Azure Stack-geïntegreerde systemen om in te schakelen van nieuwe scenario's voor uw productieworkloads. Als u Azure Stack-operators die u beheert de infrastructuur geïntegreerde systemen en services biedt, raadpleegt u onze [operator documentatie](https://docs.microsoft.com/azure/azure-stack).

De Azure Stack Development Kit (ASDK) is een implementatie van één knooppunt van de Azure-Stack die u kunt downloaden en gebruiken **gratis**. Alle ASDK onderdelen zijn geïnstalleerd in virtuele machines op een afzonderlijke host-server-computer die moeten voldoen aan of groter zijn dan de [minimale hardwarevereisten](asdk-deploy-considerations.md#hardware). De ASDK is bedoeld om een omgeving waarin u kunt Azure Stack evalueren en ontwikkelen van moderne toepassingen met behulp van API's en hulpprogramma's consistent zijn met Azure in een *niet-productie* omgeving. 

> [!IMPORTANT]
> De ASDK is niet bedoeld om te worden gebruikt of in een productieomgeving wordt ondersteund.

Omdat alle onderdelen ASDK zijn geïmplementeerd op een hostcomputer van één development kit, zijn er beperkt fysieke resources beschikbaar. Bij implementaties op een ASDK zowel de Azure Stack-infrastructuur virtuele machines als de tenant virtuele machines naast elkaar op dezelfde server-computer. Deze configuratie is niet bedoeld voor evaluatie van de schaal of prestaties.

De ASDK is ontworpen voor een Azure-consistente hybride cloud-ervaring voor:
- **Beheerders** (Azure Stack-Operators). De ASDK is een fantastische bron om te evalueren en meer informatie over de beschikbare Azure Stack-services.
- **Ontwikkelaars**. De ASDK kan worden gebruikt voor het ontwikkelen van hybride of moderne toepassingen on-premises (dev/test-omgevingen). Dit biedt herhaalbaarheid van ontwikkelervaring voorafgaand aan of naast Azure Stack-productie-implementaties. 

Bekijk deze korte video voor meer informatie over de ASDK:

> [!VIDEO https://www.youtube.com/embed/dbVWDrl00MM]


## <a name="asdk-and-multi-node-azure-stack-differences"></a>Azure Stack verschillen in ASDK en meerdere knooppunten
Implementaties van één knooppunt ASDK verschillen van de Azure Stack implementaties met meerdere knooppunten in een aantal belangrijke manieren die u moet rekening houden met.

|Beschrijving|ASDK|Azure Stack met meerdere knooppunten|
|-----|-----|-----|
|**Schalen**|Alle onderdelen zijn geïnstalleerd op een computer met één knooppunt-server.|De grootte van 4-16 knooppunten kan variëren.|
|**Flexibiliteit**|Configuratie met één knooppunt biedt geen hoge beschikbaarheid|[Hoge beschikbaarheid](../azure-stack-key-features.md#high-availability-for-azure-stack) mogelijkheden worden ondersteund.|
|**Netwerken**|De ASDK maakt gebruik van een virtuele machine met de naam AzS-BGPNAT01 alle ASDK netwerkverkeer te routeren. Er zijn geen extra switch-vereisten.|De AzS-BGPNAT01-machine bestaat niet in implementaties met meerdere knooppunten. Complexere [routering netwerkinfrastructuur](../azure-stack-network.md#network-infrastructure) met inbegrip van Top Of Rack (TOR), Baseboard Management Controller (BMC) en switches van de rand (datacenternetwerk) nodig is.|
|**Patch- en bijwerkproces**|Als u wilt verplaatsen naar een nieuwe versie van de ASDK, moet u de ASDK op de hostcomputer development kit opnieuw implementeren.|[Patch en bij te werken](../azure-stack-updates.md) proces gebruikt voor het bijwerken van de geïnstalleerde versie van Azure Stack.|
|**Ondersteuning**|Azure Stack voor MSDN-forum. Ondersteuning voor klantenservice van Microsoft en ondersteuning (CSS) is *niet* beschikbaar voor niet-productieomgevingen.|[MSDN Azure Stack-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStack) en ondersteuning voor volledige CSS.|
| | |

## <a name="learn-about-available-services"></a>Meer informatie over beschikbare services
Als een Azure Stack-operators moet u weten welke services u kunt beschikbaar maken voor uw gebruikers. Azure Stack ondersteunt een subset van de Azure-services en de lijst met ondersteunde services worden nog steeds loop der tijd veranderen.

### <a name="foundational-services"></a>Fundamentele services
Azure Stack omvat standaard de volgende "fundamentele services" wanneer u de ASDK implementeert:
- Compute
- Storage
- Netwerken
- Key Vault

Met deze fundamentele services, kunt u de infrastructuur-as-a-Service (IaaS) bieden aan uw gebruikers met een minimale configuratie.

### <a name="additional-services"></a>Extra services
De volgende aanvullende Platform-as-a-Service (PaaS)-services worden op dit moment ondersteund:
- App Service
- Azure Functions
- SQL- en MySQL-databases

> [!NOTE]
> Deze services aanvullende configuratie vereisen voordat u deze beschikbaar voor uw gebruikers maken kunt en zijn niet standaard beschikbaar zijn wanneer u de ASDK installeert.

## <a name="service-roadmap"></a>Service-roadmap
Azure Stack blijft ondersteuning toevoegen voor extra Azure-services. Zie voor meer informatie over wat op met Azure Stack komst er, de [roadmap voor Azure Stack](https://azure.microsoft.com/roadmap/?tag=azure-stack). 


## <a name="next-steps"></a>Volgende stappen
Om te beginnen evalueren van Azure Stack, moet u eerst [downloaden van de meest recente ASDK](asdk-download.md) en voorbereiden van de hostcomputer ASDK. Nadat u de development kit host hebt voorbereid, kunt u de ASDK installeren en aanmelden bij de portals beheerder en gebruiker om te beginnen met behulp van Azure Stack.