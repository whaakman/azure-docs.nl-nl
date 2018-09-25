---
title: Integratie van broncodebeheer in Azure Automation
description: Dit artikel wordt de integratie van broncodebeheer met GitHub in Azure Automation beschreven.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/17/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: c2d13a409d095bca64da781e5c5ca58553f9710c
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47047588"
---
# <a name="source-control-integration-in-azure-automation"></a>Integratie van broncodebeheer in Azure Automation

Broncodebeheer kunt u uw runbooks behouden in uw account zijn up-to-date met de scripts in uw resourcebeheerbibliotheek GitHub of Azure Dev Ops-automatisering. Broncodebeheer kunt u eenvoudig samenwerken met uw team, het bijhouden van wijzigingen en Ga terug naar de eerdere versies van uw runbooks. Bijvoorbeeld, kunt broncodebeheer u verschillende branches in broncodebeheer aan uw ontwikkelings-, test- of productieomgeving Automation-accounts, zodat u eenvoudig kunt ter bevordering van code die in uw ontwikkelomgeving, zodat uw productie-automatisering is getest synchroniseren -account.

Azure Automation biedt ondersteuning voor 3 soorten of broncodebeheer:

* GitHub
* Visual Studio teamservices (Git)
* Visual Studio teamservices (TFVC)

## <a name="pre-requisites"></a>Vereisten

* Een opslagplaats voor bronbeheer (GitHub of Visual Studio Team Services)
* Een [Run-As Account en -verbinding](manage-runas-account.md)

> [!NOTE]
> Resourcebeheertaken synchronisatie uitgevoerd onder de gebruikers Automation-Account en worden gefactureerd tegen hetzelfde tarief als andere Automation-taken.

## <a name="configure-source-control"></a>Configureren van broncodebeheer

Selecteer in uw Automation-Account **broncodebeheer (preview)** en klikt u op **+ toevoegen**

![Selecteer het besturingselement](./media/source-control-integration/select-source-control.png)

Kies **broncodebeheer type** , klikt u op **verifiëren**.

Controleer de pagina app-aanvraag machtigingen en klik op **accepteren**.

Op de **samenvatting van de bron-besturingselement** pagina, vult u uw gegevens in en klikt u op **opslaan**. De volgende tabel bevat een beschrijving van de beschikbare velden.

|Eigenschap  |Beschrijving  |
|---------|---------|
|Naam van de bron-besturingselement     | Een beschrijvende naam voor broncodebeheer        |
|Broncodebeheertype     | Het type van de bron van het besturingselement. De volgende opties zijn beschikbaar:</br> GitHub</br>Visual Studio teamservices (Git)</br>Visual Studio teamservices (TFVC)        |
|Opslagplaats     | De naam van de opslagplaats of het project. Dit is opgehaald uit de opslagplaats voor bronbeheer. Voorbeeld: $/ ContosoFinanceTFVCExample         |
|Branche     | De vertakking voor het ophalen van de bronbestanden op uit. Vertakking die gericht is op is niet beschikbaar voor het type TFVC broncodebeheer.          |
|Mappad     | De map met de runbooks om te synchroniseren. Voorbeeld: /Runbooks         |
|Automatisch synchroniseren     | Hiermee schakelt u in- of uitschakelen van automatische synchronisatie wanneer een wijziging wordt aangebracht in de opslagplaats voor bronbeheer         |
|Runbook publiceren     | Indien ingesteld op **op**nadat runbooks zijn gesynchroniseerd vanuit broncodebeheer ze automatisch worden gepubliceerd.         |
|Beschrijving     | Een tekstveld om meer duidelijkheid te geven        |

![Overzicht van de bron-besturingselement](./media/source-control-integration/source-control-summary.png)

## <a name="syncing"></a>Synchroniseren...

Als automatische synchronisatie is ingesteld bij het configureren van integratie van broncodebeheer, zou de initiële synchronisatie met een automatisch gestart. Als automatische synchronisatie niet is ingesteld, selecteert u de bron uit de tabel op de **besturingselement (Preview) voor de gegevensbron** pagina. Klik op **synchronisatie starten** om de synchronisatieproces te starten.  

U ziet de status van de huidige synchronisatietaak of vorige waarden door te klikken op de **taken synchroniseren** tabblad. Op de **broncodebeheer** vervolgkeuzelijst, selecteer een bron-besturingselement.

![Synchronisatiestatus](./media/source-control-integration/sync-status.png)

Op een taak te klikken, kunt u om de taakuitvoer weer te geven. Hier volgt een voorbeeld van de uitvoer van een bron control sync-taak.

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

## <a name="disconnecting-source-control"></a>Verbinding met broncodebeheer verbreken

Als u wilt loskoppelen van een opslagplaats voor bronbeheer, open **besturingselement (Preview) voor de gegevensbron** onder **Accountinstellingen** in uw Automation-Account.

Selecteer het bron-besturingselement dat u wilt verwijderen. Op de **samenvatting van de bron-besturingselement** pagina, klikt u op **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

Zie [Azure Automation-runbooktypen](automation-runbook-types.md) voor meer informatie over runbooktypen, hun voordelen en beperkingen
