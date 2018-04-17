---
title: Gebruik Azure Resource Manager-sjablonen te maken en configureren van een Log Analytics-werkruimte | Microsoft Docs
description: U kunt Azure Resource Manager-sjablonen maken en configureren van Log Analytics-werkruimten.
services: log-analytics
documentationcenter: ''
author: richrundmsft
manager: jochan
editor: ''
ms.assetid: d21ca1b0-847d-4716-bb30-2a8c02a606aa
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: json
ms.topic: article
ms.date: 04/11/2018
ms.author: richrund
ms.openlocfilehash: e51dab1543c9c5c1c762134b3e73d608bcd523ba
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="manage-log-analytics-using-azure-resource-manager-templates"></a>Log Analytics beheren met Azure Resource Manager-sjablonen
U kunt [Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-authoring-templates.md) maken en configureren van Log Analytics-werkruimten. Voorbeelden van de taken die u met behulp van sjablonen uitvoeren kunt zijn:

* Een werkruimte inclusief prijscategorie instelling maken 
* Een oplossing toevoegen
* Opgeslagen zoekopdrachten maken
* Maak een computergroep
* Verzamelen van IIS-logboeken van computers met de Windows-agent die is geïnstalleerd
* Verzamelen van prestatiemeteritems van Linux- en Windows-computers
* Gebeurtenissen verzamelen van syslog op Linux-computers 
* Verzamelen van gebeurtenissen van Windows-gebeurtenislogboeken
* Log analytics agent toevoegen aan een virtuele machine van Azure
* Log analytics om gegevens te indexeren verzameld met behulp van Azure diagnostics configureren

In dit artikel vindt u voorbeelden van een sjabloon die illustratie van enkele van de configuratie die u met behulp van sjablonen uitvoeren kunt.

## <a name="create-a-log-analytics-workspace"></a>Een Log Analytics-werkruimte maken
Het volgende voorbeeld wordt een werkruimte met een sjabloon van uw lokale computer. Het JSON-sjabloon is geconfigureerd om te vragen u alleen voor de naam van de werkruimte en geeft een standaardwaarde voor de parameters die waarschijnlijk moet worden gebruikt als een standaardconfiguratie in uw omgeving.  

De volgende parameters instellen een standaardwaarde:

* Locatie - standaard ingesteld op VS-Oost
* SKU - standaard ingesteld op de nieuwe Per GB prijscategorie uitgebracht in de 2018 April model prijzen

>[!WARNING]
>Als het maken en configureren van een werkruimte voor logboekanalyse in een abonnement dat in de nieuwe April 2018 prijzen model heeft gekozen, is het de enige geldige logboekanalyse prijscategorie **PerGB2018**. 
>

### <a name="create-and-deploy-template"></a>Maken en implementeren van sjabloon

1. Kopieer en plak de volgende JSON-syntaxis in het bestand:

    ```json
    {
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
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
              "eastus",
              "westus"
            ],
            "defaultValue": "eastus",
            "metadata": {
              "description": "Specifies the location in which to create the workspace."
            }
        },
        "sku": {
            "type": "String",
            "allowedValues": [
              "Standalone",
              "PerNode",
              "PerGB2018"
            ],
            "defaultValue": "PerGB2018",
            "metadata": {
            "description": "Specifies the service tier of the workspace: Standalone, PerNode, Per-GB"
        }
          },
    },
    "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('workspaceName')]",
            "apiVersion": "2017-03-15-preview",
            "location": "[parameters('location')]",
            "properties": {
                "sku": {
                    "Name": "[parameters('sku')]"
                },
                "features": {
                    "searchVersion": 1
                }
            }
          }
       ]
    }
    ```
2. Bewerk de sjabloon om te voldoen aan uw vereisten.  Bekijk [Microsoft.OperationalInsights/workspaces sjabloon](https://docs.microsoft.com/azure/templates/microsoft.operationalinsights/workspaces) verwijzing naar meer informatie over welke eigenschappen en waarden worden ondersteund. 
3. Sla dit bestand als **deploylaworkspacetemplate.json** naar een lokale map.
4. U kunt deze sjabloon nu implementeren. U PowerShell of de opdrachtregel om de werkruimte cretae te gebruiken.

   * Gebruik de volgende opdrachten uit de map met de sjabloon voor PowerShell:
   
        ```powershell
        New-AzureRmResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile deploylaworkspacetemplate.json
        ```

   * Gebruik de volgende opdrachten uit de map met de sjabloon voor de opdrachtregel:

        ```cmd
        azure config mode arm
        azure group deployment create <my-resource-group> <my-deployment-name> --TemplateFile deploylaworkspacetemplate.json
        ```

De implementatie kan enkele minuten duren. Wanneer deze is voltooid, ziet u een bericht dat lijkt op de volgende die het resultaat bevat:<br><br> ![Voorbeeld van resultaat wanneer de implementatie is voltooid](./media/log-analytics-template-workspace-configuration/template-output-01.png)

## <a name="create-and-configure-a-log-analytics-workspace"></a>Maken en configureren van een werkruimte voor logboekanalyse
De volgende sjabloon voorbeeld ziet u hoe:

1. Oplossingen toevoegen aan de werkruimte
2. Opgeslagen zoekopdrachten maken
3. Maak een computergroep
4. Verzamelen van IIS-logboeken van computers met de Windows-agent die is geïnstalleerd
5. Verzamelen van prestatiemeteritems voor logische schijf van Linux-computers (% gebruikte Inodes Beschikbare Megabytes; Percentage gebruikte ruimte; Schijfoverdrachten per seconde; Schijf lezen per seconde; Schijf schrijven per seconde)
6. Syslog-gebeurtenissen verzamelen van Linux-computers
7. Fout- en waarschuwingsberichten gebeurtenissen verzamelen uit het gebeurtenislogboek van toepassing op Windows-computers
8. Beschikbaar geheugen in megabytes-prestatiemeteritem verzamelen van Windows-computers
9. IIS-logboeken en Windows-gebeurtenislogboeken is geschreven door Azure diagnostische gegevens naar een opslagaccount verzamelen

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "workspaceName": {
      "type": "string",
      "metadata": {
        "description": "workspaceName"
      }
    },
    "serviceTier": {
      "type": "string",
      "allowedValues": [
        "Free",
        "Standalone",
        "PerNode",
        "PerGB2018"
      ],
      "metadata": {
        "description": "Service Tier: Free, Standalone, PerNode, or PerGB2018"
    }
      },
    "dataRetention": {
      "type": "int",
      "defaultValue": 30,
      "minValue": 7,
      "maxValue": 730,
      "metadata": {
        "description": "Number of days of retention. Free plans can only have 7 days, Standalone and OMS plans include 30 days for free"
      }
    },
    "location": {
      "type": "string",
      "allowedValues": [
        "East US",
        "West Europe",
        "Southeast Asia",
        "Australia Southeast"
      ]
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
      "apiVersion": "2017-03-15-preview",
      "type": "Microsoft.OperationalInsights/workspaces",
      "name": "[parameters('workspaceName')]",
      "location": "[parameters('location')]",
      "properties": {
        "sku": {
          "Name": "[parameters('serviceTier')]"
        },
    "retentionInDays": "[parameters('dataRetention')]"
      },
      "resources": [
        {
          "apiVersion": "2017-03-15-preview",
          "name": "VMSS Queries2",
          "type": "savedSearches",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "Category": "VMSS",
            "ETag": "*",
            "DisplayName": "VMSS Instance Count",
            "Query": "Event | where Source == "ServiceFabricNodeBootstrapAgent" | summarize AggregatedValue = count() by Computer",
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
          "apiVersion": "2015-11-01-preview",
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
2. De sjabloon om de gewenste configuratie bewerken
3. Gebruik PowerShell of de opdrachtregel om de sjabloon te implementeren

#### <a name="powershell"></a>PowerShell
```powershell
New-AzureRmResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile azuredeploy.json
```

#### <a name="command-line"></a>Opdrachtregel
```cmd
azure config mode arm
azure group deployment create <my-resource-group> <my-deployment-name> --TemplateFile azuredeploy.json
```

## <a name="example-resource-manager-templates"></a>Voorbeeld van de Resource Manager-sjablonen
De Azure quickstart-sjablonengalerie bevat verschillende sjablonen voor logboekanalyse, met inbegrip van:

* [Een virtuele machine met Windows met de extensie Log Analytics VM implementeren](https://azure.microsoft.com/documentation/templates/201-oms-extension-windows-vm/)
* [Een virtuele machine waarop Linux wordt uitgevoerd met de extensie Log Analytics VM implementeren](https://azure.microsoft.com/documentation/templates/201-oms-extension-ubuntu-vm/)
* [Azure Site Recovery met behulp van een bestaande werkruimte voor logboekanalyse bewaken](https://azure.microsoft.com/documentation/templates/asr-oms-monitoring/)
* [Bewaak Web-Apps van Azure met behulp van een bestaande werkruimte voor logboekanalyse](https://azure.microsoft.com/documentation/templates/101-webappazure-oms-monitoring/)
* [Controleprogramma SQL Azure met behulp van een bestaande werkruimte voor logboekanalyse](https://azure.microsoft.com/documentation/templates/101-sqlazure-oms-monitoring/)
* [Een Service Fabric-cluster implementeren en controleren van het aan een bestaande werkruimte voor logboekanalyse](https://azure.microsoft.com/documentation/templates/service-fabric-oms/)
* [Een Service Fabric-cluster implementeren en het maken van een werkruimte voor logboekanalyse voor het bewaken van deze](https://azure.microsoft.com/documentation/templates/service-fabric-vmss-oms/)

## <a name="next-steps"></a>Volgende stappen
* [Agents te implementeren in Azure VM's met behulp van Resource Manager-sjablonen](log-analytics-azure-vm-extension.md)

