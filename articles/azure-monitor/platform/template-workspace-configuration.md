---
title: Gebruik Azure Resource Manager-sjablonen te maken en configureren van een Log Analytics-werkruimte | Microsoft Docs
description: U kunt Azure Resource Manager-sjablonen maken en configureren van Log Analytics-werkruimten.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: d21ca1b0-847d-4716-bb30-2a8c02a606aa
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 07/11/2019
ms.author: magoedte
ms.openlocfilehash: a55a4b2f3045aac8dfe9e46a50074585ab3ef491
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827797"
---
# <a name="manage-log-analytics-workspace-using-azure-resource-manager-templates"></a>Log Analytics-werkruimte met behulp van Azure Resource Manager-sjablonen beheren

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

U kunt [Azure Resource Manager-sjablonen](../../azure-resource-manager/resource-group-authoring-templates.md) maken en configureren van Log Analytics-werkruimten in Azure Monitor. Voorbeelden van de taken die u met sjablonen uitvoeren kunt zijn:

* Een werkruimte met inbegrip van de prijscategorie instelling maken 
* Een oplossing toevoegen
* Opgeslagen zoekopdrachten maken
* Een computergroep maken
* Verzamelen van IIS-logboeken van computers met de Windows-agent is geïnstalleerd
* Verzamelen van prestatiemeteritems van Linux en Windows-computers
* Gebeurtenissen verzamelen van syslog op Linux-computers 
* Gebeurtenissen verzamelen van Windows-gebeurtenislogboeken
* De log analytics-agent toevoegen aan een virtuele machine van Azure
* Log analytics om gegevens te indexeren die zijn verzameld met behulp van Azure diagnostics configureren

In dit artikel bevat voorbeelden van een sjabloon die laten zien van bepaalde onderdelen van de configuratie die u met sjablonen kunt uitvoeren.

## <a name="api-versions"></a>API-versies

De volgende tabel bevat de API-versie voor de resources die in dit voorbeeld worden gebruikt.

| Resource | Resourcetype | API-versie |
|:---|:---|:---|
| Werkruimte   | workspaces    | 2017-03-15-preview |
| Search      | savedSearches | 2015-03-20 |
| Gegevensbron | gegevensbronnen   | 2015-11-01-preview |
| Oplossing    | oplossingen     | 2015-11-01-preview |

## <a name="create-a-log-analytics-workspace"></a>Een Log Analytics-werkruimte maken

Het volgende voorbeeld wordt een werkruimte met behulp van een sjabloon van uw lokale computer. De JSON-sjabloon is geconfigureerd om alleen de naam en locatie van de nieuwe werkruimte (met behulp van de standaardwaarden voor de andere werkruimte parameters zoals prijscategorie en retentie).  

### <a name="create-and-deploy-template"></a>Sjabloon maken en implementeren

1. Kopieer en plak de volgende JSON-syntaxis in het bestand:

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "type": "String",
            "metadata": {
              "description": "Specifies the name of the workspace."
            }
        },
        "location": {
            "type": "String",
            "allowedValues": [
              "australiacentral", 
              "australiaeast", 
              "australiasoutheast", 
              "brazilsouth",
              "canadacentral", 
              "centralindia", 
              "centralus", 
              "eastasia", 
              "eastus", 
              "eastus2", 
              "francecentral", 
              "japaneast", 
              "koreacentral", 
              "northcentralus", 
              "northeurope", 
              "southafricanorth", 
              "southcentralus", 
              "southeastasia", 
              "uksouth", 
              "ukwest", 
              "westcentralus", 
              "westeurope", 
              "westus", 
              "westus2" 
            ],
            "metadata": {
              "description": "Specifies the location in which to create the workspace."
            }
        }
    },
    "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('workspaceName')]",
            "apiVersion": "2015-11-01-preview",
            "location": "[parameters('location')]",
            "properties": {
                "features": {
                    "searchVersion": 1
                }
            }
          }
       ]
    }
    ```

2. De sjabloon bijwerken om aan uw eisen voldoen. Beoordeling [Microsoft.OperationalInsights/workspaces sjabloon](https://docs.microsoft.com/azure/templates/microsoft.operationalinsights/workspaces) verwijzing voor meer informatie over welke eigenschappen en waarden worden ondersteund. 
3. Sla dit bestand als **deploylaworkspacetemplate.json** naar een lokale map.
4. U kunt deze sjabloon nu implementeren. U PowerShell of de opdrachtregel gebruiken voor het maken van de werkruimte, de naam van de werkruimte en de locatie op te geven als onderdeel van de opdracht.

   * Gebruik de volgende opdrachten uit de map met de sjabloon voor PowerShell:
   
        ```powershell
        New-AzResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile deploylaworkspacetemplate.json -workspaceName <workspace-name> -location <location>
        ```

   * Gebruik de volgende opdrachten uit de map met de sjabloon voor de opdrachtregel:

        ```cmd
        azure config mode arm
        azure group deployment create <my-resource-group> <my-deployment-name> --TemplateFile deploylaworkspacetemplate.json --workspaceName <workspace-name> --location <location>
        ```

De implementatie kan enkele minuten duren. Als deze is voltooid, ziet u een bericht dat lijkt op de volgende mogelijkheden van het resultaat:<br><br> ![Voorbeeld van resultaat wanneer de implementatie is voltooid](./media/template-workspace-configuration/template-output-01.png)

## <a name="configure-a-log-analytics-workspace"></a>Een Log Analytics-werkruimte configureren

De volgende sjabloon voorbeeld ziet u hoe u:

1. Oplossingen toevoegen aan de werkruimte
2. Opgeslagen zoekopdrachten maken
3. Een computergroep maken
4. Verzamelen van IIS-logboeken van computers met de Windows-agent is geïnstalleerd
5. Verzamelen van prestatiemeteritems voor logische schijf van Linux-computers (% gebruikte Inodes Beschikbare Megabytes; Percentage gebruikte ruimte; Schijfoverdrachten per seconde; Schijf lezen per seconde; Schijf schrijven per seconde)
6. Syslog-gebeurtenissen verzamelen van Linux-computers
7. Fout- en waarschuwingsberichten gebeurtenissen verzamelen van het logboek voor toepassingsgebeurtenissen van Windows-computers
8. Beschikbaar geheugen in megabytes-prestatiemeteritem verzamelen van Windows-computers
9. Verzamelen van IIS-logboeken en Windows-gebeurtenislogboeken die is geschreven door Azure diagnostics naar een opslagaccount

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "workspaceName": {
      "type": "string",
      "metadata": {
        "description": "Workspace name"
      }
    },
    "pricingTier": {
      "type": "string",
      "allowedValues": [
        "PerGB2018",
        "Free",
        "Standalone",
        "PerNode",
        "Standard",
        "Premium"
      ],
      "defaultValue": "PerGB2018",
      "metadata": {
        "description": "Pricing tier: PerGB2018 or legacy tiers (Free, Standalone, PerNode, Standard or Premium) which are not available to all customers."
      }
    },
    "dataRetention": {
      "type": "int",
      "defaultValue": 30,
      "minValue": 7,
      "maxValue": 730,
      "metadata": {
        "description": "Number of days of retention. Workspaces in the legacy Free pricing tier can only have 7 days."
      }
    },
    {
    "immediatePurgeDataOn30Days": {
      "type": "bool",
      "metadata": {
        "description": "If set to true when changing retention to 30 days, older data will be immediately deleted. Use this with extreme caution. This only applies when retention is being set to 30 days."
      }
    },
    "location": {
      "type": "string",
      "allowedValues": [
        "australiacentral", 
        "australiaeast", 
        "australiasoutheast", 
        "brazilsouth",
        "canadacentral", 
        "centralindia", 
        "centralus", 
        "eastasia", 
        "eastus", 
        "eastus2", 
        "francecentral", 
        "japaneast", 
        "koreacentral", 
        "northcentralus", 
        "northeurope", 
        "southafricanorth", 
        "southcentralus", 
        "southeastasia", 
        "uksouth", 
        "ukwest", 
        "westcentralus", 
        "westeurope", 
        "westus", 
        "westus2"
      ],
      "metadata": {
        "description": "Specifies the location in which to create the workspace."
      }
    },
    "applicationDiagnosticsStorageAccountName": {
        "type": "string",
        "metadata": {
          "description": "Name of the storage account with Azure diagnostics output"
        }
    },
    "applicationDiagnosticsStorageAccountResourceGroup": {
        "type": "string",
        "metadata": {
          "description": "The resource group name containing the storage account with Azure diagnostics output"
        }
    }
  },
  "variables": {
    "Updates": {
      "Name": "[Concat('Updates', '(', parameters('workspaceName'), ')')]",
      "GalleryName": "Updates"
    },
    "AntiMalware": {
      "Name": "[concat('AntiMalware', '(', parameters('workspaceName'), ')')]",
      "GalleryName": "AntiMalware"
    },
    "SQLAssessment": {
      "Name": "[Concat('SQLAssessment', '(', parameters('workspaceName'), ')')]",
      "GalleryName": "SQLAssessment"
    },
    "diagnosticsStorageAccount": "[resourceId(parameters('applicationDiagnosticsStorageAccountResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName'))]"
  },
  "resources": [
    {
      "apiVersion": "2015-11-01-preview",
      "type": "Microsoft.OperationalInsights/workspaces",
      "name": "[parameters('workspaceName')]",
      "location": "[parameters('location')]",
      "properties": {
        "sku": {
          "name": "[parameters('pricingTier')]"
          "features": {
            "immediatePurgeDataOn30Days": "[parameters('immediatePurgeDataOn30Days')]"
          }
        },
    "retentionInDays": "[parameters('dataRetention')]"
      },
      "resources": [
        {
          "apiVersion": "2015-03-20",
          "name": "VMSS Queries2",
          "type": "savedSearches",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "Category": "VMSS",
            "ETag": "*",
            "DisplayName": "VMSS Instance Count",
            "Query": "Event | where Source == \"ServiceFabricNodeBootstrapAgent\" | summarize AggregatedValue = count() by Computer",
            "Version": 1
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleWindowsEvent1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "WindowsEvent",
          "properties": {
            "eventLogName": "Application",
            "eventTypes": [
              {
                "eventType": "Error"
              },
              {
                "eventType": "Warning"
              }
            ]
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleWindowsPerfCounter1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "WindowsPerformanceCounter",
          "properties": {
            "objectName": "Memory",
            "instanceName": "*",
            "intervalSeconds": 10,
            "counterName": "Available MBytes"
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleIISLog1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "IISLogs",
          "properties": {
            "state": "OnPremiseEnabled"
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleSyslog1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "LinuxSyslog",
          "properties": {
            "syslogName": "kern",
            "syslogSeverities": [
              {
                "severity": "emerg"
              },
              {
                "severity": "alert"
              },
              {
                "severity": "crit"
              },
              {
                "severity": "err"
              },
              {
                "severity": "warning"
              }
            ]
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleSyslogCollection1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "LinuxSyslogCollection",
          "properties": {
            "state": "Enabled"
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleLinuxPerf1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "LinuxPerformanceObject",
          "properties": {
            "performanceCounters": [
              {
                "counterName": "% Used Inodes"
              },
              {
                "counterName": "Free Megabytes"
              },
              {
                "counterName": "% Used Space"
              },
              {
                "counterName": "Disk Transfers/sec"
              },
              {
                "counterName": "Disk Reads/sec"
              },
              {
                "counterName": "Disk Writes/sec"
              }
            ],
            "objectName": "Logical Disk",
            "instanceName": "*",
            "intervalSeconds": 10
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleLinuxPerfCollection1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "LinuxPerformanceCollection",
          "properties": {
            "state": "Enabled"
          }
        },
        {
          "apiVersion": "2015-03-20",
          "name": "[concat(parameters('applicationDiagnosticsStorageAccountName'),parameters('workspaceName'))]",
          "type": "storageinsightconfigs",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "containers": [ 
              "wad-iis-logfiles" 
            ],
            "tables": [
              "WADWindowsEventLogsTable"
            ],
            "storageAccount": {
              "id": "[variables('diagnosticsStorageAccount')]",
              "key": "[listKeys(variables('diagnosticsStorageAccount'),'2015-06-15').key1]"
            }
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "location": "[parameters('location')]",
          "name": "[variables('Updates').Name]",
          "type": "Microsoft.OperationsManagement/solutions",
          "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationsManagement/solutions/', variables('Updates').Name)]",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          },
          "plan": {
            "name": "[variables('Updates').Name]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('Updates').GalleryName)]",
            "promotionCode": ""
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "location": "[parameters('location')]",
          "name": "[variables('AntiMalware').Name]",
          "type": "Microsoft.OperationsManagement/solutions",
          "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationsManagement/solutions/', variables('AntiMalware').Name)]",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          },
          "plan": {
            "name": "[variables('AntiMalware').Name]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('AntiMalware').GalleryName)]",
            "promotionCode": ""
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "location": "[parameters('location')]",
          "name": "[variables('SQLAssessment').Name]",
          "type": "Microsoft.OperationsManagement/solutions",
          "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationsManagement/solutions/', variables('SQLAssessment').Name)]",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          },
          "plan": {
            "name": "[variables('SQLAssessment').Name]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('SQLAssessment').GalleryName)]",
            "promotionCode": ""
          }
        }
      ]
    }
  ],
  "outputs": {
    "workspaceName": {
      "type": "string",
      "value": "[parameters('workspaceName')]"
    },
    "provisioningState": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').provisioningState]"
    },
    "source": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').source]"
    },
    "customerId": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').customerId]"
    },
    "pricingTier": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').sku.name]"
    },
    "retentionInDays": {
      "type": "int",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').retentionInDays]"
    },
    "immediatePurgeDataOn30Days": {  
      "type": "bool",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').features.immediatePurgeDataOn30Days]"
    },
    "portalUrl": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').portalUrl]"
    }
  }
}

```
### <a name="deploying-the-sample-template"></a>De voorbeeldsjabloon implementeren

De voorbeeldsjabloon implementeren:

1. De gekoppelde steekproef bijvoorbeeld in een bestand opslaan `azuredeploy.json` 
2. De sjabloon bijwerken om de gewenste configuratie
3. PowerShell of de opdrachtregel gebruiken om de sjabloon te implementeren

#### <a name="powershell"></a>PowerShell

```powershell
New-AzResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile azuredeploy.json
```

#### <a name="command-line"></a>Opdrachtregel

```cmd
azure config mode arm
azure group deployment create <my-resource-group> <my-deployment-name> --TemplateFile azuredeploy.json
```

## <a name="example-resource-manager-templates"></a>Voorbeeld van de Resource Manager-sjablonen

De galerie van Azure-quickstart-sjabloon bevat verschillende sjablonen voor Log Analytics, met inbegrip van:

* [Implementeren van een virtuele machine met Windows met de Log Analytics VM-extensie](https://azure.microsoft.com/documentation/templates/201-oms-extension-windows-vm/)
* [Implementeer een virtuele machine waarop Linux wordt uitgevoerd met de Log Analytics VM-extensie](https://azure.microsoft.com/documentation/templates/201-oms-extension-ubuntu-vm/)
* [Monitor voor Azure Site Recovery met behulp van een bestaande Log Analytics-werkruimte](https://azure.microsoft.com/documentation/templates/asr-oms-monitoring/)
* [Monitor voor Azure Web Apps met behulp van een bestaande Log Analytics-werkruimte](https://azure.microsoft.com/documentation/templates/101-webappazure-oms-monitoring/)
* [Een bestaand opslagaccount toevoegen aan Log Analytics](https://azure.microsoft.com/resources/templates/oms-existing-storage-account/)

## <a name="next-steps"></a>Volgende stappen

* [Windows-agent implementeren op virtuele Azure-machines met behulp van Resource Manager-sjabloon](../../virtual-machines/extensions/oms-windows.md).

* [Linux-agent implementeren op virtuele Azure-machines met behulp van Resource Manager-sjabloon](../../virtual-machines/extensions/oms-linux.md).