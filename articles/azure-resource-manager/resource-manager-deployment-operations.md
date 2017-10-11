---
title: Implementatiebewerkingen met Azure Resource Manager | Microsoft Docs
description: Hierin wordt beschreven hoe u Azure Resource Manager deployment bewerkingen met de portal, PowerShell, Azure CLI en REST-API.
services: azure-resource-manager,virtual-machines
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: infrastructure
ms.date: 01/13/2017
ms.author: tomfitz
ms.openlocfilehash: fb6b3b357fd1f66184e480115a9c863ba31ac193
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="view-deployment-operations-with-azure-resource-manager"></a>Bewerkingen van de implementatie weergeven met Azure Resource Manager


U kunt de bewerkingen voor een implementatie via de Azure portal bekijken. Hebt u mogelijk de meest geïnteresseerd in de bewerkingen weer te geven wanneer u een fout opgetreden tijdens de implementatie ontvangen hebt zodat dit artikel is gericht op het weergeven van bewerkingen die zijn mislukt. De portal biedt een interface waarmee u eenvoudig zoeken van de fouten en mogelijke oplossingen te bepalen.

[!INCLUDE [resource-manager-troubleshoot-introduction](../../includes/resource-manager-troubleshoot-introduction.md)]

## <a name="portal"></a>Portal
Als de implementatiebewerkingen weergeven, gebruikt u de volgende stappen uit:

1. U ziet de status van de laatste implementatie voor de resourcegroep die zijn betrokken bij de implementatie. U kunt deze status voor meer informatie.
   
    ![Implementatiestatus](./media/resource-manager-deployment-operations/deployment-status.png)
2. Ziet u de recente implementatiegeschiedenis van de. Selecteer de implementatie die is mislukt.
   
    ![Implementatiestatus](./media/resource-manager-deployment-operations/select-deployment.png)
3. Selecteer de koppeling voor een beschrijving van waarom de implementatie is mislukt. De DNS-record is niet uniek in de onderstaande afbeelding.  
   
    ![mislukte implementatie weergeven](./media/resource-manager-deployment-operations/view-error.png)
   
    Dit foutbericht moeten voldoende voor u om te beginnen met het oplossen van problemen. Als u meer informatie nodig over welke taken zijn voltooid, kunt u de bewerkingen weergeven zoals weergegeven in de volgende stappen uit.
4. U ziet de implementatiebewerkingen in de **implementatie** blade. Selecteer een bewerking voor meer informatie.
   
    ![bewerkingen weergeven](./media/resource-manager-deployment-operations/view-operations.png)
   
    In dit geval zien u dat de storage-account, het virtuele netwerk en de beschikbaarheidsset zijn gemaakt. Het openbare IP-adres is mislukt en andere resources zijn niet is uitgevoerd.
5. U kunt gebeurtenissen voor de implementatie weergeven door **gebeurtenissen**.
   
    ![gebeurtenissen weergeven](./media/resource-manager-deployment-operations/view-events.png)
6. U selecteert een voor meer informatie en Zie de gebeurtenissen voor de implementatie. U ziet te de correlatie-id's. Deze waarde kan nuttig zijn bij het werken met de technische ondersteuning voor het oplossen van een implementatie.
   
    ![Zie gebeurtenissen](./media/resource-manager-deployment-operations/see-all-events.png)

## <a name="powershell"></a>PowerShell
1. Als u de algehele status van een implementatie, gebruikt de **Get-AzureRmResourceGroupDeployment** opdracht. 

  ```powershell
  Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup
  ```

   Of u kunt de resultaten filteren voor alleen deze implementaties die zijn mislukt.

  ```powershell
  Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup | Where-Object ProvisioningState -eq Failed
  ```
   
2. Elke implementatie bevat meerdere bewerkingen. Elke bewerking vertegenwoordigt een stap in het implementatieproces. Om te ontdekken wat is een fout opgetreden bij een implementatie, moet u doorgaans om details over de implementatiebewerkingen te bekijken. U ziet de status van de bewerkingen met **Get-AzureRmResourceGroupDeploymentOperation**.

  ```powershell 
  Get-AzureRmResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName vmDeployment
  ```

    Dit retourneert meerdere bewerkingen met elkaar in de volgende indeling:

  ```powershell
  Id             : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/Microsoft.Template/operations/A3EB2DA598E0A780
  OperationId    : A3EB2DA598E0A780
  Properties     : @{provisioningOperation=Create; provisioningState=Succeeded; timestamp=2016-06-14T21:55:15.0156208Z;
                   duration=PT23.0227078S; trackingId=11d376e8-5d6d-4da8-847e-6f23c6443fbf;
                   serviceRequestId=0196828d-8559-4bf6-b6b8-8b9057cb0e23; statusCode=OK; targetResource=}
  PropertiesText : {duration:PT23.0227078S, provisioningOperation:Create, provisioningState:Succeeded,
                   serviceRequestId:0196828d-8559-4bf6-b6b8-8b9057cb0e23...}
  ```

3. Als u meer informatie over mislukte bewerkingen, halen de eigenschappen voor bewerkingen met **mislukt** status.

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object ProvisioningState -eq Failed
  ```
   
    Dit retourneert alle mislukte bewerkingen met elkaar in de volgende indeling:

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

    Noteer de serviceRequestId en de trackingId voor de bewerking. De serviceRequestId kan nuttig zijn bij het werken met de technische ondersteuning voor het oplossen van een implementatie. U gebruikt de trackingId in de volgende stap om zich te richten op een bepaalde bewerking.
4. Als u het statusbericht dat van een bepaalde bewerking is mislukt, gebruikt u de volgende opdracht:

  ```powershell
  ((Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object trackingId -eq f4ed72f8-4203-43dc-958a-15d041e8c233).StatusMessage.error
  ```

    Die wordt geretourneerd:

  ```powershell
  code           message                                                                        details
  ----           -------                                                                        -------
  DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}
  ```
4. Elke implementatiebewerking in Azure bevat-aanvraag en antwoord-inhoud. Inhoud van de aanvraag is wat u hebt verzonden naar Azure tijdens de implementatie (bijvoorbeeld: Maak een VM besturingssysteemschijf en andere bronnen). De antwoordinhoud is wat Azure van uw implementatieaanvraag teruggestuurd. Tijdens de implementatie, kunt u **DeploymentDebugLogLevel** parameter worden om op te geven dat de aanvraag en/of antwoord in het logboek behouden blijven. 

  U die informatie ophalen van het logboek, en sla het lokaal met behulp van de volgende PowerShell-opdrachten:

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName "TestDeployment" -ResourceGroupName "Test-RG").Properties.request | ConvertTo-Json |  Out-File -FilePath <PathToFile>

  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName "TestDeployment" -ResourceGroupName "Test-RG").Properties.response | ConvertTo-Json |  Out-File -FilePath <PathToFile>
  ```

## <a name="azure-cli"></a>Azure CLI

1. Ophalen van de algehele status van een implementatie met de **azure-groep implementatie weergeven** opdracht.

  ```azurecli
  azure group deployment show --resource-group ExampleGroup --name ExampleDeployment --json
  ```
  
  Een van de geretourneerde waarden is de **correlationId**. Deze waarde wordt gebruikt voor het bijhouden van gerelateerde gebeurtenissen en kan nuttig zijn bij het werken met de technische ondersteuning voor het oplossen van een implementatie.

  ```azurecli
  "properties": {
    "provisioningState": "Failed",
    "correlationId": "4002062a-a506-4b5e-aaba-4147036b771a",
  ```

2. Als de bewerkingen voor een implementatie wilt weergeven, gebruikt u het:

  ```azurecli
  azure group deployment operation list --resource-group ExampleGroup --name ExampleDeployment --json
  ```

## <a name="rest"></a>REST

1. Informatie ophalen over een implementatie met de [informatie ophalen over de sjabloonimplementatie van een](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get) bewerking.

  ```http
  GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
  ```

    In het antwoord, houd er rekening mee met name de **provisioningState**, **correlationId**, en **fout** elementen. De **correlationId** wordt gebruikt voor het bijhouden van gerelateerde gebeurtenissen en kan nuttig zijn bij het werken met de technische ondersteuning voor het oplossen van een implementatie.

  ```json
  { 
    ...
    "properties": {
      "provisioningState":"Failed",
      "correlationId":"d5062e45-6e9f-4fd3-a0a0-6b2c56b15757",
      ...
      "error":{
        "code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see http://aka.ms/arm-debug for usage details.",
        "details":[{"code":"Conflict","message":"{\r\n  \"error\": {\r\n    \"message\": \"Conflict\",\r\n    \"code\": \"Conflict\"\r\n  }\r\n}"}]
      }  
    }
  }
  ```

2. Informatie ophalen over de implementatiebewerkingen met de [lijst van alle implementatiebewerkingen voor sjabloon](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_List) bewerking. 

  ```http
  GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}/operations?$skiptoken={skiptoken}&api-version={api-version}
  ```
   
    Het antwoord bevat de aanvraag en/of antwoord informatie op basis van wat u hebt opgegeven in de **debugSetting** eigenschap tijdens de implementatie.

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
* Zie voor meer informatie over het oplossen van fouten voor bepaalde implementatie, [oplossen van veelvoorkomende fouten bij het implementeren van resources in Azure met Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Zie voor meer informatie over het gebruik van de activiteitenlogboeken van de voor het bewaken van andere soorten acties, [activiteitenlogboeken voor het beheren van Azure-resources bekijken](resource-group-audit.md).
* Zie voor het valideren van uw implementatie voordat deze wordt uitgevoerd, [een resourcegroep implementeren met Azure Resource Manager-sjabloon](resource-group-template-deploy.md).

