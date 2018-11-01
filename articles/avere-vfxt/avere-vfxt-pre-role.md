---
title: Avere rol maken met niet-controller - Avere vFXT voor Azure
description: Methode voor het maken van de vereiste RBAC-rol zonder een cluster-netwerkcontroller VM
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 547a0e0ac5254408a4064d627ec4c1e7c354a433
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50633982"
---
# <a name="create-the-avere-vfxt-cluster-runtime-access-role-without-a-controller"></a>De functie met toegang runtime voor Avere vFXT cluster zonder een domeincontroller maken

Dit document bevat een methode voor het maken van de functie cluster knooppunt toegang vanaf de opdrachtregel, voordat u het cluster netwerkcontroller-VM maken. 

Als u wilt deze van de netwerkcontroller cluster maken, lezen [maken van de functie cluster knooppunt toegang](avere-vfxt-deploy.md#create-the-cluster-node-access-role). De controller-installatiekopie bevat een rol prototype-bestand. U kunt het bestand bijwerken door uw abonnements-ID en het gebruiken voor het definiëren van de functie lokaal op de virtuele machine van de controller.

## <a name="create-an-azure-rbac-role"></a>Een Azure RBAC-rol maken

Maakt gebruik van het systeem van Avere vFXT [op rollen gebaseerd toegangsbeheer](https://docs.microsoft.com/azure/role-based-access-control/) (RBAC) voor het autoriseren van de clusterknooppunten vFXT noodzakelijke taken uit te voeren.  

Als onderdeel van normale vFXT bewerking van het cluster, individuele vFXT knooppunten nodig hebt voor handelingen zoals lezen van de Azure-resource-eigenschappen, opslag te beheren en de andere knooppunten netwerkinterface-instellingen beheren. 

Deze rol wordt gebruikt voor de knooppunten vFXT alleen, niet voor het cluster netwerkcontroller-VM.  

Als u maken van de rol voor de controller wilt, volgt u deze stappen: 

1. De Azure Cloud Shell openen in Azure portal of blader naar [ https://shell.azure.com ](https://shell.azure.com).

1. Gebruik de Azure CLI-opdracht uit om aan uw abonnement vFXT:

   ```az account set --subscription YOUR_SUBSCRIPTION_ID```

1. Deze opdrachten gebruiken voor de roldefinitie van de van de marketplace-installatiekopie downloaden en te bewerken. 

   ```bash
   wget -O- https://averedistribution.blob.core.windows.net/public/vfxtdistdoc.tgz | tar zxf - avere-cluster.json
   vi avere-cluster.json
   ``` 

4. Bewerk het bestand voor het opnemen van uw abonnements-ID en de regel boven het verwijderen. Sla het bestand op als ``avere-cluster.json``.

   ![Console teksteditor met de abonnements-ID en de 'verwijdert u deze regel' geselecteerd voor verwijdering](media/avere-vfxt-edit-role.png)

5. De rol maken:  

   ```bash
   az role definition create --role-definition /avere-cluster.json
   ```

Wanneer u het cluster maakt, geef de naam van de rol (in dit geval `avere-cluster`). Een script voor het maken van het cluster wijst de rol toe aan de zojuist gemaakte clusterknooppunten. 

## <a name="sample-cluster-node-runtime-role-definition"></a>Voorbeeld van cluster-knooppunt runtime-roldefinitie

> [!IMPORTANT] 
> De voorbeelddefinitie van dit is afkomstig uit een pre-GA-versie van het product. Als de versie die u aan met het huidige productdistributie anders is, kunt u die versie gebruiken.

```json

{
    "AssignableScopes": [
        "/subscriptions/YOUR_SUBSCRIPTION_ID_GOES_HERE"
    ],
    "Name": "avere-cluster",
    "IsCustom": "true",
    "Description": "Avere cluster runtime role",
    "NotActions": [],
    "Actions": [
        "Microsoft.Compute/virtualMachines/read",
        "Microsoft.Network/networkInterfaces/read",
        "Microsoft.Network/networkInterfaces/write",
        "Microsoft.Network/virtualNetworks/subnets/read",
        "Microsoft.Network/virtualNetworks/subnets/join/action",
        "Microsoft.Network/networkSecurityGroups/join/action",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/write"
    ],
    "DataActions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write"
    ]
}

```