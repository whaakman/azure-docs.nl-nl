---
title: Voorbeeld extra bron gegevensverbindingen mogelijk met voorbereiden van gegevens van Azure Machine Learning | Microsoft Docs
description: Dit document bevat een reeks voorbeelden van de bron-gegevensverbindingen die mogelijk met Azure Machine Learning gegevens voorbereiden zijn
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
ms.date: 02/01/2018
ms.openlocfilehash: 66c356d6d42254e7443b645bff3393daca67012b
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
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

Maak een transformatie transformeren gegevensstroom (Script). Het volgende script vervolgens gebruiken voor het laden van de gegevens van data.world.

```python
#paths = df['Path'].tolist()

import datadotworld as dw

#Load the dataset.
lds = dw.load_dataset('data-society/the-simpsons-by-the-data')

#Load specific data frame from the dataset.
df = lds.dataframes['simpsons_episodes']

```
## <a name="azure-cosmos-db-as-a-data-source-connection"></a>Azure Cosmos-database als een gegevensbronverbinding
Lees voor een voorbeeld van Azure Cosmos-database als een gegevensverbinding [Load Azure Cosmos DB als een gegevensbronverbinding](data-prep-load-azure-cosmos-db.md)
