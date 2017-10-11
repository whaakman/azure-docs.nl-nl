---
title: DNS-zones in Azure DNS - Azure CLI 1.0 beheren | Microsoft Docs
description: U kunt DNS-zones met behulp van Azure CLI 1.0 beheren. In dit artikel laat zien hoe bijwerken, verwijderen en DNS-zones maken op Azure DNS.
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 8ab63bc4-5135-4ed8-8c0b-5f0712b9afed
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/21/2016
ms.author: gwallace
ms.openlocfilehash: 588c87749f049eff5b9e0729f6769c8367ba41e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-dns-zones-in-azure-dns-using-the-azure-cli-10"></a>Het beheren van DNS-Zones in Azure DNS met de Azure CLI 1.0

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)

Deze handleiding laat zien hoe uw DNS-zones beheren met behulp van de platformoverschrijdende Azure CLI 1.0, die beschikbaar is voor Windows, Mac en Linux. U kunt ook beheren met behulp van DNS-zones [Azure PowerShell](dns-operations-dnszones.md) of de Azure-portal.

## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren

U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

* [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md): onze CLI voor het klassieke implementatiemodel en het Resource Manager-implementatiemodel.
* [Azure CLI 2.0](dns-operations-dnszones-cli.md): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel.

## <a name="introduction"></a>Inleiding

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-cli-setup](../../includes/dns-cli-setup-include.md)]

## <a name="getting-help"></a>Help opvragen

Alle 1.0 CLI-opdrachten met betrekking tot de Azure DNS beginnen met `azure network dns`. Help beschikbaar is voor elke opdracht met behulp van de `--help` optie (korte versie `-h`).  Bijvoorbeeld:

```azurecli
azure network dns -h
azure network dns zone -h
azure network dns zone create -h
```

## <a name="create-a-dns-zone"></a>Een DNS-zone maken

Een DNS-zone wordt gemaakt met de opdracht `azure network dns zone create`. Zie `azure network dns zone create -h` voor help.

Het volgende voorbeeld wordt een DNS-zone aangeroepen *contoso.com* aangeroepen in de resourcegroep *MyResourceGroup*:

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```

### <a name="to-create-a-dns-zone-with-tags"></a>Een DNS-zone maken met labels

Het volgende voorbeeld laat zien hoe een DNS-zone maken met twee [Azure Resource Manager-tags](dns-zones-records.md#tags), *project = demo* en *env = test*, met behulp van de `--tags` parameter (korte versie `-t`):

```azurecli
azure network dns zone create MyResourceGroup contoso.com -t "project=demo";"env=test"
```

## <a name="get-a-dns-zone"></a>Ophalen van een DNS-zone

Gebruik voor het ophalen van een DNS-zone `azure network dns zone show`. Zie `azure network dns zone show -h` voor help.

Het volgende voorbeeld retourneert de DNS-zone *contoso.com* en de bijbehorende gegevens van de resourcegroep *MyResourceGroup*. 

```azurecli
azure network dns zone show MyResourceGroup contoso.com
```

Het volgende voorbeeld is het antwoord.

```
info:    Executing command network dns zone show
+ Looking up the dns zone "contoso.com"
data:    Id                              : /subscriptions/.../contoso.com
data:    Name                            : contoso.com
data:    Type                            : Microsoft.Network/dnszones
data:    Location                        : global
data:    Number of record sets           : 2
data:    Max number of record sets       : 5000
data:    Name servers:
data:        ns1-01.azure-dns.com.
data:        ns2-01.azure-dns.net.
data:        ns3-01.azure-dns.org.
data:        ns4-01.azure-dns.info.
data:    Tags                            : project=demo;env=test
info:    network dns zone show command OK
```

Houd er rekening mee dat DNS-records worden niet geretourneerd door `azure network dns zone show`. DNS-records weergeven door gebruiken `azure network dns record-set list`.


## <a name="list-dns-zones"></a>Lijst met DNS-zones

Gebruik voor het inventariseren van DNS-zones `azure network dns zone list`. Zie `azure network dns zone list -h` voor help.

Het opgeven van de resourcegroep bevat alleen deze zones binnen de resourcegroep:

```azurecli
azure network dns zone list MyResourceGroup
```

Als de resourcegroep wordt weggelaten, worden alle zones in het abonnement:

```azurecli
azure network dns zone list 
```

## <a name="update-a-dns-zone"></a>Bijwerken van een DNS-zone

Een DNS-zone-bron kan worden gewijzigd met behulp van `azure network dns zone set`. Zie `azure network dns zone set -h` voor help.

Met deze opdracht wordt de DNS-recordsets binnen de zone niet bijgewerkt (Zie [het beheren van DNS-records](dns-operations-recordsets-cli-nodejs.md)). Het wordt alleen gebruikt om de eigenschappen van de bron van de zone zelf te werken. Deze eigenschappen zijn momenteel beperkt tot de [Azure Resource Manager 'labels'](dns-zones-records.md#tags) voor de zoneresource.

Het volgende voorbeeld laat zien hoe de labels voor een DNS-zone bijwerken. De bestaande labels worden vervangen door de opgegeven waarde.

```azurecli
azure network dns zone set MyResourceGroup contoso.com -t "team=support"
```

## <a name="delete-a-dns-zone"></a>Een DNS-Zone verwijderen

DNS-zones worden verwijderd met een `azure network dns zone delete`. Zie `azure network dns zone delete -h` voor help.

> [!NOTE]
> Een DNS-zone verwijderen, verwijdert tevens alle DNS-records in de zone. Deze bewerking kan niet ongedaan worden gemaakt. Als de DNS-zone gebruikt wordt, worden services met behulp van de zone mislukt wanneer de zone wordt verwijderd.
>
>Als u wilt beveiligen tegen het onbedoeld zone verwijderen, Zie [het beveiligen van DNS-zones en records](dns-protect-zones-recordsets.md).

Met deze opdracht vraagt om bevestiging. De optionele `--quiet` gaan (korte versie `-q`) deze prompt onderdrukt.

Het volgende voorbeeld laat zien hoe de zone verwijderen *contoso.com* uit resourcegroep *MyResourceGroup*.

```azurecli
azure network dns zone delete MyResourceGroup contoso.com
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [beheren recordsets en records](dns-getstarted-create-recordset-cli-nodejs.md) in uw DNS-zone.

Meer informatie over hoe [uw domein delegeren naar Azure DNS](dns-domain-delegation.md).

