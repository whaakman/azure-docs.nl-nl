---
title: Continu afleveren functie code-updates met behulp van Azure DevOps - Azure Functions
description: Meer informatie over het instellen van een Azure DevOps-pijplijn die is gericht op Azure Functions.
author: ahmedelnably
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: aelnably
ms.openlocfilehash: 0fdad0caa2deef0d7d55b30a85632f72f4ff0ecc
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67594461"
---
# <a name="continuous-delivery-by-using-azure-devops"></a>Continue levering met behulp van Azure DevOps

U kunt de functie automatisch aan een Azure Functions-app implementeren met behulp van [Azure pijplijnen](/azure/devops/pipelines/).

U hebt twee opties voor het definiëren van uw pijplijn:

- **YAML-bestand**: Een YAML-bestand beschrijft de pijplijn. Het bestand is mogelijk een build stappen en een release-sectie. Het YAML-bestand moet zich in dezelfde repo als de app.
- **Sjabloon**: Sjablonen zijn kant-en-taken die het bouwen of implementeren van uw app.

## <a name="yaml-based-pipeline"></a>Op basis van een YAML-pipeline

Voor het maken van een pijplijn op basis van een YAML, eerst uw app te bouwen en implementeer de app vervolgens.

### <a name="build-your-app"></a>Uw app ontwikkelen

Hoe u uw app in Azure-pijplijnen maken, is afhankelijk van de programmeertaal van uw app. Elke taal heeft specifieke build-stappen die een implementatie-artefact maken. Een implementatie-artefact wordt gebruikt om uw functie-app in Azure implementeren.

#### <a name="net"></a>.NET

Gebruik het volgende voorbeeld kunt u een YAML-bestand voor het bouwen van een .NET-app maken:

```yaml
pool:
      vmImage: 'VS2017-Win2016'
steps:
- script: |
    dotnet restore
    dotnet build --configuration Release
- task: DotNetCoreCLI@2
  inputs:
    command: publish
    arguments: '--configuration Release --output publish_output'
    projects: '*.csproj'
    publishWebProjects: false
    modifyOutputPath: true
    zipAfterPublish: false
- task: ArchiveFiles@2
  displayName: "Archive files"
  inputs:
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)/publish_output"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip'
    name: 'drop'
```

#### <a name="javascript"></a>Javascript

Gebruik het volgende voorbeeld kunt u een YAML-bestand voor het bouwen van een JavaScript-app maken:

```yaml
pool:
      vmImage: ubuntu-16.04 # Use 'VS2017-Win2016' if you have Windows native +Node modules
steps:
- bash: |
    if [ -f extensions.csproj ]
    then
        dotnet build extensions.csproj --output ./bin
    fi
    npm install 
    npm run build --if-present
    npm prune --production
- task: ArchiveFiles@2
  displayName: "Archive files"
  inputs:
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip'
    name: 'drop'
```

#### <a name="python"></a>Python

Het volgende voorbeeld kunt u een YAML-bestand voor het bouwen van een Python-app maken. Python wordt alleen ondersteund voor Linux Azure Functions.

```yaml
pool:
      vmImage: ubuntu-16.04
steps:
- task: UsePythonVersion@0
  displayName: "Setting python version to 3.6 as required by functions"
  inputs:
    versionSpec: '3.6'
    architecture: 'x64'
- bash: |
    if [ -f extensions.csproj ]
    then
        dotnet build extensions.csproj --output ./bin
    fi
    python3.6 -m venv worker_venv
    source worker_venv/bin/activate
    pip3.6 install setuptools
    pip3.6 install -r requirements.txt
- task: ArchiveFiles@2
  displayName: "Archive files"
  inputs:
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip'
    name: 'drop'
```
#### <a name="powershell"></a>PowerShell

Het volgende voorbeeld kunt u een YAML-bestand als u een PowerShell-app pakket wilt maken. PowerShell wordt alleen ondersteund voor Windows Azure Functions.

```yaml
pool:
      vmImage: 'VS2017-Win2016'
steps:
- task: ArchiveFiles@2
  displayName: "Archive files"
  inputs:
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip'
    name: 'drop'
```

### <a name="deploy-your-app"></a>Uw app implementeren

U moet een van de volgende YAML-voorbeelden opnemen in uw YAML-bestand, afhankelijk van het besturingssysteem host.

#### <a name="windows-function-app"></a>Functie-app voor Windows

Het volgende codefragment kunt u een Windows-functie-app implementeren:

```yaml
steps:
- task: AzureFunctionApp@1
  inputs:
    azureSubscription: '<Azure service connection>'
    appType: functionApp
    appName: '<Name of function app>'
    #Uncomment the next lines to deploy to a deployment slot
    #deployToSlotOrASE: true
    #resourceGroupName: '<Resource Group Name>'
    #slotName: '<Slot name>'
```

#### <a name="linux-function-app"></a>Linux-functie-app

Het volgende codefragment kunt u een Linux-functie-app implementeren:

```yaml
steps:
- task: AzureFunctionApp@1
  inputs:
    azureSubscription: '<Azure service connection>'
    appType: functionAppLinux
    appName: '<Name of function app>'
    #Uncomment the next lines to deploy to a deployment slot
    #Note that deployment slots is not supported for Linux Dynamic SKU
    #deployToSlotOrASE: true
    #resourceGroupName: '<Resource Group Name>'
    #slotName: '<Slot name>'
```

## <a name="template-based-pipeline"></a>Pijplijn op basis van sjabloon

Sjablonen in Azure DevOps zijn vooraf gedefinieerde groepen van de taken die het bouwen of implementeren van een app.

### <a name="build-your-app"></a>Uw app ontwikkelen

Hoe u uw app in Azure-pijplijnen maken, is afhankelijk van de programmeertaal van uw app. Elke taal heeft specifieke build-stappen die een implementatie-artefact maken. Een implementatie-artefact wordt gebruikt voor het bijwerken van uw functie-app in Azure.

Voor het gebruik van build van ingebouwde sjablonen, wanneer u een nieuwe build-pijplijn maakt, selecteert u **gebruiken de klassieke editor** voor het maken van een pijplijn met behulp van designer sjablonen.

![Selecteer de klassieke Azure-pijplijnen-editor](media/functions-how-to-azure-devops/classic-editor.png)

Nadat u de bron van uw code hebt geconfigureerd, bouw zoeken voor Azure Functions sjablonen. Selecteer de sjabloon die overeenkomt met de taal van uw app.

![Selecteer een sjabloon voor het bouwen van Azure Functions](media/functions-how-to-azure-devops/build-templates.png)

In sommige gevallen hebben build-artefacten de structuur van een specifieke map. Mogelijk moet u selecteert de **Prepend naam hoofdmap voor het archiveren van paden** selectievakje.

![De optie voor het toevoegen van de naam van de hoofdmap](media/functions-how-to-azure-devops/prepend-root-folder.png)

#### <a name="javascript-apps"></a>JavaScript-apps

Als uw JavaScript-app een afhankelijkheid van systeemeigen modules voor Windows heeft, moet u de pool agentversie bijwerken **gehost VS2017**.

![De versie van de pool agent bijwerken](media/functions-how-to-azure-devops/change-agent.png)

### <a name="deploy-your-app"></a>Uw app implementeren

Wanneer u een nieuwe release-pijplijn maakt, kunt u zoeken naar de release-sjabloon van Azure Functions.

![Zoek naar de Azure-Functions release-sjabloon](media/functions-how-to-azure-devops/release-template.png)

Implementeren naar een implementatiesleuf wordt niet ondersteund in de releasesjabloon.

## <a name="create-a-build-pipeline-by-using-the-azure-cli"></a>Een build-pijplijn maken met behulp van de Azure CLI

U kunt een build-pijplijn in Azure maken met de `az functionapp devops-pipeline create` [opdracht](/cli/azure/functionapp/devops-pipeline#az-functionapp-devops-pipeline-create). De build-pijplijn wordt gemaakt om te bouwen en uitbrengen codewijzigingen die zijn aangebracht in uw opslagplaats. De opdracht genereert een nieuw YAML-bestand met de build- en release-pijplijn definieert en deze vervolgens worden doorgevoerd in uw opslagplaats. De vereisten voor deze opdracht is afhankelijk van de locatie van uw code.

- Als uw code in GitHub:

    - U moet hebben **schrijven** machtigingen voor uw abonnement.

    - U moet de projectbeheerder in Azure DevOps.

    - U moet machtigingen voor het maken van een GitHub persoonlijk toegangstoken (PAT) met voldoende machtigingen hebben. Zie voor meer informatie, [vereisten voor installatiemachtiging PAT van GitHub.](https://aka.ms/azure-devops-source-repos)

    - U moet machtigingen hebben voor de master-vertakking in uw GitHub-opslagplaats vastleggen, zodat u de automatisch gegenereerde YAML-bestand kan worden doorgevoerd.

- Als uw code in Azure-opslagplaatsen is:

    - U moet hebben **schrijven** machtigingen voor uw abonnement.

    - U moet de projectbeheerder in Azure DevOps.

## <a name="next-steps"></a>Volgende stappen

- Controleer de [overzicht van Azure Functions](functions-overview.md).
- Controleer de [Azure DevOps-overzicht](/azure/devops/pipelines/).
