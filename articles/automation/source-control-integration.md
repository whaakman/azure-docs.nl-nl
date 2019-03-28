---
title: Integratie van broncodebeheer in Azure Automation
description: Dit artikel wordt de integratie van broncodebeheer met GitHub in Azure Automation beschreven.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/21/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 3b2df5b24a12f3d2ea5d8a03721c08f8d2a742ad
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58539986"
---
# <a name="source-control-integration-in-azure-automation"></a>Integratie van broncodebeheer in Azure Automation

Broncodebeheer kunt u uw runbooks behouden in uw Automation account zijn bijgewerkt met de scripts in uw GitHub- of Azure-opslagplaatsen resourcebeheerbibliotheek. Broncodebeheer kunt u eenvoudig samenwerken met uw team, het bijhouden van wijzigingen en Ga terug naar de eerdere versies van uw runbooks. Broncodebeheer bijvoorbeeld, kunt u verschillende branches in broncodebeheer om uw ontwikkelings-, test- of productieomgeving Automation-accounts te synchroniseren. Hiermee kunt u eenvoudig ter bevordering van code die in uw ontwikkelomgeving, zodat uw productie-Automation-account is getest. Integratie van broncodebeheer met automation biedt ondersteuning voor één richting worden gesynchroniseerd vanuit uw resourcebeheerbibliotheek.

Azure Automation ondersteunt drie typen van broncodebeheer:

* GitHub
* Azure Repos (Git)
* Azure Repos (TFVC)

## <a name="pre-requisites"></a>Vereisten

* Een opslagplaats voor bronbeheer (GitHub of Azure-opslagplaatsen)
* Een [Run As-Account](manage-runas-account.md)
* Zorg ervoor dat u hebt de [nieuwste Azure-modules](automation-update-azure-modules.md) in uw Automation-Account

> [!NOTE]
> Resourcebeheertaken synchronisatie uitgevoerd onder de gebruikers Automation-Account en worden gefactureerd tegen hetzelfde tarief als andere Automation-taken.

## <a name="configure-source-control---azure-portal"></a>Broncodebeheer - Azure-portal configureren

Selecteer in uw Automation-Account **broncodebeheer** en klikt u op **+ toevoegen**

![Selecteer het besturingselement](./media/source-control-integration/select-source-control.png)

Kies **broncodebeheer type**, klikt u op **verifiëren**. Een browservenster wordt geopend en vraagt u zich aanmelden, volgt u de stappen om verificatie te voltooien.

Op de **samenvatting van de bron-besturingselement** pagina, vult u uw gegevens in en klikt u op **opslaan**. De volgende tabel bevat een beschrijving van de beschikbare velden.

|Eigenschap  |Description  |
|---------|---------|
|Naam van de bron-besturingselement     | Een beschrijvende naam voor broncodebeheer        |
|Broncodebeheertype     | Het type van de bron van het besturingselement. De volgende opties zijn beschikbaar:</br> GitHub</br>Azure Repos (Git)</br> Azure Repos (TFVC)        |
|Opslagplaats     | De naam van de opslagplaats of het project. De eerste 200 opslagplaatsen worden geretourneerd. Als u wilt zoeken naar een opslagplaats, typ de naam in het veld en klikt u op **zoeken op GitHub**.|
|Branch     | De vertakking voor het ophalen van de bronbestanden op uit. Vertakking die gericht is op is niet beschikbaar voor het type TFVC broncodebeheer.          |
|Mappad     | De map met de runbooks om te synchroniseren. Voorbeeld: /Runbooks </br>*Alleen runbooks in de map die is opgegeven, worden gesynchroniseerd. Recursie wordt niet ondersteund.*        |
|Automatisch synchroniseren     | Hiermee schakelt u in- of uitschakelen van automatische synchronisatie wanneer een wijziging wordt aangebracht in de opslagplaats voor bronbeheer         |
|Runbook publiceren     | Indien ingesteld op **op**nadat runbooks zijn gesynchroniseerd vanuit broncodebeheer ze automatisch moeten worden gepubliceerd.         |
|Description     | Een tekstveld om meer duidelijkheid te geven        |

![Overzicht van de bron-besturingselement](./media/source-control-integration/source-control-summary.png)

> [!NOTE]
> Zorg ervoor dat u bent aangemeld met het juiste account bij het configureren van broncodebeheer. Als er een waarover twijfel bestaat, een nieuw tabblad geopend in uw browser en meld u af bij visualstudio.com of github.com en probeer het opnieuw verbinden broncodebeheer.

## <a name="configure-source-control---powershell"></a>Configureren van broncodebeheer - PowerShell

U kunt ook PowerShell gebruiken om te configureren van broncodebeheer in Azure Automation. Voor het configureren van broncodebeheer met de PowerShell-cmdlets, is een persoonlijk toegangstoken (PAT) nodig. U gebruikt de [New-AzureRmAutomationSourceControl](/powershell/module/AzureRM.Automation/New-AzureRmAutomationSourceControl) te maken van de verbinding. De cmdlet is vereist voor een beveiligde tekenreeks van het persoonlijke toegangstoken, voor informatie over het maken van een beveiligde tekenreeks, Zie [ConvertTo-SecureString](/powershell/module/microsoft.powershell.security/convertto-securestring?view=powershell-6).

### <a name="azure-repos-git"></a>Azure Repos (Git)

```powershell-interactive
New-AzureRmAutomationSourceControl -Name SCReposGit -RepoUrl https://<accountname>.visualstudio.com/<projectname>/_git/<repositoryname> -SourceType VsoGit -AccessToken <secureStringofPAT> -Branch master -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName> -FolderPath "/Runbooks"
```

### <a name="azure-repos-tfvc"></a>Azure Repos (TFVC)

```powershell-interactive
New-AzureRmAutomationSourceControl -Name SCReposTFVC -RepoUrl https://<accountname>.visualstudio.com/<projectname>/_versionControl -SourceType VsoTfvc -AccessToken <secureStringofPAT> -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName> -FolderPath "/Runbooks"
```

### <a name="github"></a>GitHub

```powershell-interactive
New-AzureRmAutomationSourceControl -Name SCGitHub -RepoUrl https://github.com/<accountname>/<reponame>.git -SourceType GitHub -FolderPath "/MyRunbooks" -Branch master -AccessToken <secureStringofPAT> -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName>
```

### <a name="personal-access-token-permissions"></a>Token Pat-machtigingen

Broncodebeheer vereist minimale machtigingen voor persoonlijke toegangstokens. De volgende tabellen bevatten de minimale machtigingen die vereist zijn voor GitHub en Azure-opslagplaatsen.

#### <a name="github"></a>GitHub

Ga voor meer informatie over het maken van een persoonlijk toegangstoken in GitHub naar [het maken van een persoonlijk toegangstoken voor de opdrachtregel](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/).

|Bereik  |Description  |
|---------|---------|
|**opslagplaats**     |         |
|status van-opslagplaats:     | Commit-toegangsstatus         |
|repo_deployment      | Implementatie-toegangsstatus         |
|public_repo     | Toegang tot openbare opslagplaatsen         |
|**admin:repo_hook**     |         |
|write:repo_hook     | Opslagplaats hooks schrijven         |
|read:repo_hook|Opslagplaats hooks lezen|

#### <a name="azure-repos"></a>Azure Repos

Voor meer informatie over het maken van een persoonlijk toegangstoken in de Azure-opslagplaatsen, gaat u naar [verifiëren van toegang met persoonlijke toegangstokens](/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate).

|Bereik  |
|---------|
|Code (lezen)     |
|Project en team (lezen)|
|Identiteit (lezen)      |
|Gebruikersprofiel (lezen)     |
|Werkitems (lezen)    |
|Verbindingen met de service (lezen, query's uitvoeren en beheren)<sup>1</sup>    |

<sup>1</sup> machtiging voor de verbindingen met de Service is alleen vereist als u automatische synchronisatie hebt ingeschakeld.

## <a name="syncing"></a>Synchroniseren...

Selecteer de gegevensbron uit de tabel op de **broncodebeheer** pagina. Klik op **synchronisatie starten** om de synchronisatieproces te starten.

U ziet de status van de huidige synchronisatietaak of vorige waarden door te klikken op de **taken synchroniseren** tabblad. Op de **broncodebeheer** vervolgkeuzelijst, selecteer een bron-besturingselement.

![Synchronisatiestatus](./media/source-control-integration/sync-status.png)

Op een taak te klikken, kunt u om de taakuitvoer weer te geven. Het volgende voorbeeld wordt de uitvoer van een bron control sync-taak.

```output
========================================================================================================

Azure Automation Source Control.
Supported runbooks to sync: PowerShell Workflow, PowerShell Scripts, DSC Configurations, Graphical, and Python 2.

Setting AzureRmEnvironment.

Getting AzureRunAsConnection.

Logging in to Azure...

Source control information for syncing:

[Url = https://ContosoExample.visualstudio.com/ContosoFinanceTFVCExample/_versionControl] [FolderPath = /Runbooks]

Verifying url: https://ContosoExample.visualstudio.com/ContosoFinanceTFVCExample/_versionControl

Connecting to VSTS...


Source Control Sync Summary:


2 files synced:
 - ExampleRunbook1.ps1
 - ExampleRunbook2.ps1



========================================================================================================
```

Aanvullende logboekregistratie is beschikbaar door te selecteren **alle logboeken** op de **Source Control Sync taaksamenvatting** pagina. Deze extra logboekvermeldingen kunnen helpen bij het oplossen van problemen die zich voordoen kunnen bij het gebruik van broncodebeheer.

## <a name="disconnecting-source-control"></a>Verbinding met broncodebeheer verbreken

Als u wilt loskoppelen van een opslagplaats voor bronbeheer, open **broncodebeheer** onder **Accountinstellingen** in uw Automation-Account.

Selecteer het bron-besturingselement dat u wilt verwijderen. Op de **samenvatting van de bron-besturingselement** pagina, klikt u op **verwijderen**.

## <a name="encoding"></a>Encoding

Als meerdere personen runbooks in uw resourcebeheerbibliotheek met verschillende editors bewerken zijn, is er een kans om uit te voeren in coderingsproblemen. Deze situatie kan leiden tot onjuiste tekens in uw runbook. Zie voor meer informatie over deze [algemene oorzaken van problemen met codering](/powershell/scripting/components/vscode/understanding-file-encoding#common-causes-of-encoding-issues)

## <a name="next-steps"></a>Volgende stappen

Zie [Azure Automation-runbooktypen](automation-runbook-types.md) voor meer informatie over runbooktypen, hun voordelen en beperkingen
