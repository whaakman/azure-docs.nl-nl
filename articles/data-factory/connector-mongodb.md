---
title: "Gegevens kopiëren van MongoDB met behulp van Azure Data Factory | Microsoft Docs"
description: "Informatie over het gegevens uit de Mongo-database kopiëren naar gegevensarchieven ondersteunde sink met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
ms.openlocfilehash: ddbd27bd832c6fc3c7a0274095d6d203ecf1092a
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-from-mongodb-using-azure-data-factory"></a>Gegevens kopiëren van MongoDB met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-on-premises-mongodb-connector.md)
> * [Versie 2 - Preview](connector-mongodb.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit een MongoDB-database. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [MongoDB-connector in V1](v1/data-factory-on-premises-mongodb-connector.md).


## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit de MongoDB-database kopiëren naar een ondersteunde sink-gegevensarchief. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder ondersteunt deze MongoDB-connector:

- MongoDB **versie 2.4, 2.6, 3.0 en 3.2**.
- Kopiëren van gegevens met **Basic** of **anoniem** verificatie.

## <a name="prerequisites"></a>Vereisten

Om gegevens te kopiëren uit een MongoDB-database die niet openbaar toegankelijk is, moet u voor het instellen van een Self-hosted integratie-Runtime. Zie [Self-hosted integratie Runtime](create-self-hosted-integration-runtime.md) artikel voor meer informatie. De Runtime-integratie biedt een ingebouwde stuurprogramma's voor MongoDB, dus u hoeft niet te handmatig installeren van een stuurprogramma bij het kopiëren van gegevens van/naar MongoDB.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met MongoDB-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor MongoDB gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type |De eigenschap type moet worden ingesteld op: **MongoDb** |Ja |
| server |IP-adres of de hostnaam de naam van de MongoDB-server. |Ja |
| poort |TCP-poort die de MongoDB-server gebruikt om te luisteren naar verbindingen van clients. |Nee (de standaardwaarde is 27017) |
| databaseName |De naam van de MongoDB-database die u wilt openen. |Ja |
| authenticationType | Het soort verificatie die wordt gebruikt voor verbinding met de MongoDB-database.<br/>Toegestane waarden zijn: **Basic**, en **anoniem**. |Ja |
| gebruikersnaam |Het gebruikersaccount voor toegang tot MongoDB. |Ja (als u basisverificatie wordt gebruikt). |
| wachtwoord |Wachtwoord voor de gebruiker. Dit veld markeren als SecureString. |Ja (als u basisverificatie wordt gebruikt). |
| authSource |De naam van de MongoDB-database die u wilt gebruiken om te controleren van uw referenties voor verificatie. |Nee. Standaard wordt het beheerdersaccount en de database die is opgegeven met behulp van de eigenschap databaseName gebruikt voor basisverificatie. |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Self-hosted integratie Runtime of Azure integratie Runtime gebruiken (als uw gegevensarchief openbaar toegankelijk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. |Nee |

**Voorbeeld:**

```json
{
    "name": "MongoDBLinkedService",
    "properties": {
        "type": "MongoDb",
        "typeProperties": {
            "server": "<server name>",
            "databaseName": "<database name>",
            "authenticationType": "Basic",
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie het artikel gegevenssets voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de MongoDB-gegevensset.

Stel de eigenschap type van de gegevensset om gegevens te kopiëren van MongoDB, **MongoDbCollection**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **MongoDbCollection** | Ja |
| CollectionName |De naam van de verzameling in de MongoDB-database. |Ja |

**Voorbeeld:**

```json
{
     "name":  "MongoDbDataset",
    "properties": {
        "type": "MongoDbCollection",
        "linkedServiceName": {
            "referenceName": "<MongoDB linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "collectionName": "<Collection name>"
        }
    }

```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door MongoDB-bron.

### <a name="mongodb-as-source"></a>MongoDB als bron

Om gegevens te kopiëren van MongoDB, stelt u het brontype in de kopieerbewerking naar **MongoDbSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **MongoDbSource** | Ja |
| query |Gebruik de aangepaste SQL-92-query om gegevens te lezen. Bijvoorbeeld: Selecteer * from MijnTabel. |Nee (als 'collectionName' in de gegevensset is opgegeven) |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromMongoDB",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<MongoDB input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "RelationalSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

> [!TIP]
> Geef bij de SQL-query, let u op de datum/tijd-indeling. Bijvoorbeeld: `$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\\'{0:yyyy-MM-dd HH:mm:ss}\\'}} AND LastModifiedDate < {{ts\\'{1:yyyy-MM-dd HH:mm:ss}\\'}}', <datetime parameter>, <datetime parameter>)`

## <a name="schema-by-data-factory"></a>Schema door Data Factory

Azure Data Factory-service schema afleidt uit een verzameling MongoDB met behulp van de **laatste 100 documenten** in de verzameling. Als deze 100 documenten niet volledig schema bevatten, kunnen sommige kolommen worden genegeerd tijdens de kopieerbewerking.

## <a name="data-type-mapping-for-mongodb"></a>Gegevenstype toewijzing voor MongoDB

Bij het kopiëren van gegevens van MongoDB, worden de volgende toewijzingen van MongoDB-gegevenstypen gebruikt voor Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en de gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe het brontype schema en de gegevens in kopieeractiviteit worden toegewezen aan de sink.

| MongoDB-gegevenstype | Data factory tussentijdse gegevenstype |
|:--- |:--- |
| Binair bestand |Byte[] |
| Boole-waarde |Boole-waarde |
| Date |Datum en tijd |
| NumberDouble |Double |
| NumberInt |Int32 |
| NumberLong |Int64 |
| ObjectID |Tekenreeks |
| Tekenreeks |Tekenreeks |
| UUID |GUID |
| Object |Renormalized plat in kolommen met '_' als geneste scheidingsteken |

> [!NOTE]
> Raadpleeg voor meer informatie over ondersteuning voor arrays met virtuele tabellen, [ondersteuning voor complexe typen virtuele tabellen met](#support-for-complex-types-using-virtual-tables) sectie.
>
> Op dit moment wordt de volgende MongoDB-gegevenstypen worden niet ondersteund: DBPointer, JavaScript, Max per minuut sleutel, reguliere expressie, symbool, Timestamp, Undefined.

## <a name="support-for-complex-types-using-virtual-tables"></a>Ondersteuning voor complexe typen met behulp van de virtuele tabellen

Azure Data Factory gebruikt een ingebouwde ODBC-stuurprogramma voor het verbinding maken met en het kopiëren van gegevens uit de MongoDB-database. Voor complexe gegevenstypen zoals matrices of objecten met verschillende typen op de documenten Normaliseert het stuurprogramma gegevens opnieuw in de bijbehorende virtuele tabellen. Als een tabel bevat een dergelijke kolommen, genereert het stuurprogramma in het bijzonder de volgende virtuele tabellen:

* Een **basistabel**, die dezelfde gegevens als de echte tabel, met uitzondering van de kolommen van het complexe type bevat. De basistabel gebruikt dezelfde naam als de echte tabel die wordt vertegenwoordigd.
* Een **virtuele tabel** voor elke kolom complex type zijn dat de geneste gegevens wordt uitgebreid. De virtuele tabellen zijn benoemde met de naam van de bestaande tabel, een scheidingsteken '_' en de naam van de matrix of object.

Virtuele tabellen verwijzen naar de gegevens in de echte tabel, waardoor het stuurprogramma voor toegang tot de gedenormaliseerd gegevens. U kunt toegang tot de inhoud van MongoDB matrices door het uitvoeren van query's en het toevoegen aan de virtuele tabellen.

### <a name="example"></a>Voorbeeld

Hier ExampleTable is bijvoorbeeld een MongoDB-tabel met één kolom met een matrix van objecten in elke cel – facturen en één kolom met een matrix van scalaire typen – classificaties.

| _id | Naam van de klant | Facturen | Servicelaag | De classificaties |
| --- | --- | --- | --- | --- |
| 1111 |ABC |[{invoice_id: '123' item: 'toaster', de prijs: '456' korting: "0,2"}, {invoice_id: '124' item: 'ingesteld', prijs: '1235' korting: "0,2"}] |Zilver |[5,6] |
| 2222 |XYZ |[{invoice_id: item '135': 'koelkast', prijs: '12543' korting: "0,0"}] |Goudkleurig |[1,2] |

Het stuurprogramma genereert meerdere virtuele tabellen te deze één tabel vertegenwoordigen. De eerste virtuele tabel is de basistabel met de naam 'ExampleTable', weergegeven in het voorbeeld. De basistabel bevat alle gegevens van de oorspronkelijke tabel, maar de gegevens van de matrices is weggelaten en in de virtuele tabellen is uitgevouwen.

| _id | Naam van de klant | Servicelaag |
| --- | --- | --- |
| 1111 |ABC |Zilver |
| 2222 |XYZ |Goudkleurig |

De volgende tabellen tonen de virtuele tabellen waarbij de oorspronkelijke matrices in het voorbeeld. Deze tabellen bevatten het volgende:

* Een verwijzing naar de oorspronkelijke primaire-sleutelkolom overeenkomt met de rij van de oorspronkelijke matrix (via de kolom _id)
* Een indicatie van de positie van de gegevens in de oorspronkelijke matrix
* De uitgebreide gegevens voor elk element in de matrix

**Tabel 'ExampleTable_Invoices':**

| _id | ExampleTable_Invoices_dim1_idx | invoice_id | item | price | Korting |
| --- | --- | --- | --- | --- | --- |
| 1111 |0 |123 |toaster |456 |0.2 |
| 1111 |1 |124 |oven |1235 |0.2 |
| 2222 |0 |135 |koelkast |12543 |0.0 |

**Tabel 'ExampleTable_Ratings':**

| _id | ExampleTable_Ratings_dim1_idx | ExampleTable_Ratings |
| --- | --- | --- |
| 1111 |0 |5 |
| 1111 |1 |6 |
| 2222 |0 |1 |
| 2222 |1 |2 |


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).