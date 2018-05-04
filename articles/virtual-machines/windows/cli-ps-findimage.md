---
title: Selecteer Windows VM-installatiekopieën in Azure | Microsoft Docs
description: Informatie over het gebruik van Azure PowerShell om te bepalen van de uitgever, aanbieding, SKU en versie voor Marketplace-VM-installatiekopieën.
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 188b8974-fabd-4cd3-b7dc-559cbb86b98a
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/28/2018
ms.author: danlep
ms.openlocfilehash: 885ee10bc63b65d936f5b433a18c4435b2503720
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/01/2018
---
# <a name="how-to-find-windows-vm-images-in-the-azure-marketplace-with-azure-powershell"></a>Het zoeken van installatiekopieën van virtuele Windows-machine in Azure Marketplace met Azure PowerShell

In dit artikel wordt beschreven hoe u met Azure PowerShell VM-installatiekopieën vinden in Azure Marketplace. Gebruik deze informatie om een Marketplace-installatiekopie opgeven wanneer u een virtuele machine via een programma met PowerShell maken Resource Manager-sjablonen of andere hulpprogramma's.

Zorg ervoor dat u geïnstalleerd en geconfigureerd, de meest recente [Azure PowerShell-module](/powershell/azure/install-azurerm-ps).

[!INCLUDE [virtual-machines-common-image-terms](../../../includes/virtual-machines-common-image-terms.md)]

## <a name="table-of-commonly-used-windows-images"></a>Tabel met veelgebruikte Windows-installatiekopieën
| Uitgever | Aanbieding | Sku |
|:--- |:--- |:--- |:--- |
| MicrosoftWindowsServer |WindowsServer |2016-Datacenter |
| MicrosoftWindowsServer |WindowsServer |2016-Datacenter-Server-Core |
| MicrosoftWindowsServer |WindowsServer |2016 Datacenter met Containers |
| MicrosoftWindowsServer |WindowsServer |2016-Nano-Server |
| MicrosoftWindowsServer |WindowsServer |2012-R2-Datacenter |
| MicrosoftWindowsServer |WindowsServer |2008-R2-SP1 |
| MicrosoftDynamicsNAV |DynamicsNAV |2017 |
| MicrosoftSharePoint |MicrosoftSharePointServer |2016 |
| MicrosoftSQLServer |SQL2014SP2-WS2012R2 |Enterprise |
| MicrosoftWindowsServerHPCPack |WindowsServerHPCPack |2012R2 |
| MicrosoftWindowsServerEssentials |WindowsServerEssentials |WindowsServerEssentials |

## <a name="navigate-the-images"></a>Installatiekopieën van het navigeren

Een installatiekopie niet vinden in een locatie op een andere manier is om uit te voeren de [Get-AzureRMVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher), [Get-AzureRMVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer), en [Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) cmdlets in de reeks. Met deze opdrachten moet u deze waarden bepalen:

1. Geef de uitgevers van installatiekopieën weer.
2. Geef de aanbiedingen voor een bepaalde uitgever weer.
3. Geef de SKU's voor een bepaalde aanbieding weer.

Voer vervolgens voor een geselecteerde SKU [Get-AzureRMVMImage](/powershell/module/azurerm.compute/get-azurermvmimage) voor een lijst met de versies te implementeren.

Geef eerst de uitgevers weer met de volgende opdrachten:

```powershell
$locName="<Azure location, such as West US>"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

Vul de naam van de gewenste uitgever in en voer de volgende opdrachten uit:

```powershell
$pubName="<publisher>"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Vul de naam van de gewenste aanbieding in en voer de volgende opdrachten uit:

```powershell
$offerName="<offer>"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Vul de naam van uw gekozen SKU en voer de volgende opdrachten:

```powershell
$skuName="<SKU>"
Get-AzureRMVMImage -Location $locName -Publisher $pubName -Offer $offerName -Sku $skuName | Select Version
```

Vanuit de uitvoer van de `Get-AzureRMVMImage` uitvoert, kunt u een installatiekopie van de versie voor het implementeren van een nieuwe virtuele machine.

De volgende opdrachten weergeven een voorbeeld van een volledige:

```powershell
$locName="West US"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName

```

Uitvoer:

```
PublisherName
-------------
a10networks
aiscaler-cache-control-ddos-and-url-rewriting-
alertlogic
AlertLogic.Extension
Barracuda.Azure.ConnectivityAgent
barracudanetworks
basho
boxless
bssw
Canonical
...
```

Voor de *legitieme Microsoft Windows Server* uitgever:

```powershell
$pubName="MicrosoftWindowsServer"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Uitvoer:

```
Offer
-----
Windows-HUB
WindowsServer
WindowsServerSemiAnnual
```

Voor de *Windows Server* bieden:

```powershell
$offerName="WindowsServer"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Uitvoer:

```
Skus
----
2008-R2-SP1
2008-R2-SP1-smalldisk
2012-Datacenter
2012-Datacenter-smalldisk
2012-R2-Datacenter
2012-R2-Datacenter-smalldisk
2016-Datacenter
2016-Datacenter-Server-Core
2016-Datacenter-Server-Core-smalldisk
2016-Datacenter-smalldisk
2016-Datacenter-with-Containers
2016-Datacenter-with-RDSH
2016-Nano-Server
```

Klik vervolgens voor de *2016 Datacenter* SKU:

```powershell
$skuName="2016-Datacenter"
Get-AzureRMVMImage -Location $locName -Publisher $pubName -Offer $offerName -Sku $skuName | Select Version
```

Nu u de geselecteerde uitgever, aanbieding, SKU en versie in een URN combineren kunt (waarden, gescheiden door:). Deze URN met geeft de `--image` parameter bij het maken van een virtuele machine met de [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) cmdlet. Houd er rekening mee dat u eventueel het versienummer in de URN door 'nieuwste vervangen kunt'. Deze versie is altijd de nieuwste versie van de installatiekopie. U kunt ook de URN met de [Set AzureRMVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) PowerShell-cmdlet. 

Als u een virtuele machine met Resource Manager-sjabloon implementeert, u instellen van de parameters van de afbeelding afzonderlijk in de `imageReference` eigenschappen. Zie de [sjabloonverwijzing](/azure/templates/microsoft.compute/virtualmachines).

[!INCLUDE [virtual-machines-common-marketplace-plan](../../../includes/virtual-machines-common-marketplace-plan.md)]


### <a name="view-plan-properties"></a>Eigenschappen van abonnement weergeven

Uitvoeren van een installatiekopie purchase plan om informatie te bekijken, de `Get-AzureRMVMImage` cmdlet. Als de `PurchasePlan` eigenschap in de uitvoer is niet `null`, de installatiekopie heeft de voorwaarden accepteren voordat programmatische implementatie moet u.  

Bijvoorbeeld, de installatiekopie van het Datacenter van Windows Server 2016 heeft geen aanvullende voorwaarden, omdat de `PurchasePlan` informatie is `null`:

```powershell
$version = "2016.127.20170406"
Get-AzureRMVMImage -Location $locName -Publisher $pubName -Offer $offerName -Skus $skuName -Version $version
```

Uitvoer:

```
Id               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westus/Publishers/MicrosoftWindowsServer/ArtifactTypes/VMImage/Offers/WindowsServer/Skus/2016-Datacenter/
                   Versions/2016.127.20170406
Location         : westus
PublisherName    : MicrosoftWindowsServer
Offer            : WindowsServer
Skus             : 2016-Datacenter
Version          : 2016.127.20170406
FilterExpression :
Name             : 2016.127.20170406
OSDiskImage      : {
                     "operatingSystem": "Windows"
                   }
PurchasePlan     : null
DataDiskImages   : []

```

Uitvoeren van een vergelijkbare opdracht voor de gegevens wetenschappelijke virtuele Machine, Windows 2016 afbeelding ziet u de volgende `PurchasePlan` eigenschappen: `name`, `product`, en `publisher`. (Enkele afbeeldingen hebben ook een `promotion code` eigenschap.) Zie de volgende secties de voorwaarden accepteren en programmatische implementatie inschakelen voor het implementeren van deze installatiekopie.

```powershell
Get-AzureRMVMImage -Location "westus" -Publisher "microsoft-ads" -Offer "windows-data-science-vm" -Skus "windows2016" -Version "0.2.02"
```

Uitvoer:

```
Id               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westus/Publishers/microsoft-ads/ArtifactTypes/VMIma
                   ge/Offers/windows-data-science-vm/Skus/windows2016/Versions/0.2.02
Location         : westus
PublisherName    : microsoft-ads
Offer            : windows-data-science-vm
Skus             : windows2016
Version          : 0.2.02
FilterExpression :
Name             : 0.2.02
OSDiskImage      : {
                     "operatingSystem": "Windows"
                   }
PurchasePlan     : {
                     "publisher": "microsoft-ads",
                     "name": "windows2016",
                     "product": "windows-data-science-vm"
                   }
DataDiskImages   : []

```

### <a name="accept-the-terms"></a>De voorwaarden accepteren

U kunt de licentievoorwaarden weergeven, met de [Get-AzureRmMarketplaceterms](/powershell/module/azurerm.marketplaceordering/get-azurermmarketplaceterms) cmdlet en geeft u de aankoop van plan bent parameters. De uitvoer bevat een koppeling met de voorwaarden voor de Marketplace-installatiekopie en ziet u of u eerder de voorwaarden geaccepteerd. Bijvoorbeeld:

```powershell
Get-AzureRmMarketplaceterms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016"
```

Uitvoer:

```
Publisher         : microsoft-ads
Product           : windows-data-science-vm
Plan              : windows2016
LicenseTextLink   : https://storelegalterms.blob.core.windows.net/legalterms/3E5ED_legalterms_MICROSOFT%253a2DADS%253a24WINDOWS%253a2DDATA%253a2DSCIENCE%253a2DV
                    M%253a24WINDOWS2016%253a24OC5SKMQOXSED66BBSNTF4XRCS4XLOHP7QMPV54DQU7JCBZWYFP35IDPOWTUKXUC7ZAG7W6ZMDD6NHWNKUIVSYBZUTZ245F44SU5AD7Q.txt
PrivacyPolicyLink : https://www.microsoft.com/EN-US/privacystatement/OnlineServices/Default.aspx
Signature         : 2UMWH6PHSAIM4U22HXPXW25AL2NHUJ7Y7GRV27EBL6SUIDURGMYG6IIDO3P47FFIBBDFHZHSQTR7PNK6VIIRYJRQ3WXSE6BTNUNENXA
Accepted          : False
Signdate          : 2/23/2018 7:43:00 PM
```

Gebruik de [Set AzureRmMarketplaceterms](/powershell/module/azurerm.marketplaceordering/set-azurermmarketplaceterms) cmdlet accepteren of weigeren van de voorwaarden. U hoeft alleen te accepteren eenmaal per abonnement voor de installatiekopie. Bijvoorbeeld:

```powershell

$agreementTerms=Get-AzureRmMarketplaceterms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016"

Set-AzureRmMarketplaceTerms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016" -Terms $agreementTerms -Accept

```

Uitvoer:

```
Publisher         : microsoft-ads
Product           : windows-data-science-vm
Plan              : windows2016
LicenseTextLink   : https://storelegalterms.blob.core.windows.net/legalterms/3E5ED_legalterms_MICROSOFT%253a2DADS%253a24WINDOWS%253a2DDATA%253a2DSCIENCE%253a2DV
                    M%253a24WINDOWS2016%253a24OC5SKMQOXSED66BBSNTF4XRCS4XLOHP7QMPV54DQU7JCBZWYFP35IDPOWTUKXUC7ZAG7W6ZMDD6NHWNKUIVSYBZUTZ245F44SU5AD7Q.txt
PrivacyPolicyLink : https://www.microsoft.com/EN-US/privacystatement/OnlineServices/Default.aspx
Signature         : VNMTRJK3MNJ5SROEG2BYDA2YGECU33GXTD3UFPLPC4BAVKAUL3PDYL3KBKBLG4ZCDJZVNSA7KJWTGMDSYDD6KRLV3LV274DLBJSS4GQ
Accepted          : True
Signdate          : 2/23/2018 7:49:31 PM
```

### <a name="deploy-using-purchase-plan-parameters"></a>Implementeren met behulp van inkoop plan parameters
Na het accepteren van de voorwaarden voor de installatiekopie, kunt u een virtuele machine in het abonnement te implementeren. Zoals u in het volgende fragment, gebruiken de [Set AzureRmVMPlan](/powershell/module/azurerm.compute/set-azurermvmplan) cmdlet om de inhoud van de Marketplace-plan voor het VM-object. Zie voor een volledige script netwerkinstellingen voor de virtuele machine maken en de implementatie voltooid de [PowerShell-scriptvoorbeelden](powershell-samples.md).

```powershell
...
$vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize Standard_D1

# Set the Marketplace plan information
$vmConfig = Set-AzureRmVMPlan -VM $vmConfig -Publisher "imagePlanPublisher" -Product "imagePlanProduct" -Name "imagePlanName"

$cred=Get-Credential

$vmConfig = Set-AzureRmVMOperatingSystem -Windows -VM $vmConfig -ComputerName "myVM" -Credential $cred

# Set the Marketplace image
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName "imagePublisher" -Offer "imageOffer" -Skus "imageSku" -Version "imageVersion"
...
```
U geeft u de VM-configuratie samen met de configuratie-objecten van het netwerk naar de `New-AzureRmVM` cmdlet.

## <a name="next-steps"></a>Volgende stappen
Maken van een virtuele machine snel met `New-AzureRmVM` met behulp van basisinstallatiekopie informatie, Zie [virtuele Windows-machine maken met PowerShell](quick-create-powershell.md).

Zie het voorbeeld van een PowerShell-script voor [een volledig geconfigureerde virtuele machine maken](../scripts/virtual-machines-windows-powershell-sample-create-vm.md).


