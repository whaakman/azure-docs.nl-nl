---
title: Aan de slag met Azure DNS Private Zones met behulp van Azure CLI 2.0 | Microsoft Docs
description: Informatie over het maken van een privé-DNS-zone en -record in Azure DNS. Dit is een stapsgewijze handleiding voor het maken en beheren van uw eerste privé-DNS-zone en -record met behulp van Azure CLI 2.0.
services: dns
documentationcenter: na
author: KumuD
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2018
ms.author: kumud
ms.openlocfilehash: d10f3201adb972468d8de7e66a940232ed19562d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
ms.locfileid: "30191446"
---
# <a name="get-started-with-azure-dns-private-zones-using-azure-cli-20"></a>Aan de slag met Azure DNS Private Zones met behulp van Azure CLI 2.0

> [!div class="op_single_selector"]
> * [PowerShell](private-dns-getstarted-powershell.md)
> * [Azure CLI 2.0](private-dns-getstarted-cli.md)

Dit artikel begeleidt u stapsgewijs door de procedure voor het maken van uw eerste privé-DNS-zone en -record met behulp van de platformoverschrijdende Azure-CLI 2.0 die beschikbaar is voor Windows, Mac en Linux. U kunt deze stappen ook uitvoeren met Azure PowerShell.

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

Een DNS-zone wordt gebruikt om de DNS-records voor een bepaald domein te hosten. Als u uw domein wilt hosten in Azure DNS, moet u een DNS-zone maken voor die domeinnaam. Alle DNS-records voor uw domein worden vervolgens gemaakt binnen deze DNS-zone. Als u een privé-DNS-zone wilt publiceren naar uw virtuele netwerk, geeft u de lijst op met virtuele netwerken die zijn toegestaan om records in de zone om te zetten.  Deze netwerken worden virtuele resolutienetwerken genoemd.  U kunt ook een virtueel netwerk opgeven waarvoor Azure DNS hostnaamrecords bewaart wanneer een virtuele machine wordt gemaakt of het IP-adres gewijzigd of vernietigd.  Dit wordt een virtueel registratienetwerk genoemd.

In deze instructies wordt aangenomen dat Azure CLI 2.0 is geïnstalleerd en dat u erbij bent aangemeld, en dat de vereiste CLI-extensie is geïnstalleerd die privézones ondersteunt. Zie [How to manage DNS zones using Azure CLI 2.0](dns-operations-dnszones-cli.md) (DNS-zones beheren met behulp van Azure CLI 2.0) voor hulp.

## <a name="to-installuse-azure-dns-private-zones-feature-public-preview"></a>De functie Azure DNS Private Zones installeren (openbare preview)
De functie Azure DNS Private Zones wordt uitgebracht in een openbare preview via een extensie van Azure CLI. De extensie dns van Azure CLI installeren 

```
az extension add --name dns
``` 

## <a name="create-the-resource-group"></a>De resourcegroep maken

Voordat u de DNS-zone maakt, wordt er een resourcegroep gemaakt waartoe de DNS-zone gaat behoren. Hieronder ziet u de opdracht.

```azurecli
az group create --name MyResourceGroup --location "West US"
```

## <a name="create-a-dns-private-zone"></a>Een privé-DNS-zone maken

Er wordt een privé-DNS-zone gemaakt met de opdracht `az network dns zone create`. Typ `az network dns zone create --help` om Help weer te geven voor deze opdracht.

In het volgende voorbeeld wordt in de resourcegroep *MyResourceGroup* een privé-DNS-zone gemaakt met de naam *contoso.local*. Deze wordt met behulp van de parameter resolution-vnets beschikbaar gemaakt (via een koppeling) in het virtuele netwerk *MyAzureVnet*. Gebruik het voorbeeld om een DNS-zone te maken door de waarden te vervangen door uw eigen waarden.

```azurecli
az network dns zone create -g MyResourceGroup -n contoso.local --zone-type Private --resolution-vnets MyAzureVnet
```

Opmerking: in bovenstaand voorbeeld behoort het virtuele netwerk MyAzureVnet tot dezelfde resourcegroep en hetzelfde abonnement als de privézone. Als u een koppeling wilt maken naar een virtueel netwerk dat tot een andere resourcegroep of een ander abonnement behoort, dient u voor de parameter resolution-vnets de volledige Azure Resource Manager-id op te geven in plaats van slechts de naam van het virtuele netwerk. 

Als Azure automatisch hostnaamrecords in de zone moet kunnen maken, gebruikt u de parameter *registration-vnets* in plaats van *resolution-vnets*.  Virtuele registratienetwerken zijn automatisch ingeschakeld voor omzetting.

```azurecli
az network dns zone create -g MyResourceGroup -n contoso.local --zone-type Private --registration-vnets MyAzureVnet
```

## <a name="create-a-dns-record"></a>Een DNS-record maken

Gebruik de opdracht `az network dns record-set [record type] add-record` om een DNS-record te maken. Voor hulp, bijvoorbeeld voor A-records, raadpleegt u `azure network dns record-set A add-record --help`.

In het volgende voorbeeld maakt u een record met de relatieve naam ip1 in de DNS-zone contoso.local in de resourcegroep MyResourceGroup. De volledig gekwalificeerde naam van de recordset is ip1.contoso.local. Het recordtype is A, met IP-adres 10.0.0.1.

```azurecli
az network dns record-set a add-record -g MyResourceGroup -z contoso.local -n ip1 -a 10.0.0.1
```

Voor andere recordtypen, voor recordsets met meerdere records, voor andere TTL-waarden, en als u bestaande records wilt wijzigen, raadpleegt u [Manage DNS records and record sets using Azure CLI 2.0](dns-operations-recordsets-cli.md) (DNS-records en -recordsets beheren met behulp van Azure CLI 2.0).

## <a name="view-records"></a>Records weergeven

Als u de DNS-records wilt weergeven in uw zone, gebruikt u:

```azurecli
az network dns record-set list -g MyResourceGroup -z contoso.com
```

## <a name="get-a-dns-private-zone"></a>Een privé-DNS-zone ophalen

Gebruik `az network dns zone show` als u een privé-DNS-zone wilt ophalen. Zie `az network dns zone show --help` voor help.

In het volgende voorbeeld wordt de DNS-zone *contoso.local* en de eraan gekoppelde gegevens vanuit resourcegroep *MyResourceGroup* geretourneerd. 

```azurecli
az network dns zone show --resource-group MyResourceGroup --name contoso.local
```

Het volgende voorbeeld is het antwoord.

```json
{
  "etag": "00000002-0000-0000-3d4d-64aa3689d201",
  "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/MyResourceGroup/providers/Microsoft.Network/dnszones/contoso.local",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "contoso.local",
  "nameServers": null,
  "numberOfRecordSets": 1,
  "registrationVirtualNetworks": [],
  "resolutionVirtualNetworks": [
    {
      "additionalProperties": {},
      "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/MyResourceGroup/providers/Microsoft.Network/virtualNetworks/MyAzureVnet",
      "resourceGroup": "MyResourceGroup"
    }
  ]
  "resourceGroup": "MyResourceGroup",
  "tags": {},
  "type": "Microsoft.Network/dnszones",
  "zoneType": "Private"
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

Met deze opdracht worden geen DNS-recordsets in de zone bijgewerkt (zie [DNS-records beheren](dns-operations-recordsets-cli.md)). De opdracht wordt alleen gebruikt voor het bijwerken van eigenschappen van de zoneresource zelf. Voor privézones kunt u de virtuele registratie- of resolutienetwerken bijwerken die aan een zone zijn gekoppeld. 

In het volgende voorbeeld wordt getoond hoe het virtuele resolutienetwerk wordt bijgewerkt dat is gekoppeld aan een privé-DNS-zone. Het bestaande, gekoppelde virtuele resolutienetwerk wordt vervangen door het nieuwe, opgegeven virtuele netwerk.

```azurecli
az network dns zone update --resource-group MyResourceGroup --name contoso.local --zone-type Private --resolution-vnets MyNewAzureVnet
```

## <a name="delete-a-dns-zone"></a>Een DNS-zone verwijderen

DNS-zones kunnen worden verwijderd met `az network dns zone delete`. Zie `az network dns zone delete --help` voor help.

> [!NOTE]
> Als u een DNS-zone verwijdert, worden ook alle DNS-records binnen de zone verwijderd. Deze bewerking kan niet ongedaan worden gemaakt. Als de DNS-zone in gebruik is en de zone wordt verwijderd, werken services die van de zone gebruik maken niet langer.
>
>Zie [DNS-zones en records beschermen](dns-protect-zones-recordsets.md) om beveiliging in te stellen tegen het per ongeluk verwijderen van zones.

Met deze opdracht wordt om bevestiging gevraagd. De optionele `--yes`-schakelaar onderdrukt deze vraag.

In het volgende voorbeeld wordt getoond hoe de zone *contoso.local* uit resourcegroep *MyResourceGroup* wordt verwijderd.

```azurecli
az network dns zone delete --resource-group myresourcegroup --name contoso.local
```

## <a name="delete-all-resources"></a>Alle resources verwijderen
 
Als u alle resources wilt verwijderen die u in dit artikel hebt gemaakt, voert u de volgende stappen uit:

```azurecli
az group delete --name MyResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

Zie [Azure DNS Overview](dns-overview.md) (Overzicht van Azure DNS) voor meer informatie over Azure DNS.

Zie [Manage DNS zones in Azure DNS using Azure CLI 2.0](dns-operations-dnszones-cli.md) (DNS-zones in Azure DNS beheren met behulp van Azure CLI 2.0) voor meer informatie over het beheren van DNS-zones in Azure DNS.

Zie [Manage DNS records and record sets in Azure DNS using Azure CLI 2.0](dns-operations-recordsets-cli.md) (DNS-records en -recordsets in Azure DNS beheren met behulp van Azure CLI 2.0) voor meer informatie over het beheren van DNS-records in Azure DNS.
