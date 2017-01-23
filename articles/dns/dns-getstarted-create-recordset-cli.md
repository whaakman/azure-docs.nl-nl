---
title: DNS-records maken met behulp van Azure CLI | Microsoft Docs
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
ms.date: 12/21/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 18a21cdc0f9641356dfaf6f6d93edfcac11af210
ms.openlocfilehash: 790af1544ed86155f5f864f3914b5fd1c4f42f4b

---

# <a name="create-dns-records-using-the-azure-cli"></a>DNS-records maken met behulp van Azure CLI

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-create-recordset-portal.md)
> * [PowerShell](dns-getstarted-create-recordset.md)
> * [Azure CLI](dns-getstarted-create-recordset-cli.md)

In dit artikel wordt uitgelegd hoe u records en recordsets maakt met Azure CLI.

## <a name="introduction"></a>Inleiding

Voordat u DNS-records in DNS Azure maakt, leest u eerst hoe Azure DNS DNS-records organiseert in DNS-recordsets.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Zie [DNS-zones en -records](dns-zones-records.md) voor meer informatie over DNS-records in Azure DNS.

## <a name="create-a-record-set-and-record"></a>Een recordset en record maken

In deze sectie wordt beschreven hoe u DNS-records maakt in Azure DNS. In de voorbeelden wordt ervan uitgegaan dat u [de Azure-CLI al hebt geïnstalleerd en aangemeld, en een DNS-zone hebt gemaakt](dns-getstarted-create-dnszone-cli.md).

In alle voorbeelden op deze pagina wordt het DNS-recordtype A gebruikt. Zie [DNS-records en -recordsets beheren met behulp van de Azure-CLI](dns-operations-recordsets-cli.md) voor andere recordtypen en meer informatie over het beheren van DNS-records en -recordsets.

## <a name="create-a-dns-record"></a>Een DNS-record maken

Gebruik de opdracht `azure network dns record-set add-record` om een DNS-record te maken. Zie `azure network dns record-set add-record -h` voor help.

Bij het maken van een record, dient u de naam van de resourcegroep, de zonenaam, de naam van de recordset, het recordtype en de details van de record die wordt gemaakt op te geven.

Als de recordset nog niet bestaat, maakt u er met deze opdracht een. Als de recordset al bestaat, voegt u met deze opdracht de opgegeven record toe aan de bestaande recordset. 

Als er een nieuwe recordset wordt gemaakt, wordt een standaard time-to-live (TTL) van 3600 gebruikt. Zie [Manage DNS records in Azure DNS using the Azure CLI](dns-operations-recordsets-cli.md) (DNS-records beheren in Azure DNS met Azure CLI) voor instructies over het gebruiken van verschillende TTL's.

In het volgende voorbeeld maakt u een A-record met de naam *www* in de zone *contoso.com* in de resourcegroep *MyResourceGroup*. Het IP-adres van de A-record is *1.2.3.4*.

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

Als u een record in de apex van de zone wilt maken (in dit geval contoso.com), gebruikt u de recordnaam "@",, inclusief de aanhalingstekens:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com "@" A -a 1.2.3.4
```

De parameters die u gebruikt om de gegevens van de record op te geven, variëren afhankelijk van het type record. Voor een record van het type 'A' geeft u bijvoorbeeld het IPv4-adres met de parameter `-a <IPv4 address>` op. Raadpleeg `azure network dns record-set add-record -h` om de parameters voor andere recordtypen weer te geven. Zie [DNS-records en -recordsets beheren met behulp van Azure CLI](dns-operations-recordsets-cli.md) voor voorbeelden van elk recordtype.


## <a name="verify-name-resolution"></a>Naamomzetting verifiëren

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




<!--HONumber=Dec16_HO3-->


