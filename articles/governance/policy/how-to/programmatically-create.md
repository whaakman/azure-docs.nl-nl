---
title: Programmatisch beleid maken en compatibiliteitsgegevens weergeven
description: Dit artikel helpt u bij het programmatisch beleid maken en beheren voor Azure Policy.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 01/26/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 575e2974131a09bdbdbc96d3ad252365ac9da86e
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55101784"
---
# <a name="programmatically-create-policies-and-view-compliance-data"></a>Programmatisch beleid maken en compatibiliteitsgegevens weergeven

Dit artikel helpt u bij het programmatisch beleid maken en beheren. Beleidsdefinities worden verschillende regels en effecten voor uw resources afdwingen. Afdwingen zorgt ervoor dat resources voldoen aan uw bedrijfsnormen en serviceovereenkomsten blijven.

Zie voor meer informatie over de naleving van [ophalen van Nalevingsgegevens](getting-compliance-data.md).

[!INCLUDE [az-powershell-update](../../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten

Voordat u begint, zorg ervoor dat de volgende vereisten wordt voldaan:

1. Installeer de [ARMClient](https://github.com/projectkudu/ARMClient) als u dit nog niet hebt gedaan. Dit is een hulpprogramma waarmee HTTP-aanvragen worden verzonden naar Azure Resource Manager-API’s.

1. Uw Azure PowerShell-module bijwerken naar de nieuwste versie. Zie [Azure PowerShell-module installeren](/powershell/azure/install-az-ps) voor gedetailleerde informatie. Zie voor meer informatie over de nieuwste versie, [Azure PowerShell](https://github.com/Azure/azure-powershell/releases).

1. Registreer de resourceprovider Policy Insights met behulp van Azure PowerShell om te valideren dat uw abonnement hiervoor met de resourceprovider geschikt. Als u wilt een resourceprovider registreren, moet u gemachtigd zijn om uit te voeren van de registreeractie voor de resourceprovider. Deze bewerking is opgenomen in de rollen Inzender en Eigenaar. Voer de volgende opdracht uit om de resourceprovider te registreren:

   ```azurepowershell-interactive
   Register-AzResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
   ```

   Zie voor meer informatie over het registreren en weergeven van resourceproviders [Resourceproviders en typen](../../../azure-resource-manager/resource-manager-supported-services.md).

1. Als u niet hebt gedaan, installeert u Azure CLI. Krijgt u de nieuwste versie [Azure CLI installeren op Windows](/cli/azure/install-azure-cli-windows).

## <a name="create-and-assign-a-policy-definition"></a>Maken en een beleidsdefinitie toewijzen

De eerste stap voor beter inzicht in uw resources is het maken en toewijzen van beleid voor uw resources. De volgende stap is om te leren hoe u programmatisch maken en toewijzen van een beleid. De voorbeeldbeleid controleert storage-accounts die zijn geopend met alle openbare netwerken met behulp van PowerShell, Azure CLI en HTTP-aanvragen.

### <a name="create-and-assign-a-policy-definition-with-powershell"></a>Maken en toewijzen van een beleidsdefinitie met PowerShell

1. Gebruik de volgende JSON-codefragment om te maken van een JSON-bestand met de naam AuditStorageAccounts.json.

   ```json
   {
       "if": {
           "allOf": [{
                   "field": "type",
                   "equals": "Microsoft.Storage/storageAccounts"
               },
               {
                   "field": "Microsoft.Storage/storageAccounts/networkAcls.defaultAction",
                   "equals": "Allow"
               }
           ]
       },
       "then": {
           "effect": "audit"
       }
   }
   ```

   Zie voor meer informatie over het ontwerpen van een beleidsdefinitie [structuur van beleidsdefinities Azure](../concepts/definition-structure.md).

1. Voer de volgende opdracht om te maken van een beleidsdefinitie met behulp van het bestand AuditStorageAccounts.json.

   ```azurepowershell-interactive
   New-AzPolicyDefinition -Name 'AuditStorageAccounts' -DisplayName 'Audit Storage Accounts Open to Public Networks' -Policy 'AuditStorageAccounts.json'
   ```

   De opdracht maakt u een beleidsdefinitie met de naam _Audit Storage Accounts openen met een openbaar netwerk_.
   Zie voor meer informatie over andere parameters die u kunt gebruiken, [New-AzPolicyDefinition](/powershell/module/az.resources/new-azpolicydefinition).

   Wanneer aangeroepen zonder locatieparameters, `New-AzPolicyDefinition` standaard ingesteld op het opslaan van de beleidsdefinitie in het geselecteerde abonnement van de context van sessies. Als u wilt de definitie van de opslaan naar een andere locatie, gebruik de volgende parameters:

   - **Abonnements-id** -opslaan naar een ander abonnement. Vereist een _GUID_ waarde.
   - **ManagementGroupName** -opslaan in een beheergroep. Vereist een _tekenreeks_ waarde.

1. Nadat u de beleidsdefinitie van uw maakt, kunt u een beleidstoewijzing maken door het uitvoeren van de volgende opdrachten:

   ```azurepowershell-interactive
   $rg = Get-AzResourceGroup -Name 'ContosoRG'
   $Policy = Get-AzPolicyDefinition -Name 'AuditStorageAccounts'
   New-AzPolicyAssignment -Name 'AuditStorageAccounts' -PolicyDefinition $Policy -Scope $rg.ResourceId
   ```

   Vervang _ContosoRG_ met de naam van de beoogde resourcegroep.

   De **bereik** parameter op `New-AzPolicyAssignment` werkt ook met abonnementen en beheergroepen. De parameter maakt gebruik van een pad van de volledige resource, die de **ResourceId** eigenschap op `Get-AzResourceGroup` retourneert. Het patroon voor **bereik** voor elke container als volgt is.
   Vervang `{rgName}`, `{subId}`, en `{mgName}` met de resource groepsnaam, abonnements-ID, en de naam van beheergroep, respectievelijk.

   - Resourcegroep: `/subscriptions/{subId}/resourceGroups/{rgName}`
   - Abonnement: `/subscriptions/{subId}/`
   - Beheergroep- `/providers/Microsoft.Management/managementGroups/{mgName}`

Zie voor meer informatie over het beheren van de resource-beleidsregels met behulp van de Azure Resource Manager PowerShell-module [Az.Resources](/powershell/module/az.resources/#policies).

### <a name="create-and-assign-a-policy-definition-using-armclient"></a>Maken en toewijzen van een beleidsdefinitie ARMClient

Gebruik de volgende procedure om de beleidsdefinitie van een te maken.

1. Kopieer de volgende JSON-fragment voor het maken van een JSON-bestand. Noem het bestand in de volgende stap.

   ```json
   "properties": {
       "displayName": "Audit Storage Accounts Open to Public Networks",
       "policyType": "Custom",
       "mode": "Indexed",
       "description": "This policy ensures that storage accounts with exposure to Public Networks are audited.",
       "parameters": {},
       "policyRule": {
           "if": {
               "allOf": [{
                       "field": "type",
                       "equals": "Microsoft.Storage/storageAccounts"
                   },
                   {
                       "field": "Microsoft.Storage/storageAccounts/networkAcls.defaultAction",
                       "equals": "Allow"
                   }
               ]
           },
           "then": {
               "effect": "audit"
           }
       }
   }
   ```

1. Maak de beleidsdefinitie met een van de volgende aanroepen:

   ```
   # For defining a policy in a subscription
   armclient PUT "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/AuditStorageAccounts?api-version=2016-12-01" @<path to policy definition JSON file>

   # For defining a policy in a management group
   armclient PUT "/providers/Microsoft.Management/managementgroups/{managementGroupId}/providers/Microsoft.Authorization/policyDefinitions/AuditStorageAccounts?api-version=2016-12-01" @<path to policy definition JSON file>
   ```

   De voorgaande {subscriptionId} vervangen door de ID van uw abonnement of {managementGroupId} met de ID van uw [beheergroep](../../management-groups/overview.md).

   Zie voor meer informatie over de structuur van de query, [beleidsdefinities – maken of bijwerken](/rest/api/resources/policydefinitions/createorupdate) en [beleidsdefinities – maken of bijwerken op beheergroep](/rest/api/resources/policydefinitions/createorupdateatmanagementgroup)

Gebruik de volgende procedure een beleidstoewijzing maken en toewijzen van de beleidsdefinitie op het niveau van de resource.

1. Kopieer de volgende JSON-fragment voor het maken van een JSON-bestand voor het toewijzen van beleid. Vervang voorbeeldinformatie in &lt; &gt; symbolen door uw eigen waarden.

   ```json
   {
       "properties": {
           "description": "This policy assignment makes sure that storage accounts with exposure to Public Networks are audited.",
           "displayName": "Audit Storage Accounts Open to Public Networks Assignment",
           "parameters": {},
           "policyDefinitionId": "/subscriptions/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/Audit Storage Accounts Open to Public Networks",
           "scope": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>"
       }
   }
   ```

1. Maak de toewijzing van beleid met behulp van de volgende oproep verzenden:

   ```
   armclient PUT "/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Authorization/policyAssignments/Audit Storage Accounts Open to Public Networks?api-version=2017-06-01-preview" @<path to Assignment JSON file>
   ```

   Vervang voorbeeldinformatie in &lt; &gt; symbolen door uw eigen waarden.

   Zie voor meer informatie over het maken van HTTP-aanroepen naar de REST-API, [Azure REST API-Resources](/rest/api/resources/).

### <a name="create-and-assign-a-policy-definition-with-azure-cli"></a>Maken en toewijzen van een beleidsdefinitie met Azure CLI

Gebruik de volgende procedure voor het maken van een beleidsdefinitie:

1. Kopieer de volgende JSON-fragment voor het maken van een JSON-bestand voor het toewijzen van beleid.

  ```json
  {
      "if": {
          "allOf": [{
                  "field": "type",
                  "equals": "Microsoft.Storage/storageAccounts"
              },
              {
                  "field": "Microsoft.Storage/storageAccounts/networkAcls.defaultAction",
                  "equals": "Allow"
              }
          ]
      },
      "then": {
          "effect": "audit"
      }
  }
  ```

   Zie voor meer informatie over het ontwerpen van een beleidsdefinitie [structuur van beleidsdefinities Azure](../concepts/definition-structure.md).

1. Voer de volgende opdracht om een beleidsdefinitie maken:

   ```azurecli-interactive
   az policy definition create --name 'audit-storage-accounts-open-to-public-networks' --display-name 'Audit Storage Accounts Open to Public Networks' --description 'This policy ensures that storage accounts with exposures to public networks are audited.' --rules '<path to json file>' --mode All
   ```

   De opdracht maakt u een beleidsdefinitie met de naam _Audit Storage Accounts openen met een openbaar netwerk_.
   Zie voor meer informatie over andere parameters die u kunt gebruiken, [az beleidsdefinitie maken](/cli/azure/policy/definition#az-policy-definition-create).

   Wanneer aangeroepen zonder locatieparameters, `az policy definition creation` standaard ingesteld op het opslaan van de beleidsdefinitie in het geselecteerde abonnement van de context van sessies. Als u wilt de definitie van de opslaan naar een andere locatie, gebruik de volgende parameters:

   - **--abonnement** -opslaan naar een ander abonnement. Vereist een _GUID_ waarde voor de abonnements-ID of een _tekenreeks_ waarde voor de naam van het abonnement.
   - **--beheergroep** -opslaan in een beheergroep. Vereist een _tekenreeks_ waarde.

1. Gebruik de volgende opdracht om een beleidstoewijzing te maken. Vervang voorbeeldinformatie in &lt; &gt; symbolen door uw eigen waarden.

   ```azurecli-interactive
   az policy assignment create --name '<name>' --scope '<scope>' --policy '<policy definition ID>'
   ```

   De **--bereik** parameter op `az policy assignment create` werkt met de beheergroep, abonnement, resourcegroep of één resource. De parameter maakt gebruik van een volledige resource-pad. Het patroon voor **--bereik** voor elke container als volgt is. Vervang `{rName}`, `{rgName}`, `{subId}`, en `{mgName}` met de resourcenaam van uw, resourcegroep de naam, abonnements-ID en naam van beheergroep, respectievelijk. `{rType}` zou worden vervangen door de **resourcetype** van de resource, zoals `Microsoft.Compute/virtualMachines` voor een virtuele machine.

   - Resource - `/subscriptions/{subID}/resourceGroups/{rgName}/providers/{rType}/{rName}`
   - Resourcegroep: `/subscriptions/{subID}/resourceGroups/{rgName}`
   - Abonnement: `/subscriptions/{subID}`
   - Beheergroep- `/providers/Microsoft.Management/managementGroups/{mgName}`

U kunt de Beleidstoewijzings-ID opvragen met behulp van PowerShell met de volgende opdracht:

```azurecli-interactive
az policy definition show --name 'Audit Storage Accounts with Open Public Networks'
```

De beleidstoewijzings-ID voor de beleidsdefinitie die u hebt gemaakt, moet lijken op het volgende voorbeeld:

```
"/subscription/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/Audit Storage Accounts Open to Public Networks"
```

Zie voor meer informatie over hoe u resourcebeleid met Azure CLI kunt beheren, [Resourcebeleid voor Azure CLI](/cli/azure/policy?view=azure-cli-latest).

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende artikelen voor meer informatie over de opdrachten en query's in dit artikel.

- [Azure REST API-Resources](/rest/api/resources/)
- [Azure PowerShell-Modules](/powershell/module/az.resources/#policies)
- [Azure CLI-opdrachten voor beleid](/cli/azure/policy?view=azure-cli-latest)
- [Resourceprovider Policy Insights REST API-naslaginformatie](/rest/api/policy-insights)
- [Resources organiseren met beheergroepen voor Azure](../../management-groups/overview.md)