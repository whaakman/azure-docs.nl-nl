---
title: Programmatisch beleid maken en Nalevingsgegevens met Azure-beleid weergeven
description: In dit artikel leidt u door via een programma maken en beheren van beleidsregels voor Azure-beleid.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/24/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: a83402316854b23fe85bff813dc9f5665bccd1fb
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794806"
---
# <a name="programmatically-create-policies-and-view-compliance-data"></a>Programmatisch beleid maken en Nalevingsgegevens weergeven

In dit artikel leidt u door via een programma maken en beheren van beleid. Ook ziet u hoe de nalevingsstatussen resource weergeven en het beleid. Beleidsdefinities van het afdwingen verschillende regels en effecten via uw resources. Afdwinging zorgt ervoor dat resources voldoen aan uw bedrijfsnormen en de serviceovereenkomsten blijven.

## <a name="prerequisites"></a>Vereisten

Voordat u begint, controleert u of de volgende vereisten wordt voldaan:

1. Als u nog niet gedaan hebt, installeert u de [ARMClient](https://github.com/projectkudu/ARMClient). Er is een hulpprogramma waarmee HTTP-aanvragen worden verzonden naar Azure Resource Manager gebaseerde API's.
2. Werk uw PowerShell-module van Azure RM bij naar de nieuwste versie. Zie voor meer informatie over de nieuwste versie [Azure PowerShell](https://github.com/Azure/azure-powershell/releases).
3. Registreer de beleid Insights-resourceprovider met Azure PowerShell om ervoor te zorgen dat uw abonnement met de resourceprovider werkt. Als u een resourceprovider wilt registreren, moet u toestemming hebben om de registreeractie voor de resourceprovider uit te voeren. Deze bewerking is opgenomen in de rollen Inzender en Eigenaar. Voer de volgende opdracht uit om de resourceprovider te registreren:

  ```azurepowershell-interactive
  Register-AzureRmResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
  ```

  Zie voor meer informatie over het registreren en weergeven van de resourceproviders [Resourceproviders en typen](../azure-resource-manager/resource-manager-supported-services.md).
4. Als u nog niet gedaan hebt, moet u Azure CLI installeren. U kunt de meest recente versie op [Azure CLI-2.0 installeren op Windows](/cli/azure/install-azure-cli-windows).

## <a name="create-and-assign-a-policy-definition"></a>Maken en toewijzen van een beleidsdefinitie

Er is de eerste stap voor betere zichtbaarheid van uw resources maken en toewijzen van beleid via uw resources. De volgende stap is te leren hoe u via een programma maken en toewijzen van een beleid. Het voorbeeldbeleid audits storage-accounts die zijn geopend met alle openbare netwerken met behulp van PowerShell, Azure CLI en HTTP-aanvragen.

### <a name="create-and-assign-a-policy-definition-with-powershell"></a>Maken en toewijzen van de beleidsdefinitie van een met PowerShell

1. Gebruik de volgende JSON-fragment een JSON-bestand met de naam AuditStorageAccounts.json maken.

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

  Zie voor meer informatie over het ontwerpen van een beleidsdefinitie [Azure definitie beleidsstructuur](policy-definition.md).
2. Voer de volgende opdracht voor het maken van de beleidsdefinitie van een met behulp van het bestand AuditStorageAccounts.json.

  ```azurepowershell-interactive
  New-AzureRmPolicyDefinition -Name 'AuditStorageAccounts' -DisplayName 'Audit Storage Accounts Open to Public Networks' -Policy 'AuditStorageAccounts.json'
  ```

  De opdracht maakt u een beleidsdefinitie van een met de naam _Audit Storage-Accounts is geopend voor openbare netwerken_. Zie voor meer informatie over andere parameters die u kunt gebruiken, [nieuw AzureRmPolicyDefinition](/powershell/module/azurerm.resources/new-azurermpolicydefinition).
3. Nadat u de beleidsdefinitie van de maakt, kunt u een beleidstoewijzing maken met de volgende opdrachten:

  ```azurepowershell-interactive
  $rg = Get-AzureRmResourceGroup -Name 'ContosoRG'
  $Policy = Get-AzureRmPolicyDefinition -Name 'AuditStorageAccounts'
  New-AzureRmPolicyAssignment -Name 'AuditStorageAccounts' -PolicyDefinition $Policy -Scope $rg.ResourceId
  ```

  Vervang _ContosoRG_ met de naam van de beoogde resourcegroep.

Zie voor meer informatie over het beheren van bronbeleid met de Azure Resource Manager PowerShell-module [AzureRM.Resources](/powershell/module/azurerm.resources/#policies).

### <a name="create-and-assign-a-policy-definition-using-armclient"></a>Maken en toewijzen van de beleidsdefinitie van een met behulp van ARMClient

Gebruik de volgende procedure om de beleidsdefinitie van een te maken.

1. Kopieer het volgende JSON-codefragment voor het maken van een JSON-bestand. U kunt het bestand in de volgende stap gaat aanroepen.

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

2. De beleidsdefinitie met een van de volgende aanroepen maken:

  ```
  # For defining a policy in a subscription
  armclient PUT "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/AuditStorageAccounts?api-version=2016-12-01" @<path to policy definition JSON file>

  # For defining a policy in a management group
  armclient PUT "/providers/Microsoft.Management/managementgroups/{managementGroupId}/providers/Microsoft.Authorization/policyDefinitions/AuditStorageAccounts?api-version=2016-12-01" @<path to policy definition JSON file>
  ```

  De voorgaande {subscriptionId} vervangen door de ID van uw abonnement of {managementGroupId} met de ID van uw [beheergroep](../azure-resource-manager/management-groups-overview.md).

  Zie voor meer informatie over de structuur van de query [beleidsdefinities – maken of bijwerken](/rest/api/resources/policydefinitions/createorupdate) en [beleidsdefinities – maken of bijwerken op beheergroep](/rest/api/resources/policydefinitions/createorupdateatmanagementgroup)

Gebruik de volgende procedure een beleidstoewijzing maken en toewijzen van de beleidsdefinitie op niveau van de resourcegroep.

1. Kopieer het volgende JSON-codefragment voor het maken van een JSON-bestand voor het toewijzen van beleid. Van de voorbeeldinformatie in vervangen &lt; &gt; symbolen met uw eigen waarden.

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

2. Maak de toewijzing van beleid met behulp van de volgende oproep verzenden:

  ```
  armclient PUT "/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Authorization/policyAssignments/Audit Storage Accounts Open to Public Networks?api-version=2017-06-01-preview" @<path to Assignment JSON file>
  ```

  Van de voorbeeldinformatie in vervangen &lt; &gt; symbolen met uw eigen waarden.

  Zie voor meer informatie over het maken van HTTP-aanroepen van de REST-API [Azure REST API Resources](/rest/api/resources/).

### <a name="create-and-assign-a-policy-definition-with-azure-cli"></a>Maken en toewijzen van de beleidsdefinitie van een met Azure CLI

Gebruik de volgende procedure voor het maken van een definitie voor:

1. Kopieer het volgende JSON-codefragment voor het maken van een JSON-bestand voor het toewijzen van beleid.

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

2. Voer de volgende opdracht om de beleidsdefinitie van een te maken:

  ```azurecli-interactive
az policy definition create --name 'audit-storage-accounts-open-to-public-networks' --display-name 'Audit Storage Accounts Open to Public Networks' --description 'This policy ensures that storage accounts with exposures to public networks are audited.' --rules '<path to json file>' --mode All
  ```

3. Gebruik de volgende opdracht voor het maken van een beleidstoewijzing. Van de voorbeeldinformatie in vervangen &lt; &gt; symbolen met uw eigen waarden.

  ```azurecli-interactive
  az policy assignment create --name '<name>' --scope '<scope>' --policy '<policy definition ID>'
  ```

U kunt de Beleidstoewijzings-ID ophalen met behulp van PowerShell met de volgende opdracht:

```azurecli-interactive
az policy definition show --name 'Audit Storage Accounts with Open Public Networks'
```

De beleidsdefinitie-ID voor de beleidsdefinitie van het dat u hebt gemaakt, moet eruitzien als in het volgende voorbeeld:

```
"/subscription/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/Audit Storage Accounts Open to Public Networks"
```

Zie voor meer informatie over hoe u bronbeleid met Azure CLI kunt beheren, [Azure CLI bronbeleid](/cli/azure/policy?view=azure-cli-latest).

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende artikelen voor meer informatie over de opdrachten en query's in dit artikel.

- [Resources voor Azure REST-API](/rest/api/resources/)
- [Azure RM-PowerShell-Modules](/powershell/module/azurerm.resources/#policies)
- [Azure CLI-opdrachten voor beleid](/cli/azure/policy?view=azure-cli-latest)
- [Beleid Insights-resourceprovider naslaginformatie over REST API](/rest/api/policy-insights)
- [Breng uw resources met Azure-beheergroepen](../azure-resource-manager/management-groups-overview.md)