---
title: Azure Data Factory Mapping Data Flow Sink Transformation
description: Azure Data Factory Mapping Data Flow Sink Transformation
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: 795b8072bbd9b248f982d061d699f490b1b63b17
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56271868"
---
# <a name="azure-data-factory-mapping-data-flow-sink-transformation"></a>Azure Data Factory Mapping Data Flow Sink Transformation

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Sink-opties](media/data-flow/windows1.png "sink-1")

Na het voltooien van uw flow-transformatie van gegevens, kunt u de getransformeerde gegevens naar een bestemming gegevensset sink. In de Sink-transformatie, kunt u de definitie van de gegevensset die u wilt gebruiken voor de uitvoergegevens van de bestemming.

Een gebruikelijk om ter compensatie voor het wijzigen van binnenkomende gegevens en ter compensatie van schema drift is het sink-de uitvoergegevens naar een map zonder een schema opgegeven in de uitvoergegevensset. U kunt ook rekening gehouden met alle kolom wijzigingen in uw bronnen hiervoor 'Toestaan Schema Drift' op de bron- en vervolgens automatisch toewijzen alle velden in de Sink.

U kunt overschrijven, toevoegen, of als de gegevensstroom mislukt wanneer u zich met een gegevensset.

U kunt ook 'automatisch ' toewijzen aan alle binnenkomende velden sink. Als u wilt de velden die u wilt sink naar de bestemming kiest, of als u wilt wijzigen van de namen van de velden op de bestemming, 'Uitschakelen' kiest voor "automatisch"toewijzen en klik vervolgens op het tabblad toewijzing uitvoervelden toe te wijzen:

![Sink-opties](media/data-flow/sink2.png "sink-2")

## <a name="output-to-one-file"></a>Uitvoer naar een bestand
Voor Azure Storage-Blob of Data Lake-sink-typen, wordt u voert u de getransformeerde gegevens naar een map. Spark parameterselectie gepartitioneerde gegevens uitvoerbestanden op basis van het partitieschema wordt gebruikt in de Sink-transformatie. U kunt het partitieschema instellen door te klikken op het tabblad 'Optimaliseren'. Als u graag ADF voor het samenvoegen van de uitvoer in een enkel bestand, klikt u op het keuzerondje 'Één partitie'.

![Sink-opties](media/data-flow/opt001.png "sink-opties")

## <a name="blob-storage-folder"></a>Map voor BLOB Storage
Als uw gegevenstransformaties zich naar Blob Store, kiest u een blob *map* als het pad naar de doelmap, geen bestand. U ADF-gegevensstroom, wordt de uitvoerbestanden in die map gegenereerd.

![Het pad naar map](media/data-flow/folderpath.png "mappad")

## <a name="optional-azure-sql-data-warehouse-sink"></a>Optional Azure SQL Data Warehouse Sink

We introduceren een vroege bèta van de ADW Sink-gegevensset voor de gegevensstroom. Hiermee kunt u de getransformeerde gegevens rechtstreeks in Azure SQL DW binnen gegevensstroom land zonder de noodzaak van het toevoegen van een Kopieeractiviteit in de pijplijn.

Beginnen met het maken van een gegevensset ADW, net zoals u zou doen voor een andere ADF-pijplijn met een gekoppelde Service met de referenties van uw ADW en kiest u de database die u wilt verbinden. Selecteer een bestaande tabel of typ de naam van de tabel die u wilt dat gegevensstroom automatisch voor u te maken van de binnenkomende velden in de naam van de tabel.

![Sink-opties](media/data-flow/adw3.png "sink-3")

Terug op de Sink-tranformation (ADW is momenteel alleen ondersteund als een Sink), kiest u de gegevensset ADW die u hebt gemaakt en het Opslagaccount dat u wilt gebruiken voor de gegevens voor de Polybase in ADW laden fasering. Het padveld van de indeling is: "containername/mapnaam".

![Sink-opties](media/data-flow/adw1.png "sink-4")

### <a name="save-policy"></a>Beleid opslaan

Overschrijven wordt de tabel worden afgekapt als deze bestaat, maakt u deze opnieuw en de gegevens worden geladen. Toevoeg, wordt de nieuwe rijen worden ingevoegd. Als de tabel uit de naam van de gegevensset tabel niet op alle in het doel ADW bestaat, gegevensstroom wordt maken van de tabel, en vervolgens de gegevens worden geladen.

Als u 'Toewijzing automatisch' uitschakelt, kunt u handmatig de velden toewijzen aan de doeltabel.

![Sink-opties voor ADW](media/data-flow/adw2.png "adw sink")

### <a name="max-concurrent-connections"></a>Maximale aantal gelijktijdige verbindingen

Bij het schrijven van uw gegevens naar een Azure database-verbinding, kunt u het maximum aantal gelijktijdige verbindingen instellen in de Sink-transformatie.

![Verbindingsopties](media/data-flow/maxcon.png "verbindingen")
