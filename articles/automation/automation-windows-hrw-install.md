---
title: Hybrid Runbook Worker voor Azure Automation in Windows
description: In dit artikel bevat informatie over het installeren van een Azure Automation Hybrid Runbook Worker die u gebruiken kunt voor het uitvoeren van runbooks op Windows gebaseerde computers in uw lokale datacentrum of cloudomgeving.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/25/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: e449e6e457c4fa568b5a4de5823014b4dcea82d0
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063868"
---
# <a name="deploy-a-windows-hybrid-runbook-worker"></a>Implementeren van een Windows hybride Runbook Worker

U kunt de hybride Runbook Worker-functie van Azure Automation voor het uitvoeren van runbooks rechtstreeks op de computer die als host voor de rol fungeert en op basis van bronnen in de omgeving voor het beheren van de lokale bronnen gebruiken. Runbooks zijn opgeslagen en beheerd in Azure Automation en vervolgens aan een of meer specifieke computers geleverd. Dit artikel wordt beschreven hoe u de hybride Runbook Worker installeert op een Windows-machine.

## <a name="installing-the-windows-hybrid-runbook-worker"></a>De Windows hybride Runbook Worker installeren

Als u wilt installeren en configureren van een hybride Runbook Worker van Windows, kunt u twee methoden. De aanbevolen methode gebruikt een Automation-runbook om het proces van het configureren van een Windows-computer volledig automatiseren. De tweede methode volgt een stapsgewijze procedure om handmatig te installeren en configureren van de rol.

> [!NOTE]
> Voor het beheren van de configuratie van uw servers die ondersteuning bieden voor de Hybrid Runbook Worker-rol met Desired State Configuration (DSC), moet u hen toevoegen als DSC-knooppunten.

De minimale vereisten voor een Windows hybride Runbook Worker zijn:

* WindowsServer 2012 of later.
* Windows PowerShell 4.0 of hoger ([WMF 4.0 downloaden](https://www.microsoft.com/download/details.aspx?id=40855)). We raden aan Windows PowerShell 5.1 ([WMF 5.1 downloaden](https://www.microsoft.com/download/details.aspx?id=54616)) voor een hogere mate van betrouwbaarheid.
* .NET framework 4.6.2 of hoger.
* Twee cores.
* 4 GB RAM-geheugen.
* Poort 443 (uitgaand).

Als u meer netwerkvereisten voor de hybride Runbook Worker, Zie [configureren van uw netwerk](automation-hybrid-runbook-worker.md#network-planning).

Zie voor meer informatie over de servers voorbereiden voor beheer met DSC [machines voorbereiden voor beheer door Azure Automation DSC](automation-dsc-onboarding.md).
Als u inschakelt de [Update beheeroplossing](../operations-management-suite/oms-solution-update-management.md), elke Windows-computer die verbonden met uw Azure-logboekanalyse-werkruimte wordt automatisch geconfigureerd als een hybride Runbook Worker ter ondersteuning van runbooks die zijn opgenomen in deze oplossing. Echter niet is geregistreerd met de Hybrid Worker-groepen in uw Automation-account al gedefinieerd. 

De computer kan worden toegevoegd aan een hybride Runbook Worker-groep in uw Automation-account voor de ondersteuning van Automation-runbooks, zolang u hetzelfde account voor de oplossing en het lidmaatschap van de Hybrid Runbook Worker. Deze functionaliteit is toegevoegd aan versie 7.2.12024.0 van de Hybrid Runbook Worker.

Nadat u een runbook worker implementeren, controleren [runbooks worden uitgevoerd op een hybride Runbook Worker](automation-hrw-run-runbooks.md) voor informatie over het configureren van uw runbooks voor het automatiseren van processen in uw on-premises datacentrum of andere cloudomgeving.

### <a name="automated-deployment"></a>Geautomatiseerde implementatie

Voer de volgende stappen voor het automatiseren van de installatie en configuratie van de Windows Hybrid Worker-rol:

1. Download het script New-OnPremiseHybridWorker.ps1 vanaf de [PowerShell Gallery](https://www.powershellgallery.com/packages/New-OnPremiseHybridWorker/DisplayScript) rechtstreeks van de computer waarop de hybride Runbook Worker-rol of vanaf een andere computer in uw omgeving. Kopieer het script voor het werkproces.

   Het script New-OnPremiseHybridWorker.ps1 vereist de volgende parameters tijdens het uitvoeren van:

   * *AutomationAccountName* (verplicht): de naam van uw Automation-account.
   * *AAResourceGroupName* (verplicht): de naam van de resourcegroep die is gekoppeld aan uw Automation-account.
   * *OMSResourceGroupName* (optioneel): de naam van de resourcegroep voor de Operations Management Suite-werkruimte. Als deze resourcegroep niet is opgegeven, *AAResourceGroupName* wordt gebruikt.
   * *HybridGroupName* (verplicht): de naam van een hybride Runbook Worker-groep die u opgeeft als doel voor de runbooks die ondersteuning bieden voor dit scenario.
   * *SubscriptionID* (verplicht): de Azure-abonnements-ID die uw Automation-account in.
   * *WorkspaceName* (optioneel): de naam van de werkruimte de logboekanalyse. Als u een werkruimte voor logboekanalyse niet hebt, wordt het script maakt en configureert u een.

     > [!NOTE]
     > De enige Automation-regio's ondersteund voor de integratie met logboekanalyse zijn momenteel **Australië-Zuidoost**, **VS-Oost 2**, **Zuidoost-Azië**, en  **West-Europa**. Als uw Automation-account zich niet in een van deze regio's, wordt het script maakt een werkruimte voor logboekanalyse maar waarschuwt u dat deze niet kan ze aan elkaar koppelt.

1. Open op uw computer **Windows PowerShell** van de **Start** scherm in de beheerdersmodus.
1. Blader naar de map waarin het script dat u hebt gedownload vanuit de shell van de PowerShell-opdrachtregel. Wijzig de waarden voor de parameters *- AutomationAccountName*, *- AAResourceGroupName*, *- OMSResourceGroupName*, *- HybridGroupName*, *- SubscriptionId*, en *- WorkspaceName*. Voer het script.

     > [!NOTE]
     > U wordt gevraagd om te verifiëren met Azure na het uitvoeren van het script. U *moet* aanmelden met een account dat lid is van de rol Abonnementsbeheerders en CO-beheerder van het abonnement.

   ```powershell-interactive
   .\New-OnPremiseHybridWorker.ps1 -AutomationAccountName <NameofAutomationAccount> -AAResourceGroupName <NameofResourceGroup>`
   -OMSResourceGroupName <NameofOResourceGroup> -HybridGroupName <NameofHRWGroup> `
   -SubscriptionId <AzureSubscriptionId> -WorkspaceName <NameOfLogAnalyticsWorkspace>
   ```

1. U wordt gevraagd akkoord gaan met het NuGet installeren en u wordt gevraagd om te verifiëren met uw Azure-referenties.

1. Nadat het script is voltooid, de **Hybrid Worker-groepen** pagina wordt de nieuwe groep en het aantal leden. Als het een bestaande groep, wordt het aantal leden wordt verhoogd. U kunt de groep in de lijst selecteren op de **Hybrid Worker-groepen** pagina en selecteer de **Hybrid Workers** tegel. Op de **Hybrid Workers** pagina ziet u elk lid van de groep die wordt vermeld.

### <a name="manual-deployment"></a>Handmatige implementatie

De eerste twee stappen eenmaal uitvoeren voor uw Automation-omgeving en vervolgens de resterende stappen herhalen voor elke worker-computer.

#### <a name="1-create-a-log-analytics-workspace"></a>1. Een Log Analytics-werkruimte maken

Als u al een werkruimte voor logboekanalyse niet hebt, maken via de instructies op de [beheren van uw werkruimte](../log-analytics/log-analytics-manage-access.md). Als u al hebt, kunt u een bestaande werkruimte gebruiken.

#### <a name="2-add-the-automation-solution-to-the-log-analytics-workspace"></a>2. De Automation-oplossing naar de werkruimte voor logboekanalyse toevoegen

Oplossingen voegen functionaliteit toe aan Log Analytics. De Automation-oplossing voegt u functionaliteit voor Azure Automation, inclusief ondersteuning voor hybride Runbook Worker. Wanneer u de oplossing voor uw werkruimte toevoegt, worden automatisch worker-onderdelen voor de agentcomputer die u in de volgende stap installeren wilt duwt.

Om toe te voegen de **Automation** oplossing voor uw werkruimte voor logboekanalyse, volg de instructies voor [toevoegen van een oplossing met behulp van de galerie met oplossingen](../log-analytics/log-analytics-add-solutions.md).

#### <a name="3-install-the-microsoft-monitoring-agent"></a>3. Microsoft Monitoring Agent installeren

Microsoft Monitoring Agent verbonden computers met logboekanalyse. Wanneer u de agent op uw on-premises computer installeren en met uw werkruimte verbinden, is het automatisch downloaden van de onderdelen die vereist voor hybride Runbook Worker zijn.

Volg de instructies op om de agent op de lokale computer installeert, [verbinding maken met Windows-computers met logboekanalyse](../log-analytics/log-analytics-windows-agent.md). U kunt dit proces voor meerdere computers meerdere werknemers toevoegen aan uw omgeving herhalen.

De agent verbonden is met Log Analytics, wordt weergegeven op de **verbonden bronnen** tabblad van de logboekanalyse **instellingen** pagina. U kunt controleren of de agent correct de Automation-oplossing heeft gedownload wanneer er een map met de naam **AzureAutomationFiles** in C:\Program Files\Microsoft Monitoring Agent\Agent. Als u wilt controleren welke versie van de hybride Runbook Worker, kunt u bladeren naar C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\ en noteer de \\ *versie* submap.

#### <a name="4-install-the-runbook-environment-and-connect-to-azure-automation"></a>4. De runbook-omgeving installeren en verbinding maken met Azure Automation

Wanneer u een agent met logboekanalyse toevoegt, wordt de Automation-oplossing pushes omlaag de **HybridRegistration** PowerShell-module bevat de **Add-HybridRunbookWorker** cmdlet. Met deze cmdlet kunt u de runbook-omgeving op de computer installeren en registreren met Azure Automation.

Open een PowerShell-sessie in de beheerdersmodus en voer de volgende opdrachten in de module te importeren:

```powershell-interactive
cd "C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\<version>\HybridRegistration"
Import-Module HybridRegistration.psd1
```

Voer vervolgens de **Add-HybridRunbookWorker** cmdlet met behulp van de volgende syntaxis:

```powershell-interactive
Add-HybridRunbookWorker –GroupName <String> -EndPoint <Url> -Token <String>
```

U kunt de vereiste informatie voor deze cmdlet uit de **sleutels beheren** pagina in de Azure-portal. Deze pagina openen door te selecteren de **sleutels** optie van de **instellingen** pagina in uw Automation-account.

![Pagina 'Sleutels beheren'](media/automation-hybrid-runbook-worker/elements-panel-keys.png)

* **GroupName** is de naam van de hybride Runbook Worker-groep. Als deze groep al in het Automation-account bestaat, is de huidige computer toegevoegd. Als deze groep niet bestaat, wordt deze toegevoegd.
* **Eindpunt** is de **URL** vermelding op de **sleutels beheren** pagina.
* **Token** is de **primaire TOEGANGSSLEUTEL** vermelding op de **sleutels beheren** pagina.

Gebruik voor het ontvangen van gedetailleerde informatie over de installatie van de **-uitgebreide** overschakelen met **Add-HybridRunbookWorker**.

#### <a name="5-install-powershell-modules"></a>5. PowerShell-modules installeren

Runbooks kunt u elk van de activiteiten en de cmdlets die zijn gedefinieerd in de modules die zijn geïnstalleerd in uw Azure Automation-omgeving. Deze modules worden niet automatisch geïmplementeerd op lokale computers, zodat u ze handmatig moet installeren. De uitzondering is de Azure-module die wordt standaard geïnstalleerd en biedt toegang tot de cmdlets voor alle Azure-services en activiteiten voor Azure Automation.

Omdat het voornaamste doel van de Hybrid Runbook Worker-functie voor het beheren van lokale bronnen, moet u waarschijnlijk de modules die ondersteuning bieden voor deze resources te installeren. Zie voor meer informatie over het installeren van Windows PowerShell-modules [Modules installeren](http://msdn.microsoft.com/library/dd878350.aspx). 

Modules die zijn geïnstalleerd moeten zich op een locatie waarnaar wordt verwezen door de **PSModulePath** omgevingsvariabele zodat ze automatisch door de hybride worker importeren kan. Zie voor meer informatie [wijzigen van het installatiepad PSModulePath](https://msdn.microsoft.com/library/dd878326%28v=vs.85%29.aspx).

## <a name="troubleshoot"></a>Problemen oplossen

Zie voor meer informatie over het oplossen van uw hybride Runbook Workers, [probleemoplossing Windows Hybrid Runbook Workers](troubleshoot/hybrid-runbook-worker.md#windows)

Zie voor aanvullende stappen over het oplossen van problemen met updatebeheer [updatebeheer: probleemoplossing](troubleshoot/update-management.md).

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het configureren van uw runbooks voor het automatiseren van processen in uw on-premises datacentrum of andere cloudomgeving, [runbooks worden uitgevoerd op een hybride Runbook Worker](automation-hrw-run-runbooks.md).
* Zie voor instructies over het verwijderen van Hybrid Runbook Workers [verwijderen Azure Automation Hybrid Runbook Workers](automation-hybrid-runbook-worker.md#remove-a-hybrid-runbook-worker).