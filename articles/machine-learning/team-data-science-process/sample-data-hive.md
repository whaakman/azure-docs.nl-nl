---
title: Voorbeeldgegevens in Azure HDInsight Hive-tabellen | Microsoft Docs
description: Omlaag steekproef nemen van gegevens in Azure HDInsight (Hadopop) Hive-tabellen
services: machine-learning,hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: f31e8d01-0fd4-4a10-b1a7-35de3c327521
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: hangzh;bradsev
ms.openlocfilehash: 357307a034b277e8c37e99bda1ed6a9a76e13f41
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="sample-data-in-azure-hdinsight-hive-tables"></a>Voorbeeldgegevens in Hive-tabellen in Azure HDInsight
In dit artikel wordt beschreven hoe down-sample gegevens die zijn opgeslagen in Azure HDInsight Hive-tabellen met Hive-query's. We hebben betrekking op dit gebruikte steekproeven op drie manieren:

* Uniform steekproeven
* Steekproeven door groepen
* Toepassing stratificatie steekproeven

De volgende **menu** koppelingen naar onderwerpen waarin wordt beschreven hoe u voorbeeldgegevens uit verschillende omgevingen voor opslag.

[!INCLUDE [cap-sample-data-selector](../../../includes/cap-sample-data-selector.md)]

**Waarom een steekproef nemen uit uw gegevens?**
Als de gegevensset die u wilt analyseren groot is, is het meestal een goed idee om de gegevens om deze aan de grootte van een kleinere maar representatief en gemakkelijker down-voorbeeld. Dit vereenvoudigt het begrip van de gegevens, exploratie en functie-engineering. De rol in het Team gegevens wetenschap proces is om in te schakelen snel maken van een prototype van de functies voor het verwerken van gegevens en machine learning-modellen.

Deze taak steekproeven is een stap in de [Team gegevens wetenschap proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="how-to-submit-hive-queries"></a>Het indienen van Hive-query 's
Hive-query's kunnen worden verzonden vanaf de opdrachtregel voor Hadoop-console op het hoofdknooppunt van het Hadoop-cluster. Om dit te doen, meld u aan bij het hoofdknooppunt van het Hadoop-cluster, open de console Hadoop vanaf de opdrachtregel en de Hive-query's van daaruit verzenden. Zie voor instructies voor het indienen van Hive-query's in de console vanaf de opdrachtregel Hadoop [het Hive-query's verzenden](move-hive-tables.md#submit).

## <a name="uniform"></a>Uniform steekproeven
Uniform steekproeven betekent dat elke rij in de gegevensset wordt door actieve evenveel kans heeft. Dit kan worden geïmplementeerd door het toevoegen van een extra veld ASELECT() in de gegevensset in de interne 'selecteren' query en in de buitenste 'selecteren' query die voorwaarde op dat willekeurige veld.

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
Wanneer steekproeven categorische gegevens, u mogelijk wilt opnemen of uitsluiten van alle van de exemplaren van een bepaalde waarde van een categorische variabele. Dit is wat door 'steekproeven per groep'.
Bijvoorbeeld, als er een categorische variabele 'Status', die waarden NY, MA, CA, NJ, PA, enzovoort, wilt u records van dezelfde toestand worden altijd samen, of ze of niet worden vastgelegd.

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
Steekproeven is toepassing stratificatie ten opzichte van een variabele categorische wanneer de voorbeelden verkregen waarden van hebben dat categorische die zich in dezelfde verhouding zoals in de bovenliggende populatie van waaruit de voorbeelden zijn verkregen. Het voorbeeld hierboven, Stel dat uw gegevens heeft onderliggende populaties door statussen, spreken NJ heeft 100 opmerkingen, NY is 60 opmerkingen en WA 300 opmerkingen. Als u het aantal steekproeven toepassing stratificatie moet 0,5 opgeeft, moet vervolgens het voorbeeld dat is verkregen er ongeveer 50, 30 en 150 opmerkingen van NJ NY en WA respectievelijk.

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

