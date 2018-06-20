---
title: Voor lokale werkbelastingen voor Contoso migratie naar Azure beoordelen | Microsoft Docs
description: Meer informatie over hoe Contoso hun lokale machines voor migratie naar Azure met Azure migratie en Database Migraton evalueert
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: raynew
ms.openlocfilehash: fb987c95afc0f77386f4f78c44f3c6825f86ee43
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232393"
---
# <a name="contoso-migration-assess-on-premises-workloads-for-migration-to-azure"></a>Migratie van Contoso: beoordelen voor lokale werkbelastingen voor migratie naar Azure

Dit artikel laat zien hoe Contoso evalueert de app van de lokale SmartHotel ter voorbereiding van de migratie naar Azure.

Dit document is de derde in reeks artikelen document hoe de lokale bronnen in het fictieve bedrijf Contoso worden gemigreerd naar de Microsoft Azure-cloud. De reeks bevat achtergrondinformatie en een reeks implementatiescenario's die aangeven hoe u een migratie-infrastructuur, beoordelen de geschiktheid van lokale bronnen voor migratie en verschillende soorten migraties worden uitgevoerd. Scenario's toenemen in complexiteit en worden er nog meer artikelen gedurende een bepaalde periode.

**Artikel** | **Details** | **Status**
--- | --- | ---
[Artikel 1: overzicht](contoso-migration-overview.md) | Biedt een overzicht van de strategie voor de migratie van Contoso, de serie artikelen en de voorbeeld-apps we gebruiken. | Beschikbaar
[Artikel 2: Een Azure-infrastructuur implementeren](contoso-migration-infrastructure.md) | Hierin wordt beschreven hoe Contoso de on-premises en Azure-infrastructuur voor migratie voorbereidt. De infrastructuur wordt gebruikt voor alle Contoso migratiescenario's. | Beschikbaar
Artikel 3: Beoordelen lokale bronnen (in dit artikel)  | Toont hoe een beoordeling van de lokale twee lagen SmartHotel app uitgevoerd in VMware in Contoso wordt uitgevoerd. Evalueren van de virtuele machines van een app met de [Azure migreren](migrate-overview.md) -service en de app SQL Server-database met de [Azure Database migratie-assistent](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Beschikbaar
[Artikel 4: Rehost virtuele machines in Azure en een beheerde SQL-exemplaar](contoso-migration-rehost-vm-sql-managed-instance.md) | Demonstreert hoe Contoso de app SmartHotel migreert naar Azure. Migreren van de app frontend virtuele machine met behulp [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), en de app-database met de [Azure databasemigratie](https://docs.microsoft.com/azure/dms/dms-overview) service om te migreren naar een SQL-exemplaar worden beheerd. | Beschikbaar
[Artikel 5: Rehost voor virtuele machines in Azure](contoso-migration-rehost-vm.md) | Toont hoe Contoso hun app SmartHotel met Site Recovery alleen virtuele machines migreert.
[Artikel 6: Rehost virtuele machines in Azure en SQL Server-beschikbaarheidsgroepen](contoso-migration-rehost-vm-sql-ag.md) | Toont hoe de app SmartHotel in Contoso worden gemigreerd. Deze Site Recovery gebruiken voor het migreren van virtuele machines van de app en de migratie van de Database-service om te migreren van de app-database naar een SQL Server-beschikbaarheidsgroep. | Beschikbaar
[Artikel 7: Een app Linux virtuele machines in Azure Rehost](contoso-migration-rehost-linux-vm.md) | Toont hoe Contoso hun osService Linux-app met Azure Site Recovery wordt gemigreerd.
[Artikel 8: Rehost een Linux-app voor virtuele machines in Azure en Azure MySQL-Server](contoso-migration-rehost-linux-vm-mysql.md) | Laat zien hoe de Contoso migreert de osService Linux-app met behulp van Site Recovery voor VM-migratie en MySQL-Workbench om te migreren (naar een exemplaar van Azure MySQL-Server. | Beschikbaar


## <a name="overview"></a>Overzicht

Geachte migratie naar Azure, wil Contoso bedrijf dat een technische en financiële analyse om te achterhalen of de lokale werkbelastingen geschikt voor migratie naar de cloud zijn uit te voeren. In het bijzonder wilt de Contoso-team beoordelen machine en de database compatibiliteit voor migratie en schatting capaciteit en de kosten voor het uitvoeren van hun bronnen in Azure.

Samengevat in de volgende tabel voor het ophalen van hun voeten NAT en de technologieën die betrokken zijn, gaan ze beoordelen twee van hun apps lokale beter te begrijpen. Houd er rekening mee dat ze voor migratiescenario's die rehost en verander apps voor migratie evalueren je. Meer informatie over configureren (rehost) en refactoring in de [Contoso Migratieoverzicht](contoso-migration-overview.md).

**Naam van app** | **Platform** | **App-categorieën** | **Details**
--- | --- | --- | ---
SmartHotel<br/><br/> Vereisten voor Contoso reizen beheert | In Windows wordt uitgevoerd met een SQL Server-database | Twee lagen app met de frontend-ASP.NET-website uitgevoerd op een virtuele machine (WEBVM) en de SQL-Server op een andere virtuele machine (SQLVM) wordt uitgevoerd | Virtuele machines zijn VMware, uitgevoerd op een ESXi-host worden beheerd door de vCenter-server.<br/><br/> De voorbeeld-app kan worden gedownload vanaf [GitHub](https://github.com/Microsoft/SmartHotel360).
OSTicket<br/><br/> Contoso Service helpdesk-app | Op Linux/Apache, met een MySQL-PHP (licht) uitgevoerd. | App met een frontend PHP-website op een virtuele machine (OSTICKETWEB) en de MySQL-database op een andere virtuele machine (OSTICKETMYSQL) met twee lagen | De app wordt gebruikt door de klant service-apps voor het bijhouden van problemen voor interne werknemers en externe klanten.<br/><br/> De voorbeeld-app kan worden gedownload vanaf [GitHub](https://github.com/osTicket/osTicket).

## <a name="current-architecture"></a>Huidige architectuur


Hier volgt een diagram die de huidige on-premises infrastructuur voor Contoso.

![Architectuur van Contoso](./media/contoso-migration-assessment/contoso-architecture.png)  

- Contoso heeft één belangrijkste datacenter zich bevinden in de plaats van New York in het oostelijk deel van de Verenigde Staten.
- Drie extra lokale takken hebben ze in de Verenigde Staten.
- Het belangrijkste datacenter is verbonden met internet via een fiber metro ethernet-verbinding (500 mbps).
- Elke vertakking is lokaal verbonden met internet met behulp van zakelijke klasse-verbindingen met IPSec VPN-tunnels terug naar het belangrijkste datacenter. Hierdoor hun gehele netwerk permanent zijn verbonden en optimaliseert de verbinding met internet.
- Het belangrijkste datacenter wordt volledig gevirtualiseerd met VMware. Ze hebben twee ESXi 6.5 virtualisatie-hosts worden beheerd door de vCenter-Server 6.5.
- Contoso maakt gebruik van Active Directory voor identiteitsbeheer en DNS-servers in het interne netwerk.
- Domeincontrollers in het datacenter wordt uitgevoerd op virtuele VMware-machines. De domeincontrollers op lokale vertakkingen uitgevoerd op fysieke servers.





## <a name="business-drivers"></a>Zakelijke drijfveren

De IT-leiding heeft nauw samengewerkt met hun zakelijke partners om te begrijpen wat het bedrijf wil bereiken met deze migratie:

- **Zakelijke groei adres**: Contoso groeit en als gevolg hiervan er druk op de on-premises systemen en infrastructuur.
- **De efficiëntie verhogen**: Contoso moet verwijderen van onnodige procedures en processen voor hun ontwikkelaars en gebruikers te stroomlijnen.  De bedrijfsbehoeften IT snel en niet afval tijd of geld, dus sneller leveren op klantvereisten.
- **Flexibiliteit te vergroten**: Contoso IT moet reageert op de behoeften van uw bedrijf. Moet mogelijk sneller dan de wijzigingen in de marketplace, zodat het slagen van een globale economie reageren.  Het mag niet ophalen in de manier waarop of een zakelijke upblokkering geworden.
- **Schaal**: als het bedrijf is groeit, Contoso IT-systemen die kunnen worden vergroot hetzelfde tempo moet opgeven.

## <a name="assessment-goals"></a>Assessment doelstellingen

Het team van de cloud Contoso is vastgemaakt omlaag doelstellingen voor hun beoordelingen migratie:

- Na de migratie moeten apps in Azure dezelfde prestatiemogelijkheden hebben als in hun on-premises VMWare-omgeving.  Verplaatsen naar de cloud betekent niet dat de appprestaties minder kritiek is.
- Contoso moet begrijpen van de compatibiliteit van hun toepassingen en databases met Azure-vereisten, evenals hun hosting opties in Azure.
- Contoso databasebeheer moet worden geminimaliseerd nadat apps zijn verplaatst naar de cloud.  
- Contoso wil weten niet alleen de migratie-opties, maar ook de kosten in verband met de infrastructuur nadat deze verplaatst naar de cloud.

## <a name="assessment-tools"></a>Analyseprogramma 's
Contoso maakt gebruik van Microsoft-hulpprogramma's voor de evaluatie. Deze hulpprogramma's zijn afgestemd op hun doelstellingen en moeten bieden alle informatie die ze nodig hebben.

**Technologie** | **Beschrijving** | **Kosten**
--- | --- | ---
[Database Migration Assistant (DMA)](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Ze DMA gebruikt om te beoordelen en het detecteren van compatibiliteitsproblemen die voor de databasefunctionaliteit in Azure gevolgen mogelijk. DMA evalueert functie pariteit tussen de SQL-bronnen en doelen en raadt aan om de prestaties en betrouwbaarheid verbeteringen. | Het is een gratis hulpprogramma dat kan worden gedownload.
[Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-overview) | Contoso gebruikt deze service vast te stellen hun virtuele VMware-machines. De service evalueert de geschiktheid voor migratie van de machines en biedt formaat- en kostenramingen voor de uitvoering in Azure.  | Er is momenteel (mogelijk 2018) geen kosten voor deze service wordt gebruikt.
[Serviceoverzicht](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) | Azure Migrate maakt gebruik van Servicetoewijzing om afhankelijkheden weer te geven tussen de computers die u wilt migreren. |  Servicetoewijzing is een onderdeel van Azure Log Analytics. Het kan momenteel gedurende 180 dagen kosteloos worden gebruikt.

In dit scenario Contoso downloadt en de DMA voor het evalueren van de lokale SQL Server-database voor hun app reizen wordt uitgevoerd. Gebruik van Azure met afhankelijkheid toewijzing voor het evalueren van de app VM's vóór de migratie naar Azure migreren.



## <a name="assessment-architecture"></a>Evaluatie-architectuur


![Beoordelingsarchitectuur voor migratie](./media/contoso-migration-assessment/migration-assessment-architecture.png)

- Contoso is de naam van een fictieve die vertegenwoordigt de organisatie van een typische bedrijf. 
- Contoso heeft een on-premises datacenter (**contoso datacenter**), met lokale domeincontrollers (CONTOSODC1, CONTOSODC2).
- Virtuele VMware-machines bevinden zich op een VMware ESXI-hosts met versie 6.5. Hosts: **contosohost1**, **contosohost2**
- De VMware-omgeving wordt beheerd door de vCenter-server 6.5 (**venter**, uitgevoerd op een virtuele machine.
- De SmartHotel reizen app:
    - De app is gelaagd over twee virtuele VMware-machines, **WEBVM** en **SQLVM**.
    - De virtuele machines zich bevinden op VMware ESXi-host **contosohost1.contoso.com**.
    - De virtuele machines worden uitgevoerd van Windows Server 2008 R2 Datacenter met SP1.
- De VMware-omgeving wordt beheerd door vCenter Server (**vcenter.contoso.com**) die wordt uitgevoerd op een virtuele machine.
- De OSTicket helpdesk-app Service:
    - De app is gelaagd over twee virtuele machines, **OSTICKETWEB** en **OSTICKETMYSQL**.
    - De virtuele machines worden uitgevoerd op Ubuntu Linux Server 16.04-LTS.
    - De OSTICKETWEB-VM wordt uitgevoerd Apache 2 en PHP 7.0.
    - De OSTICKETMYSQL VM wordt uitgevoerd, MySQL 5.7.22.

![Architectuur](./media/contoso-migration-assessment/architecture.png)


## <a name="prerequisites"></a>Vereisten

Ga als volgt Contoso (en u) voor de evaluatie moet:

- Eigenaar of bijdrager toegang voor het Azure-abonnement of voor een resourcegroep in de Azure-abonnement.
- Een on-premises vCenter-server waarop versie 5.5, 6.0 of 6.5 wordt uitgevoerd.
- Een alleen-lezen-account op de vCenter-server of machtigingen om er een te maken.
- Machtigingen om een virtuele machine te maken op de vCenter-server met behulp van een OVA-sjabloon.
- Ten minste één ESXi-host waarop versie 5.0 of hoger wordt uitgevoerd.
- Ten minste twee on-premises virtuele VMware-machines, en op één daarvan moet een SQL Server-database worden uitgevoerd.
- Machtigingen voor het migreren van Azure om agents te installeren op elke virtuele machine.
- De virtuele machines moeten directe verbinding met internet hebben.
        - U kunt de toegang tot internet beperken de [vereiste URL's](https://docs.microsoft.com/azure/migrate/concepts-collector#collector-pre-requisites).
        -Als machines zonder internetverbinding de [OMS gateway](../log-analytics/log-analytics-oms-gateway.md) moet worden geïnstalleerd op deze.
- De FQDN-naam van de virtuele machine waarop het SQL Server-exemplaar wordt uitgevoerd. Deze wordt gebruikt voor database-evaluatie.
- Windows Firewall op de SQL Server-VM moet externe verbindingen toestaan via TCP-poort 1433 (standaard), zodat de DMA verbinding kan maken.


## <a name="assessment-overview"></a>Evaluatie-overzicht

Hier volgt hoe Contoso gaat de beoordeling doen:


> [!div class="checklist"]
> * **Stap 1: Download en installeer DMA**: DMA voorbereiden voor de beoordeling van de lokale SQL Server-database.
> * **Stap 2: De database met DMA beoordelen**: uitvoeren en analyseren van de beoordeling van de database.
> * **Stap 3: Voorbereiden voor beoordeling van de virtuele machine met Azure migreren**: instellen van on-premises-accounts en tweak VMware instellingen.
> * **Stap 4: De lokale virtuele machines met het migreren van Azure detecteren**: Maak een collector voor het migreren van Azure VM. Ze voer vervolgens de collector voor het detecteren van virtuele machines voor evaluatie.
> * **Stap 5: Voorbereiden voor analyse van afhankelijkheid met Azure migreren**: agents migreren van Azure installeren op de virtuele machines, zodat ze afhankelijkheid toewijzing tussen virtuele machines kunnen zien.
> * **Stap 6: Het VMs met een Azure migreren beoordelen**: afhankelijkheden controleren, de virtuele machines te groeperen en de analyse uitvoeren. Nadat de beoordeling klaar is, analyseren ze ter voorbereiding op de migratie.


## <a name="step-1-download-and-install-the-dma"></a>Stap 1: Download en installeer de DMA

1. Contoso downloadt de DMA uit de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595).
    - De assistent kan worden geïnstalleerd op elke computer die verbinding met de SQL-exemplaar maken kan. U hoeft deze niet uit te voeren op de SQL Server-computer.
    - Deze mag niet worden uitgevoerd op de SQL Server-hostcomputer.
2. Ze het gedownloade installatiebestand (DownloadMigrationAssistant.msi) om de installatie te starten worden uitgevoerd.
3. Op de **voltooien** pagina ze selecteren **Start Microsoft Data migratie-assistent** voordat u de wizard voltooit.

## <a name="step-2-run-and-analyze-the-database-assessment-for-smarthotel"></a>Stap 2: Uitvoeren en de beoordeling van de database van de SmartHotel analyseren

Contoso kunt nu een beoordeling voor het analyseren van hun lokale SQL Server voor de app SmartHotel uitvoeren.

1. Klik in de Database migratie-assistent, ze op **nieuw**, selecteer **Assessment**, en geef de evaluatie een projectnaam - **SmartHotel**.
2. Ze selecteert de **server gegevensbrontype** als **SQL Server op Azure Virtual Machines**. 

    ![Bron selecteren](./media/contoso-migration-assessment/dma-assessment-1.png)

    > [!NOTE]
      Op dit moment biedt DMA geen ondersteuning voor evaluatie voor migratie naar een SQL beheerd exemplaar. Als tijdelijke oplossing Contoso gebruikt SQL Server op Azure VM als de veronderstelde doel voor de beoordeling.

3. In **doelversie Selecteer**, ze 2017 van SQL Server selecteren als de doelversie. Ze moeten naar Selecteer deze optie, omdat dit de versie die wordt gebruikt door SQL-exemplaar worden beheerd.
4. Ze selecteren voor het detecteren van informatie over compatibiliteit en nieuwe functies:
    - **Compatibiliteitsproblemen** Houd er rekening mee wijzigingen die migratie mogelijk verbroken of waarvoor een kleine aanpassing vóór de migratie is vereist. Het zorgt ervoor dat u geïnformeerd over alle functies die momenteel in gebruik die zijn afgeschaft. Problemen zijn op compatibiliteitsniveau ingedeeld.
    - **Nieuwe functies aanbeveling** opmerkingen bij de nieuwe functies in het doelplatform voor SQL Server die kunnen worden gebruikt voor de database na de migratie. Deze zijn ingedeeld in Prestaties, Beveiliging en Opslag.

    ![Doel selecteren](./media/contoso-migration-assessment/dma-assessment-2.png)

2. In **verbinding maken met een server**, ze de naam van de virtuele machine met de database en de referenties voor toegang tot deze invoeren. Ze nodig hebben om in te schakelen **vertrouwensrelatie servercertificaat** om ervoor te zorgen dat ze toegang krijgen tot de SQL-Server. Klik vervolgens zij op **Connect**.

    ![Doel selecteren](./media/contoso-migration-assessment/dma-assessment-3.png)

3. In **toevoegen bron**, ze de database te beoordelen en klik op toevoegen **volgende** starten van de evaluatie.
4. De beoordeling wordt gemaakt.
    
    ![Evaluatie maken](./media/contoso-migration-assessment/dma-assessment-4.png)

5. In **resultaten bekijken**, zien ze de resultaten van de beoordeling.


### <a name="analyze-the-database-assessment"></a>De evaluatie van de database analyseren

Resultaten worden weergegeven zodra ze beschikbaar zijn. Als er problemen zijn opgelost moet worden geklikt **Assessment opnieuw** opnieuw uit te voeren van de evaluatie.

1. In de **compatibiliteitsproblemen** rapporteren, deze problemen op elk compatibiliteitsniveau controleren. Compatibiliteitsniveaus worden als volgt aan SQL Server-versies toegewezen:

    - 100: SQL Server 2008/Azure SQL Database
    - 110: SQL Server 2012/Azure SQL Database
    - 120: SQL Server 2014/Azure SQL Database
    - 130: SQL Server 2016/Azure SQL Database
    - 140: SQL Server 2017/Azure SQL Database

    ![Compatibiliteitsproblemen](./media/contoso-migration-assessment/dma-assessment-5.png)

2. In de **functie aanbevelingen** rapport Contoso prestatie-, beveiligings- en functies die de beoordeling na de migratie aanbeveelt kunt bekijken. Tal van functies worden aanbevolen, inclusief In-geheugen OLTP en Columnstore, Stretch Database altijd versleuteld, dynamische-Gegevensmaskering en Transparent Data Encryption (TDE).

    ![Functieaanbevelingen](./media/contoso-migration-assessment/dma-assessment-6.png)

    > [!NOTE]
    > We raden aan dat Contoso [TDE kunt](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) voor alle SQL Server-databases en dit is zelfs meer kritieke wanneer databases bevinden zich in de cloud. TDE mag alleen worden ingeschakeld na de migratie. Als TDE al is ingeschakeld, moet u het certificaat of de asymmetrische sleutel verplaatsen naar de database master van de doelserver. [Meer informatie](https://docs.microsoft.com/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-2017).

2. De beoordeling in JSON- of CSV-indeling, kunnen zij exporteren.

Houd er rekening mee dat als u een grotere schaal uit te voeren met u kan:

- Meerdere beoordelingen gelijktijdig worden uitgevoerd en de status van de beoordelingen weergeven door het openen van de **alle beoordelingen** pagina.
- [Beoordelingen zijn tegengekomen in een SQL Server-database consolideren](https://docs.microsoft.com/sql/dma/dma-consolidatereports?view=ssdt-18vs2017#import-assessment-results-into-a-sql-server-database).
- [Beoordelingen zijn tegengekomen in een Power BI-rapport consolideren](https://docs.microsoft.com/sql/dma/dma-powerbiassesreport?view=ssdt-18vs2017).


## <a name="step-3-prepare-for-vm-assessment-with-azure-migrate"></a>Stap 3: Voorbereiden voor beoordeling van de virtuele machine met Azure migreren

Contoso moet een VMware-account die Azure migreren gebruiken voor het automatisch detecteren van virtuele machines voor de beoordeling, controleert u of machtigingen voor het maken van een VM maken Noteer de poorten die moeten worden geopend en de statistieken instellingen niveau instellen.

### <a name="set-up-a-vmware-account"></a>Een VMware-account instellen

 Detectie van de virtuele machine vereist een alleen-lezen-account in vCenter met de volgende eigenschappen: 

- Gebruikerstype: minimaal een alleen-lezen-gebruiker.
- Machtigingen: Datacentrumobject –> Doorgeven naar onderliggend object, rol=Alleen-lezen.
- Details: gebruiker wordt toegewezen op datacentrumniveau, en heeft toegang tot alle objecten in het datacentrum.
- Wijs om de toegang te beperken de rol **Geen toegang** met het object **Doorgeven naar onderliggend object** toe aan de onderliggende objecten (vSphere-hosts, gegevensopslag, VM’s en netwerken).

### <a name="verify-permissions-to-create-a-vm"></a>Machtigingen voor het maken van een virtuele machine verifiëren

Contoso Controleer of het machtigingen heeft voor een virtuele machine maken door het importeren van een bestand in. EICELLEN-indeling. [Meer informatie](https://kb.vmware.com/s/article/1023189?other.KM_Utility.getArticleLanguage=1&r=2&other.KM_Utility.getArticleData=1&other.KM_Utility.getArticle=1&ui-comm-runtime-components-aura-components-siteforce-qb.Quarterback.validateRoute=1&other.KM_Utility.getGUser=1).

### <a name="verify-ports"></a>Poorten verifiëren

De beoordeling van Contoso maakt gebruik van afhankelijkheid toewijzing. Deze functie moet een agent is geïnstalleerd op virtuele machines die u wilt beoordelen. De agent moet verbinding maken met Azure van TCP-poort 443 op elke virtuele machine. [Meer informatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid) over verbindingsvereisten.


### <a name="set-statistics-settings"></a>Instellingen opgeven voor statistieken

Voordat ze de implementatie begint, moet de instellingen van de statistieken voor de vCenter-Server in Contoso ingesteld op niveau 3. Houd rekening met het volgende:

- Na het instellen van het niveau, moet u wacht ten minste een dag voordat u de analyse uitvoeren. Anders werkt deze mogelijk niet zoals verwacht.
- Als het niveau hoger is dan 3, werkt de beoordeling wel, maar:
    - Prestatiegegevens voor schijven en netwerken worden niet verzameld.
    - Voor opslag wordt door Azure Migrate een standaardschijf in Azure aanbevolen die even groot is als de on-premises schijf.
    - Voor netwerken wordt voor elke on-premises netwerkadapter een netwerkadapter in Azure aanbevolen.
    - Voor rekencapaciteit kijkt Azure Migrate naar de VM-kernen en de geheugengrootte en wordt een Azure-VM met dezelfde configuratie aanbevolen. Als er meer Azure VM-grootten in aanmerking komen, wordt de grootte met de laagste kosten aanbevolen.
- [Meer informatie](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation#sizing) over groottebepaling op niveau 3.

Ze op het niveau als volgt instellen:

1. In de vSphere webclient open ze het vCenter server-exemplaar.
2. In **beheren** > **instellingen** > **algemene**, klikt de gebruiker op **bewerken**.
3. In **statistieken**, ze ingesteld de statistiek-instellingen op **niveau 3**.

    ![Niveau van vCenter-statistieken](./media/contoso-migration-assessment/vcenter-statistics-level.png)



## <a name="step-4-discover-vms"></a>Stap 4: VM's detecteren

Voor het detecteren van virtuele machines maakt Contoso een project Azure migreren. Ze downloaden en instellen van de VM-collector en de collector voor het detecteren van de bijbehorende lokale virtuele machines worden uitgevoerd.

### <a name="create-a-project"></a>Een project maken

1. In de [Azure-portal](https://portal.azure.com), ze zoeken naar **Azure migreren**, en maak een project (ContosoMigration).
2. Ze specificeert de projectnaam van een, het Azure-abonnement en maak een nieuwe Azure-resourcegroep, **ContosoFailoverRG**. Houd rekening met het volgende:

    - U kunt alleen een Azure Migrate-project maken in de regio West-centraal VS of VS - oost.
    - U kunt een migratie plannen voor elke doellocatie.
    - De projectlocatie wordt alleen gebruikt om de metagegevens op te slaan die zijn verzameld van on-premises virtuele machines.

    ![Azure Migrate](./media/contoso-migration-assessment/project-1.png)


### <a name="download-the-collector-appliance"></a>Het collector-apparaat downloaden

Azure Migrate maakt een on-premises virtuele machine die het collector-apparaat wordt genoemd. Deze virtuele machine detecteert on-premises virtuele VMware-machines en stuurt metagegevens van deze machines naar de service Azure Migrate. Als u het toestel collector instelt, Contoso downloadt een. EICELLEN sjabloon, en naar de lokale vCenter-server voor het maken van de virtuele machine worden geïmporteerd.

1. In het project Azure migreren > **aan de slag** > **Discover & schatten** > **Machines detecteren**, downloaden ze de. Het sjabloonbestand eicellen.
2. Zij kopiëren de project-ID en sleutel. Deze zijn nodig voor het configureren van de collector.

    ![.OVA-bestand downloaden](./media/contoso-migration-assessment/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Het collector-apparaat verifiëren

Voordat u de virtuele machine implementeert, Contoso controleert of de. EICELLEN bestand is beveiligd.

1. Op de computer waarop het bestand is gedownload, openen ze een administrator-opdrachtvenster.
2. Ze uitvoeren de volgende opdracht voor het genereren van de hash voor de eicellen:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Gebruiksvoorbeeld: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. De gegenereerde hash moet overeenkomen met deze instellingen (versie 1.0.9.7)

    **Algoritme** | **Hash-waarde**
    --- | ---
    MD5 | d5b6a03701203ff556fa78694d6d7c35
    SHA1 | f039feaa10dccd811c3d22d9a59fb83d0b01151e
    SHA256 | e5e997c003e29036f62bf3fdce96acd4a271799211a84b34b35dfd290e9bea9c


### <a name="create-the-collector-appliance"></a>Het collector-apparaat maken

Contoso kan nu het gedownloade bestand importeren in de vCenter-Server en de configuratieserver VM inrichten.

1. Klik in de console vSphere Client ze op **bestand** > **OVF-sjabloon implementeren**.

    ![OVF implementeren](./media/contoso-migration-assessment/vcenter-wizard.png)

2. In de Wizard OVF sjabloon implementeren > **bron**, ze Geef de locatie van de. EICELLEN-bestand.
3. In **naam en locatie**, ze Geef een beschrijvende naam voor de VM-collector en de inventaris-locatie waar de virtuele machine wordt gehost. Ze ook opgeven voor de host of cluster op waarop het toestel collector wordt uitgevoerd.
5. In **opslag**, ze geven de opslaglocatie en in **schijfindeling**, hoe ze willen inrichten van de opslag.
7. In **netwerktoewijzing**, ze geven het netwerk waarmee de VM-collector verbinding maakt. Het netwerk moet verbinding hebben met internet om metagegevens te kunnen verzenden naar Azure.
8. Ze Controleer de instellingen en selecteer **na de implementatie inschakelen**> **voltooien**. Nadat het apparaat is gemaakt, wordt een bericht weergegeven dat alles met succes is voltooid.

### <a name="run-the-collector-to-discover-vms"></a>De collector uitvoeren om virtuele machines te detecteren

Nu ze de collector voor het detecteren van virtuele machines worden uitgevoerd. Houd er rekening mee dat de collector momenteel alleen 'Engels (Verenigde Staten)' als de taal van het besturingssysteem en de taal van de gebruikersinterface van de collector ondersteunt.

1. In de vSphere Client-console > **Console openen**, ze geven de taal, de tijdzone en het wachtwoord voorkeuren voor de VM-collector.
2. Op het bureaublad, klikt de gebruiker op de **uitvoeren collector** snelkoppeling.

    ![Snelkoppeling naar collector](./media/contoso-migration-assessment/collector-shortcut.png)

4. In de Azure migreren Collector > **vereisten instellen**, ze accepteer de licentievoorwaarden en lees de informatie van derden.
5. De collector controleert of de virtuele machine toegang tot internet, dat de tijd is gesynchroniseerd en of de collectorservice wordt uitgevoerd (dit wordt standaard op de virtuele machine geïnstalleerd). Het VMWare PowerCLI ook geïnstalleerd.

    > [!NOTE]
    > We gaan ervan uit dat de virtuele machine rechtstreeks toegang heeft tot internet, zonder een proxy.

    ![Vereisten verifiëren](./media/contoso-migration-assessment/collector-verify-prereqs.png)


5. In **gegevens vCenter-Server opgeven**, ze geven de FQDN-naam of IP-adres van de vCenter-server en de alleen-lezen-referenties gebruikt voor detectie.
7. Ze selecteert u een bereik voor detectie van de virtuele machine. De collector kan alleen virtuele machines detecteren binnen het opgegeven bereik. U kunt het bereik instellen op een specifieke map, een datacenter of een cluster. Deze mag niet meer dan 1500 virtuele machines bevatten.

    ![Verbinding maken met vCenter](./media/contoso-migration-assessment/collector-connect-vcenter.png)

6. In **Geef migratieproject**, ze geven de project-ID Azure migreren en de sleutel die is opgehaald uit de portal. U kunt deze in het project opnieuw verkrijgen **overzicht** pagina > **Machines detecteren**.  

    ![Verbinding maken met Azure](./media/contoso-migration-assessment/collector-connect-azure.png)

7. In **verzameling voortgang** Contoso detectie bewaken en controleren metagegevens verzameld van de virtuele machines zijn binnen het bereik. De collector geeft aan hoe lang de detectie ongeveer zal duren.

    ![Bezig met verzamelen](./media/contoso-migration-assessment/collector-collection-process.png) 



### <a name="verify-vms-in-the-portal"></a>VM's verifiëren in de portal

Nadat de verzameling is voltooid, controleert Contoso dat de virtuele machines in de portal worden weergegeven.

1. In het project Azure migreren > **beheren** > **Machines**, ze de status van de virtuele machines die u wilt detecteren worden weergegeven.

    ![Gedetecteerde computers](./media/contoso-migration-assessment/discovery-complete.png)

3. Let wel: op de VM's zijn momenteel geen Azure Migrate-agents geïnstalleerd. Contoso moet worden geïnstalleerd in de afhankelijkheden die u wilt weergeven.

    ![Gedetecteerde computers](./media/contoso-migration-assessment/machines-no-agent.png)



## <a name="step-5-prepare-for-dependency-analysis"></a>Stap 5: Voorbereiden voor afhankelijkheid analyse

Als u wilt weergeven van afhankelijkheden tussen virtuele machines die Contoso wilt openen, ze downloaden en installeren van agents op de virtuele machines van de app. Contoso doet dit op alle virtuele machines voor hun apps, de Windows- en Linux.

### <a name="take-a-snapshot"></a>Een momentopname maken

Ze bewaren een kopie van de virtuele machine voordat u deze wijzigt door het maken van een momentopname voordat de agents zijn geïnstalleerd.

![Momentopname van VM](./media/contoso-migration-assessment/snapshot-vm.png)


### <a name="download-and-install-the-vm-agents"></a>De VM-agents downloaden en installeren

1. Op de **Machines** pagina ze de machine selecteren en vervolgens **vereist installatie** in de **afhankelijkheden** kolom.
2. Op de **Machines detecteren** pagina ze de volgende handelingen uit:
    - Downloaden van de agent MMA en afhankelijkheid voor elke virtuele machine van Windows
    - Download de agent MMA en afhankelijkheid voor elke Linux-VM
3. Nu kopiëren zij de werkruimte-ID en sleutel. Ze moeten deze bij het installeren van de MMA.

    ![Agent downloaden](./media/contoso-migration-assessment/download-agents.png)

### <a name="install-the-agents-on-windows-vms"></a>De agents installeren op VM's van Windows

Ze de installatie worden uitgevoerd op elke virtuele machine.

#### <a name="install-the-mma-on-windows-vms"></a>Installeer de MMA op VM's van Windows

1. Ze Dubbelklik op de gedownloade agent.
2. In **doelmap**, ze houden de standaardinstallatiemap > **volgende**.
2. In **installatieopties voor Agent**, selecteren ze **de agent verbinden met Azure-logboekanalyse** > **volgende**.

    ![MMA-installatie](./media/contoso-migration-assessment/mma-install.png)
    
5. In **Azure Log Analytics**, ze in de werkruimte-ID en sleutel die u hebt gekopieerd uit de portal te plakken. 

    ![MMA-installatie](./media/contoso-migration-assessment/mma-install2.png)

6. In **gereed voor installatie**, de MMA kan nu worden geïnstalleerd.

#### <a name="install-the-dependency-agent-on-windows-vms"></a>De afhankelijkheid-agent installeren op VM's van Windows

1. Ze Dubbelklik op de gedownloade afhankelijkheid-agent.
2. Deze accepteer de licentievoorwaarden en wacht totdat de installatie te voltooien.

    ![Agent voor afhankelijkheden](./media/contoso-migration-assessment/dependency-agent.png)


### <a name="install-the-agents-on-linux-vms"></a>De agents installeren op virtuele Linux-machines

Ze de installatie worden uitgevoerd op elke virtuele machine.

#### <a name="install-the-mma-on-linux-vms"></a>Installeer de MMA op virtuele Linux-machines

1. De bibliotheek van python ctypes worden geïnstalleerd op elke virtuele machine met behulp van: **apt sudo get-installeren van python-ctypeslib**.
2. Ze moeten de opdracht voor het installeren van de agent MMA als basis worden uitgevoerd.  Uitvoeren van de hoofdmap worden de volgende opdracht en voer het hoofdwachtwoord: **sudo -i**.
3. Nu dat ze de MMA-agent installeren.
    - Plaats het juiste werkruimte-ID en de sleutel in de opdracht.
    - Opdrachten zijn voor 64-bits.
    - De **werkruimte-ID** en **primaire sleutel** vindt u in de OMS-Portal > **instellingen**, in de **verbonden bronnen** tabblad.
    - Voer de volgende opdrachten om te downloaden van de OMS-agent, valideert de controlesom en installeren/vrijgeven de agent.

    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w 6b7fcaff-7efb-4356-ae06-516cacf5e25d -s k7gAMAw5Bk8pFVUTZKmk2lG4eUciswzWfYLDTxGcD8pcyc4oT8c6ZRgsMy3MmsQSHuSOcmBUsCjoRiG2x9A8Mg==
    ```
 


#### <a name="install-the-dependency-agent-on-linux-vms"></a>De afhankelijkheid-agent installeren op virtuele Linux-machines

Na de installatie van MMA kunt Contoso de afhankelijkheid-agent installeren op de virtuele Linux-machines.

1. De afhankelijkheid-Agent is geïnstalleerd op Linux-computers met behulp van InstallDependencyAgent-Linux64.bin, een shell-script met een zichzelf uitpakkend binaire waarde. Ze kunnen het bestand uit te voeren met behulp van servicel of Voeg schrijfrechten hebben voor het bestand zelf.

2. Installatie van de afhankelijkheid van Linux-agent als hoofdmap:

    ```
    wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin && sudo sh InstallDependencyAgent-Linux64.bin -s
    ```


## <a name="step-6-run-and-analyze-the-vm-assessment"></a>Stap 6: Uitvoeren en analyseren van de beoordeling van de virtuele machine

Contoso kan nu machine afhankelijkheden controleren en een groep maken. Ze voer vervolgens de beoordeling van de groep.

### <a name="verify-dependencies-and-create-a-group"></a>Controleer of de afhankelijkheden en een groep maken


1. Voor de machines analyseren, klikt de gebruiker op **afhankelijkheden weergeven**.

    ![Machineafhankelijkheden weergeven](./media/contoso-migration-assessment/view-machine-dependencies.png)

2. Voor de SQLVM bevat de afhankelijkheidskaart de volgende details:

    - Procesgroepen/processen met actieve netwerkverbindingen die worden uitgevoerd op SQLVM, tijdens de opgegeven periode (standaard een uur)
    - Inkomende (client) en uitgaande (server) TCP-verbindingen naar en van alle afhankelijke machines.
    - Afhankelijke machines waarop de Azure Migrate-agents zijn geïnstalleerd, worden als afzonderlijke vakken weergegeven
    - Voor machines waarop geen agents zijn geïnstalleerd, worden poort en IP-adres weergegeven.

3. Ze Klik op de machine-vak voor meer informatie, met inbegrip van de FQDN-naam, het besturingssysteem en het MAC-adres voor de machines waarop de agent is geïnstalleerd (WEBVM).

    ![Groepsafhankelijkheden weergeven](./media/contoso-migration-assessment/sqlvm-dependencies.png)

4. Nu selecteren ze de virtuele machines toevoegen aan de groep (SQLVM en WEBVM).  Ze kunnen gebruik CTRL + klikken om te selecteren van meerdere virtuele machines.
5. Ze klikt u op **groep maken**, en geef een naam (smarthotelapp).

> [!NOTE]
    > Voor gedetailleerdere afhankelijkheden kunt u het tijdsbereik uitbreiden. U kunt een specifieke duur of begin- en einddatums selecteren.


### <a name="run-an-assessment"></a>Een evaluatie uitvoeren


1. Op de **groepen** pagina ze open de groep (smarthotelapp) en klik op **assessment maken**.

    ![Een evaluatie maken](./media/contoso-migration-assessment/run-vm-assessment.png)

2. De evaluatie wordt weergegeven op de pagina **Evaluaties** > **beheren**.

Contoso gebruikt de standaardinstellingen van de evaluatie, maar u kunt instellingen aanpassen. [Meer informatie](how-to-modify-assessment.md).



### <a name="analyze-the-vm-assessment"></a>De evaluatie van de VM analyseren

Een beoordeling Azure migreren bevat informatie over de compatibiliteit van de lokale virtuele machines voor Azure, voorstel voor de juiste formaat voor de virtuele machine van Azure en de geschatte maandelijkse kosten van Azure.

![Evaluatierapport](./media/contoso-migration-assessment/assessment-overview.png)

#### <a name="review-confidence-rating"></a>Betrouwbaarheidsclassificatie controleren

![Evaluatieweergave](./media/contoso-migration-assessment/assessment-display.png)

Een beoordeling krijgt een betrouwbaarheid van 1 ster 5 ster (1 ster wordt de laagste en 5 ster de hoogste).
- De betrouwbaarheidsclassificatie wordt aan een evaluatie toegewezen op basis van de beschikbaarheid van de gegevenspunten die nodig zijn om de evaluatie te berekenen.
- Aan de hand van deze classificatie kunt u beter de betrouwbaarheid inschatten van de aanbevelingen voor de grootte die Azure Migrate geeft.
- Betrouwbaarheid is handig als u de *formaat op basis van prestaties* zoals Azure migreren mogelijk geen onvoldoende gegevenspunten doen formaat op basis van gebruik. Voor *het instellen van de grootte op basis van 'zoals on-premises'* is de betrouwbaarheidsclassificatie altijd 5 sterren, omdat Azure Migrate alle benodigde gegevenspunten heeft om de grootte van de VM in te stellen.
- De betrouwbaarheidsclassificatie van de evaluatie wordt toegekend op basis van het percentage beschikbare gegevenspunten:

   **Beschikbaarheid van gegevenspunten** | **Betrouwbaarheidsclassificatie**
   --- | ---
   0%-20% | 1 ster
   21%-40% | 2 sterren
   41%-60% | 3 sterren
   61%-80% | 4 sterren
   81%-100% | 5 sterren

#### <a name="verify-readiness"></a>Gereedheid controleren

![Gereedheid evalueren](./media/contoso-migration-assessment/azure-readiness.png)  

Het evaluatierapport toont de informatie die in de tabel is samengevat. Azure Migrate heeft de volgende informatie nodig om een op prestaties gebaseerde groottebepaling te kunnen weergeven. Als deze informatie niet kan worden verzameld, is de evaluatie van de grootte mogelijk niet juist.

- Verbruiksgegevens voor CPU en geheugen.
- Lees-/schrijf-IOPS en doorvoer voor elke schijf die is gekoppeld aan de VM.
- Informatie over netwerk-in/uit voor elke netwerkadapter die aan de VM is gekoppeld.


**Instelling** | **Indicatie** | **Details**
--- | --- | ---
**Gereed voor Azure van VM** | Geeft aan of de virtuele machine gereed is voor migratie | Mogelijke statussen:</br><br/>- Gereed voor Azure<br/><br/>- Gereed met voorwaarden <br/><br/>- Niet gereed voor Azure<br/><br/>- Gereedheid onbekend<br/><br/> Als een VM niet gereed is, ziet u met welke stappen u dit kunt verhelpen.
**Azure VM-grootte** | Voor virtuele machines die gereed zijn, wordt een Azure VM-grootte aanbevolen. | De aanbevolen grootte is afhankelijk van evaluatie-eigenschappen:<br/><br/>- Als u groottebepaling op basis van prestaties hebt gebruikt, wordt de prestatiegeschiedenis van de virtuele machines mee in overweging genomen.<br/><br/>- Als u 'als on-premises' hebt gebruikt, is de groottebepaling gebaseerd op de grootte van de on-premises VM's, en wordt geen rekening gehouden met verbruiksgegevens.
**Voorgesteld hulpprogramma** | Omdat de agents op de VM's worden uitgevoerd, wordt in Azure Migrate gekeken naar de processen die op de machine worden uitgevoerd en wordt bepaald of de machine een databasemachine is.
**VM-informatie** | Het rapport bevat instellingen voor de on-premises virtuele machine, waaronder informatie over het besturingssysteem, het opstarttype, schijven en opslag.


#### <a name="review-monthly-cost-estimates"></a>Schatting van maandelijkse kosten controleren

Deze weergave toont de totale compute- en opslagkosten om de virtuele machines in Azure uit te voeren, evenals de details per VM.

![Gereedheid evalueren](./media/contoso-migration-assessment/azure-costs.png)

- Kostenschattingen worden berekend aan de hand van de aanbevelingen voor de grootte voor een machine.
- Geschatte maandelijkse kosten voor computing en opslag worden samengevoegd voor alle virtuele machines in de groep.


## <a name="clean-up-after-assessment"></a>Na evaluatie opschonen

- Nadat de beoordeling is voltooid, wordt het toestel Azure migratie voor toekomstige evaluaties bewaard in Contoso.
- Ze uitschakelen de VMware virtuele machine. Ze moeten deze opnieuw starten wanneer dit beleid extra virtuele machines beoordeelt.
- De migratie van de Contoso-project houden zij in Azure.  Momenteel wordt geïmplementeerd in de resourcegroep ContosoFailoverRG in het Oost ons Azure-gebied.
-  De VM-collector heeft een licentie evaluatieperiode van 180 dagen. Als deze limiet is verlopen, moet ze downloaden en de collector opnieuw instellen.


## <a name="conclusion"></a>Conclusie

In dit scenario beoordeeld Contoso de SmartHotel app-database met het DMA-hulpprogramma en de lokale virtuele machines met behulp van de service Azure migreren. Ze vervolgens de beoordelingen om ervoor te zorgen dat lokale bronnen zijn gereed voor migratie naar Azure geëvalueerd.

## <a name="next-steps"></a>Volgende stappen

In het volgende artikel in deze reeks naamconflicten Contoso de SmartHotel-app in Azure met een lift en shift-migratie. Contoso migreert de frontend WEBVM voor de app met Azure Site Recovery en de app-database naar een Azure SQL beheerd-exemplaar, met behulp van de Database-Service voor migratie. [Aan de slag](contoso-migration-rehost-vm-sql-managed-instance.md) met deze implementatie.
