---
title: Een Contoso Linux helpdesk-app naar de Azure App Service en Azure MySQL herstructureren | Microsoft Docs
description: Informatie over hoe Contoso on-premises Linux app promotiebewerkingen door te migreren naar Azure App Service met behulp van GitHub voor de weblaag en Azure SQL Database.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/04/2018
ms.author: raynew
ms.openlocfilehash: 585076b3fef56fe0e59828137674d7eb14528540
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37872004"
---
# <a name="contoso-migration-refactor-a-contoso-linux-service-desk-app-to-the-azure-app-service-and-azure-mysql"></a>Migratie van Contoso: een Contoso Linux helpdesk-app naar de Azure App Service en Azure MySQL herstructureren

In dit artikel leest u hoe Contoso hun on-premises twee lagen Linux servicedesk-app (osTicket), promotiebewerkingen door te migreren naar Azure App Service met GitHub-integratie en Azure MySQL.

Dit document is de tiende in een reeks artikelen die laten zien hoe het fictieve bedrijf Contoso de on-premises bronnen naar de Microsoft Azure-cloud migreert. De serie bevat achtergrondinformatie en scenario's die laten zien hoe u een migratie-infrastructuur instellen en uitvoeren van verschillende typen migraties. Scenario's toeneemt in complexiteit en aanvullende artikelen na verloop van tijd toegevoegd.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: overzicht](contoso-migration-overview.md) | Biedt een overzicht van de strategie voor de migratie van Contoso, de artikel-serie en de voorbeeld-apps die we gebruiken. | Beschikbaar
[Artikel 2: Een Azure-infrastructuur implementeren](contoso-migration-infrastructure.md) | Hierin wordt beschreven hoe Contoso de on-premises en Azure-infrastructuur voor migratie voorbereidt. Dezelfde infrastructuur wordt gebruikt voor alle Contoso migratiescenario's. | Beschikbaar
[Artikel 3: On-premises resources evalueren](contoso-migration-assessment.md)  | Laat zien hoe een evaluatie van hun on-premises twee lagen SmartHotel app waarop VMware wordt uitgevoerd in Contoso. Beoordeling van de virtuele machines van een app met de [Azure Migrate](migrate-overview.md) -service en de SQL Server-database van de app met de [Azure Database Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Beschikbaar
[Artikel 4: Hosten op Azure VM's en een beheerd exemplaar van SQL](contoso-migration-rehost-vm-sql-managed-instance.md) | Ziet u hoe Contoso de SmartHotel-app naar Azure migreert. Migreren van de app web virtuele machine via [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), en de app-database via de [Azure Database Migration](https://docs.microsoft.com/azure/dms/dms-overview) service, om te migreren naar een SQL beheerd exemplaar. | Beschikbaar
[Artikel 5: Opnieuw hosten naar virtuele Azure-machines](contoso-migration-rehost-vm.md) | Ziet u hoe Contoso hun SmartHotel migreren naar Azure IaaS-VM's, met behulp van de Site Recovery-service.
[Artikel 6: Opnieuw hosten naar virtuele machines in Azure en SQL Server-beschikbaarheidsgroepen](contoso-migration-rehost-vm-sql-ag.md) | Laat zien hoe de app SmartHotel door Contoso worden gemigreerd. Deze Site Recovery gebruiken voor het migreren van de VM's van de app en de Database Migration service de app-database migreren naar een SQL Server-beschikbaarheidsgroep. | Beschikbaar
[Artikel 7: Een Linux-app op Azure VM's opnieuw hosten](contoso-migration-rehost-linux-vm.md) | Ziet u hoe Contoso hun osTicket Linux-app naar Azure IaaS-VM's met Azure Site Recovery migreert.
[Artikel 8: Een Linux-app op Azure VM's en Azure MySQL-Server opnieuw hosten](contoso-migration-rehost-linux-vm-mysql.md) | Ziet u hoe Contoso de osTicket Linux app migreert. Deze Site Recovery voor migratie van virtuele machine en MySQL Workbench gebruiken om te migreren naar een Azure MySQL-Server-exemplaar. | Beschikbaar
Artikel 9: Een app naar een Azure-Web-app en Azure SQL Database herstructureren | Ziet u hoe Contoso de app SmartHotel migreert naar een Azure-container gebaseerde web-app en de app-database migreert naar Azure SQL-Server. | Beschikbaar
Artikel 10: Een Linux-app met Azure App Service en de Azure MySQL-Server herstructureren | Ziet u hoe Contoso de osTicket Linux app migreert naar Azure App Service met behulp van PHP 7.0 Docker-container. De codebasis voor de implementatie is gemigreerd naar GitHub. De app-database wordt gemigreerd naar Azure MySQL. | In dit artikel.


In dit artikel migreert Contoso een app met twee lagen Linux Apache MySQL PHP (LAMP) servicedesk (osTicket) naar Azure. Als u wilt deze open-source-app wilt gebruiken, kunt u het downloaden van [GitHub](https://github.com/osTicket/osTicket).



## <a name="business-drivers"></a>Zakelijke drijfveren

Het team voor IT-leidinggevenden heeft nauw samengewerkt met hun zakelijke partners om te begrijpen wat ze willen bereiken:

- **Adres van de groei van het bedrijf**: Contoso is groeien en verplaatsen naar nieuwe markten. Ze moeten extra customer service-agents. 
- **Schaal**: de oplossing moet worden gemaakt zodat Contoso meer medewerkers van de klantenservice als het bedrijf worden geschaald kunt toevoegen.
- **Tolerantie verhogen**: alleen voor interne gebruikers In de afgelopen problemen met het systeem worden beïnvloed. Met het nieuwe bedrijfsmodel externe gebruikers worden beïnvloed en Contoso moet de app startklaar te allen tijde.

## <a name="migration-goals"></a>Migratie-doelen

Het Contoso-cloud-team heeft vastgemaakt omlaag doelstellingen voor deze migratie kan om te bepalen van de beste migratiemethode:

- De toepassing moet worden geschaald dan de huidige on-premises capaciteit en prestaties.  Contoso is verplaatsen van de toepassing om te profiteren van Azure op aanvraag schalen.
- Contoso wil de app-codebasis verplaatsen naar een pipeline voor continue levering.  Als app-wijzigingen worden gepusht naar GitHub, die ze willen implementeren die wijzigingen zonder taken voor operationele medewerkers.
- De toepassing moet zijn tegen met mogelijkheden voor groei en failover zijn. Ze willen de app in twee verschillende Azure-regio's implementeren en instellen voor het automatisch schalen.
- Contoso wil minimaliseren beheertaken database nadat de app is verplaatst naar de cloud.

## <a name="solution-design"></a>Het ontwerp van oplossing
Na het vastmaken omlaag hun doelstellingen en vereisten, Contoso ontwerpen van een implementatieoplossing controleren en identificeren van het migratieproces, met inbegrip van de Azure-services die ze voor de migratie gaat gebruiken.


## <a name="current-architecture"></a>Huidige architectuur

- De app is gelaagd over twee virtuele machines (OSTICKETWEB en OSTICKETMYSQL).
- De virtuele machines bevinden zich op VMware ESXi-host **contosohost1.contoso.com** (versie 6.5).
- De VMware-omgeving wordt beheerd door vCenter Server 6.5 (**vcenter.contoso.com**) uitgevoerd op een virtuele machine.
- Contoso heeft een on-premises datacentrum (contoso-datacenter), met een on-premises domeincontroller (**contosodc1**).

![Huidige architectuur](./media/contoso-migration-refactor-linux-app-service-mysql/current-architecture.png) 


## <a name="proposed-architecture"></a>Voorgestelde architectuur

Na het vastmaken hun huidige architectuur, doelstellingen en vereisten voor migratie, Contoso een implementatieoplossing ontwerpen en identificeert het migratieproces, met inbegrip van de Azure-services die ze voor de migratie gaat gebruiken.

- De web-app-laag op OSTICKETWEB zal worden gemigreerd met het bouwen van een Azure App Service in twee Azure-regio's. Azure App Service voor Linux worden uitgevoerd met behulp van de PHP 7.0 Docker-container.
- De app-code wordt verplaatst naar GitHub en Azure-Web-App moet worden geconfigureerd voor continue levering met GitHub.
- Azure App-Servers worden geïmplementeerd in zowel de primaire (VS-Oost 2) en de secundaire regio van (VS-midden).
- Traffic Manager zal worden ingesteld voor de twee Azure-Web-Apps in beide regio's.
- Traffic Manager worden geconfigureerd in de modus voor prioriteit om af te dwingen van het verkeer via VS-Oost 2.
- Als de Azure-appserver in VS-Oost 2 offline gaat, gebruikers via toegang tot de app in VS-midden.
- De app-database worden, gemigreerd naar de Azure MySQL PaaS-service met behulp van hulpprogramma's voor MySQL Workbench. De on-premises database zal worden lokaal een back-up gemaakt en hersteld rechtstreeks naar Azure MySQL.
- De database bevindt zich in de primaire regio VS-Oost 2, in de database-subnet (PROD-DB-EUS2) in de productienetwerk (VNET-PROD-EUS2):
- Omdat ze een productie-werkbelasting bent gemigreerd, Azure-resources voor de app bevindt zich in de resourcegroep van de productie **ContosoRG**.
- Het Traffic Manager-resource wordt geïmplementeerd in de resourcegroep van de Contoso-infrastructuur **ContosoInfraRG**.
- De on-premises machines in het Contoso-datacenter wordt buiten gebruik gesteld nadat de migratie is voltooid.


![Scenario-architectuur](./media/contoso-migration-refactor-linux-app-service-mysql/proposed-architecture.png) 


## <a name="migration-process"></a>Migratieproces

Contoso wordt als volgt het migratieproces voltooid:

1. Als een eerste stap stelt Contoso u de Azure-infrastructuur, met inbegrip van Azure App Services voor het inrichten, het instellen van Traffic Manager en het inrichten van een Azure MySQL-exemplaar.
2. Na het voorbereiden van de Azure migreren zij de database met behulp van MySQL Workbench. 
3. Nadat de database wordt uitgevoerd in Azure, ze instellen van een persoonlijke GitHub-opslagplaats voor de Azure App Service met continue levering en deze met de app osTicket laden.
4. In de Azure-portal laden ze de app vanuit GitHub naar de Docker-container met Azure App Service. 
5. Deze DNS-instellingen aanpassen en automatisch schalen configureren voor de app.

![Migratieproces](./media/contoso-migration-refactor-linux-app-service-mysql/migration-process.png) 


### <a name="azure-services"></a>Azure-services

**Service** | **Beschrijving** | **Kosten**
--- | --- | ---
[Azure App Service](https://azure.microsoft.com/services/app-service/) | De service wordt uitgevoerd en kan worden geschaald toepassingen die gebruikmaken van de service Azure PaaS voor websites.  | Prijzen zijn gebaseerd op de grootte van de exemplaren en de functies die vereist zijn. [Meer informatie](https://azure.microsoft.com/pricing/details/app-service/windows/).
[Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) | Een load balancer waarvoor DNS wordt gebruikt voor rechtstreekse gebruikers naar Azure, of externe websites en services. | Prijzen zijn gebaseerd op het aantal ontvangen DNS-query's en het aantal gecontroleerde eindpunten. | [Meer informatie](https://azure.microsoft.com/pricing/details/traffic-manager/).
[Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/) | De database is gebaseerd op de open-source MySQL-Server-engine. Het biedt een volledig beheerde, bedrijfsklare community MySQL-database als een service voor ontwikkeling en implementatie. | Prijzen op basis van compute, opslag en back-upvereisten. [Meer informatie](https://azure.microsoft.com/pricing/details/mysql/).

 
## <a name="prerequisites"></a>Vereisten

Als u (en Contoso) uitvoeren van dit scenario wilt, is hier wat u moet hebben.

**Vereisten** | **Details**
--- | ---
**Azure-abonnement** | U moet al een abonnement hebt gemaakt tijdens de eerste artikelen in deze reeks. Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan.<br/><br/> Als u een gratis account maakt, bent u de beheerder van uw abonnement en kunt u alle acties uitvoeren.<br/><br/> Als u een bestaand abonnement gebruiken en u niet de beheerder bent, moet u contact op met de beheerder om u de eigenaar of bijdrager machtigingen.<br/><br/> Als u meer gedetailleerde machtigingen nodig hebt, raadpleegt u [in dit artikel](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Azure-infrastructuur** | Instellen van hun Azure-infrastructuur, zoals beschreven in Contoso [Azure-infrastructuur voor migratie](contoso-migration-infrastructure.md).



## <a name="scenario-steps"></a>Scenariostappen

Dit is hoe Azure de migratie wordt voltooid:

> [!div class="checklist"]
> * **Stap 1: Inrichten Azure App Services**: Contoso, Web-Apps in de primaire en secundaire regio's wordt ingericht.
> * **Stap 2: Stel Traffic Manager**: ze van Traffic Manager voor de Web-Apps instellen voor Routering en load balancing-verkeer.
> * **Stap 3: Inrichten MySQL**: In Azure, Contoso een exemplaar van Azure MySQL-database inricht.
> * **Stap 4: Migreren van de database**: migreren van de database met behulp van MySQL Workbench. 
> * **Stap 5: Stel GitHub in**: Contoso stelt u een lokale GitHub-opslagplaats voor de app websites /-code.
> * **Stap 6: De web-apps implementeren**: Contoso implementeert de WebApps vanuit GitHub.




## <a name="step-1-provision-azure-app-services"></a>Stap 1: Inrichten Azure App Services

Contoso bepalingen twee Web-apps (één in elke regio) met behulp van Azure App Services.

1. Ze een Web-App-resource maken in de primaire regio in de VS-Oost 2 (**osticket eus2**) van de Azure Marketplace.
2. Ze zetten de resource in de resourcegroep van de productie **ContosoRG**.

    ![Azure-App](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app1.png) 

3. Ze een nieuw App Service-plan maken in de primaire regio (**APP-SVP-EUS2**), met behulp van de standaardgrootte.

     ![Azure-App](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app2.png) 
    
4. Contoso selecteert u een Linux-besturingssysteem met de runtime-stack PHP 7.0, dit een Docker-container is.

    ![Azure-App](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app3.png) 

5. Maken van een tweede web-app (**osticket cus**) en App service-plan voor de regio VS-midden.

    ![Azure-App](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app4.png) 


**Meer hulp nodig?**

- Meer informatie over [Azure App Service Web-apps](https://docs.microsoft.com/azure/app-service/app-service-web-overview).
- Meer informatie over [Azure App Service on Linux](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro).


## <a name="step-2-set-up-traffic-manager"></a>Stap 2: Stel Traffic Manager

Contoso ingesteld van Traffic Manager om te leiden van inkomende webaanvragen naar de Web-Apps die worden uitgevoerd op de weblaag osTicket.

1. Contoso maakt u een Traffic Manager-resource (**osticket.trafficmanager.net**) van de Azure Marketplace. Ze gebruiken prioriteit routering zodat VS-Oost 2 de primaire site is. Ze plaatst u de resource in de resourcegroep voor infrastructuur (**ContosoInfraRG**). Houd er rekening mee dat de Traffic Manager globaal en niet aan een bepaalde locatie gebonden is

    ![Traffic Manager](./media/contoso-migration-refactor-linux-app-service-mysql/traffic-manager1.png) 

2. Nu Contoso Traffic Manager configureren met eindpunten. Ze East US 2 Web-app toevoegen als de primaire site (**osticket eus2**), en de VS-midden-app als secundaire (**osticket cus**).

    ![Traffic Manager](./media/contoso-migration-refactor-linux-app-service-mysql/traffic-manager2.png) 

3. Na het toevoegen van de eindpunten, kunnen zij ze te controleren.

    ![Traffic Manager](./media/contoso-migration-refactor-linux-app-service-mysql/traffic-manager3.png)

**Meer hulp nodig?**

- Meer informatie over [Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview).
- Meer informatie over [routeren van verkeer naar een prioriteitseindpunt](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-configure-priority-routing-method).
 
## <a name="step-3-provision-azure-database-for-mysql"></a>Stap 3: Inrichting van Azure Database voor MySQL

Contoso bepalingen een MySQL-database-exemplaar in de primaire regio in de VS-Oost 2.

1. In de Azure-portal maken ze een Azure Database for MySQL-resource. 

    ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/mysql-1.png)

2. Toevoegen van de naam **contosoosticket** voor de Azure-database. Ze de database toevoegen aan de productie-resourcegroep **ContosoRG**, en geef de referenties voor deze.
3. De on-premises MySQL-database is versie 5.7 gebruikt, zodat ze deze versie voor compatibiliteit selecteren. Ze gebruiken de standaard-grootten die overeenkomen met de vereisten voor de database.

     ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/mysql-2.png)

4. Voor **redundantieopties voor back-up**, Contoso worden geselecteerd voor het gebruik van **geografisch redundante**. Deze optie kan ze de database in de secundaire regio VS-midden herstellen als er een storing optreedt. Ze kunnen deze optie alleen configureren wanneer het inrichten van de database.

    ![Redundantie](./media/contoso-migration-refactor-linux-app-service-mysql/db-redundancy.png)

4. Contoso instellen van beveiliging van de verbinding. In de database > **verbindingsbeveiliging**, ze Firewall-regels instellen om toe te staan van de database voor toegang tot Azure-services.
5. Ze toevoegen het IP-adres van de lokale werkstation-client aan het begin- en IP-adressen. Hiermee wordt de Web-apps toegang krijgen tot de MySQL-database, samen met de databaseclient die de migratie uitvoert.

    ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/mysql-3.png)



## <a name="step-4-migrate-the-database"></a>Stap 4: De database migreren

Contoso kan de database met behulp van back-up en herstel, met de MySQL-hulpprogramma's worden gemigreerd. Ze MySQL Workbench installeren, back-up van de database van OSTICKETMYSQL en vervolgens herstellen met Azure Database voor MySQL-Server.

### <a name="install-mysql-workbench"></a>MySQL Workbench installeren

1. Controles van Contoso de [vereisten en downloads MySQL Workbench](https://dev.mysql.com/downloads/workbench/?utm_source=tuicool).
2. Installatie van MySQL Workbench voor Windows in overeenstemming met de [installatie-instructies](https://dev.mysql.com/doc/workbench/en/wb-installing.html). De machine waarop ze installeren moet toegankelijk zijn voor de OSTICKETMYSQL VM en Azure via internet.
3. In MySQL Workbench maken ze een MySQL-verbinding met OSTICKETMYSQL. 

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench1.png)

4. Ze de database als exporteren **osticket**, naar een lokaal ingesloten bestand.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench2.png)

5. Nadat de database heeft back-ups lokaal, maken ze een verbinding met de Azure Database for MySQL-exemplaar.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench3.png)

6. Contoso kan nu (herstel) de database in het Azure MySQL-exemplaar van het ingesloten bestand importeren. Een nieuw schema (osticket) wordt voor het exemplaar gemaakt.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench4.png)

7. Nadat de gegevens is hersteld, kunnen worden opgevraagd met behulp van Workbench en wordt weergegeven in de Azure-portal.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench5.png)

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench6.png)

8. Ten slotte moet Contoso bijwerken van de database-informatie op de web-apps. Ze open in de MySQL-exemplaar **verbindingsreeksen**. 

     ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench7.png)

9. In de lijst met tekenreeksen, ze vinden van de Web-App-instellingen en klik om te kopiëren.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench8.png)

10. Ze open een venster Kladblok en plak de tekenreeks in een nieuw bestand en bijwerken, zodat het overeenkomt met de osticket database, de MySQL-exemplaar en de referentie-instellingen.

     ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench9.png)

11. Contoso kunt controleren of de servernaam en aanmelding vanuit **overzicht** in de MySQL-instantie in de Azure-portal.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench10.png)


## <a name="step-5-set-up-github"></a>Stap 5: GitHub instellen

Contoso maakt een nieuwe persoonlijke GitHub-opslagplaats en stelt u een verbinding met de database osTicket in Azure MySQL. Vervolgens kunt laden ze de Azure-Web-App met de app.  

1.  Ze bladert u naar de OsTicket software openbare GitHub-opslagplaats en deze aan de Contoso-GitHub-account te vertakken.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github1.png)

2. Nadat u deze fork, navigeren naar de **bevatten** map en zoek de **ost-config.php** bestand.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github2.png)


3. Het bestand wordt geopend in de browser en ze bewerken.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github3.png)

4. In de editor updates Contoso de databasegegevens specifiek **DBHOST** en **DBUSER**. 

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github4.png)

5. Deze geeft vervolgens de wijzigingen.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github5.png)

6. Voor elke web-app (**osticket eus2** en **osticket cus**), Contoso wijzigen de **toepassingsinstellingen** in Azure portal.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github6.png)

7. Ze de verbindingsreeks met de naam invoeren **osticket**, en kopieer de tekenreeks van Kladblok in de **waarde gebied**. Ze selecteren **MySQL** in de vervolgkeuzelijst naast de tekenreeks, en sla de instellingen.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github7.png)

## <a name="step-6-configure-the-web-apps"></a>Stap 6: De Web-Apps configureren

Als de laatste stap in het migratieproces, Contoso de web-apps configureren met de osTicket-websites.



1. In de primaire web-app (**osticket eus2**) open **Implementatieoptie** en stelt u de bron op **GitHub**.

    ![App configureren](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app1.png)

2. Ze selecteren voor de implementatie-opties.

    ![App configureren](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app2.png)

3. Na het instellen van de opties voor de configuratie wordt weergegeven als in behandeling in Azure portal.

    ![App configureren](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app3.png)

4. Nadat de configuratie wordt bijgewerkt en de osTicket web-app vanuit GitHub is geladen in de borderel-container met de Azure App Service, wordt de site als actief weergegeven.

    ![App configureren](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app4.png)

5. Contoso vervolgens de bovenstaande stappen wordt herhaald voor de secundaire web-app (**osticket cus**).
6. Nadat de site is geconfigureerd, is toegankelijk via de Traffic Manager-profiel. De DNS-naam is de nieuwe locatie van de app osTicket. [Meer informatie](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-domain#map-a-cname-record).

    ![App configureren](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app5.png)
    
7. Contoso wil een DNS-naam die gemakkelijk te onthouden. Maken van een record voor een alias (CNAME) **osticket.contoso.com** die verwijzen naar de naam van de Traffic Manager, klikt u in de DNS-server op de domeincontrollers.

    ![App configureren](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app6.png)

8. Ze beide configureren de **osticket eus2** en **osticket cus** web-apps om toe te staan van de aangepaste hostnamen.

    ![App configureren](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app7.png)

### <a name="set-up-autoscaling"></a>Automatisch schalen instellen

Tot slot stelt ze van automatisch schalen voor de app. Dit zorgt ervoor dat als u agents gebruikt de app, de app-instanties verhogen en op basis van de behoeften van uw bedrijf verlagen. 

1. In App Service **APP-SRV-EUS2**, Contoso open **schaaleenheid**.
2. Ze configureren een nieuwe instelling voor automatisch schalen met een enkele regel waarmee het aantal exemplaren met één verhoogd wanneer de CPU-percentage voor de huidige sessie dan 70% 10 minuten is.

    ![Automatisch schalen](./media/contoso-migration-refactor-linux-app-service-mysql/autoscale1.png)

3. Ze dezelfde instelling configureren op **APP-SRV-CUS** om ervoor te zorgen dat hetzelfde gedrag is van toepassing als de app failover naar de secundaire regio. Het enige verschil is dat ze de exemplaar-limiet ingesteld op 1, omdat dit voor alleen failovers.

   ![Automatisch schalen](./media/contoso-migration-refactor-linux-app-service-mysql/autoscale2.png)

##  <a name="clean-up-after-migration"></a>Na de migratie opschonen

Met de migratie is voltooid, wordt de app osTicket geherstructureerd uitvoeren in een Azure-Web-app met continue levering met behulp van een persoonlijke GitHub-opslagplaats. Van de app wordt uitgevoerd in twee regio's voor verhoogde flexibiliteit. De database osTicket wordt uitgevoerd in Azure database for MySQL na de migratie naar de PaaS-platform.

Contoso moet nu het volgende doen: 
- Verwijder de VMware-VM's van de vCenter-voorraad.
- De on-premises VM's van lokale back-uptaken verwijderen.
- Update interne documentatie nieuwe locaties en IP-adressen weergeven. 
- Alle resources die interactie met de on-premises VM's hebben controleren en bijwerken van de relevante instellingen of documentatie om de nieuwe configuratie weer te geven.
- Bewaken om te verwijzen naar de URL osticket trafficmanager.net om bij te houden dat de app actief is en wordt uitgevoerd opnieuw configureren.

## <a name="review-the-deployment"></a>De implementatie controleren

Met de app nu wordt uitgevoerd, moet Contoso volledig operationeel maken en hun nieuwe infrastructuur beveiligen.

### <a name="security"></a>Beveiliging

Het Contoso security team gecontroleerd op de app om te bepalen van alle beveiligingsproblemen met zich mee. Ze geïdentificeerd dat de communicatie tussen de osTicket-app en het exemplaar van de MySQL-database is niet geconfigureerd voor SSL. Ze moeten doen dit om ervoor te zorgen dat databaseverkeer kan niet worden gehackt. [Meer informatie](https://docs.microsoft.com/azure/mysql/howto-configure-ssl).

### <a name="backups"></a>Back-ups

- De web-apps osTicket bevatten geen gegevens van de gebruikersstatus en dus hoeft te back-up worden gemaakt.
- Ze hoeft te configureren van back-up voor de database. Azure Database for MySQL back-ups van de server automatisch gemaakt en opgeslagen. Ze geselecteerd om te gebruiken geo-redundantie voor de database, dus het is flexibel en gereed is voor productie. Back-ups kunnen worden gebruikt om uw server te herstellen naar een punt-in-time. [Meer informatie](https://docs.microsoft.com/azure/mysql/concepts-backup).


### <a name="licensing-and-cost-optimization"></a>Licentie- en kosten optimaliseren

- Er zijn geen licenties problemen voor de PaaS-implementatie.
- Contoso kunnen Azure Cost Management door Cloudyn, een dochteronderneming van Microsoft in licentie gegeven. Het is een kostenbeheeroplossing met meerdere Clouds management-oplossing die u helpt bij het gebruiken en beheren van Azure en andere cloudresources.  [Meer informatie](https://docs.microsoft.com/azure/cost-management/overview) over Azure Cost Management.



