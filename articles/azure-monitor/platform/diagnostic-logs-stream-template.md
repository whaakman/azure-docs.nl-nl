---
title: Automatisch diagnostische instellingen inschakelen met Resource Manager-sjabloon
description: Informatie over het gebruik van Resource Manager-sjabloon om diagnostische instellingen waarmee u kunt de diagnostische logboeken naar Event Hubs streamen of sla ze op een storage-account te maken.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 3/26/2018
ms.author: johnkem
ms.component: ''
ms.openlocfilehash: 1d199275d6c17dff72c020bd3a9807f50af8af1e
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53544036"
---
# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a>Automatisch diagnostische instellingen inschakelen bij het maken van resources met behulp van Resource Manager-sjabloon
In dit artikel laten we zien hoe u kunt een [Azure Resource Manager-sjabloon](../../azure-resource-manager/resource-group-authoring-templates.md) diagnostische instellingen configureren op een resource als deze wordt gemaakt. Hiermee kunt u automatisch wilt streamen uw logboeken met diagnostische gegevens en metrische gegevens naar Event Hubs, archiveren in een Opslagaccount of ze naar Log Analytics verzenden wanneer een resource wordt gemaakt.

> [!WARNING]
> De indeling van de logboekgegevens in het opslagaccount wordt op 1 november 2018 gewijzigd in JSON Lines. [Raadpleeg dit artikel voor een beschrijving van de gevolgen en hoe u uw tooling kunt bijwerken om de nieuwe indeling te verwerken. ](./../../azure-monitor/platform/diagnostic-logs-append-blobs.md) 
>
> 

De methode voor het inschakelen van diagnostische logboeken met behulp van Resource Manager-sjabloon, is afhankelijk van het resourcetype.

* **Niet-Compute** resources (bijvoorbeeld, Network Security Groups, Logic Apps, automatisering) gebruiken [diagnostische instellingen die worden beschreven in dit artikel](../../azure-monitor/platform/diagnostic-logs-overview.md#diagnostic-settings).
* **COMPUTE** resources (WAD/LAD-indeling) gebruikt de [WAD/LAD-configuratiebestand in dit artikel beschreven](/visualstudio/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines).

In dit artikel wordt beschreven hoe u met behulp van een van beide methoden diagnosefuncties te configureren.

De eenvoudige stappen zijn als volgt:

1. Een sjabloon maken als een JSON-bestand dat wordt beschreven hoe u het maken van de resource en diagnostische gegevens inschakelen.
2. [Implementeren van de sjabloon met een implementatiemethode](../../azure-resource-manager/resource-group-template-deploy.md).

We geven hieronder een voorbeeld van het JSON-bestand van het sjabloon u moet voor het genereren van niet-gebruikte reken-en Compute-resources.

## <a name="non-compute-resource-template"></a>Niet-Compute-resource-sjabloon
Voor niet-Compute-resources moet u twee dingen doen:

1. Parameters toevoegen aan de parameters-blob voor de storage-accountnaam, de event hub autorisatie regel-ID en/of de Log Analytics-werkruimte-ID (waardoor archivering van logboeken met diagnostische gegevens in een opslagaccount, streamen van logboeken naar Event Hubs en/of Logboeken verzenden naar Log Analytics).
   
    ```json
    "settingName": {
      "type": "string",
      "metadata": {
        "description": "Name for the diagnostic setting resource. Eg. 'archiveToStorage' or 'forSecurityTeam'."
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
2. In de matrix van de resources van de resource waarvoor u wenst te logboeken met diagnostische gegevens inschakelen, voegt u een resource van het type `[resource namespace]/providers/diagnosticSettings`.
   
    ```json
    "resources": [
      {
        "type": "providers/diagnosticSettings",
        "name": "[concat('Microsoft.Insights/', parameters('settingName'))]",
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

De eigenschappen van de blob voor de diagnostische instelling volgt [de indeling die wordt beschreven in dit artikel](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings/createorupdate). Toevoegen van de `metrics` eigenschap kunt u ook metrische gegevens voor resources verzenden naar deze dezelfde uitvoer, op voorwaarde dat [metrische gegevens van Azure Monitor biedt ondersteuning voor de resource](../../azure-monitor/platform/metrics-supported.md).

Hier volgt een volledig voorbeeld waarin een logische App maakt en schakelt streamen met Event Hubs en opslag in een storage-account.

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
      "defaultValue": "http://azure.microsoft.com/status/feed/"
    },
    "settingName": {
      "type": "string",
      "metadata": {
        "description": "Name of the setting. Name for the diagnostic setting resource. Eg. 'archiveToStorage' or 'forSecurityTeam'."
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
          "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
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
          "name": "[concat('Microsoft.Insights/', parameters('settingName'))]",
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
Om in te schakelen diagnostische gegevens op een Compute-resource, bijvoorbeeld een virtuele Machine of Service Fabric-cluster, moet u naar:

1. De Azure Diagnostics-extensie toevoegen aan de definitie van de VM-resource.
2. Geef een storage-account en/of event hub als parameter.
3. De inhoud van uw WADCfg XML-bestand in de eigenschap XMLCfg, alle XML-tekens correct escapereeksen toevoegen.

> [!WARNING]
> Deze laatste stap is lastig meteen. [Raadpleeg dit artikel](../../virtual-machines/extensions/diagnostics-template.md#diagnostics-configuration-variables) voor een voorbeeld waarin het configuratieschema van Diagnostics splitst in variabelen die worden voorafgegaan en juist opgemaakt.
> 
> 

Het hele proces, inclusief voorbeelden wordt beschreven [in dit document](../../virtual-machines/extensions/diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over Azure-logboeken met diagnostische gegevens](../../azure-monitor/platform/diagnostic-logs-overview.md)
* [Diagnostische logboeken naar Eventhubs in Azure Stream](../../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md)

