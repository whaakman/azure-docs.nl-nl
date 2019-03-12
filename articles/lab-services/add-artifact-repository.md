---
title: Een artefactopslagplaats toevoegen aan uw lab in Azure DevTest Labs | Microsoft Docs
description: Leer hoe u een artefactopslagplaats toevoegen aan uw lab in Azure DevTest labs.
services: devtest-lab
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2019
ms.author: spelluru
ms.openlocfilehash: 85763a895b61d184db033c09a413cc897ef6eaa3
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57551599"
---
# <a name="add-an-artifact-repository-to-your-lab-in-devtest-labs"></a>Een artefactopslagplaats toevoegen aan uw lab in DevTest Labs
DevTest Labs kunt u opgeven van een artefact dat moet worden toegevoegd aan een virtuele machine op het moment van de VM is gemaakt of nadat de virtuele machine is gemaakt. Dit artefact is mogelijk een hulpprogramma of een toepassing die u wilt installeren op de virtuele machine. Artefacten worden gedefinieerd in een JSON-bestand geladen vanuit een GitHub- of VSTS Git-opslagplaats. 

De [openbare artefactenopslag](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts), onderhouden door DevTest Labs, biedt veel algemene hulpprogramma's voor zowel Windows als Linux. Een koppeling naar deze opslagplaats wordt automatisch toegevoegd aan uw testomgeving. U kunt uw eigen artefactopslagplaats maken met specifieke hulpprogramma's die niet beschikbaar zijn in de openbare artefactenopslag. Zie voor meer informatie over het maken van aangepaste artefacten, [maken van aangepaste artefacten](devtest-lab-artifact-author.md).

In dit artikel bevat informatie over het toevoegen van uw aangepaste artefactopslagplaats met behulp van Azure portal, Azure Resource Management-sjablonen en Azure PowerShell. U kunt u automatisch een artefactopslagplaats toe te voegen aan een lab door PowerShell of CLI-scripts te schrijven. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten
Een opslagplaats toevoegen aan uw testomgeving, moet u eerst, belangrijke informatie ophalen uit uw opslagplaats. De volgende secties wordt beschreven hoe u aan de vereiste gegevens voor opslagplaatsen die worden gehost op **GitHub** of **Azure DevOps**.

### <a name="get-the-github-repository-clone-url-and-personal-access-token"></a>De GitHub-opslagplaats kloon-URL en Pat-token ophalen

1. Ga naar de startpagina van de GitHub-opslagplaats die het artefact of Sjabloondefinities van de Resource Manager-bevat.
2. Selecteer **Klonen of downloaden**.
3. Als de URL naar het Klembord kopiëren, selecteert de **HTTPS kloon-url** knop. De URL voor later gebruik opslaan.
4. Selecteer de installatiekopie van het profiel in de rechterbovenhoek van GitHub, en selecteer vervolgens **instellingen**.
5. In de **persoonlijke instellingen** menu aan de linkerkant, selecteer **-instellingen voor ontwikkelaars**.
6. Selecteer **persoonlijke toegangstokens** in het menu links.
7. Selecteer **nieuw token genereren**.
8. Op de **nieuwe persoonlijke toegangstoken** pagina onder **beschrijving Token**, voer een beschrijving in. Accepteer de standaarditems onder **bereiken selecteren**, en selecteer vervolgens **Token genereren**.
9. Sla het gegenereerde token. U gebruikt het token later.
10. Sluit GitHub.   

### <a name="get-the-azure-repos-clone-url-and-personal-access-token"></a>De Azure-opslagplaatsen-kloon-URL en persoonlijk toegangstoken ophalen
1. Ga naar de startpagina van uw team-verzameling (bijvoorbeeld https://contoso-web-team.visualstudio.com), en selecteer vervolgens uw project.
2. Selecteer op de startpagina van project **Code**.
3. Om weer te geven van de kloon-URL op het project **Code** weergeeft, schakelt **kloon**.
4. Sla de URL op. Gebruikt u later de URL.
5. Voor het maken van een persoonlijk toegangstoken in de vervolgkeuzelijst in het gebruikersaccountmenu selecteert **Mijn profiel**.
6. Selecteer op de pagina van de informatie profiel **Security**.
7. Op de **Security** tabblad **toevoegen**.
8. Op de **maken van een persoonlijk toegangstoken** pagina:
   1. Voer een **beschrijving** voor het token.
   2. In de **verloopt In** in de lijst met **180 dagen**.
   3. In de **Accounts** in de lijst met **alle toegankelijke accounts**.
   4. Selecteer de **alle scopes** optie.
   5. Selecteer **maakt u Token**.
9. Het nieuwe token wordt weergegeven in de **persoonlijke toegangstokens** lijst. Selecteer **kopie Token**, en sla vervolgens de waarde voor de token voor later gebruik.
10. Doorgaan met het verbinding maken met uw lab, waar de opslagplaats-sectie.

## <a name="use-azure-portal"></a>Azure Portal gebruiken
In deze sectie bevat stappen voor het toevoegen van een artefactopslagplaats aan een lab in Azure portal. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **meer Services**, en selecteer vervolgens **DevTest Labs** uit de lijst met services.
3. Selecteer in de lijst met labs, uw lab. 
4. Selecteer **configuratie en het beleid** in het menu links.
5. Selecteer **opslagplaatsen** onder **externe bronnen** sectie in het menu links.
6. Selecteer **+ toevoegen** op de werkbalk.

    ![De knop van de opslagplaats toevoegen](./media/devtest-lab-add-repo/devtestlab-add-repo.png)
5. Op de **opslagplaatsen** pagina, geeft u de volgende informatie:
  1. **Naam**. Voer een naam voor de opslagplaats.
  2. **GIT kloon-Url**. Voer de Git-HTTPS-kloon-URL die u eerder hebt gekopieerd vanuit GitHub of Azure DevOps-Services.
  3. **Vertakking**. Als u uw definities, voer de vertakking.
  4. **Persoonlijk toegangstoken**. Voer in het persoonlijke toegangstoken dat u eerder hebt ontvangen van GitHub of Azure DevOps-Services.
  5. **Mappaden**. Geef ten minste één pad ten opzichte van de kloon-URL die uw artefacten of Sjabloondefinities van de Resource Manager-bevat. Wanneer u een submap opgeeft, zorg ervoor dat u de schuine streep in het mappad.

        ![Opslagplaatsen gebied](./media/devtest-lab-add-repo/devtestlab-repo-blade.png)
6. Selecteer **Opslaan**.

## <a name="use-azure-resource-manager-template"></a>Azure Resource Manager-sjabloon gebruiken
Azure Resource Management (Azure Resource Manager)-sjablonen zijn JSON-bestanden die worden beschreven van resources in Azure die u wilt maken. Zie voor meer informatie over deze sjablonen [Authoring Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-authoring-templates.md).

In deze sectie bevat stappen voor het toevoegen van een artefactopslagplaats aan een lab met behulp van een Azure Resource Manager-sjabloon.  Als deze nog niet bestaat, maakt de sjabloon in het lab. 

### <a name="template"></a>Template
De voorbeeldsjabloon die wordt gebruikt in dit artikel worden de volgende informatie verzameld via parameters. De meeste van de parameters hebt slimme standaardinstellingen, maar er zijn enkele waarden die moeten worden opgegeven. U moet de naam van de testomgeving, URI voor de artefactopslagplaats en het beveiligingstoken voor de opslagplaats. 

- Labnaam van het.
- Weergavenaam voor de artefactopslagplaats in de DevTest Labs-gebruikersinterface (UI). De standaardwaarde is: `Team Repository`.
- URI naar de opslagplaats (voorbeeld: `https://github.com/<myteam>/<nameofrepo>.git` of `"https://MyProject1.visualstudio.com/DefaultCollection/_git/TeamArtifacts"`.
- Vertakking in de opslagplaats met artefacten. De standaardwaarde is: `master`.
- De naam van de map met artefacten. De standaardwaarde is: `/Artifacts`.
- Het type van de opslagplaats. Toegestane waarden zijn `VsoGit` of `GitHub`.
- Toegangstoken voor de opslagplaats. 

    ```json
    {
    
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "labName": {
                "type": "string"
            },
            "artifactRepositoryDisplayName": {
                "type": "string",
                "defaultValue": "Team Repository"
            },
            "artifactRepoUri": {
                "type": "string"
            },
            "artifactRepoBranch": {
                "type": "string",
                "defaultValue": "master"
            },
            "artifactRepoFolder": {
                "type": "string",
                "defaultValue": "/Artifacts"
            },
            "artifactRepoType": {
                "type": "string",
                "allowedValues": ["VsoGit", "GitHub"]
            },
            "artifactRepoSecurityToken": {
                "type": "securestring"
            }
        },
        "variables": {
            "artifactRepositoryName": "[concat('Repo-', uniqueString(subscription().subscriptionId))]"
        },
        "resources": [{
                "apiVersion": "2016-05-15",
                "type": "Microsoft.DevTestLab/labs",
                "name": "[parameters('labName')]",
                "location": "[resourceGroup().location]",
                "resources": [
                    {
                        "apiVersion": "2016-05-15",
                        "name": "[variables('artifactRepositoryName')]",
                        "type": "artifactSources",
                        "dependsOn": [
                            "[resourceId('Microsoft.DevTestLab/labs', parameters('labName'))]"
                        ],
                        "properties": {
                            "uri": "[parameters('artifactRepoUri')]",
                            "folderPath": "[parameters('artifactRepoFolder')]",
                            "branchRef": "[parameters('artifactRepoBranch')]",
                            "displayName": "[parameters('artifactRepositoryDisplayName')]",
                            "securityToken": "[parameters('artifactRepoSecurityToken')]",
                            "sourceType": "[parameters('artifactRepoType')]",
                            "status": "Enabled"
                        }
                    }
                ]
            }
        ]
    }
    ```


### <a name="deploy-the-template"></a>De sjabloon implementeren
Er zijn een paar manieren om de sjabloon implementeren in Azure en de resource gemaakt, als deze niet bestaat, of bijgewerkt, hebben als deze bestaat. Zie de volgende artikelen voor meer informatie:

- [Resources implementeren met Resource Manager-sjablonen en Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
- [Resources implementeren met Resource Manager-sjablonen en Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md)
- [Resources implementeren met Resource Manager-sjablonen en Azure Portal](../azure-resource-manager/resource-group-template-deploy-portal.md)
- [Resources implementeren met Resource Manager-sjablonen en Resource Manager REST API](../azure-resource-manager/resource-group-template-deploy-rest.md)

Laten we doorgaan en informatie over het implementeren van de sjabloon in PowerShell. Cmdlets die worden gebruikt om de sjabloon te implementeren zijn contextspecifiek, zodat de huidige tenant en het huidige abonnement worden gebruikt. Gebruik [Set AzContext](/powershell/module/az.accounts/set-azcontext) voordat u de sjabloon implementeert, indien nodig, context wijzigen.

Maak eerst een resource-groep met [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Als de resourcegroep die u wilt gebruiken al bestaat, moet u deze stap overslaan.

```powershell
New-AzResourceGroup -Name MyLabResourceGroup1 -Location westus
```

Maak vervolgens een implementatie met de resource-group via [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment). Deze cmdlet geldt de wijzigingen van resources voor Azure. Aantal resource-implementaties kunnen worden gemaakt naar een bepaalde resourcegroep. Als u meerdere keren naar dezelfde resourcegroep implementeert, zorg er dan voor dat de naam van elke implementatie is uniek.

```powershell
New-AzResourceGroupDeployment `
    -Name MyLabResourceGroup-Deployment1 `
    -ResourceGroupName MyLabResourceGroup1 `
    -TemplateFile azuredeploy.json `
    -TemplateParameterFile azuredeploy.parameters.json
```

Nadat u New-AzResourceGroupDeployment is uitgevoerd, voert de opdracht belangrijke informatie zoals de Inrichtingsstatus (moet is geslaagd) en uitvoer voor de sjabloon.
 
## <a name="use-azure-powershell"></a>Azure PowerShell gebruiken 
Deze sectie bevat een voorbeeld van PowerShell-script dat kan worden gebruikt om een artefactopslagplaats toevoegen aan een lab. Als u geen Azure PowerShell, raadpleegt u [hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview?view=azps-1.2.0) voor gedetailleerde instructies om deze te installeren.

### <a name="full-script"></a>Volledige script
Dit is het volledige script, met inbegrip van sommige uitgebreide berichten en opmerkingen:

**New-DevTestLabArtifactRepository.ps1**:

```powershell

<#

.SYNOPSIS
This script creates a new custom repository and adds it to an existing DevTest Lab.

.PARAMETER LabName
The name of the lab.

.PARAMETER LabResourceGroupName
The name of the resource group that contains the lab. 

.PARAMETER ArtifactRepositoryName
Name for the new artifact repository.
Script creates a random name for the respository if it is not specified.

.PARAMETER ArtifactRepositoryDisplayName
Display name for the artifact repository.
This is the name that shows up in the Azure portal (https://portal.azure.com) when viewing all the artifact repositories for a lab.

.PARAMETER RepositoryUri
Uri to the repository.

.PARAMETER RepositoryBranch
Branch in which artifact files can be found. Defaults to 'master'.

.PARAMETER FolderPath
Folder under which artifacts can be found. Defaults to '/Artifacts'

.PARAMETER PersonalAccessToken
Security token for access to GitHub or VSOGit repository.
See https://azure.microsoft.com/en-us/documentation/articles/devtest-lab-add-artifact-repo/ for instructions to get personal access token

.PARAMETER SourceType
Whether artifact is VSOGit or GitHub repository.

.EXAMPLE
Set-AzContext -SubscriptionId 11111111-1111-1111-1111-111111111111
.\New-DevTestLabArtifactRepository.ps1 -LabName "mydevtestlab" -LabResourceGroupName "mydtlrg" -ArtifactRepositoryName "MyTeam Repository" -RepositoryUri "https://github.com/<myteam>/<nameofrepo>.git" -PersonalAccessToken "1111...." -SourceType "GitHub"

.NOTES
Script uses the current Az context. To set the context, use the Set-AzContext cmdlet

#>

 
[CmdletBinding()]
Param(

    [Parameter(Mandatory=$true)]
    $LabName,

    [Parameter(Mandatory=$true)]
    $LabResourceGroupName,
    $ArtifactRepositoryName,
    $ArtifactRepositoryDisplayName  = 'Team Artifact Repository',

    [Parameter(Mandatory=$true)]
    $RepositoryUri,
    $RepositoryBranch = 'master',
    $FolderPath = '/Artifacts',
    
    [Parameter(Mandatory=$true)]
    $PersonalAccessToken ,
    
    [Parameter(Mandatory=$true)]
    [ValidateSet('VsoGit', 'GitHub')]
    $SourceType
)


#Set artifact repository internal name,
# if not set by user.

if ($ArtifactRepositoryName -eq $null){
    $ArtifactRepositoryName = "PrivateRepo" + (Get-Random -Maximum 999)
}

# Sign in to Azure
Connect-AzAccount


#Get Lab Resource
$LabResource = Get-AzResource -ResourceType 'Microsoft.DevTestLab/labs' -ResourceName $LabName -ResourceGroupName $LabResourceGroupName

Write-Verbose "Lab Name: $($LabResource.Name)"
Write-Verbose "Lab Resource Group Name: $($LabResource.ResourceGroupName)"
Write-Verbose "Lab Resource Location: $($LabResource.Location)"

Write-Verbose "Artifact Repository Internal Name: $ArtifactRepositoryName"

#Prepare properties object for call to New-AzResource
$propertiesObject = @{
    uri = $RepositoryUri;
    folderPath = $FolderPath;
    branchRef = $RepositoryBranch;
    displayName = $ArtifactRepositoryDisplayName;
    securityToken = $PersonalAccessToken;
    sourceType = $SourceType;
    status = 'Enabled'
}

Write-Verbose @"Properties to be passed to New-AzResource:$($propertiesObject | Out-String)"@

#Resource will be added to current subscription.
$resourcetype = 'Microsoft.DevTestLab/labs/artifactSources'
$resourceName = $LabName + '/' + $ArtifactRepositoryName
Write-Verbose "Az ResourceType: $resourcetype"
Write-Verbose "Az ResourceName: $resourceName"
 
Write-Verbose "Creating artifact repository '$ArtifactRepositoryDisplayName'..."
$result = New-AzResource -Location $LabResource.Location -ResourceGroupName $LabResource.ResourceGroupName -properties $propertiesObject -ResourceType $resourcetype -ResourceName $resourceName -ApiVersion 2016-05-15 -Force


#Alternate implementation:
# Use resourceId rather than resourcetype and resourcename parameters.
# Using resourceId allows you to specify the $SubscriptionId rather than using the
# subscription id of Get-AzContext.
#$resourceId = "/subscriptions/$SubscriptionId/resourceGroups/$($LabResource.ResourceGroupName)/providers/Microsoft.DevTestLab/labs/$LabName/artifactSources/$ArtifactRepositoryName"
#$result = New-AzResource -properties $propertiesObject -ResourceId $resourceId -ApiVersion 2016-05-15 -Force


# Check the result
if ($result.Properties.ProvisioningState -eq "Succeeded") {
    Write-Verbose ("Successfully added artifact repository source '$ArtifactRepositoryDisplayName'")
}
else {
    Write-Error ("Error adding artifact repository source '$ArtifactRepositoryDisplayName'")
}

#Return the newly created resource so it may be used in subsequent scripts
return $result
```

### <a name="run-the-powershell-script"></a>Het PowerShell-script uitvoeren
Het volgende voorbeeld ziet u hoe u het script uit te voeren: 

```powershell
Set-AzContext -SubscriptionId <Your Azure subscription ID>

.\New-DevTestLabArtifactRepository.ps1 -LabName "mydevtestlab" -LabResourceGroupName "mydtlrg" -ArtifactRepositoryName "MyTeam Repository" -RepositoryUri "https://github.com/<myteam>/<nameofrepo>.git" -PersonalAccessToken "1111...." -SourceType "GitHub"
```


### <a name="parameters"></a>Parameters
De PowerShell-voorbeeldscript in dit artikel worden de volgende parameters:

| Parameter | Description | 
| --------- | ----------- | 
| LabName | De naam van het testlab. |
| ArtifactRepositoryName | Naam voor de nieuwe artefactopslagplaats. Het script wordt een willekeurige naam voor de opslagplaats gemaakt als deze niet is opgegeven. |
| ArtifactRepositoryDisplayName | Weergavenaam voor de artefactopslagplaats. Dit is de naam die weergegeven in de Azure-portal wordt (https://portal.azure.com) bij het weergeven van alle artefact-opslagplaatsen voor een testomgeving. |
| RepositoryUri | De URI naar de opslagplaats. Voorbeelden: `https://github.com/<myteam>/<nameofrepo>.git` of `"https://MyProject1.visualstudio.com/DefaultCollection/_git/TeamArtifacts"`.| 
| RepositoryBranch | De vertakking in welke artefact bestanden worden gevonden. Standaard ingesteld op 'master'. | 
| FolderPath | De map waarin artefacten kunnen worden gevonden. Standaard ingesteld op ' / artefacten |
| PersonalAccessToken | Het beveiligingstoken voor toegang tot de opslagplaats met GitHub of VSOGit. Zie de sectie vereisten voor instructies voor het persoonlijke toegangstoken ophalen |
| sourceType | Of is artefact VSOGit of GitHub-opslagplaats. |

De opslagplaats zelf moet een interne naam voor het identificeren, die afwijkt van die de weergavenaam die wordt weergegeven in de Azure-portal. Ziet u niet de interne naam met behulp van de Azure portal, maar u deze zien bij het gebruik van Azure REST API's of Azure PowerShell. Het script geeft een naam, als deze niet is opgegeven door de gebruiker van het script.

```powershell
#Set artifact repository name, if not set by user
if ($ArtifactRepositoryName -eq $null){
    $ArtifactRepositoryName = "PrivateRepo" + (Get-Random -Maximum 999)
}
```

### <a name="powershell-commands-used-in-the-script"></a>PowerShell-opdrachten in het script gebruikt

| PowerShell-opdracht | Opmerkingen |
| ------------------ | ----- |
| [Get-AzResource](/powershell/module/az.resources/get-azresource) | Met deze opdracht wordt gebruikt voor meer informatie over het lab, zoals de locatie. |
| [New-AzResource](/powershell/module/az.resources/new-azresource) | Er is geen specifieke opdracht voor het toevoegen van artefact opslagplaatsen. De algemene [New-AzResource](/powershell/module/az.resources/new-azresource) cmdlet wordt de taak. Deze cmdlet moet ofwel de **ResourceId** of de **ResourceName** en **ResourceType** paar om te weten welk type resource die u wilt maken. Met dit voorbeeldscript maakt gebruik van de resourcenaam en de resource-type-paar. <br/><br/>U ziet dat het maken van de bron van de opslagplaats artefact op dezelfde locatie en in dezelfde resourcegroep bevinden als het lab.|

Het script voegt een nieuwe resource toe aan het huidige abonnement. Gebruik [Get-AzContext](/powershell/module/az.accounts/get-azcontext) om deze informatie te bekijken. Gebruik [Set AzContext](/powershell/module/az.accounts/set-azcontext) om in te stellen de huidige tenant en hetzelfde abonnement.

De beste manier voor het detecteren van de resourcenaam van de en resource-informatie is met de [Test Drive Azure REST-API's](https://azure.github.io/projects/apis/) website. Bekijk de [DevTest Labs-15-05-2016](https://aka.ms/dtlrestapis) provider om te controleren van de beschikbare REST-API's voor de DevTest Labs-provider. De scriptgebruikers de volgende resource-ID. 

```powershell
"/subscriptions/$SubscriptionId/resourceGroups/$($LabResource.ResourceGroupName)/providers/Microsoft.DevTestLab/labs/$LabName/artifactSources/$ArtifactRepositoryName"
```
 
Het resourcetype is dat alles na 'providers' in de URI, met uitzondering van artikelen die worden vermeld in de accolades vermeld. De resourcenaam is alles zichtbaar in de accolades. Als meer dan één item wordt verwacht voor de resourcenaam, scheidt u elk item met een slash zoals we hebben gedaan. 

```powershell
$resourcetype = 'Microsoft.DevTestLab/labs/artifactSources'
$resourceName = $LabName + '/' + $ArtifactRepositoryName
```


## <a name="next-steps"></a>Volgende stappen
- [Geef op verplichte artefacten voor uw lab in Azure DevTest Labs](devtest-lab-mandatory-artifacts.md)
- [Maken van aangepaste artefacten voor uw virtuele machine van DevTest Labs](devtest-lab-artifact-author.md)
- [Met artefacten vaststellen in de testomgeving](devtest-lab-troubleshoot-artifact-failure.md)

