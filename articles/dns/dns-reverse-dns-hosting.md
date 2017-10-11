---
title: Hosting-zones voor reverse DNS-lookup in Azure DNS | Microsoft Docs
description: Informatie over het gebruik van Azure DNS voor het hosten van de zones voor reverse DNS-lookup voor uw IP-adresbereiken
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: jonatul
ms.openlocfilehash: 3e10b25d2f9b91c96af2958fef6dc6a4fdbff301
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="hosting-reverse-dns-lookup-zones-in-azure-dns"></a>Zones voor reverse DNS-lookup in Azure DNS hosten

In dit artikel wordt uitgelegd hoe de zones voor reverse DNS-lookup voor uw toegewezen IP-adresbereiken in Azure DNS hosten. De IP-adresbereiken dat wordt vertegenwoordigd door de zone voor reverse lookup moeten worden toegewezen aan uw organisatie gewoonlijk door uw Internetprovider.

Omgekeerde DNS-server voor Azure die eigendom zijn van IP-adres die zijn toegewezen aan uw Azure-service configureren, Zie [de reverse lookup voor de IP-adressen toegewezen aan uw Azure-service configureren](dns-reverse-dns-for-azure-services.md).

Voordat u dit artikel leest, moet u bekend bent met dit [overzicht van de reverse DNS- en biedt ondersteuning in Azure](dns-reverse-dns-overview.md).

Dit artikel begeleidt u bij de stappen voor het maken van uw eerste reverse lookup DNS-zone en vastleggen met behulp van de Azure-portal, Azure PowerShell, Azure CLI 1.0 of 2.0 voor Azure CLI.

## <a name="create-a-reverse-lookup-dns-zone"></a>DNS-zone voor reverse lookup te maken

1. Aanmelden bij de [Azure-portal](https://portal.azure.com)
1. Klik in het menu Hub en op **nieuw** > **Networking** > en klik vervolgens op **DNS-zone** openen de **maken DNS-zone** blade.

   ![DNS-zone](./media/dns-reverse-dns-hosting/figure1.png)

1. Op de **maken DNS-zone** blade naam van uw DNS-zone. De naam van de zone is anders vervaardigde voor IPv4 en IPv6-voorvoegsels. Gebruik van de instructies voor [IPV4](#ipv4) of [IPv6](#ipv6) als naam voor uw zone. Klik wanneer u klaar **maken** om de zone te maken.

### <a name="ipv4"></a>IPv4

De naam van een IPv4-zone voor reverse lookup is gebaseerd op het IP-adresbereik dat wordt vertegenwoordigd. Deze moet de volgende indeling: `<IPv4 network prefix in reverse order>.in-addr.arpa`. Zie voor voorbeelden [overzicht van de reverse DNS- en biedt ondersteuning in Azure](dns-reverse-dns-overview.md#ipv4).

> [!NOTE]
> Wanneer u een omgekeerde DNS-lookup-zones in Azure DNS maakt, moet u een koppelteken (`-`) in plaats van met een slash ('/ ') in de zonenaam.
>
> Bijvoorbeeld: voor het IP-bereik 192.0.2.128/26, moet u `128-26.2.0.192.in-addr.arpa` als de naam van de zone in plaats van `128/26.2.0.192.in-addr.arpa`.
>
> Dit is omdat beide worden wel ondersteund door de DNS-standaarden, DNS-zone-namen met een slash (`/`) tekens worden niet ondersteund in Azure DNS.

Het volgende voorbeeld laat zien hoe 'Klasse C' omgekeerde DNS-zone maken met de naam `2.0.192.in-addr.arpa` in Azure DNS via de Azure-portal:

 ![DNS-zone maken](./media/dns-reverse-dns-hosting/figure2.png)

'Locatie van de resourcegroep' definieert de locatie voor de resourcegroep en heeft geen invloed op de DNS-zone. De DNS-zone-locatie is altijd 'global' en niet wordt weergegeven.

De volgende voorbeelden laten zien hoe u dit moet doen met Azure PowerShell en de Azure CLI:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmDnsZone -Name 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
azure network dns zone create MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
az network dns zone create -g MyResourceGroup -n 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a>IPv6

De naam van een IPv6-zone voor reverse lookup moet de volgende notatie: `<IPv6 network prefix in reverse order>.ip6.arpa`.  Zie voor voorbeelden [overzicht van de reverse DNS- en biedt ondersteuning in Azure](dns-reverse-dns-overview.md#ipv6).


Het volgende voorbeeld laat zien hoe een IPv6 reverse DNS-lookup zone maken met de naam `0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa` in Azure DNS via de Azure-portal:

 ![DNS-zone maken](./media/dns-reverse-dns-hosting/figure3.png)

'Locatie van de resourcegroep' definieert de locatie voor de resourcegroep en heeft geen invloed op de DNS-zone. De DNS-zone-locatie is altijd 'global' en niet wordt weergegeven.

De volgende voorbeelden laten zien hoe u dit moet doen met Azure PowerShell en de Azure CLI:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmDnsZone -Name 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azurecli-10"></a>AzureCLI 1.0

```azurecli
azure network dns zone create MyResourceGroup 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azurecli-20"></a>AzureCLI 2.0

```azurecli
az network dns zone create -g MyResourceGroup -n 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="delegate-a-reverse-dns-lookup-zone"></a>Een DNS-zone voor reverse lookup delegeren

Uw DNS-zone voor reverse lookup die worden gemaakt, moet u ervoor zorgen dat de zone van de bovenliggende zone is overgedragen. DNS-delegering kunt de DNS-naamomzettingsproces de naamservers die als host fungeert voor uw DNS-zone voor reverse lookup te vinden. Hierdoor kunnen deze naamservers om omgekeerde DNS-query's voor de IP-adressen in uw-adresbereik te beantwoorden.

Het proces van het delegeren van een DNS-zone wordt voor zones voor forward lookup beschreven in [uw domein delegeren naar Azure DNS](dns-delegate-domain-azure-dns.md). Delegering voor zones voor reverse lookup werkt op dezelfde manier. Het enige verschil is dat u de naamservers configureren met de ISP die uw IP-adresbereik, in plaats van uw domeinnaamregistrar.

## <a name="create-a-dns-ptr-record"></a>Een DNS PTR-record maken

### <a name="ipv4"></a>IPv4

Het volgende voorbeeld wordt uitgelegd hoe u een PTR-record maken in een reverse DNS-zone in Azure DNS. Voor andere typen en voor het wijzigen van bestaande records, raadpleegt u [Manage DNS records and record sets by using the Azure portal](dns-operations-recordsets-portal.md) (DNS-records en -recordsets beheren met behulp van Azure Portal).

1.  Selecteer boven in de blade **DNS-zone** de optie **+ Recordset** om de blade **Recordset toevoegen** te openen.

 ![DNS-zone](./media/dns-reverse-dns-hosting/figure4.png)

1. Op de **recordset toevoegen** blade. 
1. Selecteer **PTR** uit de record '**Type**' menu.  
1. De naam van de recordset voor een PTR-record moet de rest van het IPv4-adres in omgekeerde volgorde. In dit voorbeeld worden de eerste drie octetten al ingevuld als onderdeel van de naam van de zone (.2.0.192). Daarom is alleen het laatste octet opgegeven in het naamveld. U kunt uw recordset bijvoorbeeld de naam '**15**' voor een resource waarvan IP-adres 192.0.2.15 is.  
1. In de '**domeinnaam**"en voer de volledig gekwalificeerde domeinnaam (FQDN) van de resource met behulp van het IP-adres.
1. Selecteer onder in de blade **OK** om de DNS-record te maken.

 ![recordset toevoegen](./media/dns-reverse-dns-hosting/figure5.png)

Hier volgen enkele voorbeelden voor het voltooien van deze taak met PowerShell en de AzureCLI:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmDnsRecordSet -Name 15 -RecordType PTR -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "dc1.contoso.com")
```
#### <a name="azurecli-10"></a>AzureCLI 1.0

```azurecli
azure network dns record-set add-record MyResourceGroup 2.0.192.in-addr.arpa 15 PTR --ptrdname dc1.contoso.com  
```

#### <a name="azurecli-20"></a>AzureCLI 2.0

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 2.0.192.in-addr.arpa -n 15 --ptrdname dc1.contoso.com
```

### <a name="ipv6"></a>IPv6

Het volgende voorbeeld wordt u begeleid het proces van het nieuwe 'PTR-record te maken. Voor andere typen en voor het wijzigen van bestaande records, raadpleegt u [Manage DNS records and record sets by using the Azure portal](dns-operations-recordsets-portal.md) (DNS-records en -recordsets beheren met behulp van Azure Portal).

1. Aan de bovenkant van de **DNS-zone blade**, selecteer **+ Recordset** openen de **recordset toevoegen** blade.

  ![DNS-zone-blade](./media/dns-reverse-dns-hosting/figure6.png)

2. Op de **recordset toevoegen** blade. 
3. Selecteer **PTR** uit de record '**Type**' menu.  
4. De naam van de recordset voor een PTR-record moet de rest van het IPv6-adres in omgekeerde volgorde. Het mag geen nul gecomprimeerd bevatten. In dit voorbeeld worden de eerste 64 bits van het IPv6-al ingevuld als onderdeel van de naam van de zone (0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa). Daarom worden alleen de laatste 64 bits zijn opgegeven in het naamveld. De laatste 64 bits van het IP-adres worden ingevoerd in omgekeerde volgorde, met een punt als scheidingsteken tussen elke hexadecimaal getal. U kunt uw recordset bijvoorbeeld de naam '**e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f**' voor een resource waarvan IP-adres 2001:0db8:abdc:0000:f524:10bc:1af9:405e is.  
5. In de '**domeinnaam**"en voer de volledig gekwalificeerde domeinnaam (FQDN) van de resource met behulp van het IP-adres.
6. Selecteer onder in de blade **OK** om de DNS-record te maken.

![Recordset blade toevoegen](./media/dns-reverse-dns-hosting/figure7.png)

Hier volgen enkele voorbeelden voor het voltooien van deze taak met PowerShell en de AzureCLI:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmDnsRecordSet -Name "e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f" -RecordType PTR -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "dc2.contoso.com")
```

#### <a name="azurecli-10"></a>AzureCLI 1.0

```
azure network dns record-set add-record MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f PTR --ptrdname dc2.contoso.com 
```
 
#### <a name="azurecli-20"></a>AzureCLI 2.0

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -n e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f --ptrdname dc2.contoso.com
```

## <a name="view-records"></a>Records weergeven

De records die u hebt gemaakt wilt weergeven, gaat u naar de DNS-zone in de Azure portal. In het onderste gedeelte van de **DNS-zone** blade ziet u de records voor de DNS-zone. U ziet de standaardrecords NS en SOA, die in elke zone worden gemaakt, plus alle nieuwe records die u hebt gemaakt.

### <a name="ipv4"></a>IPv4

DNS-zone blade IPv4 PTR-records worden weergegeven:

![DNS-zone-blade](./media/dns-reverse-dns-hosting/figure8.png)

De volgende voorbeelden laten zien hoe de PTR-records met PowerShell of Azure CLI weergeven:

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzureRmDnsRecordSet -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
    azure network dns record-set list MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a>IPv6

DNS-zone blade IPv6 PTR-records worden weergegeven:

![DNS-zone-blade](./media/dns-reverse-dns-hosting/figure9.png)

Hier volgen enkele voorbeelden voor het weergeven van de records met PowerShell en de AzureCLI:

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzureRmDnsRecordSet -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
    azure network dns record-set list MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="faq"></a>Veelgestelde vragen

### <a name="can-i-host-reverse-dns-lookup-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a>Kan ik voor mijn ISP toegewezen IP-adresblokken op Azure DNS zones voor reverse DNS-lookup hosten?

Ja. De zones voor reverse lookup (ARPA) voor uw eigen IP-adresbereiken in Azure DNS hosten wordt volledig ondersteund.

De zone voor reverse lookup maakt in Azure DNS als uiteengezet in dit artikel en werk uw internetprovider [de zone delegeren](dns-domain-delegation.md).  U kunt de PTR-records voor elke reverse lookup beheren op dezelfde manier als andere recordtypen.

### <a name="how-much-does-hosting-my-reverse-dns-lookup-zone-cost"></a>Hoeveel gebruikt die als host fungeert voor mijn omgekeerde DNS-lookup zone kosten?

De DNS-zone voor reverse lookup als host fungeert voor uw Internetprovider toegewezen IP-Adresblok in Azure DNS in rekening volgens gebracht [standaardtarieven Azure DNS](https://azure.microsoft.com/pricing/details/dns/).

### <a name="can-i-host-reverse-dns-lookup-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a>Kan ik zones voor reverse DNS-lookup voor IPv4 en IPv6-adressen in Azure DNS hosten?

Ja. Dit artikel wordt uitgelegd hoe u zowel IPv4 als IPv6 reverse DNS-zones lookup maakt in Azure DNS.

### <a name="can-i-import-an-existing-reverse-dns-lookup-zone"></a>Kan ik een bestaande zone voor reverse DNS-lookup importeren?

Ja. Bestaande DNS-zones importeren in Azure DNS kunt u de Azure CLI. Dit werkt voor zowel zones voor forward lookup en zones voor reverse lookup.

Zie voor meer informatie [importeren en exporteren van een DNS-zone-bestand met de Azure CLI](dns-import-export.md).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over omgekeerde DNS [achterwaartse DNS-zoekopdracht op Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).
<br>
Meer informatie over hoe [omgekeerde DNS-records voor uw Azure-services beheren](dns-reverse-dns-for-azure-services.md).
