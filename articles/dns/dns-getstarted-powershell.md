---
title: Aan de slag met Azure DNS met behulp van PowerShell | Microsoft Docs
description: Informatie over het maken van een DNS-zone en -record in Azure DNS. Dit is een stapsgewijze handleiding voor het maken en beheren van uw eerste DNS-zone en -record met behulp van PowerShell.
services: dns
documentationcenter: na
author: KumudD
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: kumud
ms.openlocfilehash: 050111f4a5e8459e89d049ccb879b5079ff68527
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
ms.locfileid: "30175321"
---
# <a name="get-started-with-azure-dns-using-powershell"></a>Aan de slag met Azure DNS met behulp van PowerShell

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Azure CLI 1.0](dns-getstarted-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-cli.md)

Dit artikel leidt u stapsgewijs door de procedure voor het maken van uw eerste DNS-zone en -record met behulp van Azure PowerShell. U kunt deze stappen ook uitvoeren met de Azure-portal of de platformoverschrijdende Azure CLI. Azure DNS ondersteunt ook het maken van een persoonlijke domeinen. Zie [Aan de slag met privézones in Azure DNS met behulp van PowerShell](private-dns-getstarted-powershell.md) voor stapsgewijze instructies over het maken van uw eerste DNS-privézone en -record.

Een DNS-zone wordt gebruikt om de DNS-records voor een bepaald domein te hosten. Als u uw domein wilt hosten in Azure DNS, moet u een DNS-zone maken voor die domeinnaam. Alle DNS-records voor uw domein worden vervolgens gemaakt binnen deze DNS-zone. Tot slot moet u de naamservers voor het domein configureren om de DNS-zone te publiceren naar internet. Deze stappen worden hieronder allemaal beschreven.

Bij deze instructies wordt ervan uitgegaan dat u Azure PowerShell al hebt geïnstalleerd en bent aangemeld. Zie [How to manage DNS zones using PowerShell](dns-operations-dnszones.md) (DNS-zones beheren met behulp van PowerShell) voor hulp.

## <a name="create-the-resource-group"></a>De resourcegroep maken

Voordat u de DNS-zone maakt, wordt er een resourcegroep gemaakt waartoe de DNS-zone gaat behoren. Hieronder ziet u de opdracht.

```powershell
New-AzureRMResourceGroup -name MyResourceGroup -location "westus"
```

## <a name="create-a-dns-zone"></a>Een DNS-zone maken

Een DNS-zone wordt gemaakt met de cmdlet `New-AzureRmDnsZone`. In het volgende voorbeeld maakt u een DNS-zone met de naam *contoso.com* in de resourcegroep *MyResourceGroup*. Gebruik het voorbeeld om een DNS-zone te maken door de waarden te vervangen door uw eigen waarden.

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyResourceGroup
```
Azure DNS ondersteunt nu ook privé-DNS-zones (momenteel in openbare preview).  Voor meer informatie over privé-DNS-zones raadpleegt u [Using Azure DNS for private domains](private-dns-overview.md) (Azure DNS gebruiken voor privédomeinen). Zie voor een voorbeeld van het maken van een privé-DNS-zone [Aan de slag met privézones in Azure DNS met behulp van PowerShell](./private-dns-getstarted-powershell.md).

## <a name="create-a-dns-record"></a>Een DNS-record maken

U kunt recordsets maken met behulp van de cmdlet `New-AzureRmDnsRecordSet`. In het volgende voorbeeld maakt u een record met de relatieve naam 'www' in de DNS-zone 'contoso.com' in de resourcegroep 'MyResourceGroup'. De volledig gekwalificeerde naam van de recordset is 'www.contoso.com'. Het recordtype is 'A', met IP-adres '1.2.3.4', en de TTL is 3600 seconden.

```powershell
New-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName contoso.com -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4")
```

Voor andere recordtypen, voor recordsets met meerdere records, en als u bestaande records wilt wijzigen, raadpleegt u [Manage DNS records and record sets using Azure PowerShell](dns-operations-recordsets.md) (DNS-records en -recordsets beheren met behulp van Azure PowerShell). 


## <a name="view-records"></a>Records weergeven

Als u de DNS-records wilt weergeven in uw zone, gebruikt u:

```powershell
Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyResourceGroup
```


## <a name="update-name-servers"></a>Naamservers bijwerken

Wanneer uw DNS-zone en -records correct zijn ingesteld, moet u uw domeinnaam configureren voor het gebruik van de Azure DNS-naamservers . Op die manier kunnen andere gebruikers op internet uw DNS-records vinden.

De naamservers voor uw zone zijn gegeven door de cmdlet `Get-AzureRmDnsZone`:

```powershell
Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyResourceGroup

Name                  : contoso.com
ResourceGroupName     : myresourcegroup
Etag                  : 00000003-0000-0000-b40d-0996b97ed101
Tags                  : {}
NameServers           : {ns1-01.azure-dns.com., ns2-01.azure-dns.net., ns3-01.azure-dns.org., ns4-01.azure-dns.info.}
NumberOfRecordSets    : 3
MaxNumberOfRecordSets : 5000
```

Deze naamservers moeten worden geconfigureerd met de domeinnaamregistrar (waar u de domeinnaam hebt gekocht). Uw registrar zal u de mogelijkheid bieden om de naamservers voor het domein in te stellen. Zie [Uw domein delegeren naar Azure DNS](dns-domain-delegation.md) voor meer informatie.

## <a name="delete-all-resources"></a>Alle resources verwijderen

Als u alle resources wilt verwijderen die u in dit artikel hebt gemaakt, voert u de volgende stappen uit:

```powershell
Remove-AzureRMResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

Zie [Azure DNS Overview](dns-overview.md) (Overzicht van Azure DNS) voor meer informatie over Azure DNS.

Zie [Manage DNS zones in Azure DNS using PowerShell](dns-operations-dnszones.md) (DNS-zones in Azure DNS beheren met behulp van PowerShell) voor meer informatie over het beheren van DNS-zones in Azure DNS.

Zie [Manage DNS records and record sets in Azure DNS using PowerShell](dns-operations-recordsets.md) (DNS-records en -recordsets in Azure DNS beheren met behulp van PowerShell) voor meer informatie over het beheren van DNS-records in Azure DNS.

