---
title: Een DNS-zone maken met Azure CLI 2.0 | Microsoft Docs
description: Informatie over het maken van DNS-zones in Azure DNS. Dit is een stapsgewijze handleiding voor het maken en beheren van uw eerste DNS-zone met behulp van de Azure-CLI 2.0.
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
ms.date: 02/27/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 1481fcb070f383d158c5a6ae32504e498de4a66b
ms.openlocfilehash: 00a4dfbe070129ce6dc84f006d793007d55dba3f
ms.lasthandoff: 03/01/2017

---

# <a name="create-an-azure-dns-zone-using-azure-cli-20"></a>Een Azure DNS-zone maken met Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-create-dnszone-portal.md)
> * [PowerShell](dns-getstarted-create-dnszone.md)
> * [Azure CLI 1.0](dns-getstarted-create-dnszone-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-create-dnszone-cli.md)

Dit artikel begeleidt u stapsgewijs door de procedure voor het maken van een DNS-zone met behulp van de platformoverschrijdende Azure-CLI die beschikbaar is voor Windows, Mac en Linux. U kunt ook een DNS-zone met [Azure PowerShell](dns-getstarted-create-dnszone.md) of [Azure Portal](dns-getstarted-create-dnszone-portal.md) maken.

## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren

U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

* [Azure CLI 1.0](dns-getstarted-create-dnszone-cli-nodejs.md): onze CLI voor het klassieke implementatiemodel en het Resource Manager-implementatiemodel.
* [Azure CLI 2.0](dns-getstarted-create-dnszone-cli.md): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel.

## <a name="introduction"></a>Inleiding

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="set-up-azure-cli-20-for-azure-dns"></a>Azure CLI 2.0 instellen voor Azure DNS

### <a name="before-you-begin"></a>Voordat u begint

Controleer voordat u met de configuratie begint of u de volgende items hebt.

* Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of [u aanmelden voor een gratis account](https://azure.microsoft.com/pricing/free-trial/).

* Installeer de nieuwste versie van de Azure CLI. Deze is beschikbaar voor Windows, Linux en MAC. Ga naar [Azure-CLI 2.0 installeren](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2) voor meer informatie.

### <a name="sign-in-to-your-azure-account"></a>Aanmelden bij uw Azure-account

Open een consolevenster en doorloop de verificatie met uw referenties. Zie Aanmelden bij Azure vanaf de Azure CLI voor meer informatie

```azurecli
az login
```

### <a name="select-the-subscription"></a>Het abonnement selecteren

Controleer de abonnementen voor het account.

```azurecli
az account list
```

### <a name="choose-which-of-your-azure-subscriptions-to-use"></a>Kies welk Azure-abonnement u wilt gebruiken.

```azurecli
az account set --subscription "subscription name"
```

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Azure Resource Manager vereist dat er voor alle resourcegroepen een locatie wordt opgegeven. Deze locatie wordt gebruikt als de standaardlocatie voor resources in die resourcegroep. Aangezien alle DNS-resources globaal en niet regionaal zijn, is de keuze van de locatie voor de resourcegroep niet van invloed op Azure DNS.

U kunt deze stap overslaan als u een bestaande resourcegroep gebruikt.

```azurecli
az group create --name myresourcegroup --location "West US"
```

## <a name="create-a-dns-zone"></a>Een DNS-zone maken

Een DNS-zone wordt gemaakt met de opdracht `az network dns zone create`. Typ `az network dns zone create --help` om Help weer te geven voor deze opdracht.

In het volgende voorbeeld maakt u een DNS-zone met de naam *contoso.com* in de resourcegroep *MyResourceGroup*. Gebruik het voorbeeld om een DNS-zone te maken door de waarden te vervangen door uw eigen waarden.

```azurecli
az network dns zone create --resource-group myresourcegroup --name contoso.com
```

## <a name="verify-your-dns-zone"></a>Uw DNS-zone controleren

### <a name="view-records"></a>Records weergeven

Wanneer u een DNS-zone maakt, wordt de volgende DNS-records gemaakt:

* De *Start of Authority*-record (SOA). Deze record is aanwezig in de hoofdmap van elke DNS-zone.
* De gezaghebbende naamserverrecords (NS). Deze records laten zien welke servers de zone hosten. Azure DNS maakt gebruik van een pool met naamservers, waardoor er verschillende naamservers kunnen worden toegewezen aan verschillende zones in Azure DNS. Zie [Een domein delegeren naar Azure DNS](dns-domain-delegation.md) voor meer informatie.

Gebruik `az network dns record-set list` om deze records weer te geven:

```azurecli
az network dns record-set list --resource-group MyResourceGroup --zone-name contoso.com
```

Het antwoord voor `az network dns record-set list` wordt hieronder weergegeven:

```json
[
  {
    "etag": "510f4711-8b7f-414e-b8d5-c0383ea3d62e",
    "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@",
    "metadata": null,
    "name": "@",
    "nsRecords": [
      {
        "nsdname": "ns1-04.azure-dns.com."
      },
      {
        "nsdname": "ns2-04.azure-dns.net."
      },
      {
        "nsdname": "ns3-04.azure-dns.org."
      },
      {
        "nsdname": "ns4-04.azure-dns.info."
      }
    ],
    "resourceGroup": "myresourcegroup",
    "ttl": 172800,
    "type": "Microsoft.Network/dnszones/NS"
  },
  {
    "etag": "0e48e82f-f194-4d3e-a864-7e109a95c73a",
    "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/SOA/@",
    "metadata": null,
    "name": "@",
    "resourceGroup": "myresourcegroup",
    "soaRecord": {
      "email": "azuredns-hostmaster.microsoft.com",
      "expireTime": 2419200,
      "host": "ns1-04.azure-dns.com.",
      "minimumTtl": 300,
      "refreshTime": 3600,
      "retryTime": 300,
      "serialNumber": 1
    },
    "ttl": 3600,
    "type": "Microsoft.Network/dnszones/SOA"
  }
]
```

> [!NOTE]
> Recordsets in de hoofdmap (of *apex*) van een DNS-zone gebruiken **@** als recordsetnaam.

### <a name="test-name-servers"></a>Naamservers testen

U kunt testen of uw DNS-zone aanwezig is op de Azure DNS-naamservers door gebruik te maken van DNS-hulpprogramma's, zoals nslookup, dig of de PowerShell-cmdlet `Resolve-DnsName`.

Als u uw domein nog niet hebt gedelegeerd voor het gebruik van de nieuwe zone in Azure DNS, moet u de DNS-query rechtstreeks op een van de naamservers voor uw zone richten. De naamservers voor de zone worden vermeld in de NS-records verstrekt door `az network dns record-set list`.

In het volgende voorbeeld wordt met 'dig' een query op het domein contoso.com uitgevoerd, waarbij gebruik wordt gemaakt van de naamservers die zijn toegewezen aan de DNS-zone. Zorg ervoor dat u de juiste waarden voor uw zone vervangt.

```
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
```

## <a name="next-steps"></a>Volgende stappen

Na het maken van een DNS-zone, [maakt u DNS-recordsets en -records](dns-getstarted-create-recordset-cli.md) in uw zone.


