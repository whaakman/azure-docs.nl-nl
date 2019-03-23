---
title: Azure Data Factory Mapping Data Flow Sink Transformation
description: Azure Data Factory Mapping Data Flow Sink Transformation
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: a56f391aa76bd1216fd51d516adb836a2093bcba
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58371136"
---
# <a name="mapping-data-flow-sink-transformation"></a>Mapping Data Flow Sink Transformation

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Sink-opties](media/data-flow/windows1.png "sink-1")

Na het voltooien van uw flow-transformatie van gegevens, kunt u de getransformeerde gegevens naar een bestemming gegevensset sink. In de Sink-transformatie, kunt u de definitie van de gegevensset die u wilt gebruiken voor de uitvoergegevens van de bestemming. Mogelijk hebt u zoveel Sink-transformatie als de gegevensstroom is vereist.

Een gebruikelijk om ter compensatie voor het wijzigen van binnenkomende gegevens en ter compensatie van schema drift is het sink-de uitvoergegevens naar een map zonder een schema opgegeven in de uitvoergegevensset. U kunt ook rekening gehouden met alle kolom wijzigingen in uw bronnen hiervoor 'Toestaan Schema Drift' op de bron- en vervolgens automatisch toewijzen alle velden in de Sink.

U kunt overschrijven, toevoegen, of als de gegevensstroom mislukt wanneer u zich met een gegevensset.

U kunt ook 'automatisch ' toewijzen aan alle binnenkomende velden sink. Als u wilt de velden die u wilt sink naar de bestemming kiest, of als u wilt wijzigen van de namen van de velden op de bestemming, 'Uitschakelen' kiest voor "automatisch"toewijzen en klik vervolgens op het tabblad toewijzing uitvoervelden toe te wijzen:

![Sink-opties](media/data-flow/sink2.png "sink-2")

## <a name="output-to-one-file"></a>Uitvoer naar een bestand
Voor Azure Storage-Blob of Data Lake-sink-typen, wordt u voert u de getransformeerde gegevens naar een map. Spark parameterselectie gepartitioneerde gegevens uitvoerbestanden op basis van het partitieschema wordt gebruikt in de Sink-transformatie. U kunt het partitieschema instellen door te klikken op het tabblad 'Optimaliseren'. Als u graag ADF voor het samenvoegen van de uitvoer in een enkel bestand, klikt u op het keuzerondje 'Één partitie'.

![Sink-opties](media/data-flow/opt001.png "sink-opties")

## <a name="field-mapping"></a>Veldtoewijzing

Op het tabblad toewijzing van de Sink-transformatie kunt u de kolommen binnenkomend (links) toewijzen aan de bestemming (rechts). Wanneer u sink-gegevensstromen naar bestanden, noteer ADF altijd nieuwe bestanden naar een map. Wanneer u aan een database-gegevensset toewijst, kunt u een nieuwe tabel genereren met dit schema (opslaan beleid instellen ' overschrijven ') of invoegen van nieuwe rijen aan een bestaande tabel en de velden toewijzen aan de bestaande schema's.

U kunt meervoudige selectie in de toewijzingstabel gebruiken op meerdere kolommen met één klik een koppeling, loskoppelen van meerdere kolommen of meerdere rijen worden toegewezen aan de naam van de dezelfde kolom.

Als u wilt om altijd de binnenkomende set met velden en toe te wijzen aan een doel als-de instelling 'Schema Drift toestaan' is ingesteld.

![Field Mapping](media/data-flow/multi1.png "meerdere opties")

Als u wilt uw kolomtoewijzingen herstellen, drukt u op de knop 'Wijst' in te stellen de toewijzingen.

![Sink-opties](media/data-flow/sink1.png "een Sink")

![Sink-opties](media/data-flow/sink2.png "Sinks")

* Kan Schema Drift en valideren van Schema-opties zijn nu beschikbaar in Sink. Dit kunt u opdracht geven ADF kunt u volledig flexibele schemadefinities (Schema Drift) accepteren of de Sink mislukken als het schema (Schema valideren) wordt gewijzigd.

* Hiermee schakelt u de map. ADF wordt de inhoud van de sink-map voor het schrijven van de doel-bestanden in die doelmap afkappen.

## <a name="file-name-options"></a>Opties voor bestandsnamen

   * Standaard: Toestaan dat Spark op de naam van bestanden op basis van standaardinstellingen voor onderdeel
   * Patroon: Geef een patroon voor de uitvoerbestanden. Bijvoorbeeld, 'leningen [n]' maakt loans1.csv, loans2.csv,...
   * Per partitie: Geef een bestandsnaam per partitie
   * Als de gegevens in de kolom: Het uitvoerbestand ingesteld op de waarde van een kolom

> [!NOTE]
> Bestandsbewerkingen wordt alleen uitgevoerd wanneer u de activiteit gegevens stromen uitvoeren, niet in de gegevens stromen Debug-modus worden uitgevoerd

## <a name="database-options"></a>Opties voor de database

* Toestaan dat insert, update, delete, upsert-bewerking. De standaardwaarde is om te kunnen worden ingevoegd. Als u bijwerken, upsert of rijen verwijderen wilt, moet u eerst een transformatie van de rij alter toevoegen aan de rijen van de code voor deze specifieke acties. Het uitschakelen van 'Insert toestaan', stopt ADF van het invoegen van nieuwe rijen vanuit de bron.
* Afkappen van tabel (alle rijen uit de doeltabel verwijderd voordat de gegevensstroom is voltooid)
* Maak de tabel (voert drop/maken van de doeltabel voordat de gegevensstroom is voltooid)
* Batchgrootte voor grote hoeveelheden gegevens worden geladen. Voer een getal en bucket schrijfbewerkingen in segmenten
* Faseringsmodus inschakelen: Deze instructies voor ADF Polybase gebruiken bij het laden van Azure Data Warehouse als het sink-gegevensset

> [!NOTE]
> In de gegevensstroom, kunt u vragen ADF kunt u de tabeldefinitie van een nieuwe maken in de doeldatabase door in te stellen van een gegevensset in de Sink-transformatie die de naam van een nieuwe tabel heeft. In de SQL-gegevensset, klikt u op 'Bewerken' onder de tabelnaam van de en voer de naam van een nieuwe tabel. Vervolgens, in de transformatie Sink inschakelen 'Schema Drift toestaan'. Seth de instelling 'Schema importeren' op None.

![Schema van de gegevensbron transformatie](media/data-flow/dataset2.png "SQL-Schema")

![SQL-Sink opties](media/data-flow/alter-row2.png "SQL-opties")

> [!NOTE]
> Tijdens het bijwerken of verwijderen van rijen in de database-sink, moet u de sleutelkolom instellen. Op deze manier Alter rij is kan de unieke rij in de DML bepalen.

## <a name="next-steps"></a>Volgende stappen

Nu dat u de gegevensstroom hebt gemaakt, Voeg een [gegevensstroom uitvoeren activiteit aan uw pijplijn](concepts-data-flow-overview.md).
