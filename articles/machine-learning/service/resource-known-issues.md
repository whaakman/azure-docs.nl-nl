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
ms.date: 10/01/2018
ms.openlocfilehash: d4910eb3dfacb46efe3f85aea3a441bdaaeb1392
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48236409"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning-service"></a>Bekende problemen en oplossen van problemen met Azure Machine Learning-service
 
Dit artikel helpt u bij het zoeken en corrigeer de fouten of fouten opgetreden bij het gebruik van de Azure Machine Learning-service. 

## <a name="image-building-failure"></a>Installatiekopie samenstellen fout

De installatiekopie van het bouwen van fout bij het implementeren van web-service. Tijdelijke oplossing is om toe te voegen ' pynacl 1.2.1 == "als een afhankelijkheid pip Conda-bestand voor de configuratie van installatiekopie.  

## <a name="pipelines"></a>Pijplijnen
Een fout optreedt bij het aanroepen van PythonScriptStep meerdere keren in een rij zonder te hoeven wijzigen van het script of de parameters. Tijdelijke oplossing is het object PipelineData opnieuw opbouwen.

## <a name="fpgas"></a>FPGA 's
Niet mogelijk om te implementeren van modellen op FPGA's totdat u hebt aangevraagd en goedgekeurd voor FPGA quotum. Vul het aanvraagformulier voor de quota voor het aanvragen van toegang: https://aka.ms/aml-real-time-ai

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
