---
title: Selecteer Windows-VM-installatiekopieën in Azure | Microsoft Docs
description: Azure PowerShell gebruiken om te bepalen van de uitgever, aanbieding, SKU en versie voor Marketplace-VM-installatiekopieën.
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
ms.date: 01/25/2019
ms.author: danlep
ms.openlocfilehash: ebf4b413ecfbdf850f0d9e6ebc50f166e27bee0a
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58081845"
---
# <a name="find-windows-vm-images-in-the-azure-marketplace-with-azure-powershell"></a>Windows-VM-installatiekopieën zoeken in de Azure Marketplace met Azure PowerShell

In dit artikel wordt beschreven hoe u Azure PowerShell gebruiken voor het VM-installatiekopieën zoeken in de Azure Marketplace. U kunt vervolgens een Marketplace-installatiekopie opgeven wanneer u een virtuele machine via een programma met PowerShell maken, Resource Manager-sjablonen of andere hulpprogramma's.

U kunt ook bladeren beschikbare installatiekopieën en aanbiedingen met behulp van de [Azure Marketplace](https://azuremarketplace.microsoft.com/) winkel, de [Azure-portal](https://portal.azure.com), of de [Azure CLI](../linux/cli-ps-findimage.md). 

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

[!INCLUDE [virtual-machines-common-image-terms](../../../includes/virtual-machines-common-image-terms.md)]

## <a name="table-of-commonly-used-windows-images"></a>Tabel met veelgebruikte Windows-installatiekopieën

Deze tabel toont een subset van de beschikbare SKU's voor de opgegeven uitgevers en aanbiedingen.

| Uitgever | Aanbieding | Sku |
|:--- |:--- |:--- |
| MicrosoftWindowsServer |WindowsServer |2019-Datacenter |
| MicrosoftWindowsServer |WindowsServer |2019-Datacenter-Core |
| MicrosoftWindowsServer |WindowsServer |2019 Datacenter met Containers |
| MicrosoftWindowsServer |WindowsServer |2016-Datacenter |
| MicrosoftWindowsServer |WindowsServer |2016-Datacenter-Server-Core |
| MicrosoftWindowsServer |WindowsServer |2016 Datacenter met Containers |
| MicrosoftWindowsServer |WindowsServer |2012-R2-Datacenter |
| MicrosoftWindowsServer |WindowsServer |2012-Datacenter |
| MicrosoftDynamicsNAV |DynamicsNAV |2017 |
| MicrosoftSharePoint |MicrosoftSharePointServer |2019 |
| MicrosoftSQLServer |SQL2019-WS2016 |Enterprise |
| MicrosoftRServer |RServer-WS2016 |Enterprise |

## <a name="navigate-the-images"></a>De installatiekopieën doorzoeken

Eén manier om het vinden van een installatiekopie op een locatie is het uitvoeren van de [Get-AzVMImagePublisher](https://docs.microsoft.com/powershell/module/az.compute/get-azvmimagepublisher), [Get-AzVMImageOffer](https://docs.microsoft.com/powershell/module/az.compute/get-azvmimageoffer), en [Get-AzVMImageSku](https://docs.microsoft.com/powershell/module/az.compute/get-azvmimagesku) cmdlets in volgorde:

1. Geef de uitgevers van installatiekopieën weer.
2. Geef de aanbiedingen voor een bepaalde uitgever weer.
3. Geef de SKU's voor een bepaalde aanbieding weer.

Voer vervolgens voor een geselecteerde SKU [Get-AzVMImage](https://docs.microsoft.com/powershell/module/az.compute/get-azvmimage) om te implementeren in de versies weer te geven.

1. De uitgevers weer:

    ```powershell
    $locName="<Azure location, such as West US>"
    Get-AzVMImagePublisher -Location $locName | Select PublisherName
    ```

2. Vul de naam van uw gewenste uitgever en de aanbiedingen te vermelden:

    ```powershell
    $pubName="<publisher>"
    Get-AzVMImageOffer -Location $locName -Publisher $pubName | Select Offer
    ```

3. Vul de naam van de gewenste aanbieding in en lijst van de SKU's:

    ```powershell
    $offerName="<offer>"
    Get-AzVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
    ```

4. Vul de naam van uw gekozen SKU en versie van de installatiekopie ophalen:

    ```powershell
    $skuName="<SKU>"
    Get-AzVMImage -Location $locName -Publisher $pubName -Offer $offerName -Sku $skuName | Select Version
    ```
    
Uit de uitvoer van de `Get-AzVMImage` opdracht, kunt u een versie-installatiekopie naar een nieuwe virtuele machine implementeren.

Het volgende voorbeeld ziet u de volledige reeks opdrachten en de uitvoer:

```powershell
$locName="West US"
Get-AzVMImagePublisher -Location $locName | Select PublisherName
```

Gedeeltelijke uitvoer:

```
PublisherName
-------------
...
abiquo
accedian
accellion
accessdata-group
accops
Acronis
Acronis.Backup
actian-corp
actian_matrix
actifio
activeeon
adgs
advantech
advantech-webaccess
advantys
...
```

Voor de *MicrosoftWindowsServer* uitgever:

```powershell
$pubName="MicrosoftWindowsServer"
Get-AzVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Uitvoer:

```
Offer
-----
Windows-HUB
WindowsServer
WindowsServerSemiAnnual
```

Voor de *WindowsServer* bieden:

```powershell
$offerName="WindowsServer"
Get-AzVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Gedeeltelijke uitvoer:

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
2019-Datacenter
2019-Datacenter-Core
2019-Datacenter-Core-smalldisk
2019-Datacenter-Core-with-Containers
...
```

Vervolgens voor de *2019 Datacenter* SKU:

```powershell
$skuName="2019-Datacenter"
Get-AzVMImage -Location $locName -Publisher $pubName -Offer $offerName -Sku $skuName | Select Version
```

Nu u de geselecteerde uitgever, aanbieding, SKU en versie in een URN combineren kunt (waarden worden gescheiden door:). Doorgeven van deze URN met de `--image` parameter bij het maken van een virtuele machine met de [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) cmdlet. U kunt eventueel het versienummer in de URN vervangen door 'nieuwste', om op te halen van de meest recente versie van de installatiekopie.

Als u een VM met Resource Manager-sjabloon implementeren, wordt afzonderlijk in stelt u de parameters van de installatiekopie van het `imageReference` eigenschappen. Zie de [sjabloonverwijzing](/azure/templates/microsoft.compute/virtualmachines).

[!INCLUDE [virtual-machines-common-marketplace-plan](../../../includes/virtual-machines-common-marketplace-plan.md)]

### <a name="view-plan-properties"></a>Schema-eigenschappen weergeven

Als u wilt weergeven van een installatiekopie aankoop plangegevens, voer de `Get-AzVMImage` cmdlet. Als de `PurchasePlan` eigenschap in de uitvoer is niet `null`, de installatiekopie heeft voorwaarden moet u accepteren voordat u programmatische implementatie.  

Bijvoorbeeld, de *Windows Server 2016 Datacenter* installatiekopie bevat geen aanvullende voorwaarden, dus de `PurchasePlan` informatie is `null`:

```powershell
$version = "2016.127.20170406"
Get-AzVMImage -Location $locName -Publisher $pubName -Offer $offerName -Skus $skuName -Version $version
```

Uitvoer:

```
Id               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westus/Publishers/MicrosoftWindowsServer/ArtifactTypes/VMImage/Offers/WindowsServer/Skus/2016-Datacenter/Versions/2019.0.20190115
Location         : westus
PublisherName    : MicrosoftWindowsServer
Offer            : WindowsServer
Skus             : 2019-Datacenter
Version          : 2019.0.20190115
FilterExpression :
Name             : 2019.0.20190115
OSDiskImage      : {
                     "operatingSystem": "Windows"
                   }
PurchasePlan     : null
DataDiskImages   : []

```

Het volgende voorbeeld ziet u een vergelijkbare opdracht voor het *Data Science Virtual Machine - Windows 2016* installatiekopie, die de volgende `PurchasePlan` eigenschappen: `name`, `product`, en `publisher`. Sommige installatiekopieën zijn ook een `promotion code` eigenschap. Zie de volgende secties om de voorwaarden te accepteren en programmatische implementatie moet worden ingeschakeld voor het implementeren van deze installatiekopie.

```powershell
Get-AzVMImage -Location "westus" -Publisher "microsoft-ads" -Offer "windows-data-science-vm" -Skus "windows2016" -Version "0.2.02"
```

Uitvoer:

```
Id               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westus/Publishers/microsoft-ads/ArtifactTypes/VMImage/Offers/windows-data-science-vm/Skus/windows2016/Versions/19.01.14
Location         : westus
PublisherName    : microsoft-ads
Offer            : windows-data-science-vm
Skus             : windows2016
Version          : 19.01.14
FilterExpression :
Name             : 19.01.14
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

Als u de licentievoorwaarden, gebruikt u de [Get-AzMarketplaceterms](https://docs.microsoft.com/powershell/module/az.marketplaceordering/get-azmarketplaceterms) cmdlet en geeft u de aankoop van plan bent parameters. De uitvoer bevat een koppeling naar de voorwaarden voor de Marketplace-installatiekopie en laat zien of u eerder de voorwaarden geaccepteerd. Zorg ervoor dat alleen kleine letters gebruiken in de parameterwaarden.

```powershell
Get-AzMarketplaceterms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016"
```

Uitvoer:

```
Publisher         : microsoft-ads
Product           : windows-data-science-vm
Plan              : windows2016
LicenseTextLink   : https://storelegalterms.blob.core.windows.net/legalterms/3E5ED_legalterms_MICROSOFT%253a2DADS%253a24WINDOWS%253a2DDATA%253a2DSCIENCE%253a2DVM%253a24WINDOWS2016%253a24OC5SKMQOXSED66BBSNTF4XRCS4XLOHP7QMPV54DQU7JCBZWYFP35IDPOWTUKXUC7ZAG7W6ZMDD6NHWNKUIVSYBZUTZ245F44SU5AD7Q.txt
PrivacyPolicyLink : https://www.microsoft.com/EN-US/privacystatement/OnlineServices/Default.aspx
Signature         : 2UMWH6PHSAIM4U22HXPXW25AL2NHUJ7Y7GRV27EBL6SUIDURGMYG6IIDO3P47FFIBBDFHZHSQTR7PNK6VIIRYJRQ3WXSE6BTNUNENXA
Accepted          : False
Signdate          : 1/25/2019 7:43:00 PM
```

Gebruik de [Set AzMarketplaceterms](https://docs.microsoft.com/powershell/module/az.marketplaceordering/set-azmarketplaceterms) cmdlet om te accepteren of weigeren van de voorwaarden. U hoeft alleen te accepteren één keer per abonnement voor de installatiekopie. Zorg ervoor dat alleen kleine letters gebruiken in de parameterwaarden. 

```powershell
$agreementTerms=Get-AzMarketplaceterms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016"

Set-AzMarketplaceTerms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016" -Terms $agreementTerms -Accept
```

Uitvoer:

```
Publisher         : microsoft-ads
Product           : windows-data-science-vm
Plan              : windows2016
LicenseTextLink   : https://storelegalterms.blob.core.windows.net/legalterms/3E5ED_legalterms_MICROSOFT%253a2DADS%253a24WINDOWS%253a2DDATA%253a2DSCIENCE%253a2DV
                    M%253a24WINDOWS2016%253a24OC5SKMQOXSED66BBSNTF4XRCS4XLOHP7QMPV54DQU7JCBZWYFP35IDPOWTUKXUC7ZAG7W6ZMDD6NHWNKUIVSYBZUTZ245F44SU5AD7Q.txt
PrivacyPolicyLink : https://www.microsoft.com/EN-US/privacystatement/OnlineServices/Default.aspx
Signature         : XXXXXXK3MNJ5SROEG2BYDA2YGECU33GXTD3UFPLPC4BAVKAUL3PDYL3KBKBLG4ZCDJZVNSA7KJWTGMDSYDD6KRLV3LV274DLBXXXXXX
Accepted          : True
Signdate          : 2/23/2018 7:49:31 PM
```

### <a name="deploy-using-purchase-plan-parameters"></a>Implementeren met behulp van aankoop planning parameters

Na de voorwaarden van een installatiekopie te accepteren, kunt u een virtuele machine in het desbetreffende abonnement implementeren. Zoals u in het volgende codefragment, gebruikt u de [Set AzVMPlan](https://docs.microsoft.com/powershell/module/az.compute/set-azvmplan) cmdlet om in te stellen de plangegevens van Marketplace voor de VM-object. Zie voor een volledige script netwerkinstellingen voor de virtuele machine maken en uitvoeren van de implementatie, de [PowerShell-scriptvoorbeelden](powershell-samples.md).

```powershell
...

$vmConfig = New-AzVMConfig -VMName "myVM" -VMSize Standard_D1

# Set the Marketplace plan information

$publisherName = "microsoft-ads"

$productName = "windows-data-science-vm"

$planName = "windows2016"

$vmConfig = Set-AzVMPlan -VM $vmConfig -Publisher $publisherName -Product $productName -Name $planName

$cred=Get-Credential

$vmConfig = Set-AzVMOperatingSystem -Windows -VM $vmConfig -ComputerName "myVM" -Credential $cred

# Set the Marketplace image

$offerName = "windows-data-science-vm"

$skuName = "windows2016"

$version = "19.01.14"

$vmConfig = Set-AzVMSourceImage -VM $vmConfig -PublisherName $publisherName -Offer $offerName -Skus $skuName -Version $version
...
```
U zult geeft u de configuratie van de virtuele machine samen met het netwerk configuratie-objecten naar de `New-AzVM` cmdlet.

## <a name="next-steps"></a>Volgende stappen

Het maken van een virtuele machine snel met de `New-AzVM` cmdlet met behulp van informatie van de basic-installatiekopie, Zie [een Windows-machine maken met PowerShell](quick-create-powershell.md).


Een PowerShell-voorbeeldscript om te zien [maken van een volledig geconfigureerde virtuele machine](../scripts/virtual-machines-windows-powershell-sample-create-vm.md).


