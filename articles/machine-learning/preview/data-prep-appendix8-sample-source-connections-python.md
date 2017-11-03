---
title: Voorbeeld extra bron gegevensverbindingen mogelijk met voorbereiden van gegevens van Azure Machine Learning | Microsoft Docs
description: Dit document bevat een reeks voorbeelden van de bron-gegevensverbindingen die mogelijk met Azure Machine Learning gegevens voorbereiden zijn
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/11/2017
ms.openlocfilehash: 9aaf4329b25cb189146949afed89cf15619ba592
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="sample-of-custom-source-connections-python"></a>Voorbeeld van aangepaste bron-verbindingen (Python) 
Lees voordat u deze bijlage lezen, [Python uitbreidbaarheid overzicht](data-prep-python-extensibility-overview.md).

## <a name="load-data-from-dataworld"></a>Gegevens uit data.world laden

### <a name="prerequisites"></a>Vereisten

#### <a name="register-yourself-at-dataworld"></a>Uzelf op data.world registreren
U moet een API-token van de website data.world.

#### <a name="install-dataworld-library"></a>Bibliotheek data.world installeren

De opdrachtregelinterface Azure Machine Learning Workbench openen door te selecteren **bestand** > **Open opdrachtregelinterface**.

```console
pip install git+git://github.com/datadotworld/data.world-py.git
```

Voer vervolgens `dw configure` op de opdrachtregel die wordt u gevraagd uw token. Wanneer u uw token, een .dw / map wordt gemaakt in de basismap en uw token is opgeslagen.

```
API token (obtained at: https://data.world/settings/advanced): <enter API token here>
```
Nu moet u kunnen data.world bibliotheken importeren.

#### <a name="load-data-into-data-preparation"></a>Gegevens laden in het voorbereiden van gegevens

Maak een nieuwe gegevens op basis van een script-stroom. Het volgende script vervolgens gebruiken voor het laden van de gegevens van data.world.

```python
#paths = df['Path'].tolist()

import datadotworld as dw

#Load the dataset.
lds = dw.load_dataset('data-society/the-simpsons-by-the-data')

#Load specific data frame from the dataset.
df = lds.dataframes['simpsons_episodes']

```

## <a name="load-azure-cosmos-db-data-into-data-preparation"></a>Azure DB die Cosmos-gegevens laden in het voorbereiden van gegevens

Een nieuwe stroom van gegevens op basis van een script maken en het volgende script vervolgens gebruiken voor het laden van de gegevens uit Azure Cosmos DB. (De bibliotheken moeten eerst worden geïnstalleerd. (Zie het vorige referentiedocument die we koppelen aan voor meer informatie.)

```python
import pydocumentdb
import pydocumentdb.document_client as document_client

import pandas as pd

config = { 
    'ENDPOINT': '<Endpoint>',
    'MASTERKEY': '<Key>',
    'DOCUMENTDB_DATABASE': '<DBName>',
    'DOCUMENTDB_COLLECTION': '<collectionname>'
};

# Initialize the Python DocumentDB client.
client = document_client.DocumentClient(config['ENDPOINT'], {'masterKey': config['MASTERKEY']})

# Read databases and take first since id should not be duplicated.
db = next((data for data in client.ReadDatabases() if data['id'] == config['DOCUMENTDB_DATABASE']))

# Read collections and take first since id should not be duplicated.
coll = next((coll for coll in client.ReadCollections(db['_self']) if coll['id'] == config['DOCUMENTDB_COLLECTION']))

docs = client.ReadDocuments(coll['_self'])

df = pd.DataFrame(list(docs))
```
