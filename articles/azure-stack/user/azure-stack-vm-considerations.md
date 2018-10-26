---
title: Verschillen en overwegingen voor virtuele machines in Azure Stack | Microsoft Docs
description: Meer informatie over de verschillen en overwegingen bij het werken met virtuele machines in Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 6613946D-114C-441A-9F74-38E35DF0A7D7
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.author: mabrigg
ms.openlocfilehash: cb02ecb06728f5f36a0d51a3ec22cc8ba5cb44e7
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50094752"
---
# <a name="considerations-for-using-virtual-machines-in-azure-stack"></a>Overwegingen voor het gebruik van virtuele machines in Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Virtuele machines van Azure Stack bieden on-demand en schaalbare computing-resources. Voordat u virtuele machines (VM's) implementeert, moet u weten wat de verschillen tussen de virtuele machine-functies die beschikbaar zijn in Azure Stack en Microsoft Azure. In dit artikel worden deze verschillen beschreven en belangrijke aandachtspunten voor het plannen van implementaties van virtuele machines identificeert. Zie voor meer informatie over belangrijke verschillen tussen Azure Stack en Azure, de [belangrijke overwegingen met betrekking tot](azure-stack-considerations.md) artikel.

## <a name="cheat-sheet-virtual-machine-differences"></a>Overzichtskaart: verschillen van de virtuele machine

| Functie | Azure (wereldwijd) | Azure Stack |
| --- | --- | --- |
| Installatiekopieën van virtuele machines | De Azure Marketplace bevat-installatiekopieën die u gebruiken kunt om te maken van een virtuele machine. Zie de [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1) pagina om de lijst met installatiekopieën die beschikbaar in de Azure Marketplace zijn weer te geven. | Standaard, er zijn niet alle installatiekopieën beschikbaar in de Azure Stack marketplace. De beheerder van de Azure Stack-cloud moet publiceren of afbeeldingen downloaden naar de Azure Stack marketplace voordat gebruikers ze kunnen gebruiken. |
| Grootten van virtuele machines | Azure ondersteunt een groot aantal groottes voor virtuele machines. Raadpleeg voor meer informatie over de beschikbare grootten en opties, de [grootte van virtuele machines van Windows](../../virtual-machines/virtual-machines-windows-sizes.md) en [Linux VM-grootten](../../virtual-machines/linux/sizes.md) onderwerpen. | Azure Stack biedt ondersteuning voor een subset van de VM-grootten die beschikbaar in Azure zijn. Als u wilt weergeven in de lijst met ondersteunde grootten, verwijzen naar de [grootten van virtuele machines](#virtual-machine-sizes) sectie van dit artikel. |
| Quota's voor virtuele machine | [De quotalimieten](../../azure-subscription-service-limits.md#service-specific-limits) zijn ingesteld door Microsoft | De beheerder van de Azure Stack-cloud moet quota toewijzen voordat ze virtuele machines voor hun gebruikers bieden. |
| Extensies van de virtuele machine |Azure ondersteunt een groot aantal extensies voor virtuele machines. Raadpleeg voor meer informatie over de beschikbare uitbreidingen, de [extensies voor virtuele machines en functies](../../virtual-machines/windows/extensions-features.md) artikel.| Azure Stack ondersteunt een subset van de uitbreidingen die beschikbaar in Azure zijn en elk van de extensie voor specifieke versies hebben. De beheerder van de Azure Stack-cloud kunt kiezen welke extensies moet worden beschikbaar gesteld aan voor hun gebruikers. Raadpleeg de lijst van ondersteunde extensies wilt weergeven, de [extensies voor virtuele machines](#virtual-machine-extensions) sectie van dit artikel. |
| Netwerk voor virtuele machines | Openbare IP-adressen toegewezen aan de tenant virtuele machine zijn via Internet toegankelijk.<br><br><br>Virtuele Machines van Azure heeft een vaste DNS-naam | Openbare IP-adressen toegewezen aan een tenant virtuele machine zijn binnen de omgeving van Azure Stack Development Kit alleen toegankelijk. Een gebruiker moet toegang hebben tot de Azure Stack Development Kit via [RDP](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop) of [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) verbinding maken met een virtuele machine die wordt gemaakt in Azure Stack.<br><br>Virtuele machines die worden gemaakt binnen een specifiek exemplaar van Azure Stack hebben een DNS-naam op basis van de waarde die is geconfigureerd door de beheerder van de cloud. |
| Virtuele-machineopslag | Ondersteunt [beheerde schijven.](../../virtual-machines/windows/managed-disks-overview.md) | Beheerde schijven worden ondersteund in Azure Stack met versie 1808 en hoger. |
| Prestaties van de virtuele machine-schijven | Afhankelijk van schijftype en grootte. | Afhankelijk van VM-grootte van virtuele machine die de schijven zijn gekoppeld te verwijzen naar de [grootten van virtuele machines worden ondersteund in Azure Stack](azure-stack-vm-sizes.md) artikel.
| API-versies | Azure heeft altijd de meest recente API-versies voor alle functies van de virtuele machine. | Azure Stack biedt ondersteuning voor specifieke Azure-services en specifieke API-versies voor deze services. Raadpleeg de lijst van ondersteunde API-versies wilt weergeven, de [API-versies](#api-versions) sectie van dit artikel. |
|Beschikbaarheidssets voor virtuele machines|Meerdere domeinen met fouten (2 of 3 per regio)<br>Meerdere updatedomeinen<br>Beheerde schijfondersteuning|Meerdere domeinen met fouten (2 of 3 per regio)<br>Meerdere updatedomeinen (maximaal 20)<br>Er is geen ondersteuning voor beheerde schijven|
|Virtuele-machineschaalsets|Automatisch schalen wordt ondersteund|Automatisch schalen niet ondersteund.<br>Meer exemplaren toevoegen aan een schaalset met behulp van de portal, Resource Manager-sjablonen of PowerShell.

## <a name="virtual-machine-sizes"></a>Grootten van virtuele machines

Azure Stack legt resourcebeperkingen om te voorkomen dat via het verbruik van resources (server lokale en serviceniveau.) De tenant-ervaring door deze limieten te verbeteren door te verminderen van de gevolgen van het resourceverbruik van andere tenants.

- Voor netwerken uitgaande verkeer van de virtuele machine zijn er limieten voor bandbreedte aanwezig. Limieten in Azure Stack zijn hetzelfde als de limieten in Azure.
- Storage-resources implementeert Azure Stack IOPS-limieten voor opslag om te voorkomen dat basic overmatig van resources door tenants voor toegang tot opslag.
- Voor virtuele machines met meerdere gekoppelde gegevensschijven is de maximale doorvoer van elke gegevensschijf 500 IOPS voor HDD's en 2300 IOP's voor SSD's.

De volgende tabel geeft een lijst van de virtuele machines die worden ondersteund in Azure Stack samen met hun configuratie:

| Type           | Grootte          | Bereik van de ondersteunde grootten |
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

Grootten van virtuele machines en de bijbehorende resource-hoeveelheden zijn consistent tussen de Azure Stack en Azure. Dit omvat de hoeveelheid geheugen, het aantal kernen en het aantal of de grootte van gegevensschijven die kunnen worden gemaakt. Prestaties van virtuele machines met dezelfde grootte is afhankelijk van de onderliggende kenmerken van een specifieke Azure Stack-omgeving.

## <a name="virtual-machine-extensions"></a>Extensies van de virtuele machine

 Azure Stack bevat een kleine set van extensies. Updates en de aanvullende extensies zijn beschikbaar via de Marketplace-syndicatie.

Gebruik de volgende PowerShell-script om de lijst met extensies voor virtuele machines die beschikbaar in uw Azure Stack-omgeving zijn:

```powershell
Get-AzureRmVmImagePublisher -Location local | `
  Get-AzureRmVMExtensionImageType | `
  Get-AzureRmVMExtensionImage | `
  Select Type, Version | `
  Format-Table -Property * -AutoSize
```

## <a name="api-versions"></a>API-versies

Functies van virtuele machines in Azure Stack ondersteunen de volgende API-versies:

![VM-resourcetypen](media/azure-stack-vm-considerations/vm-resoource-types.png)

U kunt de volgende PowerShell-script gebruiken om op te halen van de API-versies voor de virtuele machine-functies die beschikbaar in uw Azure Stack-omgeving zijn:

```powershell
Get-AzureRmResourceProvider | `
  Select ProviderNamespace -Expand ResourceTypes | `
  Select * -Expand ApiVersions | `
  Select ProviderNamespace, ResourceTypeName, @{Name="ApiVersion"; Expression={$_}} | `
  where-Object {$_.ProviderNamespace -like “Microsoft.compute”}
```

De lijst met ondersteunde resourcetypen en API-versies kan variëren als de cloud-operator uw Azure Stack-omgeving naar een nieuwere versie bijwerkt.

## <a name="windows-activation"></a>Windows-activering

Windows-producten moeten worden gebruikt in overeenstemming met de Product Use Rights en de licentievoorwaarden voor Microsoft. Maakt gebruik van Azure Stack [automatische activering van virtuele machine](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v%3dws.11)) (AVMA) voor het activeren van Windows Server virtuele machines (VM's).

- Azure Stack-host voor het activeren van Windows met AVMA-sleutels voor Windows Server 2016. Alle virtuele machines die worden uitgevoerd van Windows Server 2012 of later automatisch geactiveerd.
- Virtuele machines die bij het uitvoeren van Windows Server 2008 R2 worden niet automatisch geactiveerd en moet worden geactiveerd met behulp van [MAK-activering](https://technet.microsoft.com/library/ff793438.aspx). Voor het gebruik van MAK-activering, moet u uw eigen productcode opgeven.

Microsoft Azure maakt gebruik van KMS-activering te activeren van Windows-VM's. Als u een virtuele machine van Azure Stack naar Azure en ondervindt problemen activeren, Zie [oplossen Azure Windows VM activeringsproblemen](https://docs.microsoft.com/azure/virtual-machines/windows/troubleshoot-activation-problems). Als u meer informatie vindt u op de [problemen oplossen-Windows-activeringsfouten op Azure Virtual machines](https://blogs.msdn.microsoft.com/mast/2017/06/14/troubleshooting-windows-activation-failures-on-azure-vms/) Azure Support Team Blog-bericht.

## <a name="next-steps"></a>Volgende stappen

[Een Windows-machine maken met PowerShell in Azure Stack](azure-stack-quick-create-vm-windows-powershell.md)
