---
title: Tijdelijke Avere vFXT niet-eigenaren oplossing - Azure
description: Tijdelijke oplossing voor het toestaan dat gebruikers zonder abonnement eigenaarsmachtiging beschikken voor het implementeren van Avere vFXT voor Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: e72e6d969649de09389ee38b94e874fad98ee08f
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50633936"
---
# <a name="authorize-non-owners-to-deploy-avere-vfxt"></a>Toestaan dat niet-eigenaren Avere vFXT implementeren

Deze instructies zijn een tijdelijke oplossing waarmee een gebruiker zonder abonnement eigenaarsbevoegdheden voor het maken van een vFXT Avere voor Azure-systeem.

(De aanbevolen manier om te implementeren, de Avere vFXT system is dat een gebruiker met eigenaarsbevoegdheden voor kan het maken van de stappen, zoals uitgelegd in [voorbereidingen voor het maken van de vFXT Avere](avere-vfxt-prereqs.md).)  

De oplossing omvat het maken van een extra functie waarmee de gebruikers voldoende machtigingen voor het installeren van het cluster. De rol moet worden gemaakt door de eigenaar van een abonnement en een eigenaar moet toewijzen aan gebruikers. 

De eigenaar van een abonnement moet ook [accepteer de gebruiksvoorwaarden](avere-vfxt-prereqs.md) voor de Avere vFXT marketplace-installatiekopie. 

> [!IMPORTANT] 
> Al deze stappen moeten worden genomen door een gebruiker met eigenaarsbevoegdheden van het abonnement dat wordt gebruikt voor het cluster.

1. Deze regels kopiëren en deze opslaat in een bestand (bijvoorbeeld `averecreatecluster.json`). Gebruik uw abonnements-ID in de `AssignableScopes` instructie.

   ```json
   {
       "AssignableScopes": ["/subscriptions/<SUBSCRIPTION_ID>"],
       "Name": "avere-create-cluster",
       "IsCustom": "true"
       "Description": "Can create Avere vFXT clusters",
       "NotActions": [],
       "Actions": [
           "Microsoft.Authorization/*/read",
           "Microsoft.Authorization/roleAssignments/*",
           "Microsoft.Authorization/roleDefinitions/*",
           "Microsoft.Compute/*/read",
           "Microsoft.Compute/availabilitySets/*",
           "Microsoft.Compute/virtualMachines/*",
           "Microsoft.Network/*/read",
           "Microsoft.Network/networkInterfaces/*",
           "Microsoft.Network/routeTables/write",
           "Microsoft.Network/routeTables/delete",
           "Microsoft.Network/routeTables/routes/delete",
           "Microsoft.Network/virtualNetworks/subnets/join/action",
           "Microsoft.Network/virtualNetworks/subnets/read",
   
           "Microsoft.Resources/subscriptions/resourceGroups/read",
           "Microsoft.Resources/subscriptions/resourceGroups/resources/read",
           "Microsoft.Storage/*/read",
           "Microsoft.Storage/storageAccounts/listKeys/action"
       ],
   }
   ```

1. Voer deze opdracht om de rol te maken:

   `az role definition create --role-definition <PATH_TO_FILE>`

    Voorbeeld:
    ```azurecli
    az role definition create --role-definition ./averecreatecluster.json
    ```

1. Deze rol toewijzen aan de gebruiker die het cluster wilt maken:

   `az role assignment create --assignee <USERNAME> --scope /subscriptions/<SUBSCRIPTION_ID> --role 'avere-create-cluster'`

Na deze procedure heeft elke gebruiker die deze rol toegewezen aan de volgende machtigingen voor het abonnement: 

* Maken en configureren van de netwerkinfrastructuur
* De netwerkcontroller cluster maken
* Uitvoeren van scripts voor het cluster maken van de netwerkcontroller cluster om het cluster te maken
