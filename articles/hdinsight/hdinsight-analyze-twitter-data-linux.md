---
title: Twitter-gegevens met Apache Hive - Azure HDInsight analyseren
description: Leer hoe u Apache Hive en Apache Hadoop op HDInsight gebruikt voor het transformeren van onbewerkte gegevens van TWitter in een doorzoekbare Hive-tabel.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 06/26/2018
ms.author: hrasheed
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 8c7f6695880cfdb0a350edc37d61e771d03b92df
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543722"
---
# <a name="analyze-twitter-data-using-apache-hive-and-apache-hadoop-on-hdinsight"></a>Twitter-gegevens met Apache Hive en Apache Hadoop in HDInsight analyseren

Meer informatie over het gebruik van [Apache Hive](https://hive.apache.org/) verwerken Twitter-gegevens. Het resultaat is een lijst van Twitter-gebruikers die de meeste tweets met een bepaald woord verzonden.

> [!IMPORTANT]  
> De stappen in dit document zijn getest in HDInsight 3.6.

## <a name="get-the-data"></a>De gegevens ophalen

Twitter kunt u de gegevens voor elke tweet ophalen als een document JavaScript Object Notation (JSON) via een REST-API. [OAuth](https://oauth.net) is vereist voor verificatie aan de API.

### <a name="create-a-twitter-application"></a>Een Twitter-toepassing maken

1. Via een webbrowser, moet u zich aanmelden bij [ https://apps.twitter.com/ ](https://apps.twitter.com/). Klik op de **Meld u nu** koppelen als u geen een Twitter-account.

2. Klik op **nieuwe App maken**.

3. Voer **naam**, **beschrijving**, **Website**. U kunt maken van een URL voor de **Website** veld. De volgende tabel ziet u enkele voorbeeldwaarden gebruiken:

   | Veld | Value |
   |:--- |:--- |
   | Name |MyHDInsightApp |
   | Description |MyHDInsightApp |
   | Website |https:\//www.myhdinsightapp.com |

4. Controleer **Ja, ik ga akkoord**, en klik vervolgens op **uw Twitter-toepassing maken**.

5. Klik op de **machtigingen** tabblad. Standaard de machtiging is **alleen-lezen**.

6. Klik op de **Keys and Access Tokens** tabblad.

7. Klik op **maken van mijn toegangstoken**.

8. Klik op **Test OAuth** in de rechterbovenhoek van de pagina.

9. Noteer **consumer key**, **Consumer secret**, **toegangstoken**, en **Access token secret**.

### <a name="download-tweets"></a>Tweets downloaden

De volgende Python-code downloadt 10.000 tweets van Twitter en sla ze naar een bestand met de naam **tweets.txt**.

> [!NOTE]  
> De volgende stappen worden uitgevoerd op het HDInsight-cluster, omdat Python al is geïnstalleerd.

1. Maak verbinding met het HDInsight-cluster via SSH:

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

3. Gebruik de volgende opdrachten voor het installeren van [Tweepy](https://www.tweepy.org/), [voortgangsbalk](https://pypi.python.org/pypi/progressbar/2.2), en andere vereiste pakketten:

   ```bash
   sudo apt install python-dev libffi-dev libssl-dev
   sudo apt remove python-openssl
   pip install virtualenv
   mkdir gettweets
   cd gettweets
   virtualenv gettweets
   source gettweets/bin/activate
   pip install tweepy progressbar pyOpenSSL requests[security]
   ```

4. Gebruik de volgende opdracht om te maken van een bestand met de naam **gettweets.py**:

   ```bash
   nano gettweets.py
   ```

5. Gebruik de volgende tekst als de inhoud van de **gettweets.py** bestand:

   ```python
   #!/usr/bin/python

   from tweepy import Stream, OAuthHandler
   from tweepy.streaming import StreamListener
   from progressbar import ProgressBar, Percentage, Bar
   import json
   import sys

   #Twitter app information
   consumer_secret='Your consumer secret'
   consumer_key='Your consumer key'
   access_token='Your access token'
   access_token_secret='Your access token secret'

   #The number of tweets we want to get
   max_tweets=10000

   #Create the listener class that receives and saves tweets
   class listener(StreamListener):
       #On init, set the counter to zero and create a progress bar
       def __init__(self, api=None):
           self.num_tweets = 0
           self.pbar = ProgressBar(widgets=[Percentage(), Bar()], maxval=max_tweets).start()

       #When data is received, do this
       def on_data(self, data):
           #Append the tweet to the 'tweets.txt' file
           with open('tweets.txt', 'a') as tweet_file:
               tweet_file.write(data)
               #Increment the number of tweets
               self.num_tweets += 1
               #Check to see if we have hit max_tweets and exit if so
               if self.num_tweets >= max_tweets:
                   self.pbar.finish()
                   sys.exit(0)
               else:
                   #increment the progress bar
                   self.pbar.update(self.num_tweets)
           return True

       #Handle any errors that may occur
       def on_error(self, status):
           print status

   #Get the OAuth token
   auth = OAuthHandler(consumer_key, consumer_secret)
   auth.set_access_token(access_token, access_token_secret)
   #Use the listener class for stream processing
   twitterStream = Stream(auth, listener())
   #Filter for these topics
   twitterStream.filter(track=["azure","cloud","hdinsight"])
   ```

    > [!IMPORTANT]  
    > Vervang de tijdelijke aanduiding voor de volgende items met de gegevens uit uw twitter-toepassing:
    >
    > * `consumer_secret`
    > * `consumer_key`
    > * `access_token`
    > * `access_token_secret`

    > [!TIP]  
    > Het filter van de onderwerpen in de laatste regel voor het bijhouden van populaire trefwoorden aanpassen. Met populaire trefwoorden op het moment dat u het script uitvoert beschikt u over snellere vastleggen van gegevens.

6. Gebruik **Ctrl + X**, klikt u vervolgens **Y** op te slaan.

7. Gebruik de volgende opdracht uitvoeren van het bestand en tweets downloaden:

    ```bash
    python gettweets.py
    ```

    Er verschijnt een voortgangsindicator. Deze telt tot 100% als de tweets worden gedownload.

   > [!NOTE]  
   > Als het duurt lang voordat de voortgangsbalk om door te gaan, moet u het filter voor het volgen van actuele onderwerpen op wijzigen. Wanneer er veel tweets over een onderwerp in het filter, krijgt u snel de 10000 tweets die nodig zijn.

### <a name="upload-the-data"></a>De gegevens uploaden

Als u wilt de gegevens uploaden naar HDInsight-opslag, gebruik de volgende opdrachten:

```bash
hdfs dfs -mkdir -p /tutorials/twitter/data
hdfs dfs -put tweets.txt /tutorials/twitter/data/tweets.txt
```

Deze opdrachten worden de gegevens opslaan op een locatie die toegankelijk is voor alle knooppunten in het cluster.

## <a name="run-the-hiveql-job"></a>Voer de taak HiveQL

1. Gebruik de volgende opdracht om te maken van een bestand met [HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual) instructies:

   ```bash
   nano twitter.hql
   ```

    Gebruik de volgende tekst als de inhoud van het bestand:

   ```hiveql
   set hive.exec.dynamic.partition = true;
   set hive.exec.dynamic.partition.mode = nonstrict;
   -- Drop table, if it exists
   DROP TABLE tweets_raw;
   -- Create it, pointing toward the tweets logged from Twitter
   CREATE EXTERNAL TABLE tweets_raw (
       json_response STRING
   )
   STORED AS TEXTFILE LOCATION '/tutorials/twitter/data';
   -- Drop and recreate the destination table
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
   -- Select tweets from the imported data, parse the JSON,
   -- and insert into the tweets table
   FROM tweets_raw
   INSERT OVERWRITE TABLE tweets
   SELECT
       cast(get_json_object(json_response, '$.id_str') as BIGINT),
       get_json_object(json_response, '$.created_at'),
       concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
       substr (get_json_object(json_response, '$.created_at'),27,4)),
       substr (get_json_object(json_response, '$.created_at'),27,4),
       case substr (get_json_object(json_response,    '$.created_at'),5,3)
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
   ```

2. Druk op **Ctrl + X**, drukt u vervolgens op **Y** op te slaan.
3. Gebruik de volgende opdracht om uit te voeren van de HiveQL opgenomen in het bestand:

   ```bash
   beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http' -i twitter.hql
   ```

    Met deze opdracht wordt uitgevoerd de **twitter.hql** bestand. Nadat de query is voltooid, ziet u een `jdbc:hive2//localhost:10001/>` prompt.

4. Gebruik de volgende query om te controleren dat de gegevens zijn geïmporteerd uit de beeline-prompt:

   ```hiveql
   SELECT name, screen_name, count(1) as cc
   FROM tweets
   WHERE text like "%Azure%"
   GROUP BY name,screen_name
   ORDER BY cc DESC LIMIT 10;
   ```

    Deze query retourneert maximaal 10 tweets waarin het woord bevatten **Azure** in de berichttekst.

    > [!NOTE]  
    > Als u het filter in gewijzigd de `gettweets.py` script, Vervang **Azure** met een van de filters die u hebt gebruikt.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe een niet-gestructureerde JSON-gegevensset omzetten in een gestructureerde [Apache Hive](https://hive.apache.org/) tabel. Zie de volgende documenten voor meer informatie over Hive in HDInsight:

* [Aan de slag met HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [HDInsight met gegevens van vertragingen van vluchten analyseren](/azure/hdinsight/interactive-query/interactive-query-tutorial-analyze-flight-data)

[curl]: https://curl.haxx.se
[curl-download]: https://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter
