---
title: Azure resource health waarschuwingen configureren met Resource Manager-sjablonen | Microsoft Docs
description: Maak waarschuwingen via een programma dat een melding wanneer uw Azure-resources niet meer beschikbaar.
author: shawntabrizi
manager: scotthit
editor: ''
services: service-health
documentationcenter: service-health
ms.assetid: ''
ms.service: service-health
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 9/4/2018
ms.author: shtabriz
ms.openlocfilehash: ee5712915e552168c0268a44af60ea58e069fc11
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54429505"
---
# <a name="configure-resource-health-alerts-using-resource-manager-templates"></a>Resource health waarschuwingen configureren met Resource Manager-sjablonen

Dit artikel wordt beschreven hoe u Resource Health waarschuwingen voor activiteitenlogboeken programmatisch met behulp van Azure Resource Manager-sjablonen en Azure PowerShell maakt.

Azure Resource Health houdt die u geïnformeerd over de huidige en historische status van uw Azure-resources. Waarschuwingen van Azure Resource Health u kunnen een melding in bijna realtime wanneer deze resources een wijziging in hun status hebben. Het maken van Resource Health kunnen waarschuwingen via een programma gebruikers maken en aanpassen van meldingen in één bulkbewerking.

## <a name="prerequisites"></a>Vereisten

Volg de instructies op deze pagina, moet u van tevoren een aantal dingen instellen:

1. U moet installeren de [Azure PowerShell-module](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps) (`AzureRm`)
2. U moet [maken of een actiegroep hergebruiken](../azure-monitor/platform/action-groups.md) geconfigureerd om u te waarschuwen

## <a name="instructions"></a>Instructies
1. Met behulp van PowerShell, meld u aan bij Azure met behulp van uw account en selecteer het abonnement dat u wilt communiceren

        Login-AzureRmAccount
        Select-AzureRmSubscription -Subscription <subscriptionId>

    > U kunt `Get-AzureRmSubscription` om de abonnementen weer te geven die u hebt toegang tot.

2. Zoeken en opslaan van de volledige Azure Resource Manager-ID voor de actiegroep

        (Get-AzureRmActionGroup -ResourceGroupName <resourceGroup> -Name <actionGroup>).Id

3. Maken en opslaan van een Resource Manager-sjabloon voor Resource Health waarschuwingen als `resourcehealthalert.json` ([hieronder voor meer informatie Zie](#resource-manager-template-for-resource-health-alerts))

4. Maak een nieuwe Azure Resource Manager-implementatie met behulp van deze sjabloon

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName <resourceGroup> -TemplateFile <path\to\resourcehealthalert.json>

5. U wordt gevraagd naar het type in de naam van de waarschuwing en de actie groep Resource-ID die u eerder hebt gekopieerd:

        Supply values for the following parameters:
        (Type !? for Help.)
        activityLogAlertName: <Alert Name>
        actionGroupResourceId: /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/microsoft.insights/actionGroups/<actionGroup>

6. Als alles correct werkt, krijgt u een bevestiging in PowerShell

        DeploymentName          : ExampleDeployment
        ResourceGroupName       : <resourceGroup>
        ProvisioningState       : Succeeded
        Timestamp               : 11/8/2017 2:32:00 AM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
                                Name                     Type       Value
                                ===============          =========  ==========
                                activityLogAlertName     String     <Alert Name>
                                activityLogAlertEnabled  Bool       True
                                actionGroupResourceId    String     /...
        
        Outputs                 :
        DeploymentDebugLogLevel :

Houd er rekening mee dat als u van plan bent over het volledig automatiseren van dit proces, gewoon moet u de Resource Manager-sjabloon wordt niet gevraagd om de waarden in stap 5 bewerken.

## <a name="resource-manager-template-for-resource-health-alerts"></a>Resource Manager-sjabloon voor waarschuwingen van de Resource Health

U kunt deze basissjabloon gebruiken als uitgangspunt voor het maken van Resource Health waarschuwingen. Deze sjabloon werkt zoals geschreven en wordt Meld u voor het ontvangen van waarschuwingen voor alle zojuist geactiveerde resource health-gebeurtenissen voor alle resources in een abonnement.

> Aan de onderkant van dit artikel hebben we ook complexere waarschuwing sjabloon die u om het signaal ruis verhouding voor waarschuwingen van de Resource Health in vergelijking met deze sjabloon verhogen moet opgenomen.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "activityLogAlertName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Activity log alert."
      }
    },
    "actionGroupResourceId": {
      "type": "string",
      "metadata": {
        "description": "Resource Id for the Action group."
      }
    }
  },
  "resources": [   
    {
      "type": "Microsoft.Insights/activityLogAlerts",
      "apiVersion": "2017-04-01",
      "name": "[parameters('activityLogAlertName')]",      
      "location": "Global",
      "properties": {
        "enabled": true,
        "scopes": [
            "[subscription().id]"
        ],        
        "condition": {
          "allOf": [
            {
              "field": "category",
              "equals": "ResourceHealth"
            },
            {
              "field": "status",
              "equals": "Active"
            }
          ]
        },
        "actions": {
          "actionGroups":
          [
            {
              "actionGroupId": "[parameters('actionGroupResourceId')]"
            }
          ]
        }
      }
    }
  ]
}
```

Een waarschuwing voor een brede zoals deze wordt echter in het algemeen niet aanbevolen. Meer informatie over hoe u deze waarschuwing te concentreren op de gebeurtenissen die is belangrijk voor ons hieronder beperken.

### <a name="adjusting-the-alert-scope"></a>De waarschuwing bereik aanpassen

Resource Health waarschuwingen kunnen worden geconfigureerd voor het controleren van gebeurtenissen op drie verschillende niveaus:

 * Abonnementsniveau
 * Het niveau van resourcegroep
 * Resourceniveau

De waarschuwing sjabloon is geconfigureerd op abonnementsniveau, maar als u configureren van de waarschuwing alleen om u te waarschuwen over bepaalde resources of resources binnen een bepaalde resourcegroep wilt, moet u gewoon wijzigen de `scopes` sectie in het bovenstaande de sjabloon.

Voor een resource niveau groepsbereik, de sectie bereiken er moet uitzien:
```json
"scopes": [
    "/subscriptions/<subscription id>/resourcegroups/<resource group>"
],
```

En voor een bereik van de resource-niveau, de sectie bereik moet uitzien:

```json
"scopes": [
    "/subscriptions/<subscription id>/resourcegroups/<resource group>/providers/<resource>"
],
```

Bijvoorbeeld: `"/subscriptions/d37urb3e-ed41-4670-9c19-02a1d2808ff9/resourcegroups/myRG/providers/microsoft.compute/virtualmachines/myVm"`

> U kunt naar de Azure-Portal gaan en kijken naar de URL bij het weergeven van uw Azure-resource om op te halen van deze tekenreeks.

### <a name="adjusting-the-resource-types-which-alert-you"></a>Aanpassen van de resource gegevenstypen die u waarschuwen

Waarschuwingen op het abonnement of resourcegroep hebben mogelijk verschillende soorten resources. Als u beperken waarschuwingen alleen afkomstig zijn van een bepaalde subset van de resourcetypen wilt, kunt u definiëren die in de `condition` gedeelte van de sjabloon als volgt te werk:

```json
"condition": {
    "allOf": [
        ...,
        {
            "anyOf": [
                {
                    "field": "resourceType",
                    "equals": "Microsoft.Compute/virtualMachines",
                    "containsAny": null
                },
                {
                    "field": "resourceType",
                    "equals": "Microsoft.Storage/storageAccounts",
                    "containsAny": null
                },
                ...
            ]
        }
    ]
},
```

Hier gebruiken we de `anyOf` wrapper om toe te staan van de resource health waarschuwing zodat deze overeenkomt met een van de voorwaarden we opgeven, zodat voor waarschuwingen die zijn gericht op specifieke brontypen.

### <a name="adjusting-the-resource-health-events-that-alert-you"></a>De Resource Health-gebeurtenissen waarin wordt aangegeven dat aanpassen
Wanneer resources worden onderworpen aan een statusgebeurtenis, kunnen ze via een reeks fasen die de status van de statusgebeurtenis vertegenwoordigt: `Active`, `InProgress`, `Updated`, en `Resolved`.

U kunt alleen een melding wanneer een resource niet in orde wordt, in welk geval u wilt configureren van de waarschuwing om u te waarschuwen als alleen de `status` is `Active`. Als u ook meldingen over de andere fasen wilt, kunt u echter de gegevens toevoegen als volgt te werk:

```json
"condition": {
    "allOf": [
        ...,
        {
            "anyOf": [
                {
                    "field": "status",
                    "equals": "Active"
                },
                {
                    "field": "status",
                    "equals": "InProgress"
                },
                        {
                    "field": "status",
                    "equals": "Resolved"
                }
            ]
        }
    ]
}
```

Als u wilt worden gewaarschuwd voor alle vier fasen van de health-gebeurtenissen, u dit probleem alles bij elkaar verwijderen kunt en de waarschuwing u ongeacht ontvangt de `status` eigenschap.

### <a name="adjusting-the-resource-health-alerts-to-avoid-unknown-events"></a>De Resource Health waarschuwingen om te voorkomen dat 'Onbekend' gebeurtenissen aanpassen

Azure Resource Health kunt melden aan u de meest recente status van uw resources door voortdurend controleren of ze met behulp van test runners-up. De relevante gerapporteerde health-statussen zijn: 'Beschikbaar', 'Niet beschikbaar' en 'Verminderde'. Echter in situaties waarin de runner en de Azure-resource kan niet communiceren, de status van een 'Onbekende' Status gerapporteerd voor de resource en die wordt beschouwd als een 'Active' health-gebeurtenis.

Echter, wanneer een resource meldt 'Onbekend', is het waarschijnlijk dat de status niet is gewijzigd sinds de laatste nauwkeurige rapport. Als u wilt verwijderen van waarschuwingen voor 'Onbekende' gebeurtenissen, kunt u deze logica kunt opgeven in de sjabloon:

```json
"condition": {
    "allOf": [
        ...,
        {
            "anyOf": [
                {
                    "field": "properties.currentHealthStatus",
                    "equals": "Available",
                    "containsAny": null
                },
                {
                    "field": "properties.currentHealthStatus",
                    "equals": "Unavailable",
                    "containsAny": null
                },
                {
                    "field": "properties.currentHealthStatus",
                    "equals": "Degraded",
                    "containsAny": null
                }
            ]
        },
        {
            "anyOf": [
                {
                    "field": "properties.previousHealthStatus",
                    "equals": "Available",
                    "containsAny": null
                },
                {
                    "field": "properties.previousHealthStatus",
                    "equals": "Unavailable",
                    "containsAny": null
                },
                {
                    "field": "properties.previousHealthStatus",
                    "equals": "Degraded",
                    "containsAny": null
                }
            ]
        },
    ]
},
```

In dit voorbeeld zijn we alleen meldingen van gebeurtenissen waarin de huidige en vorige status geen 'Onbekend'. Deze wijziging kan een nuttige aanvulling zijn als de waarschuwingen rechtstreeks naar uw mobiele telefoon of e-mail worden verzonden.

### <a name="adjusting-the-alert-to-avoid-user-initiated-events"></a>De waarschuwing om te voorkomen dat gebeurtenissen door de gebruiker geïnitieerde aanpassen

Resource Health-gebeurtenissen kunnen de trigger per platform gestart en door de gebruiker geïnitieerde gebeurtenissen. Het wellicht verstandig alleen een melding verzenden wanneer de health-gebeurtenis is veroorzaakt door het Azure-platform.

Het is eenvoudig te configureren van de waarschuwing om te filteren voor dit soort gebeurtenissen:

```json
"condition": {
    "allOf": [
        ...,
        {
            "field": "properties.cause",
            "equals": "PlatformInitiated",
            "containsAny": null
        }
    ]
}
```

## <a name="recommended-resource-health-alert-template"></a>Aanbevolen waarschuwingen Resource Health-sjabloon

Met behulp van de andere aanpassingen die in de vorige sectie worden beschreven, kunnen we een uitgebreide waarschuwing sjabloon die is geconfigureerd voor het maximaliseren van het signaal ruis verhouding maken.

Dit is wat we raden dat u gebruikt:
```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "activityLogAlertName": {
            "type": "string",
            "metadata": {
                "description": "Unique name (within the Resource Group) for the Activity log alert."
            }
        },
        "actionGroupResourceId": {
            "type": "string",
            "metadata": {
                "description": "Resource Id for the Action group."
            }
        }
    },
    "resources": [
        {
            "type": "Microsoft.Insights/activityLogAlerts",
            "apiVersion": "2017-04-01",
            "name": "[parameters('activityLogAlertName')]",
            "location": "Global",
            "properties": {
                "enabled": true,
                "scopes": [
                    "[subscription().id]"
                ],
                "condition": {
                    "allOf": [
                        {
                            "field": "category",
                            "equals": "ResourceHealth",
                            "containsAny": null
                        },
                        {
                            "anyOf": [
                                {
                                    "field": "properties.currentHealthStatus",
                                    "equals": "Available",
                                    "containsAny": null
                                },
                                {
                                    "field": "properties.currentHealthStatus",
                                    "equals": "Unavailable",
                                    "containsAny": null
                                },
                                {
                                    "field": "properties.currentHealthStatus",
                                    "equals": "Degraded",
                                    "containsAny": null
                                }
                            ]
                        },
                        {
                            "anyOf": [
                                {
                                    "field": "properties.previousHealthStatus",
                                    "equals": "Available",
                                    "containsAny": null
                                },
                                {
                                    "field": "properties.previousHealthStatus",
                                    "equals": "Unavailable",
                                    "containsAny": null
                                },
                                {
                                    "field": "properties.previousHealthStatus",
                                    "equals": "Degraded",
                                    "containsAny": null
                                }
                            ]
                        },
                        {
                            "anyOf": [
                                {
                                    "field": "properties.cause",
                                    "equals": "PlatformInitiated",
                                    "containsAny": null
                                }
                            ]
                        },
                        {
                            "anyOf": [
                                {
                                    "field": "status",
                                    "equals": "Active",
                                    "containsAny": null
                                },
                                {
                                    "field": "status",
                                    "equals": "Resolved",
                                    "containsAny": null
                                },
                                {
                                    "field": "status",
                                    "equals": "InProgress",
                                    "containsAny": null
                                }
                            ]
                        }
                    ]
                },
                "actions": {
                    "actionGroups": [
                        {
                            "actionGroupId": "[parameters('actionGroupResourceId')]"
                        }
                    ]
                }
            }
        }
    ]
}
```

U zult echter het beste kent welke configuraties zijn geschikt voor u, zodat de hulpprogramma's die u in deze documentatie onderwezen gebruiken om te maken van uw eigen aanpassingen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Resource Health:
-  [Overzicht van Azure Resource Health](Resource-health-overview.md)
-  [Resourcetypen en statuscontroles die beschikbaar zijn via Azure Resource Health](resource-health-checks-resource-types.md)

Service Health waarschuwingen maken:
-  [Waarschuwingen voor de status van de Service configureren](../azure-monitor/platform/alerts-activity-log-service-notifications.md) 
