---
title: Opties voor opslag voor gebruik met Azure HDInsight-clusters vergelijken
description: Biedt een overzicht van storage-typen en hoe ze werken met Azure HDInsight.
services: hdinsight,storage
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/20/2019
ms.openlocfilehash: 6174a8015290c3696548e1d9f575a46e4fe17b8a
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54479481"
---
# <a name="compare-storage-options-for-use-with-azure-hdinsight-clusters"></a>Opties voor opslag voor gebruik met Azure HDInsight-clusters vergelijken

Azure HDInsight-gebruikers kunnen kiezen uit een aantal opslagopties bij het maken van HDInsight-clusters:

* Azure Data Lake Storage Gen2
* Azure Storage
* Azure Data Lake Storage Gen1

Dit artikel bevat een overzicht van deze verschillende opslagtypen en hun unieke functies.

## <a name="use-azure-data-lake-storage-gen2-with-apache-hadoop-in-azure-hdinsight"></a>Azure Data Lake Storage Gen2 met Apache Hadoop in Azure HDInsight gebruiken

Zie voor meer informatie over Azure Data Lake Storage Gen2 [Inleiding tot Azure Data Lake Storage Gen2](/../storage/blobs/data-lake-storage-introduction.md).

Azure Data Lake Storage Gen2 duurt kernfuncties van Azure Data Lake Storage Gen1 zoals een compatibele Hadoop-bestandssysteem, Azure Active Directory en access POSIX gebaseerde toegangsbeheerlijsten (ACL's) en ze integreert in Azure Blob Storage. Deze combinatie kunt u profiteren van de prestaties van Azure Data Lake Storage Gen1 en tegelijkertijd opslaglagen voor Blob-opslag en beheer van de levenscyclus van gegevens.

### <a name="core-functionality-of-azure-data-lake-storage-gen2"></a>Kernfunctionaliteit van Azure Data Lake Storage Gen2

* Hadoop-compatibele toegang: Azure Data Lake Storage Gen2 kunt u beheren en toegang tot gegevens, net zoals u zou met een Hadoop Distributed File System (HDFS doen). Het stuurprogramma van de Azure Blob-bestandssysteem (ABFS) is beschikbaar in alle Apache Hadoop-omgevingen, met inbegrip van Azure HDInsight en Azure Databricks voor toegang tot gegevens die zijn opgeslagen in Data Lake Storage Gen2.

* Een hoofdverzameling van POSIX-machtigingen: Het beveiligingsmodel voor Data Lake Gen2 biedt ondersteuning voor ACL en POSIX-machtigingen, samen met enkele extra granulariteit die specifiek zijn voor Data Lake Storage Gen2. Instellingen kunnen worden geconfigureerd via de beheerhulpprogramma's of frameworks, zoals Apache Hive en Apache Spark.

* Goedkoop: Data Lake Storage Gen2 biedt de opslagcapaciteit van de lage kosten en transacties. Functies zoals Azure Blob storage-levenscyclus helpen kosten te verlagen door aan te passen factureringstarieven als gegevens worden verplaatst gedurende de levensduur.

* Als u werkt met Blob storage-hulpprogramma's, -frameworks en -apps: Data Lake Storage Gen2 blijft gewoon werken met een breed scala aan hulpprogramma's, frameworks en -toepassingen die vandaag beschikbaar zijn voor Blob-opslag.

* Geoptimaliseerde stuurprogramma: Het stuurprogramma ABFS is specifiek voor analyse van big data geoptimaliseerd. De bijbehorende REST-API's zijn opgehaald via het eindpunt van de DFS-dfs.core.windows.net.

### <a name="whats-new-about-azure-data-lake-storage-gen-2"></a>Wat is er nieuw in Azure Data Lake Storage Gen 2

#### <a name="managed-identities-for-secure-file-access"></a>Beheerde identiteiten voor toegang tot het beveiligde bestand

Azure HDInsight maakt gebruik van beheerde identiteiten voor het beveiligen van toegang tot het cluster tot bestanden in Azure Data Lake Storage Gen2. Beheerde identiteiten zijn een functie van Azure Active Directory die Azure-services een set referenties automatisch beheerd biedt. Deze referenties kunnen worden gebruikt om te verifiëren bij een service die ondersteuning biedt voor AD-verificatie. Met behulp van beheerde identiteiten, hoeft u het opslaan van referenties in code of configuratie bestanden.

Zie voor meer informatie, [wat is beheerde identiteiten voor Azure-resources](../active-directory/managed-identities-azure-resources/overview.md).

#### <a name="azure-blob-filesystem-abfs-driver"></a>Azure Blob-bestandssysteem (ABFS) stuurprogramma

Apache Hadoop-toepassingen verwachten systeemeigen te lezen en schrijven van gegevens uit de opslag van de lokale schijf. Een stuurprogramma met de Hadoop-bestandssysteem, zoals ABFS kunnen Hadoop-toepassingen werken met opslag in de cloud emuleert reguliere Hadoop file system-bewerkingen. Het stuurprogramma converteert deze opdrachten ontvangen van de toepassing in bewerkingen die het werkelijke cloudopslagplatform begrijpt.

Voorheen zou het Hadoop-bestandssysteem stuurprogramma alle bestandssysteembewerkingen converteren naar Azure Storage REST API-aanroepen aan de clientzijde en vervolgens de REST-API aanroepen. Deze client-side conversie, maar heeft geresulteerd in meerdere REST-API-aanroepen voor een enkele bestandssysteembewerking zoals Wijzig de naam van een bestand. ABFS is verplaatst enkele van de Hadoop-bestandssysteem logica van de client-side aan de serverzijde en de API van Azure Data Lake Storage Gen2 nu wordt uitgevoerd in combinatie met de Blob-API. Deze migratie verbetert de prestaties omdat nu algemene bewerkingen voor Hadoop-bestandssysteem kunnen worden uitgevoerd met een REST-API-aanroep.

Zie voor meer informatie, [stuurprogramma voor de Azure Blob-bestandssysteem (ABFS): Een speciale Azure Storage-stuurprogramma voor Hadoop](/../storage/blobs/data-lake-storage-abfs-driver.md).

#### <a name="azure-data-lake-storage-gen-2-uri-scheme"></a>Azure Data Lake Storage Gen 2 URI-schema

Azure Data Lake Storage Gen2 maakt gebruik van een nieuwe URI-schema om bestanden in Azure storage vanuit HDInsight te openen:

`abfs[s]://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/<PATH>`

Het URI-schema biedt versleuteld met SSL-toegang (`abfss://` voorvoegsel) en niet-versleutelde toegang (`abfs://` voorvoegsel). Wordt u aangeraden `abfss` waar mogelijk, zelfs bij het openen van gegevens die zich in dezelfde regio in Azure.

`<FILE_SYSTEM_NAME>` Hiermee geeft u het pad van het bestandssysteem Data Lake Storage Gen2.

`<ACCOUNT_NAME>` Geeft de naam van Azure Storage-account. Een FQDN (Fully Qualified Domain Name) is vereist.

De `<PATH>` is het bestand of map HDFS-padnaam.

Als de waarden voor `<FILE_SYSTEM_NAME>` en `<ACCOUNT_NAME>` zijn niet opgegeven, wordt het standaardbestandssysteem gebruikt. Voor de bestand op het standaardbestandssysteem kunt u een relatief of een absoluut pad gebruiken. Bijvoorbeeld, de `hadoop-mapreduce-examples.jar` -bestand dat wordt geleverd met HDInsight-clusters kan worden aangeduid met behulp van een van de volgende paden:

```
abfss://myfilesystempath@myaccount.dfs.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
abfss:///example/jars/hadoop-mapreduce-examples.jar /example/jars/hadoop-mapreduce-examples.jar
```

> [!Note]
> De bestandsnaam is `hadoop-examples.jar` in HDInsight versie 2.1- en 1.6-clusters. Als u werkt met bestanden buiten HDInsight, hulpprogramma's voor de meeste niet wordt herkend op de ABFS formatteren en in plaats daarvan een standaardpadindeling, zoals te verwachten `example/jars/hadoop-mapreduce-examples.jar`.

Zie voor meer informatie, [gebruiken de Azure Data Lake Storage Gen2 URI](/../storage/blobs/data-lake-storage-introduction-abfs-uri.md).

## <a name="use-azure-storage"></a>Azure Storage gebruiken

Azure Storage is een robuuste, algemene opslagoplossing die naadloos kan worden geïntegreerd met HDInsight. HDInsight kan een blobcontainer in Azure Storage gebruiken als het standaardbestandssysteem voor het cluster. Via een HDFS-interface (Hadoop Distributed File System) kan de volledige set onderdelen in HDInsight rechtstreeks als blobs op gestructureerde of ongestructureerde gegevens worden uitgevoerd.

> [!WARNING]  
> Er zijn verschillende opties beschikbaar bij het maken van een Azure Storage-account. De volgende tabel bevat informatie over de opties die met HDInsight worden ondersteund:

| Type opslagaccount | Ondersteunde services | Ondersteunde prestatielagen | Ondersteunde toegangslagen |
|----------------------|--------------------|-----------------------------|------------------------|
| Algemeen gebruik V2   | Blob               | Standard                    | Hot, Cool archief *    |
| Algemeen gebruik V1   | Blob               | Standard                    | N/A                    |
| Blob Storage         | Blob               | Standard                    | Hot, Cool archief *    |

Wordt niet aanbevolen dat u de standaard blob-container gebruiken voor het opslaan van bedrijfsgegevens. Het is een goede gewoonte om de standaard-blobcontainer na ieder gebruik te verwijderen om de opslagkosten te verlagen. De standaardcontainer bevat toepassings- en Logboeken. Breng de logboeken over naar een andere locatie voordat u de container verwijdert.

Met behulp van een blob-container als het standaardbestandssysteem voor meerdere clusters wordt niet ondersteund.
 
 > [!NOTE]  
 > De Archive Storage-toegangslaag is een offline-laag die een enkele uren latentie bij het ophalen en wordt niet aanbevolen voor gebruik met HDInsight. Zie voor meer informatie, [Archive Storage-toegangslaag](/../storage/blobs/storage-blob-storage-tiers.md#archive-access-tier).

### <a name="hdinsight-storage-architecture"></a>HDInsight-opslagarchitectuur
Het volgende diagram biedt een abstracte weergave van de HDInsight-opslagarchitectuur bij gebruik van Azure Storage:

![Hadoop-clusters gebruiken de HDFS-API voor toegang tot en opslag van gestructureerde en ongestructureerde gegevens in Blob Storage.](./media/hdinsight-hadoop-compare-storage-options/HDI.WASB.Arch.png "HDInsight Storage-architectuur")

HDInsight biedt toegang tot het Distributed File System dat lokaal wordt gekoppeld aan de rekenknooppunten. Dit bestandssysteem is toegankelijk via de volledig gekwalificeerde URI, bijvoorbeeld:

    hdfs://<namenodehost>/<path>

Daarnaast biedt HDInsight toegang tot gegevens die zijn opgeslagen in Azure Storage. De syntaxis is:

    wasb[s]://<containername>@<accountname>.blob.core.windows.net/<path>

Hier volgen enkele overwegingen bij het gebruik van een Azure Storage-account met HDInsight-clusters.

* **Containers in de storage-accounts die zijn verbonden met een cluster:** Omdat de accountnaam en de sleutel gekoppeld aan het cluster tijdens het maken zijn, hebt u volledige toegang tot de blobs in deze containers.

* **Openbare containers of openbare blobs in opslagaccounts die niet zijn verbonden met een cluster:** U hebt alleen-lezen toegang tot de blobs in de containers.
  
  > [!NOTE]  
  > Met openbare containers kunt u een lijst met alle beschikbare blobs in de desbetreffende container en containermetagegevens ophalen. Openbare blobs zijn alleen toegankelijk als u de exacte URL weet. Zie voor meer informatie, [toegang tot containers en blobs beheren](/../storage/blobs/storage-manage-access-to-resources.md).

* **Persoonlijke containers in opslagaccounts die niet zijn verbonden met een cluster:** U kunt de blobs in de containers geen toegang tot, tenzij u het opslagaccount definieert wanneer u de WebHCat-taken verzendt. Dit wordt verderop in dit artikel uitgelegd.

De opslagaccounts die worden gedefinieerd tijdens het creatieproces en de bijbehorende sleutels worden opgeslagen in %HADOOP_HOME%/conf/core-site.xml op de clusterknooppunten. HDInsight gebruikt standaard de opslagaccounts die zijn gedefinieerd in het bestand core-site.xml. U kunt deze instelling wijzigen met behulp van [Apache Ambari](./hdinsight-hadoop-manage-ambari.md).

Meerdere WebHCat-taken, waaronder Apache Hive, MapReduce, Apache Hadoop-streaming en Apache Pig, kunnen een beschrijving van opslagaccounts en metagegevens mee uitvoeren. (Dit werkt momenteel voor Pig met opslagaccounts, maar niet voor metagegevens.) Zie [Using an HDInsight Cluster with Alternate Storage Accounts and Metastores](https://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx) (Een HDInsight-cluster gebruiken met alternatieve opslagaccounts en metastores) voor meer informatie.

Blobs kunnen worden gebruikt voor gestructureerde en ongestructureerde gegevens. De gegevens in blobcontainers worden opgeslagen als sleutel-waardeparen en er is geen maphiërarchie. Maar het slash-teken (/) kan echter worden gebruikt binnen de sleutelnaam deze weergegeven, zodat het lijkt alsof een bestand is opgeslagen in een mapstructuur. Bijvoorbeeld, de sleutel van een blob mogelijk `input/log1.txt`. Er is niet echt `input` aanwezig, maar als gevolg van de aanwezigheid van het slash-teken in naam van de sleutel, heeft het uiterlijk van een bestandspad.

### <a id="benefits"></a>Voordelen van Azure Storage
De kosten als gevolg van de verschillende locaties voor de rekenclusters en opslagresources worden beperkt door de manier waarop de rekenclusters dicht bij de resources van het opslagaccount in de Azure-regio worden geplaatst. Dankzij het netwerk met hoge snelheid kunnen de rekenknooppunten zich op efficiënte wijze toegang verschaffen tot de gegevens in Azure Storage.

Het opslaan van gegevens in Azure Storage in plaats van HDFS heeft enkele voordelen:

* **Hergebruik van de gegevens en het delen van:** De gegevens in HDFS bevindt zich in het rekencluster. Alleen de toepassingen die toegang tot het rekencluster hebben, kunnen de gegevens met HDFS API's gebruiken. De gegevens in Azure storage zijn toegankelijk via de HDFS API's of via de Blob Storage REST-API's. Zo kan een grotere set toepassingen (inclusief andere HDInsight-clusters) en hulpprogramma's worden gebruikt om de gegevens te produceren en te gebruiken.
* **Archivering van gegevens:** Opslaan van gegevens in Azure storage, kunt de HDInsight-clusters gebruikt voor berekeningen, veilig worden verwijderd zonder verlies van gebruikersgegevens.
* **Kosten voor gegevensopslag:** Opslaan van gegevens voor de lange termijn in DFS is duurder dan het opslaan van gegevens in Azure storage, omdat de kosten van een rekencluster hoger zijn dan de kosten van Azure storage. Omdat de gegevens niet kan worden geladen voor elk rekencluster dat wordt gegenereerd, bent u bovendien ook kosten voor het laden van gegevens opslaan.
* **Elastisch uitbreiden:** Hoewel HDFS u een scale-out-bestandssysteem biedt, wordt de schaal bepaald door het aantal knooppunten die u voor uw cluster maakt. Het wijzigen van de schaal is mogelijk een complexer proces dan. Vertrouw hiervoor niet zonder meer op de elastische schalingsmogelijkheden waarover u automatisch beschikt in Azure Storage.
* **Geo-replicatie:** Uw Azure-opslag kan geografisch worden gerepliceerd. Hoewel u dit de mogelijkheid van geografisch herstel en gegevensredundantie biedt, zal een failover naar de geografisch gerepliceerde locatie van grote invloed zijn op uw prestaties en kan dit tot extra kosten leiden. U wordt daarom aangeraden de keuze van geo-replicatie goed te overwegen en alleen te gebruiken als de waarde van de gegevens de extra kosten waard zijn.

Bepaalde MapReduce-taken en -pakketten kunnen tussenliggende resultaten genereren die u niet wilt opslaan in Azure Storage. In dat geval kunt u ervoor kiezen de gegevens op te slaan in de lokale HDFS. HDInsight gebruikt DFS voor verschillende tussenliggende resultaten in Hive-taken en andere processen.

> [!NOTE]  
> De meeste HDFS-opdrachten (bijvoorbeeld `ls`, `copyFromLocal` en `mkdir`) nog steeds werken zoals verwacht. Alleen de opdrachten die specifiek voor de systeemeigen HDFS-implementatie zijn (die wordt aangeduid als DFS), zoals `fschk` en `dfsadmin`, verschillend gedrag weergeven in Azure storage.

## <a name="use-azure-data-lake-storage-gen1"></a>Azure Data Lake Storage Gen1 gebruiken

Zie voor meer informatie over Azure Data Lake Storage Gen1 [overzicht van Azure Data Lake Storage Gen1](/../data-lake-store/data-lake-store-overview.md).

Azure Data Lake Storage Gen1 is een bedrijfsbrede zeer grote opslagruimte voor big data-analysewerklasten. Met Azure Data Lake kunt u gegevens van elke grootte, type en opnamesnelheid vastleggen op één enkele locatie voor operationele en experimentele analyses.

Data Lake Storage Gen1 kan worden geopend via Hadoop (beschikbaar met een HDInsight-cluster) met de WebHDFS compatibele REST-API. Het is ontworpen om te kunnen analyseren voor de opgeslagen gegevens en is afgestemd op prestaties voor scenario's met gegevensanalyses. Het bevat out-of-the-box alle mogelijkheden op bedrijfsniveau: beveiliging, beheerbaarheid, schaalbaarheid, betrouwbaarheid en beschikbaarheid. Dit zijn allemaal essentiële factoren voor zakelijke gebruiksvoorbeelden.

![Azure Data Lake Storage](./media/hdinsight-hadoop-compare-storage-options/data-lake-store-concept.png "HDInsight-opslagarchitectuur")

Enkele van de belangrijkste mogelijkheden van Data Lake Storage Gen1 zijn de volgende.

### <a name="built-for-hadoop"></a>Gebouwd voor Hadoop

Data Lake Storage Gen1 is een Apache Hadoop-bestandssysteem dat compatibel is met Hadoop Distributed File System (HDFS) en werkt met het Hadoop-ecosysteem.  Uw bestaande HDInsight-toepassingen of services die gebruikmaken van de API WebHDFS kunnen eenvoudig integreren met Data Lake Storage Gen1. Data Lake Storage Gen1 wordt ook aangegeven dat een WebHDFS compatibele REST-interface voor toepassingen

Gegevens die zijn opgeslagen in Data Lake Storage Gen1 kunnen eenvoudig worden geanalyseerd met analytische frameworks van Hadoop zoals MapReduce of Hive. Microsoft Azure HDInsight-clusters kunnen worden ingericht en geconfigureerd voor rechtstreekse toegang tot gegevens die zijn opgeslagen in Data Lake Storage Gen1.

### <a name="unlimited-storage-petabyte-files"></a>Onbeperkte opslag, bestanden ter grootte van petabytes

Data Lake Storage Gen1 biedt onbeperkte opslag en is geschikt voor het opslaan van een verscheidenheid aan gegevens voor analyse. Het legt geen limieten op voor de grootte van accounts of bestanden, of de hoeveelheid gegevens die kunnen worden opgeslagen in een Data Lake. De grootte van afzonderlijke bestanden kan uiteenlopen van een kilobyte tot petabytes, waardoor het een uitstekende keuze is voor het opslaan van elk type gegevens. Gegevens worden blijvend opgeslagen door meerdere kopieën te maken en er is geen limiet voor de tijdsduur waarin de gegevens kunnen worden opgeslagen in de Data Lake.

### <a name="performance-tuned-for-big-data-analytics"></a>Prestaties zijn afgestemd op big data-analyses

Data Lake Storage Gen1 is gebouwd voor het uitvoeren van grootschalige analytische systemen waarvoor grote doorvoer doorzoeken en analyseren van grote hoeveelheden gegevens vereist. De Data Lake verspreidt delen van een bestand over een aantal afzonderlijke opslagservers. Hiermee verbetert u de doorvoer wanneer het bestand in parallel wordt gelezen voor het uitvoeren van gegevensanalyse.

### <a name="enterprise-ready-highly-available-and-secure"></a>Enterprise-ready: Maximaal beschikbare en veilige

Data Lake Storage Gen1 biedt industriestandaard beschikbaarheid en betrouwbaarheid. Uw gegevensassets worden blijvend opgeslagen door het maken van redundante exemplaren ter bescherming tegen onverwachte fouten. Ondernemingen kunnen Data Lake Storage Gen1 in hun oplossingen gebruiken als een belangrijk onderdeel van hun bestaande gegevensplatform.

Data Lake Storage Gen1 biedt ook geavanceerde beveiliging voor de opgeslagen gegevens. Zie voor meer informatie, [gegevens beveiligen in Azure Data Lake Storage Gen1](#DataLakeStoreSecurity).

### <a name="all-data"></a>Alle gegevens

Data Lake Storage Gen1 kunnen alle gegevens worden opgeslagen in de oorspronkelijke indeling, zoals ze gemaakt zijn, zonder eerst om te zetten. Data Lake Storage Gen1 is niet een schema worden gedefinieerd voordat de gegevens zijn geladen, vereist daardoor kan het afzonderlijke analytische framework de gegevens interpreteren en een schema definiëren op het moment van de analyse. De mogelijkheid voor het opslaan van bestanden van verschillende groottes en indelingen, maakt het mogelijk voor Data Lake Storage Gen1 verwerking van gestructureerde, semi-gestructureerde en ongestructureerde gegevens.

Data Lake Storage Gen1 containers voor gegevens zijn eigenlijk mappen en bestanden. U werkt met de opgeslagen gegevens met behulp van SDK's, Azure-portal en Azure Powershell. Als u uw gegevens in het archief plaats via deze interfaces en de juiste containers gebruikt, kunt u allerlei soorten gegevens opslaan. Data Lake Storage Gen1 voert geen speciale verwerking van gegevens op basis van het type gegevens dat wordt opgeslagen.

## <a name="DataLakeStoreSecurity"></a>Het beveiligen van gegevens in Data Lake Storage Gen1
Data Lake Storage Gen1 gebruikt Azure Active Directory voor verificatie en toegangsbeheerlijsten (ACL's) voor het beheren van toegang tot uw gegevens.

| Functie | Description |
| --- | --- |
| Verificatie |Data Lake Storage Gen1 kan worden geïntegreerd met Azure Active Directory (AAD) voor identiteits- en toegangsbeheer voor alle gegevens die zijn opgeslagen in Data Lake Storage Gen1. Als gevolg van de integratie, Data Lake Storage Gen1 voordelen van alle AAD-functies zoals multi-factor authentication, voorwaardelijke toegang, op rollen gebaseerd toegangsbeheer, bewaking van toepassingsgebruik, beveiligingsbewaking en waarschuwingen, enzovoort. Data Lake Storage Gen1 ondersteunt het OAuth 2.0-protocol voor verificatie in de REST-interface. Zie [Data Lake Storage Gen1 verificatie](/../data-lake-store/data-lakes-store-authentication-using-azure-active-directory.md)|
| Toegangsbeheer |Data Lake Storage Gen1 biedt toegangsbeheer door ondersteuning POSIX-machtigingen die worden weergegeven door het protocol WebHDFS. ACL's kunnen worden ingeschakeld voor de hoofdmap, submappen en afzonderlijke bestanden. Zie voor meer informatie over de werking van ACL's in de context van Data Lake Storage Gen1 [toegangsbeheer in Data Lake Storage Gen1](/../data-lake-store/data-lake-store-access-control.md). |
| Versleuteling |Data Lake Storage Gen1 biedt ook versleuteling voor gegevens die zijn opgeslagen in het account. U geeft de versleutelingsinstellingen op tijdens het maken van een Data Lake Storage Gen1-account. U kunt ervoor kiezen de gegevens te versleutelen of niet te versleutelen. Zie voor meer informatie, [versleuteling in Data Lake Storage Gen1](/../data-lake-store/data-lake-store-encryption.md). Zie voor instructies over het bieden van versleuteling-gerelateerde configuratie [aan de slag met Azure Data Lake Storage Gen1 met behulp van de Azure-portal](/../data-lake-store/data-lake-store-get-started-portal.md). |

Wilt u meer informatie over het beveiligen van gegevens in Data Lake Storage Gen1? Volg dan de onderstaande links.

* Zie voor instructies over het beveiligen van gegevens in Data Lake Storage Gen1 [gegevens beveiligen in Azure Data Lake Storage Gen1](/../data-lake-store/data-lake-store-secure-data.md).

## <a name="applications-compatible-with-data-lake-storage-gen1"></a>Toepassingen die compatibel zijn met Data Lake Storage Gen1
Data Lake Storage Gen1 is compatibel met de meeste open source-componenten in het Hadoop-ecosysteem. Het kan ook goed worden geïntegreerd in andere Azure-services.  Volg de onderstaande koppelingen voor meer informatie over hoe Data Lake Storage Gen1 zowel met open source-componenten, evenals andere Azure-services kunnen worden gebruikt.

* Zie [toepassingen en services die compatibel zijn met Azure Data Lake Storage Gen1](/../data-lake-store/data-lake-store-compatible-oss-other-applications.md) voor een lijst van open-sourcetoepassingen die compatibel met Data Lake Storage Gen1.
* Zie [integreren met andere Azure-services](/../data-lake-store/data-lake-store-integrate-with-other-services.md) om te begrijpen hoe Data Lake Storage Gen1 met andere Azure-services kan worden gebruikt om in te schakelen van een breder scala aan scenario's.
* Zie [scenario's voor het gebruik van Data Lake Storage Gen1](/../data-lake-store/data-lake-store-data-scenarios.md) voor meer informatie over het gebruik van Data Lake Storage Gen1 in scenario's zoals het ophalen van gegevens, gegevens verwerken, downloaden van gegevens en gegevens te visualiseren.

## <a name="what-is-data-lake-storage-gen1-file-system-adl"></a>Wat is Data Lake Storage Gen1-bestandssysteem (adl: / /)?
Data Lake Storage Gen1 kunnen worden geopend via het nieuwe bestandssysteem, het AzureDataLakeFilesystem (adl: / /), in Hadoop-omgevingen (beschikbaar met HDInsight-cluster). Toepassingen en services die gebruikmaken van adl:// kunnen profiteren van verdere optimalisatie van prestaties die op dit moment niet beschikbaar is in WebHDFS. Als gevolg hiervan, Data Lake Storage Gen1 biedt u de flexibiliteit om te behalen de beste prestaties met de aanbevolen optie adl: / / of bestaande code door u verdergaat met het gebruik van de API WebHDFS rechtstreeks te beheren. Azure HDInsight maakt volledig gebruik van AzureDataLakeFilesystem om de beste prestaties bieden op Data Lake Storage Gen1.

U kunt toegang tot uw gegevens in met behulp van Data Lake Storage Gen1 `adl://<data_lake_storage_gen1_name>.azuredatalakestore.net`. Zie voor meer informatie over hoe u toegang tot de gegevens in Data Lake Storage Gen1 [eigenschappen van de opgeslagen gegevens weergeven](/../data-lake-store/data-lake-store-get-started-portal.md#properties)



## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Azure Data Lake Storage Gen2](/../storage/blobs/data-lake-storage-introduction.md).
* [Kennismaking met Azure Storage](/../storage/common/storage-introduction.md)