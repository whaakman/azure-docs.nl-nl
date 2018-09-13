---
title: Ondersteunde gegevensbestemmingen en uitvoer beschikbaar voor gegevensvoorbereiding van Azure Machine Learning | Microsoft Docs
description: Dit document bevat een volledige lijst van ondersteunde bestemmingen en levert beschikbaar voor gegevensvoorbereiding van Azure Machine Learning
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: 123328010758eea6e7eadce29440e204f91dcef6
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35643767"
---
# <a name="supported-data-exports-for-this-preview"></a>Uitvoer van de gegevens voor deze Preview-versie ondersteund 
Het is mogelijk om te exporteren naar verschillende indelingen. U kunt deze indelingen behouden van de tussenliggende resultaten van de gegevens voor te bereiden voordat u de resultaten in de rest van de Machine Learning-werkstromen integreren.

## <a name="types"></a>Typen 
### <a name="csv-file"></a>CSV-bestand 
Een bestand met door komma's gescheiden waarden schrijven naar de opslag.

#### <a name="options"></a>Opties
- Regeleinden
- Null-waarden met vervangen
- Fouten met vervangen 
- Scheidingsteken


### <a name="parquet"></a>Parquet 
Een gegevensset schrijven naar de opslag als Parquet.

Parquet als een indeling verschillende vormen in de opslag aannemen kan. Een bestand één .parquet wordt soms gebruikt voor kleinere gegevenssets. Verschillende Python-bibliotheken ondersteuning voor lezen en schrijven naar één .parquet bestanden. 

Azure Machine Learning Workbench is momenteel, afhankelijk van de PyArrow Python-clientbibliotheek voor het schrijven van Parquet tijdens lokale interactief gebruik. Dit betekent dat één bestand Parquet zich momenteel de enige Parquet-uitvoerindeling die tijdens het gebruik van lokale interactieve wordt ondersteund.

Tijdens de scale-out wordt uitgevoerd (op Spark) afhankelijk Azure Machine Learning Workbench van van Spark Parquet lezen en schrijven van mogelijkheden. De standaarduitvoerindeling van Spark voor Parquet (momenteel het enige account ondersteund) is lijkt qua structuur op een Hive-gegevensset. Dit betekent dat een map bevat veel .parquet-bestanden die elk een kleinere partitie van een grotere gegevensset. 

#### <a name="caveats"></a>Aanvullende opmerkingen 
Parquet als een indeling nog relatief jong is en sommige implementatie-inconsistenties tussen verschillende tapewisselaars. Spark plaatst bijvoorbeeld beperkingen op welke tekens geldig in de namen van kolommen zijn bij het schrijven van naar Parquet. PyArrow doet dit niet. In een kolomnaam mag niet de volgende tekens bevatten: 
- , 
- ;
- {}
- ()
- \\n
- \\t
- =

>[!NOTE]
>- Om ervoor te zorgen compatibiliteit met Spark, telkens wanneer die u gegevens naar Parquet schrijven, instanties van deze tekens in de namen van kolommen zijn vervangen door en onderstrepingstekens (_).
>- Als u wilt zorgen voor consistentie tussen lokale en scale-out wordt uitgevoerd, geen gegevens geschreven naar Parquet, via de app, Python of Spark, heeft de kolomnamen opgeschoond Spark compatibiliteit te garanderen. Om ervoor te zorgen verwachte kolomnamen bij het schrijven naar een Parquet-tekens in Spark, verwijder de ongeldige uit de kolommen instellen voordat u ze uitschrijven van.



## <a name="locations"></a>Locaties 
### <a name="local"></a>Lokaal 
Lokale harde schijf of toegewezen netwerk opslaglocatie.

### <a name="azure-blob-storage"></a>Azure Blob Storage
Azure Blob-opslag is een Azure-abonnement vereist.

