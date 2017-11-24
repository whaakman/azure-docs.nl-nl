---
title: "Aan de slag met privézones in Azure DNS met behulp van PowerShell | Microsoft Docs"
description: "Informatie over het maken van een privé- DNS-zone en -record in Azure DNS. Dit is een stapsgewijze handleiding voor het maken en beheren van uw eerste privé-DNS-zone en -record met behulp van PowerShell."
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/20/2017
ms.author: kumud
ms.openlocfilehash: d71e2391b6415b2403447479dea4fd0a3b818ed0
ms.sourcegitcommit: 4ea06f52af0a8799561125497f2c2d28db7818e7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2017
---
# <a name="get-started-with-azure-dns-private-zones-using-powershell"></a>Aan de slag met privézones in Azure DNS met behulp van PowerShell

Dit artikel leidt u stapsgewijs door de procedure voor het maken van uw eerste privé-DNS-zone en -record met behulp van Azure PowerShell.

Een DNS-zone wordt gebruikt om de DNS-records voor een bepaald domein te hosten. Als u uw domein wilt hosten in Azure DNS, moet u een DNS-zone maken voor die domeinnaam. Alle DNS-records voor uw domein worden vervolgens gemaakt binnen deze DNS-zone. Als u een privé-DNS-zone wilt publiceren naar uw virtuele netwerk, geeft u de lijst op met virtuele netwerken die zijn toegestaan om records in de zone om te zetten.  Dit worden omzettingsnetwerken genoemd.  U kunt ook een set van virtuele netwerken opgeven waarvoor Azure DNS hostnaamrecords bewaart wanneer een virtuele machine wordt gemaakt, het IP verandert of wordt vernietigd.  Dit worden registratienetwerken genoemd.

Omdat deze functie momenteel een beheerde preview is, wordt er een PowerShell-module in preview verstrekt.

[!INCLUDE [private-dns-preview-notice](../../includes/private-dns-preview-notice.md)]

## <a name="create-the-resource-group"></a>De resourcegroep maken

Voordat u de DNS-zone maakt, wordt er een resourcegroep gemaakt waartoe de DNS-zone gaat behoren. Hieronder ziet u de opdracht.

```powershell
New-AzureRMResourceGroup -name MyResourceGroup -location "westus"
```

## <a name="create-a-dns-zone"></a>Een DNS-zone maken

Een DNS-zone wordt gemaakt met de cmdlet `New-AzureRmDnsZone`. In het volgende voorbeeld wordt een DNS-zone met de naam *contoso.local* gemaakt in de resourcegroep *MyResourceGroup*. De DNS-zone wordt beschikbaar gesteld voor het virtuele netwerk *MyAzureVnet* . Gebruik het voorbeeld om een DNS-zone te maken door de waarden te vervangen door uw eigen waarden.

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

## <a name="delete-all-resources"></a>Alle resources verwijderen

Als u alle resources wilt verwijderen die u in dit artikel hebt gemaakt, voert u de volgende stappen uit:

```powershell
Remove-AzureRMResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over privé-DNS-zones raadpleegt u [Using Azure DNS for private domains](private-dns-overview.md) (Azure DNS gebruiken voor privédomeinen).

Zie [Manage DNS records and record sets in Azure DNS using PowerShell](dns-operations-recordsets.md) (DNS-records en -recordsets in Azure DNS beheren met behulp van PowerShell) voor meer informatie over het beheren van DNS-records in Azure DNS.

