---
title: Voorbeeld van de Jupyter-notebooks met behulp van een NOAA openen gegevensset
titleSuffix: Azure Open Datasets
description: Voorbeeld van de Jupyter-notebooks voor Azure Open gegevenssets voor informatie over het open gegevenssets laden en ze gebruiken om te verrijken demogegevens gebruiken. Technieken zijn onder meer gebruik van Spark en Pandas om gegevens te verwerken.
ms.service: open-datasets
ms.topic: sample
author: cjgronlund
ms.author: cgronlun
ms.date: 05/02/2019
ms.openlocfilehash: c1b86199f13454f4785a6737b25e489d45dd53f8
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65026842"
---
# <a name="example-jupyter-notebooks-show-how-to-enrich-data-with-open-datasets"></a>Voorbeeld van de Jupyter-notebooks laten zien hoe u gegevens met Open gegevenssets duidelijker maken 
Het voorbeeld van de Jupyter-notebooks voor gegevenssets die Azure Open laten zien hoe u open gegevenssets laden en gebruik ze voor de demogegevens duidelijker maken. Technieken zijn onder meer gebruik van Apache Spark en Pandas om gegevens te verwerken.

>[!IMPORTANT]
>Als u werkt in een niet-Spark-omgeving, kunt openen gegevenssets slechts één maand van gegevens op een tijdstip met bepaalde klassen downloaden om te voorkomen dat MemoryError met grote gegevenssets.

## <a name="load-noaa-integrated-surface-database-isd-data"></a>Laden van gegevens van NOAA geïntegreerde Surface Database (RBD) 
|Notebook        | Description                                    |
|----------------|------------------------------------------------|
|[Een recente maand van Weergegevens laden in een Pandas dataframe](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-access/02-weather-to-pandas-dataframe.ipynb) | Informatie over het laden van historische weergegevens in uw favoriete Pandas dataframe. |
|[Een recente maand van weergegevens in Spark dataframe te laden](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-access/01-weather-to-spark-dataframe.ipynb) | Leer hoe u historische weergegevens in uw favoriete Spark dataframe te laden.  |

## <a name="join-demo-data-with-noaa-isd-data"></a>Demogegevens met NOAA RBD gegevens samenvoegen 
|Notebook        | Description                                    |
|----------------|------------------------------------------------|
|[Demogegevens met weergegevens - Pandas samenvoegen ](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/02-weather-join-in-pandas.ipynb) | Neem deel aan een gegevensset 1 maand demo van sensor locaties met, serverlogs, weer in een Pandas dataframe.  |
|[Demogegevens met weergegevens: Spark samenvoegen](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/01-weather-join-in-spark.ipynb) | Neem deel aan een gegevensset demo van sensor locaties met, serverlogs, weer in een Spark dataframe. |

## <a name="join-nyc-taxi-data-with-noaa-isd-data"></a>Gegevens van de NYC taxi met NOAA RBD gegevens samenvoegen 
|Notebook        | Description                                    |
|----------------|------------------------------------------------|
|[De gegevens van de fietstocht taxi verrijkt met weergegevens - Pandas](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/04-nyc-taxi-join-weather-in-pandas.ipynb) | Laden van gegevens van de groene taxi NYC (meer dan 1 maand) en het verrijken met weergegevens in een Pandas dataframe. In dit voorbeeld vervangt u de methode `get_pandas_limit` en geeft een balans tussen prestaties van de gegevens laden met de hoeveelheid gegevens.|
|[De gegevens van de fietstocht taxi verrijkt met weergegevens: Spark](https://github.com/Azure/OpenDatasetsNotebooks/blob/master/tutorials/data-join/03-nyc-taxi-join-weather-in-spark.ipynb) | NYC groen taxi gegevens laden en het verrijken met weergegevens in Spark dataframe.  |

## <a name="next-steps"></a>Volgende stappen

* [Zelfstudie: Regressie modelleren met geautomatiseerde machine learning en een open-gegevensset](tutorial-opendatasets-automl.md)
* [Python-SDK voor gegevenssets die worden geopend](https://aka.ms/open-datasets-api)
* [Azure Open gegevenssets catalogus](https://azure.microsoft.com/services/open-datasets/catalog/)
