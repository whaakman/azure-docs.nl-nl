---
title: Herstel na noodgevallen voor SaaS-apps met behulp van Azure SQL Database Geo-replicatie | Microsoft Docs
description: Informatie over het gebruik van Azure SQL Database-geo-replica's voor het herstellen van een multitenant SaaS-app in het geval van een storing
keywords: zelfstudie sql-database
services: sql-database
author: AyoOlubeko
manager: craigg
ms.service: sql-database
ms.custom: saas apps
ms.topic: article
ms.date: 04/09/2018
ms.author: ayolubek
ms.openlocfilehash: c6f3da52643caa9aa1172db5b884c5336c409715
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/20/2018
---
# <a name="disaster-recovery-for-a-multi-tenant-saas-application-using-database-geo-replication"></a>Herstel na noodgevallen voor een multitenant SaaS-toepassing met behulp van de database geo-replicatie

In deze zelfstudie maakt kennis u met een volledige noodherstelscenario voor een multitenant SaaS-toepassing geïmplementeerd met behulp van het model van de database per tenant. U gebruikt om te voorkomen dat de app een storing, [ _geo-replicatie_ ](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-geo-replication-overview) voor het maken van replica's voor de catalogus en tenant-databases in een regio alternatieve herstel. Als er een storing optreedt, failover u snel voor deze replica's normale zakelijke activiteiten te hervatten. Failover worden de databases in de oorspronkelijke regio secundaire replica's van de databases in de regio van het herstel. Wanneer deze replica's terug online achterstand ze automatisch in de status van de databases in de regio van het herstel. Nadat de onderbreking opgelost is, schakelt u terug naar de databases in de oorspronkelijke productieregio.

Deze zelfstudie behandelt de failover en failback workflows. U leert hoe:
> [!div classs="checklist"]

>* Synchronisatiedatabase en configuratiegegevens over elastische pool in de tenant-catalogus
>* Een Herstelomgeving in een andere regio, die bestaat uit de toepassing, servers en toepassingen instellen
>* Gebruik _geo-replicatie_ voor het repliceren van de catalogus en tenant-databases naar de regio herstel
>* Failover van de toepassing en de catalogus en tenant-databases voor de regio herstel 
>* Later, failover voor de toepassing, catalogus- en tenantverkeer databases terug naar de oorspronkelijke regio nadat de onderbreking is opgelost
>* De catalogus bijwerken wanneer elke tenant-database wordt een failover voor het bijhouden van de primaire locatie van elke tenant-database
>* Zorg ervoor dat de toepassing en primaire tenant-database zijn altijd die ditzelfde zijn geplaatst in dezelfde Azure-regio voor de latentie te verminderen  
 

Voordat u deze zelfstudie begint, moet dat de volgende vereisten zijn voltooid:
* De database Wingtip Tickets SaaS per tenant-app wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en de database Wingtip Tickets SaaS per tenant toepassing verkennen](saas-dbpertenant-get-started-deploy.md)  
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.

## <a name="introduction-to-the-geo-replication-recovery-pattern"></a>Inleiding tot het patroon van de recovery geo-replicatie

![Recovery-architectuur](media/saas-dbpertenant-dr-geo-replication/recovery-architecture.png)
 
Herstel na noodgeval (DR) is een belangrijk aandachtspunt voor veel toepassingen, of om wettelijke redenen of zakelijke continuïteit. Moet er een langdurige servicestoring, kan een goed voorbereid DR-plan onderbreking van de zakelijke minimaliseren. Met behulp van geo-replicatie biedt de laagste RPO en RTO door het onderhouden van databasereplica's in een regio herstel die failover mogelijk op korte opvalt.

Op basis van geo-replicatie een DR-plan bestaat uit drie onderdelen:
* Configuratie-maken en het onderhoud van de Herstelomgeving
* Herstel - failover van de app en de databases in de omgeving herstellen als er een storing optreedt,
* Repatriëring - failover van de app en de databases terug naar de oorspronkelijke regio als de toepassing is opgelost 

Alle onderdelen moeten worden beschouwd zorgvuldig, met name als besturingssysteem op grote schaal. Het plan moet over het algemeen verschillende doelstellingen te bereiken:

* Instellen
    * Tot stand brengen en onderhouden van een gespiegelde-omgeving in de regio van het herstel. Maken van de elastische pools en repliceren geen zelfstandige databases in deze Herstelomgeving reserveert capaciteit in de regio van het herstel. Onderhoud van deze omgeving omvat het repliceren van de nieuwe tenant databases zoals deze zijn ingericht.  
* Herstel
    * Wanneer een nagemaakt Herstelomgeving wordt gebruikt om de dagelijkse kosten kunt minimaliseren, moeten pools en zelfstandige databases worden uitgebreid naar volledige operationele capaciteit in de regio herstel verkrijgen
    * Nieuwe tenant in de regio herstel zo snel mogelijk inrichting inschakelen  
    * Geoptimaliseerd voor het herstellen van tenants in volgorde van prioriteit
    * Geoptimaliseerd voor het ophalen van tenants online zo snel mogelijk door te voeren stappen parallel waar praktische met
    * Netwerkfouten mislukt, opnieuw starten, en de idempotent
    * Het proces in halverwege vlucht annuleren als de oorspronkelijke regio weer on line kunnen worden.
* Repatriëring 
    * Failover van databases van de regio herstel met replica's in de oorspronkelijke regio met minimale gevolgen voor tenants: geen verlies van gegevens en de minimale periode offline per tenant.   

In deze zelfstudie worden deze uitdagingen aangepakt gebruikt functies van Azure SQL Database en de Azure-platform:

* [Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template), om zo snel mogelijk alle benodigde capaciteit reserveren. Azure Resource Manager-sjablonen worden gebruikt voor het inrichten van een spiegelbeeld van de productieservers en elastische pools in de regio van het herstel.
* [Geo-replicatie](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview), asynchroon-gerepliceerde alleen-lezen secundaire replica's voor alle databases maken. Tijdens een storing schakelt u over naar de replica's in de regio van het herstel.  Nadat de onderbreking opgelost is, schakelt u terug naar de databases in de oorspronkelijke regio zonder verlies van gegevens.
* [Asynchrone](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations) failover-bewerkingen in volgorde van prioriteit tenant minimaliseren failover voor een groot aantal databases verzonden.
* [Beheerfuncties voor herstel shard](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-recovery-manager), items in de database in de catalogus tijdens herstel en repatriëring wijzigen. Deze functies kunnen de app kan verbinding maken met tenant-databases, ongeacht de locatie zonder de app opnieuw te configureren.
* [SQL server-DNS-aliassen](https://docs.microsoft.com/azure/sql-database/dns-alias-overview), zodat naadloos inrichten van nieuwe tenants ongeacht welke regio in de app wordt uitgevoerd. DNS-aliassen worden ook gebruikt voor het toestaan van het synchronisatieproces catalogus verbinding maken met de catalogus van active ongeacht de locatie.

## <a name="get-the-disaster-recovery-scripts"></a>De disaster recovery scripts ophalen 

> [!IMPORTANT]
> Net als alle Wingtip Tickets management scripts, de DR-scripts zijn voorbeeld kwaliteit en niet moet worden gebruikt in productie. 

Het herstel scripts die worden gebruikt in deze zelfstudie en de broncode van Wingtip van toepassing zijn beschikbaar in de [Wingtip Tickets SaaS-database per tenant GitHub-opslagplaats](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant/). Bekijk de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de Wingtip Tickets management scripts deblokkeren.

## <a name="tutorial-overview"></a>Overzicht van de zelfstudie
In deze zelfstudie maakt u eerst gebruiken geo-replicatie voor het maken van replica's van de toepassing Wingtip Tickets en de databases in een andere regio. Vervolgens failover u voor deze regio voor het herstellen van een storing simuleren. Als u klaar is, is de toepassing is volledig functioneel in de regio van het herstel.

Later in een stap afzonderlijke repatriëring failover u van de catalogus en tenant-databases in de regio herstel naar de oorspronkelijke regio. De toepassing en de databases blijven beschikbaar in de hele repatriëring. Als u klaar is, is de toepassing is volledig functioneel in de oorspronkelijke regio.

> [!Note]
> De toepassing kunnen worden hersteld in de _gekoppelde regio_ van de regio waarin de toepassing wordt geïmplementeerd. Zie voor meer informatie [Azure regio's gekoppeld](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions).

## <a name="review-the-healthy-state-of-the-application"></a>Controleer de status van de toepassing in orde

Voordat u het herstelproces start, controleert u de normale status in orde van de toepassing.
1. Open in uw webbrowser de Hub Wingtip Tickets gebeurtenissen (http://events.wingtip-dpt.&lt; gebruiker&gt;. trafficmanager.net - vervangen &lt;gebruiker&gt; met uw implementatie gebruiker waarde).
    * Ga naar de onderkant van de pagina en Let op de naam van server-catalogus en de locatie in de voettekst. De locatie is de regio waarin u de app hebt geïmplementeerd.
    *TIP: Beweeg de muisaanwijzer over de locatie voor het vergroten van de weergave. * 
     ![Gebeurtenissen hub status in orde in oorspronkelijke regio](media/saas-dbpertenant-dr-geo-replication/events-hub-original-region.png)

2. Klik op de tenant Contoso overleg Hall en open de pagina van de gebeurtenis.
    * Let op de naam van de tenant in de voettekst. De locatie is hetzelfde als de locatie van de catalogusserver.

3. In de [Azure-portal](https://portal.azure.com), open de resourcegroep waarin de app wordt geïmplementeerd
    * U ziet de regio waarin de servers zijn geïmplementeerd. 

## <a name="sync-tenant-configuration-into-catalog"></a>Configuratie van de synchronisatie-tenant in catalogus

In deze taak start u een proces dat wordt gesynchroniseerd van de configuratie van de servers, elastische pools en databases die in de tenant-catalogus. Het proces houdt deze informatie bijgewerkt in de catalogus.  Het proces werkt met de actieve catalogus in de oorspronkelijke regio of in de regio van het herstel. De configuratie-informatie wordt gebruikt als onderdeel van het herstelproces om te controleren of de Herstelomgeving consistent met de originele omgeving is en vervolgens later tijdens repatriëring om te controleren of de oorspronkelijke regio consistent is gemaakt met eventuele wijzigingen de Herstelomgeving. De catalogus wordt ook gebruikt voor het bijhouden van de herstelstatus van tenantbronnen

> [!IMPORTANT]
> Ter vereenvoudiging zijn van het synchronisatieproces en andere langdurige herstel en repatriëring processen geïmplementeerd in deze zelfstudies als lokale Powershell-taken of sessies die worden uitgevoerd onder uw client-gebruikersaanmelding. De verificatietokens uitgegeven als u aanmelding na enkele uren verloopt en vervolgens de taken mislukken. In een productiescenario moeten langlopende processen worden geïmplementeerd als betrouwbare Azure-services type, uitgevoerd onder een service-principal. Zie [gebruik Azure PowerShell een service-principal maken met een certificaat](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal).

1. In de _PowerShell ISE_, opent u het ...\Learning Modules\UserConfig.psm1-bestand. Vervang `<resourcegroup>` en `<user>` op regels 10 en 11 met de waarde die wordt gebruikt wanneer u de app hebt geïmplementeerd.  Sla het bestand!

2. In de *PowerShell ISE*en opent u de ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 script instellen:
    * **$DemoScenario = 1**, een achtergrondtaak die wordt gesynchroniseerd tenant server starten en informatie over de configuratie van toepassingen in de catalogus

3. Druk op **F5** de synchronisatie-script uit te voeren. Een nieuwe PowerShell-sessie wordt geopend om te synchroniseren van de configuratie van tenantbronnen.
![Synchronisatieproces](media/saas-dbpertenant-dr-geo-replication/sync-process.png)

Het PowerShell-venster op de achtergrond uitgevoerd en blijft met de rest van de zelfstudie. 

> [!Note]
> Het synchronisatieproces verbindt met de catalogus via een DNS-alias. Deze alias is gewijzigd tijdens terugzetten en repatriëring om te verwijzen naar de actieve catalogus. Het synchronisatieproces houdt de catalogus up-to-date met de database of een groep configuratiewijzigingen aangebracht in de regio van het herstel.  Tijdens repatriëring, worden deze wijzigingen worden toegepast op de gelijkwaardige resources in de oorspronkelijke regio.

## <a name="create-secondary-database-replicas-in-the-recovery-region"></a>Maken van secundaire Databasereplica's in de regio herstel

In deze taak door een proces die een exemplaar van dubbele app implementeert en repliceert de catalogus en alle tenant databases naar een regio herstel te starten.

> [!Note]
> Deze zelfstudie voegt beveiliging op geo-replicatie met de voorbeeldtoepassing Wingtip Tickets toe. In een productiescenario voor een toepassing die gebruikmaakt van geo-replicatie zou elke tenant worden ingericht met een database geogerepliceerde vanaf het begin. Zie [ontwerpen van maximaal beschikbare services met behulp van Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery#scenario-1-using-two-azure-regions-for-business-continuity-with-minimal-downtime)

1. In de *PowerShell ISE*, opent u de ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 script en stel de volgende waarden:
    * **$DemoScenario = 2**, maakt u spiegelbeeld Herstelomgeving en repliceren van catalogus en tenant-databases

2. Druk op **F5** om het script uit te voeren. Een nieuwe PowerShell-sessie is geopend voor het maken van de replica's.
![Synchronisatieproces](media/saas-dbpertenant-dr-geo-replication/replication-process.png)  

## <a name="review-the-normal-application-state"></a>Controleer de status van de normale toepassing
De toepassing is op dit moment wordt normaal uitgevoerd in de oorspronkelijke regio en nu is beveiligd door geo-replicatie.  Alleen-lezen van de secundaire replica aanwezig zijn in de regio van het herstel voor alle databases. 
1. Bekijk in de Azure-portal uw resourcegroepen en Let op: een resourcegroep is gemaakt met - achtervoegsel in de regio recovery herstel. 

1. Gebruik de resources in de resourcegroep voor herstel.  

1. Klik op de database Contoso overleg Hall in de _tenants1-dpt -&lt;gebruiker&gt;-herstel_ server.  Klik op de Geo-replicatie aan de linkerkant. 

    ![Contoso overleg geo-replicatiekoppeling](media/saas-dbpertenant-dr-geo-replication/contoso-geo-replication.png) 

Let op de koppeling geo-replicatie tussen de primaire server in de oorspronkelijke regio en de secundaire in de regio van het herstel in de kaart Azure-regio's.  

## <a name="fail-over-the-application-into-the-recovery-region"></a>Failover van de toepassing in de regio herstel

### <a name="geo-replication-recovery-process-overview"></a>Overzicht van het herstel geo-replicatie

Het herstel script voert de volgende taken:

1. Hiermee schakelt u het Traffic Manager-eindpunt voor de web-app in de oorspronkelijke regio. Het uitschakelen van het eindpunt voorkomen dat gebruikers verbinding maken met de app in een ongeldige status moet de oorspronkelijke regio on line tijdens het herstel.

1. Maakt gebruik van een geforceerde failover van de catalogusdatabase in de regio herstel zodat deze de primaire database en werkt de _activecatalog_ alias om te verwijzen naar de server van de catalogus.

1. Updates de _newtenant_ alias om te verwijzen naar de tenant-server in de regio van het herstel. Het wijzigen van deze alias, zorgt u ervoor dat de databases voor een nieuwe tenants in de regio voor herstel zijn ingericht. 

1. Alle bestaande tenants in de catalogus herstel als offline toegang tot wilt voorkomen tenant databases voordat ze failover markeert.

1. De configuratie van alle elastische pools en gerepliceerde zelfstandige databases in de regio van het herstel voor het spiegelen van de configuratie in de oorspronkelijke regio-updates. (Deze taak is alleen nodig als groepen of gerepliceerde databases in de Herstelomgeving verkleind tijdens normale bewerkingen om kosten te verlagen).

1. Hiermee schakelt u het Traffic Manager-eindpunt voor de web-app in de regio van het herstel. Inschakelen van dit eindpunt, kunt de toepassing voor het inrichten van nieuwe tenants. In deze fase worden de bestaande tenants nog steeds offline.

1. Batches van aanvragen voor force failover-databases in volgorde van prioriteit worden verzonden.
    * Batches zijn ingedeeld zodat databases wordt een failover uitgevoerd parallel tussen alle pools.
    * Failover-aanvragen worden ingediend met asynchrone bewerkingen, zodat ze snel worden verzonden en meerdere aanvragen kunnen gelijktijdig worden verwerkt.

   > [!Note]
   > In een scenario onderbreking in zijn de primaire databases in de oorspronkelijke regio offline.  Force Failover-overschakeling op de secundaire-einden de verbinding met de primaire zonder poging tot toepassen van transacties die zijn achtergebleven in de wachtrij. In een scenario DR inzoomen zoals deze zelfstudie als er een update-activiteit op het moment van failover kan er een aantal gegevens verloren gaan. Later tijdens repatriëring, wordt wanneer u een failover-databases in de regio herstel terug naar de oorspronkelijke regio een normale failover gebruikt om te controleren of er geen gegevens verloren gaan.

1. Bewaakt de SQL database-service om te bepalen wanneer databases via is mislukt. Zodra de failover van een tenant-database wordt uitgevoerd, wordt bijgewerkt in de catalogus voor het vastleggen van de herstelstatus van de tenant-database en de tenant als on line markeren.
    * Tenant-databases zijn toegankelijk door de toepassing zodra ze zijn gemarkeerd in de catalogus online.
    * De som van waarden in de database van de tenant rowversion wordt opgeslagen in de catalogus. Deze waarde fungeert als een vingerafdruk waarmee het proces repatriëring om te bepalen of de database is bijgewerkt in de regio van het herstel.

### <a name="run-the-script-to-fail-over-to-the-recovery-region"></a>Voer het script failover naar de regio herstel

Nu voorstellen dat er een storing in het gebied waarin de toepassing is geïmplementeerd en voer het script voor herstel is:

1. In de *PowerShell ISE*, opent u de ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 script en stel de volgende waarden:
    * **$DemoScenario = 3**, de app in een regio herstelpunten door replica's worden niet via herstellen

2. Druk op **F5** om het script uit te voeren.  
    * Het script in een nieuw PowerShell-venster wordt geopend en start vervolgens een reeks PowerShell-taken die parallel worden uitgevoerd. Deze taken Failover-overschakeling tenant databases voor de regio van het herstel.
    * De regio van het herstel is de _gekoppelde regio_ die zijn gekoppeld aan de Azure-regio waarin u de toepassing is geïmplementeerd. Zie voor meer informatie [Azure regio's gekoppeld](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions). 

3. De status van het herstelproces in de PowerShell-venster bewaken.
    ![Failover-proces](media/saas-dbpertenant-dr-geo-replication/failover-process.png)

> [!Note]
> Bekijk de PowerShell-scripts in de map van Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-FailoverToReplica\RecoveryJobs ...\Learning de code voor de hersteltaken om's te bekijken.

### <a name="review-the-application-state-during-recovery"></a>Controleer de status van de toepassing tijdens het herstel
Het toepassingseindpunt is uitgeschakeld in Traffic Manager, is de toepassing niet beschikbaar. Nadat de catalogus wordt een failover voor de regio van herstel en alle huurders offline gemarkeerd, kan de toepassing weer online is gebracht. Hoewel de toepassing beschikbaar is, verschijnt elke tenant in de hub gebeurtenissen offline totdat failover van de database wordt uitgevoerd. Het is belangrijk voor het ontwerpen van uw toepassing om af te handelen offline tenant-databases.

1. Onmiddellijk nadat de catalogusdatabase is hersteld, vernieuw de Hub Wingtip Tickets gebeurtenissen in uw webbrowser.
    * In de voettekst u ziet dat de naam van de catalogus nu een _-herstel_ achtervoegsel en bevindt zich in de regio van het herstel.
    * U ziet dat tenants die nog niet zijn hersteld, zijn gemarkeerd als offline en niet kunnen worden geselecteerd.  

    > [!Note]
    > Met slechts enkele databases te herstellen, kunnen mogelijk niet vernieuwen van de browser voordat het herstel is voltooid, zodat u de tenants niet zien kan terwijl ze offline zijn. 
 
    ![Gebeurtenissen hub offline](media/saas-dbpertenant-dr-geo-replication/events-hub-offlinemode.png) 

    * Als u een offline tenant gebeurtenissen pagina rechtstreeks opent, wordt een melding offline tenant. Bijvoorbeeld, als Contoso overleg Hall offline is, probeert te openen http://events.wingtip-dpt.&lt; gebruiker&gt;.trafficmanager.net/contosoconcerthall ![pagina Offline Contoso](media/saas-dbpertenant-dr-geo-replication/dr-in-progress-offline-contosoconcerthall.png) 

### <a name="provision-a-new-tenant-in-the-recovery-region"></a>Een nieuwe tenant in de regio herstel inrichten
Zelfs voordat failover uitgevoerd voor de bestaande tenant databases, kunt u nieuwe tenants in de regio herstel inrichten.  

1. In de *PowerShell ISE*, opent u de ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 script en stelt u de volgende eigenschap:
    * **$DemoScenario = 4**, een nieuwe tenant in de regio herstel inrichten

2. Druk op **F5** voor het uitvoeren van het script en het inrichten van de nieuwe tenant. 

3. De pagina Hawthorn Hall gebeurtenissen wordt geopend in de browser wanneer deze is voltooid. Opmerking van de voettekst dat de database Hawthorn Hall wordt geleverd in de regio van het herstel.
    ![Pagina hawthorn Hall-gebeurtenissen](media/saas-dbpertenant-dr-geo-replication/hawthornhallevents.png) 

4. Vernieuw de Wingtip Tickets gebeurtenissen Hub-pagina om te bekijken dat hawthorn Hall opgenomen in de browser. 
    * Als u Hawthorn Hall ingericht zonder te wachten op voor de andere tenants te herstellen, worden andere tenants nog steeds offline.


## <a name="review-the-recovered-state-of-the-application"></a>Controleer de herstelde status van de toepassing

Wanneer het herstelproces is voltooid, zijn de toepassing en alle tenants volledig functioneel in de regio van het herstel. 

1. Zodra de weergave in het venster PowerShell-console geeft aan dat de tenants worden hersteld, vernieuwt u de Hub gebeurtenissen.  De tenants verschijnt alle online, inclusief de nieuwe tenant, Hawthorn Hall.

    ![herstelde en nieuwe tenants in de hub gebeurtenissen](media/saas-dbpertenant-dr-geo-replication/events-hub-with-hawthorn-hall.png)

2. In de [Azure-portal](https://portal.azure.com), opent u de lijst met resourcegroepen.  
    * U ziet de resourcegroep die u hebt geïmplementeerd, plus de resourcegroep herstel met de _-herstel_ achtervoegsel.  De resourcegroep herstel bevat alle resources die zijn gemaakt tijdens het herstelproces plus nieuwe resources die zijn gemaakt tijdens de onderbreking.  

3. Open de resourcegroep van herstel en Let op de volgende items:
    * De versies van het herstel van de servers catalogus en tenants1 met _-herstel_ achtervoegsel.  De herstelde catalogus en tenant-databases op deze alle servers hebben de namen in de oorspronkelijke regio gebruikt.

    * De _tenants2-dpt -&lt;gebruiker&gt;-herstel_ SQL server.  Deze server wordt gebruikt voor het inrichten van nieuwe tenants tijdens de onderbreking.
    *   De App Service met de naam, _gebeurtenissen-wingtip-dpt -&lt;recoveryregion&gt;-&lt;gebruiker & gt_; dit is het exemplaar van het herstel van de app gebeurtenissen. 

    ![Azure recovery resources ](media/saas-dbpertenant-dr-geo-replication/resources-in-recovery-region.png)    
    
4. Open de _tenants2-dpt -&lt;gebruiker&gt;-herstel_ SQL server.  U ziet deze de database bevat _hawthornhall_ en de elastische groep _Pool1_.  De _hawthornhall_ database geconfigureerd als een elastische database in _Pool1_ elastische pool.

5. Ga terug naar de resourcegroep en klik op de database Contoso overleg Hall op de _tenants1-dpt -&lt;gebruiker&gt;-herstel_ server. Klik op de Geo-replicatie aan de linkerkant.
    
    ![Contoso-database na een failover](media/saas-dbpertenant-dr-geo-replication/contoso-geo-replication-after-failover.png)

## <a name="change-tenant-data"></a>Gegevens over tenant 
In deze taak bijwerken u een van de tenant-databases. 

1. De lijst met gebeurtenissen vinden voor de Contoso overleg Hall en noteer de naam van de laatste gebeurtenis in uw browser.
2. In de *PowerShell ISE*, stelt u de volgende waarde in de ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 script:
    * **$DemoScenario = 5** een gebeurtenis verwijderen uit een tenant in de regio herstel
3. Druk op **F5** het script uitvoeren
4. Vernieuw de pagina van de gebeurtenissen Contoso overleg Hall (http://events.wingtip-dpt.&lt; gebruiker&gt;.trafficmanager.net/contosoconcerthall - Vervang &lt;gebruiker&gt; met de waarde van de gebruiker van uw implementatie) en u ziet dat de laatste gebeurtenis is verwijderd.

## <a name="repatriate-the-application-to-its-original-production-region"></a>De toepassing naar de oorspronkelijke productieregio repatriëren

Deze taak repatriates de toepassing naar de oorspronkelijke regio. In een echte scenario zou u repatriëring starten wanneer de onderbreking opgelost is.

### <a name="repatriation-process-overview"></a>Overzicht van het proces repatriëring

![Repatriëring-architectuur](media/saas-dbpertenant-dr-geo-replication/repatriation-architecture.png)

Het proces repatriëring:
1. Annuleert de verzoeken restore database openstaande of onderweg.
2. Updates de _newtenant_ alias om te verwijzen naar de server de tenants in de regio van de oorsprong. Het wijzigen van deze alias, zorgt u ervoor dat de databases voor een nieuwe tenants nu in de regio van de oorsprong ingericht wordt.
3. De seeding van gewijzigde tenant gegevens naar de oorspronkelijke regio
4. Tenant-databases in volgorde van prioriteit failover.

Failover wordt de database effectief verplaatst naar de oorspronkelijke regio. Als de database failover wordt uitgevoerd, eventuele geopende verbindingen worden verwijderd en de database is niet beschikbaar voor een paar seconden. Toepassingen worden geschreven met Pogingslogica om te controleren of dat ze opnieuw verbinding te maken.  Hoewel deze korte disconnect vaak niet opgemerkt is, kunt u repatriëren databases buiten kantooruren. 


### <a name="run-the-repatriation-script"></a>Voer het script repatriëring
Nu gaan we Denk aan de onderbreking is opgelost en voer het script repatriëring.

1. In de *PowerShell ISE*naar het ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 script.

2. Controleer of het proces van synchronisatie catalogus nog wordt uitgevoerd in de PowerShell-sessie.  Indien nodig, deze opnieuw starten door in te stellen:
    * **$DemoScenario = 1**, begin tenant-server, toepassingen en informatie over de configuratie van de database in de catalogus synchroniseren
    * Druk op **F5** om het script uit te voeren.

3.  Stel vervolgens de volgende opdracht voor het starten van het proces repatriëring:
    * **$DemoScenario = 6**, de app in de oorspronkelijke regio repatriëren
    * Druk op **F5** het script herstel uitvoeren in een nieuw PowerShell-venster.  Repatriëring duurt enkele minuten en kan worden bewaakt in het PowerShell-venster.
    ![Repatriëring proces](media/saas-dbpertenant-dr-geo-replication/repatriation-process.png)

4. Terwijl het script wordt uitgevoerd, vernieuw de pagina gebeurtenissen Hub (http://events.wingtip-dpt.&lt; gebruiker&gt;. trafficmanager.net)
    * U ziet dat alle huurders online is en toegankelijk tijdens dit proces.

5. Nadat de repatriëring voltooid is, vernieuwt u de gebeurtenissen hub en open de pagina gebeurtenissen voor Hawthorn Hall. U ziet dat deze database is is besluit zijn verricht naar de oorspronkelijke regio.
    ![Gebeurtenissen hub besluit zijn verricht](media/saas-dbpertenant-dr-geo-replication/events-hub-repatriated.png)


## <a name="designing-the-application-to-ensure-app-and-database-are-colocated"></a>Het ontwerpen van de toepassing om te controleren of de app en database zijn geplaatst 
De toepassing is zodanig ontworpen dat altijd verbinding wordt gemaakt van een exemplaar in dezelfde regio bevinden als de tenant-database. Dit ontwerp vermindert de latentie tussen de toepassing en de database. Deze optimalisatie wordt ervan uitgegaan dat de app naar database interactie chattier dan de interactie app gebruiker.  

Tenant-databases kunnen worden verdeeld over herstel en de oorspronkelijke regio's voor enige tijd tijdens repatriëring. De app zoekt de regio waarin de database bevindt zich een DNS-zoekopdracht op de naam van de tenant als volgt voor elke database. In SQL-Database is de naam van de server een alias. De naam van de alias bevat de regionaam. Als de toepassing niet in dezelfde regio bevinden als de database, wordt hij omgeleid naar het exemplaar in dezelfde regio bevinden als de databaseserver.  Omleiden naar een exemplaar in dezelfde regio bevinden als de database minimaliseert latentie tussen de app en database. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:
> [!div classs="checklist"]

>* Synchronisatiedatabase en configuratiegegevens over elastische pool in de tenant-catalogus
>* Een Herstelomgeving in een andere regio, die bestaat uit de toepassing, servers en toepassingen instellen
>* Gebruik _geo-replicatie_ voor het repliceren van de catalogus en tenant-databases naar de regio herstel
>* Failover van de toepassing en de catalogus en tenant-databases voor de regio herstel 
>* Een failback uit op de toepassing, catalogus en tenant-databases naar de oorspronkelijke regio nadat de onderbreking opgelost is

U kunt meer informatie over de technologieën Azure SQL database biedt om in te schakelen van zakelijke continuïteit in de [Business Continuity Overview](sql-database-business-continuity.md) documentatie.

## <a name="additional-resources"></a>Aanvullende resources

* [Aanvullende zelfstudies waarin voort op de Wingtip SaaS-toepassing bouwen](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-wtp-overview#sql-database-wingtip-saas-tutorials)
