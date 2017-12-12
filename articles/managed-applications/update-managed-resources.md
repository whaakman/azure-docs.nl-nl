---
title: Bijwerken van resources in Azure beheerde toepassingen | Microsoft Docs
description: Hierin wordt beschreven hoe werken voor bronnen in de beheerde resourcegroep voor een Azure-beheerde toepassing.
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/26/2017
ms.author: tomfitz
ms.openlocfilehash: d3c955d7be0e7e6d45751c0e685bad498e524d94
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="work-with-resources-in-the-managed-resource-group-for-azure-managed-application"></a>Werken met resources in de beheerde resourcegroep voor Azure beheerde toepassing

Dit artikel wordt beschreven hoe u resources die zijn geïmplementeerd als onderdeel van een beheerde toepassing bijwerkt. Als de uitgever van een beheerde toepassing hebt u toegang tot de resources in de groep beheerde resource. Voor het bijwerken van deze bronnen, moet u de beheerde resourcegroep die zijn gekoppeld aan een beheerde toepassing vinden en openen van de resource in die resourcegroep.

In dit artikel wordt ervan uitgegaan dat u hebt geïmplementeerd de beheerde toepassing in de [beheerde webtoepassing (IaaS) met Azure beheerservices](https://github.com/Azure/azure-managedapp-samples/tree/master/samples/201-managed-web-app) voorbeeldproject. Dat beheerde toepassing bevat een **Standard_D1_v2** virtuele machine. Als u geen beheerde toepassing hebt geïmplementeerd, kunt u dit artikel om vertrouwd te raken met de stappen voor het bijwerken van een groep beheerde bron.

De volgende afbeelding toont de geïmplementeerde beheerde toepassing.

![Beheerde toepassing is geïmplementeerd](./media/update-managed-resources/deployed.png)

In dit artikel leert u Azure CLI om te gebruiken:

* Identificeren van de beheerde toepassing
* De groep beheerde bron identificeren
* De resources van de virtuele machine in de groep beheerde bron identificeren
* Verander de VM-grootte (op een kleinere als niet gebruikt of een grotere ter ondersteuning van meer load)
* Een beleid toewijzen aan de beheerde resourcegroep waarmee de toegestane locaties

## <a name="get-managed-application-and-managed-resource-group"></a>Ophalen van beheerde toepassingen en beheerde resourcegroep

Als u de beheerde toepassingen in een resourcegroep, gebruikt u:

```azurecli-interactive
az managedapp list --query "[?contains(resourceGroup,'DemoApp')]"
```

Als u de ID van de beheerde resourcegroep, gebruikt u:

```azurecli-interactive
az managedapp list --query "[?contains(resourceGroup,'DemoApp')].{ managedResourceGroup:managedResourceGroupId }"
```

## <a name="resize-vms-in-managed-resource-group"></a>Grootte van virtuele machines in beheerde resourcegroep

Geef de naam van de beheerde resourcegroep overzicht van de virtuele machines in de groep van beheerde bronnen.

```azurecli-interactive
az vm list -g DemoApp6zkevchqk7sfq --query "[].{VMName:name,OSType:storageProfile.osDisk.osType,VMSize:hardwareProfile.vmSize}"
```

Voor het bijwerken van de grootte van de virtuele machines gebruiken:

```azurecli-interactive
az vm resize --size Standard_D2_v2 --ids $(az vm list -g DemoApp6zkevchqk7sfq --query "[].id" -o tsv)
```

Nadat de bewerking is voltooid, controleert u of dat de toepassing op standaard D2 v2 wordt uitgevoerd.

![Beheerde toepassing met behulp van standaard D2 v2](./media/update-managed-resources/upgraded.png)

## <a name="apply-policy-to-managed-resource-group"></a>Beleid toepassen op beheerde resourcegroep

Lees het beheerde resourcegroep en toewijzing van een beleid op dat bereik. Het beleid **e56962a6-4747-49cd-b67b-bf8b01975c4c** is een ingebouwde beleid voor het opgeven van toegestane locaties.

```azurecli-interactive
managedGroup=$(az managedapp show --name <app-name> --resource-group DemoApp --query managedResourceGroupId --output tsv)

az policy assignment create --name locationAssignment --policy e56962a6-4747-49cd-b67b-bf8b01975c4c --scope $managedGroup --params '{
                            "listofallowedLocations": {
                                "value": [
                                    "northeurope",
                                    "westeurope"
                                ]
                            }
                        }'
```

Als de toegestane locaties wilt weergeven, gebruikt u het:

```azurecli-interactive
az policy assignment show --name locationAssignment --scope $managedGroup --query parameters.listofallowedLocations.value
```

De toewijzing van beleid wordt weergegeven in de portal.

![De toewijzing van beleid weergeven](./media/update-managed-resources/assignment.png)

## <a name="next-steps"></a>Volgende stappen

* Zie [Overzicht van beheerde toepassingen](overview.md) voor een inleiding tot beheerde toepassingen.
* Zie voor voorbeeldprojecten, [voorbeeldprojecten voor Azure beheerde toepassingen](sample-projects.md).
