---
title: Beheren van API-versieprofielen in Azure Stack | Microsoft Docs
description: Meer informatie over API-versieprofielen in Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: sijuman
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: 28ff7c9f6ca5fc6365b3fe1b9a91d2159c8b3f48
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55247609"
---
# <a name="manage-api-version-profiles-in-azure-stack"></a>Beheren van API-versieprofielen in Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

API-profielen opgeven voor de Azure-resource-provider en de API-versie voor Azure REST-eindpunten. U kunt aangepaste clients maken in verschillende talen met behulp van API-profielen. Elke client gebruikt een API-profiel om contact op met de juiste resourceprovider en de API-versie voor Azure Stack.

U kunt een app om te werken met Azure-resourceproviders zonder te ontzenuwen precies welke versie van elke resourceprovider API compatibel met Azure Stack is kunt maken. Uw toepassing aan een profiel; alleen uitlijnen de SDK wordt teruggezet naar de juiste API-versie.

Dit onderwerp helpt u:

 - Meer informatie over API-profielen voor Azure Stack.
 - Lees hoe u API-profielen kunt gebruiken om uw oplossingen te ontwikkelen.
 - Leer waarop u moet code-specifieke richtlijnen.

## <a name="summary-of-api-profiles"></a>Overzicht van de API-profielen

- API-profielen worden gebruikt om weer te geven van een set met Azure-resource-providers en hun API-versies.
- API-profielen zijn gemaakt voor u om sjablonen te maken over meerdere Azure-clouds. Profielen bieden een interface compatibel en stabiel.
- Profielen worden vrijgegeven vier keer per jaar.
- Drie profiel naamconventies worden gebruikt:
    - **latest**  
        Bevat de meest recente API-versies die zijn uitgebracht in globale Azure.
    - **yyyy-mm-dd-hybrid**  
    Elk half jaar uitgebracht, deze release is gericht op consistentie en stabiliteit in meerdere clouds. Dit profiel is bedoeld voor optimale Azure Stack-compatibiliteit.
    - **jjjj-mm-dd-profiel** saldi optimale stabiliteit en de nieuwste functies.

## <a name="azure-api-profiles-and-azure-stack-compatibility"></a>Azure API-profielen en compatibiliteit met Azure Stack

De nieuwste Azure-API-profielen zijn niet compatibel met Azure Stack. U kunt de volgende naamconventies gebruiken om te identificeren welke profielen wilt gebruiken voor uw Azure Stack-oplossingen.

**Latest**  
Dit profiel is de meest recente API-versies gevonden in de globale Azure, die niet in Azure Stack werken. **Meest recente** is het grootste aantal wijzigingen die fouten veroorzaken. Het profiel hebt gereserveerd stabiliteit en compatibiliteit met andere clouds. Als u probeert te gebruiken van de meest recente API-versies, **nieuwste** is het profiel dat u moet gebruiken.

**Yyyy-mm-dd-hybrid**  
Dit profiel wordt vrijgegeven in maart en September elk jaar. Het bevat optimale stabiliteit en compatibiliteit met verschillende clouds. **Jjjj-mm-dd-hybride** is ontworpen om u te richten op algemene Azure en Azure Stack. De Azure-API-versies die worden vermeld in dit profiel is hetzelfde als die worden vermeld in Azure Stack. U kunt dit profiel gebruiken voor het ontwikkelen van code voor hybride cloudoplossingen.

**yyyy-mm-dd-profile**  
Dit profiel is die zijn uitgebracht voor algemene Azure in juni en December. Werkt niet met Azure Stack; Normaal gesproken wordt er veel belangrijke wijzigingen. Hoewel het optimale stabiliteit en de nieuwste functies, het verschil tussen saldi **nieuwste** en dit profiel is die **nieuwste** bestaat altijd de nieuwste API-versies, ongeacht wanneer de API is uitgebracht. Bijvoorbeeld, als een nieuwe API-versie morgen voor de Compute-API gemaakt wordt, die API-versie wordt weergegeven in de **nieuwste**, maar niet in de **jjjj-mm-dd-profiel** -profiel, omdat dit profiel al bestaat. **jjjj-mm-dd-profiel** bevat informatie over de meest recente versies die zijn uitgebracht vóór juni of vóór December.

## <a name="azure-resource-manager-api-profiles"></a>Azure Resource Manager-API-profielen

Azure Stack gebruikt niet de nieuwste versie van de API-versies gevonden in de globale Azure. Wanneer u een oplossing maakt, moet u de API-versie voor elke Azure-resourceprovider die compatibel is met Azure Stack.

In plaats daarvan kunt u dan onderzoek elke resourceprovider en de specifieke versie wordt ondersteund door Azure Stack, een API-profiel gebruiken. Het profiel bevat een set van resourceproviders en -API-versies. De SDK of een hulpprogramma gebouwd met de SDK wordt hersteld naar de doel- `api-version` opgegeven in het profiel. Met API-profielen, kunt u een Profielversie die voor een volledige sjabloon en tijdens runtime geldt, de Azure Resource Manager selecteert u de juiste versie van de resource.

API-profielen werken met hulpprogramma's die gebruikmaken van Azure Resource Manager, zoals PowerShell, Azure CLI, opgegeven in de SDK en Microsoft Visual Studio code. Hulpprogramma's en SDK's kunnen profielen gebruiken om te lezen welke versie van de modules en -bibliotheken om op te nemen bij het bouwen van een toepassing.

Bijvoorbeeld, als u PowerShell gebruiken voor het maken van een storage-account met de **Microsoft.Storage** resourceprovider, die ondersteuning biedt voor **api-versie** 2016-03-30 en een virtuele machine met de  **Microsoft.Compute** resourceprovider met **api-versie** 2015-12-01, moet u welke PowerShell-module ondersteunt 2016-03-30 opzoeken voor opslag en welke module biedt ondersteuning voor 2015-02-01 voor Compute, en vervolgens ze installeren. In plaats daarvan kunt u een profiel. Gebruik de cmdlet `Install-Profile <profilename>`, en de juiste versie van de modules door PowerShell wordt geladen.

Wanneer u de Python SDK gebruikt om een op basis van een Python-toepassing te bouwen, kunt u op dezelfde manier het-profiel opgeven. De SDK worden de juiste modules geladen voor de resourceproviders die u hebt opgegeven in het script.

Als ontwikkelaar kunt u zich richten op het schrijven van uw oplossing. In plaats van onderzoeken welke API-versies, de resourceprovider en de cloud werken samen, kunt u gebruikmaken van een profiel en weten dat uw code werkt op alle clouds die ondersteuning bieden voor dit profiel.

## <a name="api-profile-code-samples"></a>API-profiel-codevoorbeelden

Hier vindt u voorbeelden van code kunt u uw oplossing te integreren met de taal van uw voorkeur met Azure Stack met behulp van profielen. U kunt op dit moment richtlijnen en voorbeelden vinden voor de volgende talen:

- **.NET** kunt u de .NET API-profiel gebruiken om op te halen van de laatste, meest stabiele versie van elk resourcetype in een resource provider-pakket. Zie voor meer informatie, [gebruik API-versieprofielen met .NET in Azure Stack](azure-stack-version-profiles-net.md).
- **PowerShell**  
U kunt de **AzureRM.Bootstrapper** module beschikbaar via de PowerShell Gallery om op te halen van de PowerShell-cmdlets die is vereist voor het werken met API-versieprofielen. Zie voor meer informatie, [gebruik API-versieprofielen voor PowerShell](azure-stack-version-profiles-powershell.md).
- **Azure-CLI**  
U kunt de configuratie van uw omgeving voor het gebruik van de Azure Stack specifieke API-versie profiel bijwerken. Zie voor meer informatie, [gebruik API-versieprofielen voor Azure CLI](azure-stack-version-profiles-azurecli2.md).
- **Go**  
In de Go SDK is een profiel een combinatie van verschillende resourcetypen met verschillende versies van verschillende services. Profielen zijn beschikbaar onder de profielen/pad, met de versie in de **jjjj-MM-DD** indeling. Zie voor meer informatie, [gebruik API-versieprofielen voor Go](azure-stack-version-profiles-go.md).
- **Ruby**  
De Ruby-SDK voor Azure Stack Resource Manager biedt hulpprogramma's waarmee u kunt bouwen en beheren van uw infrastructuur. Resourceproviders in de SDK zijn Reken-, virtuele netwerken, en met de Ruby-taal. Zie voor meer informatie, [gebruik API-versieprofielen met Ruby](azure-stack-version-profiles-ruby.md)
- **Python**  
De Python-SDK biedt ondersteuning voor API-versieprofielen om u te richten op verschillende platforms, zoals Azure Stack en globale Azure. U kunt profielen van de API gebruiken om oplossingen voor een hybride cloud te maken. Zie voor meer informatie, [gebruik API-versieprofielen met Python](azure-stack-version-profiles-python.md)

## <a name="next-steps"></a>Volgende stappen

* [PowerShell voor Azure Stack installeren](azure-stack-powershell-install.md)
* [PowerShell-omgeving van de Azure Stack-gebruiker configureren](azure-stack-powershell-configure-user.md)
* [Meer informatie over resource provider API-versies ondersteund door de profielen](azure-stack-profiles-azure-resource-manager-versions.md).
