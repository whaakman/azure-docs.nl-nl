---
title: Migratie van Orchestrator naar Azure Automation | Microsoft Docs
description: Beschrijft hoe System Center Orchestrator-runbooks en integration packs migreren naar Azure Automation.
services: automation
documentationcenter: 
author: bwren
manager: stevenka
editor: tysonn
ms.assetid: 1a7da58c-7a98-49b5-9d9d-001a9f6e631a
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2016
ms.author: bwren
ms.openlocfilehash: 457888b4d38875b912ad87d44e96ab727e3ee3ee
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="migrating-from-orchestrator-to-azure-automation-beta"></a>Migratie van Orchestrator naar Azure Automation (bèta)
Runbooks in [System Center Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) zijn gebaseerd op activiteiten uit integratiepakketten die specifiek voor Orchestrator zijn geschreven terwijl runbooks in Azure Automation zijn gebaseerd op Windows PowerShell.  [Grafische runbooks](automation-runbook-types.md#graphical-runbooks) in Azure Automation hebt u een vergelijkbare weergave voor Orchestrator-runbooks met hun activiteiten voor PowerShell-cmdlets, onderliggende runbooks en activa.

De [System Center Orchestrator Migration Toolkit](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) bevat hulpprogramma's om u te helpen u bij het converteren van runbooks van Orchestrator naar een Azure Automation.  Naast de runbooks zelf converteert, moet u de integratiepakketten met de activiteiten die gebruikmaken van de runbooks converteren naar integratiemodules met Windows PowerShell-cmdlets.  

Hier volgt het basisproces voor het omzetten van Orchestrator-runbooks in Azure Automation.  Elk van deze stappen wordt beschreven in de onderstaande secties.

1. Download de [System Center Orchestrator Migration Toolkit](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) die de hulpprogramma's en modules die worden beschreven in dit artikel bevat.
2. Importeren [standaardactiviteiten Module](#standard-activities-module) in Azure Automation.  Dit omvat geconverteerde versies van Orchestrator standaardactiviteiten die kunnen worden gebruikt door geconverteerde runbooks.
3. Importeren [System Center Orchestrator-integratiemodules](#system-center-orchestrator-integration-modules) in Azure Automation voor deze integratiepakketten die wordt gebruikt door uw runbooks die toegang hebben tot System Center.
4. Converteren van aangepaste en integratiepakketten van derden met behulp van de [Integration Pack Converter](#integration-pack-converter) en importeren in Azure Automation.
5. Converteren Orchestrator runbooks met de [Runbook Converter](#runbook-converter) en installeer in Azure Automation.
6. Handmatig vereiste Orchestrator activa maken in Azure Automation, omdat de Runbook Converter deze resources niet geconverteerd.
7. Configureer een [Hybrid Runbook Worker](#hybrid-runbook-worker) in uw lokale datacentrum worden uitgevoerd geconverteerde runbooks die toegang lokale bronnen tot.

## <a name="service-management-automation"></a>Service Management Automation
[Service Management Automation](http://technet.microsoft.com/library/dn469260.aspx) (SMA) worden opgeslagen en het uitvoeren van runbooks in uw lokale datacentrum zoals Orchestrator, en maakt gebruik van de dezelfde integratiemodules als Azure Automation. De [Runbook Converter](#runbook-converter) converteert Orchestrator-runbooks naar grafische runbooks maar die niet in SMA worden ondersteund.  U kunt nog steeds installeren de [standaard activiteiten Module](#standard-activities-module) en [System Center Orchestrator-integratiemodules](#system-center-orchestrator-integration-modules) in SMA, maar moet u handmatig [herschrijven van uw runbooks](http://technet.microsoft.com/library/dn469262.aspx).

## <a name="hybrid-runbook-worker"></a>Hybrid Runbook Worker
Runbooks in Orchestrator zijn opgeslagen op een databaseserver en uitgevoerd op de runbook-servers, zowel in uw lokale datacentrum.  Runbooks in Azure Automation worden opgeslagen in de Azure-cloud en kunnen worden uitgevoerd in de lokale data center met een [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md).  Dit is hoe u gewoonlijk geconverteerd van Orchestrator, omdat ze zijn ontworpen om te worden uitgevoerd op de lokale servers runbooks wordt uitgevoerd.

## <a name="integration-pack-converter"></a>Integration Pack Converter
De Integration Pack Converter converteert integratiepakketten die zijn gemaakt met de [Orchestrator Integration Toolkit (OIT)](http://technet.microsoft.com/library/hh855853.aspx) aan op basis van Windows PowerShell die kunnen worden geïmporteerd in Azure Automation integratiemodules of Service Management Automation.  

Wanneer u de Integration Pack Converter uitvoert, krijgt u een wizard waarmee u een integration pack (oip)-bestand te selecteren.  De wizard wordt vervolgens een lijst met de activiteiten die zijn opgenomen in dat integratiepakket en kunt u selecteren die worden gemigreerd.  Wanneer u de wizard hebt voltooid, maakt deze een integratiemodule met een bijbehorende cmdlet voor elk van de activiteiten in het oorspronkelijke integratiepakket.

### <a name="parameters"></a>Parameters
Alle eigenschappen van een activiteit in het integratiepakket worden geconverteerd naar parameters van de bijbehorende cmdlet in de integratiemodule.  Windows PowerShell-cmdlets hebben een aantal [algemene parameters](http://technet.microsoft.com/library/hh847884.aspx) die kunnen worden gebruikt met alle cmdlets.  Bijvoorbeeld:-Verbose parameter zorgt ervoor dat een cmdlet voor gedetailleerde informatie over de werking ervan uitvoer.  Er is geen cmdlet mogelijk een parameter met dezelfde naam als een algemene parameter hebben.  Als een activiteit een eigenschap met dezelfde naam als een algemene parameter heeft, krijgt de wizard u een andere naam voor de parameter op te geven.

### <a name="monitor-activities"></a>Voor monitoractiviteiten
Bewaken van runbooks in Orchestrator beginnen met een [activiteitsbewaking](http://technet.microsoft.com/library/hh403827.aspx) en continu wacht om te worden aangeroepen door een bepaalde gebeurtenis worden uitgevoerd.  Azure Automation biedt geen ondersteuning voor runbooks van de monitor, zodat alle monitoractiviteiten in het integratiepakket niet worden geconverteerd.  In plaats daarvan wordt een tijdelijke aanduiding voor cmdlet gemaakt in de integratiemodule voor de monitoractiviteit.  Deze cmdlet heeft geen functionaliteit, maar kunt u een willekeurig geconverteerde runbook gebruikt om te worden geïnstalleerd.  Dit runbook kan niet worden uitgevoerd in Azure Automation, maar kan worden geïnstalleerd zodat u deze kunt wijzigen.

### <a name="integration-packs-that-cannot-be-converted"></a>Integratiepakketten die kunnen niet worden geconverteerd
Integratiepakketten die niet zijn gemaakt met OIT kunnen niet worden geconverteerd met de omzetter voor Integration Pack. Er zijn ook enkele integratiepakketten die is geleverd door Microsoft die op dit moment kan niet worden geconverteerd met dit hulpprogramma.  Geconverteerde versies van deze integratiepakketten zijn [gedownload](#system-center-orchestrator-integration-modules) zodat ze in Azure Automation- of Service Management Automation kunnen worden geïnstalleerd.

## <a name="standard-activities-module"></a>Standaardactiviteiten-Module
Orchestrator bevat een reeks [standaardactiviteiten](http://technet.microsoft.com/library/hh403832.aspx) die niet zijn opgenomen in een integratiepakket, maar worden gebruikt door veel runbooks.  De standaardactiviteiten-module is een integratiemodule met een cmdlet equivalent voor elk van deze activiteiten.  U moet deze integratiemodule in Azure Automation installeren voordat u importeert geconverteerde runbooks die een standaardactiviteit gebruiken.

Naast de ondersteuning biedt voor geconverteerde runbooks, kunnen de cmdlets in de module standaardactiviteiten door iemand die bekend zijn met Orchestrator worden gebruikt voor het bouwen van nieuwe runbooks in Azure Automation.  Hoewel de functionaliteit van alle standaard activiteiten kan worden uitgevoerd met cmdlets, kunnen ze anders werken.  De cmdlets in de module geconverteerde standaardactiviteiten zal werken op dezelfde manier als hun bijbehorende activiteiten en dezelfde parameters gebruiken.  Zo kunt u de bestaande Orchestrator-runbookauteur in hun overgang naar Azure Automation-runbooks.

## <a name="system-center-orchestrator-integration-modules"></a>System Center Orchestrator-integratiemodules
Microsoft biedt [integratiepakketten](http://technet.microsoft.com/library/hh295851.aspx) voor het bouwen van runbooks voor het automatiseren van System Center-onderdelen en andere producten.  Sommige van deze integratiepakketten die momenteel zijn gebaseerd op OIT maar momenteel niet worden geconverteerd naar integratiemodules vanwege bekende problemen.  [System Center Orchestrator-integratiemodules](https://www.microsoft.com/download/details.aspx?id=49555) bevat geconverteerde versies van deze integratiepakketten die kunnen worden geïmporteerd in Azure Automation en Service Management Automation.  

Door de RTM-versie van dit hulpprogramma wordt bijgewerkte versies van integratiepakketten op basis van OIT die kan worden omgezet met de omzetter voor Integration Pack gepubliceerd.  Richtlijnen worden ook gegeven om u te helpen bij het converteren van runbooks met behulp van activiteiten uit integratiepakketten niet op basis van OIT.

## <a name="runbook-converter"></a>Runbook-Converter
De Runbook-Converter converteert Orchestrator-runbooks in [grafische runbooks](automation-runbook-types.md#graphical-runbooks) die kunnen worden geïmporteerd in Azure Automation.  

Runbook-Converter is geïmplementeerd als een PowerShell-module met een cmdlet aangeroepen **ConverterenVan SCORunbook** die de conversie uitvoert.  Wanneer u het hulpprogramma installeert, maakt deze een snelkoppeling naar een PowerShell-sessie die door de cmdlet wordt geladen.   

Hieronder volgt het basisproces een Orchestrator-runbook converteren en importeren in Azure Automation.  De volgende secties bevatten meer informatie over het gebruik van het hulpprogramma en werken met geconverteerde runbooks.

1. Een of meer runbooks exporteren van Orchestrator.
2. Integratiemodules voor alle activiteiten in het runbook verkrijgen.
3. De Orchestrator-runbooks in het geëxporteerde bestand worden geconverteerd.
4. Lees de informatie in de logboeken voor het valideren van de conversie en het bepalen van de vereiste handmatige taken.
5. Geconverteerde runbooks in Azure Automation worden geïmporteerd.
6. Alle vereiste activa maken in Azure Automation.
7. Bewerk het runbook in Azure Automation te wijzigen van alle vereiste activiteiten.

### <a name="using-runbook-converter"></a>Met behulp van de Converter Runbook
De syntaxis voor **ConverterenVan SCORunbook** is als volgt:

    ConvertFrom-SCORunbook -RunbookPath <string> -Module <string[]> -OutputFolder <string>

* RunbookPath - pad naar het exportbestand met de runbooks te converteren.
* Module - met door komma's gescheiden lijst met integratiemodules met activiteiten in de runbooks.
* OutputFolder - pad naar de map maken geconverteerd grafische runbooks.

De volgende opdracht converteert de runbooks in een exportbestand aangeroepen **MyRunbooks.ois_export**.  Deze runbooks gebruiken de integratiepakketten van Active Directory en Data Protection Manager.

    ConvertFrom-SCORunbook -RunbookPath "c:\runbooks\MyRunbooks.ois_export" -Module c:\ip\SystemCenter_IntegrationModule_ActiveDirectory.zip,c:\ip\SystemCenter_IntegrationModule_DPM.zip -OutputFolder "c:\runbooks"


### <a name="log-files"></a>Logboekbestanden
De Runbook-Converter maakt in dezelfde locatie als het geconverteerde runbook de volgende logboekbestanden.  Als de bestanden al bestaat, wordt deze overschreven met gegevens van de laatste conversie.

| File | Inhoud |
|:--- |:--- |
| Runbook Converter - Progress.log |Gedetailleerde stappen van de conversie met inbegrip van gegevens voor elke activiteit heeft geconverteerd en waarschuwing voor elke activiteit niet geconverteerd. |
| Runbook Converter - Summary.log |Samenvatting van de laatste conversie, met inbegrip van eventuele waarschuwingen en taken die u nodig hebt om uit te voeren zoals het maken van een variabele is vereist voor het geconverteerde runbook opvolgen. |

### <a name="exporting-runbooks-from-orchestrator"></a>Exporteren van runbooks van Orchestrator
De Runbook-Converter werkt met een exportbestand van Orchestrator met een of meer runbooks.  Wordt er een bijbehorende Azure Automation-runbook voor elk Orchestrator-runbook gemaakt in het exportbestand.  

Als u wilt een runbook van Orchestrator hebt geëxporteerd, met de rechtermuisknop op de naam van het runbook in Runbook Designer en selecteer **exporteren**.  Als u wilt exporteren van alle runbooks in een map met de rechtermuisknop op de naam van de map en selecteer **exporteren**.

### <a name="runbook-activities"></a>Runbookactiviteiten
Elke activiteit in de Orchestrator-runbook converteert de Runbook Converter naar een bijbehorende activiteit in Azure Automation.  Voor de activiteiten die kunnen niet worden geconverteerd, wordt een tijdelijke aanduiding voor activiteit gemaakt in het runbook met de waarschuwing.  Nadat u het geconverteerde runbook in Azure Automation importeert, moet u een van deze activiteiten vervangen door geldige activiteiten die de vereiste functionaliteit uitvoeren.

Een Orchestrator-activiteiten in de [standaard activiteiten Module](#standard-activities-module) worden geconverteerd.  Er zijn enkele standaard Orchestrator-activiteiten die niet in deze module al en worden niet geconverteerd.  Bijvoorbeeld: **platformgebeurtenis verzenden** heeft geen Azure Automation-equivalent, omdat de gebeurtenis specifiek voor Orchestrator is.

[Activiteiten volgen die](https://technet.microsoft.com/library/hh403827.aspx) niet worden geconverteerd omdat er geen equivalent voor deze in Azure Automation is.  De uitzondering worden monitor activiteiten in [integratiepakketten geconverteerd](#integration-pack-converter) die worden geconverteerd naar de tijdelijke aanduiding-activiteit.

Een activiteit uit een [geconverteerd integratiepakket](#integration-pack-converter) worden geconverteerd als u het pad naar de integratiemodule met geven de **modules** parameter.  Voor System Center Integration Packs kunt u de [System Center Orchestrator-integratiemodules](#system-center-orchestrator-integration-modules).

### <a name="orchestrator-resources"></a>Orchestrator-bronnen
De Runbook-Converter converteert alleen runbooks, geen andere Orchestrator-bronnen zoals tellers, variabelen of verbindingen.  Prestatiemeteritems worden niet ondersteund in Azure Automation.  Variabelen en verbindingen worden ondersteund, maar moet u ze handmatig maken.  De logboekbestanden wordt gemeld als het runbook resources vereist en bijbehorende bronnen die u maakt in Azure Automation voor het geconverteerde runbook correcte werking moet opgeven.

Een runbook kan bijvoorbeeld een variabele gebruiken voor het vullen van een bepaalde waarde in een activiteit.  Het geconverteerde runbook wordt de activiteit converteren en geef een variabelenactivum in Azure Automation met dezelfde naam als de Orchestrator-variabele.  Dit zijn vastgelegd in de **Runbook Converter - Summary.log** bestand dat wordt gemaakt na de conversie.  U moet deze variabele activa handmatig maken in Azure Automation voordat u het runbook.

### <a name="input-parameters"></a>Invoerparameters
Runbooks in Orchestrator accepteren invoerparameters met de **gegevens initialiseren** activiteit.  Als het runbook wordt geconverteerd, deze activiteit bevat wordt er een [invoerparameter](automation-graphical-authoring-intro.md#runbook-input-and-output) in de Azure Automation-runbook voor elke parameter in de activiteit wordt gemaakt.  Een [Werkstroomscript besturingselement](automation-graphical-authoring-intro.md#activities) activiteit wordt gemaakt in het geconverteerde runbook die worden opgehaald en elke parameter retourneert.  Alle activiteiten in het runbook die gebruikmaken van een invoerparameter verwijzen naar de uitvoer van deze activiteit.

De reden dat deze strategie wordt gebruikt is voor het beste het spiegelen van de functionaliteit in de Orchestrator-runbook.  Activiteiten in een nieuwe grafische runbooks moeten verwijzen naar rechtstreeks met behulp van een Runbook invoer data source-invoerparameters.

### <a name="invoke-runbook-activity"></a>Activiteit Runbook aanroepen
Runbooks in Orchestrator start andere runbooks met de **Runbook aanroepen** activiteit. Als het runbook wordt geconverteerd, deze activiteit bevat en de **wachten op voltooiing** optie is ingesteld, wordt een runbook-activiteit in het geconverteerde runbook is gemaakt.  Als de **wachten op voltooiing** optie niet is ingesteld, wordt een werkstroom scriptactiviteit gemaakt die gebruikmaakt van **Start AzureAutomationRunbook** om het runbook te starten.  Nadat u het geconverteerde runbook in Azure Automation importeert, moet u deze activiteit wijzigen met de informatie die is opgegeven in de activiteit.

## <a name="related-articles"></a>Verwante artikelen:
* [System Center 2012 - Orchestrator](http://technet.microsoft.com/library/hh237242.aspx)
* [Service Management Automation](https://technet.microsoft.com/library/dn469260.aspx)
* [Hybride Runbook Worker](automation-hybrid-runbook-worker.md)
* [Standaard orchestrator-activiteiten](http://technet.microsoft.com/library/hh403832.aspx)
* [Download System Center Orchestrator Migration Toolkit](https://www.microsoft.com/en-us/download/details.aspx?id=47323)
