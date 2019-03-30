---
title: Resources implementeren met PowerShell en een sjabloon | Microsoft Docs
description: Azure Resource Manager en Azure PowerShell gebruiken om resources te implementeren naar Azure. De resources zijn gedefinieerd in een Resource Manager-sjabloon.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.assetid: 55903f35-6c16-4c6d-bf52-dbf365605c3f
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/28/2019
ms.author: tomfitz
ms.openlocfilehash: 2ef5cc702bd5035c958a8feb9b6f5051781cd3cc
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58649791"
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a>Resources implementeren met Resource Manager-sjablonen en Azure PowerShell

Meer informatie over het gebruik van Azure PowerShell met Resource Manager-sjablonen voor het implementeren van uw resources naar Azure. Zie voor meer informatie over de concepten van implementeren en beheren van uw Azure-oplossingen, [overzicht van Azure Resource Manager](resource-group-overview.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="deployment-scope"></a>Reikwijdte van de implementatie

U kunt uw implementatie naar een Azure-abonnement of een resourcegroep binnen een abonnement kunt richten. In de meeste gevallen zult u richt de implementatie naar een resourcegroep. Abonnementimplementaties gebruiken voor het toepassen van beleid en de roltoewijzingen voor het abonnement. U kunt ook abonnementimplementaties gebruiken een resourcegroep maken en implementeren van resources toe. Afhankelijk van het bereik van de implementatie, kunt u verschillende opdrachten gebruiken.

Om te implementeren op een **resourcegroep**, gebruikt u [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment):

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile <path-to-template>
```

Om te implementeren op een **abonnement**, gebruikt u [New-AzDeployment](/powershell/module/az.resources/new-azdeployment):

```azurepowershell
New-AzDeployment -Location <location> -TemplateFile <path-to-template>
```

De voorbeelden in dit artikel gebruikt brongroepimplementaties. Zie voor meer informatie over de implementatie van abonnement [maken van resourcegroepen en resources op het abonnementsniveau](deploy-to-subscription.md).

## <a name="prerequisites"></a>Vereisten

U moet een sjabloon te implementeren. Als u dit niet al hebt, downloaden en slaat u een [voorbeeldsjabloon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json) vanuit de opslagplaats voor Azure Quickstart-sjablonen. De lokale bestandsnaam gebruikt in dit artikel is **c:\MyTemplates\azuredeploy.json**.

Tenzij u de Azure Cloud shell gebruiken om sjablonen te implementeren, moet u Azure PowerShell installeren en verbinding maken met Azure:

- **Azure PowerShell-cmdlets installeren op uw lokale computer.** Zie [Aan de slag met Azure PowerShell](/powershell/azure/get-started-azureps) voor meer informatie.
- **Verbinding maken met Azure met behulp van [Connect AZAccount](/powershell/module/az.accounts/connect-azaccount)**. Als u meerdere Azure-abonnementen hebt, mogelijk moet u ook om uit te voeren [Set AzContext](/powershell/module/Az.Accounts/Set-AzContext). Zie voor meer informatie, [meerdere Azure-abonnementen gebruiken](/powershell/azure/manage-subscriptions-azureps).

## <a name="deploy-local-template"></a>Lokale sjabloon implementeren

Het volgende voorbeeld wordt een resourcegroep en implementeert u een sjabloon van uw lokale computer. De naam van de resourcegroep kan alleen alfanumerieke tekens, punten, onderstrepingstekens, afbreekstreepjes en haakjes bevatten. Het kan maximaal 90 tekens zijn. Deze mag niet eindigen op een punt.

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
  -TemplateFile c:\MyTemplates\azuredeploy.json
```

De implementatie kan enkele minuten duren.

## <a name="deploy-remote-template"></a>Externe-sjabloon implementeren

In plaats van Resource Manager-sjablonen op uw lokale computer, wellicht kunt u beter om op te slaan in een externe locatie. U kunt sjablonen opslaan in een opslagplaats voor bronbeheer (zoals GitHub). Of u kunt ze opslaan in Azure storage-account voor gedeelde toegang in uw organisatie.

Voor het implementeren van een externe-sjabloon, gebruiken de **TemplateUri** parameter. Gebruik de URI in het voorbeeld om de voorbeeldsjabloon uit GitHub te implementeren.

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```

Het vorige voorbeeld vereist een openbaar toegankelijke URI voor de sjabloon die voor de meeste scenario's, werkt omdat de sjabloon mag niet de gevoelige gegevens bevatten. Als u nodig hebt om op te geven van gevoelige gegevens (zoals een beheerderswachtwoord), moet u die waarde als een beveiligde parameter. Echter, als u niet dat de sjabloon openbaar toegankelijk moeten zijn wilt, kunt u deze beschermen door in een persoonlijke opslagcontainer op te slaan. Zie voor meer informatie over het implementeren van een sjabloon die de token van een shared access signature (SAS) is vereist, [persoonlijke sjablonen implementeren met SAS-token](resource-manager-powershell-sas-token.md). Zie voor een zelfstudie doorloopt, [zelfstudie: Azure Key Vault integreren in de Resource Manager-sjabloonimplementatie](./resource-manager-tutorial-use-key-vault.md) voor meer informatie.

## <a name="deploy-from-azure-cloud-shell"></a>Implementeren vanuit Azure Cloud shell

U kunt de [Azure Cloud Shell](https://shell.azure.com) om uw sjabloon te implementeren. Voor het implementeren van een externe-sjabloon, geeft u de URI van de sjabloon. Voor het implementeren van een lokale sjabloon, moet u eerst uw sjabloon laden in het opslagaccount voor Cloud Shell. Als u wilt bestanden uploaden naar de shell, selecteer de **uploaden/downloaden van bestanden** menupictogram in de shell-venster.

Als u wilt openen in de cloudshell, blader naar [ https://shell.azure.com ](https://shell.azure.com), of selecteer **Try It** van de volgende sectie met voorbeeldcode:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```

Als u wilt de code in de shell plakt, de shell met de rechtermuisknop en selecteer vervolgens **plakken**.

## <a name="redeploy-when-deployment-fails"></a>Opnieuw implementeren wanneer de implementatie mislukt

Deze functie wordt ook wel bekend als *Rollback bij fout*. Wanneer een implementatie is mislukt, kunt u automatisch opnieuw implementeren de implementatie van een eerdere, geslaagde geschiedenis van uw implementatie. Als u opnieuw implementeren, gebruiken de `-RollbackToLastDeployment` of `-RollBackDeploymentName` parameter in de implementatieopdracht. Deze functionaliteit is handig als u een bekende goede status voor de Infrastructuurimplementatie van uw gekomen en wilt deze optie om te worden teruggezet naar een. Er zijn een aantal aanvullende opmerkingen en beperkingen:

- Het opnieuw distribueren wordt uitgevoerd, precies zoals deze eerder is uitgevoerd met dezelfde parameters. U kunt de parameters niet wijzigen.
- De vorige implementatie wordt uitgevoerd met behulp van de [volledige modus](./deployment-modes.md#complete-mode). Alle resources die niet zijn opgenomen in de vorige implementatie worden verwijderd en de resourceconfiguraties zijn ingesteld op de vorige status. Zorg ervoor dat u volledig inzicht in de [implementatiemodi](./deployment-modes.md).
- Het opnieuw implementeren zijn alleen van invloed op de resources en eventuele wijzigingen in gegevens worden niet beïnvloed.
- Deze functie wordt alleen ondersteund voor implementaties van de resourcegroep, geen abonnement op implementaties. Zie voor meer informatie over de implementatie voor het niveau van abonnement [maken van resourcegroepen en resources op het abonnementsniveau](./deploy-to-subscription.md).

Als u wilt deze optie gebruikt, moeten uw implementaties unieke namen hebben, zodat ze kunnen worden geïdentificeerd in de geschiedenis. Als u geen unieke namen, overschrijft de huidige mislukte implementatie mogelijk de eerder geslaagde implementatie in de geschiedenis. U kunt deze optie alleen gebruiken met niveau root-implementaties. Implementaties van een geneste sjabloon zijn niet beschikbaar voor opnieuw implementeren.

Toevoegen als u wilt implementeren in de laatste geslaagde implementatie, de `-RollbackToLastDeployment` parameter als een vlag.

```azurepowershell-interactive
New-AzResourceGroupDeployment -Name ExampleDeployment02 `
  -ResourceGroupName $resourceGroupName `
  -TemplateFile c:\MyTemplates\azuredeploy.json `
  -RollbackToLastDeployment
```

Als u wilt implementeren op een specifieke implementatie, gebruikt u de `-RollBackDeploymentName` parameter en geef de naam van de implementatie.

```azurepowershell-interactive
New-AzResourceGroupDeployment -Name ExampleDeployment02 `
  -ResourceGroupName $resourceGroupName `
  -TemplateFile c:\MyTemplates\azuredeploy.json `
  -RollBackDeploymentName ExampleDeployment01
```

De opgegeven implementatie moet zijn geslaagd.

## <a name="pass-parameter-values"></a>Parameterwaarden doorgeven

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
  -TemplateFile c:\MyTemplates\azuredeploy.json `
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

## <a name="test-template-deployments"></a>Sjabloon testimplementaties

U kunt de waarden van uw sjabloon en de parameterbestanden testen zonder daadwerkelijk de resources te implementeren met [Test-AzureRmResourceGroupDeployment](/powershell/module/az.resources/test-azresourcegroupdeployment). 

```powershell
Test-AzResourceGroupDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\azuredeploy.json -storageAccountType Standard_GRS
```

Als er geen fouten worden aangetroffen, wordt de opdracht is voltooid zonder een reactie. Als er een fout wordt gedetecteerd, wordt de opdracht een foutbericht weergegeven. Doorgeven van een onjuiste waarde voor het opslagaccount-SKU, retourneert bijvoorbeeld de volgende fout:

```powershell
Test-AzResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\azuredeploy.json -storageAccountType badSku

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

- Zie voor veilig implementatie uw service in meer dan één regio, [Azure Deployment Manager](deployment-manager-overview.md).
- Als u wilt opgeven voor het verwerken van resources die aanwezig zijn in de resourcegroep, maar niet zijn gedefinieerd in de sjabloon, Zie [Azure Resource Manager-implementatiemodi](deployment-modes.md).
- Zie voor meer informatie over het definiëren van parameters in uw sjabloon, [inzicht in de structuur en de syntaxis van Azure Resource Manager-sjablonen](resource-group-authoring-templates.md).
- Zie voor meer informatie over het implementeren van een sjabloon waarvoor een SAS-token [persoonlijke sjablonen implementeren met SAS-token](resource-manager-powershell-sas-token.md).
