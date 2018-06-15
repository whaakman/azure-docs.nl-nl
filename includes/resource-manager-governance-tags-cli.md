---
title: bestand opnemen
description: bestand opnemen
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/20/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: b9484336add0719749e9f0af56bdd70fa3906ef5
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2018
ms.locfileid: "29532343"
---
U kunt twee tags toevoegen aan een resourcegroep met de [az groep update](/cli/azure/group#az_group_update) opdracht:

```azurecli-interactive
az group update -n myResourceGroup --set tags.Environment=Test tags.Dept=IT
```

Stel dat u wilt toevoegen van een derde label. Voer de opdracht opnieuw uit met de nieuwe code. Deze wordt toegevoegd aan de bestaande labels.

```azurecli-interactive
az group update -n myResourceGroup --set tags.Project=Documentation
```

Resources overnemen niet labels van de resourcegroep. Op dit moment uw resourcegroep heeft drie tags, maar de bronnen hebben geen labels. Als u alle tags toepassen vanuit een resourcegroep tot de bronnen en bestaande tags voor bronnen behouden, gebruikt u het volgende script:

```azurecli-interactive
# Get the tags for the resource group
jsontag=$(az group show -n myResourceGroup --query tags)

# Reformat from JSON to space-delimited and equals sign
t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')

# Get the resource IDs for all resources in the resource group
r=$(az resource list -g myResourceGroup --query [].id --output tsv)

# Loop through each resource ID
for resid in $r
do
  # Get the tags for this resource
  jsonrtag=$(az resource show --id $resid --query tags)
  
  # Reformat from JSON to space-delimited and equals sign
  rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
  
  # Reapply the updated tags to this resource
  az resource tag --tags $t$rt --id $resid
done
```

U kunt ook labels van de resourcegroep toepassen op de bronnen zonder de bestaande labels te houden:

```azurecli-interactive
# Get the tags for the resource group
jsontag=$(az group show -n myResourceGroup --query tags)

# Reformat from JSON to space-delimited and equals sign
t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')

# Get the resource IDs for all resources in the resource group
r=$(az resource list -g myResourceGroup --query [].id --output tsv)

# Loop through each resource ID
for resid in $r
do
  # Apply tags from resource group to this resource
  az resource tag --tags $t --id $resid
done
```

Gebruiken om te combineren meerdere waarden in een enkel label, een JSON-tekenreeks.

```azurecli-interactive
az group update -n myResourceGroup --set tags.CostCenter='{"Dept":"IT","Environment":"Test"}'
```

Als u wilt verwijderen van alle codes voor een resourcegroep, gebruiken:

```azurecli-interactive
az group update -n myResourceGroup --remove tags
```
