---
title: On-premises Apache Hadoop-clusters migreren naar Azure HDInsight - opslag aanbevolen procedures
description: Informatie over aanbevolen procedures van de opslagruimte voor migratie on-premises Hadoop-clusters op Azure HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: ashishth
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: hrasheed
ms.openlocfilehash: 4f4aedd1d85a83e6f55d5729b82b88e2e9e8c00d
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50415930"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---storage-best-practices"></a>On-premises Apache Hadoop-clusters migreren naar Azure HDInsight - opslag aanbevolen procedures

In dit artikel biedt aanbevelingen voor de opslag van gegevens in Azure HDInsight-systemen. Het onderdeel van een serie die biedt best practices om u te helpen migreren on-premises Apache Hadoop-systemen tot Azure HDInsight.

## <a name="choose-the-right-storage-system-for-hdinsight-clusters"></a>Kies het juiste opslagsysteem voor HDInsight-clusters

De mapstructuur van de on-premises Apache Hadoop File System (HDFS) kan opnieuw worden gemaakt in Azure Storage of Azure Data Lake-opslag. U kunt het HDInsight-clusters die worden gebruikt voor berekeningen zonder verlies van gebruikersgegevens vervolgens veilig verwijderen. Beide services kunnen worden gebruikt als zowel een extra bestandssysteem als het standaardbestandssysteem voor een HDInsight-cluster. Het HDInsight-cluster en het opslagaccount dat moeten worden gehost in dezelfde regio.

### <a name="azure-storage"></a>Azure Storage

HDInsight-clusters kunnen de blob-container in Azure Storage gebruiken als het standaardbestandssysteem of een extra bestandssysteem. De prijscategorie Standard storage-account wordt ondersteund voor gebruik met HDInsight-clusters. De Premier laag wordt niet ondersteund. In de standaard-blobcontainer worden clusterspecifieke gegevens opgeslagen, zoals taakgeschiedenis en logboekbestanden. Het delen van een blobcontainer als het standaardbestandssysteem voor meerdere clusters wordt niet ondersteund.

De storage-accounts die zijn gedefinieerd in het proces voor het maken en de bijbehorende sleutels worden opgeslagen in `%HADOOP_HOME%/conf/core-site.xml` op de clusterknooppunten. Ze kunnen ook toegankelijk in de sectie 'Aangepaste core site' in HDFS-configuratie in de Ambari-UI. De opslagaccountsleutel standaard is versleuteld en een aangepaste ontsleuteling-script wordt gebruikt voor het ontsleutelen van de sleutels voordat het wordt doorgegeven aan de Hadoop-daemons. De taken, zoals Hive, MapReduce, Hadoop-streaming en Pig, voert een beschrijving van opslagaccounts en metagegevens bevatten.

Azure-opslag kan geografisch worden gerepliceerd. Hoewel geo-replicatie geografisch herstel en gegevensredundantie biedt, een failover naar de geografisch gerepliceerde locatie grote invloed zijn op de prestaties en het mogelijk extra kosten in rekening. De aanbeveling is het kiezen van de geo-replicatie goed te overwegen en alleen als de waarde van de gegevens de extra kosten waard.

Een van de volgende indelingen kan worden gebruikt voor toegang tot gegevens die zijn opgeslagen in Azure Storage:

- `wasb:///`: Toegang standaardopslag met behulp van niet-versleutelde communicatie.
- `wasbs:///`: Toegang standaardopslag met behulp van versleutelde communicatie.
- `wasb://<container-name>@<account-name>.blob.core.windows.net/`: Wordt gebruikt bij het communiceren met een niet-standaard-storage-account. 

[Azure Storage Scalability and Performance Targets](../../storage/common/storage-scalability-targets.md) bevat de huidige limieten van Azure storage-accounts. Als de behoeften van de toepassing groter is dan de schaalbaarheidsdoelen van een enkel opslagaccount, kan de toepassing worden gemaakt voor het gebruik van meerdere opslagaccounts en vervolgens partitie-gegevensobjecten voor deze opslagaccounts.

[Azure Storage Analytics](../../storage/storage-analytics.md) biedt metrische gegevens voor alle storage-services en Azure portal kunnen geconfigureerde verzamelen metrische gegevens moeten worden gevisualiseerd via grafieken. Waarschuwingen kunnen worden gemaakt om u te waarschuwen wanneer drempels voor metrische gegevens van storage resource is bereikt.

Azure Storage biedt [voorlopig verwijderen voor blob-objecten](../../storage/blobs/storage-blob-soft-delete.md) voor het herstellen van gegevens wanneer deze per ongeluk wordt gewijzigd of verwijderd door een toepassing of de gebruiker van andere storage-account.

U kunt maken [blob-momentopnamen](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob). Een momentopname is een alleen-lezen versie van een blob die moet worden uitgevoerd op een punt in tijd en biedt een manier om back-up van een blob. Nadat een momentopname is gemaakt, kan deze worden gelezen, gekopieerd, of verwijderd, maar niet gewijzigd.

> [!Note]
> Voor oudere versie van on-premises-on-premises Hadoop-distributies die beschikt niet over de 'wasbs'-certificaat moet worden geïmporteerd in het archief van de vertrouwensrelatie van Java.

De volgende methoden kunnen worden gebruikt voor het importeren van certificaten in het archief van de vertrouwensrelatie Java:

Het ssl-certificaat van het Azure-Blob naar een bestand downloaden

```bash
echo -n | openssl s_client -connect <storage-account>.blob.core.windows.net:443 | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > Azure_Storage.cer
```

Het bovenstaande bestand importeren in de Java-vertrouwensarchief op alle knooppunten

```bash
keytool -import -trustcacerts -keystore /path/to/jre/lib/security/cacerts -storepass changeit -noprompt -alias blobtrust -file Azure_Storage.cer
```

Controleer of het certificaat toegevoegd in het vertrouwensarchief met

```bash
keytool -list -v -keystore /path/to/jre/lib/security/cacerts
```

Raadpleeg voor meer informatie de volgende artikelen:

- [Azure storage gebruiken met Azure HDInsight-clusters](../hdinsight-hadoop-use-blob-storage.md)
- [Schaalbaarheids- en Prestatiedoelen voor Azure Storage](../../storage/common/storage-scalability-targets.md)
- [Controlelijst voor prestaties en schaalbaarheid van Microsoft Azure Storage](../../storage/common/storage-performance-checklist.md)
- [Monitor, diagnose, and troubleshoot Microsoft Azure Storage](../../storage/common/storage-monitoring-diagnosing-troubleshooting.md) (Bewaken, diagnosticeren en problemen oplossen in Microsoft Azure Storage)
- [Een opslagaccount in Azure portal controleren](../../storage/common/storage-monitor-storage-account.md)

### <a name="azure-data-lake-storage-gen1"></a>Azure Data Lake Storage Gen1

Azure Data Lake Storage implementeert HDFS en POSIX stijl model voor toegangsbeheer. Het biedt eerste-klas integratie met AAD voor goed korrelig toegangsbeheer. Er zijn geen beperkingen aan de grootte van gegevens die kunnen worden opgeslagen of de mogelijkheid voor het uitvoeren van enorme parallelle analyses.

Raadpleeg voor meer informatie de volgende artikelen:

- [Maken van HDInsight-clusters met Data Lake Storage met behulp van Azure portal](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)
- [Data Lake Storage gebruiken met Azure HDInsight-clusters](../hdinsight-hadoop-use-data-lake-store.md)

### <a name="azure-data-lake-storage-gen2-preview"></a>Azure Data Lake Storage Gen2 (preview)

Azure Data Lake Storage Gen2 is de meest recente opslag biedt en is in Preview-versie op het moment van schrijven van dit document. Het verenigt de belangrijkste mogelijkheden van de eerste generatie van Azure Data Lake-opslag met een Hadoop-compatibel bestand system eindpunt, rechtstreeks geïntegreerd in Azure Blob Storage. Deze uitbreiding combineert de voordelen van het schalen en kosten van opslag van objecten met de betrouwbaarheid en prestaties meestal alleen met on-premises bestandssystemen die zijn gekoppeld.

ADLS Gen 2 is gebouwd boven [Azure Blob-opslag](../../storage/blobs/storage-blobs-introduction.md) en kunt u samenwerken met gegevens met behulp van beide beschermingsparadigma in het systeem en de object-opslag. Functies van [Azure Data Lake Storage Gen1](../../data-lake-store/index.md), zoals de semantiek van het bestandssysteem, op bestandsniveau beveiliging en schaal worden gecombineerd met lage kosten, gelaagde opslag, mogelijkheden voor hoge beschikbaarheid/noodherstel herstel en een grote SDK/hulpprogramma's -ecosysteem van [Azure Blob-opslag](../../storage/blobs/storage-blobs-introduction.md). In Data Lake Storage Gen2 blijven alle kenmerken van de opslag van objecten tijdens het toevoegen van de voordelen van een interface file system geoptimaliseerd voor analytics werkbelastingen.

Een fundamenteel onderdeel van de Data Lake Storage Gen2 is de toevoeging van een [hiërarchische naamruimte](../../storage/data-lake-storage/namespace.md) naar de Blob storage-service organiseert die objecten/bestanden in een hiërarchie van mappen voor toegang tot gegevens voor prestaties. De hiërarchische structuur kan bewerkingen, zoals naam wijzigen of verwijderen van een directory om te worden van één atomic metagegevens worden uitgevoerd op de map in plaats van inventariseren en verwerken van alle objecten die delen van het voorvoegsel van de naam van de map.

In het verleden had cloudanalyses te boeten op het gebied van prestaties, beheer en beveiliging. De sleutel-functies van Azure Data Lake Storage (ADLS) Gen2 zijn als volgt:

- **Hadoop-compatibele toegang**: Azure Data Lake Storage Gen2 kunt u beheren en toegang tot gegevens, net zoals u zou met doen een [Hadoop Distributed File System (HDFS)](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html). De nieuwe [ABFS stuurprogramma](../../storage/data-lake-storage/abfs-driver.md) is beschikbaar in alle Apache Hadoop-omgevingen die zijn opgenomen in [Azure HDInsight](../index.yml). Dit stuurprogramma kunt u toegang tot gegevens die zijn opgeslagen in Data Lake Storage Gen2.

- **Een hoofdverzameling van POSIX-machtigingen**: het beveiligingsmodel voor Data Lake Gen2 biedt volledige ondersteuning voor ACL en POSIX-machtigingen, samen met enkele extra granulariteit die specifiek zijn voor Data Lake Storage Gen2. Instellingen kunnen worden geconfigureerd via de beheerhulpprogramma's of frameworks, zoals Hive- en Spark.

- **Voordelige**: Data Lake Storage Gen2 is uitgerust met lage kosten voor de opslagcapaciteit en transacties. Als de overgang van de gegevens via de volledige levenscyclus, factureringstarieven wijzigen om te minimaliseren kosten via de ingebouwde functies zoals [Azure Blob storage-levenscyclus](../../storage/common/storage-lifecycle-managment-concepts.md).

- **Werkt met Blob storage-hulpprogramma's, frameworks en -apps**: Data Lake Storage Gen2 blijft gewoon werken met een breed scala aan hulpprogramma's, frameworks en -toepassingen die vandaag beschikbaar zijn voor Blob-opslag.

- **Geoptimaliseerde stuurprogramma**: het stuurprogramma van de Azure Blob-bestandssysteem (ABFS) is [speciaal geoptimaliseerd](../../storage/data-lake-storage/abfs-driver.md) voor analyse van big data. De bijbehorende REST-API's zijn opgehaald via het eindpunt van de DFS-dfs.core.windows.net.

Een van de volgende indelingen kan worden gebruikt voor toegang tot gegevens die zijn opgeslagen in ADLS Gen2:
- `abfs:///`: Toegang tot de standaard Data Lake Storage voor het cluster.
- `abfs[s]://file_system@account_name.dfs.core.windows.net`: Wordt gebruikt bij het communiceren met een niet-standaard Data Lake-opslag.

Raadpleeg voor meer informatie de volgende artikelen:

- [Inleiding tot Azure Data Lake Storage Gen2 Preview](../../storage/data-lake-storage/introduction.md)
- [Het bestandssysteem van Azure Blob-stuurprogramma (ABFS.md)](../../storage/data-lake-storage/abfs-driver.md)

## <a name="protect-azure-storage-key-visibility-within-the-on-premises-hadoop-cluster-configuration"></a>Azure Storage key zichtbaarheid in de configuratie van on-premises Hadoop-cluster beveiligen

De Azure storage-sleutels die zijn toegevoegd aan de Hadoop-configuratiebestanden, stel de verbinding in tussen op de lokale HDFS- en Azure Blob-opslag. Deze sleutels kunnen worden beveiligd door ze te versleutelen met het referentieproviderframework van Hadoop. Als versleuteld, kunnen worden opgeslagen en de toegang tot.

**Voor het inrichten van de referenties:**

```bash
hadoop credential create fs.azure.account.key.account.blob.core.windows.net -value <storage key> -provider jceks://hdfs@headnode.xx.internal.cloudapp.net/path/to/jceks/file
```

**De bovenstaande providerpad toevoegen aan de core-site.xml of aan de configuratie van de Ambari onder aangepaste core-site:**

```xml
<property>
    <name>hadoop.security.credential.provider.path</name>
        <value>
        jceks://hdfs@headnode.xx.internal.cloudapp.net/path/to/jceks
        </value>
    <description>Path to interrogate for protected credentials.</description>
</property>
```

> [!Note]
> De eigenschap provider pad kan ook worden toegevoegd aan de opdrachtregel van de opdracht distcp in plaats van sleutel op niveau van het cluster op core-site.xml als volgt:

```bash
hadoop distcp -D hadoop.security.credential.provider.path=jceks://hdfs@headnode.xx.internal.cloudapp.net/path/to/jceks /user/user1/ wasb:<//yourcontainer@youraccount.blob.core.windows.net/>user1
```

## <a name="restrict-access-to-azure-storage-data-using-sas-signatures"></a>Toegang tot Azure storage-gegevens met behulp van SAS-handtekeningen beperken

HDInsight standaard heeft volledige toegang tot gegevens in de Azure Storage-accounts die zijn gekoppeld aan het cluster. Gedeelde Access Signatures (SAS) op de blob-container kan worden gebruikt om te beperken van toegang tot de gegevens, zoals gebruikers voorzien van alleen-lezen toegang tot de gegevens.

### <a name="using-the-sas-token-created-with-python"></a>Met behulp van de SAS-token dat is gemaakt met python

1. Open de [SASToken.py](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature/blob/master/Python/SASToken.py) bestands- en wijzigt u de volgende waarden:
    - Naam_van_beleid: de naam moet worden gebruikt voor het opgeslagen beleid te maken.
    - storage_account_name: de naam van uw storage-account.
    - storage_account_key: de sleutel voor het opslagaccount.
    - storage_container_name: de container in het opslagaccount dat u wilt toegang te beperken.
    - example_file_path: het pad naar een bestand dat is geüpload naar de container

2. Het bestand SASToken.py wordt geleverd met de `ContainerPermissions.READ + ContainerPermissions.LIST` machtigingen en kan worden aangepast op basis van de use-case.

3. Voer het script als volgt uit: `python SASToken.py`

4. Wanneer het script is voltooid, wordt de SAS-token die vergelijkbaar is met de volgende tekst weergegeven: `sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14`

5. Als u wilt beperken van toegang tot een container met Shared Access Signature, moet u een aangepaste vermelding toevoegen aan de core-site-configuratie voor het cluster onder Ambari HDFS configuraties geavanceerde aangepaste eigenschap van de core-site toevoegen.

6. Gebruik de volgende waarden voor de **sleutel** en **waarde** velden:

    **Sleutel**: `fs.azure.sas.YOURCONTAINER.YOURACCOUNT.blob.core.windows.net` **waarde**: de SAS-sleutel die wordt geretourneerd door de Python-toepassing vanaf stap 4 hierboven.

7. Klik op de **toevoegen** klikken om deze sleutel en waarde opslaan en klik vervolgens op de **opslaan** om op te slaan van de configuratiewijzigingen. Wanneer u hierom wordt gevraagd, een beschrijving van de wijziging ('toe te voegen toegang tot de SAS-opslag' bijvoorbeeld) toevoegen en klik vervolgens op **opslaan**.

8. In de Ambari-webgebruikersinterface, HDFS Selecteer in de lijst aan de linkerkant en selecteer vervolgens **start opnieuw op alle betrokken** Vervolgkeuzelijst lijst aan de rechterkant van de Service-acties. Wanneer u hierom wordt gevraagd, selecteert u **alle opnieuw starten bevestigen**.

9. Herhaal dit proces voor MapReduce2 en YARN.

Er zijn drie belangrijke dingen om te weten over het gebruik van SAS-Tokens in Azure:

1. Wanneer de SAS-tokens worden gemaakt met de machtigingen 'Lezen + LIST', ' schrijven en verwijderen ' gebruikers die toegang hebben tot de Blob-container met deze SAS-token niet mogelijk gegevens. Gebruikers die toegang tot de Blob-container met deze SAS-token en probeer het een schrijven of verwijderen van de bewerking ontvangt een bericht als `"This request is not authorized to perform this operation"`.

2. Wanneer de SAS-tokens worden gegenereerd met `READ + LIST + WRITE` machtigingen (om te beperken `DELETE` alleen), opdrachten, zoals `hadoop fs -put` eerst schrijven naar een `\_COPYING\_` -bestand en probeer het vervolgens naar de naam van het bestand. Met deze bewerking HDFS wordt toegewezen aan een `copy+delete` voor WASB. Omdat de `DELETE` machtiging is niet opgegeven, de 'put"mislukken. De `\_COPYING\_` bewerking is een Hadoop-functie die bestemd zijn voor sommige gelijktijdigheidsbeheer. Er is momenteel geen manier om te beperken van alleen de bewerking 'DELETE' zonder dat ' "schrijfbewerkingen ook.

3. Helaas, de hadoop Referentieprovider en de belangrijkste provider voor ontsleutelen (ShellDecryptionKeyProvider) op dit moment werken niet met de SAS-tokens en dus het momenteel niet worden beveiligd vanaf zichtbaarheid.

Zie voor meer informatie, [gebruikt Azure Storage handtekeningen voor gedeelde toegang te beperken van toegang tot gegevens in HDInsight](../hdinsight-storage-sharedaccesssignature-permissions.md)

## <a name="use-data-encryption-and-replication"></a>Gebruik gegevensversleuteling en replicatie

Alle gegevens die zijn geschreven naar Azure Storage automatisch versleuteld met behulp van [Storage Service Encryption (SSE)](../../storage/common/storage-service-encryption.md). De gegevens in de Azure storage-account worden altijd gerepliceerd voor hoge beschikbaarheid. Wanneer u een opslagaccount maakt, kunt u een van de volgende replicatieopties kiezen:

- [Lokaal redundante opslag (LRS)](../../storage/common/storage-redundancy-lrs.md)
- [Zone-redundante opslag (ZRS)](../../storage/common/storage-redundancy-zrs.md)
- [Geografisch redundante opslag (GRS)](../../storage/common/storage-redundancy-grs.md)
- [Geografisch redundante opslag met leestoegang (RA-GRS)](../../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage)

Azure Data Lake Storage biedt lokaal redundante opslag (LRS), maar u moet ook kritieke gegevens kopiëren naar een ander Data Lake Storage-account in een andere regio met een frequentie die is afgestemd op de behoeften van het noodherstelplan. Er zijn verschillende methoden om te kopiëren van gegevens, waaronder [ADLCopy](../../data-lake-store/data-lake-store-copy-data-azure-storage-blob.md), DistCp, [Azure PowerShell](../../data-lake-store/data-lake-store-get-started-powershell.md), of [Azure Data Factory](../../data-factory/connector-azure-data-lake-store.md). Het is ook raadzaam om af te dwingen van het toegangsbeleid van Data Lake Storage-account om te voorkomen dat per ongeluk verwijderen.

Raadpleeg voor meer informatie de volgende artikelen:

- [Azure storage-replicatie](../../storage/common/storage-redundancy.md)
- [Richtlijnen voor noodherstel voor Azure Data Lake Storage (ADLS)](../../data-lake-store/data-lake-store-disaster-recovery-guidance.md)

## <a name="attach-additional-azure-storage-accounts-to-the-cluster"></a>Aanvullende Azure storage-accounts koppelen aan het cluster

Een Azure Storage-account of Azure Data Lake storage-account tijdens het maakproces HDInsight wordt gekozen als het standaardbestandssysteem. Naast dit standaardopslagaccount kunnen extra opslagaccounts in hetzelfde Azure-abonnement of andere Azure-abonnementen worden toegevoegd tijdens het maken van het cluster, of nadat een cluster is gemaakt.

Extra storage-account kan worden toegevoegd in een op de volgende manieren:
- Ambari HDFS Config geavanceerde aangepaste core-site toevoegen van de opslag accountnaam en sleutel van de services opnieuw starten
- Met behulp van [Script actie](../hdinsight-hadoop-add-storage.md) door door te geven van het storage-accountnaam en de sleutel

> [!Note]
> In geldige use cases, de limieten voor de Azure-opslag kunnen worden verhoogd via een aanvraag aan [ondersteuning voor Azure](https://azure.microsoft.com/support/faq/).

Raadpleeg voor meer informatie de volgende artikelen:
- [Extra opslagaccounts toevoegen aan HDInsight](../hdinsight-hadoop-add-storage.md)
- [Aanvullende Azure storage-accounts koppelen aan het cluster](https://blogs.msdn.microsoft.com/ashish/2016/08/25/hdinsight-attach-additional-azure-storage-accounts/)

## <a name="next-steps"></a>Volgende stappen

Lees het volgende artikel in deze reeks:

- [Gegevens migreren best practices voor on-premises naar Azure HDInsight Hadoop-migratie](apache-hadoop-on-premises-migration-best-practices-data-migration.md)