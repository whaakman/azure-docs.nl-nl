---
title: Herstel na noodgevallen voor SaaS-apps met geografisch redundante back-ups van Azure SQL Database | Microsoft Docs
description: Informatie over het gebruik van Azure SQL Database geografisch redundante back-ups herstellen van een multitenant SaaS-app in het geval van een storing
keywords: zelfstudie sql-database
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: saas apps
ms.topic: article
ms.date: 04/16/2018
ms.author: ayolubek
ms.openlocfilehash: a677e6eb583e293f83df824804aa4cd6f8f5d778
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="recover-a-multi-tenant-saas-application-using-geo-restore-from-database-backups"></a>Een multitenant SaaS-toepassing met behulp van geo-herstel van de databaseback-ups herstellen

In deze zelfstudie maakt kennis u met een volledige noodherstelscenario voor een multitenant SaaS-toepassing geïmplementeerd met behulp van het model van de database per tenant. U gebruikt [ _geo-restore_ ](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-recovery-using-backups) de catalogus en tenant-databases te herstellen vanuit automatisch handhaven geografisch redundante back-ups in een alternatieve herstel gebied. Nadat de onderbreking opgelost is, gebruikt u [ _geo-replicatie_ ](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-geo-replication-overview) te repatriëren gewijzigde databases naar hun oorspronkelijke regio.

![geo-restore-architectuur](media/saas-dbpertenant-dr-geo-restore/geo-restore-architecture.png)

Geo-restore is de laagste kosten noodherstel voor SQL-Database.  Echter terugzetten vanuit geografisch redundante back-ups kan leiden tot verlies van gegevens van maximaal één uur en kunnen een aanzienlijke tijd duren, afhankelijk van de grootte van elke database. **Als u wilt herstellen toepassingen met de laagst mogelijke RPO en RTO, gebruikt u geo-replicatie in plaats van geo-restore**.

Deze zelfstudie behandelt de herstel- en repatriëring werkstromen. In deze zelfstudie leert u procedures om het volgende te doen:
> [!div class="checklist"]

>* Synchronisatiedatabase en configuratiegegevens over elastische pool in de tenant-catalogus
>* Een spiegelbeeld omgeving in een regio 'herstel', die bestaat uit de toepassing, servers en toepassingen instellen    
>* Catalogus en tenant-databases via _geo-herstel_
>* De tenant catalog en het gewijzigde tenant-databases via repatriëren _geo-replicatie_ nadat de onderbreking opgelost is
>* Catalogus bijwerken als elke database wordt hersteld (of besluit zijn verricht) voor het bijhouden van de huidige locatie van het actieve exemplaar van elke tenant-database
>* Zorg ervoor dat de toepassing en tenant-database zijn altijd samen geplaatst in dezelfde Azure-regio voor de latentie te verminderen  
 

Voordat u deze zelfstudie begint, moet dat de volgende vereisten zijn voltooid:
* De database Wingtip Tickets SaaS per tenant-app wordt geïmplementeerd. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en de database Wingtip Tickets SaaS per tenant toepassing verkennen](saas-dbpertenant-get-started-deploy.md)  
* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.

## <a name="introduction-to-the-geo-restore-recovery-pattern"></a>Inleiding tot het patroon van de recovery geo-herstel

Herstel na noodgeval (DR) is een belangrijk aandachtspunt voor veel toepassingen, of om wettelijke redenen of zakelijke continuïteit. Moet er een langdurige servicestoring, kan een goed voorbereid DR-plan onderbreking van de zakelijke minimaliseren. Een DR-abonnement op basis van geo-herstel moet worden gedaan met verschillende doelstellingen:
 * Alle benodigde capaciteit in de regio gekozen herstel zo snel mogelijk gereserveerd om ervoor te zorgen is beschikbaar voor de tenant-databases herstellen.
 * Een spiegelbeeld Herstelomgeving die overeenkomt met de oorspronkelijke configuratie van de groep van toepassingen en de database tot stand brengen 
 * Het moet mogelijk het herstelproces in halverwege vlucht annuleren als de oorspronkelijke regio weer online komt.
 * Inschakelen van de tenant snel worden ingericht, zodat de nieuwe tenant onboarding kunt zo snel mogelijk opnieuw opstarten  
 * Geoptimaliseerd voor het herstellen van tenants in volgorde van prioriteit
 * Geoptimaliseerd voor het ophalen van tenants online zo snel mogelijk door te voeren stappen parallel waar praktische met
 * Netwerkfouten mislukt, opnieuw starten, en de idempotent
 * Repatriëren databases naar hun oorspronkelijke regio met minimale gevolgen voor tenants wanneer de onderbreking is opgelost.  

> [!Note]
> De toepassing kunnen worden hersteld in de _gekoppelde regio_ van de regio waarin de toepassing wordt geïmplementeerd. Zie voor meer informatie [Azure regio's gekoppeld](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions).   



In deze zelfstudie worden deze uitdagingen aangepakt gebruikt functies van Azure SQL Database en de Azure-platform:

* [Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template), om zo snel mogelijk alle benodigde capaciteit reserveren. Azure Resource Manager-sjablonen worden gebruikt voor het inrichten van een spiegelbeeld van de oorspronkelijke servers en elastische pools in de regio van het herstel. Een afzonderlijke server en de pool worden ook gemaakt voor het inrichten van nieuwe tenants. 
* [Clientbibliotheek voor elastische Database](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-elastic-database-client-library) (EDCL) maken en onderhouden van een tenant-database-catalogus.  De catalogus is uitgebreid met configuratiegegevens regelmatig vernieuwd en de database.
* [Beheerfuncties voor herstel shard](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-recovery-manager) van de EDCL items in de database locatie in de catalogus tijdens herstel en repatriëring onderhouden.  
* [Geo-restore](https://docs.microsoft.com/azure/sql-database/sql-database-disaster-recovery), de catalogus en tenant-databases herstellen vanuit automatisch handhaven geografisch redundante back-ups. 
* [Asynchrone herstelbewerkingen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations) in volgorde van prioriteit van de tenant die door het systeem voor elke groep in de wachtrij en verwerkt in batches, zodat de toepassingen niet overbelast is verzonden. Deze bewerkingen kunnen worden geannuleerd vóór of tijdens het uitvoeren van indien nodig.    
* [Geo-replicatie](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview)om databases naar de oorspronkelijke regio repatriëren na de storing. Gebruik van geo-replicatie, zorgt dat er is geen verlies van gegevens en de minimale gevolgen voor de tenant.
* [SQL server-DNS-aliassen](https://docs.microsoft.com/azure/sql-database/dns-alias-overview) waarmee het verbinding maken met de catalogus van active ongeacht de locatie van het synchronisatieproces van catalogus.  

## <a name="get-the-disaster-recovery--scripts"></a>De disaster recovery scripts ophalen 

De DR-scripts die worden gebruikt in deze zelfstudie zijn beschikbaar in de [Wingtip Tickets SaaS-database per tenant GitHub-opslagplaats](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant). Bekijk de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de Wingtip Tickets management scripts deblokkeren.
> [!IMPORTANT]
> Net als alle Wingtip Tickets management scripts, de DR-scripts zijn voorbeeld kwaliteit en niet moet worden gebruikt in productie.   

## <a name="review-the-healthy-state-of-the-application"></a>Controleer de status van de toepassing in orde
Voordat u het herstelproces start, controleert u de normale status in orde van de toepassing.
1. Open in uw webbrowser de Hub Wingtip Tickets gebeurtenissen (http://events.wingtip-dpt.&lt; gebruiker&gt;. trafficmanager.net - vervangen &lt;gebruiker&gt; met uw implementatie gebruiker waarde).
    * Ga naar de onderkant van de pagina en Let op de naam van server-catalogus en de locatie in de voettekst.  De locatie is de regio waarin u de app hebt geïmplementeerd.    
        TIP: Beweeg de muisaanwijzer over de locatie voor het vergroten van de weergave.

    ![Gebeurtenissen hub status in orde in oorspronkelijke regio](media/saas-dbpertenant-dr-geo-restore/events-hub-original-region.png)

1. Klik op de tenant Contoso overleg Hall en open de pagina van de gebeurtenis.
    * U ziet de servernaam voor tenants in de voettekst. De locatie is hetzelfde als de locatie van de catalogusserver.

    ![Contoso overleg Hall oorspronkelijke regio](media/saas-dbpertenant-dr-geo-restore/contoso-original-location.png)    
1. In de [Azure-portal](https://portal.azure.com)controleert en open de resourcegroep waarin u de app hebt geïmplementeerd.
    * U ziet de resources en de regio waarin de app service-onderdelen en SQL Database-servers zijn geïmplementeerd.

## <a name="sync-tenant-configuration-into-catalog"></a>Configuratie van de synchronisatie-tenant in catalogus

In deze taak start u een proces voor het synchroniseren van de configuratie van de servers, elastische pools en databases die in de tenant-catalogus.  Deze informatie wordt later gebruikt voor het configureren van een omgeving spiegelbeeld in de regio van het herstel.

> [!IMPORTANT]
> Ter vereenvoudiging zijn van het synchronisatieproces en andere langdurige herstel en repatriëring processen geïmplementeerd in deze voorbeelden als lokale Powershell-taken of sessies die worden uitgevoerd onder uw client-gebruikersaanmelding. De verificatietokens uitgegeven als u aanmelding verlopen na enkele uren en vervolgens de taken mislukken. In een productiescenario moeten langlopende processen worden geïmplementeerd als betrouwbare Azure-services type, uitgevoerd onder een service-principal. Zie [gebruik Azure PowerShell een service-principal maken met een certificaat](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal). 

1. In de _PowerShell ISE_, opent u het ...\Learning Modules\UserConfig.psm1-bestand. Vervang `<resourcegroup>` en `<user>` op regels 10 en 11 met de waarde die wordt gebruikt wanneer u de app hebt geïmplementeerd.  Sla het bestand!

2. In de *PowerShell ISE*, opent u de ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 script.
    *  Tijdens deze zelfstudie, voert u elk van de scenario's in deze PowerShell script zodat dit bestand open houden.

3. Stelt u het volgende:
    * **$DemoScenario = 1**, een achtergrondtaak die wordt gesynchroniseerd tenant server starten en informatie over de configuratie van toepassingen in de catalogus

3. Druk op **F5** de synchronisatie-script uit te voeren. 
    *  Deze informatie wordt later gebruikt om ervoor te zorgen dat herstel het spiegelbeeld van de servers, -adresgroepen en databases in de regio van het herstel maakt.  
![Synchronisatieproces](media/saas-dbpertenant-dr-geo-restore/sync-process.png)

Het PowerShell-venster op de achtergrond uitgevoerd en blijft met de rest van deze zelfstudie.

> [!Note]
> Het synchronisatieproces verbindt met de catalogus via een DNS-alias. De alias is gewijzigd tijdens terugzetten en repatriëring om te verwijzen naar de actieve catalogus. Het synchronisatieproces houdt de catalogus up-to-date met de database of een groep configuratiewijzigingen aangebracht in de regio van het herstel.  Tijdens repatriëring, worden deze wijzigingen worden toegepast op de gelijkwaardige resources in de oorspronkelijke regio.

## <a name="geo-restore-recovery-process-overview"></a>Overzicht van het herstel geo-herstel

Het herstelproces geo-restore implementeert de toepassing en databases herstelt vanuit back-ups in de regio van het herstel.

Het herstelproces doet het volgende:

1. Hiermee schakelt u het Traffic Manager-eindpunt voor de web-app in de oorspronkelijke regio. Het uitschakelen van het eindpunt voorkomen dat gebruikers verbinding maken met de app in een ongeldige status moet de oorspronkelijke regio on line tijdens het herstel.

1. Richt een herstel-catalogusserver in het herstel de regio geo herstelt de catalogusdatabase, en updates de _activecatalog_ alias om te verwijzen naar de teruggezette catalog-server.  
    * Wijzigen van de catalogus alias zorgt ervoor dat het synchronisatieproces catalogus altijd wordt gesynchroniseerd met de catalogus van active.

1. Geeft alle bestaande tenants in de catalogus herstel als offline toegang tot wilt voorkomen tenant databases voordat deze kunnen worden hersteld.

1. Richt een exemplaar van de app in de regio van herstel en geconfigureerd voor gebruik van de herstelde catalogus in deze regio.
    * Als u wilt behouden latentie tot een minimum te beperken, is de voorbeeld-app zodanig ontworpen dat deze altijd verbinding met een tenant-database in dezelfde regio maakt.

1. Richt een server en de elastische pool waarin nieuwe tenants worden ingericht. Maken van deze bronnen, zorgt u ervoor dat het inrichten van nieuwe tenants niet wordt verwerkt door het herstel van bestaande tenants.

1. Updates van de nieuwe tenant alias om te verwijzen naar de server voor de nieuwe tenant-databases in de regio van het herstel. Het wijzigen van deze alias, zorgt u ervoor dat de databases voor een nieuwe tenants in de regio voor herstel zijn ingericht.
        
1. Bepalingen servers en elastische pools in het gebied herstel voor tenant databases terugzetten. Deze servers en -groepen zijn het spiegelbeeld van de configuratie in de oorspronkelijke regio.  Pools vooraf inrichten reserveert de capaciteit die nodig zijn voor alle databases te herstellen.
    * Een storing in een regio kan aanzienlijke druk op de beschikbare bronnen in de gekoppelde regio plaatsen.  Als u gebruik maakt van geo-herstel voor herstel na Noodgevallen, wordt vervolgens het reserveren van resources snel aanbevolen. Overweeg het gebruik van geo-replicatie als het is essentieel dat een toepassing moet worden hersteld in een specifieke regio. 

1. Hiermee schakelt u het Traffic Manager-eindpunt voor de Web-app in de regio van het herstel. Inschakelen van dit eindpunt, kunt de toepassing voor het inrichten van nieuwe tenants. In deze fase worden de bestaande tenants nog steeds offline.

1. Batches van aanvragen voor het herstellen van databases in volgorde van prioriteit worden verzonden. 
    * Batches zijn ingedeeld zodat databases tussen alle pools parallel worden hersteld.  
    * Herstel aanvraagt asynchroon worden verzonden, zodat ze worden verzonden snel en in de wachtrij voor uitvoering in elke groep.
    * Omdat de restore-aanvragen worden parallel verwerkt over alle toepassingen, is het beter om belangrijke tenants verdelen over veel toepassingen. 

1. Bewaakt de SQL database-service om te bepalen wanneer databases worden hersteld. Wanneer een database van de tenant is teruggezet, het online in de catalogus is gemarkeerd en de som rowversion voor de tenant-database wordt geregistreerd. 
    * Tenant-databases zijn toegankelijk door de toepassing zodra ze zijn gemarkeerd in de catalogus online.
    * De som van waarden in de database van de tenant rowversion wordt opgeslagen in de catalogus. Deze som fungeert als een vingerafdruk waarmee het proces repatriëring om te bepalen of de database is bijgewerkt in de regio van het herstel.      

## <a name="run-the-recovery-script"></a>Voer het script herstel

> [!IMPORTANT]
> Deze zelfstudie herstelt databases vanuit geografisch redundante back-ups. Hoewel deze back-ups gewoonlijk beschikbaar binnen 10 minuten zijn, er kan een uur duren voordat ze beschikbaar zijn. Het script wordt onderbroken totdat ze beschikbaar zijn.  Tijd om een koffie!

Nu voorstellen dat er een storing in het gebied waarin de toepassing is geïmplementeerd en voer het script voor herstel is:

1. In de *PowerShell ISE*, stel de volgende waarden in de ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 script:
    * **$DemoScenario = 2**, de app in een regio herstel door terug te zetten vanuit geografisch redundante back-ups herstellen

1. Druk op **F5** om het script uit te voeren.  
    * Het script in een nieuw PowerShell-venster wordt geopend en start vervolgens een set PowerShell-taken die parallel worden uitgevoerd.  Deze taken worden naar de regio herstel servers, -adresgroepen en -databases herstellen. 
    * De regio van het herstel is de _gekoppelde regio_ die zijn gekoppeld aan de Azure-regio waarin u de toepassing is geïmplementeerd. Zie voor meer informatie [Azure regio's gekoppeld](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions). 

1. De status van het herstelproces in de PowerShell-venster bewaken.

    ![Herstelproces](media/saas-dbpertenant-dr-geo-restore/dr-in-progress.png)

>Bekijk de PowerShell-scripts in de map van Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-RestoreFromBackup\RecoveryJobs ...\Learning de code voor de hersteltaken om's te bekijken.

## <a name="review-the-application-state-during-recovery"></a>Controleer de status van de toepassing tijdens het herstel
Het toepassingseindpunt is uitgeschakeld in Traffic Manager, is de toepassing niet beschikbaar. De catalogus is hersteld en worden alle huurders offline gemarkeerd.  Het toepassingseindpunt in de regio recovery wordt ingeschakeld en de toepassing weer online is. Hoewel de toepassing beschikbaar is, tenants offline worden weergegeven in de hub gebeurtenissen totdat hun databases worden hersteld. Het is belangrijk voor het ontwerpen van uw toepassing om af te handelen offline tenant-databases.

1. Nadat de catalogusdatabase is hersteld, maar voordat de tenants weer online zijn, vernieuwt u de Hub Wingtip Tickets gebeurtenissen in uw webbrowser.
    * In de voettekst u ziet dat de naam van de catalogus nu een _-herstel_ achtervoegsel en bevindt zich in de regio van het herstel.
    * U ziet dat tenants die nog niet zijn hersteld zijn gemarkeerd als offline, en niet kunnen geselecteerd worden.   
 
    ![Herstelproces](media/saas-dbpertenant-dr-geo-restore/events-hub-tenants-offline-in-recovery-region.png)    

    * Als u een tenant gebeurtenissen pagina rechtstreeks opent terwijl de tenant offline is, verschijnt de pagina een melding offline tenant. Bijvoorbeeld, als Contoso overleg Hall offline is, probeert te openen http://events.wingtip-dpt.&lt; gebruiker&gt;.trafficmanager.net/contosoconcerthall ![herstelproces](media/saas-dbpertenant-dr-geo-restore/dr-in-progress-offline-contosoconcerthall.png)

## <a name="provision-a-new-tenant-in-the-recovery-region"></a>Een nieuwe tenant in de regio herstel inrichten
Zelfs voordat tenant databases worden teruggezet, kunt u nieuwe tenants in de regio herstel inrichten. Nieuwe tenant databases ingericht in de regio van het herstel wordt worden besluit zijn verricht met de herstelde databases later.   

1. In de *PowerShell ISE*, in de ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 script, stelt u de volgende eigenschap:
    * **$DemoScenario = 3**, een nieuwe tenant in de regio herstel inrichten

1. Druk op **F5** om het script uit te voeren. 

1. De pagina Hawthorn Hall gebeurtenissen wordt geopend in de browser bij het inrichten is voltooid. 
    * U ziet dat de database Hawthorn Hall bevindt zich in de regio van het herstel.
    ![Hawthorn Hall ingericht in de regio herstel](media/saas-dbpertenant-dr-geo-restore/hawthorn-hall-provisioned-in-recovery-region.png)

1. Vernieuw de Wingtip Tickets gebeurtenissen Hub-pagina om te bekijken dat hawthorn Hall opgenomen in de browser. 
    * Als u Hawthorn Hall ingericht zonder te wachten op voor de andere tenants te herstellen, worden andere tenants nog steeds offline.

## <a name="review-the-recovered-state-of-the-application"></a>Controleer de herstelde status van de toepassing

Wanneer het herstelproces is voltooid, zijn de toepassing en alle tenants volledig functioneel in de regio van het herstel. 

1. Zodra de weergave in het venster PowerShell-console geeft aan dat de tenants worden hersteld, vernieuwt u de Hub gebeurtenissen.  De tenants verschijnt alle online, inclusief de nieuwe tenant, Hawthorn Hall.

    ![herstelde en nieuwe tenants in de hub gebeurtenissen](media/saas-dbpertenant-dr-geo-restore/events-hub-with-hawthorn-hall.png)

1. Klik op Contoso overleg Hall en open de pagina gebeurtenissen.
    * Let op dat de database bevindt zich op de herstelserver zich in de regio van het herstel in de voettekst.

    ![Contoso in de regio herstel](media/saas-dbpertenant-dr-geo-restore/contoso-recovery-location.png)

1. In de [Azure-portal](https://portal.azure.com), opent u de lijst met resourcegroepen.  
    * U ziet de resourcegroep die u hebt geïmplementeerd, plus de resourcegroep herstel met de _-herstel_ achtervoegsel.  De resourcegroep herstel bevat alle resources die zijn gemaakt tijdens het herstelproces plus nieuwe resources die zijn gemaakt tijdens de onderbreking.  

1. Open de resourcegroep van herstel en Let op de volgende items:
    * De versies van het herstel van de servers catalogus en tenants1 met _-herstel_ achtervoegsel.  De herstelde catalogus en tenant-databases op deze alle servers hebben de namen in de oorspronkelijke regio gebruikt.

    * De _tenants2-dpt -&lt;gebruiker&gt;-herstel_ SQL server.  Deze server wordt gebruikt voor het inrichten van nieuwe tenants tijdens de onderbreking.
    *   De App Service met de naam, _gebeurtenissen-wingtip-dpt -&lt;recoveryregion&gt;-&lt;gebruiker & gt_; dit is het exemplaar van het herstel van de app gebeurtenissen. 

    ![Contoso in de regio herstel](media/saas-dbpertenant-dr-geo-restore/resources-in-recovery-region.png)   
    
1. Open de _tenants2-dpt -&lt;gebruiker&gt;-herstel_ SQL server.  U ziet deze de database bevat _hawthornhall_ en de elastische groep _Pool1_.  De _hawthornhall_ database geconfigureerd als een elastische database in _Pool1_ elastische pool.

## <a name="change-tenant-data"></a>Gegevens over tenant 
In deze taak bijwerken u een van de herstelde tenant-databases. Het proces repatriëring kopieert herstelde databases die zijn gewijzigd naar de oorspronkelijke regio. 

1. Blader door de gebeurtenissen in uw browser, zoeken naar lijst met de gebeurtenissen voor het Contoso overleg Hall, en noteer de laatste gebeurtenis _ernstig Strauss_.

1. In de *PowerShell ISE*, stelt u de volgende waarde in de ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 script:
    * **$DemoScenario = 4**, een gebeurtenis verwijderen uit een tenant in de regio herstel
1. Druk op **F5** het script wilt uitvoeren.
1. Vernieuw de pagina van de gebeurtenissen Contoso overleg Hall (http://events.wingtip-dpt.&lt; gebruiker&gt;.trafficmanager.net/contosoconcerthall) en u ziet dat de gebeurtenis ernstig Strauss ontbreekt.

Op dit moment in de zelfstudie hebt u de toepassing, wat wordt nu uitgevoerd in de regio herstel hersteld.  U hebt een nieuwe tenant in de regio herstel ingericht en gegevens van een van de herstelde tenants gewijzigd.  

> [!Note]
> Andere zelfstudies in de steekproef zijn niet ontworpen om uit te voeren met de app in de herstelstatus. Als u verkennen andere zelfstudies wilt, zorg er dan voor dat de toepassing eerst repatriëren.

## <a name="repatriation-process-overview"></a>Overzicht van het proces repatriëring

Het proces repatriëring wordt de toepassing en de databases naar de oorspronkelijke regio teruggezet na een storing is opgelost.

![Geo-restore repatriëring](media/saas-dbpertenant-dr-geo-restore/geo-restore-repatriation.png) 


Het proces:

1. Elke activiteit lopende terugzetten stopt en openstaande of onderweg database terugzetten verzoeken annuleert.

1. Opnieuw activeren in de oorspronkelijke regio tenant databases die niet zijn gewijzigd sinds de storing.  Dit omvat de databases die nog niet zijn hersteld en databases die zijn hersteld, maar niet later zijn gewijzigd. De databases opnieuw geactiveerde zijn exact als laatste toegankelijk is voor hun tenants.

1. Richt een spiegelbeeld van de nieuwe tenants server en de elastische pool in de oorspronkelijke regio. Nadat deze actie voltooid is, wordt de alias van de nieuwe tenant wordt bijgewerkt om te verwijzen naar deze server. Bijwerken van de alias zorgt ervoor dat de nieuwe tenant voorbereiden om te worden uitgevoerd in de oorspronkelijke regio in plaats van de regio van het herstel.

1. Geo-replicatie gebruikt, verplaatst de catalogus voor de oorspronkelijke regio van het herstel de regio.

1. Updates van toepassingen is geconfigureerd in de oorspronkelijke regio zodat deze consistent zijn met wijzigingen in de regio herstel tijdens de onderbreking.

1. De vereiste server (s) en pool(s) voor het hosten van nieuwe databases gemaakt tijdens de onderbreking maakt.

1. Geo-replicatie gebruikt, repatriates hersteld databases die zijn bijgewerkt na herstellen-tenant en alle nieuwe tenant databases tijdens de onderbreking is ingericht. 

1. Ruimt resources in de regio van het herstelpunt is gemaakt tijdens het terugzetten.

Stap 1-3 worden om te beperken het aantal tenant-databases die moeten worden besluit zijn verricht, onmiddellijk uitgevoerd.  

Stap 4 wordt alleen uitgevoerd als de catalogus in de regio recovery is gewijzigd tijdens de onderbreking. De catalogus wordt bijgewerkt als nieuwe tenants worden gemaakt of als een database of een groep-configuratie wordt gewijzigd in de regio van het herstel.

Het is belangrijk dat de stap 7 zorgt ervoor minimaal ongemak tenants dat en gegevens niet verloren. Voor dit doel te bereiken, het proces gebruikt _geo-replicatie_.

Voordat elke database geogerepliceerde is, wordt de bijbehorende database in de oorspronkelijke regio verwijderd. De database in de regio van het herstel is vervolgens geogerepliceerde, het maken van een secundaire replica in de oorspronkelijke regio. Zodra de replicatie is voltooid, de tenant is gemarkeerd als offline in de catalogus, die wordt verbroken verbindingen met de database in de regio van het herstel. De database is vervolgens storing opgetreden, waardoor in behandeling zijnde transacties die moeten worden verwerkt op de secundaire waardoor gegevens niet verloren. Failover, worden de databaserollen omgekeerd.  De secundaire in de oorspronkelijke regio, wordt de primaire alleen-lezen database en de database in de regio van het herstel wordt een secundaire alleen-lezen. De tenant-vermelding in de catalogus wordt bijgewerkt om te verwijzen naar de database in de oorspronkelijke regio en de tenant is gemarkeerd als online.  Op dit moment is repatriëring van de database voltooid. 

Toepassingen worden geschreven met Pogingslogica om te controleren of dat ze automatisch opnieuw verbinding maken wanneer er verbindingen zijn verbroken.  Wanneer u ze opnieuw verbinding maakt met behulp van de catalogus voor de verbinding broker, ze verbinding maken met de repatriated database in de oorspronkelijke regio. Hoewel de korte disconnect vaak niet opgemerkt is, kunt u repatriëren databases buiten kantooruren. 

Wanneer een database is besluit zijn verricht, kan de secundaire database in de regio van het herstel kan worden verwijderd. De database in de oorspronkelijke regio is vervolgens is afhankelijk van geo-herstel voor DR-beveiliging opnieuw.

In stap 8 worden resources in het gebied van herstel, met inbegrip van de herstel-servers en toepassingen, verwijderd.

## <a name="run-the-repatriation-script"></a>Voer het script repatriëring
Nu gaan we Denk aan de onderbreking is opgelost en voer het script repatriëring.

Als u de zelfstudie hebt gevolgd, het script onmiddellijk wordt opnieuw geactiveerd Fabrikam Jazz vereniging en kornoelje Dojo in de oorspronkelijke regio omdat ze niet gewijzigd. Het repatriates vervolgens de nieuwe tenant Hawthorn Hall en Contoso overleg Hall omdat deze is gewijzigd. Het script repatriates ook de catalogus is bijgewerkt wanneer Hawthorn Hall is ingericht.
  
1. In de *PowerShell ISE*naar het ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 script.

1. Controleer of het proces van synchronisatie catalogus nog wordt uitgevoerd in de PowerShell-sessie.  Indien nodig, deze opnieuw starten door in te stellen:
    * **$DemoScenario = 1**, begin tenant-server, toepassingen en informatie over de configuratie van de database in de catalogus synchroniseren
    * Druk op **F5** om het script uit te voeren.
1.  Stel vervolgens de volgende opdracht voor het starten van het proces repatriëring:
    * **$DemoScenario = 5**, de app in de oorspronkelijke regio repatriëren
    * Druk op **F5** het script herstel uitvoeren in een nieuw PowerShell-venster.  Repatriëring duurt enkele minuten en kan worden bewaakt in het PowerShell-venster.
1. Terwijl het script wordt uitgevoerd, vernieuw de pagina gebeurtenissen Hub (http://events.wingtip-dpt.&lt; gebruiker&gt;. trafficmanager.net)
    * U ziet dat alle huurders online is en toegankelijk tijdens dit proces.
1. Klik op de vereniging van Fabrikam Jazz om dit te openen. Als u deze tenant niet is gewijzigd, ziet u in de voettekst de server is al teruggedraaid naar de oorspronkelijke server.
1. Open of vernieuw de pagina van de gebeurtenissen Contoso overleg Hall en u ziet in de voettekst dat de database is nog steeds op de _-herstel_ server in eerste instantie.  
1. Vernieuw de pagina Contoso overleg Hall gebeurtenissen wanneer het proces repatriëring is voltooid en u ziet dat de database zich nu in uw oorspronkelijke regio.
1. Vernieuw de hub gebeurtenissen opnieuw en Hawthorn Hall openen en u ziet dat de database bevindt zich ook in de oorspronkelijke regio. 

## <a name="clean-up-recovery-region-resources-after-repatriation"></a>Herstel regio resources opschonen na repatriëring
Zodra de repatriëring is voltooid, wordt het veilig verwijderen van de resources in de regio van het herstel. 

> [!IMPORTANT]
> Deze resources zo spoedig mogelijk als u wilt stoppen alle facturering voor deze verwijdert.

Het herstelproces wordt gemaakt van alle herstel-resources in een resourcegroep voor herstel. Het opruimproces wordt deze resourcegroep verwijderen en verwijder alle verwijzingen naar de bronnen uit de catalogus. 

1. In de *PowerShell ISE*, in de ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 script, verzameling:
    * **$DemoScenario = 6**, verouderd resources verwijderen uit de herstel-gebied

1. Druk op **F5** om het script uit te voeren.

Na het opruimen van de scripts, is de toepassing weer terug waar deze is gestart.  U kunt het script opnieuw uitvoert of andere zelfstudies op dit moment uitproberen.

## <a name="designing-the-application-to-ensure-app-and-database-are-colocated"></a>Het ontwerpen van de toepassing om te controleren of de app en database zijn geplaatst 
De toepassing is zodanig ontworpen dat altijd verbinding wordt gemaakt van een exemplaar in dezelfde regio bevinden als de tenant-database. Dit ontwerp vermindert de latentie tussen de toepassing en de database. Deze optimalisatie wordt ervan uitgegaan dat de app naar database interactie chattier dan de interactie app gebruiker.  

Tenant-databases kunnen worden verdeeld over herstel en de oorspronkelijke regio's voor enige tijd tijdens repatriëring.  De app zoekt de regio waarin de database bevindt zich een DNS-zoekopdracht op de naam van de tenant als volgt voor elke database. In SQL-Database is de naam van de server een alias. De naam van de alias bevat de regionaam. Als de toepassing niet in dezelfde regio bevinden als de database, wordt hij omgeleid naar het exemplaar in dezelfde regio bevinden als de databaseserver.  Omleiden naar een exemplaar in dezelfde regio bevinden als de database minimaliseert latentie tussen de app en database.  

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:
> [!div class="checklist"]

>* De tenant-catalogus gebruiken voor het opslaan van regelmatig vernieuwd configuratie-informatie, zodat een spiegelbeeld Herstelomgeving worden gemaakt in een andere regio
>* Geo-restore Azure SQL-databases te herstellen in het gebied herstel gebruiken
>* De tenant-catalogus naar aanleiding van herstelde tenant databaselocaties bijwerken  
>* Een DNS-alias gebruiken om in te schakelen van een toepassing verbinding maken met de catalogus van de tenant in de gehele zonder herconfiguratie
>* Geo-replicatie gebruiken om te repatriëren herstelde databases naar hun oorspronkelijke regio na een storing opgelost is

Nu de [herstel na noodgevallen voor een multitenant SaaS-toepassing met behulp van de database geo-replicatie](saas-dbpertenant-dr-geo-replication.md) voor informatie over het aanzienlijk minder tijd die nodig zijn voor het herstellen van een grootschalige multitenant-toepassing met behulp van geo-replicatie.

## <a name="additional-resources"></a>Aanvullende resources

* [Aanvullende zelfstudies waarin voort op de Wingtip SaaS-toepassing bouwen](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-wtp-overview#sql-database-wingtip-saas-tutorials)
