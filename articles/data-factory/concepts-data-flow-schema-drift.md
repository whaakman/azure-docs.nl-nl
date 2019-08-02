---
title: Gegevens stroom schema drift Azure Data Factory toewijzing
description: Flexibele gegevens stromen bouwen in Azure Data Factory met schema-drift
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/04/2018
ms.openlocfilehash: b5777300f5033569caf3868218e747df3ff83a76
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68640227"
---
# <a name="mapping-data-flow-schema-drift"></a>Gegevens stroom schema drift toewijzen

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Het concept van schema drift is het geval waarin uw bronnen vaak meta gegevens wijzigen. U kunt velden, kolommen, typen, enzovoort, toevoegen, verwijderen of wijzigen. Zonder verwerking voor schema drift wordt uw gegevens stroom kwetsbaar voor wijzigingen in gegevens bron wijzigingen in de upstream. Wanneer binnenkomende kolommen en velden worden gewijzigd, mislukken de gebruikelijke ETL-patronen omdat ze vaak aan deze bron namen worden gekoppeld.

Als u wilt beveiligen tegen schema drift, is het belang rijk dat u beschikt over de mogelijkheden van een hulp programma voor gegevens stroom zodat u, als een technicus, de volgende handelingen kunt uitvoeren:

* Bronnen definiëren die zijn ververanderd veld namen, gegevens typen, waarden en grootten
* Transformatie parameters definiëren die kunnen werken met gegevens patronen in plaats van in code vastgelegde velden en waarden
* Expressies definiëren die inzicht krijgen in patronen die overeenkomen met binnenkomende velden, in plaats van met behulp van benoemde velden

## <a name="how-to-implement-schema-drift-in-adf-mapping-data-flows"></a>Schema-drift implementeren in gegevens stromen voor ADF-toewijzing
ADF biedt systeem eigen ondersteuning voor flexibele schema's die van uitvoering naar uitvoering veranderen, zodat u een generieke gegevens transformatie logica kunt bouwen zonder dat u uw gegevens stromen opnieuw hoeft te compileren.

* Kies ' schema-drift toestaan ' in de bron transformatie

<img src="media/data-flow/schemadrift001.png" width="400">

* Wanneer u deze optie hebt geselecteerd, worden alle binnenkomende velden uit uw bron gelezen tijdens elke uitvoering van de gegevens stroom en worden ze door gegeven via de hele stroom naar de sink.

* Alle nieuw gedetecteerde kolommen (gedrijfde kolommen) worden standaard geleverd als teken reeks gegevens type. Kies in de bron transformatie de optie ' verstempelde kolom typen ' als u wilt dat ADF automatisch gegevens typen van de bron afleiden.

* Zorg ervoor dat u automatische toewijzing gebruikt om alle nieuwe velden in de Sink-trans formatie toe te wijzen, zodat alle nieuwe velden in uw doel worden gepickt en gelandd en stel schema-drift toestaan in op de sink.

<img src="media/data-flow/automap.png" width="400">

* Alles werkt wanneer er nieuwe velden worden geïntroduceerd in dat scenario met een eenvoudige toewijzing van de Bron-> Sink (kopiëren).

* Als u trans formaties wilt toevoegen aan die werk stroom die schema drift afhandelt, kunt u patroon vergelijking gebruiken om kolommen te vergelijken met de naam, het type en de waarde.

* Klik op kolom patroon toevoegen in de afgeleide kolom of aggregatie transformatie als u een trans formatie wilt maken die de ' schema-drift ' begrijpt.

<img src="media/data-flow/columnpattern.png" width="400">

> [!NOTE]
> U moet in uw gegevens stroom een architectuur besluit nemen om schema drift in uw stroom te accepteren. Wanneer u dit doet, kunt u zich beschermen tegen schema wijzigingen van de bronnen. U verliest echter een vroege binding van uw kolommen en typen in uw gegevens stroom. Azure Data Factory behandelt schema-drift als late-binding stromen, dus wanneer u trans formaties bouwt, zijn de kolom namen niet voor u beschikbaar in de schema weergaven in de stroom.

<img src="media/data-flow/taxidrift1.png" width="400">

In de voorbeeld gegevens stroom van de taxi-demo bevindt zich een voor beeld van een schema drift in de onderste gegevens stroom met de bron TripFare. In de cumulatieve trans formatie ziet u dat we het ontwerp van het kolom patroon gebruiken voor de aggregatie velden. In plaats van de naamgeving van specifieke kolommen of op zoek naar kolommen op basis van de positie, wordt ervan uitgegaan dat de gegevens kunnen veranderen en mogelijk niet in dezelfde volg orde worden weer gegeven als tussen uitvoeringen.

In dit voor beeld van Azure Data Factory gegevensstroom schema voor het verwerken van gegevens stromen hebben we een samen stelling en aggregatie gemaakt waarmee wordt gescand op kolommen van het type Double. u weet dat het gegevens domein prijzen voor elke reis bevat. We kunnen vervolgens een cumulatieve wiskundige berekening uitvoeren voor alle dubbele velden in de bron, ongeacht waar de kolom zich bevindt en ongeacht de naam van de kolom.

De syntaxis van de Azure Data Factory gegevens stroom gebruikt $ $ om elke overeenkomende kolom uit uw overeenkomende patroon weer te geven. U kunt ook zoeken op kolom namen met behulp van complexe Zoek opdrachten voor teken reeksen en reguliere expressies. In dit geval gaan we een nieuwe geaggregeerde veld naam maken op basis van elke overeenkomst met een dubbel type kolom en voegt u de tekst ```_total``` toe aan elke overeenkomende naam: 

```concat($$, '_total')```

Vervolgens worden de waarden voor elk van deze overeenkomende kolommen afgerond en opgeteld:

```round(sum ($$))```

U kunt deze schema drift-functionaliteit op het werk zien met de Azure Data Factory gegevens stroom voor beeld van een taxi. Schakel over op de foutopsporingssessie met de schakel optie voor fout opsporing boven aan het ontwerp oppervlak voor gegevens stromen, zodat u uw resultaten interactief kunt zien:

<img src="media/data-flow/taxidrift2.png" width="800">

## <a name="access-new-columns-downstream"></a>Toegang tot nieuwe kolommen downstream
Wanneer u nieuwe kolommen met kolom patronen genereert, kunt u deze nieuwe kolommen later in uw gegevensstroom transformaties met deze methoden benaderen:

* Gebruik ' byPosition ' om de nieuwe kolommen op positie nummer te identificeren.
* Gebruik ' byName ' om de nieuwe kolommen te identificeren met hun naam.
* Gebruik in kolom patronen "naam", "Stream", "position" of "type" of een wille keurige combi natie van deze om nieuwe kolommen te zoeken.

## <a name="rule-based-mapping"></a>Toewijzing op basis van een regel
De SELECT-en Sink-trans formatie ondersteunen patroon vergelijking via op regels gebaseerde toewijzing. Hiermee kunt u regels maken waarmee u geplaatste kolommen aan kolom aliassen kan toewijzen en die kolommen naar uw bestemming kunnen opvangen.

## <a name="next-steps"></a>Volgende stappen
In de [taal van de data flow-expressie](data-flow-expression-functions.md) vindt u aanvullende voorzieningen voor kolom patronen en schema-drift, waaronder "byName" en "byPosition".
