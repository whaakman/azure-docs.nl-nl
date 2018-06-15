---
title: Aanbevolen procedures voor het gebruik van Azure Data Lake Store | Microsoft Docs
description: Meer informatie over de aanbevolen procedures over gegevensopname, datum, beveiliging en prestaties voor het werken met Azure Data Lake Store
services: data-lake-store
documentationcenter: ''
author: sachinsbigdata
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: sachins
ms.openlocfilehash: 9fd6b72a7d09f85f7a6e60e5af4035ffc3862d2c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34625335"
---
# <a name="best-practices-for-using-azure-data-lake-store"></a>Aanbevolen procedures voor het gebruik van Azure Data Lake Store
In dit artikel leert u over aanbevolen procedures en overwegingen voor het werken met Azure Data Lake Store. Dit artikel bevat informatie over beveiliging, prestaties, tolerantie en bewaking voor Data Lake Store. Werken met echte big data in services zoals Azure HDInsight is voordat u Data Lake Store complex. Moest u gegevens verdelen over meerdere Blob storage-accounts zodat petabyte opslag en optimale prestaties op deze schaal kunnen worden gerealiseerd. Met Data Lake Store worden de meeste van de vaste limieten voor de grootte en de prestaties verwijderd. Er zijn echter nog steeds bepaalde overwegingen die van dit artikel behandelt zodat u de beste prestaties met Data Lake Store kunt ophalen. 

## <a name="security-considerations"></a>Beveiligingsoverwegingen

Azure Data Lake Store biedt POSIX toegang beheert en gedetailleerde controle voor Azure Active Directory (Azure AD) gebruikers, groepen en service-principals. Deze besturingselementen toegang kunnen worden ingesteld op de bestaande bestanden en mappen. De toegangscontrole kunnen ook worden gebruikt voor het maken van de standaardwaarden die kunnen worden toegepast op nieuwe bestanden of mappen. Als machtigingen zijn ingesteld op bestaande mappen en de onderliggende objecten, moeten de machtigingen doorgegeven recursief voor elk object. Als er veel bestanden, de machtigingen doorgeven lang duren kan. De tijd kan variëren tussen 30 tot 50 objecten per seconde is verwerkt. Plan daarom de structuur en gebruikersgroepen van de map op de juiste wijze aan. Anders kan dit onverwachte vertragingen en problemen veroorzaken wanneer u met uw gegevens werkt. 

Stel dat u hebt een map met 100.000 onderliggende objecten. Als u de ondergrens van 30 objecten die per seconde is verwerkt, voor het bijwerken van de machtiging voor de hele map kunnen een uur duren. Meer informatie over Data Lake Store ACL's zijn beschikbaar op [toegangsbeheer in Azure Data Lake Store](data-lake-store-access-control.md). Voor verbeterde prestaties op ACL's recursief toewijst, kunt u de Azure Data Lake Command-Line Tool. Het hulpprogramma maakt meerdere threads en recursieve navigatie logica snel naar miljoenen bestanden toepassen ACL's. Het hulpprogramma is beschikbaar voor Linux en Windows, en de [documentatie](https://github.com/Azure/data-lake-adlstool) en [downloadt](http://aka.ms/adlstool-download) voor dit hulpprogramma u op GitHub vindt. Deze dezelfde prestatieverbeteringen kunnen worden ingeschakeld door uw eigen hulpmiddelen die zijn geschreven met de Data Lake Store [.NET](data-lake-store-data-operations-net-sdk.md) en [Java](data-lake-store-get-started-java-sdk.md) SDK's.

### <a name="use-security-groups-versus-individual-users"></a>Beveiligingsgroepen versus afzonderlijke gebruikers gebruiken 

Als u werkt met big data in Data Lake Store, wordt waarschijnlijk een service-principal gebruikt voor het toestaan van services zoals Azure HDInsight werken met de gegevens. Echter, kunnen er gevallen waarin individuele gebruikers toegang tot de gegevens ook moeten. In dergelijke gevallen moet u Azure Active Directory [beveiligingsgroepen](data-lake-store-secure-data.md#create-security-groups-in-azure-active-directory) in plaats van afzonderlijke gebruikers toewijzen aan mappen en bestanden. 

Wanneer een beveiligingsgroep is toegewezen machtigingen, gebruikers toevoegen of verwijderen uit de groep heeft geen updates nodig hebben naar Data Lake Store. Dit zorgt er ook u niet langer zijn dan de limiet van [32 toegang en ACL's standaard](../azure-subscription-service-limits.md#data-lake-store-limits) (dit omvat de vier POSIX-stijl-ACL's die altijd gekoppeld aan elke bestanden en mappen zijn: [de gebruiker die eigenaar](data-lake-store-access-control.md#the-owning-user), [de eigenaar groep](data-lake-store-access-control.md#the-owning-group), [het masker](data-lake-store-access-control.md#the-mask-and-effective-permissions), en andere).

### <a name="security-for-groups"></a>Beveiliging voor groepen 

Zoals besproken, wanneer gebruikers moeten toegang tot de Data Lake Store is het raadzaam te gebruiken van Azure Active Directory-beveiligingsgroepen. Sommige groepen worden gestart met mogelijk aanbevolen **ReadOnlyUsers**, **WriteAccessUsers**, en **FullAccessUsers** voor de hoofdmap van het account en zelfs afzonderlijke waarden voor de sleutel submappen. Als er andere overwegen verwachte groepen van gebruikers die later kunnen worden toegevoegd, maar niet is geïdentificeerd nog u dummy-beveiligingsgroepen die toegang tot bepaalde mappen hebben te maken. Met behulp van de beveiligingsgroep, zorgt u ervoor dat later u niet lang hoeft verwerkingstijd voor het nieuwe machtigingen toewijzen aan duizenden bestanden. 

### <a name="security-for-service-principals"></a>Beveiliging voor service-principals 

Azure Active Directory-service-principals worden doorgaans gebruikt door services zoals Azure HDInsight voor toegang tot gegevens in Data Lake Store. Afhankelijk van de toegangsvereisten voor de meerdere werklasten mogelijk zijn enkele overwegingen voor een optimale beveiliging binnen en buiten de organisatie. Voor veel klanten een principal één Azure Active Directory-service wordt mogelijk voldoende en deze kan volledige machtigingen hebben in de hoofdmap van de Data Lake Store. Andere klanten mogelijk meerdere clusters met andere service-principals waar één cluster volledige toegang tot de gegevens en een ander cluster met alleen leestoegang heeft. Net als bij de beveiligingsgroepen kunt u een service-principal maken voor elk scenario (lezen, schrijven, volledige) verwacht wanneer een Data Lake Store-account is gemaakt. 

### <a name="enable-the-data-lake-store-firewall-with-azure-service-access"></a>De firewall Data Lake Store met Azure-Servicetoegang inschakelen 

Data Lake Store ondersteunt de optie van inschakelen van een firewall en alleen toegang tot Azure-services, wat wordt aanbevolen voor een kleinere aanvalsvector van externe beveiligingsrisico's te beperken. Firewall kan worden ingeschakeld voor het Data Lake Store-account in de Azure portal via de **Firewall** > **Firewall inschakelen (aan)** > **toegang tot Azure-services toestaan**  opties.  

![Firewall-instellingen in Data Lake Store](./media/data-lake-store-best-practices/data-lake-store-firewall-setting.png "Firewall-instellingen in Data Lake Store")

Zodra de firewall is ingeschakeld, worden alleen Azure-services, zoals HDInsight, hebt Data Factory, SQL Data Warehouse, enz. u toegang tot Data Lake Store. Als gevolg van de interne netwerkadresomzetting gebruikt door Azure, de firewall Data Lake Store biedt geen ondersteuning voor het beperken van de specifieke services door IP- en is alleen bedoeld voor beperkingen van eindpunten buiten Azure, bijvoorbeeld on-premises. 

## <a name="performance-and-scale-considerations"></a>Aandachtspunten voor prestaties en schaalbaarheid

Een van de meest krachtige functies van Data Lake Store is verwijdert deze de vaste limieten op de doorvoer van gegevens. Verwijderen van de limieten kan klanten hun gegevensgrootte groeien, en vergezeld gaan van de prestatie-eisen zonder dat u de gegevens hoeft te shard. Een van de meest belangrijke overwegingen voor het optimaliseren van de prestaties van Data Lake Store is dat deze het beste gegeven de parallelle uitvoering uitvoert.

### <a name="improve-throughput-with-parallelism"></a>De doorvoer te verbeteren met parallelle uitvoering 

8-12-threads per core overwegen voor de optimale doorvoer voor lezen/schrijven. Dit wordt veroorzaakt door blokkerende leest/schrijft op één thread en meer threads hoger gelijktijdigheid van taken op de virtuele machine kunnen toestaan. Om ervoor te zorgen dat de niveaus zijn in orde en parallelle uitvoering kan worden verhoogd, moet u voor het bewaken van de CPU-gebruik van de VM.   

### <a name="avoid-small-file-sizes"></a>Kleine bestandsgrootte voorkomen

POSIX-machtigingen en controle in Data Lake Store beschikt over een overhead die duidelijk bij het werken met een groot aantal kleine bestanden. Als een best practice moet u uw gegevens in grotere bestanden versus duizenden of miljoenen kleine bestanden schrijven naar het Data Lake Store batch. Vermijden van kleine bestandsgrootte kunt beschikt over meerdere voordelen, zoals:

* De verificatiecontroles in meerdere bestanden te verlagen
* Geopende bestanden verminderd
* Snellere kopiëren/replicatie
* Minder bestanden worden verwerkt bij het bijwerken van Data Lake Store POSIX-machtigingen 

Afhankelijk van welke services en werkbelastingen gegevens gebruikt worden, is het een goede grootte in overweging moet nemen voor bestanden 256 MB of hoger. Als de grootte kunnen niet batch worden opgenomen wanneer die terechtkomen in Data Lake Store, kunt u een afzonderlijke compressie-taak die deze bestanden tot grotere groepen combineert hebben. Zie voor meer informatie en aanbeveling op grootte en de gegevens in Data Lake Store ordenen [structuur, uw gegevensset](data-lake-store-performance-tuning-guidance.md#structure-your-data-set).

### <a name="large-file-sizes-and-potential-performance-impact"></a>Grootten van grote bestanden en potentiële invloed op de prestaties

Hoewel Data Lake Store biedt ondersteuning voor grote bestanden tot petabytes in grootte, voor optimale prestaties en afhankelijk van het proces voor het lezen van de gegevens kan niet zijn ideaal om te gaan gemiddeld boven de 2 GB. Wanneer u bijvoorbeeld **Distcp** om gegevens te kopiëren tussen locaties of andere storage-accounts, bestanden, het hoogste niveau van granulatie gebruikt om te bepalen van de kaart taken zijn. Dus als u 10 bestanden die zijn van 1 TB kopieert, maximaal 10 mappers toegewezen. Bovendien hebt u veel bestanden met mappers toegewezen, werken in eerste instantie de mappers parallel grote bestanden te verplaatsen. Als de taak wordt gestart kon worden afgesloten omlaag slechts enkele mappers blijven toegewezen en u kunt worden vastgelopen met een enkele mapper toegewezen aan een groot bestand. Microsoft heeft ingediend verbeteringen in Distcp voor het oplossen van dit probleem in toekomstige versies van Hadoop.  

Een ander voorbeeld rekening moet houden is bij het gebruik van Azure Data Lake Analytics met Data Lake Store. Afhankelijk van de verwerking uitgevoerd door de zelfstandig uitpakken, kunnen een aantal bestanden dat niet kunnen worden gesplitst (bijvoorbeeld XML, JSON) te maken in prestaties bij groter dan 2 GB. In gevallen waarbij bestanden kunnen worden gesplitst door een zelfstandig uitpakken (bijvoorbeeld, CSV), hebben grote bestanden de voorkeur.

### <a name="capacity-plan-for-your-workload"></a>Capaciteit plan voor uw workload 

Azure Data Lake Store Hiermee verwijdert u de vaste IO beperking-limieten die op de Blob storage-accounts zijn geplaatst. Er zijn echter nog steeds zachte limieten die moeten worden overwogen. De standaardlimiet voor inkomend en uitgaand bandbreedteregeling voldoen aan de behoeften van de meeste scenario's. Als uw werkbelasting nodig hebben de limieten verhoogd, samen met Microsoft ondersteuning. Bekijk ook de limieten tijdens de fase bewijs van concept zodat bandbreedteregeling i/o-limieten zijn bereikt tijdens de productie. Als dat gebeurt, moet deze mogelijk wachten voor een handmatige verhoging van het technische team van Microsoft. I/o-beperking optreedt, Azure Data Lake Store retourneert een foutcode van 429 als in het ideale geval moet opnieuw worden geprobeerd met een juiste exponentieel uitstel-beleid. 

### <a name="optimize-writes-with-the-data-lake-store-driver-buffer"></a>'Schrijft' optimaliseren met de stuurprogramma-buffer van Data Lake Store 

Uitvoeren om te optimaliseren en verlaagt IOP's bij het schrijven naar Data Lake Store uit Hadoop, schrijfbewerkingen zo dicht mogelijk bij de buffergrootte van Data Lake Store stuurprogramma mogelijk. Willen niet langer zijn dan de buffergrootte voordat, zoals wanneer streamen met Apache Storm of Spark-streaming-werkbelastingen. Bij het schrijven naar Data Lake Store uit HDInsight/Hadoop, is het belangrijk te weten dat Data Lake Store een stuurprogramma met een buffer 4 MB is. Net als veel bestand besturingsprogramma's, kan deze buffer handmatig worden leeggemaakt voordat de grootte van de 4 MB is bereikt. Als dat niet het geval is, worden onmiddellijk naar opslag leeggemaakt als de volgende schrijfactie de maximale buffergrootte overschrijdt. Waar mogelijk, moet u een overloop of een aanzienlijke underrun van de buffer voorkomen wanneer het beleid door het aantal of tijd venster synchroniseren/domeincontrollerlocaties leeg.

## <a name="resiliency-considerations"></a>Overwegingen voor tolerantie 

Wanneer een systeem met Data Lake Store of een cloudservice worden veranderd, moet u rekening houden met de beschikbaarheidsvereisten en reageren op potentiële onderbrekingen in de service. Een probleem met het specifieke exemplaar kan worden gelokaliseerd of zelfs regio hele, dus een plan dat voor beide belangrijk is. Afhankelijk van de **beoogde hersteltijd** en de **beoogd herstelpunt** serviceovereenkomsten voor uw werkbelasting, u kunt een meer of minder agressieve strategie voor hoge beschikbaarheid en herstel na noodgevallen.

### <a name="high-availability-and-disaster-recovery"></a>Hoge beschikbaarheid en herstel na noodgevallen 

Hoge beschikbaarheid (HA) en noodherstel (DR) kunnen soms worden gecombineerd, hoewel elk een strategie voor een enigszins verschillen, heeft vooral als het gaat om de gegevens. Data Lake Store verwerkt al 3 x replicatie achter de schermen ter bescherming tegen gelokaliseerde hardwarefouten. Omdat replicatie tussen regio's niet is ingebouwd in, moet u deze zelf beheren. Wanneer u een plan voor HA, in het geval van een onderbreking van de service moet de werkbelasting toegang tot de meest recente gegevens zo snel mogelijk via overschakelt naar een afzonderlijk gerepliceerde exemplaar lokaal of in een nieuw gebied.  

In een strategie voor Noodherstel voorbereiden voor het onwaarschijnlijke geval van een onherstelbare fout van een regio is het ook belangrijk om gegevens gerepliceerd naar een andere regio. Deze gegevens kan hetzelfde zijn als de gerepliceerde gegevens van de HA in eerste instantie zijn. U moet echter ook rekening houden uw vereisten voor rand gevallen, zoals waar u maken van periodieke momentopnamen wilt mogelijk voor het terugvallen op beschadiging van gegevens. Overweeg rolling delta momentopnamen van 1, 6 en 24-uurs punten op de winkel lokale en/of secundaire volgens toleranties risico afhankelijk van het belang en de grootte van de gegevens. 

Voor gegevenstolerantie met Data Lake Store verdient geo repliceren uw gegevens naar een afzonderlijke regio met een frequentie die voldoet aan uw vereisten HA-/ DR in het ideale geval elk uur. Deze replicatiefrequentie minimaliseert zeer grote hoeveelheden gegevens verplaatsingen waarvoor kunnen concurrerende doorvoer moet met het systeem en een betere beoogd herstelpunt (RPO). Bovendien kunt u overwegen manieren voor de toepassing met behulp van Data Lake Store automatisch een failover uitvoeren naar de secundaire account via bewaking triggers of lengte van mislukte pogingen of ten minste een melding te verzenden naar beheerders handmatig kunt ingrijpen. Houd er rekening mee dat er afweging mislukt via versus wachten op een service weer online is. Als de gegevens nog niet voltooid repliceren, kan een failover leiden tot verlies van gegevens, inconsistentie of complexe samenvoegen van de gegevens. 

Hieronder ziet u de belangrijkste drie aanbevolen opties voor het organiseren replicatie tussen de Data Lake Store-accounts en de belangrijkste verschillen tussen elk van deze servers.


|  |Distcp  |Azure Data Factory  |AdlCopy  |
|---------|---------|---------|---------|
|**Schaallimieten**     | Begrensd door worker-knooppunten        | Beperkt door de maximale Cloud gegevensverplaatsing eenheden        | Gebonden door Analytics-eenheden        |
|**Ondersteunt de delta's kopiëren**     |   Ja      | Nee         | Nee         |
|**Ingebouwde orchestration**     |  Nee (Oozie luchtstroom of cron taken gebruiken)       | Ja        | Nee (Azure Automation- of Windows Taakplanner gebruiken)         |
|**Ondersteunde bestandssystemen**     | ADL, HDFS, WASB, S3, GS, CFS        |Talrijke, Zie [Connectors](../data-factory/connector-azure-blob-storage.md).         | ADL naar ADL, WASB naar ADL (alleen bij dezelfde regio)        |
|**Ondersteuning van het besturingssysteem**     |Een Hadoop-besturingssysteem         | N/A          | Windows 10         |
   

### <a name="use-distcp-for-data-movement-between-two-locations"></a>Distcp gebruiken voor de verplaatsing van gegevens tussen twee locaties 

Kort voor gedistribueerde kopie, Distcp is een Linux-opdrachtregelprogramma dat wordt geleverd met Hadoop en biedt gedistribueerde gegevensverplaatsing tussen twee locaties. De twee locaties kunnen Data Lake Store, HDFS, WASB of S3 zijn. Dit hulpprogramma maakt gebruik van MapReduce-taken op een Hadoop-cluster (bijvoorbeeld HDInsight) uit te schalen op alle knooppunten. Distcp wordt beschouwd als de snelste manier om big gegevens zonder speciale compressie voor netwerkapparaten te verplaatsen. Distcp biedt ook een optie voor de delta's tussen twee locaties, automatische nieuwe pogingen ingangen, evenals dynamische schaling van compute alleen bij te werken. Deze aanpak is zeer efficiënte wanneer het gaat om het repliceren van items zoals Hive/Spark tabellen waarvoor veel grote bestanden in een enkele map en u alleen wilt kopiëren via de gewijzigde gegevens. Distcp is daarom het meest aanbevolen hulpprogramma voor het kopiëren van gegevens tussen big gegevensarchieven. 

Kopiëren taken kunnen worden geactiveerd door Apache Oozie werkstromen met behulp van de frequentie of gegevens triggers, evenals Linux cron taken. Voor intensieve replicatietaken verdient het draaien een afzonderlijke HDInsight Hadoop-cluster die kunnen worden afgestemd en specifiek voor de taken van het exemplaar wordt geschaald. Dit zorgt ervoor dat exemplaar taken niet kritieke taken verstoren. Het cluster kunt u als replicatie wordt uitgevoerd op een breed genoeg frequentie, zelfs offline gezet tussen elke taak. Als failover wordt uitgevoerd naar de secundaire regio, moet u een ander cluster ook in de secundaire regio worden gerepliceerd van nieuwe gegevens terug naar de primaire Data Lake Store-account zodra deze weer van wordt ingezet. Zie voor voorbeelden van het gebruik van Distcp [Distcp gebruiken om gegevens tussen Azure Storage-Blobs en Data Lake Store te kopiëren](data-lake-store-copy-data-wasb-distcp.md).

### <a name="use-azure-data-factory-to-schedule-copy-jobs"></a>Azure Data Factory gebruiken voor het plannen van taken kopiëren 

Azure Data Factory kan ook worden gebruikt voor het plannen van taken van exemplaar met behulp van een **Kopieeractiviteit**, en kan ook worden ingesteld op een frequentie via de **Wizard kopiëren**. Houd er rekening mee dat Azure Data Factory een limiet van cloud data movement eenheden (DMUs heeft) en uiteindelijk caps de doorvoer/compute voor werkbelastingen van grote hoeveelheden gegevens. Bovendien biedt Azure Data Factory momenteel geen updates van de verschillen tussen Data Lake Store-accounts, dus mappen toe, zoals Hive-tabellen een volledige kopie vereist te repliceren. Raadpleeg de [Kopieeractiviteit afstemmen handleiding](../data-factory/v1/data-factory-copy-activity-performance.md) voor meer informatie over het kopiëren van de Data Factory. 

### <a name="adlcopy"></a>AdlCopy

AdlCopy is een Windows-opdrachtregelprogramma waarmee u gegevens te kopiëren tussen twee Data Lake Store-accounts alleen binnen dezelfde regio. Het hulpprogramma AdlCopy biedt een optie voor zelfstandige of de optie voor het gebruik van een Azure Data Lake Analytics-account om uit te voeren van uw project kopiëren. Hoewel het oorspronkelijk gebouwd is op verzoek kopieën in plaats van een robuuste replicatie, bevat deze een andere optie om gedistribueerde kopiëren via Data Lake Store-accounts binnen dezelfde regio. Voor de betrouwbaarheid, is het raadzaam de premium Data Lake Analytics-optie gebruiken voor elke werkbelasting productie. De zelfstandige versie bezet reacties kunt terugkeren en beperkte schaal en bewaking. 

Zoals Distcp moet de AdlCopy worden gedirigeerd door iets zoals Azure Automation- of Windows Taakplanner. Net als bij de Data Factory AdlCopy biedt geen ondersteuning voor alleen bijgewerkte bestanden zijn gekopieerd, maar kopieert en bestaande bestanden overschrijven. Zie voor meer informatie en voorbeelden van het gebruik van AdlCopy [gegevens kopiëren van Azure Storage-Blobs naar Data Lake Store](data-lake-store-copy-data-azure-storage-blob.md).

## <a name="monitoring-considerations"></a>Overwegingen met betrekking tot bewaking 

Data Lake Store biedt gedetailleerde diagnostische logboeken en controle. Data Lake Store biedt een aantal basismetrieken in de Azure-portal onder het Data Lake Store-account en in Azure-Monitor. Beschikbaarheid van Data Lake Store wordt weergegeven in de Azure-portal. Met deze metriek wordt echter wordt elke zeven minuten vernieuwd en kan niet via een openbaar blootgestelde API worden opgevraagd. Als u de meest recente beschikbaarheid van een Data Lake Store-account, moet u uw eigen synthetische tests uit om de beschikbaarheid controleren uitvoeren. Andere metrische gegevens zoals gebruik van de totale opslag en lees-/ schrijfaanvragen inkomend en uitgaand kan tot 24 uur vernieuwen duren. Dus moeten meer up-to-date metrische gegevens handmatig via Hadoop-opdrachtregelprogramma's of verzamelen logboekinformatie worden berekend. De snelste manier om op te halen van de meest recente opslaggebruik met deze opdracht HDFS vanaf het knooppunt van een Hadoop-cluster (bijvoorbeeld hoofdknooppunt):   

    hdfs dfs -du -s -h adl://<adls_account_name>.azuredatalakestore.net:443/

### <a name="export-data-lake-store-diagnostics"></a>Diagnostische gegevens export Data Lake Store 

Een van de snelste manieren om toegang te krijgen tot doorzoekbare Logboeken in Data Lake Store is het inschakelen van de back-upfunctie voor logboekbestanden aan **logboekanalyse** onder de **Diagnostics** blade voor het Data Lake Store-account. Dit biedt directe toegang tot de binnenkomende logboeken met tijd en de inhoud filters, samen met opties (e-mailadres/webhook) waarschuwingen geactiveerd binnen 15 minuten durende intervallen. Zie voor instructies [toegang tot diagnoselogboeken voor Azure Data Lake Store](data-lake-store-diagnostic-logs.md). 

Voor meer realtime waarschuwingen en meer controle over waar u de logboeken land, kunt u Logboeken te exporteren naar Azure EventHub waar inhoud kan worden geanalyseerd afzonderlijk of via een tijdvenster om realtime meldingen naar een wachtrij te verzenden. Een afzonderlijke toepassing zoals een [logische App](../connectors/connectors-create-api-azure-event-hubs.md) kunnen vervolgens gebruiken en de waarschuwingen op het juiste kanaal communiceren, evenals metrische gegevens aan de bewaking van hulpprogramma's zoals NewRelic, Datadog of AppDynamics verzenden. U kunt ook als u een hulpprogramma van derden zoals ElasticSearch gebruikt, u kunt de logboeken te exporteren naar Blob Storage en gebruiken de [Azure Logstash invoegtoepassing](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob) gebruiken voor de gegevens in de stack Elasticsearch Kibana en Logstash (ELK).

### <a name="turn-on-debug-level-logging-in-hdinsight"></a>Schakelt foutopsporing-niveau van logboekregistratie in HDInsight 

Als Data Lake Store-upfunctie voor logboekbestanden niet is ingeschakeld, Azure HDInsight biedt ook een manier om in te schakelen [clientzijde logboekregistratie voor Data Lake Store](data-lake-store-performance-tuning-mapreduce.md) via log4j. U moet de volgende eigenschap instellen **Ambari** > **YARN** > **Config** > **geavanceerde yarn-log4j configuraties**: 

    log4j.logger.com.microsoft.azure.datalake.store=DEBUG 

Als de eigenschap is ingesteld en de knooppunten worden opnieuw opgestart, Data Lake Store diagnostics wordt geschreven naar de YARN-logboeken op de knooppunten (/tmp/<user>/yarn.log), en belangrijke informatie, zoals fouten of beperking (foutcode HTTP 429) kan worden bewaakt. Deze informatie kan ook worden bewaakt in logboekanalyse of waar logboeken worden verzonden in de [Diagnostics](data-lake-store-diagnostic-logs.md) blade van het Data Lake Store-account. Het verdient aanbeveling met ten minste clientzijde logboekregistratie is ingeschakeld, of gebruikmaken van de optie met Data Lake Store voor operationeel inzicht en gemakkelijker foutopsporing-upfunctie voor logboekbestanden.

### <a name="run-synthetic-transactions"></a>Synthetische transacties worden uitgevoerd 

De metrische servicegegevens beschikbaarheid voor Data Lake Store in de Azure portal is op dit moment 7 minuten durende vernieuwen venster. Bovendien kunnen niet worden opgevraagd met behulp van een openbaar blootgestelde API. Daarom is het aanbevolen een eenvoudige toepassing bouwt die synthetische transacties naar Data Lake Store die tot de beschikbaarheid van de minuut bieden kunnen komt. Het maken van een voorbeeld mogelijk een webtaak, logische App of Azure-functie-App als u wilt uitvoeren, lezen, maken, en bijwerken op basis van Data Lake Store en de resultaten verzenden naar uw oplossing voor controle. De bewerkingen worden uitgevoerd in een tijdelijke map en vervolgens verwijderd nadat de test, die elke 30 tot 60 seconden, afhankelijk van vereisten kan worden uitgevoerd.

## <a name="directory-layout-considerations"></a>Overwegingen voor Directory-indeling

Wanneer gegevens in een data lake aanvoer, het is belangrijk de structuur van de gegevens vooraf te plannen zodat beveiliging partitioneren en het verwerken van effectief worden gebruikt. Veel van de volgende aanbevelingen kunnen worden gebruikt of u nu met Azure Data Lake Store, Blob-opslag of HDFS. Elke werkbelasting heeft verschillende vereisten op hoe de gegevens wordt verbruikt, maar hieronder vindt u enkele algemene indeling rekening moet houden bij het werken met IoT en batch-scenario's.

### <a name="iot-structure"></a>IoT-structuur 

In de IoT-werkbelastingen, kan er een grote hoeveelheid gegevens wordt bevindt zich in de gegevensopslag die zich over meerdere producten, apparaten, organisaties en klanten. Het is belangrijk de directory-indeling voor de organisatie, beveiliging en efficiënte verwerking van de gegevens voor de stroom consumenten vooraf te plannen. Een algemene sjabloon in overweging moet nemen mogelijk de volgende indeling: 

    {Region}/{SubjectMatter(s)}/{yyyy}/{mm}/{dd}/{hh}/ 

Bijvoorbeeld telemetrie voor een vliegtuig-engine binnen het Verenigd Koninkrijk aanvoer kan er uitzien zoals de volgende structuur: 

    UK/Planes/BA1293/Engine1/2017/08/11/12/ 

Er is een belangrijke reden voor het plaatsen van de datum die aan het einde van de mapstructuur. Als u wilt vergrendelen bepaalde regio's of onderwerp belangrijk is voor gebruikers/groepen, kunt klikt u eenvoudig doen met de POSIX-machtigingen. Anders als er een behoefte te beperken tot een bepaalde beveiligingsgroep alleen de gegevens UK of bepaalde vlakken weergeeft, zijn met de datumstructuur op de voorgrond een afzonderlijke machtigingen vereist voor een groot aantal mappen aan onder elke map uur. Bovendien hebben de datumstructuur op de voorgrond zou exponentieel het aantal mappen toeneemt naarmate er meer tijd is een fout op.

### <a name="batch-jobs-structure"></a>Structuur van batch-taken 

Vanuit een hoog niveau is een veelgebruikte methode in batch-verwerking op grond van gegevens in een map 'in'. Vervolgens, nadat de gegevens is verwerkt, plaats de nieuwe gegevens in een map 'out' voor downstream-processen te gebruiken. Deze directorystructuur wordt soms gebruikt voor taken die de verwerking voor afzonderlijke bestanden vereisen en mogelijk niet massively parallelle verwerking via grote gegevenssets. Als de IoT-structuur hierboven aanbevolen, heeft een goede mapstructuur de bovenliggende mappen voor dingen zoals regio en onderwerp zaken (bijvoorbeeld, organisatie, product/producent). Deze structuur helpt bij het beveiligen van de gegevens over uw organisatie en beter beheer van de gegevens in uw werkbelastingen. Overweeg daarnaast datum en tijd in de structuur, zodat betere ordening, gefilterde zoekopdrachten, beveiliging en automatisering bij de verwerking. De mate van detail voor de datumstructuur wordt bepaald door het interval waarop de gegevens worden geüpload of verwerkt, zoals elk uur, dagelijks of zelfs maandelijks. 

Soms bestand verwerking is mislukt vanwege gegevensbeschadiging of onverwachte indeling. In dergelijke gevallen mapstructuur kan profiteren van een **/slechte** map u de bestanden voor meer controle wilt verplaatsen. De batch-job mogelijk ook moet u de rapportage of melding van deze *onjuiste* bestanden handmatig kunt ingrijpen. Houd rekening met de structuur van de volgende sjabloon: 

    {Region}/{SubjectMatter(s)}/In/{yyyy}/{mm}/{dd}/{hh}/ 
    {Region}/{SubjectMatter(s)}/Out/{yyyy}/{mm}/{dd}/{hh}/ 
    {Region}/{SubjectMatter(s)}/Bad/{yyyy}/{mm}/{dd}/{hh}/ 

Bijvoorbeeld, ontvangt een marketing bedrijf dagelijkse gegevens uitgepakt van updates van de klant van hun clients in Noord-Amerika. Het lijkt op het volgende fragment voor en na wordt verwerkt: 

    NA/Extracts/ACMEPaperCo/In/2017/08/14/updates_08142017.csv 
    NA/Extracts/ACMEPaperCo/Out/2017/08/14/processed_updates_08142017.csv 
 
In de meeste gevallen van batch gegevens rechtstreeks in databases zoals Hive of traditionele SQL-databases wordt verwerkt, is er geen behoefte aan een **/in** of **/out** map omdat de uitvoer al probeert een map voor de Hive-tabel of de externe database moeten worden gescheiden. Bijvoorbeeld dagelijks uitgepakt van klanten in hun respectieve mappen en orchestration door Azure Data Factory Apache Oozie ongeveer zou land of Apache luchtstroom zou een dagelijkse taak voor Hive of Spark om te verwerken en de gegevens schrijven naar een Hive-tabel activeren.

## <a name="next-steps"></a>Volgende stappen     

* [Overzicht van Azure Data Lake Store](data-lake-store-overview.md) 
* [Toegangsbeheer in Azure Data Lake Store](data-lake-store-access-control.md) 
* [Beveiliging in Azure Data Lake Store](data-lake-store-security-overview.md)
* [Azure Data Lake Store voor prestaties afstemmen](data-lake-store-performance-tuning-guidance.md)
* [Prestaties afstemmen richtlijnen voor het gebruik van HDInsight Spark met Azure Data Lake Store](data-lake-store-performance-tuning-spark.md)
* [Prestaties afstemmen richtlijnen voor het gebruik van HDInsight Hive met Azure Data Lake Store](data-lake-store-performance-tuning-hive.md)
* [Organisatie van gegevens met behulp van Azure Data Factory voor Azure Data Lake Store](https://mix.office.com/watch/1oa7le7t2u4ka)
* [HDInsight-clusters maken met Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md) 
