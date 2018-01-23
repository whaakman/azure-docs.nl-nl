---
title: Het maken van SQL Server-VM's met Azure PowerShell | Microsoft Docs
description: "Bevat de stappen en PowerShell-opdrachten voor het maken van een virtuele machine in Azure met SQL Server-installatiekopieën voor virtuele machine-galerie."
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
ms.date: 11/29/2017
ms.author: jroth
ms.openlocfilehash: e6d1f36d998ac8726e3a74b31772a5dd5a24bd58
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="how-to-create-sql-server-virtual-machines-with-azure-powershell"></a>Het maken van virtuele machines van SQL Server met Azure PowerShell

Deze handleiding beschrijft de mogelijkheden voor het maken van SQL Server-VM's van Windows met Azure PowerShell. Zie voor een gestroomlijnde Azure PowerShell-voorbeeld met meer standaardwaarden, de [SQL VM Azure PowerShell Quick Start](quickstart-sql-vm-create-powershell.md).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

Voor deze snelstartgids is moduleversie 3.6 of later van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

## <a name="configure-your-subscription"></a>Configureer uw abonnement

1. Open PowerShell en zorg dat u toegang hebt tot uw Azure-account door de opdracht **Add-AzureRmAccount** uit te voeren.

   ```PowerShell
   Add-AzureRmAccount
   ```

1. Als het goed is, ziet u nu een aanmeldingsscherm waar u uw referenties kunt invoeren. Gebruik hetzelfde e-mailadres en wachtwoord waarmee u zich aanmeldt bij Azure Portal.

## <a name="define-image-variables"></a>Definieer de installatiekopie van variabelen
Om te vereenvoudigen hergebruik en script aangemaakte, starten door een aantal variabelen definiëren. Wijzig de parameterwaarden zoals u dat wilt, maar pas op voor de naamgeving van beperkingen met betrekking tot de naam van lengten en speciale tekens bij het wijzigen van de opgegeven waarden.

### <a name="location-and-resource-group"></a>Locatie en resourcegroep
Twee variabelen gebruiken voor het definiëren van het gegevensgebied en de resourcegroep waarin u de andere bronnen voor de virtuele machine maken.

Naar wens wijzigen en vervolgens de volgende cmdlets voor het initialiseren van deze variabelen wordt uitgevoerd.

```PowerShell
$Location = "SouthCentralUS"
$ResourceGroupName = "sqlvm2"
```

### <a name="storage-properties"></a>Eigenschappen van opslag
De volgende variabelen gebruiken voor het definiëren van de storage-account en het type opslag moet worden gebruikt door de virtuele machine.

Naar wens wijzigen en vervolgens de volgende cmdlet om te initialiseren van deze variabelen wordt uitgevoerd. Houd er rekening mee dat in dit voorbeeld we gebruiken [Premium-opslag](../premium-storage.md), wat wordt aanbevolen voor productieworkloads.

```PowerShell
$StorageName = $ResourceGroupName + "storage"
$StorageSku = "Premium_LRS"
```

### <a name="network-properties"></a>Eigenschappen van het netwerk
De volgende variabelen gebruiken voor het definiëren van de netwerkinterface, de TCP/IP-toewijzingsmethode, naam van het virtuele netwerk, de naam van de virtuele subnet, het bereik van IP-adressen voor het virtuele netwerk, het bereik van IP-adressen voor het subnet en het openbare domein-naamlabel moet worden gebruikt door het netwerk in de virtuele machine.

Naar wens wijzigen en vervolgens de volgende cmdlet om te initialiseren van deze variabelen wordt uitgevoerd.

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

### <a name="virtual-machine-properties"></a>Eigenschappen van virtuele machine
De volgende variabelen gebruiken voor het definiëren van de naam van de virtuele machine, de computernaam, de grootte van de virtuele machine en de naam van de besturingssysteem-schijf voor de virtuele machine.

Naar wens wijzigen en vervolgens de volgende cmdlet om te initialiseren van deze variabelen wordt uitgevoerd.

```PowerShell
$VMName = $ResourceGroupName + "VM"
$ComputerName = $ResourceGroupName + "Server"
$VMSize = "Standard_DS13"
$OSDiskName = $VMName + "OSDisk"
```

### <a name="choose-a-sql-server-image"></a>Kies een installatiekopie van SQL Server
De volgende variabelen gebruiken voor het definiëren van de installatiekopie van SQL Server moet worden gebruikt voor de virtuele machine.

1. Eerst een lijst met alle van de aanbiedingen van de installatiekopie van SQL Server met de **Get-AzureRmVMImageOffer** opdracht:

   ```PowerShell
   Get-AzureRmVMImageOffer -Location $Location -Publisher 'MicrosoftSQLServer'
   ```

1. Voor deze zelfstudie, moet u de volgende variabelen gebruiken om op te geven van SQL Server 2017 op Windows Server 2016.

   ```PowerShell
   $OfferName = "SQL2017-WS2016"
   $PublisherName = "MicrosoftSQLServer"
   $Version = "latest"
   ```

1. Lijst van de volgende, uit de beschikbare edities voor uw aanbieding.

   ```PowerShell
   Get-AzureRmVMImageSku -Location $Location -Publisher 'MicrosoftSQLServer' -Offer $OfferName | Select Skus
   ```

1. Voor deze zelfstudie gebruikt u de ontwikkelaarsversie van SQL Server 2017 (**SQLDEV**). De ontwikkelaarsversie vrijelijk is gelicentieerd voor het testen en ontwikkeling en u betaalt alleen voor de kosten van het uitvoeren van de virtuele machine.

   ```PowerShell
   $Sku = "SQLDEV"
   ```

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Met het Resource Manager-implementatiemodel is het eerste object die u maakt de resourcegroep. Gebruik de [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet voor het maken van een Azure-resourcegroep en de bijbehorende resources met de naam van een resourcegroep en locatie die is gedefinieerd door de variabelen die u eerder is geïnitialiseerd.

Voer de volgende cmdlet als u wilt maken van uw nieuwe resourcegroep.

```PowerShell
New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location
```

## <a name="create-a-storage-account"></a>Een opslagaccount maken
De virtuele machine vereist storage-resources voor de besturingssysteemschijf en voor de SQL Server-gegevens en logboekbestanden. Voor de eenvoud maken we één schijf voor beide. U kunt extra schijven koppelen later via de [toevoegen Azure-schijf](/powershell/module/azure/add-azuredisk) cmdlet om te kunnen plaatsen van uw SQL Server-gegevens en logboekbestanden op schijven met specifieke bestanden. Gebruik de [nieuw AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet een standard-opslag-account maken in uw nieuwe resourcegroep en met de naam van het opslagaccount, opslag Sku-naam en locatie die is gedefinieerd met behulp van de variabelen die u eerder is geïnitialiseerd .

De volgende cmdlet als u wilt maken van uw nieuwe opslagaccount worden uitgevoerd.

```PowerShell
$StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName `
   -Name $StorageName -SkuName $StorageSku `
   -Kind "Storage" -Location $Location
```

> [!TIP]
> Maken van het opslagaccount kan een paar minuten duren.

## <a name="create-network-resources"></a>Maken van netwerkbronnen
De virtuele machine vereist een aantal netwerkbronnen voor verbinding met het netwerk.

* Elke virtuele machine vereist een virtueel netwerk.
* Een virtueel netwerk moet ten minste één subnet dat is gedefinieerd.
* Een netwerkinterface moet worden gedefinieerd met een openbare of een particulier IP-adres.

### <a name="create-a-virtual-network-subnet-configuration"></a>Een virtueel netwerk subnetconfiguratie maken
Begint met het maken van de subnetconfiguratie van een voor het virtuele netwerk. In deze zelfstudie maken we een standaard subnet met de [nieuw AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) cmdlet. We maken onze subnetconfiguratie virtueel netwerk met de naam en adres subnetvoorvoegsel gedefinieerd met behulp van de variabelen die u eerder is geïnitialiseerd.

> [!NOTE]
> U kunt aanvullende eigenschappen van de configuratie van het virtuele netwerk subnet met deze cmdlet definiëren, maar dat is buiten het bereik van deze zelfstudie.

De volgende cmdlet als u wilt maken van uw virtuele subnet-configuratie worden uitgevoerd.

```PowerShell
$SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
```

### <a name="create-a-virtual-network"></a>Een virtueel netwerk maken
Maak vervolgens uw virtuele netwerk met de [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) cmdlet. Het virtuele netwerk in uw nieuwe resourcegroep maken, met de naam, de locatie en het adres voorvoegsel gedefinieerd met behulp van de variabelen die u eerder is geïnitialiseerd en de subnet-configuratie die u hebt gedefinieerd in de vorige stap.

De volgende cmdlet als u wilt maken van het virtuele netwerk worden uitgevoerd.

```PowerShell
$VNet = New-AzureRmVirtualNetwork -Name $VNetName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
```

### <a name="create-the-public-ip-address"></a>Het openbare IP-adres maken
Nu we hebben onze virtueel netwerk is gedefinieerd, moet er een IP-adres configureren voor verbinding met de virtuele machine. Voor deze zelfstudie maken we een openbaar IP-adres met behulp van dynamische IP-adressen ter ondersteuning van verbinding met Internet. Gebruik de [nieuw AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) cmdlet voor het maken van het openbare IP-adres adres in de resourcegroep die eerder is gemaakt en met de naam, locatie, toewijzingsmethode en DNS-domeinnaamlabel gedefinieerd met behulp van de variabelen die u hebt eerder is geïnitialiseerd.

> [!NOTE]
> U kunt aanvullende eigenschappen van het openbare IP-adres met behulp van deze cmdlet definiëren, maar dat is buiten het bereik van deze eerste zelfstudie. U kunt ook een persoonlijk adres of een adres maken met een statisch adres, maar die ook is buiten het bereik van deze zelfstudie.

Voer de volgende cmdlet als u wilt maken van uw openbare IP-adres.

```PowerShell
$PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
```

### <a name="create-the-network-security-group"></a>De netwerkbeveiligingsgroep maken
De virtuele machine en SQL Server om verkeer te beveiligen, maak een netwerkbeveiligingsgroep.

1. Maak eerst een groep van de netwerkbeveiligingsregel voor RDP waarmee verbindingen met extern bureaublad.

   ```PowerShell
   $NsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name "RDPRule" -Protocol Tcp `
      -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 3389 -Access Allow
   ```
1. Configureer een groep netwerkbeveiligingsregel waarmee verkeer op TCP-poort 1433. Hierdoor kunnen verbindingen met SQL Server via internet.

   ```PowerShell
   $NsgRuleSQL = New-AzureRmNetworkSecurityRuleConfig -Name "MSSQLRule"  -Protocol Tcp `
      -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 1433 -Access Allow
   ```

1. Vervolgens maken de netwerkbeveiligingsgroep.

   ```PowerShell
   $Nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $ResourceGroupName `
      -Location $Location -Name $NsgName `
      -SecurityRules $NsgRuleRDP,$NsgRuleSQL
   ```

### <a name="create-the-network-interface"></a>De netwerkinterface maken
Er zijn nu gereed voor het maken van de netwerkinterface die de virtuele machine wordt gebruikt. We noemen de [nieuw AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) cmdlet onze netwerkinterface maken in de resourcegroep die eerder hebt gemaakt en met de naam, locatie, subnet en openbaar IP-adres dat eerder is gedefinieerd.

De volgende cmdlet als u wilt maken van de netwerkinterface worden uitgevoerd.

```PowerShell
$Interface = New-AzureRmNetworkInterface -Name $InterfaceName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id `
   -NetworkSecurityGroupId $Nsg.Id
```

## <a name="configure-a-vm-object"></a>Een VM-object configureren
Nu dat we hebben een opslag- en netwerkbronnen die zijn gedefinieerd, zijn er klaar voor het definiëren van rekenresources voor de virtuele machine. In deze zelfstudie wij Geef de grootte van de virtuele machine en de verschillende eigenschappen van het besturingssysteem, de netwerkinterface die we eerder hebt gemaakt, blob-opslag te definiëren en geef vervolgens de schijf van het besturingssysteem opgeven.

### <a name="create-the-vm-object"></a>Het VM-object maken
Starten door op te geven van de grootte van de virtuele machine. We geven een DS13 voor deze zelfstudie. We noemen de [nieuw AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) cmdlet geen object configureerbare virtuele machine maken met de naam en de grootte die is gedefinieerd met behulp van de variabelen die u eerder is geïnitialiseerd.

De volgende cmdlet als u wilt maken van het object van de virtuele machine worden uitgevoerd.

```PowerShell
$VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
```

### <a name="create-a-credential-object-to-hold-the-name-and-password-for-the-local-administrator-credentials"></a>Maken van een referentieobject voor het opslaan van de naam en het wachtwoord voor de lokale administrator-referenties
Voordat we de eigenschappen van het besturingssysteem voor de virtuele machine instellen kunt, moet de referenties voor het lokale administrator-account als een beveiligde tekenreeks opgeven. Gebruik hiervoor de [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.

De volgende cmdlet uitvoeren en typ de naam en het wachtwoord moet worden gebruikt voor het lokale administrator-account in de virtuele machine in het venster PowerShell-referentie-aanvraag.

```PowerShell
$Credential = Get-Credential -Message "Type the name and password of the local administrator account."
```

### <a name="set-the-operating-system-properties-for-the-virtual-machine"></a>Stel de eigenschappen van het besturingssysteem voor de virtuele machine
Nu we klaar zijn voor het instellen van eigenschappen van het besturingssysteem van de virtuele machine met [Set AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) cmdlet in te stellen van het type besturingssysteem als Windows, vereisen de [agent van de virtuele machine](../agent-user-guide.md) om te worden geïnstalleerd, geeft dat de cmdlet mogelijk automatisch bijwerken maakt en stelt de naam van de virtuele machine, de computernaam en de referentie die met behulp van de variabelen die u eerder is geïnitialiseerd.

De volgende cmdlet om in te stellen van de eigenschappen van het besturingssysteem voor uw virtuele machine worden uitgevoerd.

```PowerShell
$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine `
   -Windows -ComputerName $ComputerName -Credential $Credential `
   -ProvisionVMAgent -EnableAutoUpdate
```

### <a name="add-the-network-interface-to-the-virtual-machine"></a>Voeg de netwerkinterface op de virtuele machine
We Voeg vervolgens de netwerkinterface die we hebben gemaakt eerder aan de virtuele machine. Roep de [toevoegen AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) cmdlet om toe te voegen van de netwerkinterface met behulp van de network interface-variabele die u eerder hebt gedefinieerd.

De volgende cmdlet om in te stellen van de netwerkinterface voor uw virtuele machine worden uitgevoerd.

```PowerShell
$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
```

### <a name="set-the-blob-storage-location-for-the-disk-to-be-used-by-the-virtual-machine"></a>Stel de locatie van de blob-opslag voor de schijf moet worden gebruikt door de virtuele machine
Vervolgens stelt u de locatie van de blob-opslag voor de schijf moet worden gebruikt door de virtuele machine met behulp van de variabelen die u eerder hebt gedefinieerd.

De volgende cmdlet om de locatie van blob storage worden uitgevoerd.

```PowerShell
$OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
```

### <a name="set-the-operating-system-disk-properties-for-the-virtual-machine"></a>Het besturingssysteem van de eigenschappen van de schijf voor de virtuele machine instellen
Vervolgens stelt het besturingssysteem van de eigenschappen van de schijf voor de virtuele machine. Gebruik de [Set AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) cmdlet om op te geven dat het besturingssysteem voor de virtuele machine, zijn afkomstig van een afbeelding ingesteld om alleen-lezen (omdat SQL Server wordt geïnstalleerd op dezelfde schijf) en de virtuele machine definiëren de naam en de besturingssysteemschijf is gedefinieerd met behulp van de variabelen die we eerder hebt gedefinieerd.

De volgende cmdlet voor het instellen van het besturingssysteem schijfeigenschappen voor uw virtuele machine worden uitgevoerd.

```PowerShell
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name `
   $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage
```

### <a name="specify-the-platform-image-for-the-virtual-machine"></a>Geef de platforminstallatiekopie voor de virtuele machine
Onze laatste stap in de configuratie is de platform-afbeelding voor de virtuele machine opgeven. In deze zelfstudie gebruiken we de meest recente SQL Server 2016 CTP-installatiekopie. Gebruik de [Set AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) cmdlet deze installatiekopie te gebruiken zoals gedefinieerd door de variabelen die u eerder hebt gedefinieerd.

De volgende cmdlet om op te geven van de platforminstallatiekopie voor uw virtuele machine worden uitgevoerd.

```PowerShell
$VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine `
   -PublisherName $PublisherName -Offer $OfferName `
   -Skus $Sku -Version $Version
```

## <a name="create-the-sql-vm"></a>De SQL-VM maken
Nu u de configuratiestappen hebt voltooid, bent u klaar om te maken van de virtuele machine. Gebruik de [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) cmdlet voor het maken van de virtuele machine met behulp van de variabelen die we hebt gedefinieerd.

De volgende cmdlet als u wilt maken van uw virtuele machine worden uitgevoerd.

```PowerShell
New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine
```

De virtuele machine wordt gemaakt.

> [!NOTE]
> U kunt de fout over diagnostische gegevens voor opstarten te negeren. Standard-opslagaccount is gemaakt voor diagnostische gegevens over opstarten omdat het opgegeven opslagaccount voor de schijf van de virtuele machine een premium-opslagaccount is.

## <a name="install-the-sql-iaas-agent"></a>SQL IaaS-agent installeren
Virtuele machines van SQL Server ondersteunen geautomatiseerde beheerfuncties met de [uitbreiding voor SQL Server IaaS-Agent](virtual-machines-windows-sql-server-agent-extension.md). Als u de agent wilt installeren op de nieuwe VM, voert u de volgende opdracht uit nadat deze is gemaakt.

   ```PowerShell
   Set-AzureRmVMSqlServerExtension -ResourceGroupName $ResourceGroupName -VMName $VMName -name "SQLIaasExtension" -version "1.2" -Location $Location
   ```

## <a name="remove-a-test-vm"></a>Een test-virtuele machine verwijderen

Als het niet nodig is dat de VM continu wordt uitgevoerd, kunt u onnodige kosten voorkomen door de virtuele machine te stoppen wanneer deze niet in gebruik is. Met de volgende opdracht wordt de VM gestopt, maar blijft deze beschikbaar voor toekomstig gebruik.

```PowerShell
Stop-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
```

U kunt ook alle resources die aan de virtuele machine zijn gekoppeld, definitief verwijderen met de opdracht **Remove-AzureRmResourceGroup**. Wees voorzichtig met het gebruik van deze opdracht, want hiermee verwijdert u ook de virtuele machine zelf definitief.

## <a name="example-script"></a>Voorbeeldscript
Het volgende script bevat de volledige PowerShell-script voor deze zelfstudie. Er wordt vanuit gegaan dat u al ingesteld hebt voor gebruik met de Azure-abonnement de **Add-AzureRmAccount** en **Select-AzureRmSubscription** opdrachten.

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
Nadat de virtuele machine is gemaakt, kunt u:

- Verbinding maken met de virtuele machine via Extern bureaublad (RDP).
- SQL Server-instellingen configureren in de portal voor uw virtuele machine, met inbegrip van:
   - [Instellingen voor de opslag](virtual-machines-windows-sql-server-storage-configuration.md) 
   - [Geautomatiseerde beheertaken](virtual-machines-windows-sql-server-agent-extension.md)
- [Connectiviteit configureren](virtual-machines-windows-sql-connect.md).
- Verbinding maken met clients en toepassingen naar het nieuwe exemplaar van SQL Server.

