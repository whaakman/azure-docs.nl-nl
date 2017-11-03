---
title: Maken van een SQL Server-Machine in Azure PowerShell (Resource Manager) | Microsoft Docs
description: "Bevat de stappen en PowerShell-scripts voor het maken van een virtuele machine in Azure met SQL Server-installatiekopieën voor virtuele machine-galerie."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: 98d50dd8-48ad-444f-9031-5378d8270d7b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/29/2017
ms.author: jroth
ms.openlocfilehash: 33c306258b6be40f2c5cbc016e3c84e36bf61e0d
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2017
---
# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-resource-manager"></a>Inrichten van een SQL Server-virtuele machine met Azure PowerShell (Resource Manager)
> [!div class="op_single_selector"]
> * [Portal](virtual-machines-windows-portal-sql-server-provision.md)
> * [PowerShell](virtual-machines-windows-ps-sql-create.md)
>
>

## <a name="overview"></a>Overzicht
Deze zelfstudie ziet u het maken van een enkele Azure virtuele machine met de **Azure Resource Manager** implementatiemodel met Azure PowerShell-cmdlets. We gaan een enkele virtuele machine met behulp van één schijf station van een installatiekopie in de SQL-galerie maken in deze zelfstudie. Nieuwe providers voor de opslag-, netwerk- en rekenresources die wordt gebruikt door de virtuele machine u kunt maken. Als u bestaande providers voor een van deze resources hebt, kunt u deze providers in plaats daarvan.

Als u de klassieke versie van dit onderwerp, Zie [een SQL Server-machine met behulp van Azure PowerShell klassieke inrichten](../classic/ps-sql-create.md).

## <a name="prerequisites"></a>Vereisten
Voor deze zelfstudie hebt u het volgende nodig:

* Een Azure-account en abonnement voordat u begint. Als u niet hebt, zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
* [Azure PowerShell)](/powershell/azure/overview), minimale versie van 1.4.0 of hoger (deze zelfstudie geschreven met behulp van versie 1.5.0).
  * Voor het ophalen van uw versie, typ **Azure Get-Module - ListAvailable**.

## <a name="configure-your-subscription"></a>Configureer uw abonnement
Open Windows PowerShell en toegang tot uw Azure-account maken door de volgende cmdlet. U krijgt een teken in het scherm uw referenties in te voeren. Gebruik dezelfde e-mailadres en wachtwoord dat u aan te melden bij de Azure-portal.

```PowerShell
Add-AzureRmAccount
```

Na het aanmelden is mogelijk informatie te zien op het scherm met de naam van abonnement en de ID van uw standaardabonnement. Dit is het abonnement waarin de bronnen voor deze zelfstudie wordt worden gemaakt, tenzij u naar een ander abonnement wijzigen. Als u meerdere abonnementen hebt, voer de volgende cmdlet voor een lijst van al uw abonnementen:

```PowerShell
Get-AzureRmSubscription
```
Voer de volgende opdracht met het doel-SubscriptionName Ga naar een ander abonnement.

```PowerShell
Select-AzureRmSubscription -SubscriptionName YourTargetSubscriptionName
```

## <a name="define-image-variables"></a>Definieer de installatiekopie van variabelen
Om te vereenvoudigen bruikbaarheid van en kennis van de voltooide script uit deze zelfstudie, wordt we gestart door een aantal variabelen definiëren. Wijzig de parameterwaarden zoals u dat wilt, maar pas op voor de naamgeving van beperkingen met betrekking tot de naam van lengten en speciale tekens bij het wijzigen van de opgegeven waarden.

### <a name="location-and-resource-group"></a>Locatie en resourcegroep
Twee variabelen gebruiken voor het definiëren van het gegevensgebied en de resourcegroep waarin u de andere bronnen voor de virtuele machine maakt.

Naar wens wijzigen en vervolgens de volgende cmdlets voor het initialiseren van deze variabelen wordt uitgevoerd.

```PowerShell
$Location = "SouthCentralUS"
$ResourceGroupName = "sqlvm1"
```

### <a name="storage-properties"></a>Eigenschappen van opslag
De volgende variabelen gebruiken voor het definiëren van de storage-account en het type opslag moet worden gebruikt door de virtuele machine.

Naar wens wijzigen en vervolgens de volgende cmdlet om te initialiseren van deze variabelen wordt uitgevoerd. Houd er rekening mee dat in dit voorbeeld we gebruiken [Premium-opslag](../premium-storage.md), wat wordt aanbevolen voor productieworkloads. Zie voor meer informatie over deze richtlijnen en andere aanbevelingen [best practices prestaties for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-performance.md).

```PowerShell
$StorageName = $ResourceGroupName + "storage"
$StorageSku = "Premium_LRS"
```

### <a name="network-properties"></a>Eigenschappen van het netwerk
De volgende variabelen gebruiken voor het definiëren van de netwerkinterface, de TCP/IP-toewijzingsmethode, naam van het virtuele netwerk, de naam van de virtuele subnet, het bereik van IP-adressen voor het virtuele netwerk, het bereik van IP-adressen voor het subnet en het openbare domein-naamlabel moet worden gebruikt door het netwerk in de virtuele machine.

Naar wens wijzigen en vervolgens de volgende cmdlet om te initialiseren van deze variabelen wordt uitgevoerd.

```PowerShell
$InterfaceName = $ResourceGroupName + "ServerInterface"
$TCPIPAllocationMethod = "Dynamic"
$VNetName = $ResourceGroupName + "VNet"
$SubnetName = "Default"
$VNetAddressPrefix = "10.0.0.0/16"
$VNetSubnetAddressPrefix = "10.0.0.0/24"
$DomainName = "sqlvm1"
```

### <a name="virtual-machine-properties"></a>Eigenschappen van virtuele machine
De volgende variabelen gebruiken voor het definiëren van de naam van de virtuele machine, de computernaam, de grootte van de virtuele machine en de naam van de besturingssysteem-schijf voor de virtuele machine.

Naar wens wijzigen en vervolgens de volgende cmdlet om te initialiseren van deze variabelen wordt uitgevoerd.

```PowerShell
$VMName = $ResourceGroupName + "VM"
$ComputerName = $ResourceGroupName + "Server"
$VMSize = "Standard_DS13"
$OSDiskName = $VMName + "OSDisk"
```

### <a name="image-properties"></a>Eigenschappen van de installatiekopie
De volgende variabelen gebruiken voor het definiëren van de afbeelding moet worden gebruikt voor de virtuele machine. In dit voorbeeld wordt wordt de installatiekopie van het SQL Server 2016 Developer-editie gebruikt. De ontwikkelaarsversie vrijelijk is gelicentieerd voor het testen en ontwikkeling en u betaalt alleen voor de kosten van het uitvoeren van de virtuele machine.

Naar wens wijzigen en vervolgens de volgende cmdlet om te initialiseren van deze variabelen wordt uitgevoerd.

```PowerShell
$PublisherName = "MicrosoftSQLServer"
$OfferName = "SQL2016-WS2016"
$Sku = "SQLDEV"
$Version = "latest"
```

Houd er rekening mee dat u een volledige lijst met aanbiedingen van SQL Server-installatiekopie met de opdracht Get-AzureRmVMImageOffer kunt krijgen:

```PowerShell
Get-AzureRmVMImageOffer -Location 'East US' -Publisher 'MicrosoftSQLServer'
```

En u ziet de SKU's die beschikbaar zijn voor een aanbieding met de opdracht Get-AzureRmVMImageSku. De volgende opdracht toont alle SKU's beschikbaar zijn voor de **SQL2016SP1 WS2016** bieden.

```PowerShell
Get-AzureRmVMImageSku -Location $Location -Publisher 'MicrosoftSQLServer' -Offer 'SQL2016SP1-WS2016' | Select Skus
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Met het Resource Manager-implementatiemodel is het eerste object die u maakt de resourcegroep. We gebruiken de [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet voor het maken van een Azure-resourcegroep en de bijbehorende resources met de naam van een resourcegroep en locatie die is gedefinieerd door de variabelen die u eerder is geïnitialiseerd.

Voer de volgende cmdlet als u wilt maken van uw nieuwe resourcegroep.

```PowerShell
New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location
```

## <a name="create-a-storage-account"></a>Een opslagaccount maken
De virtuele machine vereist storage-resources voor de besturingssysteemschijf en voor de SQL Server-gegevens en logboekbestanden. Voor eenvoud, u kunt één schijf maken voor beide. U kunt extra schijven koppelen later via de [toevoegen Azure-schijf](/powershell/module/azure/add-azuredisk) cmdlet om te kunnen plaatsen van uw SQL Server-gegevens en logboekbestanden op schijven met specifieke bestanden. We gebruiken de [nieuw AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet een standard-opslag-account maken in uw nieuwe resourcegroep en met de naam van het opslagaccount, opslag Sku-naam en locatie die is gedefinieerd met behulp van de variabelen die u eerder is geïnitialiseerd.

De volgende cmdlet als u wilt maken van uw nieuwe opslagaccount worden uitgevoerd.

```PowerShell
$StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location
```

## <a name="create-network-resources"></a>Maken van netwerkbronnen
De virtuele machine vereist een aantal netwerkbronnen voor verbinding met het netwerk.

* Elke virtuele machine vereist een virtueel netwerk.
* Een virtueel netwerk moet ten minste één subnet dat is gedefinieerd.
* Een netwerkinterface moet worden gedefinieerd met een openbare of een particulier IP-adres.

### <a name="create-a-virtual-network-subnet-configuration"></a>Een virtueel netwerk subnetconfiguratie maken
Er wordt gestart door het maken van de subnetconfiguratie van een voor het virtuele netwerk. In deze zelfstudie maken we een standaard subnet met de [nieuw AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) cmdlet. We gaan onze subnetconfiguratie virtueel netwerk maken met de naam en adres subnetvoorvoegsel gedefinieerd met behulp van de variabelen die u eerder is geïnitialiseerd.

> [!NOTE]
> U kunt aanvullende eigenschappen van de configuratie van het virtuele netwerk subnet met deze cmdlet definiëren, maar dat is buiten het bereik van deze zelfstudie.

De volgende cmdlet als u wilt maken van uw virtuele subnet-configuratie worden uitgevoerd.

```PowerShell
$SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
```

### <a name="create-a-virtual-network"></a>Een virtueel netwerk maken
Vervolgens maken we onze virtueel netwerk met behulp van de [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) cmdlet. We gaan het virtuele netwerk in uw nieuwe resourcegroep maken, met de naam, de locatie en het adres voorvoegsel gedefinieerd met behulp van de variabelen die u eerder is geïnitialiseerd en de subnet-configuratie die u hebt gedefinieerd in de vorige stap.

De volgende cmdlet als u wilt maken van het virtuele netwerk worden uitgevoerd.

```PowerShell
$VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
```

### <a name="create-the-public-ip-address"></a>Het openbare IP-adres maken
Nu we hebben onze virtueel netwerk is gedefinieerd, moet er een IP-adres configureren voor verbinding met de virtuele machine. We gaan een openbaar IP-adres met behulp van dynamische IP-adressen ter ondersteuning van verbinding met Internet maken voor deze zelfstudie. We gebruiken de [nieuw AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) cmdlet voor het maken van het openbare IP-adres in de groep gemaakt resource prevously en met de naam, locatie, toewijzingsmethode en DNS-domeinnaamlabel gedefinieerd met behulp van de variabelen die u eerder is geïnitialiseerd.

> [!NOTE]
> U kunt aanvullende eigenschappen van het openbare IP-adres met behulp van deze cmdlet definiëren, maar dat is buiten het bereik van deze eerste zelfstudie. U kunt ook een persoonlijk adres of een adres maken met een statisch adres, maar die ook is buiten het bereik van deze zelfstudie.

Voer de volgende cmdlet als u wilt maken van uw openbare IP-adres.

```PowerShell
$PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
```

### <a name="create-the-network-interface"></a>De netwerkinterface maken
Er zijn nu gereed voor het maken van de netwerkinterface die de virtuele machine wordt gebruikt. We gebruiken de [nieuw AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) cmdlet onze netwerkinterface maken in de resourcegroep die eerder hebt gemaakt en met de naam, locatie, subnet en openbaar IP-adres dat eerder is gedefinieerd.

De volgende cmdlet als u wilt maken van de netwerkinterface worden uitgevoerd.

```PowerShell
$Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id
```

## <a name="configure-a-vm-object"></a>Een VM-object configureren
Nu dat we hebben een opslag- en netwerkbronnen die zijn gedefinieerd, zijn er klaar voor het definiëren van rekenresources voor de virtuele machine. In deze zelfstudie wordt wij Geef de grootte van de virtuele machine en de verschillende eigenschappen van het besturingssysteem, de netwerkinterface die we eerder hebt gemaakt, blob-opslag te definiëren en geef vervolgens de schijf van het besturingssysteem opgeven.

### <a name="create-the-vm-object"></a>Het VM-object maken
Er wordt gestart door te geven van de grootte van de virtuele machine. We geven een DS13 voor deze zelfstudie. We gebruiken de [nieuw AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) cmdlet geen object configureerbare virtuele machine maken met de naam en de grootte die is gedefinieerd met behulp van de variabelen die u eerder is geïnitialiseerd.

De volgende cmdlet als u wilt maken van het object van de virtuele machine worden uitgevoerd.

```PowerShell
$VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
```

### <a name="create-a-credential-object-to-hold-the-name-and-password-for-the-local-administrator-credentials"></a>Maken van een referentieobject voor het opslaan van de naam en het wachtwoord voor de lokale administrator-referenties
Voordat we de eigenschappen van het besturingssysteem voor de virtuele machine instellen kunt, moet de referenties voor het lokale administrator-account als een beveiligde tekenreeks opgeven. U kunt dit doen we gebruiken de [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.

De volgende cmdlet uitvoeren en typ de naam en het wachtwoord moet worden gebruikt voor het lokale administrator-account in de virtuele machine van Windows in het venster Windows PowerShell-referentie.

```PowerShell
$Credential = Get-Credential -Message "Type the name and password of the local administrator account."
```

### <a name="set-the-operating-system-properties-for-the-virtual-machine"></a>Stel de eigenschappen van het besturingssysteem voor de virtuele machine
We kunnen nu klaar voor het instellen van eigenschappen van het besturingssysteem van de virtuele machine. We gebruiken de [Set AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) cmdlet in te stellen van het type besturingssysteem als Windows, vereisen de [agent van de virtuele machine](../classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) om te worden geïnstalleerd, opgeven dat de cmdlet mogelijk automatisch bijwerken maakt en stel de naam van de virtuele machine, de computernaam en de referentie die met behulp van de variabelen die u eerder is geïnitialiseerd.

De volgende cmdlet om in te stellen van de eigenschappen van het besturingssysteem voor uw virtuele machine worden uitgevoerd.

```PowerShell
$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate
```

### <a name="add-the-network-interface-to-the-virtual-machine"></a>Voeg de netwerkinterface op de virtuele machine
We zullen de netwerkinterface die is gemaakt vervolgens eerder toevoegen aan de virtuele machine. We gebruiken de [toevoegen AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) cmdlet om toe te voegen van de netwerkinterface met behulp van de network interface-variabele die u eerder hebt gedefinieerd.

De volgende cmdlet om in te stellen van de netwerkinterface voor uw virtuele machine worden uitgevoerd.

```PowerShell
$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
```

### <a name="set-the-blob-storage-location-for-the-disk-to-be-used-by-the-virtual-machine"></a>Stel de locatie van de blob-opslag voor de schijf moet worden gebruikt door de virtuele machine
We Stel vervolgens de blob-opslaglocatie voor de schijf moet worden gebruikt door de virtuele machine met behulp van de variabelen die u eerder hebt gedefinieerd.

De volgende cmdlet om de locatie van blob storage worden uitgevoerd.

```PowerShell
$OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
```

### <a name="set-the-operating-system-disk-properties-for-the-virtual-machine"></a>Het besturingssysteem van de eigenschappen van de schijf voor de virtuele machine instellen
Vervolgens we wordt het besturingssysteem schijfeigenschappen instellen voor de virtuele machine. We gebruiken de [Set AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) cmdlet om op te geven dat het besturingssysteem voor de virtuele machine, zijn afkomstig van een afbeelding om in te stellen om alleen-lezen (omdat SQL Server wordt geïnstalleerd op dezelfde schijf) en definieer de naam van de virtuele machine en de besturingssysteemschijf is gedefinieerd met behulp van de variabelen die we eerder hebt gedefinieerd.

De volgende cmdlet voor het instellen van het besturingssysteem schijfeigenschappen voor uw virtuele machine worden uitgevoerd.

```PowerShell
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage
```

### <a name="specify-the-platform-image-for-the-virtual-machine"></a>Geef de platforminstallatiekopie voor de virtuele machine
Onze laatste stap in de configuratie is de platform-afbeelding voor de virtuele machine opgeven. In deze zelfstudie gebruiken we de meest recente SQL Server 2016 CTP-installatiekopie. We gebruiken de [Set AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) cmdlet deze installatiekopie te gebruiken zoals gedefinieerd door de variabelen die u eerder hebt gedefinieerd.

De volgende cmdlet om op te geven van de platforminstallatiekopie voor uw virtuele machine worden uitgevoerd.

```PowerShell
$VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version
```

## <a name="create-the-sql-vm"></a>De virtuele machine van SQL maken
Nu u de configuratiestappen hebt voltooid, bent u klaar om te maken van de virtuele machine. We gebruiken de [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) cmdlet voor het maken van de virtuele machine met behulp van de variabelen die we hebt gedefinieerd.

De volgende cmdlet als u wilt maken van uw virtuele machine worden uitgevoerd.

```PowerShell
New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine
```

De virtuele machine wordt gemaakt. U ziet dat er een standaard opslagaccount voor diagnostische gegevens over opstarten is gemaakt omdat het opgegeven opslagaccount voor de schijf van de virtuele machine een premium-opslagaccount is.

U kunt nu deze machine weergeven in de Azure-Portal om te zien [het openbare IP-adres en de volledig gekwalificeerde domeinnaam](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="example-script"></a>Voorbeeldscript
Het volgende script bevat de volledige PowerShell-script voor deze zelfstudie. Er wordt vanuit gegaan dat u al ingesteld hebt voor gebruik met de Azure-abonnement de **Add-AzureRmAccount** en **Select-AzureRmSubscription** opdrachten.

```PowerShell
# Variables

## Global
$Location = "SouthCentralUS"
$ResourceGroupName = "sqlvm1"

## Storage
$StorageName = $ResourceGroupName + "storage"
$StorageSku = "Premium_LRS"

## Network
$InterfaceName = $ResourceGroupName + "ServerInterface"
$VNetName = $ResourceGroupName + "VNet"
$SubnetName = "Default"
$VNetAddressPrefix = "10.0.0.0/16"
$VNetSubnetAddressPrefix = "10.0.0.0/24"
$TCPIPAllocationMethod = "Dynamic"
$DomainName = "sqlvm1"

##Compute
$VMName = $ResourceGroupName + "VM"
$ComputerName = $ResourceGroupName + "Server"
$VMSize = "Standard_DS13"
$OSDiskName = $VMName + "OSDisk"

##Image
$PublisherName = "MicrosoftSQLServer"
$OfferName = "SQL2016-WS2016"
$Sku = "Enterprise"
$Version = "latest"

# Resource Group
New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

# Storage
$StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

# Network
$SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
$VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
$PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
$Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

# Compute
$VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
$Credential = Get-Credential -Message "Type the name and password of the local administrator account."
$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
$OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

# Image
$VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

## Create the VM in Azure
New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine
```

## <a name="next-steps"></a>Volgende stappen
Nadat de virtuele machine is gemaakt, bent u klaar om te verbinden met de virtuele machine met RDP en setup connectiviteit. Zie voor meer informatie [Connect op een virtuele Machine in Azure (Resource Manager) van SQL Server](virtual-machines-windows-sql-connect.md).
