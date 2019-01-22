---
title: Beperken van netwerktoegang tot PaaS-resources - Azure PowerShell | Microsoft Docs
description: In dit artikel leert u hoe u begrenzen en beperken van toegang tot het netwerk naar Azure-resources, zoals Azure Storage en Azure SQL Database, met virtual network-service-eindpunten met Azure PowerShell.
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
ms.openlocfilehash: e70a17271dee9f78f13c06ca2fd24dc39b20c6a4
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54425200"
---
# <a name="restrict-network-access-to-paas-resources-with-virtual-network-service-endpoints-using-powershell"></a>Netwerktoegang tot PaaS-resources beperken met virtual network-service-eindpunten met behulp van PowerShell

Met service-eindpunten voor virtuele netwerken kunt u de netwerktoegang tot sommige Azure-servicebronnen beperken tot een subnet van een virtueel netwerk. U kunt ook internettoegang tot de resources verwijderen. Service-eindpunten zorgen voor een rechtstreekse verbinding van uw virtuele netwerk met ondersteunde Azure-services, zodat u de privéadresruimte van uw virtuele netwerk kunt gebruiken voor toegang tot de Azure-services. Verkeer dat bestemd is voor Azure-resources via de service-eindpunten blijft altijd op het Microsoft Azure-backbone-netwerk. In dit artikel leert u het volgende:

* Een virtueel netwerk maken met één subnet
* Een subnet toevoegen en een service-eindpunt inschakelen
* Een Azure-resource maken en alleen toegang ertoe toestaan vanaf een subnet
* Een virtuele machine (VM) implementeren op elk subnet
* Toegang tot een resource vanaf een subnet bevestigen
* Bevestigen dat toegang wordt geweigerd aan een resource vanaf een subnet en internet

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, is voor dit artikel versie 5.4.1 of hoger van de Azure PowerShell-module vereist. Voer ` Get-Module -ListAvailable AzureRM` uit om te kijken welke versie is geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/azurerm/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Voordat u een virtueel netwerk maakt, moet u maken van een resourcegroep voor het virtuele netwerk en alle andere resources in dit artikel hebt gemaakt. Maak een resourcegroep met behulp van de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup*: 

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

Maak een subnetconfiguratie met [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Het volgende voorbeeld wordt de subnetconfiguratie van een voor een subnet met de naam *openbare*:

```azurepowershell-interactive
$subnetConfigPublic = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Public `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

Het subnet in het virtuele netwerk maken met het schrijven van de subnetconfiguratie naar het virtuele netwerk met [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork):

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="enable-a-service-endpoint"></a>Een service-eindpunt inschakelen 

U kunt de service-eindpunten alleen voor services die ondersteuning bieden voor service-eindpunten inschakelen. Service-eindpunt ingeschakeld services beschikbaar weergeven in een Azure-locatie met [Get-AzureRmVirtualNetworkAvailableEndpointService](/powershell/module/azurerm.network/get-azurermvirtualnetworkavailableendpointservice). Het volgende voorbeeld retourneert een lijst van service-eindpunt ingeschakeld services beschikbaar in de *eastus* regio. De lijst met services die zijn geretourneerd zal na verloop van tijd toenemen naarmate er meer Azure-services worden service-eindpunt ingeschakeld.

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

## <a name="restrict-network-access-for-a-subnet"></a>Netwerktoegang voor een subnet beperken

Netwerkbeveiliging maken beveiligingsregels met [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig). De volgende regel toestaat uitgaande toegang tot het openbare IP-adressen toegewezen aan de Azure Storage-service: 

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

De volgende regel weigert toegang tot alle openbare IP-adressen. De vorige regel overschrijft deze regel, vanwege de hogere prioriteit, die toegang tot het openbare IP-adressen van Azure Storage biedt.

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

De volgende regel staat Remote Desktop Protocol (RDP)-verkeer binnenkomende verbindingen naar het subnet vanaf elke locatie. Extern bureaublad-verbindingen zijn toegestaan voor het subnet, zodat u toegang tot het netwerk aan een resource in een latere stap kunt controleren.

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

Maak een netwerkbeveiligingsgroep met [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup). Het volgende voorbeeld wordt een netwerkbeveiligingsgroep met de naam *myNsgPrivate*.

```azurepowershell-interactive
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myNsgPrivate `
  -SecurityRules $rule1,$rule2,$rule3
```

Koppelen van de netwerkbeveiligingsgroep voor de *persoonlijke* subnet met de [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) en schrijf de subnetconfiguratie vervolgens naar het virtuele netwerk. Het volgende voorbeeld wordt de *myNsgPrivate* netwerkbeveiligingsgroep naar de *persoonlijke* subnet:

```azurepowershell-interactive
Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $VirtualNetwork `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -ServiceEndpoint Microsoft.Storage `
  -NetworkSecurityGroup $nsg

$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="restrict-network-access-to-a-resource"></a>Netwerktoegang tot een resource beperken

De stappen die nodig zijn om netwerktoegang te beperken tot resources die zijn gemaakt met Azure-services waarvoor service-eindpunten zijn ingeschakeld, verschillen per service. Zie de documentatie voor afzonderlijke services voor specifieke stappen voor elke service. De rest van dit artikel bevat stappen voor het beperken van toegang tot het netwerk voor een Azure Storage-account, als voorbeeld.

### <a name="create-a-storage-account"></a>Create a storage account

Maak een Azure storage-account met [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount). Vervang `<replace-with-your-unique-storage-account-name>` met een naam die uniek is voor alle Azure locaties, tussen 3 en 24 tekens lang, met behulp van alleen cijfers en kleine letters.

```azurepowershell-interactive
$storageAcctName = '<replace-with-your-unique-storage-account-name>'

New-AzureRmStorageAccount `
  -Location EastUS `
  -Name $storageAcctName `
  -ResourceGroupName myResourceGroup `
  -SkuName Standard_LRS `
  -Kind StorageV2
```

Nadat het opslagaccount is gemaakt, de sleutel voor het opslagaccount ophalen in een variabele met [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey):

```azurepowershell-interactive
$storageAcctKey = (Get-AzureRmStorageAccountKey `
  -ResourceGroupName myResourceGroup `
  -AccountName $storageAcctName).Value[0]
```

De sleutel wordt gebruikt voor een bestandsshare maken in een latere stap. Voer `$storageAcctKey` en noteer de waarde, zoals u ook moet handmatig invoeren in een latere stap wanneer u de bestandsshare aan een station in een virtuele machine toewijzen.

### <a name="create-a-file-share-in-the-storage-account"></a>Een bestandsshare maken in het opslagaccount

Een context maken voor uw storage-account en -sleutel met [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext). De context bevat de naam en toegangssleutel van het opslagaccount:

```azurepowershell-interactive
$storageContext = New-AzureStorageContext $storageAcctName $storageAcctKey
```

Maak een bestandsshare met [New-AzureStorageShare](/powershell/module/azure.storage/new-azurestorageshare):

$share = New-AzureStorageShare my-file-share -Context $storageContext

### <a name="deny-all-network-access-to-a-storage-account"></a>Alle toegang tot het netwerk naar een opslagaccount weigeren

Standaard accepteren opslagaccounts netwerkverbindingen van clients in ieder netwerk. Om te beperken de toegang tot geselecteerde netwerken, wijzigt u de standaardactie voor *weigeren* met [Update AzureRmStorageAccountNetworkRuleSet](/powershell/module/azurerm.storage/update-azurermstorageaccountnetworkruleset). Zodra het netwerktoegang is geweigerd, is de storage-account niet toegankelijk is vanaf een netwerk.

```azurepowershell-interactive
Update-AzureRmStorageAccountNetworkRuleSet  `
  -ResourceGroupName "myresourcegroup" `
  -Name $storageAcctName `
  -DefaultAction Deny
```

### <a name="enable-network-access-from-a-subnet"></a>Netwerktoegang vanuit een subnet inschakelen

Ophalen van het gemaakte virtuele netwerk met [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork) en vervolgens het privé-subnet-object op te halen in een variabele met [Get-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig):

```azurepowershell-interactive
$privateSubnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName "myResourceGroup" `
  -Name "myVirtualNetwork" `
  | Get-AzureRmVirtualNetworkSubnetConfig `
  -Name "Private"
```

Toestaan van toegang tot het netwerk naar het opslagaccount van de *persoonlijke* subnet met de [toevoegen AzureRmStorageAccountNetworkRule](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig).

```azurepowershell-interactive
Add-AzureRmStorageAccountNetworkRule `
  -ResourceGroupName "myresourcegroup" `
  -Name $storageAcctName `
  -VirtualNetworkResourceId $privateSubnet.Id
```

## <a name="create-virtual-machines"></a>Virtuele machines maken

Implementeer een VM in elk subnet om de netwerktoegang tot een opslagaccount te testen.

### <a name="create-the-first-virtual-machine"></a>De eerste virtuele machine maken

Maak een virtuele machine in de *openbare* subnet met de [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Wanneer de volgende opdracht wordt uitgevoerd, wordt u gevraagd referenties op te geven. De waarden die u invoert, worden geconfigureerd als de gebruikersnaam en het wachtwoord voor de virtuele machine. Met de optie `-AsJob` wordt de virtuele machine op de achtergrond gemaakt, zodat u kunt doorgaan met de volgende stap.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "Public" `
    -Name "myVmPublic" `
    -AsJob
```

Uitvoer is vergelijkbaar met de volgende voorbeelduitvoer wordt geretourneerd:

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command                  
--     ----            -------------   -----         -----------     --------             -------                  
1      Long Running... AzureLongRun... Running       True            localhost            New-AzureRmVM     
```

### <a name="create-the-second-virtual-machine"></a>De tweede virtuele machine maken

Maak een virtuele machine in de *persoonlijke* subnet:

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "Private" `
    -Name "myVmPrivate"
```

Het duurt enkele minuten voor Azure te maken van de virtuele machine. Ga niet verder met de volgende stap voordat Azure klaar is met het maken van de virtuele machine en wordt uitvoer geretourneerd naar PowerShell. 

## <a name="confirm-access-to-storage-account"></a>Toegang tot opslagaccount bevestigen

Gebruik [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) om het openbare IP-adres van een virtuele machine op te halen. Het volgende voorbeeld wordt het openbare IP-adres van de *myVmPrivate* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmPrivate `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Vervang `<publicIpAddress>` in de volgende opdracht door het openbare IP-adres dat met de vorige opdracht is geretourneerd en voer vervolgens de volgende opdracht in: 

```powershell
mstsc /v:<publicIpAddress>
```

Er wordt een Remote Desktop Protocol-bestand (.rdp) gemaakt en gedownload naar uw computer. Open het gedownloade RDP-bestand. Selecteer **Verbinding maken** wanneer hierom wordt gevraagd. Voer de gebruikersnaam en het wachtwoord in die u hebt opgegeven bij het maken van de virtuele machine. Mogelijk moet u **Meer opties** en vervolgens **Een ander account gebruiken** selecteren om de aanmeldingsgegevens op te geven die u hebt ingevoerd tijdens het maken van de VM. Selecteer **OK**. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Als u de waarschuwing ontvangt, selecteert u **Ja** of **Doorgaan** om door te gaan met de verbinding.

Wijs op de VM *myVmPrivate* de Azure-bestandsshare toe aan station Z met behulp van PowerShell. Voordat u de opdrachten die volgen uitvoert, Vervang `<storage-account-key>` en `<storage-account-name>` met waarden uit de opgegeven of opgehaald [een opslagaccount maken](#create-a-storage-account).

```powershell
$acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\my-file-share" -Credential $credential
```
PowerShell retourneert uitvoer die er ongeveer zo uitziet als in dit voorbeeld:

```powershell
Name           Used (GB)     Free (GB) Provider      Root
----           ---------     --------- --------      ----
Z                                      FileSystem    \\vnt.file.core.windows.net\my-f...
```

De Azure-bestandsshare is toegewezen aan station Z.

Bevestig dat de virtuele machine geen uitgaande verbinding naar een andere openbare IP-adressen heeft:

```powershell
ping bing.com
```

U krijgt geen antwoorden, omdat de netwerkbeveiligingsgroep die is gekoppeld aan het *Privé*-subnet geen uitgaande toegang toestaat aan andere openbare IP-adressen dan de adressen die zijn toegewezen aan de Azure Storage-service.

Sluit de externe bureaubladsessie naar de VM *myVmPrivate*.

## <a name="confirm-access-is-denied-to-storage-account"></a>Bevestigen dat toegang tot opslagaccount wordt geweigerd

Ophalen van het openbare IP-adres van de *myVmPublic* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmPublic `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Vervang `<publicIpAddress>` in de volgende opdracht door het openbare IP-adres dat met de vorige opdracht is geretourneerd en voer vervolgens de volgende opdracht in: 

```powershell
mstsc /v:<publicIpAddress>
```

Op de *myVmPublic* VM, wijst u de Azure-bestandsshare aan station Z. Voordat u de opdrachten die volgen uitvoert, Vervang `<storage-account-key>` en `<storage-account-name>` met waarden uit de opgegeven of opgehaald [een opslagaccount maken](#create-a-storage-account).

```powershell
$acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\my-file-share" -Credential $credential
```

Toegang tot de share wordt geweigerd en u ontvangt een `New-PSDrive : Access is denied` fout. De toegang wordt geweigerd omdat de VM *myVmPublic* is geïmplementeerd in het *Openbare* subnet. Het *Openbare* subnet heeft geen service-eindpunt ingeschakeld voor Azure Storage en het opslagaccount staat alleen netwerktoegang toe van het *Privé*-subnet, en niet van het *Openbare*subnet.

Sluit de externe bureaubladsessie naar de VM *myVmPublic*.

Op uw computer probeert weer te geven van de bestandsshares in de storage-account met de volgende opdracht:

```powershell-interactive
Get-AzureStorageFile `
  -ShareName my-file-share `
  -Context $storageContext
```

De toegang is geweigerd en u ontvangt een *Get-AzureStorageFile: De externe server heeft een fout geretourneerd: (403)-verboden. HTTP-statuscode: 403 - HTTP foutmelding: Deze aanvraag is niet gemachtigd voor deze bewerking* fout, omdat de computer zich niet in de *persoonlijke* subnet van de *MyVirtualNetwork* virtueel netwerk.

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) gebruiken om de resourcegroep en alle resources die deze bevat te verwijderen:

```azurepowershell-interactive 
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een service-eindpunt voor een subnet van een virtueel netwerk ingeschakeld. U hebt geleerd dat service-eindpunten kunnen worden ingeschakeld voor met meerdere Azure-services geïmplementeerde resources. U hebt een Azure Storage-account gemaakt en netwerktoegang tot het opslagaccount beperkt tot alleen resources binnen een subnet van een virtueel netwerk. Zie voor meer informatie over service-eindpunten [Overzicht voor service-eindpunten](virtual-network-service-endpoints-overview.md) en [Subnetten beheren](virtual-network-manage-subnet.md).

Als u meerdere virtuele netwerken in uw account hebt, kunt u twee virtuele netwerken met elkaar verbinden zodat de resources in beide virtuele netwerken met elkaar kunnen communiceren. Voor meer informatie Zie [virtuele netwerken verbinden](tutorial-connect-virtual-networks-powershell.md).
