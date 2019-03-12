---
title: Aangepaste toegang controllerrol - Avere vFXT voor Azure
description: Een aangepaste rol voor de controller Avere vFXT cluster maken
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: v-erkell
ms.openlocfilehash: c408efa7ca01928e25ac03f5ca63d0aef7d88839
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57770031"
---
# <a name="customized-controller-access-role"></a>Rol voor aangepaste controllertoegang

De vFXT Avere voor Azure-cluster-netwerkcontroller maakt gebruik van een beheerde identiteit en de op rollen gebaseerd toegangsbeheer (RBAC) toe te staan dat het maken en beheren van het cluster. 

Standaard wordt de clustercontroller is toegewezen de [ingebouwde rol van eigenaar](../role-based-access-control/built-in-roles.md#owner). Ook de toegang van de controller is afgestemd op de resourcegroep - elementen buiten de resourcegroep van het cluster kunnen niet worden gewijzigd.

In dit artikel wordt uitgelegd hoe u uw eigen toegangsfunctie voor de cluster-netwerkcontroller in plaats van de standaardinstelling te maken. 

## <a name="edit-the-role-prototype"></a>Het prototype van de rol bewerken

Starten van de rol prototype beschikbaar op <https://github.com/Azure/Avere/blob/master/src/vfxt/src/roles/AvereContributor.txt>.

```json
{
  "AssignableScopes": [
    "/subscriptions/YOUR SUBSCRIPTION ID HERE"
  ],
  "Name": "Avere custom contributor",
  "IsCustom": true,
  "Description": "Can create and manage an Avere vFXT cluster.",
  "NotActions": [],
  "Actions": [
    "Microsoft.Authorization/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/availabilitySets/*",
    "Microsoft.Compute/virtualMachines/*",
    "Microsoft.Compute/disks/*",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Network/*/read",
    "Microsoft.Network/networkInterfaces/*",
    "Microsoft.Network/virtualNetworks/subnets/join/action",
    "Microsoft.Network/virtualNetworks/subnets/read",
    "Microsoft.Resources/deployments/*",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Resources/subscriptions/resourceGroups/resources/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Storage/storageAccounts/listKeys/action",
    "Microsoft.Support/*"
  ],
  "DataActions": []
}
```

De abonnements-ID voor de vFXT Avere voor Azure-implementatie in de instructie AssignableScopes toevoegen. De naam van de aanpassen en toevoegen of alter definities indien nodig. 

Wees voorzichtig als u bevoegdheden beperkt. Het maken van clusters kan mislukken als de controller heeft niet voldoende toegang hebt. 

Voor meer informatie over wat de clustercontroller bevoegdheden moet maken van een cluster, [open een ondersteuningsticket](avere-vfxt-open-ticket.md#open-a-support-ticket-for-your-avere-vfxt). 

De definitie van de aangepaste rol opslaan als een .json-bestand. 

## <a name="define-the-role"></a>De rol definiëren 

Volg deze stappen voor de definitie van de aangepaste rol toevoegen aan uw abonnement. 

1. De Azure Cloud Shell openen in Azure portal of blader naar [ https://shell.azure.com ](https://shell.azure.com).

1. Gebruik de Azure CLI-opdracht uit om aan uw abonnement vFXT:

   ```azurecli
   az account set --subscription YOUR_SUBSCRIPTION_ID
   ```

1. De rol maken:

   ```azurecli
   az role definition create --role-definition /avere-contributor-custom.json
   ```

   Gebruik de bestandsnaam en het pad in plaats van ```/avere-contributor-custom.json``` in dit voorbeeld. 

Sla de uitvoer van de opdracht van de definitie rol: deze bevat de rol-id die u nodig hebt om op te geven aan de clustersjabloon maken. 

## <a name="find-the-role-id"></a>De rol-ID vinden

De sjabloon voor de implementatie Avere vFXT moet van de rol unieke identifier (GUID) een aangepaste rol toewijzen aan de controller. 

De GUID van de rol is een tekenreeks van 32 tekens in dit formulier: 8e3af657-a8ff-443c-a75c-2fe8c4bcb635

Als u de GUID van uw rol opzoeken, gebruikt u deze opdracht met de rolnaam van uw in de ```--name``` parameter.

```azurecli
az role definition list --query '[*].{roleName:roleName, name:name}' -o table --name 'YOUR ROLE NAME'
```
Voer deze tekenreeks in de **Avere cluster rol-ID maken** veld bij het implementeren van de vFXT Avere voor Azure.

## <a name="next-steps"></a>Volgende stappen

Lees meer informatie over de vFXT Avere implementeren voor Azure in [de vFXT-cluster implementeren](avere-vfxt-deploy.md)
