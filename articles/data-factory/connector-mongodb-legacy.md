---
title: Gegevens kopiëren van MongoDB met Azure Data Factory | Microsoft Docs
description: Leer hoe u gegevens kopiëren van Mongo DB naar ondersteunde sink-gegevensopslag met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/20/2018
ms.author: jingwang
ms.openlocfilehash: 1ffd1b96e721707f69c47a7cbf11d60f17f3a7d2
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54105420"
---
# <a name="copy-data-from-mongodb-using-azure-data-factory"></a>Gegevens kopiëren van MongoDB met Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-on-premises-mongodb-connector.md)
> * [Huidige versie](connector-mongodb.md)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit een MongoDB-database. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

>[!IMPORTANT]
>ADF release van een nieuwe MongoDB-connector biedt betere systeemeigen MongoDB vergelijken met de voor deze implementatie op basis van ODBC ondersteuning, verwijzen naar [MongoDB-connector](connector-mongodb.md) artikel voor meer informatie. Deze oudere MongoDB-connector wordt ondersteund als bewaard-is achterwaarts compatibele voor elke nieuwe workload gebruik voor de nieuwe connector.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit de MongoDB-database kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen/put door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Om precies ondersteunt deze MongoDB-connector:

- MongoDB **versie 2.4, 2.6, 3.0, 3.2, 3.4 en 3.6**.
- Kopiëren van gegevens met **Basic** of **anoniem** verificatie.

## <a name="prerequisites"></a>Vereisten

Om gegevens te kopiëren uit een MongoDB-database die niet openbaar toegankelijk is, moet u voor het instellen van een zelfgehoste Cloudintegratieruntime. Zie [zelfgehoste Cloudintegratieruntime](create-self-hosted-integration-runtime.md) artikel voor meer informatie. De Integration Runtime biedt een ingebouwde stuurprogramma's voor MongoDB, dus hoeft u handmatig een stuurprogramma installeren bij het kopiëren van gegevens van MongoDB.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor MongoDB-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor MongoDB gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type |De eigenschap type moet worden ingesteld op: **MongoDb** |Ja |
| server |IP-adres of de hostnaam de naam van de MongoDB-server. |Ja |
| poort |TCP-poort die de MongoDB-server wordt gebruikt om te luisteren naar clientverbindingen. |Nee (de standaardinstelling is 27017) |
| databaseName |De naam van de MongoDB-database die u wilt openen. |Ja |
| authenticationType | Het type verificatie gebruikt voor verbinding met de MongoDB-database.<br/>Toegestane waarden zijn: **Basic**, en **anonieme**. |Ja |
| gebruikersnaam |Gebruikersaccount voor toegang tot MongoDB. |Ja (als basisverificatie wordt gebruikt). |
| wachtwoord |Het wachtwoord voor de gebruiker. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). |Ja (als basisverificatie wordt gebruikt). |
| authSource |De naam van de MongoDB-database die u wilt gebruiken om te controleren of uw referenties voor verificatie. |Nee. Standaard wordt het beheerdersaccount en de database die is opgegeven met behulp van de eigenschap databaseName gebruiken voor basisverificatie. |
| enableSsl | Hiermee geeft u op of de verbindingen met de server zijn versleuteld met behulp van SSL. De standaardwaarde is false.  | Nee |
| allowSelfSignedServerCert | Hiermee geeft u op of zelfondertekende certificaten van de server is toegestaan. De standaardwaarde is false.  | Nee |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt de zelfgehoste Cloudintegratieruntime of Azure Integration Runtime gebruiken (als uw gegevensarchief openbaar toegankelijk zijn is). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

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

Zie voor een volledige lijst van eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets en secties, [gegevenssets en gekoppelde services](concepts-datasets-linked-services.md). De volgende eigenschappen worden ondersteund voor MongoDB-gegevensset:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **MongoDbCollection** | Ja |
| collectionName |De naam van de verzameling in de MongoDB-database. |Ja |

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
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de MongoDB-bron.

### <a name="mongodb-as-source"></a>MongoDB als bron

De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **MongoDbSource** | Ja |
| query |Gebruik de aangepaste SQL-92-query om gegevens te lezen. Bijvoorbeeld: Selecteer * uit MyTable. |Nee (als 'collectionName' in de gegevensset is opgegeven) |

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
                "type": "MongoDbSource",
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
> Wanneer de SQL-query, let u op de datum/tijd-indeling opgeeft. Bijvoorbeeld: `SELECT * FROM Account WHERE LastModifiedDate >= '2018-06-01' AND LastModifiedDate < '2018-06-02'` of voor het gebruik van parameter `SELECT * FROM Account WHERE LastModifiedDate >= '@{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-dd HH:mm:ss')}' AND LastModifiedDate < '@{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-dd HH:mm:ss')}'`

## <a name="schema-by-data-factory"></a>Schema door Data Factory

Azure Data Factory-service schema afleidt uit een MongoDB-verzameling met behulp van de **laatste 100 documenten** in de verzameling. Als deze 100 documenten niet volledige schema bevatten, kunnen sommige kolommen worden genegeerd tijdens de kopieerbewerking.

## <a name="data-type-mapping-for-mongodb"></a>Toewijzing voor MongoDB-gegevenstype

Het kopiëren van gegevens van MongoDB, worden de volgende toewijzingen van MongoDB-gegevenstypen gebruikt om Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe copy activity in het schema en de gegevens van een brontype aan de sink toegewezen.

| MongoDB-gegevenstype | Data factory tussentijdse gegevenstype |
|:--- |:--- |
| Binair bestand |Byte[] |
| Booleaans |Booleaans |
| Date |DateTime |
| NumberDouble |Double-waarde |
| NumberInt |Int32 |
| NumberLong |Int64 |
| ObjectID |Reeks |
| Reeks |Reeks |
| UUID |GUID |
| Object |Renormalized afvlakken in kolommen met '_' als geneste scheidingsteken |

> [!NOTE]
> Raadpleeg voor meer informatie over ondersteuning voor arrays met behulp van virtuele tabellen, [ondersteuning voor complexe typen met behulp van virtuele tabellen](#support-for-complex-types-using-virtual-tables) sectie.
>
> De volgende typen van de MongoDB-gegevens worden op dit moment niet ondersteund: DBPointer, JavaScript, Max/Min sleutel, reguliere expressie, symbool, Timestamp, niet gedefinieerd.

## <a name="support-for-complex-types-using-virtual-tables"></a>Ondersteuning voor complexe typen virtuele tabellen gebruiken

Azure Data Factory maakt gebruik van een ingebouwde ODBC-stuurprogramma's verbinding maken met en gegevens kopiëren van de MongoDB-database. Voor complexe gegevenstypen zoals matrices of objecten met verschillende typen in de documenten Normaliseert het stuurprogramma opnieuw gegevens in de bijbehorende virtuele tabellen. Met name als een tabel bevat een dergelijke kolommen, genereert het stuurprogramma in de volgende virtuele tabellen:

* Een **basistabel**, die dezelfde gegevens als de echte tabel, met uitzondering van het complexe typekolommen bevat. De basistabel gebruikt dezelfde naam als de echte tabel die wordt vertegenwoordigd.
* Een **virtuele tabel** voor elke typekolom complexe waarmee de geneste gegevens wordt uitgebreid. De virtuele tabellen zijn met de naam met de naam van de echte tabel, een scheidingsteken '_' en de naam van de matrix of één object.

Virtuele tabellen verwijzen naar de gegevens in de echte tabel, waardoor het stuurprogramma voor toegang tot de Gedenormaliseerde gegevens. U kunt toegang tot de inhoud van de MongoDB-matrices door het uitvoeren van query's en deze koppelen aan de virtuele tabellen.

### <a name="example"></a>Voorbeeld

Hier ExampleTable is bijvoorbeeld een MongoDB-tabel met één kolom met een matrix met objecten in elke cel – facturen, en één kolom met een matrix met scalaire typen – classificaties.

| i_d | Naam van de klant | Facturen | Servicelaag | Beoordelingen |
| --- | --- | --- | --- | --- |
| 1111 |ABC |[{invoice_id: "123", item: "toaster", prijs: "456", korting: "0.2"}, {invoice_id: "124", item: "oven", prijs: "1235" korting: "0.2"}] |Zilver |[5,6] |
| 2222 |XYZ |[{invoice_id: "135", item: "koelkast", prijs: "12543", korting: "0.0"}] |Goudkleurig |[1,2] |

Het stuurprogramma genereert meerdere virtuele tabellen om weer te geven deze enkele tabel. De eerste virtuele tabel is de basistabel met de naam 'ExampleTable", weergegeven in het voorbeeld. De basistabel bevat alle gegevens van de oorspronkelijke tabel, maar de gegevens van de matrices is weggelaten en in de virtuele tabellen is uitgevouwen.

| i_d | Naam van de klant | Servicelaag |
| --- | --- | --- |
| 1111 |ABC |Zilver |
| 2222 |XYZ |Goudkleurig |

De volgende tabellen ziet u de virtuele tabellen waarbij de oorspronkelijke matrices in het voorbeeld. Deze tabellen bevatten de volgende:

* Een verwijzing naar de oorspronkelijke primaire-sleutelkolom overeenkomt met de rij van de oorspronkelijke matrix (via de kolom i_d)
* Een indicatie van de positie van de gegevens binnen de oorspronkelijke matrix
* De uitgebreide gegevens voor elk element in de matrix

**Tabel 'ExampleTable_Invoices':**

| i_d | ExampleTable_Invoices_dim1_idx | invoice_id | Item | price | Korting |
| --- | --- | --- | --- | --- | --- |
| 1111 |0 |123 |toaster |456 |0.2 |
| 1111 |1 |124 |oven |1235 |0.2 |
| 2222 |0 |135 |koelkast |12543 |0,0 |

**Tabel 'ExampleTable_Ratings':**

| i_d | ExampleTable_Ratings_dim1_idx | ExampleTable_Ratings |
| --- | --- | --- |
| 1111 |0 |5 |
| 1111 |1 |6 |
| 2222 |0 |1 |
| 2222 |1 |2 |


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
