---
title: Gebruik van geografisch redundante back-ups van Azure SQL Database voor herstel na noodgevallen van SaaS-apps | Microsoft Docs
description: Informatie over het gebruik van Azure SQL Database geografisch redundante back-ups herstellen van een multitenant SaaS-app in het geval van een storing
keywords: zelfstudie sql-database
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: saas apps
ms.topic: conceptual
ms.date: 04/16/2018
ms.author: ayolubek
ms.openlocfilehash: fc28622ad6d2569d455f82a30a84c47647baa1e2
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34645331"
---
# <a name="use-geo-restore-to-recover-a-multitenant-saas-application-from-database-backups"></a>Geo-herstel met een multitenant SaaS-toepassing herstellen vanuit back-ups

Deze zelfstudie behandelt een volledige noodherstelscenario voor een multitenant SaaS-toepassing geïmplementeerd met de database per tenant-model. U gebruikt [geo-restore](https://docs.microsoft.com/azure/sql-database/sql-database-recovery-using-backups) de catalogus en tenant-databases te herstellen vanuit automatisch handhaven geografisch redundante back-ups in een alternatieve herstel gebied. Nadat de onderbreking opgelost is, gebruikt u [geo-replicatie](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview) te repatriëren gewijzigde databases naar hun oorspronkelijke regio.

![geo-restore-architectuur](media/saas-dbpertenant-dr-geo-restore/geo-restore-architecture.png)

Geo-restore is de laagste kosten noodherstel voor Azure SQL Database. Echter, terugzetten vanuit geografisch redundante back-ups kan leiden tot verlies van gegevens van maximaal één uur. Het kan aanzienlijke tijd duren, afhankelijk van de grootte van elke database. 

> [!NOTE]
> Toepassingen met de laagst mogelijke RPO en RTO herstellen met behulp van geo-replicatie in plaats van geo-herstel.

Deze zelfstudie behandelt de herstel- en repatriëring werkstromen. In deze zelfstudie leert u procedures om het volgende te doen:
> [!div class="checklist"]

>* Synchronisatiedatabase en configuratiegegevens over elastische pool in de tenant-catalogus.
>* Instellen van een omgeving spiegelbeeld in een regio herstel met toepassingen, servers en toepassingen.   
>* Catalogus en tenant-databases herstellen met behulp van geo-herstel.
>* Gebruik geo-replicatie om de catalogus van de tenant en de gewijzigde tenant databases repatriëren nadat de onderbreking opgelost is.
>* Catalogus bijwerken als elke database wordt hersteld (of besluit zijn verricht) voor het bijhouden van de huidige locatie van het actieve exemplaar van elke tenant-database.
>* Zorg ervoor dat de toepassing en het tenant-database altijd dezelfde zich in dezelfde Azure-regio voor de latentie te verminderen. 
 

Voordat u deze zelfstudie begint, moet u de volgende vereisten voldoen:
* De database Wingtip Tickets SaaS per tenant app implementeren. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en de database Wingtip Tickets SaaS per tenant toepassing verkennen](saas-dbpertenant-get-started-deploy.md). 
* Installeer Azure PowerShell. Zie voor meer informatie [aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-the-geo-restore-recovery-pattern"></a>Inleiding tot het patroon van de recovery geo-herstel

Herstel na noodgeval (DR) is een belangrijk aandachtspunt voor veel toepassingen, of om wettelijke redenen of zakelijke continuïteit. Als er een langdurige servicestoring, kan een goed voorbereid DR-plan business onderbreking kunt minimaliseren. Een DR-abonnement op basis van geo-herstel moet worden gedaan met verschillende doelstellingen:
 * Alle benodigde capaciteit in de regio gekozen herstel zo snel mogelijk om ervoor te zorgen dat deze tenant database weer beschikbaar is gereserveerd.
 * Het opzetten van een omgeving voor het herstel van spiegelbeeld die overeenkomt met de oorspronkelijke configuratie van de groep van toepassingen en -database. 
 * Annulering van het herstelproces in halverwege vlucht toestaan als de oorspronkelijke regio weer online wordt gezet.
 * Schakel tenant snel worden ingericht, zodat de nieuwe tenant onboarding kunt zo snel mogelijk opnieuw opstarten.
 * Voor het herstellen van tenants in volgorde van prioriteit worden geoptimaliseerd.
 * Als u online tenants zo snel mogelijk als volgt stappen parallel waar praktische worden geoptimaliseerd.
 * Worden robuuste mislukt, opnieuw starten, en de idempotent.
 * Repatriëren databases naar hun oorspronkelijke regio met minimale gevolgen voor tenants wanneer de onderbreking is opgelost.  

> [!NOTE]
> De toepassing wordt hersteld in de gekoppelde gebied van de regio waarin de toepassing wordt geïmplementeerd. Zie voor meer informatie [Azure regio's gekoppeld](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).   

Deze zelfstudie maakt gebruik van functies van Azure SQL Database en de Azure-platform om deze uitdagingen:

* [Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template), om zo snel mogelijk alle benodigde capaciteit reserveren. Azure Resource Manager-sjablonen worden gebruikt voor het inrichten van een spiegelbeeld van de oorspronkelijke servers en elastische pools in de regio van het herstel. Een afzonderlijke server en de pool worden ook gemaakt voor het inrichten van nieuwe tenants.
* [Clientbibliotheek voor elastische Database](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-client-library) (EDCL) maken en onderhouden van een tenant-database-catalogus. De uitgebreide catalogus bevat configuratiegegevens regelmatig vernieuwd en de database.
* [Beheerfuncties voor herstel shard](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-recovery-manager) van de EDCL, items in de database locatie in de catalogus tijdens herstel en repatriëring onderhouden.  
* [Geo-restore](https://docs.microsoft.com/azure/sql-database/sql-database-disaster-recovery), de catalogus en tenant-databases herstellen vanuit automatisch handhaven geografisch redundante back-ups. 
* [Asynchrone herstelbewerkingen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations), in volgorde van prioriteit tenant verzonden, worden door het systeem voor elke groep in de wachtrij en verwerkt in batches, zodat de groep is niet overbelast. Deze bewerkingen kunnen worden geannuleerd vóór of tijdens het uitvoeren van indien nodig.   
* [Geo-replicatie](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview)om databases naar de oorspronkelijke regio repatriëren na de storing. Er is geen verlies van gegevens en minimale gevolgen voor de tenant wanneer u geo-replicatie gebruikt.
* [SQL server-DNS-aliassen](https://docs.microsoft.com/azure/sql-database/dns-alias-overview), zodat het verbinding maken met de catalogus van active ongeacht de locatie van het synchronisatieproces van catalogus.  

## <a name="get-the-disaster-recovery-scripts"></a>De disaster recovery scripts ophalen

De DR-scripts die worden gebruikt in deze zelfstudie zijn beschikbaar in de [Wingtip Tickets SaaS-database per tenant GitHub-opslagplaats](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant). Bekijk de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de Wingtip Tickets management scripts deblokkeren.

> [!IMPORTANT]
> Net als alle Wingtip Tickets management scripts, de DR-scripts zijn voorbeeld kwaliteit en niet moet worden gebruikt in productie.

## <a name="review-the-healthy-state-of-the-application"></a>Controleer de status van de toepassing in orde
Voordat u het herstelproces start, controleert u de normale status in orde van de toepassing.

1. Open in uw webbrowser de Wingtip Tickets gebeurtenissen hub (http://events.wingtip-dpt.&lt; gebruiker&gt;. trafficmanager.net, Vervang &lt;gebruiker&gt; met uw implementatie gebruiker waarde).
    
   Ga naar de onderkant van de pagina en Let op de naam van server-catalogus en de locatie in de voettekst. De locatie is de regio waarin u de app hebt geïmplementeerd.    

   > [!TIP]
   > Beweeg de muisaanwijzer over de locatie voor het vergroten van de weergave.

   ![Gebeurtenissen hub status in orde in oorspronkelijke regio](media/saas-dbpertenant-dr-geo-restore/events-hub-original-region.png)

2. Selecteer de tenant Contoso overleg Hall en open de pagina van de gebeurtenis.

   U ziet de servernaam van de tenant in de voettekst. De locatie is hetzelfde als de locatie van de catalogusserver.

   ![Contoso overleg Hall oorspronkelijke regio](media/saas-dbpertenant-dr-geo-restore/contoso-original-location.png) 

3. In de [Azure-portal](https://portal.azure.com)controleert en open de resourcegroep waarin u de app hebt geïmplementeerd.

   U ziet de resources en de regio waarin de app service-onderdelen en SQL Database-servers zijn geïmplementeerd.

## <a name="sync-the-tenant-configuration-into-the-catalog"></a>De tenantconfiguratie van de in de catalogus synchroniseren

In deze taak start u een proces voor het synchroniseren van de configuratie van de servers, elastische pools en databases die in de tenant-catalogus. Deze informatie wordt later gebruikt voor het configureren van een omgeving spiegelbeeld in de regio van het herstel.

> [!IMPORTANT]
> Ter vereenvoudiging zijn van het synchronisatieproces en andere langlopende herstel en repatriëring processen geïmplementeerd in deze voorbeelden als lokale PowerShell-taken of sessies die worden uitgevoerd onder uw client-gebruikersaanmelding. De verificatietokens uitgegeven wanneer u zich aanmeldt vervalt na enkele uren, en vervolgens de taken mislukken. In een productiescenario moeten langlopende processen worden geïmplementeerd als betrouwbare Azure-services type, uitgevoerd onder een service-principal. Zie [gebruik Azure PowerShell een service-principal maken met een certificaat](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal). 

1. Open het bestand van de Modules\UserConfig.psm1 ...\Learning in de PowerShell ISE. Vervang `<resourcegroup>` en `<user>` op regels 10 en 11 met de waarde die wordt gebruikt wanneer u de app hebt geïmplementeerd. Sla het bestand op.

2. Open de ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 script in de PowerShell ISE.

    In deze zelfstudie maakt u de scenario's in deze PowerShell-script uitvoeren zodat dit bestand open houden.

3. Stelt u het volgende:

    $DemoScenario = 1: start een achtergrondtaak die wordt gesynchroniseerd tenant-server en configuratiegegevens over opslaggroepen in de catalogus.

4. Selecteer het synchronisatie-script wordt uitgevoerd, F5. 

    Deze informatie wordt later gebruikt om ervoor te zorgen dat herstel het spiegelbeeld van de servers, -adresgroepen en databases in de regio van het herstel maakt.  

    ![Synchronisatieproces](media/saas-dbpertenant-dr-geo-restore/sync-process.png)

Het PowerShell-venster op de achtergrond uitgevoerd en blijft met de rest van deze zelfstudie.

> [!NOTE]
> Het synchronisatieproces verbindt met de catalogus via een DNS-alias. De alias is gewijzigd tijdens terugzetten en repatriëring om te verwijzen naar de actieve catalogus. Het synchronisatieproces houdt de catalogus bijgewerkt met de database of een groep configuratiewijzigingen aangebracht in de regio van het herstel. Tijdens repatriëring, worden deze wijzigingen worden toegepast op de gelijkwaardige resources in de oorspronkelijke regio.

## <a name="geo-restore-recovery-process-overview"></a>Overzicht van het herstel geo-herstel

Het herstelproces geo-restore implementeert de toepassing en databases herstelt vanuit back-ups in de regio van het herstel.

Het herstelproces doet het volgende:

1. Hiermee schakelt u het Azure Traffic Manager-eindpunt voor de web-app in de oorspronkelijke regio. Het uitschakelen van het eindpunt voorkomen dat gebruikers verbinding maken met de app in een ongeldige status moet de oorspronkelijke regio on line tijdens het herstel.

2. Voorziet in een herstel de catalogusdatabase-server in het gebied voor herstel, geo herstelacties catalogus en werkt de alias activecatalog om te verwijzen naar de teruggezette catalog-server. Het wijzigen van de catalogus-alias, zorgt u ervoor dat het synchronisatieproces catalogus altijd wordt gesynchroniseerd met de catalogus van active.

3. Geeft alle bestaande tenants in de catalogus herstel als offline toegang tot wilt voorkomen tenant databases voordat deze kunnen worden hersteld.

4. Richt een exemplaar van de app in de regio van herstel en geconfigureerd voor gebruik van de herstelde catalogus in deze regio. Als u wilt behouden latentie tot een minimum te beperken, is de voorbeeld-app ontworpen altijd verbinding maken met de database van een tenant in dezelfde regio.

5. Richt een server en de elastische pool waarin nieuwe tenants zijn ingericht. Maken van deze bronnen, zorgt u ervoor dat het inrichten van nieuwe tenants niet wordt verwerkt door het herstel van bestaande tenants.

6. Updates van de nieuwe tenant alias om te verwijzen naar de server voor de nieuwe tenant-databases in de regio van het herstel. Het wijzigen van deze alias, zorgt u ervoor dat de databases voor een nieuwe tenants in de regio voor herstel zijn ingericht.
        
7. Bepalingen servers en elastische pools in het gebied herstel voor tenant databases terugzetten. Deze servers en -groepen zijn het spiegelbeeld van de configuratie in de oorspronkelijke regio. Pools vooraf inrichten reserveert de capaciteit die nodig zijn voor alle databases te herstellen.

    Een storing in een regio mogelijk aanzienlijke druk op de beschikbare bronnen in de gekoppelde regio plaatsen. Als u gebruik maakt van geo-herstel voor herstel na Noodgevallen, wordt vervolgens het reserveren van resources snel aanbevolen. Houd rekening met geo-replicatie als het is essentieel dat een toepassing in een specifieke regio is hersteld. 

8. Hiermee schakelt u het Traffic Manager-eindpunt voor de web-app in de regio van het herstel. Inschakelen van dit eindpunt, kunt de toepassing voor het inrichten van nieuwe tenants. In deze fase worden de bestaande tenants nog steeds offline.

9. Batches van aanvragen voor het herstellen van databases in volgorde van prioriteit worden verzonden. 

    * Batches zijn ingedeeld zodat databases tussen alle pools parallel worden hersteld.  

    * Herstel aanvraagt asynchroon worden verzonden, zodat ze worden verzonden snel en in de wachtrij voor uitvoering in elke groep.

    * Omdat de restore-aanvragen worden parallel verwerkt over alle toepassingen, is het beter om belangrijke tenants verdelen over veel toepassingen. 

10. Bewaakt de SQL Database-service om te bepalen wanneer databases worden hersteld. Nadat de database van een tenant is teruggezet, het online in de catalogus gemarkeerd en de som rowversion voor de tenant-database wordt geregistreerd. 

    * Tenant-databases zijn toegankelijk door de toepassing zodra ze zijn gemarkeerd in de catalogus online.

    * De som van waarden in de database van de tenant rowversion wordt opgeslagen in de catalogus. Deze som fungeert als een vingerafdruk waarmee het proces repatriëring om te bepalen of de database is bijgewerkt in de regio van het herstel.       

## <a name="run-the-recovery-script"></a>Voer het script herstel

> [!IMPORTANT]
> Deze zelfstudie herstelt databases vanuit geografisch redundante back-ups. Hoewel deze back-ups gewoonlijk beschikbaar binnen 10 minuten zijn, kan het een uur duren. Het script wordt onderbroken totdat ze beschikbaar zijn.

Stel dat er een storing in het gebied waarin de toepassing is geïmplementeerd, en voer het script voor herstel is:

1. In de PowerShell ISE, stel de volgende waarde in het ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1-script:

    $DemoScenario = 2: de app in een regio herstelpunt herstellen door terug te zetten vanuit geografisch redundante back-ups.

2. Selecteer het script wordt uitgevoerd, F5.  

    * Het script in een nieuw PowerShell-venster wordt geopend en start vervolgens een set PowerShell-taken die parallel worden uitgevoerd. Deze taken worden naar de regio herstel servers, -adresgroepen en -databases herstellen.

    * De regio van het herstel is de gekoppelde regio die is gekoppeld aan de Azure-regio waarin u de toepassing is geïmplementeerd. Zie voor meer informatie [Azure regio's gekoppeld](https://docs.microsoft.com/azure/best-practices-availability-paired-regions). 

3. De status van het herstelproces in de PowerShell-venster bewaken.

    ![Herstelproces](media/saas-dbpertenant-dr-geo-restore/dr-in-progress.png)

> [!NOTE]
> Bekijk de PowerShell-scripts in de map van Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-RestoreFromBackup\RecoveryJobs ...\Learning de code voor de hersteltaken om's te bekijken.

## <a name="review-the-application-state-during-recovery"></a>Controleer de status van de toepassing tijdens het herstel
Het toepassingseindpunt is uitgeschakeld in Traffic Manager, is de toepassing niet beschikbaar. De catalogus is hersteld, en alle huurders offline zijn gemarkeerd. Het toepassingseindpunt in de regio recovery wordt ingeschakeld en de toepassing weer online is. Hoewel de toepassing beschikbaar is, tenants offline worden weergegeven in de hub gebeurtenissen totdat hun databases worden hersteld. Het is belangrijk voor het ontwerpen van uw toepassing om af te handelen offline tenant-databases.

* Nadat de catalogusdatabase is hersteld, maar voordat de tenants weer online zijn, vernieuwt u de hub Wingtip Tickets gebeurtenissen in uw webbrowser.

    * U ziet dat de naam van de catalogus nu heeft in de voettekst - herstel achtervoegsel en bevindt zich in de regio van het herstel.

    * U ziet dat tenants die nog niet zijn hersteld zijn gemarkeerd als offline, en niet kunnen geselecteerd worden.   
 
    ![Herstelproces](media/saas-dbpertenant-dr-geo-restore/events-hub-tenants-offline-in-recovery-region.png)    

    * Als u een tenant gebeurtenissen pagina rechtstreeks opent terwijl de tenant offline is, verschijnt de pagina een offline melding van de tenant. Bijvoorbeeld, als Contoso overleg Hall offline is, probeert te openen http://events.wingtip-dpt.&lt; gebruiker&gt;.trafficmanager.net/contosoconcerthall.

    ![Herstelproces](media/saas-dbpertenant-dr-geo-restore/dr-in-progress-offline-contosoconcerthall.png)

## <a name="provision-a-new-tenant-in-the-recovery-region"></a>Een nieuwe tenant in de regio herstel inrichten
Zelfs voordat tenant databases worden teruggezet, kunt u nieuwe tenants in de regio herstel inrichten. Nieuwe tenant databases in de regio herstel ingericht zijn besluit zijn verricht met de herstelde databases later.   

1. In de PowerShell ISE, in het ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 script stelt u de volgende eigenschap:

    $DemoScenario = 3: een nieuwe tenant in de regio herstel inrichten.

2. Selecteer het script wordt uitgevoerd, F5.

3. De pagina Hawthorn Hall gebeurtenissen wordt geopend in de browser bij het inrichten is voltooid. 

    U ziet dat de database Hawthorn Hall bevindt zich in de regio van het herstel.

    ![Hawthorn Hall ingericht in de regio herstel](media/saas-dbpertenant-dr-geo-restore/hawthorn-hall-provisioned-in-recovery-region.png)

4. In de browser de Wingtip Tickets gebeurtenissen hub pagina vernieuwen om te zien dat hawthorn Hall opgenomen. 

    Als u Hawthorn Hall ingericht zonder te wachten op voor de andere tenants om terug te zetten, andere tenants nog steeds mogelijk offline.

## <a name="review-the-recovered-state-of-the-application"></a>Controleer de herstelde status van de toepassing

Wanneer het herstelproces is voltooid, zijn de toepassing en alle tenants volledig functioneel in de regio van het herstel. 

1. Nadat het scherm in het venster PowerShell-console dat de tenants worden hersteld geeft, vernieuw de hub gebeurtenissen. 

    De tenants alle weergegeven online, inclusief de nieuwe tenant, Hawthorn Hall.

    ![herstelde en nieuwe tenants in de hub gebeurtenissen](media/saas-dbpertenant-dr-geo-restore/events-hub-with-hawthorn-hall.png)

2. Klik op Contoso overleg Hall en open de pagina gebeurtenissen. 

    U ziet dat de database zich op de herstelserver zich in de regio van het herstel bevindt in de voettekst.

    ![Contoso in de regio herstel](media/saas-dbpertenant-dr-geo-restore/contoso-recovery-location.png)

3. In de [Azure-portal](https://portal.azure.com), opent u de lijst met resourcegroepen.  

    U ziet de resourcegroep die u hebt geïmplementeerd, plus de herstel-bron groeperen met het achtervoegsel voor de - herstel. De resourcegroep herstel bevat alle resources die zijn gemaakt tijdens het herstelproces plus nieuwe resources die zijn gemaakt tijdens de onderbreking. 

4. Open de resourcegroep van herstel en Let op de volgende items:

    * De versies van het herstel van de catalogus en tenants1-servers, met het achtervoegsel voor de - herstel. De herstelde catalogus en tenant-databases op deze alle servers hebben de namen in de oorspronkelijke regio gebruikt.

    * De tenants2-dpt -&lt;gebruiker&gt;-herstel SQL server. Deze server wordt gebruikt voor het inrichten van nieuwe tenants tijdens de onderbreking.

    * App service met de naam events-wingtip-dpt -&lt;recoveryregion&gt;-&lt;gebruiker&gt;, dit is het exemplaar van het herstel van de app gebeurtenissen.

    ![Contoso bronnen in de regio herstel](media/saas-dbpertenant-dr-geo-restore/resources-in-recovery-region.png) 
    
5. Open de tenants2-dpt -&lt;gebruiker&gt;-herstel SQL server. U ziet dat deze de hawthornhall database en de elastische groep Pool1 bevat. De database hawthornhall is geconfigureerd als een elastische database in de elastische groep Pool1.

## <a name="change-the-tenant-data"></a>De tenant-gegevens wijzigen 
In deze taak bijwerken u een van de herstelde tenant-databases. Het proces repatriëring kopieën hersteld databases die zijn gewijzigd naar de oorspronkelijke regio. 

1. In uw browser de lijst met gebeurtenissen vinden voor de Contoso overleg Hall, blader door de gebeurtenissen en Let op de laatste gebeurtenis, ernstig Strauss.

2. In de PowerShell ISE, stel de volgende waarde in het ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1-script:

    $DemoScenario = 4: een gebeurtenis verwijderen uit een tenant in de regio van het herstel.

3. Selecteer voor het uitvoeren van het script F5.

4. Vernieuw de pagina van de gebeurtenissen Contoso overleg Hall (http://events.wingtip-dpt.&lt; gebruiker&gt;.trafficmanager.net/contosoconcerthall), en u ziet dat de gebeurtenis ernstig Strauss ontbreekt.

Op dit moment in de zelfstudie hebt u de toepassing, wat wordt nu uitgevoerd in de regio herstel hersteld. U hebt een nieuwe tenant in de regio herstel ingericht en gegevens van een van de herstelde tenants gewijzigd.  

> [!NOTE]
> Andere zelfstudies in de steekproef zijn niet ontworpen om uit te voeren met de app in de herstelstatus. Als u verkennen andere zelfstudies wilt, zorg er dan voor dat de toepassing eerst repatriëren.

## <a name="repatriation-process-overview"></a>Overzicht van het proces repatriëring

Het proces repatriëring wordt de toepassing en de databases naar de oorspronkelijke regio teruggezet na een storing is opgelost.

![Geo-restore repatriëring](media/saas-dbpertenant-dr-geo-restore/geo-restore-repatriation.png) 

Het proces:

1. Elke activiteit lopende terugzetten stopt en openstaande of onderweg database terugzetten verzoeken annuleert.

2. Opnieuw activeren in de oorspronkelijke regio tenant databases die niet zijn gewijzigd sinds de storing. Deze databases zijn die nog niet is hersteld en die worden hersteld, maar niet later worden gewijzigd. De databases opnieuw geactiveerde zijn exact als laatste toegankelijk is voor hun tenants.

3. Richt een spiegelbeeld van de nieuwe tenant-server en de elastische pool in de oorspronkelijke regio. Nadat deze actie voltooid is, wordt de nieuwe tenant alias bijgewerkt om te verwijzen naar deze server. Bijwerken van de alias zorgt ervoor dat de nieuwe tenant voorbereiden om te worden uitgevoerd in de oorspronkelijke regio in plaats van de regio van het herstel.

3. Geo-replicatie gebruikt voor het verplaatsen van de catalogus voor de oorspronkelijke regio van het herstel de regio.

4. Updates van toepassingen is geconfigureerd in de oorspronkelijke regio zodat deze consistent zijn met wijzigingen die zijn aangebracht in de regio herstel tijdens de onderbreking.

5. De vereiste servers en toepassingen voor het hosten van nieuwe databases gemaakt tijdens de onderbreking maakt.

6. Maakt gebruik van geo-replicatie repatriëren hersteld tenant databases die zijn bijgewerkt na herstellen en alle nieuwe tenant databases tijdens de onderbreking is ingericht. 

7. Ruimt resources in de regio van het herstelpunt is gemaakt tijdens het terugzetten.

Beperk het aantal tenant-databases die moeten worden besluit zijn verricht, worden stap 1 tot 3 onmiddellijk uitgevoerd.  

Stap 4 wordt alleen uitgevoerd als de catalogus in de regio recovery is gewijzigd tijdens de onderbreking. De catalogus wordt bijgewerkt als nieuwe tenants worden gemaakt of als een database of een groep-configuratie wordt gewijzigd in de regio van het herstel.

Het is belangrijk dat de stap 7 zorgt ervoor minimaal ongemak tenants dat en gegevens niet verloren. Het proces gebruikt voor dit doel te bereiken, geo-replicatie.

Voordat elke database geogerepliceerde is, wordt de bijbehorende database in de oorspronkelijke regio verwijderd. De database in de regio van het herstel is vervolgens geogerepliceerde, het maken van een secundaire replica in de oorspronkelijke regio. Nadat de replicatie is voltooid, de tenant is gemarkeerd als offline in de catalogus, die wordt verbroken verbindingen met de database in de regio van het herstel. De database vervolgens failover wordt uitgevoerd, waardoor in behandeling zijnde transacties worden verwerkt op de secundaire dus geen gegevens verloren. 

Failover, worden de databaserollen omgekeerd. De secundaire in de oorspronkelijke regio, wordt de primaire alleen-lezen database en de database in de regio van het herstel wordt een secundaire alleen-lezen. De tenant-vermelding in de catalogus wordt bijgewerkt om te verwijzen naar de database in de oorspronkelijke regio en de tenant is gemarkeerd als online. Op dit moment is repatriëring van de database voltooid. 

Toepassingen worden geschreven met Pogingslogica om ervoor te zorgen dat ze automatisch opnieuw verbinding maken wanneer er verbindingen zijn verbroken. Wanneer ze de catalogus gebruiken voor het opnieuw verbinden broker, ze verbinding maken met de repatriated database in de oorspronkelijke regio. Hoewel de korte disconnect vaak niet opgemerkt is, kunt u kiezen om het repatriëren databases buiten kantooruren.

Nadat u een database is besluit zijn verricht, kan de secundaire database in de regio van het herstel kan worden verwijderd. De database in de oorspronkelijke regio is vervolgens opnieuw afhankelijk van geo-herstel voor DR-beveiliging.

In stap 8 worden resources in het gebied van herstel, met inbegrip van de herstel-servers en toepassingen, verwijderd.

## <a name="run-the-repatriation-script"></a>Voer het script repatriëring
Stel, we de onderbreking is opgelost en voer het script repatriëring.

Als u de zelfstudie hebt gevolgd, het script onmiddellijk wordt opnieuw geactiveerd Fabrikam Jazz vereniging en kornoelje Dojo in de oorspronkelijke regio omdat ze niet gewijzigd. Het repatriates vervolgens de nieuwe tenant Hawthorn Hall en Contoso overleg Hall omdat deze is gewijzigd. Het script repatriates ook de catalogus is bijgewerkt wanneer Hawthorn Hall is ingericht.
  
1. In de PowerShell ISE, in de ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 script, Controleer of het synchronisatieproces van catalogus is nog steeds in de PowerShell-sessie. Indien nodig, deze opnieuw starten door in te stellen:

    $DemoScenario = 1: begint met het synchroniseren van tenant-server, toepassingen en informatie over de configuratie van de database in de catalogus.

    Selecteer het script wordt uitgevoerd, F5.

2.  Stel vervolgens de volgende opdracht voor het starten van het proces repatriëring:

    $DemoScenario = 5: de app in de oorspronkelijke regio repatriëren.

    Selecteer het herstelpunt script uitgevoerd in een nieuw PowerShell-venster, F5. Repatriëring duurt enkele minuten en in het venster PowerShell kan worden bewaakt.

3. Terwijl het script wordt uitgevoerd, vernieuw de pagina van de hub gebeurtenissen (http://events.wingtip-dpt.&lt; gebruiker&gt;. trafficmanager.net).

    U ziet dat alle huurders online is en toegankelijk tijdens dit proces.

4. Selecteer de vereniging van Fabrikam Jazz om dit te openen. Als u deze tenant niet wijzigt, zoals u ziet in de voettekst de server is al teruggedraaid naar de oorspronkelijke server.

5. Open of vernieuw de pagina Contoso overleg Hall gebeurtenissen. U ziet in de voettekst of, in eerste instantie is de database nog steeds op de - server is. 

6. Vernieuw de pagina Contoso overleg Hall gebeurtenissen wanneer het proces repatriëring is voltooid en u ziet dat de database zich nu in uw oorspronkelijke regio.

7. Vernieuw de hub gebeurtenissen opnieuw en open Hawthorn Hall. U ziet dat de database bevindt zich ook in de oorspronkelijke regio. 

## <a name="clean-up-recovery-region-resources-after-repatriation"></a>Herstel regio resources opschonen na repatriëring
Nadat repatriëring voltooid is, wordt het veilig verwijderen van de resources in de regio van het herstel. 

> [!IMPORTANT]
> Deze resources zo spoedig mogelijk als u wilt stoppen alle facturering voor deze verwijdert.

Het herstelproces wordt gemaakt van alle herstel-resources in een resourcegroep voor herstel. Het opruimproces worden deze resourcegroep verwijderd en verwijdert u alle verwijzingen naar de bronnen uit de catalogus. 

1. In de PowerShell ISE, in het ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 script instellen:
    
    $DemoScenario = 6: verouderd resources verwijderen uit de regio van het herstel.

2. Selecteer het script wordt uitgevoerd, F5.

Na het opruimen van de scripts, is de toepassing weer terug waar deze is gestart. Op dit moment kunt u het script opnieuw uitvoert of andere zelfstudies uitproberen.

## <a name="designing-the-application-to-ensure-that-the-app-and-the-database-are-co-located"></a>De toepassing om ervoor te zorgen dat de app en de database CO-locaties zijn ontwerpen 
De toepassing is ontworpen om altijd verbinding te maken van een exemplaar in dezelfde regio bevinden als de tenant-database. Dit ontwerp vermindert de latentie tussen de toepassing en de database. Deze optimalisatie wordt ervan uitgegaan dat de app naar database interactie chattier dan de interactie app gebruiker.  

Tenant-databases kunnen worden verdeeld over herstel en oorspronkelijke regio's voor enige tijd tijdens repatriëring. De app zoekt de regio waarin de database bevindt zich een DNS-zoekopdracht op de naam van de tenant als volgt voor elke database. In SQL-Database is de naam van de server een alias. De naam van de alias bevat de regionaam. Als de toepassing niet in dezelfde regio bevinden als de database, wordt hij omgeleid naar het exemplaar in dezelfde regio bevinden als de databaseserver. Omleiden naar het exemplaar in dezelfde regio bevinden als de database minimaliseert latentie tussen de app en de database.  

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:
> [!div class="checklist"]

>* Gebruik de catalogus tenant wilt regelmatig vernieuwd configuratiegegevens, waardoor een spiegelbeeld Herstelomgeving worden gemaakt in een andere regio.
>* Azure SQL-databases in de regio herstelpunt herstellen met behulp van geo-herstel.
>* De tenant-catalogus naar aanleiding van herstelde tenant databaselocaties bijwerken. 
>* Een DNS-alias gebruiken om in te schakelen van een toepassing verbinding maken met de catalogus van de tenant in de gehele zonder herconfiguratie.
>* Geo-replicatie gebruiken om te repatriëren herstelde databases naar hun oorspronkelijke regio na een storing opgelost is.

Probeer de [herstel na noodgevallen voor een multitenant SaaS-toepassing met behulp van de database geo-replicatie](saas-dbpertenant-dr-geo-replication.md) zelfstudie voor informatie over het gebruik van geo-replicatie in aanzienlijk minder tijd nodig voor het herstellen van een grootschalige multitenant-toepassing.

## <a name="additional-resources"></a>Aanvullende resources

[Aanvullende zelfstudies waarin voort op de Wingtip SaaS-toepassing bouwen](https://docs.microsoft.com/azure/sql-database/sql-database-wtp-overview#sql-database-wingtip-saas-tutorials)
