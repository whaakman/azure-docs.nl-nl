---
title: Gebruik Azure CLI om een beleidstoewijzing te maken voor het identificeren van niet-compatibele resources in uw Azure-omgeving
description: PowerShell gebruiken om een Azure Policy-toewijzing te maken om niet-compatibele resources te identificeren.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/07/2018
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: 27f00f24c1c644e340ff8a2843b56e863136c368
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34194805"
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-in-your-azure-environment-with-the-azure-cli"></a>Een beleidstoewijzing maken om niet-compatibele resources te identificeren in uw Azure-omgeving met Azure CLI

De eerste stap in het begrijpen van naleving in Azure is het identificeren van de status van uw resources. In deze quickstart gaat u een beleidstoewijzing maken voor het identificeren van virtuele machines die geen beheerde schijven gebruiken.

Als u dit proces helemaal hebt doorlopen, kunt u virtuele machines identificeren die geen beheerde schijven gebruiken. Ze zijn *niet-compatibel* met de beleidstoewijzing.

Azure CLI wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. In deze gids wordt Azure CLI gebruikt om een beleidstoewijzing te maken om niet-compatibele resources te identificeren in uw Azure-omgeving.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Voor deze quickstart moet u versie 2.0.4 of hoger van Azure CLI uitvoeren om de CLI lokaal te installeren en te gebruiken. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli).

## <a name="prerequisites"></a>Vereisten

Registreer de resourceprovider Policy Insights met behulp van Azure CLI. Als u de resourceprovider registreert, controleer dan of uw abonnement ermee werkt. Als u een resourceprovider wilt registreren, moet u toestemming hebben om de registreeractie voor de resourceprovider uit te voeren. Deze bewerking is opgenomen in de rollen Inzender en Eigenaar. Voer de volgende opdracht uit om de resourceprovider te registreren:

```azurecli-interactive
az provider register –-namespace 'Microsoft.PolicyInsights'
```

Zie [Resourceproviders en -typen](../azure-resource-manager/resource-manager-supported-services.md) voor meer informatie over het registreren en weergeven van resourceproviders.

## <a name="create-a-policy-assignment"></a>Een beleidstoewijzing maken

In deze quickstart maakt u een beleidstoewijzing en wijst u de definitie Virtuele machines zonder beheerde schijven controleren toe. Deze beleidsdefinitie identificeert resources die niet voldoen aan de voorwaarden die zijn vastgelegd in de beleidsdefinitie.

Voer de volgende opdracht uit om een beleidstoewijzing te maken:

```azurecli-interactive
az policy assignment create --name 'Audit Virtual Machines without Managed Disks Assignment' --scope '<scope>' --policy '<policy definition ID>'
```

De voorgaande opdracht maakt gebruik van de volgende informatie:

- **Naam**: weergavenaam voor de beleidstoewijzing. In dit geval gebruikt u de toewijzing *Virtuele machines zonder beheerde schijven controleren*.
- **Beleid**: de id van de beleidsdefinitie, op basis waarvan u de toewijzing maakt. In dit geval is het de beleidsdefinitie *Virtuele machines zonder beheerde schijven controleren*. Voer de volgende opdracht uit om de id van de beleidstoewijzing te verkrijgen: `az policy definition show --name 'Audit Virtual Machines without Managed Disks Assignment'`
- **Bereik**: een bereik bepaalt voor welke resources of groep resources de beleidstoewijzing wordt afgedwongen. Dit kan variëren van een abonnement tot resourcegroepen. Vergeet niet om &lt;scope&gt; te vervangen door de naam van uw resourcegroep.

## <a name="identify-non-compliant-resources"></a>Niet-compatibele resources identificeren

Als u de resources wilt zien die niet compatibel zijn onder deze nieuwe toewijzing, voert u de volgende opdrachten uit om de id van de beleidstoewijzing te verkrijgen:

```azurepowershell-interactive
$policyAssignment = Get-AzureRmPolicyAssignment | Where-Object { $_.Properties.DisplayName -eq 'Audit Virtual Machines without Managed Disks' }
$policyAssignment.PolicyAssignmentId
```

Zie [Get-AzureRMPolicyAssignment](/powershell/module/azurerm.resources/get-azurermpolicyassignment) voor meer informatie over beleidstoewijzings-id's.

Voer daarna de volgende opdracht uit om de resource-id's te verkrijgen van de niet-compatibele resources. Deze worden uitgevoerd naar een JSON-bestand:

```
armclient post "/subscriptions/<subscriptionID>/resourceGroups/<rgName>/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2017-12-12-preview&$filter=IsCompliant eq false and PolicyAssignmentId eq '<policyAssignmentID>'&$apply=groupby((ResourceId))" > <json file to direct the output with the resource IDs into>
```

De resultaten zien er ongeveer als volgt uit:

```json
{
    "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest",
    "@odata.count": 3,
    "value": [{
            "@odata.id": null,
            "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
            "ResourceId": "/subscriptions/<subscriptionId>/resourcegroups/<rgname>/providers/microsoft.compute/virtualmachines/<virtualmachineId>"
        },
        {
            "@odata.id": null,
            "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
            "ResourceId": "/subscriptions/<subscriptionId>/resourcegroups/<rgname>/providers/microsoft.compute/virtualmachines/<virtualmachine2Id>"
        },
        {
            "@odata.id": null,
            "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
            "ResourceId": "/subscriptions/<subscriptionName>/resourcegroups/<rgname>/providers/microsoft.compute/virtualmachines/<virtualmachine3ID>"
        }

    ]
}
```

De resultaten zijn vergelijkbaar met wat in de weergave van de Azure-portal meestal wordt vermeld onder **Niet-compatibele resources**.

## <a name="clean-up-resources"></a>Resources opschonen

Andere handleidingen in deze verzameling zijn op deze quickstart gebaseerd. Als u verder wilt gaan met volgende zelfstudies, verwijder dan niet de resources die u in deze quickstart hebt gemaakt. Als u niet van plan bent om verder te gaan, verwijdert u de toewijzing die u hebt gemaakt. Voer daarvoor de volgende opdracht uit:

```azurecli-interactive
az policy assignment delete –name 'Audit Virtual Machines without Managed Disks Assignment' --scope '/subscriptions/<subscriptionID>/<resourceGroupName>'
```

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u een beleidsdefinitie toegewezen om niet-compatibele resources in uw Azure-omgeving te identificeren.

Voor meer informatie over het toewijzen van beleid en hoe u ervoor zorgt dat resources die u in de **toekomst** gaat maken compatibel zijn, gaat u verder met de zelfstudie voor:

> [!div class="nextstepaction"]
> [Beleid maken en beheren](create-manage-policy.md)