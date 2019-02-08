---
title: Een Azure DNS-subdomein met behulp van Azure PowerShell delegeren
description: Informatie over het overdragen van een Azure DNS-subdomein met behulp van Azure PowerShell.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 2/7/2019
ms.author: victorh
ms.openlocfilehash: 40b2a4d98e6269d9740856ba44c1043af75ce1b8
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55896675"
---
# <a name="delegate-an-azure-dns-subdomain-using-azure-powershell"></a>Een Azure DNS-subdomein met behulp van Azure PowerShell delegeren

Azure PowerShell kunt u een DNS-subdomein delegeren. Als u eigenaar van het domein contoso.com, kunt u bijvoorbeeld een subdomein met de naam delegeren *engineering* aan een andere, afzonderlijke zone die u afzonderlijk vanaf de zone contoso.com beheren kunt.

Als u liever, kunt u een subdomein met delegeren de [Azure Portal](delegate-subdomain.md).

> [!NOTE]
> Contoso.com wordt gebruikt als voorbeeld in dit artikel. Vervang uw eigen domeinnaam door contoso.com.

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

## <a name="prerequisites"></a>Vereisten

Voor het overdragen van een Azure DNS-subdomein, moet u eerst uw openbare domein naar Azure DNS delegeren. Zie [een domein delegeren naar Azure DNS](./dns-delegate-domain-azure-dns.md) voor instructies over het configureren van de naamservers voor overdracht. Nadat uw domein wordt overgedragen naar de Azure DNS-zone, kunt u een subdomein delegeren.

## <a name="create-a-zone-for-your-subdomain"></a>Maak een zone voor het subdomein

Maak eerst de zone voor de **engineering** subdomein.

`New-AzDnsZone -ResourceGroupName <resource group name> -Name engineering.contoso.com`

## <a name="note-the-name-servers"></a>Houd er rekening mee de naamservers

Noteer vervolgens de vier naamservers voor de engineering-subdomein.

`Get-AzDnsRecordSet -ZoneName engineering.contoso.com -ResourceGroupName <resource group name> -RecordType NS`

## <a name="create-a-test-record"></a>Een test-record maken

Maak een **A** record in de technische zone voor testdoeleinden.

   `New-AzDnsRecordSet -ZoneName engineering.contoso.com -ResourceGroupName <resource group name> -Name www -RecordType A -ttl 3600 -DnsRecords (New-AzDnsRecordConfig -IPv4Address 10.10.10.10)`.

## <a name="create-an-ns-record"></a>Een NS-record maken

Maak vervolgens een naam (naamserver)-record voor de **engineering** zone in de zone contoso.com.

```azurepowershell
$Records = @()
$Records += New-AzDnsRecordConfig -Nsdname <name server 1 noted previously>
$Records += New-AzDnsRecordConfig -Nsdname <name server 2 noted previously>
$Records += New-AzDnsRecordConfig -Nsdname <name server 3 noted previously>
$Records += New-AzDnsRecordConfig -Nsdname <name server 4 noted previously>
$RecordSet = New-AzDnsRecordSet -Name engineering -RecordType NS -ResourceGroupName <resource group name> -TTL 3600 -ZoneName contoso.com -DnsRecords $Records
```

## <a name="test-the-delegation"></a>De overdracht testen

Nslookup gebruiken voor het testen van de overdracht.

1. Open een PowerShell-venster.
2. Typ het volgende achter de opdrachtprompt `nslookup www.engineering.contoso.com.`
3. U moet een niet-bindend antwoord met het adres ontvangt **10.10.10.10**.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [reverse-DNS voor services die worden gehost in Azure configureren](dns-reverse-dns-for-azure-services.md).