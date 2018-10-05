---
title: Integratie van broncodebeheer in Azure Automation
description: Dit artikel wordt de integratie van broncodebeheer met GitHub in Azure Automation beschreven.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/26/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9bbf3582da2664b6e6429677d47aad4d69a7c1bb
ms.sourcegitcommit: 4edf9354a00bb63082c3b844b979165b64f46286
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48785321"
---
# <a name="source-control-integration-in-azure-automation"></a>Integratie van broncodebeheer in Azure Automation

Broncodebeheer kunt u uw runbooks behouden in uw Automation account zijn bijgewerkt met de scripts in uw GitHub- of Azure Dev Ops resourcebeheerbibliotheek. Broncodebeheer kunt u eenvoudig samenwerken met uw team, het bijhouden van wijzigingen en Ga terug naar de eerdere versies van uw runbooks. Broncodebeheer bijvoorbeeld, kunt u verschillende branches in broncodebeheer om uw ontwikkelings-, test- of productieomgeving Automation-accounts te synchroniseren. Hiermee kunt u eenvoudig ter bevordering van code die in uw ontwikkelomgeving, zodat uw productie-Automation-account is getest.

Azure Automation biedt ondersteuning voor 3 soorten of broncodebeheer:

* GitHub
* Visual Studio teamservices (Git)
* Visual Studio teamservices (TFVC)

## <a name="pre-requisites"></a>Vereisten

* Een opslagplaats voor bronbeheer (GitHub of Visual Studio Team Services)
* De juiste [machtigingen](#personal-access-token-permissions) naar de opslagplaats voor bronbeheer
* Een [Run-As Account en -verbinding](manage-runas-account.md)

> [!NOTE]
> Resourcebeheertaken synchronisatie uitgevoerd onder de gebruikers Automation-Account en worden gefactureerd tegen hetzelfde tarief als andere Automation-taken.

## <a name="configure-source-control"></a>Configureren van broncodebeheer

Selecteer in uw Automation-Account **broncodebeheer (preview)** en klikt u op **+ toevoegen**

![Selecteer het besturingselement](./media/source-control-integration/select-source-control.png)

Kies **broncodebeheer type**, klikt u op **verifiëren**.

Controleer de pagina app-aanvraag machtigingen en klik op **accepteren**.

Op de **samenvatting van de bron-besturingselement** pagina, vult u uw gegevens in en klikt u op **opslaan**. De volgende tabel bevat een beschrijving van de beschikbare velden.

|Eigenschap  |Beschrijving  |
|---------|---------|
|Naam van de bron-besturingselement     | Een beschrijvende naam voor broncodebeheer        |
|Broncodebeheertype     | Het type van de bron van het besturingselement. De volgende opties zijn beschikbaar:</br> GitHub</br>Visual Studio teamservices (Git)</br> Visual Studio teamservices (TFVC)        |
|Opslagplaats     | De naam van de opslagplaats of het project. Deze waarde wordt opgehaald uit de opslagplaats voor bronbeheer. Voorbeeld: $/ ContosoFinanceTFVCExample         |
|Branche     | De vertakking voor het ophalen van de bronbestanden op uit. Vertakking die gericht is op is niet beschikbaar voor het type TFVC broncodebeheer.          |
|Mappad     | De map met de runbooks om te synchroniseren. Voorbeeld: /Runbooks         |
|Automatisch synchroniseren     | Hiermee schakelt u in- of uitschakelen van automatische synchronisatie wanneer een wijziging wordt aangebracht in de opslagplaats voor bronbeheer         |
|Runbook publiceren     | Indien ingesteld op **op**nadat runbooks zijn gesynchroniseerd vanuit broncodebeheer ze automatisch worden gepubliceerd.         |
|Beschrijving     | Een tekstveld om meer duidelijkheid te geven        |

![Overzicht van de bron-besturingselement](./media/source-control-integration/source-control-summary.png)

## <a name="syncing"></a>Synchroniseren...

Automatische synchronisatie configureren bij het configureren van integratie van broncodebeheer, de initiële synchronisatie wordt automatisch gestart. Als automatische synchronisatie niet is ingesteld, selecteert u de bron uit de tabel op de **besturingselement (Preview) voor de gegevensbron** pagina. Klik op **synchronisatie starten** om de synchronisatieproces te starten.  

U ziet de status van de huidige synchronisatietaak of vorige waarden door te klikken op de **taken synchroniseren** tabblad. Op de **broncodebeheer** vervolgkeuzelijst, selecteer een bron-besturingselement.

![Synchronisatiestatus](./media/source-control-integration/sync-status.png)

Op een taak te klikken, kunt u om de taakuitvoer weer te geven. Het volgende voorbeeld wordt de uitvoer van een bron control sync-taak.

```output
========================================================================================================

Azure Automation Source Control Public Preview.
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

## <a name="personal-access-token-permissions"></a>Token Pat-machtigingen

Broncodebeheer vereist minimale machtigingen voor persoonlijke toegangstokens. De volgende tabellen bevatten de minimale machtigingen die vereist zijn voor GitHub en DevOps met Azure.

### <a name="github"></a>GitHub

|Bereik  |Beschrijving  |
|---------|---------|
|**opslagplaats**     |         |
|status van-opslagplaats:     | Commit-toegangsstatus         |
|repo_deployment      | Implementatie-toegangsstatus         |
|public_repo     | Toegang tot openbare opslagplaatsen         |
|**Admin: repo_hook**     |         |
|schrijven: repo_hook     | Opslagplaats hooks schrijven         |
|lezen: repo_hook|Opslagplaats hooks lezen|

### <a name="azure-devops"></a>Azure DevOps

|Bereik  |
|---------|
|Code (lezen)     |
|Project en team (lezen)|
|Identiteit (lezen)      |
|Gebruikersprofiel (lezen)     |
|Werkitems (lezen)    |
|Verbindingen met de service (lezen, query's uitvoeren en beheren)<sup>1</sup>    |

<sup>1</sup>de machtiging voor verbindingen met de Service is alleen vereist als u automatische synchronisatie hebt ingeschakeld.

## <a name="disconnecting-source-control"></a>Verbinding met broncodebeheer verbreken

Als u wilt loskoppelen van een opslagplaats voor bronbeheer, open **besturingselement (Preview) voor de gegevensbron** onder **Accountinstellingen** in uw Automation-Account.

Selecteer het bron-besturingselement dat u wilt verwijderen. Op de **samenvatting van de bron-besturingselement** pagina, klikt u op **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

Zie [Azure Automation-runbooktypen](automation-runbook-types.md) voor meer informatie over runbooktypen, hun voordelen en beperkingen
