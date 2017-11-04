## <a name="specifying-structure-definition-for-rectangular-datasets"></a>Structuurdefinitie voor de rechthoekige gegevenssets opgeven
De sectie structuur in de gegevenssets JSON is een **optionele** sectie voor rechthoekige tabellen (met rijen en kolommen) en bevat een verzameling van kolommen voor de tabel. U gebruikt de sectie structuur voor beide met type-informatie voor typeconversies of kolomtoewijzingen doen. De volgende secties worden deze functies in detail beschreven. 

Elke kolom bevat de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| naam |De naam van de kolom. |Ja |
| type |Het gegevenstype van de kolom. Zie type conversies hieronder voor meer details met betrekking tot wanneer moet u informatie opgeven |Nee |
| Cultuur |.NET gebaseerde cultuur moet worden gebruikt wanneer het type is opgegeven en .NET-type Datetime of Datetimeoffset. Standaardwaarde is "en-us '. |Nee |
| Indeling |Indeling van tekenreeks moet worden gebruikt wanneer het type is opgegeven en .NET-type Datetime of Datetimeoffset. |Nee |

Het volgende voorbeeld ziet u de sectie met de structuur JSON voor een tabel met drie kolommen userid, naam en lastlogindate.

```json
"structure": 
[
    { "name": "userid"},
    { "name": "name"},
    { "name": "lastlogindate"}
],
```

Gebruik de volgende richtlijnen voor wanneer 'structuur,' informatie op te nemen en wat u wilt opnemen de **structuur** sectie.

* **Voor gestructureerde gegevensbronnen** dat store schema en het type gegevens samen met de gegevens zelf (bronnen, zoals Azure-tabel van SQL Server, Oracle, enzovoort), moet u de sectie 'structuur' alleen als u wilt kolomtoewijzing van specifieke bron doen kolommen aan specifieke kolommen in sink en hun namen komen niet overeen (Zie de details in de kolom toewijzing hieronder). 
  
    Zoals eerder vermeld, is de type-informatie optioneel in de sectie 'structuur'. Voor gestructureerde bronnen al type-informatie beschikbaar is als onderdeel van de definitie van de gegevensset in de gegevensopslag, dus u mag geen bevatten informatie over wanneer u de sectie "structuur" opneemt.
* **Voor schema op lezen gegevensbronnen (specifiek Azure-blobopslag)** kunt u gegevens opslaan zonder een schema of het type informatie met de gegevens op te slaan. Voor deze soorten gegevensbronnen moet u 'structuur' opnemen in de volgende gevallen 2:
  * U wilt doen kolomtoewijzing.
  * Wanneer de dataset een bron in een kopieeractiviteit is, kunt u informatie in 'structuur' en gegevensfactory deze type-informatie wordt gebruikt voor conversie naar native-typen voor de sink. Zie [gegevens verplaatsen en naar Azure Blob](../articles/data-factory/v1/data-factory-azure-blob-connector.md) artikel voor meer informatie.

### <a name="supported-net-based-types"></a>Ondersteund. Op basis van NET typen
Data factory ondersteunt de volgende CLS compatibele .NET op basis van typewaarden voor het bieden van informatie in 'structuur' voor schema op lezen gegevensbronnen zoals Azure blob.

* Int16
* Int32 
* Int64
* Één
* dubbele
* Decimale
* Byte]
* BOOL
* Tekenreeks 
* GUID
* Datum/tijd
* DateTimeOffset
* Periode 

U kunt eventueel ook 'Cultuur' & 'indeling' string vergemakkelijkt het parseren van de aangepaste datum/tijd-tekenreeks voor datum/tijd & Datetimeoffset opgeven. Zie voorbeeld voor typeconversie hieronder.

