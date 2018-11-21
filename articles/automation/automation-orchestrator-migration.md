---
title: Migreren vanuit Orchestrator naar Azure Automation
description: Beschrijft hoe u System Center Orchestrator-runbooks en integration packs migreren naar Azure Automation.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 80b0523f8442e30e6af329263be454fa545933d6
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2018
ms.locfileid: "52275279"
---
# <a name="migrating-from-orchestrator-to-azure-automation-beta"></a>Migreren vanuit Orchestrator naar Azure Automation (bèta)
Runbooks in [System Center Orchestrator](https://technet.microsoft.com/library/hh237242.aspx) zijn gebaseerd op de activiteiten uit integratiepakketten die specifiek voor Orchestrator zijn geschreven, terwijl runbooks in Azure Automation zijn gebaseerd op Windows PowerShell.  [Grafische runbooks](automation-runbook-types.md#graphical-runbooks) in Azure Automation hebt u een vergelijkbare weergave voor Orchestrator-runbooks met hun activiteiten voor PowerShell-cmdlets, onderliggende runbooks en activa.

De [System Center Orchestrator Migration Toolkit](https://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) bevat hulpprogramma's om u te helpen bij het converteren van runbooks uit Orchestrator voor Azure Automation.  Naast de runbooks zelf converteert, moet u de integratiepakketten met de activiteiten die gebruikmaken van de runbooks converteren naar integratiemodules met Windows PowerShell-cmdlets.  

Hieronder volgt het basisproces voor het converteren van Orchestrator-runbooks voor Azure Automation.  Elk van deze stappen wordt beschreven in de onderstaande secties.

1. Download de [System Center Orchestrator Migration Toolkit](https://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) die bevat de hulpprogramma's en modules die in dit artikel worden besproken.
2. Importeren [standaardactiviteiten Module](#standard-activities-module) in Azure Automation.  Dit omvat geconverteerde versies van standard Orchestrator-activiteiten die kunnen worden gebruikt door geconverteerde runbooks.
3. Importeren [System Center Orchestrator-integratiemodules](#system-center-orchestrator-integration-modules) in Azure Automation voor deze integratiepakketten die worden gebruikt door uw runbooks die toegang hebben tot System Center.
4. Converteren van aangepaste en integratiepakketten van derden met behulp van de [Integration Pack Converter](#integration-pack-converter) en importeren in Azure Automation.
5. Converteren Orchestrator runbooks met de [Runbook Converter](#runbook-converter) en installeren in Azure Automation.
6. Handmatig vereiste Orchestrator assets in Azure Automation maken omdat de Runbook-Converter deze resources niet geconverteerd.
7. Configureer een [Hybrid Runbook Worker](#hybrid-runbook-worker) in uw lokale datacentrum om uit te voeren geconverteerde runbooks die toegang lokale bronnen tot.

## <a name="service-management-automation"></a>Service Management Automation
[Service Management Automation](https://technet.microsoft.com/library/dn469260.aspx) (SMA) worden opgeslagen en uitvoeren van runbooks in uw lokale datacentrum, zoals Orchestrator, en u de dezelfde integratiemodules als Azure Automation. De [Runbook Converter](#runbook-converter) wordt omgezet in Orchestrator-runbooks grafische runbooks maar die niet in SMA worden ondersteund.  U kunt nog steeds installeren de [Standard activiteiten Module](#standard-activities-module) en [integratiemodules van System Center Orchestrator](#system-center-orchestrator-integration-modules) in SMA worden ingevoerd, maar moet u handmatig [herschrijven van uw runbooks](https://technet.microsoft.com/library/dn469262.aspx).

## <a name="hybrid-runbook-worker"></a>Hybrid Runbook Worker
Runbooks in Orchestrator zijn opgeslagen op een database-server en op runbook-servers, zowel in uw lokale datacentrum worden uitgevoerd.  Runbooks in Azure Automation worden opgeslagen in de Azure-cloud en kunnen worden uitgevoerd in uw lokale gegevens center met een [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md).  Dit is hoe u runbooks geconverteerd van Orchestrator, omdat ze zijn ontworpen om te worden uitgevoerd op lokale servers gewoonlijk wordt uitgevoerd.

## <a name="integration-pack-converter"></a>Integration Pack Converter
De Integration Pack Converter converteert integratiepakketten die zijn gemaakt met behulp van de [Orchestrator Integration Toolkit (OIT)](https://technet.microsoft.com/library/hh855853.aspx) naar integratiemodules op basis van Windows PowerShell die kunnen worden geïmporteerd in Azure Automation of Service Management Automation.  

Wanneer u het Integration Pack conversieprogramma uitvoert, krijgt u een wizard waarmee u kunt een integration pack (oip)-bestand te selecteren.  De wizard klikt u vervolgens een lijst met de activiteiten die zijn opgenomen in dat integratiepakket en kunt u selecteren die worden gemigreerd.  Wanneer u de wizard hebt voltooid, maakt het een integratiemodule met een bijbehorende cmdlet voor elk van de activiteiten in het oorspronkelijke integratiepakket.

### <a name="parameters"></a>Parameters
Alle eigenschappen van een activiteit in het integratiepakket worden geconverteerd naar parameters van de bijbehorende cmdlet in de integratiemodule.  Windows PowerShell-cmdlets hebben een aantal [algemene parameters](https://technet.microsoft.com/library/hh847884.aspx) die kan worden gebruikt met alle cmdlets.  Bijvoorbeeld, de - uitgebreide parameter zorgt ervoor dat een cmdlet voor het uitvoeren van gedetailleerde informatie over de werking ervan.  Er is geen cmdlet kan een parameter met dezelfde naam als een algemene parameter hebben.  Als er een activiteit is een eigenschap met dezelfde naam als een algemene parameter, krijgt de wizard u een andere naam voor de parameter op te geven.

### <a name="monitor-activities"></a>Activiteiten bewaken
Bewaken van runbooks in Orchestrator beginnen met een [activiteiten](https://technet.microsoft.com/library/hh403827.aspx) en continu worden aangeroepen door een bepaalde gebeurtenis wordt nog uitgevoerd.  Azure Automation biedt geen ondersteuning voor runbooks van de monitor, zodat alle activiteiten controleren in het integratiepakket niet worden geconverteerd.  In plaats daarvan wordt een tijdelijke aanduiding voor cmdlet gemaakt in de integratiemodule voor de activiteit controleren.  Deze cmdlet heeft geen functionaliteit, maar hierdoor kan een willekeurig geconverteerde runbook gebruikt om te worden geïnstalleerd.  Dit runbook is niet mogelijk om uit te voeren in Azure Automation, maar deze kan worden geïnstalleerd zodat u kunt deze wijzigen.

### <a name="integration-packs-that-cannot-be-converted"></a>Integratiepakketten niet kunnen worden geconverteerd
Integratiepakketten die niet zijn gemaakt met OIT kunnen niet worden geconverteerd met de Integration Pack Converter. Er zijn ook enkele integratiepakketten die is geleverd door Microsoft op dit moment niet kan worden geconverteerd met dit hulpprogramma.  Geconverteerde versies van deze integratiepakketten zijn [gedownload](#system-center-orchestrator-integration-modules) zodat ze kunnen worden geïnstalleerd in Azure Automation of Service Management Automation.

## <a name="standard-activities-module"></a>Standaardactiviteiten-Module
Orchestrator bevat een set [standaardactiviteiten](https://technet.microsoft.com/library/hh403832.aspx) die niet zijn opgenomen in een integratiepakket, maar worden gebruikt door veel runbooks.  De standaardactiviteiten-module is een integratiemodule met inbegrip van een cmdlet equivalent voor elk van deze activiteiten.  Voordat u importeert geconverteerde runbooks die een standard-activiteit gebruiken, moet u deze integratiemodule installeren in Azure Automation.

Naast ondersteuning voor geconverteerde runbooks, kan de cmdlets in de module standaardactiviteiten door iemand die bekend zijn met Orchestrator worden gebruikt voor het bouwen van nieuwe runbooks in Azure Automation.  Hoewel de functionaliteit van de standaard activiteiten kan worden uitgevoerd met cmdlets, kunnen ze anders werken.  De cmdlets in de module geconverteerde standaardactiviteiten zal werken op dezelfde manier als hun bijbehorende activiteiten en gebruikt u dezelfde parameters.  Dit kan helpen de auteur van het bestaande Orchestrator runbook in de overgang naar Azure Automation-runbooks.

## <a name="system-center-orchestrator-integration-modules"></a>System Center Orchestrator-integratiemodules
Microsoft biedt [integratiepakketten](https://technet.microsoft.com/library/hh295851.aspx) voor het bouwen van runbooks voor het automatiseren van System Center-onderdelen en andere producten.  Sommige van deze integratiepakketten die momenteel zijn gebaseerd op OIT maar momenteel kan niet worden geconverteerd naar integratiemodules vanwege bekende problemen.  [System Center Orchestrator-integratiemodules](https://www.microsoft.com/download/details.aspx?id=49555) bevat geconverteerde versies van deze integratiepakketten die kunnen worden geïmporteerd in Azure Automation en Service Management Automation.  

Door de RTM-versie van dit hulpprogramma, wordt bijgewerkte versies van de integratiepakketten op basis van OIT die kan worden geconverteerd met de Integration Pack Converter gepubliceerd.  Richtlijnen wordt ook om u te helpen bij het converteren van runbooks met behulp van activiteiten van de integratiepakketten niet op basis van OIT worden opgegeven.

## <a name="runbook-converter"></a>Runbook-Converter
De Runbook-Converter converteert Orchestrator-runbooks in [grafische runbooks](automation-runbook-types.md#graphical-runbooks) die kunnen worden geïmporteerd in Azure Automation.  

Runbook-Converter wordt geïmplementeerd als een PowerShell-module met een cmdlet met de naam **ConvertFrom-SCORunbook** die de conversie wordt uitgevoerd.  Wanneer u het hulpprogramma hebt geïnstalleerd, wordt een snelkoppeling naar een PowerShell-sessie die wordt geladen van de cmdlet gemaakt.   

Hieronder volgt het basisproces te converteren van een Orchestrator-runbook importeren in Azure Automation.  De volgende secties bevatten meer informatie over het gebruik van het hulpprogramma en werken met geconverteerde runbooks.

1. Een of meer runbooks exporteren uit Orchestrator.
2. Integratiemodules voor alle activiteiten in het runbook verkrijgen.
3. De Orchestrator-runbooks in het geëxporteerde bestand converteren.
4. Lees de informatie in de logboeken voor het valideren van de conversie en het bepalen van alle vereiste handmatige taken.
5. Geconverteerde runbooks in Azure Automation importeren.
6. Maak alle vereiste assets in Azure Automation.
7. Bewerk het runbook in Azure Automation, zodat het geen vereiste activiteiten wijzigen.

### <a name="using-runbook-converter"></a>Met behulp van Runbook-Converter
De syntaxis voor **ConvertFrom-SCORunbook** is als volgt:

```powershell
ConvertFrom-SCORunbook -RunbookPath <string> -Module <string[]> -OutputFolder <string>
```

* RunbookPath - pad naar de export-bestand met de runbooks om te converteren.
* Module - met door komma's gescheiden lijst met activiteiten in de runbooks integratiemodules.
* OutputFolder - pad naar de map maken geconverteerd grafische runbooks.

De volgende opdracht converteert de runbooks in een exportbestand met de naam **MyRunbooks.ois_export**.  Deze runbooks gebruikt de Active Directory en Data Protection Manager-integratiepakketten.

```powershell
ConvertFrom-SCORunbook -RunbookPath "c:\runbooks\MyRunbooks.ois_export" -Module c:\ip\SystemCenter_IntegrationModule_ActiveDirectory.zip,c:\ip\SystemCenter_IntegrationModule_DPM.zip -OutputFolder "c:\runbooks"
```

### <a name="log-files"></a>Logboekbestanden
De Runbook-Converter maakt de volgende logboekbestanden op dezelfde locatie als de geconverteerde runbook.  Als de bestanden al bestaat, wordt deze overschreven met de gegevens van de laatste conversie.

| File | Inhoud |
|:--- |:--- |
| Runbook-Converter - Progress.log |Gedetailleerde stappen van de conversie met inbegrip van informatie voor elke activiteit is geconverteerd en waarschuwing voor elke activiteit die niet worden geconverteerd. |
| Runbook-Converter - Summary.log |Overzicht van de laatste conversie, met inbegrip van eventuele waarschuwingen en taken die u nodig hebt om uit te voeren zoals het maken van een variabele die is vereist voor het geconverteerde runbook opvolgen. |

### <a name="exporting-runbooks-from-orchestrator"></a>Runbooks exporteren vanuit Orchestrator
De Runbook-Converter werkt met een exportbestand van Orchestrator met een of meer runbooks.  Een corresponderende Azure Automation-runbook voor elke Orchestrator-runbook wordt gemaakt in het exportbestand.  

Voor het exporteren van een runbook uit Orchestrator, met de rechtermuisknop op de naam van het runbook in Runbook Designer en selecteer **exporteren**.  Voor het exporteren van alle runbooks in een map met de rechtermuisknop op de naam van de map en selecteer **exporteren**.

### <a name="runbook-activities"></a>Runbook-activiteiten
De Runbook-Converter converteert elke activiteit in de Orchestrator-runbook en een bijbehorende activiteit in Azure Automation.  Voor de activiteiten die kunnen niet worden geconverteerd, wordt een tijdelijke aanduiding voor activiteit gemaakt in het runbook met de waarschuwing.  Nadat u de geconverteerde runbook in Azure Automation importeert, moet u een van deze activiteiten vervangen door geldige activiteiten die de vereiste functionaliteit uitvoeren.

Een Orchestrator-activiteiten in de [Standard activiteiten Module](#standard-activities-module) worden geconverteerd.  Er zijn enkele standaardactiviteiten voor Orchestrator die zich niet in deze module echter niet worden geconverteerd.  Bijvoorbeeld, **platformgebeurtenis verzenden** heeft geen equivalent van de Azure Automation sinds de gebeurtenis specifiek voor Orchestrator is.

[Activiteiten controleren](https://technet.microsoft.com/library/hh403827.aspx) niet worden geconverteerd omdat er geen equivalent voor deze in Azure Automation is.  De uitzondering zijn monitor activiteiten in [integratiepakketten geconverteerd](#integration-pack-converter) die worden geconverteerd naar de tijdelijke aanduiding-activiteit.

Elke activiteit uit een [geconverteerd integratiepakket](#integration-pack-converter) worden geconverteerd als u het pad naar de integratiemodule met de **modules** parameter.  Voor de integratiepakketten voor System Center, kunt u de [integratiemodules van System Center Orchestrator](#system-center-orchestrator-integration-modules).

### <a name="orchestrator-resources"></a>Orchestrator-bronnen
De Runbook-Converter converteert alleen runbooks, geen andere Orchestrator-bronnen zoals tellers, variabelen of verbindingen.  Items worden niet ondersteund in Azure Automation.  Variabelen en verbindingen worden ondersteund, maar u moet deze handmatig maken.  De logboekbestanden wordt gemeld als het runbook is vereist voor deze resources en bijbehorende bronnen die u nodig hebt om te maken in Azure Automation voor het geconverteerde runbook correcte werking opgeven.

Een runbook kan bijvoorbeeld een variabele gebruiken voor het vullen van een bepaalde waarde in een activiteit.  Het geconverteerde runbook converteren van de activiteit en een variabele asset opgeven in Azure Automation met dezelfde naam als de Orchestrator-variabele.  Dit wordt weergegeven de **conversieprogramma voor Runbook - Summary.log** -bestand dat is gemaakt na de conversie.  U moet deze variabele asset handmatig maken in Azure Automation voordat u het runbook.

### <a name="input-parameters"></a>Invoerparameters
Runbooks in Orchestrator accepteren invoerparameters die zijn opgegeven met de **gegevens initialiseren** activiteit.  Als het runbook wordt omgezet, deze activiteit bevat, wordt er een [invoerparameter](automation-graphical-authoring-intro.md#runbook-input-and-output) in de Azure Automation runbook is gemaakt voor elke parameter in de activiteit.  Een [Werkstroomscript besturingselement](automation-graphical-authoring-intro.md#activities) activiteit wordt gemaakt in het geconverteerde runbook die worden opgehaald en elke parameter retourneert.  Alle activiteiten in het runbook die gebruikmaken van een invoerparameter verwijzen naar de uitvoer van deze activiteit.

De reden dat deze strategie wordt gebruikt is om het beste mirror van de functionaliteit in de Orchestrator-runbook.  Activiteiten in de nieuwe grafische runbooks moeten verwijzen naar rechtstreeks met behulp van een bron van de invoergegevens Runbook-invoerparameters.

### <a name="invoke-runbook-activity"></a>Activiteit Runbook aanroepen
Runbooks in Orchestrator start andere runbooks met de **Runbook aanroepen** activiteit. Als het runbook wordt omgezet, deze activiteit bevat en de **wachten op voltooiing** optie is ingesteld, wordt een runbook-activiteit in het geconverteerde runbook is gemaakt.  Als de **wachten op voltooiing** optie niet is ingesteld, wordt een werkstroom scriptactiviteit gemaakt die gebruikmaakt van **Start AzureAutomationRunbook** om het runbook te starten.  Nadat u de geconverteerde runbook in Azure Automation importeert, moet u deze activiteit wijzigen met de informatie die is opgegeven in de activiteit.

## <a name="related-articles"></a>Verwante artikelen:
* [System Center 2012 - Orchestrator](https://technet.microsoft.com/library/hh237242.aspx)
* [Service Management Automation](https://technet.microsoft.com/library/dn469260.aspx)
* [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md)
* [Standaardactiviteiten voor orchestrator](https://technet.microsoft.com/library/hh403832.aspx)
* [Download System Center Orchestrator Migratietoolkit](https://www.microsoft.com/en-us/download/details.aspx?id=47323)
