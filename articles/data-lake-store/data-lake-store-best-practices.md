---
title: Aanbevolen procedures voor het gebruik van Azure Data Lake Storage Gen1 | Microsoft Docs
description: Informatie over de aanbevolen procedures voor opname van gegevens, datum beveiliging en prestaties met betrekking tot het gebruik van Azure Data Lake Storage Gen1 (voorheen bekend als Azure Data Lake Store)
services: data-lake-store
documentationcenter: ''
author: sachinsbigdata
manager: jhubbard
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: sachins
ms.openlocfilehash: ef2b5fe6c9b70eaea5ab4db2d4a0ca59ff82dbb9
ms.sourcegitcommit: 794bfae2ae34263772d1f214a5a62ac29dcec3d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44391892"
---
# <a name="best-practices-for-using-azure-data-lake-storage-gen1"></a>Aanbevolen procedures voor het gebruik van Azure Data Lake Storage Gen1

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

In dit artikel leert u over aanbevolen procedures en overwegingen voor het werken met Azure Data Lake Storage Gen1. In dit artikel bevat informatie over beveiliging, prestaties, flexibiliteit en controle van het Data Lake Storage Gen1. Werken met echt big data in services zoals Azure HDInsight is voordat u Data Lake Storage Gen1 complex. Moest u gegevens in meerdere Blob storage-accounts zodat petabyte-opslag en optimale prestaties op die kunnen worden geschaald kunnen worden bereikt. Met Data Lake Storage Gen1, zijn de meeste van de vaste limieten voor grootte en prestaties worden verwijderd. Er zijn echter nog steeds bepaalde overwegingen die van dit artikel vindt u zodat u de beste prestaties met Data Lake Storage Gen1 kan krijgen. 

## <a name="security-considerations"></a>Beveiligingsoverwegingen

Azure Data Lake Storage Gen1 aanbiedingen POSIX besturingselementen voor toegang en gedetailleerde controle voor Azure Active Directory (Azure AD)-gebruikers, groepen en service-principals. Deze besturingselementen voor toegang kunnen worden ingesteld op de bestaande bestanden en mappen. De besturingselementen voor toegang kunnen ook worden gebruikt om te maken van de standaardwaarden die kunnen worden toegepast op nieuwe bestanden of mappen. Wanneer de machtigingen zijn ingesteld op bestaande mappen en de onderliggende objecten, moeten de machtigingen is doorgegeven recursief van elk object. Als zich een groot aantal bestanden, kan de machtigingen doorgeven lang duren. De tijd die nodig kan variëren tussen 30 tot 50 objecten dat per seconde wordt verwerkt. Daarom kan de map structuur- en gebruikersgroepen op de juiste wijze plannen. Anders kan dit leiden tot onverwachte vertragingen en problemen wanneer u met uw gegevens werkt. 

Stel dat u hebt een map met 100.000 onderliggende objecten. Als u de ondergrens van 30-objecten dat per seconde wordt verwerkt, voor het bijwerken van de machtiging voor de hele map kunnen een uur duren. Meer informatie over Data Lake Storage Gen1 ACL's zijn beschikbaar op [toegangsbeheer in Azure Data Lake Storage Gen1](data-lake-store-access-control.md). Voor betere prestaties op ACL's recursief toewijzen, kunt u de Azure Data Lake Command-Line Tool. Het hulpprogramma maakt meerdere threads en recursieve navigatie logica voor het snel ACL's toepassen op miljoenen bestanden. Het hulpprogramma is beschikbaar voor Linux en Windows, en de [documentatie](https://github.com/Azure/data-lake-adlstool) en [downloads](http://aka.ms/adlstool-download) voor dit hulpprogramma u op GitHub vinden kunt. Deze dezelfde prestatieverbeteringen kunnen worden ingeschakeld door uw eigen hulpprogramma's die zijn geschreven met de Data Lake Storage Gen1 [.NET](data-lake-store-data-operations-net-sdk.md) en [Java](data-lake-store-get-started-java-sdk.md) SDK's.

### <a name="use-security-groups-versus-individual-users"></a>Beveiligingsgroepen ten opzichte van afzonderlijke gebruikers gebruiken 

Als u werkt met big data in Data Lake Storage Gen1, wordt waarschijnlijk een service-principal gebruikt om toe te staan van services zoals Azure HDInsight om te werken met de gegevens. Maar mogelijk zijn er gevallen waarbij afzonderlijke gebruikers toegang hebben tot de gegevens ook moeten. In dergelijke gevallen moet u Azure Active Directory [beveiligingsgroepen](data-lake-store-secure-data.md#create-security-groups-in-azure-active-directory) in plaats van afzonderlijke gebruikers toewijzen aan bestanden en mappen. 

Nadat een beveiligingsgroep is toegewezen machtigingen, toevoegen of verwijderen van gebruikers uit de groep zijn vereist om updates voor Data Lake Storage Gen1. Dit ook zorgt ervoor dat u niet langer zijn dan de limiet van [32 standaard-ACL's](../azure-subscription-service-limits.md#data-lake-store-limits) (dit omvat de vier POSIX-stijl ACL's die altijd gekoppeld aan alle bestanden en mappen zijn: [de gebruiker die eigenaar is](data-lake-store-access-control.md#the-owning-user), [de groep die eigenaar is](data-lake-store-access-control.md#the-owning-group), [het masker](data-lake-store-access-control.md#the-mask), en andere).

### <a name="security-for-groups"></a>Beveiliging voor groepen 

Zoals besproken, wanneer gebruikers nodig hebben toegang tot Data Lake Storage Gen1, is het raadzaam te gebruiken van Azure Active Directory-beveiligingsgroepen. Enkele aanbevolen te beginnen met groepen kunnen worden **ReadOnlyUsers**, **WriteAccessUsers**, en **FullAccessUsers** voor de hoofdmap van het account en zelfs afzonderlijke resources voor de sleutel submappen. Als er andere verwachte groepen van gebruikers die later kunnen worden toegevoegd, maar niet zijn geïdentificeerd nog, dat u kunt overwegen dummy-beveiligingsgroepen die toegang tot bepaalde mappen hebben maken. Met behulp van de beveiligingsgroep, zorgt u ervoor dat later u niet een lange hoeft verwerkingstijd voor het nieuwe machtigingen toewijzen aan duizenden bestanden. 

### <a name="security-for-service-principals"></a>Beveiliging voor service-principals 

Azure Active Directory-service-principals worden meestal gebruikt voor toegang tot gegevens in Data Lake Storage Gen1-services zoals Azure HDInsight. Afhankelijk van de toegangsvereisten voor meerdere workloads, is het mogelijk dat er een enkele overwegingen om te zorgen voor beveiliging binnen en buiten de organisatie. Een enkele Azure Active Directory service-principal is waarschijnlijk voldoende voor veel klanten en zij hebben volledige machtigingen in de hoofdmap van het Data Lake Storage Gen1-account. Andere klanten mogelijk meerdere clusters met andere service-principals waarbij één cluster volledige toegang tot de gegevens en een ander cluster met alleen lezen-toegang heeft. Net als bij de beveiligingsgroepen, kunt u overwegen een service-principal maken voor elk scenario (lezen, schrijven, volledig verwacht) wanneer een Gen1 van Data Lake Storage-account is gemaakt. 

### <a name="enable-the-data-lake-storage-gen1-firewall-with-azure-service-access"></a>De firewall van de Data Lake Storage Gen1 met toegang tot Azure-service inschakelen 

Data Lake Storage Gen1 ondersteunt de mogelijkheid om een firewall inschakelen en alleen toegang tot Azure-services, die wordt aanbevolen voor een kleinere aanvalsvector van buitenaf indringers te beperken. Firewall kan worden ingeschakeld op de Gen1 van Data Lake Storage-account in Azure portal via de **Firewall** > **Firewall inschakelen (aan)** > **toestaan van toegang tot Azure Services** opties.  

![Firewall-instellingen in Data Lake Storage Gen1](./media/data-lake-store-best-practices/data-lake-store-firewall-setting.png "Firewall-instellingen in Data Lake Storage Gen1")

Hebben toegang tot Data Lake Storage Gen1 als firewall ingeschakeld, worden alleen Azure-services zoals HDInsight is, Data Factory, SQL Data Warehouse, enzovoort. Vanwege een interne NAT gebruikt door Azure, de Data Lake Storage Gen1 firewall biedt geen ondersteuning voor het beperken van de specifieke services door IP en is alleen bedoeld voor beperkingen van eindpunten buiten Azure, bijvoorbeeld on-premises. 

## <a name="performance-and-scale-considerations"></a>Aandachtspunten voor prestaties en schaal

Een van de krachtigste functies van Data Lake Storage Gen1 is dat de vaste limieten op de doorvoer van gegevens worden verwijderd. Verwijderen van de limieten kan klanten de gegevensgrootte van hun laten groeien en vergezeld gaan van prestatie-eisen zonder te hoeven shard de gegevens. Een van de belangrijkste overwegingen voor het optimaliseren van prestaties van de Data Lake Storage Gen1 is dat deze het beste wanneer de opgegeven parallelle uitvoering wordt uitgevoerd.

### <a name="improve-throughput-with-parallelism"></a>De doorvoer verbeteren met parallelle uitvoering 

8-12-threads per kern overwegen voor de meest optimale lezen/schrijven-doorvoer. Dit wordt veroorzaakt door blokkerende lees-/ schrijfbewerkingen op een enkele thread en meer threads kunt hogere gelijktijdigheid op de virtuele machine. Om ervoor te zorgen dat niveaus in orde is en parallelle uitvoering kan worden verhoogd, moet voor het bewaken van de CPU-gebruik van de virtuele machine.   

### <a name="avoid-small-file-sizes"></a>Kleine bestandsgrootte voorkomen

POSIX-machtigingen en controle in Data Lake Storage Gen1 wordt geleverd met een overhead die duidelijk wordt bij het werken met een groot aantal kleine bestanden. Als een best practice, moet u uw gegevens in grotere bestanden ten opzichte van duizenden of miljoenen kleine bestanden schrijven naar Data Lake Storage Gen1 batch. Kleine bestandsgrootte vermijden kunt beschikt over meerdere voordelen, zoals:

* De verificatiecontroles in meerdere bestanden te verlagen
* Bestand openen verbindingen verlaagd
* Snellere kopiëren/replicatie
* Minder bestanden worden verwerkt tijdens het bijwerken van Data Lake Storage Gen1 POSIX-machtigingen 

Afhankelijk van welke services en werkbelastingen de gegevens gebruiken, een goede grootte van bestanden is 256 MB of hoger. Als de grootte kunnen niet worden batchgewijs wanneer terechtkomen in Data Lake Storage Gen1, kunt u een afzonderlijke compressie-taak die deze bestanden tot grotere schijven combineert hebben. Zie voor meer informatie en aanbevelingen voor bestandsgrootten en organiseren van de gegevens in Data Lake Storage Gen1 [structuur van uw gegevensset](data-lake-store-performance-tuning-guidance.md#structure-your-data-set).

### <a name="large-file-sizes-and-potential-performance-impact"></a>Grote bestanden en de mogelijke invloed op de prestaties

Hoewel Data Lake Storage Gen1 biedt ondersteuning voor grote bestanden tot petabytes in grootte, voor optimale prestaties en afhankelijk van het proces lezen van gegevens, kan het niet zijn ideaal om te gaan op gemiddelde hierboven 2 GB. Wanneer u bijvoorbeeld **Distcp** om gegevens te kopiëren tussen locaties of andere storage-accounts, bestanden het hoogste niveau van granulatie gebruikt om te bepalen van de kaart taken zijn. Dus als u 10 de bestanden die zijn van 1 TB kopieert, worden maximaal 10 mappers toegewezen. Bovendien hebt u veel bestanden met mappers toegewezen, werken in eerste instantie de mappers parallel om grote bestanden te verplaatsen. Als de taak wordt gestart kon worden afgesloten omlaag echter slechts een paar mappers blijven toegewezen en u kunt een loopt vast bij het toewijzen van een enkel toegewezen aan een groot bestand. Microsoft heeft ingediend verbeteringen voor Distcp om dit probleem in toekomstige versies van Hadoop.  

Een ander voorbeeld om te overwegen is bij het gebruik van Azure Data Lake Analytics met Data Lake Storage Gen1. Afhankelijk van de verwerking door de extractor uitgevoerd, kunnen een aantal bestanden dat niet kunnen worden gesplitst (bijvoorbeeld, XML, JSON) te maken krijgen in de prestaties als groter dan 2 GB. In gevallen waarin bestanden kunnen worden gesplitst door een extractor (bijvoorbeeld CSV), hebben grote bestanden de voorkeur.

### <a name="capacity-plan-for-your-workload"></a>Capaciteit plannen voor uw workload 

Azure Data Lake Storage Gen1 Hiermee verwijdert u de vaste i/o beperkingslimieten die worden geplaatst op Blob storage-accounts. Er zijn echter nog steeds dynamische limieten die moeten worden overwogen. De beperking standaardlimieten voor Inkomend/uitgaand verkeer te voldoen aan de behoeften van de meeste scenario's. Als uw werkbelasting de limieten verhogen moet, kunt u werken met Microsoft ondersteuning. Bekijk ook de limieten in de fase testen van concept zodat de i/o-beperking limieten zijn bereikt tijdens de productie. Als dit gebeurt, kan er wachten vereist voor een handmatige verhoging van het technische team van Microsoft. Als i/o-beperking optreedt, Azure Data Lake Storage Gen1 retourneert foutcode 429 en in het ideale geval opnieuw moet worden uitgevoerd met een juiste exponentieel uitstel-beleid. 

### <a name="optimize-writes-with-the-data-lake-storage-gen1-driver-buffer"></a>'Schrijft' optimaliseren met de Data Lake Storage Gen1 stuurprogramma buffer 

Om prestaties te optimaliseren en IOP's verminderen bij het schrijven naar Data Lake Storage Gen1 uit Hadoop, uitvoeren van schrijfbewerkingen zo dicht bij de buffergrootte van Data Lake Storage Gen1 stuurprogramma mogelijk. Wil niet langer zijn dan de buffergrootte voordat, zoals bij het streamen met Apache Storm of Spark streaming-workloads. Bij het schrijven naar Data Lake Storage Gen1 uit HDInsight/Hadoop, is het belangrijk te weten dat Data Lake Storage Gen1 een stuurprogramma met een buffer 4 MB heeft. Net als veel bestand systeemstuurprogramma's, kan deze buffer handmatig worden leeggemaakt voordat de grootte van de 4 MB is bereikt. Als dat niet het geval is, worden onmiddellijk naar de opslag leeggemaakt als de volgende schrijven de maximale grootte van de buffer overschrijdt. Waar mogelijk, moet u een overschrijding of een aanzienlijke underrun van de buffer voorkomen wanneer het beleid door het venster count of tijd synchroniseren/leegmaken.

## <a name="resiliency-considerations"></a>Overwegingen voor tolerantie 

Bij het ontwerpen van een systeem met Data Lake Storage Gen1 of een cloudservice, moet u rekening houden met vereisten voor beschikbaarheid en het reageren op potentiële onderbrekingen in de service. Een probleem naar het specifieke exemplaar kan worden gelokaliseerd of zelfs regiobrede, dus dat u een plan voor beide is belangrijk. Afhankelijk van de **beoogde hersteltijd** en de **beoogd herstelpunt** Sla's voor uw workload, zou u een meer of minder agressief strategie voor hoge beschikbaarheid en herstel na noodgevallen.

### <a name="high-availability-and-disaster-recovery"></a>Hoge beschikbaarheid en herstel na noodgevallen 

Hoge beschikbaarheid (HA) en herstel na noodgeval (DR) kunnen ook worden gecombineerd, hoewel elk een iets andere strategie, heeft met name als het gaat om de gegevens. Data Lake Storage Gen1 verwerkt reeds 3 x replicatie achter de schermen ter bescherming tegen lokaal optredende hardwarestoringen. Omdat replicatie tussen regio's niet is ingebouwd, moet u dit zelf beheren. Bij het bouwen van een plan voor hoge beschikbaarheid, in het geval van een onderbreking van de service moet de werkbelasting toegang hebben tot de meest recente gegevens zo snel mogelijk door over te schakelen naar een afzonderlijk gerepliceerde exemplaar lokaal of in een nieuwe regio.  

In een strategie voor herstel na Noodgevallen, om voor te bereiden voor het onwaarschijnlijke geval van een catastrofale uitval van een regio, is het ook belangrijk om gegevens gerepliceerd naar een andere regio. Deze gegevens kan in eerste instantie zijn hetzelfde als de gerepliceerde HA-gegevens. U moet echter ook uw vereisten voor randgevallen zoals beschadiging van gegevens waar u maken van een periodieke momentopname wilt mogelijk voor het terugvallen op overwegen. Afhankelijk van het belang en de grootte van de gegevens, kunt u overwegen rolling delta-momentopnamen van 1 - 6- en 24-uurs punten op de lokale en/of secundaire store, op basis van risico's toleranties. 

Voor tolerantie van gegevens met Data Lake Storage Gen1, het verdient aanbeveling hierop geo-replicatie de gegevens naar een afzonderlijke regio met een frequentie die voldoet aan de eisen aan de HA-/ DR in het ideale geval elk uur. Deze frequentie van de replicatie minimaliseert grootschalige gegevens verplaatsingen van het type waarvoor kunnen concurrerende doorvoerbehoeften met het systeem en een betere beoogde herstelpunt (RPO). Bovendien moet u rekening houden met manieren voor de toepassing met behulp van Data Lake Storage Gen1 automatisch een failover uitvoeren naar de secundaire account via bewaking triggers of lengte van mislukte pogingen of ten minste een melding verzonden naar beheerders voor handmatige tussenkomst. Houd er rekening mee dat er een afweging van failover wordt uitgevoerd versus wachten op een service weer online komt. Als de gegevens nog niet voltooid repliceren, kan een failover leiden tot verlies van gegevens, inconsistentie of complexe samenvoegen van de gegevens. 

Hieronder vindt u de belangrijkste drie aanbevolen opties voor het orkestreren van replicatie tussen Gen1 van Data Lake Storage-accounts en de belangrijkste verschillen tussen elk van deze.


|  |Distcp  |Azure Data Factory  |AdlCopy  |
|---------|---------|---------|---------|
|**Schaallimieten**     | Begrensd door worker-knooppunten        | Beperkt door de Cloudgegevens maximum aantal eenheden        | Gebonden bent aan de Analytics units        |
|**Ondersteunt het kopiëren van delta 's**     |   Ja      | Nee         | Nee         |
|**Ingebouwde orchestration**     |  Nee (Oozie luchtstroom of cron-taken gebruiken)       | Ja        | Nee (Azure Automation- of Windows Taakplanner gebruiken)         |
|**Ondersteunde bestandssystemen**     | ADL, HDFS, WASB, S3, GS, CFS        |Talloze, Zie [Connectors](../data-factory/connector-azure-blob-storage.md).         | ADL naar ADL, WASB naar ADL (alleen bij dezelfde regio)        |
|**Ondersteuning voor het besturingssysteem**     |Elk besturingssysteem met Hadoop         | N/A          | Windows 10         |
   

### <a name="use-distcp-for-data-movement-between-two-locations"></a>Distcp gebruiken voor verplaatsing van gegevens tussen twee locaties 

Kort voor gedistribueerde kopiëren, Distcp is een Linux-opdrachtregelprogramma dat wordt geleverd met Hadoop en gedistribueerde gegevensverplaatsing tussen twee locaties biedt. De twee locaties kunnen worden Gen1 van Data Lake-opslag, HDFS, WASB of S3. Dit hulpprogramma maakt gebruik van MapReduce-taken op een Hadoop-cluster (bijvoorbeeld HDInsight) om uit te schalen op alle knooppunten. Distcp wordt beschouwd als de snelste manier om grote gegevens zonder compressie van speciale netwerkapparaten te verplaatsen. Distcp biedt ook een optie om bij te werken alleen delta's tussen twee locaties, automatische nieuwe pogingen verwerkt, evenals dynamisch schalen van de rekencapaciteit. Deze benadering is zeer efficiënt wanneer het gaat om zaken zoals Hive/Spark-tabellen waarvoor veel grote bestanden in één map kunnen worden te repliceren en u wilt dat alleen de gewijzigde gegevens worden overschreven. Distcp is daarom het meest aanbevolen hulpprogramma voor het kopiëren van gegevens tussen big data-archieven. 

Kopie taken kunnen worden geactiveerd door Apache Oozie-werkstromen met behulp van de frequentie of gegevens triggers, evenals Linux cron-taken. Voor intensieve replicatietaken, is het raadzaam om een afzonderlijke HDInsight Hadoop-cluster dat kan worden afgestemd op en schaal specifiek voor de kopie-taken te implementeren. Dit zorgt ervoor dat exemplaar taken niet essentiële taken storen. Als replicatie uitgevoerd op een groot genoeg frequentie, kan het cluster zelfs offline gezet tussen elke taak. Als Failover-overschakeling uitvoeren naar de secundaire regio, ervoor zorgen dat een ander cluster ook in de secundaire regio voor het repliceren van nieuwe gegevens terug naar de primaire Gen1 van Data Lake Storage-account genomen wordt nadat deze is hersteld. Zie voor meer voorbeelden van het gebruik van Distcp [Distcp gebruiken om te kopiëren van gegevens tussen Azure Storage-Blobs en Data Lake Storage Gen1](data-lake-store-copy-data-wasb-distcp.md).

### <a name="use-azure-data-factory-to-schedule-copy-jobs"></a>Azure Data Factory gebruiken om taken van de kopie te plannen 

Azure Data Factory kan ook worden gebruikt voor het plannen van taken van exemplaar met behulp van een **Kopieeractiviteit**, en kan ook worden ingesteld op een frequentie via de **Kopieerwizard**. Houd er rekening mee dat Azure Data Factory een limiet van cloud-eenheden voor gegevensverplaatsing (DMUs heeft) en de doorvoer/compute voor workloads met veel gegevens uiteindelijk hoofdletters. Bovendien biedt Azure Data Factory op dit moment geen updates van de verschillen tussen Gen1 van Data Lake Storage-accounts, zodat, zoals Hive-tabellen moet een volledige kopie om te repliceren. Raadpleeg de [Kopieeractiviteit afstemmingshandleiding](../data-factory/copy-activity-performance.md) voor meer informatie over het kopiëren met Data Factory. 

### <a name="adlcopy"></a>AdlCopy

AdlCopy is een Windows-opdrachtregelprogramma waarmee u het kopiëren van gegevens tussen twee Gen1 van Data Lake Storage-accounts alleen binnen dezelfde regio. Het hulpprogramma AdlCopy biedt een optie voor zelfstandige of de optie voor het gebruik van een Azure Data Lake Analytics-account om uit te voeren van de taak voor het kopiëren. Hoewel deze was oorspronkelijk bedoeld voor on-demand kopieën in plaats van een robuuste replicatie, biedt een andere optie om gedistribueerde kopiëren voor Gen1 van Data Lake Storage-accounts binnen dezelfde regio. Voor betrouwbaarheid, is het aanbevolen om het gebruik van de premium-Data Lake Analytics-optie voor een productie-werkbelasting. De zelfstandige versie bezet antwoorden kunt terugkeren en beperkte schaal en bewaking. 

Zoals Distcp moet de AdlCopy worden gecoördineerd door iets zoals Azure Automation- of Windows Taakplanner. Net als bij de Data Factory, AdlCopy biedt geen ondersteuning voor het kopiëren van alleen bijgewerkte bestanden, maar recopies en bestaande bestanden overschrijven. Zie voor meer informatie en voorbeelden van het gebruik van AdlCopy [gegevens kopiëren van Azure Storage-Blobs naar Data Lake Storage Gen1](data-lake-store-copy-data-azure-storage-blob.md).

## <a name="monitoring-considerations"></a>Overwegingen met betrekking tot bewaking 

Data Lake Storage Gen1 biedt gedetailleerde logboeken met diagnostische gegevens en controle. Data Lake Storage Gen1 biedt enkele eenvoudige metrische gegevens in Azure portal onder het account van de Data Lake Storage Gen1 en in Azure Monitor. Beschikbaarheid van Data Lake Storage Gen1 wordt weergegeven in de Azure-portal. Met deze metriek wordt echter wordt vernieuwd om de zeven minuten en via een openbaar daarvoor beschikbare API kan niet worden opgevraagd. Als u de meest recente beschikbaarheid van een Data Lake Storage Gen1-account, moet u uw eigen synthetische tests uit om te valideren beschikbaarheid uitvoeren. Andere metrische gegevens zoals Totaal opslaggebruik aanvragen lezen/schrijven en Inkomend/uitgaand verkeer kan worden vernieuwd tot 24 uur duren. Meer recente metrische gegevens moeten dus handmatig worden berekend via Hadoop-opdrachtregelprogramma's of gegevens verzamelen. De snelste manier om de meest recente gebruik van de opslag wordt met deze opdracht HDFS uitgevoerd vanaf het knooppunt in een Hadoop-cluster (bijvoorbeeld hoofdknooppunt):   

    hdfs dfs -du -s -h adl://<adlsg1_account_name>.azuredatalakestore.net:443/

### <a name="export-data-lake-storage-gen1-diagnostics"></a>Data Lake Storage Gen1 diagnostische gegevens exporteren 

Een van de snelste manieren om toegang te krijgen tot doorzoekbare logboeken van Data Lake Storage Gen1 bestaat uit het inschakelen van back-upfunctie voor **Log Analytics** onder de **Diagnostics** blade voor het Data Lake Storage Gen1-account. Dit biedt directe toegang tot binnenkomende logboeken tijd en inhoud filters, samen met opties (e-mailadres/webhook) waarschuwingen geactiveerd in intervallen van 15 minuten. Zie voor instructies [diagnostische logboeken openen voor Azure Data Lake Storage Gen1](data-lake-store-diagnostic-logs.md). 

Voor meer realtime waarschuwingen en meer controle over waar u het land van de logboeken, kunt u Logboeken exporteren naar Azure Event hub waar inhoud kan worden geanalyseerd afzonderlijk of gedurende een bepaalde periode om realtime meldingen naar een wachtrij te verzenden. Een afzonderlijke toepassing zoals een [logische App](../connectors/connectors-create-api-azure-event-hubs.md) kan vervolgens gebruiken en de waarschuwingen op het juiste kanaal communiceren, evenals metrische gegevens naar controlehulpprogramma's zoals NewRelic, Datadog of AppDynamics indienen. U kunt ook als u van een hulpprogramma van derden zoals ElasticSearch gebruikmaakt, u kunt de logboeken exporteren naar Blob-opslag en de [Azure Logstash-invoegtoepassing](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob) gebruiken voor de gegevens in uw stack Elasticsearch, Kibana en Logstash (ELK).

### <a name="turn-on-debug-level-logging-in-hdinsight"></a>Schakel op het foutopsporingsniveau logboekregistratie in HDInsight 

Als Data Lake Storage Gen1 back-upfunctie niet is ingeschakeld, Azure HDInsight biedt ook een manier om in te schakelen [client-side-registratie voor Data Lake Storage Gen1](data-lake-store-performance-tuning-mapreduce.md) via log4j. U moet de volgende eigenschap instellen **Ambari** > **YARN** > **Config** > **geavanceerde yarn-log4j configuraties**: 

    log4j.logger.com.microsoft.azure.datalake.store=DEBUG 

Als de eigenschap is ingesteld en de knooppunten opnieuw worden opgestart, Data Lake Storage Gen1 diagnostische gegevens naar de YARN-Logboeken is geschreven op de knooppunten (/tmp/<user>/yarn.log), en belangrijke informatie zoals fouten of beperking (foutcode HTTP 429), kan worden bewaakt. Deze informatie kan ook worden bewaakt in Log Analytics of waar de logboeken zijn verzonden naar in de [Diagnostics](data-lake-store-diagnostic-logs.md) blade van het Data Lake Storage Gen1-account. Het verdient aanbeveling ten minste client-side-logboekregistratie ingeschakeld of gebruikmaken van de optie met Data Lake Storage Gen1 voor operationele zichtbaarheid en beter foutopsporing-upfunctie voor logboekbestanden.

### <a name="run-synthetic-transactions"></a>Synthetische transacties uitvoeren 

Op dit moment heeft de beschikbaarheid van metrische servicegegevens in Azure portal voor de Data Lake Storage Gen1 7 minuten durende vernieuwen venster. Bovendien kan niet worden opgevraagd met behulp van een vrij toegankelijke API. Het verdient daarom kan een eenvoudige toepassing bouwt die synthetische transacties naar Data Lake Storage Gen1 u maximaal één minuut beschikbaar krijgt komt. Een voorbeeld van de maken een webtaak, logische App of Azure Function-App voor het uitvoeren van een lezen, maken, en bijwerken op basis van Data Lake Storage Gen1 en de resultaten verzenden naar uw oplossing voor controle. De bewerkingen kunnen worden uitgevoerd in een tijdelijke map en vervolgens verwijderd nadat de test, die elke 30 tot 60 seconden, afhankelijk van vereisten kan worden uitgevoerd.

## <a name="directory-layout-considerations"></a>Overwegingen voor Directory-indeling

Wanneer gegevens terechtkomen in een data lake, is het belangrijk dat u de structuur van de gegevens vooraf te plannen zodat beveiliging, partitionering en verwerking kunnen worden effectief gebruikt. Veel van de volgende aanbevelingen kunnen worden gebruikt of u nu met Azure Data Lake Storage Gen1, Blob-opslag of HDFS. Elke werkbelasting kent verschillende vereisten op hoe de gegevens is verbruikt, maar hieronder worden enkele algemene indeling om te overwegen bij het werken met IoT en batch-scenario's.

### <a name="iot-structure"></a>IoT-structuur 

In de IoT-workloads, kan er een grote hoeveelheid gegevens wordt bevindt zich in het gegevensarchief die zich over verschillende producten, apparaten, organisaties en klanten uitstrekt. Het is belangrijk de directory-indeling voor de organisatie, beveiliging en efficiënte verwerking van de gegevens voor de stroom consumenten vooraf te plannen. Een algemene sjabloon kunt u mogelijk de volgende indeling: 

    {Region}/{SubjectMatter(s)}/{yyyy}/{mm}/{dd}/{hh}/ 

Bijvoorbeeld: telemetrie voor een vliegtuig-engine in het Verenigd Koninkrijk aanvoer eruit kan de volgende structuur: 

    UK/Planes/BA1293/Engine1/2017/08/11/12/ 

Er is een belangrijke reden voor het plaatsen van de datum aan het einde van de mapstructuur. Als u vergrendelen bepaalde regio's of onderwerp zaken aan gebruikers/groepen wilt, kunt klikt u vervolgens u eenvoudig doen met de POSIX-machtigingen. Anders als er behoefte aan een bepaalde beveiligingsgroep bij het weergeven van alleen de gegevens van het Verenigd Koninkrijk of bepaalde vlakken beperken, is met de datumstructuur voor een afzonderlijke machtiging vereist voor een groot aantal mappen aan onder de map voor elk uur. Bovendien zou met de datumstructuur voor exponentieel toeneemt naarmate het aantal mappen tijd is een fout op.

### <a name="batch-jobs-structure"></a>Structuur van de batch-taken 

Een veelgebruikte aanpak in batchverwerking is van een op hoog niveau, op grond van gegevens in een map 'in'. Vervolgens, nadat de gegevens is verwerkt, plaatst u de nieuwe gegevens naar een map 'uit' voor downstream-processen te gebruiken. Deze directorystructuur wordt soms gebruikt voor taken die de verwerkingen uitvoert op afzonderlijke bestanden en krachtige parallelle verwerking niet mogelijk voor grote gegevenssets. Een goede directory-structuur heeft, zoals de IoT-structuur aanbevolen hierboven, de bovenliggende mappen voor, zoals de regio en onderwerp zaken (bijvoorbeeld, organisatie, product/producent). Deze structuur helpt bij het beveiligen van de gegevens in uw organisatie en beter beheer van de gegevens in uw workloads. Bovendien kunt u datum en tijd in de structuur voor betere organisatie, gefilterde zoekopdrachten, beveiliging en automatisering in de verwerking. Het granulariteitsniveau voor de datumstructuur wordt bepaald door het interval op waarmee de gegevens wordt geüpload naar of verwerkt, zoals elk uur, dagelijks, of zelfs maandelijks. 

Soms bestand verwerking is mislukt vanwege een onverwachte opmaak of beschadiging van gegevens. In dergelijke gevallen mapstructuur mogelijk profiteren van een **/ongeldige** map u de bestanden voor meer controle wilt verplaatsen. De batch-taak kan ook afhandelen het rapport of de melding van deze *beschadigde* bestanden voor handmatige tussenkomst. Houd rekening met de structuur van de volgende sjabloon: 

    {Region}/{SubjectMatter(s)}/In/{yyyy}/{mm}/{dd}/{hh}/ 
    {Region}/{SubjectMatter(s)}/Out/{yyyy}/{mm}/{dd}/{hh}/ 
    {Region}/{SubjectMatter(s)}/Bad/{yyyy}/{mm}/{dd}/{hh}/ 

Bijvoorbeeld, ontvangt een marketing vast dagelijkse gegevens extraheert van updates van de klant van hun clients in Noord-Amerika. Vóór en na te zijn verwerkt kan er uitzien zoals in het volgende codefragment: 

    NA/Extracts/ACMEPaperCo/In/2017/08/14/updates_08142017.csv 
    NA/Extracts/ACMEPaperCo/Out/2017/08/14/processed_updates_08142017.csv 
 
In de meeste gevallen van batchgegevens rechtstreeks in, zoals Hive-databases of traditionele SQL-databases die worden verwerkt, is er geen behoefte hebt aan een **/in** of **/out** map omdat de uitvoer reeds naar gaat een map voor de Hive-tabel of externe database te scheiden. Bijvoorbeeld, dagelijkse extracten van klanten in hun respectieve mappen en orchestration zou land door iets zoals Azure Data Factory, Apache Oozie, of Apache luchtstroom zou een dagelijkse Hive of Spark-taak om te verwerken en de gegevens schrijven naar een Hive-tabel activeren.

## <a name="next-steps"></a>Volgende stappen     

* [Overzicht van Azure Data Lake Storage Gen1](data-lake-store-overview.md) 
* [Toegangsbeheer in Azure Data Lake Storage Gen1](data-lake-store-access-control.md) 
* [Beveiliging in Azure Data Lake Storage Gen1](data-lake-store-security-overview.md)
* [Azure Data Lake Storage Gen1 voor prestaties afstemmen](data-lake-store-performance-tuning-guidance.md)
* [Richtlijnen voor het gebruik van HDInsight Spark met Azure Data Lake Storage Gen1 afstemmen van prestaties](data-lake-store-performance-tuning-spark.md)
* [Richtlijnen voor het gebruik van Hive in HDInsight met Azure Data Lake Storage Gen1 afstemmen van prestaties](data-lake-store-performance-tuning-hive.md)
* [Met Azure Data Factory voor Azure Data Lake Storage Gen1 gegevensindeling](https://mix.office.com/watch/1oa7le7t2u4ka)
* [Maken van HDInsight-clusters met Data Lake Storage Gen1](data-lake-store-hdinsight-hadoop-use-portal.md) 
