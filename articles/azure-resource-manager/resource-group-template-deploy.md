---
title: Resources implementeren met PowerShell en een sjabloon | Microsoft Docs
description: Gebruik Azure Resource Manager en Azure PowerShell om een resources implementeren op Azure. De resources zijn gedefinieerd in een Resource Manager-sjabloon.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.assetid: 55903f35-6c16-4c6d-bf52-dbf365605c3f
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2018
ms.author: tomfitz
ms.openlocfilehash: 6e6cd7f1677d8ae11f05c2a2bca4233603a29408
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55075666"
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a>Resources implementeren met Resource Manager-sjablonen en Azure PowerShell

In dit artikel wordt uitgelegd hoe u Azure PowerShell gebruiken met Resource Manager-sjablonen voor het implementeren van uw resources naar Azure. Als u niet bekend bent met de concepten van het implementeren en beheren van uw Azure-oplossingen, Zie [overzicht van Azure Resource Manager](resource-group-overview.md).

De Resource Manager-sjabloon die u implementeert, kunnen een lokaal bestand op uw computer of een extern bestand dat in een opslagplaats, zoals GitHub bevindt zich. De sjabloon die u in dit artikel implementeert is beschikbaar als [sjabloon van de storage-account in GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).

Installeer zo nodig de Azure PowerShell-module door de instructies in de [Azure PowerShell-handleiding](/powershell/azure/overview) uit te voeren en voer vervolgens `Connect-AzAccount` uit om verbinding te maken met Azure.

<a id="deploy-local-template" />

## <a name="deploy-a-template-from-your-local-machine"></a>Een sjabloon implementeren vanuit uw lokale computer

Bij het implementeren van resources naar Azure, u:

1. Aanmelden bij uw Azure-account
2. Maak een resourcegroep die als de container voor de geïmplementeerde resources fungeert. De naam van de resourcegroep kan alleen alfanumerieke tekens, punten, onderstrepingstekens, afbreekstreepjes en haakjes bevatten. Het kan maximaal 90 tekens zijn. Deze mag niet eindigen op een punt.
3. Implementeren in de resourcegroep van de sjabloon die u de resources definieert te maken

Een sjabloon kunt parameters waarmee u kunt het aanpassen van de implementatie bevatten. U kunt bijvoorbeeld waarden die zijn toegesneden opgeven voor een bepaalde omgeving (zoals ontwikkelen, testen en productie). De voorbeeldsjabloon definieert een parameter voor de SKU van het opslagaccount.

Het volgende voorbeeld wordt een resourcegroep en implementeert u een sjabloon van uw lokale computer:

```powershell
Connect-AzAccount

Select-AzSubscription -SubscriptionName <yourSubscriptionName>
 
New-AzResourceGroup -Name ExampleResourceGroup -Location "South Central US"
New-AzResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType Standard_GRS
```

De implementatie kan enkele minuten duren. Als deze is voltooid, ziet u een bericht dat het resultaat bevat:

```powershell
ProvisioningState       : Succeeded
```

## <a name="deploy-a-template-from-an-external-source"></a>Een sjabloon implementeren vanuit een externe bron

In plaats van Resource Manager-sjablonen op uw lokale computer, wellicht kunt u beter om op te slaan in een externe locatie. U kunt sjablonen opslaan in een opslagplaats voor bronbeheer (zoals GitHub). Of u kunt ze opslaan in Azure storage-account voor gedeelde toegang in uw organisatie.

Voor het implementeren van een externe-sjabloon, gebruiken de **TemplateUri** parameter. Gebruik de URI in het voorbeeld om de voorbeeldsjabloon uit GitHub te implementeren.

```powershell
New-AzResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -storageAccountType Standard_GRS
```

Het vorige voorbeeld vereist een openbaar toegankelijke URI voor de sjabloon die voor de meeste scenario's, werkt omdat de sjabloon mag niet de gevoelige gegevens bevatten. Als u nodig hebt om op te geven van gevoelige gegevens (zoals een beheerderswachtwoord), moet u die waarde als een beveiligde parameter. Echter, als u niet dat de sjabloon openbaar toegankelijk moeten zijn wilt, kunt u deze beschermen door in een persoonlijke opslagcontainer op te slaan. Zie voor meer informatie over het implementeren van een sjabloon die de token van een shared access signature (SAS) is vereist, [persoonlijke sjablonen implementeren met SAS-token](resource-manager-powershell-sas-token.md).

[!INCLUDE [resource-manager-cloud-shell-deploy.md](../../includes/resource-manager-cloud-shell-deploy.md)]

Gebruik de volgende opdrachten in de Cloud Shell:

```powershell
New-AzResourceGroup -Name ExampleResourceGroup -Location "South Central US"
New-AzResourceGroupDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri <copied URL> `
  -storageAccountType Standard_GRS
```

## <a name="deploy-to-more-than-one-resource-group-or-subscription"></a>Implementeren naar meer dan één resourcegroep of abonnement

Normaal gesproken implementeren u alle resources in uw sjabloon één resourcegroep bestaan. Er zijn echter scenario's waarin u wilt een set met resources samen te implementeren, maar plaats deze in verschillende resourcegroepen of abonnementen. U kunt implementeren met slechts vijf resourcegroepen in een enkele implementatie. Zie voor meer informatie, [implementeren-Azure-resources aan meer dan één abonnement of resourcegroep](resource-manager-cross-resource-group-deployment.md).

<a id="parameter-file" />

## <a name="parameters"></a>Parameters

Als u wilt doorgeven van parameterwaarden, kunt u parameters inline of een parameterbestand. De voorgaande voorbeelden in dit artikel weergeven inline parameters.

### <a name="inline-parameters"></a>Inline-parameters

Als u wilt in line-parameters kunt toevoegen, bevatten de namen van de parameter met de `New-AzResourceGroupDeployment` opdracht. Bijvoorbeeld: een tekenreeks of matrix doorgeven aan een sjabloon, gebruiken:

```powershell
$arrayParam = "value1", "value2"
New-AzResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\demotemplate.json `
  -exampleString "inline string" `
  -exampleArray $arrayParam
```

U kunt ook de inhoud van bestand ophalen en verstrekken die inhoud als een inline-parameter.

```powershell
$arrayParam = "value1", "value2"
New-AzResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\demotemplate.json `
  -exampleString $(Get-Content -Path c:\MyTemplates\stringcontent.txt -Raw) `
  -exampleArray $arrayParam
```

Een parameterwaarde ophalen uit een bestand is handig wanneer u moet opgeven-configuratiewaarden. U kunt bijvoorbeeld opgeven [cloud-init-waarden voor een virtuele Linux-machine](../virtual-machines/linux/using-cloud-init.md).

### <a name="parameter-files"></a>Parameterbestanden

In plaats van de parameters doorgeven als inline-waarden in het script, wellicht vindt u het eenvoudiger te gebruiken van een JSON-bestand met de parameterwaarden. De parameter-bestand kan een lokaal bestand of een extern bestand aan een URI die toegankelijk zijn.

De parameterbestand moet zich in de volgende indeling:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "storageAccountType": {
         "value": "Standard_GRS"
     }
  }
}
```

U ziet dat de parametersectie bevat een parameternaam op die overeenkomt met de parameter die is gedefinieerd in uw sjabloon (parameter). Het parameterbestand bevat een waarde voor de parameter. Deze waarde wordt automatisch doorgegeven aan de sjabloon tijdens de implementatie. U kunt meer dan één parameterbestand maken, en geeft u in de juiste parameter-bestand voor het scenario. 

Kopieer het voorgaande voorbeeld en sla deze op als een bestand met de naam `storage.parameters.json`.

Als u wilt een lokale parameterbestand doorgeven, gebruikt u de **TemplateParameterFile** parameter:

```powershell
New-AzResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json `
  -TemplateParameterFile c:\MyTemplates\storage.parameters.json
```

Als u wilt een parameterbestand met externe doorgeven, gebruikt u de **TemplateParameterUri** parameter:

```powershell
New-AzResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.parameters.json
```

### <a name="parameter-precedence"></a>Parameter-prioriteit

U kunt gebruiken in line-parameters en een lokale parameterbestand in dezelfde implementatiebewerking. U kunt bijvoorbeeld enkele waarden in de lokale parameterbestand opgeven en toevoegen van andere waarden inline tijdens de implementatie. Als u waarden voor een parameter in de lokale parameterbestand en inline opgeeft, wordt de waarde inline voorrang.

Echter wanneer u een externe parameterbestand gebruikt, geeft u de kan geen andere waarden de inline of vanuit een lokaal bestand. Wanneer u een parameterbestand in de **TemplateParameterUri** parameter, alle inline parameters worden genegeerd. Geef alle parameterwaarden in het externe bestand. Als uw sjabloon een gevoelige waarde die u niet in het parameterbestand opnemen bevat, die waarde toevoegen aan een key vault of dynamisch alle parameter waarden inline opgeeft tussen.

### <a name="parameter-name-conflicts"></a>Parameter naamconflicten

Als de sjabloon een parameter met dezelfde naam als een van de parameters in de PowerShell-opdracht bevat, PowerShell biedt de parameter van de sjabloon voor het achtervoegsel **FromTemplate**. Bijvoorbeeld, een parameter met de naam **ResourceGroupName** in uw sjabloon veroorzaakt een conflict met de **ResourceGroupName** parameter in de [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) de cmdlet. U wordt gevraagd om een waarde voor **ResourceGroupNameFromTemplate**. In het algemeen moet u deze verwarring voorkomen door het niet naming parameters met dezelfde naam als parameters gebruikt voor implementatiebewerkingen.

## <a name="test-a-template-deployment"></a>Een sjabloonimplementatie testen

U kunt de waarden van uw sjabloon en de parameterbestanden testen zonder daadwerkelijk de resources te implementeren met [Test-AzureRmResourceGroupDeployment](/powershell/module/az.resources/test-azresourcegroupdeployment). 

```powershell
Test-AzResourceGroupDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType Standard_GRS
```

Als er geen fouten worden aangetroffen, wordt de opdracht is voltooid zonder een reactie. Als er een fout wordt gedetecteerd, wordt de opdracht een foutbericht weergegeven. Doorgeven van een onjuiste waarde voor het opslagaccount-SKU, retourneert bijvoorbeeld de volgende fout:

```powershell
Test-AzResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType badSku

Code    : InvalidTemplate
Message : Deployment template validation failed: 'The provided value 'badSku' for the template parameter 'storageAccountType'
          at line '15' and column '24' is not valid. The parameter value is not part of the allowed value(s):
          'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.
Details :
```

Als uw sjabloon een syntaxisfout heeft, retourneert de opdracht een foutbericht met dat de sjabloon kan niet worden geparseerd. Het bericht geeft aan dat het regelnummer en de positie van de fout bij het parseren.

```powershell
Test-AzResourceGroupDeployment : After parsing a value an unexpected character was encountered: 
  ". Path 'variables', line 31, position 3.
```

## <a name="next-steps"></a>Volgende stappen
* De voorbeelden in dit artikel worden resources implementeren op een resourcegroep in uw standaardabonnement. Zie voor het gebruik van een ander abonnement [meerdere Azure-abonnementen beheren](/powershell/azure/manage-subscriptions-azureps).
* Als u wilt opgeven voor het verwerken van resources die aanwezig zijn in de resourcegroep, maar niet zijn gedefinieerd in de sjabloon, Zie [Azure Resource Manager-implementatiemodi](deployment-modes.md).
* Zie voor meer informatie over het definiëren van parameters in uw sjabloon, [inzicht in de structuur en de syntaxis van Azure Resource Manager-sjablonen](resource-group-authoring-templates.md).
* Zie voor tips over het oplossen van veelvoorkomende implementatiefouten [veelvoorkomende problemen oplossen Azure-implementatie met Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Zie voor meer informatie over het implementeren van een sjabloon waarvoor een SAS-token [persoonlijke sjablonen implementeren met SAS-token](resource-manager-powershell-sas-token.md).
* Zie voor veilig implementatie uw service in meer dan één regio, [Azure Deployment Manager](deployment-manager-overview.md).
