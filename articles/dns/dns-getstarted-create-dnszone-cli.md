---
title: Een DNS-zone maken met CLI| Microsoft Docs
description: Informatie over het maken van DNS-zones in Azure DNS. Dit is een stapsgewijze handleiding voor het maken en beheren van uw eerste DNS-zone met behulp van de Azure-CLI.
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 1514426a-133c-491a-aa27-ee0962cea9dc
ms.service: dns
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/21/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: f156e4d4c5ffddb7e93ebf21baa75864e0e260e9
ms.openlocfilehash: d00792a4bb19e194dbbcee8b9c11e4a744891388

---

# <a name="create-an-azure-dns-zone-using-cli"></a>Een Azure DNS-zone maken met CLI

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-create-dnszone-portal.md)
> * [PowerShell](dns-getstarted-create-dnszone.md)
> * [Azure-CLI](dns-getstarted-create-dnszone-cli.md)

Dit artikel begeleidt u stapsgewijs door de procedure voor het maken van een DNS-zone met behulp van de platformoverschrijdende Azure-CLI die beschikbaar is voor Windows, Mac en Linux. U kunt ook een DNS-zone met [Azure PowerShell](dns-getstarted-create-dnszone.md) of [Azure Portal](dns-getstarted-create-dnszone-portal.md) maken.

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-cli-setup](../../includes/dns-cli-setup-include.md)]


## <a name="create-a-dns-zone"></a>Een DNS-zone maken

Een DNS-zone wordt gemaakt met de opdracht `azure network dns zone create`. Typ `azure network dns zone create -h` om Help weer te geven voor deze opdracht.

In het volgende voorbeeld maakt u een DNS-zone met de naam *contoso.com* in de resourcegroep *MyResourceGroup*. Gebruik het voorbeeld om een DNS-zone te maken door de waarden te vervangen door uw eigen waarden.

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```

## <a name="verify-your-dns-zone"></a>Uw DNS-zone controleren

### <a name="view-records"></a>Records weergeven

Wanneer u een DNS-zone maakt, wordt de volgende DNS-records gemaakt:

* De *Start of Authority*-record (SOA). Deze record is aanwezig in de hoofdmap van elke DNS-zone.
* De gezaghebbende naamserverrecords (NS). Deze records laten zien welke servers de zone hosten. Azure DNS maakt gebruik van een pool met naamservers, waardoor er verschillende naamservers kunnen worden toegewezen aan verschillende zones in Azure DNS. Zie [Een domein delegeren naar Azure DNS](dns-domain-delegation.md) voor meer informatie.

Gebruik `azure network dns-record-set list` om deze records weer te geven:

```azurecli
azure network dns record-set list MyResourceGroup contoso.com

info:    Executing command network dns record-set list
+ Looking up the DNS Record Sets
data:    Name                            : @
data:    Type                            : NS
data:    TTL                             : 172800
data:    Records:
data:      ns1-01.azure-dns.com.
data:      ns2-01.azure-dns.net.
data:      ns3-01.azure-dns.org.
data:      ns4-01.azure-dns.info.
data:
data:    Name                            : @
data:    Type                            : SOA
data:    TTL                             : 3600
data:    Email                           : azuredns-hostmaster.microsoft.com
data:    Host                            : ns1-01.azure-dns.com.
data:    Serial Number                   : 2
data:    Refresh Time                    : 3600
data:    Retry Time                      : 300
data:    Expire Time                     : 2419200
data:    Minimum TTL                     : 300
data:
info:    network dns record-set list command OK
```

> [!NOTE]
> Recordsets in de hoofdmap (of *apex*) van een DNS-zone gebruiken **@** als recordsetnaam.

### <a name="test-name-servers"></a>Naamservers testen

U kunt testen of uw DNS-zone aanwezig is op de Azure DNS-naamservers door gebruik te maken van DNS-hulpprogramma's, zoals nslookup, dig of de PowerShell-cmdlet `Resolve-DnsName`.

Als u uw domein nog niet hebt gedelegeerd voor het gebruik van de nieuwe zone in Azure DNS, moet u de DNS-query rechtstreeks op een van de naamservers voor uw zone richten. De naamservers voor de zone worden vermeld in de NS-records verstrekt door `azure network dns record-set list`.

In het volgende voorbeeld wordt met 'dig' een query op het domein contoso.com uitgevoerd, waarbij gebruik wordt gemaakt van de naamservers die zijn toegewezen aan de DNS-zone. Zorg ervoor dat u de juiste waarden voor uw zone vervangt.

     > dig @ns1-01.azure-dns.com contoso.com
     
     <<>> DiG 9.10.2-P2 <<>> @ns1-01.azure-dns.com contoso.com
    (1 server found)
    global options: +cmd
     Got answer:
    ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 60963
     flags: qr aa rd; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1
     WARNING: recursion requested but not available

     OPT PSEUDOSECTION:
     EDNS: version: 0, flags:; udp: 4000
      QUESTION SECTION:
    contoso.com.                        IN      A

     AUTHORITY SECTION:
    contoso.com.         3600     IN      SOA     ns1-01.azure-dns.com. azuredns-hostmaster.microsoft.com. 1 3600 300 2419200 300

    Query time: 93 msec
    SERVER: 208.76.47.5#53(208.76.47.5)
    WHEN: Tue Jul 21 16:04:51 Pacific Daylight Time 2015
    MSG SIZE  rcvd: 120

## <a name="next-steps"></a>Volgende stappen

Na het maken van een DNS-zone, [maakt u DNS-recordsets en -records](dns-getstarted-create-recordset-cli.md) in uw zone.




<!--HONumber=Dec16_HO3-->


