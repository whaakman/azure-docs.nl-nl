---
title: Implementeren en bijwerken van toepassingen en services met Azure Resource Manager | Microsoft Docs
description: Informatie over het implementeren van toepassingen en services op een Service Fabric-cluster met een Azure Resource Manager-sjabloon.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/06/2017
ms.author: dekapur
ms.openlocfilehash: ca11199e51774e766113309150d8a260427cb4b4
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="manage-applications-and-services-as-azure-resource-manager-resources"></a>Toepassingen en services als Azure Resource Manager-resources beheren

U kunt toepassingen en services implementeren op uw Service Fabric-cluster via Azure Resource Manager. Dit betekent dat in plaats van toepassingen implementeren en beheren via PowerShell of CLI nadat ik wachttijd voor het cluster gereed is, u kunt nu express toepassingen en services in JSON en in de dezelfde Resource Manager-sjabloon als uw cluster implementeren. Het proces van registratie van toepassing, inrichting en implementatie van alle gebeurt in één stap.

Dit is de aanbevolen manier om eventuele setup, governance of cluster management toepassingen die u nodig in uw cluster hebt implementeren. Dit omvat de [Patch Orchestration toepassing](service-fabric-patch-orchestration-application.md), Watchdogs en alle toepassingen die moeten worden uitgevoerd in uw cluster voordat andere toepassingen of services worden geïmplementeerd. 

Indien van toepassing, moet u uw toepassingen beheren als Resource Manager-resources te verbeteren:
* Audittrail: Resource Manager controles van elke bewerking en houdt een gedetailleerde *activiteitenlogboek* die kunnen helpen u traceren van wijzigingen in deze toepassingen en het cluster.
* Op rollen gebaseerde toegangsbeheer (RBAC): kan het beheren van toegang tot clusters, evenals de toepassingen die zijn geïmplementeerd op het cluster worden uitgevoerd via dezelfde Resource Manager-sjabloon.
* Azure Resource Manager (via Azure portal) wordt een one-stop-shop voor het beheren van uw cluster en implementaties van essentiële toepassingen.

Het volgende fragment toont de verschillende soorten resources die kunnen worden beheerd via een sjabloon:

```json
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes/versions",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'), '/', parameters('applicationTypeVersion'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applications",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applications/services",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'), '/', parameters('serviceName'))]",
    "location": "[variables('clusterLocation')]"
}
```


## <a name="add-a-new-application-to-your-resource-manager-template"></a>Een nieuwe toepassing toevoegen aan uw Resource Manager-sjabloon

1. Bereid uw cluster Resource Manager-sjabloon voor implementatie. Zie [Service Fabric-cluster maken met behulp van Azure Resource Manager](service-fabric-cluster-creation-via-arm.md) voor meer informatie hierover.
2. Denk na over enkele van de toepassingen die u van plan bent in het cluster implementeert. Zijn er die wordt altijd uitgevoerd die andere toepassingen afhankelijkheden kunnen ondernemen? Bent u van plan over het implementeren van een cluster governance of installatieprogramma's? Dit soort toepassingen zijn beste beheerd via een Resource Manager-sjabloon, zoals hierboven wordt beschreven. 
3. Nadat u hebt gekeken hoe welke toepassingen u wilt worden geïmplementeerd op deze manier, wordt de toepassingen hebt worden verpakt, ingepakt en plaatsen op een bestandsshare. De share moet toegankelijk zijn via een REST-eindpunt voor Azure Resource Manager gebruiken tijdens de implementatie.
4. Beschrijving van de toepassing eigenschappen in het Resource Manager-sjabloon hieronder uw cluster-declaratie. Deze eigenschappen omvatten replica of instance count en eventuele ketens afhankelijkheid tussen resources (andere toepassingen of services). Zie voor een lijst van uitgebreide eigenschappen, de [REST API Swagger Spec](https://aka.ms/sfrpswaggerspec). Houd er rekening mee dat dit is geen vervanging voor de toepassing of Service-manifesten, maar in plaats daarvan wordt een overzicht gegeven van wat er op als onderdeel van het cluster Resource Manager-sjabloon. Hier volgt een voorbeeldsjabloon met een stateless service implementeren *Service1* en een stateful service *Service2* als onderdeel van *Toepassing1*:

  ```json
  {
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "clusterName": {
        "type": "string",
        "defaultValue": "Cluster",
        "metadata": {
          "description": "Name of your cluster - Between 3 and 23 characters. Letters and numbers only."
        }
      },
      "applicationTypeName": {
        "type": "string",
        "defaultValue": "ApplicationType",
        "metadata": {
          "description": "The application type name."
        }
      },
      "applicationTypeVersion": {
        "type": "string",
        "defaultValue": "1",
        "metadata": {
          "description": "The application type version."
        }
      },
      "appPackageUrl": {
        "type": "string",
        "metadata": {
          "description": "The URL to the application package sfpkg file."
        }
      },
      "applicationName": {
        "type": "string",
        "defaultValue": "Application1",
        "metadata": {
          "description": "The name of the application resource."
        }
      },
      "serviceName": {
        "type": "string",
        "defaultValue": "Service1",
        "metadata": {
          "description": "The name of the service resource in the format of {applicationName}~{serviceName}."
        }
      },
      "serviceTypeName": {
        "type": "string",
        "defaultValue": "Service1Type",
        "metadata": {
          "description": "The name of the service type."
        }
      },
      "serviceName2": {
        "type": "string",
        "defaultValue": "Service2",
        "metadata": {
          "description": "The name of the service resource in the format of {applicationName}~{serviceName}."
        }
      },
      "serviceTypeName2": {
        "type": "string",
        "defaultValue": "Service2Type",
        "metadata": {
          "description": "The name of the service type."
        }
      }
    },
    "variables": {
      "clusterLocation": "[resourcegroup().location]"
    },
    "resources": [
      {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters/applicationTypes",
        "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'))]",
        "location": "[variables('clusterLocation')]",
        "dependsOn": [],
        "properties": {
          "provisioningState": "Default"
        }
      },
      {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters/applicationTypes/versions",
        "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'), '/', parameters('applicationTypeVersion'))]",
        "location": "[variables('clusterLocation')]",
        "dependsOn": [
          "[concat('Microsoft.ServiceFabric/clusters/', parameters('clusterName'), '/applicationTypes/', parameters('applicationTypeName'))]"
        ],
        "properties": {
          "provisioningState": "Default",
          "appPackageUrl": "[parameters('appPackageUrl')]"
        }
      },
      {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters/applications",
        "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
        "location": "[variables('clusterLocation')]",
        "dependsOn": [
          "[concat('Microsoft.ServiceFabric/clusters/', parameters('clusterName'), '/applicationTypes/', parameters('applicationTypeName'), '/versions/', parameters('applicationTypeVersion'))]"
        ],
        "properties": {
          "provisioningState": "Default",
          "typeName": "[parameters('applicationTypeName')]",
          "typeVersion": "[parameters('applicationTypeVersion')]",
          "parameters": {},
          "upgradePolicy": {
            "upgradeReplicaSetCheckTimeout": "01:00:00.0",
            "forceRestart": "false",
            "rollingUpgradeMonitoringPolicy": {
              "healthCheckWaitDuration": "00:02:00.0",
              "healthCheckStableDuration": "00:05:00.0",
              "healthCheckRetryTimeout": "00:10:00.0",
              "upgradeTimeout": "01:00:00.0",
              "upgradeDomainTimeout": "00:20:00.0"
            },
            "applicationHealthPolicy": {
              "considerWarningAsError": "false",
              "maxPercentUnhealthyDeployedApplications": "50",
              "defaultServiceTypeHealthPolicy": {
                "maxPercentUnhealthyServices": "50",
                "maxPercentUnhealthyPartitionsPerService": "50",
                "maxPercentUnhealthyReplicasPerPartition": "50"
              }
            }
          }
        }
      },
      {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters/applications/services",
        "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'), '/', parameters('serviceName'))]",
        "location": "[variables('clusterLocation')]",
        "dependsOn": [
          "[concat('Microsoft.ServiceFabric/clusters/', parameters('clusterName'), '/applications/', parameters('applicationName'))]"
        ],
        "properties": {
          "provisioningState": "Default",
          "serviceKind": "Stateless",
          "serviceTypeName": "[parameters('serviceTypeName')]",
          "instanceCount": "-1",
          "partitionDescription": {
            "partitionScheme": "Singleton"
          },
          "correlationScheme": [],
          "serviceLoadMetrics": [],
          "servicePlacementPolicies": []
        }
      },
      {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters/applications/services",
        "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'), '/', parameters('serviceName2'))]",
        "location": "[variables('clusterLocation')]",
        "dependsOn": [
          "[concat('Microsoft.ServiceFabric/clusters/', parameters('clusterName'), '/applications/', parameters('applicationName'))]"
        ],
        "properties": {
          "provisioningState": "Default",
          "serviceKind": "Stateful",
          "serviceTypeName": "[parameters('serviceTypeName2')]",
          "targetReplicaSetSize": "3",
          "minReplicaSetSize": "2",
          "replicaRestartWaitDuration": "00:01:00.0",
          "quorumLossWaitDuration": "00:02:00.0",
          "standByReplicaKeepDuration": "00:00:30.0",
          "partitionDescription": {
            "partitionScheme": "UniformInt64Range",
            "count": "5",
            "lowKey": "1",
            "highKey": "5"
          },
          "hasPersistedState": "true",
          "correlationScheme": [],
          "serviceLoadMetrics": [],
          "servicePlacementPolicies": [],
          "defaultMoveCost": "Low"
        }
      }
    ]
  }
  ```

  > [!NOTE] 
  > De *apiVersion* moet worden ingesteld op `"2017-07-01-preview"`. Deze sjabloon kan ook onafhankelijk van het cluster worden geïmplementeerd, zolang het cluster al is geïmplementeerd.

5. Implementeer! 

## <a name="manage-an-existing-application-via-resource-manager"></a>Een bestaande toepassing via Resource Manager beheren

Als uw cluster al actief is en een aantal toepassingen dat u beheren als Resource Manager wilt, resources al zijn geïmplementeerd, in plaats van de toepassingen verwijderen en opnieuw te implementeren op deze, kunt u een PUT-aanroep met de dezelfde API's om de toepassingen ophalen bevestigd als Resource Manager-resources. 


## <a name="next-steps"></a>Volgende stappen

* Gebruik de [Service Fabric CLI](service-fabric-cli.md) of [PowerShell](service-fabric-deploy-remove-applications.md) voor het implementeren van andere toepassingen in uw cluster. 
* [Upgrade van uw Service Fabric-cluster](service-fabric-cluster-upgrade.md)

