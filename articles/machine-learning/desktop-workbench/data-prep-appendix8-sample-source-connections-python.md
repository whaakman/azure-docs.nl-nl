---
title: Voorbeeld van de aanvullende bron-gegevensverbindingen mogelijk is met het voorbereiden van gegevens van Azure Machine Learning | Microsoft Docs
description: Dit document bevat een aantal voorbeelden van bron-gegevensverbindingen die mogelijk met Azure Machine Learning-gegevensvoorbereiding zijn
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
ms.openlocfilehash: 89a30c070abe3b10414c7284bb33f2c8216ee0c8
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35644121"
---
# <a name="sample-of-custom-source-connections-python"></a>Voorbeeld van verbindingen voor aangepaste gegevensbronnen (Python) 
Lees voordat u deze bijlage lezen, [Python uitbreidbaarheid overzicht](data-prep-python-extensibility-overview.md).

## <a name="load-data-from-dataworld"></a>Gegevens laden van data.world

### <a name="prerequisites"></a>Vereisten

#### <a name="register-yourself-at-dataworld"></a>Uzelf te registreren bij data.world
U moet een API-token van de data.world-website.

#### <a name="install-dataworld-library"></a>Data.world-bibliotheek installeren

Open de opdrachtregelinterface van Azure Machine Learning Workbench door te selecteren **bestand** > **Open opdrachtregelinterface**.

```console
pip install git+git://github.com/datadotworld/data.world-py.git
```

Voer vervolgens `dw configure` op de opdrachtregel, waarbij u wordt gevraagd voor uw token in. Wanneer u uw token, een .dw / directory wordt gemaakt in de basismap en het token is opgeslagen.

```
API token (obtained at: https://data.world/settings/advanced): <enter API token here>
```
Nu moet u kunnen zijn voor het importeren van data.world-bibliotheken.

#### <a name="load-data-into-data-preparation"></a>Gegevens laden in het voorbereiden van gegevens

Maak een transformatie gegevens stromen transformeren (Script). Het volgende script vervolgens gebruiken voor het laden van de gegevens van data.world.

```python
#paths = df['Path'].tolist()

import datadotworld as dw

#Load the dataset.
lds = dw.load_dataset('data-society/the-simpsons-by-the-data')

#Load specific data frame from the dataset.
df = lds.dataframes['simpsons_episodes']

```
## <a name="azure-cosmos-db-as-a-data-source-connection"></a>Azure Cosmos DB als een verbinding met de gegevensbron
Lees voor een voorbeeld van Azure Cosmos DB als een gegevensverbinding, [Load Azure Cosmos DB als een gegevensbronverbinding](data-prep-load-azure-cosmos-db.md)
