---
title: Uitvoeren van Runbook in Azure Automation | Microsoft Docs
description: Beschrijft de details van hoe een runbook in Azure Automation wordt verwerkt.
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: d10c8ce2-2c0b-4ea7-ba3c-d20e09b2c9ca
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/15/2017
ms.author: magoedte;bwren
ms.openlocfilehash: a443071aee3e0f845de4387322d2866157a9fe87
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2017
---
# <a name="runbook-execution-in-azure-automation"></a>Uitvoeren van Runbook in Azure Automation
Wanneer u een runbook in Azure Automation start, wordt een taak gemaakt. Een taak is één uitvoeringsinstantie van een runbook. Een Azure Automation worker is toegewezen aan elke taak uitvoeren. Werknemers worden gedeeld door meerdere Azure-accounts, zijn taken van andere Automation-accounts geïsoleerd van elkaar. U doet geen hebben controle over welke worker-services de aanvraag voor de taak. Één runbook kan meerdere taken tegelijk actief hebben.  De omgeving worden uitgevoerd voor de taken van hetzelfde Automation-Account kan opnieuw worden gebruikt. Wanneer u de lijst met runbooks in de Azure portal weergeeft, geeft de status van alle taken die zijn gestart voor elk runbook. U kunt de lijst met taken voor elk runbook weergeven om de status van elk bijhouden. Voor een beschrijving van de status van een andere taak [status van een taak](#job-statuses).

Het volgende diagram toont de levenscyclus van een runbooktaak voor [grafische runbooks](automation-runbook-types.md#graphical-runbooks) en [PowerShell Workflow-runbooks](automation-runbook-types.md#powershell-workflow-runbooks).

![Status van een taak - PowerShell-werkstroom](./media/automation-runbook-execution/job-statuses.png)

Het volgende diagram toont de levenscyclus van een runbooktaak voor [PowerShell-runbooks](automation-runbook-types.md#powershell-runbooks).

![Status van een taak - PowerShell-Script](./media/automation-runbook-execution/job-statuses-script.png)

Uw taken hebben toegang tot uw Azure-resources door het maken van een verbinding met uw Azure-abonnement. Ze hebben alleen toegang tot bronnen in uw datacentrum als deze resources toegankelijk via de openbare cloud zijn.

## <a name="job-statuses"></a>Status van een taak
De volgende tabel beschrijft de verschillende statussen die mogelijk voor een taak zijn.

| Status | Beschrijving |
|:--- |:--- |
| Voltooid |De taak is voltooid. |
| Mislukt |Voor [grafisch en PowerShell Workflow-runbooks](automation-runbook-types.md), het runbook kan niet worden gecompileerd.  Voor [PowerShell-Script runbooks](automation-runbook-types.md), het runbook kan niet worden gestart of de taak is een uitzondering opgetreden. |
| Is mislukt, wacht voor bronnen |De taak is mislukt omdat het werkproces de [evenredige verdeling](#fair-share) driemaal beperken en gestart vanaf het dezelfde controlepunt of vanaf het begin van het runbook elke keer. |
| In wachtrij |De taak is in afwachting voor bronnen op een Automation worker beschikbaar zijn, zodat het kan worden gestart. |
| Starting |De taak is toegewezen aan een werknemer en het systeem is bezig te starten. |
| Hervatten |Het systeem is bezig de taak wordt hervat nadat deze is onderbroken. |
| Actief |De taak wordt uitgevoerd. |
| Wordt uitgevoerd, wachten op resources |De taak is verwijderd omdat het werkproces de [evenredige verdeling](#fair-share) limiet. Het hervatten snel uit de laatste controlepunt. |
| Stopped |De taak is gestopt door de gebruiker voordat deze is voltooid. |
| Stopping |Het systeem is bezig te stoppen van de taak. |
| Uitgesteld |De taak is onderbroken door de gebruiker, door het systeem of door een opdracht in het runbook. Een onderbroken taak kan opnieuw worden gestart en hervat vanaf het laatste controlepunt of vanaf het begin van het runbook als er geen controlepunten. Het runbook wordt alleen door het systeem worden onderbroken wanneer er een uitzondering optreedt. ErrorActionPreference is standaard ingesteld op **doorgaan**, wil zeggen dat de taak wordt uitgevoerd op een fout. Als deze voorkeursvariabele is ingesteld op **stoppen**, en vervolgens de taak wordt onderbroken op een fout opgetreden.  Van toepassing op [grafisch en PowerShell Workflow-runbooks](automation-runbook-types.md) alleen. |
| Onderbreken |Het systeem probeert te onderbreken van de taak op verzoek van de gebruiker. Het runbook moet het volgende controlepunt bereiken voordat deze kan worden onderbroken. Als het laatste controlepunt al doorgegeven, is voordat deze kan worden onderbroken voltooid.  Van toepassing op [grafisch en PowerShell Workflow-runbooks](automation-runbook-types.md) alleen. |

## <a name="viewing-job-status-from-the-azure-portal"></a>Status van de Azure-portal weergeven
U kunt een samengevatte status van alle runbooktaken weergeven of inzoomen op gegevens van een specifiek runbook-taak in de Azure portal of door integratie met uw werkruimte voor logboekanalyse van Microsoft Operations Management Suite (OMS) om door te sturen runbook de status en taak streams taak configureren.  Zie voor meer informatie over de integratie met OMS Log Analytics [doorsturen taakstatus en taak stromen van Automation voor logboekanalyse (OMS)](automation-manage-send-joblogs-log-analytics.md).  

### <a name="automation-runbook-jobs-summary"></a>Automation-runbooktaken samenvatting
Aan de rechterkant van de geselecteerde Automation-account, ziet u een overzicht van alle runbooktaken voor een geselecteerde Automation-account onder **taak statistieken** tegel.<br><br> ![Statistieken voor taak tegel](./media/automation-runbook-execution/automation-account-job-status-summary.png).<br> Deze tegel wordt weergegeven voor een aantal en de grafische weergave van de taakstatus voor alle taken die worden uitgevoerd.  

Op de tegel te klikken geeft de **taken** blade die een overzicht van alle taken die worden uitgevoerd, met de status, taakuitvoering en begin en einde tijden bevat.<br><br> ![Blade Automation-account-uptaken](./media/automation-runbook-execution/automation-account-jobs-status-blade.png)<br><br>  U kunt de lijst met taken filteren door te selecteren **Filter de taken** en filter op een specifiek runbook, taakstatus, of in de vervolgkeuzelijst, het bereik van de datum/tijd om te zoeken in.<br><br> ![Status van de taak filteren](./media/automation-runbook-execution/automation-account-jobs-filter.png)

U kunt ook overzichtsgegevens voor een specifiek runbook taak weergeven door te selecteren dat runbook uit de **Runbooks** blade in uw Automation-account en selecteer vervolgens de **taken** tegel.  Dit geeft de **taken** blade en daar kunt u klikken op de record van de taak om de details en de uitvoer weer te geven.<br><br> ![Blade Automation-account-uptaken](./media/automation-runbook-execution/automation-runbook-job-summary-blade.png)<br> 

### <a name="job-summary"></a>Taakoverzicht
U kunt een lijst van alle taken die zijn gemaakt voor een bepaald runbook en de meest recente status weergeven. U kunt deze lijst op de taakstatus en het datumbereik voor de laatste wijziging aan het project op te filteren. Als u wilt weergeven in de gedetailleerde informatie en uitvoer, klik op de naam van een taak. De gedetailleerde weergave van de taak bevat de waarden voor de runbookparameters die zijn opgegeven voor die taak.

U kunt de volgende stappen gebruiken om de taken voor een runbook weer te geven.

1. Selecteer in de Azure-portal **Automation** en selecteer vervolgens de naam van een Automation-account.
2. Selecteer in de hub **Runbooks** en klik vervolgens op de **Runbooks** blade een runbook in de lijst selecteren.
3. Op de blade voor het geselecteerde runbook, klikt u op de **taken** tegel.
4. Klik op een van de taken in de lijst en op de blade runbook taak details kunt u de details en de uitvoer weergeven.

## <a name="retrieving-job-status-using-windows-powershell"></a>Taakstatus ophalen met Windows PowerShell
U kunt de [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx) voor het ophalen van de taken gemaakt voor een runbook en de details van een bepaalde taak. Als u een runbook met Windows PowerShell Start [Start AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx), en vervolgens wordt de resulterende taak geretourneerd. Gebruik [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx)uitvoer naar een taak ophalen-uitvoer.

De volgende voorbeeldopdrachten de laatste taak voor een voorbeeldrunbook opgehaald en sorden de status, de waarden voor de runbookparameters en de uitvoer van de taak.

    $job = (Get-AzureRmAutomationJob –AutomationAccountName "MyAutomationAccount" `
    –RunbookName "Test-Runbook" -ResourceGroupName "ResourceGroup01" | sort LastModifiedDate –desc)[0]
    $job.Status
    $job.JobParameters
    Get-AzureRmAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAcct" -Id $job.JobId –Stream Output

## <a name="fair-share"></a>Evenredige verdeling
Om de resources kan delen met alle runbooks in de cloud, wordt Azure Automation tijdelijk elke taak geladen nadat deze actief is geweest gedurende drie uur.  Tijdens deze periode kunnen taken voor [op basis van PowerShell-runbooks](automation-runbook-types.md#powershell-runbooks) worden gestopt en wordt niet opnieuw worden gestart.  De taak status ziet **gestopt**.  Dit type runbook wordt altijd opnieuw opgestart vanaf het begin omdat controlepunten wordt niet ondersteund.  

[PowerShell-werkstroom runbooks](automation-runbook-types.md#powershell-workflow-runbooks) wordt hervat vanaf de laatste [controlepunt](https://docs.microsoft.com/system-center/sma/overview-powershell-workflows#bk_Checkpoints).  Na het uitvoeren van drie uur zal de runbooktaak worden onderbroken door de service en de status ervan toont **uitgevoerd, wachten op resources**.  Zodra een sandbox beschikbaar, het runbook wordt automatisch opnieuw gestart door de Automation-service en wordt hervat vanaf het laatste controlepunt.  Dit is normaal gedrag van de PowerShell-werkstroom voor onderbreken/opnieuw te starten.  Als het runbook opnieuw groter is dan drie uur van de runtime, het proces wordt herhaald, worden drie pogingen gedaan.  Na het derde opnieuw opstarten, als het runbook nog niet voltooid in drie uur en vervolgens de runbooktaak is mislukt en de status van de taak aangeeft **mislukt, wacht resources**.  In dit geval ontvangt u de volgende uitzondering aan de fout.

*De taak kan niet worden voortgezet omdat deze is herhaaldelijk onbeschikbaar gemaakt vanaf het controlepunt dezelfde. Zorg ervoor dat uw Runbook heeft geen langdurige bewerkingen zonder dat de status.*

Dit is de service van runbooks voor onbepaalde tijd worden uitgevoerd zonder te voltooien, beveiligen, omdat ze niet de volgende controlepunt te maken kunnen zonder het ontladen opnieuw.

Als het runbook geen controlepunten heeft of de taak niet het eerste controlepunt heeft bereikt voordat het wordt ongedaan gemaakt, klikt u vervolgens opnieuw wordt gestart vanaf het begin.  

Wanneer u een runbook maakt, moet u ervoor zorgen dat de tijd voor het uitvoeren van alle activiteiten tussen de twee controlepunten niet meer dan drie uur. Mogelijk moet u controlepunten toevoegen aan uw runbook om ervoor te zorgen dat deze niet deze drie uur limiet is bereikt of lange onderverdelen bewerkingen uitgevoerd. Uw runbook kan bijvoorbeeld een opnieuw indexeren uitvoeren op een grote SQL-database. Als deze één bewerking niet voltooid binnen de limiet van evenredige verdeling, worden de taak de verwijderd en opnieuw worden opgestart vanaf het begin. In dit geval moet u de bewerking opnieuw indexeren in meerdere stappen, zoals het indexeren van één tabel tegelijk onderverdelen en vervolgens een controlepunt invoegen na elke bewerking, zodat de taak na de laatste bewerking hervatten kan te voltooien.

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over de verschillende methoden die kunnen worden gebruikt voor het starten van een runbook in Azure Automation, [een runbook starten in Azure Automation](automation-starting-a-runbook.md)

