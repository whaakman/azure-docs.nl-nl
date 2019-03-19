---
title: Zones voor omgekeerde DNS-zoekacties in Azure DNS hosten | Microsoft Docs
description: Meer informatie over het gebruik van Azure DNS voor het hosten van de zones voor omgekeerde DNS-zoekacties voor de IP-adresbereiken
services: dns
documentationcenter: na
author: vhorne
manager: jeconnoc
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: victorh
ms.openlocfilehash: cb2f04c692d4b5f385a89ba6a3071c20ef1bdf21
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58118252"
---
# <a name="host-reverse-dns-lookup-zones-in-azure-dns"></a>Omgekeerde DNS-lookup-zones hosten in Azure DNS

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

In dit artikel wordt uitgelegd hoe u de zones voor omgekeerde DNS-zoekacties voor de toegewezen IP-adresbereiken in Azure DNS hosten. De IP-adresbereiken die wordt vertegenwoordigd door de zones voor reverse lookup moeten worden toegewezen aan uw organisatie, meestal door uw Internetprovider.

Omgekeerde DNS-server voor een Azure-eigendom IP-adres dat toegewezen aan uw Azure-service configureren, Zie [configureren reverse-DNS voor services die worden gehost in Azure](dns-reverse-dns-for-azure-services.md).

Voordat u dit artikel leest, moet u bekend bent met de [overzicht omgekeerde DNS-en ondersteuning in Azure](dns-reverse-dns-overview.md).

In dit artikel begeleidt u bij de stappen voor het maken van uw eerste DNS-zone voor reverse lookup en -record met behulp van de Azure portal, Azure PowerShell, Azure klassieke CLI of Azure CLI.

## <a name="create-a-reverse-lookup-dns-zone"></a>Maken van een DNS-zone voor reverse lookup

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Op de **Hub** in het menu **nieuw** > **netwerken**, en selecteer vervolgens **DNS-zone**.

   ![Selectie van de 'DNS-zone'](./media/dns-reverse-dns-hosting/figure1.png)

1. In de **DNS-zone maken** deelvenster naam van uw DNS-zone. De naam van de zone is anders voor IPv4 en IPv6-voorvoegsels die zijn ontworpen. Volg de instructies voor [IPv4](#ipv4) of [IPv6](#ipv6) om de naam van de zone. Wanneer u klaar bent, selecteert u **maken** om de zone te maken.

### <a name="ipv4"></a>IPv4

De naam van een IPv4-zone voor reverse lookup is gebaseerd op de IP-adresbereik dat wordt vertegenwoordigd. Deze moet de volgende indeling: `<IPv4 network prefix in reverse order>.in-addr.arpa`. Zie voor voorbeelden van [overzicht omgekeerde DNS-en ondersteuning in Azure](dns-reverse-dns-overview.md#ipv4).

> [!NOTE]
> Wanneer u classless zones voor omgekeerde DNS-zoekacties in Azure DNS maakt, moet u een koppelteken (`-`) in plaats van een slash (`/`) in de naam van de zone.
>
> Bijvoorbeeld, voor de IP-bereik 192.0.2.128/26, moet u `128-26.2.0.192.in-addr.arpa` als de naam van de zone in plaats van `128/26.2.0.192.in-addr.arpa`.
>
> Hoewel de DNS-standaarden beide methoden ondersteunen, Azure DNS DNS-zonenamen bevatten biedt geen ondersteuning voor schuine streep (`/`) teken.

Het volgende voorbeeld ziet over het maken van een omgekeerde DNS-zone van klasse C, met de naam `2.0.192.in-addr.arpa` in Azure DNS via Azure portal:

 !['DNS-zone maken' in het deelvenster met vakken ingevuld](./media/dns-reverse-dns-hosting/figure2.png)

**Locatie voor resourcegroep** definieert de locatie voor de resourcegroep. Dit heeft geen invloed op de DNS-zone. De locatie van de DNS-zone is altijd 'global' en wordt niet weergegeven.

De volgende voorbeelden laten zien hoe deze taak uitvoeren met behulp van Azure PowerShell en Azure CLI.

#### <a name="powershell"></a>PowerShell

```powershell
New-AzDnsZone -Name 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-classic-cli"></a>Klassieke versie van Azure CLI

```azurecli
azure network dns zone create MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli"></a>Azure-CLI

```azurecli
az network dns zone create -g MyResourceGroup -n 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a>IPv6

De naam van een IPv6-zone voor reverse lookup moet de volgende notatie: `<IPv6 network prefix in reverse order>.ip6.arpa`.  Zie voor voorbeelden van [overzicht omgekeerde DNS-en ondersteuning in Azure](dns-reverse-dns-overview.md#ipv6).


Het volgende voorbeeld laat zien hoe u een IPv6 reverse DNS-lookup-zone maken met de naam `0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa` in Azure DNS via Azure portal:

 !['DNS-zone maken' in het deelvenster met vakken ingevuld](./media/dns-reverse-dns-hosting/figure3.png)

**Locatie voor resourcegroep** definieert de locatie voor de resourcegroep. Dit heeft geen invloed op de DNS-zone. De locatie van de DNS-zone is altijd 'global' en wordt niet weergegeven.

De volgende voorbeelden laten zien hoe deze taak uitvoeren met behulp van Azure PowerShell en Azure CLI.

#### <a name="powershell"></a>PowerShell

```powershell
New-AzDnsZone -Name 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-classic-cli"></a>Klassieke versie van Azure CLI

```azurecli
azure network dns zone create MyResourceGroup 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azure-cli"></a>Azure-CLI

```azurecli
az network dns zone create -g MyResourceGroup -n 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="delegate-a-reverse-dns-lookup-zone"></a>Een DNS-zone voor reverse lookup delegeren

Nu dat u uw DNS-zone voor reverse lookup hebt gemaakt, moet u ervoor zorgen dat de zone van de bovenliggende zone wordt overgedragen. DNS-delegering kunt het DNS-naamomzettingsproces te vinden van de naamservers die uw DNS-zone voor reverse lookup hosten. Deze naamservers kunnen u omgekeerde DNS-query's voor de IP-adressen in het adresbereik vervolgens beantwoorden.

Zones voor forward lookup, het proces voor het overdragen van een DNS-zone is beschreven in [uw domein delegeren naar Azure DNS](dns-delegate-domain-azure-dns.md). Delegering voor zones voor reverse lookup werkt op dezelfde manier. Het enige verschil is dat u wilt de naamservers configureren met de Internet-provider die uw IP-adresbereik, in plaats van uw domeinnaamregistrar.

## <a name="create-a-dns-ptr-record"></a>Een DNS PTR-record maken

### <a name="ipv4"></a>IPv4

Het volgende voorbeeld wordt uitgelegd hoe u voor het maken van een PTR-record in een omgekeerde DNS-zone in Azure DNS. Voor andere typen en voor het wijzigen van bestaande records, raadpleegt u [Manage DNS records and record sets by using the Azure portal](dns-operations-recordsets-portal.md) (DNS-records en -recordsets beheren met behulp van Azure Portal).

1. Aan de bovenkant van de **DNS-zone** venster **+ Recordset** openen de **recordset toevoegen** deelvenster.

   ![Knop voor het maken van een Recordset](./media/dns-reverse-dns-hosting/figure4.png)

1. De naam van de recordset voor een PTR-record moet de rest van het IPv4-adres in omgekeerde volgorde. 

   In dit voorbeeld worden de eerste drie octetten al ingevuld als onderdeel van de naam van de zone (.2.0.192). Daarom alleen het laatste achttal werd is opgegeven in de **naam** vak. Bijvoorbeeld: naam van de recordset **15** voor een resource met IP-adres 192.0.2.15 is.  
1. Voor **Type**, selecteer **PTR**.  
1. Voor **domeinnaam**, voer de volledig gekwalificeerde domeinnaam (FQDN) van de resource die gebruikmaakt van het IP-adres.
1. Selecteer **OK** opnemen onder aan het deelvenster voor het maken van de DNS-server.

   !['Recordset toevoegen' in het deelvenster met vakken ingevuld](./media/dns-reverse-dns-hosting/figure5.png)

De volgende voorbeelden laten zien hoe deze taak uitvoeren met behulp van PowerShell of Azure CLI.

#### <a name="powershell"></a>PowerShell

```powershell
New-AzDnsRecordSet -Name 15 -RecordType PTR -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -Ptrdname "dc1.contoso.com")
```
#### <a name="azure-classic-cli"></a>Klassieke versie van Azure CLI

```azurecli
azure network dns record-set add-record MyResourceGroup 2.0.192.in-addr.arpa 15 PTR --ptrdname dc1.contoso.com  
```

#### <a name="azure-cli"></a>Azure-CLI

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 2.0.192.in-addr.arpa -n 15 --ptrdname dc1.contoso.com
```

### <a name="ipv6"></a>IPv6

Het volgende voorbeeld wordt uitgelegd hoe u voor het maken van nieuwe PTR-record. Voor andere typen en voor het wijzigen van bestaande records, raadpleegt u [Manage DNS records and record sets by using the Azure portal](dns-operations-recordsets-portal.md) (DNS-records en -recordsets beheren met behulp van Azure Portal).

1. Aan de bovenkant van de **DNS-zone** venster **+ Recordset** openen de **recordset toevoegen** deelvenster.

   ![Knop voor het maken van een Recordset](./media/dns-reverse-dns-hosting/figure6.png)

2. De naam van de recordset voor een PTR-record moet de rest van het IPv6-adres in omgekeerde volgorde. Het mag geen nul gecomprimeerd bevatten. 

   In dit voorbeeld worden de eerste 64 bits van het IPv6-al ingevuld als onderdeel van de naam van de zone (0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa). Daarom alleen de laatste 64 bits zijn opgegeven in de **naam** vak. De laatste 64-bits van het IP-adres zijn in omgekeerde volgorde, met een specifieke periode als het scheidingsteken tussen elke hexadecimaal nummer ingevoerd. Bijvoorbeeld: naam van de recordset **e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f** voor een resource met IP-adres 2001:0db8:abdc:0000:f524:10bc:1af9:405e is.  
3. Voor **Type**, selecteer **PTR**.  
4. Voor **domeinnaam**, voer de FQDN-naam van de resource die gebruikmaakt van het IP-adres.
5. Selecteer **OK** opnemen onder aan het deelvenster voor het maken van de DNS-server.

!['Recordset toevoegen' in het deelvenster met vakken ingevuld](./media/dns-reverse-dns-hosting/figure7.png)

De volgende voorbeelden laten zien hoe deze taak uitvoeren met behulp van PowerShell of Azure CLI.

#### <a name="powershell"></a>PowerShell

```powershell
New-AzDnsRecordSet -Name "e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f" -RecordType PTR -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzDnsRecordConfig -Ptrdname "dc2.contoso.com")
```

#### <a name="azure-classic-cli"></a>Klassieke versie van Azure CLI

```
azure network dns record-set add-record MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f PTR --ptrdname dc2.contoso.com 
```
 
#### <a name="azure-cli"></a>Azure-CLI

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -n e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f --ptrdname dc2.contoso.com
```

## <a name="view-records"></a>Records weergeven

Als u wilt weergeven van de records die u hebt gemaakt, blader naar de DNS-zone in Azure portal. In het onderste gedeelte van de **DNS-zone** deelvenster ziet u de records voor de DNS-zone. U ziet de standaard NS en SOA-records, plus alle nieuwe records die u hebt gemaakt. De NS en SOA-records worden in elke zone gemaakt. 

### <a name="ipv4"></a>IPv4

De **DNS-zone** deelvenster ziet u het IPv4-PTR-records:

![Deelvenster 'DNS-zone' met IPv4-records](./media/dns-reverse-dns-hosting/figure8.png)

De volgende voorbeelden laten zien hoe de PTR-records weergeven met behulp van PowerShell of Azure CLI.

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzDnsRecordSet -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-classic-cli"></a>Klassieke versie van Azure CLI

```azurecli
    azure network dns record-set list MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli"></a>Azure-CLI

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a>IPv6

De **DNS-zone** deelvenster ziet u de IPv6-PTR-records:

![Deelvenster 'DNS-zone' met IPv6-records](./media/dns-reverse-dns-hosting/figure9.png)

De volgende voorbeelden laten zien hoe de records weergeven met behulp van PowerShell of Azure CLI.

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzDnsRecordSet -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-classic-cli"></a>Klassieke versie van Azure CLI

```azurecli
    azure network dns record-set list MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azure-cli"></a>Azure-CLI

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="faq"></a>Veelgestelde vragen

### <a name="can-i-host-reverse-dns-lookup-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a>Kan ik voor mijn ISP-toegewezen IP-adresblokken op Azure DNS-zones voor omgekeerde DNS-zoekacties hosten?

Ja. Die als host fungeert voor de zones voor reverse lookup (ARPA) voor uw eigen IP-adresbereiken in Azure DNS wordt volledig ondersteund.

De zone voor reverse lookup maken in Azure DNS, zoals in dit artikel wordt beschreven, en werk vervolgens verder met de Internet-provider [delegeren van de zone](dns-domain-delegation.md). U kunt vervolgens de PTR-records voor elke reverse lookup beheren op dezelfde manier als andere recordtypen.

### <a name="how-much-does-hosting-my-reverse-dns-lookup-zone-cost"></a>Hoeveel gebruikt die als host fungeert voor de omgekeerde DNS-lookup zone kosten?

Hosting van de DNS-zone voor reverse lookup voor uw Internet-provider toegewezen IP-Adresblok in Azure DNS wordt in rekening gebracht tegen [tarieven voor Azure DNS](https://azure.microsoft.com/pricing/details/dns/).

### <a name="can-i-host-reverse-dns-lookup-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a>Kan ik de zones voor omgekeerde DNS-zoekacties voor zowel IPv4 als IPv6-adressen in Azure DNS hosten?

Ja. In dit artikel wordt uitgelegd hoe u zowel IPv4 als IPv6 DNS-zones voor omgekeerde zoekacties in Azure DNS maakt.

### <a name="can-i-import-an-existing-reverse-dns-lookup-zone"></a>Kan ik een bestaande zone voor reverse DNS-lookup importeren?

Ja. U kunt Azure CLI gebruiken om te importeren van bestaande DNS-zones in Azure DNS. Deze methode werkt voor zowel zones voor forward lookup en zones voor reverse lookup.

Zie voor meer informatie, [importeren en exporteren van een DNS-zonebestand met behulp van Azure CLI](dns-import-export.md).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over omgekeerde DNS [omgekeerde DNS-zoekactie voor Wikipedia](https://en.wikipedia.org/wiki/Reverse_DNS_lookup).
<br>
Meer informatie over het [omgekeerde DNS-records voor uw Azure-services beheren](dns-reverse-dns-for-azure-services.md).
