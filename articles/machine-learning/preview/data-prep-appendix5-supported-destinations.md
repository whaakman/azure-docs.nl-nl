---
title: Ondersteunde gegevens doelen en uitvoer beschikbaar met voorbereiden van gegevens van Azure Machine Learning | Microsoft Docs
description: Dit document bevat een volledige lijst met ondersteunde doelen en levert als beschikbaar voor Azure Machine Learning gegevens voorbereiden
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/07/2017
ms.openlocfilehash: 50d2d481b91199630bbfbf3cfdd21a1bf3062ff0
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="supported-data-exports-for-this-preview"></a>Uitvoer van de gegevens voor deze preview ondersteund 
Het is mogelijk om te exporteren naar verschillende indelingen. U kunt deze indelingen gebruiken voor het bewaren van de tussenliggende resultaten van het voorbereiden van gegevens voordat u de resultaten in de rest van de Machine Learning-werkstroom integreren.

## <a name="types"></a>Typen 
### <a name="csv-file"></a>CSV-bestand 
Een bestand met door komma's gescheiden waarden schrijven naar de opslag.

#### <a name="options"></a>Opties
- Regeleinden
- Null-waarden met vervangen
- Fouten met vervangen 
- Scheidingsteken


### <a name="parquet"></a>Parketvloeren 
Een gegevensset niet schrijven naar de opslag als parketvloeren.

Parketvloeren als een indeling verschillende vormen in de opslag aannemen kan. Een bestand één .parquet wordt soms gebruikt voor kleinere gegevenssets. Verschillende Python-bibliotheken ondersteuning voor lezen en schrijven naar één .parquet bestanden. 

Azure Machine Learning-Workbench is op dit moment is afhankelijk van de PyArrow Python-bibliotheek voor het uitschrijven van parketvloeren tijdens het gebruik van lokale interactieve. Dit betekent dat één bestand parketvloeren is momenteel alleen parketvloeren indeling van de uitvoer die tijdens het gebruik van lokale interactieve wordt ondersteund.

Tijdens de scale-out wordt uitgevoerd (op Spark) afhankelijk Azure Machine Learning Workbench is van Spark parketvloeren lezen en schrijven van mogelijkheden. Van Spark standaardindeling uitvoer voor parketvloeren (momenteel de enige ondersteunde) lijkt qua structuur naar een Hive-gegevensset. Dit betekent dat een map veel .parquet bestanden bevat die elk een kleinere partitie van een grotere gegevensset. 

#### <a name="caveats"></a>Aanvullende opmerkingen 
Parketvloeren als een indeling relatief jonge is en sommige implementatie inconsistenties tussen verschillende tapewisselaars. Bijvoorbeeld, Spark beperkingen waarop tekens geldig in kolomnamen zijn bij het schrijven van naar parketvloeren wordt geplaatst. PyArrow doet dit niet. De volgende tekens mogen niet in een kolomnaam: 
- ,
- ;
- {}
- ()
- \\n
- \\t
- =

>[!NOTE]
>- Compatibiliteit met Spark, zodat elk gewenst moment schrijven van gegevens naar parketvloeren, instanties van deze tekens in kolomnamen worden vervangen door en onderstrepingstekens (_).
>- De consistentie tussen lokale en scale-out wordt uitgevoerd, geen gegevens geschreven naar parketvloeren, via de app, Python of Spark, heeft de kolomnamen opgeschoonde Spark compatibiliteit te garanderen. Verwijder verwachte kolomnamen bij het schrijven naar parketvloeren tekens in Spark, zodat de ongeldige ingesteld van de kolommen gebufferd voordat ze uit.



## <a name="locations"></a>Locaties 
### <a name="local"></a>Lokaal 
Lokale harde schijf of locatie voor de opslag toegewezen netwerkpad.

### <a name="azure-blob-storage"></a>Azure Blob Storage
Azure Blob storage is een Azure-abonnement vereist.

