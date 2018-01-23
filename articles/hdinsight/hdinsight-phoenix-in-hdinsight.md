---
title: Apache Phoenix in HDInsight - Azure HDInsight | Microsoft Docs
description: 
services: hdinsight
documentationcenter: 
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/19/2018
ms.author: ashishth
ms.openlocfilehash: a82ddc4a94688df87043ef93f24956efb93220c4
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="apache-phoenix-in-hdinsight"></a>Apache Phoenix in HDInsight

[Apache Phoenix](http://phoenix.apache.org/) is een open-source, massively parallelle relationele databaselaag gebouwd op [HBase](hbase/apache-hbase-overview.md). Phoenix kunt u gebruikmaken van SQL-achtige query's over HBase. Phoenix gebruikt JDBC-stuurprogramma's onder om gebruikers te maken, verwijderen, SQL-tabellen, indexen, weergaven en reeksen en upsert rijen afzonderlijk en bulksgewijs veranderen. Phoenix maakt gebruik van systeemeigen noSQL-compilatie in plaats van MapReduce voor het compileren van query's met mogelijkheden voor het maken van toepassingen boven op HBase lage latentie. Phoenix voegt coprocessors ter ondersteuning van de client opgegeven code uitgevoerd in de adresruimte van de server, de code die ditzelfde geplaatst met de gegevens uitvoert. Hierdoor minimaliseert client/server-gegevens worden overgedragen.

Apache Phoenix wordt geopend big data-query's voor niet-ontwikkelaars die de syntaxis van een SQL-achtige kunnen gebruiken in plaats van te programmeren. Phoenix is geoptimaliseerd voor HBase, in tegenstelling tot andere hulpprogramma's zoals [Hive](hadoop/hdinsight-use-hive.md) en Spark SQL. Het voordeel voor ontwikkelaars is maximaal zodat query's met veel minder code te schrijven.
<!-- [Spark SQL](spark/apache-spark-sql-with-hdinsight.md)  -->

Wanneer u een SQL-query verzendt, wordt Phoenix compileert van de query voor systeemeigen HBase-aanroepen en wordt de scan (of het plan) voor optimalisatie parallel uitgevoerd. Deze laag van abstractie, maakt de ontwikkelaar bij het schrijven van MapReduce-taken te richten in plaats daarvan op de zakelijke logica en de werkstroom van de toepassing rond Phoenix van big data-opslag.

## <a name="query-performance-optimization-and-other-features"></a>Optimalisatie van de query-prestaties en andere functies

Apache Phoenix voegt verschillende prestatieverbeteringen en functies op HBase-query's.

### <a name="secondary-indexes"></a>Secundaire indexen

HBase is een enkele index die lexicographically op de primaire rijsleutel is gesorteerd. Deze records kunnen alleen worden benaderd via de rijsleutel van de. Records worden geopend via een van de kolommen dan de rijsleutel vereist scannen van alle gegevens tijdens het toepassen van het filter vereist. In een secundaire index kolommen of expressies die geïndexeerd formulier zijn scant een alternatieve rijsleutel zoekacties als bereik voor deze index.

Maken van een secundaire index met de `CREATE INDEX` opdracht:

```sql
CREATE INDEX ix_purchasetype on SALTEDWEBLOGS (purchasetype, transactiondate) INCLUDE (bookname, quantity);
```

Deze methode kan een aanzienlijke prestatieverbetering worden verkregen via het uitvoeren van query's één geïndexeerd. Dit type secundaire index is een **die betrekking hebben op index**, met alle van de kolommen die zijn opgenomen in de query. Daarom moet het opzoeken van de tabel is niet vereist en de index voldoet aan de query.

### <a name="views"></a>Weergaven

Phoenix weergaven bieden een manier om een HBase-beperking, waar de prestaties verslechteren bij het maken van meer dan ongeveer 100 fysieke tabellen begint ondervangen. Phoenix weergaven kan meerdere *virtuele tabellen* delen één onderliggende fysieke HBase-tabel.

Maken van een weergave Phoenix is vergelijkbaar met het gebruik van standaard SQL-syntaxis voor weergave. Een verschil is dat u kolommen voor de weergave, naast de kolommen die zijn overgenomen van de basistabel definiëren kunt. U kunt ook nieuwe toevoegen `KeyValue` kolommen.

Hier is bijvoorbeeld een fysieke tabel genaamd `product_metrics` met de definitie van de volgende:

```sql
CREATE  TABLE product_metrics (
    metric_type CHAR(1),
    created_by VARCHAR, 
    created_date DATE, 
    metric_id INTEGER
    CONSTRAINT pk PRIMARY KEY (metric_type, created_by, created_date, metric_id));
```

Een weergave definiëren via deze tabel, met aanvullende kolommen:

```sql
CREATE VIEW mobile_product_metrics (carrier VARCHAR, dropped_calls BIGINT) AS
SELECT * FROM product_metrics
WHERE metric_type = 'm';
```

U kunt meer kolommen later toevoegen met de `ALTER VIEW` instructie.

### <a name="skip-scan"></a>Scan overslaan

Een of meer kolommen van een samengestelde index overslaan scan gebruikt afzonderlijke waarden vinden. In tegenstelling tot een scan bereik overslaan scan intra-rij scannen, zodat implementeert [verbeterde prestaties](http://phoenix.apache.org/performance.html#Skip-Scan). Tijdens het scannen, wordt de eerste overeenkomende waarde overgeslagen samen met de index tot de volgende waarde is gevonden.

Maakt gebruik van een scan overslaan de `SEEK_NEXT_USING_HINT` inventarisatie van het HBase-filter. Met behulp van `SEEK_NEXT_USING_HINT`, de scan overslaan van wordt bijgehouden welke set van sleutels of bereiken van sleutels, in elke kolom wordt gezocht. De overslaan scannen en neemt een sleutel die is doorgegeven aan deze tijdens de evaluatie van het filter en wordt vastgesteld of een van de combinaties. Als dat niet het geval is, wordt de scan overslaan evalueert de hoogste toets om naar te gaan.

### <a name="transactions"></a>Transacties

Hoewel HBase rijniveau transacties biedt, Phoenix kan worden geïntegreerd met [Tephra](http://tephra.io/) om toe te voegen cross-rij- en kruistabel transactieondersteuning met volledige [ACID](https://en.wikipedia.org/wiki/ACID) semantiek.

Als met traditionele SQL transacties kunnen transacties die zijn opgegeven door de transactiebeheerder Phoenix u om te controleren of een atomic-eenheid van gegevens is upserted, terugdraaien van de transactie als de upsert-bewerking is mislukt op elke tabel transactie is ingeschakeld.

Om in te schakelen Phoenix transacties, Zie de [Apache Phoenix transactie documentatie](http://phoenix.apache.org/transactions.html).

Als u een nieuwe tabel met transacties die zijn ingeschakeld, stelt de `TRANSACTIONAL` eigenschap `true` in een `CREATE` instructie:

```sql
CREATE TABLE my_table (k BIGINT PRIMARY KEY, v VARCHAR) TRANSACTIONAL=true;
```

Om een bestaande tabel om te worden transactionele wijzigen, gebruikt u dezelfde eigenschap in een `ALTER` instructie:

```sql
ALTER TABLE my_other_table SET TRANSACTIONAL=true;
```

> [!NOTE]
> U kunt een transactionele tabel niet overschakelen terug naar de niet-transactionele wordt.

### <a name="salted-tables"></a>Gezouten tabellen

*Regio server hotspotting* kan zich voordoen bij het schrijven van records met opeenvolgende sleutels naar HBase. Hoewel u meerdere regio-servers in uw cluster hebt, worden uw schrijfbewerkingen op slechts één is optreedt. Dit punt waar maakt het hotspotting probleem waarbij, in plaats van uw schrijven werkbelasting wordt verdeeld over alle beschikbare regio servers, slechts één de belasting verwerkt. Omdat elke regio een vooraf gedefinieerde maximale grootte heeft wanneer een regio die limiet bereikt, is deze opgesplitst in twee kleine gebieden. Wanneer dit gebeurt, wordt een van deze nieuwe gebieden nieuwe records, om de nieuwe hotspot.

Om dit probleem verhelpen en betere prestaties bereiken, vooraf splitsen tabellen zodat alle servers regio gelijkmatig worden gebruikt. Phoenix bevat *tabellen gezouten*, transparant de salting byte toe te voegen aan de rijsleutel voor een bepaalde tabel. De tabel is vooraf op de grenzen van de bytes salt om ervoor te zorgen gelijk verdeling tussen regio servers tijdens de eerste fase van de tabel splitsen. Deze aanpak verdeelt de werkbelasting schrijven over alle beschikbare regio servers verbetering van de voor schrijven en prestaties gelezen. Als u wilt een tabel salt, geef de `SALT_BUCKETS` eigenschap tabel wanneer de tabel is gemaakt:

```sql
CREATE TABLE Saltedweblogs (
    transactionid varchar(500) Primary Key,
    transactiondate Date NULL,
    customerid varchar(50) NULL,
    bookid varchar(50) NULL,
    purchasetype varchar(50) NULL,
    orderid varchar(50) NULL,
    bookname varchar(50) NULL,
    categoryname varchar(50) NULL,
    invoicenumber varchar(50) NULL,
    invoicestatus varchar(50) NULL,
    city varchar(50) NULL,
    state varchar(50) NULL,
    paymentamount DOUBLE NULL,
    quantity INTEGER NULL,
    shippingamount DOUBLE NULL) SALT_BUCKETS=4;
```

## <a name="enable-and-tune-phoenix-with-ambari"></a>Inschakelen en Phoenix met Ambari afstemmen

Een HDInsight-HBase-cluster bevat de [Ambari UI](hdinsight-hadoop-manage-ambari.md) voor het maken van wijzigingen in de configuratie.

1. -Of uitschakelen van Phoenix te bepalen en de Phoenix query time-outinstellingen, meld u aan bij de Ambari-Webgebruikersinterface (`https://YOUR_CLUSTER_NAME.azurehdinsight.net`) met de referenties van uw Hadoop-gebruiker.

2. Selecteer **HBase** uit de lijst van services in het menu links, selecteer de **Configs** tabblad.

    ![Ambari HBase-configuratie](./media/hdinsight-phoenix-in-hdinsight/ambari-hbase-config.png)

3. Zoek de **Phoenix SQL** configuratiesectie inschakelen of uitschakelen van phoenix en de querytime-out ingesteld.

    ![De configuratiesectie Ambari Phoenix SQL](./media/hdinsight-phoenix-in-hdinsight/ambari-phoenix.png)

## <a name="see-also"></a>Zie ook

* [Apache Phoenix gebruiken met HBase op basis van Linux-clusters in HDInsight](hbase/apache-hbase-phoenix-squirrel-linux.md)
