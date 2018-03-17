---
title: Een inleiding op Azure Stack Development Kit (ASDK) | Microsoft Docs
description: Hierin wordt beschreven wat de ASDK is en algemene gebruiksvoorbeelden voor het evalueren van Microsoft Azure-Stack.
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 5a268a29c7a767084049bf56270aa8bc9d2ccc3f
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/17/2018
---
# <a name="what-is-the-azure-stack-development-kit"></a>Wat is Azure Stack Development Kit?
[Microsoft Azure-Stack geïntegreerd systemen](.\.\azure-stack-poc.md) in grootte variëren van 4-12-knooppunten en gezamenlijk worden ondersteund door een partner voor hardware- en Microsoft. Gebruik Azure Stack geïntegreerd systemen nieuwe scenario's voor uw productie-workloads te maken. Als u een Azure-Stack-operator die de infrastructuur geïntegreerde systemen beheert en -services biedt, Zie onze [operator documentatie](https://docs.microsoft.com/azure/azure-stack).

Azure Stack Development Kit (ASDK) is een implementatie met één knooppunt van de Azure-Stack die u kunt downloaden en gebruiken **gratis**. Alle ASDK onderdelen zijn geïnstalleerd in de virtuele machines die worden uitgevoerd op een enkele server hostcomputer die moeten voldoen aan of groter zijn dan de [minimale hardwarevereisten](asdk-deploy-considerations.md#hardware). De ASDK is bedoeld voor een omgeving waarin u kunt Azure-Stack evalueren en ontwikkelen van moderne toepassingen met behulp van API's en tooling consistent zijn met Azure in een *niet-productieve* omgeving. 

> [!IMPORTANT]
> De ASDK is niet bedoeld om te worden gebruikt of in een productieomgeving wordt ondersteund.

Omdat alle ASDK-onderdelen op een hostcomputer in één development kit worden geïmplementeerd, zijn er beperkt fysieke resources beschikbaar. Met implementaties ASDK, de virtuele machines van Azure-Stack-infrastructuur en tenant virtuele machines naast elkaar bestaan op dezelfde server-computer. Deze configuratie is niet bedoeld voor evaluatie van scale- of Prestatieweergave.

De ASDK is zodanig ontworpen dat een Azure-consistente hybride cloud-ervaring voor:
- **Beheerders** (Azure Stack-Operators). De ASDK is een zeer waardevolle resource om te evalueren en meer informatie over de beschikbare Azure Stack-services.
- **Ontwikkelaars**. De ASDK kan worden gebruikt voor het ontwikkelen van hybride of moderne toepassingen on-premises (omgevingen ontwikkelen en testen). Dit biedt herhaalbaarheid van ontwikkeling biedt voorafgaand aan of naast Azure Stack productie-implementaties. 

Bekijk deze korte video voor meer informatie over de ASDK:

> [!VIDEO https://www.youtube.com/embed/dbVWDrl00MM]


## <a name="asdk-and-multi-node-azure-stack-differences"></a>Azure Stack verschillen in ASDK en met meerdere knooppunten
Implementaties van één knooppunt ASDK verschillen van Azure Stack implementaties met meerdere knooppunten in een paar belangrijke manieren waarop u houden moet rekening.

|Beschrijving|ASDK|Azure Stack met meerdere knooppunten|
|-----|-----|-----|
|**Schalen**|Alle onderdelen zijn geïnstalleerd op een server met één knooppunt.|De grootte van 4-12-knooppunten kan variëren.|
|**Herstelmogelijkheden**|Configuratie met één knooppunt biedt geen hoge beschikbaarheid|[Hoge beschikbaarheid](.\.\azure-stack-key-features.md#high-availability-for-azure-stack) mogelijkheden worden ondersteund.|
|**Netwerken**|De ASDK maakt gebruik van een virtuele machine met de naam AzS BGPNAT01 alle ASDK netwerkverkeer te routeren. Er zijn geen extra switch-vereisten.|De VM AzS BGPNAT01 bestaat niet in implementaties met meerdere knooppunten. Complexere [routering netwerkinfrastructuur](.\.\azure-stack-network.md#network-infrastructure) zoals Top Of Rack (TOR), Baseboard Management Controller (BMC) en switches rand (datacenternetwerk) nodig.|
|**Patch- en bijwerkproces**|Als u wilt verplaatsen naar een nieuwe versie van de ASDK, moet u de ASDK op de hostcomputer van development kit opnieuw implementeren.|[De patch en bij te werken](.\.\azure-stack-updates.md) proces gebruikt voor het bijwerken van de geïnstalleerde versie van de Azure-Stack.|
|**Ondersteuning**|Azure-Stack MSDN-forum. Ondersteuning voor klantenservice van Microsoft en ondersteuning (CSS) is *niet* beschikbaar voor niet-productieve omgevingen.|[Azure-Stack MSDN-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStack) en volledige CSS ondersteunen.|
| | |

## <a name="learn-about-available-services"></a>Meer informatie over de beschikbare services
Als een Azure-Stack-Operator moet u weten welke services kunt u beschikbaar maken voor uw gebruikers. Azure-Stack ondersteunt een subset van de Azure-services en de lijst met ondersteunde services blijft ontwikkelen gedurende een bepaalde periode.

### <a name="foundational-services"></a>Fundamentele services
Azure-Stack omvat standaard de volgende 'fundamentele services' wanneer u de ASDK implementeert:
- Compute
- Storage
- Netwerken
- Key Vault

U kunt deze fundamentele Services Infrastructure-as-a-Service (IaaS) aanbieden aan uw gebruikers met een minimale configuratie.

### <a name="additional-services"></a>Extra services
Op dit moment wordt worden de volgende aanvullende Platform-as-a-Service (PaaS)-services ondersteund:
- App Service
- Azure Functions
- SQL- en MySQL-databases

> [!NOTE]
> Deze services aanvullende configuratie vereisen voordat u ze beschikbaar voor uw gebruikers maken kunt en zijn niet standaard beschikbaar wanneer u de ASDK installeert.

## <a name="service-roadmap"></a>Roadmap voor service
Azure Stack blijft ondersteuning toevoegen voor extra Azure-services. Zie voor meer informatie over wat er met de Azure-Stack volgende, de [Azure Stack roadmap](https://azure.microsoft.com/roadmap/?tag=azure-stack). 


## <a name="next-steps"></a>Volgende stappen
Om te beginnen Azure Stack evalueren, moet u de development kit host voorbereiden server-computer en vervolgens [installeren de ASDK](asdk-deploy.md). Daarna kunt kunt u aanmelden bij de portals beheerder en gebruiker aan de slag met Azure-Stack.