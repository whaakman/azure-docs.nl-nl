---
title: Het implementeren van Windows 10 op Azure met Multitenant-rechten die als host fungeert
description: Meer informatie over het maximaliseren van uw Windows Software Assurance voordelen voor het maken van lokale licenties naar Azure
services: virtual-machines-windows
documentationcenter: 
author: xujing
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 1/24/2018
ms.author: xujing
ms.openlocfilehash: 6ad3b294e1d53d03f6ceb61048c8f657d8b471c0
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="how-to-deploy-windows-10-on-azure-with-multitenant-hosting-rights"></a>Het implementeren van Windows 10 op Azure met Multitenant-rechten die als host fungeert 
Voor klanten met Windows 10 Enterprise E3/E5 per gebruiker of Windows virtuele bureaublad toegang per gebruiker (abonnement gebruikerslicenties of abonnement voor extra gebruikerslicenties) kunt u uw Windows 10-licenties in de cloud met Multitenant Hosting-rechten voor Windows 10 en Windows 10 virtuele Machines in Azure uitvoeren zonder te betalen voor een andere licentie. Zie voor meer informatie [Multitenant die als host fungeert voor Windows 10](https://www.microsoft.com/en-us/CloudandHosting/licensing_sca.aspx).

> [!NOTE]
> Dit artikel ziet u de licentiegegevens voordeel voor installatiekopieën van Windows 10 Pro Desktop implementeren op Azure Marketplace.
> - Raadpleeg voor Windows 7, 8.1, 10 Enterprise (x64) afbeeldingen op Azure Marketplace voor MSDN-abonnement [Windows-client in Azure voor scenario's ontwikkelen en testen](client-images.md)
> - Raadpleeg voor Windows Server-licentieverlening voordelen [hybride Azure gebruiken voordelen voor installatiekopieën van Windows Server](hybrid-use-benefit-licensing.md).
>

## <a name="deploying-windows-10-image-from-azure-marketplace"></a>Windows 10-installatiekopie uit Azure Marketplace implementeren 
Voor implementaties van Powershell, CLI en Azure Resource Manager-sjabloon, kan de installatiekopie van het Windows 10 worden gevonden met de volgende publishername, aanbieding, sku.

| OS  |      PublisherName      |  Aanbieding | Sku |
|:----------|:-------------:|:------|:------|
| Windows 10 Pro    | MicrosoftWindowsDesktop | Windows-10  | RS2-Pro   |
| Windows 10 Pro N  | MicrosoftWindowsDesktop | Windows-10  | RS2-ProN  |
| Windows 10 Pro    | MicrosoftWindowsDesktop | Windows-10  | RS3-Pro   |
| Windows 10 Pro N  | MicrosoftWindowsDesktop | Windows-10  | RS3-ProN  |

## <a name="uploading-windows-10-vhd-to-azure"></a>Uploaden van Windows 10 VHD naar Azure
Als u een gegeneraliseerde Windows 10-VHD uploaden wilt, houd rekening met dat Windows 10 heeft geen ingebouwde administrator-account is standaard ingeschakeld. Bevatten de volgende opdracht als onderdeel van de aangepaste scriptextensie zodat de ingebouwde administrator-account.

```powershell
Net user <username> /active:yes
```

De volgende powershell-codefragment is markeren alle administrator-accounts als actief, met inbegrip van de ingebouwde administrator. In dit voorbeeld is handig als de ingebouwde administrator-gebruikersnaam onbekend is.
```powershell
$adminAccount = Get-WmiObject Win32_UserAccount -filter "LocalAccount=True" | ? {$_.SID -Like "S-1-5-21-*-500"}
if($adminAccount.Disabled)
{
    $adminAccount.Disabled = $false
    $adminAccount.Put()
}
```
Voor meer informatie: 
* [Het VHD uploaden naar Azure](upload-generalized-managed.md)
* [Het voorbereiden van een Windows-VHD te uploaden naar Azure](prepare-for-upload-vhd-image.md)


## <a name="deploying-windows-10-with-multitenant-hosting-rights"></a>Implementatie van Windows 10 met Multitenant Hosting rechten
Zorg ervoor dat u hebt [geïnstalleerd en geconfigureerd, de meest recente Azure PowerShell](/powershell/azure/overview). Als u uw VHD hebt voorbereid, de harde schijf geüpload naar uw Azure Storage-account met de `Add-AzureRmVhd` cmdlet als volgt:

```powershell
Add-AzureRmVhd -ResourceGroupName "myResourceGroup" -LocalFilePath "C:\Path\To\myvhd.vhd" `
    -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd"
```


**Implementeren met behulp van de implementatie van Azure Resource Manager-sjabloon** binnen de Resource Manager-sjablonen, een extra parameter voor `licenseType` kan worden opgegeven. U kunt meer lezen over [Azure Resource Manager-sjablonen ontwerpen](../../resource-group-authoring-templates.md). Zodra u uw VHD die is geüpload naar Azure hebt, bewerkt u Resource Manager-sjabloon voor het licentietype opnemen als onderdeel van de compute-provider en de sjabloon als normale implementeren:
```json
"properties": {  
   "licenseType": "Windows_Client",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   }
```

**Implementeren via PowerShell** wanneer u uw Windows Server-VM via PowerShell implementeert, hebt u een extra parameter voor `-LicenseType`. Zodra u uw VHD die is geüpload naar Azure hebt, maakt u een VM met `New-AzureRmVM` en geef het type van de licentieverlening als volgt:
```powershell
New-AzureRmVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Client"
```

## <a name="verify-your-vm-is-utilizing-the-licensing-benefit"></a>Controleer of uw virtuele machine wordt gebruikt door de licentiegegevens benefit
Nadat u uw virtuele machine hebt geïmplementeerd via de PowerShell- of Resource Manager-implementatiemethode, Controleer of het licentietype met `Get-AzureRmVM` als volgt:
```powershell
Get-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
```

De uitvoer is vergelijkbaar met het volgende voorbeeld voor Windows 10 met de juiste licentie-type:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Client
```

Dit is anders dan uitvoer met de volgende virtuele machine geïmplementeerd zonder Azure hybride gebruik voordeel-licentieverlening, zoals een virtuele machine rechtstreeks vanuit de galerie van Azure is geïmplementeerd:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              :
```

## <a name="additional-information-about-joining-azure-ad"></a>Meer informatie over deelname aan Azure AD
>[!NOTE]
>Azure levert alle Windows-VM's met de ingebouwde administrator-account kan niet worden gebruikt om toe te voegen AAD. Bijvoorbeeld: *instellingen > Account > toegang werk of School > + Connect* werkt niet. U moet maken en meld u aan als een tweede administrator-account lid worden van Azure AD-mannually. U kunt ook configureren met Azure AD dat gebruikmaakt van een inrichtingspakket, gebruik de koppeling is de *Vervolgstappen* sectie voor meer informatie.
>
>

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [VDA configureren voor Windows 10](https://docs.microsoft.com/windows/deployment/vda-subscription-activation)
- Meer informatie over [Multitenant die als host fungeert voor Windows 10](https://www.microsoft.com/en-us/CloudandHosting/licensing_sca.aspx)


