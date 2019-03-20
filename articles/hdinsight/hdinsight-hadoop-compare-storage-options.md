---
title: Opties voor opslag voor gebruik met Azure HDInsight-clusters vergelijken
description: Biedt een overzicht van storage-typen en hoe ze werken met Azure HDInsight.
services: hdinsight,storage
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/04/2019
ms.openlocfilehash: fa08d2fb2185bd4b6cd0e2e9d20e1c44a4a35eae
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58101479"
---
# <a name="compare-storage-options-for-use-with-azure-hdinsight-clusters"></a>Opties voor opslag voor gebruik met Azure HDInsight-clusters vergelijken

U kunt kiezen tussen een paar andere Azure storage-services bij het maken van HDInsight-clusters:

* Azure Storage
* Azure Data Lake Storage Gen2
* Azure Data Lake Storage Gen1

Dit artikel bevat een overzicht van deze opslagtypen en de bijbehorende unieke functies.

De volgende tabel geeft een overzicht van de Azure Storage-services die worden ondersteund met verschillende versies van HDInsight:

| Opslagservice | Accounttype | Namespace-Type | Ondersteunde services | Ondersteunde prestatielagen | Ondersteunde toegangslagen | HDInsight-versie | Clustertype |
|---|---|---|---|---|---|---|---|
|Azure Data Lake Storage Gen2| Algemeen gebruik V2 | Hiërarchisch (bestandssysteem) | Blob | Standard | Hot, Cool en Archive | 3.6 + | Alle |
|Azure Storage| Algemeen gebruik V2 | Object | Blob | Standard | Hot, Cool en Archive | 3.6 + | Alle |
|Azure Storage| Algemeen gebruik V1 | Object | Blob | Standard | N/A | Alle | Alle |
|Azure Storage| Blob Storage | Object | Blob | Standard | Hot, Cool en Archive | Alle | Alle |
|Azure Data Lake Storage Gen1| N/A | Hiërarchisch (bestandssysteem) | N/A | N/A | N/A | Alleen 3.6 | Overal behalve HBase |

Zie voor meer informatie over Azure Storage-toegangslagen [Azure Blob-opslag: Premium (preview), Hot, Cool en Archive storage-lagen](../storage/blobs/storage-blob-storage-tiers.md)

U kunt een cluster met behulp van verschillende combinaties van services voor het primaire en een optionele secundaire opslag maken. De volgende tabel geeft een overzicht van de cluster-opslagconfiguraties die momenteel worden ondersteund in HDInsight:

| HDInsight-versie | Primaire opslag | Secundaire opslag | Ondersteund |
|---|---|---|---|
| 3.6 & 4.0 | Standaard-Blob | Standaard-Blob | Ja |
| 3.6 & 4.0 | Standaard-Blob | Data Lake Storage Gen2 | Nee |
| 3.6 & 4.0 | Standaard-Blob | Data Lake Storage Gen1 | Ja |
| 3.6 & 4.0 | Data Lake Storage Gen2 * | Data Lake Storage Gen2 | Ja |
| 3.6 & 4.0 | Data Lake Storage Gen2 * | Standaard-Blob | Ja |
| 3.6 & 4.0 | Data Lake Storage Gen2 | Data Lake Storage Gen1 | Nee |
| 3.6 | Data Lake Storage Gen1 | Data Lake Storage Gen1 | Ja |
| 3.6 | Data Lake Storage Gen1 | Standaard-Blob | Ja |
| 3.6 | Data Lake Storage Gen1 | Data Lake Storage Gen2 | Nee |
| 4.0 | Data Lake Storage Gen1 | Alle | Nee |

* = Dit wordt mogelijk een of meerdere accounts van Data Lake Storage Gen2, zolang ze alle ingesteld zijn met de dezelfde beheerde identiteit voor toegang tot het cluster.

## <a name="use-azure-data-lake-storage-gen2-with-apache-hadoop-in-azure-hdinsight"></a>Azure Data Lake Storage Gen2 met Apache Hadoop in Azure HDInsight gebruiken

Azure Data Lake Storage Gen2 duurt core functies van Azure Data Lake Storage Gen1 en ze integreert in Azure Blob-opslag. Deze functies omvatten een bestandssysteem dat compatibel is met Hadoop, Azure Active Directory (Azure AD), en POSIX gebaseerde toegangsbeheerlijsten (ACL's). Deze combinatie kunt u profiteren van de prestaties van Azure Data Lake Storage Gen1 en tegelijkertijd het beheer van de levenscyclus van opslaglagen en gegevens van Blob-opslag.

Zie voor meer informatie over Azure Data Lake Storage Gen2 [Inleiding tot Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md).

### <a name="core-functionality-of-azure-data-lake-storage-gen2"></a>Kernfunctionaliteit van Azure Data Lake Storage Gen2

* **Toegang tot die compatibel is met Hadoop:** U kunt in Azure Data Lake Storage Gen2, beheren en toegang tot gegevens, net zoals u zou met een Hadoop Distributed File System (HDFS doen). Het stuurprogramma van de Azure Blob File System (ABFS) is beschikbaar in alle Apache Hadoop-omgevingen, met inbegrip van Azure HDInsight en Azure Databricks. ABFS gebruiken voor toegang tot gegevens die zijn opgeslagen in Data Lake Storage Gen2.

* **Een hoofdverzameling van POSIX-machtigingen:** Het beveiligingsmodel voor Data Lake Gen2 biedt ondersteuning voor ACL en POSIX-machtigingen, samen met enkele extra granulariteit die specifiek zijn voor Data Lake Storage Gen2. Instellingen kunnen worden geconfigureerd via de beheerhulpprogramma's of frameworks, zoals Apache Hive en Apache Spark.

* **Kosteneffectiviteit:** Data Lake Storage Gen2 biedt de opslagcapaciteit van de lage kosten en transacties. Functies zoals Azure Blob storage-levenscyclus helpen kosten te verlagen door aan te passen factureringstarieven als gegevens worden verplaatst via de levenscyclus.

* **Compatibiliteit met Blob storage-hulpprogramma's, -frameworks en -apps:** Data Lake Storage Gen2 blijft gewoon werken met een breed scala aan hulpprogramma's, frameworks en -toepassingen voor Blob-opslag.

* **Geoptimaliseerde stuurprogramma:** Het stuurprogramma ABFS is specifiek voor analyse van big data geoptimaliseerd. De bijbehorende REST-API's zijn opgehaald via het distributed file system (DFS)-eindpunt, dfs.core.windows.net.

### <a name="whats-new-for-azure-data-lake-storage-gen-2"></a>Wat is er nieuw voor Azure Data Lake Storage Gen 2

#### <a name="managed-identities-for-secure-file-access"></a>Beheerde identiteiten voor toegang tot het beveiligde bestand

Azure HDInsight maakt gebruik van beheerde identiteiten voor het beveiligen van toegang tot het cluster tot bestanden in Azure Data Lake Storage Gen2. Beheerde identiteiten zijn een functie van Azure Active Directory die Azure-services een set referenties automatisch beheerd biedt. Deze referenties kunnen worden gebruikt om te verifiëren bij een service die ondersteuning biedt voor Active Directory-verificatie. Met behulp van beheerde identiteiten, hoeft u het opslaan van referenties in code of configuratie bestanden.

Zie voor meer informatie, [beheerde identiteiten voor een Azure-resources](../active-directory/managed-identities-azure-resources/overview.md).

#### <a name="azure-blob-file-system-driver"></a>Azure Blob-bestandssysteem-stuurprogramma

Apache Hadoop-toepassingen verwachten systeemeigen te lezen en schrijven van gegevens uit de opslag van de lokale schijf. Een stuurprogramma van het bestandssysteem Hadoop zoals ABFS kan Hadoop-toepassingen voor opslag in de cloud door emulatie van het normale bewerkingen in Hadoop het bestandssysteem. Het stuurprogramma converteert deze opdrachten ontvangen van de toepassing in bewerkingen die het werkelijke cloudopslagplatform begrijpt.

Voorheen was het stuurprogramma van het bestandssysteem Hadoop alle bewerkingen in het bestandssysteem geconverteerd naar Azure Storage REST API-aanroepen aan de clientzijde en vervolgens de REST-API aangeroepen. Deze client-side conversie, maar heeft geresulteerd in meerdere REST-API-aanroepen voor een enkel bestand system-bewerking, zoals de naam van een bestand. ABFS heeft enkele van de Hadoop-bestand systeemlogica verplaatst vanaf de client aan de serverzijde. De API van Azure Data Lake Storage Gen2 wordt nu uitgevoerd in combinatie met de Blob-API. Deze migratie verbetert de prestaties omdat nu algemene Hadoop-bestand systeembewerkingen kunnen worden uitgevoerd met een REST-API-aanroep.

Zie voor meer informatie, [stuurprogramma voor de Azure Blob-bestandssysteem (ABFS): Een speciale Azure Storage-stuurprogramma voor Hadoop](../storage/blobs/data-lake-storage-abfs-driver.md).

#### <a name="uri-scheme-for-azure-data-lake-storage-gen-2"></a>URI-schema voor Azure Data Lake Storage Gen 2 

Azure Data Lake Storage Gen2 maakt gebruik van een nieuwe URI-schema om bestanden te openen in Azure Storage vanuit HDInsight:

`abfs[s]://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/<PATH>`

Het URI-schema biedt SSL-versleutelde toegang (`abfss://` voorvoegsel) en niet-versleutelde toegang (`abfs://` voorvoegsel). Gebruik `abfss` waar mogelijk, zelfs bij het openen van gegevens die zich in dezelfde regio in Azure.

`<FILE_SYSTEM_NAME>` Hiermee geeft u het pad van het bestandssysteem Data Lake Storage Gen2.

`<ACCOUNT_NAME>` Geeft de naam van Azure Storage-account. Een FQDN (Fully Qualified Domain Name) is vereist.

`<PATH>` is het bestand of map HDFS-padnaam.

Als de waarden voor `<FILE_SYSTEM_NAME>` en `<ACCOUNT_NAME>` zijn niet opgegeven, wordt het standaardbestandssysteem gebruikt. Gebruik een relatief pad of een absoluut pad voor de bestanden op het standaardbestandssysteem. Bijvoorbeeld, de `hadoop-mapreduce-examples.jar` -bestand dat wordt geleverd met HDInsight-clusters kan worden aangeduid met behulp van een van de volgende paden:

```
abfss://myfilesystempath@myaccount.dfs.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
abfss:///example/jars/hadoop-mapreduce-examples.jar /example/jars/hadoop-mapreduce-examples.jar
```

> [!Note]
> De bestandsnaam is `hadoop-examples.jar` in HDInsight versie 2.1- en 1.6-clusters. Als u met bestanden buiten HDInsight werkt, ziet u dat de meeste hulpprogramma's niet herkent de ABFS indeling, maar in plaats daarvan een standaardpadindeling zoals verwacht `example/jars/hadoop-mapreduce-examples.jar`.

Zie voor meer informatie, [gebruiken de Azure Data Lake Storage Gen2 URI](../storage/blobs/data-lake-storage-introduction-abfs-uri.md).

## <a name="azure-storage"></a>Azure Storage

Azure Storage is een robuuste algemene opslagoplossing die naadloos geïntegreerd met HDInsight. HDInsight kan een blobcontainer in Azure Storage gebruiken als het standaardbestandssysteem voor het cluster. Via een HDFS-interface, kan de volledige set onderdelen in HDInsight rechtstreeks op gestructureerde of ongestructureerde gegevens als blobs werken.

Beste afzonderlijke storage-containers voor de standaardclusteropslag en uw bedrijfsgegevens gebruiken voor het isoleren van de HDInsight-logboeken en de tijdelijke bestanden uit uw eigen zakelijke gegevens. We raden ook aan de standaard-blobcontainer, waarin toepassings- en systeemlogboeken, verwijderen na elke gebruik om te verminderen van kosten voor gegevensopslag. Breng de logboeken over naar een andere locatie voordat u de container verwijdert.

### <a name="hdinsight-storage-architecture"></a>HDInsight-opslagarchitectuur

Het volgende diagram biedt een abstracte weergave van de HDInsight-architectuur van Azure Storage:

![Diagram waarin wordt getoond hoe Hadoop-clusters gebruiken de HDFS API voor toegang tot en gestructureerde en ongestructureerde gegevens opslaan in blobopslag](./media/hdinsight-hadoop-compare-storage-options/HDI.WASB.Arch.png "HDInsight-opslagarchitectuur")

HDInsight biedt toegang tot het Distributed File System dat lokaal wordt gekoppeld aan de rekenknooppunten. Dit bestandssysteem is toegankelijk via de volledig gekwalificeerde URI, bijvoorbeeld:

    hdfs://<namenodehost>/<path>

Tot en met HDInsight kunt u ook toegang tot gegevens in Azure Storage. De syntaxis is als volgt:

    wasb[s]://<containername>@<accountname>.blob.core.windows.net/<path>

Wanneer een Azure Storage-account gebruiken met HDInsight-clusters, houd rekening met de volgende principes:

* **Containers in de storage-accounts die zijn verbonden met een cluster:** Omdat de accountnaam en de sleutel gekoppeld aan het cluster tijdens het maken zijn, hebt u volledige toegang tot de blobs in deze containers.

* **Openbare containers of openbare blobs in opslagaccounts die zijn *niet* verbonden met een cluster:** U hebt alleen-lezen toegang tot de blobs in de containers.
  
  > [!NOTE]  
  > Openbare containers kunt u om een lijst met alle blobs die beschikbaar in die container zijn en containermetagegevens ophalen. Openbare blobs zijn alleen toegankelijk als u de exacte URL weet. Zie [Anonieme leestoegang tot containers en blobs beheren](../storage/blobs/storage-manage-access-to-resources.md) voor meer informatie.

* **Persoonlijke containers in opslagaccounts die zijn *niet* verbonden met een cluster:** U kunt de blobs in de containers geen toegang tot, tenzij u het opslagaccount definieert wanneer u de WebHCat-taken verzendt. 

De opslagaccounts die worden gedefinieerd tijdens het creatieproces en de bijbehorende sleutels worden opgeslagen in %HADOOP_HOME%/conf/core-site.xml op de clusterknooppunten. HDInsight gebruikt standaard de opslagaccounts die zijn gedefinieerd in het bestand core-site.xml. U kunt deze instelling wijzigen met behulp van [Apache Ambari](./hdinsight-hadoop-manage-ambari.md).

Meerdere WebHCat-taken, waaronder Apache Hive, MapReduce, Apache Hadoop-streaming en Apache Pig, kunnen een beschrijving van opslagaccounts en metagegevens mee uitvoeren. (Dit geldt momenteel voor Pig met opslagaccounts, maar niet voor metagegevens.) Zie voor meer informatie, [met behulp van een HDInsight-cluster met alternatieve opslagaccounts en metastores](https://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).

Blobs kunnen worden gebruikt voor gestructureerde en ongestructureerde gegevens. BLOB-containers worden gegevens opgeslagen als sleutel/waarde-paren en geen maphiërarchie hebben. Naam van de sleutel kan echter een slash (/) zodat het lijkt alsof een bestand is opgeslagen in een mapstructuur bevatten. Voor de sleutel van een blob kan bijvoorbeeld `input/log1.txt`. Er is niet echt `input` aanwezig, maar vanwege het slash-teken in naam van de sleutel, de sleutel ziet eruit als een bestandspad.

### <a id="benefits"></a>Voordelen van Azure Storage
Rekenclusters en opslagresources die niet geplaatst zijn hebben impliciete prestatiekosten in. Deze kosten worden verholpen door de manier waarop die de rekenclusters dicht bij de resources van het opslagaccount in de Azure-regio. In deze regio, de rekenknooppunten efficiënt via toegang tot de gegevens dankzij het netwerk in Azure Storage.

Wanneer u de gegevens in Azure Storage in plaats van HDFS opslaat, krijgt u verschillende voordelen:

* **Hergebruik van de gegevens en het delen van:** De gegevens in HDFS bevindt zich in het rekencluster. Alleen de toepassingen die toegang tot het rekencluster hebben, kunnen de gegevens met HDFS API's gebruiken. De gegevens in Azure Storage, daarentegen, is toegankelijk via de HDFS API's of REST-API's van de Blob-opslag. Vanwege deze opstelling weergegeven, kan een grotere set van toepassingen (inclusief andere HDInsight-clusters) en hulpprogramma's worden gebruikt om te produceren en gebruiken de gegevens.

* **Archivering van gegevens:** Wanneer gegevens worden opgeslagen in Azure Storage, kunnen de HDInsight-clusters die worden gebruikt voor berekeningen veilig worden verwijderd zonder verlies van gebruikersgegevens.

* **Kosten voor gegevensopslag:** Opslaan van gegevens voor de lange termijn in DFS is duurder dan wanneer de gegevens opgeslagen in Azure Storage, omdat de kosten van een rekencluster hoger zijn dan de kosten van Azure Storage. Omdat de gegevens niet kan worden geladen voor elk rekencluster dat wordt gegenereerd, bent u ook kosten voor het laden van de gegevens ook opslaan.

* **Elastisch uitbreiden:** Hoewel HDFS u een scale-out-bestandssysteem biedt, wordt de schaal bepaald door het aantal knooppunten die u voor uw cluster maakt. Wijzigen van de schaal is gecompliceerder dan te vertrouwen op de elastische schalingsmogelijkheden waarover u automatisch in Azure Storage beschikt.

* **Geo-replicatie:** De Azure-opslag kan geografisch worden gerepliceerd. Hoewel geo-replicatie u geografisch herstel en gegevensredundantie biedt, een failover naar de geografisch gerepliceerde locatie ernstige problemen is van invloed op de prestaties van uw en kan deze extra kosten in rekening. Geo-replicatie dus kies voorzichtig en alleen als de waarde van de gegevens de extra kosten uitlijnen.

Bepaalde MapReduce-taken en -pakketten kunnen tussenliggende resultaten genereren die u wilt natuurlijk niet opslaan in Azure Storage maken. In dat geval kunt u kiezen voor het opslaan van de gegevens in de lokale HDFS. HDInsight gebruikt DFS voor verschillende tussenliggende resultaten in Hive-taken en andere processen.

> [!NOTE]  
> De meeste HDFS-opdrachten (bijvoorbeeld `ls`, `copyFromLocal`, en `mkdir`) werkt zoals verwacht in Azure Storage. Alleen de opdrachten die specifiek voor de systeemeigen HDFS-implementatie zijn (die wordt aangeduid als DFS), zoals `fschk` en `dfsadmin`, verschillend gedrag weergeven in Azure Storage.

## <a name="overview-of-azure-data-lake-storage-gen1"></a>Overzicht van Azure Data Lake Storage Gen1

Azure Data Lake Storage Gen1 is een bedrijfsbrede grootschalige opslagplaats voor big data-analysewerklasten. Met behulp van Azure Data Lake, kunt u gegevens van elke grootte, het type en de snelheid van de opname op één locatie voor operationele en verkennende analyse vastleggen.

Toegang tot Data Lake Storage Gen1 via Hadoop (beschikbaar met een HDInsight-cluster) met behulp van de WebHDFS compatibele REST-API's. Data Lake Storage Gen1 is ontworpen om te kunnen analyseren voor de opgeslagen gegevens en is afgestemd op prestaties in scenario's met gegevensanalyses. Het bevat de mogelijkheden die essentieel voor echte zakelijke gebruiksvoorbeelden zijn buiten het vak. Deze mogelijkheden omvatten beveiliging, beheerbaarheid, schaalbaarheid, betrouwbaarheid en beschikbaarheid.

Zie voor meer informatie over Azure Data Lake Storage Gen1, de gedetailleerde [overzicht van Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-overview.md).

De belangrijkste mogelijkheden van Data Lake Storage Gen1 omvatten het volgende.

### <a name="compatibility-with-hadoop"></a>Compatibiliteit met Hadoop

Data Lake Storage Gen1 is een Apache Hadoop-bestandssysteem dat compatibel is met HDFS en werkt met het Hadoop-ecosysteem.  Uw bestaande HDInsight-toepassingen of services die gebruikmaken van de API WebHDFS kunnen eenvoudig integreren met Data Lake Storage Gen1. Data Lake Storage Gen1 wordt ook aangegeven dat een WebHDFS compatibele REST-interface voor toepassingen.

Gegevens die zijn opgeslagen in Data Lake Storage Gen1 kunnen eenvoudig worden geanalyseerd met analytische frameworks van Hadoop zoals MapReduce of Hive. Azure HDInsight-clusters kunnen worden ingericht en geconfigureerd voor rechtstreekse toegang tot gegevens die zijn opgeslagen in Data Lake Storage Gen1.

### <a name="unlimited-storage-petabyte-files"></a>Onbeperkte opslag, bestanden ter grootte van petabytes

Data Lake Storage Gen1 biedt onbeperkte opslag en is geschikt voor het opslaan van een verscheidenheid aan gegevens voor analyse. Deze opleggen limieten voor grootte van accounts, bestanden of de hoeveelheid gegevens die kunnen worden opgeslagen in een data lake niet. Afzonderlijke bestanden kan uiteenlopen van de grootte van kilobyte tot petabytes, als u Data Lake Storage Gen1 een uitstekende keuze voor het opslaan van elk type gegevens. Gegevens worden blijvend opgeslagen door meerdere kopieën te maken en er zijn geen beperkingen op hoe lang de gegevens kunnen worden opgeslagen in de data lake.

### <a name="performance-tuning-for-big-data-analytics"></a>Prestaties afstemmen voor big data analytics

Data Lake Storage Gen1 is gebouwd voor het uitvoeren van grootschalige analytische systemen waarvoor grote doorvoer doorzoeken en analyseren van grote hoeveelheden gegevens vereist. De data lake verspreidt delen van een bestand via verschillende afzonderlijke opslagservers. Wanneer u gegevens analyseert, verbetert deze installatie de leesdoorvoer als het bestand in parallel wordt gelezen.

### <a name="readiness-for-enterprise-highly-available-and-secure"></a>Gereedheid voor onderneming: Maximaal beschikbare en veilige

Data Lake Storage Gen1 biedt industriestandaard beschikbaarheid en betrouwbaarheid. Gegevensassets worden blijvend opgeslagen: redundante exemplaren bescherming tegen onverwachte fouten. Ondernemingen kunnen Data Lake Storage Gen1 in hun oplossingen gebruiken als een belangrijk onderdeel van hun bestaande gegevensplatform.

Data Lake Storage Gen1 biedt ook geavanceerde beveiliging voor opgeslagen gegevens. Zie voor meer informatie, [gegevens beveiligen in Azure Data Lake Storage Gen1](#DataLakeStoreSecurity).

### <a name="flexible-data-structures"></a>Flexibele gegevensstructuren

Data Lake Storage Gen1 kunnen alle gegevens worden opgeslagen in de oorspronkelijke indeling, zoals ze gemaakt zijn, zonder eerst om te zetten. Data Lake Storage Gen1 zijn vereist om een schema worden gedefinieerd voordat de gegevens zijn geladen. Het afzonderlijke analytische framework de gegevens worden geïnterpreteerd en definieert een schema op het moment van de analyse. Omdat deze bestanden van verschillende groottes en indelingen kunt opslaan, kan Data Lake Storage Gen1 gestructureerde, semigestructureerde en ongestructureerde gegevens verwerken.

Data Lake Storage Gen1 containers voor gegevens zijn eigenlijk mappen en bestanden. U kunt gebruiken voor de opgeslagen gegevens met behulp van SDK's, de Azure-portal en Azure PowerShell. Als u uw gegevens in het archief met behulp van deze interfaces en de juiste containers plaatst, kunt u elk type gegevens kunt opslaan. Data Lake Storage Gen1 voert niet geen speciale-verwerking van gegevens op basis van het type gegevens dat wordt opgeslagen.

## <a name="DataLakeStoreSecurity"></a>Beveiliging van gegevens in Data Lake Storage Gen1
Data Lake Storage Gen1 gebruikt Azure Active Directory voor verificatie en maakt gebruik van toegangsbeheerlijsten (ACL's) voor het beheren van toegang tot uw gegevens.

| **Functie** | **Beschrijving** |
| --- | --- |
| Authentication |Data Lake Storage Gen1 kan worden geïntegreerd met Azure Active Directory (Azure AD) voor identiteits- en toegangsbeheer voor alle gegevens die zijn opgeslagen in Data Lake Storage Gen1. Vanwege de integratie, Data Lake Storage Gen1 profiteren van alle Azure AD-functies. Deze functies omvatten multi-factor authentication, voorwaardelijke toegang, op rollen gebaseerd toegangsbeheer, bewaking van toepassingsgebruik, beveiligingsbewaking en waarschuwingen, enzovoort. Data Lake Storage Gen1 ondersteunt het OAuth 2.0-protocol voor verificatie in de REST-interface. Zie [verificatie in Azure Data Lake Storage Gen1 met behulp van Azure Active Directory](../data-lake-store/data-lakes-store-authentication-using-azure-active-directory.md)|
| Toegangsbeheer |Data Lake Storage Gen1 biedt toegangsbeheer door ondersteuning POSIX-machtigingen die beschikbaar worden gesteld door het protocol WebHDFS. ACL's kunnen worden ingeschakeld voor de hoofdmap, submappen en afzonderlijke bestanden. Zie voor meer informatie over de werking van ACL's in de context van Data Lake Storage Gen1 [toegangsbeheer in Data Lake Storage Gen1](../data-lake-store/data-lake-store-access-control.md). |
| Versleuteling |Data Lake Storage Gen1 biedt ook versleuteling voor gegevens die zijn opgeslagen in het account. U geeft de versleutelingsinstellingen op tijdens het maken van een Data Lake Storage Gen1-account. U kunt de gegevens te versleutelen of opt niet te versleutelen. Zie voor meer informatie, [versleuteling in Data Lake Storage Gen1](../data-lake-store/data-lake-store-encryption.md). Zie voor instructies over het bieden van een versleuteling-gerelateerde configuratie [aan de slag met Azure Data Lake Storage Gen1 met behulp van de Azure-portal](../data-lake-store/data-lake-store-get-started-portal.md). |

Zie voor meer informatie over het beveiligen van gegevens in Data Lake Storage Gen1 [beveiligen van gegevens die zijn opgeslagen in Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-secure-data.md).

## <a name="applications-that-are-compatible-with-data-lake-storage-gen1"></a>Toepassingen die compatibel met Data Lake Storage Gen1 zijn
Data Lake Storage Gen1 is compatibel met de meeste open source-componenten in het Hadoop-ecosysteem. Het kan ook goed worden geïntegreerd in andere Azure-services.  Volg de onderstaande koppelingen voor meer informatie over hoe Data Lake Storage Gen1 zowel met open source-componenten, evenals andere Azure-services kunnen worden gebruikt.

* Zie [Open source big data-toepassingen die met Azure Data Lake Storage Gen1 werken](../data-lake-store/data-lake-store-compatible-oss-other-applications.md) voor een lijst van open-sourcetoepassingen die compatibel zijn met Data Lake Storage Gen1.
* Zie [Azure Data Lake Storage Gen1 is integreren met andere Azure-services](../data-lake-store/data-lake-store-integrate-with-other-services.md) om te begrijpen hoe u Data Lake Storage Gen1 met andere Azure-services gebruikt om in te schakelen van een breder scala aan scenario's.
* Zie [met behulp van Azure op Data Lake Storage Gen1 voor big data-vereisten](../data-lake-store/data-lake-store-data-scenarios.md) voor meer informatie over het gebruik van Data Lake Storage Gen1 in scenario's zoals het ophalen van gegevens, gegevens verwerken, downloaden van gegevens en gegevens te visualiseren.

## <a name="data-lake-storage-gen1-file-system-adl"></a>Data Lake Storage Gen1-bestandssysteem (adl: / /)
U kunt in Hadoop-omgevingen (beschikbaar met een HDInsight-cluster), Data Lake Storage Gen1 openen via het nieuwe bestandssysteem, het AzureDataLakeFilesystem (adl: / /). De prestaties van toepassingen en services die gebruikmaken van adl: / / kan worden geoptimaliseerd op een manier die niet momenteel beschikbaar is in WebHDFS zijn. Als gevolg hiervan, wanneer u Data Lake Storage Gen1 gebruikt, u krijgt de flexibiliteit om de beschikbare de beste prestaties met behulp van de aanbevolen adl: / / of bestaande code door u verdergaat met het gebruik van de API WebHDFS rechtstreeks te beheren. Azure HDInsight maakt volledig gebruik van AzureDataLakeFilesystem voor de beste prestaties op Data Lake Storage Gen1.

Toegang tot uw gegevens in Data Lake Storage Gen1 met behulp van de volgende:

`adl://<data_lake_storage_gen1_name>.azuredatalakestore.net`

Zie voor meer informatie over hoe u toegang tot de gegevens in Data Lake Storage Gen1 [acties die beschikbaar zijn voor de opgeslagen gegevens](../data-lake-store/data-lake-store-get-started-portal.md#properties).



## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md)
* [Kennismaking met Azure Storage](../storage/common/storage-introduction.md)