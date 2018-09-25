---
title: Bekende problemen en probleemoplossing voor Azure Machine Learning-service
description: Een lijst van de bekende problemen, oplossingen, en problemen oplossen
services: machine-learning
author: j-martens
ms.author: jmartens
ms.reviewer: mldocs
ms.service: machine-learning
ms.component: core
ms.topic: article
ms.date: 09/24/2018
ms.openlocfilehash: 27e73bc75c5f04190bad3dab49c1d46782982a18
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47034130"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning-service"></a>Bekende problemen en oplossen van problemen met Azure Machine Learning-service
 
Dit artikel helpt u bij het zoeken en corrigeer de fouten of fouten opgetreden bij het gebruik van de Azure Machine Learning-service. 


## <a name="databricks-and-azure-machine-learning"></a>Databricks en Azure Machine Learning

**Aanbeveling voor Databricks-cluster:** 

Uw Azure Databricks-cluster maken als v4.x met Python 3. U wordt aangeraden een cluster met hoge gelijktijdigheid.
 
**AML SDK installeren op Databricks is mislukt wanneer er meer pakketten zijn geïnstalleerd** sommige pakketten, zoals `psutil upgrade libs`, kan leiden tot conflicten. Om installatiefouten te voorkomen,-pakketten door blokkering lib versie te installeren. Dit probleem is met betrekking tot Databricks en niet met betrekking tot AML SDK. Voorbeeld:
```
pstuil cryptography==1.5 pyopenssl==16.0.0 ipython=2.2.0
```


## <a name="gather-diagnostics-information"></a>Verzamelen van diagnostische gegevens
Soms kan het handig zijn als u diagnostische gegevens opgeven kunt wanneer u hulp vragen. Dit is waar de logboekbestanden bevinden:

## <a name="resource-quotas"></a>Resourcequota

Meer informatie over de [resourcequota](how-to-manage-quotas.md) optreden tijdens het werken met Azure Machine Learning.

## <a name="get-more-support"></a>Meer ondersteuning krijgen

U kunt aanvragen verzenden voor ondersteuning en hulp krijgen van de technische ondersteuning, forums en meer. [Meer informatie...](support-for-aml-services.md)
