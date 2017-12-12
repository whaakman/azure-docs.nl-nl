---
title: Belangrijke verschillen tussen Azure en Azure Stack wanneer u services gebruiken en bouwen van apps | Microsoft Docs
description: Wat u moet weten wanneer u services gebruiken of voor het bouwen van apps voor Azure-Stack.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: c81f551d-c13e-47d9-a5c2-eb1ea4806228
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 09/25/2017
ms.author: mabrigg
ms.openlocfilehash: 695824ef2537a97ea0530f2c33ad24d5cd9e20f8
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="key-considerations-using-services-or-building-apps-for-azure-stack"></a>Belangrijke overwegingen: met behulp van services of het ontwikkelen van apps voor Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Wanneer u services gebruiken of apps voor Azure-Stack maken, moet u begrijpen dat er verschillen tussen Azure-Stack en Azure zijn. Dit artikel bevat een overzicht van de belangrijke aandachtspunten voor wanneer u Azure-Stack als uw ontwikkelomgeving voor hybride cloud.

## <a name="overview"></a>Overzicht

Azure Stack is een hybride cloudplatform waarmee u Azure-services kunt gebruiken vanuit het datacenter van uw bedrijf of serviceprovider. Als een ontwikkelaar, kunt u apps die worden uitgevoerd op Azure-Stack. Vervolgens kunt u deze apps implementeren naar Azure-Stack naar Azure, of kunt u echt hybride-apps die gebruikmaken van de verbinding tussen een Azure-Stack cloud en Azure.

Uw Azure-Stack-operator kunt u weten welke services beschikbaar zijn voor u om te gebruiken en ondersteuning krijgen. Ze bieden deze services via hun aangepaste schema's en aanbiedingen.

De Azure technische inhoud wordt ervan uitgegaan dat apps worden ontwikkeld voor een Azure-service in plaats van Azure-Stack. Wanneer u bouwen en implementeren van apps op Azure-Stack, moet u enkele belangrijke verschillen, zoals begrijpen:

* Azure-Stack biedt een subset van de services en functies die beschikbaar in Azure zijn.
* Uw bedrijf of service provider kunt kiezen welke services die ze willen aanbieden. Dit omvat aangepaste services of toepassingen. Ze kunnen hun eigen aangepaste documentatie aanbieden.
* Moet u de juiste Azure Stack-specifieke eindpunten (bijvoorbeeld de URL's van de portal en het Azure Resource Manager-eindpunt).
* U moet PowerShell en API-versies die worden ondersteund door Azure-Stack gebruiken. Dit zorgt ervoor dat uw apps in Azure-Stack en Azure werkt.

## <a name="cheat-sheet-high-level-differences"></a>Blad cheats: belangrijke verschillen

De volgende tabel beschrijft de belangrijke verschillen tussen Azure-Stack en Azure. Houd de volgende punten wanneer u voor Azure-Stack ontwikkelen of services van Azure-Stack gebruiken.

| Onderwerp | Azure (global) | Azure Stack |
| -------- | ------------- | ----------|
| Wie werkt het? | Microsoft | Uw organisatie of service provider.|
| Wie kan u contact opnemen voor ondersteuning? | Microsoft | Een geïntegreerd systeem contact op met uw Azure-Stack-operator (bij uw organisatie of service provider) voor ondersteuning.<br><br>Voor ondersteuning van Azure Stack Development Kit, gaat u naar de [Microsoft forums](https://social.msdn.microsoft.com/Forums/home?forum=azurestack). Omdat de development kit een evaluatieomgeving is, is geen officiële ondersteuning wordt aangeboden via Microsoft Customer ondersteunen Services (CSS).
| Beschikbare services | Overzicht van de [Azure producten](https://azure.microsoft.com/services/?b=17.04b). Beschikbare services verschillen per Azure-regio. | Azure-Stack ondersteunt een subset van de Azure-services. Werkelijke services variëren, afhankelijk van uw organisatie of service provider kiest te bieden.
| Azure Resource Manager-eindpunt * | https://management.azure.com | Gebruik het eindpunt dat uw Azure-Stack-operator opgegeven voor een systeem Azure Stack geïntegreerd.<br><br>Gebruik voor de development kit: https://management.local.azurestack.external
| Portal-URL * | [https://Portal.Azure.com](https://portal.azure.com) | Ga naar de URL die uw Azure-Stack-operator opgegeven voor een systeem Azure Stack geïntegreerd.<br><br>Gebruik voor de development kit: https://portal.local.azurestack.external
| Regio | U kunt selecteren welke regio die u implementeren wilt op. | Gebruik voor een Azure-Stack geïntegreerd, de regio die beschikbaar is op uw systeem.<br><br>Voor de development kit worden regio altijd **lokale**.
| Resourcegroepen | Een resourcegroep kan regio's omspannen. | Er is slechts één regio voor zowel geïntegreerde systemen en de development kit.
|Ondersteunde naamruimten, resourcetypen en API-versies | De meest recente (of eerdere versies die nog niet zijn afgeschaft). | Azure-Stack ondersteunt specifieke versies. Zie de sectie 'Vereisten versie' van dit artikel.
| | |

* Als u een Azure-Stack-operator, Zie [de Beheerdersportal](../azure-stack-manage-portals.md) en [basisprincipes van beheer](../azure-stack-manage-basics.md) voor meer informatie.

## <a name="helpful-tools-and-best-practices"></a>Nuttige hulpmiddelen en best practices
 
 Microsoft biedt verschillende hulpprogramma's en richtlijnen die u helpt ontwikkelen voor Azure-Stack.

| Aanbeveling | Verwijzingen | 
| -------- | ------------- | 
| Installeer de juiste hulpprogramma's op uw werkstation ontwikkelaars. | - [Installeer PowerShell](azure-stack-powershell-install.md)<br>- [Hulpprogramma's downloaden](azure-stack-powershell-download.md)<br>- [PowerShell configureren](azure-stack-powershell-configure-user.md)<br>- [Installeer Visual Studio](azure-stack-install-visual-studio.md) 
| Informatie over het volgende bekijken:<br>-Overwegingen met betrekking tot azure Resource Manager-sjabloon<br>-Het zoeken van Quick Start-sjablonen<br>-Een beleidsmodule gebruiken voor het gebruik van Azure te ontwikkelen voor Azure-Stack | [Ontwikkelen voor Azure Stack](azure-stack-developer.md) | 
| Controleer en volg de aanbevolen procedures voor sjablonen. | [Resource Manager Quick Start-sjablonen](https://github.com/Azure/azure-quickstart-templates/blob/master/1-CONTRIBUTION-GUIDE/best-practices.md#best-practices)
| | |

## <a name="version-requirements"></a>Versievereisten

Azure-Stack ondersteunt specifieke versies van Azure PowerShell en Azure service API's. Ondersteunde versies moet u gebruiken om ervoor te zorgen dat uw app voor beide Azure-Stack en naar Azure implementeren kunt.

Om ervoor te zorgen dat u een juiste versie van Azure PowerShell, gebruikt u [API-versie profielen](azure-stack-version-profiles.md). Om te bepalen het meest recente versie profiel voor API die u kunt gebruiken, moet u weten dat build van Azure-Stack u. U kunt deze informatie ophalen van de beheerder van uw Azure-Stack.

>[!NOTE]
 Als u de Azure-Stack Development Kit en u beheerderstoegang hebben, Zie de sectie 'De huidige versie niet vaststellen' [updates beheren](https://docs.microsoft.com/azure/azure-stack/azure-stack-updates#determine-the-current-version) om te bepalen van de Azure-Stack-build.

Voor andere API's, voer de volgende PowerShell-opdracht om uit te voeren van de naamruimten, resourcetypen en API-versies die worden ondersteund in uw Azure-Stack-abonnement. Zoals u ziet mogelijk nog steeds verschillen op het eigenschapniveau van een. (Voor deze opdracht te laten werken, moet er al [geïnstalleerd](azure-stack-powershell-install.md) en [geconfigureerd](azure-stack-powershell-configure-user.md) PowerShell voor een Azure-Stack-omgeving. U moet ook een abonnement hebt met een aanbieding voor Azure-Stack.)

 ```powershell
Get-AzureRmResourceProvider | Select ProviderNamespace -Expand ResourceTypes | Select * -Expand ApiVersions | `
Select ProviderNamespace, ResourceTypeName, @{Name="ApiVersion"; Expression={$_}} 
```

Voorbeeld van uitvoer (afgekapt): ![voorbeeld van uitvoer van de opdracht Get-AzureRmResourceProvider](media/azure-stack-considerations/image1.png)
 
## <a name="next-steps"></a>Volgende stappen

Zie voor meer gedetailleerde informatie over de verschillen op het niveau van een service:

* [Overwegingen voor virtuele Machines in Azure Stack](azure-stack-vm-considerations.md)
* [Overwegingen voor opslag in Azure-Stack](azure-stack-acs-differences.md)
* [Overwegingen voor het Azure-Stack-netwerken](azure-stack-network-differences.md)
