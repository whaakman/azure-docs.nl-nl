---
title: DNS-zones in Azure DNS - Azure CLI 2.0 beheren | Microsoft Docs
description: U kunt DNS-zones met Azure CLI 2.0 beheren. In dit artikel laat zien hoe bijwerken, verwijderen en DNS-zones maken op Azure DNS.
services: dns
documentationcenter: na
author: KumudD
manager: timlt
ms.assetid: 8ab63bc4-5135-4ed8-8c0b-5f0712b9afed
ms.service: dns
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: kumud
ms.openlocfilehash: 2042d9c2864a4f8da474e0df38882414bfe3417e
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="how-to-manage-dns-zones-in-azure-dns-using-the-azure-cli-20"></a>Het beheren van DNS-Zones in Azure DNS met de Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)


Deze handleiding laat zien hoe uw DNS-zones beheren met behulp van de platformoverschrijdende Azure CLI, die beschikbaar is voor Windows, Mac en Linux. U kunt ook beheren met behulp van DNS-zones [Azure PowerShell](dns-operations-dnszones.md) of de Azure-portal.

## <a name="introduction"></a>Inleiding

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="set-up-azure-cli-20-for-azure-dns"></a>Azure CLI 2.0 instellen voor Azure DNS

### <a name="before-you-begin"></a>Voordat u begint

Controleer voordat u met de configuratie begint of u de volgende items hebt.

* Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of [u aanmelden voor een gratis account](https://azure.microsoft.com/pricing/free-trial/).

* Installeer de nieuwste versie van de Azure CLI 2.0, beschikbaar voor Windows, Linux of MAC. Ga naar [Azure-CLI 2.0 installeren](https://docs.microsoft.com/cli/azure/install-az-cli2) voor meer informatie.

### <a name="sign-in-to-your-azure-account"></a>Aanmelden bij uw Azure-account

Open een consolevenster en doorloop de verificatie met uw referenties. Zie Aanmelden bij Azure vanaf de Azure CLI voor meer informatie

```
az login
```

### <a name="select-the-subscription"></a>Het abonnement selecteren

Controleer de abonnementen voor het account.

```
az account list
```

Kies welk Azure-abonnement u wilt gebruiken.

```azurecli
az account set --subscription "subscription name"
```

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Azure Resource Manager vereist dat er voor alle resourcegroepen een locatie wordt opgegeven. Deze locatie wordt gebruikt als de standaardlocatie voor resources in die resourcegroep. Aangezien alle DNS-resources globaal en niet regionaal zijn, is de keuze van de locatie voor de resourcegroep niet van invloed op Azure DNS.

U kunt deze stap overslaan als u een bestaande resourcegroep gebruikt.

```azurecli
az group create --name myresourcegroup --location "West US"
```

## <a name="getting-help"></a>Help opvragen

Alle 2.0 CLI-opdrachten met betrekking tot de Azure DNS beginnen met `az network dns`. Help beschikbaar is voor elke opdracht met behulp van de `--help` optie (korte versie `-h`).  Bijvoorbeeld:

```azurecli
az network dns --help
az network dns zone --help
az network dns zone create --help
```

## <a name="create-a-dns-zone"></a>Een DNS-zone maken

Een DNS-zone wordt gemaakt met de opdracht `az network dns zone create`. Zie `az network dns zone create -h` voor help.

Het volgende voorbeeld wordt een DNS-zone aangeroepen *contoso.com* aangeroepen in de resourcegroep *MyResourceGroup*:

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com
```

### <a name="to-create-a-dns-zone-with-tags"></a>Een DNS-zone maken met labels

Het volgende voorbeeld laat zien hoe een DNS-zone maken met twee [Azure Resource Manager-tags](dns-zones-records.md#tags), *project = demo* en *env = test*, met behulp van de `--tags` parameter (korte versie `-t`):

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com --tags "project=demo" "env=test"
```

## <a name="get-a-dns-zone"></a>Ophalen van een DNS-zone

Gebruik voor het ophalen van een DNS-zone `az network dns zone show`. Zie `az network dns zone show --help` voor help.

Het volgende voorbeeld retourneert de DNS-zone *contoso.com* en de bijbehorende gegevens van de resourcegroep *MyResourceGroup*. 

```azurecli
az network dns zone show --resource-group myresourcegroup --name contoso.com
```

Het volgende voorbeeld is het antwoord.

```json
{
  "etag": "00000002-0000-0000-3d4d-64aa3689d201",
  "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "contoso.com",
  "nameServers": [
    "ns1-04.azure-dns.com.",
    "ns2-04.azure-dns.net.",
    "ns3-04.azure-dns.org.",
    "ns4-04.azure-dns.info."
  ],
  "numberOfRecordSets": 4,
  "resourceGroup": "myresourcegroup",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

Houd er rekening mee dat DNS-records worden niet geretourneerd door `az network dns zone show`. DNS-records weergeven door gebruiken `az network dns record-set list`.


## <a name="list-dns-zones"></a>Lijst met DNS-zones

Gebruik voor het inventariseren van DNS-zones `az network dns zone list`. Zie `az network dns zone list --help` voor help.

Het opgeven van de resourcegroep bevat alleen deze zones binnen de resourcegroep:

```azurecli
az network dns zone list --resource-group MyResourceGroup
```

Als de resourcegroep wordt weggelaten, worden alle zones in het abonnement:

```azurecli
az network dns zone list 
```

## <a name="update-a-dns-zone"></a>Bijwerken van een DNS-zone

Een DNS-zone-bron kan worden gewijzigd met behulp van `az network dns zone update`. Zie `az network dns zone update --help` voor help.

Met deze opdracht wordt de DNS-recordsets binnen de zone niet bijgewerkt (Zie [het beheren van DNS-records](dns-operations-recordsets-cli.md)). Het wordt alleen gebruikt om de eigenschappen van de bron van de zone zelf te werken. Deze eigenschappen zijn momenteel beperkt tot de [Azure Resource Manager 'labels'](dns-zones-records.md#tags) voor de zoneresource.

Het volgende voorbeeld laat zien hoe de labels voor een DNS-zone bijwerken. De bestaande labels worden vervangen door de opgegeven waarde.

```azurecli
az network dns zone update --resource-group myresourcegroup --name contoso.com --set tags.team=support
```

## <a name="delete-a-dns-zone"></a>Een DNS-zone verwijderen

DNS-zones worden verwijderd met een `az network dns zone delete`. Zie `az network dns zone delete --help` voor help.

> [!NOTE]
> Een DNS-zone verwijderen, verwijdert tevens alle DNS-records in de zone. Deze bewerking kan niet ongedaan worden gemaakt. Als de DNS-zone gebruikt wordt, worden services met behulp van de zone mislukt wanneer de zone wordt verwijderd.
>
>Als u wilt beveiligen tegen het onbedoeld zone verwijderen, Zie [het beveiligen van DNS-zones en records](dns-protect-zones-recordsets.md).

Met deze opdracht vraagt om bevestiging. De optionele `--yes` schakeloptie deze vraag wordt geblokkeerd.

Het volgende voorbeeld laat zien hoe de zone verwijderen *contoso.com* uit resourcegroep *MyResourceGroup*.

```azurecli
az network dns zone delete --resource-group myresourcegroup --name contoso.com
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [beheren recordsets en records](dns-getstarted-create-recordset-cli.md) in uw DNS-zone.

Meer informatie over hoe [uw domein delegeren naar Azure DNS](dns-domain-delegation.md).

