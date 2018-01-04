---
title: Azure Automation Hybrid Runbook Workers | Microsoft Docs
description: In dit artikel bevat informatie over het installeren en gebruiken van Hybrid Runbook Worker die is een functie van Azure Automation kunt u runbooks uitvoeren op machines in uw lokale datacentrum of de cloudprovider.
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: tysonn
ms.assetid: 06227cda-f3d1-47fe-b3f8-436d2b9d81ee
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 0fcf7485d64d2e947be1730d168ed0a645a50714
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="automate-resources-in-your-data-center-or-cloud-with-hybrid-runbook-worker"></a>Automatiseren van bronnen in uw datacenter of de cloud met Hybrid Runbook Worker
Runbooks in Azure Automation heeft geen toegang tot resources in andere clouds of in uw on-premises omgeving omdat ze worden uitgevoerd in de Azure-cloud.  De Hybrid Runbook Worker-functie van Azure Automation kunt u direct op de computer die als host fungeert voor de rol en op basis van bronnen in de omgeving voor het beheren van de lokale bronnen runbooks worden uitgevoerd. Runbooks zijn opgeslagen en beheerd in Azure Automation en vervolgens aan een of meer specifieke computers geleverd.  

Deze functionaliteit wordt geïllustreerd in de volgende afbeelding:<br>  

![Overzicht van hybride Runbook Worker](media/automation-offering-get-started/automation-infradiagram-networkcomms.png)

Zie voor een technisch overzicht van de Hybrid Runbook Worker-rol en implementatie overwegingen [Automation architectuuroverzicht](automation-offering-get-started.md#automation-architecture-overview).    

## <a name="hybrid-runbook-worker-groups"></a>Hybrid Runbook Worker-groepen
Elke Hybrid Runbook Worker is lid van een hybride Runbook Worker-groep die u opgeeft wanneer u de agent installeert.  Een groep kan één agent bevatten, maar u kunt meerdere agents installeren in een groep voor hoge beschikbaarheid.

Wanneer u een runbook op een hybride Runbook Worker start, geeft u de groep die op wordt uitgevoerd.  De leden van de groep bepalen welke worker-services de aanvraag.  U kunt een bepaalde worker niet opgeven.

## <a name="relationship-to-service-management-automation"></a>De relatie met de Service Management Automation
[Service Management Automation (SMA)](https://technet.microsoft.com/library/dn469260.aspx) kunt u de dezelfde runbooks die worden ondersteund door Azure Automation in uw lokale datacentrum worden uitgevoerd. SMA wordt doorgaans samen met Windows Azure Pack geïmplementeerd als Windows Azure Pack, een grafische interface voor SMA management bevat. In tegenstelling tot Azure Automation is SMA vereist een lokale installatie met webservers die als host voor de API van een database naar de runbooks en SMA-configuratie bevatten, en Runbook Workers runbooktaken uitvoeren. Azure Automation biedt deze services in de cloud en alleen moet u de Hybrid Runbook Workers in uw lokale omgeving behouden.

Als u een bestaande SMA-gebruiker bent, kunt u uw runbooks in Azure Automation worden gebruikt met Hybrid Runbook Worker zonder wijzigingen, ervan uitgaande dat ze hun eigen resources als verificatie uitvoeren dat wordt beschreven in [runbooks worden uitgevoerd op een hybride Runbook Werknemer](automation-hrw-run-runbooks.md).  Runbooks in SMA worden uitgevoerd op de workerserver waarmee u dat de verificatie voor de runbooks bepalen kan in de context van het serviceaccount.

U kunt de volgende criteria om te bepalen of Azure Automation met Hybrid Runbook Worker- of Service Management Automation meer geschikt is voor uw vereisten.

* SMA vereist een lokale installatie van de onderliggende onderdelen die zijn verbonden met Windows Azure Pack als een grafische beheerinterface vereist is. Meer lokale bronnen nodig zijn met hogere onderhoudskosten dan Azure Automation, die alleen een agent geïnstalleerd op de lokale runbook workers moet. De agents worden beheerd door Operations Management Suite en verdere verlagen de onderhoudskosten.
* Azure Automation slaat de runbooks in de cloud en zorgt ervoor dat deze lokale hybride Runbook workers. Als uw beveiligingsbeleid dit gedrag niet toelaat, moet u SMA gebruiken.
* SMA is opgenomen in System Center; en is daarom een System Center 2012 R2-licentie vereist. Azure Automation is gebaseerd op een abonnementsmodel gelaagde.
* Azure Automation heeft geavanceerde functies zoals grafische runbooks die niet beschikbaar in SMA.

## <a name="installing-the-windows-hybrid-runbook-worker"></a>De Windows hybride Runbook Worker installeren 

Als u wilt installeren en configureren van een hybride Runbook Worker van Windows, zijn er twee methoden beschikbaar.  De aanbevolen methode gebruikt een Automation-runbook om het proces dat is vereist voor het configureren van een Windows-computer volledig te automatiseren.  De tweede methode volgt een stapsgewijze procedure om handmatig te installeren en configureren van de rol.  

> [!NOTE]
> Voor het beheren van de configuratie van uw servers die ondersteuning bieden de hybride Runbook Worker-rol met Desired State Configuration (DSC), moet u hen toevoegen als DSC-knooppunten.  Voor meer informatie over het voorbereiden voor beheer met DSC, Zie [machines voorbereiden voor beheer door Azure Automation DSC](automation-dsc-onboarding.md).           
><br>
>Als u inschakelt de [Update beheeroplossing](../operations-management-suite/oms-solution-update-management.md), elke Windows-computer die is verbonden met de OMS-werkruimte wordt automatisch geconfigureerd als een hybride Runbook Worker ter ondersteuning van runbooks die zijn opgenomen in deze oplossing.  Het is echter niet geregistreerd bij de Hybrid Worker-groepen in uw Automation-account al gedefinieerd.  De computer kan worden toegevoegd aan een hybride Runbook Worker-groep in uw Automation-account voor de ondersteuning van Automation-runbooks, zolang u hetzelfde account voor de oplossing en de hybride Runbook Worker-groepslidmaatschap gebruikt.  Deze functionaliteit is toegevoegd aan versie 7.2.12024.0 van de Hybrid Runbook Worker.  

Controleer de volgende informatie met betrekking tot de [hardware- en softwarevereisten](automation-offering-get-started.md#hybrid-runbook-worker) en [informatie voor het voorbereiden van uw netwerk](automation-offering-get-started.md#network-planning) voordat u begint met het implementeren van een hybride Runbook Worker.  Nadat u hebt een runbook worker is geïmplementeerd, controleren [runbooks worden uitgevoerd op een hybride Runbook Worker](automation-hrw-run-runbooks.md) voor informatie over het configureren van uw runbooks voor het automatiseren van processen in uw on-premises datacentrum of andere cloudomgeving.  
 
### <a name="automated-deployment"></a>Geautomatiseerde implementatie

Voer de volgende stappen uit voor het automatiseren van de installatie en configuratie van de Windows Hybrid Worker-rol.  

1. Download de *nieuw OnPremiseHybridWorker.ps1* script van de [PowerShell Gallery](https://www.powershellgallery.com/packages/New-OnPremiseHybridWorker/1.0/DisplayScript) rechtstreeks van de computer waarop de hybride Runbook Worker-rol of vanaf een andere computer in uw omgeving en Kopieer het naar de werknemer.  

    De *nieuw OnPremiseHybridWorker.ps1* vereist dat de volgende parameters tijdens het uitvoeren van script:

  * *AutomationAccountName* (verplicht) - de naam van uw Automation-account.  
  * *ResourceGroupName* (verplicht) - de naam van de resourcegroep die zijn gekoppeld aan uw Automation-account.  
  * *HybridGroupName* (verplicht) - de naam van een hybride Runbook Worker-groep die u opgeeft als doel voor de runbooks die dit scenario te ondersteunen. 
  *  *SubscriptionID* (verplicht) - de Azure-abonnements-Id die uw Automation-account in.
  *  *WorkspaceName* (optioneel): de naam van de OMS-werkruimte.  Als u een OMS-werkruimte niet hebt, wordt het script maakt en configureert u een.  

     > [!NOTE]
     > De enige Automation-regio's ondersteund voor de integratie met OMS zijn momenteel - **Australië-Zuidoost**, **VS-Oost 2**, **Zuidoost-Azië**, en **West-Europa** .  Als uw Automation-account zich niet in een van deze regio's, het script maakt een OMS-werkruimte, maar er een waarschuwingsbericht weergegeven dat er kan geen koppeling ze samen.
     > 
2. Start op uw computer **Windows PowerShell** van de **Start** scherm in de beheerdersmodus.  
3. Ga naar de map waarin het script dat u hebt gedownload en het wijzigen van de waarden voor parameters worden uitgevoerd vanuit de PowerShell opdrachtregel-shell, *- AutomationAccountName*, *- ResourceGroupName*, *- HybridGroupName*, *- SubscriptionId*, en *- WorkspaceName*.

     > [!NOTE] 
     > U wordt gevraagd om te verifiëren met Azure nadat u het script hebt uitgevoerd.  U **moet** aanmelden met een account dat lid is van de rol Abonnementsbeheerders en CO-beheerder van het abonnement.  
     >  
    
        .\New-OnPremiseHybridWorker.ps1 -AutomationAccountName <NameofAutomationAccount> `
        -ResourceGroupName <NameofOResourceGroup> -HybridGroupName <NameofHRWGroup> `
        -SubscriptionId <AzureSubscriptionId> -WorkspaceName <NameOfOMSWorkspace>

4. U wordt gevraagd om akkoord te installeren te **NuGet** en wordt u gevraagd om te verifiëren met uw Azure-referenties.<br><br> ![Uitvoering van script New-OnPremiseHybridWorker](media/automation-hybrid-runbook-worker/new-onpremisehybridworker-scriptoutput.png)

5. Nadat het script voltooid is, ziet de Hybrid Worker-groepen blade de nieuwe groep en het aantal leden of als een bestaande groep, het aantal leden wordt verhoogd.  U kunt de groep in de lijst selecteren op de **Hybrid Worker-groepen** blade en selecteer de **Hybrid Workers** tegel.  Op de **Hybrid Workers** blade ziet u elk lid van de groep die wordt vermeld.  

### <a name="manual-deployment"></a>Handmatige implementatie 
De eerste twee stappen eenmaal uitvoeren voor uw Automation-omgeving en vervolgens de resterende stappen herhalen voor elke worker-computer.

#### <a name="1-create-operations-management-suite-workspace"></a>1. Operations Management Suite-werkruimte maken
Als u nog geen een Operations Management Suite-werkruimte, maakt u een met instructies voor [beheren van uw werkruimte](../log-analytics/log-analytics-manage-access.md). Als u al hebt, kunt u een bestaande werkruimte gebruiken.

#### <a name="2-add-automation-solution-to-operations-management-suite-workspace"></a>2. Automation-oplossing toevoegen aan Operations Management Suite-werkruimte
Functionaliteit toevoegen oplossingen aan Operations Management Suite.  De Automation-oplossing voegt u functionaliteit voor Azure Automation biedt ook ondersteuning voor hybride Runbook Worker.  Wanneer u de oplossing voor uw werkruimte toevoegt, duwt wordt automatisch omlaag worker-onderdelen voor de agentcomputer die u in de volgende stap installeren wilt.

Volg de instructies voor [toevoegen van een oplossing met behulp van de galerie met oplossingen](../log-analytics/log-analytics-add-solutions.md) toevoegen de **Automation** oplossing voor uw Operations Management Suite-werkruimte.

#### <a name="3-install-the-microsoft-monitoring-agent"></a>3. Microsoft Monitoring Agent installeren
Microsoft Monitoring Agent verbonden computers met Operations Management Suite.  Wanneer u de agent op uw on-premises computer installeren en met uw werkruimte verbinden, worden de onderdelen die vereist zijn voor hybride Runbook Worker automatisch gedownload.

Volg de instructies voor [verbinding maken met Windows-computers met logboekanalyse](../log-analytics/log-analytics-windows-agent.md) voor het installeren van de agent op de lokale computer.  U kunt dit proces voor meerdere computers meerdere werknemers toevoegen aan uw omgeving herhalen.

Wanneer de agent is met Operations Management Suite verbonden, wordt het weergegeven op de **verbonden bronnen** tabblad van de Operations Management Suite **instellingen** deelvenster.  U kunt controleren of de agent correct de Automation-oplossing heeft gedownload wanneer er een map met de naam **AzureAutomationFiles** in C:\Program Files\Microsoft Monitoring Agent\Agent.  Als u wilt controleren welke versie van de hybride Runbook Worker, u kunt navigeren naar C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\ en noteer de \\ *versie* submap.   

#### <a name="4-install-the-runbook-environment-and-connect-to-azure-automation"></a>4. De runbook-omgeving installeren en verbinding maken met Azure Automation
Wanneer u een agent met Operations Management Suite toevoegt, wordt de Automation-oplossing pushes omlaag de **HybridRegistration** PowerShell-module bevat de **Add-HybridRunbookWorker** cmdlet.  Met deze cmdlet kunt u de runbook-omgeving op de computer installeren en registreren met Azure Automation.

Open een PowerShell-sessie in de beheerdersmodus en voer de volgende opdrachten in de module te importeren.

    cd "C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\<version>\HybridRegistration"
    Import-Module HybridRegistration.psd1

Voer vervolgens de **Add-HybridRunbookWorker** cmdlet met de volgende syntaxis:

    Add-HybridRunbookWorker –GroupName <String> -EndPoint <Url> -Token <String>

U kunt de vereiste informatie voor deze cmdlet uit de **sleutels beheren** blade in de Azure portal.  Deze blade openen door te selecteren de **sleutels** optie van de **instellingen** blade in uw Automation-account.

![Overzicht van hybride Runbook Worker](media/automation-hybrid-runbook-worker/elements-panel-keys.png)

* **GroupName** is de naam van de Hybrid Runbook Worker-groep. Als deze groep al in het automation-account bestaat, is klikt u vervolgens de huidige computer toegevoegd.  Als deze niet bestaat nog, wordt deze toegevoegd.
* **Eindpunt** is de **URL** veld in de **sleutels beheren** blade.
* **Token** is de **primaire toegangssleutel** in de **sleutels beheren** blade.  

Gebruik de **-uitgebreide** overschakelen met **Add-HybridRunbookWorker** voor het ontvangen van gedetailleerde informatie over de installatie.

#### <a name="5-install-powershell-modules"></a>5. PowerShell-modules installeren
Runbooks kunt u elk van de activiteiten en de cmdlets die zijn gedefinieerd in de modules die zijn geïnstalleerd in uw Azure Automation-omgeving.  Deze modules worden niet automatisch geïmplementeerd op lokale computers, zodat u ze handmatig moet installeren.  De uitzondering is de Azure-module, dat standaard toegang tot de cmdlets voor alle Azure-services en activiteiten bieden voor Azure Automation is geïnstalleerd.

Omdat het voornaamste doel van de Hybrid Runbook Worker-functie voor het beheren van lokale bronnen, moet u waarschijnlijk de modules die ondersteuning bieden voor deze resources te installeren.  U kunt verwijzen naar [Modules installeren](http://msdn.microsoft.com/library/dd878350.aspx) voor informatie over het installeren van Windows PowerShell-modules.  Modules die zijn geïnstalleerd, moeten zich in een locatie waarnaar wordt verwezen door de omgevingsvariabele PSModulePath zodat ze automatisch worden geïmporteerd door de hybride worker.  Zie voor meer informatie [wijzigen van het installatiepad PSModulePath](https://msdn.microsoft.com/library/dd878326%28v=vs.85%29.aspx). 

## <a name="removing-hybrid-runbook-worker"></a>Hybride Runbook Worker verwijderen 
U kunt een of meer Hybrid Runbook Workers verwijderen uit een groep of kunt u de groep, afhankelijk van uw vereisten.  Als u wilt een hybride Runbook Worker verwijderen uit een on-premises computer, moet u de volgende stappen uitvoeren.

1. Ga in de Azure-portal naar uw Automation-account.  
2. Van de **instellingen** blade Selecteer **sleutels** en noteer de waarden voor veld **URL** en **primaire toegangssleutel**.  Deze informatie moet u voor de volgende stap.
3. Open een PowerShell-sessie in de beheerdersmodus en voer de volgende opdracht - `Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey>`.  Gebruik de **-uitgebreide** overschakelen voor een gedetailleerd logboek van het verwijderingsproces.

> [!NOTE]
> Microsoft Monitoring Agent wordt niet verwijderd van de computer, alleen de functionaliteit en de configuratie van de Hybrid Runbook Worker-rol.  

## <a name="remove-hybrid-worker-groups"></a>Hybrid Worker-groepen verwijderen
Voor een groep verwijdert, moet u eerst de hybride Runbook Worker verwijderen uit elke computer die lid is van de groep met de bovenstaande procedure en voer de volgende stappen uit om de groep te verwijderen.  

1. Open het Automation-account in de Azure portal.
2. Selecteer de **Hybrid Worker-groepen** tegel en in de **Hybrid Worker-groepen** blade, selecteer de groep die u wilt verwijderen.  Na het selecteren van de specifieke groep de **Hybrid worker-groep** eigenschappenblade wordt weergegeven.<br> ![Blade Hybrid Runbook Worker-groep](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)   
3. Klik op de eigenschappenblade voor de geselecteerde groep **verwijderen**.  Selecteer een bericht weergegeven waarin u deze actie te bevestigen **Ja** als u zeker dat u wilt doorgaan.<br> ![Dialoogvenster voor bevestiging groep verwijderen](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)<br> Dit proces kan enkele seconden duren en u kunt de voortgang bijhouden onder **Meldingen** in het menu.  

## <a name="troubleshooting"></a>Problemen oplossen 
De hybride Runbook Worker is afhankelijk van de Microsoft Monitoring Agent te communiceren met uw Automation-account voor het registreren van de werknemer, ontvangen van runbooktaken en status rapporteren. Als de registratie van de werknemer is mislukt, volgen hier enkele mogelijke oorzaken voor de fout:  

1. De hybride worker zich achter een proxy of firewall.  
    Controleer of dat de computer heeft uitgaande toegang tot de *.azure automation.net op poort 443.  

2. De computer die de hybride worker op wordt uitgevoerd heeft minder dan de minimale hardware [vereisten](automation-offering-get-started.md#hybrid-runbook-worker).  
    Computers met de hybride Runbook Worker aan de minimale hardwarevereisten voldoen voordat u deze als host voor deze functie. Gebruik anders, afhankelijk van het Resourcegebruik van andere achtergrondprocessen en conflicten veroorzaakt door runbooks tijdens het uitvoeren van de computer wordt er meer dan worden gebruikt en ervoor zorgen dat de runbook-taak vertragingen of time-outs.
   Controleer de computer moeten worden uitgevoerd van de Hybrid Runbook Worker-functie voldoet aan de minimale hardwarevereisten.  Zo ja, monitor CPU en geheugen gebruik om te bepalen een correlatie tussen de prestaties van Hybrid Runbook Worker-processen en vensters.  Als er geheugen of CPU-belasting, dit kan duiden op hoeft bij te werken of toevoegen van extra processors of geheugen voor het adres van de resource-knelpunt en los de fout te verhogen. Selecteer een andere berekeningsresource die de minimum vereisten en schaal ondersteunen kan wanneer werkbelasting eisen duiden op dat een verhoging is nodig.
    
3. De service Microsoft Monitoring Agent wordt niet uitgevoerd.  
    Als de Microsoft Monitoring Agent Windows-service niet wordt uitgevoerd, dit voorkomt dat de hybride Runbook Worker communicatie met Azure Automation.  Controleer of de agent wordt uitgevoerd met de volgende opdracht in PowerShell: `get-service healthservice`.  Als de service wordt gestopt, voert u de volgende opdracht in PowerShell om de service te starten: `start-service healthservice`.  

4. In de **toepassingen en Services Logs\Operations Manager** gebeurtenislogboek, ziet u gebeurtenis 4502 en EventMessage met **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**met de volgende beschrijving: *het certificaat dat is doorgegeven door de service <wsid>. oms.opinsights.azure.com is niet uitgegeven door een certificeringsinstantie die wordt gebruikt voor Microsoft-services. Neem contact op met de netwerkbeheerder om te controleren of er een proxy die TLS/SSL-communicatie wordt onderschept worden uitgevoerd. Artikel KB3126513 bevat extra informatie over probleemoplossing voor problemen met de netwerkverbinding.*
    Dit kan worden veroorzaakt door uw proxy- of -firewall blokkeert communicatie met Microsoft Azure.  Controleer of dat de computer heeft uitgaande toegang tot de *.azure automation.net op poort 443.

Logboeken worden lokaal opgeslagen op elke worker hybride op C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes.  U kunt controleren of er zijn waarschuwingen of fouten die zijn geschreven naar de **toepassingen en Services Logs\Microsoft-SMA\Operations** en **toepassingen en Services Logs\Operations Manager** in het gebeurtenislogboek die duidt dit op een verbindings- of andere probleem met betrekking tot de voorbereiding van de rol aan Azure Automation of probleem tijdens het normale bewerkingen uitvoeren.  

## <a name="next-steps"></a>Volgende stappen
Bekijk [runbooks worden uitgevoerd op een hybride Runbook Worker](automation-hrw-run-runbooks.md) voor informatie over het configureren van uw runbooks voor het automatiseren van processen in uw on-premises datacentrum of andere cloudomgeving.
