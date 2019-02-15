---
title: Azure Data Factory cumulatieve gegevenstransformatie stroom toewijzen
description: Azure Data Factory stroom cumulatieve transformatie
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: 3793c49da32845d559d73faa25c571d3f86e062f
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56271817"
---
# <a name="azure-data-factory-mapping-data-flow-aggregate-transformation"></a>Azure Data Factory cumulatieve gegevenstransformatie stroom toewijzen

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

De statistische transformatie is waarin u opgeeft aggregaties van kolommen in uw data-stromen. U kunt in de opbouwfunctie voor verschillende soorten aggregaties (dat wil zeggen SUM, MIN, MAX, aantal, enz.) definiëren en maak een nieuw veld in de uitvoer die deze aggregaties met optionele group by-velden bevat.

![Opties voor transformatie aggregeren](media/data-flow/agg.png "aggregeren 1")

## <a name="group-by"></a>Groeperen op
(Optioneel) Kies een Group by-component voor de aggregatie en gebruik de naam van een bestaande kolom of een nieuwe naam. Gebruik 'kolom toevoegen' meer group by-componenten toevoegen en klik op het tekstvak naast de naam van de kolom om te starten van de opbouwfunctie voor alleen een bestaande kolom, combinatie van kolommen of expressies voor de groepering selecteren.

## <a name="the-aggregate-column-tab"></a>Het tabblad statistische kolom 
(Vereist) Kies het tabblad statistische kolom om de aggregatie-expressies te bouwen. U kunt kiezen van een bestaande kolom aan de waarde overschreven met de aggregatie, of maak een nieuw veld met de nieuwe naam voor de aggregatie. De expressie die u wilt gebruiken voor de aggregatie moet worden ingevoerd in het rechter in naast de naam kolomkiezer. Te klikken op het tekstvak wordt de opbouwfunctie voor expressies geopend.

![Opties voor transformatie aggregeren](media/data-flow/agg2.png "aggregator")

## <a name="data-preview-in-expression-builder"></a>Voorbeeld van gegevens in de opbouwfunctie voor expressies

In de foutopsporingsmodus de opbouwfunctie voor expressies kan niet worden gegenereerd, voorbeelden van gegevens met statistische functies. Voorbeelden van gegevens voor statistische transformaties bekijken, sluit u de opbouwfunctie voor expressies en het gegevensprofiel uit de flow-ontwerper weergeven.
