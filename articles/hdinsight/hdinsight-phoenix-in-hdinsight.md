---
title: Apache Phoenix in HDInsight - Azure HDInsight
description: ''
services: hdinsight
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/19/2018
ms.author: ashishth
ms.openlocfilehash: 833f240572b10e9d07da0ded27f5848822a70f46
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53744328"
---
# <a name="apache-phoenix-in-hdinsight"></a>Apache Phoenix in HDInsight

[Apache Phoenix](https://phoenix.apache.org/) is een open source, uiterst parallelle relationele databaselaag die is gebouwd op [Apache HBase](hbase/apache-hbase-overview.md). Phoenix kunt u gebruikmaken van SQL-achtige query's over HBase. Phoenix maakt gebruik van JDBC-stuurprogramma's onder zodat gebruikers kunnen maken, verwijderen, SQL-tabellen, indexen, weergaven en reeksen en upsert rijen afzonderlijk en bulksgewijs wijzigen. Phoenix maakt gebruik van systeemeigen compilatie noSQL in plaats van MapReduce gebruiken voor het compileren van query's, waardoor het maken van toepassingen van de lage latentie op basis van HBase. Phoenix coprocessors ter ondersteuning van de uitvoering van code client wordt geleverd in de adresruimte van de server uitvoeren van de code geplaatst met de gegevens wordt toegevoegd. Deze aanpak minimaliseert de overdracht van client/server-gegevens.

Apache Phoenix opent u big data-query's voor niet-ontwikkelaars die een SQL-achtige syntaxis kunnen gebruiken in plaats van programmeren. Phoenix is geoptimaliseerd voor HBase, in tegenstelling tot andere hulpprogramma's zoals [Apache Hive](hadoop/hdinsight-use-hive.md) en Apache Spark SQL. Het voordeel voor ontwikkelaars is zeer goed presterende query's met veel minder code te schrijven.
<!-- [Spark SQL](spark/apache-spark-sql-with-hdinsight.md)  -->

Wanneer u een SQL-query verzendt, wordt Phoenix compileert de query op HBase systeemeigen aanroepen en de scan (of plan) voor optimalisatie parallel wordt uitgevoerd. Deze laag van abstractie kan de ontwikkelaar van het schrijven van MapReduce-taken, in plaats daarvan richten op de bedrijfslogica en de werkstroom van hun toepassing rond Phoenix van big data-opslag.

## <a name="query-performance-optimization-and-other-features"></a>Optimalisatie van prestaties van query's en andere functies

Apache Phoenix voegt verschillende verbeterde prestaties en functies op HBase-query's.

### <a name="secondary-indexes"></a>Secundaire indexen

HBase is een één-index die is lexicographically gesorteerd op de primaire rijsleutel. Deze records kunnen alleen worden geopend via de rijsleutel. Records openen via een andere kolom dan de rijsleutel moet scannen van alle gegevens die tijdens het toepassen van de vereiste filter. In een secundaire index, de kolommen of expressies die geïndexeerd formulier zijn scant een alternatieve rijsleutel, zodat zoekopdrachten en het bereik van die index.

Maken van een secundaire index met de `CREATE INDEX` opdracht:

```sql
CREATE INDEX ix_purchasetype on SALTEDWEBLOGS (purchasetype, transactiondate) INCLUDE (bookname, quantity);
```

Deze benadering kan een aanzienlijke prestatieverbetering worden verkregen via het uitvoeren van query's één geïndexeerd. Dit type secundaire index is een **die betrekking hebben op index**, met alle van de kolommen die zijn opgenomen in de query. Daarom de zoekopdracht in de tabel is niet vereist en de index voldoet aan de query.

### <a name="views"></a>Weergaven

Phoenix-weergaven bieden een manier om te strijden tegen een HBase-beperking, waar de prestaties verslechteren bij het maken van meer dan ongeveer 100 fysieke tabellen begint. Phoenix weergaven kan meerdere *virtuele tabellen* voor het delen van één onderliggende fysieke HBase-tabel.

Het maken van een weergave Phoenix is vergelijkbaar met het gebruik van de standaard SQL-weergave-syntaxis. Een verschil is dat u de kolommen voor de weergave, naast de kolommen die zijn overgenomen van de basistabel kunt definiëren. U kunt ook nieuwe toevoegen `KeyValue` kolommen.

Dit is bijvoorbeeld een fysieke tabel met de naam `product_metrics` met de definitie van de volgende:

```sql
CREATE  TABLE product_metrics (
    metric_type CHAR(1),
    created_by VARCHAR, 
    created_date DATE, 
    metric_id INTEGER
    CONSTRAINT pk PRIMARY KEY (metric_type, created_by, created_date, metric_id));
```

Een weergave definiëren voor deze tabel, met extra kolommen:

```sql
CREATE VIEW mobile_product_metrics (carrier VARCHAR, dropped_calls BIGINT) AS
SELECT * FROM product_metrics
WHERE metric_type = 'm';
```

U kunt meer kolommen later toevoegen met de `ALTER VIEW` instructie.

### <a name="skip-scan"></a>Scan overslaan

Overslaan scan maakt gebruik van een of meer kolommen van een samengestelde index zoeken naar afzonderlijke waarden. In tegenstelling tot een scan bereik overslaan scan intra-rij scannen, wordt vrijgegeven implementeert [verbeterde prestaties](https://phoenix.apache.org/performance.html#Skip-Scan). Tijdens het zoeken naar, wordt de eerste overeenkomende waarde overgeslagen samen met de index tot de volgende waarde is gevonden.

Maakt gebruik van een scan overslaan de `SEEK_NEXT_USING_HINT` inventarisatie van het HBase-filter. Met behulp van `SEEK_NEXT_USING_HINT`, de scan overslaan van wordt bijgehouden welke set sleutels of bereiken van sleutels, zijn in elke kolom wordt gezocht. De overslaan scannen en heeft een sleutel die is doorgegeven aan deze tijdens de evaluatie van filter en bepaalt of het is een van de combinaties. Als dat niet het geval is, wordt de scan overslaan evalueert de hoogste op een toets om naar te gaan.

### <a name="transactions"></a>Transacties

HBase biedt u beveiliging op rijniveau transacties, Phoenix kan worden geïntegreerd met [Tephra](https://tephra.io/) om toe te voegen cross-rij- en cross-tabel transactieondersteuning met volledige [ACID](https://en.wikipedia.org/wiki/ACID) semantiek.

Als met traditionele SQL-transacties kunnen transacties die worden geleverd via de transactiebeheerder Phoenix u om te controleren of een atomische eenheid van gegevens is upserted, terugdraaien van de transactie als de upsert-bewerking is mislukt op een willekeurige tabel transactie is ingeschakeld.

Zie voor het inschakelen van Phoenix transacties de [Apache Phoenix transactie documentatie](https://phoenix.apache.org/transactions.html).

Als u een nieuwe tabel met transacties die zijn ingeschakeld, stelt u de `TRANSACTIONAL` eigenschap `true` in een `CREATE` instructie:

```sql
CREATE TABLE my_table (k BIGINT PRIMARY KEY, v VARCHAR) TRANSACTIONAL=true;
```

Als u wilt een bestaande tabel om te worden transactionele wijzigen, gebruikt u de dezelfde eigenschap in een `ALTER` instructie:

```sql
ALTER TABLE my_other_table SET TRANSACTIONAL=true;
```

> [!NOTE]  
> U kunt niet een transactionele tabel overstappen terug naar de niet-transactionele wordt.

### <a name="salted-tables"></a>Gezouten tabellen

*Regio server hotspotting* kan zich voordoen bij het schrijven van records met opeenvolgende sleutels naar HBase. Hoewel u meerdere regioservers in uw cluster hebt, worden de schrijfbewerkingen op slechts één is optreden. Deze concentratie maakt het hotspotting probleem waarbij, in plaats van uw write-werkbelasting wordt verdeeld over alle beschikbare regioservers, is slechts een de belasting verwerken. Omdat elke regio een vooraf gedefinieerde maximumgrootte heeft heeft als een regio die maximale grootte bereikt, wordt dit verdeeld in twee kleine gebieden. Wanneer dit gebeurt, neemt een van deze nieuwe regio's alle nieuwe records, worden de nieuwe hotspot.

Dit probleem te verhelpen en betere prestaties, vooraf splitsen tabellen zodat alle regioservers evenredig worden gebruikt. Phoenix bevat *tabellen gezouten*, transparant de salting byte toe te voegen aan de rijsleutel voor een bepaalde tabel. De tabel is vooraf op de salt byte grenzen bevinden om ervoor te zorgen gelijke verdeling van de belasting tussen regioservers tijdens de eerste fase van de tabel splitsen. Deze methode wordt de werkbelasting schrijven verspreid over alle beschikbare regioservers, verbeteren de schrijven en lezen van de prestaties. Als u wilt een tabel salt, geef de `SALT_BUCKETS` eigenschap tabel wanneer de tabel is gemaakt:

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

## <a name="enable-and-tune-phoenix-with-apache-ambari"></a>Inschakelen en af te stemmen Phoenix met Apache Ambari

Een HDInsight HBase-cluster bevat de [Ambari UI](hdinsight-hadoop-manage-ambari.md) voor het aanbrengen van wijzigingen in de configuratie.

1. Phoenix uit te schakelen, en voor het beheren van Phoenix query-time-outinstellingen, meld u aan bij de Ambari-Webgebruikersinterface (`https://YOUR_CLUSTER_NAME.azurehdinsight.net`) met de referenties van uw Hadoop-gebruiker.

2. Selecteer **HBase** uit de lijst met services in het menu links, selecteer de **Peeringconfiguraties** tabblad.

    ![Ambari HBase-configuratie](./media/hdinsight-phoenix-in-hdinsight/ambari-hbase-config.png)

3. Zoek de **Phoenix SQL** configuratiesectie inschakelen of uitschakelen phoenix en stel de time-out van de query.

    ![De configuratiesectie Ambari Phoenix SQL](./media/hdinsight-phoenix-in-hdinsight/ambari-phoenix.png)

## <a name="see-also"></a>Zie ook

* [Apache Phoenix gebruiken met HBase op basis van Linux-clusters in HDInsight](hbase/apache-hbase-phoenix-squirrel-linux.md)
