---
title: Implementatiegeschiedenis met Azure Resource Manager | Microsoft Docs
description: Beschrijft hoe u Azure Resource Manager-implementatiebewerkingen met de portal, PowerShell, Azure CLI en REST-API bekijken.
tags: top-support-issue
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: tomfitz
ms.openlocfilehash: 58d22e3fcae5c30e5d7dcc39b317afeef4a693ee
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65605999"
---
# <a name="view-deployment-history-with-azure-resource-manager"></a>Geschiedenis van de implementatie weergeven met Azure Resource Manager

Azure Resource Manager kunt u de implementatiegeschiedenis van uw bekijken en controleren van specifieke bewerkingen die in eerdere implementaties. U kunt zien de resources die zijn geïmplementeerd, en informatie over eventuele fouten.

Zie voor hulp bij het oplossen van fouten van bepaalde implementatie, [oplossen van veelvoorkomende fouten bij het implementeren van resources naar Azure met Azure Resource Manager](resource-manager-common-deployment-errors.md).

## <a name="portal"></a>Portal

Voor meer informatie over een implementatie uit de implementatiegeschiedenis.

1. Selecteer de resourcegroep die u wilt onderzoeken.

1. Selecteer de koppeling onder **implementaties**.

   ![Selecteer de implementatiegeschiedenis](./media/resource-manager-deployment-operations/select-deployment-history.png)

1. Selecteer een van de implementaties van geschiedenis van de implementatie.

   ![Selecteer implementatie](./media/resource-manager-deployment-operations/select-details.png)

1. Een samenvatting van de implementatie wordt weergegeven, samen met een overzicht van de resources die zijn geïmplementeerd.

    ![Implementatieoverzicht](./media/resource-manager-deployment-operations/view-deployment-summary.png)

1. Als u de sjabloon die wordt gebruikt voor de implementatie, selecteer **sjabloon**. U kunt de sjabloon opnieuw downloaden.

    ![Sjabloon weergeven](./media/resource-manager-deployment-operations/show-template-from-history.png)

1. Als uw implementatie is mislukt, ziet u een foutbericht weergegeven. Selecteer het foutbericht voor meer informatie.

    ![mislukte implementatie weergeven](./media/resource-manager-deployment-operations/show-error.png)

1. Het gedetailleerde foutbericht wordt weergegeven.

    ![Foutdetails weergeven](./media/resource-manager-deployment-operations/show-details.png)

1. De correlatie-ID wordt gebruikt voor het bijhouden van gerelateerde gebeurtenissen en kan nuttig zijn bij het werken met de technische ondersteuning voor het oplossen van een implementatie.

    ![Correlatie-ID ophalen](./media/resource-manager-deployment-operations/get-correlation-id.png)

1. Voor meer informatie over de stap die is mislukt, selecteert u **Bewerkingsdetails**.

    ![Implementatiebewerkingen selecteren](./media/resource-manager-deployment-operations/select-deployment-operations.png)

1. U ziet de details voor deze stap van de implementatie.

    ![Informatie over de bewerking weergeven](./media/resource-manager-deployment-operations/show-operation-details.png)

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Als u de algemene status van een implementatie, gebruikt de **Get-AzResourceGroupDeployment** opdracht.

```azurepowershell-interactive
Get-AzResourceGroupDeployment -ResourceGroupName ExampleGroup
```

Of u kunt de resultaten filteren voor alleen deze implementaties die zijn mislukt.

```azurepowershell-interactive
Get-AzResourceGroupDeployment -ResourceGroupName ExampleGroup | Where-Object ProvisioningState -eq Failed
```

De correlatie-ID wordt gebruikt voor het bijhouden van gerelateerde gebeurtenissen en kan nuttig zijn bij het werken met de technische ondersteuning voor het oplossen van een implementatie. Voor de correlatie-ID, gebruikt u:

```azurepowershell-interactive
(Get-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -DeploymentName azuredeploy).CorrelationId
```

Elke implementatie bevat meerdere bewerkingen. Elke bewerking vertegenwoordigt een stap in het implementatieproces. Voor het detecteren van wat is een fout opgetreden bij een implementatie, moet u doorgaans informatie over de implementaties. U ziet de status van de bewerkingen met **Get-AzResourceGroupDeploymentOperation**.

```azurepowershell-interactive
Get-AzResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName azuredeploy
```

Deze retourneert meerdere bewerkingen met elkaar in de volgende indeling:

```powershell
Id             : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/Microsoft.Template/operations/A3EB2DA598E0A780
OperationId    : A3EB2DA598E0A780
Properties     : @{provisioningOperation=Create; provisioningState=Succeeded; timestamp=2019-05-13T21:42:40.7151512Z;
                duration=PT23.0227078S; trackingId=11d376e8-5d6d-4da8-847e-6f23c6443fbf;
                serviceRequestId=0196828d-8559-4bf6-b6b8-8b9057cb0e23; statusCode=OK; targetResource=}
PropertiesText : {duration:PT23.0227078S, provisioningOperation:Create, provisioningState:Succeeded,
                serviceRequestId:0196828d-8559-4bf6-b6b8-8b9057cb0e23...}
```

Meer informatie over mislukte bewerkingen ophalen van de eigenschappen voor bewerkingen met **mislukt** staat.

```azurepowershell-interactive
(Get-AzResourceGroupDeploymentOperation -DeploymentName azuredeploy -ResourceGroupName ExampleGroup).Properties | Where-Object ProvisioningState -eq Failed
```

Deze retourneert alle mislukte bewerkingen met elkaar in de volgende indeling:

```powershell
provisioningOperation : Create
provisioningState     : Failed
timestamp             : 2019-05-13T21:42:40.7151512Z
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

Als u het statusbericht van een bepaalde bewerking is mislukt, gebruikt u de volgende opdracht uit:

```azurepowershell-interactive
((Get-AzResourceGroupDeploymentOperation -DeploymentName azuredeploy -ResourceGroupName ExampleGroup).Properties | Where-Object trackingId -eq f4ed72f8-4203-43dc-958a-15d041e8c233).StatusMessage.error
```

Dat geeft als resultaat:

```powershell
code           message                                                                        details
----           -------                                                                        -------
DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}
```

Elke implementatiebewerking in Azure bevat inhoud voor aanvragen en reacties. Tijdens de implementatie, kunt u **DeploymentDebugLogLevel** parameter om op te geven dat de aanvraag en/of het antwoord worden geregistreerd.

U die gegevens ophalen uit het logboek en sla het lokaal met behulp van de volgende PowerShell-opdrachten:

```powershell
(Get-AzResourceGroupDeploymentOperation -DeploymentName "TestDeployment" -ResourceGroupName "Test-RG").Properties.request | ConvertTo-Json |  Out-File -FilePath <PathToFile>

(Get-AzResourceGroupDeploymentOperation -DeploymentName "TestDeployment" -ResourceGroupName "Test-RG").Properties.response | ConvertTo-Json |  Out-File -FilePath <PathToFile>
```

## <a name="azure-cli"></a>Azure-CLI

Als u de algemene status van een implementatie, gebruikt de **azure group deployment show** opdracht.

```azurecli-interactive
az group deployment show -g ExampleGroup -n ExampleDeployment
```
  
De correlatie-ID wordt gebruikt voor het bijhouden van gerelateerde gebeurtenissen en kan nuttig zijn bij het werken met de technische ondersteuning voor het oplossen van een implementatie.

```azurecli-interactive
az group deployment show -g ExampleGroup -n ExampleDeployment --query properties.correlationId
```

Als de bewerkingen voor een implementatie wilt weergeven, gebruikt u het:

```azurecli-interactive
az group deployment operation list -g ExampleGroup -n ExampleDeployment
```

## <a name="rest"></a>REST

Voor informatie over een implementatie, gebruikt de [informatie over de sjabloonimplementatie van een](https://docs.microsoft.com/rest/api/resources/deployments) bewerking.

```
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

Voor informatie over implementaties, gebruikt [lijst van alle implementatiebewerkingen voor sjabloon](https://docs.microsoft.com/rest/api/resources/deployments). 

```
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

