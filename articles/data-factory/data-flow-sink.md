---
title: Azure Data Factory Mapping Data Flow Sink Transformation
description: Azure Data Factory Mapping Data Flow Sink Transformation
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: dba043721c2d81b7fe2c254f62328e54bb959cdc
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56729369"
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

### <a name="output-settings"></a>Uitvoerinstellingen

Overschrijven wordt de tabel worden afgekapt als deze bestaat, maakt u deze opnieuw en de gegevens worden geladen. Toevoeg, wordt de nieuwe rijen worden ingevoegd. Als de tabel uit de naam van de gegevensset tabel niet op alle in het doel ADW bestaat, gegevensstroom wordt maken van de tabel, en vervolgens de gegevens worden geladen.

Als u 'Toewijzing automatisch' uitschakelt, kunt u handmatig de velden toewijzen aan de doeltabel.

![Sink-opties voor ADW](media/data-flow/adw2.png "adw sink")

#### <a name="field-mapping"></a>Veldtoewijzing

Op het tabblad toewijzing van de Sink-transformatie kunt u de kolommen binnenkomend (links) toewijzen aan de bestemming (rechts). Wanneer u sink-gegevensstromen naar bestanden, noteer ADF altijd nieuwe bestanden naar een map. Wanneer u aan een database-gegevensset toewijst, kunt u een nieuwe tabel genereren met dit schema (opslaan beleid instellen ' overschrijven ') of invoegen van nieuwe rijen aan een bestaande tabel en de velden toewijzen aan de bestaande schema's.

U kunt meervoudige selectie in de toewijzingstabel gebruiken op meerdere kolommen met één klik een koppeling, loskoppelen van meerdere kolommen of meerdere rijen worden toegewezen aan de naam van de dezelfde kolom.

![Field Mapping](media/data-flow/multi1.png "meerdere opties")

Als u wilt uw kolomtoewijzingen herstellen, drukt u op de knop 'Wijst' in te stellen de toewijzingen.

![Verbindingen](media/data-flow/maxcon.png "Verbindingen")

### <a name="updates-to-sink-transformation-for-adf-v2-ga-version"></a>Updates van Sink-transformatie voor ADF V2 GA-versie

![Sink-opties](media/data-flow/sink1.png "een Sink")

![Sink-opties](media/data-flow/sink2.png "Sinks")

* Kan Schema Drift en valideren van Schema-opties zijn nu beschikbaar in Sink. Dit kunt u opdracht geven ADF kunt u volledig flexibele schemadefinities (Schema Drift) accepteren of de Sink mislukken als het schema (Schema valideren) wordt gewijzigd.

* Hiermee schakelt u de map. ADF wordt de inhoud van de sink-map voor het schrijven van de doel-bestanden in die doelmap afkappen.

* Opties voor bestandsnamen

   * Standaard: Toestaan dat Spark op de naam van bestanden op basis van standaardinstellingen voor onderdeel
   * Patroon: Voer een naam voor de uitvoerbestanden
   * Per partitie: Geef een bestandsnaam per partitie
   * Als de gegevens in de kolom: Het uitvoerbestand ingesteld op de waarde van een kolom

> [!NOTE]
> Bestandsbewerkingen wordt alleen uitgevoerd wanneer u de activiteit gegevens stromen uitvoeren, niet in de gegevens stromen Debug-modus worden uitgevoerd

Met de SQL-sink-typen, kunt u het volgende instellen:

* Tabel afkappen
* Maak de tabel (voert drop/maken)
* Batchgrootte voor grote hoeveelheden gegevens worden geladen. Voer een getal en bucket schrijfbewerkingen in segmenten.

![Veldtoewijzing](media/data-flow/sql001.png "SQL-opties")

## <a name="next-steps"></a>Volgende stappen

Nu dat u de gegevensstroom hebt gemaakt, Voeg een [gegevensstroom uitvoeren activiteit aan uw pijplijn](https://docs.microsoft.com/azure/data-factory/concepts-data-flow-overview).
