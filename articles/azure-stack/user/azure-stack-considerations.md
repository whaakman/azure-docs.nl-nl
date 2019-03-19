---
title: Informatie over de belangrijkste verschillen tussen Azure en Azure Stack bij gebruik van services en het bouwen van apps | Microsoft Docs
description: Wat u moet weten voor het gebruik van services of apps bouwen voor Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: c81f551d-c13e-47d9-a5c2-eb1ea4806228
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 12/27/2018
ms.author: sethm
ms.lastreviewed: 12/27/2018
ms.openlocfilehash: 158f22ec2ab07bfc2f893d4b0c55d862e0b19de0
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58106658"
---
# <a name="key-considerations-using-services-or-building-apps-for-azure-stack"></a>Belangrijke overwegingen: Met behulp van services of het bouwen van apps voor Azure Stack

Voordat u gebruik van services of apps voor Azure Stack bouwen, moet u weten wat de verschillen tussen Azure Stack en Azure. In dit artikel geeft belangrijke aandachtspunten voor wanneer u Azure Stack als het hulpprogramma voor het ontwikkelen van uw hybride-cloudomgeving.

## <a name="overview"></a>Overzicht

Azure Stack is een hybride cloudplatform waarmee u Azure-services kunt gebruiken vanuit het datacenter van uw bedrijf of serviceprovider. U kunt een app bouwen in Azure Stack en vervolgens te implementeren naar Azure Stack, naar Azure of naar uw Azure hybrid cloud.

Uw Azure Stack-operators laat u weten welke services zijn beschikbaar voor u om te gebruiken en ondersteuning krijgen. Ze bieden deze services via hun aangepaste plannen en aanbiedingen.

De Azure technische inhoud wordt ervan uitgegaan dat apps worden ontwikkeld voor een Azure-service in plaats van Azure Stack. Wanneer u apps bouwen en met Azure Stack implementeren, moet u enkele belangrijke verschillen, zoals begrijpen:

* Azure Stack biedt een **subset** van de services en functies die beschikbaar in Azure zijn.
* Uw bedrijf of service provider kunt kiezen welke services ze willen aanbieden. De beschikbare opties advies inwinnen bij aangepaste services of toepassingen. Ze kunnen hun eigen aangepaste documentatie bieden.
* Moet u de juiste Azure Stack-specifieke eindpunten (bijvoorbeeld de URL's voor het adres van de portal en het eindpunt van de Azure Resource Manager).
* U moet PowerShell en API-versies die worden ondersteund door Azure Stack. Met behulp van ondersteunde versies, zorgt u ervoor dat uw apps in zowel Azure als Azure Stack werken.

## <a name="cheat-sheet-high-level-differences"></a>Overzichtskaart: Belangrijke verschillen

De volgende tabel beschrijft de belangrijke verschillen tussen Azure Stack en Azure. Deze verschillen in waarmee u rekening moet houden bij het ontwikkelen voor Azure Stack of Azure Stack-services gebruiken.

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

| Onderwerp | Azure (wereldwijd) | Azure Stack |
| -------- | ------------- | ----------|
| Wie werkt het? | Microsoft | Uw organisatie of de service-provider.|
| Wie moet u contact opnemen voor ondersteuning? | Microsoft | Neem contact op met uw Azure Stack-operators (in uw organisatie of service provider) voor ondersteuning voor een geïntegreerd systeem.<br><br>Voor ondersteuning voor Azure Stack Development Kit, gaat u naar de [Microsoft forums](https://social.msdn.microsoft.com/Forums/home?forum=azurestack). Omdat de development kit een evaluatieomgeving is, is er geen officiële ondersteuning aangeboden via Microsoft de klant ondersteuning klantenondersteuning (CSS).
| Beschikbare services | Overzicht van de [Azure-producten](https://azure.microsoft.com/services/?b=17.04b). Beschikbare services verschillen per Azure-regio. | Azure Stack biedt ondersteuning voor een subset van de Azure-services. Werkelijke services variëren op basis van uw organisatie of service provider kiest te bieden.
| Azure Resource Manager-eindpunt * | https://management.azure.com | Gebruik het eindpunt dat uw Azure Stack-operator opgegeven voor een geïntegreerde Azure Stack-systeem.<br><br>Voor de development kit, gebruikt: https://management.local.azurestack.external
| Portal-URL * | [https://portal.azure.com](https://portal.azure.com) | Ga naar de URL die uw Azure Stack-operator opgegeven voor een geïntegreerde Azure Stack-systeem.<br><br>Voor de development kit, gebruikt: https://portal.local.azurestack.external
| Regio | U kunt selecteren welke regio u implementeren wilt op. | Voor een geïntegreerde Azure Stack-systeem, gebruikt u de regio die beschikbaar is op uw systeem.<br><br>Voor de development kit, regio altijd worden **lokale**.
| Resourcegroepen | Een resourcegroep kan regio's omvatten. | Voor zowel geïntegreerde systemen en de development kit is er slechts één regio.
|Ondersteunde naamruimten, resourcetypen en API-versies | De meest recente (of eerdere versies die nog niet zijn afgeschaft). | Azure Stack biedt ondersteuning voor specifieke versies. Zie de [versievereisten](#version-requirements) sectie van dit artikel.
| | |

* Als u Azure Stack-operators, Zie [met behulp van de beheerdersportal](../azure-stack-manage-portals.md) en [basisprincipes van beheer](../azure-stack-manage-basics.md) voor meer informatie.

## <a name="helpful-tools-and-best-practices"></a>Nuttige hulpmiddelen en best practices
 
 Microsoft biedt hulpprogramma's en richtlijnen die u helpen ontwikkelen voor Azure Stack.

| Aanbeveling | Verwijzingen |
| -------- | ------------- |
| Installeer de juiste hulpprogramma's op uw ontwikkelwerkstation. | - [PowerShell installeren](azure-stack-powershell-install.md)<br>- [Hulpprogramma's downloaden](azure-stack-powershell-download.md)<br>- [PowerShell configureren](azure-stack-powershell-configure-user.md)<br>- [Visual Studio installeren](azure-stack-install-visual-studio.md) 
| Lees de informatie over de volgende items:<br>-Overwegingen met betrekking tot azure Resource Manager-sjabloon<br>-Het zoeken naar QuickStart-sjablonen<br>-Met een beleidsmodule kunt u Azure gebruiken om te ontwikkelen voor Azure Stack | [Ontwikkelen voor Azure Stack](azure-stack-developer.md) | 
| Bekijk en volg de aanbevolen procedures voor sjablonen. | [Resource Manager Quickstart-sjablonen](https://github.com/Azure/azure-quickstart-templates/blob/master/1-CONTRIBUTION-GUIDE/best-practices.md#best-practices)
| | |

## <a name="version-requirements"></a>Vereisten voor de versie

Azure Stack biedt ondersteuning voor specifieke versies van Azure PowerShell en Azure service-API's. Ondersteunde versies gebruiken om ervoor te zorgen dat uw app naar zowel Azure Stack en naar Azure implementeren kunt.

Om ervoor te zorgen dat u een juiste versie van Azure PowerShell, gebruikt u [API-versieprofielen](azure-stack-version-profiles.md). Ontdek de build van Azure Stack u om te bepalen het meest recente versie profiel voor API die u kunt gebruiken. U kunt deze informatie krijgen van uw Azure Stack-beheerder.

> [!NOTE]
>  Als u de Azure Stack Development Kit en u beheerdersrechten hebt, raadpleegt u de [bepalen van de huidige versie](../azure-stack-updates.md#determine-the-current-version) sectie om te bepalen van de Azure Stack-build.

Voor andere API's, voer de volgende PowerShell-opdracht om uit te voeren van de naamruimten, resourcetypen en API-versies die worden ondersteund in uw Azure Stack-abonnement. Zoals u ziet mogelijk nog steeds verschillen op het eigenschapniveau van een. Voor deze opdracht te laten werken, moet u hebt al [geïnstalleerd](azure-stack-powershell-install.md) en [geconfigureerd](azure-stack-powershell-configure-user.md) PowerShell voor Azure Stack-omgeving. U moet ook een abonnement op een Azure Stack-aanbieding hebben.

```powershell
Get-AzureRmResourceProvider | Select ProviderNamespace -Expand ResourceTypes | Select * -Expand ApiVersions | `
Select ProviderNamespace, ResourceTypeName, @{Name="ApiVersion"; Expression={$_}} 
```

Voorbeeld van uitvoer (afgekapt): ![Voorbeeld van uitvoer van de opdracht Get-AzureRmResourceProvider](media/azure-stack-considerations/image1.png)
 
## <a name="next-steps"></a>Volgende stappen

Zie voor meer gedetailleerde informatie over de verschillen op het serviceniveau van een:

* [Overwegingen voor virtuele Machines in Azure Stack](azure-stack-vm-considerations.md)
* [Overwegingen voor opslag in Azure Stack](azure-stack-acs-differences.md)
* [Aandachtspunten voor Azure Stack-netwerken](azure-stack-network-differences.md)
