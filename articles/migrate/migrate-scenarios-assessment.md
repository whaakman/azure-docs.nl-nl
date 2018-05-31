---
title: On-premises workloads evalueren voor migratie naar Azure met DMA en Azure Migrate | Microsoft Docs
description: Informatie over hoe u Azure voorbereidt voor de migratie van on-premises machines met behulp van de DMS (Data Migration Assistant) en de Azure Migrate-service.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 05/18/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 0d8ef36e001aaf417b84efaf99a992fd64f01b6f
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34366338"
---
# <a name="scenario-1-assess-on-premises-workloads-for-migration-to-azure"></a>Scenario 1: On-premises workloads evalueren voor migratie naar Azure

Omdat Contoso migratie naar Azure overweegt, wil het bedrijf een technische en financiële evaluatie uitvoeren om na te gaan of hun on-premises werkbelastingen wel geschikt voor migratie naar de cloud. Ze willen met name de machine- en databasecompatibiliteit evalueren voor migratie en een schatting maken van de capaciteit en kosten voor het uitvoeren van hun resources in Azure.

Om praktijkervaring op te doen en meer inzicht te krijgen in de betreffende technologieën, gaan ze een kleine on-premises reis-app evalueren en migreren. Het is een app met twee lagen, waarbij een web-app wordt uitgevoerd op één virtuele machine en een SQL Server-database wordt uitgevoerd op de tweede virtuele machine. De toepassing wordt geïmplementeerd in VMware en de omgeving wordt beheerd door een vCenter Server. Ze gaan de beoordeling uitvoeren met behulp van de DMA (Data Migration Assistant) en de Azure Migrate-service. Als u dit scenario wilt uitproberen met deze reis-app, kunt u de app downloaden van [github](https://github.com/Microsoft/SmartHotel360).

**Technologie** | **Beschrijving** | **Kosten**
--- | --- | ---
[DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | DMA evalueert en detecteert compatibiliteitsproblemen die invloed kunnen hebben op de databasefunctionaliteit in Azure. Daarnaast wordt met DMA de functiepariteit tussen uw SQL Server-bron en -doel geëvalueerd en worden er aanbevelingen gedaan voor prestatie- en betrouwbaarheidsverbeteringen van de doelomgeving. | Het is een gratis hulpprogramma dat kan worden gedownload.
[Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-overview) | De service helpt u bij de evaluatie van on-premises computers voor migratie naar Azure. De service evalueert de geschiktheid voor migratie van de machines en biedt formaat- en kostenramingen voor de uitvoering in Azure. Op dit moment kunt u de service Azure Migrate gebruiken om on-premises VMware-VM's te evalueren voor migratie naar Azure. | Momenteel (april 2018) zijn er geen kosten verbonden aan het gebruik van deze service.
[Serviceoverzicht](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) | Azure Migrate maakt gebruik van Servicetoewijzing om afhankelijkheden weer te geven tussen de computers die u wilt migreren. |  Servicetoewijzing is een onderdeel van Azure Log Analytics. Het kan momenteel gedurende 180 dagen kosteloos worden gebruikt.

In dit scenario downloaden we DMA en voeren we het uit om de on-premises SQL Server-database voor onze reis-app te evalueren. We gebruiken Azure Migrate met afhankelijkheidstoewijzing om de VM's voor de app te evalueren voordat we ze migreren naar Azure.

> [!NOTE]
> Voor dit scenario gebruiken we 'SQL Server op een Azure-VM' als evaluatiedoel voor de database. In het volgende scenarioartikel wordt de migratie echter uitgevoerd naar een Azure SQL beheerd exemplaar. We gebruiken deze benadering omdat DMA momenteel geen ondersteuning biedt voor evaluatie naar een Azure SQL beheerd exemplaar als doel.

## <a name="architecture"></a>Architectuur


![Beoordelingsarchitectuur voor migratie](./media/migrate-scenarios-assessment/migration-assessment-architecture.png)

In dit scenario geldt het volgende:
- Contoso is een fictieve naam voor een gemiddelde onderneming. Contoso wil de eigen on-premises reis-app beoordelen en migreren. De app is gelaagd over twee VM's.
- Contoso heeft een on-premises datacenter (**contoso-datacenter**) met een on-premises domeincontroller (**contosodc1**).
- De interne reis-app is gelaagd over twee virtuele machines, **WEBVM** en **SQLVM**, en bevindt zich op VMware ESXi-host **contosohost1.contoso.com**.
- De VMware-omgeving wordt beheerd door vCenter Server (**vcenter.contoso.com**) die wordt uitgevoerd op een virtuele machine.




## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om dit scenario te implementeren:

- Een on-premises vCenter-server waarop versie 5.5, 6.0 of 6.5 wordt uitgevoerd.
- Een alleen-lezen-account op de vCenter-server of machtigingen om er een te maken.
- Machtigingen om een virtuele machine te maken op de vCenter-server met behulp van een OVA-sjabloon.
- Ten minste één ESXi-host waarop versie 5.0 of hoger wordt uitgevoerd.
- Ten minste twee on-premises virtuele VMware-machines, en op één daarvan moet een SQL Server-database worden uitgevoerd.
- U moet gemachtigd zijn om Azure Migrate-agents te installeren op elke virtuele machine.
- De virtuele machines moeten directe verbinding met internet hebben.
        - U kunt de toegang tot internet beperken de [vereiste URL's](https://docs.microsoft.com/azure/migrate/concepts-collector#collector-pre-requisites).
        - Als u VM's zonder internetverbinding hebt, moet u een [OMS-gateway](../log-analytics/log-analytics-oms-gateway.md) op deze machines downloaden en installeren.
- De FQDN-naam van de virtuele machine waarop het SQL Server-exemplaar wordt uitgevoerd. Deze wordt gebruikt voor database-evaluatie.
- Windows Firewall op de SQL Server-VM moet externe verbindingen toestaan via TCP-poort 1433 (standaard), zodat de DMA verbinding kan maken.


## <a name="scenario-overview"></a>Overzicht van scenario's

Hier ziet u wat we gaan doen:


> [!div class="checklist"]
> * **Stap 1: Azure voorbereiden**. U hebt alleen een Azure-abonnement nodig.
> * **Stap 2: DMA downloaden en installeren**: Bereid DMA voor op de evaluatie van de on-premises SQL Server-database.
> * **Stap 3: De database evalueren**: Voer de database-evaluatie uit en analyseer deze.
> * **Stap 4: De evaluatie van de virtuele machine met Azure Migrate voorbereiden**: Stel on-premises-accounts in en verfijn de VMware-instellingen.
> * **Stap 5: On-premises VM's detecteren**: Maak een collector-VM voor Azure Migrate. Voer vervolgens de collector uit om virtuele machines voor evaluatie te detecteren.
> * **Stap 6: De afhankelijkheidsanalyse voorbereiden**: Installeer Azure Migrate-agents op de virtuele machines, zodat u de afhankelijkheidstoewijzing tussen virtuele machines kunt zien.
> * **Stap 7: De virtuele machines beoordelen**: Controleer afhankelijkheden, groepeer de virtuele machines en voer de analyse uit. Wanneer de evaluatie is uitgevoerd, analyseert u deze ter voorbereiding van de migratie.


## <a name="step-1-prepare-azure"></a>Stap 1: Azure voorbereiden

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan.

- Als u een gratis account maakt, bent u de beheerder van uw abonnement en kunt u alle acties uitvoeren.
- Als u een bestaand abonnement hebt waarvan u niet de beheerder bent, moet u de beheerder vragen om u de machtiging Eigenaar of Bijdrager te geven voor het abonnement, of voor de resourcegroep die u voor dit scenario gebruikt.


## <a name="step-2-download-and-install-the-dma"></a>Stap 2: DMA downloaden en installeren

1. Download de DMA via het [Microsoft Downloadcenter](https://www.microsoft.com/download/details.aspx?id=53595).
    - U kunt de assistent installeren op elke computer die verbinding kan maken met het SQL-exemplaar. U hoeft deze niet uit te voeren op de SQL Server-computer.
    - Voer deze niet uit op de SQL Server-hostcomputer.
2. Dubbelklik op het gedownloade installatiebestand (DownloadMigrationAssistant.msi) om de installatie te starten.
3. Controleer op de pagina **Voltooien** of **Microsoft Data Migration Assistant starten** is geselecteerd en klik op **Voltooien**.

## <a name="step-3-run-and-analyze-the-database-assessment"></a>Stap 3: De evaluatie van de database uitvoeren en analyseren

Voer een evaluatie uit om uw bron-SQL Server-exemplaar te evalueren ten aanzien van een bepaald doel.

1. Selecteer in **Nieuw** de optie **Evaluatie** en geeft de evaluatie een projectnaam.
2. Selecteer in **Bronservertype** de optie **SQL Server**. Selecteer in **Doelservertype** de optie **SQL Server op Azure Virtual Machines**.

    ![Bron selecteren](./media/migrate-scenarios-assessment/dma-assessment-1.png)

    > [!NOTE]
      Op dit moment biedt DMA geen ondersteuning voor evaluatie voor migratie naar een SQL beheerd exemplaar. Als tijdelijke oplossing gebruiken we voor de evaluatie SQL Server op Azure VM als ons veronderstelde doel.

1.  Geef in **Doelversie selecteren** de doelversie van SQL Server op die u in Azure wilt uitvoeren en wat u in de evaluatie wilt detecteren:
    - In **Compatibiliteitsproblemen** vindt u informatie over de wijzigingen die de migratie kunnen verstoren, of over kleine aanpassingen die vóór de migratie zijn vereist. U vindt hier ook informatie over functies die u momenteel gebruikt en die zijn afgeschaft. Problemen zijn op compatibiliteitsniveau ingedeeld.
    - In **Aanbeveling voor nieuwe functies** vindt u informatie over nieuwe functies op het SQL Server-doelplatform die u na de migratie voor uw database kunt gebruiken. Deze zijn ingedeeld in Prestaties, Beveiliging en Opslag.

    ![Doel selecteren](./media/migrate-scenarios-assessment/dma-assessment-2.png)

2. Geef in **Verbinding maken met een server** de naam op van de computer waarop het SQL Server-exemplaar wordt uitgevoerd, het verificatietype en de verbindingsgegevens. Klik vervolgens op **Verbinden**.

    ![Doel selecteren](./media/migrate-scenarios-assessment/dma-assessment-3.png)

3. Selecteer in **Bron toevoegen** de database die u wilt evalueren en klik op **Toevoegen**.
4. Er wordt een evaluatie met de opgegeven naam gemaakt.

    ![Evaluatie maken](./media/migrate-scenarios-assessment/dma-assessment-4.png)

5.  **Volgende** om de evaluatie te starten.
6. In **Resultaten bekijken** ziet u de resultaten voor de evaluatietests die u hebt ingeschakeld.


### <a name="analyze-the-database-assessment"></a>De evaluatie van de database analyseren

Resultaten worden in de assistent weergegeven zodra ze beschikbaar zijn.

1. Controleer in rapport **Compatibiliteitsproblemen** op elk compatibiliteitsniveau of uw database problemen heeft en zo ja, hoe u deze kunt oplossen. Compatibiliteitsniveaus worden als volgt aan SQL Server-versies toegewezen:
    - 100: SQL Server 2008/Azure SQL Database
    - 110: SQL Server 2012/Azure SQL Database
    - 120: SQL Server 2014/Azure SQL Database
    - 130: SQL Server 2016/Azure SQL Database
    - 140: SQL Server 2017/Azure SQL Database

    ![Compatibiliteitsproblemen](./media/migrate-scenarios-assessment/dma-assessment-5.png)

2. In het rapport **Functieaanbevelingen** bekijkt u de prestatie-, beveiligings- en opslagfuncties die op basis van de evaluatie worden aanbevolen om na de migratie te configureren. Tal van functies worden aanbevolen, waaronder In-Memory OLTP en Columnstore, Stretch Database, Always Encrypted, Dynamic Data Masking en Transparent Data Encryption.

    ![Functieaanbevelingen](./media/migrate-scenarios-assessment/dma-assessment-6.png)

3. Als u eventuele problemen hebt opgelost, klikt u op **Evaluatie opnieuw starten** om deze opnieuw uit te voeren.
4. Klik op **Rapport exporteren** om het evaluatierapport in JSON- of CSV-indeling te converteren.

Als u een evaluatie op grotere schaal uitvoert:

- U kunt meerdere evaluaties tegelijk uitvoeren en de status van de evaluaties weergeven door de pagina **Alle evaluaties** te openen.
- U kunt [Evaluaties samenvoegen in een SQL Server-database](https://docs.microsoft.com/sql/dma/dma-consolidatereports?view=ssdt-18vs2017#import-assessment-results-into-a-sql-server-database).
- U kunt [Evaluaties samenvoegen in een PowerBI-rapport](https://docs.microsoft.com/sql/dma/dma-powerbiassesreport?view=ssdt-18vs2017).


## <a name="step-4-prepare-for-vm-assessment-with-azure-migrate"></a>Stap 4: VM-evaluatie met Azure Migrate voorbereiden

Maak een VMware-account dat door Azure Migrate wordt gebruikt om automatisch de virtuele machines voor evaluatie te detecteren. Controleer of u gemachtigd bent om een virtuele machine te maken. Noteer de poorten die geopend moeten zijn en stel het niveau van de instellingen voor statistieken in.

### <a name="set-up-a-vmware-account"></a>Een VMware-account instellen

 U hebt een alleen-lezen-account in vCenter nodig. Als u dat niet hebt, maak dan een VMware-account met de volgende eigenschappen:

- Gebruikerstype: minimaal een alleen-lezen-gebruiker.
- Machtigingen: Datacentrumobject –> Doorgeven naar onderliggend object, rol=Alleen-lezen.
- Details: gebruiker wordt toegewezen op datacentrumniveau, en heeft toegang tot alle objecten in het datacentrum.
- Wijs om de toegang te beperken de rol **Geen toegang** met het object **Doorgeven naar onderliggend object** toe aan de onderliggende objecten (vSphere-hosts, gegevensopslag, VM’s en netwerken).

### <a name="verify-permissions-to-create-a-vm"></a>Machtigingen voor het maken van een virtuele machine verifiëren

Controleer of u gemachtigd bent om een virtuele machine te maken door een bestand in OVA-indeling te importeren. [Meer informatie](https://kb.vmware.com/s/article/1023189?other.KM_Utility.getArticleLanguage=1&r=2&other.KM_Utility.getArticleData=1&other.KM_Utility.getArticle=1&ui-comm-runtime-components-aura-components-siteforce-qb.Quarterback.validateRoute=1&other.KM_Utility.getGUser=1).

### <a name="verify-ports"></a>Poorten verifiëren

In dit scenario gaat u de afhankelijkheidstoewijzing configureren. Voor deze functie moet een agent zijn geïnstalleerd op de virtuele machines die u wilt evalueren. Deze agent moet vanuit TCP-poort 443 op elke virtuele machine verbinding kunnen maken met Azure. [Meer informatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid) over verbindingsvereisten.


### <a name="set-statistics-settings"></a>Instellingen opgeven voor statistieken

Voordat u de implementatie start, moet u de instellingen voor statistieken voor de vCenter Server instellen op niveau 3. Houd rekening met het volgende:
- Nadat u het niveau hebt ingesteld, moet u minimaal een dag wachten voordat u de evaluatie uitvoert. Anders werkt deze mogelijk niet zoals verwacht.
- Als het niveau hoger is dan 3, werkt de beoordeling wel, maar:
    - Prestatiegegevens voor schijven en netwerken worden niet verzameld.
    - Voor opslag wordt door Azure Migrate een standaardschijf in Azure aanbevolen die even groot is als de on-premises schijf.
    - Voor netwerken wordt voor elke on-premises netwerkadapter een netwerkadapter in Azure aanbevolen.
    - Voor rekencapaciteit kijkt Azure Migrate naar de VM-kernen en de geheugengrootte en wordt een Azure-VM met dezelfde configuratie aanbevolen. Als er meer Azure VM-grootten in aanmerking komen, wordt de grootte met de laagste kosten aanbevolen.


[Meer informatie](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation#sizing) over groottebepaling op niveau 3.

Stel het niveau als volgt in:

1. Open het vCenter server-exemplaar in de vSphere-webclient.
2. Ga naar het tabblad **Beheren** en klik onder **Instellingen** op **Algemeen**.
3. Klik op **Bewerken** en stel in **Statistieken** de niveau-instellingen voor statistieken in op **Niveau 3**.

    ![Niveau van vCenter-statistieken](./media/migrate-scenarios-assessment/vcenter-statistics-level.png)



## <a name="step-5-discover-vms"></a>Stap 5: VM's detecteren

Maak een Azure Migrate-project en download en installeer de collector-VM. Voer daarna de collector uit om uw virtuele machines te detecteren.

### <a name="create-a-project"></a>Een project maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com) en klik op **Een resource maken**.
2. Zoek naar **Azure Migrate**. Selecteer **Azure Migrate**  in de zoekresultaten en klik op **Maken**.
3. Geef een projectnaam op en het Azure-abonnement.
4. Maak een nieuwe resourcegroep.
5. Geef de locatie op voor het project en klik op **Maken**.

    - U kunt alleen een Azure Migrate-project maken in de regio West-centraal VS of VS - oost.
    - U kunt een migratie plannen voor elke doellocatie.
    - De projectlocatie wordt alleen gebruikt om de metagegevens op te slaan die zijn verzameld van on-premises virtuele machines.

    ![Azure Migrate](./media/migrate-scenarios-assessment/project-1.png)




### <a name="download-the-collector-appliance"></a>Het collector-apparaat downloaden

Azure Migrate maakt een on-premises virtuele machine die het collector-apparaat wordt genoemd. Deze virtuele machine detecteert on-premises virtuele VMware-machines en stuurt metagegevens van deze machines naar de service Azure Migrate. Om het collector-apparaat in te stellen, downloadt u een .OVA-bestand en importeert u het op de on-premises vCenter Server om de virtuele machine te maken.

1. Klik in het Azure Migrate-project op **Aan de slag** > **Detecteren en evalueren** > **Machines detecteren**.
2. Klik in **Machines detecteren** op **Downloaden** om het .OVA-bestand te downloaden.
3. Kopieer de project-id en -sleutel uit **Projectreferenties kopiëren**. U hebt deze nodig tijdens de configuratie van collector.

    ![.OVA-bestand downloaden](./media/migrate-scenarios-assessment/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Het collector-apparaat verifiëren

Controleer of het .OVA-bestand veilig is voordat u het implementeert.

1. Open op de machine waarop u het bestand hebt gedownload een opdrachtvenster voor beheerders.
2. Gebruik de volgende opdracht om de hash voor het OVA-bestand te genereren:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Gebruiksvoorbeeld: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. De gegenereerde hash moet overeenkomen met deze instellingen (versie 1.0.9.8)

    **Algoritme** | **Hash-waarde**
    --- | ---
    MD5 | b5d9f0caf15ca357ac0563468c2e6251
    SHA1 | d6179b5bfe84e123fabd37f8a1e4930839eeb0e5
    SHA256 | 09c68b168719cb93bd439ea6a5fe21a3b01beec0e15b84204857061ca5b116ff


### <a name="create-the-collector-appliance"></a>Het collector-apparaat maken

Importeer het gedownloade bestand naar de vCenter Server.

1. Klik in de Client vSphere-console op **Bestand** > **OVF-sjabloon implementeren**.

    ![OVF implementeren](./media/migrate-scenarios-assessment/vcenter-wizard.png)

2. Geef in de wizard voor het implementeren van OVF-sjablonen > **Source** de locatie van het OVA-bestand op en klik op **Next**.
3. Klik in **OVF Template Details** op **Next**. Klik in **End User License Agreement** op **Accept** om de overeenkomst te accepteren en klik op **Next**.
4. Geef in **Name and Location** een beschrijvende naam op voor de collector-VM en het inventarisobject waarin de virtuele machine wordt gehost en klik op **Next**. Geef de host of het cluster op waarop de collector-VM wordt uitgevoerd.
5. Geef in **Storage** op waar u bestanden voor de VM wilt opslaan en klik op **Next**.
6. Geef in **Disk Format** op hoe u de opslag wilt inrichten.
7. Geef in **Netwerktoewijzing** het netwerk op waarmee de collector-VM verbinding maakt. Het netwerk moet verbinding hebben met internet om metagegevens te kunnen verzenden naar Azure.
8. Controleer de instellingen in **Ready to Complete**, selecteer **Power on after deployment** en klik op **Finish**.

Nadat het apparaat is gemaakt, wordt een bericht weergegeven dat alles met succes is voltooid.

### <a name="run-the-collector-to-discover-vms"></a>De collector uitvoeren om virtuele machines te detecteren

Opmerking vóór u begint: de collector ondersteunt alleen 'Engels (Verenigde Staten)' als de taal van het besturingssysteem en de taal van de gebruikersinterface van de collector.

1. Klik in de vSphere Client-console met de rechtermuisknop op de VM > **Console openen**.
2. Geef de voorkeursinstellingen voor de taal, de tijdzone en wachtwoorden op voor het apparaat.
3. Klik op het bureaublad op de snelkoppeling **Collector uitvoeren**.

    ![Snelkoppeling naar collector](./media/migrate-scenarios-assessment/collector-shortcut.png)

4. Open in de Azure Migrate Collector het onderdeel **Vereisten instellen**.
    - Accepteer de licentievoorwaarden en lees de informatie van derden.
    - De collector controleert of de virtuele machine toegang heeft tot internet, of de tijd wordt gesynchroniseerd en of de collectorservice wordt uitgevoerd (deze is standaard op de virtuele machine geïnstalleerd). De collector controleert ook of VMWare PowerCLI is geïnstalleerd.

    > [!NOTE]
    > We gaan ervan uit dat de virtuele machine rechtstreeks toegang heeft tot internet, zonder een proxy.

    ![Vereisten verifiëren](./media/migrate-scenarios-assessment/collector-verify-prereqs.png)


5. Doe het volgende in **vCenter Server-details opgeven**:
    - Geef de naam (FQDN) of het IP-adres op van de vCenter Server.
    - Geef voor **User name** en **Password** de referenties op voor het alleen-lezen-account dat de collector gebruikt om virtuele machines op de vCenter Server te detecteren.
    - Selecteer in **Select scope** een bereik voor VM-detectie. De collector kan alleen virtuele machines detecteren binnen het opgegeven bereik. U kunt het bereik instellen op een specifieke map, een datacenter of een cluster. Deze mag niet meer dan 1500 virtuele machines bevatten.

    ![Verbinding maken met vCenter](./media/migrate-scenarios-assessment/collector-connect-vcenter.png)

6. Geef in **Specify migration project** de Azure Migrate project-id en -sleutel op die u hebt gekopieerd in de portal. Als u deze niet hebt gekopieerd, opent u Azure Portal vanuit de collector-VM. Klik op de **overzichtspagina** van het project op **Machines detecteren** en kopieer de waarden.  

    ![Verbinding maken met Azure](./media/migrate-scenarios-assessment/collector-connect-azure.png)

7. In **View collection progress** bekijkt u de detectie en controleert u of metagegevens die vanuit de virtuele machines worden verzameld, zich binnen het bereik bevinden. De collector geeft aan hoe lang de detectie ongeveer zal duren.

    ![Bezig met verzamelen](./media/migrate-scenarios-assessment/collector-collection-process.png)



### <a name="verify-vms-in-the-portal"></a>VM's verifiëren in de portal

Wanneer de verzameling is voltooid, controleert u of de virtuele machines worden weergegeven in de portal.

1. Klik in het project Azure Migrate op **Beheren** > **Machines**.
2. Controleer of de virtuele machines die u wilt detecteren op de pagina worden weergegeven.

    ![Gedetecteerde computers](./media/migrate-scenarios-assessment/discovery-complete.png)

3. Let wel: op de VM's zijn momenteel geen Azure Migrate-agents geïnstalleerd. Deze moeten worden geïnstalleerd om afhankelijkheden te kunnen weergeven.

    ![Gedetecteerde computers](./media/migrate-scenarios-assessment/machines-no-agent.png)



## <a name="step-6-prepare-for-dependency-analysis"></a>Stap 6: Afhankelijkheidsanalyse voorbereiden

Om de afhankelijkheden te bekijken tussen virtuele machines die we willen evalueren, moeten we agents downloaden en installeren op de VM's (WEBVM en SQLVM) van de web-app.

### <a name="take-a-snapshot"></a>Een momentopname maken

Als u een kopie wilt maken van de virtuele machine voordat u deze wijzigt, maakt u een momentopname voordat u de agents installeert.

![Momentopname van VM](./media/migrate-scenarios-assessment/snapshot-vm.png)


### <a name="download-and-install-the-vm-agents"></a>De VM-agents downloaden en installeren

1.  Selecteer de VM op de pagina Azure Migrate-project > **Machines** en klik op **Vereist installatie** in de kolom **Afhankelijkheden**.
2.  Op de pagina **Machines detecteren** van elke virtuele machine downloadt en installeert u de MMA (Microsoft Monitoring Agent) en de agent voor afhankelijkheden.
3.  Kopieer de werkruimte-id en -sleutel. U hebt deze nodig tijdens de installatie van de MMA.

    ![Agent downloaden](./media/migrate-scenarios-assessment/download-agents.png)



#### <a name="install-the-mma"></a>De MMA installeren

1. Dubbelklik op de gedownloade agent.
2. Klik op de pagina **Welkom** op **Volgende**. Klik op de pagina **Licentievoorwaarden** op **Akkoord** om de licentie te accepteren.
3. Behoud in **Doelmap** de standaardinstallatiemap > **Volgende**.
4. Selecteer in **Installatieopties voor agent** de optie **De agent verbinden met Azure Log Analytics** > **Volgende**.

    ![MMA-installatie](./media/migrate-scenarios-assessment/mma-install.png)
5. In **Azure Log Analytics** plakt u de werkruimte-id en -sleutel die u in de portal hebt gekopieerd. Klik op **Volgende**.
    ![MMA-installatie](./media/migrate-scenarios-assessment/mma-install2.png)

6. Installeer de MMA in **Gereed voor installatie**.



#### <a name="install-the-dependency-agent"></a>De afhankelijkheidsagent installeren

1.  Dubbelklik op de gedownloade afhankelijkheidsagent.
2.  Klik op de pagina **Licentievoorwaarden** op **Akkoord** om de licentie te accepteren.
3.  Wacht tijdens het **installeren** totdat de installatie is voltooid. Klik op **Volgende**.

    ![Agent voor afhankelijkheden](./media/migrate-scenarios-assessment/dependency-agent.png)



## <a name="step-7-run-and-analyze-the-vm-assessment"></a>Stap 7: De evaluatie van de VM uitvoeren en analyseren

Controleer VM-afhankelijkheden en maak een groep. Voer daarna de evaluatie uit.

### <a name="verify-dependencies-and-create-a-group"></a>Controleer afhankelijkheden en maak een groep.

1.  Klik op de pagina **Machines** voor de virtuele machines die u wilt analyseren op **Afhankelijkheden weergeven**.

    ![Machineafhankelijkheden weergeven](./media/migrate-scenarios-assessment/view-machine-dependencies.png)

2. Voor de SQLVM bevat de afhankelijkheidskaart de volgende details:

    - Procesgroepen/processen met actieve netwerkverbindingen die worden uitgevoerd op SQLVM, tijdens de opgegeven periode (standaard een uur)
    - Inkomende (client) en uitgaande (server) TCP-verbindingen naar en van alle afhankelijke machines.
    - Afhankelijke machines waarop de Azure Migrate-agents zijn geïnstalleerd, worden als afzonderlijke vakken weergegeven
    - Voor machines waarop geen agents zijn geïnstalleerd, worden poort en IP-adres weergegeven.

 3. Voor machines waarop de agent is geïnstalleerd (WEBVM) klikt u op het VM-vak om meer informatie weer te geven, zoals de FQDN-naam, het besturingssysteem of het MAC-adres.

    ![Groepsafhankelijkheden weergeven](./media/migrate-scenarios-assessment/sqlvm-dependencies.png)

4. Selecteer nu de virtuele machines die u wilt toevoegen aan de groep (SQLVM en WEBVM).  Houd Ctrl ingedrukt terwijl u klikt als u meerdere virtuele machines wilt selecteren.
5. Klik op **Groep maken** en geef een naam op (smarthotelapp).

> [!NOTE]
    > Voor gedetailleerdere afhankelijkheden kunt u het tijdsbereik uitbreiden. U kunt een specifieke duur of begin- en einddatums selecteren.


### <a name="run-an-assessment"></a>Een evaluatie uitvoeren


1. De groep (smarthotelapp) openen op de pagina **Groepen** 
2. Klik op **Evaluatie maken**.

    ![Een evaluatie maken](./media/migrate-scenarios-assessment/run-vm-assessment.png)

3. De evaluatie wordt weergegeven op de pagina **Evaluaties** > **beheren**.


### <a name="modify-assessment-settings"></a>Evaluatie-instellingen wijzigen

Voor deze zelfstudie zijn de standaardevaluatie-instellingen gebruikt, maar u kunt instellingen als volgt aanpassen:

1. Selecteer de evaluatie op de pagina **Evaluaties** van het migratieproject en klik op **Eigenschappen bewerken**.
2. Wijzig de eigenschappen in overeenstemming met de volgende tabel:

    **Instelling** | **Details** | **Standaard**
    --- | --- | ---
    **Doellocatie** | De Azure-locatie waarnaar u wilt migreren | Geen standaard.
    **Opslagredundantie** | Het type opslagredundantie dat na de migratie door de virtuele Azure-machines wordt gebruikt. | De standaardwaarde is [Lokaal redundante opslag (LRS)](../storage/common/storage-redundancy-lrs.md). Azure Migrate ondersteunt alleen evaluaties op basis van beheerde schijven en beheerde schijven ondersteunen alleen LRS. Vandaar de optie LRS.
    **Criterium voor het aanpassen van de grootte** | Het door Azure Migrate te gebruiken criterium om VM's op de juiste grootte te maken voor Azure. U kunt de grootte bepalen *op basis van prestaties* of de grootte van de virtuele machines *als on-premises* bepalen, zonder rekening te houden de prestatiegeschiedenis. | Groottebepaling op basis van prestaties is de standaardoptie.
    **Prestatiegeschiedenis** | De duur die in overweging moet worden genomen voor de evaluatie van de prestaties van de virtuele machines. Deze eigenschap is alleen toepasbaar als het criterium voor groottebepaling *Groottebepaling op basis van prestaties* is. | De standaardoptie is één dag.
    **Percentiel gebruik** | De percentielwaarde van de prestatievoorbeeldset waarmee rekening moet worden gehouden voor een juiste groottebepaling. Deze eigenschap is alleen toepasbaar als het criterium voor groottebepaling *Groottebepaling op basis van prestaties* is.  | De standaardwaarde is 95e percentiel.
    **Prijscategorie** | U kunt de [prijscategorie (Basic/Standard)](../virtual-machines/windows/sizes-general.md) van de Azure-VM's opgeven. Als u bijvoorbeeld van plan bent om een productieomgeving te migreren, doet u er waarschijnlijk goed aan te kiezen voor de categorie Standard. Hiermee beschikt u over VM's met lage latentie, maar zijn de kosten mogelijk wel hoger. Hebt u daarentegen een ontwikkel-/testomgeving, dan is de categorie Basic wellicht de juiste keuze. Hierbij krijgt u VM's met een hogere latentie, maar lagere kosten. | Standaard wordt de categorie [Standard](../virtual-machines/windows/sizes-general.md) gebruikt.
    **Comfortfactor** | Tijdens de evaluatie houdt Azure Migrate rekening met een buffer (comfortfactor). Deze buffer wordt toegepast boven op de gegevens over machinegebruik voor VM's (CPU, geheugen, schijf en netwerk). De comfortfactor houdt rekening met factoren zoals seizoensgebonden gebruik, een korte prestatiegeschiedenis en een mogelijke gebruikstoename in de toekomst.<br/><br/> Een VM met 10 kernen en een gebruik van 20% komt bijvoorbeeld gewoonlijk overeen met een VM met 2 kernen. Met een comfortfactor van 2,0x is het resultaat echter een VM met 4 kernen. | De standaardinstelling is 1,3x.
    **Aanbieding** | [Azure-aanbieding](https://azure.microsoft.com/support/legal/offer-details/) waarvoor u zich hebt ingeschreven. | De standaardinstelling is [Betalen naar gebruik](https://azure.microsoft.com/offers/ms-azr-0003p/).
    **Valuta** | Factureringsvaluta. | De standaardinstelling is Amerikaanse dollars.
    **Korting (%)** | Een abonnement-specifieke korting die u bovenop de Azure-aanbieding ontvangt. | De standaardinstelling is 0%.
    **Azure Hybrid Benefit** | Geef op of u Software Assurance hebt en in aanmerking komt voor [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Als deze optie is ingesteld op Ja, worden niet-Windows Azure-prijzen voor Windows-VM's gehanteerd. | Standaard is Ja.

3. Klik op **Opslaan** om de evaluatie-instellingen bij te werken.


### <a name="analyze-the-vm-assessment"></a>De evaluatie van de VM analyseren

Een Azure Migrate-evaluatie bevat de volgende informatie: of de on-premises VM's compatibel zijn met Azure, wat de voorgestelde juiste grootte voor elke Azure-VM is en wat de geschatte maandelijkse kosten voor Azure zijn.

![Evaluatierapport](./media/migrate-scenarios-assessment/assessment-overview.png)

#### <a name="review-confidence-rating"></a>Betrouwbaarheidsclassificatie controleren

![Evaluatieweergave](./media/migrate-scenarios-assessment/assessment-display.png)

Uw evaluatie krijgt een betrouwbaarheidsclassificatie van 1 tot 5 sterren (waarbij 1 ster de laagste en 5 sterren de hoogste classificatie aangeeft).
- De betrouwbaarheidsclassificatie wordt aan een evaluatie toegewezen op basis van de beschikbaarheid van de gegevenspunten die nodig zijn om de evaluatie te berekenen.
- Aan de hand van deze classificatie kunt u beter de betrouwbaarheid inschatten van de aanbevelingen voor de grootte die Azure Migrate geeft.
- Betrouwbaarheidsclassificatie is nuttig als u *de grootte instelt op basis van prestaties*, omdat in Azure Migrate mogelijk niet voldoende gegevenspunten zijn om de grootte in te stellen op basis van gebruik. Voor *het instellen van de grootte op basis van 'zoals on-premises'* is de betrouwbaarheidsclassificatie altijd 5 sterren, omdat Azure Migrate alle benodigde gegevenspunten heeft om de grootte van de VM in te stellen.
- De betrouwbaarheidsclassificatie van de evaluatie wordt toegekend op basis van het percentage beschikbare gegevenspunten:

   **Beschikbaarheid van gegevenspunten** | **Betrouwbaarheidsclassificatie**
   --- | ---
   0%-20% | 1 ster
   21%-40% | 2 sterren
   41%-60% | 3 sterren
   61%-80% | 4 sterren
   81%-100% | 5 sterren

#### <a name="verify-readiness"></a>Gereedheid controleren

![Gereedheid evalueren](./media/migrate-scenarios-assessment/azure-readiness.png)  

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

![Gereedheid evalueren](./media/migrate-scenarios-assessment/azure-costs.png)

- Kostenschattingen worden berekend aan de hand van de aanbevelingen voor de grootte voor een machine.
- Geschatte maandelijkse kosten voor computing en opslag worden samengevoegd voor alle virtuele machines in de groep.


## <a name="conclusion"></a>Conclusie

In dit scenario hebben we het volgende gedaan:

> [!div class="checklist"]
> * De on-premises database geëvalueerd met het hulpprogramma DMA.
> * De on-premises VM's, met afhankelijkheidstoewijzing, geëvalueerd met de service Azure Migrate.
> * De beoordelingen gecontroleerd om na te gaan of de on-premises resources gereed zijn voor migratie naar Azure.

## <a name="next-steps"></a>Volgende stappen

We gaan nu verder met het volgende scenario, om een [lift-and-shift-migratie](migrate-scenarios-lift-and-shift.md) van de on-premises VM's en database naar Azure uit te voeren.
