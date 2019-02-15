---
title: Azure Data Factory gegevenstransformatie stroom bron toewijzen
description: Azure Data Factory gegevenstransformatie stroom bron toewijzen
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 35f4e794caf84aba860b98e68eadcdcd88e77952
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56271869"
---
# <a name="azure-data-factory-mapping-data-flow-source-transformation"></a>Azure Data Factory gegevenstransformatie stroom bron toewijzen

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

De bron-transformatie configureert u een gegevensbron die u gebruiken wilt om gegevens in de gegevensstroom. Mogelijk hebt u meer dan 1 bron op een enkele gegevensstroom transformeren. Begin altijd het ontwerpen van uw gegevens stromen met de bron.

> [!NOTE]
> Elke gegevensstroom vereist ten minste één bron transformatie. Voeg zo veel extra bronnen die u nodig hebt

![Opties voor transformatie](media/data-flow/source.png "bron")

De gegevensstroom-bron moet worden gekoppeld aan één ADF-gegevensset, die bepaalt de vorm en de locatie van uw gegevens schrijven naar of lezen vanaf.

## <a name="data-flow-staging-areas"></a>Ruimten met tijdelijke bestanden van de gegevensstroom

ADF-gegevensstroom heeft de regel van zicht tot 5 primaire "staging" gebieden in Azure uit te voeren van uw gegevenstransformaties: Azure-Blob, ADLS Gen 1, ADLS van generatie 2, Azure SQL-database en Azure SQL DW. ADF heeft toegang tot bijna 80 verschillende systeemeigen connectors, dus als u wilt opnemen die andere gegevensbronnen in de gegevensstroom, eerst fase die gegevens in één van deze vijf primaire gegevensstroom fasering gebieden eerst met behulp van de Kopieeractiviteit:

## <a name="options"></a>Opties

### <a name="allow-schema-drift"></a>Schema drift toestaan
Selecteer Schema Drift toestaan als de bronkolommen wordt vaak veranderen. Deze instelling kunnen alle binnenkomende velden van de bron langs de transformaties voor de Sink.

### <a name="fail-if-columns-in-the-dataset-are-not-found"></a>Mislukt als kolommen in de gegevensset zijn niet gevonden
Selecteer deze optie om af te dwingen een bron schemavalidatie is dat niet wordt uw gegevensstroom als kolommen die naar verwachting vanuit de bron niet aanwezig zijn.

### <a name="sampling"></a>Steekproeven
Steekproeven gebruiken om te beperken van het aantal rijen van de bron.  Dit is handig wanneer u slechts een voorbeeld van uw gegevens nodig hebt voor het testen en foutopsporing.

### <a name="define-schema"></a>Schema definiëren

![Bron-transformatie](media/data-flow/source2.png "2 van bron")

### <a name="you-can-modify-the-name-of-the-source-columns-and-their-associated-data-types"></a>U kunt de naam van de kolommen in de gegevensbron en hun bijbehorende gegevenstypen wijzigen

U moet de gegevenstypen voor elk veld hier in de bron-transformatie plaats in de gegevensset definiëren voor de bron-bestandstypen die niet sterk getypeerde (dat wil zeggen platte bestanden in plaats van Parquet-bestanden).

Als u niet de kolomnamen en gegevenstypen in de gegevensstroom ziet, is het waarschijnlijk omdat u niet is gedefinieerd in de sectie Schema definiëren van de Sink. U moet alleen om dit te doen als u geen Schema Drift verwerking van gegevens stromen.

Hier in de 'definiëren Schema' is tabblad op de bron-transformatie waarin u de gegevenstypen en -indelingen kunt instellen:

![Bron-transformatie](media/data-flow/source003.png "gegevenstypen")

### <a name="optimize"></a>Optimaliseren

![Bron van partities](media/data-flow/sourcepart.png "partitioneren")

Op het tabblad optimaliseren voor de bron-transformatie ziet u een extra partities type met de naam 'Bron'. Dit wordt alleen licht-up wanneer u Azure SQL-database hebt geselecteerd als de bron. Dit komt doordat ADF parallel verbindingen wilt voor het uitvoeren van grote query's op uw Azure SQL DB-gegevensbron.

Partitioneren van gegevens op uw bron-SQL-database is optioneel, maar is handig voor grote query's. U hebt hiervoor twee opties:

### <a name="column"></a>Kolom

Selecteer een kolom van de brontabel naar partitie op. U moet ook het maximale aantal verbindingen instellen.

### <a name="query-condition"></a>Queryvoorwaarde

U kunt eventueel kiezen voor het partitioneren van de verbindingen op basis van een query. Voor deze optie, plaatst u de inhoud van een WHERE-predicaat. Dat wil zeggen, jaar > 1980
