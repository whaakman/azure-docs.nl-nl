---
title: Instellen van een transformatie sink in de functie gegevensstroom toewijzing van Azure Data Factory
description: Meer informatie over het instellen van een transformatie sink in de toewijzing van gegevensstroom.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: 4341cbb0e24330d535f5211c088f0068eab33af7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65596262"
---
# <a name="sink-transformation-for-a-data-flow"></a>Sink-transformatie voor een gegevensstroom

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Nadat u uw gegevensstroom transformeren, kunt u de gegevens naar een bestemming gegevensset sink. Kies in de sink-transformatie, de gegevenssetdefinitie van een voor de uitvoergegevens van de bestemming. U kunt hebben als veel transformaties sink als de gegevensstroom is vereist.

Aan het account voor schema drift en wijzigingen in de binnenkomende gegevens sink van de uitvoergegevens naar een map zonder een schema opgegeven in de uitvoergegevensset. U kunt ook rekening gehouden met kolom wijzigingen in uw bronnen hiervoor **toestaan schema drift** in de bron. Vervolgens velden automatisch toewijzen alle in de sink.

![Opties op het tabblad Sink, met inbegrip van de optie automatisch toewijzen](media/data-flow/sink1.png "sink-1")

Sink-alle binnenkomende velden, schakel **toewijzing automatisch**. Als u de velden om op te vangen naar de bestemming of te wijzigen van de namen van de velden op de bestemming, uit te schakelen **toewijzing automatisch**. Open vervolgens de **toewijzing** tabblad uitvoervelden toe te wijzen.

![Opties op het tabblad toewijzing](media/data-flow/sink2.png "sink-2")

## <a name="output"></a>Uitvoer 
Voor Azure Blob storage of Data Lake Storage-sink typen, voert u de getransformeerde gegevens naar een map. Spark genereert gepartitioneerde gegevens uitvoerbestanden op basis van het partitieschema die gebruikmaakt van de sink-transformatie. 

U kunt instellen dat het partitieschema van de **optimaliseren** tabblad. Als u Data Factory voor het samenvoegen van de uitvoer in een enkel bestand, selecteer **één partitie**.

![Opties op het tabblad optimaliseren](media/data-flow/opt001.png "sink-opties")

## <a name="field-mapping"></a>Veldtoewijzing

Op de **toewijzing** tabblad van de sink-transformatie, kunt u de binnenkomende kolommen aan de linkerkant toewijzen aan de bestemmingen aan de rechterkant. Wanneer u sink-gegevensstromen naar bestanden, wordt Data Factory altijd nieuwe bestanden schrijven naar een map. Wanneer u aan een database-gegevensset toewijst, kunt u een nieuwe tabel die gebruikmaakt van dit schema door in te stellen genereren **beleid opslaan** naar **overschrijven**. Of nieuwe rijen in een bestaande tabel invoegen en vervolgens de velden toewijzen aan de bestaande schema's. 

![Het tabblad toewijzing](media/data-flow/sink2.png "Sinks")

In de tabel met toewijzingen kunt u meervoudige selectie voor het koppelen van meerdere kolommen, loskoppelen van meerdere kolommen of meerdere rijen worden toegewezen aan de naam van de dezelfde kolom.

Altijd de binnenkomende set velden toewijzen aan een doel, zoals ze zijn en volledig accepteren flexibele schemadefinities, selecteer **toestaan schema drift**.

![Het tabblad toewijzing met velden die zijn toegewezen aan kolommen in de gegevensset](media/data-flow/multi1.png "meerdere opties")

Als u uw kolomtoewijzingen herstellen, selecteert u **opnieuw toewijzen**.

![Het tabblad Sink](media/data-flow/sink1.png "een Sink")

Selecteer **valideren schema** de sink mislukken als het schema wordt gewijzigd.

Selecteer **schakelt u de map** afkappen van de inhoud van de sink-map voor het schrijven van de doel-bestanden in die map.

## <a name="file-name-options"></a>Opties voor bestandsnamen

Instellen van het benoemen van bestanden: 

   * **Standaard**: Spark op de naam van bestanden op basis van standaardinstellingen voor onderdeel toestaan.
   * **Patroon**: Geef een patroon voor de uitvoerbestanden. Bijvoorbeeld, **leningen [n]** maakt loans1.csv, loans2.csv, enzovoort.
   * **Per partitie**: Geef een bestandsnaam per partitie.
   * **Als gegevens in de kolom**: Het uitvoerbestand ingesteld op de waarde van een kolom.
   * **Uitvoer naar een enkel bestand**: Met deze optie wordt ADF de gepartitioneerde uitvoerbestanden combineren tot één bestand met de naam. Als u wilt deze optie gebruikt, moet uw gegevensset worden omgezet naar een mapnaam. Ook, houd er rekening mee dat deze samenvoeging kan mogelijk niet op basis van de grootte van knooppunt.

> [!NOTE]
> Start operations bestand alleen als u de gegevensstroom uitvoeren activiteit uitvoert. Ze starten in modus gegevens Flow foutopsporing niet.

## <a name="database-options"></a>Opties voor de database

Database-instellingen kiezen:

* **Werk de methode bij**: De standaardwaarde is om te kunnen worden ingevoegd. Schakel **toestaan insert** als u wilt stoppen met het invoegen van nieuwe rijen vanuit de bron. Als u wilt bijwerken, upsert, of verwijderen van rijen, moet u eerst een transformatie alter-rij toevoegen aan de rijen van de code voor deze acties. 
* **Opnieuw maken van tabel**: Niet verwijderen of uw doeltabel maken voordat de gegevensstroom is voltooid.
* **Afkappen tabel**: Alle rijen uit de doeltabel verwijderen voordat de gegevensstroom is voltooid.
* **Batchgrootte**: Voer een getal en bucket schrijfbewerkingen in segmenten. Gebruik deze optie voor grote hoeveelheden gegevens geladen. 
* **Faseringsmodus inschakelen**: PolyBase gebruiken wanneer u Azure Data Warehouse als de sink-gegevensset laden.

![Het tabblad instellingen met de SQL-sink-opties die](media/data-flow/alter-row2.png "SQL-opties")

> [!NOTE]
> In de gegevensstroom, kunt u Data Factory te maken van een nieuwe definitie van de tabel in de doeldatabase sturen. Voor het maken van de definitie van de tabel, stelt u een gegevensset in de sink-transformatie die de naam van een nieuwe tabel heeft. Selecteer in de SQL-gegevensset, onder de naam van de tabel **bewerken** en voer de naam van een nieuwe tabel. In de sink-transformatie, schakelt u **toestaan schema drift**. Stel **schema importeren** naar **geen**.

![SQL-gegevensset-instellingen, die laat zien waar de tabelnaam bewerken](media/data-flow/dataset2.png "SQL-Schema")

> [!NOTE]
> Als u bijwerken of verwijderen van rijen in de database-sink, moet u de sleutelkolom instellen. Deze instelling kan de transformatie alter-rij om te bepalen van de unieke rij in de bibliotheek voor gegevensverplaatsing (DML).

## <a name="next-steps"></a>Volgende stappen

Nu dat u de gegevensstroom hebt gemaakt, Voeg een [gegevensstroom activiteit aan uw pijplijn](concepts-data-flow-overview.md).
