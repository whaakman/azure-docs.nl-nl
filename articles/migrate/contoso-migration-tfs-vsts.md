---
title: Herstructureren van een Team Foundation Server-implementatie naar Visual Studio Team Services (VSTS) in Azure | Microsoft Docs
description: Informatie over hoe Contoso de on-premises TFS-implementatie door te migreren promotiebewerkingen deze naar Visual Studio Team Services (VSTS) in Azure.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: raynew
ms.openlocfilehash: f854c269eaf65335fb5654709fe98857d11865b2
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190132"
---
# <a name="contoso-migration--refactor-a-team-foundation-server-deployment-to-visual-studio-team-services-vsts"></a>Migratie van Contoso: een Team Foundation Server-implementatie naar Visual Studio Team Services (VSTS) herstructureren

In dit artikel leest u hoe Contoso haar Team Foundation Server (TFS) on-premises implementatie door te migreren is herstructurering deze naar Visual Studio Team Services (VSTS) in Azure. Het Contoso-ontwikkelteam TFS hebt gebruikt voor teamsamenwerking en bronbeheer voor de afgelopen vijf jaar. Nu wil het team verplaatsen naar een cloud-gebaseerde oplossing voor ontwikkelen en testen, en voor broncodebeheer. VSTS wordt een rol spelen als het team naar een model met DevOps verplaatsen en nieuwe cloud-eigen Apps.

Dit document is een in een reeks artikelen die laten zien hoe het fictieve bedrijf Contoso de on-premises bronnen naar de Microsoft Azure-cloud migreert. De serie bevat achtergrondinformatie en scenario's die laten zien hoe u een migratie-infrastructuur instellen en uitvoeren van verschillende typen migraties. Scenario's toeneemt in complexiteit. Aanvullende artikelen gaan we toevoegen na verloop van tijd.


**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: overzicht](contoso-migration-overview.md) | Overzicht van de serie artikelen, strategie voor de migratie van Contoso en de voorbeeld-apps die worden gebruikt in de reeks. | Beschikbaar
[Artikel 2: Azure-infrastructuur implementeren](contoso-migration-infrastructure.md) | Contoso bereidt u de on-premises infrastructuur en de Azure-infrastructuur voor migratie. Dezelfde infrastructuur wordt gebruikt voor alle migratieartikelen in de reeks. | Beschikbaar
[Artikel 3: Evalueer on-premises bronnen voor migratie naar Azure](contoso-migration-assessment.md)  | Contoso wordt uitgevoerd een evaluatie van de on-premises SmartHotel-app die wordt uitgevoerd op VMware. Contoso beoordeelt virtuele machines van app met behulp van de Azure Migrate-service en de app SQL Server-database met behulp van Data Migration Assistant. | Beschikbaar
[Artikel 4: Opnieuw hosten van een app op een Azure-VM en het beheerde exemplaar van SQL Database](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso wordt een lift-and-shift-migratie naar Azure voor de on-premises SmartHotel app uitgevoerd. Contoso migreert u de app front-end virtuele machine via [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso de app-database migreert naar een Azure SQL Database Managed Instance met de [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Beschikbaar  
[Artikel 5: Een app op Azure VM's opnieuw hosten](contoso-migration-rehost-vm.md) | De VM's van de SmartHotel app migreert Contoso naar Azure-VM's met behulp van de Site Recovery-service. | Beschikbaar
[Artikel 6: Opnieuw hosten naar virtuele machines in Azure en SQL Server-beschikbaarheidsgroepen](contoso-migration-rehost-vm-sql-ag.md) | De app SmartHotel, Contoso worden gemigreerd. Contoso maakt gebruik van Site Recovery voor het migreren van de VM's van de app. De Database Migration Service wordt gebruikt voor het migreren van de app-database naar een SQL Server-cluster dat wordt beveiligd door een AlwaysOn-beschikbaarheidsgroep. | Beschikbaar
[Artikel 7: Een Linux-app op Azure VM's opnieuw hosten](contoso-migration-rehost-linux-vm.md) | Contoso een lift-and-shift-migratie van de app van de osTicket Linux Azure-virtuele machines, met behulp van Azure Site Recovery is voltooid | Beschikbaar
[Artikel 8: Een Linux-app op Azure VM's en Azure MySQL-Server opnieuw hosten](contoso-migration-rehost-linux-vm-mysql.md) | Contoso de osTicket Linux app overzet naar virtuele Azure-machines met Azure Site Recovery en de app-database migreert naar een Azure MySQL-Server-exemplaar met behulp van MySQL Workbench. | Beschikbaar
[Artikel 9: Een app op Azure Web Apps en Azure SQL database herstructureren](contoso-migration-refactor-web-app-sql.md) | Contoso-de app SmartHotel migreert naar een Azure-Web-App en de app-database migreert naar een Azure SQL Server-exemplaar met Database Migration Assistant | Beschikbaar
[Artikel 10: Een Linux-app op Azure-Web-Apps en Azure MySQL herstructureren](contoso-migration-refactor-linux-app-service-mysql.md) | De Linux-app osTicket migreert Contoso naar een Azure-web-app op meerdere Azure-regio's met behulp van Azure Traffic Manager, geïntegreerd met GitHub voor continue levering. Contoso migreert de app-database naar een Azure Database for MySQL-exemplaar. | Beschikbaar 
Artikel 11: Herstructureren TFS op VSTS | Contoso migreert de on-premises Team Foundation Server-implementatie naar Visual Studio Team Services in Azure. | In dit artikel.
[Artikel 12: Opnieuw ontwerpen van een app op Azure-containers en Azure SQL Database](contoso-migration-rearchitect-container-sql.md) | De app SmartHotel migreert Contoso naar Azure. Vervolgens rearchitects wordt de weblaag app als een Windows-container die wordt uitgevoerd in Azure Service Fabric en de database met Azure SQL Database. | Beschikbaar
[Artikel 13: Opnieuw opbouwen van een app in Azure](contoso-migration-rebuild.md) | Contoso wordt opnieuw gemaakt zijn SmartHotel-app met een scala aan mogelijkheden van Azure en services, waaronder Azure App Service, Azure Kubernetes Service (AKS), Azure Functions, Azure Cognitive Services en Azure Cosmos DB. | Beschikbaar



## <a name="business-drivers"></a>Zakelijke drijfveren

Het team voor IT-leidinggevenden heeft nauw samengewerkt met zakelijke partners te identificeren van doelen voor de toekomst. Partners niet overmatig betrokken zijn bij de ontwikkelprogramma's en technologieën, maar ze deze punten zijn opgenomen:

- **Software**:, ongeacht de kernactiviteiten alle bedrijven die nu zijn ingesteld op softwarebedrijven, met inbegrip van Contoso. Zakelijke leidinggevende is geïnteresseerd in hoe IT kunt leiden van het bedrijf met de nieuwe werkwijze voor gebruikers en ervaringen voor hun klanten.
- **Efficiëntie**: Contoso moet te stroomlijnen en te verwijderen van onnodige procedures voor ontwikkelaars en gebruikers. Hierdoor wordt het bedrijf voor de levering op klantvereisten efficiënter. De bedrijfsbehoeften IT snel, zonder verspild tijd of geld.
- **Flexibiliteit**: Contoso IT moet reageren op bedrijfsbehoeften en sneller dan de marketplace om in te schakelen succes in een globale economie reageren. IT mag niet een upblokkering voor het bedrijf.

## <a name="migration-goals"></a>Migratie-doelen

Het Contoso-cloud-team heeft vastgemaakt omlaag doelstellingen voor de migratie naar VSTS:

- Het team moet een hulpprogramma voor het migreren van de gegevens naar de cloud. Moeten worden enkele handmatige processen die nodig zijn.
- Werkitem van gegevens en geschiedenis van het afgelopen jaar moet worden gemigreerd.
- Het team wilt niet dat voor het instellen van nieuwe gebruikersnamen en wachtwoorden. Alle huidige toewijzingen van systeem moeten worden onderhouden.
- Het team wil overstappen van Team Foundation Version Control (TFVC) naar Git voor broncodebeheer.
- De cutover naar Git is een 'tip migratie"dat de invoer van alleen de meest recente versie van de broncode. Dit gebeurt tijdens een downtime wanneer al het werk wordt gestopt als de codebase ploegen. Het team zich van bewust dat alleen de huidige master-vertakking geschiedenis beschikbaar na de verplaatsing zijn.
- Het team zich zorgen over de wijziging en wilt testen voordat u een volledige verplaatsen. Het team wil toegang tot TFS behouden, zelfs na de overstap naar VSTS.
- Contoso heeft meerdere verzamelingen, en wilt beginnen met een met slechts een paar projecten voor meer informatie over het proces.
- Het team weten dat TFS-verzamelingen een-op-een-relatie met VSTS-accounts, met meerdere URL's zijn. Maar dit komt overeen met het huidige model van scheiding van voor code-databases en projecten.


## <a name="proposed-architecture"></a>Voorgestelde architectuur

- Contoso wordt de TFS-projecten naar de cloud verplaatsen en hun projecten of bron besturingselement on-premises niet langer te hosten.
- TFS worden gemigreerd naar VSTS.
- Contoso heeft momenteel een TFS-verzameling met de naam **ContosoDev**, die worden gemigreerd naar een VSTS-account met de naam **contosodevmigration.visualstudio.com**.
- De projecten, werkitems, bugs en iteraties van het afgelopen jaar worden, gemigreerd naar VSTS.
- Contoso gebruik van Azure Active Directory, die Contoso tijdens de implementatie van instellen de [Azure-infrastructuur](contoso-migration-infrastructure.md) aan het begin van de migratieplanning van de. 


![Scenario-architectuur](./media/contoso-migration-tfs-vsts/architecture.png) 


## <a name="migration-process"></a>Migratieproces

Contoso wordt als volgt het migratieproces voltooid:

1. Er is een groot aantal voorbereiding die betrokken zijn. Upgraden van de TFS-implementatie naar een ondersteunde moet als eerste stap, Contoso. Contoso TFS 2017 Update 3 momenteel wordt uitgevoerd, maar voor het gebruik van de migratie van de database moet een ondersteunde versie van 2018 uitvoeren met de meest recente updates.
2. Na de upgrade, worden Contoso uitgevoerd het TFS-hulpprogramma voor migratie en valideren van de verzameling.
3. Contoso wordt een set bestanden voorbereiding bouwen en uitvoeren van een migratie droge uitvoeren voor het testen.
4. Contoso Voer vervolgens een andere migratie, dit moment een volledige migratie met werkitems, fouten, sprints en code.
5. Na de migratie, worden Contoso verplaatst de code van TFVC naar Git.

![Migratieproces](./media/contoso-migration-tfs-vsts/migration-process.png) 


## <a name="scenario-steps"></a>Scenariostappen

Hier ziet u hoe Contoso de migratie wordt voltooid:

> [!div class="checklist"]
> * **Stap 1: Een Azure storage-account maken**: dit storage-account wordt gebruikt tijdens het migratieproces.
> * **Stap 2: Upgrade TFS**: Contoso werkt de implementatie naar TFS 2018 Upgrade 2. 
> * **Stap 3: Valideren verzameling**: Contoso valideert de TFS-verzameling in voorbereiding voor migratie.
> * **Stap 4: Voorbereiding buildbestand**: Contoso maakt u de migratiebestanden met behulp van het TFS-hulpprogramma voor migratie. 


## <a name="step-1-create-a-storage-account"></a>Stap 1: Een storage-account maken

1. In de Azure-portal, beheerders van Contoso een opslagaccount maken (**contosodevmigration**).
2. Ze plaatsen het account in de secundaire regio die ze voor failover - centraal VS gebruiken. Ze een algemeen standard-account met lokaal redundante opslag gebruiken.

    ![Storage-account](./media/contoso-migration-tfs-vsts/storage1.png) 


**Meer hulp nodig?**

- [Inleiding tot Azure storage](https://docs.microsoft.com/azure/storage/common/storage-introduction).
- [Maak een opslagaccount](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).


## <a name="step-2-upgrade-tfs"></a>Stap 2: Upgrade TFS

Beheerders van Contoso upgrade de TFS-server naar TFS 2018 Update 2. Voordat ze starten:

- Ze downloaden [TFS 2018 Update 2](https://visualstudio.microsoft.com/downloads/)
- Ze controleren of de [hardwarevereisten](https://docs.microsoft.com/tfs/server/requirements), en lezen via de [opmerkingen bij de release](https://docs.microsoft.com/visualstudio/releasenotes/tfs2018-relnotes) en [gotcha's upgrade](https://docs.microsoft.com/tfs/server/upgrade/get-started#before-you-upgrade-to-tfs-2018).

Ze bijwerken als volgt:

1. Als u wilt starten, deze back-up van de TFS-server (die wordt uitgevoerd op een VMware-vM) en maakt u een VMware-momentopname.

    ![TFS](./media/contoso-migration-tfs-vsts/upgrade1.png) 

2. Het TFS-installatieprogramma wordt gestart, en ze ervoor kiezen de installatielocatie. Het installatieprogramma heeft internettoegang nodig.

    ![TFS](./media/contoso-migration-tfs-vsts/upgrade2.png) 

3. Nadat de installatie is voltooid, wordt de configuratiewizard voor Server gestart.

    ![TFS](./media/contoso-migration-tfs-vsts/upgrade3.png) 

4. Na de verificatie wordt in de Wizard de upgrade is voltooid.

     ![TFS](./media/contoso-migration-tfs-vsts/upgrade4.png) 

5. Ze controleren of de TFS-installatie aan de hand van de projecten, werkitems en code.

     ![TFS](./media/contoso-migration-tfs-vsts/upgrade5.png) 

> [!NOTE]
> Sommige TFS-upgrades nodig om uit te voeren van de Wizard Functies configureren nadat de upgrade is voltooid. [Meer informatie](https://docs.microsoft.com/vsts/work/customize/configure-features-after-upgrade?utm_source=ms&utm_medium=guide&utm_campaign=vstsdataimportguide&view=vsts).

**Meer hulp nodig?**

Meer informatie over [TFS upgraden](https://docs.microsoft.com/tfs/server/upgrade/get-started).

## <a name="step-3-validate-the-tfs-collection"></a>Stap 3: Valideren de TFS-verzameling

Contoso-beheerders het TFS-hulpprogramma voor migratie voor de database van de verzameling ContosoDev te valideren voordat de migratie uitvoeren.

1. Ze downloaden en pak deze uit de [hulpprogramma voor migratie van TFS](https://www.microsoft.com/download/details.aspx?id=54274). Het is belangrijk om te downloaden van de versie van de TFS-update die wordt uitgevoerd. De versie kan worden weergegeven in de beheerconsole.

    ![TFS](./media/contoso-migration-tfs-vsts/collection1.png)

2. Ze Voer het hulpprogramma voor het uitvoeren van de validatie, met de URL van de teamprojectverzameling op te geven:

        **TfsMigrator validate /collection:http://contosotfs:8080/tfs/ContosoDev**


3. Het hulpprogramma toont een fout.

    ![TFS](./media/contoso-migration-tfs-vsts/collection2.png)

5. Ze zich op het logboek bestanden bevinden zich in de **logboeken** map, net vóór de locatie van het hulpprogramma. Een logboekbestand wordt gegenereerd voor elke primaire validatie. **TfsMigration.log** bevat de belangrijkste informatie.

    ![TFS](./media/contoso-migration-tfs-vsts/collection3.png)

4. Ze dit item, met betrekking tot identiteit niet vinden.

    ![TFS](./media/contoso-migration-tfs-vsts/collection4.png)

5. Ze worden uitgevoerd **TfsMigration valideren/Help** op de opdrachtregel, en u ziet dat de opdracht **/tenantDomainName** lijkt te zijn vereist om te controleren van identiteiten.

     ![TFS](./media/contoso-migration-tfs-vsts/collection5.png)

6. Ze voert de opdracht van de validatie opnieuw uit en voeg deze waarde, samen met hun Azure AD-naam: **TfsMigrator/Collection valideren:http://contosotfs:8080/tfs/ContosoDev /tenantDomainName:contosomigration.onmicrosoft.com**.

    ![TFS](./media/contoso-migration-tfs-vsts/collection7.png)

7. Een Azure AD-aanmelding In het scherm wordt weergegeven en ze voert u de referenties van een globale beheerder.

     ![TFS](./media/contoso-migration-tfs-vsts/collection8.png)

8. De validatie is geslaagd en wordt bevestigd door het hulpprogramma.

    ![TFS](./media/contoso-migration-tfs-vsts/collection9.png)



## <a name="step-4-create-the-migration-files"></a>Stap 4: De migratiebestanden maken

Met de validatie is voltooid, Contoso-beheerders het TFS-hulpprogramma voor migratie gebruiken om te maken van bestanden voor de migratie.

1. Ze de voorbereidingsstap in het hulpprogramma worden uitgevoerd.

    **TfsMigrator/Collection voorbereiden:http://contosotfs:8080/tfs/ContosoDev /tenantDomainName:contosomigration.onmicrosoft.com /accountRegion:cus**

     ![Voorbereiden](./media/contoso-migration-tfs-vsts/prep1.png)

    Voorbereiden doet het volgende:
    - De verzameling voor een lijst van alle gebruikers wordt gescand en vult u het logboek van de kaart identificeren (**IdentityMapLog.csv**])
    - Zorgt ervoor dat de verbinding met Azure Active Directory om een overeenkomst voor elke identiteit te vinden.
    - Contoso heeft al geïmplementeerd van Azure AD en gesynchroniseerd met behulp van AD Connect, zodat voorbereiden kunnen vinden van de overeenkomende identiteiten en deze markeren als actief.

2. Een Azure AD-aanmelding In het scherm wordt weergegeven en ze voert u de referenties van een globale beheerder.

    ![Voorbereiden](./media/contoso-migration-tfs-vsts/prep2.png)

3. Voorbereiden is voltooid, en het hulpprogramma wordt gerapporteerd dat de bestanden importeren is gegenereerd.

    ![Voorbereiden](./media/contoso-migration-tfs-vsts/prep3.png)

4. Ze ziet nu dat de IdentityMapLog.csv en het bestand import.json beide zijn gemaakt in een nieuwe map.

    ![Voorbereiden](./media/contoso-migration-tfs-vsts/prep4.png)

5. Het bestand import.json bevat instellingen voor importeren. Het bevat informatie zoals de naam van het gewenste en gegevens over het opslagaccount. De meeste van de velden worden automatisch ingevuld. Sommige velden vereist invoer van de gebruiker. Ze open het bestand en wordt de naam van de VSTS-account moet worden gemaakt: **contosodevmigration**. Met deze naam, de URL van hun VSTS worden **contosodevmigration.visualstudio.com**.

    ![Voorbereiden](./media/contoso-migration-tfs-vsts/prep5.png)

    > [!NOTE]
    > Het account moet worden gemaakt vóór de migratie, kan worden gewijzigd nadat de migratie is voltooid.

6. Ze het identiteit kaart logboekbestand controleren die toont de accounts die in VSTS tijdens het importeren wordt geopend. 

    - Actieve identiteiten verwijzen naar identiteiten die gebruikers in VSTS na het importeren worden.
    - Op VSTS, deze identiteiten in licentie wordt gegeven, en weergegeven als een gebruiker in het account na de migratie.
    - Deze identiteiten zijn gemarkeerd als **Active** in de **Importstatus verwacht** kolom in het bestand.

    ![Voorbereiden](./media/contoso-migration-tfs-vsts/prep6.png)



## <a name="step-5-migrate-to-vsts"></a>Stap 5: Migreren naar VSTS

Met de voorbereiding op locatie, kunnen beheerders van Contoso nu zich richten op de migratie. Nadat de migratie is uitgevoerd, schakel ze TFVC bent met Git voor versiebeheer te gebruiken.

Voordat ze gaat plannen beheerders van de downtime met het ontwikkelingsteam, de verzameling offline voor migratie uitvoeren. Dit zijn de stappen voor het migratieproces:

1. **Loskoppelen van de verzameling**: ze identiteitsgegevens voor de verzameling bevindt zich in de configuratiedatabase van de TFS-server terwijl de verzameling is aangesloten en online. Wanneer een verzameling is losgekoppeld van de TFS-server, wordt een kopie van die identiteitsgegevens, en deze pakketten met de verzameling voor transport. Zonder deze gegevens, kan niet het gedeelte van de identiteit van de import worden uitgevoerd. Het wordt aanbevolen dat de verzameling losgekoppelde blijven totdat het importeren is voltooid, omdat er geen manier voor het importeren van de wijzigingen dat is opgetreden tijdens het importeren.
2. **Genereren van een back-up**: de volgende stap van het migratieproces is dat ze voor het genereren van een back-up die kan worden geïmporteerd in VSTS. Data-tier Application onderdeel pakketten (DACPAC) is een SQL Server-functie waarmee databasewijzigingen worden verpakt in een enkel bestand en geïmplementeerd voor andere exemplaren van SQL. Het kan ook rechtstreeks naar VSTS worden hersteld en wordt daarom gebruikt als de verpakking-methode voor het ophalen van gegevens te verzamelen in de cloud. Contoso gebruikt het hulpprogramma SqlPackage.exe voor het genereren van de DACPAC. Dit hulpprogramma is opgenomen in SQL Server Data Tools.
3. **Uploaden naar storage**: nadat de DACPAC is gemaakt, ze te uploaden naar Azure Storage. Nadat deze geüpload, krijgen ze een shared access signature (SAS), dat het hulpprogramma voor migratie van TFS-toegang tot de opslag.
4. **Vul het importeren**: ze kunnen dan ontbrekende velden in het importbestand, met inbegrip van de instelling DACPAC invullen. Beginnen met ze moeten opgeven dat ze willen doen een **is een** importeren om te controleren of alles goed voordat u de volledige migratie werkt.
5. **Test**: droge uitvoeren invoer te verzamelingmigratie testen. Droog uitvoeringen beperkte levensduur hebben en worden verwijderd voordat de migratie van een productie wordt uitgevoerd. Ze bent na een bepaalde tijd automatisch verwijderd. Een opmerking over wanneer de uitvoering droge worden verwijderd, is opgenomen in de e-mail met succesbericht ontvangen nadat het importeren is voltooid. Houd er rekening mee houden en dienovereenkomstig te plannen.
6. **Voltooien van de productie-migratie**: met de testmodus de migratie is voltooid, Contoso-beheerders de laatste migratie doen door de import.json bijwerken en opnieuw importeren uit te voeren.



### <a name="detach-the-collection"></a>Loskoppelen van de verzameling

Voordat u begint nemen beheerders van Contoso een lokale SQL Server-back-up en VMware-momentopname van de TFS-server, voordat het loskoppelen.

1.  In de TFS-beheerconsole, ze de verzameling die ze willen loskoppelen selecteren (**ContosoDev**).

    ![Migreren](./media/contoso-migration-tfs-vsts/migrate1.png)

2. In **algemene**, selecteren ze **verzameling loskoppelen**

    ![Migreren](./media/contoso-migration-tfs-vsts/migrate2.png)

3. In de verzameling loskoppelen Team Project Wizard > **bericht onderhoud**, ze een bericht bieden voor gebruikers die proberen verbinding maken met projecten in de verzameling.

    ![Migreren](./media/contoso-migration-tfs-vsts/migrate3.png)

4. In **ontkoppelen uitgevoerd**, ze de voortgang controleren en klikt u op **volgende** wanneer het proces is voltooid.

    ![Migreren](./media/contoso-migration-tfs-vsts/migrate4.png)

5. In **Gereedheidscontroles**wanneer controles ze voltooien klikt u op **loskoppelen**.

    ![Migreren](./media/contoso-migration-tfs-vsts/migrate5.png)

6. Ze klikt u op **sluiten** te voltooien.

    ![Migreren](./media/contoso-migration-tfs-vsts/migrate6.png)

6. De verzameling is niet meer worden verwezen in de TFS-beheerconsole.

    ![Migreren](./media/contoso-migration-tfs-vsts/migrate7.png)


### <a name="generate-a-dacpac"></a>Een DACPAC genereren

Beheerders van Contoso maken een back-up (DACPAC) voor het importeren in VSTS.

- SqlPackage.exe in SQL Server Data Tools wordt gebruikt voor het maken van de DACPAC. Er zijn meerdere versies van geïnstalleerd met SQL Server Data Tools, bevindt zich onder de mappen met namen, zoals 120, 130 en 140 SqlPackage.exe. Het is belangrijk de DACPAC voorbereiden met de juiste versie.
- TFS-2018 invoer moeten SqlPackage.exe gebruiken vanuit de map 140 of hoger.  Voor CONTOSOTFS, dit bestand bevindt zich in map: **C:\Program Files (x86) \Microsoft Visual Studio\2017\Enterprise\Common7\IDE\Extensions\Microsoft\SQLDB\DAC\140**.


Beheerders van Contoso genereren de DACPAC als volgt:

1. Ze open een opdrachtprompt en navigeer naar de locatie SQLPackage.exe. Ze typt deze volgende opdracht voor het genereren van de DACPAC u:

    **SqlPackage.exe /sourceconnectionstring: "Data Source = SQLSERVERNAME\INSTANCENAME; Initial Catalog = Tfs_ContosoDev; Integrated Security = True" /targetFile:C:\TFSMigrator\Tfs_ContosoDev.dacpac /action:extract /p:ExtractAllTableData = true /p: ik gnoreUserLoginMappings = true /p:IgnorePermissions = true /p:Storage = geheugen** 

    ![Backup](./media/contoso-migration-tfs-vsts/backup1.png)

2. Het volgende bericht wordt weergegeven nadat de opdracht wordt uitgevoerd.

    ![Backup](./media/contoso-migration-tfs-vsts/backup2.png)

3. Ze controleren of de eigenschappen van de DACPACfile

    ![Backup](./media/contoso-migration-tfs-vsts/backup3.png)

### <a name="update-the-file-to-storage"></a>Het bestand bijwerken naar opslag

Nadat de DACPAC is gemaakt, wordt het in Contoso geüpload naar Azure Storage.

1. Ze [download en installeer](https://azure.microsoft.com/features/storage-explorer/) Azure Storage Explorer.

    ![Uploaden](./media/contoso-migration-tfs-vsts/backup5.png)

4. Ze verbinding maken met hun abonnement en Ga naar het opslagaccount dat ze voor de migratie gemaakt (**contosodevmigration**). Maken van een nieuwe blobcontainer **vstsmigration**.

    ![Uploaden](./media/contoso-migration-tfs-vsts/backup6.png)

5. Ze opgeven de DACPAC-bestand voor uploaden als een blok-blob.

    ![Uploaden](./media/contoso-migration-tfs-vsts/backup7.png)
    
7. Nadat het bestand de geüpload, ze klikt u op de bestandsnaam > **SAS genereren**. Ze Vouw de blob-containers in het storage-account, selecteert u de container met de bestanden importeren en klikt u op **Shared Access Signature ophalen**.

    ![Uploaden](./media/contoso-migration-tfs-vsts/backup8.png)

8. Ze accepteer de standaardinstellingen en klik op **maken**. Dit kan toegang worden ingeschakeld voor 24 uur.

    ![Uploaden](./media/contoso-migration-tfs-vsts/backup9.png)

9. Ze kopiëren de Shared Access Signature-URL, zodat deze kan worden gebruikt door het TFS-hulpprogramma voor migratie.

    ![Uploaden](./media/contoso-migration-tfs-vsts/backup10.png)

> [!NOTE]
> De migratie moet gebeuren voordat binnen de toegestane tijd venster of machtigingen verlopen.
> Geen een SAS-sleutel genereren vanuit de Azure-portal. -Sleutels gegenereerd als volgt zijn binnen het bereik van account en werkt niet met het importeren.

### <a name="fill-in-the-import-settings"></a>Vul in de instellingen voor importeren

Eerder, beheerders van Contoso gedeeltelijk ingevuld het importbestand-specificatie (import.json). Nu moeten de resterende instellingen toe te voegen.

Ze opent u het bestand import.json en vul de volgende velden: • locatie: locatie van de SAS-sleutel die hierboven is gegenereerd.
• Dacpac: Stel de naam op het DACPAC-bestand dat u hebt geüpload naar het opslagaccount. De extensie 'dacpac' bevatten.
• ImportType: ingesteld op is een voorlopig.


![Instellingen importeren](./media/contoso-migration-tfs-vsts/import1.png)


### <a name="do-a-dry-run-migration"></a>Een migratie testmodus

Beheerders van Contoso start met een migratie testmodus, om te controleren of dat alles werkt zoals verwacht.

1. Deze open een opdrachtprompt en zoek naar de locatie TfsMigration (C:\TFSMigrator).
2. Als eerste stap valideren ze het importbestand. Ze willen om er zeker van te zijn dat het bestand correct is ingedeeld, en of de SAS-sleutel werkt.

    **TfsMigrator /importFile:C:\TFSMigrator\import.json /validateonly importeren**

3. De validatie is een fout die de SAS-sleutel een langer verlooptijd moet geretourneerd.

    ![Testmodus](./media/contoso-migration-tfs-vsts/test1.png)

3. Ze Azure Storage Explorer gebruiken voor het maken van een nieuwe SAS-sleutel met verlopen ingesteld op zeven dagen.

    ![Testmodus](./media/contoso-migration-tfs-vsts/test2.png)

3. Deze update van het bestand import.json en voer de validatie opnieuw uit. Deze tijd voltooid.

    **TfsMigrator /importFile:C:\TFSMigrator\import.json /validateonly importeren**

    ![Testmodus](./media/contoso-migration-tfs-vsts/test3.png)
    
7. Ze start het droge uitvoeren:

    **TfsMigrator importeren /importFile:C:\TFSMigrator\import.json**

8. Een bericht wordt weergegeven om te bevestigen dat de migratie. Houd er rekening mee de tijdsduur waarvoor de voorbereide gegevens worden onderhouden nadat het droge uitvoeren.

    ![Testmodus](./media/contoso-migration-tfs-vsts/test4.png)

9. Azure AD aanmeldingspagina wordt weergegeven, en met Contoso Admin-aanmelding moet worden voltooid.

    ![Testmodus](./media/contoso-migration-tfs-vsts/test5.png)

10. Een bericht toont informatie over het importeren.

    ![Testmodus](./media/contoso-migration-tfs-vsts/test6.png)

11. Na 15 minuten of dat ze Blader naar de URL en raadpleegt u de volgende informatie:

     ![Testmodus](./media/contoso-migration-tfs-vsts/test7.png)

12. Nadat de migratie is voltooid, aanmeldt een potentiële klanten Contoso Dev met VSTS om te controleren of de uitvoering droge opgeleverd. Na verificatie moet VSTS enkele details voor het bevestigen van het account.

    ![Testmodus](./media/contoso-migration-tfs-vsts/test8.png)

13. In VSTS ziet de Lead ontwikkelen dat de projecten zijn gemigreerd naar VSTS. Er is een melding dat het account in 15 dagen worden verwijderd.

    ![Testmodus](./media/contoso-migration-tfs-vsts/test9.png)

14. De Dev Lead openen op een van de projecten en opent u **werkitems** > **aan mij toegewezen**. Hiermee wordt aangegeven dat item werkgegevens is gemigreerd, samen met de identiteit.

    ![Testmodus](./media/contoso-migration-tfs-vsts/test10.png)

15. De potentiële klant wordt ook gecontroleerd andere projecten en code, om te bevestigen dat de broncode en de geschiedenis is gemigreerd.

    ![Testmodus](./media/contoso-migration-tfs-vsts/test11.png)


### <a name="run-the-production-migration"></a>De productie-migratie uitvoeren

Het droge Run is voltooid verder beheerders van Contoso met de productie-migratie. Ze het droge uitvoeren verwijderen, bijwerken van de instellingen voor importeren en importeren opnieuw uitvoeren.

1. In de portal VSTS verwijderen ze de testmodus-account.
2. Bijwerken van het bestand import.json om in te stellen de **ImportType** naar **ProductionRun**.

    ![Productie](./media/contoso-migration-tfs-vsts/full1.png)

3. Ze de migratie te starten als ze hebben gedaan voor het uitvoeren van de testmodus: **TfsMigrator importeren /importFile:C:\TFSMigrator\import.json**.
4. Een bericht ziet u om te bevestigen van de migratie, en u wordt gewaarschuwd dat gegevens kan worden ondergebracht in een veilige locatie als een werkgebied voor maximaal zeven dagen.

    ![Productie](./media/contoso-migration-tfs-vsts/full2.png)

5. In Azure AD-aanmelding geeft ze een Contoso Admin-aanmelding.

    ![Productie](./media/contoso-migration-tfs-vsts/full3.png)

6. Een bericht toont informatie over het importeren.

    ![Productie](./media/contoso-migration-tfs-vsts/full4.png)

7. Na ongeveer 15 minuten, ze zoeken naar de URL en ziet u de volgende informatie:

    ![Productie](./media/contoso-migration-tfs-vsts/full5.png)

8. Nadat de migratie is voltooid voor een logboeken Contoso Dev leidt naar VSTS om te controleren of opgeleverd de migratie. Na aanmelding ziet de potentiële klant projecten zijn gemigreerd.

    ![Productie](./media/contoso-migration-tfs-vsts/full6.png)

8. De Dev Lead openen op een van de projecten en opent u **werkitems** > **aan mij toegewezen**. Hiermee wordt aangegeven dat item werkgegevens is gemigreerd, samen met de identiteit.

    ![Productie](./media/contoso-migration-tfs-vsts/full7.png)

9. De potentiële klant controleert overige werkgegevens item om te bevestigen.

    ![Productie](./media/contoso-migration-tfs-vsts/full8.png)

15. De potentiële klant wordt ook gecontroleerd andere projecten en code, om te bevestigen dat de broncode en de geschiedenis is gemigreerd.

    ![Productie](./media/contoso-migration-tfs-vsts/full9.png)


### <a name="move-source-control-from-tfvc-to-git"></a>Broncodebeheer verplaatsen van TFVC bent met GIT

Met de migratie is voltooid wil Contoso verplaatsen van TFVC naar Git voor broncodebeheer. Beheerders van Contoso moet de broncode op dit moment in het VSTS-account als Git-opslagplaatsen in hetzelfde account importeren.

1. In de portal VSTS ze een van de opslagplaatsen TFVC openen (**$/ PolicyConnect**) en te controleren.

    ![Git](./media/contoso-migration-tfs-vsts/git1.png)

2. Ze klikt u op de **bron** vervolgkeuzelijst > **importeren**.

    ![Git](./media/contoso-migration-tfs-vsts/git2.png)

3. In **gegevensbrontype** selecteren ze **TFVC**, en geef het pad naar de opslagplaats. Ze hebben besloten om niet te migreren van de geschiedenis.

    ![Git](./media/contoso-migration-tfs-vsts/git3.png)

    > [!NOTE]
    > Vanwege de verschillen in hoe TFVC en Git informatie over het beheer van versie opgeslagen, is het raadzaam dat Contoso-geschiedenis niet migreren. Dit is de benadering die Microsoft heeft genomen wanneer deze Windows en andere producten gemigreerd vanuit gecentraliseerd versiebeheer naar Git.

4. Na het importeren bekijken beheerders de code.

    ![Git](./media/contoso-migration-tfs-vsts/git4.png)

5. Ze het proces herhalen voor de tweede opslagplaats (**$/ SmartHotelContainer**).

    ![Git](./media/contoso-migration-tfs-vsts/git5.png)

6. Bekijk de bron en de Dev-Leads gaat ermee akkoord dat de migratie naar VSTS is uitgevoerd. VSTS wordt nu de bron voor de ontwikkeling van alle binnen teams die betrokken zijn bij de migratie.

    ![Git](./media/contoso-migration-tfs-vsts/git6.png)



**Meer hulp nodig?**

[Meer informatie](https://docs.microsoft.com/vsts/git/import-from-tfvc?view=vsts) over het importeren van TFVC.

##  <a name="clean-up-after-migration"></a>Na de migratie opschonen

Met de migratie is voltooid moet Contoso het volgende doen:

- Controleer de [na importeren](https://docs.microsoft.com/vsts/articles/migration-post-import?view=vsts) artikel voor informatie over het importeren van extra activiteiten.
- Verwijder de TFVC-opslagplaatsen of plaats deze in de modus alleen-lezen. De code bases kunnen niet gebruikt, maar kan worden verwezen voor de geschiedenis.

## <a name="next-steps"></a>Volgende stappen

Contoso moet VSTS en Git-training voor relevante teamleden opgeven.



