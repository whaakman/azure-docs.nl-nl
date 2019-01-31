---
Titel: Studio-werkruimte met Azure Resource Manager titleSuffix implementeren: Azure Machine Learning Studio description: Over het implementeren van een werkruimte voor Azure Machine Learning met behulp van Azure Resource Manager-sjabloon-services: machine learning ms.service: machine learning ms.subservice: studio ms.topic: artikel

Auteur: ericlicoding ms.author: amlstudiodocs ms.custom: seodec18 ms.date: 02/05/2018
---

# <a name="deploy-azure-machine-learning-studio-workspace-using-azure-resource-manager"></a>Azure Machine Learning Studio-werkruimte met behulp van Azure Resource Manager implementeren

Met behulp van een Azure Resource Manager-sjabloon voor de implementatie u tijd bespaart doordat u een schaalbare manier om te implementeren met elkaar verbonden onderdelen met een validatie en mechanisme voor opnieuw proberen. Als u Azure Machine Learning-werkruimtes instelt, bijvoorbeeld, moet u een Azure storage-account voor het eerst configureert en implementeert u uw werkruimte. Stel dit handmatig doen voor honderden werkruimten. Eenvoudiger alternatief is het gebruik van een Azure Resource Manager-sjabloon om een Azure Machine Learning-werkruimte en alle afhankelijkheden ervan te implementeren. In dit artikel gaat u door dit stapsgewijze proces. Zie voor een goed overzicht van Azure Resource Manager [overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

## <a name="step-by-step-create-a-machine-learning-workspace"></a>Stap voor stap: een Machine Learning-werkruimte maken
Er wordt een Azure-resourcegroep maken en implementeren van een nieuw Azure storage-account en een nieuwe Azure Machine Learning-werkruimte met behulp van Resource Manager-sjabloon. Zodra de implementatie voltooid is, wordt er belangrijke informatie over de werkruimten die zijn gemaakt (de primaire sleutel, de werkruimte-id en de URL van de werkruimte) afgedrukt.

### <a name="create-an-azure-resource-manager-template"></a>Een Azure Resource Manager-sjabloon maken
Een Machine Learning-werkruimte is vereist voor het opslaan van de gegevensset die is gekoppeld aan het Azure storage-account.
De volgende sjabloon maakt gebruik van de naam van de resourcegroep voor het genereren van de naam van het opslagaccount en de naam van de werkruimte.  Gebruikt ook de naam van opslagaccount als een eigenschap bij het maken van de werkruimte.

```
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "variables": {
        "namePrefix": "[resourceGroup().name]",
        "location": "[resourceGroup().location]",
        "mlVersion": "2016-04-01",
        "stgVersion": "2015-06-15",
        "storageAccountName": "[concat(variables('namePrefix'),'stg')]",
        "mlWorkspaceName": "[concat(variables('namePrefix'),'mlwk')]",
        "mlResourceId": "[resourceId('Microsoft.MachineLearning/workspaces', variables('mlWorkspaceName'))]",
        "stgResourceId": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "storageAccountType": "Standard_LRS"
    },
    "resources": [
        {
            "apiVersion": "[variables('stgVersion')]",
            "name": "[variables('storageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[variables('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "apiVersion": "[variables('mlVersion')]",
            "type": "Microsoft.MachineLearning/workspaces",
            "name": "[variables('mlWorkspaceName')]",
            "location": "[variables('location')]",
            "dependsOn": ["[variables('stgResourceId')]"],
            "properties": {
                "UserStorageAccountId": "[variables('stgResourceId')]"
            }
        }
    ],
    "outputs": {
        "mlWorkspaceObject": {"type": "object", "value": "[reference(variables('mlResourceId'), variables('mlVersion'))]"},
        "mlWorkspaceToken": {"type": "string", "value": "[listWorkspaceKeys(variables('mlResourceId'), variables('mlVersion')).primaryToken]"},
        "mlWorkspaceWorkspaceID": {"type": "string", "value": "[reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId]"},
        "mlWorkspaceWorkspaceLink": {"type": "string", "value": "[concat('https://studio.azureml.net/Home/ViewWorkspace/', reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId)]"}
    }
}

```
Deze sjabloon opslaan als mlworkspace.json bestand onder c:\temp\.

### <a name="deploy-the-resource-group-based-on-the-template"></a>De resourcegroep, op basis van de sjabloon implementeren
* Open PowerShell.
* Modules voor Azure Resource Manager en Azure Service Management installeren

```
# Install the Azure Resource Manager modules from the PowerShell Gallery (press “A”)
Install-Module AzureRM -Scope CurrentUser

# Install the Azure Service Management modules from the PowerShell Gallery (press “A”)
Install-Module Azure -Scope CurrentUser
```

   Deze stappen downloadt en installeert de modules die nodig zijn voor de resterende stappen uitvoeren. Dit moet slechts één keer worden uitgevoerd in de omgeving waarin u de PowerShell-opdrachten worden uitgevoerd.

* Verificatie op Azure

```
# Authenticate (enter your credentials in the pop-up window)
Connect-AzureRmAccount
```
Deze stap moet worden herhaald voor elke sessie. Eenmaal is geverifieerd, moet uw abonnementsgegevens worden weergegeven.

![Azure Account][1]

Nu we hebben toegang tot Azure, kunnen we de resourcegroep maken.

* Een resourcegroep maken

```
$rg = New-AzureRmResourceGroup -Name "uniquenamerequired523" -Location "South Central US"
$rg
```

Controleer of dat de resourcegroep correct is ingericht. **ProvisioningState** moet 'geslaagd zijn."
Naam van de resourcegroep wordt gebruikt door de sjabloon voor het genereren van de naam van het opslagaccount. De opslagaccountnaam moet tussen 3 en 24 tekens lang zijn en cijfers en kleine letters bevatten.

![Resource Group][2]

* Met behulp van de implementatie van resourcegroep, een nieuwe Machine Learning-werkruimte implementeren.

```
# Create a Resource Group, TemplateFile is the location of the JSON template.
$rgd = New-AzureRmResourceGroupDeployment -Name "demo" -TemplateFile "C:\temp\mlworkspace.json" -ResourceGroupName $rg.ResourceGroupName
```

Zodra de implementatie is voltooid, is het eenvoudig om de eigenschappen van de werkruimte die u hebt geïmplementeerd. Bijvoorbeeld, u kunt toegang tot de primaire sleutel Token.

```
# Access Azure Machine Learning studio Workspace Token after its deployment.
$rgd.Outputs.mlWorkspaceToken.Value
```

Een andere manier om het ophalen van tokens van bestaande werkruimte is het gebruik van de opdracht Invoke-AzureRmResourceAction. Bijvoorbeeld, kunt u de primaire en secundaire tokens van alle werkruimten weergeven.

```
# List the primary and secondary tokens of all workspaces
Get-AzureRmResource |? { $_.ResourceType -Like "*MachineLearning/workspaces*"} |% { Invoke-AzureRmResourceAction -ResourceId $_.ResourceId -Action listworkspacekeys -Force}
```
Nadat de werkruimte is ingericht, kunt u ook automatiseren veel Azure Machine Learning Studio-taken met behulp van de [PowerShell-Module voor Azure Machine Learning](https://aka.ms/amlps).

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Azure Resource Manager-sjablonen](../../azure-resource-manager/resource-group-authoring-templates.md).
* Bekijk de [opslagplaats voor Azure Quickstart-sjablonen](https://github.com/Azure/azure-quickstart-templates).
* Bekijk deze video over [Azure Resource Manager](https://channel9.msdn.com/Events/Ignite/2015/C9-39).
* Zie de [verwijzing help voor Resource Manager-sjabloon](https://docs.microsoft.com/azure/templates/microsoft.machinelearning/allversions) 
 <!--Image references--> [1]: [2]./media/deploy-with-resource-manager-template/azuresubscription.png: ./media/deploy-with-resource-manager-template/ resourcegroupprovisioning.PNG


<!--Link references-->
