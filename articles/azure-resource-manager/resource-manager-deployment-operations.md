---
title: Implementatiebewerkingen met Azure Resource Manager | Microsoft Docs
description: Beschrijft hoe u Azure Resource Manager-implementatiebewerkingen met de portal, PowerShell, Azure CLI en REST-API bekijken.
services: azure-resource-manager,virtual-machines
documentationcenter: ''
tags: top-support-issue
author: tfitzmac
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-multiple
ms.workload: infrastructure
ms.date: 09/28/2018
ms.author: tomfitz
ms.openlocfilehash: 8fee1e29ab3a267d77e4e43beb2c42587da5314d
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54103856"
---
# <a name="view-deployment-operations-with-azure-resource-manager"></a>Implementatiebewerkingen bekijken met Azure Resource Manager

U kunt de bewerkingen voor een implementatie via de Azure-portal weergeven. Het is mogelijk dat u meest geïnteresseerd zijn in de bewerkingen weer te geven wanneer u een fout tijdens de implementatie ontvangen hebt, zodat in dit artikel is gericht op het weergeven van bewerkingen die zijn mislukt. De portal biedt een interface waarmee u eenvoudig vinden van de fouten en mogelijke oplossingen te bepalen.

U kunt uw implementatie oplossen door te kijken naar de logboeken voor controle of de implementatiebewerkingen. In dit artikel ziet u beide methoden. Zie voor hulp bij het oplossen van fouten van bepaalde implementatie, [oplossen van veelvoorkomende fouten bij het implementeren van resources naar Azure met Azure Resource Manager](resource-manager-common-deployment-errors.md).

## <a name="portal"></a>Portal
Als de implementatiebewerkingen weergeven, gebruikt u de volgende stappen uit:

1. U ziet de status van de laatste implementatie voor de resourcegroep die betrokken zijn bij de implementatie. U kunt deze status voor meer informatie.
   
    ![Implementatiestatus](./media/resource-manager-deployment-operations/deployment-status.png)
2. U ziet de recente implementatiegeschiedenis. Selecteer de implementatie die is mislukt.
   
    ![Implementatiestatus](./media/resource-manager-deployment-operations/select-deployment.png)
3. Selecteer de koppeling voor een beschrijving van waarom de implementatie is mislukt. In de onderstaande afbeelding is de DNS-record niet uniek.  
   
    ![mislukte implementatie weergeven](./media/resource-manager-deployment-operations/view-error.png)
   
    Dit foutbericht moet voldoende zijn voor u om te beginnen met het oplossen van problemen. Als u meer informatie over welke taken zijn voltooid, ziet u de bewerkingen zoals wordt weergegeven in de volgende stappen uit.
4. U kunt alle implementatiebewerkingen weergeven. Selecteer een bewerking voor meer informatie.
   
    ![bewerkingen weergeven](./media/resource-manager-deployment-operations/view-operations.png)
   
    In dit geval ziet u dat de storage-account, het virtuele netwerk en de beschikbaarheidsset met succes zijn gemaakt. Het openbare IP-adres is mislukt en andere resources zijn niet uitgevoerd.
5. U kunt gebeurtenissen voor de implementatie bekijken door te selecteren **gebeurtenissen**.
   
    ![Gebeurtenissen weergeven](./media/resource-manager-deployment-operations/view-events.png)
6. U ziet alle gebeurtenissen voor de implementatie en selecteert u een voor meer informatie. U ziet de correlatie-id's. Deze waarde kan nuttig zijn bij het werken met de technische ondersteuning voor het oplossen van een implementatie.
   
    ![evenementen weergeven](./media/resource-manager-deployment-operations/see-all-events.png)

## <a name="powershell"></a>PowerShell
1. Als u de algemene status van een implementatie, gebruikt de **Get-AzureRmResourceGroupDeployment** opdracht. 

  ```powershell
  Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup
  ```

   Of u kunt de resultaten filteren voor alleen deze implementaties die zijn mislukt.

  ```powershell
  Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup | Where-Object ProvisioningState -eq Failed
  ```
   
1. Voor de correlatie-ID, gebruikt u:

  ```powershell
  (Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -DeploymentName azuredeploy).CorrelationId
  ```

1. Elke implementatie bevat meerdere bewerkingen. Elke bewerking vertegenwoordigt een stap in het implementatieproces. Voor het detecteren van wat is een fout opgetreden bij een implementatie, moet u doorgaans informatie over de implementaties. U ziet de status van de bewerkingen met **Get-AzureRmResourceGroupDeploymentOperation**.

  ```powershell 
  Get-AzureRmResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName vmDeployment
  ```

    Deze retourneert meerdere bewerkingen met elkaar in de volgende indeling:

  ```powershell
  Id             : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/Microsoft.Template/operations/A3EB2DA598E0A780
  OperationId    : A3EB2DA598E0A780
  Properties     : @{provisioningOperation=Create; provisioningState=Succeeded; timestamp=2016-06-14T21:55:15.0156208Z;
                   duration=PT23.0227078S; trackingId=11d376e8-5d6d-4da8-847e-6f23c6443fbf;
                   serviceRequestId=0196828d-8559-4bf6-b6b8-8b9057cb0e23; statusCode=OK; targetResource=}
  PropertiesText : {duration:PT23.0227078S, provisioningOperation:Create, provisioningState:Succeeded,
                   serviceRequestId:0196828d-8559-4bf6-b6b8-8b9057cb0e23...}
  ```

1. Meer informatie over mislukte bewerkingen ophalen van de eigenschappen voor bewerkingen met **mislukt** staat.

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object ProvisioningState -eq Failed
  ```
   
    Deze retourneert alle mislukte bewerkingen met elkaar in de volgende indeling:

  ```powershell
  provisioningOperation : Create
  provisioningState     : Failed
  timestamp             : 2016-06-14T21:54:55.1468068Z
  duration              : PT3.1449887S
  trackingId            : f4ed72f8-4203-43dc-958a-15d041e8c233
  serviceRequestId      : a426f689-5d5a-448d-a2f0-9784d14c900a
  statusCode            : BadRequest
  statusMessage         : @{error=}
  targetResource        : @{id=/subscriptions/{guid}/resourceGroups/ExampleGroup/providers/
                          Microsoft.Network/publicIPAddresses/myPublicIP;
                          resourceType=Microsoft.Network/publicIPAddresses; resourceName=myPublicIP}
  ```

    Houd er rekening mee de serviceRequestId en de trackingId voor de bewerking. De serviceRequestId kan nuttig zijn bij het werken met de technische ondersteuning voor het oplossen van een implementatie. U gebruikt de trackingId in de volgende stap om zich te richten op een bepaalde bewerking.
1. Als u het statusbericht van een bepaalde bewerking is mislukt, gebruikt u de volgende opdracht uit:

  ```powershell
  ((Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object trackingId -eq f4ed72f8-4203-43dc-958a-15d041e8c233).StatusMessage.error
  ```

    Dat geeft als resultaat:

  ```powershell
  code           message                                                                        details
  ----           -------                                                                        -------
  DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}
  ```
1. Elke implementatiebewerking in Azure bevat inhoud voor aanvragen en reacties. De aanvraaginhoud is wat u hebt verzonden naar Azure tijdens de implementatie (bijvoorbeeld een virtuele machine, maken de schijf met besturingssysteem en andere resources). De inhoud van de reactie is wat Azure van uw implementatieaanvraag teruggestuurd. Tijdens de implementatie, kunt u **DeploymentDebugLogLevel** parameter om op te geven dat de aanvraag en/of het antwoord worden bewaard in het logboek. 

  U die gegevens ophalen uit het logboek en sla het lokaal met behulp van de volgende PowerShell-opdrachten:

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName "TestDeployment" -ResourceGroupName "Test-RG").Properties.request | ConvertTo-Json |  Out-File -FilePath <PathToFile>

  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName "TestDeployment" -ResourceGroupName "Test-RG").Properties.response | ConvertTo-Json |  Out-File -FilePath <PathToFile>
  ```

## <a name="azure-cli"></a>Azure-CLI

1. Ophalen van de algehele status van een implementatie met de **azure group deployment show** opdracht.

  ```azurecli
  az group deployment show -g ExampleGroup -n ExampleDeployment
  ```
  
1. Een van de geretourneerde waarden is de **correlationId**. Deze waarde wordt gebruikt voor het bijhouden van gerelateerde gebeurtenissen en kan nuttig zijn bij het werken met de technische ondersteuning voor het oplossen van een implementatie.

  ```azurecli
  az group deployment show -g ExampleGroup -n ExampleDeployment --query properties.correlationId
  ```

1. Als de bewerkingen voor een implementatie wilt weergeven, gebruikt u het:

  ```azurecli
  az group deployment operation list -g ExampleGroup -n ExampleDeployment
  ```

## <a name="rest"></a>REST

1. Informatie ophalen over een implementatie met de [informatie over de sjabloonimplementatie van een](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get) bewerking.

  ```http
  GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
  ```

    Let met name in het antwoord op de **provisioningState**, **correlationId**, en **fout** elementen. De **correlationId** wordt gebruikt voor het bijhouden van gerelateerde gebeurtenissen en kan nuttig zijn bij het werken met de technische ondersteuning voor het oplossen van een implementatie.

  ```json
  { 
    ...
    "properties": {
      "provisioningState":"Failed",
      "correlationId":"d5062e45-6e9f-4fd3-a0a0-6b2c56b15757",
      ...
      "error":{
        "code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/arm-debug for usage details.",
        "details":[{"code":"Conflict","message":"{\r\n  \"error\": {\r\n    \"message\": \"Conflict\",\r\n    \"code\": \"Conflict\"\r\n  }\r\n}"}]
      }  
    }
  }
  ```

2. Informatie over implementaties met [lijst van alle implementatiebewerkingen voor sjabloon](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_List). 

  ```http
  GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}/operations?$skiptoken={skiptoken}&api-version={api-version}
  ```
   
    Het antwoord bevat aanvraag en/of antwoord informatie op basis van wat u hebt opgegeven in de **debugSetting** eigenschap tijdens de implementatie.

  ```json
  {
    ...
    "properties": 
    {
      ...
      "request":{
        "content":{
          "location":"West US",
          "properties":{
            "accountType": "Standard_LRS"
          }
        }
      },
      "response":{
        "content":{
          "error":{
            "message":"Conflict","code":"Conflict"
          }
        }
      }
    }
  }
  ```


## <a name="next-steps"></a>Volgende stappen
* Zie voor hulp bij het oplossen van fouten van bepaalde implementatie, [oplossen van veelvoorkomende fouten bij het implementeren van resources naar Azure met Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Zie voor meer informatie over het gebruik van de activiteitenlogboeken voor het bewaken van andere typen acties, [activiteitenlogboeken voor het beheren van Azure-resources bekijken](resource-group-audit.md).
* Zie voor het valideren van uw implementatie voordat deze wordt uitgevoerd, [implementeren van een resourcegroep met Azure Resource Manager-sjabloon](resource-group-template-deploy.md).

