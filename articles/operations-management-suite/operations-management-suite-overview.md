---
title: Overzicht van Operations Management Suite (OMS) | Microsoft Docs
description: Microsoft Operations Management Suite (OMS) is een cloudoplossing voor IT-beheer van Microsoft waarmee u uw on-premises en cloudinfrastructuur kunt beheren en beveiligen.  In dit artikel worden de waarde van OMS en de verschillende services en functies van OMS beschreven. Ook vindt u hier koppelingen naar de gedetailleerde inhoud ervan.
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 9dc437b9-e83c-45da-917c-cb4f4d8d6333
ms.service: operations-management-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2017
ms.author: bwren
ms.openlocfilehash: 94dedebe48060441cd3167fea87f6b721eb14517
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="what-is-operations-management-suite-oms"></a>Wat is Operations Management Suite (OMS)?
Dit artikel is een inleiding tot Operations Management Suite (OMS) met inbegrip van een kort overzicht van de bedrijfswaarde die het biedt, de services en beheeroplossingen die het bevat, en de aanbiedingen waarin verschillende services en oplossingen worden gecombineerd.  Bovendien vindt u hier koppelingen naar gedetailleerde documentatie over de implementatie en het gebruik van de diverse services en oplossingen.

## <a name="from-on-premises-to-the-cloud"></a>Van on-premises naar de cloud
Microsoft biedt al langere tijd producten voor het beheren van bedrijfsoplossingen.  In 2007 werden meerdere producten samengevoegd in de System Center-suite met beheerproducten.  Deze bevatte Configuration Manager, dat functies biedt zoals softwaredistributie en -inventaris; Operations Manager, dat proactieve bewaking voor systemen en toepassingen biedt; Orchestrator, dat runbooks bevat voor het automatiseren van handmatige processen; en Data Protection Manager voor het maken van back-ups en het herstellen van kritieke gegevens.

Doordat steeds meer computerresources werden verplaatst naar de cloud, kregen System Center-producten meer cloudfuncties, zoals Operations Manager en Orchestrator voor het beheer van resources in Azure.  Ze waren echter nog steeds primair bedoeld als on-premises oplossingen, en vereisten daarom een aanzienlijke investering in implementatie en onderhoud van een on-premises beheeromgeving.  Er was een nieuwe benadering van beheer nodig om volledig te profiteren van de cloud en toekomstige toepassingen te kunnen ondersteunen.

## <a name="introducing-operations-management-suite"></a>Kennismaking met Operations Management Suite
Operations Management Suite (ook wel bekend als OMS) is een verzameling beheerservices die volledig op de cloud zijn gericht.  In plaats van on-premises resources te implementeren en beheren, worden OMS-onderdelen volledig in Azure gehost.  De benodigde configuratie is minimaal en u kunt alles letterlijk binnen een paar minuten in werking zetten.  

- **Implementatie met minimale kosten en complexiteit.**  Omdat alle onderdelen en gegevens van OMS in Azure worden opgeslagen, kunt u in korte tijd alles gereed hebben zonder de complexiteit van en investering in on-premises onderdelen.
- **Gemakkelijk schalen in de cloud.**  U hoeft zich geen zorgen te maken dat u betaalt voor computerresources die u niet nodig hebt of dat u onvoldoende opslagruimte hebt. Dankzij de cloudimplementatie betaalt u alleen voor wat u daadwerkelijk gebruikt en kunt u gemakkelijk omhoog schalen naar precies die gewenste belasting.  U kunt om te beginnen een paar resources beheren en vervolgens omhoog schalen naar uw complete omgeving.
- **Profiteer van de nieuwste functies.**  Er worden continu functies aan OMS toegevoegd en bestaande functies worden bijgewerkt.  Van al deze functies kunt u op elk gewenst moment gebruikmaken zonder updates te hoeven implementeren.
- **Geïntegreerde services.**  Elke afzonderlijke OMS-service voegt waarde toe, maar samen kunnen de services complexe beheerscenario's oplossen.  Een runbook in Azure Automation kan bijvoorbeeld een failover-proces aansturen met Azure Site Recovery en vervolgens informatie vastleggen in Log Analytics om een waarschuwing te genereren.
- **Wereldwijde kennis.**  De beheeroplossingen in OMS hebben continu toegang tot de meest recente informatie.  De beveiligings- en controleoplossing kan bijvoorbeeld een bedreigingsanalyse uitvoeren aan de hand van de nieuwste bedreigingen die wereldwijd worden gedetecteerd.
- **Toegang waar u ook bent.**  U hebt toegang tot uw beheeromgeving vanaf elke locatie, mits u de beschikking hebt over een browser.  Installeer de OMS-app op uw smartphone voor snelle toegang tot uw bewakingsgegevens.

### <a name="is-it-just-for-the-cloud"></a>Is OMS alleen bedoeld voor de cloud?
Hoewel OMS-services worden uitgevoerd in de cloud, betekent dat niet dat ze uw on-premises omgeving niet effectief kunnen beheren.  Als u in uw datacenter een agent op een Windows- of Linux-computer installeert, stuurt deze gegevens naar Log Analytics. Hier kunnen ze worden geanalyseerd samen met alle overige gegevens die met de cloud- of on-premises services zijn verzameld.  Gebruik Azure Backup en Azure Site Recovery om back-ups op te slaan in de cloud en een hoge beschikbaarheid voor on-premises resources te garanderen.  
Runbooks in de cloud hebben doorgaans geen toegang tot uw on-premises resources, maar u kunt ook een agent installeren op een of meer computers die vervolgens runbooks hosten in uw datacenter.  Wanneer u een runbook start, kunt u eenvoudig aangeven of u het wilt uitvoeren in de cloud of op een lokale werkrol.

## <a name="hybrid-management-with-system-center"></a>Hybride beheer met System Center
Als u een bestaande installatie van System Center hebt, kunt u onderdelen integreren met OMS-services om een hybride oplossing voor zowel uw on-premises als cloudomgevingen te creëren, waarbij u gebruikmaakt van de relatieve specialisaties van elk product.  Verbind uw bestaande Operations Manager-beheergroep met Log Analytics om beheerde agents in de cloud te analyseren.  Gebruik uw bestaande back-upproces met Data Protection Manager om back-ups van uw gegevens op te slaan in de cloud.  


## <a name="oms-services"></a>OMS-services
De kernfunctionaliteit van OMS wordt geleverd door een reeks services die in Azure worden uitgevoerd.  Elke service biedt een specifieke beheerfunctie. U kunt services combineren om verschillende beheerscenario's te bewerkstelligen.

|| Service | Beschrijving |
|:--|:--|:--|
| ![Log Analytics](media/operations-management-suite-overview/icon-log-analytics.png) | Log Analytics | Bewaak en analyseer de beschikbaarheid en prestaties van verschillende resources, met inbegrip van fysieke en virtuele machines. |
| ![Azure Automation](media/operations-management-suite-overview/icon-automation.png) | Automatisering | Automatiseer handmatige processen en dwing configuraties af voor fysieke en virtuele machines. |
| ![Azure Backup](media/operations-management-suite-overview/icon-backup.png) | Back-up | Maak back-ups van en herstel kritieke gegevens. |
| ![Azure Site Recovery](media/operations-management-suite-overview/icon-site-recovery.png) | Site Recovery | Bied hoge beschikbaarheid voor kritieke toepassingen. |

### <a name="log-analytics"></a>Log Analytics
[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) biedt bewakingsservices voor OMS door in een centrale opslagplaats gegevens te verzamelen van beheerde resources.  Deze gegevens kunnen gebeurtenissen, prestatiegegevens en aangepaste gegevens omvatten die via de API worden geleverd. Na verzameling zijn de gegevens beschikbaar voor waarschuwingen, analyse en export.  Met deze methode kunt u gegevens uit een groot aantal bronnen consolideren, zodat u gegevens uit uw Azure-services met uw bestaande on-premises omgeving kunt combineren.  De methode maakt ook een duidelijk onderscheid tussen het verzamelen van gegevens en het bewerken hiervan. Zo zijn alle bewerkingen beschikbaar voor alle soorten gegevens.  

![Overzicht van Log Analytics](media/operations-management-suite-overview/overview-log-analytics.png)

#### <a name="collecting-data"></a>Verzamelen van gegevens
Er zijn tal van manieren waarop u gegevens in de opslagplaats kunt verzamelen, zodat Log Analytics ze kan analyseren.

- **Windows- of Linux-computers en -VM's.**  U installeert de Microsoft Monitoring Agent op [Windows](../log-analytics/log-analytics-windows-agent.md)- en [Linux](../log-analytics/log-analytics-linux-agents.md)-computers of -VM's waarvan u gegevens wilt verzamelen.  De agent downloadt vanuit Log Analytics automatisch een configuratie die de gebeurtenissen en prestatiegegevens definieert die moeten worden verzameld.  Met behulp van Azure Portal kunt u de agent gemakkelijk installeren op virtuele machines die in Azure worden uitgevoerd.  Als u een bestaande Operations Manager-omgeving hebt, kunt u de beheergroep koppelen aan Log Analytics en automatisch gegevens gaan verzamelen van alle bestaande agenten.
- **Azure-services.**  Log Analytics verzamelt in de opslagplaats telemetrie van [Azure Diagnostics en Azure Monitor](../log-analytics/log-analytics-azure-storage.md), zodat u Azure-resources kunt bewaken.
- **Gegevensverzamelaar-API.**  Log Analytics heeft een [REST-API voor het invullen van gegevens vanuit elke client](../log-analytics/log-analytics-data-collector-api.md).  Hiermee kunt u gegevens verzamelen vanuit toepassingen van derden en aangepaste beheerscenario's implementeren.  Een veelvoorkomende methode is het gebruik van een runbook in Azure Automation om gegevens te verzamelen en vervolgens de gegevensverzamelaar-API te gebruiken om de gegevens naar de opslagplaats te schrijven.

#### <a name="reporting-and-analyzing-data"></a>Rapporteren en analyseren van gegevens
Log Analytics bevat een krachtige querytaal om gegevens te extraheren die zijn opgeslagen in de opslagplaats.  Omdat de gegevens van alle bronnen worden opgeslagen als records, kunt u gegevens uit meerdere bronnen in één query analyseren.
  
Log Analytics biedt naast ad-hocanalyse meerdere manieren om gegevens te rapporteren en analyseren vanuit een query.

- **Weergaven en dashboards.**  [Weergaven](../log-analytics/log-analytics-view-designer.md) en [dashboards](../log-analytics/log-analytics-dashboards.md) visualiseren de resultaten van een query in de portal.  Beheeroplossingen bevatten meestal weergaven die de gegevens vanuit de oplossing analyseren.  U kunt ook uw eigen aangepaste weergaven maken voor het analyseren van gegevens en ze direct beschikbaar maken in uw aangepaste portal.
- **Exporteren.**  U kunt de resultaten van een willekeurige query exporteren zodat u ze kunt analyseren buiten Log Analytics.  U kunt zelfs een reguliere export naar [Power BI](../log-analytics/log-analytics-powerbi.md) plannen, die omvangrijke visualisatie- en analysemogelijkheden biedt.
- **API voor zoeken in logboeken.**  Log Analytics heeft een [REST-API voor het verzamelen van gegevens vanuit elke client](../log-analytics/log-analytics-log-search-api.md).  Hierdoor kunt u programmatisch werken met gegevens die in de opslagplaats worden verzameld of ze openen vanuit een ander hulpprogramma voor bewaking.

#### <a name="alerting"></a>Waarschuwingen
Log Analytics kan u [proactief waarschuwen](../log-analytics/log-analytics-alerts.md) of corrigerende actie ondernemen wanneer er een probleem wordt gedetecteerd.  Net als andere analyses in Log Analytics wordt dit gedaan met zoeken in logboeken.  Deze zoekopdracht wordt volgens een vast schema uitgevoerd en er wordt een waarschuwing gemaakt als de resultaten aan bepaalde criteria voldoen.

![Waarschuwingen in Log Analytics](media/operations-management-suite-overview/overview-alerts.png)

Naast het maken van een waarschuwingenrecord in de Log Analytics-opslagplaats, kunnen waarschuwingen de volgende acties uitvoeren.

- **E-mail.**  Een e-mail verzenden om u proactief te waarschuwen over een gedetecteerd probleem.
- **Runbook.**  Een waarschuwing in Log Analytics kan een runbook starten in Azure Automation.  Dit gebeurt meestal om te proberen het gedetecteerde probleem te corrigeren.  Het runbook kan worden gestart in de cloud in het geval van een probleem in Azure of een andere cloud. Het kan ook op een lokale agent worden gestart bij een probleem op een fysieke of virtuele machine.
- **Webhook.**  Een waarschuwing kan een webhook starten en er gegevens aan doorgeven uit de resultaten van de zoekopdracht in logboeken.  Dit maakt integratie met externe services, zoals een alternatief waarschuwingssysteem, mogelijk. Het kan ook proberen om een corrigerende actie uit te voeren voor een externe website.

### <a name="azure-automation"></a>Azure Automation
[Azure Automation](http://azure.microsoft.com/documentation/services/automation) biedt procesautomatisering en configuratiebeheer voor OMS.  Het automatiseert handmatige processen en helpt configuraties af te dwingen voor fysieke en virtuele computers.  

#### <a name="process-automation"></a>Procesautomatisering
Azure Automation automatiseert handmatige processen met behulp van [runbooks](../automation/automation-runbook-types.md) die zijn gebaseerd op een PowerShell-script of PowerShell-werkstroom.  Dit omvat ook assets die runbooks ondersteunen, zoals variabelen die kunnen worden gedeeld tussen meerdere runbooks en referenties en verbindingen waarmee u versleutelde gegevens kunt opslaan die mogelijk vereist zijn voor de verificatie van een runbook.
Runbooks bieden procesautomatisering voor de andere services in de suite.  Omdat elk van de andere services toegankelijk is met PowerShell of via een REST-API, kunt u runbooks maken om functies uit te voeren zoals het verzamelen van beheergegevens in Log Analytics of het initiëren van een back-up met Azure Backup.

##### <a name="accessing-resources"></a>Toegang tot resources
Omdat runbooks zijn gebaseerd op PowerShell, kunnen ze alle resources beheren die toegankelijk zijn met de PowerShell-cmdlets.  Wanneer u [een module laadt](../automation/automation-integration-modules.md) in uw Automation-account, wordt deze beschikbaar voor alle runbooks in dat account. 
 
Wanneer runbooks worden uitgevoerd in de cloud, kunnen ze alle resources openen die toegankelijk zijn vanuit de cloud.  Dit kunnen resources zijn in uw Azure-abonnement, in een andere cloud zoals Amazon Web Services (AWS), of een service die toegankelijk is via een REST-API.  Runbooks in de cloud worden niet uitgevoerd onder referenties, maar kunnen gebruikmaken van Automation-assets zoals referenties, verbindingen en certificaten om toegang tot resources te verifiëren.

Resources in uw datacenter zijn waarschijnlijk niet toegankelijk vanuit een runbook dat wordt uitgevoerd in de cloud.  U kunt wel een of meer [Hybrid Runbook Workers](../automation/automation-hybrid-runbook-worker.md) installeren in uw datacenter om runbooks uit te voeren waarvoor toegang tot lokale resources vereist is.  Wanneer u een runbook start, geeft u op of u het wilt uitvoeren in de cloud of op een specifieke werkrol.

![Azure Automation-runbooks](media/operations-management-suite-overview/overview-runbooks.png)

##### <a name="starting-a-runbook"></a>Een runbook starten
Runbooks kunnen worden [gestart via een aantal methoden](../automation/automation-starting-a-runbook.md) zodat ze kunnen worden opgenomen in verschillende beheerscenario's.  

- **Azure Portal.**  Net als andere Azure-services kan Azure Automation worden beheerd via Azure Portal.  U kunt niet alleen runbooks starten, maar ze ook importeren of uw eigen maken.
- **Gepland.**  U kunt plannen dat runbooks met regelmatige tussenpozen starten.  Hierdoor kunt u een regulier beheerproces automatisch herhalen of gegevens verzamelen in Log Analytics.
- **PowerShell en API.**  U kunt runbooks starten en ze vereiste parameterinformatie doorgeven van een PowerShell-cmdlet of de REST-API van Azure Automation.  
- **Webhook.**  Eer kan een webhook worden gemaakt voor elke runbook zodat het kan worden gestart vanaf externe toepassingen of websites.
- **Waarschuwing in Log Analytics.**  Een waarschuwing in Log Analytics kan automatisch een runbook starten om te proberen het probleem dat is geïdentificeerd door de waarschuwing te corrigeren.

#### <a name="configuration-management"></a>Configuration Management
[PowerShell Desired State Configuration (DSC)](../automation/automation-dsc-overview.md) is een beheerplatform in Windows PowerShell waarmee u de configuratie van fysieke en virtuele machines kunt implementeren en afdwingen.  Azure Automation beheert DSC-configuraties en biedt een pull-server in de cloud die agents kunnen openen om de vereiste configuraties op te halen.

![Azure Automation DSC](media/operations-management-suite-overview/overview-dsc.png)

### <a name="azure-backup-and-azure-site-recovery"></a>Azure Backup en Azure Site Recovery
Azure Backup en Azure Site Recovery dragen bij aan bedrijfscontinuïteit en herstel na noodgevallen.  Ze hebben allebei functies waarmee u ervoor kunt zorgen dat toepassingen beschikbaar blijven als er storingen optreden en terugkeren naar de normale werking wanneer de systemen weer online zijn.  Beide services dragen bij aan de doelstellingen voor herstelpunten (RPO's) en de doelstellingen voor het verlagen van hersteltijden (RTO's) die zijn gedefinieerd voor uw organisatie. Uw RPO bepaalt de acceptabele limiet waarbij gegevens niet beschikbaar zijn tijdens een storing en de RTO bepaalt de limiet voor de acceptabele hoeveelheid tijd waarin een service of app niet beschikbaar is bij een storing.

#### <a name="azure-backup"></a>Azure Backup
[Azure Backup](http://azure.microsoft.com/documentation/services/backup) biedt services voor het maken van back-ups en herstellen van gegevens voor OMS.  Het beschermt uw toepassingsgegevens en bewaart deze jarenlang, zonder dat u grote investeringen hoeft te doen en met een minimum aan operationele kosten.  U kunt er back-ups van gegevens mee maken afkomstig van fysieke en virtuele Windows-servers, naast back-ups van werkbelastingen zoals SQL Server en SharePoint.  Azure Backup kan ook worden gebruikt door System Center Data Protection Manager (DPM) om beveiligde gegevens te repliceren naar Azure voor redundantie en opslag voor de lange termijn.

Beveiligde gegevens in Azure Backup worden opgeslagen in een back-upkluis in een bepaalde geografische regio. De gegevens worden in dezelfde regio gerepliceerd en kunnen, afhankelijk van het type kluis, ook naar een andere regio worden gerepliceerd voor meer flexibiliteit.

Azure Backup heeft drie fundamentele scenario's.

- **Windows-computer met Azure Backup-agent.** Hiermee kunt u vanaf elke Windows-server of -client rechtstreeks naar uw Azure Backup-kluis back-ups van bestanden en mappen maken.<br><br>![Windows-computer met Azure Backup-agent](media/operations-management-suite-overview/overview-backup-01.png)
- **System Center Data Protection Manager (DPM) of Microsoft Azure Backup Server.** Hiermee kunt u de DPM of Microsoft Azure Backup Server gebruiken om van bestanden en mappen en workloads van toepassingen, zoals SQL en SharePoint, back-ups te maken naar de lokale opslag en deze vervolgens te repliceren naar uw Azure Backup Vault. Ondersteunt Windows- en Linux-VM's op Hyper-V of VMware.<br><br>![System Center Data Protection Manager (DPM) of Microsoft Azure Backup Server](media/operations-management-suite-overview/overview-backup-02.png)
- **Extensies van virtuele Azure-machines.** Maak back-ups van Windows- of Linux-VM's in Azure naar uw Azure Backup Vault.<br><br>![Extensies voor virtuele Azure-machines](media/operations-management-suite-overview/overview-backup-03.png)



#### <a name="azure-site-recovery"></a>Azure Site Recovery
[Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) biedt bedrijfscontinuïteit bij het orkestreren van replicatie van on-premises virtuele en fysieke machines naar Azure of een secundaire site. Als uw primaire site niet beschikbaar is, kunt u een failover toepassen naar de secundaire locatie zodat gebruikers kunnen doorwerken en failback toepassen wanneer systemen weer normaal beginnen te functioneren. 

Azure Site Recovery biedt hoge beschikbaarheid voor servers en toepassingen.  Het draagt bij aan de bedrijfscontinuïteit en de strategie voor herstel na noodgevallen (BCDR) door de replicatie, failover en het herstel van virtuele on-premises Hyper-V-machines, virtuele VMware-machines en fysieke Windows/Linux-servers in te delen. U kunt machines naar een secundair datacenter repliceren of uw datacentrum uitbreiden door ze naar Azure te repliceren. Site Recovery biedt ook eenvoudige functionaliteit voor failover en herstel voor werkbelastingen. Site Recovery is geïntegreerd met mechanismen voor herstel na noodgevallen, zoals SQL Server AlwaysOn, en biedt plannen voor herstel voor een eenvoudige failover van werkbelastingen die zich gelaagd op meerdere computers bevinden.

Azure Site Recovery heeft drie fundamentele replicatiescenario's.

- **Virtuele Hyper-V-machines repliceren.**  Als virtuele Hyper-V-machines worden beheerd in VMM-clouds, kunt u naar een secundair datacenter of naar de Azure-opslag repliceren. Replicatie naar Azure gaat via een beveiligde internetverbinding. Replicatie naar een secundair datacenter gaat via LAN.  Als virtuele Hyper-V-machines niet met VMM worden beheerd, kunt u alleen naar de Azure-opslag repliceren. Replicatie naar Azure gaat via een beveiligde internetverbinding.<br><br>![Virtuele Hyper-V-machines repliceren](media/operations-management-suite-overview/overview-siterecovery-hyperv.png)
- **Virtuele VMware-machines repliceren.**  U kunt on-premises virtuele VMware-machines repliceren naar een secundair datacenter met VMware of naar de Azure-opslag. Replicatie naar Azure kan plaatsvinden via een VPN tussen sites, via Azure ExpressRoute of via een beveiligde internetverbinding. Replicatie naar een secundair datacenter vindt plaats via het InMage Scout-gegevenskanaal.<br><br>![Virtuele VMware-machines repliceren](media/operations-management-suite-overview/overview-siterecovery-vmware.png)
- **Fysieke Windows- en Linux-servers repliceren**  U kunt fysieke servers repliceren naar een secundair datacenter of naar de Azure-opslag. Replicatie naar Azure kan plaatsvinden via een VPN tussen sites, via Azure ExpressRoute of via een beveiligde internetverbinding. Replicatie naar een secundair datacenter vindt plaats via het InMage Scout-gegevenskanaal. Azure Site Recovery heeft een OMS-oplossing waarmee enkele statistieken worden weergegeven. Voor bewerkingen moet u echter de Azure-portal gebruiken.<br><br>![Fysieke Windows- en Linux-servers repliceren](media/operations-management-suite-overview/overview-siterecovery-physical.png)


Met Site Recovery worden metagegevens opgeslagen in kluizen in een bepaalde geografische Azure-regio. Er worden geen gerepliceerde gegevens opgeslagen met de Site Recovery-service.

## <a name="management-solutions"></a>Beheeroplossingen
[Beheeroplossingen](operations-management-suite-solutions.md) zijn voorverpakte sets met logica die een bepaald beheerscenario implementeren door van een of meer OMS-services gebruik te maken.  Er zijn verschillende oplossingen beschikbaar van Microsoft en partners die u gemakkelijk kunt toevoegen aan uw Azure-abonnement om de waarde van uw investering in OMS te vergroten.  Als partner kunt u uw eigen oplossingen maken voor het ondersteunen van uw toepassingen en services en ze aanbieden aan gebruikers via de Azure Marketplace of snel starten-sjablonen.

Een goed voorbeeld van een oplossing die gebruikmaakt van meerdere services om aanvullende functionaliteit te bieden, is de [oplossing voor updatebeheer](oms-solution-update-management.md).  Deze oplossing maakt gebruik van de Log Analytics-agent voor Windows en Linux om gegevens te verzamelen over vereiste updates op elke agent.  Het schrijft deze gegevens naar de opslagplaats van Log Analytics waar u ze kunt analyseren met een inbegrepen dashboard.  Wanneer u een implementatie maakt, worden runbooks in Azure Automation gebruikt om vereiste updates te installeren.  U beheert dit hele proces in de portal en hoeft zich geen zorgen te maken over de onderliggende details.

![Oplossing](media/operations-management-suite-overview/overview-solution.png)

De meeste oplossingen kunnen een of meer van de volgende functies uitvoeren.

- Verzamelen van aanvullende gegevens.  Log Analytics verzamelt een verscheidenheid aan gegevens van clients en services inclusief gebeurtenissen en prestatiegegevens.  Een beheeroplossing kan aanvullende informatie verzamelen die niet beschikbaar is vanuit andere gegevensbronnen, vaak met behulp van Azure Automation-runbooks.
- Bieden van aanvullende analyse van verzamelde gegevens.  Beheeroplossingen omvatten dashboards en weergaven die analyse en visualisatie van gegevens bieden.  Deze koppelen terug naar vooraf gedefinieerde zoekopdrachten in logboeken waarmee u verder in kunt gaan op de gedetailleerde gegevens.  Ze kunnen ook analyses uitvoeren op gegevens die al zijn verzameld in de opslagplaats, die bijvoorbeeld beveiligingsgebeurtenissen doorzoeken op patronen die wijzen op een bedreiging.
- Toevoegen van functionaliteit.  Een aantal oplossingen die worden geleverd door Microsoft kunnen bouwen op de mogelijkheden van de basisservices om aanvullende functionaliteit te bieden.  Serviceoverzicht biedt bijvoorbeeld een eigen console voor het detecteren en in kaart brengen van server- en procesafhankelijkheden in realtime.
Er worden regelmatig oplossingen aan OMS toegevoegd door Microsoft en partners, zodat de waarde van uw investering continu wordt vergroot.  U kunt zoeken naar Microsoft-oplossingen en ze installeren via de oplossingencatalogus in de OMS-portal of zoeken naar oplossingen van Microsoft en partners en ze installeren via de Azure Marketplace in Azure Portal.  

![Galerie van oplossingen](media/operations-management-suite-overview/solution-gallery.png)


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics).
* Meer informatie over [Azure Automation](../automation/automation-intro.md).
* Meer informatie over [Azure Backup](http://azure.microsoft.com/documentation/services/backup).
* Meer informatie over [Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery).
* Ontdek de [beschikbare oplossingen](../log-analytics/log-analytics-add-solutions.md) in de verschillende OMS-aanbiedingen. 

