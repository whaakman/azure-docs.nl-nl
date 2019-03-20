---
title: Multi-VM-omgevingen en PaaS-resources maken met Azure Resource Manager-sjablonen | Microsoft Docs
description: Meer informatie over het maken van multi-VM-omgevingen en PaaS-resources in Azure DevTest Labs van een Azure Resource Manager-sjabloon
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2018
ms.author: spelluru
ms.openlocfilehash: ebe5c65f701c0a1c7c02182800a35bfbeed5b0be
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58181381"
---
# <a name="create-multi-vm-environments-and-paas-resources-with-azure-resource-manager-templates"></a>Multi-VM-omgevingen en PaaS-resources met Azure Resource Manager-sjablonen maken

De [Azure-portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) kunt u eenvoudig [één virtuele machine tegelijk toevoegen aan een lab](https://docs.microsoft.com/azure/devtest-lab/devtest-lab-add-vm). Als de omgeving meerdere virtuele machines bevat, moet elke virtuele machine echter worden afzonderlijk gemaakt. Voor scenario's zoals een Web-app voor meerdere lagen of een SharePoint-farm, een methode nodig om toe te staan voor het maken van meerdere virtuele machines in één stap. Met behulp van Azure Resource Manager-sjablonen, kunt u nu definieert de infrastructuur en configuratie van uw Azure-oplossing en herhaaldelijk implementeren meerdere virtuele machines in een consistente status. Deze functie biedt de volgende voordelen:

- Azure Resource Manager-sjablonen worden geladen rechtstreeks vanuit uw resourcebeheerbibliotheek (GitHub of Azure DevOps Services Git).
- Wanneer geconfigureerd, uw gebruikers een omgeving kunnen maken door het verzamelen van een Azure Resource Manager-sjabloon via Azure portal, net als bij andere soorten [VM-basissen](./devtest-lab-comparing-vm-base-image-types.md).
- Azure PaaS-resources kunnen worden ingericht in een omgeving van een Azure Resource Manager-sjabloon naast de IaaS-VM's.
- De kosten van omgevingen kan worden bijgehouden in het lab naast de afzonderlijke virtuele machines die zijn gemaakt door andere soorten databases.
- PaaS-resources worden gemaakt en wordt weergegeven in de kosten bijhouden; virtuele machine automatisch afsluiten is echter niet van toepassing op PaaS-resources.

Meer informatie over de vele [voordelen van het gebruik van Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#the-benefits-of-using-resource-manager) als u wilt implementeren, bijwerken of verwijderen van al uw labresources in één bewerking.

> [!NOTE]
> Wanneer u een Resource Manager-sjabloon als uitgangspunt gebruikt om meer lab VM's te maken, zijn er enkele verschillen in gedachten moeten houden of Multi-VM's of één voor virtuele machines te maken. [Een virtuele machine van Azure Resource Manager-sjabloon gebruiken](devtest-lab-use-resource-manager-template.md) worden deze verschillen in meer detail uitgelegd.
>

## <a name="devtest-labs-public-environments"></a>Openbare omgevingen met DevTest Labs
Azure DevTest Labs is een [openbare opslagplaats van Azure Resource Manager-sjablonen](https://github.com/Azure/azure-devtestlab/tree/master/Environments) die u kunt gebruiken om omgevingen te maken zonder verbinding maken met een externe bron van GitHub zelf. Deze opslagplaats bevat veelgebruikte sjablonen, zoals Azure Web Apps, Service Fabric-Cluster en development environment van de SharePoint-Farm. Deze functie is vergelijkbaar met de openbare opslagplaats voor artefacten die is opgenomen voor elke lab die u maakt. De opslagplaats van de omgeving kunt u snel aan de slag met vooraf opgestelde omgevingssjablonen met minimale invoer parameters, zodat u met een goede aan de slag te gaan ervaring te bieden voor PaaS-resources binnen labs. Zie voor meer informatie, [configureren en het gebruik openbare omgevingen in DevTest Labs](devtest-lab-configure-use-public-environments.md).

## <a name="configure-your-own-template-repositories"></a>Configureren van uw eigen sjabloon-opslagplaatsen
Als een van de aanbevolen procedures met infrastructure-as-code en configuratie als code, moeten omgevingssjablonen worden beheerd in broncodebeheer. Azure DevTest Labs met deze procedure volgt en alle sjablonen van Azure Resource Manager rechtstreeks vanuit uw GitHub- of Azure DevOps Services Git-opslagplaatsen laadt. Als gevolg hiervan kunnen Resource Manager-sjablonen worden gebruikt voor de volledige release-cyclus, vanuit de testomgeving aan de productie-omgeving.

Bekijk de sjablonen die zijn gemaakt door de DevTest Labs-team in de [openbare GitHub-opslagplaats](https://github.com/Azure/azure-devtestlab/tree/master/Environments). In deze openbare opslagplaats, kunt u de sjablonen die worden gedeeld met anderen kunt u rechtstreeks gebruiken of pas ze aan uw eigen wensen aan weergeven. Nadat u de sjabloon hebt gemaakt, wordt dit opslaan in deze opslagplaats om deze te delen met anderen. U kunt ook uw eigen Git-opslagplaats met sjablonen die kunnen worden gebruikt voor het instellen van omgevingen in de cloud instellen. 

Er zijn een aantal regels voor het ordenen van uw Azure Resource Manager-sjablonen in een opslagplaats:

- Het bestand master sjabloon moet de naam `azuredeploy.json`. 

    ![Sleutel Azure Resource Manager-sjabloonbestanden](./media/devtest-lab-create-environment-from-arm/master-template.png)

- Als u gebruiken de parameterwaarden die zijn gedefinieerd in een parameterbestand wilt, de parameter-bestand moet de naam `azuredeploy.parameters.json`.
- U kunt de parameters `_artifactsLocation` en `_artifactsLocationSasToken` kan de parametersLink URI-waarde, zodat u DevTest Labs geneste sjablonen automatisch worden beheerd. Zie voor meer informatie, [hoe Azure DevTest Labs vergemakkelijkt geneste Resource Manager sjabloonimplementaties voor testomgevingen](https://blogs.msdn.microsoft.com/devtestlab/2017/05/23/how-azure-devtest-labs-makes-nested-arm-template-deployments-easier-for-testing-environments/).
- Metagegevens kan worden gedefinieerd om de weergavenaam van sjabloon en beschrijving te geven. Deze metagegevens moet zich in een bestand met de naam `metadata.json`. Het volgende voorbeeld-metagegevensbestand ziet u hoe u de weergavenaam en beschrijving opgeven: 

    ```json
    { 
        "itemDisplayName": "<your template name>", 
        "description": "<description of the template>" 
    }
    ```

De volgende stappen helpen u bij het toevoegen van een opslagplaats aan uw testomgeving met behulp van de Azure portal. 

1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer **alle Services**, en selecteer vervolgens **DevTest Labs** in de lijst.
1. Selecteer de gewenste lab in de lijst met labs.   
1. Op van het lab **overzicht** venster **configuratie en het beleid**.

    ![Configuratie en beleid](./media/devtest-lab-create-environment-from-arm/configuration-and-policies-menu.png)

1. Uit de **configuratie en het beleid** instellingen in de lijst met **opslagplaatsen**. De **opslagplaatsen** deelvenster geeft een lijst van de opslagplaatsen die zijn toegevoegd aan het lab. Een opslagplaats met de naam `Public Repo` automatisch is gegenereerd voor alle labs en verbinding maakt met de [DevTest Labs GitHub-opslagplaats](https://github.com/Azure/azure-devtestlab) bevat verschillende VM-artefacten voor uw gebruik.

    ![Openbare opslagplaats](./media/devtest-lab-create-environment-from-arm/public-repo.png)

1. Selecteer **toevoegen +** om toe te voegen van de opslagplaats voor uw Azure Resource Manager-sjablonen.
1. Als de tweede **opslagplaatsen** deelvenster wordt geopend, voert u de benodigde gegevens als volgt:
    - **Naam** -Geef de naam van de opslagplaats die wordt gebruikt in het lab.
    - **GIT-kloon-URL** -HTTPS GIT-kloon-URL invoeren van GitHub of Azure DevOps-Services.  
    - **Vertakking** -vertakking invoeren voor toegang tot de Sjabloondefinities van uw Azure Resource Manager-. 
    - **Persoonlijk toegangstoken** -het persoonlijke toegangstoken veilig toegang krijgen tot de opslagplaats wordt gebruikt. Als u uw token van Azure DevOps-Services, selecteer  **&lt;uwnaam >> Mijn profiel > Beveiliging > token voor openbare toegang**. Als u uw token vanuit GitHub, selecteert u uw avatar gevolgd door het selecteren van **instellingen > token voor openbare toegang**. 
    - **Mappaden** -met een van de twee invoervelden, voer het pad dat begint met een slash - / - en is ten opzichte van de Git-kloon URI naar een artefact definities (eerste invoerveld) of de Sjabloondefinities van uw Azure Resource Manager- .   
    
        ![Openbare opslagplaats](./media/devtest-lab-create-environment-from-arm/repo-values.png)


1. Wanneer alle vereiste velden worden ingevoerd en de validatie, selecteer **opslaan**.

De volgende sectie helpt u bij het maken van omgevingen van een Azure Resource Manager-sjabloon.

## <a name="create-an-environment-from-a-resource-manager-template-using-the-azure-portal"></a>Een omgeving maken van een Resource Manager-sjabloon met behulp van de Azure-portal

Nadat een opslagplaats voor Azure Resource Manager-sjablonen in het lab is geconfigureerd, kan uw labgebruikers een omgeving met behulp van Azure portal met de volgende stappen kunnen maken:

1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer **alle Services**, en selecteer vervolgens **DevTest Labs** in de lijst.
1. Selecteer de gewenste lab in de lijst met labs.   
1. Selecteer in het deelvenster van de testomgeving **toevoegen +**.
1. De **kiest u een base** deelvenster geeft de basisinstallatiekopieën kunt u met de Azure Resource Manager-sjablonen als eerste weergegeven. Selecteer de gewenste Azure Resource Manager-sjabloon.

    ![Een basis kiezen](./media/devtest-lab-create-environment-from-arm/choose-a-base.png)
  
1. Op de **toevoegen** deelvenster, voer de **omgevingsnaam** waarde. De omgevingsnaam van de is wat wordt weergegeven voor uw gebruikers in het lab. De resterende velden zijn gedefinieerd in de Azure Resource Manager-sjabloon. Als de standaardwaarden zijn gedefinieerd in de sjabloon of de `azuredeploy.parameter.json` bestand aanwezig is, de standaardwaarden worden weergegeven in de invoervelden. Voor de parameters van het type *beveiligde tekenreeks*, kunt u de geheimen in uw Azure key vault. Zie voor meer informatie over het opslaan van geheimen in een key vault en het gebruik ervan bij het maken van de labresources, [Store geheimen in Azure Key Vault](devtest-lab-store-secrets-in-key-vault.md).  

    ![Deelvenster toevoegen](./media/devtest-lab-create-environment-from-arm/add.png)

    > [!NOTE]
    > Er zijn verschillende parameterwaarden die - zelfs als opgegeven - als de lege waarden worden weergegeven. Dus als gebruikers die waarden voor parameters in een Azure Resource Manager-sjabloon toewijst, DevTest Labs biedt niet de waarden weergeven. Lege velden, in plaats daarvan worden weergegeven waar labgebruikers een waarde moeten invoeren bij het maken van de omgeving.
    > 
    > - GEN-UNIQUE
    > - GEN-UNIQUE-[N]
    > - ALG-SSH-PUB-KEY
    > - GEN-PASSWORD 
 
1. Selecteer **toevoegen** om de omgeving te maken. De omgeving begint onmiddellijk wordt ingericht met de status weergegeven de **mijn virtuele machines** lijst. Een nieuwe resourcegroep wordt automatisch gemaakt door lab voor het inrichten van alle resources die zijn gedefinieerd in de Azure Resource Manager-sjabloon.
1. Zodra de omgeving is gemaakt, selecteert u de omgeving in de **mijn virtuele machines** lijst opent u het deelvenster van de groep resource en alle resources die zijn ingericht in de omgeving te bladeren.
    
    ![Mijn lijst met virtuele machines](./media/devtest-lab-create-environment-from-arm/all-environment-resources.png)
   
   De omgeving zodanig dat alleen de lijst met virtuele machines die zijn ingericht in de omgeving weergeven, kunt u ook uitbreiden.
    
    ![Mijn lijst met virtuele machines](./media/devtest-lab-create-environment-from-arm/my-vm-list.png)

1. Klik op een van de omgevingen om weer te geven van de beschikbare acties -, zoals het toepassen van artefacten, Bezig met koppelen van gegevensschijven, veranderende tijd voor automatisch afsluiten en meer.

    ![Omgeving-acties](./media/devtest-lab-create-environment-from-arm/environment-actions.png)

## <a name="automate-deployment-of-environments"></a>Automatiseer de implementatie van omgevingen
Azure DevTest Labs biedt de mogelijkheid om te gebruiken een [Management Resource Manager-sjabloon](../azure-resource-manager/resource-group-authoring-templates.md) een omgeving maken met een set met resources in het lab. Deze omgevingen kunnen alle Azure-resources dat kunnen worden gemaakt met behulp van Resource Manager-sjablonen bevatten. DevTest Labs-omgevingen kunnen gebruikers gemakkelijk ingewikkelde infrastructuren implementeren in een consistente manier binnen de grenzen van het testlab. Op dit moment een omgeving toe te voegen aan een lab met behulp van de Azure-portal is uitvoerbaar als één keer te maken, maar in een ontwikkeling of tests situatie, waarbij meerdere creaties optreden, een automatische implementatie maakt een verbeterde ervaring voor.

Voltooid de volgende stappen uit de [configureren van uw eigen sjabloon opslagplaatsen](#configure-your-own-template-repositories) sectie voordat u doorgaat: 

1. Maak de Resource Manager-sjabloon die definieert de resources die worden gemaakt. 
2. Stel de Resource Manager-sjabloon in Git een opslagplaats. 
3. Verbinding maken met de Git-opslagplaats aan het lab. 

### <a name="powershell-script-to-deploy-the-resource-manager-template"></a>PowerShell-script voor het implementeren van de Resource Manager-sjabloon
Het PowerShell-script opslaan in de volgende sectie op uw harde schijf (bijvoorbeeld: deployenv.ps1) en voer het script nadat u waarden voor SubscriptionId, ResourceGroupName, LabName, RepositoryName opgeven, TemplateName (map) in de Git-repo EnvironmentName.

```powershell
./deployenv.ps1 -SubscriptionId "000000000-0000-0000-0000-0000000000000" -LabName "mydevtestlab" -ResourceGroupName "mydevtestlabRG994248" -RepositoryName "SP Repository" -TemplateName "My Environment template name" -EnvironmentName "SPResourceGroupEnv"  
```

#### <a name="sample-script"></a>Voorbeeldscript
Hier volgt het voorbeeld van een script te maken van een omgeving in uw testomgeving. De opmerkingen in het script kunnen u beter begrijpen van het script. 

```powershell
#Requires -Version 3.0
#Requires -Module AzureRM.Resources

[CmdletBinding()]

param (
# ID of the Azure Subscription where the lab is created.
[string] [Parameter(Mandatory=$true)] $SubscriptionId,

# Name of the lab (existing) in which to create the environment.
[string] [Parameter(Mandatory=$true)] $LabName,

# Name of the connected repository in the lab. 
[string] [Parameter(Mandatory=$true)] $RepositoryName,

# Name of the template (folder name in the Git repository) based on which the environment will be created.
[string] [Parameter(Mandatory=$true)] $TemplateName,

# Name of the environment to be created in the lab.
[string] [Parameter(Mandatory=$true)] $EnvironmentName,

# The parameters to be passed to the template. Each parameter is prefixed with “-param_”. 
# For example, if the template has a parameter named “TestVMName” with a value of “MyVMName”, the string in $Params will have the form: `-param_TestVMName MyVMName`. 
# This convention allows the script to dynamically handle different templates.
[Parameter(ValueFromRemainingArguments=$true)]
    $Params
)

# Save this script as the deployenv.ps1 file
# Run the script after you specify values for SubscriptionId, ResourceGroupName, LabName, RepositoryName, TemplateName (folder) in the Git repo, EnvironmentName
# ./deployenv.ps1 -SubscriptionId "000000000-0000-0000-0000-0000000000000" -LabName "mydevtestlab" -ResourceGroupName "mydevtestlabRG994248" -RepositoryName "SP Repository" -TemplateName "My Environment template name" -EnvironmentName "SPResourceGroupEnv"    

# Comment this statement to completely automate the environment creation.    
# Sign in to Azure. 
Connect-AzureRmAccount

# Select the subscription that has the lab.  
Set-AzureRmContext -SubscriptionId $SubscriptionId | Out-Null

# Get information about the user, specifically the user ID, which is used later in the script.  
$UserId = $((Get-AzureRmADUser -UserPrincipalName (Get-AzureRmContext).Account).Id.Guid)
        
# Get information about the lab such as lab location. 
$lab = Get-AzureRmResource -ResourceType "Microsoft.DevTestLab/labs" -Name $LabName -ResourceGroupName $ResourceGroupName 
if ($lab -eq $null) { throw "Unable to find lab $LabName in subscription $SubscriptionId." } 
    
# Get information about the repository in the lab. 
$repository = Get-AzureRmResource -ResourceGroupName $lab.ResourceGroupName `
    -ResourceType 'Microsoft.DevTestLab/labs/artifactsources' `
    -ResourceName $LabName `
    -ApiVersion 2016-05-15 `
    | Where-Object { $RepositoryName -in ($_.Name, $_.Properties.displayName) } `
    | Select-Object -First 1
if ($repository -eq $null) { throw "Unable to find repository $RepositoryName in lab $LabName." } 

# Get information about the Resource Manager template based on which the environment will be created. 
$template = Get-AzureRmResource -ResourceGroupName $lab.ResourceGroupName `
    -ResourceType "Microsoft.DevTestLab/labs/artifactSources/armTemplates" `
    -ResourceName "$LabName/$($repository.Name)" `
    -ApiVersion 2016-05-15 `
    | Where-Object { $TemplateName -in ($_.Name, $_.Properties.displayName) } `
    | Select-Object -First 1
if ($template -eq $null) { throw "Unable to find template $TemplateName in lab $LabName." } 

# Build the template parameters with parameter name and values.     
$parameters = Get-Member -InputObject $template.Properties.contents.parameters -MemberType NoteProperty | Select-Object -ExpandProperty Name
$templateParameters = @()

# The custom parameters need to be extracted from $Params and formatted as name/value pairs.
$Params | ForEach-Object {
    if ($_ -match '^-param_(.*)' -and $Matches[1] -in $parameters) {
        $name = $Matches[1]                
    } elseif ( $name ) {
        $templateParameters += @{ "name" = "$name"; "value" = "$_" }
        $name = $null #reset name variable
    }
}

# Once name/value pairs are isolated, create an object to hold the necessary template properties
$templateProperties = @{ "deploymentProperties" = @{ "armTemplateId" = "$($template.ResourceId)"; "parameters" = $templateParameters }; } 

# Now, create or deploy the environment in the lab by using the New-AzureRmResource command. 
New-AzureRmResource -Location $Lab.Location `
    -ResourceGroupName $lab.ResourceGroupName `
    -Properties $templateProperties `
    -ResourceType 'Microsoft.DevTestLab/labs/users/environments' `
    -ResourceName "$LabName/$UserId/$EnvironmentName" `
    -ApiVersion '2016-05-15' -Force 
 
Write-Output "Environment $EnvironmentName completed."
```

U kunt ook Azure CLI gebruiken om resources met Resource Manager-sjablonen te implementeren. Zie voor meer informatie, [resources implementeren met Resource Manager-sjablonen en Azure CLI](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-cli).

> [!NOTE]
> Alleen een gebruiker met machtigingen voor lab-eigenaar kan VM's van Resource Manager-sjabloon maken met behulp van Azure PowerShell. Als u wilt automatiseren met behulp van Resource Manager-sjabloon maken van VM en u alleen machtigingen hebt, kunt u de [ **az lab vm maken** opdracht in de CLI](https://docs.microsoft.com/cli/azure/lab/vm#az-lab-vm-create).

### <a name="general-limitations"></a>Algemene beperkingen 

Houd rekening met deze beperkingen bij het gebruik van Resource Manager-sjabloon in DevTest Labs:

- Een Resource Manager-sjabloon die u maakt mag niet verwijzen naar bestaande resources. het kan alleen verwijzen naar nieuwe resources. Worden bovendien, als u een bestaande Resource Manager-sjabloon die u normaal gesproken buiten DevTest Labs implementeren en deze verwijzingen naar bestaande resources bevat, het kan niet gebruikt in het lab.

   De enige uitzondering hierop is dat u **kunt** verwijzen naar een bestaand virtueel netwerk. 

- Formules kunnen niet worden gemaakt op basis van lab-VM's die zijn gemaakt op basis van een Resource Manager-sjabloon. 

- Aangepaste installatiekopieën kunnen niet worden gemaakt op basis van lab-VM's die zijn gemaakt op basis van een Resource Manager-sjabloon. 

- De meeste beleidsregels worden niet geëvalueerd bij het implementeren van Resource Manager-sjablonen.

   U kunt bijvoorbeeld mogelijk een lab wordt beleid op te geven dat een gebruiker alleen vijf virtuele machines kunt maken. Een gebruiker kan echter een Resource Manager-sjabloon die wordt gemaakt van tientallen VM's implementeren. Beleidsregels die niet zijn geëvalueerd zijn onder andere:

   - Aantal virtuele machines per gebruiker
   - Aantal premium-VM's per lab-gebruiker
   - Aantal premium-schijven per lab-gebruiker


### <a name="configure-environment-resource-group-access-rights-for-lab-users"></a>Omgeving resource group-toegangsrechten voor labgebruikers configureren

Labgebruikers kunnen een Resource Manager-sjabloon implementeren. Maar ze hebben standaard de toegangsrechten lezer, wat betekent dat ze niet de resources in een omgeving resourcegroep wijzigen. Ze kunnen bijvoorbeeld stoppen of starten van hun resources.

Volg deze stappen voor uw labgebruikers Inzender toegangsrechten te verlenen. Vervolgens, wanneer ze een Resource Manager-sjabloon implementeren, ze zich de resources in die omgeving bewerken. 


1. Op van uw lab **overzicht** venster **configuratie en het beleid**.
1. Selecteer **Lab instellingen**.
1. Selecteer in het deelvenster instellingen Lab **Inzender** schrijven om machtigingen te verlenen aan gebruikers van de testomgeving.

    ![Gebruikerstoegangsrechten lab configureren](./media/devtest-lab-create-environment-from-arm/configure-access-rights.png)

1. Selecteer **Opslaan**.

## <a name="next-steps"></a>Volgende stappen
* Nadat een virtuele machine is gemaakt, kunt u verbinding met de virtuele machine met selecteren **Connect** management in het deelvenster van de virtuele machine.
* Resources weergeven en beheren in een omgeving door het selecteren van de omgeving in de **mijn virtuele machines** lijst in uw testomgeving. 
* Verken de [Azure Resource Manager-sjablonen van Azure Quickstart-sjablonen](https://github.com/Azure/azure-quickstart-templates).
