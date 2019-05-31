---
title: Instellen van een transformatie in de functie gegevensstroom toewijzing van de bron van Azure Data Factory
description: Meer informatie over het instellen van de transformatie van een bron in de gegevensstroom toewijzen.
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 51c1ea7b554178f7fb3f264bf731ffd5872ceea2
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66234550"
---
# <a name="source-transformation-for-mapping-data-flow"></a>De transformatie van de bron voor het toewijzen van gegevensstroom 

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Een bron-transformatie configureert u de gegevensbron voor de gegevensstroom. Een gegevensstroom kan meer dan één bron transformatie bevatten. Bij het ontwerpen van stromen, begint altijd met een transformatie van de bron.

Elke gegevensstroom is vereist voor transformatie van ten minste één bron. Zo veel bronnen die nodig is voor het voltooien van uw gegevenstransformaties toevoegen. U kunt deelnemen aan deze bronnen, samen met een transformatie join of een union transformatie.

> [!NOTE]
> Wanneer u fouten uw gegevensstroom opspoort, worden gegevens gelezen uit de gegevensbron met behulp van de instelling van de lijnen of de bron-limieten voor foutopsporing. Voor het schrijven van gegevens naar een sink, moet u de gegevensstroom van een pijplijn gegevensstroom activiteit uitvoeren. 

![Opties voor transformatie op het tabblad instellingen voor gegevensbron](media/data-flow/source.png "bron")

Uw bron gegevensstroom transformatie met exact één gegevensset van Data Factory koppelen. De gegevensset definieert de shape en de locatie van de gegevens die u wilt voor lezen van of schrijven. U kunt jokertekens en de bestandsnaam lijsten gebruiken in uw bron om te werken met meer dan één bestand tegelijk.

## <a name="data-flow-staging-areas"></a>Ruimten met tijdelijke bestanden gegevensstroom

Gegevensstroom werkt met *staging* gegevenssets die bevinden zich allemaal in Azure. Deze gegevenssets gebruiken voor het Faseren van wanneer u bij het transformeren van uw gegevens. 

Data Factory heeft toegang tot bijna 80 systeemeigen connectors. Als u wilt opnemen gegevens uit de andere bronnen in de gegevensstroom, door het hulpprogramma Copy Activity te gebruiken om te zetten die gegevens in een van de gegevensstroom gegevensset fasering gebieden.

## <a name="options"></a>Opties

Schema- en steekproeven opties kiezen voor uw gegevens.

### <a name="allow-schema-drift"></a>Schema drift toestaan
Selecteer **toestaan schema drift** als de bronkolommen wordt vaak veranderen. Deze instelling kunt alle binnenkomende bronvelden langs de transformaties voor de sink.

### <a name="validate-schema"></a>Schema valideren

Als de inkomende versie van de brongegevens komt niet overeen met het schema van de opgegeven, wordt de gegevensstroom niet kunnen worden uitgevoerd.

![Instellingen voor openbare gegevensbron, met de opties voor het valideren schema toestaan schema drift en steekproeven](media/data-flow/source1.png "openbare bron 1")

### <a name="sample-the-data"></a>De voorbeeldgegevens
Schakel **steekproeven** om te beperken van het aantal rijen van de bron. Gebruik deze instelling als u testen of een sample van gegevens vanuit de bron voor foutopsporing.

## <a name="define-schema"></a>Schema definiëren

Wanneer de bronbestanden worden niet sterk getypeerd (voorbeeld, platte bestanden in plaats van voor Parquet-bestanden), definieert u de gegevenstypen voor elk veld hier in de bron-transformatie.  

![Transformatie-instellingen op het tabblad van het schema definiëren van de gegevensbron](media/data-flow/source2.png "2 van bron")

U kunt de namen van de kolommen in een select transformatie later wijzigen. Gebruik een transformatie afgeleide kolom te wijzigen van de gegevenstypen. Voor sterk getypeerde bronnen, kunt u de gegevenstypen in een later selecteren transformatie wijzigen. 

![Gegevenstypen in een select transformatie](media/data-flow/source003.png "gegevenstypen")

### <a name="optimize-the-source-transformation"></a>De bron-transformatie optimaliseren

Op de **optimaliseren** tabblad voor de bron-transformatie, ziet u mogelijk een **bron** partitietype. Deze optie is beschikbaar, alleen wanneer de bron Azure SQL Database is. Dit komt doordat Data Factory wil verbindingen te grote query's uitvoeren op de bron van uw SQL-Database maken.

![Partitie-instellingen van de gegevensbron](media/data-flow/sourcepart2.png "partitioneren")

U hoeft te partitioneren van gegevens op uw bron-SQL-Database, maar partities zijn handig voor grote query's. U kunt uw partitie baseren op een kolom of een query.

### <a name="use-a-column-to-partition-data"></a>Een kolom met partitiegegevens gebruiken

Selecteer in de brontabel een kolom toe aan de partitie op. Ook het maximum aantal verbindingen instellen.

### <a name="use-a-query-to-partition-data"></a>Gebruik van een query voor het partitioneren van gegevens

U kunt kiezen voor het partitioneren van de verbindingen op basis van een query. Voer de inhoud van een WHERE-predicaat. Bijvoorbeeld, jaar > 1980 invoeren.

## <a name="source-file-management"></a>Beheer van bron

Kies de instellingen voor het beheren van bestanden in de bron. 

![Nieuwe instellingen voor gegevensbron](media/data-flow/source2.png "nieuwe instellingen")

* **Pad met jokertekens**: Kies in de bronmap, een reeks van bestanden die overeenkomen met een patroon. Deze instelling overschrijft elk bestand in de gegevenssetdefinitie van de.
* **Lijst met bestanden**: Dit is een bestandsset. Maak een tekstbestand met een lijst van bestanden voor het verwerken van relatief pad. Verwijzen naar dit tekstbestand.
* **Kolom voor het opslaan van bestandsnaam**: De naam van het bronbestand Store in een kolom in uw gegevens. Voer hier een nieuwe naam voor het opslaan van de tekenreeks met de bestandsnaam.
* **Na voltooiing**: Wilt u niets met het bronbestand doen nadat de gegevens stroomuitvoeringen, het bronbestand verwijderen of verplaatsen van het bronbestand. De paden voor de verplaatsing zijn relatief.

### <a name="sql-datasets"></a>SQL-gegevenssets

Als de bron in SQL-Database of SQL Data Warehouse is, hebt u extra opties voor het beheer van de bron-bestand.

* **Query**: Voer een SQL-query voor de bron. Deze instelling overschrijft elke tabel die u hebt gekozen in de gegevensset. Houd er rekening mee dat **Order By** componenten hier worden niet ondersteund, maar u kunt een volledige instructie SELECT FROM instellen. U kunt ook de gebruiker gedefinieerde functies gebruiken. **Selecteer * uit udfGetData()** is een UDF in SQL die een tabel retourneert. Deze query wordt een tabel die u in uw gegevensstroom gebruiken kunt produceren.
* **Batchgrootte**: Voer een batchgrootte voor grote hoeveelheden gegevens naar leesbewerkingen wordt verdeeld.

> [!NOTE]
> Bestandsbewerkingen uitvoeren alleen als u de gegevensstroom vanuit een pijplijn worden uitgevoerd starten (een pijplijn foutopsporing of uitvoering uitvoeren) die gebruikmaakt van de activiteit gegevensstroom uitvoeren in een pijplijn. Bestand operations *niet* uitvoeren in de foutopsporingsmodus gegevensstroom.

### <a name="projection"></a>Projectie

Zoals schema's in gegevenssets bepaalt de projectie in een gegevensbron de kolommen met gegevens, typen en indelingen uit de brongegevensopslag. 

![Instellingen op het tabblad projectie](media/data-flow/source3.png "projectie")

Als uw tekstbestand geen schema opgegeven is, selecteert u **gegevenstype detecteren** zodat Data Factory wordt voorbeeld en de gegevenstypen afleiden. Selecteer **definiëren standaardindeling** om automatische detectie de standaardgegevens bestandsindelingen. 

U kunt de kolom-gegevenstypen in een transformatie later afgeleide kolom wijzigen. Gebruik een transformatie selecteren om te wijzigen van de kolomnamen.

![Instellingen voor de opmaak van de gegevens standaard](media/data-flow/source2.png "standaard indelingen")

## <a name="next-steps"></a>Volgende stappen

Beginnen met het bouwen een [afgeleide kolom transformatie](data-flow-derived-column.md) en een [transformatie Selecteer](data-flow-select.md).
