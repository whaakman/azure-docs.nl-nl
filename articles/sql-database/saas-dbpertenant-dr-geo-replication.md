---
title: Herstel na noodgevallen voor SaaS-apps met behulp van Geo-replicatie van Azure SQL Database | Microsoft Docs
description: Meer informatie over het gebruik van geo-replica's van de Azure SQL Database om te herstellen van een multitenant SaaS-app na een storing
keywords: zelfstudie sql-database
services: sql-database
author: AyoOlubeko
manager: craigg
ms.service: sql-database
ms.custom: saas apps
ms.topic: conceptual
ms.date: 04/09/2018
ms.author: ayolubek
ms.openlocfilehash: 1af13857bdde06b70cb8d01db8d9668cc6ce99e6
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45983964"
---
# <a name="disaster-recovery-for-a-multi-tenant-saas-application-using-database-geo-replication"></a>Herstel na noodgevallen voor een multitenant SaaS-toepassing met behulp van de database geo-replicatie

In deze zelfstudie maakt kennis u met een volledige noodherstelscenario voor een multitenant SaaS-toepassing geïmplementeerd met behulp van de database-per-tenant-model. Ter bescherming van de app na een storing, gebruikt u [ _geo-replicatie_ ](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview) te maken van replica's voor de catalogus en tenant-databases in een andere regio. Als er een storing optreedt, schakelt snel u over naar deze replica's naar het normale zakelijke activiteiten te hervatten. Bij failover worden de databases in de oorspronkelijke regio secundaire replica's van de databases in de herstelregio voor. Zodra deze replica's weer online komt achterstand ze automatisch naar de status van de databases in de herstelregio voor. Nadat de onderbreking opgelost is, schakelt u terug naar de databases in de oorspronkelijke productieregio.

Deze zelfstudie worden de failover en failback-werkstromen. U leert het volgende:
> [!div classs="checklist"]

>* Databases en elastische groep configuratie-informatie in de tenantcatalogus synchroniseren
>* Instellen van een omgeving voor herstel in een andere regio op, die bestaat uit de toepassing, servers en toepassingen
>* Gebruik _geo-replicatie_ voor het repliceren van de catalogus en tenant-databases naar de recovery-regio
>* Failover van de toepassing en de catalogus en tenant-databases naar de recovery-regio 
>* Later, failover van de toepassing, -catalogus en tenant-databases terug naar de oorspronkelijke regio nadat de onderbreking is verholpen
>* Catalogus bijwerken als elke tenantdatabase failover wordt uitgevoerd voor het bijhouden van de primaire locatie van de database van elke tenant
>* Zorg ervoor dat de toepassing en de primaire tenant-database altijd in dezelfde Azure-regio te verminderen latentie zijn geplaatst  
 

Voordat u deze zelfstudie begint, moet dat de volgende vereisten zijn voltooid:
* De Wingtip Tickets SaaS-database per tenant-app is geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en verkennen van de Wingtip Tickets SaaS-database per tenant-toepassing](saas-dbpertenant-get-started-deploy.md)  
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.

## <a name="introduction-to-the-geo-replication-recovery-pattern"></a>Inleiding tot de herstelpatroon geo-replicatie

![Recovery-architectuur](media/saas-dbpertenant-dr-geo-replication/recovery-architecture.png)
 
Herstel na noodgeval (DR) is een belangrijk aandachtspunt voor veel toepassingen, of om wettelijke redenen of zakelijke continuïteit. Moet er een langdurige serviceonderbreking, kunt een goed is voorbereid plan voor herstel na Noodgevallen bedrijfsonderbreking minimaliseren. Met behulp van geo-replicatie biedt de laagste RPO en RTO dankzij de Databasereplica's in een regio die failover op korte merken.

Een DR-plan op basis van geo-replicatie bestaat uit drie onderdelen:
* Configuratie-maken en onderhouden van de omgeving voor herstel
* Herstel - failover van de app en -databases in de omgeving herstellen als er een storing optreedt,
* Repatriëring - failover van de app en de databases terug naar de oorspronkelijke regio zodra de toepassing is opgelost 

Alle onderdelen moeten worden afgewogen, met name als besturingssysteem op schaal. Het plan moet over het algemeen verschillende doelstellingen te bereiken:

* Instellen
    * Tot stand brengen en onderhouden van een mirror-image-omgeving in de herstelregio voor. Het maken van de elastische pools en repliceren van een individuele databases in deze omgeving recovery capaciteit in de herstelregio gereserveerd. Onderhoud van deze omgeving omvat het repliceren van de nieuwe tenant-databases zoals deze zijn ingericht.  
* Herstel
    * Wanneer een gereduceerde Herstelomgeving wordt gebruikt om dagelijkse kosten te minimaliseren, moeten pools en individuele databases worden opgeschaald naar volledige operationele capaciteit in de herstelregio verkrijgen
    * Nieuwe tenants worden ingericht in de herstelregio zo snel mogelijk inschakelen  
    * Geoptimaliseerd voor het herstellen van tenants in volgorde van prioriteit
    * Geoptimaliseerd voor het ophalen van tenants online zo snel mogelijk door te voeren stappen parallel waar praktische met
    * Bestand is tegen storingen, opnieuw starten, en idempotent zijn
    * Mogelijk zijn om te annuleren van het proces in halverwege vlucht als de oorspronkelijke regio weer online komt.
* Repatriëring 
    * Databases van de herstelregio voor op replica's in de oorspronkelijke regio met minimale gevolgen voor tenants failover: Er zijn geen gegevens verloren gaan en de minimale periode offline per tenant.   

In deze zelfstudie, worden deze uitdagingen opgelost met behulp van functies van Azure SQL Database en het Azure-platform:

* [Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template), zo snel mogelijk alle benodigde capaciteit reserveren. Azure Resource Manager-sjablonen worden gebruikt voor het inrichten van een mirror-installatiekopie van de productieservers en elastische pools in de herstelregio voor.
* [Geo-replicatie](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview), asynchroon gerepliceerde alleen-lezen-secundaire databases voor alle databases maken. Tijdens een storing schakelt u over naar de replica's in de herstelregio voor.  Nadat de onderbreking opgelost is, schakelt u terug naar de databases in de oorspronkelijke regio met zonder verlies van gegevens.
* [Asynchrone](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations) failover-bewerkingen in volgorde van de tenant-prioriteit, failover paraatheid voor grote aantallen databases worden verzonden.
* [Functies van shard management gegevensherstel](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-recovery-manager)items in de database in de catalogus worden gewijzigd tijdens het herstel en repatriëring. Deze functies kunnen verbinding maken met de tenant-databases, ongeacht de locatie zonder de app opnieuw met de app.
* [SQL server-DNS-aliassen](https://docs.microsoft.com/azure/sql-database/dns-alias-overview), om in te schakelen naadloze levering van nieuwe tenants, ongeacht welke regio in de app wordt uitgevoerd. DNS-aliassen worden ook gebruikt om toe te staan van het proces voor het synchroniseren van catalogus verbinding maken met de actieve catalogus, ongeacht de locatie.

## <a name="get-the-disaster-recovery-scripts"></a>Scripts voor herstel van het noodgeval ophalen 

> [!IMPORTANT]
> Net als alle Wingtip Tickets management scripts, de DR-scripts zijn voorbeeld kwaliteit en niet moet worden gebruikt in de productieomgeving. 

Het herstel scripts die worden gebruikt in deze zelfstudie en de broncode van de Wingtip-toepassing zijn beschikbaar in de [Wingtip Tickets SaaS-database per tenant GitHub-opslagplaats](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant/). Bekijk de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de blokkering opheffen van de Wingtip Tickets-management-scripts.

## <a name="tutorial-overview"></a>Overzicht van de zelfstudie
In deze zelfstudie gebruikt u eerst geo-replicatie te maken van replica's van de toepassing Wingtip Tickets en de bijbehorende databases in een andere regio. Vervolgens kunt failover u uitvoeren naar deze regio om te herstellen na een storing simuleren. Als u klaar bent, is de toepassing is volledig functioneel in de herstelregio voor.

Later, in een afzonderlijke repatriëring stap maakt u een failover de catalogus en tenant-databases in de herstelregio naar de oorspronkelijke regio. De toepassing en databases blijven beschikbaar in de hele repatriëring. Als u klaar bent, is de toepassing is volledig functioneel in de oorspronkelijke regio.

> [!Note]
> De toepassing wordt hersteld naar de _gekoppelde regio_ van de regio waarin de toepassing is geïmplementeerd. Zie voor meer informatie, [gekoppelde Azure-regio's](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

## <a name="review-the-healthy-state-of-the-application"></a>Controleer de status in orde van de toepassing

Voordat u het herstelproces start, controleert u de normale, gezonde status van de toepassing.
1. Open in uw webbrowser de Wingtip Tickets Events Hub (http://events.wingtip-dpt.&lt; gebruiker&gt;. trafficmanager.net - vervangen &lt;gebruiker&gt; met de waarde van de gebruiker van uw implementatie).
    * Ga naar de onderkant van de pagina en ziet u de naam van de catalogus-server en de locatie in de voettekst. De locatie is de regio waarin u de app hebt geïmplementeerd.
    *TIP: Beweeg de muisaanwijzer over de locatie voor het vergroten van de weergave. * 
     ![Gebeurtenissen hub in orde in oorspronkelijke regio](media/saas-dbpertenant-dr-geo-replication/events-hub-original-region.png)

2. Klik op de tenant Contoso Concert Hall en open de pagina van de gebeurtenis.
    * In de voettekst, ziet u de naam van de tenant. De locatie is hetzelfde als de locatie van de catalogusserver.

3. In de [Azure-portal](https://portal.azure.com), opent u de resourcegroep waarin de app is geïmplementeerd
    * U ziet de regio waarin de servers worden geïmplementeerd. 

## <a name="sync-tenant-configuration-into-catalog"></a>Tenant-synchronisatieconfiguratie in catalogus

In deze taak start u een proces dat worden gesynchroniseerd met de configuratie van de servers, elastische pools en databases in de tenantcatalogus. Het proces houdt deze informatie recente in de catalogus.  Het proces werkt met de actieve catalogus in de oorspronkelijke regio of in de herstelregio voor. De configuratie-informatie wordt gebruikt als onderdeel van het herstelproces om ervoor te zorgen betrouwbare omgeving voor het herstel consistent met de oorspronkelijke omgeving is en vervolgens later tijdens repatriëring om te controleren of de oorspronkelijke regio consistent is gemaakt met eventuele wijzigingen in de omgeving voor herstel. De catalogus wordt ook gebruikt voor het bijhouden van de herstelstatus van tenantbronnen

> [!IMPORTANT]
> Het synchronisatieproces en andere langlopende processen voor herstel en repatriëring voor het gemak zijn geïmplementeerd in deze zelfstudies als lokale Powershell-taken of sessies die worden uitgevoerd onder uw client-gebruikersaanmelding. De verificatietokens afgegeven wanneer u zich hebt aangemeld na enkele uren verloopt en de taken vervolgens mislukt. In een productiescenario voor, moeten de langlopende processen worden geïmplementeerd als betrouwbare Azure-services van een of andere, die wordt uitgevoerd onder een service-principal. Zie [gebruik Azure PowerShell een service-principal maken met een certificaat](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal).

1. In de _PowerShell ISE_, opent u het ...\Learning Modules\UserConfig.psm1-bestand. Vervang `<resourcegroup>` en `<user>` op regels 10 en 11 met de waarde die wordt gebruikt tijdens de implementatie van de app.  Sla het bestand.

2. In de *PowerShell ISE*en opent u de ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 script instellen:
    * **$DemoScenario = 1**, start u een achtergrondtaak die worden gesynchroniseerd met tenantserver en informatie over de configuratie van toepassingen in de catalogus

3. Druk op **F5** de synchronisatie-script uit te voeren. Een nieuwe PowerShell-sessie wordt geopend om te synchroniseren van de configuratie van tenantbronnen.
![Synchronisatieproces](media/saas-dbpertenant-dr-geo-replication/sync-process.png)

Laat u het PowerShell-venster op de achtergrond uitgevoerd en gaat u verder met de rest van de zelfstudie. 

> [!Note]
> Het synchronisatieproces verbindt met de catalogus via een DNS-alias. Deze alias is gewijzigd tijdens het terugzetten en repatriëring om te verwijzen naar de actieve catalogus. Het synchronisatieproces houdt de catalogus up-to-date met de database of pool configuratiewijzigingen aangebracht in de herstelregio voor.  Tijdens de repatriëring, worden deze wijzigingen worden toegepast op de gelijkwaardige resources in de oorspronkelijke regio.

## <a name="create-secondary-database-replicas-in-the-recovery-region"></a>Secundaire database-replica's maken in de herstelregio

In deze taak start u een proces dat een exemplaar van dubbele app implementeert en de catalogus en alle tenantdatabases naar een herstelregio voor repliceert.

> [!Note]
> In deze zelfstudie voegt geo-replicatie-beveiliging toe aan de voorbeeldtoepassing Wingtip Tickets. In een productiescenario voor een toepassing die gebruikmaakt van geo-replicatie, zou elke tenant worden ingericht met een database via geo-replicatie vanaf het begin. Zie [maximaal beschikbare services met behulp van Azure SQL-Database ontwerpen](https://docs.microsoft.com/azure/sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery#scenario-1-using-two-azure-regions-for-business-continuity-with-minimal-downtime)

1. In de *PowerShell ISE*, opent u de ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 script en stel de volgende waarden:
    * **$DemoScenario = 2**, maakt u spiegelbeeld Herstelomgeving en catalogus en tenant-databases repliceren

2. Druk op **F5** om het script uit te voeren. Hiermee opent u een nieuwe PowerShell-sessie voor het maken van de replica's.
![Synchronisatieproces](media/saas-dbpertenant-dr-geo-replication/replication-process.png)  

## <a name="review-the-normal-application-state"></a>Controleer de status van de normale toepassing
De toepassing is op dit moment normaal gesproken wordt uitgevoerd in de oorspronkelijke regio en nu wordt beveiligd door geo-replicatie.  Alleen-lezen secundaire replica's aanwezig zijn in de herstelregio voor alle databases. 
1. In de Azure-portal, bekijk uw resourcegroepen en merk op dat een resourcegroep is gemaakt met - recovery-achtervoegsel in de herstelregio voor. 

1. Verken de resources in de resourcegroep van recovery.  

1. Klik op de Contoso Concert Hall-database op de _tenants1-dpt -&lt;gebruiker&gt;-recovery_ server.  Klik op de Geo-replicatie aan de linkerkant. 

    ![Koppeling van Contoso Concert geo-replicatie](media/saas-dbpertenant-dr-geo-replication/contoso-geo-replication.png) 

Houd er rekening mee de koppeling voor geo-replicatie tussen de primaire in de oorspronkelijke regio en de secundaire server in de herstelregio in de map Azure-regio's.  

## <a name="fail-over-the-application-into-the-recovery-region"></a>Failover van de toepassing in de herstelregio

### <a name="geo-replication-recovery-process-overview"></a>Overzicht van recovery geo-replicatie

Het herstelscript worden de volgende taken uitgevoerd:

1. Hiermee schakelt u het Traffic Manager-eindpunt voor de web-app in de oorspronkelijke regio. Uitschakelen van het eindpunt wordt voorkomen dat gebruikers verbinding maken met de app in een ongeldige status moet de oorspronkelijke regio online is gekomen tijdens het herstel.

1. Wordt een geforceerde failover van de catalogusdatabase in de herstelregio gebruikt om de primaire database en werkt de _activecatalog_ alias om te verwijzen naar de herstelserver van de catalogus.

1. Updates de _newtenant_ alias om te verwijzen naar de tenantserver in de herstelregio voor. Deze alias te wijzigen, zorgt u ervoor dat de databases voor alle nieuwe tenants worden ingericht in de herstelregio voor. 

1. Markeert alle bestaande tenants in de catalogus herstellen als offline als u wilt toegang tot de tenant-databases voorkomen voordat ze failover is uitgevoerd.

1. Werkt u de configuratie van alle elastische pools en gerepliceerde individuele databases in de herstelregio voor het spiegelen van de configuratie in de oorspronkelijke regio. (Deze taak is alleen nodig als groepen of gerepliceerde databases in de omgeving voor herstel zijn verkleind tijdens normale bewerkingen om kosten te verlagen).

1. Hiermee kunt het Traffic Manager-eindpunt voor de web-app in de herstelregio voor. Dit eindpunt inschakelt, kan de toepassing voor het inrichten van nieuwe tenants. Bestaande tenants worden in deze fase nog steeds offline.

1. Batches van aanvragen om af te dwingen een failover-databases in de volgorde van prioriteit worden verzonden.
    * Batches worden zo geordend dat databases worden niet via parallel in alle pools.
    * Failover-aanvragen worden verzonden met behulp van asynchrone bewerkingen, zodat ze snel worden verzonden en meerdere aanvragen kunnen gelijktijdig worden verwerkt.

   > [!Note]
   > In een scenario onderbreking zijn de primaire databases in de oorspronkelijke regio offline.  Geforceerde failover wordt uitgevoerd in de secundaire-einden de verbinding met de primaire zonder probeert toe te passen resterende transacties die in de wachtrij. In een DR drill-scenario, zoals in deze zelfstudie, als er een update-activiteit op het moment van failover kunnen er gegevens verloren gaan. Later, tijdens repatriëring, is als u een failover-databases in de herstelregio terug naar de oorspronkelijke regio een normale failover gebruikt om ervoor te zorgen dat er geen gegevens verloren gaan.

1. Bewaakt de SQL database-service om te bepalen wanneer databases een failover zijn. Nadat de failover van een tenantdatabase wordt uitgevoerd, werkt het vastleggen van de herstelstatus van de tenant-database en markeren van de tenant als online-catalogus.
    * Tenant-databases kunnen worden geopend door de toepassing zodra ze online beschikbaar in de catalogus zijn gemarkeerd.
    * Som van rowversion waarden in de tenant-database is opgeslagen in de catalogus. Deze waarde fungeert als een vingerafdruk waarmee het proces repatriëring om te bepalen of de database is bijgewerkt in de herstelregio voor.

### <a name="run-the-script-to-fail-over-to-the-recovery-region"></a>Voer het script om te fungeren als failover voor de herstelregio

Nu is er een storing in de regio waarin de toepassing is geïmplementeerd en voer het herstelscript imagine:

1. In de *PowerShell ISE*, opent u de ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 script en stel de volgende waarden:
    * **$DemoScenario = 3**, de app in een herstelregio door failover wordt uitgevoerd naar de replica's herstellen

2. Druk op **F5** om het script uit te voeren.  
    * Het script wordt geopend in een nieuwe PowerShell-venster en start vervolgens een reeks PowerShell-taken die parallel worden uitgevoerd. Deze taken is failover van tenant-databases naar de herstelregio.
    * De herstelregio is de _gekoppelde regio_ die zijn gekoppeld aan de Azure-regio waarin u de toepassing is geïmplementeerd. Zie voor meer informatie, [gekoppelde Azure-regio's](https://docs.microsoft.com/azure/best-practices-availability-paired-regions). 

3. Controleer de status van het herstelproces in het PowerShell-venster.
    ![Failover-proces](media/saas-dbpertenant-dr-geo-replication/failover-process.png)

> [!Note]
> Als u wilt verkennen de code voor de recovery-taken, de PowerShell-scripts in de map van de ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-FailoverToReplica\RecoveryJobs bekijken

### <a name="review-the-application-state-during-recovery"></a>Controleer de status van de toepassing tijdens het herstel
Terwijl het toepassingseindpunt van de is uitgeschakeld in Traffic Manager, wordt de toepassing niet beschikbaar is. Nadat de catalogus is een failover naar de recovery-regio en alle tenants gemarkeerd als offline, is de toepassing weer online. Hoewel de toepassing beschikbaar is, elke tenant wordt als offline weergegeven in de events hub totdat failover van de database wordt uitgevoerd. Het is belangrijk om uw toepassing om af te handelen offline tenantdatabases te ontwerpen.

1. Onmiddellijk nadat de catalogusdatabase is hersteld, vernieuwt u de Wingtip Tickets Events Hub in uw webbrowser.
    * U ziet dat de naam van de catalogus nu heeft in de voettekst een _-recovery_ achtervoegsel en bevindt zich in de herstelregio voor.
    * U ziet dat tenants die nog niet zijn hersteld, zijn gemarkeerd als offline, en zijn niet worden geselecteerd.  

    > [!Note]
    > Met slechts enkele databases te herstellen, kunnen mogelijk niet de browser vernieuwen voordat het herstel is voltooid, zodat u de tenants niet zien kan terwijl ze offline zijn. 
 
    ![Gebeurtenissen hub offline](media/saas-dbpertenant-dr-geo-replication/events-hub-offlinemode.png) 

    * Als u de pagina van de tenant van een offline-gebeurtenissen rechtstreeks opent, wordt een melding offline tenant. Bijvoorbeeld, als Contoso Concert Hall offline is, probeert te openen http://events.wingtip-dpt.&lt; gebruiker&gt;.trafficmanager.net/contosoconcerthall ![Contoso Offline pagina](media/saas-dbpertenant-dr-geo-replication/dr-in-progress-offline-contosoconcerthall.png) 

### <a name="provision-a-new-tenant-in-the-recovery-region"></a>Een nieuwe tenant in de herstelregio inrichten
Voordat de bestaande tenantdatabases failover hebt uitgevoerd, kunt u nieuwe tenants in de herstelregio inrichten.  

1. In de *PowerShell ISE*, opent u de ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 script en stelt u de volgende eigenschap:
    * **$DemoScenario = 4**, een nieuwe tenant in de herstelregio inrichten

2. Druk op **F5** voor het uitvoeren van het script en de nieuwe tenant inrichten. 

3. De pagina van de gebeurtenissen Hawthorn Hall wordt geopend in de browser wanneer deze is voltooid. Opmerking van de voettekst dat de database Hawthorn Hall is ingericht in de herstelregio voor.
    ![Pagina hawthorn Hall-gebeurtenissen](media/saas-dbpertenant-dr-geo-replication/hawthornhallevents.png) 

4. Vernieuw de Wingtip Tickets Events Hub-pagina als u wilt zien dat hawthorn Hall opgenomen in de browser. 
    * Als u Hawthorn Hall ingericht zonder te wachten op de andere tenants om terug te zetten, kunnen andere tenants kunnen nog steeds offline.


## <a name="review-the-recovered-state-of-the-application"></a>Controleer de herstelde status van de toepassing

Wanneer het herstelproces is voltooid, zijn de toepassing en alle tenants volledig functioneel in de herstelregio voor. 

1. Zodra de weergave in de PowerShell-consolevenster geeft aan dat alle tenants worden hersteld dat, vernieuw de Events Hub.  De tenants wordt alle weergegeven online, met inbegrip van de nieuwe tenant, Hawthorn Hall.

    ![herstelde en nieuwe tenants in de events hub](media/saas-dbpertenant-dr-geo-replication/events-hub-with-hawthorn-hall.png)

2. In de [Azure-portal](https://portal.azure.com), opent u de lijst met resourcegroepen.  
    * U ziet de resourcegroep die u hebt geïmplementeerd, plus de herstel-resourcegroep met de _-recovery_ achtervoegsel.  De resourcegroep van recovery bevat alle resources die zijn gemaakt tijdens het herstelproces en nieuwe resources die zijn gemaakt tijdens de storing.  

3. Open de recovery-resourcegroep en ziet u de volgende items:
    * De herstel-versies van de catalogus en tenants1-servers, met _-recovery_ achtervoegsel.  De herstelde catalogus en tenant-databases op deze alle servers hebben de namen in de oorspronkelijke regio gebruikt.

    * De _tenants2-dpt -&lt;gebruiker&gt;-recovery_ SQL server.  Deze server wordt gebruikt voor het inrichten van nieuwe tenants gedurende de storing.
    *   De naam van de App Service, _gebeurtenissen-wingtip-dpt -&lt;recoveryregion&gt;-&lt;gebruiker & gt_; is de herstel-exemplaar van de app. 

    ![Azure recovery-resources ](media/saas-dbpertenant-dr-geo-replication/resources-in-recovery-region.png)    
    
4. Open de _tenants2-dpt -&lt;gebruiker&gt;-recovery_ SQL server.  U ziet dat deze de database bevat _hawthornhall_ en de elastische pool _Pool1_.  De _hawthornhall_ database is geconfigureerd als een elastische database in _Pool1_ elastische pool.

5. Ga terug naar de resourcegroep en klik op de Contoso Concert Hall-database op de _tenants1-dpt -&lt;gebruiker&gt;-recovery_ server. Klik op de Geo-replicatie aan de linkerkant.
    
    ![Contoso-database na een failover](media/saas-dbpertenant-dr-geo-replication/contoso-geo-replication-after-failover.png)

## <a name="change-tenant-data"></a>Gegevens van de tenant wijzigen 
In deze taak werkt u een van de tenant-databases. 

1. De lijst met gebeurtenissen vinden voor de Contoso Concert Hall en noteer de gebeurtenisnaam van de laatste in uw browser.
2. In de *PowerShell ISE*, stel de volgende waarde in de ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 script:
    * **$DemoScenario = 5** een gebeurtenis verwijderen uit een tenant in de herstelregio
3. Druk op **F5** om uit te voeren van het script
4. Vernieuw de pagina van de gebeurtenissen Contoso Concert Hall (http://events.wingtip-dpt.&lt; gebruiker&gt;.trafficmanager.net/contosoconcerthall - vervangen &lt;gebruiker&gt; met de waarde van de gebruiker van uw implementatie) en u ziet dat de laatste gebeurtenis is verwijderd.

## <a name="repatriate-the-application-to-its-original-production-region"></a>De toepassing naar de oorspronkelijke productieregio repatriëren

Deze taak repatriates de toepassing naar de oorspronkelijke regio. In een echte scenario zou u repatriëring starten wanneer de onderbreking verholpen is.

### <a name="repatriation-process-overview"></a>Overzicht van het repatriëring

![Repatriëring-architectuur](media/saas-dbpertenant-dr-geo-replication/repatriation-architecture.png)

Het proces repatriëring:
1. Alle aanvragen die openstaande of die onderweg zijn database terugzetten, wordt geannuleerd.
2. Updates de _newtenant_ alias om te verwijzen naar de server de tenants in de regio van de oorsprong. Deze alias te wijzigen, zorgt u ervoor dat de databases voor alle nieuwe tenants nu worden ingericht in de regio van de oorsprong.
3. De seeding van alle gegevens van de gewijzigde tenant naar de oorspronkelijke regio
4. Failover-schakeling tenant-databases in volgorde van prioriteit.

Failover wordt de database effectief verplaatst naar de oorspronkelijke regio. Wanneer failover optreedt van de database, alle open verbindingen worden verwijderd en de database is niet beschikbaar voor een paar seconden. Toepassingen moeten worden geschreven met de logica voor opnieuw proberen om te controleren of dat ze opnieuw verbinding te maken.  Hoewel deze korte verbreken van de verbinding vaak niet opgemerkt is, moet u besluiten repatriëren databases buiten kantooruren. 


### <a name="run-the-repatriation-script"></a>Voer het script repatriëring
Nu gaan we imagine de onderbreking is opgelost en voer het script repatriëring.

1. In de *PowerShell ISE*, in de ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 script.

2. Controleren of het proces van synchronisatie catalogus nog steeds wordt uitgevoerd in de PowerShell-sessie.  Indien nodig, start u het opnieuw door in te stellen:
    * **$DemoScenario = 1**, begin tenantserver, toepassingen en informatie over de configuratie van de database in de catalogus synchroniseren
    * Druk op **F5** om het script uit te voeren.

3.  Stel vervolgens de volgende opdracht voor het starten van het proces repatriëring:
    * **$DemoScenario = 6**, de app in de oorspronkelijke regio repatriëren
    * Druk op **F5** herstelscript uitvoeren in een nieuwe PowerShell-venster.  Repatriëring kan enkele minuten duren en kan worden gecontroleerd in het PowerShell-venster.
    ![Repatriëring proces](media/saas-dbpertenant-dr-geo-replication/repatriation-process.png)

4. Terwijl het script wordt uitgevoerd, vernieuw de Events Hub-pagina (http://events.wingtip-dpt.&lt; gebruiker&gt;. trafficmanager.net)
    * U ziet dat de Wizardoptie online en toegankelijk tijdens dit proces.

5. Nadat de repatriëring voltooid is, vernieuw de Events hub en open de pagina gebeurtenissen voor Hawthorn Hall. U ziet dat deze database is is besluit zijn verricht naar de oorspronkelijke regio.
    ![Besluit zijn verricht gebeurtenissen-hub](media/saas-dbpertenant-dr-geo-replication/events-hub-repatriated.png)


## <a name="designing-the-application-to-ensure-app-and-database-are-colocated"></a>Het ontwerpen van de toepassing om te controleren of de app en database zijn geplaatst 
De toepassing is zo ontworpen dat deze altijd verbinding vanaf een instantie in dezelfde regio als de tenant-database maakt. Dit ontwerp vermindert de latentie tussen de toepassing en de database. Deze optimalisatie wordt ervan uitgegaan dat de app en database-interactie chattier dan de tussenkomst van de app-gebruiker is.  

Tenant-databases kunnen worden verdeeld over herstel en de oorspronkelijke regio's voor enige tijd tijdens repatriëring. Voor elke database, wordt de app zoekt u de regio waarin de database bevindt zich aan de hand van een DNS-zoekopdracht op de naam van de tenant. In SQL-Database is de naam van de server een alias. De naam van de alias bevat de regionaam van de. Als de toepassing niet in dezelfde regio als de database, wordt hij omgeleid naar het exemplaar in dezelfde regio als de database-server.  Omleiden naar in dezelfde regio als de database-exemplaar wordt geminimaliseerd latentie tussen app en database. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:
> [!div classs="checklist"]

>* Databases en elastische groep configuratie-informatie in de tenantcatalogus synchroniseren
>* Instellen van een omgeving voor herstel in een andere regio op, die bestaat uit de toepassing, servers en toepassingen
>* Gebruik _geo-replicatie_ voor het repliceren van de catalogus en tenant-databases naar de recovery-regio
>* Failover van de toepassing en de catalogus en tenant-databases naar de recovery-regio 
>* Een failback-de toepassing, -catalogus en tenant-databases naar de oorspronkelijke regio na de onderbreking verholpen is

U kunt meer informatie over de technologieën die Azure SQL database biedt om in te schakelen bedrijfscontinuïteit in de [overzicht voor zakelijke continuïteit](sql-database-business-continuity.md) documentatie.

## <a name="additional-resources"></a>Aanvullende resources

* [Aanvullende zelfstudies voort op de Wingtip SaaS-toepassing bouwen](https://docs.microsoft.com/azure/sql-database/sql-database-wtp-overview#sql-database-wingtip-saas-tutorials)
