---
title: Azure Automation integratie van broncodebeheer met GitHub Enterprise
description: De details van integratie met GitHub Enterprise configureren voor het bronbeheer van Automation-runbooks worden beschreven.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 09/25/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: d987e7d1e47e2e88c2f615e1d1bb5446c575ef47
ms.sourcegitcommit: 4edf9354a00bb63082c3b844b979165b64f46286
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48785195"
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-github-enterprise"></a>Azure Automation-scenario: Automation integratie van broncodebeheer met GitHub Enterprise

Automation biedt momenteel ondersteuning voor integratie van broncodebeheer, zodat u kunt het koppelen van runbooks in uw Automation-account aan een GitHub-resourcebeheerbibliotheek. Echter, klanten die hebben geïmplementeerd [GitHub Enterprise](https://enterprise.github.com/home) ter ondersteuning van hun DevOps-praktijken, ook wilt gebruiken voor het beheren van de levenscyclus van runbooks die zijn ontwikkeld voor het automatiseren van bedrijfsprocessen en service management-bewerkingen.

In dit scenario hebt u een Windows-computer in uw datacenter die is geconfigureerd als Hybrid Runbook Worker met de Azure Resource Manager-modules en Git-hulpprogramma's geïnstalleerd. De Hybrid worker-machine heeft een kloon van de lokale Git-opslagplaats. Wanneer het runbook wordt uitgevoerd op hybrid worker, wordt de Git-map gesynchroniseerd en wordt de inhoud van het runbook-bestand zijn geïmporteerd in het Automation-account.

Dit artikel wordt beschreven hoe u deze configuratie in uw omgeving Azure Automation kunt instellen. Begint u met het configureren van Automation met de beveiligingsreferenties, runbooks die zijn vereist ter ondersteuning van dit scenario en de implementatie van een Hybrid Runbook Worker in uw datacenter naar de runbooks worden uitgevoerd en toegang tot de opslagplaats met GitHub Enterprise om te synchroniseren van runbooks met uw Automation-account.

## <a name="getting-the-scenario"></a>Het scenario ophalen

Dit scenario bestaat uit twee PowerShell-runbooks die u rechtstreeks vanuit importeren kunt de [Runbookgalerie](automation-runbook-gallery.md) in Azure portal of downloaden van de [PowerShell Gallery](https://www.powershellgallery.com).

### <a name="runbooks"></a>Runbooks

Runbook | Beschrijving|
--------|------------|
Exporteren-RunAsCertificateToHybridWorker | Runbook exporteert een RunAs-certificaat van een Automation-account aan een hybrid worker zodat runbooks van de werknemer met Azure verifiëren kan als u wilt importeren van runbooks in het Automation-account.|
Synchronisatie-LocalGitFolderToAutomationAccount | Runbook worden gesynchroniseerd met de lokale Git-map op de hybride-computer en importeer vervolgens de runbook-bestanden (*.ps1) in het Automation-account.|

### <a name="credentials"></a>Referenties

Referentie | Beschrijving|
-----------|------------|
GitHRWCredential | Referentie-element u maken om de gebruikersnaam en het wachtwoord voor een gebruiker met machtigingen voor de hybrid worker.|

## <a name="installing-and-configuring-this-scenario"></a>Dit scenario installeren en configureren

### <a name="prerequisites"></a>Vereisten

1. De synchronisatie-LocalGitFolderToAutomationAccount runbook verifieert met behulp van de [uitvoeren als-account](automation-sec-configure-azure-runas-account.md).

2. Een Log Analytics-werkruimte met de Azure Automation-oplossing ingeschakeld en geconfigureerd, is ook vereist. Als u niet hebt die is gekoppeld aan het Automation-account voor het installeren en configureren van dit scenario, is deze gemaakt en geconfigureerd voor u, wanneer u de **New-OnPremiseHybridWorker.ps1** script van het hybride-runbook de werknemer.

    > [!NOTE]
    > De volgende regio's ondersteunen momenteel alleen Automation-integratie met Log Analytics - **Australië-Zuidoost**, **VS-Oost 2**, **Zuidoost-Azië**, en  **West-Europa**.

3. Een computer die kan fungeren als een toegewezen Hybrid Runbook Worker die ook als host fungeert voor de GitHub-software en onderhouden van de runbook-bestanden (*runbook*.ps1) in een bronmap in het bestandssysteem om te synchroniseren tussen GitHub en uw automatisering -account.

### <a name="import-and-publish-the-runbooks"></a>Importeren en publiceren van de runbooks

Voor het importeren van de *Export-RunAsCertificateToHybridWorker* en *synchronisatie LocalGitFolderToAutomationAccount* runbooks uit de Runbook-galerie van uw Automation-account in Azure portal, gaat u als volgt de de procedures in [Runbook importeren vanuit de Runbook Gallery](automation-runbook-gallery.md#to-import-a-runbook-from-the-runbook-gallery-with-the-azure-portal). Publiceren van de runbooks nadat ze zijn geïmporteerd in uw Automation-account.

### <a name="deploy-and-configure-hybrid-runbook-worker"></a>Implementeren en configureren van Hybrid Runbook Worker

Als u een Hybrid Runbook Worker al geïmplementeerd in uw datacenter hebt, moet u raadpleegt u de vereisten en volg de stappen van de geautomatiseerde installatie met de procedure in Azure Automation Hybrid Runbook Workers - automatiseren installeren en configureren voor [Windows](automation-windows-hrw-install.md#automated-deployment) of [Linux](automation-linux-hrw-install.md#installing-a-linux-hybrid-runbook-worker). Nadat u hebt de hybrid worker is geïnstalleerd op een computer, uitvoeren van de volgende stappen uit om de configuratie ter ondersteuning van dit scenario te voltooien.

1. Meld u aan bij de computer die als host fungeert voor de Hybrid Runbook Worker-rol met een account dat lokale beheerdersrechten heeft en maak een map voor het opslaan van de Git-runbook-bestanden. Kloon de interne Git-opslagplaats naar de map.
1. Als u nog geen een RunAs-account gemaakt of als u wilt maken van een nieuwe een specifiek voor dit doel, vanuit Azure portal gaat u naar de Automation-accounts, selecteert u uw Automation-account en maakt een [referentie-element](automation-credentials.md) die bevat de gebruikersnaam en wachtwoord voor een gebruiker met machtigingen voor de hybrid worker.
1. In uw Automation-account [bewerkt u het runbook](automation-edit-textual-runbook.md)**Export-RunAsCertificateToHybridWorker** en wijzig de waarde voor de variabele *$Password* met een sterk wachtwoord.    Nadat u de waarde wijzigen, klikt u op **publiceren** hebben de conceptversie van het runbook gepubliceerd.
1. Het runbook starten **Export-RunAsCertificateToHybridWorker**, en klik in de **Runbook starten** blade, onder de optie **uitvoeringsinstellingen** selecteert u de optie  **Hybrid Worker** en selecteer in de vervolgkeuzelijst de Hybrid worker-groep die u eerder hebt gemaakt voor dit scenario.

    Hiermee exporteert u een certificaat naar de hybrid worker zodat runbooks op de worker kan worden geverifieerd met Azure met behulp van de uitvoeren als-verbinding voor het beheer van Azure-resources (met name voor dit scenario - runbooks importeren naar het Automation-account).

1. Selecteer de Hybrid worker-groep die eerder hebt gemaakt in uw Automation-account en [opgeven van een RunAs-account](automation-hrw-run-runbooks.md#runas-account) voor de Hybrid worker-groep, en vervolgens de referentie-element dat u zojuist hebt of al hebt gemaakt. Dit zorgt ervoor dat het runbook synchroniseren Git-opdrachten kunt uitvoeren. 
1. Het runbook starten **synchronisatie LocalGitFolderToAutomationAccount**, bieden de volgende vereiste invoerparameters en in de **Runbook starten** blade, onder de optie **instellingen uitvoeren**  selecteert u de optie **Hybrid Worker** en selecteer in de vervolgkeuzelijst de Hybrid worker-groep die u eerder hebt gemaakt voor dit scenario:

   * *ResourceGroup* -de naam van de resourcegroep die is gekoppeld aan uw Automation-account
   * *AutomationAccountName* -de naam van uw Automation-account
   * *GitPath* -een lokale map of bestand op de Hybrid Runbook Worker waarbij Git is ingesteld voor het ophalen van de meest recente wijzigingen in

    Hiermee worden gesynchroniseerd met de lokale Git-map op de hybrid worker-computer en vervolgens de .ps1-bestanden importeert uit de bronmap voor het Automation-account.

1. Overzichtsgegevens van taak voor het runbook weergeven door het selecteren van de **Runbooks** -blade in uw Automation-account en selecteer vervolgens de **taken** tegel. Controleer of het met succes voltooid door het selecteren van de **alle logboeken** tegel en beoordelen van de stroom gedetailleerd logboek.

## <a name="next-steps"></a>Volgende stappen

* Zie [Azure Automation-runbooktypen](automation-runbook-types.md) voor meer informatie over runbooktypen, hun voordelen en beperkingen
* Zie [Systeemeigen PowerShell-scriptondersteuning in Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/) voor meer informatie over de functie voor PowerShelll-scriptondersteuning
