---
title: Azure Data Factory gegevenstransformatie stroom Pivot toewijzen
description: Gegevens uit rijen naar kolommen met behulp van Azure Data Factory toewijzing Flow Pivot gegevenstransformatie van draaipunt
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: e16cac281b77f3ca93d9ef358ae806203bc8b663
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61348449"
---
# <a name="azure-data-factory-pivot-transformation"></a>Azure data factory pivot transformatie
[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Gebruik Pivot in ADF-gegevensstroom als een aggregatie waarbij een of meer groeperende kolommen zijn afzonderlijke rijwaarden omgezet in afzonderlijke kolommen heeft. U kunt in principe rijwaarden draaien in nieuwe kolommen (gegevens omzetten in metagegevens).

![Opties van draaipunt](media/data-flow/pivot1.png "1 van draaipunt")

## <a name="group-by"></a>Groeperen op

![Opties van draaipunt](media/data-flow/pivot2.png "2 van draaipunt")

Stel eerst de kolommen die u groeperen wilt op voor de aggregatie pivot. U kunt instellen dat meer dan 1 kolom hier met het plusteken naast de lijst met kolommen.

## <a name="pivot-key"></a>Sleutel draaien

![Opties van draaipunt](media/data-flow/pivot3.png "3 van draaipunt")

De Pivot-sleutel is de kolom die ADF uit de rij wordt Schakel over naar de kolom. Elke unieke waarde in de gegevensset voor dit veld wordt standaard Schakel over naar een kolom. U kunt echter (optioneel) de waarden uit de gegevensset die u wilt Schakel over naar kolomwaarden invoeren. Dit is de kolom die bepalen de nieuwe kolommen die worden gemaakt.

## <a name="pivoted-columns"></a>Gedraaide kolommen

![Opties van draaipunt](media/data-flow/pivot4.png "4 van draaipunt")

Ten slotte, kiest u de aggregatie die u wilt gebruiken voor de gedraaide waarden en weten hoe u de kolommen in de nieuwe projectie van de uitvoer van de transformatie moet worden weergegeven.

(Optioneel) U kunt een specifiek naamgevingspatroon: met een voorvoegsel, midden en achtervoegsel om te worden toegevoegd aan elke nieuwe kolomnaam van de rijwaarden instellen.

Bijvoorbeeld zou 'Verkoop' door 'Regio' draaien leiden tot nieuwe kolomwaarden van elke waarde van de verkopen, dat wil zeggen "25", "50", "1000", etc. Echter als u een waarde van het aanpassingsvoorvoegsel van 'Sales-' instellen, toevoegen elke waarde in de kolom 'Sales-' aan het begin van de waarde.

![Opties van draaipunt](media/data-flow/pivot5.png "5 van draaipunt")

Instellen van de rangschikking van de kolom "Normale" samen worden gegroepeerd alle gedraaide kolommen met hun geaggregeerde waarden. Instellen van de kolommen rangschikking aan 'Laterale' wordt schakelen tussen de kolom en waarde.

### <a name="aggregation"></a>Aggregatie

Om in te stellen de aggregatie die u wilt gebruiken voor de pivot-waarden, klikt u op het veld onder aan het deelvenster kolommen gedraaid. Voert u in de gegevensstroom ADF opbouwfunctie voor expressies kunt u een aggregatie van expressie maken en geef een beschrijvende aliasnaam voor uw nieuwe geaggregeerde waarden.

De ADF gegevens Flow expressietaal gebruiken om te beschrijven van de transformaties gedraaide kolom in de opbouwfunctie voor expressies: https://aka.ms/dataflowexpressions.

## <a name="pivot-metadata"></a>De metagegevens van de draaitabel

De Pivot-transformatie worden nieuwe kolomnamen die dynamisch op basis van de binnenkomende gegevens zijn produceren. De sleutel Pivot produceert de waarden voor elke nieuwe kolomnaam. Als u geen afzonderlijke waarden opgeven en wilt maken van dynamische kolomnamen voor elke unieke waarde in de Pivot-sleutel, de gebruikersinterface wordt niet weergegeven voor de metagegevens in inspecteren en er is geen kolom doorgeven aan de Sink-transformatie. Als u waarden voor de sleutel Pivot hebt ingesteld, klikt u vervolgens ADF de namen van de nieuwe kolommen kunt bepalen en de kolomnamen worden die u in het inspecteren en Sink-toewijzing.

### <a name="landing-new-columns-in-sink"></a>Nieuwe kolommen terechtkomen in Sink

Zelfs met dynamische kolomnamen in de draaitabel, kunt u nog steeds uw nieuwe kolomnamen en waarden in uw doelarchief sink. Alleen ingesteld 'Schema Drift toestaan' op in de Sink-instellingen. De naam van de nieuwe dynamische wordt niet weergegeven in de kolommetagegevens van de, maar de schemaoptie drift kunt u de gegevens komt.

### <a name="view-metadata-in-design-mode"></a>Metagegevens weergeven in de ontwerpmodus

Als u wilt weergeven van de nieuwe kolomnamen als metagegevens in inspecteren en u wilt zien van de kolommen die expliciet wordt doorgegeven aan de Sink-transformatie, stelt u expliciete waarden op het tabblad Pivot-sleutel.

### <a name="how-to-rejoin-original-fields"></a>Het oorspronkelijke velden weer
De Pivot-transformatie wordt alleen de kolommen die worden gebruikt in de aggregatie, de groepering en de pivot actie van het project. Als u opnemen van de andere kolommen uit de vorige stap in de stroom wilt, gebruikt u een nieuwe vertakking in de vorige stap en het patroon self-join om de stroom verbinding met de oorspronkelijke metagegevens te gebruiken.

## <a name="next-steps"></a>Volgende stappen

Probeer de [draaitabel opheffen voor transformatie](data-flow-unpivot.md) met kolomwaarden in rijwaarden. 
