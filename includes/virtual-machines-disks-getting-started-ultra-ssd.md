---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 05/10/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 742e0028b1f92beb8300cc97f09d8292259fbc0a
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67712412"
---
# <a name="enable-and-deploy-azure-ultra-ssds-preview"></a>Inschakelt en implementeert Azure ultra SSD's (preview)

Azure ultra Solid-State stations (SSD) (preview) bieden hoge doorvoer, hoge IOPS en consistente lage latentie schijfopslag voor Azure IaaS virtuele machines (VM's). Deze nieuwe aanbieding biedt boven aan de prestaties van de regel op het niveau van de dezelfde beschikbaarheid als onze bestaande schijven-aanbiedingen. Een groot voordeel van ultra SSD's is de mogelijkheid om de prestaties van de SSD, samen met uw workloads zonder dat uw virtuele machines opnieuw moet worden dynamisch te wijzigen. Ultra SSD's zijn geschikt voor gegevensintensieve workloads, zoals SAP HANA, databases van de bovenste laag en transactie-zware workloads.

Op dit moment ultra SSD's zijn beschikbaar als preview en moet u [inschrijven](https://aka.ms/UltraSSDPreviewSignUp) in de Preview-versie om ze toegang te krijgen.

## <a name="determine-your-availability-zone"></a>Bepalen van uw binnen een beschikbaarheidszone

Na goedkeuring, moet u bepalen welke u werkt, om te kunnen gebruiken ultra SSD's binnen een beschikbaarheidszone. Voer een van de volgende opdrachten om te bepalen welke zone in VS-Oost 2 de ultra schijf om te implementeren:

PowerShell: `Get-AzComputeResourceSku | where {$_.ResourceType -eq "disks" -and $_.Name -eq "UltraSSD_LRS" }`

CLI: `az vm list-skus --resource-type disks --query "[?name=='UltraSSD_LRS'].locationInfo"`

Het antwoord is vergelijkbaar met het onderstaande formulier, waarbij X staat voor de zone moet worden gebruikt voor het implementeren van in VS-Oost 2. X kan 1, 2 of 3 zijn.

Behoud de **Zones** waarde staat voor de beschikbaarheidszone en u deze nodig heeft om te implementeren van een ultra SSD.

|ResourceType  |Name  |Location  |Zones  |Beperking  |Mogelijkheid  |Waarde  |
|---------|---------|---------|---------|---------|---------|---------|
|Schijven     |UltraSSD_LRS         |eastus2         |X         |         |         |         |

> [!NOTE]
> Als er geen reactie van de opdracht is, wordt uw registratie voor de functie nog steeds is in behandeling of niet goedgekeurd nog.

Nu dat u weet welke zone om in te implementeren, volgt u de implementatiestappen in dit artikel om op te halen van uw eerste virtuele machines geïmplementeerd met de ultra SSD.

## <a name="deploy-an-ultra-ssd-using-azure-resource-manager"></a>Implementeren van een ultra SSD met Azure Resource Manager

Bepaal eerst de VM-grootte te implementeren. Als onderdeel van deze Preview-versie, worden alleen DsV3 en de EsV3-VM-families ondersteund. Raadpleeg de tweede tabel op deze [blog](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/) voor meer informatie over deze VM-grootten.

Als u maken van een virtuele machine met meerdere ultra SSD's wilt, raadpleegt u het voorbeeld [een virtuele machine maken met meerdere ultra SSD](https://aka.ms/UltraSSDTemplate).

Als u van plan bent om uw eigen sjabloon te gebruiken, zorg ervoor dat **apiVersion** voor `Microsoft.Compute/virtualMachines` en `Microsoft.Compute/Disks` is ingesteld als `2018-06-01` (of hoger).

De sku van de schijf ingesteld op **UltraSSD_LRS**, stel daarna de schijfcapaciteit, IOPS, binnen een beschikbaarheidszone en doorvoer in MBps die moet maken van een ultra-schijf.

Zodra de virtuele machine is ingericht, kunt u partitioneren en formatteren van de gegevensschijven en configureert deze voor uw workloads.

## <a name="deploy-an-ultra-ssd-using-cli"></a>Implementeren van een ultra SSD met behulp van CLI

Bepaal eerst de VM-grootte te implementeren. Als onderdeel van deze Preview-versie, worden alleen DsV3 en de EsV3-VM-families ondersteund. Raadpleeg de tweede tabel op deze [blog](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/) voor meer informatie over deze VM-grootten.

Voor het gebruik van ultra SSD's, moet u een virtuele machine die is geschikt voor gebruik van ultra SSD's maken.

Vervangen of in te stellen de **$vmname**, **$rgname**, **$diskname**, **$location**, **$password**, **$user** variabelen met uw eigen waarden. Stel **$zone** aan de waarde van uw binnen een beschikbaarheidszone die u hebt verkregen via de [het begin van dit artikel](#determine-your-availability-zone). Voer vervolgens de volgende CLI-opdracht om een ultra ingeschakelde VM te maken:

```azurecli-interactive
az vm create --subscription $subscription -n $vmname -g $rgname --image Win2016Datacenter --ultra-ssd-enabled true --zone $zone --authentication-type password --admin-password $password --admin-username $user --attach-data-disks $diskname --size Standard_D4s_v3 --location $location
```

### <a name="create-an-ultra-ssd-using-cli"></a>Maken van een ultra SSD met behulp van CLI

Nu dat u een virtuele machine die is geschikt voor gebruik van ultra SSD's hebt, kunt u maken en koppelen van een ultra SSD toe.

```azurecli-interactive
location="eastus2"
subscription="xxx"
rgname="ultraRG"
diskname="ssd1"
vmname="ultravm1"
zone=123

#create an Ultra SSD disk
az disk create `
--subscription $subscription `
-n $diskname `
-g $rgname `
--size-gb 4 `
--location $location `
--zone $zone `
--sku UltraSSD_LRS `
--disk-iops-read-write 1000 `
--disk-mbps-read-write 50
```

### <a name="adjust-the-performance-of-an-ultra-ssd-using-cli"></a>Aanpassen van de prestaties van een ultra SSD met behulp van CLI

Ultra SSD's bieden een unieke mogelijkheid waardoor u om aan te passen van de prestaties, de volgende opdracht ziet u hoe u deze functie wilt gebruiken:

```azurecli-interactive
az disk update `
--subscription $subscription `
--resource-group $rgname `
--name $diskName `
--set diskIopsReadWrite=80000 `
--set diskMbpsReadWrite=800
```

## <a name="deploy-an-ultra-ssd-using-powershell"></a>Implementeren van een ultra SSD met behulp van PowerShell

Bepaal eerst de VM-grootte te implementeren. Als onderdeel van deze Preview-versie, worden alleen DsV3 en de EsV3-VM-families ondersteund. Raadpleeg de tweede tabel op deze [blog](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/) voor meer informatie over deze VM-grootten.

Voor het gebruik van ultra SSD's, moet u een virtuele machine die is geschikt voor gebruik van ultra SSD's maken. Vervangen of in te stellen de **$resourcegroup** en **$vmName** variabelen met uw eigen waarden. Stel **$zone** aan de waarde van uw binnen een beschikbaarheidszone die u hebt verkregen via de [het begin van dit artikel](#determine-your-availability-zone). Voer daarna het volgende [New-AzVm](/powershell/module/az.compute/new-azvm) opdracht voor het maken van een ultra ingeschakeld:

```powershell
New-AzVm `
    -ResourceGroupName $resourcegroup `
    -Name $vmName `
    -Location "eastus2" `
    -Image "Win2016Datacenter" `
    -EnableUltraSSD `
    -size "Standard_D4s_v3" `
    -zone $zone
```

### <a name="create-an-ultra-ssd-using-powershell"></a>Maken van een ultra SSD met behulp van PowerShell

Nu dat u een virtuele machine die is geschikt voor gebruik van ultra SSD's hebt, kunt u maken en koppelen van een ultra SSD toe:

```powershell
$diskconfig = New-AzDiskConfig `
-Location 'EastUS2' `
-DiskSizeGB 8 `
-DiskIOPSReadWrite 1000 `
-DiskMBpsReadWrite 100 `
-AccountType UltraSSD_LRS `
-CreateOption Empty `
-zone $zone;

New-AzDisk `
-ResourceGroupName $resourceGroup `
-DiskName 'Disk02' `
-Disk $diskconfig;
```

### <a name="adjust-the-performance-of-an-ultra-ssd-using-powershell"></a>Aanpassen van de prestaties van een ultra SSD met behulp van PowerShell

Ultra SSD's hebben een unieke mogelijkheid waardoor u om aan te passen van de prestaties, de volgende opdracht is een voorbeeld waarin Hiermee past u de prestaties zonder dat de schijf loskoppelen:

```powershell
$diskupdateconfig = New-AzDiskUpdateConfig -DiskMBpsReadWrite 2000
Update-AzDisk -ResourceGroupName $resourceGroup -DiskName $diskName -DiskUpdate $diskupdateconfig
```

## <a name="next-steps"></a>Volgende stappen

Als u wilt het proberen van het nieuwe schijftype [toegang aanvragen tot de Preview-versie met deze enquête](https://aka.ms/UltraSSDPreviewSignUp).