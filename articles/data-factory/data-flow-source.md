---
title: Azure Data Factory gegevenstransformatie stroom bron toewijzen
description: Azure Data Factory gegevenstransformatie stroom bron toewijzen
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 20491981cb02e428ff4114b9456d74b0de651be8
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57569022"
---
# <a name="mapping-data-flow-source-transformation"></a>Gegevenstransformatie stroom bron toewijzen

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

De bron-transformatie configureert u een gegevensbron die u gebruiken wilt om gegevens in de gegevensstroom. Mogelijk hebt u meer dan één bron transformatie in een enkele gegevensstroom. Begin altijd het ontwerpen van uw gegevens stromen met een transformatie van de bron.

> [!NOTE]
> Elke gegevensstroom vereist ten minste één bron transformatie. Voeg zo veel extra bronnen die u nodig hebt om te voltooien, uw gegevenstransformaties. U kunt deelnemen aan deze bronnen, samen met een Join- of Union transformatie. Wanneer u fouten opsporen in uw gegevensstroom tijdens foutopsporing sessies, de gegevens worden gelezen uit de gegevensbron met behulp van de instelling van steekproeven of foutopsporing bron limieten. Echter worden, er worden geen gegevens naar een Sink moeten geschreven totdat u de gegevensstroom vanaf een pipeline-stroomactiviteit van gegevens uitvoert. 

![Opties voor transformatie](media/data-flow/source.png "bron")

Elke bron gegevensstroom transformatie moet worden gekoppeld aan één Data Factory-gegevensset. De gegevensset definieert de vorm en de locatie van uw gegevens te schrijven naar of te lezen uit. U kunt lijsten met jokertekens en bestand in uw bron om te werken met meer dan één bestand tegelijk bij het gebruik van bestandsbronnen.

## <a name="data-flow-staging-areas"></a>Ruimten met tijdelijke bestanden van de gegevensstroom

Gegevensstroom werkt met "staging" gegevenssets die bevinden zich allemaal in Azure. Deze stroom gegevens gegevenssets worden gebruikt voor gegevens om uit te voeren van uw gegevenstransformaties voor fasering. Data Factory heeft toegang tot bijna 80 verschillende systeemeigen connectors. Als u wilt opnemen gegevens uit de andere bronnen in uw gegevensstroom, eerst fase die gegevens in één van deze gebieden gegevensset fasering gegevensstroom met behulp van de Kopieeractiviteit.

## <a name="options"></a>Opties

### <a name="allow-schema-drift"></a>Schema drift toestaan
Selecteer Schema Drift toestaan als de bronkolommen wordt vaak veranderen. Deze instelling kunnen alle binnenkomende velden van de bron langs de transformaties voor de Sink.

### <a name="validate-schema"></a>Schema valideren

![Openbare gegevensbron](media/data-flow/source1.png "openbare bron 1")

Als de inkomende versie van de brongegevens komt niet overeen met het gedefinieerde schema, klikt u vervolgens mislukt de uitvoering van de gegevensstroom.

### <a name="sampling"></a>Steekproeven
Steekproeven gebruiken om te beperken van het aantal rijen van de bron.  Dit is handig bij het testen of steekproeven van gegevens vanuit de bron voor foutopsporing.

## <a name="define-schema"></a>Schema definiëren

![Bron-transformatie](media/data-flow/source2.png "2 van bron")

U moet de gegevenstypen voor elk veld hier in de bron-transformatie definiëren voor bron-bestandstypen die niet sterk getypeerde (dat wil zeggen platte bestanden in plaats van Parquet-bestanden). Vervolgens kunt u de namen van de kolommen in een transformatie selecteren en de gegevenstypen in een transformatie afgeleide kolom wijzigen. 

![Bron-transformatie](media/data-flow/source003.png "gegevenstypen")

Voor sterk getypeerde bronnen, kunt u de gegevenstypen in een latere Selecteer transformatie wijzigen. 

### <a name="optimize"></a>Optimaliseren

![Bron van partities](media/data-flow/sourcepart.png "partitioneren")

Op het tabblad optimaliseren voor de bron-transformatie ziet u een extra partities type met de naam 'Bron'. Dit wordt alleen licht-up wanneer u Azure SQL-database hebt geselecteerd als de bron. Dit komt doordat ADF parallel verbindingen wilt voor het uitvoeren van grote query's op uw Azure SQL DB-gegevensbron.

Partitioneren van gegevens op uw bron-SQL-database is optioneel, maar is handig voor grote query's. U hebt hiervoor twee opties:

### <a name="column"></a>Kolom

Selecteer een kolom van de brontabel naar partitie op. U moet ook het maximale aantal verbindingen instellen.

### <a name="query-condition"></a>Queryvoorwaarde

U kunt eventueel kiezen voor het partitioneren van de verbindingen op basis van een query. Voor deze optie, plaatst u de inhoud van een WHERE-predicaat. Dat wil zeggen, jaar > 1980

## <a name="source-file-management"></a>Beheer van bron
![Nieuwe instellingen voor gegevensbron](media/data-flow/source2.png "nieuwe instellingen")

* Jokerteken pad naar het kiezen van een reeks van bestanden uit de bronmap die overeenkomen met een patroon. Dit overschrijft elk bestand dat u hebt ingesteld in de gegevenssetdefinitie van de.
* Lijst met bestanden. Hetzelfde als een bestandsset. Verwijzen naar een tekstbestand dat u met een lijst van bestanden maakt voor het verwerken van relatief pad.
* Kolom voor het opslaan van bestandsnaam slaat de naam van het bestand van de bron in een kolom in uw gegevens. Voer hier een nieuwe naam voor het opslaan van de tekenreeks met de bestandsnaam.
* Na voltooiing (u kunt niets met het bronbestand doen nadat de gegevensstroom wordt uitgevoerd, de bron-bestanden verwijderen of verplaatsen van de bronbestanden. De paden voor verplaatsing zijn relatieve paden.

### <a name="sql-datasets"></a>SQL-gegevenssets

Wanneer u van Azure SQL DB of Azure SQL DW als de bron gebruikmaakt, hebt u extra opties.

* Query: Voer een SQL-query voor de bron. Instellen van een query wordt een tabel die u hebt gekozen in de gegevensset wordt overschreven. Houd er rekening mee dat Order By-componenten niet hier worden ondersteund.

* Batchgrootte: Voer een batchgrootte voor grote hoeveelheden gegevens in de batch-formaat leesbewerkingen wordt verdeeld.

> [!NOTE]
> De instellingen van het bestand-bewerking wordt alleen uitgevoerd wanneer de gegevensstroom wordt uitgevoerd vanuit een pijplijnuitvoering (pijplijn foutopsporing of uitvoering uitvoeren) met behulp van de activiteit gegevens stromen uitvoeren in een pijplijn. Bestandsbewerkingen niet uitgevoerd in de foutopsporingsmodus gegevensstroom.

### <a name="projection"></a>Projectie

![Projectie](media/data-flow/source3.png "projectie")

Net als bij de schema's in gegevenssets, de projectie in bron definieert de kolommen met gegevens, gegevenstypen en opmaak van gegevens uit de brongegevensopslag. Als u een tekstbestand met geen schema opgegeven hebt, klikt u op 'Detecteren Data Type' om te vragen van ADF om te voorbeeld en de gegevenstypen afleiden. U kunt instellen dat de standaardgegevens indelingen voor automatische detectie toepassen met behulp van de knop 'Standaardindeling definiëren'. U kunt de kolom-gegevenstypen in een latere afgeleide kolom-transformatie kunt wijzigen. De kolomnamen kunnen worden gewijzigd met behulp van de Select-transformatie.

![De standaardnotatie](media/data-flow/source2.png "standaard indelingen")

## <a name="next-steps"></a>Volgende stappen

Begin met het maken van de gegevenstransformatie van uw met [afgeleide kolom](data-flow-derived-column.md) en [Selecteer](data-flow-select.md).
