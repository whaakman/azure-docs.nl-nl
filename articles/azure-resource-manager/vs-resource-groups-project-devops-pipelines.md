---
title: CI/CD met Azure-pijplijnen en Resource Manager-sjablonen
description: Beschrijft het instellen van continue integratie in Azure pijplijnen met behulp van Azure Resource Group deployment projecten in Visual Studio om Resource Manager-sjablonen te implementeren.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: article
ms.date: 06/12/2019
ms.author: tomfitz
ms.openlocfilehash: b70b38904c0373c53c3731dd0442511116d9c4de
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67191458"
---
# <a name="integrate-resource-manager-templates-with-azure-pipelines"></a>Resource Manager-sjablonen integreren met Azure-pijplijnen

Visual Studio biedt het Azure-resourcegroep-project voor het maken van sjablonen en ze worden geïmplementeerd op uw Azure-abonnement. U kunt dit project integreren met Azure-pijplijnen voor continue integratie en continue implementatie (CI/CD).

Er zijn twee manieren om sjablonen met Azure-pijplijnen te implementeren:

* **Taak met een Azure PowerShell-script toevoegen**. Deze optie heeft het voordeel van de consistentie worden gedurende de levenscyclus van ontwikkeling, omdat u gebruikt hetzelfde script dat is opgenomen in de Visual Studio-project (implementeren-AzureResourceGroup.ps1). De script fasen artefacten van uw project een storage-account dat toegang heeft tot Resource Manager. Artefacten zijn de items in uw project, gekoppelde sjablonen, scripts en binaire waarden van toepassingen. Het script implementeert vervolgens de sjabloon.

* **Taken voor het kopiëren en implementeren van taken toevoegen**. Deze optie biedt een alternatief voor het project-script. U configureren twee taken in de pijplijn. Een taak bereidt de artefacten en de andere taak implementeert u de sjabloon.

In dit artikel bevat beide methoden.

## <a name="prepare-your-project"></a>Voorbereiden van uw project

In dit artikel wordt ervan uitgegaan dat uw Visual Studio-project en Azure DevOps-organisatie gereed zijn voor het maken van de pijplijn. De volgende stappen laten zien hoe om te controleren of dat u klaar bent:

* U hebt een Azure DevOps-organisatie. Als u nog geen hebt, [maakt u er een gratis](/azure/devops/pipelines/get-started/pipelines-sign-up?view=azure-devops). Als uw team al een Azure DevOps-organisatie, zorg ervoor dat u een beheerder bent van het Azure DevOps-project dat u wilt gebruiken.

* U hebt geconfigureerd een [met de service](/azure/devops/pipelines/library/connect-to-azure?view=azure-devops) aan uw Azure-abonnement. De taken in de pijplijn wordt uitgevoerd onder de identiteit van de service-principal. Zie voor stappen voor het maken van de verbinding, [maken van een DevOps-project](resource-manager-tutorial-use-azure-pipelines.md#create-a-devops-project).

* U hebt een Visual Studio-project dat is gemaakt op basis van de **Azure-resourcegroep** starter-sjabloon. Zie voor meer informatie over het maken van dat type project [maken en implementeren van Azure-resourcegroepen met Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

* Visual Studio-project is [verbonden met een Azure DevOps-project](/azure/devops/repos/git/share-your-code-in-git-vs-2017?view=azure-devops).

## <a name="create-pipeline"></a>Pijplijn maken

1. Als u een pijplijn nog niet eerder hebt toegevoegd, moet u een nieuwe pijplijn. Selecteer in uw organisatie Azure DevOps **pijplijnen** en **nieuwe pijplijn**.

   ![Toevoegen van nieuwe pijplijn](./media/vs-resource-groups-project-devops-pipelines/new-pipeline.png)

1. Geef op waar uw code is opgeslagen. De volgende afbeelding ziet u **Azure opslagplaatsen Git**.

   ![Code-bron selecteren](./media/vs-resource-groups-project-devops-pipelines/select-source.png)

1. Selecteer de opslagplaats met de code voor uw project in die bron.

   ![Opslagplaats selecteren](./media/vs-resource-groups-project-devops-pipelines/select-repo.png)

1. Selecteer het type van de pijplijn te maken. U kunt selecteren **Starter pijplijn**.

   ![Selecteer pijplijn](./media/vs-resource-groups-project-devops-pipelines/select-pipeline.png)

U kunt ofwel een taak met Azure PowerShell of het bestand kopiëren toevoegen en implementeren van taken.

## <a name="azure-powershell-task"></a>Azure PowerShell-taak

Deze sectie wordt beschreven hoe u continue implementatie configureren met behulp van een enkele taak die wordt uitgevoerd het PowerShell-script in uw project. Hiermee maakt u de volgende YAML-bestand een [Azure PowerShell-taak](/azure/devops/pipelines/tasks/deploy/azure-powershell?view=azure-devops):

```yaml
pool:
  name: Hosted Windows 2019 with VS2019
  demands: azureps

steps:
- task: AzurePowerShell@3
  inputs:
    azureSubscription: 'demo-deploy-sp'
    ScriptPath: 'AzureResourceGroupDemo/Deploy-AzureResourceGroup.ps1'
    ScriptArguments: -ResourceGroupName 'demogroup' -ResourceGroupLocation 'centralus' 
    azurePowerShellVersion: LatestVersion
```

Als u de taak instelt op `AzurePowerShell@3`, de pijplijn worden opdrachten van de AzureRM-module gebruikt om de verbinding te verifiëren. Het PowerShell-script in de Visual Studio-project maakt standaard gebruik van de AzureRM-module. Als u uw script te gebruiken hebt bijgewerkt de [Az module](/powershell/azure/new-azureps-module-az), de taak ingesteld op `AzurePowerShell@4`.

```yaml
steps:
- task: AzurePowerShell@4
```

Voor `azureSubscription`, geeft u de naam van de serviceverbinding die u hebt gemaakt.

```yaml
inputs:
    azureSubscription: '<your-connection-name>'
```

Voor `scriptPath`, geef het relatieve pad van de pijplijn-bestand voor uw script. U kunt zoeken in uw opslagplaats om te zien van het pad.

```yaml
ScriptPath: '<your-relative-path>/<script-file-name>.ps1'
```

Als u niet te fase artefacten hoeft, geeft de naam en locatie van een resourcegroep te gebruiken voor implementatie. Het Visual Studio-script wordt de resourcegroep gemaakt als deze nog niet bestaat.

```yaml
ScriptArguments: -ResourceGroupName '<resource-group-name>' -ResourceGroupLocation '<location>'
```

Als u fase-artefacten naar een bestaand opslagaccount wilt, gebruikt:

```yaml
ScriptArguments: -ResourceGroupName '<resource-group-name>' -ResourceGroupLocation '<location>' -UploadArtifacts -ArtifactStagingDirectory '$(Build.StagingDirectory)' -StorageAccountName '<your-storage-account>'
```

Nu, dat u hoe u de taak maakt begrijpt, we gaan door de stappen voor het bewerken van de pijplijn.

1. Open uw pijplijn en vervang de inhoud door uw YAML:

   ```yaml
   pool:
     name: Hosted Windows 2019 with VS2019
     demands: azureps

   steps:
   - task: AzurePowerShell@3
     inputs:
       azureSubscription: 'demo-deploy-sp'
       ScriptPath: 'AzureResourceGroupDemo/Deploy-AzureResourceGroup.ps1'
       ScriptArguments: -ResourceGroupName 'demogroup' -ResourceGroupLocation 'centralus' -UploadArtifacts -ArtifactStagingDirectory '$(Build.StagingDirectory)' -StorageAccountName 'stage3a4176e058d34bb88cc'
       azurePowerShellVersion: LatestVersion
   ```

1. Selecteer **Opslaan**.

   ![Pijplijn opslaan](./media/vs-resource-groups-project-devops-pipelines/save-pipeline.png)

1. Voeg een bericht voor het doorvoeren en wijzigingen rechtstreeks doorvoeren **master**.

1. Wanneer u selecteert **opslaan**, de build-pijplijn wordt automatisch uitgevoerd. Ga terug naar de samenvatting van uw build-pijplijn en bekijk de status.

   ![Resultaten weergeven](./media/vs-resource-groups-project-devops-pipelines/view-results.png)

U kunt de pijplijn die momenteel worden uitgevoerd om details over de taken te bekijken. Wanneer deze is voltooid, ziet u de resultaten voor elke stap.

## <a name="copy-and-deploy-tasks"></a>Kopieer en implementeren van taken

Deze sectie wordt beschreven hoe u continue implementatie configureren met behulp van een twee taken om te zetten van de artefacten en de sjabloon te implementeren. 

De volgende YAML-bevat de [kopieertaak van Azure bestand](/azure/devops/pipelines/tasks/deploy/azure-file-copy?view=azure-devops):

```yaml
- task: AzureFileCopy@3
  displayName: 'Stage files'
  inputs:
    SourcePath: 'AzureResourceGroup1'
    azureSubscription: 'demo-deploy-sp'
    Destination: 'AzureBlob'
    storage: 'stage3a4176e058d34bb88cc'
    ContainerName: 'democontainer'
    outputStorageUri: 'artifactsLocation'
    outputStorageContainerSasToken: 'artifactsLocationSasToken'
    sasTokenTimeOutInMinutes: '240'
```

Er zijn verschillende onderdelen van deze taak te wijzigen voor uw omgeving. De `SourcePath` geeft de locatie van de artefacten ten opzichte van de pijplijn-bestand. In dit voorbeeld worden de bestanden bestaan in een map met de naam `AzureResourceGroup1` waarvan de naam van het project is.

```yaml
SourcePath: '<path-to-artifacts>'
```

Voor `azureSubscription`, geeft u de naam van de serviceverbinding die u hebt gemaakt.

```yaml
azureSubscription: '<your-connection-name>'
```

Voor opslag en de container en geeft u de namen van de storage-account en een container die u gebruiken wilt voor het opslaan van de artefacten. Het opslagaccount moet bestaan.

```yaml
storage: '<your-storage-account-name>'
ContainerName: '<container-name>'
```

De volgende YAML-bevat de [taak voor implementatie van Azure resource group](/azure/devops/pipelines/tasks/deploy/azure-resource-group-deployment?view=azure-devops):

```yaml
- task: AzureResourceGroupDeployment@2
  displayName: 'Deploy template'
  inputs:
    azureSubscription: 'demo-deploy-sp'
    resourceGroupName: 'demogroup'
    location: 'centralus'
    templateLocation: 'URL of the file'
    csmFileLink: '$(artifactsLocation)WebSite.json$(artifactsLocationSasToken)'
    csmParametersFileLink: '$(artifactsLocation)WebSite.parameters.json$(artifactsLocationSasToken)'
    overrideParameters: '-_artifactsLocation $(artifactsLocation) -_artifactsLocationSasToken "$(artifactsLocationSasToken)"'
```

Er zijn verschillende onderdelen van deze taak te wijzigen voor uw omgeving. Voor `azureSubscription`, geeft u de naam van de serviceverbinding die u hebt gemaakt.

```yaml
azureSubscription: '<your-connection-name>'
```

Voor `resourceGroupName` en `location`, geef de naam en locatie van de resourcegroep die u implementeren wilt op. De taak wordt de resourcegroep gemaakt als deze nog niet bestaat.

```yaml
resourceGroupName: '<resource-group-name>'
location: '<location>'
```

De implementatie van taak is gekoppeld aan een sjabloon met de naam `WebSite.json` en een parameterbestand met de naam WebSite.parameters.json. Gebruik de namen van uw sjabloon en de parameter-bestanden.

Nu, dat u hoe u de taken maakt begrijpt, we gaan door de stappen voor het bewerken van de pijplijn.

1. Open uw pijplijn en vervang de inhoud door uw YAML:

   ```yaml
   pool:
     name: Hosted Windows 2019 with VS2019

   steps:
   - task: AzureFileCopy@3
     displayName: 'Stage files'
     inputs:
       SourcePath: 'AzureResourceGroup1'
       azureSubscription: 'demo-deploy-sp'
       Destination: 'AzureBlob'
       storage: 'stage3a4176e058d34bb88cc'
       ContainerName: 'democontainer'
       outputStorageUri: 'artifactsLocation'
       outputStorageContainerSasToken: 'artifactsLocationSasToken'
       sasTokenTimeOutInMinutes: '240'
   - task: AzureResourceGroupDeployment@2
     displayName: 'Deploy template'
     inputs:
       azureSubscription: 'demo-deploy-sp'
       resourceGroupName: demogroup
       location: 'centralus'
       templateLocation: 'URL of the file'
       csmFileLink: '$(artifactsLocation)WebSite.json$(artifactsLocationSasToken)'
       csmParametersFileLink: '$(artifactsLocation)WebSite.parameters.json$(artifactsLocationSasToken)'
       overrideParameters: '-_artifactsLocation $(artifactsLocation) -_artifactsLocationSasToken "$(artifactsLocationSasToken)"'
   ```

1. Selecteer **Opslaan**.

1. Voeg een bericht voor het doorvoeren en wijzigingen rechtstreeks doorvoeren **master**.

1. Wanneer u selecteert **opslaan**, de build-pijplijn wordt automatisch uitgevoerd. Ga terug naar de samenvatting van uw build-pijplijn en bekijk de status.

   ![Resultaten weergeven](./media/vs-resource-groups-project-devops-pipelines/view-results.png)

U kunt de pijplijn die momenteel worden uitgevoerd om details over de taken te bekijken. Wanneer deze is voltooid, ziet u de resultaten voor elke stap.

## <a name="next-steps"></a>Volgende stappen

Zie voor stapsgewijze procedure over het gebruik van Azure-pijplijnen met Resource Manager-sjablonen [zelfstudie: Continue integratie van Azure Resource Manager-sjablonen met Azure-pijplijnen](resource-manager-tutorial-use-azure-pipelines.md).
