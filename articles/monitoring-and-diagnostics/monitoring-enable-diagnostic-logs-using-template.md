---
title: Automatisch inschakelen van diagnostische instellingen met een Resource Manager-sjabloon | Microsoft Docs
description: Informatie over het Resource Manager-sjabloon gebruiken om diagnostische instellingen waarmee u kunt uw logboeken met diagnostische gegevens naar Event Hubs stream of op te slaan in een opslagaccount te maken.
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: a8a88a8c-4a48-4df6-8f7e-d90634d39c57
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/22/2017
ms.author: johnkem
ms.openlocfilehash: 6355433dab7bac910dd89a50b74df13d6cf1b8fc
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a>Diagnostische instellingen voor automatisch inschakelen bij het maken van de resource met een Resource Manager-sjabloon
In dit artikel laten we zien hoe u kunt een [Azure Resource Manager-sjabloon](../azure-resource-manager/resource-group-authoring-templates.md) diagnostische instellingen configureren op een bron wanneer deze wordt gemaakt. Hiermee kunt u op automatisch starten streaming uw diagnostische logboeken en metrische gegevens naar Event Hubs in een Opslagaccount wilt archiveren, of ze worden verzonden naar logboekanalyse wanneer een bron wordt gemaakt.

De methode voor het inschakelen van diagnostische logboeken met een Resource Manager-sjabloon is afhankelijk van het brontype.

* **Niet-Compute** resources (bijvoorbeeld Netwerkbeveiligingsgroepen Logic Apps automatisering) gebruiken [diagnostische instellingen die worden beschreven in dit artikel](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).
* **COMPUTE** resources (af/LAD gebaseerde) gebruikt de [af/LAD configuratiebestand beschreven in dit artikel](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).

In dit artikel wordt beschreven hoe diagnostische gegevens met een van de methoden configureren.

De eenvoudige stappen zijn als volgt:

1. Een sjabloon maken als een JSON-bestand dat wordt beschreven hoe u voor het maken van de bron en diagnostische gegevens inschakelen.
2. [De sjabloon met een implementatiemethode implementeert](../azure-resource-manager/resource-group-template-deploy.md).

We bieden hieronder een voorbeeld van het JSON-bestand voor sjabloon die u wilt genereren voor niet-berekenings- en rekenresources.

## <a name="non-compute-resource-template"></a>Niet-Compute resource-sjabloon
Voor niet-rekenresources moet u twee dingen doen:

1. Voeg parameters toe aan de parameters-blob voor de opslagaccountnaam, de event hub autorisatie regel-ID en/of de OMS-logboekanalyse werkruimte-ID (archivering van diagnostische logboeken in een opslagaccount, streamen van logboeken naar Event Hubs en/of Logboeken verzenden naar logboek inschakelen Analytics).
   
    ```json
    "settingName": {
      "type": "string",
      "metadata": {
        "description": "Name of the setting."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "eventHubAuthorizationRuleId": {
      "type": "string",
      "metadata": {
        "description": "Resource ID of the event hub authorization rule for the Event Hubs namespace in which the event hub should be created or streamed to."
      }
    },
    "eventHubName": {
      "type": "string",
      "metadata": {
        "description": "Optional. Name of the event hub within the namespace to which logs are streamed. Without this, an event hub is created for each log category."
      }
    },
    "workspaceId":{
      "type": "string",
      "metadata": {
        "description": "Azure Resource ID of the Log Analytics workspace for the Log Analytics workspace to which logs will be sent."
      }
    }
    ```
2. In de matrix resources van de bron waarvoor u logboeken met diagnostische gegevens inschakelen, voegt u een resource van het type `[resource namespace]/providers/diagnosticSettings`.
   
    ```json
    "resources": [
      {
        "type": "providers/diagnosticSettings",
        "name": "Microsoft.Insights/service",
        "dependsOn": [
          "[/*resource Id for which Diagnostic Logs will be enabled>*/]"
        ],
        "apiVersion": "2017-05-01-preview",
        "properties": {
          "name": "[parameters('settingName')]",
          "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
          "eventHubAuthorizationRuleId": "[parameters('eventHubAuthorizationRuleId')]",
          "eventHubName": "[parameters('eventHubName')]",
          "workspaceId": "[parameters('workspaceId')]",
          "logs": [ 
            {
              "category": "/* log category name */",
              "enabled": true,
              "retentionPolicy": {
                "days": 0,
                "enabled": false
              }
            }
          ],
          "metrics": [
            {
              "category": "AllMetrics",
              "enabled": true,
              "retentionPolicy": {
                "enabled": false,
                "days": 0
              }
            }
          ]
        }
      }
    ]
    ```

De blob eigenschappen voor de diagnostische instelling volgt [de indeling die wordt beschreven in dit artikel](https://docs.microsoft.com/rest/api/monitor/ServiceDiagnosticSettings/CreateOrUpdate). Toevoegen van de `metrics` eigenschap kunt u ook resource metrische gegevens verzenden naar deze dezelfde uitvoer, op voorwaarde dat [Azure Monitor metrische gegevens biedt ondersteuning voor de resource](monitoring-supported-metrics.md).

Hier volgt een voorbeeld van een volledige die een logische App maakt en Hiermee schakelt u streaming met Event Hubs en opslag in een opslagaccount.

```json

{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "logicAppName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Logic App that will be created."
      }
    },
    "testUri": {
      "type": "string",
      "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
    },
    "settingName": {
      "type": "string",
      "metadata": {
        "description": "Name of the setting."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "eventHubAuthorizationRuleId": {
      "type": "string",
      "metadata": {
        "description": "Resource ID of the event hub authorization rule for the Event Hubs namespace in which the event hub should be created or streamed to."
      }
    },
    "eventHubName": {
      "type": "string",
      "metadata": {
        "description": "Optional. Name of the event hub within the namespace to which logs are streamed. Without this, an event hub is created for each log category."
      }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Logic/workflows",
      "name": "[parameters('logicAppName')]",
      "apiVersion": "2016-06-01",
      "location": "[resourceGroup().location]",
      "properties": {
        "definition": {
          "$schema": "http://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "testURI": {
              "type": "string",
              "defaultValue": "[parameters('testUri')]"
            }
          },
          "triggers": {
            "recurrence": {
              "type": "recurrence",
              "recurrence": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          "actions": {
            "http": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('testUri')"
              },
              "runAfter": {}
            }
          },
          "outputs": {}
        },
        "parameters": {}
      },
      "resources": [
        {
          "type": "providers/diagnosticSettings",
          "name": "Microsoft.Insights/service",
          "dependsOn": [
            "[resourceId('Microsoft.Logic/workflows', parameters('logicAppName'))]"
          ],
          "apiVersion": "2017-05-01-preview",
          "properties": {
            "name": "[parameters('settingName')]",
            "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
            "eventHubAuthorizationRuleId": "[parameters('eventHubAuthorizationRuleId')]",
            "eventHubName": "[parameters('eventHubName')]",
            "workspaceId": "[parameters('workspaceId')]",
            "logs": [
              {
                "category": "WorkflowRuntime",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              }
            ],
            "metrics": [
              {
                "timeGrain": "PT1M",
                "enabled": true,
                "retentionPolicy": {
                  "enabled": false,
                  "days": 0
                }
              }
            ]
          }
        }
      ],
      "dependsOn": []
    }
  ]
}

```

## <a name="compute-resource-template"></a>COMPUTE resource-sjabloon
Schakel diagnostische gegevens op een berekeningsresource door bijvoorbeeld een virtuele Machine of Service Fabric-cluster, moet u:

1. De extensie Azure Diagnostics aan de definitie van de VM-resource toevoegen.
2. Geef een opslag-account en/of event hub als parameter.
3. De inhoud van uw WADCfg XML-bestand in de eigenschap XMLCfg, alle XML-tekens juist aanhalingstekens toevoegen.

> [!WARNING]
> Deze laatste stap is lastig direct. [Raadpleeg dit artikel](../virtual-machines/windows/extensions-diagnostics-template.md#diagnostics-configuration-variables) voor een voorbeeld waarin het configuratieschema Diagnostics splitst in variabelen die zijn escape-teken en de juiste indeling.
> 
> 

Het hele proces, inclusief voorbeelden, wordt beschreven [in dit document](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over Azure diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md)
* [Stream Azure logboeken met diagnostische gegevens naar Event Hubs](monitoring-stream-diagnostic-logs-to-event-hubs.md)

