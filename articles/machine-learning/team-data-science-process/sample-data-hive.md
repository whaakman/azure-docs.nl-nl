---
title: Voorbeeldgegevens in Azure HDInsight Hive-tabellen - Team Data Science Process
description: Down-sampling van gegevens die zijn opgeslagen in Azure HDInsight Hive-tabellen met behulp van Hive-query's kunt u de gegevens verkleinen tot een grootte die beter beheersbare voor analyse.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/13/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: c417950e07ae3c6922aa260a3ef40d862870aa1e
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55452325"
---
# <a name="sample-data-in-azure-hdinsight-hive-tables"></a>Voorbeeldgegevens in Hive-tabellen in Azure HDInsight
In dit artikel wordt beschreven hoe u down-sampling van gegevens die zijn opgeslagen in Azure HDInsight Hive-tabellen met behulp van Hive-query's te verkleinen tot een grootte die beter beheersbare voor analyse. Deze drie methoden voor dit gebruikte steekproeven omvatten:

* Uniform steekproeven
* Steekproeven in groepen
* Toepassing stratificatie steekproeven

**Waarom sample van uw gegevens?**
Als de gegevensset die u van plan bent om te analyseren groot is, is het doorgaans een goed idee om down-sampling van de gegevens om deze aan de grootte van een kleiner, maar representatieve en gemakkelijker. Down-sampling vereenvoudigt het uitvoeren van inzicht in gegevens verkennen en feature-engineering. De rol in het Team Data Science Process is om in te schakelen, snel ontwikkelen van prototypen van de functies voor het verwerken van gegevens en machine learning-modellen.

Deze taak steekproeven is een stap in de [Team Data Science Process (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/).

## <a name="how-to-submit-hive-queries"></a>Het indienen van Hive-query 's
Hive-query's kunnen worden verzonden vanaf de opdrachtregel Hadoop-console op het hoofdknooppunt van het Hadoop-cluster. Om dit te doen, meld u aan bij het hoofdknooppunt van het Hadoop-cluster, open de opdrachtregelconsole Hadoop en het Hive-query's van daaruit indienen. Zie voor instructies over het indienen van Hive-query's in de Hadoop-opdrachtregelconsole [hoe u Hive-query's verzenden](move-hive-tables.md#submit).

## <a name="uniform"></a> Uniform steekproeven
Uniform steekproeven betekent dat elke rij in de gegevensset heeft een gelijke kans, worden steekproeven genomen. Dit kan worden geïmplementeerd door toe te voegen een extra veld ASELECT() in de gegevensset in de binnenste query voor "selecteren" en in de buitenste 'selecteren'-query die voorwaarde op een willekeurig veld.

Hier volgt een voorbeeld van een query:

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

Hier `<sample rate, 0-1>` Hiermee geeft u het deel van de records die de gebruikers willen dat er een steekproef.

## <a name="group"></a> Steekproeven in groepen
Wanneer categorische steekproef nemen voor gegevens, kunt u op te nemen of alle van de exemplaren van een waarde van de variabele categorische uitsluiten. De sortering van het samplen bijhouden heet 'steekproeven per groep'. Bijvoorbeeld, als er een categorische variabele "*status*', die waarden zoals NY, MA, CA, NJ en PA heeft, wilt u records uit elke de status, of ze niet worden vastgelegd.

Hier volgt een voorbeeldquery die voorbeelden van groep:

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
Steekproeven is toepassing stratificatie met betrekking tot een categorische variabele wanneer de voorbeelden die zijn verkregen categorische waarden die aanwezig zijn in dezelfde verhouding zoals ze in de populatie van de bovenliggende waren hebben. Met behulp van hetzelfde voorbeeld als hierboven, Stel dat uw gegevens heeft de volgende opmerkingen per statussen: NJ heeft 100 opmerkingen, NY is 60 opmerkingen en WA heeft 300 opmerkingen. Als u het aantal steekproeven toepassing stratificatie moet 0,5 opgeeft, klikt u vervolgens moet het voorbeeld dat is verkregen ongeveer 50, 30 en 150 opmerkingen van NJ, NY en WA respectievelijk.

Hier volgt een voorbeeld van een query:

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


Zie voor meer informatie over de meer geavanceerde steekproeven methoden die beschikbaar in Hive zijn [LanguageManual steekproeven](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).

