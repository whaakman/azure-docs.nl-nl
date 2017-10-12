---
title: Aan de slag met Azure DNS met behulp van Azure CLI 1.0 | Microsoft Docs
description: Informatie over het maken van een DNS-zone en -record in Azure DNS. Dit is een stapsgewijze handleiding voor het maken en beheren van uw eerste DNS-zone en -record met behulp van de Azure CLI 1.0.
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: jonatul
ms.openlocfilehash: f7943b71bbd16c36df09436973d92539eb62b210
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-dns-using-azure-cli-10"></a>Aan de slag met Azure DNS met behulp van Azure-CLI 1.0

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Azure CLI 1.0](dns-getstarted-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-cli.md)

Dit artikel begeleidt u stapsgewijs door de procedure voor het maken van uw eerste DNS-zone en -record met behulp van de platformoverschrijdende Azure-CLI 1.0 die beschikbaar is voor Windows, Mac en Linux. U kunt deze stappen ook uitvoeren met de Azure-portal of Azure PowerShell.

Een DNS-zone wordt gebruikt om de DNS-records voor een bepaald domein te hosten. Als u uw domein wilt hosten in Azure DNS, moet u een DNS-zone maken voor die domeinnaam. Alle DNS-records voor uw domein worden vervolgens gemaakt binnen deze DNS-zone. Tot slot moet u de naamservers voor het domein configureren om de DNS-zone te publiceren naar internet. Deze stappen worden hieronder allemaal beschreven.

Bij deze instructies wordt ervan uitgegaan dat u Azure CLI 1.0 al hebt geïnstalleerd en bent aangemeld. Zie [How to manage DNS zones using Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) (DNS-zones beheren met behulp van Azure CLI 1.0) voor hulp.

## <a name="create-the-resource-group"></a>De resourcegroep maken

Voordat u de DNS-zone maakt, wordt er een resourcegroep gemaakt waartoe de DNS-zone gaat behoren. Hieronder ziet u de opdracht.

```azurecli
azure group create --name MyResourceGroup --location "West US"
```

## <a name="create-a-dns-zone"></a>Een DNS-zone maken

Een DNS-zone wordt gemaakt met de opdracht `azure network dns zone create`. Typ `azure network dns zone create -h` om Help weer te geven voor deze opdracht.

In het volgende voorbeeld maakt u een DNS-zone met de naam *contoso.com* in de resourcegroep *MyResourceGroup*. Gebruik het voorbeeld om een DNS-zone te maken door de waarden te vervangen door uw eigen waarden.

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```


## <a name="create-a-dns-record"></a>Een DNS-record maken

Gebruik de opdracht `azure network dns record-set add-record` om een DNS-record te maken. Zie `azure network dns record-set add-record -h` voor help.

In het volgende voorbeeld maakt u een record met de relatieve naam 'www' in de DNS-zone 'contoso.com' in de resourcegroep 'MyResourceGroup'. De volledig gekwalificeerde naam van de recordset is 'www.contoso.com'. Het recordtype is 'A', met IP-adres '1.2.3.4' en een standaard-TTL van 3600 seconden (1 uur).

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

Voor andere recordtypen, voor recordsets met meerdere records, voor andere TTL-waarden, en als u bestaande records wilt wijzigen, raadpleegt u [Manage DNS records and record sets using Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md) (DNS-records en -recordsets beheren met behulp van Azure CLI 1.0).


## <a name="view-records"></a>Records weergeven

Als u de DNS-records wilt weergeven in uw zone, gebruikt u:

```azurecli
azure network dns record-set list MyResourceGroup contoso.com
```


## <a name="update-name-servers"></a>Naamservers bijwerken

Wanneer uw DNS-zone en -records correct zijn ingesteld, moet u uw domeinnaam configureren voor het gebruik van de Azure DNS-naamservers . Op die manier kunnen andere gebruikers op internet uw DNS-records vinden.

De naamservers voor uw zone zijn gegeven door de opdracht `azure network dns zone show`:

```azurecli
azure network dns zone show MyResourceGroup contoso.com

info:    Executing command network dns zone show
+ Looking up the dns zone "contoso.com"
data:    Id                              : /subscriptions/a385a691-bd93-41b0-8084-8213ebc5bff7/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com
data:    Name                            : contoso.com
data:    Type                            : Microsoft.Network/dnszones
data:    Location                        : global
data:    Number of record sets           : 3
data:    Max number of record sets       : 5000
data:    Name servers:
data:        ns1-01.azure-dns.com.
data:        ns2-01.azure-dns.net.
data:        ns3-01.azure-dns.org.
data:        ns4-01.azure-dns.info.
data:    Tags                            :
info:    network dns zone show command OK
```

Deze naamservers moeten worden geconfigureerd met de domeinnaamregistrar (waar u de domeinnaam hebt gekocht). Uw registrar zal u de mogelijkheid bieden om de naamservers voor het domein in te stellen. Zie [Uw domein delegeren naar Azure DNS](dns-domain-delegation.md) voor meer informatie.

## <a name="delete-all-resources"></a>Alle resources verwijderen
 
Als u alle resources wilt verwijderen die u in dit artikel hebt gemaakt, voert u de volgende stappen uit:

```azurecli
azure group delete --name MyResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

Zie [Azure DNS Overview](dns-overview.md) (Overzicht van Azure DNS) voor meer informatie over Azure DNS.

Zie [Manage DNS zones in Azure DNS using Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) (DNS-zones in Azure DNS beheren met behulp van Azure CLI 1.0) voor meer informatie over het beheren van DNS-zones in Azure DNS.

Zie [Manage DNS records and record sets in Azure DNS using Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md) (DNS-records en -recordsets in Azure DNS beheren met behulp van Azure CLI 1.0) voor meer informatie over het beheren van DNS-records in Azure DNS.

