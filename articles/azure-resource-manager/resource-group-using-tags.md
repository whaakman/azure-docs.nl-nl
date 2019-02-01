---
title: Azure-resources voor logische organisatie tag | Microsoft Docs
description: Laat zien hoe tags om in te delen, facturering en het beheren van Azure-resources wilt toepassen.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 003a78e5-2ff8-4685-93b4-e94d6fb8ed5b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: AzurePortal
ms.devlang: na
ms.topic: conceptual
ms.date: 11/20/2018
ms.author: tomfitz
ms.openlocfilehash: d7f84e188324bcdb9fb1a26d701805978f1fd625
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55489390"
---
# <a name="use-tags-to-organize-your-azure-resources"></a>Tags gebruiken om uw Azure-resources te organiseren

[!INCLUDE [resource-manager-governance-tags](../../includes/resource-manager-governance-tags.md)]

U kunt tags toepassen op resources, moet de gebruiker schrijftoegang heeft tot dat resourcetype hebben. Voor tags toepassen op alle brontypen, gebruikt u de [Inzender](../role-based-access-control/built-in-roles.md#contributor) rol. Om toe te passen tags aan slechts één brontype, gebruikt u de inzendersrol voor die bron. Bijvoorbeeld: u kunt tags toepassen op virtuele machines, gebruiken de [Inzender voor virtuele machines](../role-based-access-control/built-in-roles.md#virtual-machine-contributor).

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="powershell"></a>PowerShell

Voor de voorbeelden in dit artikel hebt u versie 6.0 of hoger van Azure PowerShell nodig. Als u geen versie 6.0 of hoger, [werk uw versie bij](/powershell/azure/install-az-ps).

Gebruik het volgende om de bestaande tags van een *resourcegroep* te bekijken:

```azurepowershell-interactive
(Get-AzResourceGroup -Name examplegroup).Tags
```

Dat script retourneert de volgende indeling:

```powershell
Name                           Value
----                           -----
Dept                           IT
Environment                    Test
```

Gebruik het volgende om de bestaande tags te bekijken van een *resource met een opgegeven bron-id*:

```azurepowershell-interactive
(Get-AzResource -ResourceId /subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Storage/storageAccounts/<storage-name>).Tags
```

Of gebruik het volgende om de bestaande tags te bekijken van een *resource met een opgegeven naam en resourcegroep*:

```azurepowershell-interactive
(Get-AzResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
```

Gebruik het volgende om de *resourcegroepen met een specifieke tag* op te halen:

```azurepowershell-interactive
(Get-AzResourceGroup -Tag @{ Dept="Finance" }).ResourceGroupName
```

Gebruik het volgende om de *resources met een specifieke tag* op te halen:

```azurepowershell-interactive
(Get-AzResource -Tag @{ Dept="Finance"}).Name
```

Om op te halen *resources met een naam van een specifieke tag*, gebruiken:

```azurepowershell-interactive
(Get-AzResource -TagName Dept).Name
```

Wanneer u tags op een resource of resourcegroep toepast, overschrijft u de bestaande tags van die resource of resourcegroep. Als u een resource of resourcegroep hebt met bestaande tags, gaat u anders te werk dan wanneer dat niet het geval is.

Gebruik het volgende om tags toe te voegen aan een *resourcegroep zonder bestaande tags*:

```azurepowershell-interactive
Set-AzResourceGroup -Name examplegroup -Tag @{ Dept="IT"; Environment="Test" }
```

Als u tags wilt toevoegen aan een *resourcegroep met bestaande tags*, haalt u de bestaande tags op, voegt u de nieuwe tag toe en past u de tags opnieuw toe:

```azurepowershell-interactive
$tags = (Get-AzResourceGroup -Name examplegroup).Tags
$tags.Add("Status", "Approved")
Set-AzResourceGroup -Tag $tags -Name examplegroup
```

Gebruik het volgende om tags toe te voegen aan een *resource zonder bestaande tags*:

```azurepowershell-interactive
$r = Get-AzResource -ResourceName examplevnet -ResourceGroupName examplegroup
Set-AzResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceId $r.ResourceId -Force
```

Gebruik het volgende om tags toe te voegen aan een *resource met bestaande tags*:

```azurepowershell-interactive
$r = Get-AzResource -ResourceName examplevnet -ResourceGroupName examplegroup
$r.Tags.Add("Status", "Approved")
Set-AzResource -Tag $r.Tags -ResourceId $r.ResourceId -Force
```

Gebruik het volgende script als u alle tags van een resourcegroep op de bijbehorende resources wilt toepassen *zonder de bestaande tags voor de resources te behouden*:

```azurepowershell-interactive
$groups = Get-AzResourceGroup
foreach ($g in $groups)
{
    Get-AzResource -ResourceGroupName $g.ResourceGroupName | ForEach-Object {Set-AzResource -ResourceId $_.ResourceId -Tag $g.Tags -Force }
}
```

Gebruik het volgende script als u alle tags van een resourcegroep op de bijbehorende resources wilt toepassen *en de bestaande tags voor de resources die geen dubbele waarden zijn, wilt behouden*:

```azurepowershell-interactive
$group = Get-AzResourceGroup "examplegroup"
if ($null -ne $group.Tags) {
    $resources = Get-AzResource -ResourceGroupName $group.ResourceGroupName
    foreach ($r in $resources)
    {
        $resourcetags = (Get-AzResource -ResourceId $r.ResourceId).Tags
        if ($resourcetags)
        {
            foreach ($key in $group.Tags.Keys)
            {
                if (-not($resourcetags.ContainsKey($key)))
                {
                    $resourcetags.Add($key, $group.Tags[$key])
                }
            }
            Set-AzResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
        }
        else
        {
            Set-AzResource -Tag $group.Tags -ResourceId $r.ResourceId -Force
        }
    }
}
```

Geef een lege hash-tabel door om alle tags te verwijderen:

```azurepowershell-interactive
Set-AzResourceGroup -Tag @{} -Name examplegroup
```

## <a name="azure-cli"></a>Azure-CLI

Gebruik het volgende om de bestaande tags van een *resourcegroep* te bekijken:

```azurecli
az group show -n examplegroup --query tags
```

Dat script retourneert de volgende indeling:

```json
{
  "Dept"        : "IT",
  "Environment" : "Test"
}
```

Of, om te zien van de bestaande tags voor een *resource met een specifieke naam, type en resource-groep*, gebruiken:

```azurecli
az resource show -n examplevnet -g examplegroup --resource-type "Microsoft.Network/virtualNetworks" --query tags
```

Als u een verzameling resources lussen, wilt u weergeven van de resource van resource-ID. Verderop in dit artikel ziet u een volledig voorbeeld. Gebruik het volgende om de bestaande tags te bekijken van een *resource met een opgegeven bron-id*:

```azurecli
az resource show --id <resource-id> --query tags
```

Als u de resourcegroepen die een specifieke tag, gebruikt `az group list`:

```azurecli
az group list --tag Dept=IT
```

Als u alle resources die beschikt over een bepaalde tag en waarde, gebruikt `az resource list`:

```azurecli
az resource list --tag Dept=Finance
```

Wanneer u tags op een resource of resourcegroep toepast, overschrijft u de bestaande tags van die resource of resourcegroep. Als u een resource of resourcegroep hebt met bestaande tags, gaat u anders te werk dan wanneer dat niet het geval is.

Gebruik het volgende om tags toe te voegen aan een *resourcegroep zonder bestaande tags*:

```azurecli
az group update -n examplegroup --set tags.Environment=Test tags.Dept=IT
```

Gebruik het volgende om tags toe te voegen aan een *resource zonder bestaande tags*:

```azurecli
az resource tag --tags Dept=IT Environment=Test -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

Tags toevoegen aan een resource die al tags heeft, ophalen van de bestaande tags, opnieuw opmaken van die waarde en de bestaande en nieuwe labels toepassen:

```azurecli
jsonrtag=$(az resource show -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks" --query tags)
rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
az resource tag --tags $rt Project=Redesign -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

Gebruik het volgende script als u alle tags van een resourcegroep op de bijbehorende resources wilt toepassen *zonder de bestaande tags voor de resources te behouden*:

```azurecli
groups=$(az group list --query [].name --output tsv)
for rg in $groups
do
  jsontag=$(az group show -n $rg --query tags)
  t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')
  r=$(az resource list -g $rg --query [].id --output tsv)
  for resid in $r
  do
    az resource tag --tags $t --id $resid
  done
done
```

Alle tags wilt toepassen vanuit een resourcegroep op de bijbehorende resources en *behouden de bestaande tags voor de resources*, gebruik het volgende script:

```azurecli
groups=$(az group list --query [].name --output tsv)
for rg in $groups
do
  jsontag=$(az group show -n $rg --query tags)
  t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')
  r=$(az resource list -g $rg --query [].id --output tsv)
  for resid in $r
  do
    jsonrtag=$(az resource show --id $resid --query tags)
    rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
    az resource tag --tags $t$rt --id $resid
  done
done
```

## <a name="templates"></a>Sjablonen

[!INCLUDE [resource-manager-tags-in-templates](../../includes/resource-manager-tags-in-templates.md)]

## <a name="portal"></a>Portal

[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]

## <a name="rest-api"></a>REST-API

De Azure portal en PowerShell gebruik van de [Resource Manager REST API](https://docs.microsoft.com/rest/api/resources/) achter de schermen. Als u integreren in een andere omgeving-tagging wilt, kunt u labels krijgen met behulp van **ophalen** op de resource-ID en werk de set van labels met behulp van een **PATCH** aanroepen.

## <a name="tags-and-billing"></a>Tags en facturering

U kunt tags gebruiken voor het groeperen van uw factureringsgegevens. Bijvoorbeeld, als u meerdere VM's voor verschillende organisaties uitvoert, gebruikt u de tags aan het gebruik van de groep door kostenplaats. U kunt ook tags gebruiken voor het categoriseren van kosten door de runtimeomgeving, zoals het gebruik van de facturering voor virtuele machines die worden uitgevoerd in de productie-omgeving.

U kunt informatie over tags via ophalen de [gebruik van Azure-resources en RateCard APIs](../billing/billing-usage-rate-card-overview.md) of het gebruik door komma's gescheiden waarden (CSV)-bestand. Downloaden van het gebruiksbestand van de [Azure-Accountcentrum](https://account.azure.com/Subscriptions) of Azure-portal. Zie voor meer informatie, [downloaden of weergeven van uw Azure-factuur en de dagelijkse gebruiksgegevens](../billing/billing-download-azure-invoice-daily-usage-date.md). Bij het downloaden van het gebruiksbestand van het Azure-Accountcentrum, selecteer **versie 2**. Voor services die ondersteuning bieden voor tags met facturering, de labels worden weergegeven in de **Tags** kolom.

Zie voor REST API-bewerkingen, [Azure Billing REST API-verwijzing](/rest/api/billing/).

## <a name="next-steps"></a>Volgende stappen

* Niet alle resourcetypen ondersteunen tags. Om te bepalen als u een label op een resourcetype toepassen kunt, Zie [ondersteuning voor Azure-resources taggen](tag-support.md).
* U kunt beperkingen en conventies met behulp van aangepaste beleidsregels toepassen voor uw abonnement. Een beleid dat u definieert mogelijk dat alle resources een waarde hebben voor een bepaalde tag. Zie voor meer informatie, [wat is Azure Policy?](../azure-policy/azure-policy-introduction.md)
* Zie voor een inleiding tot het gebruik van de portal, [met behulp van de Azure-portal voor het beheren van uw Azure-resources](resource-group-portal.md).  
