---
title: Best practices voor kosten en het formaat van werkbelastingen naar Azure gemigreerd | Microsoft Docs
description: Aanbevolen procedures voor het stuk ophalen en grootte van werkbelastingen naar Azure gemigreerd.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 12/08/2018
ms.author: raynew
ms.openlocfilehash: b8ff1ae2f4c07dc59bd1ffb631378817493b96b0
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "53995040"
---
# <a name="best-practices-for-costing-and-sizing-workloads-migrated-to-azure"></a>Aanbevolen procedures voor workloads met kosten en sizing gemigreerd naar Azure

Als u plannen en ontwerpen voor migratie, ligt de focus op kosten zorgt u ervoor de succes op lange termijn van uw Azure-migratie. Tijdens een migratieproject is het essentieel dat alle teams (financiën, beheer, app-teams enzovoort) te begrijpen bijbehorende kosten.
- Schatting van de migratie van uw clouduitgaven, met een basislijn voor maandelijks, elk kwartaal, voorafgaand aan de migratie en jaarlijkse budget doelen is essentieel voor succes.
- Na de migratie, moet u kosten optimaliseren, workloads continu bewaken en plannen voor toekomstige gebruikspatronen. Gemigreerde bronnen kunnen beginnen als een soort werkbelasting, maar in een ander type loop der tijd veranderen, op basis van gebruik, kosten en verschuivende zakelijke vereisten.

Dit artikel wordt beschreven aanbevolen procedures voor kosten en vergroten/verkleinen vóór en na de migratie.  

> [!IMPORTANT]
> De aanbevolen procedures en adviezen die worden beschreven in dit artikel zijn gebaseerd op Azure-platform en service-functies die beschikbaar zijn op het moment van schrijven. Functies en mogelijkheden veranderen verloop van tijd. Niet alle aanbevelingen mogelijk van toepassing is voor uw implementatie, dus klik op wat voor u werkt.


## <a name="before-migration"></a>Vóór de migratie 

Voordat u uw werkbelastingen naar de cloud verplaatsen, maak een schatting van de maandelijkse kosten van het uitvoeren van deze in Azure. Cloudkosten proactief beheren, kunt u voldoen aan uw operationele uitgaven (OpEx) budget. Als budget beperkt is, kunt u dit vóór de migratie rekening houden.  Houd rekening met workloads converteren naar serverloze Azure-technologieën, indien van toepassing, om kosten te verlagen.

De aanbevolen procedures in deze sectie helpt u bij de kosten schatten, tot de benodigde grootte voor virtuele machines en opslag uitvoeren, gebruikmaken van Azure Hybrid benefits, gereserveerde virtuele machines gebruiken en maak een schatting van cloud uitgaven voor abonnementen.



## <a name="best-practice-estimate-monthly-workload-costs"></a>Aanbevolen: Schatting maandelijkse kosten van de werkbelasting
 
Er zijn een aantal hulpprogramma's die kunt u voor een prognose van uw maandelijkse factuur voor de gemigreerde werkbelasting.

- **Azure prijscalculator**: U selecteert de producten die u wenst te schatten, bijvoorbeeld virtuele machines en opslag. Er worden kosten in de prijscalculator, voor het bouwen van een schatting ingevoerd.

 ![Azure prijscalculator](./media/migrate-best-practices-costs/pricing.png) *prijscalculator van Azure*

- **Azure Migrate**: Voor een schatting van de kosten die u wilt controleren en voor alle resources die zijn vereist voor het uitvoeren van uw workloads in Azure-account. Om te verkrijgen van deze gegevens, maakt u de inventaris van uw assets, met inbegrip van servers, virtuele machines, databases en opslag. U kunt Azure Migrate gebruiken om deze informatie te verzamelen.

 - Azure Migrate wordt gedetecteerd en uw on-premises omgeving voor een inventaris beoordeelt.
 - Azure Migrate kunt toewijzen en u afhankelijkheden tussen virtuele machines weergeven zodat u beschikt over een volledig overzicht.
 - Een Azure Migrate-evaluatie bevat de geschatte kosten.
    - De rekenkosten: Met behulp van de Azure-VM-grootte aanbevolen wanneer u een evaluatie maken, Azure Migrate maakt gebruik van de facturering-API voor het berekenen van de geschatte maandelijkse kosten voor virtuele machine. De schatting van de rekening gehouden met het besturingssysteem, software assurance, gereserveerde instanties, VM uptime, locatie en valuta-instellingen. Het combineert de kosten voor alle virtuele machines in de evaluatie en een totale maandelijkse kosten worden berekend.
    - Kosten voor gegevensopslag: Azure Migrate berekent de totale maandelijkse kosten voor opslag door samenvoeging van de kosten voor opslag van alle virtuele machines in een evaluatie. U kunt de maandelijkse opslagkosten voor voor een specifieke machine berekenen door samenvoeging van de maandelijkse kosten van alle gekoppelde schijven. 

    ![Azure Migrate](./media/migrate-best-practices-costs/assess.png) *Azure Migrate-evaluatie*

**Meer informatie:**
- [Gebruik](https://azure.microsoft.com/pricing/calculator/) de prijscalculator van Azure.
- [Bekijk een overzicht](https://docs.microsoft.com/azure/migrate/migrate-overview) van Azure Migrate.
- [Meer informatie over](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation) Azure Migrate-evaluaties.
- [Meer informatie](https://docs.microsoft.com/azure/dms/dms-overview) over Database Migration Service (DMS).

## <a name="best-practice-right-size-vms"></a>Aanbevolen: Virtuele machines de juiste grootte

U kunt een aantal opties kiezen bij het implementeren van virtuele Azure-machines om workloads te ondersteunen. Elk VM-type heeft specifieke functies en verschillende combinaties van CPU, geheugen en schijven. Virtuele machines worden als volgt gegroepeerd.

**Type** | **Details** | **Gebruiken**
--- | --- | ---
**Algemeen doel** | Evenwichtige CPU-geheugenverhouding. | Goed voor testen en ontwikkelen, kleine tot middelgrote databases, met weinig of gemiddeld verkeer webservers.
**Geoptimaliseerde rekenkracht** | Hoge CPU-geheugenverhouding. | Goed voor middelgrootte webserver, netwerkapparatuur, batchprocessen, app-servers.
**Geoptimaliseerd voor geheugen** | Hoge geheugen-naar-CPU. | Goed voor relationele databases, middelgrote tot grote cache, in-memory analyse.
**Geoptimaliseerde opslag** | Snelle doorvoer van schijfgegevens en IO. | Goed voor big data, SQL en NoSQL-databases.
**Geoptimaliseerde GPU** | Gespecialiseerde VM's. Één of meerdere GPU's. | Zware grafische afbeeldingen en video bewerken.
**Hoge prestaties** | Snelste en krachtigste CPU. VM's met optionele hoge doorvoer netwerkinterfaces (RDMA) | Essentiële hoogwaardige apps.

- Het is belangrijk om te begrijpen van de prijsverschillen tussen deze VM's en de effecten op lange termijn budget.
- Elk type heeft een aantal VM-reeks in het.
- Bovendien wanneer u een virtuele machine binnen een reeks selecteert, u kunt alleen de virtuele machine omhoog en omlaag schalen binnen de reeks. Bijvoorbeeld, een DSv2\_2 omhoog schalen tot DSv2\_4, maar het kan niet worden gewijzigd in een andere reeks zoals Fsv2\_2.

**Meer informatie:**
- [Meer informatie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) over VM-typen en -grootte en -grootten van de kaart te typen.
- [Plan](https://docs.microsoft.com/azure/cloud-services/cloud-services-sizes-specs) VM-grootte.
- [Beoordeling](https://docs.microsoft.com/azure/migrate/contoso-migration-assessment) een voorbeeldevaluatie van het voor het fictieve bedrijf Contoso.

## <a name="best-practice-select-the-right-storage"></a>Aanbevolen: Selecteer de juiste opslag

Afstemmen en onderhouden van on-premises opslag (SAN- of NAS), en de netwerken voor de ondersteuning hiervan, is kostbaar en tijdrovend. Gegevens uit een bestand (opslag) is vaak gemigreerd naar de cloud om te verlichten operationele en beheerproblemen. Microsoft biedt verschillende opties voor het verplaatsen van gegevens naar Azure, en moet u beslissingen neemt over deze opties. Verzamelen van het juiste opslagtype voor de gegevens kunt opslaan van uw organisatie verschillende duizenden euro's elke maand. Enkele overwegingen:

- Gegevens die niet worden geopend veel, en niet bedrijfskritisch is, hoeft niet te worden geplaatst op de meest dure opslag.
- Daarentegen moeten belangrijke kritieke zakelijke gegevens zich bevinden op een hogere laag opslagopties.
- Tijdens de migratie plannen, een inventarisatie maken van gegevens en het classificeren met urgentie, naar de meest geschikte opslag worden toegewezen. Houd rekening met budget en kosten, evenals de prestaties. Kosten mag per se niet zijn de belangrijkste factor voor besluitvorming. Verzamelen van de minst dure optie kan ertoe leiden dat de werkbelasting op de prestaties en beschikbaarheid van risico's. 

### <a name="storage-data-types"></a>Opslag-gegevenstypen

Azure biedt verschillende typen gegevens in de opslag.

**Gegevenstype** | **Details** | **Gebruik** 
--- | --- |  ---
**Blobs** | Geoptimaliseerd voor het opslaan van grote hoeveelheden ongestructureerde objecten, zoals tekst of binaire gegevens<br/><br/> | Toegang tot gegevens vanaf elke locatie via HTTP/HTTPS. | Gebruik voor scenario's voor streamen en willekeurige toegang. Bijvoorbeeld behoeve van afbeeldingen en documenten rechtstreeks aan een browser, video en audio streamen en opslaan van gegevens voor herstel na noodgevallen en back-ups.
**Bestanden** | Beheerde bestandsshares via SMB 3.0 | Gebruik bij het migreren van on-premises bestandsshares, en om meerdere toegang/verbindingen met gegevens uit een bestand.
**Schijven** | Gebaseerd op de pagina-blobs.<br/><br/> Schijftype (snelheid): Standaard (HDD of SSD) of Premium (SSD).<br/><br/>Schijfbeheer: Niet-beheerde (u beheert instellingen voor schijven en opslag) of beheerd (selecteert u het schijftype en Azure beheert de schijf voor u). | Premium-schijven voor virtuele machines gebruiken. Beheerde schijven gebruiken voor eenvoudig beheer en schalen.
**wachtrijen** | Store en op te halen van grote aantallen berichten benaderd via geverifieerde oproepen (HTTP of HTTPS) | Verbinding maken met app-onderdelen met asynchrone message Queuing.
**Tabellen** | Store tabellen. | Nu onderdeel van Azure Cosmos DB Table-API.



### <a name="access-tiers"></a>Toegangslagen
Azure storage biedt verschillende opties voor toegang tot blob-gegevens. De juiste toegangslaag te selecteren, zorgt ervoor dat blok-blob-gegevens op te slaan in de meest kostenefficiënte manier.

**Type** | **Details** | **Gebruik**
--- | --- | ---
**hot** | Hogere opslagkosten dan ' Cool '. Lagere kosten voor gegevenstoegang dan ' Cool '.<br/><br/>Dit is de standaardlaag. | Gebruiken voor gegevens die actief worden gebruikt die regelmatig worden geopend.
**' Cool '** | Lagere opslagkosten kosten dan warm. Kosten voor gegevenstoegang hoger dan warm.<br/><br/> Store voor minimaal 30 dagen. | Store op korte termijn, gegevens zijn beschikbaar, maar gebruikt niet regelmatig worden.
**Archief** | Gebruikt voor afzonderlijke blok-blobs.<br/><br/> Meest voordelige optie voor de opslag. Toegang tot gegevens is duurder dan hot en cold. | Gebruik voor de gegevens die kan tolereren server uren latentie bij het ophalen en blijven in de laag voor ten minste 180 dagen. 

### <a name="storage-account-types"></a>Storage-accounttypen

Azure biedt verschillende soorten opslagaccounts en prestatielagen.

**Accounttype** | **Details** | **Gebruik**
--- | --- | ---
**Algemeen gebruik v2 Standard** | Biedt ondersteuning voor blobs (blokkeren, pagina, toevoegen), bestanden, schijven, wachtrijen en tabellen.<br/><br/> Biedt ondersteuning voor laag Hot, Cool en Archive. ZRS wordt ondersteund. | Gebruik voor de meeste scenario's en de meeste typen gegevens. Standard storage-accounts kunnen worden harde schijf of op basis van SSD.
**Algemeen gebruik v2 Premium** | Biedt ondersteuning voor Blob storage-gegevens (pagina-blobs). Biedt ondersteuning voor laag Hot, Cool en Archive. ZRS wordt ondersteund.<br/><br/> Opgeslagen op SSD. | Microsoft adviseert om voor alle virtuele machines.
**Algemeen gebruik v1** | Toegang tot meerdere lagen wordt niet ondersteund. Biedt geen ondersteuning voor ZRS | Gebruik deze optie als apps nodig hebben, het model van de klassieke Azure-implementatie.
**Blob** | Gespecialiseerde storage-account voor het opslaan van ongestructureerde objecten. Biedt blok-blobs en toevoeg-blobs alleen (Er is geen bestand, wachtrij, tabel of schijf storage-services). Biedt de dezelfde duurzaamheid, beschikbaarheid, schaalbaarheid en prestaties als algemeen gebruik v2. | u pagina-blobs niet opslaan in deze accounts en daarom kan VHD-bestanden niet opslaan. U kunt een toegangslaag Hot of Cool instellen.

### <a name="storage-redundancy-options"></a>Opties voor opslagredundantie

Storage-accounts kunt verschillende soorten redundantie voor herstelvermogen en hoge beschikbaarheid.

**Type** | **Details** | **Gebruik**
--- | --- | ---
**Lokaal redundante opslag (LRS)** | Beschermt tegen uitval van een lokale door te repliceren in een enkele opslageenheid op een afzonderlijke foutdomein en updatedomein. Meerdere kopieën van uw gegevens blijven in één datacenter. Biedt ten minste 99,999999999% (11 9\'s) duurzaamheid van objecten in een bepaald jaar. | U kunt als uw app worden gegevens opgeslagen die eenvoudig kunnen worden gerepareerd.
**Zone-redundante opslag (ZRS)** | Beschermt opnieuw een uitval van het datacenter te repliceren in meerdere drie opslagclusters in één regio. Elke opslagcluster is fysiek gescheiden en zich in een eigen binnen een beschikbaarheidszone. Biedt ten minste 99,9999999999% (12 9\'s) duurzaamheid van objecten in een bepaald jaar door meerdere kopieën van uw gegevens op meerdere datacenters of regio's. | Houd rekening met als u consistentie, duurzaamheid en hoge beschikbaarheid. Kan geen bescherming tegen een regionaal noodgeval als meerdere zones permanent worden beïnvloed.
**Geografisch redundante opslag (GRS)** | Worden beschermd tegen uitval van een hele regio repliceren van gegevens naar een secundaire regio die honderden mijl weg van de primaire. Biedt ten minste 99,99999999999999% (16-9\'s) duurzaamheid van objecten in een bepaald jaar. | Replicagegevens is niet beschikbaar tenzij Microsoft een failover naar de secundaire regio initieert. Als failover optreedt, is lees- en schrijftoegang beschikbaar.
**Geografisch redundante opslag met leestoegang (RA-GRS)** | Dit is vergelijkbaar met GRS. Biedt ten minste 99,99999999999999% (16-9\'s) duurzaamheid van objecten in een bepaald jaar | Biedt en 99,99% beschikbaarheid voor lezen doordat leestoegang van de tweede regio die wordt gebruikt voor GRS.

**Meer informatie:**
- [Beoordeling](https://azure.microsoft.com/pricing/details/storage/) prijzen voor Azure Storage.
- [Meer informatie over](https://docs.microsoft.com/azure/storage/common/storage-import-export-service) Azure Import/Export voor migratie van grote hoeveelheden gegevens naar de Azure-blobs en -bestanden. 
- [Vergelijk](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) blobs, bestanden en gegevens van schijf opslagtypen.
- [Meer informatie](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers) over toegangslagen.
- [Beoordeling](https://docs.microsoft.com/azure/storage/common/storage-account-overview?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) verschillende soorten opslagaccounts.
- Meer informatie over [opslagredundantie](https://docs.microsoft.com/azure/storage/common/storage-redundancy), [LRS](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs?toc=%2fazure%2fstorage%2fqueues%2ftoc.json), [ZRS](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs?toc=%2fazure%2fstorage%2fqueues%2ftoc.json), [GRS](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs?toc=%2fazure%2fstorage%2fqueues%2ftoc.json), en [Read-access GRS](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs?toc=%2fazure%2fstorage%2fqueues%2ftoc.json#read-access-geo-redundant-storage).
- [Meer informatie](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) over Azure Files.



## <a name="best-practice-leverage-azure-hybrid-benefits"></a>Aanbevolen: Maak gebruik van Azure Hybrid benefits

Microsoft is vanwege een jaar van de software-investeringen in systemen, zoals Windows Server en SQL Server in een unieke positie te bieden klanten waarde in de cloud, met aanzienlijke kortingen die andere cloudproviders niet per se biedt. 

Een geïntegreerde Microsoft on-premises/Azure-productaanbod genereert concurrerende en kosten voordelen. Als u momenteel een besturingssysteem of andere softwarelicenties via software assurance (SA) hebt, kunt u deze licenties uitvoeren met u naar de cloud voor met Azure Hybrid Benefit.

**Meer informatie:**

- [Kijk eens](https://azure.microsoft.com/pricing/hybrid-benefit/) de Besparingscalculator Hybrid Benefit.
- [Meer informatie](https://azure.microsoft.com/pricing/hybrid-benefit/) over Hybrid Benefit voor Windows Server.
- [Beoordeling](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance#bring-your-own-license-byol) prijsinformatie voor SQL Server Azure VM's.


## <a name="best-practice-use-reserved-vm-instances"></a>Aanbevolen: Gebruik van gereserveerde VM-instanties

De meeste cloudplatforms zijn ingesteld als betalen per gebruik. Dit model geeft nadelen, omdat u niet per se weet hoe dynamische workloads zijn. Wanneer u duidelijk bedoelingen voor een werkbelasting opgeeft, wordt u bijdragen aan de infrastructuur plannen.

Met behulp van Azure gereserveerde VM-instanties, betaalt u vooraf voor een of drie jaar term VM-exemplaar. 

- Vooruitbetaling biedt een korting op de resources die u gebruikt.
- U kunt VM, SQL database-compute, Azure Cosmos DB of andere resourcekosten aanzienlijk verkorten door tot 72% bij betalen per gebruik. 
- Reserveringen bieden een korting van facturering, en niet van invloed op de runtimestatus van uw resources.
- U kunt gereserveerde instanties annuleren.

![Gereserveerde instanties](./media/migrate-best-practices-costs/reserve.png)
*voor Azure gereserveerde virtuele machines*

**Meer informatie:**
- [Meer informatie over](https://docs.microsoft.com/azure/billing/billing-save-compute-costs-reservations) Azure reserveringen.
- [Lezen](https://azure.microsoft.com/pricing/reserved-vm-instances/#faq) de gereserveerde instanties Veelgestelde vragen over.
- [Richtlijnen voor prijzen](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance#bring-your-own-license-byol) voor SQL Server Azure VM's.


## <a name="best-practice-aggregate-cloud-spend-across-subscriptions"></a>Aanbevolen: Cumulatieve clouduitgaven voor abonnementen

Het is onvermijdelijk dat uiteindelijk meer dan één Azure-abonnement hebt u. Bijvoorbeeld, moet u mogelijk een extra abonnement voor het scheiden van de grenzen van ontwikkeling en productie, of mogelijk hebt u een platform dat een afzonderlijk abonnement is vereist voor elke client. De mogelijkheid het samenvoegen van gegevens in alle abonnementen in één platform reporting is een belangrijke functie.

Om dit te doen, kunt u Azure Cost Management-API's. U kunt vervolgens na het aggregeren van gegevens in één bron, zoals Azure SQL, hulpprogramma's zoals Power BI gebruiken aan de oppervlakte van de cumulatieve gegevens. U kunt samengevoegde abonnement, en gedetailleerde rapporten maken. Bijvoorbeeld, voor gebruikers die proactieve inzicht in kostenbeheer nodig hebben, kunt u specifieke weergaven van de kosten, op basis van de afdeling, resourcegroep enzovoort. U hoeft niet te voorzien van deze volledige toegang tot Azure factureringsgegevens.

**Meer informatie:**

- [Bekijk een overzicht](https://docs.microsoft.com/azure/billing/billing-consumption-api-overview) van de Azure-verbruik-API.
- [Meer informatie over](https://docs.microsoft.com/power-bi/desktop-connect-azure-consumption-insights) verbinding te maken met Azure Consumption Insights maken in Power BI Desktop.
- [Meer informatie over het](https://docs.microsoft.com/azure/billing/billing-manage-access) beheren van toegang tot factureringsgegevens voor Azure met behulp van op rollen gebaseerd toegangsbeheer (RBAC).
 
## <a name="after-migration"></a>Na de migratie

Na een geslaagde migratie van uw workloads en een paar weken van het verzamelen van gegevens, hebt u een goed beeld van de kosten van resources.
- Als u gegevens analyseren, kunt u beginnen met het genereren van een basislijn budget voor Azure-resourcegroepen en resources.
- Als u waar uw budget cloud wordt besteed weet, kunt u hoe u de kosten verder reduceren analyseren.

Aanbevolen procedures in deze sectie bevatten met behulp van Azure Cost Management voor kosten ook beter budgetteren en analyse, het bewaken van resources en resource group budgetten implementeren en controleren, opslag en virtuele machines te optimaliseren.

## <a name="best-practice-use-azure-cost-management"></a>Aanbevolen: Gebruik Azure Cost Management

Microsoft biedt Azure Cost Management waarmee u kunt als volgt uitgave bij te houden:

- Helpt u bij het controleren en beheren van Azure-uitgaven en optimaal gebruik wordt gemaakt van resources.
- Beoordelingen van uw volledige abonnement en alle bijbehorende resources en aanbevelingen doet.
- Biedt een volledige-API, integratie van externe hulpprogramma's en financiële systemen voor rapportage.
- Bijgehouden Resourcegebruik en cloudkosten met een enkele, samengevoegde weergave beheren.
- Uitgebreide operationele en financiële inzichten waarmee u onderbouwde beslissingen a.

In Cost Management, kunt u het volgende doen:


- **Maak een budget**: Maak een budget voor financiële aansprakelijkheid.
    - U kunt een account maken voor de services die u verbruikt of zich abonneert op voor een bepaalde periode (maandelijks, elk kwartaal, per jaar) en een bereik (abonnementen/resourcegroepen). Bijvoorbeeld, kunt u het budget van een Azure-abonnement gedurende een maand, per kwartaal of jaar.
    - Nadat u een budget gemaakt, wordt deze weergegeven in kostenanalyse. Voor het weergeven van uw budget voor de huidige uitgaven is een van de eerste stappen die nodig zijn bij het analyseren van uw kosten en uitgaven.
    - E-mailmeldingen kunnen worden verzonden wanneer de budgetdrempels wordt bereikt.
    - U kunt kosten beheergegevens exporteren naar Azure storage, voor analyse.

    ![Cost Management budget](./media/migrate-best-practices-costs/budget.png) *Azure Cost Management budget*

- **Voer een kostenanalyse**: Een kostenanalyse te verkennen en analyseren van uw organisatie kosten, zodat u inzicht in hoe de kosten worden opgebouwd, ophalen en trends in uitgaven te identificeren.
    - Kostenanalyse is beschikbaar voor EA-gebruikers.
    - U kunt gegevens van cost analysis voor een aantal scopes, met inbegrip van afdeling, account, abonnement of resourcegroep weergeven.
    - U krijgt een kostenanalyse waarin de totale kosten voor de huidige maand en de totale dagelijkse kosten. 

    ![Kosten van analysis Management](./media/migrate-best-practices-costs/analysis.png) *analyse van Azure Cost Management*
- **Ontvang aanbevelingen**: Ontvang aanbevelingen van Advisor waarin u u ziet hoe u kunt optimaliseren en efficiëntie te verbeteren.


**Meer informatie:**

- [Bekijk een overzicht](https://docs.microsoft.com/azure/cost-management/overview) van Azure Cost Management.
- [Meer informatie over het](https://docs.microsoft.com/azure/cost-management/cost-mgt-best-practices) optimaliseren van uw investeringen in de cloud met Azure Cost Management.
- [Meer informatie over het](https://docs.microsoft.com/azure/cost-management/use-reports) met behulp van Azure Cost Management-rapporten.
- [Ophalen van een zelfstudie](https://docs.microsoft.com/azure/cost-management/tutorial-acm-opt-recommendations?toc=/azure/billing/TOC.json) voor het optimaliseren van de kosten van aanbevelingen.
- [Beoordeling](https://docs.microsoft.com/rest/api/consumption/budgets) de Azure API-verbruik.

## <a name="best-practice-monitor-resource-utilization"></a>Aanbevolen: Brongebruik door de monitor

U betaalt voor wat u gebruikt, wanneer resources worden verbruikt, en u niet betaalt wanneer ze niet in Azure. Voor virtuele machines, vindt facturering plaats wanneer een virtuele machine wordt toegewezen, en niet in rekening gebracht nadat een VM ongedaan is gemaakt. U moet hart controleren van virtuele machines in gebruik en controleer of de VM-grootte.

- Uw VM-workloads om te bepalen van basislijnen voortdurend worden geëvalueerd.
- Als uw werkbelasting sterk maandag tot en met vrijdag, 8: 00 uur op 18: 00 uur gebruikt, maar nauwelijks buiten deze uren wordt gebruikt, kunt u virtuele machines downgraden buiten piektijden. Dit kan betekenen dat het wijzigen van VM-grootten, of met behulp van de virtuele-machineschaalset ingesteld op virtuele machines automatisch schalen omhoog of omlaag.
- Sommige bedrijven "uitstellen", virtuele machines door ze in een agenda te plaatsen die aangeeft wanneer ze beschikbaar moeten zijn en wanneer ze niet nodig zijn. 
- Naast het controleren van de virtuele machine, moet u andere netwerkresources, zoals de ExpressRoute- en virtuele netwerkgateways voor onder- en gebruik bewaken.
- U kunt met behulp van een aantal hulpprogramma's van Microsoft, waaronder Azure Cost Management, Azure Monitor en Azure Advisor VM-gebruik bewaken. Hulpprogramma's van derden zijn ook beschikbaar.  

**Meer informatie:**
- Bekijk een overzicht van [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) en [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-overview).
- [Ophalen](https://docs.microsoft.com/azure/advisor/advisor-cost-recommendations) Advisor aanbevelingen kosten.
- [Meer informatie over het [kosten van aanbevelingen optimaliseren](https://docs.microsoft.com/azure/cost-management/tutorial-acm-opt-recommendations?toc=/azure/billing/TOC.json), en [onverwachte kosten te voorkomen dat](https://docs.microsoft.com/azure/billing/billing-getting-started).
- [Meer informatie over](https://github.com/Azure/azure-quickstart-templates/tree/master/azure-resource-optimization-toolkit/) de Toolkit Azure-Resource-optimalisatie (ARO)

## <a name="best-practice-implement-resource-group-budgets"></a>Aanbevolen: Resource group budgetten implementeren

Resourcegroepen worden vaak gebruikt om weer te geven van de grenzen van de kosten. Samen met dit patroon blijft het team van Azure voor het ontwikkelen van nieuwe en verbeterde manieren voor het bijhouden en analyseren van de resource uitgaven op verschillende niveaus, waaronder de mogelijkheid om te maken van budgetten in de resourcegroep en resources.  

- Een resource-group-budget kunt u bijhouden van de kosten die zijn gekoppeld aan een resourcegroep.
- U kunt waarschuwingen activeren en een groot aantal playbooks uitvoeren zoals het budget is bereikt of overschreden. 

**Meer informatie:**

- [Meer informatie over het](https://docs.microsoft.com/azure/billing/billing-cost-management-budget-scenario) kosten beheren met Azure-budget uitgeven.
- [Volg een zelfstudie](https://docs.microsoft.com/azure/cost-management/tutorial-acm-create-budgets?toc=/azure/billing/TOC.json) maken en beheren van een Azure budget.


## <a name="best-practice-optimize-azure-monitor-retention"></a>Aanbevolen: Azure Monitor retentie optimaliseren

Als u resources wilt naar Azure verplaatsen en diagnostische logboekregistratie voor deze inschakelen, kunt u een groot aantal logboekgegevens genereren. Doorgaans wordt deze logboekgegevens verzonden naar een opslagaccount dat toegewezen aan een Log Analytics-werkruimte.

- Hoe langer de bewaarperiode logboek, hoe meer gegevens hebt u.
- Niet alle logboekgegevens zijn en sommige resources meer gegevens dan andere wordt gegenereerd.
- Vanwege regelgeving en naleving is het waarschijnlijk, moet u enkele bronnen die langer is dan de andere logboekgegevens bewaren.
- U moet een zorgvuldige regel helpen tussen het optimaliseren van uw opslagkosten logboek bij te houden en de logboekgegevens die u nodig hebt.
- U wordt aangeraden evalueren en instellen van de logboekregistratie onmiddellijk na voltooiing van een migratie, zodat u geld uit te bewaren, logboeken van het geen belang niet.

**Meer informatie:**

- [Meer informatie over](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs) gebruik en geschatte kosten bewaken.
 
## <a name="best-practice-optimize-storage"></a>Aanbevolen: Opslag optimaliseren

Als u aanbevolen procedures voor het selecteren van opslag voor de migratie hebt gevolgd, bent u waarschijnlijk zo volop enkele voordelen. Er zijn echter waarschijnlijk extra opslagkosten die u nog steeds kunt optimaliseren. Na verloop van tijd verouderen bestanden en blobs. Gegevens niet meer worden gebruikt, maar de wettelijke vereisten kunnen dit betekenen dat u wilt behouden gedurende een bepaalde periode. Daarom mogelijk niet moet u deze opslaan op de opslag met hoge prestaties die u hebt gebruikt voor de oorspronkelijke migratie.

Identificeren en verouderde gegevens verplaatst naar goedkoper opslagplaatsen kunnen een grote invloed hebben op uw maandelijkse opslag budget en kostenbesparingen. Azure biedt tal van manieren om te identificeren en deze verouderde gegevens vervolgens opslaan.

- Profiteer van laag voor algemeen gebruik v2-opslag, verplaatsen, minder belangrijke gegevens van warm naar koud en gearchiveerd lagen.
- Met StorSimple kunt verplaatsen van verouderde gegevens op basis van aangepaste beleidsregels. 

**Meer informatie:**
- [Meer informatie](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers) over toegangslagen.
- [Bekijk een overzicht](https://docs.microsoft.com/azure/azure-monitor/overview) van StorSimple, en [StorSimple prijzen](https://azure.microsoft.com/pricing/details/storsimple/).

## <a name="best-practice-automate-vm-optimization"></a>Aanbevolen: Optimalisatie van de virtuele machine automatiseren

Er is een einddoel het toegankelijk maken van het uitvoeren van een virtuele machine in de cloud om te maximaliseren van de CPU, geheugen en schijfruimte die wordt gebruikt. Als u virtuele machines die geoptimaliseerd zijn niet detecteren of hebben vaak perioden wanneer virtuele machines worden niet gebruikt, het verstandig om ze uitschakelen, vergroten of verkleinen van deze met behulp van VM-schaalsets.

U kunt een virtuele machine met Azure Automation, VM scale sets, automatisch afsluiten en scripts of 3rd party oplossingen kunt optimaliseren. 

**Meer informatie:**

- [Meer informatie over het](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-vertical-scale-reprovision) verticaal automatisch schalen gebruiken.
- [Planning](https://azure.microsoft.com/updates/azure-devtest-labs-schedule-vm-auto-start/) een virtuele machine automatisch starten.
- [Meer informatie over het](https://docs.microsoft.com/azure/automation/automation-solution-vm-management) starten of stoppen van VM's buiten kantooruren in Azure Automation.
- [Vindt u meer informatie] over [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-overview), en de [Azure Resource Optimization (ARO) Toolkit](https://github.com/Azure/azure-quickstart-templates/tree/master/azure-resource-optimization-toolkit/).

## <a name="best-practices-use-logic-apps-and-runbooks-with-budgets-api"></a>Aanbevolen procedures: Logic Apps en runbooks met budgetten API gebruiken

Azure biedt een REST-API die toegang tot uw factureringsgegevens voor de tenant heeft.

- U kunt de budgetten-API gebruiken om externe systemen en werkstromen die worden geactiveerd door metrische gegevens die u samengesteld op basis van de gegevens van de API te integreren.
- U kunt gebruiks-en resource ophalen in uw favoriete hulpprogramma's voor gegevensanalyse.
- De Azure Resource Usage- en RateCard-API’s kunnen u helpen uw kosten nauwkeurig te voorspellen en te beheren.
- De API's worden geïmplementeerd als een Resourceprovider en worden opgenomen in de API's beschikbaar gemaakt door de Azure Resource Manager.
- De API budgetten kunnen worden geïntegreerd met Azure Logic Apps en Runbooks.

**Meer informatie:**

- [Meer informatie](https://docs.microsoft.com/rest/api/consumption/budgets) over de API-budget uitgeven.
- [Inzichten verkrijgen](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview) in het gebruik van Azure met de API voor facturering.


## <a name="best-practice-implement-serverless-technologies"></a>Aanbevolen: Serverloze technologieën implementeren

VM-workloads worden vaak gemigreerd "as is" geen downtime. Virtuele machines kunnen vaak taken die zijn onregelmatige, waarbij een korte periode uit te voeren, of u kunt ook vele uren hosten. Bijvoorbeeld, taak virtuele machines die worden uitgevoerd van geplande taken, zoals Windows scheduler of PowerShell-scripts. Wanneer deze taken worden niet uitgevoerd, u toch opvangen VM en schijf-kosten voor opslag.

Na de migratie, na een grondige analyse van deze typen taken kunt u overwegen ze te migreren naar serverloze technologieën zoals Azure Functions of Azure Batch-taken. Met deze oplossing hoeft u niet meer te beheren en onderhouden van de virtuele machines, waardoor een extra kosten besparen. 

**Meer informatie:**
- [Meer informatie over](https://azure.microsoft.com/services/functions/) Azure Functions
- [Meer informatie over](https://azure.microsoft.com/en-us/services/batch/) Azure Batch
  
## <a name="next-steps"></a>Volgende stappen 

Bekijk de andere aanbevolen procedures:

- [Aanbevolen procedures](migrate-best-practices-security-management.md) voor beveiliging en beheer na de migratie.
- [Aanbevolen procedures](migrate-best-practices-networking.md) voor netwerken na de migratie.

