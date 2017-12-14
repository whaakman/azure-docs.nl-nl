---
title: Integratie van broncodebeheer Azure Automation met GitHub voor ondernemingen | Microsoft Docs
description: Beschrijft de details van integratie met GitHub voor ondernemingen configureren voor het besturingselement van Automation-runbooks.
services: automation
documentationCenter: 
authors: georgewallace
manager: jwhit
editor: 
ms.assetid: e01d817c-7d38-421c-adf5-647a4b526eb4
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: magoedte
ms.openlocfilehash: cf72c6d05e2872bea84b8a7218bd318d5b8c9694
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-github-enterprise"></a>Azure Automation-scenario - integratie van broncodebeheer automatisering met GitHub voor ondernemingen

Automation biedt momenteel ondersteuning voor integratie van broncodebeheer, waarmee u runbooks in uw Automation-account aan een GitHub-resourcebeheerbibliotheek koppelen.  Echter, klanten die hebben geïmplementeerd [GitHub Enterprise](https://enterprise.github.com/home) ter ondersteuning van hun DevOps-werkwijze ook wilt gebruiken voor het beheren van de levenscyclus van runbooks die zijn ontwikkeld voor het automatiseren van bedrijfsprocessen en beheerbewerkingen-service.  

In dit scenario hebt u een Windows-computer in uw datacenter die is geconfigureerd als een hybride Runbook Worker met de Azure Resource Manager-modules en Git-hulpprogramma's geïnstalleerd.  De Hybrid worker-machine heeft een kloon van de lokale Git-opslagplaats.  Wanneer het runbook wordt uitgevoerd op de hybride worker, de Git-map is gesynchroniseerd en de inhoud van het runbook-bestand zijn geïmporteerd in het Automation-account.

In dit artikel wordt beschreven hoe deze configuratie in uw Azure Automation-omgeving instellen. Begin met het configureren van automatisering met de beveiligingsreferenties, runbooks die zijn vereist ter ondersteuning van dit scenario en de implementatie van een hybride Runbook Worker in uw datacenter voeren de runbooks en toegang hebben tot uw onderneming GitHub-opslagplaats om te synchroniseren van runbooks met uw Automation-account.  


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

2. Een werkruimte van de Microsoft Operations Management Suite (OMS) met de Azure Automation-oplossing ingeschakeld en geconfigureerd, is ook vereist.  Als u niet die is gekoppeld aan het Automation-account gebruikt om te installeren en configureren van dit scenario hebt, is deze gemaakt en geconfigureerd voor u wanneer u de **nieuw OnPremiseHybridWorker.ps1** script van de hybride runbook de werknemer.        

    > [!NOTE]
    > De volgende regio's ondersteunen momenteel alleen Automation-integratie met OMS - **Australië-Zuidoost**, **VS-Oost 2**, **Zuidoost-Azië**, en **West Europa**. 

3. Een computer die kan fungeren als een toegewezen hybride Runbook Worker die ook host fungeert voor de GitHub-software en onderhouden van de runbook-bestanden (*runbook*.ps1) in een bronmap in het bestandssysteem synchronisatie tussen GitHub en uw Automation-account.

### <a name="import-and-publish-the-runbooks"></a>Importeren en publiceren van de runbooks

Voor het importeren van de *Export RunAsCertificateToHybridWorker* en *Sync LocalGitFolderToAutomationAccount* runbooks uit de Runbookgalerie van uw Automation-account in de Azure portal, volgt u de de procedures in [Runbook importeren vanuit de galerie Runbook](automation-runbook-gallery.md#to-import-a-runbook-from-the-runbook-gallery-with-the-azure-portal). Publiceer de runbooks nadat ze correct zijn geïmporteerd in uw Automation-account.

### <a name="deploy-and-configure-hybrid-runbook-worker"></a>Implementeren en configureren van de hybride Runbook Worker

Als u niet een hybride Runbook Worker al geïmplementeerd in uw datacenter hebt, Controleer de vereisten en volg de stappen van de geautomatiseerde installatie van de procedure in [Azure Automation Hybrid Runbook Workers - automatiseren installeren en Configuratie](automation-hybrid-runbook-worker.md#automated-deployment).  Zodra u de hybride worker op een computer hebt geïnstalleerd, voer de volgende stappen uit om de configuratie voor dit scenario te voltooien.

1. Meld u bij de computer die als host fungeert voor de Hybrid Runbook Worker-rol met een account met lokale beheerdersrechten en maak een map waarin de Git-runbook-bestanden.  Kloon de interne Git-opslagplaats naar de map.
2. Als u nog geen een RunAs-account gemaakt of als u wilt maken van een nieuwe een specifiek voor dit doel, vanuit de Azure-portal gaat u naar de Automation-accounts, selecteer uw Automation-account en maak een [referentieasset](automation-credentials.md) die bevat de gebruikersnaam en wachtwoord voor een gebruiker met machtigingen voor de hybride worker.  
3. Van uw Automation-account [bewerken van het runbook](automation-edit-textual-runbook.md)**Export RunAsCertificateToHybridWorker** en wijzig de waarde voor de variabele *$Password* met een sterk wachtwoord.  Nadat u de waarde wijzigen, klikt u op **publiceren** hebben de conceptversie van het runbook gepubliceerd. 
5. Het runbook starten **Export RunAsCertificateToHybridWorker**, en in de **Runbook starten** blade onder de optie **instellingen uitvoeren** selecteert u de optie  **Hybride Worker** en selecteer in de vervolgkeuzelijst de Hybrid worker-groep die u eerder hebt gemaakt voor dit scenario.  

    Hiermee exporteert u een certificaat naar de hybride worker zodat runbooks op de worker kan worden geverifieerd met Azure met behulp van de uitvoeren als-verbinding om Azure-resources (met name voor dit scenario - import runbooks voor het Automation-account) te beheren.

4. Selecteer de Hybrid worker-groep die eerder hebt gemaakt in uw Automation-account en [Geef een RunAs-account](automation-hrw-run-runbooks.md#runas-account) voor de Hybrid worker-groep en kies de referentie-element dat u zojuist hebt of al hebt gemaakt.  Dit zorgt ervoor dat het runbook Sync Git-opdrachten kunt uitvoeren. 
5. Het runbook starten **Sync LocalGitFolderToAutomationAccount**, bieden de volgende vereiste invoerparameters en in de **Runbook starten** blade onder de optie **instellingen uitvoeren**  selecteert u de optie **Hybrid Worker** en selecteer in de vervolgkeuzelijst de Hybrid worker-groep die u eerder hebt gemaakt voor dit scenario:
    * *ResourceGroup* -de naam van de resourcegroep die zijn gekoppeld aan uw Automation-account
    * *AutomationAccountName* -de naam van uw Automation-account
    * *GitPath* -de lokale map of het bestand op de hybride Runbook Worker waarbij Git is ingesteld voor het ophalen van de meest recente wijzigingen in

    Dit wordt de lokale Git-map op de computer van de werknemer hybride synchroniseren en de .ps1-bestanden vervolgens importeren uit de bronmap voor het Automation-account.

    ![Synchronisatie-LocalGitFolderToAutomationAccount Runbook starten](media/automation-scenario-source-control-integration-with-github-ent/start-runbook-synclocalgitfoldertoautoacct.png)<br>

7. Overzichtsgegevens voor het runbook taak weergeven door te selecteren in de **Runbooks** blade in uw Automation-account en selecteer vervolgens de **taken** tegel.  Bevestig het met succes voltooid door het selecteren van de **alle logboeken** tegel en beoordelen van de stroom gedetailleerd logboek.  

## <a name="next-steps"></a>Volgende stappen

-  Zie [Azure Automation-runbooktypen](automation-runbook-types.md) voor meer informatie over runbooktypen, hun voordelen en beperkingen
-  Zie [Systeemeigen PowerShell-scriptondersteuning in Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/) voor meer informatie over de functie voor PowerShelll-scriptondersteuning
