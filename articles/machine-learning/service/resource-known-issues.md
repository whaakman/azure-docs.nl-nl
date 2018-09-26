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
ms.openlocfilehash: d84040dc440c373ae9bae6dbac7a95109a387ba7
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47162743"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning-service"></a>Bekende problemen en oplossen van problemen met Azure Machine Learning-service
 
Dit artikel helpt u bij het zoeken en corrigeer de fouten of fouten opgetreden bij het gebruik van de Azure Machine Learning-service. 


## <a name="databricks"></a>Databricks

Problemen met Databricks en Azure Machine Learning.

1. Aanbeveling voor Databricks-cluster:
   
   Uw Azure Databricks-cluster maken als v4.x met Python 3. U wordt aangeraden een cluster met hoge gelijktijdigheid.
 
1. AML SDK installeren is mislukt op Databricks wanneer meer pakketten zijn geïnstalleerd.

   Sommige pakketten, zoals `psutil upgrade libs`, kan leiden tot conflicten. Om installatiefouten te voorkomen,-pakketten door blokkering lib versie te installeren. Dit probleem is met betrekking tot Databricks en niet met betrekking tot AML SDK. Voorbeeld:
   ```python
   pstuil cryptography==1.5 pyopenssl==16.0.0 ipython=2.2.0
   ```

## <a name="gather-diagnostics-information"></a>Verzamelen van diagnostische gegevens
Soms kan het handig zijn als u diagnostische gegevens opgeven kunt wanneer u hulp vragen. Dit is waar de logboekbestanden bevinden:

## <a name="resource-quotas"></a>Resourcequota

Meer informatie over de [resourcequota](how-to-manage-quotas.md) optreden tijdens het werken met Azure Machine Learning.

## <a name="get-more-support"></a>Meer ondersteuning krijgen

U kunt aanvragen verzenden voor ondersteuning en hulp krijgen van de technische ondersteuning, forums en meer. [Meer informatie...](support-for-aml-services.md)
