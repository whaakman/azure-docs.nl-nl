---
title: Voer een met behulp van Azure DB die Cosmos en HDInsight Hadoop | Microsoft Docs
description: Ontdek hoe u een eenvoudige Hive, Pig en MapReduce-taak uitvoert met Azure Cosmos DB en Azure HDInsight.
services: cosmos-db
author: dennyglee
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 06f0ea9d-07cb-4593-a9c5-ab912b62ac42
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 01/19/2018
ms.author: denlee
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 181954e4657166db8aa94021ad093437d8c7abfd
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="Azure Cosmos DB-HDInsight"></a>Voer een Apache Hive, Pig of Hadoop met behulp van Azure DB die Cosmos en HDInsight
Deze zelfstudie leert u hoe u uitvoert [Apache Hive][apache-hive], [Apache Pig][apache-pig], en [Apache Hadoop] [ apache-hadoop] MapReduce-taken in Azure HDInsight met Hadoop-connector Cosmos-database. Hadoop-connector cosmos-database kunt Cosmos DB om te fungeren als een bron- en sink voor Hive, Pig en MapReduce-taken. Deze zelfstudie gaat Cosmos DB gebruiken als de gegevensbron en de bestemming voor Hadoop-taken.

> [!IMPORTANT] 
> De Spark op Azure DB die Cosmos-connector is de aanbevolen optie voor het Azure HDInsight verbinden met Azure Cosmos DB. Zie voor meer informatie [versnellen realtime big data-analyses met de Spark op Azure Cosmos DB connector](spark-connector.md).

Na het voltooien van deze zelfstudie, hebt u mogelijk de volgende vragen beantwoorden:

* Hoe ik gegevens laden van de Cosmos-database met behulp van een Hive, Pig of MapReduce-taak?
* Hoe kan ik gegevens opslaan in Cosmos-database met behulp van een Hive, Pig of MapReduce-taak?

Het is raadzaam om aan de slag door de volgende video, waar we uitvoeren via een met behulp van de Cosmos-DB en HDInsight Hive-taak kijken.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Use-Azure-DocumentDB-Hadoop-Connector-with-Azure-HDInsight/player]
>
>

Keer vervolgens terug naar dit artikel, waar u de volledige informatie over hoe u analytics-taken op uw gegevens Cosmos DB uitvoeren kunt hebt ontvangen.

> [!TIP]
> Deze zelfstudie wordt ervan uitgegaan dat u ervaring met Apache Hadoop, Hive en/of Pig hebt. Als u niet bekend met Apache Hadoop Hive en Pig bent, raden wij aan via de [Apache Hadoop-documentatie][apache-hadoop-doc]. Deze zelfstudie wordt ervan uitgegaan dat u ervaring met Cosmos DB hebt en een Cosmos-DB-account hebben. Als u niet bekend met Cosmos DB bent of u beschikt niet over een Cosmos-DB-account, check onze [aan de slag] [ getting-started] pagina.
>
>

Geen tijd om de zelfstudie te voltooien en wilt ophalen van de PowerShell-scripts voor het volledige voorbeeld voor Hive, Pig en MapReduce? Geen probleem, ze [hier][hdinsight-samples]. De download bevat ook de hql, pig en java-bestanden voor deze voorbeelden.

## <a name="NewestVersion"></a>Meest recente versie
<table border='1'>
    <tr><th>Versie van Hadoop-Connector</th>
        <td>1.2.0</td></tr>
    <tr><th>Script-Uri</th>
        <td>https://portalcontent.blob.core.windows.net/scriptaction/documentdb-hadoop-installer-v04.ps1</td></tr>
    <tr><th>Datum gewijzigd</th>
        <td>04/26/2016</td></tr>
    <tr><th>Ondersteunde HDInsight-versies</th>
        <td>3.1, 3.2</td></tr>
    <tr><th>Wijzigingenlogboek</th>
        <td>Bijgewerkt Azure Cosmos DB Java SDK voor 1.6.0</br>
            Toegevoegde ondersteuning voor gepartitioneerde verzamelingen op als een bron- en sink</br>
        </td></tr>
</table>

## <a name="Prerequisites"></a>Vereisten
Voordat u de instructies in deze zelfstudie, zorg ervoor dat u het volgende hebt:

* Een Cosmos-DB-account, een database en een verzameling met documenten in. Zie voor meer informatie [aan de slag met Cosmos DB][getting-started]. Voorbeeldgegevens importeren in een Cosmos-DB-account met de [Cosmos DB import-hulpprogramma][import-data].
* De doorvoer. Leest en schrijft uit HDInsight naar uw toegewezen aanvraageenheden voor uw verzamelingen worden geteld.
* De uitvoer van capaciteit voor een aanvullende opgeslagen procedure binnen elke verzameling. De opgeslagen procedures worden gebruikt voor het overdragen van de resulterende documenten.
* Capaciteit voor de resulterende documenten uit de Hive, Pig of MapReduce-taken.
* [*Optioneel*] capaciteit voor een aanvullende verzameling.

> [!WARNING]
> Om te voorkomen dat het maken van een nieuwe verzameling tijdens een van de taken, kunt u de resultaten naar stdout afdrukken, sla de uitvoer naar de container WASB of geef een bestaande verzameling op. In het geval van een bestaande verzameling geven, nieuwe documenten gemaakt in de verzameling en de al bestaande documenten alleen worden beïnvloed als er een conflict in *id's*. **De connector wordt automatisch bestaande documenten overschreven met de ID-conflicten**. U kunt deze functie uitschakelen door de optie upsert op false. Als upsert ingesteld op false is en een conflict optreedt, mislukt de taak Hadoop; een id conflict fout wordt gemeld.
>
>

## <a name="ProvisionHDInsight"></a>Stap 1: Maak een nieuw HDInsight-cluster
Deze zelfstudie wordt scriptactie vanuit de Azure Portal voor het aanpassen van uw HDInsight-cluster. In deze zelfstudie gebruiken we de Azure Portal om uw HDInsight-cluster te maken. Voor instructies over het gebruik van PowerShell-cmdlets of de HDInsight-SDK voor .NET, bekijk de [aanpassen HDInsight-clusters met behulp van de scriptactie] [ hdinsight-custom-provision] artikel.

1. Aanmelden bij de [Azure-Portal][azure-portal].
2. Klik op **+ nieuw** zoekt boven aan het linkernavigatiegedeelte **HDInsight** in de bovenste zoekbalk op de nieuwe blade.
3. **HDInsight** gepubliceerd door **Microsoft** aan de bovenkant van de resultaten worden weergegeven. Klik hierop en klik vervolgens op **maken**.
4. Blade maken op het nieuwe HDInsight-Cluster, Voer uw **clusternaam** en selecteer de **abonnement** wilt u deze bron op onder inrichten.

    <table border='1'>
        <tr><td>Clusternaam</td><td>De naam van het cluster.<br/>
DNS-naam moet beginnen en eindigen met een teken alfanumerieke en streepjes kan bevatten.<br/>
Het veld moet een tekenreeks tussen 3 en 63 tekens lang zijn.</td></tr>
        <tr><td>Naam abonnement</td>
            <td>Als u meer dan één Azure-abonnement hebt, selecteert u het abonnement dat als voor uw HDInsight-cluster host fungeert. </td></tr>
    </table>
5.Klik op **clustertype Selecteer** en de volgende eigenschappen instellen op de opgegeven waarden.

    <table border='1'>
        <tr><td>Clustertype</td><td><strong>Hadoop</strong></td></tr>
        <tr><td>Cluster-laag</td><td><strong>Standard</strong></td></tr>
        <tr><td>Besturingssysteem</td><td><strong>Windows</strong></td></tr>
        <tr><td>Versie</td><td>meest recente versie</td></tr>
    </table>

    Klik nu op **Selecteer**.

    ![Hadoop HDInsight initiële clusterdetails bieden][image-customprovision-page1]
6. Klik op **referenties** uw aanmeldgegevens en referenties voor externe toegang instellen. Kies uw **Cluster aanmelding gebruikersnaam** en **aanmeldingswachtwoord Cluster**.

    Als u wilt dat extern in uw cluster, selecteert u *Ja* onderaan de blade en geef een gebruikersnaam en wachtwoord.
7. Klik op **gegevensbron** instellen van uw primaire locatie voor toegang tot gegevens. Kies de **selectiemethode** en geef een bestaand opslagaccount of maak een nieuwe.
8. Geef op de blade dezelfde een **standaard Container** en een **locatie**. En klik op **Selecteer**.

   > [!NOTE]
   > Selecteer een locatie dicht bij uw regio Cosmos-DB-account voor betere prestaties
   >
   >
9. Klik op **prijzen** selecteren van het aantal en type van knooppunten. U kunt de standaardconfiguratie houden en schalen het aantal Worker-knooppunten later op.
10. Klik op **optionele configuratie**, klikt u vervolgens **acties Script** in de Blade optionele configuratie.

     Voer de volgende informatie voor het aanpassen van uw HDInsight-cluster in Script-acties.

     <table border='1'>
         <tr><th>Eigenschap</th><th>Waarde</th></tr>
         <tr><td>Naam</td>
             <td>Geef een naam voor de scriptactie.</td></tr>
         <tr><td>Script-URI</td>
             <td>Geef de URI moet het script dat wordt opgeroepen voor het aanpassen van het cluster.</br></br>
Voer in: </br> <strong>https://portalcontent.blob.core.windows.net/scriptaction/documentdb-hadoop-installer-v04.ps1</strong>.</td></tr>
         <tr><td>Kop</td>
             <td>Klik op het selectievakje in om te worden uitgevoerd op het hoofdknooppunt van het PowerShell-script.</br></br>
             <strong>Schakel dit selectievakje in</strong>.</td></tr>
         <tr><td>Werknemer</td>
             <td>Klik op het selectievakje in als u wilt uitvoeren van het PowerShell-script op het werkrolknooppunt.</br></br>
             <strong>Schakel dit selectievakje in</strong>.</td></tr>
         <tr><td>Zookeeper</td>
             <td>Klik op het selectievakje in als u wilt uitvoeren van het PowerShell-script op de Zookeeper.</br></br>
             <strong>Niet nodig</strong>.
             </td></tr>
         <tr><td>Parameters</td>
             <td>Geef de parameters op, indien vereist door het script.</br></br>
             <strong>Er zijn geen Parameters nodig</strong>.</td></tr>
     </table>
11.Maken van een nieuwe **resourcegroep** of gebruik een bestaande resourcegroep onder uw Azure-abonnement.
12. Controleer nu **vastmaken aan dashboard** bijhouden van de implementatie en klikt u op **maken**!

## <a name="InstallCmdlets"></a>Stap 2: Installeer en configureer Azure PowerShell
1. Installeer Azure PowerShell. Instructies hiervoor vindt u [hier][powershell-install-configure].

   > [!NOTE]
   > U kunt ook van HDInsight online Hive-Editor gebruiken voor Hive-query's. Om dit te doen, moet u zich aanmelden bij de [Azure Portal][azure-portal], klikt u op **HDInsight** in het linkerdeelvenster om een lijst met uw HDInsight-clusters weer te geven. Klik op het cluster dat u wilt uitvoeren van Hive-query's op en klik vervolgens op **Query Console**.
   >
   >
2. Open de Azure PowerShell Integrated Scripting Environment:

   * Op een computer met Windows 8 of WindowsServer 2012 of nieuwer, kunt u de ingebouwde zoekopdracht. Typ in het startscherm **powershell ise** en klik op **Enter**.
   * Gebruik het menu Start op een computer met een besturingssysteem dat ouder dan Windows 8 of Windows Server 2012. Typ in het menu Start **opdrachtprompt** in het zoekvak en vervolgens in de lijst met resultaten, klikt u op **opdrachtprompt**. Typ in het opdrachtpromptvenster **powershell_ise** en klik op **Enter**.
3. Uw Azure-Account toevoegen.

   1. Typ in het consolevenster **Add-AzureAccount** en klik op **Enter**.
   2. Typ in het e-mailadres dat is gekoppeld aan uw Azure-abonnement en klikt u op **doorgaan**.
   3. Typ in het wachtwoord voor uw Azure-abonnement.
   4. Klik op **aanmelden**.
4. Het volgende diagram worden de belangrijke onderdelen van uw omgeving Azure PowerShell-scripts.

    ![Diagram voor Azure PowerShell][azure-powershell-diagram]

## <a name="RunHive"></a>Stap 3: Voer een Hive-taak met behulp van Azure DB die Cosmos en HDInsight
> [!IMPORTANT]
> Alle variabelen aangegeven door een combinatie moeten worden ingevuld met behulp van uw configuratie-instellingen.
>
>

1. De volgende variabelen instellen in het deelvenster van de PowerShell-Script.

        # Provide Azure subscription name, the Azure Storage account and container that is used for the default HDInsight file system.
        $subscriptionName = "<SubscriptionName>"
        $storageAccountName = "<AzureStorageAccountName>"
        $containerName = "<AzureStorageContainerName>"

        # Provide the HDInsight cluster name where you want to run the Hive job.
        $clusterName = "<HDInsightClusterName>"
2. <p>Laten we beginnen construeren van de queryreeks. We moet een Hive-query die wordt het systeem gegenereerde tijdstempels (_ts) en de unieke id's (_rid) uit een verzameling Azure Cosmos DB alle documenten, telt alle documenten per minuut en slaat vervolgens de resultaten weer in een nieuwe Azure DB die Cosmos-verzameling schrijven.</p>

    <p>Eerst gaan we een Hive-tabel maken van onze Azure DB die Cosmos-verzameling. Het volgende codefragment toevoegen aan het deelvenster met PowerShell-Script <strong>nadat</strong> het codefragment van #1. Zorg ervoor dat u de optionele queryparameter om documenten kunnen alleen _ts trim en _rid.</p>

   > [!NOTE]
   > **Naamgeving van DocumentDB.inputCollections is niet een fout.** Ja, kunnen we meerdere verzamelingen als invoer toe te voegen: </br>
   >
   >

        '*DocumentDB.inputCollections*' = '*\<DocumentDB Input Collection Name 1\>*,*\<DocumentDB Input Collection Name 2\>*' A1A</br> The collection names are separated without spaces, using only a single comma.

        # Create a Hive table using data from Azure Cosmos DB. Pass Azure Cosmos DB the query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "drop table DocumentDB_timestamps; "  +
                            "create external table DocumentDB_timestamps(id string, ts BIGINT) "  +
                            "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' "  +
                            "tblproperties ( " +
                                "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                "'DocumentDB.key' = '<DocumentDB Primary Key>', " +
                                "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                "'DocumentDB.inputCollections' = '<DocumentDB Input Collection Name>', " +
                                "'DocumentDB.query' = 'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

1. Vervolgens maken we een Hive-tabel voor de uitvoer-verzameling. De eigenschappen van de uitvoer is de maand, dag, uur, minuut en het totale aantal exemplaren.

   > [!NOTE]
   > **Naamgeving van DocumentDB.outputCollections was nog opnieuw niet een fout.** Ja, kunnen we meerdere verzamelingen toe te voegen als uitvoer: </br>
   > '*DocumentDB.outputCollections*'='*\<DocumentDB verzameling uitvoernaam 1\>*,*\<DocumentDB verzameling uitvoernaam 2\>* ' </br> De verzamelingsnamen van de worden zonder spaties, met slechts een enkele komma gescheiden. </br></br>
   > Documenten worden gedistribueerde round robin in meerdere verzamelingen. Een batch van documenten worden opgeslagen in één verzameling en vervolgens een tweede reeks documenten worden opgeslagen in de volgende verzameling, enzovoort.
   >
   >

       # Create a Hive table for the output data to Azure Cosmos DB.
       $queryStringPart2 = "drop table DocumentDB_analytics; " +
                             "create external table DocumentDB_analytics(Month INT, Day INT, Hour INT, Minute INT, Total INT) " +
                             "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' " +
                             "tblproperties ( " +
                                 "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                 "'DocumentDB.key' = '<DocumentDB Primary Key>', " +  
                                 "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                 "'DocumentDB.outputCollections' = '<DocumentDB Output Collection Name>' ); "
2. Tot slot gaan we de documenten tally per maand, dag, uur en minuut en invoegen van de resultaten weer in de uitvoer van de Hive-tabel.

        # GROUP BY minute, COUNT entries for each, INSERT INTO output Hive table.
        $queryStringPart3 = "INSERT INTO table DocumentDB_analytics " +
                              "SELECT month(from_unixtime(ts)) as Month, day(from_unixtime(ts)) as Day, " +
                              "hour(from_unixtime(ts)) as Hour, minute(from_unixtime(ts)) as Minute, " +
                              "COUNT(*) AS Total " +
                              "FROM DocumentDB_timestamps " +
                              "GROUP BY month(from_unixtime(ts)), day(from_unixtime(ts)), " +
                              "hour(from_unixtime(ts)) , minute(from_unixtime(ts)); "
3. Voeg het volgende fragment script voor het maken van de definitie van een Hive-taak uit de vorige query.

        # Create a Hive job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $hiveJobDefinition = New-AzureHDInsightHiveJobDefinition -Query $queryString

    U kunt ook gebruiken om op te geven van een scriptbestand HiveQL op HDFS-schakeloptie in het bestand.
4. Voeg het volgende codefragment voor het opslaan van de begintijd en verzenden van de Hive-taak.

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $hiveJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $hiveJobDefinition
5. Voeg de volgende wachten tot de Hive-taak te voltooien.

        # Wait for the Hive job to complete.
        Wait-AzureHDInsightJob -Job $hiveJob -WaitTimeoutInSeconds 3600
6. Voeg de volgende om de standaarduitvoer en de begin- en eindtijden te drukken.

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $hiveJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
7. **Voer** het nieuwe script! **Klik op** de knop groen uitvoeren.
8. Controleer de resultaten. Meld u aan bij de [Azure-Portal][azure-portal].

   1. Klik op <strong>Bladeren</strong> in het paneel aan de linkerkant. </br>
   2. Klik op <strong>Alles</strong> in de rechterbovenhoek van het paneel bladeren. </br>
   3. Zoek en klik op <strong>Azure Cosmos DB Accounts</strong>. </br>
   4. Zoek vervolgens uw <strong>Azure Cosmos-DB Account</strong>, klikt u vervolgens <strong>Azure Cosmos DB Database</strong> en uw <strong>Azure Cosmos DB verzameling</strong> die zijn gekoppeld aan de verzameling uitvoer is opgegeven in uw Hive-query.</br>
   5. Tot slot op <strong>documentverkenner</strong> onder <strong>hulpprogramma's voor ontwikkelaars</strong>.</br></p>

   Hier ziet u de resultaten van uw Hive-query.

   ![De resultaten van de hive-query][image-hive-query-results]

## <a name="RunPig"></a>Stap 4: Een Pig-taak met Cosmos DB en HDInsight uitvoeren
> [!IMPORTANT]
> Alle variabelen aangegeven door een combinatie moeten worden ingevuld met behulp van uw configuratie-instellingen.
>
>

1. De volgende variabelen instellen in het deelvenster van de PowerShell-Script.

        # Provide Azure subscription name.
        $subscriptionName = "Azure Subscription Name"

        # Provide HDInsight cluster name where you want to run the Pig job.
        $clusterName = "Azure HDInsight Cluster Name"
2. <p>Laten we beginnen construeren van de queryreeks. We moet een Pig-query die wordt het systeem gegenereerde tijdstempels (_ts) en de unieke id's (_rid) uit een verzameling Azure Cosmos DB alle documenten, telt alle documenten per minuut en slaat vervolgens de resultaten weer in een nieuwe Azure DB die Cosmos-verzameling schrijven.</p>
    <p>Documenten van de Cosmos-database eerst laden in HDInsight. Het volgende codefragment toevoegen aan het deelvenster met PowerShell-Script <strong>nadat</strong> het codefragment van #1. Zorg ervoor dat u een query toevoegen aan de optionele parameter van de query DocumentDB om onze documenten kunnen alleen _ts trim en _rid.</p>

   > [!NOTE]
   > Ja, kunnen we meerdere verzamelingen als invoer toe te voegen: </br>
   > '*\<DocumentDB verzameling invoernaam 1\>*,*\<DocumentDB verzameling invoernaam 2\>*'</br> De verzamelingsnamen van de worden zonder spaties, met slechts een enkele komma gescheiden. </b>
   >
   >

    Documenten worden gedistribueerde round robin in meerdere verzamelingen. Een batch van documenten worden opgeslagen in één verzameling en vervolgens een tweede reeks documenten worden opgeslagen in de volgende verzameling, enzovoort.

        # Load data from Cosmos DB. Pass the Azure Cosmos DB query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "DocumentDB_timestamps = LOAD '<DocumentDB Endpoint>' USING com.microsoft.azure.documentdb.pig.DocumentDBLoader( " +
                                                        "'<DocumentDB Primary Key>', " +
                                                        "'<DocumentDB Database Name>', " +
                                                        "'<DocumentDB Input Collection Name>', " +
                                                        "'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "
3. Vervolgens laten we de documenten tally door de maand, dag, uur, minuut en het totale aantal exemplaren.

       # GROUP BY minute and COUNT entries for each.
       $queryStringPart2 = "timestamp_record = FOREACH DocumentDB_timestamps GENERATE `$0#'id' as id:int, ToDate((long)(`$0#'ts') * 1000) as timestamp:datetime; " +
                           "by_minute = GROUP timestamp_record BY (GetYear(timestamp), GetMonth(timestamp), GetDay(timestamp), GetHour(timestamp), GetMinute(timestamp)); " +
                           "by_minute_count = FOREACH by_minute GENERATE FLATTEN(group) as (Year:int, Month:int, Day:int, Hour:int, Minute:int), COUNT(timestamp_record) as Total:int; "
4. Stel ten slotte de resultaten opslaan in onze nieuwe uitvoer-verzameling.

   > [!NOTE]
   > Ja, kunnen we meerdere verzamelingen toe te voegen als uitvoer: </br>
   > '\<DocumentDB verzameling uitvoernaam 1\>,\<DocumentDB verzameling uitvoernaam 2\>'</br> De verzamelingsnamen van de worden zonder spaties, met slechts een enkele komma gescheiden.</br>
   > Documenten worden over meerdere verzamelingen gedistribueerde round robin. Een batch van documenten worden opgeslagen in één verzameling en vervolgens een tweede reeks documenten worden opgeslagen in de volgende verzameling, enzovoort.
   >
   >

        # Store output data to Cosmos DB.
        $queryStringPart3 = "STORE by_minute_count INTO '<DocumentDB Endpoint>' " +
                            "USING com.microsoft.azure.documentdb.pig.DocumentDBStorage( " +
                                "'<DocumentDB Primary Key>', " +
                                "'<DocumentDB Database Name>', " +
                                "'<DocumentDB Output Collection Name>'); "
5. Voeg het volgende fragment script voor het maken van de definitie van een Pig-taak uit de vorige query.

        # Create a Pig job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $pigJobDefinition = New-AzureHDInsightPigJobDefinition -Query $queryString -StatusFolder $statusFolder

    U kunt ook gebruiken om op te geven van een scriptbestand Pig op HDFS-schakeloptie in het bestand.
6. Voeg het volgende codefragment voor het opslaan van de begintijd en verzenden van de Pig-taak.

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $pigJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $pigJobDefinition
7. Voeg de volgende wachten tot de Pig-taak te voltooien.

        # Wait for the Pig job to complete.
        Wait-AzureHDInsightJob -Job $pigJob -WaitTimeoutInSeconds 3600
8. Voeg de volgende om de standaarduitvoer en de begin- en eindtijden te drukken.

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $pigJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
9. **Voer** het nieuwe script! **Klik op** de knop groen uitvoeren.
10. Controleer de resultaten. Meld u aan bij de [Azure-Portal][azure-portal].

    1. Klik op <strong>Bladeren</strong> in het paneel aan de linkerkant. </br>
    2. Klik op <strong>Alles</strong> in de rechterbovenhoek van het paneel bladeren. </br>
    3. Zoek en klik op <strong>Azure Cosmos DB Accounts</strong>. </br>
    4. Zoek vervolgens uw <strong>Azure Cosmos-DB Account</strong>, klikt u vervolgens <strong>Azure Cosmos DB Database</strong> en uw <strong>Azure Cosmos DB verzameling</strong> die zijn gekoppeld aan de verzameling uitvoer is opgegeven in uw Pig-query.</br>
    5. Tot slot op <strong>documentverkenner</strong> onder <strong>hulpprogramma's voor ontwikkelaars</strong>.</br></p>

    Hier ziet u de resultaten van uw query Pig.

    ![Pig-queryresultaten][image-pig-query-results]

## <a name="RunMapReduce"></a>Stap 5: Een MapReduce-taak met behulp van Azure DB die Cosmos en HDInsight uitvoeren
1. De volgende variabelen instellen in het deelvenster van de PowerShell-Script.

        $subscriptionName = "<SubscriptionName>"   # Azure subscription name
        $clusterName = "<ClusterName>"             # HDInsight cluster name
2. Er moet een MapReduce-taak die het aantal exemplaren voor elke eigenschap van het Document uit de verzameling van uw Azure Cosmos DB telt uitvoeren. In dit fragment script toevoegen **nadat** het bovenstaande codefragment.

        # Define the MapReduce job.
        $TallyPropertiesJobDefinition = New-AzureHDInsightMapReduceJobDefinition -JarFile "wasb:///example/jars/TallyProperties-v01.jar" -ClassName "TallyProperties" -Arguments "<DocumentDB Endpoint>","<DocumentDB Primary Key>", "<DocumentDB Database Name>","<DocumentDB Input Collection Name>","<DocumentDB Output Collection Name>","<[Optional] DocumentDB Query>"

   > [!NOTE]
   > TallyProperties v01.jar wordt geleverd met de aangepaste installatie van de Cosmos DB Hadoop-Connector.
   >
   >
3. Voeg de volgende opdracht om het verzenden van de MapReduce-taak.

        # Save the start time and submit the job.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $TallyPropertiesJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $TallyPropertiesJobDefinition | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600  

    Naast de taakdefinitie MapReduce, moet u ook de naam van het HDInsight-cluster waarop u wilt uitvoeren van de MapReduce-taak en de referenties opgeven. De begin-AzureHDInsightJob is een aanroep van asynchrone uitvoering. Gebruiken om te controleren op de voltooiing van de taak, de *wacht AzureHDInsightJob* cmdlet.
4. Voeg de volgende opdracht om te controleren of er fouten met de MapReduce-taak uitgevoerd.

        # Get the job output and print the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $TallyPropertiesJob.JobId -StandardError
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
5. **Voer** het nieuwe script! **Klik op** de knop groen uitvoeren.
6. Controleer de resultaten. Meld u aan bij de [Azure-Portal][azure-portal].

   1. Klik op <strong>Bladeren</strong> in het paneel aan de linkerkant.
   2. Klik op <strong>Alles</strong> in de rechterbovenhoek van het paneel bladeren.
   3. Zoek en klik op <strong>Azure Cosmos DB Accounts</strong>.
   4. Zoek vervolgens uw <strong>Azure Cosmos-DB Account</strong>, klikt u vervolgens <strong>Azure Cosmos DB Database</strong> en uw <strong>Azure Cosmos DB verzameling</strong> die zijn gekoppeld aan de verzameling uitvoer is opgegeven in uw MapReduce-taak.
   5. Tot slot op <strong>documentverkenner</strong> onder <strong>hulpprogramma's voor ontwikkelaars</strong>.

      Hier ziet u de resultaten van uw MapReduce-taak.

      ![MapReduce-queryresultaten][image-mapreduce-query-results]

## <a name="NextSteps"></a>De volgende stappen
Gefeliciteerd! U hebt zojuist uw eerste Hive, Pig en MapReduce-taken met behulp van Azure DB die Cosmos en HDInsight uitgevoerd.

We hebben onze Hadoop-Connector die afkomstig zijn geopend. Als u geïnteresseerd bent, u kunt bijdragen op [GitHub][github].

Zie voor meer informatie de volgende artikelen:

* [Een Java-toepassing met Azure Cosmos DB ontwikkelen][sql-api-java-application]
* [Het ontwikkelen van Java-MapReduce-programma's voor Hadoop in HDInsight][hdinsight-develop-deploy-java-mapreduce]
* [Aan de slag met Hadoop Hive in HDInsight analyseren mobiele telefoon gebruiken][hdinsight-get-started]
* [MapReduce gebruiken met HDInsight][hdinsight-use-mapreduce]
* [Hive gebruiken met HDInsight][hdinsight-use-hive]
* [Pig gebruiken met HDInsight][hdinsight-use-pig]
* [HDInsight-clusters met behulp van de scriptactie aanpassen][hdinsight-hadoop-customize-cluster]

[apache-hadoop]: http://hadoop.apache.org/
[apache-hadoop-doc]: http://hadoop.apache.org/docs/current/
[apache-hive]: http://hive.apache.org/
[apache-pig]: http://pig.apache.org/
[getting-started]: sql-api-get-started.md

[azure-portal]: https://portal.azure.com/
[azure-powershell-diagram]: ./media/run-hadoop-with-hdinsight/azurepowershell-diagram-med.png

[hdinsight-samples]: http://portalcontent.blob.core.windows.net/samples/documentdb-hdinsight-samples.zip
[github]: https://github.com/Azure/azure-documentdb-hadoop
[sql-api-java-application]: sql-api-java-application.md
[import-data]: import-data.md

[hdinsight-custom-provision]: ../hdinsight/hdinsight-provision-clusters.md
[hdinsight-develop-deploy-java-mapreduce]:../hdinsight/hadoop/apache-hadoop-develop-deploy-java-mapreduce-linux.md
[hdinsight-hadoop-customize-cluster]: ../hdinsight/hdinsight-hadoop-customize-cluster.md
[hdinsight-get-started]: ../hdinsight/hdinsight-hadoop-tutorial-get-started-windows.md
[hdinsight-storage]: ../hdinsight/hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]:../hdinsight/hadoop/hdinsight-use-hive.md
[hdinsight-use-mapreduce]:../hdinsight/hadoop/hdinsight-use-mapreduce.md
[hdinsight-use-pig]:../hdinsight/hadoop/hdinsight-use-pig.md

[image-customprovision-page1]: ./media/run-hadoop-with-hdinsight/customprovision-page1.png
[image-hive-query-results]: ./media/run-hadoop-with-hdinsight/hivequeryresults.PNG
[image-mapreduce-query-results]: ./media/run-hadoop-with-hdinsight/mapreducequeryresults.PNG
[image-pig-query-results]: ./media/run-hadoop-with-hdinsight/pigqueryresults.PNG

[powershell-install-configure]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0
