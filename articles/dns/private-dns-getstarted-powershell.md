---
title: 'Zelfstudie: Een Azure DNS-privézone maken met Azure PowerShell'
description: In deze zelfstudie maakt en test u een DNS-privézone en -record in Azure DNS. Dit is een stapsgewijze handleiding voor het maken en beheren van uw eerste privé-DNS-zone en -record met behulp van Azure PowerShell.
services: dns
author: vhorne
ms.service: dns
ms.topic: tutorial
ms.date: 7/24/2018
ms.author: victorh
ms.openlocfilehash: 73b8dfd741543560cd6ebf26178618a70bdae5f6
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55992769"
---
# <a name="tutorial-create-an-azure-dns-private-zone-using-azure-powershell"></a>Zelfstudie: Een Azure DNS-privézone maken met Azure PowerShell

Deze zelfstudie leidt u stapsgewijs door de procedure voor het maken van uw eerste privé-DNS-zone en -record met behulp van Azure PowerShell.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

Een DNS-zone wordt gebruikt om de DNS-records voor een bepaald domein te hosten. Als u uw domein wilt hosten in Azure DNS, moet u een DNS-zone maken voor die domeinnaam. Alle DNS-records voor uw domein worden vervolgens gemaakt binnen deze DNS-zone. Als u een privé-DNS-zone wilt publiceren naar uw virtuele netwerk, geeft u de lijst op met virtuele netwerken die zijn toegestaan om records in de zone om te zetten.  Deze worden *virtuele resolutienetwerken* genoemd. U kunt ook een virtueel netwerk opgeven waarvoor Azure DNS hostnaamrecords bewaart wanneer een virtuele machine wordt gemaakt, een ander IP-adres krijgt of wordt verwijderd.  Dit wordt een *virtueel resolutienetwerk* genoemd.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een privé-DNS-zone maken
> * Virtuele testmachines maken
> * Een extra DNS-record maken
> * De privézone testen

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

U kunt deze zelfstudie desgewenst volgen met de [Azure CLI](private-dns-getstarted-cli.md).

<!--- ## Get the Preview PowerShell modules
These instructions assume you have already installed and signed in to Azure PowerShell, including ensuring you have the required modules for the Private Zone feature. -->

<!---[!INCLUDE [dns-powershell-setup](../../includes/dns-powershell-setup-include.md)] -->

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

## <a name="create-the-resource-group"></a>De resourcegroep maken

Eerst maakt u een resourcegroep die de DNS-zone gaat bevatten: 

```azurepowershell
New-AzResourceGroup -name MyAzureResourceGroup -location "eastus"
```

## <a name="create-a-dns-private-zone"></a>Een privé-DNS-zone maken

Een DNS-zone wordt gemaakt door de cmdlet `New-AzDnsZone` met de waarde *Private* te gebruiken voor de parameter **ZoneType**. In het volgende voorbeeld wordt een DNS-zone met de naam **contoso.local** gemaakt in de resourcegroep **MyAzureResourceGroup**. De DNS-zone wordt beschikbaar gesteld voor het virtuele netwerk **MyAzureVnet** .

Als de parameter **ZoneType** wordt weggelaten, wordt de zone gemaakt als een openbare zone, dus deze parameter is vereist voor het maken van een privézone. 

```azurepowershell
$backendSubnet = New-AzVirtualNetworkSubnetConfig -Name backendSubnet -AddressPrefix "10.2.0.0/24"
$vnet = New-AzVirtualNetwork `
  -ResourceGroupName MyAzureResourceGroup `
  -Location eastus `
  -Name myAzureVNet `
  -AddressPrefix 10.2.0.0/16 `
  -Subnet $backendSubnet

New-AzDnsZone -Name contoso.local -ResourceGroupName MyAzureResourceGroup `
   -ZoneType Private `
   -RegistrationVirtualNetworkId @($vnet.Id)
```

Als u een zone voor alleen naamomzetting (geen automatische hostnaam maken) wilt maken, kunt u de parameter *ResolutionVirtualNetworkId* gebruiken in plaats van de parameter*RegistrationVirtualNetworkId*.

> [!NOTE]
> U kunt de automatisch gemaakte hostnaamrecords niet zien. Maar later kunt u een test uitvoeren om te controleren of ze bestaan.

### <a name="list-dns-private-zones"></a>Lijst met privé-DNS-zones weergeven

Door de zonenaam weg te laten uit `Get-AzDnsZone`, kunt u een opsomming maken van alle zones in een resourcegroep. Deze bewerking retourneert een matrix met zoneobjecten.

```azurepowershell
Get-AzDnsZone -ResourceGroupName MyAzureResourceGroup
```

Door zowel de zonenaam als de resourcegroepnaam uit `Get-AzDnsZone` weg te laten, kunt u een opsomming maken van alle zones in het Azure-abonnement.

```azurepowershell
Get-AzDnsZone
```

## <a name="create-the-test-virtual-machines"></a>De virtuele testmachines maken

Maak nu twee virtuele machines, zodat u uw DNS-privézone kunt testen:

```azurepowershell
New-AzVm `
    -ResourceGroupName "myAzureResourceGroup" `
    -Name "myVM01" `
    -Location "East US" `
    -subnetname backendSubnet `
    -VirtualNetworkName "myAzureVnet" `
    -addressprefix 10.2.0.0/24 `
    -OpenPorts 3389

New-AzVm `
    -ResourceGroupName "myAzureResourceGroup" `
    -Name "myVM02" `
    -Location "East US" `
    -subnetname backendSubnet `
    -VirtualNetworkName "myAzureVnet" `
    -addressprefix 10.2.0.0/24 `
    -OpenPorts 3389
```

Het duurt enkele minuten voordat dit is voltooid.

## <a name="create-an-additional-dns-record"></a>Een extra DNS-record maken

U kunt recordsets maken met behulp van de cmdlet `New-AzDnsRecordSet`. In het volgende voorbeeld maakt u een record met de relatieve naam **db** in de DNS-zone **contoso.local** in de resourcegroep **MyAzureResourceGroup**. De volledig gekwalificeerde naam van de recordset is **db.contoso.local**. Het recordtype is 'A', met IP-adres '10.2.0.4' en de TTL is 3600 seconden.

```azurepowershell
New-AzDnsRecordSet -Name db -RecordType A -ZoneName contoso.local `
   -ResourceGroupName MyAzureResourceGroup -Ttl 3600 `
   -DnsRecords (New-AzDnsRecordConfig -IPv4Address "10.2.0.4")
```

### <a name="view-dns-records"></a>DNS-records weergeven

Als u de DNS-records in uw zone wilt weergeven,voert u de volgende opdracht uit:

```azurepowershell
Get-AzDnsRecordSet -ZoneName contoso.local -ResourceGroupName MyAzureResourceGroup
```
Denk eraan dat u de automatisch gemaakte A-records niet kunt zien voor uw twee virtuele testmachines.

## <a name="test-the-private-zone"></a>De privézone testen

U kunt nu de naamomzetting testen voor uw **contoso.local**-privézone.

### <a name="configure-vms-to-allow-inbound-icmp"></a>VM’s configureren voor het toestaan van inkomende ICMP

U kunt de pingopdracht gebruiken voor het testen van naamomzetting. Configureer de firewall dus op beide virtuele machines om inkomende ICMP-pakketten toe te staan.

1. Maak verbinding met myVM01 en open een Windows PowerShell-venster met beheerdersbevoegdheden.
2. Voer de volgende opdracht uit:

   ```powershell
   New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
   ```

Herhaal voor myVM02.

### <a name="ping-the-vms-by-name"></a>De VM's pingen op naam

1. Ping vanuit de myVM02 Windows PowerShell-opdrachtprompt myVM01 met de naam van de automatisch geregistreerde hostnaam:
   ```
   ping myVM01.contoso.local
   ```
   De uitvoer ziet er ongeveer als volgt uit:
   ```
   PS C:\> ping myvm01.contoso.local

   Pinging myvm01.contoso.local [10.2.0.4] with 32 bytes of data:
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time=1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128

   Ping statistics for 10.2.0.4:
       Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
   Approximate round trip times in milli-seconds:
       Minimum = 0ms, Maximum = 1ms, Average = 0ms
   PS C:\>
   ```
2. Ping nu de **db**-naam die u eerder hebt gemaakt:
   ```
   ping db.contoso.local
   ```
   De uitvoer ziet er ongeveer als volgt uit:
   ```
   PS C:\> ping db.contoso.local

   Pinging db.contoso.local [10.2.0.4] with 32 bytes of data:
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128

   Ping statistics for 10.2.0.4:
       Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
   Approximate round trip times in milli-seconds:
       Minimum = 0ms, Maximum = 0ms, Average = 0ms
   PS C:\>
   ```

## <a name="delete-all-resources"></a>Alle resources verwijderen

Verwijder de **MyAzureResourceGroup** als u deze niet langer nodig hebt om de resources die u in deze zelfstudie hebt gemaakt te verwijderen.

```azurepowershell
Remove-AzResourceGroup -Name MyAzureResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een DNS-privézone geïmplementeerd, een DNS-record gemaakt en de zone getest.
U kunt nu meer leren over DNS-privézones.

> [!div class="nextstepaction"]
> [Azure DNS gebruiken voor persoonlijke domeinen](private-dns-overview.md)




