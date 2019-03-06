---
title: Azure Data Factory gegevensstroom gegevenssets toewijzen
description: Azure Data Factory toewijzing gegevensstroom is sepecific gegevensset compatibel
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/14/2019
ms.openlocfilehash: c704fdba22138eb8e4610a7bc34bd4c2cc98f4ca
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57432087"
---
# <a name="mapping-data-flow-datasets"></a>Toewijzing van gegevensstroom gegevenssets

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Gegevenssets zijn een Data Factory-constructie die de vorm van de gegevens waarmee die u in de pijplijn werkt. In de gegevensstroom moet rij- en gegevens op de gegevenssetdefinitie van een opdelen nauwkeurigere. Gegevenssets die worden gebruikt in een besturingselement analysepijplijnbehoeften hoeft niet de dezelfde diepte van inzicht in gegevens.

Gegevenssets in de gegevensstroom worden gebruikt in de bron en Sink-transformaties. Ze worden gebruikt voor het definiëren van de basisgegevens schema's. Als u geen schema in uw gegevens, kunt u Schema Drift instellen op voor de bron en Sink. Met het schema is gedefinieerd in de gegevensset, hebt u de gerelateerde gegevenstypen, opmaak van gegevens, locatie en verbindingsinformatie van de bijbehorende gekoppelde Service. De metagegevens van de gegevenssets wordt weergegeven in de bron-transformatie als de bron '-projectie.

## <a name="dataset-types"></a>Typen gegevensset

Op dit moment in de gegevensstroom vindt u vier typen van gegevensset:

* Azure SQL Database
* Azure SQL DW
* Parquet (van ADLS & Blob)
* Tekst met scheidingstekens (van ADLB & Blob)

Gegevensstroom gegevenssets afzonderlijke de *gegevensbrontype* uit de *verbindingstype van de gekoppelde Service*. Meestal in de Data Factory, kies het verbindingstype (Blob, ADLS, enzovoort) en vervolgens het type van bestand in de gegevensset te definiëren. In de gegevensstroom kiest u de typen gegevensbronnen, die gekoppeld aan verschillende verbindingstypen van de gekoppelde Service worden kunnen.

![Opties voor transformatie](media/data-flow/dataset1.png "bronnen")

## <a name="data-flow-compatible-datasets"></a>Gegevensstroom compatibel gegevenssets

Bij het maken van een nieuwe gegevensset, is er een selectievakje met het label 'Data Flow compatibel' in de rechterbovenhoek van het paneel. Op deze knop klikt, wordt alleen de gegevenssets die kunnen worden gebruikt met de gegevens stromen filteren. 

## <a name="import-schemas"></a>Schema's importeren

Bij het importeren van het schema van de gegevensstroom gegevenssets, ziet u een knop Schema importeren. Op deze knop klikt, geeft u twee opties: Importeren uit de bron of importeren uit een lokaal bestand. In de meeste gevallen zult u het schema importeren rechtstreeks vanuit de bron. Echter, als u een bestaand schema-bestand (Parquet-bestanden of CSV met koppen) hebt, u kunt verwijzen naar die lokale bestands- en Data Factory het schema op basis van dat schemabestand definiëren.

## <a name="next-steps"></a>Volgende stappen

Begin met [het maken van een nieuwe gegevensstroom](data-flow-create.md) en voeg een transformatie van de bron. Configureer vervolgens de gegevensset voor de bron.

