---
title: Implementeren en upgraden van toepassingen en services met Azure Resource Manager | Microsoft Docs
description: Informatie over het implementeren van toepassingen en services in een Service Fabric-cluster met behulp van een Azure Resource Manager-sjabloon.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/06/2017
ms.author: dekapur
ms.openlocfilehash: 7aa7fc2620fa02af4a720a97eece3c0734252245
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42057204"
---
# <a name="manage-applications-and-services-as-azure-resource-manager-resources"></a>Toepassingen en services als Azure Resource Manager-resources beheren

U kunt toepassingen en services implementeren naar uw Service Fabric-cluster via Azure Resource Manager. Dit betekent dat in plaats van implementeren en beheren van toepassingen via PowerShell of CLI nadat er na afloop van het cluster gereed is, kunt u nu toepassingen en services in JSON express en deze implementeren in de dezelfde Resource Manager-sjabloon als uw cluster. Het proces van registratie van toepassing, inrichting en implementatie alle wordt uitgevoerd in één stap.

Dit is de aanbevolen manier om een installatie, governance of Clusterbeheertoepassingen die u nodig in uw cluster hebt implementeren. Dit omvat de [Patch Orchestration toepassing](service-fabric-patch-orchestration-application.md), Watchdogs of toepassingen die moeten worden uitgevoerd in uw cluster voordat u andere toepassingen of services worden geïmplementeerd. 

Indien van toepassing, beheren van uw toepassingen als Resource Manager-resources te verbeteren:
* Audittrail: Resource Manager voert een controle uit elke bewerking en houdt een gedetailleerde *activiteitenlogboek* die kunt u alle wijzigingen aan deze toepassingen en het cluster te traceren.
* Op rollen gebaseerd toegangsbeheer (RBAC): kan het beheer van toegang tot clusters, evenals de toepassingen die zijn geïmplementeerd op het cluster worden uitgevoerd via dezelfde Resource Manager-sjabloon.
* Azure Resource Manager (via Azure portal), wordt een one-stop-shop voor het beheren van uw cluster en implementaties van essentiële toepassingen.

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

1. Voorbereiden van uw cluster Resource Manager-sjabloon voor implementatie. Zie [maken van een Service Fabric-cluster met behulp van Azure Resource Manager](service-fabric-cluster-creation-via-arm.md) voor meer informatie hierover.
2. Denk na over enkele van de toepassingen die u van plan bent over het implementeren van in het cluster. Zijn er die zal altijd worden uitgevoerd die andere toepassingen kan duren voordat er afhankelijkheden op? Bent u van plan zijn over het implementeren van een cluster governance of van installatietoepassingen? Deze soorten toepassingen zijn best beheerd via Resource Manager-sjabloon, zoals hierboven wordt beschreven. 
3. Wanneer u welke toepassingen u wilt worden geïmplementeerd op deze manier hebt begrepen, de toepassingen moeten worden verpakt, ingepakt en plaatsen op een bestandsshare. De share moet toegankelijk zijn via een REST-eindpunt voor Azure Resource Manager te gebruiken tijdens de implementatie.
4. In het Resource Manager-sjabloon, onder de declaratie van uw cluster, beschrijft eigenschappen van de toepassing. Deze eigenschappen zijn doelreplica of count en eventuele afhankelijkheidsketens tussen resources (andere toepassingen of services). Zie voor een lijst van uitgebreide eigenschappen, de [specificaties REST API Swagger](https://aka.ms/sfrpswaggerspec). Houd er rekening mee dat dit is geen vervanging voor de toepassing of Service zich voordoet, maar in plaats daarvan beschrijft een aantal van wat is er in als onderdeel van de Resource Manager-sjabloon van het cluster. Hier volgt een voorbeeldsjabloon met de implementatie van een staatloze service *Service1* en een stateful service *Service2* als onderdeel van *Toepassing1*:

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
        "defaultValue": "Application1~Service1",
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
        "defaultValue": "Application1~Service2",
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

5. Implementeer. 

## <a name="manage-an-existing-application-via-resource-manager"></a>Een bestaande toepassing via Resource Manager beheren

Als uw cluster al actief is en sommige toepassingen die u beheren als Resource Manager wilt, resources al zijn geïmplementeerd, in plaats van de toepassingen te verwijderen en opnieuw te implementeren op deze, kunt u een PUT-aanroep met behulp van dezelfde API's om de toepassingen ophalen erkend als Resource Manager-resources. 

> [!NOTE]
> De clusterupgrade van een worden niet in orde apps genegeerd zodat de klant kunt opgeven ' maxPercentUnhealthyApplications: 100 ' in de sectie ' upgradeDescription/healthPolicy'; gedetailleerde beschrijvingen voor alle instellingen zijn [documentatie over Service Fabrics REST API-Cluster Upgradebeleid](https://docs.microsoft.com/en-us/rest/api/servicefabric/sfrp-model-clusterupgradepolicy).

## <a name="next-steps"></a>Volgende stappen

* Gebruik de [Service Fabric-CLI](service-fabric-cli.md) of [PowerShell](service-fabric-deploy-remove-applications.md) om andere toepassingen in uw cluster te implementeren. 
* [Uw Service Fabric-cluster upgraden](service-fabric-cluster-upgrade.md)

