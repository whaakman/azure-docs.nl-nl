---
title: Over het implementeren van Windows 10 op Azure met Multitenant Hosting-rechten
description: Meer informatie over het optimaliseren van uw Windows Software Assurance benefits om on-premises licenties naar Azure
services: virtual-machines-windows
documentationcenter: ''
author: xujing
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 1/24/2018
ms.author: xujing
ms.openlocfilehash: 7f43528c55cd22c2649ca0f1208da6f41695b98e
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57569974"
---
# <a name="how-to-deploy-windows-10-on-azure-with-multitenant-hosting-rights"></a>Over het implementeren van Windows 10 op Azure met Multitenant Hosting-rechten 
Voor klanten met Windows 10 Enterprise E3/E5 per gebruiker of Windows virtuele bureaublad toegang per gebruiker (abonnement gebruikerslicenties of Invoegtoepassingslicenties gebruiker abonnement) kunt u uw Windows 10-licenties naar de cloud brengen met Multitenant Hosting rechten voor Windows 10 en Windows 10 Virtual Machines uitvoeren in Azure zonder te hoeven betalen voor een andere licentie. Zie voor meer informatie, [Multitenant die als host fungeert voor Windows 10](https://www.microsoft.com/en-us/CloudandHosting/licensing_sca.aspx).

> [!NOTE]
> Dit artikel laat u de licentievoordeel voor Windows 10 Pro Desktop-installatiekopieën implementeren op Azure Marketplace.
> - Raadpleeg voor Windows 7, 8.1, 10 Enterprise (x64) installatiekopieën op Azure Marketplace voor MSDN-abonnementen, [Windows-client in Azure voor ontwikkelings-/ Testscenario's](client-images.md)
> - Raadpleeg voor Windows Server-licenties van voordelen, [Azure Hybrid voordelen gebruiken voor Windows Server-installatiekopieën](hybrid-use-benefit-licensing.md).
>

## <a name="deploying-windows-10-image-from-azure-marketplace"></a>Implementatie van Windows 10-installatiekopie van Azure Marketplace 
Voor implementaties van Powershell, CLI en Azure Resource Manager-sjabloon, kunt u de installatiekopie van Windows 10 vinden met de volgende publishername, aanbieding, sku.

| OS  |      PublisherName      |  Aanbieding | Sku |
|:----------|:-------------:|:------|:------|
| Windows 10 Pro    | MicrosoftWindowsDesktop | Windows-10  | RS2-Pro   |
| Windows 10 Pro N  | MicrosoftWindowsDesktop | Windows-10  | RS2-ProN  |
| Windows 10 Pro    | MicrosoftWindowsDesktop | Windows-10  | RS3-Pro   |
| Windows 10 Pro N  | MicrosoftWindowsDesktop | Windows-10  | RS3-ProN  |

## <a name="uploading-windows-10-vhd-to-azure"></a>Uploaden van Windows 10-VHD voor Azure
Als u een gegeneraliseerde VHD met Windows 10 uploadt, Let op dat Windows 10 heeft geen ingebouwde administrator-account is standaard ingeschakeld. Om in te schakelen het ingebouwde administrator-account, moet u de volgende opdracht als onderdeel van de aangepaste scriptextensie opnemen.

```powershell
Net user <username> /active:yes
```

De volgende powershell-codefragment is het markeren van alle beheerdersaccounts als actief is, met inbegrip van de ingebouwde administrator. In dit voorbeeld is handig als de ingebouwde administrator-gebruikersnaam onbekend is.
```powershell
$adminAccount = Get-WmiObject Win32_UserAccount -filter "LocalAccount=True" | ? {$_.SID -Like "S-1-5-21-*-500"}
if($adminAccount.Disabled)
{
    $adminAccount.Disabled = $false
    $adminAccount.Put()
}
```
Voor meer informatie: 
* [Over het VHD uploaden naar Azure](upload-generalized-managed.md)
* [Hoe bereid ik een Windows-VHD te uploaden naar Azure](prepare-for-upload-vhd-image.md)


## <a name="deploying-windows-10-with-multitenant-hosting-rights"></a>Implementatie van Windows 10 met Multitenant Hosting-rechten
Zorg ervoor dat u hebt [geïnstalleerd en geconfigureerd, de nieuwste Azure PowerShell](/powershell/azure/overview). Als u uw VHD hebt voorbereid, de VHD uploaden naar uw Azure Storage-account met de `Add-AzVhd` cmdlet als volgt te werk:

```powershell
Add-AzVhd -ResourceGroupName "myResourceGroup" -LocalFilePath "C:\Path\To\myvhd.vhd" `
    -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd"
```


**Implementeren met behulp van Azure Resource Manager-sjabloonimplementatie** binnen uw Resource Manager-sjablonen, een extra parameter voor `licenseType` kan worden opgegeven. U kunt meer lezen over [Azure Resource Manager-sjablonen](../../resource-group-authoring-templates.md). Zodra u uw VHD die is geüpload naar Azure hebt, bewerkt u Resource Manager-sjabloon voor het licentietype opnemen als onderdeel van de compute-provider en de sjabloon als normale implementeren:
```json
"properties": {
    "licenseType": "Windows_Client",
    "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
    }
```

**Implementeren via PowerShell** bij het implementeren van uw Windows Server-machine via PowerShell, hebt u een extra parameter voor `-LicenseType`. Zodra u uw VHD die is geüpload naar Azure hebt, maakt u een virtuele machine met `New-AzVM` en geef het type van de licentieverlening als volgt:
```powershell
New-AzVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Client"
```

## <a name="verify-your-vm-is-utilizing-the-licensing-benefit"></a>Controleer of dat de virtuele machine met behulp van de licentievoordeel
Nadat u uw virtuele machine hebt geïmplementeerd via de PowerShell- of Resource Manager-implementatiemethode, Controleer of het licentietype met `Get-AzVM` als volgt:
```powershell
Get-AzVM -ResourceGroup "myResourceGroup" -Name "myVM"
```

De uitvoer is vergelijkbaar met het volgende voorbeeld voor Windows 10 met het type van de juiste licentie:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Client
```

Dit anders uitvoer met de volgende virtuele machine geïmplementeerd zonder Azure Hybrid Use Benefit licentiëring, zoals een virtuele machine rechtstreeks vanuit de Azure-galerie geïmplementeerd:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              :
```

## <a name="additional-information-about-joining-azure-ad"></a>Meer informatie over deelname aan Azure AD
>[!NOTE]
>Alle Windows-VM's met ingebouwde administrator-account kan niet worden gebruikt voor deelname aan AAD ingericht. Bijvoorbeeld, *instellingen > Account > toegang tot werk of School > + Connect* werkt niet. U moet maken en meld u aan als een tweede administrator-account worden gekoppeld aan Azure AD handmatig. U kunt ook configureren met Azure AD met behulp van een inrichtingspakket, gebruik de koppeling is de *Vervolgstappen* sectie voor meer informatie.
>
>

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [VDA configureren voor Windows 10](https://docs.microsoft.com/windows/deployment/vda-subscription-activation)
- Meer informatie over [Multitenant die als host fungeert voor Windows 10](https://www.microsoft.com/en-us/CloudandHosting/licensing_sca.aspx)


