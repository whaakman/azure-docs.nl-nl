---
title: Beheren van profielen voor API-versie in Azure-Stack | Microsoft Docs
description: Meer informatie over API-versie-profielen in Azure-Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2018
ms.author: mabrigg
ms.reviewer: sijuman
ms.openlocfilehash: e568ffd2c3adb97ed0b727b85e7888fb797db1f9
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="manage-api-version-profiles-in-azure-stack"></a>Profielen voor API-versie in Azure-Stack beheren

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

API-profielen opgeven voor de Azure-resource-provider en de API-versie voor de REST van de Azure-eindpunten. U kunt aangepaste clients in verschillende talen met behulp van API-profielen maken. Elke client gebruikt een API-profiel te maken met de juiste resourceprovider en de API-versie voor Azure-Stack.

U kunt een app werkt met Azure-resourceproviders zonder te sorteren uit precies welke versie van elke resourceprovider API compatibel met Azure-Stack is maken. Uw toepassing aan een profiel; alleen uitlijnen de SDK wordt teruggezet naar de juiste API-versie.

In dit onderwerp helpt u:

 - Profielen van de API voor Azure-Stack kennen.
 - Meer informatie over hoe u API profielen kunt gebruiken om uw oplossingen te ontwikkelen.
 - Zie waar vind ik code-specifieke richtlijnen.

## <a name="summary-of-api-profiles"></a>Overzicht van de API-profielen

- API-profielen worden gebruikt om een verzameling Azure-resourceproviders en de versies van hun API te representeren.
- API-profielen zijn gemaakt voor ontwikkelaars, zodat ze sjablonen tussen meerdere Azure-Clouds maken kunnen. Profielen zijn ontworpen om te voldoen aan de noodzaak van een compatibel en stabiele-interface.
- Profielen worden vrijgegeven vier keer per jaar.
- Drie profiel naamconventies worden gebruikt:
    - **meest recente**  
        Bevat de meest recente API-versies in globale Azure uitgebracht.
    - **yyyy-mm-dd-hybrid**  
    Uitgebracht op een halfjaarlijkse uitgebracht, deze release is gericht op consistentie en stabiliteit tussen meerdere clouds. Dit profiel is bedoeld voor optimale Azure Stack-compatibiliteit.
    - **jjjj-mm-dd-profiel** tussen optimale stabiliteit en de nieuwste functies.

### <a name="azure-api-profiles-and-azure-stack-compatibility"></a>Azure-Stack-compatibiliteit en Azure API-profielen

De nieuwste Azure-API-profielen zijn niet compatibel met Azure-Stack. U kunt de volgende naamconventies gebruiken om te bepalen welke profielen wilt gebruiken voor uw Azure-Stack-oplossingen.

**meest recente**  
Dit profiel is de meest recente API-versies gevonden in globale Azure niet in Azure-Stack werkt. **Meest recente** heeft het grootste aantal grote wijzigingen. Het profiel wordt gereserveerd stabiliteit en compatibiliteit met andere clouds. Als u probeert te gebruiken van de meest recente API-versies **nieuwste** is het profiel dat u moet gebruiken.

**Jjjj-mm-dd-hybride**  
Dit profiel is uitgebracht in maart en September elk jaar. Dit profiel heeft optimale stabiliteit en compatibiliteit met verschillende clouds. **Jjjj-mm-dd-hybride** is bedoeld als globale Azure en Azure-Stack. De Azure-API-versies die worden beschreven in dit profiel is hetzelfde als de waarden die worden vermeld op Azure-Stack. U kunt dit profiel gebruiken voor het ontwikkelen van code voor hybride cloudoplossingen.

**jjjj-mm-dd-profiel**  
Dit profiel wordt voor globale Azure uitgebracht in juni en December. Dit profiel werkt op basis van de Azure-Stack; niet doorgaans wordt er veel grote wijzigingen. Hoewel deze bevindt zich tussen optimale stabiliteit en de nieuwste functies, het verschil tussen **nieuwste** en dit profiel is dat **nieuwste** bestaan altijd de nieuwste API-versies, ongeacht wanneer de API is uitgebracht. Bijvoorbeeld, als een nieuwe API-versie is gemaakt voor de Compute-API morgen, die API-versie wordt weergegeven de **nieuwste**, maar niet in de **jjjj-mm-dd-profiel** omdat dit profiel al bestaat.  **jjjj-mm-dd-profiel** bevat informatie over de meest recente versies die zijn uitgebracht vóór juni of vóór December.

## <a name="azure-resource-manager-api-profiles"></a>Azure Resource Manager-API-profielen

Azure Stack gebruikt geen de nieuwste versie van de API-versies gevonden in globale Azure. Wanneer u een oplossing maakt, moet u de API-versie vinden voor elke Azure-resourceprovider die compatibel is met de Azure-Stack.

In plaats daarvan dan onderzoek elke resourceprovider en de specifieke versie ondersteund door Azure Stack, kunt u een API-profiel. Het profiel bevat een set resourceproviders en API-versies. De SDK of een hulpprogramma dat wordt gebouwd met de SDK, wordt de gegevensmodelversie teruggezet naar de doel-api-versie opgegeven in het profiel. Met API-profielen, kunt u een Profielversie die voor een volledige sjabloon geldt en tijdens runtime, selecteert u de Azure Resource Manager de juiste versie van de resource.

API-profielen werken met hulpprogramma's die gebruikmaken van Azure Resource Manager, zoals PowerShell, Azure CLI, opgegeven in de SDK en Microsoft Visual Studio code. Hulpprogramma's en SDK's kunnen profielen gebruiken om te lezen welke versie van de modules en bibliotheken om op te nemen bij het bouwen van een toepassing.

**Ontwikkelingsscenario's voor het gebruik van profiel**  
Stel dat u PowerShell gebruikt om te maken:

* Een opslagaccount die gebruikmaakt van de **Microsoft.Storage** resourceprovider, die ondersteuning biedt voor api-versie 2016-03-30.
* Een virtuele machine die gebruikmaakt van de **Microsoft.Compute** resourceprovider, die ondersteuning biedt voor api-versie 2015-12-01.

In plaats van zoeken en installeren van de PowerShell-modules die ondersteuning bieden voor de api-versies die u voor opslagruimte en rekencapaciteit nodig hebt, kunt u een profiel. Gebruik de cmdlet ** installeren profiel * profilename *** en PowerShell laadt u de juiste versie van de modules.

Op dezelfde manier als u de Python SDK voor het bouwen van een toepassing op basis van Python gebruikt, kunt u een profiel. De SDK worden de juiste modules geladen voor de resourceproviders die u hebt opgegeven in uw script.

Als een ontwikkelaar, kunt u zich richten op het schrijven van uw oplossing. U kunt een profiel dat bekend is dat uw code zal werken tussen alle clouds die ondersteuning bieden voor het profiel gebruiken.

## <a name="api-profile-code-samples"></a>API-profiel-codevoorbeelden

Hier vindt u codevoorbeelden kunt u uw oplossing integreren met uw voorkeurstaal met Azure-Stack door middel van profielen. Op dit moment vindt u instructies en voorbeelden voor de volgende talen:

- **PowerShell**  
U kunt de **AzureRM.Bootstrapper** module beschikbaar via de PowerShell-galerie ophalen van de PowerShell-cmdlets die werken met profielen voor API-versie vereist. Zie voor informatie [profielen voor gebruik API-versie voor PowerShell](azure-stack-version-profiles-powershell.md).
- **Azure CLI 2.0**  
U kunt de configuratie van uw omgeving voor het gebruik van de Azure-Stack specifiek API-versie profiel bijwerken. Zie voor meer informatie [profielen voor gebruik API-versie voor Azure CLI 2.0](azure-stack-version-profiles-azurecli2.md).
- **GO**  
In de SDK gaat u is een profiel een combinatie van verschillende brontypen met verschillende versies van andere services. profielen zijn beschikbaar onder de profielen / pad met de versie in de **jjjj-MM-DD** indeling. Zie voor informatie [profielen voor gebruik API-versie voor Ga](azure-stack-version-profiles-go.md).
- **Ruby**  
De SDK voor Ruby voor Azure Stack van Resource Manager biedt hulpprogramma's waarmee u kunt ontwikkelen en beheren van uw infrastructuur. Resourceproviders in de SDK bevatten compute, virtuele netwerken en opslag, waarbij de Ruby taal. Zie voor informatie [gebruik API-versie profielen met Ruby](azure-stack-version-profiles-ruby.md)

## <a name="next-steps"></a>Volgende stappen

* [PowerShell voor Azure Stack installeren](azure-stack-powershell-install.md)
* [Configureren van de gebruiker van de Stack van Azure PowerShell-omgeving](azure-stack-powershell-configure-user.md)
* [Bekijk de details over resource provider API-versies ondersteund door de profielen](azure-stack-profiles-azure-resource-manager-versions.md).
