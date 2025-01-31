---
title: Beheren van DNS-zones in Azure DNS - Azure CLI | Microsoft Docs
description: U kunt DNS-zones met behulp van Azure CLI kunt beheren. In dit artikel laat zien hoe bijwerken, verwijderen en het maken van DNS-zones in Azure DNS.
services: dns
documentationcenter: na
author: vhorne
manager: timlt
ms.assetid: 8ab63bc4-5135-4ed8-8c0b-5f0712b9afed
ms.service: dns
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: victorh
ms.openlocfilehash: df741b34e1268c547723af87401760197d395780
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61293826"
---
# <a name="how-to-manage-dns-zones-in-azure-dns-using-the-azure-cli"></a>Over het beheren van DNS-Zones in Azure DNS met behulp van de Azure CLI

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure-CLI](dns-operations-dnszones-cli.md)


Deze handleiding wordt beschreven hoe u uw DNS-zones beheren met behulp van de platformoverschrijdende Azure CLI, die beschikbaar is voor Windows, Mac en Linux. U kunt ook beheren met uw DNS-zones met behulp van [Azure PowerShell](dns-operations-dnszones.md) of de Azure-portal.

Deze handleiding behandelt specifiek de openbare DNS-zones. Zie voor meer informatie over het gebruik van Azure CLI voor het beheren van Privézones in Azure DNS [aan de slag met Azure DNS Private Zones met behulp van Azure CLI](private-dns-getstarted-cli.md).

## <a name="introduction"></a>Inleiding

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="set-up-azure-cli-for-azure-dns"></a>Set up Azure CLI for Azure DNS (Azure CLI instellen voor Azure DNS)

### <a name="before-you-begin"></a>Voordat u begint

Controleer voordat u met de configuratie begint of u de volgende items hebt.

* Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of [u aanmelden voor een gratis account](https://azure.microsoft.com/pricing/free-trial/).

* Installeer de nieuwste versie van de Azure CLI. Deze is beschikbaar voor Windows, Linux en MAC. Lees [Azure-CLI installeren](https://docs.microsoft.com/cli/azure/install-az-cli2) voor meer informatie.

### <a name="sign-in-to-your-azure-account"></a>Aanmelden bij uw Azure-account

Open een consolevenster en doorloop de verificatie met uw referenties. Raadpleeg [Log in to Azure from the Azure CLI (Aanmelden bij Azure vanaf de Azure CLI)](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest) voor meer informatie.

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

### <a name="optional-to-installuse-azure-dns-private-zones-feature-public-preview"></a>Optioneel: De functie Azure DNS Private Zones installeren (openbare preview)
De functie Azure DNS Private Zones wordt uitgebracht in een openbare preview via een extensie van Azure CLI. De extensie dns van Azure CLI installeren 
```
az extension add --name dns
``` 

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Azure Resource Manager vereist dat er voor alle resourcegroepen een locatie wordt opgegeven. Deze locatie wordt gebruikt als de standaardlocatie voor resources in die resourcegroep. Aangezien alle DNS-resources globaal en niet regionaal zijn, is de keuze van de locatie voor de resourcegroep niet van invloed op Azure DNS.

U kunt deze stap overslaan als u een bestaande resourcegroep gebruikt.

```azurecli
az group create --name myresourcegroup --location "West US"
```

## <a name="getting-help"></a>Ondersteuning vragen

Alle Azure-CLI-opdrachten met betrekking tot Azure DNS beginnen met `az network dns`. Help-informatie is beschikbaar voor elke opdracht met behulp van de `--help` optie (kort formulier `-h`).  Bijvoorbeeld:

```azurecli
az network dns --help
az network dns zone --help
az network dns zone create --help
```

## <a name="create-a-dns-zone"></a>Een DNS-zone maken

Een DNS-zone wordt gemaakt met de opdracht `az network dns zone create`. Zie `az network dns zone create -h` voor help.

Het volgende voorbeeld wordt een DNS-zone met de naam *contoso.com* in de resourcegroep met de naam *MyResourceGroup*:

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com
```

### <a name="to-create-a-dns-zone-with-tags"></a>Een DNS-zone maken met tags

Het volgende voorbeeld laat zien hoe een DNS-zone maken met twee [tags van Azure Resource Manager](dns-zones-records.md#tags), *project = demo* en *env = test*, met behulp van de `--tags` parameter ( Kort formulier `-t`):

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com --tags "project=demo" "env=test"
```

## <a name="get-a-dns-zone"></a>Een DNS-zone ophalen

Als u wilt ophalen van een DNS-zone, gebruikt u `az network dns zone show`. Zie `az network dns zone show --help` voor help.

Het volgende voorbeeld retourneert de DNS-zone *contoso.com* en de bijbehorende gegevens vanuit resourcegroep *MyResourceGroup*. 

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

DNS-records worden niet door `az network dns zone show` geretourneerd. Gebruik `az network dns record-set list` als u DNS-records wilt vermelden.


## <a name="list-dns-zones"></a>DNS-zones vermelden

Gebruik `az network dns zone list` als u een opsomming wilt maken van DNS-zones. Zie `az network dns zone list --help` voor help.

Bij het opgeven van de resourcegroep worden alleen de zones in de resourcegroep vermeld:

```azurecli
az network dns zone list --resource-group MyResourceGroup
```

Als de resourcegroepen worden weggelaten, worden alle zones in het abonnement vermeld:

```azurecli
az network dns zone list 
```

## <a name="update-a-dns-zone"></a>Een DNS-zone bijwerken

Wijzigingen aan een DNS-zoneresource kunnen met `az network dns zone update` worden gemaakt. Zie `az network dns zone update --help` voor help.

Met deze opdracht worden geen DNS-recordsets in de zone bijgewerkt (zie [DNS-records beheren](dns-operations-recordsets-cli.md)). De opdracht wordt alleen gebruikt voor het bijwerken van eigenschappen van de zoneresource zelf. Deze eigenschappen zijn momenteel beperkt tot de [Azure Resource Manager-tags'](dns-zones-records.md#tags) voor de zoneresource.

Het volgende voorbeeld ziet hoe u de tags van een DNS-zone bijwerken. De bestaande tags zijn vervangen door de opgegeven waarde.

```azurecli
az network dns zone update --resource-group myresourcegroup --name contoso.com --set tags.team=support
```

## <a name="delete-a-dns-zone"></a>Een DNS-zone verwijderen

DNS-zones kunnen worden verwijderd met `az network dns zone delete`. Zie `az network dns zone delete --help` voor help.

> [!NOTE]
> Als u een DNS-zone verwijdert, worden ook alle DNS-records binnen de zone verwijderd. Deze bewerking kan niet ongedaan worden gemaakt. Als de DNS-zone in gebruik is en de zone wordt verwijderd, werken services die van de zone gebruik maken niet langer.
>
>Zie [DNS-zones en records beschermen](dns-protect-zones-recordsets.md) om beveiliging in te stellen tegen het per ongeluk verwijderen van zones.

Met deze opdracht wordt om bevestiging gevraagd. De optionele `--yes`-schakelaar onderdrukt deze vraag.

Het volgende voorbeeld laat zien hoe de zone verwijderen *contoso.com* uit resourcegroep *MyResourceGroup*.

```azurecli
az network dns zone delete --resource-group myresourcegroup --name contoso.com
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [beheren recordsets en records](dns-getstarted-create-recordset-cli.md) in uw DNS-zone.

Meer informatie over het [uw domein delegeren naar Azure DNS](dns-domain-delegation.md).

