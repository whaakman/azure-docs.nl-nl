---
title: Inrichting-handleiding voor SQL Server-VM's met Azure PowerShell | Microsoft Docs
description: Bevat de stappen en PowerShell-opdrachten voor het maken van een Azure-VM met SQL Server-installatiekopieën voor virtuele machines galerie.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-resource-manager
ms.assetid: 98d50dd8-48ad-444f-9031-5378d8270d7b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 12/21/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 321f6fc043147c0a1559c93fb5ada3d33d9e3dd9
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54359966"
---
# <a name="how-to-provision-sql-server-virtual-machines-with-azure-powershell"></a>Over het inrichten van SQL Server-machines met Azure PowerShell

Deze handleiding beschrijft uw opties voor het maken van SQL Server-VM's van Windows met Azure PowerShell. Zie voor een gestroomlijnde Azure PowerShell-voorbeeld met meer standaardwaarden, de [SQL VM Azure PowerShell-snelstartgids](quickstart-sql-vm-create-powershell.md).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

In dit artikel is moduleversie 3.6 of hoger van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

## <a name="configure-your-subscription"></a>Uw abonnement configureren

1. Open PowerShell en zorg dat u toegang hebt tot uw Azure-account door de opdracht **Connect-AzureRmAccount** uit te voeren.

   ```PowerShell
   Connect-AzureRmAccount
   ```

1. U ziet een scherm uw referenties in te voeren. Gebruik hetzelfde e-mailadres en wachtwoord waarmee u zich aanmeldt bij Azure Portal.

## <a name="define-image-variables"></a>Image-variabelen definiëren
Waarden opnieuw gebruiken en vereenvoudig het maken van script, beginnen met het definiëren van een aantal variabelen. De parameterwaarden wijzigen als u wilt, maar houd rekening met beperkingen met betrekking tot de naam van de lengte en speciale tekens bevatten bij het wijzigen van de waarden geleverd naming.

### <a name="location-and-resource-group"></a>Locatie en resourcegroep
Definieer het gegevensgebied en de resourcegroep waarin u de andere VM-resources maken.

Als u wilt en voer vervolgens deze cmdlets voor het initialiseren van deze variabelen wijzigen.

```PowerShell
$Location = "SouthCentralUS"
$ResourceGroupName = "sqlvm2"
```

### <a name="storage-properties"></a>Eigenschappen van opslag
Definieer de storage-account en het type opslag moet worden gebruikt door de virtuele machine.

Als u wilt en voer de volgende cmdlet voor het initialiseren van deze variabelen wijzigen. Wordt u aangeraden [Premium Storage](../premium-storage.md) voor productieworkloads.

```PowerShell
$StorageName = $ResourceGroupName + "storage"
$StorageSku = "Premium_LRS"
```

### <a name="network-properties"></a>Netwerkeigenschappen
Definieert de eigenschappen kunnen worden gebruikt door het netwerk in de virtuele machine. 

- Netwerkinterface
- TCP/IP-toewijzingsmethode
- Naam van virtueel netwerk
- De naam van het virtuele subnet
- Bereik van IP-adressen voor het virtuele netwerk
- Bereik van IP-adressen voor het subnet
- Openbare domeinnaamlabel

Als u wilt en voer deze cmdlet voor het initialiseren van deze variabelen wijzigen.

```PowerShell
$InterfaceName = $ResourceGroupName + "ServerInterface"
$NsgName = $ResourceGroupName + "nsg"
$TCPIPAllocationMethod = "Dynamic"
$VNetName = $ResourceGroupName + "VNet"
$SubnetName = "Default"
$VNetAddressPrefix = "10.0.0.0/16"
$VNetSubnetAddressPrefix = "10.0.0.0/24"
$DomainName = $ResourceGroupName
```

### <a name="virtual-machine-properties"></a>Eigenschappen van de virtuele machine
De naam van de virtuele machine, de computernaam, de grootte van de virtuele machine en de naam van de besturingssysteem-schijf voor de virtuele machine definiëren.

Als u wilt en voer deze cmdlet voor het initialiseren van deze variabelen wijzigen.

```PowerShell
$VMName = $ResourceGroupName + "VM"
$ComputerName = $ResourceGroupName + "Server"
$VMSize = "Standard_DS13"
$OSDiskName = $VMName + "OSDisk"
```

### <a name="choose-a-sql-server-image"></a>Kies een installatiekopie van SQL Server

De volgende variabelen gebruiken voor het definiëren van de SQL Server-installatiekopie moet worden gebruikt voor de virtuele machine. 

1. Eerst een lijst met alle van de SQL Server-installatiekopie-aanbiedingen met de `Get-AzureRmVMImageOffer` opdracht. Met deze opdracht worden de huidige installatiekopieën die beschikbaar zijn in de Azure-Portal en ook oudere installatiekopieën die kunnen alleen worden geïnstalleerd met PowerShell:

   ```PowerShell
   Get-AzureRmVMImageOffer -Location $Location -Publisher 'MicrosoftSQLServer'
   ```

1. Voor deze zelfstudie, moet u de volgende variabelen gebruiken om op te geven van SQL Server 2017 op Windows Server 2016.

   ```PowerShell
   $OfferName = "SQL2017-WS2016"
   $PublisherName = "MicrosoftSQLServer"
   $Version = "latest"
   ```

1. De beschikbare versies voor uw aanbod vervolgens een lijst.

   ```PowerShell
   Get-AzureRmVMImageSku -Location $Location -Publisher 'MicrosoftSQLServer' -Offer $OfferName | Select Skus
   ```

1. Voor deze zelfstudie gebruikt u de SQL Server 2017 Developer edition (**SQLDEV**). De Developer edition wordt naar eigen inzicht in licentie gegeven voor het testen en ontwikkeling en u betaalt alleen voor de kosten van het uitvoeren van de virtuele machine.

   ```PowerShell
   $Sku = "SQLDEV"
   ```

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Met het Resource Manager-implementatiemodel is het eerste object dat u maakt de resourcegroep. Gebruik de [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet voor het maken van een Azure-resourcegroep en de bijbehorende resources. Geef de variabelen die u eerder voor de naam van de resourcegroep en de locatie is geïnitialiseerd.

Voer deze cmdlet om uw nieuwe resourcegroep te maken.

```PowerShell
New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location
```

## <a name="create-a-storage-account"></a>Create a storage account
De virtuele machine vereist een storage-resources voor de besturingssysteemschijf en voor de SQL Server-gegevens en logboekbestanden bestanden. Voor het gemak maakt u één schijf voor beide. U kunt extra schijven toevoegen met behulp van de [toevoegen-Azure-schijf](/powershell/module/servicemanagement/azure/add-azuredisk) cmdlet voor het plaatsen van uw SQL Server-gegevens en logboekbestanden bestanden op toegewezen schijven. Gebruik de [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet voor het maken van een standard storage-account in uw nieuwe resourcegroep. Geef de variabelen die u eerder voor de storage-accountnaam, de opslag-Sku-naam en de locatie is geïnitialiseerd.

Voer deze cmdlet om uw nieuwe opslagaccount te maken.

```PowerShell
$StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName `
   -Name $StorageName -SkuName $StorageSku `
   -Kind "Storage" -Location $Location
```

> [!TIP]
> Het maken van het opslagaccount kan een paar minuten duren.

## <a name="create-network-resources"></a>Netwerkbronnen maken
De virtuele machine vereist een aantal netwerkbronnen voor verbinding met het netwerk.

* Elke virtuele machine vereist een virtueel netwerk.
* Een virtueel netwerk moet ten minste één subnet dat is gedefinieerd.
* Een netwerkinterface moet worden gedefinieerd met een openbare of particuliere IP-adres.

### <a name="create-a-virtual-network-subnet-configuration"></a>Maak een subnetconfiguratie virtueel netwerk
Beginnen met het maken van een subnetconfiguratie voor het virtuele netwerk. Voor deze zelfstudie maakt u een standaard-subnet op met de [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) cmdlet. Geef de variabelen die u eerder voor de naam en adres subnetvoorvoegsel is geïnitialiseerd.

> [!NOTE]
> U kunt aanvullende eigenschappen van de configuratie voor het subnet van virtueel netwerk met behulp van deze cmdlet definiëren, maar dat valt buiten het bereik van deze zelfstudie.

Deze cmdlet voor het maken van uw virtuele subnet-configuratie uitvoeren.

```PowerShell
$SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
```

### <a name="create-a-virtual-network"></a>Een virtueel netwerk maken
Maak vervolgens uw virtuele netwerk in uw nieuwe resource groep met de [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) cmdlet. Geef de variabelen die u eerder voor de naam, locatie en adresvoorvoegsel is geïnitialiseerd. De subnetconfiguratie van het dat u hebt gedefinieerd in de vorige stap gebruiken.

Deze cmdlet voor het maken van het virtuele netwerk uitvoeren.

```PowerShell
$VNet = New-AzureRmVirtualNetwork -Name $VNetName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
```

### <a name="create-the-public-ip-address"></a>Het openbare IP-adres maken
Nu dat uw virtuele netwerk is gedefinieerd, moet u een IP-adres voor de verbinding met de virtuele machine configureren. Voor deze zelfstudie maakt u een openbaar IP-adres met behulp van dynamische IP-adressering ter ondersteuning van verbinding met Internet. Gebruik de [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) cmdlet voor het maken van het openbare IP-adres in uw nieuwe resourcegroep. Geef de variabelen die u eerder voor de naam, locatie, toewijzingsmethode en DNS-domeinnaamlabel is geïnitialiseerd.

> [!NOTE]
> U kunt aanvullende eigenschappen van het openbare IP-adres met behulp van deze cmdlet definiëren, maar dat valt buiten het bereik van deze eerste zelfstudie. U kunt ook een privé-adres of een adres maken met een statisch adres, maar die ook valt buiten het bereik van deze zelfstudie.

Voer deze cmdlet voor het maken van uw openbare IP-adres.

```PowerShell
$PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
```

### <a name="create-the-network-security-group"></a>De netwerkbeveiligingsgroep maken
De virtuele machine en SQL Server om verkeer te beveiligen, moet u een netwerkbeveiligingsgroep maken.

1. Maak eerst een regel voor de netwerkbeveiligingsgroep voor de RDP-verbinding toestaan van verbindingen met extern bureaublad.

   ```PowerShell
   $NsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name "RDPRule" -Protocol Tcp `
      -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 3389 -Access Allow
   ```
1. Configureer een netwerkbeveiligingsgroepregel waarmee verkeer op TCP-poort 1433. In dat geval kunt verbindingen met SQL Server via internet.

   ```PowerShell
   $NsgRuleSQL = New-AzureRmNetworkSecurityRuleConfig -Name "MSSQLRule"  -Protocol Tcp `
      -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 1433 -Access Allow
   ```

1. De netwerkbeveiligingsgroep maken.

   ```PowerShell
   $Nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $ResourceGroupName `
      -Location $Location -Name $NsgName `
      -SecurityRules $NsgRuleRDP,$NsgRuleSQL
   ```

### <a name="create-the-network-interface"></a>De netwerkinterface maken
U bent nu klaar om te maken van de netwerkinterface voor uw virtuele machine. Gebruik de [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) cmdlet voor het maken van de netwerkinterface in uw nieuwe resourcegroep. Geef de naam, locatie, subnet en openbaar IP-adres die eerder zijn gedefinieerd.

Voer deze cmdlet voor het maken van de netwerkinterface.

```PowerShell
$Interface = New-AzureRmNetworkInterface -Name $InterfaceName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id `
   -NetworkSecurityGroupId $Nsg.Id
```

## <a name="configure-a-vm-object"></a>Een VM-object configureren
Nu dat de opslag- en netwerkresources zijn gedefinieerd, u kunt voor het definiëren van rekenresources voor de virtuele machine.

- Geef de grootte van de virtuele machine en de verschillende eigenschappen van het besturingssysteem.
- Geef op de netwerkinterface die u eerder hebt gemaakt.
- Definieer de blob-opslag.
- Geef op de besturingssysteemschijf.

### <a name="create-the-vm-object"></a>De VM-object maken
Gestart door de grootte van de virtuele machine op te geven. Geef een DS13 voor deze zelfstudie. Gebruik de [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) cmdlet om een object configureerbare virtuele machine te maken. Geef de variabelen die u eerder voor de naam en de grootte is geïnitialiseerd.

Deze cmdlet voor het maken van het virtuele-machineobject uitvoeren.

```PowerShell
$VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
```

### <a name="create-a-credential-object-to-hold-the-name-and-password-for-the-local-administrator-credentials"></a>Maak een referentieobject voor het opslaan van de naam en het wachtwoord voor de lokale administrator-referenties
Voordat u de eigenschappen van het besturingssysteem voor de virtuele machine instellen kunt, moet u de referenties voor het lokale administrator-account als een beveiligde tekenreeks opgeven. U doet dit door gebruik van de [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.

De volgende cmdlet uitvoeren en typ in het venster van de aanvraag in de PowerShell-referentie, de naam en het wachtwoord moet worden gebruikt voor het lokale administrator-account in de virtuele machine.

```PowerShell
$Credential = Get-Credential -Message "Type the name and password of the local administrator account."
```

### <a name="set-the-operating-system-properties-for-the-virtual-machine"></a>Stel de eigenschappen van het besturingssysteem voor de virtuele machine
Nu bent u klaar om in te stellen van de eigenschappen van het besturingssysteem van de virtuele machine met de [Set AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) cmdlet.

- Het type besturingssysteem instellen als Windows.
- Vereisen dat de [virtuele-machineagent](../../extensions/agent-windows.md) moet worden geïnstalleerd.
- Geef op dat de cmdlet schakelt u automatisch bijwerken.
- Geef de variabelen die u eerder voor de naam van de virtuele machine, de computernaam en de referentie is geïnitialiseerd.

Deze cmdlet om in te stellen van de eigenschappen van het besturingssysteem voor uw virtuele machine uitvoeren.

```PowerShell
$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine `
   -Windows -ComputerName $ComputerName -Credential $Credential `
   -ProvisionVMAgent -EnableAutoUpdate
```

### <a name="add-the-network-interface-to-the-virtual-machine"></a>De netwerkinterface toevoegen aan de virtuele machine
Gebruik vervolgens de [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) cmdlet om toe te voegen van de netwerkinterface met behulp van de variabele die u eerder hebt gedefinieerd.

Deze cmdlet om in te stellen van de netwerkinterface voor uw virtuele machine uitvoeren.

```PowerShell
$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
```

### <a name="set-the-blob-storage-location-for-the-disk-to-be-used-by-the-virtual-machine"></a>Stel de locatie in blob storage voor de schijf moet worden gebruikt door de virtuele machine
Vervolgens stelt u de opslaglocatie voor de blob voor de VM schijf met behulp van de variabelen die u eerder hebt gedefinieerd.

Deze cmdlet om in te stellen van de locatie van de blob-opslag uitvoeren.

```PowerShell
$OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
```

### <a name="set-the-operating-system-disk-properties-for-the-virtual-machine"></a>Eigenschappen van de schijf voor de virtuele machine van het besturingssysteem instellen
Het besturingssysteem vervolgens stelt de eigenschappen van de schijf voor de virtuele machine met de [Set-azurermvmosdisk,](/powershell/module/azurerm.compute/set-azurermvmosdisk) cmdlet. 

- Geef op dat het besturingssysteem voor de virtuele machine, zijn afkomstig van een installatiekopie.
- Cache voor lezen alleen (omdat SQL Server wordt geïnstalleerd op dezelfde schijf) instellen.
- Geef de variabelen die u eerder voor de VM-naam en de besturingssysteemschijf is geïnitialiseerd.

Deze cmdlet om in te stellen van het besturingssysteem schijfeigenschappen voor uw virtuele machine uitvoeren.

```PowerShell
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name `
   $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage
```

### <a name="specify-the-platform-image-for-the-virtual-machine"></a>Geef de platform-installatiekopie voor de virtuele machine
De laatste configuratiestap is om op te geven van de platform-installatiekopie voor uw virtuele machine. Voor deze zelfstudie gebruikt u de meest recente SQL Server 2016 CTP-installatiekopie. Gebruik de [Set AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) cmdlet voor het gebruik van deze installatiekopie met de variabelen die u eerder hebt gedefinieerd.

Deze cmdlet om op te geven van de platform-installatiekopie voor uw virtuele machine uitvoeren.

```PowerShell
$VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine `
   -PublisherName $PublisherName -Offer $OfferName `
   -Skus $Sku -Version $Version
```

## <a name="create-the-sql-vm"></a>De SQL-VM maken
Nu dat u de configuratiestappen hebt voltooid, bent u klaar om te maken van de virtuele machine. Gebruik de [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) cmdlet voor het maken van de virtuele machine met behulp van de variabelen die u hebt gedefinieerd.

> [!TIP]
> Het maken van de virtuele machine kan een paar minuten duren.

Deze cmdlet voor het maken van uw virtuele machine uitvoeren.

```PowerShell
New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine
```

De virtuele machine wordt gemaakt.

> [!NOTE]
> Als er een fout over diagnostische gegevens over opstarten, kunt u deze negeren. Een standard storage-account is gemaakt voor diagnostische gegevens over opstarten omdat het opgegeven opslagaccount voor de virtuele machine-schijf een premium storage-account is.

## <a name="install-the-sql-iaas-agent"></a>SQL IaaS-agent installeren
Virtuele machines met SQL Server ondersteuning voor geautomatiseerde functies met de [SQL Server IaaS Agent-extensie](virtual-machines-windows-sql-server-agent-extension.md). Voor het installeren van de agent op de nieuwe virtuele machine, moet u de volgende opdracht uitvoeren nadat deze gemaakt.


   ```PowerShell
   Set-AzureRmVMSqlServerExtension -ResourceGroupName $ResourceGroupName -VMName $VMName -name "SQLIaasExtension" -version "1.2" -Location $Location
   ```

## <a name="stop-or-remove-a-vm"></a>Stoppen of verwijderen van een virtuele machine

Als u de virtuele machine continu worden uitgevoerd hoeft, kunt u onnodige kosten voorkomen door te stoppen wanneer deze niet in gebruik. Met de volgende opdracht wordt de VM gestopt, maar blijft deze beschikbaar voor toekomstig gebruik.

```PowerShell
Stop-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
```

U kunt ook alle resources die aan de virtuele machine zijn gekoppeld, definitief verwijderen met de opdracht **Remove-AzureRmResourceGroup**. Gebruik deze opdracht doet dus permanent worden verwijderd van de virtuele machine, dus zorgvuldig.

## <a name="example-script"></a>Voorbeeldscript
Het volgende script bevat het volledige PowerShell-script voor deze zelfstudie. Hierbij wordt ervan uitgegaan dat u hebt al ingesteld het Azure-abonnement voor gebruik met de **Connect-AzureRmAccount** en **Select-AzureRmSubscription** opdrachten.

```PowerShell
# Variables

## Global
$Location = "SouthCentralUS"
$ResourceGroupName = "sqlvm2"

## Storage
$StorageName = $ResourceGroupName + "storage"
$StorageSku = "Premium_LRS"

## Network
$InterfaceName = $ResourceGroupName + "ServerInterface"
$NsgName = $ResourceGroupName + "nsg"
$VNetName = $ResourceGroupName + "VNet"
$SubnetName = "Default"
$VNetAddressPrefix = "10.0.0.0/16"
$VNetSubnetAddressPrefix = "10.0.0.0/24"
$TCPIPAllocationMethod = "Dynamic"
$DomainName = $ResourceGroupName

##Compute
$VMName = $ResourceGroupName + "VM"
$ComputerName = $ResourceGroupName + "Server"
$VMSize = "Standard_DS13"
$OSDiskName = $VMName + "OSDisk"

##Image
$PublisherName = "MicrosoftSQLServer"
$OfferName = "SQL2017-WS2016"
$Sku = "SQLDEV"
$Version = "latest"

# Resource Group
New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

# Storage
$StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

# Network
$SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
$VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
$PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
$NsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name "RDPRule" -Protocol Tcp -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389 -Access Allow
$NsgRuleSQL = New-AzureRmNetworkSecurityRuleConfig -Name "MSSQLRule"  -Protocol Tcp -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 1433 -Access Allow
$Nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $ResourceGroupName -Location $Location -Name $NsgName -SecurityRules $NsgRuleRDP,$NsgRuleSQL
$Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id -NetworkSecurityGroupId $Nsg.Id

# Compute
$VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
$Credential = Get-Credential -Message "Type the name and password of the local administrator account."
$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
$OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

# Image
$VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

# Create the VM in Azure
New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

# Add the SQL IaaS Extension
Set-AzureRmVMSqlServerExtension -ResourceGroupName $ResourceGroupName -VMName $VMName -name "SQLIaasExtension" -version "1.2" -Location $Location
```

## <a name="next-steps"></a>Volgende stappen
Nadat de virtuele machine is gemaakt, kunt u het volgende doen:

- Verbinding maken met de virtuele machine met RDP.
- SQL Server-instellingen configureren in de portal voor uw virtuele machine, met inbegrip van:
   - [Opslaginstellingen](virtual-machines-windows-sql-server-storage-configuration.md) 
   - [Geautomatiseerde beheertaken](virtual-machines-windows-sql-server-agent-extension.md)
- [Connectiviteit configureren](virtual-machines-windows-sql-connect.md)
- Verbinding maken met de clients en toepassingen naar het nieuwe SQL Server-exemplaar

