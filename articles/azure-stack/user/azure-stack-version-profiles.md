---
title: Beheren van profielen voor API-versie in Azure-Stack | Microsoft Docs
description: Meer informatie over API-versie-profielen in Azure-Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 8A336052-8520-41D2-AF6F-0CCE23F727B4
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2018
ms.author: mabrigg
ms.reviewer: sijuman
ms.openlocfilehash: 452ed1de0588b380747edaa44dd0cc3805c51392
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="manage-api-version-profiles-in-azure-stack"></a>Profielen voor API-versie in Azure-Stack beheren

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

API-profielen opgeven voor de Azure-resource-provider en de API-versie voor de REST van de Azure-eindpunten. U kunt aangepaste clients in verschillende talen met behulp van API-profielen maken. Elke client gebruikt een API-profiel te maken met de juiste resourceprovider en de API-versie voor Azure-Stack. 

U kunt een app werkt met Azure-resourceproviders zonder te sorteren uit precies welke versie van elke resourceprovider API compatibel met Azure-Stack is maken. Uw toepassing aan een profiel; alleen uitlijnen de SDK wordt teruggezet naar de juiste API-versies.


In dit onderwerp helpt u:
 - Profielen van de API voor Azure-Stack kennen.
 - Hoe kunt u API profielen gebruiken om uw oplossingen te ontwikkelen.
 - Waar u wilt zoeken voor code-specifieke richtlijnen.

## <a name="summary-of-api-profiles"></a>Overzicht van de API-profielen

- API-profielen worden gebruikt om een verzameling Azure-resourceproviders en de versies van hun API te representeren.
- API-profielen zijn gemaakt voor ontwikkelaars om sjablonen te maken tussen meerdere Azure-Clouds. Ze zijn ontworpen om te voldoen aan de behoefte aan een compatibel en stabiele-interfaces.
- Profielen worden vrijgegeven vier keer per jaar.
- Er zijn drie naamgevingsregels van profiel:
    - **meest recente**  
        Meest recente API-versies uitgebracht in Azure.
    - **yyyy-mm-dd-hybrid**  
    Uitgebracht op een halfjaarlijkse uitgebracht, deze release gericht op consistentie en stabiliteit tussen meerdere clouds.
    - **jjjj-mm-dd-profiel**  
    Bevindt zich tussen optimale stabiliteit en de nieuwste functies.

## <a name="azure-resource-manager-api-profiles"></a>Azure Resource Manager-API-profielen

Azure Stack gebruikt geen de nieuwste versie van de API-versies gevonden in globale Azure. Voordat u uw eigen oplossing maakt, moet u de API-versie voor elke resourceprovider niet vinden in Azure die compatibel is met de Azure-Stack.

In plaats daarvan dan onderzoek elke resourceprovider en de specifieke versie ondersteund door Azure Stack, kunt u een API-profiel. Het profiel bevat een set resourceproviders en API-versies. De SDK of een hulpprogramma dat wordt gebouwd met de SDK, wordt de gegevensmodelversie teruggezet naar de doel-api-versie opgegeven in het profiel. Met API-profielen, kunt u een Profielversie die voor een volledige sjabloon geldt en tijdens runtime, selecteert u de Azure Resource Manager de juiste versie van de resource.

API-profielen werken met hulpprogramma's die gebruikmaken van Azure Resource Manager, zoals PowerShell, Azure CLI, opgegeven in de SDK en Microsoft Visual Studio code. Hulpprogramma's en SDK's kunnen profielen gebruiken om te lezen welke versie van de modules en bibliotheken om op te nemen bij het bouwen van een toepassing.

Bijvoorbeeld, als gebruik PowerShell voor het maken van een storage-account met de **Microsoft.Storage** resourceprovider, die ondersteuning biedt voor 2016-03-30 api-versie en een virtuele machine met behulp van de Microsoft.Compute-resourceprovider met api-versie 2015-12-01 , moet u om te zoeken die ondersteuning voor PowerShell-Module biedt 2016-03-30 voor opslag en welke Module ondersteunt 2015-02-01 voor berekeningen en te installeren. In plaats daarvan kunt u een profiel. Gebruik de cmdlet ** installeren profiel * profilename *** en PowerShell laadt u de juiste versie van de modules.

Wanneer u de Python SDK voor het bouwen van een toepassing op basis van Python, kunt u ook het profiel opgeven. De SDK worden de juiste modules geladen voor de resourceproviders die u hebt opgegeven in uw script.

Als een ontwikkelaar, kunt u zich richten op het schrijven van uw oplossing. In plaats van te onderzoeken welke api-versies, resourceprovider, en welke cloud-werkt samen, moet u gebruikmaken van een profiel en weet dat uw code zal werken tussen alle clouds die ondersteuning bieden voor dit profiel.

## <a name="api-profile-code-samples"></a>API-profiel-codevoorbeelden

Hier vindt u codevoorbeelden kunt u uw oplossing integreren met uw voorkeurstaal met Azure-Stack door middel van profielen. Op dit moment vindt u instructies en voorbeelden voor de volgende talen:

- **PowerShell**  
U kunt de **AzureRM.Bootstrapper** module beschikbaar via de PowerShell-galerie ophalen van de PowerShell-cmdlets die werken met profielen voor API-versie vereist.  
Zie voor informatie [profielen voor gebruik API-versie voor PowerShell](azure-stack-version-profiles-powershell.md).
- **Azure CLI 2.0**  
U kunt de configuratie van uw omgeving voor het gebruik van de Azure-Stack specifiek API-versie profiel bijwerken.  
Zie voor meer informatie [profielen voor gebruik API-versie voor Azure CLI 2.0](azure-stack-version-profiles-azurecli2.md).
- **GO**  
In de SDK gaat u is een profiel een combinatie van verschillende brontypen met verschillende versies van andere services. profielen zijn beschikbaar onder de profielen / pad met de versie in de **jjjj-MM-DD** indeling.  
Zie voor meer informatie [profielen voor gebruik API-versie voor Ga](azure-stack-version-profiles-go.md).

## <a name="next-steps"></a>Volgende stappen
* [PowerShell voor Azure Stack installeren](azure-stack-powershell-install.md)
* [Configureren van de gebruiker van de Stack van Azure PowerShell-omgeving](azure-stack-powershell-configure-user.md)
* [Bekijk de details over resource provider API-versies ondersteund door de profielen](azure-stack-profiles-azure-resource-manager-versions.md).
