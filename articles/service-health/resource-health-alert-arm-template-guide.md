---
title: Azure resource health waarschuwingen configureren met Resource Manager-sjablonen | Microsoft Docs
description: Maak waarschuwingen via een programma dat een melding wanneer uw Azure-resources niet meer beschikbaar.
author: stephbaron
ms.author: stbaron
ms.topic: conceptual
ms.service: service-health
ms.date: 9/4/2018
ms.openlocfilehash: e552b90a4c98eff7b04cbeaef26aa10749ce672c
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66493402"
---
# <a name="configure-resource-health-alerts-using-resource-manager-templates"></a>Resource health waarschuwingen configureren met Resource Manager-sjablonen

Dit artikel wordt beschreven hoe u Resource Health waarschuwingen voor activiteitenlogboeken programmatisch met behulp van Azure Resource Manager-sjablonen en Azure PowerShell maakt.

Azure Resource Health houdt die u geïnformeerd over de huidige en historische status van uw Azure-resources. Waarschuwingen van Azure Resource Health u kunnen een melding in bijna realtime wanneer deze resources een wijziging in hun status hebben. Het maken van Resource Health kunnen waarschuwingen via een programma gebruikers maken en aanpassen van meldingen in één bulkbewerking.

> [!NOTE]
> Resource Health waarschuwingen zijn momenteel in preview.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten

Volg de instructies op deze pagina, moet u van tevoren een aantal dingen instellen:

1. U moet installeren de [Azure PowerShell-module](https://docs.microsoft.com/powershell/azure/install-Az-ps)
2. U moet [maken of een actiegroep hergebruiken](../azure-monitor/platform/action-groups.md) geconfigureerd om u te waarschuwen

## <a name="instructions"></a>Instructies
1. Met behulp van PowerShell, meld u aan bij Azure met behulp van uw account en selecteer het abonnement dat u wilt communiceren

        Login-AzAccount
        Select-AzSubscription -Subscription <subscriptionId>

    > U kunt `Get-AzSubscription` om de abonnementen weer te geven die u hebt toegang tot.

2. Zoeken en opslaan van de volledige Azure Resource Manager-ID voor de actiegroep

        (Get-AzActionGroup -ResourceGroupName <resourceGroup> -Name <actionGroup>).Id

3. Maken en opslaan van een Resource Manager-sjabloon voor Resource Health waarschuwingen als `resourcehealthalert.json` ([hieronder voor meer informatie Zie](#resource-manager-template-options-for-resource-health-alerts))

4. Maak een nieuwe Azure Resource Manager-implementatie met behulp van deze sjabloon

        New-AzResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName <resourceGroup> -TemplateFile <path\to\resourcehealthalert.json>

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

## <a name="resource-manager-template-options-for-resource-health-alerts"></a>Resource Manager Sjabloonopties voor waarschuwingen van de Resource Health

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
                },
                {
                    "field": "status",
                    "equals": "Updated"
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

Houd er rekening mee dat het is mogelijk voor de eigenschappen currentHealthStatus en previousHealthStatus moet null zijn in sommige gebeurtenissen. Bijvoorbeeld, wanneer een bijgewerkte gebeurtenis is het waarschijnlijk dat de status van de resource niet is gewijzigd sinds het laatste rapport alleen die informatie over aanvullende gebeurtenissen beschikbaar is (bijvoorbeeld veroorzaken). Daarom met behulp van de bovenstaande component kan leiden tot bepaalde waarschuwingen die niet worden geactiveerd, omdat de waarden voor properties.currentHealthStatus en properties.previousHealthStatus wordt ingesteld op null.

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
Houd er rekening mee dat het is mogelijk veroorzaakt dit veld niet null zijn in sommige gebeurtenissen. Dat wil zeggen, een overgang health plaatsvindt (bijvoorbeeld beschikbaar voor niet beschikbaar) en de gebeurtenis is vastgelegd onmiddellijk te voorkomen dat de melding vertragingen. Daarom met behulp van de bovenstaande component kan leiden tot een waarschuwing niet wordt geactiveerd, omdat de waarde van de eigenschap properties.clause wordt ingesteld op null.

## <a name="complete-resource-health-alert-template"></a>Volledige Resource Health waarschuwing sjabloon

Met behulp van de andere aanpassingen die worden beschreven in de vorige sectie, als volgt een voorbeeldsjabloon die is geconfigureerd voor het maximaliseren van het signaal verhouding van de ruis. Houd er rekening mee valkuilen een notitie waarin de currentHealthStatus, previousHealthStatus en eigenschapswaarden oorzaak mag niet null zijn in sommige gebeurtenissen.

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
                                },
                                {
                                    "field": "status",
                                    "equals": "Updated",
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
-  [Gebeurtenisschema in het Azure-activiteitenlogboek](../azure-monitor/platform/activity-log-schema.md)
