---
title: Verschillen en overwegingen voor virtuele machines in Azure Stack | Microsoft Docs
description: Meer informatie over de verschillen en overwegingen bij het werken met virtuele machines in Azure-Stack.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 6613946D-114C-441A-9F74-38E35DF0A7D7
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: brenduns
ms.openlocfilehash: 83a0b8ff040425ac30cff96936f2f639fd1b5643
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
---
# <a name="considerations-for-using-virtual-machines-in-azure-stack"></a>Overwegingen voor het gebruik van virtuele machines in Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Stack van virtuele machines van Azure bieden op aanvraag, schaalbare computerbronnen. Voordat u virtuele machines (VM's) implementeert, moet u het verschil tussen de functies van de virtuele machine beschikbaar zijn in Azure-Stack en Microsoft Azure. Dit artikel beschrijft deze verschillen en identificeert belangrijke aandachtspunten voor het plannen van implementaties van virtuele machines. Zie voor meer informatie over belangrijke verschillen tussen Azure-Stack en Azure, de [belangrijke overwegingen](azure-stack-considerations.md) artikel.

## <a name="cheat-sheet-virtual-machine-differences"></a>Blad cheats: verschillen van de virtuele machine

| Functie | Azure (global) | Azure Stack |
| --- | --- | --- |
| Installatiekopieën van virtuele machines | Azure Marketplace bevat afbeeldingen die u gebruiken kunt voor het maken van een virtuele machine. Zie de [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1) pagina om de lijst met afbeeldingen die beschikbaar in Azure Marketplace zijn weer te geven. | Standaard bevat niet alle installatiekopieën beschikbaar zijn in de Stack van Azure marketplace. De beheerder van de Azure-Stack cloud moet publiceren of installatiekopieën downloaden naar de Stack van Azure marketplace voordat gebruikers ze kunnen gebruiken. |
| Grootten van virtuele machines | Azure ondersteunt een groot aantal verschillende grootten voor virtuele machines. Raadpleeg voor meer informatie over de beschikbare grootten en opties voor de [Windows virtuele machines grootten](../../virtual-machines/virtual-machines-windows-sizes.md) en [grootten van virtuele machines van Linux](../../virtual-machines/linux/sizes.md) onderwerpen. | Azure-Stack ondersteunt een subset van de grootte van virtuele machines die beschikbaar in Azure zijn. Als u wilt weergeven in de lijst met ondersteunde grootten, verwijzen naar de [grootten van virtuele machines](#virtual-machine-sizes) sectie van dit artikel. |
| Quota's voor virtuele machine | [De quotalimieten voor](../../azure-subscription-service-limits.md#service-specific-limits) zijn ingesteld door Microsoft | De beheerder van de Azure-Stack cloud moet quota toewijzen voordat ze aan virtuele machines te aan hun gebruikers bieden. |
| Extensies van de virtuele machine |Azure biedt ondersteuning voor tal van uitbreidingen van de virtuele machine. Raadpleeg voor meer informatie over de beschikbare uitbreidingen, de [uitbreidingen van de virtuele machine en functies](../../virtual-machines/windows/extensions-features.md) artikel.| Azure-Stack ondersteunt een subset van de uitbreidingen die beschikbaar in Azure zijn en elk van de extensie specifieke versies hebben. De beheerder van de Azure-Stack cloud kunt kiezen welke uitbreidingen die beschikbaar voor gebruikers. Raadpleeg de lijst met ondersteunde extensies wilt weergeven, de [uitbreidingen van de virtuele machine](#virtual-machine-extensions) sectie van dit artikel. |
| Netwerk voor virtuele machines | Openbare IP-adressen toegewezen aan virtuele machines van tenants zijn via Internet toegankelijk.<br><br><br>Virtuele Machines in Azure heeft een vaste DNS-naam | Openbare IP-adressen toegewezen aan de virtuele machine van een tenant zijn binnen de omgeving van Azure Stack Development Kit alleen toegankelijk. Een gebruiker moet toegang hebben tot de Azure-Stack Development Kit via [RDP](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop) of [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) verbinding maken met een virtuele machine die wordt gemaakt in Azure-Stack.<br><br>Virtuele machines die zijn gemaakt binnen een specifiek exemplaar van de Azure-Stack is een DNS-naam op basis van de waarde die is geconfigureerd door de beheerder van de cloud. |
| Virtuele-machineopslag | Ondersteunt [schijven die worden beheerd.](../../virtual-machines/windows/managed-disks-overview.md) | Beheerde schijven zijn nog niet ondersteund in Azure-Stack. |
| API-versies | Azure heeft altijd de nieuwste API-versies voor alle functies van de virtuele machine. | Azure ondersteunt Stack specifieke Azure-services en de specifieke API-versies voor deze services. Raadpleeg de lijst met ondersteunde API-versies wilt weergeven, de [API-versies](#api-versions) sectie van dit artikel. |
|Virtuele machine beschikbaarheidssets|Meerdere domeinen met fouten (2 of 3 per regio)<br>Meerdere domeinen van de update<br>Ondersteuning voor schijven beheerd|Meerdere domeinen met fouten (2 of 3 per regio)<br>Meerdere domeinen van de update (maximaal 20)<br>Er is geen ondersteuning voor beheerde schijven|
|Virtuele-machineschaalsets|Automatisch geschaald ondersteund|Automatisch geschaald niet ondersteund.<br>Meer exemplaren toevoegen aan een schaal ingesteld met de portal, Resource Manager-sjablonen of PowerShell.

## <a name="virtual-machine-sizes"></a>Grootten van virtuele machines

Azure Stack legt limieten om te voorkomen dat via het verbruik van bronnen (server lokale en serviceniveau.) Deze limieten voor de tenant-ervaring te verbeteren doordat de gevolgen van het brongebruik van andere tenants.

- Voor netwerken uitgaande van de virtuele machine zijn er bandbreedte caps aanwezig. Hoofdletters in Azure-Stack zijn hetzelfde als de caps in Azure.
- Voor opslagbronnen implementeert Azure Stack IOPS opslaglimieten om te voorkomen dat basic overmatig van bronnen door tenants voor toegang tot opslag.
- Voor virtuele machines met meerdere schijven van de bijgesloten gegevens is de maximale doorvoer van elke gegevensschijf 500 IOPS aan voor HHDs en 2300 IOPS voor SSD's.

De volgende tabel bevat de virtuele machines die worden ondersteund op Azure-Stack samen met hun configuratie:

| Type           | Grootte          | Aantal ondersteunde formaten |
| ---------------| ------------- | ------------------------ |
|Algemeen doel |Basic A        |[A0 - A4](azure-stack-vm-sizes.md#basic-a)                   |
|Algemeen doel |Standard A     |[A0 - A7](azure-stack-vm-sizes.md#standard-a)              |
|Algemeen doel |D-serie       |[D1 - D4](azure-stack-vm-sizes.md#d-series)              |
|Algemeen doel |Dv2-serie     |[D1_v2 - D5_v2](azure-stack-vm-sizes.md#ds-series)        |
|Algemeen doel |DS-serie      |[DS1 - DS4](azure-stack-vm-sizes.md#dv2-series)            |
|Algemeen doel |DSv2-serie    |[DS1_v2 - DS5_v2](azure-stack-vm-sizes.md#dsv2-series)      |
|Geoptimaliseerd geheugen|D-serie       |[D11 - D14](azure-stack-vm-sizes.md#mo-d)            |
|Geoptimaliseerd geheugen|DS-serie      |[DS11 - DS14](azure-stack-vm-sizes.md#mo-ds)|
|Geoptimaliseerd geheugen|Dv2-serie     |[D11_v2 - DS14_v2](azure-stack-vm-sizes.md#mo-dv2)     |
|Geoptimaliseerd geheugen|DSv2-serie-  |[DS11_v2 - DS14_v2](azure-stack-vm-sizes.md#mo-dsv2)    |

Grootte van virtuele machines en hun bijbehorende resource hoeveelheden zijn consistent met de Azure-Stack en Azure. Dit omvat de hoeveelheid geheugen, het aantal kernen en het nummer of de grootte van gegevensschijven dat kan worden gemaakt. Prestaties van virtuele machines met dezelfde grootte is afhankelijk van de onderliggende kenmerken van een bepaalde Azure Stack-omgeving.

## <a name="virtual-machine-extensions"></a>Extensies van de virtuele machine

 Azure Stack bevat een kleine set van extensies. Updates en de aanvullende extensies zijn beschikbaar via de Marketplace-syndicatiefeed.

Gebruik de volgende PowerShell-script om de lijst met extensies van virtuele machine die beschikbaar in uw Azure-Stack-omgeving zijn:

```powershell
Get-AzureRmVmImagePublisher -Location local | `
  Get-AzureRmVMExtensionImageType | `
  Get-AzureRmVMExtensionImage | `
  Select Type, Version | `
  Format-Table -Property * -AutoSize
```

## <a name="api-versions"></a>API-versies

Functies van de virtuele machine in Azure-Stack ondersteunen de volgende API-versies:

![Brontypen VM](media/azure-stack-vm-considerations/vm-resoource-types.png)

Ophalen van de API-versies voor de functies van de virtuele machine die beschikbaar in uw Azure-Stack-omgeving zijn kunt u het volgende PowerShell-script:

```powershell
Get-AzureRmResourceProvider | `
  Select ProviderNamespace -Expand ResourceTypes | `
  Select * -Expand ApiVersions | `
  Select ProviderNamespace, ResourceTypeName, @{Name="ApiVersion"; Expression={$_}} | `
  where-Object {$_.ProviderNamespace -like “Microsoft.compute”}
```

De lijst met ondersteunde resourcetypen en API-versies kan verschillen als de operator cloud uw Azure-Stack-omgeving naar een nieuwere versie bijwerkt.

## <a name="windows-activation"></a>Windows-activering

Windows-producten moeten worden gebruikt in overeenstemming met de Product Use Rights en de licentievoorwaarden voor Microsoft. Maakt gebruik van Azure Stack [automatische activering van virtuele machine](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v%3dws.11)) (AVMA) voor het activeren van virtuele machines (VM's) voor Windows Server.

- Azure-Stack host Windows kan worden geactiveerd met AVMA-sleutels voor Windows Server 2016. Alle virtuele machines die bij het uitvoeren van Windows Server 2012 of later worden automatisch geactiveerd.
- Virtuele machines die bij het uitvoeren van Windows Server 2008 R2 niet automatisch worden geactiveerd en moet worden geactiveerd met behulp van [MAK-activering](https://technet.microsoft.com/library/ff793438.aspx).

Microsoft Azure maakt gebruik van KMS-activering voor het activeren van VM's van Windows. Als u een virtuele machine uit Azure Stack naar Azure en optreden activeren problemen verplaatst, Zie [oplossen Azure Windows virtuele machine activeringsproblemen](https://docs.microsoft.com/azure/virtual-machines/windows/troubleshoot-activation-problems). Als u meer informatie vindt u op de [probleemoplossing Windows activation-fouten op Azure Virtual machines](https://blogs.msdn.microsoft.com/mast/2017/06/14/troubleshooting-windows-activation-failures-on-azure-vms/) Azure Support-teamblog post.

## <a name="next-steps"></a>Volgende stappen

[Een virtuele Windows-machine maken met PowerShell in Azure-Stack](azure-stack-quick-create-vm-windows-powershell.md)