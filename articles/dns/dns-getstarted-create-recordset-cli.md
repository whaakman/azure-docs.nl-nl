---
title: Een recordset en records voor een DNS-zone maken met CLI| Microsoft Docs
description: Hostrecords voor Azure DNS maken. Recordsets en records instellen met CLI
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 02b897d3-e83b-4257-b96d-5c29aa59e843
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/09/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: bfbffe7843bc178cdf289c999925c690ab82e922
ms.openlocfilehash: e377f176fe24a8e7e42d409f86d6b0093ce5e7c4

---

# <a name="create-dns-record-sets-and-records-by-using-cli"></a>DNS-recordsets en records maken met CLI

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-create-recordset-portal.md)
> * [PowerShell](dns-getstarted-create-recordset.md)
> * [Azure CLI](dns-getstarted-create-recordset-cli.md)

In dit artikel wordt uitgelegd hoe u records en recordsets maakt met CLI. Als u dit wilt doen, moet u eerst goed begrijpen wat DNS-records en recordsets zijn.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

In deze sectie wordt beschreven hoe u DNS-records maakt in Azure DNS. In de voorbeelden wordt ervan uitgegaan dat u [de Azure-CLI al hebt geïnstalleerd en aangemeld, en een DNS-zone hebt gemaakt](dns-getstarted-create-dnszone-cli.md).

In alle voorbeelden op deze pagina wordt het DNS-recordtype A gebruikt. Zie [DNS-records en -recordsets beheren met behulp van de Azure-CLI](dns-operations-recordsets-cli.md) voor andere recordtypen en meer informatie over het beheren van DNS-records en -recordsets.

## <a name="create-a-record-set-and-record"></a>Een recordset en record maken

In deze sectie leert u hoe u een recordset en records maakt. In dit voorbeeld maakt u een recordset met de relatieve naam www in de DNS-zone contoso.com. De volledig gekwalificeerde naam van de records is www.contoso.com. Het recordtype is A en deTTL (Time to Live) is 60 seconden. Nadat deze stap is voltooid, hebt u een lege recordset gemaakt.

Als u een record in de apex van de zone wilt maken (in dit geval contoso.com), gebruikt u de recordnaam "@",, inclusief de aanhalingstekens. Dit is een algemene DNS-conventie.

### <a name="1-create-a-record-set"></a>1. Een recordset maken

Als het nieuwe record dezelfde naam en hetzelfde type als een bestaand record heeft, moet u het toevoegen aan de bestaande recordset. U kunt deze stap overslaan en doorgaan met [Records toevoegen](#add-records) hieronder. Als het nieuwe record een andere naam en een ander type dan alle bestaande records heeft, moet u een nieuwe recordset maken.

U kunt recordsets maken met behulp van de opdracht `azure network dns record-set create`. Zie `azure network dns record-set create -h` voor help.  

Als u een recordset maakt, moet u de recordsetnaam, de zone, de TTL (time to live) en het recordtype opgeven. 

```azurecli
azure network dns record-set create myresourcegroup contoso.com www A 60
```

Nadat deze stap is voltooid, hebt u een lege recordset (www) gemaakt. Als u de nieuwe www-recordset wilt gebruiken, moet u eerst records aan de set toevoegen.

### <a name="2-add-records"></a>2. Records toevoegen

U voegt records aan de recordsets toe met `azure network dns record-set add-record`. Zie `azure network dns record-set add-record -h` voor help.

De parameters voor het toevoegen van records aan een recordset variëren afhankelijk van het type recordset. Als u bijvoorbeeld een recordset van type A gebruikt, kunt alleen records opgeven met de parameter `-a <IPv4 address>`. Raadpleeg `azure network dns record-set add-record -h` om de parameters voor andere recordtypen weer te geven.

Gebruik de volgende opdracht om een A-record toe te voegen aan het hierboven gemaakt www-record:

```azurecli
azure network dns record-set add-record myresourcegroup contoso.com  www A  -a 1.2.3.4
```

### <a name="verify-name-resolution"></a>Naamomzetting verifiëren

U kunt testen of uw DNS-records aanwezig zijn op de Azure DNS-naamservers door gebruik te maken van DNS-hulpprogramma's, zoals nslookup, dig of de [Resolve-DnsName PowerShell-cmdlet](https://technet.microsoft.com/library/jj590781.aspx).

Als u uw domein nog niet hebt gedelegeerd voor het gebruik van de nieuwe zone in Azure DNS, moet u [de DNS-query rechtstreeks op een van de naamservers voor uw zone richten](dns-getstarted-create-dnszone.md#test-name-servers). Zorg ervoor dat u in de onderstaande opdracht de juiste waarden voor uw records opneemt.

    nslookup
    > set type=A
    > server ns1-01.azure-dns.com
    > www.contoso.com

    Server:  ns1-01.azure-dns.com
    Address:  40.90.4.1

    Name:    www.contoso.com
    Address:  1.2.3.4

## <a name="next-steps"></a>Volgende stappen

Leer hoe u [uw domeinnaam delegeert naar Azure DNS-naamservers](dns-domain-delegation.md).

Leer hoe u [DNS-zones beheert via de Azure-CLI](dns-operations-dnszones-cli.md).

Leer hoe u [DNS-records en -recordsets beheert via de Azure-CLI](dns-operations-recordsets-cli.md).




<!--HONumber=Dec16_HO2-->


