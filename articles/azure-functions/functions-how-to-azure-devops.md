---
title: Continu afleveren functie code-updates met behulp van Azure DevOps
description: Meer informatie over het instellen van een Azure DevOps-pijplijn die gericht is op Azure Functions.
author: ahmedelnably
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: aelnably
ms.custom: ''
ms.openlocfilehash: 86f1c36bd5f972a6ebd573019a22b0c0d07dc480
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64928090"
---
# <a name="continuous-delivery-using-azure-devops"></a>Continue levering met behulp van Azure DevOps

U kunt de functie automatisch implementeren op een Azure-functie-app met [Azure pijplijnen](/azure/devops/pipelines/).
Als u uw pijplijn definieert, kunt u het volgende gebruiken:

- YAML-bestand: Dit bestand beschrijft de pijplijn, heeft deze mogelijk een gedeelte van de stappen build, en een sectie voor versie. Het YAML-bestand moet zich in dezelfde repo als de app.

- Sjablonen: Sjablonen zijn gereed taken die het bouwen of implementeren van uw app gemaakt.

## <a name="yaml-based-pipeline"></a>Op basis van een YAML-pipeline

### <a name="build-your-app"></a>Uw app ontwikkelen

Het bouwen van uw app in Azure pijplijnen, is afhankelijk van de programmeertaal van uw app.
Elke taal heeft specifieke build-stappen voor het maken van een artefact voor implementatie, die kan worden gebruikt om uw functie-app in Azure implementeren.

#### <a name="net"></a>.NET

Het volgende voorbeeld kunt u uw YAML-bestand voor het bouwen van uw .NET-app maken.

```yaml
jobs:
  - job: Build
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
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)/publish_output/s"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/$(Build.BuildId).zip'
    name: 'drop'
```

#### <a name="javascript"></a>Javascript

Gebruik het volgende voorbeeld kunt u uw YAML-bestand voor het bouwen van uw JavaScript-app maken:

```yaml
jobs:
  - job: Build
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
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)/publish_output/s"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/$(Build.BuildId).zip'
    name: 'drop'
```

#### <a name="python"></a>Python

Gebruik het volgende voorbeeld kunt u uw YAML-bestand voor het bouwen van uw Python-app maken, Python, wordt alleen ondersteund voor Linux Azure Functions:

```yaml
jobs:
  - job: Build
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
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)/publish_output/s"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/$(Build.BuildId).zip'
    name: 'drop'
```

### <a name="deploy-your-app"></a>Uw app implementeren

Afhankelijk van het besturingssysteem host moet u de volgende YAML-voorbeeld opnemen in uw YAML-bestand.

#### <a name="windows-function-app"></a>Windows-functie App

Het volgende codefragment kan worden gebruikt om te implementeren op een Windows-functie-app

```yaml
steps:
- task: AzureFunctionApp@1
  inputs:
    azureSubscription: '<Azure service connection>'
    appType: functionApp
    appName: '<Name of function app>'
```

#### <a name="linux-function-app"></a>Linux-functie App

Het volgende codefragment kan worden gebruikt om te implementeren op een Linux-functie-app

```yaml
steps:
- task: AzureFunctionApp@1
  inputs:
    azureSubscription: '<Azure service connection>'
    appType: functionAppLinux
    appName: '<Name of function app>'
```

## <a name="template-based-pipeline"></a>Pijplijn op basis van sjabloon

Sjablonen in Azure DevOps, zijn vooraf gedefinieerde groep met taken die het bouwen of implementeren van een app.

### <a name="build-your-app"></a>Uw app ontwikkelen

Het bouwen van uw app in Azure pijplijnen, is afhankelijk van de programmeertaal van uw app. Elke taal heeft specifieke build-stappen voor het maken van een artefact voor implementatie, die kan worden gebruikt voor het bijwerken van uw functie-app in Azure.
Kies voor het gebruik van de build van ingebouwde sjablonen, bij het maken van een nieuwe build-pijplijn, **gebruiken de klassieke editor** om een pijplijn met behulp van de designer sjablonen te maken

![Azure pijplijnen klassieke editor](media/functions-how-to-azure-devops/classic-editor.png)

Na het configureren van de bron van uw code zoeken voor Azure Functions-sjablonen maken en kies de sjabloon die overeenkomt met de taal van uw app.

![Azure Functions bouwen sjablonen](media/functions-how-to-azure-devops/build-templates.png)

#### <a name="javascript-apps"></a>JavaScript-apps

Als uw JavaScript-app hebt een afhankelijkheid op systeemeigen modules voor Windows, moet u om bij te werken:

- De versie van de Agentpool **VS2017 die worden gehost**

  ![Besturingssysteem van de Build-Agent wijzigen](media/functions-how-to-azure-devops/change-agent.png)

- Het script in de **extensies bouwen** stap in de sjabloon `IF EXIST *.csproj dotnet build extensions.csproj --output ./bin`

  ![Script wijzigen](media/functions-how-to-azure-devops/change-script.png)

### <a name="deploy-your-app"></a>Uw app implementeren

Bij het maken van een nieuwe release-pijplijn, zoekt u Azure Functions-release-sjabloon.

![](media/functions-how-to-azure-devops/release-template.png)

## <a name="creating-an-azure-pipeline-using-the-azure-cli"></a>Het maken van een Azure-pijplijn met de Azure CLI

Met behulp van de `az functionapp devops-pipeline create` [opdracht](/cli/azure/functionapp/devops-pipeline#az-functionapp-devops-pipeline-create), een Azure-pijplijn gemaakt voor het bouwen en release codewijzigingen in uw opslagplaats. De opdracht wordt een nieuw YAML-bestand met de build- en release-pijplijn definieert genereren en deze door te voeren op uw opslagplaats.
De vereisten voor deze opdracht is afhankelijk van de locatie van uw code:

- Als uw code in GitHub:

    - U moet beschikken over **schrijven** machtiging aan uw abonnement.

    - Bent u de projectbeheerder in Azure DevOps.

    - U hebt de machtiging voor het maken van een GitHub persoonlijk toegangstoken met de juiste machtigingen. [Vereisten voor Installatiemachtiging van GitHub PAT.](https://aka.ms/azure-devops-source-repos)

    - U gemachtigd bent om door te voeren naar de hoofdvertakking in uw GitHub-opslagplaats om door te voeren van de automatisch gegenereerde YAML-bestand.

- Als uw code in Azure-opslagplaatsen is:

    - U moet beschikken over **schrijven** machtiging aan uw abonnement.

    - Bent u de projectbeheerder in Azure DevOps.

## <a name="next-steps"></a>Volgende stappen

+ [Overzicht van Azure Functions](functions-overview.md)
+ [Overzicht van Azure DevOps](/azure/devops/pipelines/)
