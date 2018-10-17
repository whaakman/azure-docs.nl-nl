---
title: 'SaaS-apps: Azure SQL Database geografisch redundante back-ups voor herstel na noodgevallen | Microsoft Docs'
description: Meer informatie over het gebruik van geografisch redundante back-ups van Azure SQL Database om te herstellen van een multitenant SaaS-app na een storing
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: AyoOlubeko
ms.author: ayolubek
ms.reviewer: sstein
manager: craigg
ms.date: 10/15/2018
ms.openlocfilehash: a78632ed6215c467f53938569621cfb18f9e51ca
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49352928"
---
# <a name="use-geo-restore-to-recover-a-multitenant-saas-application-from-database-backups"></a>Geo-herstel met een multitenant SaaS-toepassing herstellen vanuit back-ups

Deze zelfstudie worden een volledige noodherstelscenario voor een multitenant SaaS-toepassing geïmplementeerd met de database per tenant-model. U gebruikt [geo-herstel](https://docs.microsoft.com/azure/sql-database/sql-database-recovery-using-backups) om te herstellen van de catalogus en tenant-databases van automatisch onderhouden geografisch redundante back-ups in een andere regio. Nadat de onderbreking opgelost is, gebruikt u [geo-replicatie](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview) te repatriëren gewijzigde databases naar hun oorspronkelijke regio.

![Geo-restore-architectuur](media/saas-dbpertenant-dr-geo-restore/geo-restore-architecture.png)

Geo-restore is de oplossing voor noodherstel van de laagste kosten voor Azure SQL Database. Echter, terugzetten vanuit geografisch redundante back-ups kan leiden tot verlies van gegevens van maximaal één uur. Het kan veel tijd, afhankelijk van de grootte van elke database duren. 

> [!NOTE]
> Toepassingen met de laagst mogelijke RPO en de RTO herstellen met behulp van geo-replicatie in plaats van geo-herstel.

Deze zelfstudie worden zowel terugzetten en repatriëring werkstromen. In deze zelfstudie leert u procedures om het volgende te doen:
> [!div class="checklist"]

>* Databases en elastische groep configuratie-informatie in de tenantcatalogus synchroniseren.
>* Instellen van een omgeving spiegelbeeld in een herstelregio met toepassingen, servers en toepassingen.   
>* Catalogus en tenant-databases herstellen met behulp van geo-herstel.
>* Gebruik geo-replicatie voor de tenant catalog en het gewijzigde tenantdatabases repatriëren nadat de onderbreking verholpen is.
>* Catalogus bijwerken wanneer elke database wordt hersteld (of besluit zijn verricht) om bij te houden van de huidige locatie van het actieve exemplaar van de database van elke tenant.
>* Zorg ervoor dat de toepassing en de tenant-database altijd dezelfde zich in dezelfde Azure-regio te verminderen latentie. 
 

Voordat u deze zelfstudie begint, moet u de volgende vereisten voldoen:
* De Wingtip Tickets SaaS-database per tenant-app implementeren. Als u wilt implementeren in minder dan vijf minuten, Zie [implementeren en verkennen van de Wingtip Tickets SaaS-database per tenant toepassing](saas-dbpertenant-get-started-deploy.md). 
* Installeer Azure PowerShell. Zie voor meer informatie, [aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-the-geo-restore-recovery-pattern"></a>Inleiding tot de herstelpatroon geo-herstel

Herstel na noodgeval (DR) is een belangrijk aandachtspunt voor veel toepassingen, of om wettelijke redenen of zakelijke continuïteit. Als er een langdurige serviceonderbreking, kan een goed is voorbereid plan voor herstel na Noodgevallen bedrijfsonderbreking kunt minimaliseren. Een DR-plan op basis van geo-herstel moet verschillende doelstellingen uitvoeren:
 * Alle benodigde capaciteit in de regio van de gekozen herstelsite zo snel mogelijk om ervoor te zorgen dat deze beschikbaar zijn om te herstellen van tenantdatabases is gereserveerd.
 * Stel een omgeving voor het herstel van spiegelbeeld die overeenkomt met de oorspronkelijke configuratie van de groep van toepassingen en -database. 
 * Annulering van het herstelproces halverwege vlucht toestaan als de oorspronkelijke regio weer online komt.
 * Schakel tenants worden ingericht snel, zodat de nieuwe tenant-onboarding zo snel mogelijk opnieuw kunt starten.
 * Als u wilt herstellen van tenants in volgorde van prioriteit worden geoptimaliseerd.
 * Om op te halen tenants online zo snel mogelijk door te voeren stappen parallel waar praktische met worden geoptimaliseerd.
 * Zijn tegen storingen, opnieuw starten, en idempotent zijn.
 * Repatriëren databases naar hun oorspronkelijke regio met minimale gevolgen voor tenants wanneer de onderbreking is verholpen.  

> [!NOTE]
> De toepassing wordt hersteld naar de gekoppelde regio van de regio waarin de toepassing is geïmplementeerd. Zie voor meer informatie, [gekoppelde Azure-regio's](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).   

In deze zelfstudie maakt gebruik van functies van Azure SQL Database en het Azure-platform om deze uitdagingen:

* [Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template), zo snel mogelijk alle benodigde capaciteit reserveren. Azure Resource Manager-sjablonen worden gebruikt voor het inrichten van een mirror-installatiekopie van de oorspronkelijke servers en elastische pools in de herstelregio voor. Een afzonderlijke server en de pool worden ook gemaakt voor het inrichten van nieuwe tenants.
* [Clientbibliotheek voor elastic Database](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-client-library) (EDCL) maken en onderhouden van een tenantcatalogus voor de database. De uitgebreide catalog bevat regelmatig vernieuwd pool- en configuratie-informatie.
* [Functies van shard management gegevensherstel](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-recovery-manager) van de EDCL, databasevermeldingen die locatie in de catalogus tijdens het herstel en repatriëring onderhouden.  
* [Geo-herstel](https://docs.microsoft.com/azure/sql-database/sql-database-disaster-recovery), om te herstellen van de catalogus en tenant-databases van automatisch onderhouden geografisch redundante back-ups. 
* [Asynchrone herstelbewerkingen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations), in volgorde van prioriteit tenant verzonden, worden door het systeem voor elke groep in de wachtrij en verwerkt in batches, zodat de groep niet wordt overbelast. Deze bewerkingen kunnen worden geannuleerd vóór of tijdens het uitvoeren van indien nodig.   
* [Geo-replicatie](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview), te repatriëren databases naar de oorspronkelijke regio na de storing. Er is geen verlies van gegevens en minimale gevolgen voor de tenant wanneer u geo-replicatie gebruikt.
* [SQL server-DNS-aliassen](https://docs.microsoft.com/azure/sql-database/dns-alias-overview), waarmee het synchronisatieproces catalogus verbinding maken met de actieve catalogus, ongeacht de locatie.  

## <a name="get-the-disaster-recovery-scripts"></a>Scripts voor herstel van het noodgeval ophalen

De DR-scripts die worden gebruikt in deze zelfstudie zijn beschikbaar in de [Wingtip Tickets SaaS-database per tenant GitHub-opslagplaats](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant). Bekijk de [algemene richtlijnen](saas-tenancy-wingtip-app-guidance-tips.md) voor stappen voor het downloaden en de blokkering opheffen van de Wingtip Tickets-management-scripts.

> [!IMPORTANT]
> Net als alle Wingtip Tickets management scripts, de DR-scripts zijn voorbeeld kwaliteit en niet moet worden gebruikt in de productieomgeving.

## <a name="review-the-healthy-state-of-the-application"></a>Controleer de status in orde van de toepassing
Voordat u het herstelproces start, controleert u de normale, gezonde status van de toepassing.

1. Open in uw webbrowser de Wingtip Tickets events hub (http://events.wingtip-dpt.&lt; gebruiker&gt;. trafficmanager.net, Vervang &lt;gebruiker&gt; met de waarde van de gebruiker van uw implementatie).
    
   Ga naar de onderkant van de pagina en ziet u de naam van de catalogus-server en de locatie in de voettekst. De locatie is de regio waarin u de app hebt geïmplementeerd.    

   > [!TIP]
   > Beweeg de muisaanwijzer over de locatie voor het vergroten van de weergave.

   ![Gebeurtenissen hub in orde in oorspronkelijke regio](media/saas-dbpertenant-dr-geo-restore/events-hub-original-region.png)

2. Selecteer de tenant Contoso Concert Hall en open de pagina van de gebeurtenis.

   In de voettekst, ziet u de naam van de server van de tenant. De locatie is hetzelfde als de locatie van de catalogusserver.

   ![Contoso Concert Hall oorspronkelijke regio](media/saas-dbpertenant-dr-geo-restore/contoso-original-location.png) 

3. In de [Azure-portal](https://portal.azure.com), Controleer en opent u de resourcegroep waarin u de app hebt geïmplementeerd.

   U ziet de resources en de regio waarin de onderdelen van app service en SQL Database-servers worden geïmplementeerd.

## <a name="sync-the-tenant-configuration-into-the-catalog"></a>De configuratie van de tenant in de catalogus synchroniseren

In deze taak start u een proces voor het synchroniseren van de configuratie van de servers, elastische pools en databases in de tenantcatalogus. Deze informatie wordt later een omgeving spiegelbeeld configureren in de herstelregio gebruikt.

> [!IMPORTANT]
> Het synchronisatieproces en het herstel van andere langlopende en repatriëring processen voor het gemak zijn geïmplementeerd in deze voorbeelden als lokale PowerShell-taken of sessies die worden uitgevoerd onder uw client-gebruikersaanmelding. De verificatietokens uitgegeven wanneer u zich verlopen na enkele uren en de taken mislukken vervolgens. In een productiescenario voor, moeten de langlopende processen worden geïmplementeerd als betrouwbare Azure-services van een of andere, die wordt uitgevoerd onder een service-principal. Zie [gebruik Azure PowerShell een service-principal maken met een certificaat](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal). 

1. Open het bestand van de Modules\UserConfig.psm1 ...\Learning in de PowerShell ISE. Vervang `<resourcegroup>` en `<user>` op regels 10 en 11 met de waarde die wordt gebruikt tijdens de implementatie van de app. Sla het bestand op.

2. Open de ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 script in de PowerShell ISE.

    In deze zelfstudie moet u elk van de scenario's in dit PowerShell-script wordt uitgevoerd, dus houd dit bestand geopend.

3. Stelt u het volgende:

    $DemoScenario = 1: start een achtergrondtaak die worden gesynchroniseerd met tenantserver en informatie over de configuratie van de groep van toepassingen in de catalogus.

4. Selecteer het synchronisatie-script wordt uitgevoerd, F5. 

    Deze informatie wordt later gebruikt om ervoor te zorgen dat recovery het spiegelbeeld van de servers, pools en databases in de herstelregio voor maakt.  

    ![Synchronisatieproces](media/saas-dbpertenant-dr-geo-restore/sync-process.png)

Laat u het PowerShell-venster op de achtergrond uitgevoerd en gaat u verder met de rest van deze zelfstudie.

> [!NOTE]
> Het synchronisatieproces verbindt met de catalogus via een DNS-alias. De alias is gewijzigd tijdens het terugzetten en repatriëring om te verwijzen naar de catalogus van active. Het synchronisatieproces houdt de catalogus up-to-date met de database of pool configuratiewijzigingen aangebracht in de herstelregio voor. Tijdens de repatriëring, worden deze wijzigingen worden toegepast op de gelijkwaardige resources in de oorspronkelijke regio.

## <a name="geo-restore-recovery-process-overview"></a>Overzicht van geo-herstel het herstelproces

Het herstelproces geo-herstel implementeert de toepassing en databases herstellen vanuit back-ups in de herstelregio voor.

Het herstelproces doet het volgende:

1. Hiermee schakelt u de Azure Traffic Manager-eindpunt voor de web-app in de oorspronkelijke regio. Uitschakelen van het eindpunt wordt voorkomen dat gebruikers verbinding maken met de app in een ongeldige status moet de oorspronkelijke regio online is gekomen tijdens het herstel.

2. Bepalingen een herstel-server in de herstelregio, geo-herstellen van de catalogusdatabase catalogus en updates van de alias activecatalog om te verwijzen naar de herstelde catalog-server. Wijzigen van de alias van de catalogus, zorgt u ervoor dat het synchronisatieproces catalogus altijd wordt gesynchroniseerd met de actieve catalogus.

3. Markeert alle bestaande tenants in de catalogus herstellen als offline als u wilt voorkomen dat toegang tot de tenant-databases, voordat deze kunnen worden hersteld.

4. Een exemplaar van de app in de herstelregio ingericht en geconfigureerd voor het gebruik van de herstelde catalogus in die regio. De voorbeeld-app is ontworpen om te voorkomen dat latentie tot een minimum te beperken, altijd verbinding maken met een tenantdatabase in dezelfde regio.

5. Richt een server en een elastische pool waarin nieuwe tenants worden ingericht. Het maken van deze resources, zorgt u ervoor dat nieuwe tenants inrichten niet leiden tot met het herstel van bestaande tenants problemen.

6. Hiermee worden de nieuwe tenant alias om te verwijzen naar de server voor de nieuwe tenant-databases in de herstelregio bijgewerkt. Deze alias te wijzigen, zorgt u ervoor dat de databases voor alle nieuwe tenants worden ingericht in de herstelregio voor.
        
7. Bepaalde servers en elastische pools in de herstelregio voor het herstellen van tenantdatabases. Deze servers en toepassingen zijn het spiegelbeeld van de configuratie in de oorspronkelijke regio. Inrichten van pools een behoudt de capaciteit die nodig zijn voor alle databases te herstellen.

    Een storing in een regio mogelijk aanzienlijke druk op de resources die beschikbaar zijn in de gekoppelde regio plaatsen. Als u zich op geografisch herstel voor herstel na Noodgevallen baseert, wordt klikt u vervolgens snel reserveren resources aanbevolen. Houd rekening met geo-replicatie als het is essentieel dat een toepassing in een bepaalde regio is hersteld. 

8. Hiermee kunt het Traffic Manager-eindpunt voor de web-app in de herstelregio voor. Dit eindpunt inschakelt, kan de toepassing voor het inrichten van nieuwe tenants. Bestaande tenants worden in deze fase nog steeds offline.

9. Batches van aanvragen voor het herstellen van databases in volgorde van prioriteit worden verzonden. 

    * Batches zijn ingedeeld zodat databases gelijktijdig in alle pools worden hersteld.  

    * Herstel aanvraagt asynchroon worden verzonden, zodat ze zijn ingediend snel en in de wachtrij voor uitvoering in elke groep geplaatst.

    * Omdat de restore-aanvragen worden parallel verwerkt op alle groepen, is het beter om belangrijke tenants verdelen over veel groepen. 

10. Bewaakt de service SQL Database om te bepalen wanneer databases worden hersteld. Nadat een tenantdatabase wordt hersteld, het online in de catalogus gemarkeerd en een som rowversion voor de tenant-database wordt vastgelegd. 

    * Tenant-databases kunnen worden geopend door de toepassing zodra ze online beschikbaar in de catalogus zijn gemarkeerd.

    * Som van rowversion waarden in de tenant-database is opgeslagen in de catalogus. Dit bedrag fungeert als een vingerafdruk waarmee het proces repatriëring om te bepalen als de database is bijgewerkt in de herstelregio voor.       

## <a name="run-the-recovery-script"></a>Voer het herstelscript

> [!IMPORTANT]
> In deze zelfstudie worden de databases hersteld vanuit geografisch redundante back-ups. Hoewel deze back-ups binnen 10 minuten doorgaans beschikbaar zijn, kan dit tot een uur duren. Het script wordt onderbroken totdat ze beschikbaar zijn.

Stel dat er is een onderbreking in de regio waarin de toepassing is geïmplementeerd, en voer het herstelscript:

1. Stel de volgende waarde in het ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 script in de PowerShell ISE:

    $DemoScenario = 2: de app in een regio voor recovery herstellen door te herstellen vanuit geografisch redundante back-ups.

2. Selecteer het script wordt uitgevoerd, F5.  

    * Het script wordt geopend in een nieuwe PowerShell-venster en start vervolgens een set PowerShell-taken die parallel worden uitgevoerd. Deze taken herstellen-servers, pools en databases naar de herstelregio.

    * De herstelregio is de gekoppelde regio die is gekoppeld aan de Azure-regio waarin u de toepassing is geïmplementeerd. Zie voor meer informatie, [gekoppelde Azure-regio's](https://docs.microsoft.com/azure/best-practices-availability-paired-regions). 

3. Controleer de status van het herstelproces in het PowerShell-venster.

    ![Herstelproces](media/saas-dbpertenant-dr-geo-restore/dr-in-progress.png)

> [!NOTE]
> Als u wilt verkennen de code voor de recovery-taken, de PowerShell-scripts in de map van de ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-RestoreFromBackup\RecoveryJobs bekijken

## <a name="review-the-application-state-during-recovery"></a>Controleer de status van de toepassing tijdens het herstel
Terwijl het toepassingseindpunt van de is uitgeschakeld in Traffic Manager, wordt de toepassing niet beschikbaar is. De catalogus is hersteld, en alle tenants offline zijn gemarkeerd. Het toepassingseindpunt in de herstelregio wordt ingeschakeld en de toepassing weer online is. Hoewel de toepassing beschikbaar is, weergegeven als tenants offline in de events hub totdat hun databases worden hersteld. Het is belangrijk om uw toepassing om af te handelen offline tenantdatabases te ontwerpen.

* Nadat de catalogusdatabase is hersteld, maar voordat de tenants weer online zijn, vernieuwt u de Wingtip Tickets events hub in uw webbrowser.

    * U ziet dat de naam van de catalogus nu heeft in de voettekst een - recovery-achtervoegsel en bevindt zich in de herstelregio voor.

    * U ziet dat tenants die nog niet zijn hersteld zijn gemarkeerd als offline en niet worden geselecteerd zijn.   
 
    ![Herstelproces](media/saas-dbpertenant-dr-geo-restore/events-hub-tenants-offline-in-recovery-region.png)    

    * Als u van een tenant gebeurtenissen pagina rechtstreeks openen terwijl de tenant offline is is bereikt, wordt de pagina een tenant offline-melding weergegeven. Bijvoorbeeld, als Contoso Concert Hall offline is, probeert te openen http://events.wingtip-dpt.&lt; gebruiker&gt;.trafficmanager.net/contosoconcerthall.

    ![Herstelproces](media/saas-dbpertenant-dr-geo-restore/dr-in-progress-offline-contosoconcerthall.png)

## <a name="provision-a-new-tenant-in-the-recovery-region"></a>Een nieuwe tenant in de herstelregio inrichten
Zelfs nog voor tenant-databases zijn hersteld, kunt u nieuwe tenants in de herstelregio inrichten. Nieuwe tenantdatabases ingericht in de herstelregio zijn besluit zijn verricht met de herstelde databases later opnieuw.   

1. In de PowerShell ISE in het script van de Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 voor ...\Learning, stelt de volgende eigenschap:

    $DemoScenario = 3: een nieuwe tenant in de herstelregio in te richten.

2. Selecteer het script wordt uitgevoerd, F5.

3. De pagina van de gebeurtenissen Hawthorn Hall wordt geopend in de browser bij het inrichten is voltooid. 

    U ziet dat de database Hawthorn Hall bevindt zich in de herstelregio voor.

    ![Hawthorn Hall ingericht in de herstelregio](media/saas-dbpertenant-dr-geo-restore/hawthorn-hall-provisioned-in-recovery-region.png)

4. Vernieuwen de Wingtip Tickets hub-pagina als u wilt zien dat hawthorn Hall opgenomen gebeurtenissen in de browser. 

    Als u Hawthorn Hall ingericht zonder te wachten op de andere tenants om terug te zetten, kunnen andere tenants kunnen nog steeds zijn offline.

## <a name="review-the-recovered-state-of-the-application"></a>Controleer de herstelde status van de toepassing

Wanneer het herstelproces is voltooid, zijn de toepassing en alle tenants volledig functioneel in de herstelregio voor. 

1. Nadat u de weergave in de PowerShell-consolevenster geeft aan dat alle tenants worden hersteld, vernieuw de events hub. 

    De tenants die alle weergegeven online, met inbegrip van de nieuwe tenant, Hawthorn Hall.

    ![herstelde en nieuwe tenants in de events hub](media/saas-dbpertenant-dr-geo-restore/events-hub-with-hawthorn-hall.png)

2. Klik op Contoso Concert Hall en open de pagina gebeurtenissen. 

    U ziet dat de database bevindt zich op de herstelserver zich in de herstelregio in de voettekst.

    ![Contoso in de herstelregio](media/saas-dbpertenant-dr-geo-restore/contoso-recovery-location.png)

3. In de [Azure-portal](https://portal.azure.com), opent u de lijst met resourcegroepen.  

    U ziet de resourcegroep die u hebt geïmplementeerd, plus de recovery-resource-groep, met het achtervoegsel voor de - herstel. De resourcegroep van recovery bevat alle resources die zijn gemaakt tijdens het herstelproces en nieuwe resources die zijn gemaakt tijdens de storing. 

4. Open de recovery-resourcegroep en ziet u de volgende items:

    * De herstel-versies van de catalogus en tenants1-servers, met het achtervoegsel voor de - herstel. De herstelde catalogus en tenant-databases op deze alle servers hebben de namen in de oorspronkelijke regio gebruikt.

    * De tenants2-dpt -&lt;gebruiker&gt;-recovery SQL server. Deze server wordt gebruikt voor het inrichten van nieuwe tenants gedurende de storing.

    * De appservice met de naam van de gebeurtenissen-wingtip-dpt -&lt;recoveryregion&gt;-&lt;gebruiker&gt;, dit is de herstel-exemplaar van de app.

    ![Contoso-resources in de herstelregio](media/saas-dbpertenant-dr-geo-restore/resources-in-recovery-region.png) 
    
5. Open de tenants2-dpt -&lt;gebruiker&gt;-recovery SQL server. U ziet dat deze de hawthornhall database en de elastische pool Pool1 bevat. De database hawthornhall is geconfigureerd als een elastische database in de elastische pool Pool1.

## <a name="change-the-tenant-data"></a>Wijzigen van de gegevens van de tenant 
In deze taak werkt u een van de herstelde tenant-databases. Het proces repatriëring kopieën hersteld databases die zijn gewijzigd naar de oorspronkelijke regio. 

1. De lijst met gebeurtenissen vinden voor de Contoso Concert Hall, blader door de gebeurtenissen en ziet u de laatste gebeurtenis, ernstig Strauss in uw browser.

2. Stel de volgende waarde in het ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 script in de PowerShell ISE:

    $DemoScenario = 4: een gebeurtenis verwijderen uit een tenant in de herstelregio voor.

3. Selecteer F5 voor het uitvoeren van het script.

4. Vernieuw de pagina van de gebeurtenissen Contoso Concert Hall (http://events.wingtip-dpt.&lt; gebruiker&gt;.trafficmanager.net/contosoconcerthall), en u ziet dat de gebeurtenis ernstig Strauss ontbreekt.

Op dit moment in de zelfstudie hebt u de toepassing, die nu wordt uitgevoerd in de herstelregio hersteld. U hebt een nieuwe tenant in de herstelregio ingericht en gegevens van een van de herstelde tenants gewijzigd.  

> [!NOTE]
> Andere zelfstudies in het voorbeeld zijn niet ontworpen om uit te voeren met de app in de herstelstatus. Als u andere zelfstudies verkennen wilt, zorg er dan voor dat de toepassing eerst repatriëren.

## <a name="repatriation-process-overview"></a>Overzicht van het repatriëring

Het proces repatriëring wordt de toepassing en de bijbehorende databases naar de oorspronkelijke regio na een storing is opgelost.

![Geo-herstel repatriëring](media/saas-dbpertenant-dr-geo-restore/geo-restore-repatriation.png) 

Het proces:

1. Elke activiteit lopende herstel stopt en annuleert alle openstaande of die onderweg zijn database terugzetten aanvragen.

2. Opnieuw activeren in de oorspronkelijke regio tenant-databases, die niet zijn gewijzigd sinds de onderbreking. Deze databases zijn die nog niet is hersteld en die worden hersteld, maar niet later zijn gewijzigd. De databases opnieuw geactiveerde zijn exact als laatste toegankelijk door hun tenants.

3. Richt een mirror-installatiekopie van de server en een elastische pool van de nieuwe tenant in de oorspronkelijke regio. Nadat deze actie voltooid is, wordt de nieuwe tenant-alias wordt bijgewerkt om te verwijzen naar deze server. Bijwerken van de alias zorgt ervoor dat de nieuwe tenant-onboarding voorkomt in de oorspronkelijke regio in plaats van de herstelregio voor.

3. Geo-replicatie gebruikt om de catalogus naar de oorspronkelijke regio van de recovery-regio's.

4. Updates van toepassingen configureren in de oorspronkelijke regio zodat deze consistent zijn met wijzigingen die zijn aangebracht in de herstelregio gedurende de storing.

5. Hiermee maakt u de vereiste servers en toepassingen voor het hosten van alle nieuwe databases die zijn gemaakt tijdens de storing.

6. Maakt gebruik van geo-replicatie repatriëren hersteld tenant-databases die zijn bijgewerkt na herstellen en alle nieuwe tenantdatabases dat tijdens de onderbreking is ingericht. 

7. Opschonen van resources die in de herstelregio tijdens het herstelproces is gemaakt.

Stappen 1-3 worden om te beperken het nummer van de tenant-databases die moeten worden besluit zijn verricht, onmiddellijk uitgevoerd.  

Stap 4 wordt alleen uitgevoerd als de catalogus in de herstelregio gedurende de storing is gewijzigd. De catalogus wordt bijgewerkt als nieuwe tenants zijn gemaakt of als een database of pool-configuratie is gewijzigd in de herstelregio voor.

Het is belangrijk dat stap 7 zorgt ervoor minimale verstoring voor tenants dat en er zijn geen gegevens verloren gegaan zijn. Voor dit doel te bereiken, maakt het proces gebruik van geo-replicatie.

Voordat elke database geo-replicatie is, wordt de bijbehorende database in de oorspronkelijke regio wordt verwijderd. De database in de herstelregio is vervolgens geo-replicatie, het maken van een secundaire replica in de oorspronkelijke regio. Nadat de replicatie is voltooid, is de tenant in de catalogus, die alle verbindingen met de database in de herstelregio offline gemarkeerd. De database vervolgens failover wordt uitgevoerd, waardoor alle in afwachting van transacties worden verwerkt op de secundaire dus geen gegevens verloren is gegaan. 

Bij failover worden de databaserollen omgekeerd. De secundaire server in de oorspronkelijke regio, wordt de primaire alleen-lezen-database en de database in de herstelregio niet langer een secundaire alleen-lezen. De tenant-vermelding in de catalogus is bijgewerkt met verwijzingen naar de database in de oorspronkelijke regio en de tenant is gemarkeerd als online. Op dit moment is repatriëring van de database voltooid. 

Toepassingen moeten worden geschreven met de logica voor opnieuw proberen om ervoor te zorgen dat ze automatisch opnieuw verbinding maken wanneer er verbindingen zijn verbroken. Wanneer ze de catalogus gebruiken als Broker optreden voor het opnieuw verbinden, ze verbinding maken met de repatriated database in de oorspronkelijke regio. Hoewel de korte verbreken van de verbinding vaak niet opgemerkt is, wilt u mogelijk repatriëren databases buiten kantooruren.

Nadat u een database is besluit zijn verricht, kan de secundaire database in de herstelregio worden verwijderd. De database in de oorspronkelijke regio vervolgens gebruik opnieuw van geo-herstel voor DR-beveiliging.

In stap 8, worden resources in de herstelregio, met inbegrip van de recovery-servers en -groepen, verwijderd.

## <a name="run-the-repatriation-script"></a>Voer het script repatriëring
Stel de onderbreking is opgelost en voer het script repatriëring.

Als u de zelfstudie hebt gevolgd, het script onmiddellijk opnieuw geactiveerd Fabrikam Jazz Club en kornoelje Dojo in de oorspronkelijke regio omdat ze niet gewijzigd. Deze vervolgens repatriates de nieuwe tenant Hawthorn Hall en Contoso Concert Hall omdat deze is gewijzigd. Het script repatriates ook de catalogus die is bijgewerkt wanneer Hawthorn Hall is ingericht.
  
1. Controleer in de PowerShell ISE, in de ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 script, of dat het proces van synchronisatie catalogus nog steeds wordt uitgevoerd in de PowerShell-sessie. Indien nodig, start u het opnieuw door in te stellen:

    $DemoScenario = 1: start met het synchroniseren van de tenantserver-, pool- en informatie over de configuratie van de database in de catalogus.

    Selecteer het script wordt uitgevoerd, F5.

2.  Stel vervolgens de volgende opdracht voor het starten van het proces repatriëring:

    $DemoScenario = 5: de app in de oorspronkelijke regio repatriëren.

    Selecteer het herstelscript uitgevoerd in een nieuw PowerShell-venster, F5. Repatriëring duurt enkele minuten en kan worden gecontroleerd in het PowerShell-venster.

3. Terwijl het script wordt uitgevoerd, vernieuw de pagina van de hub gebeurtenissen (http://events.wingtip-dpt.&lt; gebruiker&gt;. trafficmanager.net).

    U ziet dat de Wizardoptie online en toegankelijk tijdens dit proces.

4. Selecteer de Fabrikam Jazz Club om dit te openen. Als u deze tenant is niet wijzigt, wordt de kennisgeving van de voettekst die de server al is hersteld naar de oorspronkelijke server.

5. Open of vernieuw de pagina van Contoso Concert Hall gebeurtenissen. U ziet in de voettekst of, in eerste instantie wordt de database nog steeds op de - herstelserver is. 

6. Vernieuw de pagina van de gebeurtenissen Contoso Concert Hall wanneer het proces repatriëring is voltooid en u ziet dat de database zich nu in uw oorspronkelijke regio.

7. Vernieuw de events hub opnieuw en Hawthorn Hall openen. U ziet dat de database bevindt zich ook in de oorspronkelijke regio. 

## <a name="clean-up-recovery-region-resources-after-repatriation"></a>Herstel de regio resources opschonen na repatriëring
Nadat repatriëring voltooid is, is het veilig om de resources in de herstelregio te verwijderen. 

> [!IMPORTANT]
> Deze informatiebronnen om alle facturering voor hen stoppen onmiddellijk te verwijderen.

Het herstelproces wordt gemaakt van alle herstel-resources in een resourcegroep van recovery. Het opruimproces Hiermee verwijdert u deze resourcegroep en verwijdert u alle verwijzingen naar de resources uit de catalogus. 

1. In de PowerShell ISE, in het ...\Learning Modules\Business bedrijfscontinuïteit en noodherstel Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 script instellen:
    
    $DemoScenario = 6: verouderd resources verwijderen uit de herstelregio voor.

2. Selecteer het script wordt uitgevoerd, F5.

Na het opschonen van de scripts, worden de toepassing weer is waar deze is gestart. U kunt op dit moment, voer het script opnieuw uit of probeer andere zelfstudies.

## <a name="designing-the-application-to-ensure-that-the-app-and-the-database-are-co-located"></a>Het ontwerpen van de toepassing om ervoor te zorgen dat de app en de database op de juiste CO-locaties 
De toepassing is ontworpen om altijd verbinding te maken van een instantie in dezelfde regio als de database van de tenant. Dit ontwerp vermindert de latentie tussen de toepassing en de database. Deze optimalisatie wordt ervan uitgegaan dat de app en database-interactie chattier dan de tussenkomst van de app-gebruiker is.  

Tenant-databases kunnen zijn verdeeld over de oorspronkelijke regio's en herstel voor enige tijd tijdens repatriëring. Voor elke database, wordt de app zoekt u de regio waarin de database bevindt zich aan de hand van een DNS-zoekopdracht op de naam van de tenant. In SQL-Database is de naam van de server een alias. De naam van de alias bevat de regionaam van de. Als de toepassing niet in dezelfde regio als de database, wordt hij omgeleid naar het exemplaar in dezelfde regio als de database-server. Omleiden naar het exemplaar in dezelfde regio als de database wordt geminimaliseerd latentie tussen de app en de database.  

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:
> [!div class="checklist"]

>* De tenantcatalogus gebruiken voor het opslaan van regelmatig vernieuwd configuratie-informatie, waarmee een omgeving voor het herstel van spiegelbeeld moet worden gemaakt in een andere regio.
>* Azure SQL-databases in de herstelregio herstellen met behulp van geo-herstel.
>* Update-tenantcatalogus om tenant herstelde databaselocaties weer te geven. 
>* Een DNS-alias gebruiken om in te schakelen van een toepassing verbinding maken met de tenantcatalogus in de gehele zonder herconfiguratie.
>* Gebruik geo-replicatie te repatriëren herstelde databases naar hun oorspronkelijke regio na een storing opgelost is.

Probeer de [herstel na noodgevallen voor een multitenant SaaS-toepassing met behulp van geo-replicatie van de database](saas-dbpertenant-dr-geo-replication.md) zelfstudie voor informatie over het gebruik van geo-replicatie naar aanzienlijk minder tijd nodig om te zetten van een grootschalige toepassing met meerdere tenants.

## <a name="additional-resources"></a>Aanvullende resources

[Aanvullende zelfstudies voort op de Wingtip SaaS-toepassing bouwen](https://docs.microsoft.com/azure/sql-database/sql-database-wtp-overview#sql-database-wingtip-saas-tutorials)
