---
title: Aanbevolen procedures voor het gebruik van Azure Data Lake Storage Gen2 | Microsoft Docs
description: Informatie over de aanbevolen procedures voor opname van gegevens, datum beveiliging en prestaties met betrekking tot het gebruik van Azure Data Lake Storage Gen2 (voorheen bekend als Azure Data Lake Store)
services: storage
author: sachinsbigdata
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: article
ms.date: 12/06/2018
ms.author: sachins
ms.openlocfilehash: ad77204f0c5d916b4006ffa68a9608429f93f87a
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55246055"
---
# <a name="best-practices-for-using-azure-data-lake-storage-gen2"></a>Aanbevolen procedures voor het gebruik van Azure Data Lake Storage Gen2

In dit artikel leert u over aanbevolen procedures en overwegingen voor het werken met Azure Data Lake Storage Gen2. In dit artikel bevat informatie over beveiliging, prestaties, flexibiliteit en controle van het Data Lake Storage Gen2. Werken met echt big data in services zoals Azure HDInsight is voordat u Data Lake Storage Gen2 complex. Moest u gegevens in meerdere Blob storage-accounts zodat petabyte-opslag en optimale prestaties op die kunnen worden geschaald kunnen worden bereikt. Met Data Lake Storage Gen2, zijn de meeste van de vaste limieten voor grootte en prestaties worden verwijderd. Er zijn echter nog steeds bepaalde overwegingen die van dit artikel vindt u zodat u de beste prestaties met Data Lake Storage Gen2 kan krijgen.

## <a name="security-considerations"></a>Beveiligingsoverwegingen

Azure Data Lake Storage Gen2 biedt besturingselementen voor POSIX-toegang voor gebruikers, groepen en service-principals van Azure Active Directory (Azure AD). Deze besturingselementen voor toegang kunnen worden ingesteld op de bestaande bestanden en mappen. De besturingselementen voor toegang kunnen ook worden gebruikt om standaardmachtigingen die worden automatisch op nieuwe bestanden of mappen toegepast kunnen te maken. Meer informatie over Data Lake Storage Gen2 ACL's zijn beschikbaar op [toegangsbeheer in Azure Data Lake Storage Gen2](storage-data-lake-storage-access-control.md).

### <a name="use-security-groups-versus-individual-users"></a>Beveiligingsgroepen ten opzichte van afzonderlijke gebruikers gebruiken

Met Radiotuner werken met big data in Data Lake Storage Gen2, is het waarschijnlijk dat een service-principal wordt gebruikt om toe te staan van services zoals Azure HDInsight om te werken met de gegevens. Maar mogelijk zijn er gevallen waarbij afzonderlijke gebruikers toegang hebben tot de gegevens ook moeten. In alle gevallen moet overwegen om met behulp van Azure Active Directory [beveiligingsgroepen](../common/storage-auth-aad.md) in plaats van afzonderlijke gebruikers toewijzen aan mappen en bestanden.

Nadat een beveiligingsgroep is toegewezen machtigingen, toevoegen of verwijderen van gebruikers uit de groep zijn vereist om updates voor Data Lake Storage Gen2. Hierdoor kunnen ook Zorg ervoor dat u niet langer zijn dan het maximum aantal vermeldingen voor toegangsbeheer per toegangsbeheerlijst (ACL). Dit nummer is momenteel 32 (met inbegrip van de vier POSIX-stijl ACL's die altijd gekoppeld aan elk bestand en map zijn): de gebruiker die eigenaar is, de groep die eigenaar is, het masker en andere. Elke directory kan hebben twee typen ACL, de toegang tot de ACL en de standaard-ACL voor een totaal van 64 vermeldingen voor toegangsbeheer. Zie voor meer informatie over deze ACL's, [toegangsbeheer in Azure Data Lake Storage Gen2](data-lake-storage-access-control.md).

### <a name="security-for-groups"></a>Beveiliging voor groepen

Wanneer u of uw gebruikers toegang tot gegevens in een storage-account met de hiërarchische naamruimte ingeschakeld, is het raadzaam te gebruiken van Azure Active Directory-beveiligingsgroepen. Enkele aanbevolen te beginnen met groepen kunnen worden **ReadOnlyUsers**, **WriteAccessUsers**, en **FullAccessUsers** voor de hoofdmap van het bestandssysteem en zelfs afzonderlijke resources voor belangrijkste submappen. Als er andere verwachte groepen van gebruikers die later kunnen worden toegevoegd, maar niet zijn geïdentificeerd nog, dat u kunt overwegen dummy-beveiligingsgroepen die toegang tot bepaalde mappen hebben maken. Met behulp van de beveiligingsgroep, zorgt u ervoor dat u niet lang verwerkingstijd wanneer nieuwe machtigingen toewijzen aan duizenden bestanden.

### <a name="security-for-service-principals"></a>Beveiliging voor service-principals

Azure Active Directory-service-principals worden meestal gebruikt voor services zoals Azure Databricks voor toegang tot gegevens in Data Lake Storage Gen2. Een enkele Azure Active Directory service-principal is waarschijnlijk voldoende voor veel klanten en zij hebben volledige machtigingen in de hoofdmap van de Data Lake Storage Gen2 bestandssysteem. Andere klanten mogelijk meerdere clusters met andere service-principals waarbij één cluster volledige toegang tot de gegevens en een ander cluster met alleen lezen-toegang heeft. 

### <a name="enable-the-data-lake-storage-gen2-firewall-with-azure-service-access"></a>De firewall van de Data Lake Storage Gen2 met toegang tot Azure-service inschakelen

Data Lake Storage Gen2 ondersteunt de mogelijkheid om een firewall inschakelen en alleen toegang tot Azure-services, dit wordt aanbevolen om te beperken de vector van externe aanvallen te beperken. Firewall kan worden ingeschakeld op een storage-account in Azure portal via de **Firewall** > **Firewall inschakelen (aan)** > **toegang tot Azure-servicestoestaan** opties.

Azure Databricks-clusters toevoegen aan een virtueel netwerk dat toegang via de Firewall van de opslag kan worden toegestaan, vereist het gebruik van een preview-functie van Databricks. Als u wilt deze functie inschakelt, moet u een ondersteuningsaanvraag in plaats.

## <a name="resiliency-considerations"></a>Overwegingen voor tolerantie

Bij het ontwerpen van een systeem met Data Lake Storage Gen2 of een cloudservice, moet u rekening houden met vereisten voor beschikbaarheid en het reageren op potentiële onderbrekingen in de service. Een probleem naar het specifieke exemplaar kan worden gelokaliseerd of zelfs regiobrede, dus dat u een plan voor beide is belangrijk. Afhankelijk van de beoogde hersteltijd en het herstel wijst doelstelling Sla's voor uw workload, u kunt een meer of minder agressief strategie voor hoge beschikbaarheid en herstel na noodgevallen.

### <a name="high-availability-and-disaster-recovery"></a>Hoge beschikbaarheid en herstel na noodgevallen

Hoge beschikbaarheid (HA) en herstel na noodgeval (DR) kunnen ook worden gecombineerd, hoewel elk een iets andere strategie, heeft met name als het gaat om de gegevens. Data Lake Storage Gen2 verwerkt reeds 3 x replicatie achter de schermen ter bescherming tegen lokaal optredende hardwarestoringen. Bovendien andere replicatieopties, zoals ZRS verbeteren HA bij GRS en RA-GRS verbeteren Noodherstel. Bij het bouwen van een plan voor hoge beschikbaarheid, in het geval van een onderbreking van de service moet de werkbelasting toegang hebben tot de meest recente gegevens zo snel mogelijk door over te schakelen naar een afzonderlijk gerepliceerde exemplaar lokaal of in een nieuwe regio.

In een strategie voor herstel na Noodgevallen, om voor te bereiden voor het onwaarschijnlijke geval van een catastrofale uitval van een regio, is het ook belangrijk om gegevens gerepliceerd naar een andere regio met behulp van GRS of RA-GRS-replicatie. U moet ook rekening houden met uw vereisten voor randgevallen zoals beschadiging van gegevens waar u maken van een periodieke momentopname wilt mogelijk voor het terugvallen op. Afhankelijk van het belang en de grootte van de gegevens, kunt u overwegen rolling delta-momentopnamen van 1 - 6- en 24-uurs perioden, op basis van risico's toleranties.

Voor tolerantie van gegevens met Data Lake Storage Gen2, het verdient aanbeveling hierop geo-replicatie de gegevens via GRS of RA-GRS die voldoet aan de vereisten van uw HA-/ DR. Bovendien moet u rekening houden met manieren voor de toepassing met behulp van Data Lake Storage Gen2 automatisch een failover uitvoeren naar de secundaire regio via bewaking triggers of lengte van mislukte pogingen of ten minste een melding verzonden naar beheerders voor handmatige tussenkomst. Houd er rekening mee dat er een afweging van failover wordt uitgevoerd versus wachten op een service weer online komt.

### <a name="use-distcp-for-data-movement-between-two-locations"></a>Distcp gebruiken voor verplaatsing van gegevens tussen twee locaties

Kort voor gedistribueerde kopiëren, DistCp is een Linux-opdrachtregelprogramma dat wordt geleverd met Hadoop en gedistribueerde gegevensverplaatsing tussen twee locaties biedt. De twee locaties kunnen worden Data Lake Storage Gen2, HDFS- of S3. Dit hulpprogramma maakt gebruik van MapReduce-taken op een Hadoop-cluster (bijvoorbeeld HDInsight) om uit te schalen op alle knooppunten. Distcp wordt beschouwd als de snelste manier om grote gegevens zonder compressie van speciale netwerkapparaten te verplaatsen. Distcp biedt ook een optie om bij te werken alleen delta's tussen twee locaties, automatische nieuwe pogingen verwerkt, evenals dynamisch schalen van de rekencapaciteit. Deze benadering is zeer efficiënt wanneer het gaat om zaken zoals Hive/Spark-tabellen waarvoor veel grote bestanden in één map kunnen worden te repliceren en u wilt dat alleen de gewijzigde gegevens worden overschreven. Distcp is daarom het meest aanbevolen hulpprogramma voor het kopiëren van gegevens tussen big data-archieven.

Kopie taken kunnen worden geactiveerd door Apache Oozie-werkstromen met behulp van de frequentie of gegevens triggers, evenals Linux cron-taken. Voor intensieve replicatietaken, is het raadzaam om een afzonderlijke HDInsight Hadoop-cluster dat kan worden afgestemd op en schaal specifiek voor de kopie-taken te implementeren. Dit zorgt ervoor dat exemplaar taken niet essentiële taken storen. Als replicatie uitgevoerd op een groot genoeg frequentie, kan het cluster zelfs offline gezet tussen elke taak. Als Failover-overschakeling uitvoeren naar de secundaire regio, ervoor zorgen dat een ander cluster ook in de secundaire regio voor het repliceren van nieuwe gegevens terug naar het hoofdaccount van Data Lake Storage Gen2 genomen wordt nadat deze is hersteld. Zie voor meer voorbeelden van het gebruik van Distcp [Distcp gebruiken om te kopiëren van gegevens tussen Azure Storage-Blobs en Data Lake Storage Gen2](../blobs/data-lake-storage-use-distcp.md).

### <a name="use-azure-data-factory-to-schedule-copy-jobs"></a>Azure Data Factory gebruiken om taken van de kopie te plannen

Azure Data Factory kan ook worden gebruikt om taken van exemplaar met behulp van een Kopieeractiviteit te plannen en kan ook worden ingesteld op een frequentie via de Wizard kopiëren. Houd er rekening mee dat Azure Data Factory een limiet van cloud-eenheden voor gegevensverplaatsing (DMUs heeft) en de doorvoer/compute voor workloads met veel gegevens uiteindelijk hoofdletters. Bovendien biedt Azure Data Factory op dit moment geen updates van de verschillen tussen Data Lake Storage Gen2 accounts, zodat de mappen, zoals Hive-tabellen moet een volledige kopie om te repliceren. Raadpleeg de [data factory artikel](../../data-factory/load-azure-data-lake-storage-gen2.md) voor meer informatie over het kopiëren met Data Factory.

## <a name="monitoring-considerations"></a>Overwegingen met betrekking tot bewaking

Data Lake Storage Gen2 biedt metrische gegevens in Azure portal onder het account van de Data Lake Storage Gen2 en in Azure Monitor. Beschikbaarheid van Data Lake Storage Gen2 wordt weergegeven in de Azure-portal. Als u de meest recente beschikbaarheid van een Data Lake Storage Gen2-account, moet u uw eigen synthetische tests uit om te valideren beschikbaarheid uitvoeren. Andere metrische gegevens zoals Totaal opslaggebruik lezen/schrijven-aanvragen en Inkomend/uitgaand verkeer zijn beschikbaar om te worden gebruikt door de bewaking van toepassingen en kunt ook waarschuwingen activeren wanneer drempelwaarden (bijvoorbeeld gemiddelde latentie of het aantal fouten per minuut) worden overschreden.

## <a name="directory-layout-considerations"></a>Overwegingen voor Directory-indeling

Wanneer gegevens terechtkomen in een data lake, is het belangrijk dat u de structuur van de gegevens vooraf te plannen zodat beveiliging, partitionering en verwerking kunnen worden effectief gebruikt. Veel van de volgende aanbevelingen zijn van toepassing voor alle werkbelastingen voor big data. Elke werkbelasting kent verschillende vereisten op hoe de gegevens is verbruikt, maar hieronder worden enkele algemene indeling om te overwegen bij het werken met IoT en batch-scenario's.

### <a name="iot-structure"></a>IoT-structuur

In de IoT-workloads, kan er een grote hoeveelheid gegevens wordt bevindt zich in het gegevensarchief die zich over verschillende producten, apparaten, organisaties en klanten uitstrekt. Het is belangrijk de directory-indeling voor de organisatie, beveiliging en efficiënte verwerking van de gegevens voor de stroom consumenten vooraf te plannen. Een algemene sjabloon kunt u mogelijk de volgende indeling:

    {Region}/{SubjectMatter(s)}/{yyyy}/{mm}/{dd}/{hh}/

Bijvoorbeeld: telemetrie voor een vliegtuig-engine in het Verenigd Koninkrijk aanvoer eruit kan de volgende structuur:

    UK/Planes/BA1293/Engine1/2017/08/11/12/

Er is een belangrijke reden voor het plaatsen van de datum aan het einde van de mapstructuur. Als u vergrendelen bepaalde regio's of onderwerp zaken aan gebruikers/groepen wilt, kunt klikt u vervolgens u eenvoudig doen met de POSIX-machtigingen. Anders als er behoefte aan een bepaalde beveiligingsgroep bij het weergeven van alleen de gegevens van het Verenigd Koninkrijk of bepaalde vlakken beperken, is met de datumstructuur voor een afzonderlijke machtiging vereist voor een groot aantal mappen in de map voor elk uur. Bovendien zou met de datumstructuur voor exponentieel naarmate het aantal mappen tijd is een fout op.

### <a name="batch-jobs-structure"></a>Structuur van de batch-taken

Een veelgebruikte aanpak in batchverwerking is van een op hoog niveau, op grond van gegevens in een map 'in'. Vervolgens, nadat de gegevens is verwerkt, plaatst u de nieuwe gegevens in een directory 'uit' voor downstream-processen te gebruiken. Deze directorystructuur wordt soms gebruikt voor taken die de verwerkingen uitvoert op afzonderlijke bestanden en krachtige parallelle verwerking niet mogelijk voor grote gegevenssets. Een goede directory-structuur heeft, zoals de IoT-structuur aanbevolen hierboven, de bovenliggende mappen voor, zoals de regio en onderwerp zaken (bijvoorbeeld, organisatie, product/producent). Deze structuur helpt bij het beveiligen van de gegevens in uw organisatie en beter beheer van de gegevens in uw workloads. Bovendien kunt u datum en tijd in de structuur voor betere organisatie, gefilterde zoekopdrachten, beveiliging en automatisering in de verwerking. Het granulariteitsniveau voor de datumstructuur wordt bepaald door het interval op waarmee de gegevens wordt geüpload naar of verwerkt, zoals elk uur, dagelijks, of zelfs maandelijks.

Soms bestand verwerking is mislukt vanwege een onverwachte opmaak of beschadiging van gegevens. In dergelijke gevallen mapstructuur mogelijk profiteren van een **/ongeldige** map u de bestanden voor meer controle wilt verplaatsen. De batch-taak kan ook afhandelen het rapport of de melding van deze *beschadigde* bestanden voor handmatige tussenkomst. Houd rekening met de structuur van de volgende sjabloon:

    {Region}/{SubjectMatter(s)}/In/{yyyy}/{mm}/{dd}/{hh}/
    {Region}/{SubjectMatter(s)}/Out/{yyyy}/{mm}/{dd}/{hh}/
    {Region}/{SubjectMatter(s)}/Bad/{yyyy}/{mm}/{dd}/{hh}/

Bijvoorbeeld, ontvangt een marketing vast dagelijkse gegevens extraheert van updates van de klant van hun clients in Noord-Amerika. Vóór en na te zijn verwerkt kan er uitzien zoals in het volgende codefragment:

    NA/Extracts/ACMEPaperCo/In/2017/08/14/updates_08142017.csv
    NA/Extracts/ACMEPaperCo/Out/2017/08/14/processed_updates_08142017.csv

In de meeste gevallen van batchgegevens rechtstreeks in, zoals Hive-databases of traditionele SQL-databases die worden verwerkt, is er geen behoefte hebt aan een **/in** of **/out** map omdat de uitvoer reeds naar gaat een map voor de Hive-tabel of externe database te scheiden. Bijvoorbeeld, dagelijkse extracten van klanten in hun respectieve mappen en orchestration zou land door iets zoals Azure Data Factory, Apache Oozie, of Apache luchtstroom zou een dagelijkse Hive of Spark-taak om te verwerken en de gegevens schrijven naar een Hive-tabel activeren.
