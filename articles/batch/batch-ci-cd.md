---
title: Gebruik Azure-pijplijnen te bouwen en implementeren van oplossingen voor HPC - Azure Batch | Microsoft Docs
description: Informatie over het implementeren van een build en release-pijplijn voor een HPC-toepassing die wordt uitgevoerd op Azure Batch.
author: christianreddington
ms.author: chredd
ms.date: 03/28/2019
ms.topic: conceptual
services: batch
ms.openlocfilehash: 1e3b2d42ae390b9eb755e568b82fc1a90e0eae6a
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58806887"
---
# <a name="use-azure-pipelines-to-build-and-deploy-hpc-solutions"></a>Azure-pijplijnen te bouwen en implementeren van HPC-oplossingen gebruiken

Azure DevOps-services bieden een scala aan hulpprogramma's die worden gebruikt door ontwikkelteams bij het bouwen van een aangepaste toepassing. Hulpprogramma's van Azure DevOps kunnen vertalen in geautomatiseerde het ontwikkelen en testen van compute-oplossingen voor hoge prestaties. In dit artikel laat zien hoe u een continue integratie (CI) en continue implementatie (CD) met behulp van die Azure-pijplijnen voor een krachtige oplossing die zijn geïmplementeerd op Azure Batch compute instelt.

Azure pijplijnen biedt een scala aan moderne CI/CD-processen voor het bouwen, implementeren, testen en controlesoftware. Deze processen Versnel de levering van software, zodat u kunt zich richten op uw code in plaats van ondersteuning voor de infrastructuur en bewerkingen.

## <a name="create-an-azure-pipeline"></a>Een Azure-pijplijn

In dit voorbeeld wordt een build maakt en release-pijplijn naar een Azure Batch-infrastructuur implementeren en uitbrengen van een toepassingspakket. Ervan uitgaande dat de code lokaal wordt ontwikkeld, is dit de algemene implementatie-stroom:

![Diagram van de stroom van de implementatie in onze pijplijn](media/batch-ci-cd/DeploymentFlow.png)

### <a name="setup"></a>Instellen

Als u wilt volgen de stappen in dit artikel, moet u een Azure DevOps-organisatie en een teamproject.

* [Maak een Azure DevOps-organisatie](https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization?view=azure-devops)
* [Een project maken in Azure DevOps](https://docs.microsoft.com/azure/devops/organizations/projects/create-project?view=azure-devops)

### <a name="source-control-for-your-environment"></a>Broncodebeheer voor uw omgeving

Broncodebeheer kan teams voor het bijhouden van wijzigingen in de codebasis en eerdere versies van de code controleren.

Broncodebeheer is normaal gesproken hand in hand met softwarecode beschouwd. Over de onderliggende infrastructuur? Dit brengt ons infrastructuur als Code, waarbij we Azure Resource Manager-sjablonen of andere open-source-alternatieven gebruiken zal om te declaratief definiëren de onderliggende infrastructuur.

In dit voorbeeld is sterk afhankelijk van een getal van Resource Manager-sjablonen (JSON-documenten) en de bestaande binaire bestanden. U kunt deze voorbeelden kopieert in uw opslagplaats en push ze naar Azure DevOps.

De structuur van de codebase gebruikt in dit voorbeeld lijkt op het volgende:

* Een **arm-sjablonen** map, met een aantal Azure Resource Manager-sjablonen. De sjablonen worden beschreven in dit artikel.
* Een **-clienttoepassing** map, namelijk een kopie van de [Azure Batch .NET-bestand verwerken met ffmpeg](https://github.com/Azure-Samples/batch-dotnet-ffmpeg-tutorial) voorbeeld. Dit is niet nodig voor dit artikel.
* Een **hpc-toepassing** map, de Windows 64-bits versie van [ffmpeg 3.4](https://ffmpeg.zeranoe.com/builds/win64/static/ffmpeg-3.4-win64-static.zip).
* Een **pijplijnen** map. Dit document bevat een overzicht van onze bouwproces YAML-bestand. Dit wordt besproken in het artikel.

In deze sectie wordt ervan uitgegaan dat u bekend bent met versie beheer- en ontwerpen Resource Manager-sjablonen. Als u niet bekend bent met deze begrippen, Zie de volgende pagina's voor meer informatie.

* [Wat is broncodebeheer?](https://docs.microsoft.com/azure/devops/user-guide/source-control?view=azure-devops)
* [Informatie over de structuur en de syntaxis van Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-authoring-templates.md)

#### <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen

In dit voorbeeld maakt gebruik van meerdere Resource Manager-sjablonen voor het implementeren van onze oplossing. Om dit te doen, gebruiken we een aantal mogelijkheden sjablonen (vergelijkbaar met eenheden of modules) die een specifiek stukje functionaliteit implementeren. We gebruiken ook een voor end-to-end-oplossingssjabloon die verantwoordelijk is voor deze onderliggende mogelijkheden samen te brengen. Er zijn een aantal voordelen van deze benadering:

* De onderliggende mogelijkheid sjablonen kunnen afzonderlijk worden eenheid getest.
* De onderliggende mogelijkheid-sjablonen kunnen worden gedefinieerd als een standaard binnen een organisatie en opnieuw worden gebruikt in meerdere oplossingen.

In dit voorbeeld is er een voor end-to-end-oplossingssjabloon (deployment.json) die drie sjablonen wordt geïmplementeerd. De onderliggende sjablonen zijn mogelijkheid sjablonen, die verantwoordelijk is voor het implementeren van een bepaald aspect van de oplossing.

![Voorbeeld van een gekoppelde sjabloonstructuur met behulp van Azure Resource Manager-sjablonen](media/batch-ci-cd/ARMTemplateHierarchy.png)

De eerste sjabloon die in dit artikel is voor een Azure Storage-Account. Onze oplossing vereist een storage-account om de toepassing aan de Batch-Account te implementeren. Het verdient aanbeveling wordt op de hoogte van de [referentiegids van Resource Manager-sjabloon voor Microsoft.Storage-resourcetypen](https://docs.microsoft.com/azure/templates/microsoft.storage/allversions) bij het bouwen van Resource Manager-sjablonen voor Storage-Accounts.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "accountName": {
            "type": "string",
            "metadata": {
                 "description": "Name of the Azure Storage Account"
             }
         }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('accountName')]",
            "sku": {
                "name": "Standard_LRS"
            },
            "apiVersion": "2018-02-01",
            "location": "[resourceGroup().location]",
            "properties": {}
        }
    ],
    "outputs": {
        "blobEndpoint": {
          "type": "string",
          "value": "[reference(resourceId('Microsoft.Storage/storageAccounts', parameters('accountName'))).primaryEndpoints.blob]"
        },
        "resourceId": {
          "type": "string",
          "value": "[resourceId('Microsoft.Storage/storageAccounts', parameters('accountName'))]"
        }
    }
}
```

Vervolgens gaan we de Batch-Account voor Azure-sjabloon. De Azure Batch-Account fungeert als een platform voor het uitvoeren van meerdere toepassingen in pools (groeperingen van machines). Het verdient aanbeveling wordt op de hoogte van de [referentiegids van Resource Manager-sjabloon voor resourcetypen Microsoft.Batch](https://docs.microsoft.com/azure/templates/microsoft.batch/allversions) bij het bouwen van Resource Manager-sjablonen voor Batch-Accounts.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "batchAccountName": {
           "type": "string",
           "metadata": {
                "description": "Name of the Azure Batch Account"
            }
        },
        "storageAccountId": {
           "type": "string",
           "metadata": {
                "description": "ID of the Azure Storage Account"
            }
        }
    },
    "variables": {},
    "resources": [
        {
            "name": "[parameters('batchAccountName')]",
            "type": "Microsoft.Batch/batchAccounts",
            "apiVersion": "2017-09-01",
            "location": "[resourceGroup().location]",
            "properties": {
              "poolAllocationMode": "BatchService",
              "autoStorage": {
                  "storageAccountId": "[parameters('storageAccountId')]"
              }
            }
          }
    ],
    "outputs": {}
}
```

De volgende sjabloon toont een voorbeeld het maken van een Azure Batch-Pool (de back-end-machines voor het verwerken van onze toepassingen). Het verdient aanbeveling wordt op de hoogte van de [referentiegids van Resource Manager-sjabloon voor resourcetypen Microsoft.Batch](https://docs.microsoft.com/azure/templates/microsoft.batch/allversions) bij het bouwen van Resource Manager-sjablonen voor Batch-Account-Pools.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "batchAccountName": {
           "type": "string",
           "metadata": {
                "description": "Name of the Azure Batch Account"
           }
        },
        "batchAccountPoolName": {
            "type": "string",
            "metadata": {
                 "description": "Name of the Azure Batch Account Pool"
             }
         }
    },
    "variables": {},
    "resources": [
        {
            "name": "[concat(parameters('batchAccountName'),'/', parameters('batchAccountPoolName'))]",
            "type": "Microsoft.Batch/batchAccounts/pools",
            "apiVersion": "2017-09-01",
            "properties": {
                "deploymentConfiguration": {
                    "virtualMachineConfiguration": {
                        "imageReference": {
                            "publisher": "Canonical",
                            "offer": "UbuntuServer",
                            "sku": "18.04-LTS",
                            "version": "latest"
                        },
                        "nodeAgentSkuId": "batch.node.ubuntu 18.04"
                    }
                },
                "vmSize": "Standard_D1_v2"
            }
          }
    ],
    "outputs": {}
}
```

Tot slot hebben we een sjabloon die lijkt op een orchestrator fungeert. Deze sjabloon is verantwoordelijk voor het implementeren van de mogelijkheid-sjablonen.

U kunt ook meer informatie over [gekoppelde Azure Resource Manager-sjablonen maken](../azure-resource-manager/resource-manager-tutorial-create-linked-templates.md) in een apart artikel.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "templateContainerUri": {
           "type": "string",
           "metadata": {
                "description": "URI of the Blob Storage Container containing the Azure Resouce Manager templates"
            }
        },
        "templateContainerSasToken": {
           "type": "string",
           "metadata": {
                "description": "The SAS token of the container containing the Azure Resouce Manager templates"
            }
        },
        "applicationStorageAccountName": {
            "type": "string",
            "metadata": {
                 "description": "Name of the Azure Storage Account"
            }
         },
        "batchAccountName": {
            "type": "string",
            "metadata": {
                 "description": "Name of the Azure Batch Account"
            }
         },
         "batchAccountPoolName": {
             "type": "string",
             "metadata": {
                  "description": "Name of the Azure Batch Account Pool"
              }
          }
    },
    "variables": {},
    "resources": [
        {
            "apiVersion": "2017-05-10",
            "name": "storageAccountDeployment",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                  "uri": "[concat(parameters('templateContainerUri'), '/storageAccount.json', parameters('templateContainerSasToken'))]",
                  "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "accountName": {"value": "[parameters('applicationStorageAccountName')]"}
                }
            }
        },  
        {
            "apiVersion": "2017-05-10",
            "name": "batchAccountDeployment",
            "type": "Microsoft.Resources/deployments",
            "dependsOn": [
                "storageAccountDeployment"
            ],
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                  "uri": "[concat(parameters('templateContainerUri'), '/batchAccount.json', parameters('templateContainerSasToken'))]",
                  "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "batchAccountName": {"value": "[parameters('batchAccountName')]"},
                    "storageAccountId": {"value": "[reference('storageAccountDeployment').outputs.resourceId.value]"}
                }
            }
        },
        {
            "apiVersion": "2017-05-10",
            "name": "poolDeployment",
            "type": "Microsoft.Resources/deployments",
            "dependsOn": [
                "batchAccountDeployment"
            ],
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                  "uri": "[concat(parameters('templateContainerUri'), '/batchAccountPool.json', parameters('templateContainerSasToken'))]",
                  "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "batchAccountName": {"value": "[parameters('batchAccountName')]"},
                    "batchAccountPoolName": {"value": "[parameters('batchAccountPoolName')]"}
                }
            }
        }
    ],
    "outputs": {}
}
```

#### <a name="the-hpc-solution"></a>De HPC-oplossing

De infrastructuur en software kunnen worden gedefinieerd als code en geplaatst in dezelfde opslagplaats.

De ffmpeg wordt voor deze oplossing gebruikt als het toepassingspakket. Het ffmpeg-pakket kan worden gedownload [hier](https://ffmpeg.zeranoe.com/builds/win64/static/ffmpeg-3.4-win64-static.zip).

![Voorbeeld van de Git-opslagplaats structuur](media/batch-ci-cd/git-repository.jpg)

Er zijn vier belangrijkste secties in deze opslagplaats:

* De **arm-sjablonen** map waarin onze infrastructuur als Code
* De **hpc-toepassing** map met de binaire bestanden voor ffmpeg
* De **pijplijnen** map met de definitie voor de build-pijplijn.
* **Optioneel**: De **-clienttoepassing** map waarin code van .NET-toepassing. We doen dit niet gebruiken in het voorbeeld, maar in uw eigen project, u kunt desgewenst voor het uitvoeren van de uitvoeringen van de HPC-Batch-toepassing via een clienttoepassing.

> [!NOTE]
> Dit is slechts één voorbeeld van een structuur voor een codebase. Deze methode wordt gebruikt voor de toepassing aan te tonen dat de toepassing, infrastructuur en pijplijncode zijn opgeslagen in dezelfde opslagplaats.

Nu de broncode is ingesteld, kunnen we de eerste build gaan.

## <a name="continuous-integration"></a>Continue integratie

[Azure pijplijnen](https://docs.microsoft.com/azure/devops/pipelines/get-started/?view=azure-devops), binnen Azure DevOps-Services, helpt u bij het implementeren van een pijplijn bouwen, testen en implementeren voor uw toepassingen.

In deze fase van uw pijplijn, zijn meestal tests uitgevoerd voor het valideren van code en maken van de betreffende onderdelen van de software. Het aantal en typen van tests en eventuele extra taken die u uitvoert zal afhankelijk zijn van uw bredere build en strategie voor de release.

## <a name="preparing-the-hpc-application"></a>Voorbereiden van de HPC-toepassing

In dit voorbeeld wordt de nadruk op de **hpc-toepassing** map. De **hpc-toepassing** map is de ffmpeg-software die wordt uitvoeren in de Azure Batch-account.

1. Navigeer naar de sectie Builds van Azure-pijplijnen in uw organisatie Azure DevOps. Maak een **nieuwe pijplijn**.

    ![Een nieuwe Build-pijplijn](media/batch-ci-cd/new-build-pipeline.jpg)

1. U hebt twee opties om een Build-pijplijn te maken:

    a. [Met behulp van de visuele ontwerpfunctie](https://docs.microsoft.com/azure/devops/pipelines/get-started-designer?view=azure-devops&tabs=new-nav). Om dit te gebruiken, klikt u op "Gebruiken de visuele ontwerpfunctie" in de **nieuwe pijplijn** pagina.

    b. [Met behulp van YAML bouwt](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml?view=azure-devops). U kunt een nieuwe YAML-pijplijn maken door te klikken op de GitHub-optie op de nieuwe pagina in de pijplijn of Azure-opslagplaatsen. U kunt ook het voorbeeld hieronder opslaan in uw broncodebeheer en verwijzen naar een bestaand YAML-bestand door te klikken op de visuele ontwerper, en vervolgens de sjabloon YAML.

    ```yml
    # To publish an application into Azure Batch, we need to
    # first zip the file, and then publish an artifact, so that
    # we can take the necessary steps in our release pipeline.
    steps:
    # First, we Zip up the files required in the Batch Account
    # For this instance, those are the ffmpeg files
    - task: ArchiveFiles@2
      displayName: 'Archive applications'
      inputs:
        rootFolderOrFile: hpc-application
        includeRootFolder: false
        archiveFile: '$(Build.ArtifactStagingDirectory)/package/$(Build.BuildId).zip'
    # Publish that zip file, so that we can use it as part
    # of our Release Pipeline later
    - task: PublishPipelineArtifact@0
      inputs:
        artifactName: 'hpc-application'
        targetPath: '$(Build.ArtifactStagingDirectory)/package'
    ```

1. Als de build is geconfigureerd, indien nodig, selecteert u **opslaan en in de wachtrij**. Als u continue integratie ingeschakeld (in de **Triggers** sectie), de build wordt automatisch geactiveerd wanneer een nieuwe doorvoer naar de opslagplaats wordt gemaakt, voldoen aan de voorwaarden instellen in de build.

    ![Voorbeeld van een bestaande Build-pijplijn](media/batch-ci-cd/existing-build-pipeline.jpg)

1. Dynamische updates van de voortgang van uw build Azure DevOps weergeven door te navigeren naar de **bouwen** sectie van de Azure-pijplijnen. Selecteer de juiste versie van uw build-definitie.

    ![Live uitvoer van de weergave van uw build](media/batch-ci-cd/Build-1.jpg)

> [!NOTE]
> Als u een clienttoepassing voor het uitvoeren van uw HPC-Batch-toepassing gebruikt, moet u een afzonderlijke build-definitie voor de toepassing maken. U vindt een aantal handleidingen in de [Azure pijplijnen](https://docs.microsoft.com/azure/devops/pipelines/get-started/index?view=azure-devops) documentatie.

## <a name="continuous-deployment"></a>Doorlopende implementatie

Azure pijplijnen wordt ook gebruikt voor het implementeren van uw toepassing en de onderliggende infrastructuur. [Pipelines vrijgeven](https://docs.microsoft.com/azure/devops/pipelines/release/what-is-release-management?view=azure-devops) is het onderdeel dat wordt continue implementatie ingeschakeld en uw release-proces wordt geautomatiseerd.

### <a name="deploying-your-application-and-underlying-infrastructure"></a>Implementeren van uw toepassing en de onderliggende infrastructuur

Er zijn een aantal stappen die betrokken zijn bij het implementeren van de infrastructuur. Zoals we hebben gebruikt [gekoppelde sjablonen](../azure-resource-manager/resource-group-linked-templates.md), deze sjablonen moet toegankelijk zijn vanaf een openbaar eindpunt (HTTP of HTTPS). Dit wordt mogelijk een opslagplaats in GitHub, of een Azure Blob Storage-Account of een andere locatie voor de opslag. De geüploade sjabloon artefacten kunnen blijven beveiligen, omdat ze kunnen worden ondergebracht in een privé-modus, maar toegankelijk met behulp van een vorm van token shared access signature (SAS). Het volgende voorbeeld ziet u hoe u een infrastructuur met sjablonen van een Azure Storage-blob te implementeren.

1. Maak een **nieuwe Release-definitie**, en selecteert u een lege definitie. Vervolgens moet u de zojuist gemaakte omgeving wijzigen in iets dat relevant is voor onze pijplijn.

    ![Eerste Release-pijplijn](media/batch-ci-cd/Release-0.jpg)

1. Een afhankelijkheid op de pijplijn bouwen om op te halen van de uitvoer voor de HPC-toepassing maken.

    > [!NOTE]
    > Nogmaals, houd er rekening mee de **Alias van de gegevensbron**, zoals dit is vereist wanneer de taken zijn gemaakt in de Release-definitie.

    ![Een artefact-koppeling naar de HPCApplicationPackage maken in de juiste build-pijplijn](media/batch-ci-cd/Release-1.jpg)

1. Een koppeling naar een andere artefacten, ditmaal, een Azure-opslagplaats maken. Dit is vereist voor toegang tot de Resource Manager-sjablonen die zijn opgeslagen in uw opslagplaats. Als het Resource Manager-sjablonen hoeven niet compileren, moet u niet doorgestuurd via een build-pijplijn.

    > [!NOTE]
    > Nogmaals, houd er rekening mee de **Alias van de gegevensbron**, zoals dit is vereist wanneer de taken zijn gemaakt in de Release-definitie.

    ![Een artefact koppeling maken naar de Azure-opslagplaatsen](media/batch-ci-cd/Release-2.jpg)

1. Navigeer naar de **variabelen** sectie. Het is raadzaam om te maken van een aantal variabelen in de pijplijn, zodat u de dezelfde gegevens in meerdere taken worden niet invoeren. Dit zijn de variabelen die worden gebruikt in dit voorbeeld, en hoe ze invloed hebben op de implementatie.

    * **applicationStorageAccountName**: Naam van het Opslagaccount voor het opslaan van binaire bestanden HPC-toepassing
    * **batchAccountApplicationName**: Naam van de toepassing in de Azure Batch-Account
    * **batchAccountName**: Naam van de Azure Batch-Account
    * **batchAccountPoolName**: Naam van de groep van virtuele machines uitvoeren van de verwerking
    * **batchApplicationId**: Unieke ID voor de Azure Batch-toepassing
    * **batchApplicationVersion**: Semantische versie van uw batch-toepassing (dat wil zeggen, de binaire ffmpeg)
    * **Locatie**: Locatie voor de Azure-Resources worden geïmplementeerd
    * **resourceGroupName**: Naam van de resourcegroep moet worden gemaakt, en waar uw resources worden geïmplementeerd
    * **storageAccountName**: Naam van het Opslagaccount voor het opslaan van de gekoppelde Resource Manager-sjablonen

    ![Voorbeeld van variabelen die zijn ingesteld voor de release van Azure-pijplijnen](media/batch-ci-cd/Release-4.jpg)

1. Navigeer naar de taken voor de Dev-omgeving. In de onderstaande momentopname, ziet u zes taken. Deze taken: de gecomprimeerde ffmpeg-bestanden te downloaden, het implementeren van een storage-account voor het hosten van de geneste Resource Manager-sjablonen, kopieert u deze Resource Manager-sjablonen naar het opslagaccount, de batch-account en de vereiste afhankelijkheden implementeren, maken van een toepassing in de Azure Batch-Account en het uploaden van het toepassingspakket met de Azure Batch-Account.

    ![Voorbeeld van de taken die wordt gebruikt voor het vrijgeven van de HPC-toepassing naar Azure Batch](media/batch-ci-cd/Release-3.jpg)

1. Voeg de **pijplijn artefact downloaden (Preview)** taak en stel de volgende eigenschappen:
    * **Weergavenaam:** ApplicationPackage-Agent downloaden
    * **De naam van het artefact te downloaden:** hpc-toepassing
    * **Pad naar het downloaden naar**: $(System.DefaultWorkingDirectory)

1. Maak een Opslagaccount voor het opslaan van uw artefacten. Een bestaand opslagaccount van de oplossing kan worden gebruikt, maar voor de ingesloten voorbeeld en de isolatie van inhoud, maken we een toegewezen storage-account voor onze artefacten (om precies het Resource Manager-sjablonen).

    Voeg de **Azure Resource Group Deployment** taak en stel de volgende eigenschappen:
    * **Weergavenaam:** Storage-Account voor Resource Manager-sjablonen implementeren
    * **Azure-abonnement:** Selecteer het juiste Azure-abonnement
    * **Actie**: Resourcegroep maken of bijwerken
    * **Resource Group**: $(resourceGroupName)
    * **Locatie**: $(location)
    * **Template**: $(System.ArtifactsDirectory)/**{YourAzureRepoArtifactSourceAlias}**/arm-templates/storageAccount.json
    * **Sjabloonparameters overschrijven**: $(storageAccountName) - accountnaam

1. Upload de artefacten vanuit broncodebeheer naar het Opslagaccount. Er is een pijplijn van Azure-taak uitvoeren. Als onderdeel van deze taak, worden de URL van de opslag Account-Container en de SAS-Token output naar een variabele in Azure-pijplijnen. Dit betekent dat deze tijdens deze fase van de agent opnieuw kan worden gebruikt.

    Voeg de **bestandskopie Azure** taak en stel de volgende eigenschappen:
    * **Source:** $(System.ArtifactsDirectory)/**{YourAzureRepoArtifactSourceAlias}**/arm-templates/
    * **Azure-verbindingstype**: Azure Resource Manager
    * **Azure-abonnement:** Selecteer het juiste Azure-abonnement
    * **Het doeltype**: Azure Blob
    * **RM Storage Account**: $(storageAccountName)
    * **Containernaam**: sjablonen
    * **De URI van Container**: templateContainerUri
    * **Storage-Container SAS-Token**: templateContainerSasToken

1. De orchestrator-sjabloon implementeren. Intrekken van de orchestrator-sjabloon van eerder, zult u merken dat er parameters voor de URL Storage Account Container, naast het SAS-token. U zou moeten zien dat de variabelen die zijn vereist in het Resource Manager-sjabloon ofwel worden bewaard in de sectie met variabelen van de release-definitie of vanaf een andere Azure-pijplijnen taak (bijvoorbeeld, onderdeel van de kopieertaak van Azure Blob) zijn ingesteld.

    Voeg de **Azure Resource Group Deployment** taak en stel de volgende eigenschappen:
    * **Weergavenaam:** Implementeren met Azure Batch
    * **Azure-abonnement:** Selecteer het juiste Azure-abonnement
    * **Actie**: Resourcegroep maken of bijwerken
    * **Resource Group**: $(resourceGroupName)
    * **Locatie**: $(location)
    * **Template**: $(System.ArtifactsDirectory)/**{YourAzureRepoArtifactSourceAlias}**/arm-templates/deployment.json
    * **Sjabloonparameters overschrijven**: ```-templateContainerUri $(templateContainerUri) -templateContainerSasToken $(templateContainerSasToken) -batchAccountName $(batchAccountName) -batchAccountPoolName $(batchAccountPoolName) -applicationStorageAccountName $(applicationStorageAccountName)```

Een algemene praktijk is het gebruik van Azure Key Vault-taken. Als de Service-Principal (verbinding met uw Azure-abonnement) een juiste toegangsbeleid instelt heeft, geheimen kunnen downloaden van een Azure Key Vault en worden gebruikt als variabelen in de pijplijn. De naam van het geheim worden met de gekoppelde waarde ingesteld. Een geheim van sshPassword kan bijvoorbeeld naar worden verwezen met $(sshPassword) in de release-definitie.

1. De volgende stappen roept de Azure CLI. De eerste wordt gebruikt voor het maken van een toepassing in Azure Batch. en upload de gekoppelde pakketten.

    Voeg de **Azure CLI** taak en stel de volgende eigenschappen:
    * **Weergavenaam:** Toepassing maken in Azure Batch-Account
    * **Azure-abonnement:** Selecteer het juiste Azure-abonnement
    * **Scriptlocatie**: Inline Script
    * **Inline Script**: ```az batch application create --application-id $(batchApplicationId) --name $(batchAccountName) --resource-group $(resourceGroupName)```

1. De tweede stap wordt gebruikt voor het uploaden van de gekoppelde pakketten naar de toepassing. In ons geval de ffmpeg-bestanden.

    Voeg de **Azure CLI** taak en stel de volgende eigenschappen:
    * **Weergavenaam:** Pakket uploaden naar Azure Batch-Account
    * **Azure-abonnement:** Selecteer het juiste Azure-abonnement
    * **Scriptlocatie**: Inline Script
    * **Inline Script**: ```az batch application package create --application-id $(batchApplicationId)  --name $(batchAccountName)  --resource-group $(resourceGroupName) --version $(batchApplicationVersion) --package-file=$(System.DefaultWorkingDirectory)/$(Release.Artifacts.{YourBuildArtifactSourceAlias}.BuildId).zip```

    > [!NOTE]
    > Het versienummer van het toepassingspakket is ingesteld op een variabele. Dit is handig als u het overschrijven van vorige versies van het pakket werkt, en als u wilt voor het handmatig beheren van het versienummer van het pakket gepusht naar Azure Batch.

1. Maak een nieuwe versie door te selecteren **Release > Maak een nieuwe release**. Zodra geactiveerd, selecteert u de koppeling naar uw nieuwe versie om de status weer te geven.

1. U kunt de live-uitvoer van de agent weergeven door het selecteren van de **logboeken** knop onder uw omgeving.

    ![De status van de release weergeven](media/batch-ci-cd/Release-5.jpg)

### <a name="testing-the-environment"></a>De testomgeving

Zodra de omgeving is ingesteld, controleert u of dat de volgende tests uit kunnen worden voltooid.

Verbinding maken met het nieuwe Azure Batch-Account met de Azure CLI uit vanaf de PowerShell-opdrachtprompt.

* Meld u aan bij uw Azure-account met `az login` en volg de instructies om te verifiëren.
* Het Batch-account nu worden geverifieerd: `az batch account login -g <resourceGroup> -n <batchAccount>`

#### <a name="list-the-available-applications"></a>Lijst van de beschikbare toepassingen

```azurecli
az batch application list -g <resourcegroup> -n <batchaccountname>
```

#### <a name="check-the-pool-is-valid"></a>Controleer dat de pool is geldig

```azurecli
az batch pool list
```

Noteer de waarde van `currentDedicatedNodes` uit de uitvoer van deze opdracht. Deze waarde wordt aangepast in de volgende test.

#### <a name="resize-the-pool"></a>De pool vergroten of verkleinen

De pool vergroten of verkleinen zodat er rekenknooppunten beschikbaar voor de taak en de taak testen, neem contact op met de opdracht van de lijst van toepassingen om te zien van de huidige status totdat het formaat is voltooid en er knooppunten beschikbaar zijn

```azurecli
az batch pool resize --pool-id <poolname> --target-dedicated-nodes 4
```

## <a name="next-steps"></a>Volgende stappen

Naast dit artikel zijn er twee zelfstudies die gebruikmaken van ffmpeg, met behulp van .NET en Python. Zie deze zelfstudies voor meer informatie over het communiceren met een Batch-account via een eenvoudige toepassing.

* [Een parallelle workload uitvoeren met Azure Batch met behulp van de Python-API](tutorial-parallel-python.md)
* [Een parallelle workload uitvoeren met Azure Batch met behulp van de .NET API](tutorial-parallel-dotnet.md)
