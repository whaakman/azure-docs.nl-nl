---
title: Voorbeeldgegevens in Azure HDInsight Hive-tabellen | Microsoft Docs
description: Omlaag steekproef nemen van gegevens in Azure HDInsight (Hadopop) Hive-tabellen
services: machine-learning,hdinsight
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: f31e8d01-0fd4-4a10-b1a7-35de3c327521
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: bradsev
ms.openlocfilehash: d765c2adc8a3aa77d903490875c7f8ad622ef4d2
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2017
---
# <a name="sample-data-in-azure-hdinsight-hive-tables"></a>Voorbeeldgegevens in Hive-tabellen in Azure HDInsight
In dit artikel wordt beschreven hoe gegevens die zijn opgeslagen in Azure HDInsight Hive-tabellen met Hive-query's verkleinen tot een grootte beter kan worden beheerd voor analyse down-voorbeeld. Deze drie methoden voor dit gebruikte steekproeven te omvatten:

* Uniform steekproeven
* Steekproeven door groepen
* Toepassing stratificatie steekproeven

De volgende **menu** koppelingen naar onderwerpen waarin wordt beschreven hoe u voorbeeldgegevens uit verschillende omgevingen voor opslag.

[!INCLUDE [cap-sample-data-selector](../../../includes/cap-sample-data-selector.md)]

**Waarom een steekproef nemen uit uw gegevens?**
Als de gegevensset die u wilt analyseren groot is, is het meestal een goed idee om de gegevens om deze aan de grootte van een kleinere maar representatief en gemakkelijker down-voorbeeld. Omlaag steekproeven vergemakkelijkt gegevens understanding exploratie en functie-engineering. De rol in het Team gegevens wetenschap proces is om in te schakelen snel maken van een prototype van de functies voor het verwerken van gegevens en machine learning-modellen.

Deze taak steekproeven is een stap in de [Team gegevens wetenschap proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="how-to-submit-hive-queries"></a>Het indienen van Hive-query 's
Hive-query's kunnen worden verzonden vanaf de opdrachtregel Hadoop-console op het hoofdknooppunt van het Hadoop-cluster. Om dit te doen, meld u aan bij het hoofdknooppunt van het Hadoop-cluster, open de console van Hadoop-opdrachtregel en de Hive-query's van daaruit verzenden. Zie voor instructies voor het indienen van Hive-query's in de Hadoop-opdrachtregelconsole [het Hive-query's verzenden](move-hive-tables.md#submit).

## <a name="uniform"></a>Uniform steekproeven
Uniform steekproeven betekent dat elke rij in de gegevensset wordt door actieve evenveel kans heeft. Dit kan worden geïmplementeerd door toe te voegen een extra veld ASELECT() de gegevensset in de interne 'selecteren' query en in de buitenste 'selecteren' query die voorwaarde op dat willekeurige veld.

Hier volgt een voorbeeldquery:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, …, fieldN
    from
        (
        select
            field1, field2, …, fieldN, rand() as samplekey
        from <hive table name>
        )a
    where samplekey<='${hiveconf:sampleRate}'

Hier `<sample rate, 0-1>` geeft het aantal records die u wilt dat de gebruikers voor een steekproef.

## <a name="group"></a>Steekproeven door groepen
Wanneer steekproeven categorische gegevens, u mogelijk wilt opnemen of uitsluiten van alle van de exemplaren voor een bepaalde waarde van de variabele categorische. Dit soort steekproeven wordt 'steekproeven per groep' genoemd. Bijvoorbeeld, als er een categorische variabele '*status*', die waarden zoals NY, MA CA, NJ en PA heeft, kunt u records voor elke status worden samen, of ze of niet worden vastgelegd.

Hier volgt een voorbeeldquery die voorbeelden per groep:

    SET sampleRate=<sample rate, 0-1>;
    select
        b.field1, b.field2, …, b.catfield, …, b.fieldN
    from
        (
        select
            field1, field2, …, catfield, …, fieldN
        from <table name>
        )b
    join
        (
        select
            catfield
        from
            (
            select
                catfield, rand() as samplekey
            from <table name>
            group by catfield
            )a
        where samplekey<='${hiveconf:sampleRate}'
        )c
    on b.catfield=c.catfield

## <a name="stratified"></a>Toepassing stratificatie steekproeven
Steekproeven is toepassing stratificatie ten opzichte van een variabele categorische wanneer de voorbeelden verkregen categorische waarden die aanwezig in dezelfde verhouding zijn zijn als in de populatie bovenliggende hebben. Met behulp van het hetzelfde voorbeeld zoals hierboven, stel uw gegevens heeft de volgende opmerkingen bij de statussen: NJ heeft 100 opmerkingen, NY is 60 opmerkingen en WA 300 opmerkingen. Als u het aantal steekproeven toepassing stratificatie moet 0,5 opgeeft, moet vervolgens het voorbeeld dat is verkregen er ongeveer 50, 30 en 150 opmerkingen van NJ NY en WA respectievelijk.

Hier volgt een voorbeeldquery:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, field3, ..., fieldN, state
    from
        (
        select
            field1, field2, field3, ..., fieldN, state,
            count(*) over (partition by state) as state_cnt,
              rank() over (partition by state order by rand()) as state_rank
          from <table name>
        ) a
    where state_rank <= state_cnt*'${hiveconf:sampleRate}'


Zie voor meer informatie over meer geavanceerde steekproeven methoden die beschikbaar in de component zijn [LanguageManual steekproeven](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).

