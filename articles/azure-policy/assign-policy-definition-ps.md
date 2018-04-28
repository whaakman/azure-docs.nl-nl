---
title: 'Quickstart: PowerShell gebruiken om een beleidstoewijzing te maken om niet-compatibele resources te identificeren in uw Azure-omgeving | Microsoft Docs'
description: In deze quickstart gebruikt u PowerShell om een Azure Policy-toewijzing te maken om niet-compatibele resources te identificeren.
services: azure-policy
keywords: ''
author: DCtheGeek
ms.author: dacoulte
ms.date: 04/10/2018
ms.topic: quickstart
ms.service: azure-policy
ms.custom: mvc
ms.openlocfilehash: f39177f5a2e5878570ed750a42009c2203257b13
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-create-a-policy-assignment-to-identify-non-compliant-resources-using-the-azure-rm-powershell-module"></a>Quickstart: Een beleidstoewijzing maken om niet-compatibele resources te identificeren met de PowerShell-module van Azure Resource Manager

De eerste stap in het begrijpen van naleving in Azure is het identificeren van de status van uw resources. In deze quickstart maakt u een beleidstoewijzing om te identificeren welke virtuele machines geen beheerde schijven gebruiken. Daarna gaat u de virtuele machines identificeren die *niet-compatibel* zijn met de beleidstoewijzing.

De PowerShell-module van AzureRM wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. In deze handleiding wordt uitgelegd hoe u AzureRM gebruikt voor het maken van een beleidstoewijzing. Met het beleid identificeert u niet-compatibele resources in uw Azure-omgeving.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

- Voor u begint, moet u ervoor zorgen dat de nieuwste versie van PowerShell is geïnstalleerd. Zie [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) (Azure PowerShell installeren en configureren) voor gedetailleerde informatie.
- Werk uw PowerShell-module van Azure RM bij naar de nieuwste versie. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).
- Registreer de resourceprovider Policy Insights met behulp van Azure PowerShell. Als u de resourceprovider registreert, controleer dan of uw abonnement ermee werkt. Als u een resourceprovider wilt registreren, moet u toestemming hebben om de registreeractie voor de resourceprovider uit te voeren. Deze bewerking is opgenomen in de rollen Inzender en Eigenaar. Voer de volgende opdracht uit om de resourceprovider te registreren:

  ```
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.PolicyInsights
  ```

  Zie [Resourceproviders en -typen](../azure-resource-manager/resource-manager-supported-services.md) voor meer informatie over het registreren en weergeven van resourceproviders.

## <a name="create-a-policy-assignment"></a>Een beleidstoewijzing maken

In deze quickstart maakt u een beleidstoewijzing en wijzen we de definitie *Virtuele machines zonder beheerde schijven controleren* toe. Deze beleidsdefinitie identificeert resources die niet voldoen aan de voorwaarden die zijn vastgelegd in de beleidsdefinitie.

Voer de volgende opdrachten uit om een nieuwe beleidstoewijzing te maken:

```powershell
$rg = Get-AzureRmResourceGroup -Name "<resourceGroupName>"
$definition = Get-AzureRmPolicyDefinition | where {$_.properties.displayName -eq "Audit VMs that do not use managed disks"}
New-AzureRMPolicyAssignment -Name "Audit Virtual Machines without Managed Disks" -Scope $rg.ResourceId -PolicyDefinition $definition -Sku @{Name='A1';Tier='Standard'}
```

In de voorgaande opdrachten wordt de volgende informatie gebruikt:

- **Naam**: weergavenaam voor de beleidstoewijzing. In dit geval gebruikt u de toewijzing *Virtuele machines zonder beheerde schijven controleren*.
- **Definitie**: de beleidsdefinitie, op basis waarvan u de toewijzing maakt. In dit geval is het de beleidsdefinitie *Virtuele machines zonder beheerde schijven controleren*.
- **Bereik**: een bereik bepaalt voor welke resources of groep resources de beleidstoewijzing wordt afgedwongen. Dit kan variëren van een abonnement tot resourcegroepen. Vergeet niet om &lt;scope&gt; te vervangen door de naam van uw resourcegroep.
- **SKU**: met deze opdracht maakt u een beleidstoewijzing met de Standaard-laag. Met de Standard-laag kunt u geschaald management, nalevingsbeoordeling en herstel bewerkstelligen. Zie [Prijzen voor Azure Policy](https://azure.microsoft.com/pricing/details/azure-policy) voor extra details over prijscategorieën.


U bent er nu klaar voor om niet-compatibele resources te identificeren om inzicht te krijgen in de nalevingsstatus van uw omgeving.

## <a name="identify-non-compliant-resources"></a>Niet-compatibele resources identificeren

Gebruik de volgende informatie om te identificeren welke resources niet compatibel zijn met de beleidstoewijzing die u hebt gemaakt. Voer de volgende opdrachten uit:

```powershell
$policyAssignment = Get-AzureRmPolicyAssignment | where {$_.properties.displayName -eq "Audit Virtual Machines without Managed Disks"}
```

```powershell
$policyAssignment.PolicyAssignmentId
```

Zie [Get-AzureRMPolicyAssignment](/powershell/module/azurerm.resources/get-azurermpolicyassignment) voor meer informatie over beleidstoewijzings-id's.

Voer daarna de volgende opdracht uit om de resource-id's te verkrijgen van de niet-compatibele resources. Deze worden uitgevoerd naar een JSON-bestand:

```powershell
armclient post "/subscriptions/<subscriptionID>/resourceGroups/<rgName>/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2017-12-12-preview&$filter=IsCompliant eq false and PolicyAssignmentId eq '<policyAssignmentID>'&$apply=groupby((ResourceId))" > <json file to direct the output with the resource IDs into>
```
De resultaten zien er ongeveer als volgt uit:


```
{
"@odata.context":"https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest",
"@odata.count": 3,
"value": [
{
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

Volgende handleidingen in deze verzameling zijn op deze quickstart gebaseerd. Als u verder wilt gaan met andere zelfstudies, verwijder dan niet de resources die u in deze quickstart hebt gemaakt. Als u niet van plan bent om verder te gaan, voert u de volgende opdracht uit om de toewijzing te verwijderen die u hebt gemaakt:

```powershell
Remove-AzureRmPolicyAssignment -Name "Audit Virtual Machines without Managed Disks Assignment" -Scope /subscriptions/<subscriptionID>/<resourceGroupName>
```

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u een beleidsdefinitie toegewezen om niet-compatibele resources in uw Azure-omgeving te identificeren.

Voor meer informatie over het toewijzen van beleid en om ervoor te zorgen dat **toekomstige** resources die worden gemaakt compatibel zijn, gaat u verder met de zelfstudie voor:

> [!div class="nextstepaction"]
> [Beleid maken en beheren](./create-manage-policy.md)
