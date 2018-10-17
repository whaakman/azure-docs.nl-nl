---
title: Azure Deployment Manager gebruiken met Resource Manager-sjablonen | Microsoft Docs
description: Resource Manager-sjablonen met Azure Deployment Manager gebruiken om Azure-resources te implementeren.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/04/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: ae0d37dd8df9b076e72959e9036ba35d322e4e63
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48803712"
---
# <a name="tutorial-use-azure-deployment-manager-with-resource-manager-templates-public-preview"></a>Zelfstudie: Azure Deployment Manager gebruiken met Resource Manager-sjablonen (openbare preview)

Meer informatie over het gebruik van [Azure Deployment Manager](./deployment-manager-overview.md) om uw toepassingen in meerdere regio's te implementeren. Als u Deployment Manager wilt gebruiken, moet u twee sjablonen maken:

* **Een topologiesjabloon**: beschrijft de Azure-resources die uw toepassingen vormen en waar ze moeten worden geïmplementeerd.
* **Een implementatiesjabloon**: beschrijft de stappen die moeten worden genomen bij het implementeren van uw toepassingen.

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * Inzicht in het scenario
> * De zelfstudiebestanden downloaden
> * De artefacten voorbereiden
> * De door de gebruiker gedefinieerde beheerde identiteit maken
> * De servicetopologiesjabloon maken
> * De implementatiesjabloon maken
> * De sjablonen implementeren
> * De implementatie controleren
> * De nieuwere versie implementeren
> * Resources opschonen

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u dit artikel wilt voltooien, hebt u het volgende nodig:

* Enige ervaring met het ontwikkelen van [Azure Resource Manager-sjablonen](./resource-group-overview.md).
* Azure Deployment Manager bevindt zich in openbare preview. Als u zich wilt aanmelden met behulp van Azure Deployment Manager, vult u het [aanmeldingsformulier](https://aka.ms/admsignup) in. 
* Azure PowerShell. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.
* Deployment Manager-cmdlets. Als u deze prerelease-cmdlets wilt installeren, hebt u de nieuwste versie van PowerShellGet nodig. Zie [PowerShellGet installeren](/powershell/gallery/installing-psget) voor informatie over het ophalen van de nieuwste versie. Nadat u PowerShellGet hebt geïnstalleerd, sluit u het PowerShell-venster. Open een nieuw PowerShell-venster en gebruik de volgende opdracht:

    ```
    Install-Module -Name AzureRM.DeploymentManager -AllowPrerelease
    ```
* [Microsoft Azure Storage Explorer](https://go.microsoft.com/fwlink/?LinkId=708343&clcid=0x409). Azure Storage Explorer is niet vereist, maar maakt het wat gemakkelijker.

## <a name="understand-the-scenario"></a>Inzicht in het scenario

De servicetopologiesjabloon beschrijft de Azure-resources die uw service vormen en waar ze moeten worden geïmplementeerd. De definitie van de servicetopologie bevat de volgende hiërarchie:

* Servicetopologie
    * Services
        * Service-eenheden

Het volgende diagram illustreert de servicetopologie die in deze zelfstudie wordt gebruikt:

![Diagram van het scenario in de zelfstudie over Azure Deployment Manager](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-scenario-diagram.png)

Er zijn twee services toegewezen aan de locaties in VS-west en VS-oost.  Elke service heeft twee service eenheden - een webtoepassingsfront-end en een opslagaccount voor de back-end. De definities van de service-eenheid bevatten koppelingen naar de sjabloon en parameterbestanden voor het maken van de webtoepassingen en de opslagaccounts.

## <a name="download-the-tutorial-files"></a>De zelfstudiebestanden downloaden

1. Download [de sjablonen en de artefacten](https://armtutorials.blob.core.windows.net/admtutorial/ADMTutorial.zip) die in deze zelfstudie worden gebruikt.
2. Pak de bestanden uit op uw computer.

Onder de hoofdmap bevinden zich twee mappen:

- **ADMTemplates**: bevat de Deployment Manager-sjablonen, namelijk:
    - CreateADMServiceTopology.json
    - CreateADMServiceTopology.Parameters.json
    - CreateADMRollout.json
    - CreateADMRollout.Parameters.json
- **ArtifactStore**: bevat zowel de sjabloonartefacten als de binaire artefacten. Zie [De artefacten voorbereiden](#prepare-the-artifacts).

Let op: er zijn twee sets met sjablonen.  Eén set bestaat uit de Deployment Manager-sjablonen die worden gebruikt voor het implementeren van de servicetopologie en de implementatie; de andere set wordt aangeroepen vanuit de service-eenheden om webservices en opslagaccounts te maken.

## <a name="prepare-the-artifacts"></a>De artefacten voorbereiden

De map ArtifactStore uit de download bevat twee mappen:

![Diagram van de artefactbron in de zelfstudie over Azure Deployment Manager](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-artifact-source-diagram.png)

- De map **templates**: bevat de sjabloonartefacten. **1.0.0.0** en **1.0.0.1** vertegenwoordigen de twee versies van de binaire artefacten. Binnen elke versie is er een map voor elke service (service VS-oost en service VS-west). Elke service heeft een combinatie van een sjabloon en parameterbestanden voor het maken van een opslagaccount en een andere combinatie voor het maken van een webtoepassing. De webtoepassingsjabloon roept een gecomprimeerd pakket aan, dat de bestanden voor de webtoepassing bevat. Het gecomprimeerde bestand is een binair artefact dat is opgeslagen in de map met binaire bestanden.
- De map **binaries**: bevat de binaire artefacten. **1.0.0.0** en **1.0.0.1** vertegenwoordigen de twee versies van de binaire artefacten. Binnen elke versie is een zip-bestand voor het maken van de webtoepassing op de locatie van VS west en het andere zip-bestand voor het maken van de webtoepassing op de locatie van VS-oost.

De twee versies (1.0.0.0 en 1.0.0.1) zijn voor de [implementatie van de revisie](#deploy-the-revision). Hoewel zowel de sjabloonartefacten als de binaire artefacten twee versies hebben, zijn alleen de binaire artefacten verschillend tussen de twee versies. In de praktijk worden binaire artefacten vaker bijgewerkt in vergelijking met sjabloonartefacten.

1. Open **\ArtifactStore\templates\1.0.0.0\ServiceWUS\CreateStorageAccount.json** in een teksteditor. Dit is een basissjabloon voor het maken van een nieuw opslagaccount.  
2. Open **\ArtifactStore\templates\1.0.0.0\ServiceWUS\CreateWebApplication.json**. 

    ![Sjabloon voor webtoepassing maken in zelfstudie over Azure Deployment Manager](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-create-web-application-packageuri.png)

    De sjabloon roept een implementatiepakket aan dat de bestanden van de webtoepassing bevat. In deze zelfstudie bevat het gecomprimeerde pakket alleen een index.html-bestand.
3. Open **\ArtifactStore\templates\1.0.0.0\ServiceWUS\CreateWebApplicationParameters.json**. 

    ![ContainerRoot sjabloonparameters webtoepassing maken in zelfstudie over Azure Deployment Manager](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-create-web-application-parameters-deploypackageuri.png)

    De waarde van deployPackageUri is het pad naar het implementatiepakket. De parameter bevat een **$containerRoot**-variabele. De waarde van $containerRoot wordt verstrekt in de [implementatiesjabloon](#create-the-rollout-template) door de SAS-bronlocatie van het artefact, de artefacthoofdmap en deployPackageUri samen te voegen.
4. Open **\ArtifactStore\binaries\1.0.0.0\helloWorldWebAppWUS.zip\index.html**.  

    ```html
    <html>
      <head>
        <title>Azure Deployment Manager tutorial</title>
      </head>
      <body>
        <p>Hello world from west U.S.!</p>
        <p>Version 1.0.0.0</p>
      </body>
    </html>
    ```
    In de HTML worden de locatie en de versie-informatie weergegeven. Het binaire bestand in de map 1.0.0.1 geeft 'Versie 1.0.0.1' weer. Nadat u de service hebt geïmplementeerd, kunt u naar deze pagina's navigeren.
5. Bekijk andere artefactbestanden. Dit helpt u een beter inzicht te krijgen in het scenario.

Sjabloonartefacten worden gebruikt door de servicetopologiesjabloon, en binaire artefacten worden gebruikt door de implementatiesjabloon. Zowel de topologiesjabloon als de implementatiesjabloon definieert een Azure-resource met een artefactbron, wat een resource is die wordt gebruikt om Resource Manager te wijzen op de sjabloon en binaire artefacten die in de implementatie worden gebruikt. Ter vereenvoudiging van de zelfstudie wordt één opslagaccount gebruikt voor het opslaan van zowel de sjabloonartefacten als de binaire artefacten. Beide artefactbronnen verwijzen naar hetzelfde opslagaccount.

1. Een Azure-opslagaccount maken. Zie [Snelstart: Blobs uploaden, downloaden en vermelden met behulp van Azure Portal](../storage/blobs/storage-quickstart-blobs-portal.md) voor instructies.
2. Maak een blobcontainer in het opslagaccount.
3. Kopieer de twee mappen (binaire bestanden en sjablonen) en de inhoud van de twee mappen naar de blobcontainer. [Microsoft Azure Storage Explorer](https://go.microsoft.com/fwlink/?LinkId=708343&clcid=0x409) biedt ondersteuning voor slepen en neerzetten.
4. Haal de SAS-locatie van de container op met behulp van de volgende instructies:

    1. Navigeer vanuit Azure Storage Explorer naar de blobcontainer.
    2. Klik met de rechtermuisknop op de blobcontainer in het linkerdeelvenster en selecteer vervolgens **Shared Access Signature ophalen**.
    3. Geef waarden op voor **Begintijd** en **Verlooptijd**.
    4. Selecteer **Maken**.
    5. Maak een kopie van de URL. Deze URL is nodig voor om een veld in te vullen in de twee parameterbestanden, het [parameterbestand voor de topologie](#topology-parameters-file) en [parameterbestand voor de implementatie](#rollout-parameters-file).

## <a name="create-the-user-assigned-managed-identity"></a>Door de gebruiker toegewezen beheerde identiteit maken

Later in de zelfstudie voert u een implementatie uit. Een door de gebruiker toegewezen beheerde identiteit is nodig voor het uitvoeren van de implementatieacties (bijvoorbeeld het implementeren van de webtoepassingen en het opslagaccount). Deze identiteit moet toegang hebben tot het Azure-abonnement waarin u de service implementeert en voldoende machtigingen hebben om de implementatie van het artefact te kunnen voltooien.

U moet een door de gebruiker toegewezen beheerde identiteit maken en toegangsbeheer voor uw abonnement configureren.

> [!IMPORTANT]
> De door de gebruiker toegewezen beheerde identiteit moet zich op dezelfde locatie als de [implementatie](#create-the-rollout-template) bevinden. Momenteel kunnen de Deployment Manager-resources, met inbegrip van de implementatie, alleen worden gemaakt in VS-midden of VS-oost 2.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Maak een [door de gebruiker toegewezen beheerde identiteit](../active-directory/managed-identities-azure-resources/overview.md).
3. Selecteer in de portal **Abonnementen** in het linkermenu en selecteer vervolgens uw abonnement.
4. Selecteer **Toegangsbeheer (IAM)** en selecteer vervolgens **Toevoegen**
5. Typ of selecteer de volgende waarden:

    ![Toegangscontrole voor door de gebruiker toegewezen beheerde identiteit in de zelfstudie over Azure Deployment Manager](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-access-control.png)

    - **Rol**: geef voldoende machtigingen om de implementatie van het artefact (de webtoepassingen en de opslagaccounts) te kunnen voltooien. Selecteer **Inzender** in deze zelfstudie. In de praktijk wilt u de machtigingen tot het minimum beperken.
    - **Toegang toegewezen tot**: selecteer **Door de gebruiker toegewezen beheerde identiteit**.
    - Selecteer door de gebruiker toegewezen beheerde identiteit die u eerder in de zelfstudie hebt gemaakt.
6. Selecteer **Opslaan**.

## <a name="create-the-service-topology-template"></a>De servicetopologiesjabloon maken

Open **\ADMTemplates\CreateADMServiceTopology.json**.

### <a name="the-parameters"></a>De parameters

De sjabloon bevat de volgende parameters:

![Topologiesjabloonparameters in de zelfstudie over Azure Deployment Manager](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-topology-template-parameters.png)

- **namePrefix**: dit voorvoegsel wordt gebruikt voor het maken van de namen van de Deployment Manager-resources. Wanneer bijvoorbeeld het voorvoegsel 'jdoe' wordt gebruikt, is de naam van de servicetopologie **jdoe**ServiceTopology.  De resourcenamen worden gedefinieerd in de sectie met variabelen van deze sjabloon.
- **azureResourcelocation**: ter vereenvoudiging van de zelfstudie delen alle resources deze locatie, tenzij anders aangegeven. Momenteel kunnen Azure Deployment Manager-resources, met inbegrip van de implementatie, alleen worden gemaakt in **US - centraal** of **US - oost 2**.
- **artifactSourceSASLocation**: de SAS-URI naar de blobcontainer waar service-eenheidsjabloon- en parameterbestanden worden opgeslagen voor implementatie.  Zie [De artefacten voorbereiden](#prepare-the-artifacts).
- **templateArtifactRoot**: het offsetpad van de blobcontainer waarin de sjablonen en parameters worden opgeslagen. De standaardwaarde is **templates/1.0.0.0**. Wijzig deze waarde niet tenzij u de mapstructuur wilt wijzigen zoals wordt uitgelegd in [De artefacten voorbereiden](#prepare-the-artifacts). In deze zelfstudie worden relatieve paden gebruikt.  Het volledige pad wordt samengesteld door het samenvoegen van **artifactSourceSASLocation**, **templateArtifactRoot** en **templateArtifactSourceRelativePath** (of **parametersArtifactSourceRelativePath**).
- **targetSubscriptionID**: de abonnements-id waarnaar de Deployment Manager-resources worden geïmplementeerd en gefactureerd. Gebruik in deze zelfstudie uw abonnements-id.

### <a name="the-variables"></a>De variabelen

In de sectie met variabelen worden de namen van de resources, de Azure-locaties voor de twee services: **Service WUS** en **Service EUS**, en de artefactpaden gedefinieerd:

![Topologiesjabloonvariabelen in de zelfstudie over Azure Deployment Manager](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-topology-template-variables.png)

Vergelijk de artefactpaden met de mapstructuur die u hebt geüpload naar het opslagaccount. U ziet dat de artefactpaden relatieve paden zijn. Het volledige pad wordt samengesteld door het samenvoegen van **artifactSourceSASLocation**, **templateArtifactRoot** en **templateArtifactSourceRelativePath** (of **parametersArtifactSourceRelativePath**).

### <a name="the-resources"></a>De resources

Op het hoogste niveau zijn er twee resources gedefinieerd: *een bronartefact* en *een servicetopologie*.

De definitie van de artefactbron is:

![Artefactbron van topologiesjabloonresources in de zelfstudie over Azure Deployment Manager](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-topology-template-resources-artifact-source.png)

Op de volgende schermafbeelding ziet u alleen bepaalde onderdelen van de definities van de servicetopologie, services van de service-eenheden:

![Servicetopologie van topologiesjabloonresources in de zelfstudie over Azure Deployment Manager](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-topology-template-resources-service-topology.png)

- **artifactSourceId** wordt gebruikt om de artifactbronresource te koppelen aan de servicetopologieresource.
- **dependsOn**: alle servicetopologieresources zijn afhankelijk van de artefactbron.
- **artefacten** verwijzen naar de sjabloonartefacten.  Hier worden relatieve paden gebruikt. Het volledige pad wordt samengesteld door het samenvoegen van artifactSourceSASLocation (gedefinieerd in de artefactbron), artifactRoot (gedefinieerd in de artefactbron) en templateArtifactSourceRelativePath (of parametersArtifactSourceRelativePath).

### <a name="topology-parameters-file"></a>Topologieparameterbestand

U maakt een parameterbestand dat wordt gebruikt in combinatie met de topologiesjabloon.

1. Open **\ADMTemplates\CreateADMServiceTopology.Parameters** in Visual Studio Code of in een teksteditor.
2. Vul de parameterwaarden in:

    - **namePrefix**: voer een tekenreeks met 4-5 tekens in. Dit voorvoegsel wordt gebruikt om voor Azure unieke resourcenamen te maken.
    - **azureResourceLocation**: als u niet bekend bent met Azure-locaties, gebruikt u in deze zelfstudie **centralus**.
    - **artifactSourceSASLocation**: voer de SAS-URI naar de hoofdmap (de blobcontainer) waar service-eenheidsjabloon- en parameterbestanden worden opgeslagen voor implementatie in.  Zie [De artefacten voorbereiden](#prepare-the-artifacts).
    - **templateArtifactRoot**: gebruik in deze zelfstudie **templates/1.0.0.0**, tenzij u de mapstructuur van de artefacten wijzigt.
    - **tragetScriptionID**: voer uw Azure-abonnements-id in.

> [!IMPORTANT]
> De topologiesjabloon en de implementatiesjabloon delen enkele algemene parameters. Deze parameters moeten dezelfde waarden hebben. Deze parameters zijn: **namePrefix**, **azureResourceLocation** en **artifactSourceSASLocation** (beide artefactbronnen delen hetzelfde opslagaccount in deze zelfstudie).

## <a name="create-the-rollout-template"></a>De implementatiesjabloon maken

Open **\ADMTemplates\CreateADMRollout.json**.

### <a name="the-parameters"></a>De parameters

De sjabloon bevat de volgende parameters:

![Implementatiesjabloonparameters in de zelfstudie over Azure Deployment Manager](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-rollout-template-parameters.png)

- **namePrefix**: dit voorvoegsel wordt gebruikt voor het maken van de namen van de Deployment Manager-resources. Wanneer u bijvoorbeeld het voorvoegsel 'jdoe' gebruikt, is de naam van de implementatie **jdoe**Rollout.  De namen worden gedefinieerd in de sectie met variabelen van de sjabloon.
- **azureResourcelocation**: ter vereenvoudiging van de zelfstudie delen alle Deployment Manager-resources deze locatie, tenzij anders aangegeven. Momenteel kunnen Azure Deployment Manager-resources, met inbegrip van de implementatie, alleen worden gemaakt in **US - centraal** of **US - oost 2**.
- **artifactSourceSASLocation**: de SAS-URI naar de hoofdmap (de blobcontainer) waar service-eenheidsjabloon- en parameterbestanden worden opgeslagen voor implementatie.  Zie [De artefacten voorbereiden](#prepare-the-artifacts).
- **binaryArtifactRoot**: de standaardwaarde is **binaries/1.0.0.0**. Wijzig deze waarde niet tenzij u de mapstructuur wilt wijzigen zoals wordt uitgelegd in [De artefacten voorbereiden](#prepare-the-artifacts). In deze zelfstudie worden relatieve paden gebruikt.  Het volledige pad wordt samengesteld door het samenvoegen van **artifactSourceSASLocation**, **binaryArtifactRoot** en de **deployPackageUri** die is opgegeven in CreateWebApplicationParameters.json.  Zie [De artefacten voorbereiden](#prepare-the-artifacts).
- **managedIdentityID**: de door de gebruiker toegewezen beheerde identiteit die de implementatieacties uitvoert. Zie [Door de gebruiker toegewezen beheerde identiteit maken](#create-the-user-assigned-managed-identity).

### <a name="the-variables"></a>De variabelen

In de sectie met variabelen worden de namen van de resources gedefinieerd. Zorg ervoor dat de naam van de servicetopologie, de servicenamen en de namen van de service-eenheden overeenkomen met de namen die zijn gedefinieerd in de [topologiesjabloon](#create-the-service-topology-template).

![Implementatiesjabloonvariabelen in de zelfstudie over Azure Deployment Manager](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-rollout-template-variables.png)

### <a name="the-resources"></a>De resources

Op het hoogste niveau zijn er drie resources gedefinieerd: een bronartefact, een stap en een implementatie.

De definitie van de artefactbron is identiek aan die in de topologiesjabloon is gedefinieerd.  Zie [De servicetopologietopologiesjabloon maken](#create-the-service-topology-tempate) voor meer informatie.

Op de volgende schermafbeeldingen wordt de definitie van de wachtstap weergegeven:

![Wachtstap implementatiesjabloonresources in de zelfstudie over Azure Deployment Manager](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-rollout-template-resources-wait-step.png)

Voor de duur wordt gebruikgemaakt van de [ISO 8601-norm](https://en.wikipedia.org/wiki/ISO_8601#Durations). **PT1M** (hoofdletters zijn verplicht) is een voorbeeld van 1 minuut wachten. 

Op de volgende schermafbeelding worden alleen bepaalde onderdelen van de definitie van de implementatie weergegeven:

![Implementatie van implementatiesjabloonresources in de zelfstudie over Azure Deployment Manager](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-rollout-template-resources-rollout.png)

- **dependsOn**: de implementatieresource is afhankelijk van de artefactbronresource en alle gedefinieerde stappen.
- **artifactSourceId** wordt gebruikt om de artefactbronresource te koppelen aan de servicetopologieresource.
- **targetServiceTopologyId** wordt gebruikt om de servicetopologieresource te koppelen aan de implementatieresource.
- **deploymentTargetId**: dit is de service-eenheidresource-ID van de servicetopologieresource.
- **preDeploymentSteps** en **postDeploymentSteps**: bevat de implementatiestappen. In de sjabloon wordt een wachtstap aangeroepen.
- **dependsOnStepGroups**: configureer de afhankelijkheden tussen de stapgroepen.

### <a name="rollout-parameters-file"></a>Implementatieparameterbestand

U maakt een parameterbestand dat wordt gebruikt in combinatie met de implementatiesjabloon.

1. Open **\ADMTemplates\CreateADMRollout.Parameters** in Visual Studio Code of in een teksteditor.
2. Vul de parameterwaarden in:

    - **namePrefix**: voer een tekenreeks met 4-5 tekens in. Dit voorvoegsel wordt gebruikt om voor Azure unieke resourcenamen te maken.
    - **azureResourceLocation**: momenteel kunnen Azure Deployment Manager-resources, met inbegrip van de implementatie, alleen worden gemaakt in **VS-midden** of **VS-oost 2**.
    - **artifactSourceSASLocation**: voer de SAS-URI naar de hoofdmap (de blobcontainer) waar service-eenheidsjabloon- en parameterbestanden worden opgeslagen voor implementatie in.  Zie [De artefacten voorbereiden](#prepare-the-artifacts).
    - **binaryArtifactRoot**: gebruik in deze zelfstudie **binaries/1.0.0.0**, tenzij u de mapstructuur van de artefacten wijzigt.
    - **managedIdentityID**: voer de door de gebruiker toegewezen beheerde identiteit in. Zie [Door de gebruiker toegewezen beheerde identiteit maken](#create-the-user-assigned-managed-identity). De syntaxis is:

        ```
        "/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/Microsoft.ManagedIdentity/userassignedidentities/<ManagedIdentityName>"
        ```

> [!IMPORTANT]
> De topologiesjabloon en de implementatiesjabloon delen enkele algemene parameters. Deze parameters moeten dezelfde waarden hebben. Deze parameters zijn: **namePrefix**, **azureResourceLocation** en **artifactSourceSASLocation** (beide artefactbronnen delen hetzelfde opslagaccount in deze zelfstudie).

## <a name="deploy-the-templates"></a>De sjablonen implementeren

Azure PowerShell kan worden gebruikt om de sjablonen te implementeren. 

1. Voer het script uit om de servicetopologie te implementeren.

    ```powershell
    $deploymentName = "<Enter a Deployment Name>"
    $resourceGroupName = "<Enter a Resource Group Name>"
    $location = "Central US"  
    $filePath = "<Enter the File Path to the Downloaded Tutorial Files>"
    
    # Create a resource group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    
    # Create the service topology
    New-AzureRmResourceGroupDeployment `
        -Name $deploymentName `
        -ResourceGroupName $resourceGroupName `
        -TemplateFile "$filePath\ADMTemplates\CreateADMServiceTopology.json" `
        -TemplateParameterFile "$filePath\ADMTemplates\CreateADMServiceTopology.Parameters.json"
    ```

2. Controleer of de servicetopologie en de onderstreepte resources zijn gemaakt met behulp van Azure Portal:

    ![Geïmplementeerde servicetopologieresources in de zelfstudie over Azure Deployment Manager](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-deployed-topology-resources.png)

    **Verborgen typen weergeven** moet worden geselecteerd om de resources weer te geven.

3. De implementatiesjabloon implementeren:

    ```powershell
    # Create the rollout
    New-AzureRmResourceGroupDeployment `
        -Name $deploymentName `
        -ResourceGroupName $resourceGroupName `
        -TemplateFile "$filePath\ADMTemplates\CreateADMRollout.json" `
        -TemplateParameterFile "$filePath\ADMTemplates\CreateADMRollout.Parameters.json"
    ```

4. Controleer de voortgang van de implementatie met behulp van het volgende PowerShell-script:

    ```powershell
    # Get the rollout status
    $rolloutname = "<Enter the Rollout Name>"
    Get-AzureRmDeploymentManagerRollout `
        -ResourceGroupName $resourceGroupName `
        -Name $rolloutName
    ```

    De Deployment Manager PowerShell-cmdlets moeten worden geïnstalleerd voordat u deze cmdlet kunt uitvoeren. Zie [Vereisten](#prerequisite).

    Het volgende voorbeeld toont de actieve status:
    
    ```
    ResourceGroupName       : adm0925rg
    BuildVersion            : 1.0.0.0
    ArtifactSourceId        : /subscriptions/<SubscriptionID>/resourceGroups/adm0925rg/providers/Microsoft.DeploymentManager/artifactSources/adm0925ArtifactSourceRollout
    TargetServiceTopologyId : /subscriptions/<SubscriptionID>/resourceGroups/adm0925rg/providers/Microsoft.DeploymentManager/serviceTopologies/adm0925ServiceTopology
    Status                  : Running
    TotalRetryAttempts      : 0
    OperationInfo           : Microsoft.Azure.Commands.DeploymentManager.Models.PSRolloutOperationInfo
    Services                : {adm0925ServiceEUS, adm0925ServiceWUS}
    Name                    : adm0925Rollout
    Type                    : Microsoft.DeploymentManager/rollouts
    Location                : centralus
    Id                      : /subscriptions/<SubscriptionID>/resourcegroups/adm0925rg/providers/Microsoft.DeploymentManager/rollouts/adm0925Rollout
    Tags                    :
    ```

    Wanneer de implementatie met succes is voltooid, ziet u dat er twee extra resourcegroepen zijn gemaakt, één voor elke service.

## <a name="verify-the-deployment"></a>De implementatie controleren

1. Open de [Azure Portal](https://portal.azure.com).
2. Blader naar de zojuist gemaakte webtoepassingen onder de nieuwe resourcegroepen die zijn gemaakt door de implementatie.
3. Open de webtoepassing in een webbrowser. Controleer de locatie en de versie in het index.html-bestand.

## <a name="deploy-the-revision"></a>De revisie implementeren

Wanneer u een nieuwe versie (1.0.0.1) voor de webtoepassing hebt. Kunt u de volgende procedure gebruiken om de webtoepassing opnieuw te implementeren.

1. Open CreateADMRollout.Parameters.json.
2. Werk **binaryArtifactRoot** naar **binaries/1.0.0.1** bij.
3. Voer de implementatie opnieuw uit volgens de instructies in [De sjablonen implementeren](#deploy-the-templates).
4. Controleer de implementatie volgens de instructies in [De implementatie controleren](#verify-the-deployment). Op de webpagina wordt de versie 1.0.0.1 weergegeven.

## <a name="clean-up-resources"></a>Resources opschonen

Schoon de geïmplementeerd Azure-resources, wanneer u deze niet meer nodig hebt, op door de resourcegroep te verwijderen.

1. Selecteer **Resourcegroep** in het linkermenu van Azure Portal.
2. Gebruik het veld **Filteren op naam** om u te beperken tot de resourcegroepen die u in deze zelfstudie hebt gemaakt. Er zijn er 3-4:

    - **&lt;namePrefix > rg**: bevat de Deployment Manager-resources.
    - **&lt;namePrefix > ServiceWUSrg**: bevat de resources die zijn gedefinieerd door ServiceWUS.
    - **&lt;namePrefix>ServiceEUSrg**: bevat de resources die zijn gedefinieerd door ServiceEUS.
    - De resourcegroep voor de door de gebruiker gedefinieerde beheerde identiteit.
3. Selecteer de naam van de resourcegroep.  
4. Selecteer **Resourcegroep verwijderen** in het bovenste menu.
5. Herhaal de laatste twee stappen als u andere resourcegroepen wilt verwijderen die zijn gemaakt in deze zelfstudie.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u Azure Deployment Manager gebruikt. Zie [de documentatie bij Azure Resource Manager](/azure/azure-resource-manager/) voor meer informatie.