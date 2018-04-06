---
title: Toegang tot het netwerk beperken tot bronnen in PaaS - Azure PowerShell | Microsoft Docs
description: In dit artikel leert u hoe u en beperken van toegang tot het netwerk voor Azure-resources, zoals Azure Storage en Azure SQL Database met de service-eindpunten van een virtueel netwerk met Azure PowerShell.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want only resources in a virtual network subnet to access an Azure PaaS resource, such as an Azure Storage account.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/14/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 28c95e1333b4641e50284a869135a9608dd3242f
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="restrict-network-access-to-paas-resources-with-virtual-network-service-endpoints-using-powershell"></a>Toegang tot het netwerk beperken tot PaaS-resources met de service-eindpunten van een virtueel netwerk met behulp van PowerShell

Service-eindpunten voor virtueel netwerk kunnen u de netwerktoegang tot bepaalde bronnen Azure-service naar een virtueel netwerksubnet beperken. U kunt ook internettoegang tot de resources verwijderen. Service-eindpunten bieden rechtstreekse verbinding met uw virtuele netwerk met de ondersteunde Azure-services, zodat u kunt persoonlijke adresruimte van het virtuele netwerk gebruiken voor toegang tot de Azure-services. Verkeer dat is bestemd voor Azure-resources via de service-eindpunten altijd blijft op de Microsoft Azure-backbone-netwerk. In dit artikel leert u hoe:

* Een virtueel netwerk maken met één subnet
* Toevoegen van een subnet en een service-eindpunt inschakelen
* Een Azure-resource maken en toegang tot het netwerk laat van alleen een subnet
* Een virtuele machine (VM) implementeren voor elk subnet
* Toegang tot een resource van een subnet bevestigen
* Controleer de toegang is geweigerd tot een bron van een subnet en het internet

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Als u wilt installeren en gebruiken van PowerShell lokaal, in dit artikel is vereist voor de Azure PowerShell moduleversie 5.4.1 of hoger. Voer ` Get-Module -ListAvailable AzureRM` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Voordat u een virtueel netwerk maakt, moet u maken van een resourcegroep voor het virtuele netwerk en andere resources die zijn gemaakt in dit artikel. Maak een resourcegroep met [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup*: 

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

Maak een virtueel netwerk met [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). Het volgende voorbeeld wordt een virtueel netwerk met de naam *myVirtualNetwork* met het adresvoorvoegsel *10.0.0.0/16*.

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

Maken van een subnetconfiguratie met [nieuw AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Het volgende voorbeeld wordt een subnetconfiguratie voor een subnet met de naam *openbare*:

```azurepowershell-interactive
$subnetConfigPublic = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Public `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

Maken van het subnet in het virtuele netwerk met de configuratie van subnetten met schrijven naar het virtuele netwerk met [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork):

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="enable-a-service-endpoint"></a>Een service-eindpunt inschakelen 

U kunt de service-eindpunten die alleen voor services die ondersteuning bieden voor service-eindpunten inschakelen. Service-eindpunt diensten beschikbaar weergeven in een Azure-locatie met [Get-AzureRmVirtualNetworkAvailableEndpointService](/powershell/module/azurerm.network/get-azurermvirtualnetworkavailableendpointservice). Het volgende voorbeeld wordt een lijst met services-service-eindpunt-functionaliteit beschikbaar zijn in de *eastus* regio. De lijst met services die zijn geretourneerd zullen groeien na verloop van tijd meer Azure-services komen service-eindpunt is ingeschakeld.

```azurepowershell-interactive
Get-AzureRmVirtualNetworkAvailableEndpointService -Location eastus | Select Name
``` 

Maak een ander subnet in het virtuele netwerk. In dit voorbeeld wordt een subnet met de naam *persoonlijke* wordt gemaakt met een service-eindpunt voor *Microsoft.Storage*: 

```azurepowershell-interactive
$subnetConfigPrivate = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -VirtualNetwork $virtualNetwork `
  -ServiceEndpoint Microsoft.Storage

$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="restrict-network-access-for-a-subnet"></a>Beperken van toegang tot het netwerk voor een subnet

Maken van netwerkbeveiliging groep beveiligingsregels voor verbindingen met [nieuw AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig). De volgende regel staat uitgaande toegang tot het openbare IP-adressen toegewezen aan de Azure Storage-service: 

```azurepowershell-interactive
$rule1 = New-AzureRmNetworkSecurityRuleConfig `
  -Name Allow-Storage-All `
  -Access Allow `
  -DestinationAddressPrefix Storage `
  -DestinationPortRange * `
  -Direction Outbound `
  -Priority 100 `
  -Protocol * `
  -SourceAddressPrefix VirtualNetwork `
  -SourcePortRange *
```

De volgende regel de toegang weigert aan alle openbare IP-adressen. Deze regel, vanwege de hogere prioriteit, die toegang tot het openbare IP-adressen van Azure Storage biedt heeft voorrang op de vorige regel.

```azurepowershell-interactive
$rule2 = New-AzureRmNetworkSecurityRuleConfig `
  -Name Deny-Internet-All `
  -Access Deny `
  -DestinationAddressPrefix Internet `
  -DestinationPortRange * `
  -Direction Outbound `
  -Priority 110 `
  -Protocol * `
  -SourceAddressPrefix VirtualNetwork `
  -SourcePortRange *
```

De volgende regel staat Remote Desktop Protocol (RDP) verkeer inkomend met het subnet vanaf elke locatie. Extern bureaublad-verbindingen zijn toegestaan voor het subnet zodat u toegang tot een resource in een latere stap van het netwerk kunt bevestigen.

```azurepowershell-interactive
$rule3 = New-AzureRmNetworkSecurityRuleConfig `
  -Name Allow-RDP-All `
  -Access Allow `
  -DestinationAddressPrefix VirtualNetwork `
  -DestinationPortRange 3389 `
  -Direction Inbound `
  -Priority 120 `
  -Protocol * `
  -SourceAddressPrefix * `
  -SourcePortRange *
```

Maken van een netwerkbeveiligingsgroep met [nieuw AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup). Het volgende voorbeeld wordt een netwerkbeveiligingsgroep met de naam *myNsgPrivate*.

```azurepowershell-interactive
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myNsgPrivate `
  -SecurityRules $rule1,$rule2,$rule3
```

Koppelen van de netwerkbeveiligingsgroep voor de *persoonlijke* subnet met [Set AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) en noteert u de configuratie van subnetten aan het virtuele netwerk. Het volgende voorbeeld wordt de *myNsgPrivate* netwerkbeveiligingsgroep voor de *persoonlijke* subnet:

```azurepowershell-interactive
Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $VirtualNetwork `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -ServiceEndpoint Microsoft.Storage `
  -NetworkSecurityGroup $nsg

$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="restrict-network-access-to-a-resource"></a>Beperken van toegang tot het netwerk aan een resource

Stapsgewijze instructies voor het beperken van toegang tot het netwerk naar bronnen die zijn gemaakt via de Azure-services ingeschakeld voor service-eindpunten varieert op services. Zie de documentatie voor afzonderlijke services voor specifieke stappen voor elke service. De rest van dit artikel bevat stappen voor het beperken van toegang tot het netwerk voor een Azure Storage-account als voorbeeld.

### <a name="create-a-storage-account"></a>Create a storage account

Maken van een Azure storage-account met [nieuw AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount). Vervang `<replace-with-your-unique-storage-account-name>` met een naam die is uniek voor alle Azure locaties tussen 3 tot 24 tekens lang zijn, met behulp van alleen cijfers en kleine letters.

```azurepowershell-interactive
$storageAcctName = '<replace-with-your-unique-storage-account-name>'

New-AzureRmStorageAccount `
  -Location EastUS `
  -Name $storageAcctName `
  -ResourceGroupName myResourceGroup `
  -SkuName Standard_LRS `
  -Kind StorageV2
```

Nadat het opslagaccount is gemaakt, het ophalen van de sleutel voor het opslagaccount in een variabele met [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey):

```azurepowershell-interactive
$storageAcctKey = (Get-AzureRmStorageAccountKey `
  -ResourceGroupName myResourceGroup `
  -AccountName $storageAcctName).Value[0]
```

De sleutel wordt gebruikt voor het maken van een bestandsshare in een latere stap. Voer `$storageAcctKey` en noteer de waarde, zoals u ook moet handmatig invoeren in een latere stap als u de bestandsshare aan een station in een virtuele machine toewijst.

### <a name="create-a-file-share-in-the-storage-account"></a>Een bestandsshare in de storage-account maken

Een context maken voor uw opslagaccount en de sleutel met [nieuw AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext). De context kapselt de naam en het account opslagaccountsleutel:

```azurepowershell-interactive
$storageContext = New-AzureStorageContext $storageAcctName $storageAcctKey
```

Maken van een bestandsshare met [nieuw AzureStorageShare](/powershell/module/azure.storage/new-azurestorageshare):

$share = New-AzureStorageShare mijn-bestandsshare - Context $storageContext

### <a name="deny-all-network-access-to-a-storage-account"></a>Alle toegang tot het netwerk naar een opslagaccount weigeren

Storage-accounts accepteren standaard netwerkverbindingen van clients in een netwerk. Om te beperken van toegang tot geselecteerde netwerken, wijzigt u de standaardactie voor *weigeren* met [Update AzureRmStorageAccountNetworkRuleSet](/powershell/module/azurerm.storage/update-azurermstorageaccountnetworkruleset). Het opslagaccount is niet toegankelijk is vanaf een netwerk wanneer het netwerktoegang is geweigerd.

```azurepowershell-interactive
Update-AzureRmStorageAccountNetworkRuleSet  `
  -ResourceGroupName "myresourcegroup" `
  -Name $storageAcctName `
  -DefaultAction Deny
```

### <a name="enable-network-access-from-a-subnet"></a>Toegang tot het netwerk van een subnet inschakelen

Ophalen van het gemaakte virtuele netwerk met [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork) en vervolgens de persoonlijke subnet-object op te halen in een variabele met [Get-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig):

```azurepowershell-interactive
$privateSubnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName "myResourceGroup" `
  -Name "myVirtualNetwork" `
  | Get-AzureRmVirtualNetworkSubnetConfig `
  -Name "Private"
```

Toestaan van toegang tot het netwerk naar de storage-account van de *persoonlijke* subnet met [toevoegen AzureRmStorageAccountNetworkRule](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig).

```azurepowershell-interactive
Add-AzureRmStorageAccountNetworkRule `
  -ResourceGroupName "myresourcegroup" `
  -Name $storageAcctName `
  -VirtualNetworkResourceId $privateSubnet.Id
```

## <a name="create-virtual-machines"></a>Virtuele machines maken

Als u wilt testen toegang tot het netwerk naar een opslagaccount, moet u een virtuele machine implementeren met elk subnet.

### <a name="create-the-first-virtual-machine"></a>De eerste virtuele machine maken

Maken van een virtuele machine in de *openbare* subnet met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Wanneer de volgende opdracht wordt uitgevoerd, wordt u gevraagd referenties op te geven. De waarden die u invoert worden geconfigureerd als de gebruikersnaam en het wachtwoord voor de virtuele machine. De `-AsJob` optie maakt de virtuele machine op de achtergrond, zodat u kunt doorgaan met de volgende stap.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "Public" `
    -Name "myVmPublic" `
    -AsJob
```

De uitvoer is vergelijkbaar met de volgende voorbeelduitvoer wordt geretourneerd:

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command                  
--     ----            -------------   -----         -----------     --------             -------                  
1      Long Running... AzureLongRun... Running       True            localhost            New-AzureRmVM     
```

### <a name="create-the-second-virtual-machine"></a>Het tweede virtuele machine maken

Maken van een virtuele machine in de *persoonlijke* subnet:

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "Private" `
    -Name "myVmPrivate"
```

Het duurt enkele minuten voor Azure maken van de virtuele machine. Ga niet door met de volgende stap totdat Azure is de virtuele machine gemaakt en wordt de uitvoer geretourneerd naar PowerShell. 

## <a name="confirm-access-to-storage-account"></a>Toegang tot de storage-account bevestigen

Gebruik [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) te retourneren van het openbare IP-adres van een virtuele machine. Het volgende voorbeeld wordt het openbare IP-adres van de *myVmPrivate* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmPrivate `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Vervang `<publicIpAddress>` in de volgende opdracht met het openbare IP-adres van de vorige opdracht geretourneerd en voer vervolgens de volgende opdracht: 

```powershell
mstsc /v:<publicIpAddress>
```

Een Remote Desktop Protocol (RDP)-bestand is gemaakt en gedownload op uw computer. Open het gedownloade rdp-bestand. Als u wordt gevraagd, selecteert u **Connect**. Geef de gebruikersnaam en wachtwoord die u hebt opgegeven bij het maken van de virtuele machine. U wilt selecteren **meer opties**, klikt u vervolgens **gebruik een ander account**, de referenties die u hebt opgegeven tijdens het maken van de virtuele machine opgeven. Selecteer **OK**. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Als u de waarschuwing ontvangt, selecteert u **Ja** of **doorgaan**, om door te gaan met de verbinding.

Op de *myVmPrivate* VM, de Azure-bestandsshare voor het station Z met behulp van PowerShell toewijzen. Voordat u de opdrachten die volgen, vervangt `<storage-account-key>` en `<storage-account-name>` met waarden door u opgegeven of opgehaald [een opslagaccount maken](#create-a-storage-account).

```powershell
$acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\my-file-share" -Credential $credential
```
PowerShell retourneert de uitvoer lijkt op de volgende voorbeelduitvoer:

```powershell
Name           Used (GB)     Free (GB) Provider      Root
----           ---------     --------- --------      ----
Z                                      FileSystem    \\vnt.file.core.windows.net\my-f...
```

De Azure-bestandsshare is toegewezen aan het station Z.

Bevestig dat de virtuele machine geen uitgaande verbinding heeft met andere openbare IP-adressen heeft:

```powershell
ping bing.com
```

U ontvangt geen antwoorden, omdat de netwerkbeveiligingsgroep die is gekoppeld aan de *persoonlijke* subnet kan geen uitgaande toegang tot de openbare IP-adressen dan de adressen die zijn toegewezen aan de Azure Storage-service.

Sluit de extern-bureaubladsessie naar de *myVmPrivate* VM.

## <a name="confirm-access-is-denied-to-storage-account"></a>Toegang is geweigerd voor storage-account bevestigen

Ophalen van het openbare IP-adres van de *myVmPublic* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmPublic `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Vervang `<publicIpAddress>` in de volgende opdracht met het openbare IP-adres van de vorige opdracht geretourneerd en voer vervolgens de volgende opdracht: 

```powershell
mstsc /v:<publicIpAddress>
```

Op de *myVmPublic* VM, probeert de Azure-bestandsshare om toe te wijzen station Z. Voordat u de opdrachten die volgen, vervangt `<storage-account-key>` en `<storage-account-name>` met waarden door u opgegeven of opgehaald [een opslagaccount maken](#create-a-storage-account).

```powershell
$acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\my-file-share" -Credential $credential
```

Toegang tot de share wordt geweigerd en u ontvangt een `New-PSDrive : Access is denied` fout. Toegang is geweigerd omdat de *myVmPublic* VM is geïmplementeerd in de *openbare* subnet. De *openbare* subnet beschikt niet over een service-eindpunt dat is ingeschakeld voor Azure Storage en het opslagaccount kunt u alleen toegang tot het netwerk van de *persoonlijke* subnet, niet de *openbare*subnet.

Sluit de extern-bureaubladsessie naar de *myVmPublic* VM.

Van uw computer probeert weer te geven van de bestandsshares in de storage-account met de volgende opdracht:

```powershell-interactive
Get-AzureStorageFile `
  -ShareName my-file-share `
  -Context $storageContext
```

Toegang is geweigerd en u ontvangt een *Get-AzureStorageFile: de externe server heeft een fout geretourneerd: (403) verboden. HTTP-statuscode: 403 - HTTP-foutbericht: deze aanvraag is niet geautoriseerd voor het uitvoeren van deze bewerking* fout, omdat de computer zich niet in de *persoonlijke* subnet van de *MyVirtualNetwork* virtueel netwerk.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, kunt u [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) verwijderen van de resourcegroep en alle resources die deze bevat:

```azurepowershell-interactive 
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een service-eindpunt voor een virtueel netwerksubnet ingeschakeld. Hebt u geleerd dat de service-eindpunten kunnen worden ingeschakeld voor resources met meerdere Azure-services worden geïmplementeerd. U hebt een Azure Storage-account en een beperkte netwerktoegang tot het opslagaccount alleen bronnen binnen een virtueel netwerksubnet gemaakt. Zie voor meer informatie over service-eindpunten, [overzicht van de Service-eindpunten](virtual-network-service-endpoints-overview.md) en [subnetten beheren](virtual-network-manage-subnet.md).

Als u meerdere virtuele netwerken in uw account hebt, kunt u twee virtuele netwerken met elkaar verbinden zodat de resources in elk virtueel netwerk met elkaar kunnen communiceren. Voor meer informatie Zie [verbinding maken met virtuele netwerken](tutorial-connect-virtual-networks-powershell.md).