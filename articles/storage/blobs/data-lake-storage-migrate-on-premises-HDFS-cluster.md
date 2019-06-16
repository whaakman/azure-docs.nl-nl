---
title: Gebruik Azure Data Box voor het migreren van gegevens van on-premises HDFS opslaan naar Azure Storage
description: Gegevens migreren van een on-premises HDFS gegevensopslag naar Azure Storage
services: storage
author: normesta
ms.service: storage
ms.date: 06/11/2019
ms.author: normesta
ms.topic: article
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 65ef586d74297e692160dc075dead2f0b1d973b5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67116973"
---
# <a name="use-azure-data-box-to-migrate-data-from-an-on-premises-hdfs-store-to-azure-storage"></a>Azure Data Box gebruiken om gegevens te migreren vanuit een on-premises HDFS gegevensopslag naar Azure Storage

U kunt gegevens migreren van een on-premises HDFS gegevensopslag van uw Hadoop-cluster in Azure Storage (blob-opslag of Data Lake Storage Gen2) met behulp van een Data Box-apparaat. U kunt kiezen uit een 80 TB Data Box of een grote 770 TB Data Box.

Dit artikel helpt u deze taken uit te voeren:

> [!div class="checklist"]
> * Bereid uw gegevens te migreren.
> * Uw gegevens kopiëren naar een Data Box of een Data Box zware apparaat.
> * Het apparaat terug naar Microsoft verzenden.
> * De gegevens naar Data Lake Storage Gen2 verplaatsen.

## <a name="prerequisites"></a>Vereisten

U moet deze dingen om de migratie te voltooien.

* Twee opslagaccounts; met een hiërarchische naamruimte ingeschakeld op deze, en één die niet.

* Een on-premises Hadoop-cluster met de brongegevens.

* Een [Azure Data Box-apparaat](https://azure.microsoft.com/services/storage/databox/).

  * [Uw Data Box bestellen](https://docs.microsoft.com/azure/databox/data-box-deploy-ordered) of [Data Box-zwaar](https://docs.microsoft.com/azure/databox/data-box-heavy-deploy-ordered). Tijdens het ordenen van uw apparaat, vergeet niet dat een opslagaccount kiezen **niet** hiërarchische naamruimten die zijn ingeschakeld op deze hebben. Dit komt doordat Data Box-apparaten bieden nog geen ondersteuning voor directe opname in Azure Data Lake Storage Gen2. U moet te kopiëren naar een opslagaccount en klikt u vervolgens een tweede exemplaar bij het Gen2 ADLS-account. Instructies hiervoor staan vermeld in de onderstaande stappen.

  * Bekabelen en verbinding maken met uw [Data Box](https://docs.microsoft.com/azure/databox/data-box-deploy-set-up) of [Data Box zware](https://docs.microsoft.com/azure/databox/data-box-heavy-deploy-set-up) naar een on-premises netwerk.

Als u klaar bent, laten we beginnen.

## <a name="copy-your-data-to-a-data-box-device"></a>Uw gegevens kopiëren naar een Data Box-apparaat

Als uw gegevens in een enkel Data Box-apparaat past, moet u de gegevens kopiëren naar de Data Box-apparaat. 

Als de gegevensgrootte van uw is groter dan de capaciteit van de Data Box-apparaat, gebruikt u de [optionele procedure voor het splitsen van de gegevens op meerdere apparaten van de Data Box](#appendix-split-data-across-multiple-data-box-devices) en voer deze stap. 

Als u wilt de gegevens van uw on-premises HDFS gegevensopslag kopiëren naar een Data Box-apparaat, moet u een aantal dingen instellen en vervolgens met de [DistCp](https://hadoop.apache.org/docs/stable/hadoop-distcp/DistCp.html) hulpprogramma.

Volg deze stappen voor het kopiëren van gegevens via de REST API's van de Blob/objectopslag naar uw Data Box-apparaat. De interface van de REST-API maakt het apparaat weergegeven als een HDFS-archief met uw cluster.

1. Voordat u de gegevens via REST kopiëren, de beveiliging en verbinding primitieven verbinding maken met de REST-interface op de Data Box of de gegevens in het zware identificeren Aanmelden bij de lokale web-UI van Data Box en gaat u naar **verbinding maken en kopiëren** pagina. Op basis van de Azure storage-account voor het apparaat onder **toegang krijgen tot instellingen**, zoek en selecteer **REST**.

    ![De pagina "Verbinding maken en kopiëren"](media/data-lake-storage-migrate-on-premises-HDFS-cluster/data-box-connect-rest.png)

2. In de storage-account voor toegang en het dialoogvenster voor uploaden van gegevens, Kopieer de **eindpunt van Blob service** en de **opslagaccountsleutel**. Van het eindpunt van blob service, laat de `https://` en de afsluitende slash bevatten.

    In dit geval wordt het eindpunt is: `https://mystorageaccount.blob.mydataboxno.microsoftdatabox.com/`. Het hostgedeelte van de URI die u gebruikt is: `mystorageaccount.blob.mydataboxno.microsoftdatabox.com`. Zie voor een voorbeeld hoe u [verbinding maken met REST via http](/azure/databox/data-box-deploy-copy-data-via-rest). 

     ![Het dialoogvenster "Toegang tot opslagaccount en gegevens uploaden"](media/data-lake-storage-migrate-on-premises-HDFS-cluster/data-box-connection-string-http.png)

3. Het eindpunt en de Data Box of zware van gegevens in het knooppunt IP-adres toevoegen `/etc/hosts` op elk knooppunt.

    ```    
    10.128.5.42  mystorageaccount.blob.mydataboxno.microsoftdatabox.com
    ```

    Als u van een ander mechanisme voor DNS gebruikmaakt, u moet ervoor zorgen dat de Data Box-eindpunt kan worden omgezet.

4. Stel de shell-variabele `azjars` naar de locatie van de `hadoop-azure` en `azure-storage` jar-bestanden. U vindt deze bestanden in de installatiemap van Hadoop.

    Om te bepalen als deze bestanden bestaat, gebruik de volgende opdracht: `ls -l $<hadoop_install_dir>/share/hadoop/tools/lib/ | grep azure`. Vervang de `<hadoop_install_dir>` tijdelijke aanduiding door het pad naar de map waarin u Hadoop hebt geïnstalleerd. Let erop dat u de volledig gekwalificeerde paden gebruiken.

    Voorbeelden:

    `azjars=$hadoop_install_dir/share/hadoop/tools/lib/hadoop-azure-2.6.0-cdh5.14.0.jar` `azjars=$azjars,$hadoop_install_dir/share/hadoop/tools/lib/microsoft-windowsazure-storage-sdk-0.6.0.jar`

5. De storage-container die u wilt gebruiken voor het kopiëren van gegevens maken. Als onderdeel van deze opdracht moet u ook een doelmap opgeven. Dit kan een dummy doelmap op dit moment zijn.

    ```
    hadoop fs -libjars $azjars \
    -D fs.AbstractFileSystem.wasb.Impl=org.apache.hadoop.fs.azure.Wasb \
    -D fs.azure.account.key.<blob_service_endpoint>=<account_key> \
    -mkdir -p  wasb://<container_name>@<blob_service_endpoint>/<destination_directory>
    ```

    * Vervang de `<blob_service_endpoint>` tijdelijke aanduiding door de naam van het eindpunt van de blob-service.

    * Vervang de `<account_key>` tijdelijke aanduiding door de toegangssleutel van uw account.

    * Vervang de `<container-name>` tijdelijke aanduiding door de naam van de container.

    * Vervang de `<destination_directory>` tijdelijke aanduiding door de naam van de map die u wilt uw gegevens te kopiëren.

6. Voer een lijst met opdracht uit om ervoor te zorgen dat uw container en map zijn gemaakt.

    ```
    hadoop fs -libjars $azjars \
    -D fs.AbstractFileSystem.wasb.Impl=org.apache.hadoop.fs.azure.Wasb \
    -D fs.azure.account.key.<blob_service_endpoint>=<account_key> \
    -ls -R  wasb://<container_name>@<blob_service_endpoint>/
    ```

   * Vervang de `<blob_service_endpoint>` tijdelijke aanduiding door de naam van het eindpunt van de blob-service.

   * Vervang de `<account_key>` tijdelijke aanduiding door de toegangssleutel van uw account.

   * Vervang de `<container-name>` tijdelijke aanduiding door de naam van de container.

7. Gegevens kopiëren van het Hadoop HDFS naar gegevens in Blob-opslag, naar de container die u eerder hebt gemaakt. Als de map die u naar kopiëren wilt niet wordt gevonden, de opdracht wordt deze automatisch aangemaakt.

    ```
    hadoop distcp \
    -libjars $azjars \
    -D fs.AbstractFileSystem.wasb.Impl=org.apache.hadoop.fs.azure.Wasb \
    -D fs.azure.account.key.<blob_service_endpoint<>=<account_key> \
    -filters <exclusion_filelist_file> \
    [-f filelist_file | /<source_directory> \
           wasb://<container_name>@<blob_service_endpoint>/<destination_directory>
    ```

    * Vervang de `<blob_service_endpoint>` tijdelijke aanduiding door de naam van het eindpunt van de blob-service.

    * Vervang de `<account_key>` tijdelijke aanduiding door de toegangssleutel van uw account.

    * Vervang de `<container-name>` tijdelijke aanduiding door de naam van de container.

    * Vervang de `<exlusion_filelist_file>` tijdelijke aanduiding door de naam van het bestand met de lijst met bestandsuitsluitingen.

    * Vervang de `<source_directory>` tijdelijke aanduiding door de naam van de map waarin de gegevens die u wilt kopiëren.

    * Vervang de `<destination_directory>` tijdelijke aanduiding door de naam van de map die u wilt uw gegevens te kopiëren.

    De `-libjars` optie wordt gebruikt om de `hadoop-azure*.jar` en de afhankelijke `azure-storage*.jar` bestanden beschikbaar voor `distcp`. Dit kan al gebeuren voor sommige clusters.

    Het volgende voorbeeld ziet u hoe de `distcp` opdracht wordt gebruikt om gegevens te kopiëren.

    ```
     hadoop distcp \
    -libjars $azjars \
    -D fs.AbstractFileSystem.wasb.Impl=org.apache.hadoop.fs.azure.Wasb \
    -D fs.azure.account.key.mystorageaccount.blob.mydataboxno.microsoftdatabox.com=myaccountkey \
    -filter ./exclusions.lst -f /tmp/copylist1 -m 4 \
    /data/testfiles \
    wasb://hdfscontainer@mystorageaccount.blob.mydataboxno.microsoftdatabox.com/data
    ```
  
    Voor het verbeteren van de snelheid kopiëren:

    * Wijzig het aantal mappers. (Maakt gebruik van het bovenstaande voorbeeld `m` = 4 mappers.)

    * Probeer uit te voeren meerdere `distcp` parallel.

    * Houd er rekening mee dat grote bestanden beter dan kleine bestanden uitvoeren.

## <a name="ship-the-data-box-to-microsoft"></a>De Data Box naar Microsoft verzenden

Volg deze stappen voor het voorbereiden en verzenden van de Data Box-apparaat naar Microsoft.

1. Eerste, [voorbereiding voor verzending op uw Data Box of de gegevens in het zware](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data-via-rest).

2. Nadat de voorbereiding van het apparaat voltooid is, moet u de stuklijst-bestanden downloaden. U gaat deze stuklijst gebruiken of manifest van de bestanden later om te controleren of de gegevens geüpload naar Azure.

3. Het apparaat uitschakelen en verwijderen van de kabels.

4. Een ophalen met UPS plannen.

    * Zie voor de Data Box-apparaten, [verzenden van uw Data Box](https://docs.microsoft.com/azure/databox/data-box-deploy-picked-up).

    * Zie voor gegevens in het zware apparaten [verzend uw Data Box-zwaar](https://docs.microsoft.com/azure/databox/data-box-heavy-deploy-picked-up).

5. Nadat Microsoft het apparaat ontvangt, is verbonden met het datacenternetwerk en de gegevens wordt geüpload naar het opslagaccount dat u (met hiërarchische naamruimten uitgeschakeld opgeeft) wanneer u de bestelling heeft geplaatst apparaat. Controleer of op basis van de bestanden stuklijst dat al uw gegevens is geüpload naar Azure. Nu kunt u deze gegevens verplaatsen naar een Data Lake Storage Gen2 storage-account.

## <a name="move-the-data-into-azure-data-lake-storage-gen2"></a>De gegevens in Azure Data Lake Storage Gen2 verplaatsen

U hebt al de gegevens in uw Azure Storage-account. Nu u de gegevens kopiëren naar uw Azure Data Lake storage-account en toegangsmachtigingen van toepassing op bestanden en mappen.

> [!NOTE]
> Deze stap is nodig als u Azure Data Lake Storage Gen2 als uw gegevensarchief. Als u alleen een blob storage-account zonder hiërarchische naamruimte als uw gegevensopslag, kunt u deze sectie overslaan.

### <a name="copy-data-to-the-azure-data-lake-storage-gen-2-account"></a>Gegevens kopiëren naar het Azure Data Lake Storage Gen 2-account

U kunt gegevens kopiëren met behulp van Azure Data Factory, of met behulp van uw Azure op basis van Hadoop-cluster.

* Zie voor het gebruik van Azure Data Factory, [Azure Data Factory om gegevens te verplaatsen naar ADLS Gen2](https://docs.microsoft.com/azure/data-factory/load-azure-data-lake-storage-gen2). Zorg ervoor dat u opgeeft **Azure Blob Storage** als de bron.

* Voer deze opdracht DistCp voor het gebruik van uw Azure op basis van Hadoop-cluster:

    ```bash
    hadoop distcp -Dfs.azure.account.key.<source_account>.dfs.windows.net=<source_account_key> abfs://<source_container> @<source_account>.dfs.windows.net/<source_path> abfs://<dest_container>@<dest_account>.dfs.windows.net/<dest_path>
    ```

    * Vervang de `<source_account>` en `<dest_account>` tijdelijke aanduidingen door de namen van de bron- en storage-accounts.

    * Vervang de `<source_container>` en `<dest_container>` tijdelijke aanduidingen door de namen van de bron- en -containers.

    * Vervang de `<source_path>` en `<dest_path>` tijdelijke aanduidingen door de paden voor bron- en doelserver.

    * Vervang de `<source_account_key>` tijdelijke aanduiding door de toegangssleutel van het opslagaccount die de gegevens bevat.

    Met deze opdracht kopieert zowel gegevens en metagegevens van uw storage-account in uw Data Lake Storage Gen2 storage-account.

### <a name="create-a-service-principal-for-your-azure-data-lake-storage-gen2-account"></a>Een service-principal voor uw Azure Data Lake Storage Gen2-account maken

Zie voor het maken van een service-principal, [het: Gebruik de portal voor het maken van een Azure AD-toepassing en service-principal die toegang hebben tot resources](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

* Wanneer u de stappen uitvoert in de sectie [De toepassing toewijzen aan een rol](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#assign-the-application-to-a-role) van het artikel, moet u ervoor zorgen dat de rol **Gegevensbijdrager voor opslagblob** is toegewezen aan de service-principal.

* Bij het uitvoeren van de stappen in de [waarden ophalen voor het aanmelden](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) gedeelte van het artikel, de toepassings-ID en de geheime waarden van de client in een tekstbestand. U hebt deze binnenkort nodig.

### <a name="generate-a-list-of-copied-files-with-their-permissions"></a>Een lijst van de gekopieerde bestanden met hun machtigingen genereren

Vanuit het on-premises Hadoop-cluster, moet u deze opdracht uitvoeren:

```bash

sudo -u hdfs ./copy-acls.sh -s /{hdfs_path} > ./filelist.json
```

Met deze opdracht genereert een lijst van de gekopieerde bestanden met hun machtigingen.

> [!NOTE]
> Afhankelijk van het aantal bestanden in het HDFS, kan met deze opdracht lang duren om uit te voeren.

### <a name="generate-a-list-of-identities-and-map-them-to-azure-active-directory-add-identities"></a>Een lijst met identiteiten genereren en toe te wijzen aan Azure Active Directory (ADD)-id 's

1. Download de `copy-acls.py` script. Zie de [helperscripts downloaden en instellen van uw edge-knooppunt om uit te voeren ze](#download-helper-scripts) sectie van dit artikel.

2. Voer deze opdracht uit om een lijst van de unieke id's te genereren.

   ```bash
   
   ./copy-acls.py -s ./filelist.json -i ./id_map.json -g
   ```

   Met dit script genereert een bestand met de naam `id_map.json` die de identiteit die u nodig hebt om te wijzen aan op basis van software-id's bevat.

3. Open de `id_map.json` bestand in een teksteditor.

4. Voor elk JSON-object dat wordt weergegeven in het bestand, werkt u de `target` kenmerk van een AAD UPN (User Principal Name) of object-id (OID), met de juiste toegewezen identiteit. Als u klaar bent, sla het bestand. U moet dit bestand in de volgende stap.

### <a name="apply-permissions-to-copied-files-and-apply-identity-mappings"></a>Machtigingen toepassen op de gekopieerde bestanden en toewijzingen van de identiteit van toepassing

Voer deze opdracht machtigingen toepassen op de gegevens die u hebt gekopieerd naar de Data Lake Storage Gen2-account:

```bash
./copy-acls.py -s ./filelist.json -i ./id_map.json  -A <storage-account-name> -C <container-name> --dest-spn-id <application-id>  --dest-spn-secret <client-secret>
```

* Vervang de tijdelijke plaatsaanduiding `<storage-account-name>` door de naam van uw opslagaccount.

* Vervang de `<container-name>` tijdelijke aanduiding door de naam van de container.

* Vervang de `<application-id>` en `<client-secret>` tijdelijke aanduidingen door de toepassing-ID en het clientgeheim die u hebt verzameld tijdens het maken van de service-principal.

## <a name="appendix-split-data-across-multiple-data-box-devices"></a>Bijlage: Gegevens verdeeld over meerdere Data Box-apparaten

Voordat u uw gegevens naar een Data Box-apparaat hebt verplaatst, moet u ervoor te zorgen dat uw gegevens zijn zo georganiseerd dat past op een Data Box-apparaat en uitsluiten van overbodige bestanden te downloaden van sommige helperscripts.

<a id="download-helper-scripts" />

### <a name="download-helper-scripts-and-set-up-your-edge-node-to-run-them"></a>Download helperscripts en instellen van uw edge-knooppunt om ze te voeren

1. Vanuit uw edge of het hoofdknooppunt van uw on-premises Hadoop-cluster, moet u deze opdracht uitvoeren:

   ```bash
   
   git clone https://github.com/jamesbak/databox-adls-loader.git
   cd databox-adls-loader
   ```

   Met deze opdracht wordt de GitHub-opslagplaats die de helperscripts bevat gekloond.

2. Zorg ervoor dat de [jq](https://stedolan.github.io/jq/) pakket is geïnstalleerd op uw lokale computer.

   ```bash
   
   sudo apt-get install jq
   ```

3. Installeer de [aanvragen](http://docs.python-requests.org/en/master/) python-pakket.

   ```bash
   
   pip install requests
   ```

4. Set machtigingen op de vereiste scripts uitvoeren.

   ```bash
   
   chmod +x *.py *.sh

   ```

### <a name="ensure-that-your-data-is-organized-to-fit-onto-a-data-box-device"></a>Zorg ervoor dat uw gegevens zijn onderverdeeld om aan te passen op een Data Box-apparaat

Als de grootte van uw gegevens groter is dan de grootte van een enkel Data Box-apparaat, kunt u bestanden splitsen in groepen die u kunt opslaan op meerdere Data Box-apparaten.

Als uw gegevens niet langer zijn dan de grootte van een afzonderlijke Data Box-apparaat, kunt u doorgaan met de volgende sectie.

1. Uitvoeren met verhoogde bevoegdheden, de `generate-file-list` script dat u hebt gedownload door de instructies in de vorige sectie.

   Hier volgt een beschrijving van de opdrachtparameters:

   ```
   sudo -u hdfs ./generate-file-list.py [-h] [-s DATABOX_SIZE] [-b FILELIST_BASENAME]
                    [-f LOG_CONFIG] [-l LOG_FILE]
                    [-v {DEBUG,INFO,WARNING,ERROR}]
                    path

   where:
   positional arguments:
   path                  The base HDFS path to process.

   optional arguments:
   -h, --help            show this help message and exit
   -s DATABOX_SIZE, --databox-size DATABOX_SIZE
                        The size of each Data Box in bytes.
   -b FILELIST_BASENAME, --filelist-basename FILELIST_BASENAME
                        The base name for the output filelists. Lists will be
                        named basename1, basename2, ... .
   -f LOG_CONFIG, --log-config LOG_CONFIG
                        The name of a configuration file for logging.
   -l LOG_FILE, --log-file LOG_FILE
                        Name of file to have log output written to (default is
                        stdout/stderr)
   -v {DEBUG,INFO,WARNING,ERROR}, --log-level {DEBUG,INFO,WARNING,ERROR}
                        Level of log information to output. Default is 'INFO'.
   ```

2. Kopieer het gegenereerde bestand voor HDFS, bevat zodat ze toegankelijk zijn voor de [DistCp](https://hadoop.apache.org/docs/stable/hadoop-distcp/DistCp.html) taak.

   ```
   hadoop fs -copyFromLocal {filelist_pattern} /[hdfs directory]
   ```

### <a name="exclude-unnecessary-files"></a>Onnodige bestanden uitsluiten

U moet een aantal mappen uitsluiten van de taak DisCp. Bijvoorbeeld, uitsluiten mappen met informatie over de status waarin het cluster uitgevoerd worden behouden.

Op de on-premises Hadoop-cluster waar u van plan bent om te starten van de taak DistCp, maak een bestand dat Hiermee wordt de lijst met mappen die u wilt uitsluiten.

Hier volgt een voorbeeld:

```
.*ranger/audit.*
.*/hbase/data/WALs.*
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de werking van Data Lake Storage Gen2 met HDInsight-clusters. Zie [Azure Data Lake Storage Gen2 gebruiken met Azure HDInsight-clusters](../../hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2.md).
