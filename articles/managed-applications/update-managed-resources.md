---
title: Bijwerken van resources in Azure beheerde toepassingen | Microsoft Docs
description: Hierin wordt beschreven hoe werken aan resources in de beheerde resourcegroep voor een door Azure beheerde toepassing.
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 10/26/2017
ms.author: tomfitz
ms.openlocfilehash: 21f4e0aa339eb0c746f9b9b06f8aaada6c4d4b71
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61043409"
---
# <a name="work-with-resources-in-the-managed-resource-group-for-azure-managed-application"></a>Werken met resources in de beheerde resourcegroep voor Azure beheerde toepassing

In dit artikel wordt beschreven hoe u resources die zijn geïmplementeerd als onderdeel van een beheerde toepassing bijwerken. Als de uitgever van een beheerde toepassing hebt u toegang tot de resources in de beheerde resourcegroep. Voor het bijwerken van deze resources, moet u de beheerde resourcegroep die is gekoppeld aan een beheerde toepassing vinden en toegang tot de resource in die resourcegroep.

In dit artikel wordt ervan uitgegaan dat u hebt geïmplementeerd de beheerde toepassing in de [beheerde webtoepassing (IaaS) met Azure-beheerservices](https://github.com/Azure/azure-managedapp-samples/tree/master/samples/201-managed-web-app) voorbeeldproject. Dat beheerde toepassing bevat een **Standard_D1_v2** virtuele machine. Als u geen beheerde toepassing hebt geïmplementeerd, kunt u in dit artikel nog steeds gebruiken om vertrouwd te raken met de stappen voor het bijwerken van een beheerde resourcegroep.

De volgende afbeelding ziet u de geïmplementeerde beheerde toepassing.

![Geïmplementeerde beheerde toepassing](./media/update-managed-resources/deployed.png)

In dit artikel hebt u Azure CLI te gebruiken:

* Identificeren van de beheerde toepassing
* Identificeren van de beheerde resourcegroep
* Identificeren van de virtuele machine resource (s) in de beheerde resourcegroep
* Wijzigen van de VM-grootte (toekennen aan een kleiner formaat als niet gebruikt, of een grotere ter ondersteuning van meer laden)
* Een beleid toewijzen aan de beheerde resourcegroep waarmee de toegestane locaties

## <a name="get-managed-application-and-managed-resource-group"></a>Beheerde toepassing en de beheerde resourcegroep opvragen

Voor de beheerde toepassingen in een resourcegroep, gebruikt u:

```azurecli-interactive
az managedapp list --query "[?contains(resourceGroup,'DemoApp')]"
```

Voor de ID van de beheerde resourcegroep, gebruikt u:

```azurecli-interactive
az managedapp list --query "[?contains(resourceGroup,'DemoApp')].{ managedResourceGroup:managedResourceGroupId }"
```

## <a name="resize-vms-in-managed-resource-group"></a>Grootte van virtuele machines in de beheerde resourcegroep

Als u wilt zien van de virtuele machines in de beheerde resourcegroep, geef de naam van de beheerde resourcegroep.

```azurecli-interactive
az vm list -g DemoApp6zkevchqk7sfq --query "[].{VMName:name,OSType:storageProfile.osDisk.osType,VMSize:hardwareProfile.vmSize}"
```

Voor het bijwerken van de grootte van de virtuele machines gebruiken:

```azurecli-interactive
az vm resize --size Standard_D2_v2 --ids $(az vm list -g DemoApp6zkevchqk7sfq --query "[].id" -o tsv)
```

Nadat de bewerking is voltooid, controleert u of dat de toepassing wordt uitgevoerd op Standard D2 v2.

![Beheerde toepassing met behulp van standaard D2 v2](./media/update-managed-resources/upgraded.png)

## <a name="apply-policy-to-managed-resource-group"></a>Beleid toepassen op de beheerde resourcegroep

De beheerde resourcegroep en de toewijzing van een beleid krijgen op dat bereik. Het beleid **e56962a6-4747-49cd-b67b-bf8b01975c4c** is een ingebouwde beleid voor toegestane locaties op te geven.

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

Als u wilt zien van de toegestane locaties, gebruikt u:

```azurecli-interactive
az policy assignment show --name locationAssignment --scope $managedGroup --query parameters.listofallowedLocations.value
```

De beleidstoewijzing wordt weergegeven in de portal.

![Beleidstoewijzing weergeven](./media/update-managed-resources/assignment.png)

## <a name="next-steps"></a>Volgende stappen

* Zie [Overzicht van beheerde toepassingen](overview.md) voor een inleiding tot beheerde toepassingen.
* Zie voor voorbeeldprojecten, [voorbeeldprojecten voor Azure beheerde toepassingen](sample-projects.md).
