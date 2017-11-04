---
title: Twitter gegevens analyseren met Hadoop in HDInsight - Azure | Microsoft Docs
description: Informatie over het gebruik van Hive om gegevens met Hadoop in HDInsight vinden van de frequentie van de informatie over het gebruik van een bepaald woord Twitter te analyseren.
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 78e4ea33-9714-424d-ac07-3d60ecaebf2e
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: a5f97dfa084291cefde9bf27b5639926de1bc80e
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Twitter-gegevens met Hive in HDInsight analyseren
Sociale websites zijn een van de belangrijke drijvende kracht voor big data acceptatie. Openbare API's die worden geleverd door sites zoals Twitter zijn nuttig gegevensbron voor het analyseren en kennis van populaire trends.
In deze zelfstudie wordt u tweets verkrijgen door middel van een streaming-API Twitter en gebruik vervolgens Apache Hive in Azure HDInsight om een lijst met Twitter-gebruikers die de meeste tweets die deel uitmaakt van een bepaald woord verzonden.

> [!IMPORTANT]
> De stappen in dit document moet een HDInsight op basis van Windows-cluster. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie. Zie voor specifieke stappen voor een cluster op basis van Linux, [analyseren Twitter-gegevens met Hive in HDInsight (Linux)](hdinsight-analyze-twitter-data-linux.md).

## <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u het volgende hebben of hebben gedaan:

* **Een werkstation** met Azure PowerShell installeren en configureren.

    Voor het uitvoeren van Windows PowerShell-scripts, moet u Azure PowerShell als administrator uitvoeren en het uitvoeringsbeleid instellen op *RemoteSigned*. Zie [Run Windows PowerShell-scripts][powershell-script].

    Voordat u Windows PowerShell-scripts uitvoert, zorg ervoor dat u bent verbonden met uw Azure-abonnement met behulp van de volgende cmdlet:

    ```powershell
    Login-AzureRmAccount
    ```

    Als u meerdere Azure-abonnementen hebt, gebruikt u de volgende cmdlet instellen van het huidige abonnement:

    ```powershell
    Select-AzureRmSubscription -SubscriptionID <Azure Subscription ID>
    ```

    > [!IMPORTANT]
    > Azure PowerShell-ondersteuning voor het beheer van HDInsight-resources met behulp van Azure Service Manager is **afgeschaft** en per 1 januari 2017 verdwenen. In de stappen in dit document worden de nieuwe HDInsight-cmdlets gebruikt die met Azure Resource Manager werken.
    >
    > Volg de stappen in [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) (Azure PowerShell installeren en configureren) als u de nieuwste versie van Azure PowerShell wilt installeren. Als u scripts hebt die moeten worden gewijzigd om ze te kunnen gebruiken met de nieuwe cmdlets die werken met Azure Resource Manager, raadpleegt u [Migrating to Azure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) (Migreren naar op Azure Resource Manager gebaseerde hulpprogramma’s voor HDInsight-clusters) voor meer informatie.

* **Een Azure HDInsight-cluster**. Zie voor instructies over het inrichten van het cluster [aan de slag met HDInsight] [ hdinsight-get-started] of [HDInsight-clusters inrichten][hdinsight-provision]. Naam van het cluster moet u later in de zelfstudie.

De volgende tabel bevat de bestanden in deze zelfstudie gebruikt:

| Bestanden | Beschrijving |
| --- | --- |
| /tutorials/Twitter/Data/tweets.txt |De brongegevens voor de Hive-taak. |
| /tutorials/Twitter/output |De uitvoermap voor het Hive-taak. Het Hive-taak uitvoer standaardbestandsnaam is **000000_0**. |
| tutorials/Twitter/Twitter.hql |Het scriptbestand HiveQL. |
| /tutorials/Twitter/JobStatus |De status van de Hadoop-taak. |

## <a name="get-twitter-feed"></a>Get Twitter-feeds
In deze zelfstudie gebruikt u de [Twitter streaming-API's][twitter-streaming-api]. De specifieke Twitter streaming-API die u wilt gebruiken is [statussen-/ filter][twitter-statuses-filter].

> [!NOTE]
> Een bestand met 10.000 tweets en het Hive-scriptbestand (behandeld in de volgende sectie) zijn geüpload in een openbare Blob-container. Als u wilt de geüploade bestanden gebruiken, kunt u deze sectie overslaan.

[Tweets gegevens](https://dev.twitter.com/docs/platform-objects/tweets) wordt opgeslagen in de notatie JSON (JavaScript Object)-indeling die een complexe geneste structuur bevat. In plaats van een groot aantal regels code schrijven met behulp van een conventionele programmeertaal, kunt u deze geneste structuur in een Hive-tabel transformeren zodat deze kan worden doorzocht door een Structured Query Language (SQL)-taal genaamd HiveQL, zoals.

Twitter maakt gebruik van OAuth voor geautoriseerde toegang tot de API. OAuth is een authenticatieprotocol dat Hiermee gebruikers toepassingen kunnen te handelen namens hen zonder het delen van hun wachtwoord goedkeuren. Meer informatie kunt vinden op [oauth.net](http://oauth.net/) of in de uitstekende [basisinformatie over OAuth](http://hueniverse.com/oauth/) van Hueniverse.

De eerste stap bij het gebruik van OAuth is een nieuwe toepassing maken op de site Developer Twitter.

**Een Twitter-toepassing maken**

1. Aanmelden bij [https://apps.twitter.com/](https://apps.twitter.com/). Klik op de **nu aanmelden** koppelen als u een Twitter-account niet hebt.
2. Klik op **nieuwe App maken**.
3. Voer **naam**, **beschrijving**, **Website**. U kunt maken van een URL op voor de **Website** veld. De volgende tabel ziet u enkele voorbeeldwaarden te gebruiken:

   | Veld | Waarde |
   | --- | --- |
   |  Naam |MyHDInsightApp |
   |  Beschrijving |MyHDInsightApp |
   |  Website |http://www.myhdinsightapp.com |
4. Controleer **Ja, ik ga akkoord**, en klik vervolgens op **uw Twitter-toepassing maken**.
5. Klik op de **machtigingen** tabblad. Standaard de machtiging is **alleen-lezen**. Dit is voldoende voor deze zelfstudie.
6. Klik op de **sleutels en toegangstokens** tabblad.
7. Klik op **maken van mijn toegangstoken**.
8. Klik op **Test OAuth** in de rechterbovenhoek van de pagina.
9. Noteer **consumentsleutel**, **consumentgeheim**, **toegangstoken**, en **Access token geheim**. Verderop in de zelfstudie moet u de waarden.

In deze zelfstudie kunt u Windows PowerShell gebruiken om de webservice aanroepen. Het populaire hulpprogramma web service aanroepen is [ *Curl*][curl]. CURL kan worden gedownload vanaf [hier][curl-download].

> [!NOTE]
> Als u de curl-opdracht in Windows gebruikt, kunt u dubbele aanhalingstekens in plaats van enkele aanhalingstekens voor de optiewaarden.

**Tweets ophalen**

1. Open Windows PowerShell Integrated Scripting Environment (ISE). (Typ op het Windows 8 startscherm **PowerShell_ISE** en klik vervolgens op **Windows PowerShell ISE**. Zie [Start Windows PowerShell in Windows 8 en Windows][powershell-start].)
2. Het volgende script kopiëren naar het deelvenster script:

    ```powershell
    #region - variables and constants
    $clusterName = "<HDInsightClusterName>" # Enter the HDInsight cluster name

    # Enter the OAuth information for your Twitter application
    $oauth_consumer_key = "<TwitterAppConsumerKey>";
    $oauth_consumer_secret = "<TwitterAppConsumerSecret>";
    $oauth_token = "<TwitterAppAccessToken>";
    $oauth_token_secret = "<TwitterAppAccessTokenSecret>";

    $destBlobName = "tutorials/twitter/data/tweets.txt" # This script saves the tweets into this blob.

    $trackString = "Azure, Cloud, HDInsight" # This script gets the tweets containing these keywords.
    $track = [System.Uri]::EscapeDataString($trackString);
    $lineMax = 10000  # The script will get this number of tweets. It is about 3 minutes every 100 lines.
    #endregion

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    Login-AzureRmAccount
    #endregion

    #region - Create a block blob object for writing tweets into Blob storage
    Write-Host "Get the default storage account name and Blob container name using the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -Name $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $storageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $containerName = $myCluster.DefaultStorageContainer
    Write-Host "`tThe storage account name is $storageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $containerName." -ForegroundColor Yellow

    Write-Host "Define the Azure storage connection string ..." -ForegroundColor Green
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$storageAccountName;AccountKey=$storageAccountKey"
    Write-Host "`tThe connection string is $storageConnectionString." -ForegroundColor Yellow

    Write-Host "Create block blob object ..." -ForegroundColor Green
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($containerName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
    #end region

    # region - Format OAuth strings
    Write-Host "Format oauth strings ..." -ForegroundColor Green
    $oauth_nonce = [System.Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes([System.DateTime]::Now.Ticks.ToString()));
    $ts = [System.DateTime]::UtcNow - [System.DateTime]::ParseExact("01/01/1970", "dd/MM/yyyy", $null)
    $oauth_timestamp = [System.Convert]::ToInt64($ts.TotalSeconds).ToString();

    $signature = "POST&";
    $signature += [System.Uri]::EscapeDataString("https://stream.twitter.com/1.1/statuses/filter.json") + "&";
    $signature += [System.Uri]::EscapeDataString("oauth_consumer_key=" + $oauth_consumer_key + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_nonce=" + $oauth_nonce + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_signature_method=HMAC-SHA1&");
    $signature += [System.Uri]::EscapeDataString("oauth_timestamp=" + $oauth_timestamp + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_token=" + $oauth_token + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_version=1.0&");
    $signature += [System.Uri]::EscapeDataString("track=" + $track);

    $signature_key = [System.Uri]::EscapeDataString($oauth_consumer_secret) + "&" + [System.Uri]::EscapeDataString($oauth_token_secret);

    $hmacsha1 = new-object System.Security.Cryptography.HMACSHA1;
    $hmacsha1.Key = [System.Text.Encoding]::ASCII.GetBytes($signature_key);
    $oauth_signature = [System.Convert]::ToBase64String($hmacsha1.ComputeHash([System.Text.Encoding]::ASCII.GetBytes($signature)));

    $oauth_authorization = 'OAuth ';
    $oauth_authorization += 'oauth_consumer_key="' + [System.Uri]::EscapeDataString($oauth_consumer_key) + '",';
    $oauth_authorization += 'oauth_nonce="' + [System.Uri]::EscapeDataString($oauth_nonce) + '",';
    $oauth_authorization += 'oauth_signature="' + [System.Uri]::EscapeDataString($oauth_signature) + '",';
    $oauth_authorization += 'oauth_signature_method="HMAC-SHA1",'
    $oauth_authorization += 'oauth_timestamp="' + [System.Uri]::EscapeDataString($oauth_timestamp) + '",'
    $oauth_authorization += 'oauth_token="' + [System.Uri]::EscapeDataString($oauth_token) + '",';
    $oauth_authorization += 'oauth_version="1.0"';

    $post_body = [System.Text.Encoding]::ASCII.GetBytes("track=" + $track);
    #endregion

    #region - Read tweets
    Write-Host "Create HTTP web request ..." -ForegroundColor Green
    [System.Net.HttpWebRequest] $request = [System.Net.WebRequest]::Create("https://stream.twitter.com/1.1/statuses/filter.json");
    $request.Method = "POST";
    $request.Headers.Add("Authorization", $oauth_authorization);
    $request.ContentType = "application/x-www-form-urlencoded";
    $body = $request.GetRequestStream();

    $body.write($post_body, 0, $post_body.length);
    $body.flush();
    $body.close();
    $response = $request.GetResponse() ;

    Write-Host "Start stream reading ..." -ForegroundColor Green

    Write-Host "Define a MemoryStream and a StreamWriter for writing ..." -ForegroundColor Green
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream

    $sReader = New-Object System.IO.StreamReader($response.GetResponseStream())

    $inrec = $sReader.ReadLine()
    $count = 0
    while (($inrec -ne $null) -and ($count -le $lineMax))
    {
        if ($inrec -ne "")
        {
            Write-Host "`n`t $count tweets received." -ForegroundColor Yellow

            $writeStream.WriteLine($inrec)
            $count ++
        }

        $inrec=$sReader.ReadLine()
    }
    #endregion

    #region - Write tweets to Blob storage
    Write-Host "Write to the destination blob ..." -ForegroundColor Green
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)

    $sReader.close()
    #endregion

    Write-Host "Completed!" -ForegroundColor Green
    ```

3. Stel de eerste vijf tot acht variabelen in het script:

    Variabele|Beschrijving
    ---|---
    $clusterName|Dit is de naam van het HDInsight-cluster waar u de toepassing uitvoeren.
    $oauth_consumer_key|Dit is de toepassing Twitter **consumentsleutel** u opgeschreven eerder wanneer u de Twitter-toepassing gemaakt.
    $oauth_consumer_secret|Dit is de toepassing Twitter **consumentgeheim** u eerder hebt genoteerd.
    $oauth_token|Dit is de toepassing Twitter **toegangstoken** u eerder hebt genoteerd.
    $oauth_token_secret|Dit is de toepassing Twitter **access token geheim** u eerder hebt genoteerd.
    $destBlobName|Dit is de uitvoer-blob-naam. De standaardwaarde is **tutorials/twitter/data/tweets.txt**. Als u de standaardwaarde wijzigt, moet u de Windows PowerShell-scripts worden dienovereenkomstig bijgewerkt.
    $trackString|De webservice wordt tweets die betrekking hebben op deze trefwoorden geretourneerd. De standaardwaarde is **Azure, Cloud, HDInsight**. Als u de standaardwaarde wijzigt, wordt u de Windows PowerShell-scripts dienovereenkomstig bijgewerkt.
    $lineMax|De waarde bepaalt hoeveel tweets het script wordt gelezen. Het duurt ongeveer drie minuten 100 tweets lezen. U kunt een groter aantal instellen, maar duurt het meer tijd om te downloaden.

1. Druk op **F5** om het script uit te voeren. Als u problemen als tijdelijke oplossing ondervindt alle regels selecteren en druk vervolgens op **F8**.
2. U ziet 'Voltooi!' aan het einde van de uitvoer. Eventuele foutberichten wordt rood weergegeven.

U kunt het uitvoerbestand controleren als de validatieprocedure **/tutorials/twitter/data/tweets.txt**, op de Azure Blob-opslag met behulp van een Azure storage explorer of Azure PowerShell. Zie voor een Windows PowerShell-voorbeeldscript voor het weergeven van bestanden, [gebruik Blob storage met HDInsight][hdinsight-storage-powershell].

## <a name="create-hiveql-script"></a>HiveQL-script maken
Met Azure PowerShell, kunt u meerdere HiveQL-instructies een tegelijk uitvoeren of de instructie van HiveQL in een scriptbestand van het pakket. In deze zelfstudie maakt u een HiveQL-script. Het scriptbestand moet worden geüpload naar Azure Blob-opslag. In de volgende sectie stelt uitvoeren u het scriptbestand met behulp van Azure PowerShell.

> [!NOTE]
> Het Hive-scriptbestand en een bestand met 10.000 tweets zijn geüpload in een openbare Blob-container. Als u wilt de geüploade bestanden gebruiken, kunt u deze sectie overslaan.

Het HiveQL-script wordt het volgende doen:

1. **Verwijderen van de tabel tweets_raw** als de tabel al bestaat.
2. **De tweets_raw Hive-tabel maken**. Deze tijdelijke gestructureerde Hive-tabel bevat de gegevens voor verdere uitpakken, transformeren en laden (ETL) verwerking. Zie voor informatie over partities, [Hive-zelfstudie][apache-hive-tutorial].
3. **Gegevens laden** van de bronmap, /tutorials/twitter/data. De gegevensset grote tweets in geneste JSON-indeling heeft nu is omgezet in de structuur van een tijdelijke Hive-tabel.
4. **Verwijderen van de tabel tweets** als de tabel al bestaat.
5. **Maken van de tabel tweets**. Voordat u kunt een query op basis van de gegevensset tweets met Hive, moet u een ander ETL-proces worden uitgevoerd. Dit proces ETL definieert een meer gedetailleerde tabelschema voor de gegevens die u hebt opgeslagen in de tabel 'twitter_raw'.
6. **Overschrijven tabel invoegen**. Dit complexe Hive-script wordt een reeks lang MapReduce-taken starten door het Hadoop-cluster. Dit kan ongeveer 10 minuten duren, afhankelijk van uw gegevensset en de grootte van het cluster.
7. **INSERT map overschrijven**. Een query uitvoert en de uitvoer van de gegevensset naar een bestand. Deze query retourneert een lijst met Twitter gebruikers afzender meeste tweets of het woord 'Azure'.

**Een Hive-script maken en uploaden naar Azure**

1. Open Windows PowerShell ISE.
2. Het volgende script kopiëren naar het deelvenster script:

    ```powershell
    #region - variables and constants
    $clusterName = "<Existing HDInsight Cluster Name>" # Enter your HDInsight cluster name
    $subscriptionID = "<Azure Subscription ID>"

    $sourceDataPath = "/tutorials/twitter/data"
    $outputPath = "/tutorials/twitter/output"
    $hqlScriptFile = "tutorials/twitter/twitter.hql"

    $hqlStatements = @"
    set hive.exec.dynamic.partition = true;
    set hive.exec.dynamic.partition.mode = nonstrict;

    DROP TABLE tweets_raw;
    CREATE EXTERNAL TABLE tweets_raw (
        json_response STRING
    )
    STORED AS TEXTFILE LOCATION '$sourceDataPath';

    DROP TABLE tweets;
    CREATE TABLE tweets
    (
        id BIGINT,
        created_at STRING,
        created_at_date STRING,
        created_at_year STRING,
        created_at_month STRING,
        created_at_day STRING,
        created_at_time STRING,
        in_reply_to_user_id_str STRING,
        text STRING,
        contributors STRING,
        retweeted STRING,
        truncated STRING,
        coordinates STRING,
        source STRING,
        retweet_count INT,
        url STRING,
        hashtags array<STRING>,
        user_mentions array<STRING>,
        first_hashtag STRING,
        first_user_mention STRING,
        screen_name STRING,
        name STRING,
        followers_count INT,
        listed_count INT,
        friends_count INT,
        lang STRING,
        user_location STRING,
        time_zone STRING,
        profile_image_url STRING,
        json_response STRING
    );

    FROM tweets_raw
    INSERT OVERWRITE TABLE tweets
    SELECT
        cast(get_json_object(json_response, '$.id_str') as BIGINT),
        get_json_object(json_response, '$.created_at'),
        concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
        substr (get_json_object(json_response, '$.created_at'),27,4)),
        substr (get_json_object(json_response, '$.created_at'),27,4),
        case substr (get_json_object(json_response, '$.created_at'),5,3)
            when "Jan" then "01"
            when "Feb" then "02"
            when "Mar" then "03"
            when "Apr" then "04"
            when "May" then "05"
            when "Jun" then "06"
            when "Jul" then "07"
            when "Aug" then "08"
            when "Sep" then "09"
            when "Oct" then "10"
            when "Nov" then "11"
            when "Dec" then "12" end,
        substr (get_json_object(json_response, '$.created_at'),9,2),
        substr (get_json_object(json_response, '$.created_at'),12,8),
        get_json_object(json_response, '$.in_reply_to_user_id_str'),
        get_json_object(json_response, '$.text'),
        get_json_object(json_response, '$.contributors'),
        get_json_object(json_response, '$.retweeted'),
        get_json_object(json_response, '$.truncated'),
        get_json_object(json_response, '$.coordinates'),
        get_json_object(json_response, '$.source'),
        cast (get_json_object(json_response, '$.retweet_count') as INT),
        get_json_object(json_response, '$.entities.display_url'),
        array(
            trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[1].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[2].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[3].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[4].text')))),
        array(
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[1].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[2].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[3].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[4].screen_name')))),
        trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
        trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
        get_json_object(json_response, '$.user.screen_name'),
        get_json_object(json_response, '$.user.name'),
        cast (get_json_object(json_response, '$.user.followers_count') as INT),
        cast (get_json_object(json_response, '$.user.listed_count') as INT),
        cast (get_json_object(json_response, '$.user.friends_count') as INT),
        get_json_object(json_response, '$.user.lang'),
        get_json_object(json_response, '$.user.location'),
        get_json_object(json_response, '$.user.time_zone'),
        get_json_object(json_response, '$.user.profile_image_url'),
        json_response
    WHERE (length(json_response) > 500);

    INSERT OVERWRITE DIRECTORY '$outputPath'
    SELECT name, screen_name, count(1) as cc
        FROM tweets
        WHERE text like "%Azure%"
        GROUP BY name,screen_name
        ORDER BY cc DESC LIMIT 10;
    "@
    #endregion

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green

    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }

    Select-AzureRmSubscription -SubscriptionId $subscriptionID

    #endregion

    #region - Create a block blob object for writing the Hive script file
    Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow

    Write-Host "Define the connection string ..." -ForegroundColor Green
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"

    Write-Host "Create block blob objects referencing the hql script file" -ForegroundColor Green
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $hqlScriptBlob = $storageContainer.GetBlockBlobReference($hqlScriptFile)

    Write-Host "Define a MemoryStream and a StreamWriter for writing ... " -ForegroundColor Green
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream
    $writeStream.Writeline($hqlStatements)
    #endregion

    #region - Write the Hive script file to Blob storage
    Write-Host "Write to the destination blob ... " -ForegroundColor Green
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $hqlScriptBlob.UploadFromStream($memStream)
    #endregion

    Write-Host "Completed!" -ForegroundColor Green
    ```

3. Stel de eerste twee variabelen in het script:

   | Variabele | Beschrijving |
   | --- | --- |
   |  $clusterName |Voer de naam van het HDInsight-cluster waar u de toepassing uitvoeren. |
   |  $subscriptionID |Voer uw Azure-abonnement-ID. |
   |  $sourceDataPath |De Azure Blob storage locatie waar u het Hive-query's de gegevens van leest. U hoeft niet te wijzigen van deze variabele. |
   |  $outputPath |De Azure Blob storage locatie waar de Hive-query wordt de resultaten. U hoeft niet te wijzigen van deze variabele. |
   |  $hqlScriptFile |De locatie en de bestandsnaam van het scriptbestand HiveQL. U hoeft niet te wijzigen van deze variabele. |
4. Druk op **F5** om het script uit te voeren. Als u problemen als tijdelijke oplossing ondervindt alle regels selecteren en druk vervolgens op **F8**.
5. U ziet 'Voltooi!' aan het einde van de uitvoer. Eventuele foutberichten wordt rood weergegeven.

U kunt het uitvoerbestand controleren als de validatieprocedure **/tutorials/twitter/twitter.hql**, op de Azure Blob-opslag met behulp van een Azure storage explorer of Azure PowerShell. Zie voor een Windows PowerShell-voorbeeldscript voor het weergeven van bestanden, [gebruik Blob storage met HDInsight][hdinsight-storage-powershell].

## <a name="process-twitter-data-by-using-hive"></a>Twitter-gegevens verwerken met behulp van Hive
U hebt al het werk voorbereiding. U kunt nu het Hive-script aanroepen en de resultaten controleren.

### <a name="submit-a-hive-job"></a>Verzenden van een Hive-taak
Gebruik de volgende Windows PowerShell-script de Hive-script uit te voeren. U moet de eerste variabele ingesteld.

> [!NOTE]
> Instellen als u de tweets en het HiveQL-script dat u hebt geüpload in de laatste twee secties, $hqlScriptFile op ' / tutorials/twitter/twitter.hql '. Ingesteld voor het gebruik van de waarden die voor u zijn geüpload naar een openbare blob $hqlScriptFile op 'wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql'.

```powershell
#region variables and constants
$clusterName = "<Existing Azure HDInsight Cluster Name>"
$httpUserName = "admin"
$httpUserPassword = "<HDInsight Cluster HTTP User Password>"

#use one of the following
$hqlScriptFile = "wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql"
$hqlScriptFile = "/tutorials/twitter/twitter.hql"

$statusFolder = "/tutorials/twitter/jobstatus"
#endregion

$myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $myCluster.ResourceGroup
$defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
$defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value

$defaultBlobContainerName = $myCluster.DefaultStorageContainer

#region - Invoke Hive
Write-Host "Invoke Hive ... " -ForegroundColor Green

# Create the HDInsight cluster
$pw = ConvertTo-SecureString -String $httpUserPassword -AsPlainText -Force
$httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

Use-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName -HttpCredential $httpCredential
$response = Invoke-AzureRmHDInsightHiveJob -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -file $hqlScriptFile -StatusFolder $statusFolder #-OutVariable $outVariable

Write-Host "Display the standard error log ... " -ForegroundColor Green
$jobID = ($response | Select-String job_ | Select-Object -First 1) -replace ‘\s*$’ -replace ‘.*\s’
Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $jobID -DefaultContainer $defaultBlobContainerName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -HttpCredential $httpCredential
#endregion
```

### <a name="check-the-results"></a>De resultaten controleren
De volgende Windows PowerShell-script gebruiken om te controleren van de uitvoer van de Hive-taak. U moet de eerste twee variabelen worden ingesteld.

```powershell
#region variables and constants
$clusterName = "<Existing Azure HDInsight Cluster Name>"

$blob = "tutorials/twitter/output/000000_0" # The name of the blob to be downloaded.
#endregion

#region - Create an Azure storage context object
Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
$myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $myCluster.ResourceGroup
$defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
$defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
$defaultBlobContainerName = $myCluster.DefaultStorageContainer

Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow

Write-Host "Create a context object ... " -ForegroundColor Green
$storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
#endregion

#region - Download blob and display blob
Write-Host "Download the blob ..." -ForegroundColor Green
cd $HOME
Get-AzureStorageBlobContent -Container $defaultBlobContainerName -Blob $blob -Context $storageContext -Force

Write-Host "Display the output ..." -ForegroundColor Green
Write-Host "==================================" -ForegroundColor Green
cat "./$blob"
Write-Host "==================================" -ForegroundColor Green
#end region
```

> [!NOTE]
> De Hive-tabel gebruikt \001 als het veldscheidingsteken. Het scheidingsteken is niet zichtbaar in de uitvoer.

Nadat de resultaten van de analyse in Azure Blob-opslag zijn geplaatst, kunt u de gegevens exporteren naar een Azure SQL database/SQL-server, de gegevens exporteren naar Excel via Power Query of verbinding maken met uw toepassing uit om de gegevens met behulp van het Hive ODBC-stuurprogramma. Zie voor meer informatie [Sqoop gebruiken met HDInsight][hdinsight-use-sqoop], [vertraging vluchtgegevens met HDInsight analyseren][hdinsight-analyze-flight-delay-data], [Excel verbinding naar HDInsight met Power Query][hdinsight-power-query], en [Excel verbinding naar HDInsight met het stuurprogramma Microsoft Hive ODBC][hdinsight-hive-odbc].

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebben we gezien hoe u een niet-gestructureerde JSON-gegevensset transformeren naar een gestructureerde Hive-tabel om te zoeken, verkennen en analyseren van gegevens van Twitter met behulp van HDInsight op Azure. Voor meer informatie zie:

* [Aan de slag met HDInsight][hdinsight-get-started]
* [Vertraging vluchtgegevens met HDInsight analyseren][hdinsight-analyze-flight-delay-data]
* [Excel verbinden met HDInsight met Power Query][hdinsight-power-query]
* [Excel verbinden met HDInsight met het Microsoft Hive ODBC-stuurprogramma][hdinsight-hive-odbc]
* [Sqoop gebruiken met HDInsight][hdinsight-use-sqoop]

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176961.aspx

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]:hdinsight-hadoop-use-blob-storage.md#access-blobs-using-azure-powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]:hadoop/hdinsight-use-sqoop.md
[hdinsight-power-query]:hadoop/apache-hadoop-connect-excel-power-query.md
[hdinsight-hive-odbc]:hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md
