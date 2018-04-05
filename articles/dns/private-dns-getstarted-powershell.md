---
title: Aan de slag met privézones in Azure DNS met behulp van PowerShell | Microsoft Docs
description: Informatie over het maken van een privé- DNS-zone en -record in Azure DNS. Dit is een stapsgewijze handleiding voor het maken en beheren van uw eerste privé-DNS-zone en -record met behulp van PowerShell.
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/20/2017
ms.author: kumud
ms.openlocfilehash: d9e5c9b8caa5349fbcda9b71f7524fb9e99b976c
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="get-started-with-azure-dns-private-zones-using-powershell"></a>Aan de slag met privézones in Azure DNS met behulp van PowerShell

Dit artikel leidt u stapsgewijs door de procedure voor het maken van uw eerste privé-DNS-zone en -record met behulp van Azure PowerShell.

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

Een DNS-zone wordt gebruikt om de DNS-records voor een bepaald domein te hosten. Als u uw domein wilt hosten in Azure DNS, moet u een DNS-zone maken voor die domeinnaam. Alle DNS-records voor uw domein worden vervolgens gemaakt binnen deze DNS-zone. Als u een privé-DNS-zone wilt publiceren naar uw virtuele netwerk, geeft u de lijst op met virtuele netwerken die zijn toegestaan om records in de zone om te zetten.  Deze netwerken worden virtuele resolutienetwerken genoemd.  U kunt ook een virtueel netwerk opgeven waarvoor Azure DNS hostnaamrecords bewaart wanneer een virtuele machine wordt gemaakt, een ander IP-adres krijgt of wordt vernietigd.  Dit wordt een virtueel registratienetwerk genoemd.

# <a name="get-the-preview-powershell-modules"></a>De preview van de PowerShell-modules downloaden
In deze instructies wordt ervan uitgegaan dat u Azure PowerShell al hebt geïnstalleerd en erbij bent aangemeld, en dat u over de vereiste modules voor de privézonefunctie beschikt. 

[!INCLUDE [dns-powershell-setup](../../includes/dns-powershell-setup-include.md)]

## <a name="create-the-resource-group"></a>De resourcegroep maken

Voordat u de DNS-zone maakt, wordt er een resourcegroep gemaakt waartoe de DNS-zone gaat behoren. In het onderstaande voorbeeld ziet u de opdracht.

```powershell
New-AzureRMResourceGroup -name MyResourceGroup -location "westus"
```

## <a name="create-a-dns-private-zone"></a>Een privé-DNS-zone maken

Een DNS-zone wordt gemaakt door de cmdlet `New-AzureRmDnsZone` met de waarde 'Private' te gebruiken voor de parameter ZoneType. In het volgende voorbeeld wordt een DNS-zone met de naam *contoso.local* gemaakt in de resourcegroep *MyResourceGroup*. De DNS-zone wordt beschikbaar gesteld voor het virtuele netwerk *MyAzureVnet* . Gebruik het voorbeeld om een DNS-zone te maken door de waarden te vervangen door uw eigen waarden.

Als de parameter ZoneType wordt weggelaten, wordt de zone gemaakt als een openbare zone, dus deze parameter is vereist als u een privézone wilt maken. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name MyAzureVnet -ResourceGroupName VnetResourceGroup
New-AzureRmDnsZone -Name contoso.local -ResourceGroupName MyResourceGroup -ZoneType Private -ResolutionVirtualNetworkId @($vnet.Id)
```

Als Azure automatisch hostnaamrecords in de zone moet kunnen maken, gebruikt u de parameter *RegistrationVirtualNetworkId* in plaats van *ResolutionVirtualNetworkId*.  Virtuele registratienetwerken zijn automatisch ingeschakeld voor omzetting.

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name MyAzureVnet -ResourceGroupName VnetResourceGroup
New-AzureRmDnsZone -Name contoso.local -ResourceGroupName MyResourceGroup -ZoneType Private -RegistrationVirtualNetworkId @($vnet.Id)
```

## <a name="create-a-dns-record"></a>Een DNS-record maken

U kunt recordsets maken met behulp van de cmdlet `New-AzureRmDnsRecordSet`. In het volgende voorbeeld maakt u een record met de relatieve naam 'db' in de DNS-zone 'contoso.local' in de resourcegroep 'MyResourceGroup'. De volledig gekwalificeerde naam van de recordset is 'db.contoso.local'. Het recordtype is 'A', met IP-adres '10.0.0.4' en de TTL is 3600 seconden.

```powershell
New-AzureRmDnsRecordSet -Name db -RecordType A -ZoneName contoso.local -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "10.0.0.4")
```

Voor andere recordtypen, voor recordsets met meerdere records, en als u bestaande records wilt wijzigen, raadpleegt u [Manage DNS records and record sets using Azure PowerShell](dns-operations-recordsets.md) (DNS-records en -recordsets beheren met behulp van Azure PowerShell). 

## <a name="view-records"></a>Records weergeven

Als u de DNS-records wilt weergeven in uw zone, gebruikt u:

```powershell
Get-AzureRmDnsRecordSet -ZoneName contoso.local -ResourceGroupName MyResourceGroup
```

# <a name="list-dns-private-zones"></a>Lijst met privé-DNS-zones weergeven

Door de zonenaam weg te laten uit `Get-AzureRmDnsZone`, kunt u een opsomming maken van alle zones in een resourcegroep. Deze bewerking retourneert een matrix met zoneobjecten.

```powershell
$zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup
```

Door zowel de zonenaam als de resourcegroepnaam uit `Get-AzureRmDnsZone` weg te laten, kunt u een opsomming maken van alle zones in het Azure-abonnement.

```powershell
$zoneList = Get-AzureRmDnsZone
```

## <a name="update-a-dns-private-zone"></a>Een privé-DNS-zone bijwerken

Wijzigingen aan een DNS-zoneresource kunnen worden aangebracht met behulp van `Set-AzureRmDnsZone`. Met deze cmdlet worden geen DNS-recordsets in de zone bijgewerkt (zie [DNS-records beheren](dns-operations-recordsets.md)). De cmdlet wordt alleen gebruikt voor het bijwerken van eigenschappen van de zoneresource zelf. De schrijfbare zone-eigenschappen zijn momenteel beperkt tot [Azure Resource Manager-'tags' voor de zoneresource](dns-zones-records.md#tags) en de parameters 'RegistrationVirtualNetworkId' en 'ResolutionVirtualNetworkId' voor privézones.

In het onderstaande voorbeeld wordt het Registration Virtual Network dat aan een zone is gekoppeld, vervangen door een nieuw netwerk: MyNewAzureVnet.

Let op: u moet de parameter ZoneType niet opgeven bij het bijwerken, anders dan bij het maken. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name MyNewAzureVnet -ResourceGroupName MyResourceGroup
Set-AzureRmDnsZone -Name contoso.local -ResourceGroupName MyResourceGroup -RegistrationVirtualNetworkId @($vnet.Id)
```

In het onderstaande voorbeeld wordt het Resolution Virtual Network dat aan een zone is gekoppeld, vervangen door een nieuw netwerk met de naam 'MyNewAzureVnet'.

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name MyNewAzureVnet -ResourceGroupName MyResourceGroup
Set-AzureRmDnsZone -Name contoso.local -ResourceGroupName MyResourceGroup -ResolutionVirtualNetworkId @($vnet.Id)
```

## <a name="delete-a-dns-private-zone"></a>Een privé-DNS-zone verwijderen

Privé-DNS-zones kunnen net als openbare zones worden verwijderd met de cmdlet `Remove-AzureRmDnsZone`.

> [!NOTE]
> Als u een DNS-zone verwijdert, worden ook alle DNS-records binnen de zone verwijderd. Deze bewerking kan niet ongedaan worden gemaakt. Als de DNS-zone in gebruik is en de zone wordt verwijderd, werken services die van de zone gebruik maken niet langer.
>
>Zie [DNS-zones en records beschermen](dns-protect-zones-recordsets.md) om beveiliging in te stellen tegen het per ongeluk verwijderen van zones.

Gebruik een van de volgende twee manieren om een DNS-zone te verwijderen:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group-name"></a>Geef de zone op met behulp van de zonenaam en de resourcegroepnaam

```powershell
Remove-AzureRmDnsZone -Name contoso.local -ResourceGroupName MyAzureResourceGroup
```

### <a name="specify-the-zone-using-a-zone-object"></a>Geef de zone op met behulp van een $zone-object

U kunt de te verwijderen zone opgeven met behulp van een `$zone`-object dat wordt geretourneerd door `Get-AzureRmDnsZone`.

```powershell
$zone = Get-AzureRmDnsZone -Name contoso.local -ResourceGroupName MyResourceGroup
Remove-AzureRmDnsZone -Zone $zone
```

Het zone-object kan ook worden doorgesluisd in plaats van te worden doorgegeven als een parameter:

```powershell
Get-AzureRmDnsZone -Name contoso.local -ResourceGroupName MyResourceGroup | Remove-AzureRmDnsZone

```

## <a name="confirmation-prompts"></a>Bevestigingsprompts

De cmdlets `New-AzureRmDnsZone`, `Set-AzureRmDnsZone` en `Remove-AzureRmDnsZone` ondersteunen bevestigingspromts.

Zowel in `New-AzureRmDnsZone` als `Set-AzureRmDnsZone` wordt om bevestiging gevraagd als de PowerShell-voorkeursvariabele `$ConfirmPreference` de waarde `Medium` of lager heeft. Vanwege het potentieel grote effect van het verwijderen van een DNS-zone vraagt de cmdlet `Remove-AzureRmDnsZone` om bevestiging als de PowerShell-variabele `$ConfirmPreference` een andere waarde heeft dan `None`.

Omdat de standaardwaarde voor `$ConfirmPreference` `High` is, wordt standaard alleen in `Remove-AzureRmDnsZone` om bevestiging gevraagd.

U kunt de huidige instelling van `$ConfirmPreference` overschrijven met behulp van de parameter `-Confirm`. Als u `-Confirm` of `-Confirm:$True` opgeeft, vraagt de cmdlet u om bevestiging voordat deze wordt uitgevoerd. Als u `-Confirm:$False` opgeeft, wordt u niet om bevestiging gevraagd.

Zie [over voorkeursvariabelen](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables) voor meer informatie over `-Confirm` en `$ConfirmPreference`.


## <a name="delete-all-resources"></a>Alle resources verwijderen

Als u alle resources wilt verwijderen die u in dit artikel hebt gemaakt, voert u de volgende stappen uit:

```powershell
Remove-AzureRMResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over privé-DNS-zones raadpleegt u [Using Azure DNS for private domains](private-dns-overview.md) (Azure DNS gebruiken voor privédomeinen).

Lees meer over een aantal veelvoorkomende [Privézone-scenario's](./private-dns-scenarios.md) die u kunt realiseren met Privézones in Azure DNS.

Zie [Manage DNS records and record sets in Azure DNS using PowerShell](dns-operations-recordsets.md) (DNS-records en -recordsets in Azure DNS beheren met behulp van PowerShell) voor meer informatie over het beheren van DNS-records in Azure DNS.

