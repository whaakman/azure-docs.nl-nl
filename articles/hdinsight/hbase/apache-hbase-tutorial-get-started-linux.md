---
title: Zelfstudie - gebruik Apache HBase in Azure HDInsight
description: Volg deze zelfstudie voor Apache HBase om te starten met hadoop in HDInsight. Maak tabellen vanuit de HBase-shell en gebruik Hive om query's uit te voeren op de tabellen.
keywords: hbase-opdracht, hbase-voorbeeld
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: tutorial
ms.date: 06/25/2019
ms.author: hrasheed
ms.openlocfilehash: 48b02a042b55af9ff65f57220f7a64c9cbde8848
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445551"
---
# <a name="tutorial-use-apache-hbase-in-azure-hdinsight"></a>Zelfstudie: Apache HBase in Azure HDInsight gebruiken

Deze zelfstudie wordt gedemonstreerd hoe u een Apache HBase-cluster maken in Azure HDInsight, HBase-tabellen maken en query uitvoeren op tabellen met behulp van Apache Hive.  Raadpleeg voor algemene informatie over HBase [overzicht van HDInsight HBase](./apache-hbase-overview.md).

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Apache HBase-cluster maken
> * HBase-tabellen maken en gegevens invoegen
> * Apache Hive gebruiken met Apache HBase query
> * HBase REST API's gebruiken met Curl
> * De clusterstatus controleren

## <a name="prerequisites"></a>Vereisten

* Een SSH-client. Zie voor meer informatie [Verbinding maken met HDInsight (Apache Hadoop) via SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

* Bash. De voorbeelden in dit artikel voor het gebruik van de Bash-shell op Windows 10 voor de curl-opdrachten. Zie [Windows-subsysteem voor Linux-installatie handleiding voor Windows 10](https://docs.microsoft.com/windows/wsl/install-win10) voor de installatiestappen.  Andere [Unix shells](https://www.gnu.org/software/bash/) ook werkt.  De voorbeelden curl, met enkele kleine wijzigingen kunnen worden gebruikt voor een Windows-opdrachtprompt.  U kunt ook de Windows PowerShell-cmdlet gebruiken [Invoke-RestMethod](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-restmethod).

## <a name="create-apache-hbase-cluster"></a>Apache HBase-cluster maken

De volgende procedure maakt gebruik van een Azure Resource Manager-sjabloon om een HBase-cluster en het afhankelijke standaard Azure Storage-account te maken. Zie [Op Linux gebaseerde Hadoop-clusters maken in HDInsight](../hdinsight-hadoop-provision-linux-clusters.md) voor meer inzicht in de parameters die voor deze procedure worden gebruikt en andere methoden voor het maken van clusters.

1. Selecteer de volgende afbeelding voor de sjabloon in Azure portal. De sjabloon bevindt zich in [Azure-snelstartsjablonen](https://azure.microsoft.com/resources/templates/).

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Voer op de blade **Aangepaste implementatie** de volgende waarden in:

    |Eigenschap |Description |
    |---|---|
    |Abonnement|Selecteer uw Azure-abonnement dat wordt gebruikt om het cluster te maken.|
    |Resourcegroep|Maken van een Azure-resourcebeheergroep of gebruik een bestaande resourcegroep.|
    |Location|Geef de locatie van de resourcegroep. |
    |ClusterName|Voer een naam voor het HBase-cluster.|
    |Cluster-aanmeldingsnaam en wachtwoord|De standaardaanmeldingsnaam is **admin**.|
    |SSH-gebruikersnaam en wachtwoord|De standaardgebruikersnaam is **sshuser**.|

    Andere parameters zijn optioneel.  

    Elk cluster is afhankelijk van een Azure Storage-account. Nadat u een cluster hebt verwijderd, blijven de gegevens in het opslagaccount staan. De naam van het standaardopslagaccount voor het cluster is de naam waaraan 'store' is toegevoegd. Deze is vastgelegd in de sectie met sjabloonvariabelen.

3. Selecteer **Ik ga akkoord met de bovenstaande voorwaarden** en selecteer vervolgens **Kopen**. Het duurt ongeveer 20 minuten om een cluster te maken.

Nadat een HBase-cluster is verwijderd, kunt u een ander HBase-cluster maken met de dezelfde standaard blob-container. Het nieuwe cluster haalt de HBase-tabellen op die u hebt gemaakt in het oorspronkelijke cluster. Om inconsistenties te voorkomen, wordt u aangeraden de HBase-tabellen uit te schakelen voordat u het cluster verwijdert.

## <a name="create-tables-and-insert-data"></a>Tabellen maken en gegevens invoegen

U kunt SSH gebruiken om verbinding te maken met HBase-clusters en vervolgens [Apache HBase Shell](https://hbase.apache.org/0.94/book/shell.html) gebruiken om HBase-tabellen te maken, gegevens in te voegen, en gegevens te doorzoeken.

Voor de meeste mensen worden de gegevens weergegeven in een tabelindeling:

![Tabelgegevens in HDInsight HBase](./media/apache-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-tabular.png)

In HBase (een implementatie van [Cloud BigTable](https://cloud.google.com/bigtable/)), dezelfde gegevens ziet eruit als:

![BigTable-gegevens in HDInsight HBase](./media/apache-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-bigtable.png)

**De HBase-shell gebruiken**

1. Gebruik `ssh` opdracht verbinding maken met uw HBase-cluster. De onderstaande opdracht bewerken door te vervangen `CLUSTERNAME` met de naam van het cluster, en voer vervolgens de opdracht:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Gebruik `hbase shell` opdracht om te beginnen de interactieve HBase-shell. Voer de volgende opdracht in uw SSH-verbinding:

    ```bash
    hbase shell
    ```

1. Gebruik `create` opdracht voor het maken van een HBase-tabel met twee kolomfamilies. De tabel- en kolomnamen zijn hoofdlettergevoelig. Voer de volgende opdracht in:

    ```hbaseshell
    create 'Contacts', 'Personal', 'Office'
    ```

1. Gebruik `list` opdracht om een lijst van alle tabellen in HBase. Voer de volgende opdracht in:

    ```hbase
    list
    ```

1. Gebruik `put` opdracht voor het invoegen van waarden op een opgegeven kolom in een opgegeven rij in een bepaalde tabel. Voer de volgende opdrachten:

    ```hbaseshell
    put 'Contacts', '1000', 'Personal:Name', 'John Dole'
    put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
    put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
    put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
    ```

1. Gebruik `scan` opdracht om te scannen en retourneert de `Contacts` tabelgegevens. Voer de volgende opdracht in:

    ```hbase
    scan 'Contacts'
    ```

    ![HDInsight Hadoop HBase-shell](./media/apache-hbase-tutorial-get-started-linux/hdinsight-hbase-shell.png)

1. Gebruik `get` opdracht voor het ophalen van inhoud van een rij. Voer de volgende opdracht in:

    ```hbaseshell
    get 'Contacts', '1000'
    ```

    Ziet u hetzelfde resultaat als wanneer u de `scan` opdracht omdat er slechts één rij is.

    Zie voor meer informatie over het HBase-tabelschema [Inleiding tot Apache HBase-schemaontwerp](http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf). Zie voor meer HBase-opdrachten [Snelzoekgids voor Apache HBase](https://hbase.apache.org/book.html#quickstart).

1. Gebruik `exit` opdracht om te stoppen van de interactieve HBase-shell. Voer de volgende opdracht in:

    ```hbaseshell
    exit
    ```

**Gegevens bulksgewijs laden in de HBase-tabel met contacten**

U kunt in HBase verschillende methoden gebruiken om gegevens in tabellen te laden.  Zie [Bulk loading](https://hbase.apache.org/book.html#arch.bulk.load) (Bulkgsgewijs laden) voor meer informatie.

Een bestand met voorbeeldgegevens vindt u in een openbare blob-container `wasb://hbasecontacts\@hditutorialdata.blob.core.windows.net/contacts.txt`.  De inhoud van het gegevensbestand is:

    8396    Calvin Raji      230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu         646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie         508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson     674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller    397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile     592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee        870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes       599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander  670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander    998-555-0171    230-555-0200    771 Northridge Drive

U kunt een tekstbestand maken en het bestand desgewenst uploaden naar uw eigen opslagaccount. Zie voor instructies [gegevens uploaden voor Apache Hadoop-taken in HDInsight](../hdinsight-upload-data.md).

Deze procedure maakt gebruik van de `Contacts` HBase-tabel in de laatste procedure hebt gemaakt.

1. Van uw open ssh-verbinding, voer de volgende opdracht uit om de gegevens te transformeren naar StoreFiles-bestand en opslaan op een relatief pad dat is opgegeven door `Dimporttsv.bulk.output`.

    ```bash
    hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt
    ```

2. Voer de volgende opdracht voor het uploaden van de gegevens van `/example/data/storeDataFileOutput` aan de HBase-tabel:

    ```bash
    hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts
    ```

3. U kunt de HBase-shell openen en gebruiken de `scan` opdracht om de inhoud van de tabel weer te geven.

## <a name="use-apache-hive-to-query-apache-hbase"></a>Apache Hive gebruiken met Apache HBase query

U kunt gegevens in HBase-tabellen opvragen met behulp van [Apache Hive](https://hive.apache.org/). In dit gedeelte maakt u een Hive-tabel die is toegewezen aan de HBase-tabel en deze gebruikt om een query voor de gegevens in uw HBase-tabel uit te voeren.

1. Van uw ssh-verbinding openen, gebruikt u de volgende opdracht om Beeline te starten:

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
    ```

    Zie voor meer informatie over Beeline [Hive gebruiken met Hadoop in HDInsight met Beeline](../hadoop/apache-hadoop-use-hive-beeline.md).

1. Voer de volgende [HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual) script voor het maken van een Hive-tabel die is toegewezen aan de HBase-tabel. Zorg ervoor dat u hebt gemaakt aan de tabel waarnaar eerder in dit artikel wordt verwezen met behulp van de HBase-shell voordat u deze instructie uitvoert.

    ```hiveql
    CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
    STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
    WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
    TBLPROPERTIES ('hbase.table.name' = 'Contacts');
    ```

1. Voer het volgende HiveQL-script uit om een query uit te voeren voor de gegevens in de HBase-tabel:

    ```hiveql
    SELECT count(rowkey) AS rk_count FROM hbasecontacts;
    ```

1. Gebruiken om af te sluiten Beeline `!exit`.

1. Om af te sluiten uw ssh-verbinding, gebruik `exit`.

## <a name="use-hbase-rest-apis-using-curl"></a>HBase REST API's gebruiken met Curl

De REST API is beveiligd via [basisverificatie](https://en.wikipedia.org/wiki/Basic_access_authentication). U moet aanvragen altijd uitvoeren via een beveiligde HTTP-verbinding (HTTPS). Zo zorgt u ervoor dat uw referenties veilig worden verzonden naar de server.

1. Start de omgevingsvariabele voor gebruiksgemak. De onderstaande opdrachten bewerken door te vervangen `MYPASSWORD` met het wachtwoord voor clusteraanmelding. Vervang `MYCLUSTERNAME` met de naam van uw HBase-cluster. Voer de opdrachten.

    ```bash
    export password='MYPASSWORD'
    export clustername=MYCLUSTERNAME
    ```

1. Gebruik de volgende opdracht om een lijst met bestaande HBase-tabellen weer te geven:

    ```bash
    curl -u admin:$password \
    -G https://$clustername.azurehdinsight.net/hbaserest/
    ```

1. Gebruik de volgende opdracht om een nieuwe HBase-tabel met twee kolomfamilies te maken:

    ```bash
    curl -u admin:$password \
    -X PUT "https://$clustername.azurehdinsight.net/hbaserest/Contacts1/schema" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
    -v
    ```

    Het schema wordt opgegeven in de JSON-indeling.
1. Gebruik de volgende opdracht om enkele gegevens in te voegen:

    ```bash
    curl -u admin:$password \
    -X PUT "https://$clustername.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "{\"Row\":[{\"key\":\"MTAwMA==\",\"Cell\": [{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}]}]}" \
    -v
    ```

    U moet de waarden die in de schakeloptie -d zijn opgegeven, met Base64 coderen. In het voorbeeld:

   * MTAwMA==: 1000
   * UGVyc29uYWw6TmFtZQ ==: Persoonlijk: naam
   * Sm9obiBEb2xl: Joep Davids

     [false-row-key](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) maakt het mogelijk om meerdere waarden (in batch) in te voegen.

1. Gebruik de volgende opdracht om een rij te verkrijgen:

    ```bash
    curl -u admin:$password \
    GET "https://$clustername.azurehdinsight.net/hbaserest/Contacts1/1000" \
    -H "Accept: application/json" \
    -v
    ```

Zie [Apache HBase Reference Guide](https://hbase.apache.org/book.html#_rest) (Snelzoekgids voor Apache HBase) voor meer informatie over HBase REST.

> [!NOTE]  
> Thrift wordt niet ondersteund door HBase in HDInsight.
>
> Wanneer u Curl of een andere REST-communicatie gebruikt met WebHCat, moet u de aanvragen verifiëren door de gebruikersnaam en het wachtwoord voor de beheerder van het HDInsight-cluster op te geven. U moet ook de clusternaam gebruiken als onderdeel van de URI (Uniform Resource Identifier) die wordt gebruikt om de aanvragen naar de server te verzenden:
> 
>   
>        curl -u <UserName>:<Password> \
>        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status
>   
>    Het antwoord dat u ontvangt, is vergelijkbaar met het volgende antwoord:
>   
>        {"status":"ok","version":"v1"}

## <a name="check-cluster-status"></a>De clusterstatus controleren

HBase in HDInsight wordt geleverd met een webgebruikersinterface voor het bewaken van clusters. Met de webgebruikersinterface kunt u statistieken of informatie over regio's aanvragen.

**De HBase-hoofdinterface openen**

1. Meld u aan bij de Ambari-Webinterface op `https://CLUSTERNAME.azurehdinsight.net` waar `CLUSTERNAME` is de naam van uw HBase-cluster.

1. Selecteer **HBase** in het menu links.

1. Selecteer **snelkoppelingen** verwijzen naar de actieve Zookeeper-knooppuntkoppeling boven aan de pagina en selecteer vervolgens **HBase Master UI**.  De interface wordt in een nieuw browsertabblad geopend:

   ![HDInsight HBase HMaster-interface](./media/apache-hbase-tutorial-get-started-linux/hdinsight-hbase-hmaster-ui.png)

   De HBase-hoofdinterface bevat de volgende onderdelen:

   - regioservers
   - back-upmasters
   - tabellen
   - taken
   - softwarekenmerken

## <a name="clean-up-resources"></a>Resources opschonen

Om inconsistenties te voorkomen, wordt u aangeraden de HBase-tabellen uit te schakelen voordat u het cluster verwijdert. U kunt de HBase-opdracht gebruiken `disable 'Contacts'`. Als u deze toepassing verder niet meer gebruikt, verwijdert u het HBase-cluster dat u hebt gemaakt, via de volgende stappen:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
1. Typ **HDInsight** in het **Zoekvak** bovenaan.
1. Selecteer onder **Services** de optie **HDInsight-clusters**.
1. Klik in de lijst met HDInsight-clusters die wordt weergegeven, op de **...** naast het cluster dat u voor deze zelfstudie hebt gemaakt.
1. Klik op **Verwijderen**. Klik op **Ja**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u een Apache HBase-cluster maakt en hoe u tabellen maken en de gegevens in deze tabellen vanuit de HBase-shell weergeeft. U hebt ook geleerd hoe u een Hive-query op gegevens in HBase-tabellen uitvoert en hoe u de HBase C# REST API's gebruikt om een HBase-tabel te maken en gegevens op te halen uit de tabel. Voor meer informatie zie:

> [!div class="nextstepaction"]
> [Overzicht van HDInsight HBase](./apache-hbase-overview.md)