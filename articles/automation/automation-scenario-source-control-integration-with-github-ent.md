---
title: Integratie van broncodebeheer Azure Automation met GitHub voor ondernemingen
description: Beschrijft de details van integratie met GitHub voor ondernemingen configureren voor het besturingselement van Automation-runbooks.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 04/17/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 65894e40b192c6a5a226fa7a1dfb5cb0cfabb972
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-github-enterprise"></a>Azure Automation-scenario - integratie van broncodebeheer automatisering met GitHub voor ondernemingen

Automation biedt momenteel ondersteuning voor integratie van broncodebeheer, waarmee u runbooks in uw Automation-account aan een GitHub-resourcebeheerbibliotheek koppelen. Echter, klanten die hebben geïmplementeerd [GitHub Enterprise](https://enterprise.github.com/home) ter ondersteuning van hun DevOps-werkwijze ook wilt gebruiken voor het beheren van de levenscyclus van runbooks die zijn ontwikkeld voor het automatiseren van bedrijfsprocessen en beheerbewerkingen-service.

In dit scenario hebt u een Windows-computer in uw datacenter die is geconfigureerd als een hybride Runbook Worker met de Azure Resource Manager-modules en Git-hulpprogramma's geïnstalleerd. De Hybrid worker-machine heeft een kloon van de lokale Git-opslagplaats. Wanneer het runbook wordt uitgevoerd op de hybride worker, de Git-map is gesynchroniseerd en de inhoud van het runbook-bestand zijn geïmporteerd in het Automation-account.

In dit artikel wordt beschreven hoe deze configuratie in uw Azure Automation-omgeving instellen. U start door Automation configureren met de beveiligingsreferenties, runbooks die zijn vereist ter ondersteuning van dit scenario en de implementatie van een hybride Runbook Worker in uw datacenter voeren de runbooks en toegang hebben tot uw onderneming GitHub-opslagplaats om te synchroniseren van runbooks met uw Automation-account.

## <a name="getting-the-scenario"></a>Het scenario ophalen

Dit scenario bestaat uit twee PowerShell-runbooks die u rechtstreeks vanuit importeren kunt de [Runbookgalerie](automation-runbook-gallery.md) in de Azure-portal of downloaden van de [PowerShell Gallery](https://www.powershellgallery.com).

### <a name="runbooks"></a>Runbooks

Runbook | Beschrijving|
--------|------------|
Export RunAsCertificateToHybridWorker | Runbook exporteert een RunAs-certificaat van een Automation-account voor een hybride worker zodat runbooks voor het werkproces met Azure verifiëren kan om het importeren van runbooks in het Automation-account.|
Synchronisatie LocalGitFolderToAutomationAccount | Runbook wordt de lokale Git-map op de machine hybride gesynchroniseerd en vervolgens de runbook-bestanden (*.ps1) importeren in het Automation-account.|

### <a name="credentials"></a>Referenties

Referentie | Beschrijving|
-----------|------------|
GitHRWCredential | Referentie-element u maken om de gebruikersnaam en het wachtwoord voor een gebruiker met machtigingen voor de hybride worker bevatten.|

## <a name="installing-and-configuring-this-scenario"></a>Dit scenario installeren en configureren

### <a name="prerequisites"></a>Vereisten

1. De runbook-synchronisatie LocalGitFolderToAutomationAccount verifieert met behulp van de [Azure uitvoeren als-account](automation-sec-configure-azure-runas-account.md).

2. Een werkruimte voor logboekanalyse met de Azure Automation-oplossing ingeschakeld en geconfigureerd, is ook vereist. Als u niet die is gekoppeld aan het Automation-account gebruikt om te installeren en configureren van dit scenario hebt, is deze gemaakt en geconfigureerd voor u wanneer u de **nieuw OnPremiseHybridWorker.ps1** script van de hybride runbook de werknemer.

    > [!NOTE]
    > De volgende regio's ondersteunen momenteel alleen Automation-integratie met Log Analytics - **Australië-Zuidoost**, **VS-Oost 2**, **Zuidoost-Azië**, en  **West-Europa**.

3. Een computer kan fungeren als een toegewezen hybride Runbook Worker die ook fungeert als host voor de GitHub-software en onderhouden van de runbook-bestanden (*runbook*.ps1) in een bronmap in het bestandssysteem synchronisatie tussen GitHub en uw Automation account.

### <a name="import-and-publish-the-runbooks"></a>Importeren en publiceren van de runbooks

Voor het importeren van de *Export RunAsCertificateToHybridWorker* en *Sync LocalGitFolderToAutomationAccount* runbooks uit de Runbookgalerie van uw Automation-account in de Azure portal, volgt u de de procedures in [Runbook importeren vanuit de galerie Runbook](automation-runbook-gallery.md#to-import-a-runbook-from-the-runbook-gallery-with-the-azure-portal). Publiceer de runbooks nadat ze correct zijn geïmporteerd in uw Automation-account.

### <a name="deploy-and-configure-hybrid-runbook-worker"></a>Implementeren en configureren van de hybride Runbook Worker

Als er geen een hybride Runbook Worker al geïmplementeerd in uw datacenter, moet u de vereisten doornemen en de geautomatiseerde installatie stappen met de procedure in Azure Automation Hybrid Runbook Workers - automatiseren installeren en configureren voor [Windows](automation-windows-hrw-install.md#automated-deployment) of [Linux](automation-linux-hrw-install.md#installing-linux-hybrid-runbook-worker). Zodra u de hybride worker op een computer hebt geïnstalleerd, voer de volgende stappen uit om de configuratie voor dit scenario te voltooien.

1. Meld u bij de computer die als host fungeert voor de Hybrid Runbook Worker-rol met een account met lokale beheerdersrechten en maak een map waarin de Git-runbook-bestanden. Kloon de interne Git-opslagplaats naar de map.
1. Als u nog geen een RunAs-account gemaakt of als u wilt maken van een nieuwe een specifiek voor dit doel, vanuit de Azure-portal gaat u naar de Automation-accounts, selecteert u uw Automation-account en maken een [referentieasset](automation-credentials.md) die bevat de gebruikersnaam en wachtwoord voor een gebruiker met machtigingen voor de hybride worker.
1. Van uw Automation-account [bewerken van het runbook](automation-edit-textual-runbook.md)**Export RunAsCertificateToHybridWorker** en wijzig de waarde voor de variabele *$Password* met een sterk wachtwoord.  Nadat u de waarde wijzigen, klikt u op **publiceren** hebben de conceptversie van het runbook gepubliceerd.
1. Het runbook starten **Export RunAsCertificateToHybridWorker**, en in de **Runbook starten** blade onder de optie **instellingen uitvoeren** selecteert u de optie  **Hybride Worker** en selecteer in de vervolgkeuzelijst de Hybrid worker-groep die u eerder hebt gemaakt voor dit scenario.

    Hiermee exporteert u een certificaat naar de hybride worker zodat runbooks op de worker kan worden geverifieerd met Azure met behulp van de uitvoeren als-verbinding om Azure-resources (met name voor dit scenario - import runbooks voor het Automation-account) te beheren.

1. Selecteer de Hybrid worker-groep die eerder hebt gemaakt in uw Automation-account en [Geef een RunAs-account](automation-hrw-run-runbooks.md#runas-account) voor de Hybrid worker-groep en kies de referentie-element dat u zojuist hebt of al hebt gemaakt. Dit zorgt ervoor dat het runbook Sync Git-opdrachten kunt uitvoeren. 
1. Het runbook starten **Sync LocalGitFolderToAutomationAccount**, bieden de volgende vereiste invoerparameters en in de **Runbook starten** blade onder de optie **instellingen uitvoeren**  selecteert u de optie **Hybrid Worker** en selecteer in de vervolgkeuzelijst de Hybrid worker-groep die u eerder hebt gemaakt voor dit scenario:

   * *ResourceGroup* -de naam van de resourcegroep die zijn gekoppeld aan uw Automation-account
   * *AutomationAccountName* -de naam van uw Automation-account
   * *GitPath* -de lokale map of het bestand op de hybride Runbook Worker waarbij Git is ingesteld voor het ophalen van de meest recente wijzigingen in

    Hiermee wordt de lokale Git-map op de computer van de werknemer hybride gesynchroniseerd en importeert vervolgens de .ps1-bestanden uit de bronmap voor het Automation-account.

1. Overzichtsgegevens voor het runbook taak weergeven door te selecteren in de **Runbooks** blade in uw Automation-account en selecteer vervolgens de **taken** tegel. Bevestig het met succes voltooid door het selecteren van de **alle logboeken** tegel en beoordelen van de stroom gedetailleerd logboek.

## <a name="next-steps"></a>Volgende stappen

* Zie [Azure Automation-runbooktypen](automation-runbook-types.md) voor meer informatie over runbooktypen, hun voordelen en beperkingen
* Zie [Systeemeigen PowerShell-scriptondersteuning in Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/) voor meer informatie over de functie voor PowerShelll-scriptondersteuning
