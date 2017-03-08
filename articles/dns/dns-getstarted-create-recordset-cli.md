---
title: DNS-records maken met behulp van Azure CLI 2.0 | Microsoft Docs
description: Hostrecords voor Azure DNS maken. Recordsets en records instellen met Azure CLI 2.0
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 02b897d3-e83b-4257-b96d-5c29aa59e843
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 1481fcb070f383d158c5a6ae32504e498de4a66b
ms.openlocfilehash: ca713647a9f795d7803c32eac7fba9b5b6ac95cf
ms.lasthandoff: 03/01/2017

---

# <a name="how-to-create-dns-records-using-the-azure-cli-20"></a>DNS-records maken met behulp van Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-create-recordset-portal.md)
> * [PowerShell](dns-getstarted-create-recordset.md)
> * [Azure CLI 1.0](dns-getstarted-create-recordset-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-create-recordset-cli.md)

In dit artikel wordt uitgelegd hoe u records en recordsets maakt met Azure CLI.

## <a name="introduction"></a>Inleiding

Voordat u DNS-records in DNS Azure maakt, leest u eerst hoe Azure DNS DNS-records organiseert in DNS-recordsets.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Zie [DNS-zones en -records](dns-zones-records.md) voor meer informatie over DNS-records in Azure DNS.

## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren

U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

* [Azure CLI 1.0](dns-getstarted-create-recordset-cli-nodejs.md): onze CLI voor het klassieke implementatiemodel en het Resource Manager-implementatiemodel.
* [Azure CLI 2.0](dns-getstarted-create-recordset-cli.md): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel.

## <a name="create-a-record-set-and-record"></a>Een recordset en record maken

In deze sectie wordt beschreven hoe u DNS-records maakt in Azure DNS. In de voorbeelden wordt ervan uitgegaan dat u [de Azure-CLI 2.0 al hebt geïnstalleerd en aangemeld, en een DNS-zone hebt gemaakt](dns-getstarted-create-dnszone-cli.md).

In alle voorbeelden op deze pagina wordt het DNS-recordtype A gebruikt. Zie [DNS-records en -recordsets beheren met behulp van de Azure-CLI 2.0](dns-operations-recordsets-cli.md) voor andere recordtypen en meer informatie over het beheren van DNS-records en -recordsets.

## <a name="create-a-dns-record"></a>Een DNS-record maken

Gebruik de opdracht `az network dns record-set ? add` om een DNS-record te maken (waar is het recordtype?). Zie `az network dns record-set --help` voor help.

Bij het maken van een record, dient u de naam van de resourcegroep, de zonenaam, de naam van de recordset, het recordtype en de details van de record die wordt gemaakt op te geven.

Als de recordset nog niet bestaat, maakt u er met deze opdracht een. Als de recordset al bestaat, voegt u met deze opdracht de opgegeven record toe aan de bestaande recordset. 

Als er een nieuwe recordset wordt gemaakt, wordt een standaard time-to-live (TTL) van 3600 gebruikt. Zie [DNS-records beheren in Azure DNS met Azure CLI 2.0](dns-operations-recordsets-cli.md) voor instructies over het gebruik van verschillende TTL's.

In het volgende voorbeeld maakt u een A-record met de naam *www* in de zone *contoso.com* in de resourcegroep *MyResourceGroup*. Het IP-adres van de A-record is *1.2.3.4*.

```azurecli
az network dns record-set a create --resource-group myresourcegroup --zone-name contoso.com --record-set-name www --ipv4-address 1.2.3.4
```

Als u een recordset in de apex van de zone wilt maken (in dit geval ´contoso.com´), gebruikt u de recordnaam ´@´, inclusief de aanhalingstekens:

```azurecli
az network dns record-set a --resource-group myresourcegroup --zone-name contoso.com --record-set-name "@" --ipv4-address 1.2.3.4
```

De parameters die u gebruikt om de gegevens van de record op te geven, variëren afhankelijk van het type record. Voor een record van het type 'A' geeft u bijvoorbeeld het IPv4-adres met de parameter `--ipv4-address <IPv4 address>` op. Raadpleeg `az network dns record --help` om de parameters voor andere recordtypen weer te geven. Zie [DNS-records en -recordsets beheren met behulp van Azure CLI 2.0](dns-operations-recordsets-cli.md) voor voorbeelden van elk recordtype.


## <a name="verify-name-resolution"></a>Naamomzetting verifiëren

U kunt testen of uw DNS-records aanwezig zijn op de Azure DNS-naamservers door gebruik te maken van DNS-hulpprogramma's, zoals nslookup, dig of de [Resolve-DnsName PowerShell-cmdlet](https://technet.microsoft.com/library/jj590781.aspx).

Als u uw domein nog niet hebt gedelegeerd voor het gebruik van de nieuwe zone in Azure DNS, moet u [de DNS-query rechtstreeks op een van de naamservers voor uw zone richten](dns-getstarted-create-dnszone.md#test-name-servers). Zorg ervoor dat u in de onderstaande opdracht de juiste waarden voor uw records opneemt.

```
nslookup
> set type=A
> server ns1-01.azure-dns.com
> www.contoso.com

Server:  ns1-01.azure-dns.com
Address:  40.90.4.1

Name:    www.contoso.com
Address:  1.2.3.4
```

## <a name="next-steps"></a>Volgende stappen

Leer hoe u [uw domeinnaam delegeert naar Azure DNS-naamservers](dns-domain-delegation.md).

Meer informatie over [DNS-zones beheren via de Azure-CLI 2.0](dns-operations-dnszones-cli.md).

Meer informatie over [DNS-records en -recordsets beheren via de Azure-CLI 2.0](dns-operations-recordsets-cli.md).


