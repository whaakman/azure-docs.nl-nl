---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 11/09/2018
ms.author: jingwang
ms.openlocfilehash: 1ab404b838af65dcb75395dfeee1ca0553e497a1
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/12/2018
ms.locfileid: "51572036"
---
## <a name="specifying-structure-definition-for-rectangular-datasets"></a>Structuurdefinitie opgeven voor rechthoekige gegevenssets
De sectie structuur in de gegevenssets JSON wordt een **optionele** sectie voor rechthoekige tabellen (met rijen en kolommen) en bevat een verzameling van kolommen voor de tabel. U gebruikt de sectie structuur voor beide met type-informatie voor typeconversies of kolomtoewijzingen doen. De volgende secties worden deze functies in detail beschreven. 

Elke kolom bevat de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| naam |Naam van de kolom. |Ja |
| type |Het gegevenstype van de kolom. Zie type conversies hieronder voor meer informatie met betrekking tot wanneer moet u informatie opgeven |Nee |
| cultuur |.NET gebaseerde cultuur moet worden gebruikt wanneer het type is opgegeven en .NET-type datum/tijd- of Datetimeoffset. De standaardwaarde is "en-us '. |Nee |
| Indeling |Tekenreeks die moet worden gebruikt bij het type is opgegeven en .NET-type datum/tijd- of Datetimeoffset-indeling. |Nee |

Het volgende voorbeeld toont de sectie met de structuur JSON voor een tabel met drie kolommen gebruikers-id, naam en lastlogindate.

```json
"structure": 
[
    { "name": "userid"},
    { "name": "name"},
    { "name": "lastlogindate"}
],
```

Gebruik de volgende richtlijnen voor wanneer 'structureren' informatie op te nemen en wat u wilt opnemen de **structuur** sectie.

* **Voor gestructureerde gegevensbronnen** dat store-schema en het type gegevens, samen met de gegevens zelf (bronnen, zoals Azure-tabel van SQL Server, Oracle, enzovoort), moet u de sectie 'structuur' alleen als u wilt kolomtoewijzing van specifieke bron doen kolommen met specifieke kolommen in de sink- en hun namen zijn niet hetzelfde zijn (Zie de informatie in de kolom koppeling hieronder). 
  
    Zoals eerder vermeld, is de informatie is optioneel in de sectie "structuur". Voor gestructureerde gegevensbronnen, informatie over het type is al beschikbaar als onderdeel van de definitie van de gegevensset in het gegevensarchief, dus u moet geen informatie over het type wanneer u de sectie "structuur" opgeeft.
* **Voor het schema op lezen gegevensbronnen (name Azure blob)** kunt u voor het opslaan van gegevens zonder een schema of het type informatie met de gegevens op te slaan. Voor deze typen gegevensbronnen moet u "structuur" opnemen in de volgende gevallen 2:
  * Wilt u doen in de kolomtoewijzing.
  * Als de gegevensset een bron in een kopieeractiviteit is, kunt u informatie in "structuur" en data factory wordt deze informatie worden gebruikt voor conversie naar native-typen voor de sink. Zie [gegevens van en naar Azure Blob verplaatsen](../articles/data-factory/v1/data-factory-azure-blob-connector.md) artikel voor meer informatie.

### <a name="supported-net-based-types"></a>Ondersteund. Op basis van de NET typen
Data factory ondersteunt de volgende CLS compatibele .NET op basis van typewaarden voor het ontwikkelen van type informatie in "structuur" voor het schema op meer gegevensbronnen, zoals Azure blob.

* Int16
* Int32 
* Int64
* Enkelvoudig
* Double-waarde
* decimaal
* Byte[]
* BOOL
* Reeks 
* GUID
* Datum en tijd
* Datetimeoffset
* Periode 

U kunt eventueel ook 'Cultuur' & '-indeling'-tekenreeks in het kader van uw aangepaste datum/tijd-tekenreeks parseren voor datum/tijd & Datetimeoffset opgeven. Zie het voorbeeld voor conversie van onderstaande.

