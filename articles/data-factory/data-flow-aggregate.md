---
title: Samenvoegen van de transformatie in het toewijzen van gegevensstroom - Azure Data Factory | Microsoft Docs
description: Leer hoe u gegevens op schaal in Azure Data Factory met de toewijzing van gegevens Flow cumulatieve transformatie worden geaggregeerd.
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: 21135b26d4bc840b3fcb091e675e5e6bd24d8548
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2019
ms.locfileid: "67312119"
---
# <a name="aggregate-transformation-in-mapping-data-flow"></a>Cumulatieve transformatie in de gegevensstroom toewijzen 

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

De statistische transformatie is waarin u opgeeft aggregaties van kolommen in uw data-stromen. De opbouwfunctie voor expressies kunt u verschillende soorten aggregaties zoals som, MIN, MAX en aantal die kan worden gegroepeerd op bestaande of berekende kolommen definiëren.

## <a name="group-by"></a>Groeperen op
Selecteer een bestaande kolom of maak een nieuwe berekende kolom moet worden gebruikt als een groep door de component voor de aggregatie. Selecteer de gewenste kolom in de vervolgkeuzelijst voor het gebruik van een bestaande kolom. Beweeg de muisaanwijzer over de component voor het maken van een nieuwe berekende kolom, en klikt u op 'Berekende kolom'. Hiermee opent u de [gegevens Flow opbouwfunctie voor expressies](concepts-data-flow-expression-builder.md). Nadat u de berekende kolom maken, voert u de naam van de uitvoer-kolom in het veld 'Naam als'. Als u toevoegen van een andere groep by-component wilt, Beweeg de muisaanwijzer over een bestaande component en klikt u op '+'.

![Cumulatieve transformatie groeperen op instellingen](media/data-flow/agg.png "cumulatieve transformatie groeperen op instellingen")

> [!NOTE]
> Een component group by is optioneel in een statistische transformatie.

## <a name="aggregate-column"></a>Statistische kolom 
Kies het tabblad 'Statistische functies' om statistische expressies te bouwen. U kunt kiezen van een bestaande kolom en de waarde overschreven met de aggregatie of een nieuw veld maken met een nieuwe naam. De aggregatie van expressie wordt ingevoerd in het vak rechts naast de naam kolomkiezer. Als u wilt de expressie bewerken, klikt u op het tekstvak om de opbouwfunctie voor expressies te openen. Een extra aggregatie, Beweeg de muisaanwijzer over een bestaande expressie toevoegen en klik op '+' te maken van een nieuwe aggregatiekolom of [kolom patroon](concepts-data-flow-column-pattern.md).

![Statistische samengevoegde transformatie-instellingen](media/data-flow/agg2.png "samen samengevoegde transformatie-instellingen")

> [!NOTE]
> De expressie van elke aggregatie moet ten minste één statistische functie bevatten.

> [!NOTE]
> In de foutopsporingsmodus de opbouwfunctie voor expressies kan niet worden gegenereerd, voorbeelden van gegevens met statistische functies. Als u voorbeelden van gegevens voor statistische transformaties, sluit de opbouwfunctie voor expressies en bekijkt u de gegevens via het tabblad Voorbeeld van gegevens.

## <a name="next-steps"></a>Volgende stappen

-Definiëren met behulp van aggregatie op basis van het venster de [venster transformatie](data-flow-window.md)