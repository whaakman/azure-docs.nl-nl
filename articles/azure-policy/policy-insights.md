---
title: Programmatisch beleid maken en weergeven compatibiliteitsgegevens met Azure-beleid | Microsoft Docs
description: In dit artikel leidt u door via een programma maken en beheren van beleidsregels voor Azure-beleid.
services: azure-policy
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 03/28/2018
ms.topic: article
ms.service: azure-policy
manager: carmonm
ms.custom: ''
ms.openlocfilehash: 1809f0b7ef386bb9eeaa55982178e4cd5e1dd2e2
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/30/2018
---
# <a name="programmatically-create-policies-and-view-compliance-data"></a>Programmatisch beleid maken en Nalevingsgegevens weergeven

In dit artikel leidt u door via een programma maken en beheren van beleid. Ook ziet u hoe de nalevingsstatussen resource weergeven en het beleid. Beleidsdefinities van het afdwingen verschillende regels en acties via uw resources. Afdwinging zorgt ervoor dat resources voldoen aan uw bedrijfsnormen en de serviceovereenkomsten blijven.

## <a name="prerequisites"></a>Vereisten

Voordat u begint, controleert u of de volgende vereisten wordt voldaan:

1. Als u nog niet gedaan hebt, installeert u de [ARMClient](https://github.com/projectkudu/ARMClient). Er is een hulpprogramma waarmee HTTP-aanvragen worden verzonden naar Azure Resource Manager gebaseerde API's.
2. Werk uw PowerShell-module van Azure RM bij naar de nieuwste versie. Zie voor meer informatie over de nieuwste versie, Azure PowerShell https://github.com/Azure/azure-powershell/releases.
3. Registreer de beleid Insights-resourceprovider met Azure PowerShell om ervoor te zorgen dat uw abonnement met de resourceprovider werkt. Als u een resourceprovider wilt registreren, moet u toestemming hebben om de registreeractie voor de resourceprovider uit te voeren. Deze bewerking is opgenomen in de rollen Inzender en Eigenaar. Voer de volgende opdracht uit om de resourceprovider te registreren:

    ```
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.PolicyInsights
    ```

    Zie voor meer informatie over het registreren en weergeven van de resourceproviders [Resourceproviders en typen](../azure-resource-manager/resource-manager-supported-services.md).

4. Als u nog niet gedaan hebt, moet u Azure CLI installeren. U kunt de meest recente versie op [Azure CLI-2.0 installeren op Windows](/azure/install-azure-cli-windows?view=azure-cli-latest).

## <a name="create-and-assign-a-policy-definition"></a>Maken en toewijzen van een beleidsdefinitie

Er is de eerste stap voor betere zichtbaarheid van uw resources maken en toewijzen van beleid via uw resources. De volgende stap is te leren hoe u via een programma maken en toewijzen van een beleid. Het voorbeeldbeleid audits storage-accounts die zijn geopend met alle openbare netwerken met behulp van PowerShell, Azure CLI en HTTP-aanvragen.

De volgende opdrachten maken beleidsdefinities voor de prijscategorie Standard. De prijscategorie Standard helpt u bij bereiken geschaald management, nalevingsbeoordeling en herstel. Zie voor meer informatie over Prijscategorieën [prijzen van Azure beleid](https://azure.microsoft.com/pricing/details/azure-policy).

### <a name="create-and-assign-a-policy-definition-with-powershell"></a>Maken en toewijzen van de beleidsdefinitie van een met PowerShell

1. Gebruik de volgende JSON-fragment een JSON-bestand met de naam AuditStorageAccounts.json maken.

    ```
    {
    "if": {
      "allOf": [
        {
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

    ```
    PS C:\>New-AzureRmPolicyDefinition -Name "AuditStorageAccounts" -DisplayName "Audit Storage Accounts Open to Public Networks" -Policy C:\AuditStorageAccounts.json
    ```

    De opdracht maakt u een beleidsdefinitie van een met de naam _Audit Storage-Accounts is geopend voor openbare netwerken_. Zie voor meer informatie over andere parameters die u kunt gebruiken, [nieuw AzureRmPolicyDefinition](/powershell/module/azurerm.resources/new-azurermpolicydefinition?view=azurermps-4.4.1).

3. Nadat u de beleidsdefinitie van de maakt, kunt u een beleidstoewijzing maken met de volgende opdrachten:

    ```
$rg = Get-AzureRmResourceGroup -Name "ContosoRG"
```

    ```
$Policy = Get-AzureRmPolicyDefinition -Name "AuditStorageAccounts"
    ```

    ```
New-AzureRmPolicyAssignment -Name "AuditStorageAccounts" -PolicyDefinition $Policy -Scope $rg.ResourceId –Sku @{Name='A1';Tier='Standard'}
    ```

    Vervang _ContosoRG_ met de naam van de beoogde resourcegroep.

Zie voor meer informatie over het beheren van bronbeleid met de Azure Resource Manager PowerShell-module [AzureRM.Resources](/powershell/module/azurerm.resources/?view=azurermps-4.4.1#policies).

### <a name="create-and-assign-a-policy-definition-using-armclient"></a>Maken en toewijzen van de beleidsdefinitie van een met behulp van ARMClient

Gebruik de volgende procedure om de beleidsdefinitie van een te maken.

1. Kopieer het volgende JSON-codefragment voor het maken van een JSON-bestand. U kunt het bestand in de volgende stap gaat aanroepen.

    ```
    {
    "properties": {
        "displayName": "Audit Storage Accounts Open to Public Networks",
        "policyType": "Custom",
        "mode": "Indexed",
        "description": "This policy ensures that storage accounts with exposure to Public Networks are audited.",
        "parameters": {},
        "policyRule": {
              "if": {
                "allOf": [
                  {
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
}
```

2. Maken van de beleidsdefinitie met behulp van de volgende oproep verzenden:

    ```
    armclient PUT "/subscriptions/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/AuditStorageAccounts?api-version=2016-12-01 @<path to policy definition JSON file>"
    ```

    Vervang de preceding_ &lt;subscriptionId&gt; met de ID van uw beoogde abonnement.

Zie voor meer informatie over de structuur van de query [beleidsdefinities – maken of bijwerken](/rest/api/resources/policydefinitions/createorupdate).


Gebruik de volgende procedure een beleidstoewijzing maken en toewijzen van de beleidsdefinitie op niveau van de resourcegroep.

1. Kopieer het volgende JSON-codefragment voor het maken van een JSON-bestand voor het toewijzen van beleid. Van de voorbeeldinformatie in vervangen &lt; &gt; symbolen met uw eigen waarden.

    ```
    {
  "properties": {
"description": "This policy assignment makes sure that storage accounts with exposure to Public Networks are audited.",
"displayName": "Audit Storage Accounts Open to Public Networks Assignment",
"parameters": {},
"policyDefinitionId":"/subscriptions/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/Audit Storage Accounts Open to Public Networks",
"scope": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>"
},
"sku": {
    "name": "A1",
    "tier": "Standard"
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

    ```
    {
                  "if": {
                    "allOf": [
                      {
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

    ```
az policy definition create --name 'audit-storage-accounts-open-to-public-networks' --display-name 'Audit Storage Accounts Open to Public Networks' --description 'This policy ensures that storage accounts with exposures to public networks are audited.' --rules '<path to json file>' --mode All
    ```

Gebruik de volgende opdracht voor het maken van een beleidstoewijzing. Van de voorbeeldinformatie in vervangen &lt; &gt; symbolen met uw eigen waarden.

```
az policy assignment create --name '<Audit Storage Accounts Open to Public Networks in Contoso RG' --scope '<scope>' --policy '<policy definition ID>' --sku 'standard'
```

U kunt de Beleidstoewijzings-ID ophalen met behulp van PowerShell met de volgende opdracht:

```
az policy definition show --name 'Audit Storage Accounts with Open Public Networks'
```

De beleidsdefinitie-ID voor de beleidsdefinitie van het dat u hebt gemaakt, moet eruitzien als in het volgende voorbeeld:

```
"/subscription/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/Audit Storage Accounts Open to Public Networks"
```

Zie voor meer informatie over hoe u bronbeleid met Azure CLI kunt beheren, [Azure CLI bronbeleid](/cli/azure/policy?view=azure-cli-latest).

## <a name="identify-non-compliant-resources"></a>Niet-compatibele resources identificeren

Een bron is niet-compatibel als deze niet aan beleid of initiatief regels in een toewijzing. De volgende tabel toont hoe verschillende beleidsacties werken met de evaluatie van de voorwaarde voor de resulterende compatibiliteitsstatus:

| **Resource-status** | **Actie** | **Evaluatie van het beleid** | **Compatibiliteitsstatus** |
| --- | --- | --- | --- |
| Bestaat | Weigeren, controleren, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | True | Niet-compatibel |
| Bestaat | Weigeren, controleren, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | False | Compatibel |
| Nieuw | Audit, AuditIfNotExist\* | True | Niet-compatibel |
| Nieuw | Audit, AuditIfNotExist\* | False | Compatibel |

\* De acties Append, DeployIfNotExist en AuditIfNotExist vereisen de instructie om te worden voldaan. Welke acties moeten echter ook de voorwaarde bestaan moet FALSE zijn niet compatibel. Indien TRUE, activeert de voorwaarde als evaluatie van de voorwaarde bestaan voor de bijbehorende resources.

Om beter te begrijpen hoe resources worden gemarkeerd als niet-compatibel, gaan we gebruiken de toewijzing van voorbeeld van een beleid dat hierboven is gemaakt.

Stel bijvoorbeeld dat er een resourcegroep: ContsoRG, met bepaalde opslagaccounts (rood gemarkeerd) die zichtbaar zijn voor openbare netwerken.

![Storage-accounts die zijn blootgesteld aan openbare netwerken](./media/policy-insights/resource-group01.png)

In dit voorbeeld moet u Let van beveiligingsrisico's. Nu dat u een beleidstoewijzing gemaakt hebt, wordt deze voor alle opslagaccounts in de resourcegroep ContosoRG geëvalueerd. Gebeurtenissen uit te voeren op de drie niet-compatibele storage-accounts, als gevolg daarvan wijzigen van hun statussen voor **niet-compatibel.**

![Niet-compatibele storage-accounts gecontroleerd](./media/policy-insights/resource-group03.png)

Gebruik de volgende procedure om resources in een resourcegroep die niet compatibel zijn met de beleidstoewijzing. In het voorbeeld zijn de resources storage-accounts in de resourcegroep ContosoRG.

1. De beleidstoewijzings-ID ophalen met de volgende opdrachten:

    ```
    $policyAssignment = Get-AzureRmPolicyAssignment | where {$_.properties.displayName -eq "Audit Storage Accounts with Open Public Networks"}
    ```

    ```
    $policyAssignment.PolicyAssignmentId
    ```

    Zie voor meer informatie over het ophalen van een beleidstoewijzing ID [Get-AzureRMPolicyAssignment](https://docs.microsoft.com/en-us/powershell/module/azurerm.resources/Get-AzureRmPolicyAssignment?view=azurermps-4.4.1).

2. Voer de volgende opdracht om de resource-id van de niet-compatibele bronnen gekopieerd naar een JSON-bestand:

    ```
    armclient post "/subscriptions/<subscriptionID>/resourceGroups/<rgName>/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2017-12-12-preview&$filter=IsCompliant eq false and PolicyAssignmentId eq '<policyAssignmentID>'&$apply=groupby((ResourceId))" > <json file to direct the output with the resource IDs into>
    ```

3. De resultaten moeten eruitzien als in het volgende voorbeeld:

  ```
      {
  "@odata.context":"https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest",
  "@odata.count": 3,
  "value": [
  {
      "@odata.id": null,
      "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
        "ResourceId": "/subscriptions/<subscriptionId>/resourcegroups/<rgname>/providers/microsoft.storage/storageaccounts/<storageaccount1Id>"
      },
      {
        "@odata.id": null,
        "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
        "ResourceId": "/subscriptions/<subscriptionId>/resourcegroups/<rgname>/providers/microsoft.storage/storageaccounts/<storageaccount2Id>"
             },
  {
        "@odata.id": null,
        "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
        "ResourceId": "/subscriptions/<subscriptionName>/resourcegroups/<rgname>/providers/microsoft.storage/storageaccounts/<storageaccount3ID>"
             }
  ]
  }
  ```

De resultaten gelijk zijn aan die u doorgaans die worden vermeld ziet onder **niet-compatibel resources** in de [Azure portalweergave](assign-policy-definition.md#identify-non-compliant-resources).

Niet-compatibele bronnen zijn momenteel alleen geïdentificeerd met de Azure portal en met HTTP-aanvragen. Zie voor meer informatie over het uitvoeren van query's beleid statussen, de [beleidsstatus](/rest/api/policy-insights/policystates) API referentieartikel.

## <a name="view-policy-events"></a>De beleid-gebeurtenissen weergeven

Wanneer een bron wordt gemaakt of bijgewerkt, wordt een resultaat van evaluatie van beleid wordt gegenereerd. Resultaten heten _Beleidsgebeurtenissen_. Voer de volgende query om alle Beleidsgebeurtenissen die zijn gekoppeld aan de beleidstoewijzing weer te geven.

```
armclient POST "/subscriptions/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/Audit Storage Accounts Open to Public Networks/providers/Microsoft.PolicyInsights/policyEvents/default/queryResults?api-version=2017-12-12-preview"
```

De resultaten zien er ongeveer als volgt uit:

```
{
  "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyEvents/$metadata#default",
  "@odata.count": 1,
  "value": [
    {
      "@odata.id": null,
      "@odata.context": "https://management.azure.com/subscriptions/<subscriptionId>/providers/Microsoft.PolicyInsights/policyEvents/$metadata#default/$entity",
      "NumAuditEvents": 3
    }
  ]
}

```

U kunt alleen Beleidsgebeurtenissen met HTTP-aanvragen weergeven zoals beleid-statussen. Zie voor meer informatie over het uitvoeren van query's Beleidsgebeurtenissen de [Beleidsgebeurtenissen](/rest/api/policy-insights/policyevents) artikel.

## <a name="change-a-policy-assignments-pricing-tier"></a>Wijzig de beleidstoewijzing van een van prijscategorie

U kunt de *Set AzureRmPolicyAssignment* PowerShell-cmdlet om bij te werken met prijzen laag naar Standard of vrije voor een bestaande beleidstoewijzing van. Bijvoorbeeld:

```
Set-AzureRmPolicyAssignment -Id /subscriptions/<subscriptionId/resourceGroups/<resourceGroupName>/providers/Microsoft.Authorization/policyAssignments/<policyAssignmentID> -Sku @{Name='A1';Tier='Standard'}
```

Zie voor meer informatie over de cmdlet [Set AzureRmPolicyAssignment](/powershell/module/azurerm.resources/Set-AzureRmPolicyAssignment?view=azurermps-4.4.1).

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende artikelen voor meer informatie over de opdrachten en query's in dit artikel.

- [Resources voor Azure REST-API](/rest/api/resources/)
- [Azure RM-PowerShell-Modules](/powershell/module/azurerm.resources/?view=azurermps-4.4.1#policies)
- [Azure CLI-opdrachten voor beleid](/cli/azure/policy?view=azure-cli-latest)
- [Beleid Insights-resourceprovider naslaginformatie over REST API](/rest/api/policy-insights)
