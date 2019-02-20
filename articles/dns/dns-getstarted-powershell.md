---
title: 'Snelstart: Een Azure DNS-zone en -record maken met behulp van Azure PowerShell'
description: Informatie over het maken van een DNS-zone en -record in Azure DNS. Dit is een stapsgewijze snelstartgids voor het maken en beheren van uw eerste DNS-zone en -record met behulp van Azure PowerShell.
services: dns
author: vhorne
ms.service: dns
ms.topic: quickstart
ms.date: 12/4/2018
ms.author: victorh
ms.openlocfilehash: 839c97ccccbc1ce2cf646afcd27894a190eda1b0
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "56000883"
---
# <a name="quickstart-create-an-azure-dns-zone-and-record-using-azure-powershell"></a>Quickstart: Een Azure DNS-zone en -record maken met behulp van Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

In deze snelstartgids maakt u uw eerste DNS-zone en -record met behulp van Azure PowerShell. U kunt deze stappen ook uitvoeren met [Azure Portal](dns-getstarted-portal.md) of de [Azure CLI](dns-getstarted-cli.md). 

Een DNS-zone wordt gebruikt om de DNS-records voor een bepaald domein te hosten. Als u uw domein wilt hosten in Azure DNS, moet u een DNS-zone maken voor die domeinnaam. Alle DNS-records voor uw domein worden vervolgens gemaakt binnen deze DNS-zone. Tot slot moet u de naamservers voor het domein configureren om de DNS-zone te publiceren naar internet. Deze stappen worden hieronder allemaal beschreven.

Azure DNS ondersteunt ook het maken van persoonlijke domeinen. Zie [Aan de slag met privézones in Azure DNS met behulp van PowerShell](private-dns-getstarted-powershell.md) voor stapsgewijze instructies over het maken van uw eerste DNS-privézone en -record.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="create-the-resource-group"></a>De resourcegroep maken

Voordat u de DNS-zone maakt, maakt u een resourcegroep die de DNS-zone gaat bevatten:

```powershell
New-AzResourceGroup -name MyResourceGroup -location "eastus"
```

## <a name="create-a-dns-zone"></a>Een DNS-zone maken

Een DNS-zone wordt gemaakt met de cmdlet `New-AzDnsZone`. In het volgende voorbeeld maakt u een DNS-zone met de naam *contoso.com* in de resourcegroep *MyResourceGroup*. Gebruik het voorbeeld om een DNS-zone te maken door de waarden te vervangen door uw eigen waarden.

```powershell
New-AzDnsZone -Name contoso.com -ResourceGroupName MyResourceGroup
```

## <a name="create-a-dns-record"></a>Een DNS-record maken

U kunt recordsets maken met behulp van de cmdlet `New-AzDnsRecordSet`. In het volgende voorbeeld maakt u een record met de relatieve naam 'www' in de DNS-zone 'contoso.com' in de resourcegroep 'MyResourceGroup'. De volledig gekwalificeerde naam van de recordset is 'www.contoso.com'. Het recordtype is 'A', met IP-adres '1.2.3.4', en de TTL is 3600 seconden.

```powershell
New-AzDnsRecordSet -Name www -RecordType A -ZoneName contoso.com -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -IPv4Address "1.2.3.4")
```

## <a name="view-records"></a>Records weergeven

Als u de DNS-records wilt weergeven in uw zone, gebruikt u:

```powershell
Get-AzDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyResourceGroup
```

## <a name="update-name-servers"></a>Naamservers bijwerken

Wanneer uw DNS-zone en -records correct zijn ingesteld, moet u uw domeinnaam configureren voor het gebruik van de Azure DNS-naamservers . Op die manier kunnen andere gebruikers op internet uw DNS-records vinden.

De naamservers voor uw zone zijn gegeven door de cmdlet `Get-AzDnsZone`:

```powershell
Get-AzDnsZone -Name contoso.com -ResourceGroupName MyResourceGroup

Name                  : contoso.com
ResourceGroupName     : myresourcegroup
Etag                  : 00000003-0000-0000-b40d-0996b97ed101
Tags                  : {}
NameServers           : {ns1-01.azure-dns.com., ns2-01.azure-dns.net., ns3-01.azure-dns.org., ns4-01.azure-dns.info.}
NumberOfRecordSets    : 3
MaxNumberOfRecordSets : 5000
```

Deze naamservers moeten worden geconfigureerd met de domeinnaamregistrar (waar u de domeinnaam hebt gekocht). Uw registrar zal u de mogelijkheid bieden om de naamservers voor het domein in te stellen. Zie [Zelfstudie: Uw domein hosten in Azure DNS](dns-delegate-domain-azure-dns.md#delegate-the-domain) voor meer informatie.

## <a name="delete-all-resources"></a>Alle resources verwijderen

Als u ze niet langer nodig hebt, kunt u alle resources die u in deze snelstartgids hebt gemaakt verwijderen door de resourcegroep te verwijderen:

```powershell
Remove-AzResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

Nu u uw eerste DNS-zone en -record hebt gemaakt met behulp van Azure PowerShell, kunt u records voor een web-app maken in een aangepast domein.

> [!div class="nextstepaction"]
> [DNS-records voor een web-app in een aangepast domein maken](./dns-web-sites-custom-domain.md)

