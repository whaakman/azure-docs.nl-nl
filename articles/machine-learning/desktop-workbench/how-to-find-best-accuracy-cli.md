---
title: Uitvoeringen zoeken met de beste nauwkeurigheid en de kortste duur in Azure Machine Learning Workbench | Microsoft Docs
description: Een end-to-end-use-case beste nauwkeurigheid via CLI zoeken met behulp van Azure Machine Learning Workbench
services: machine-learning
author: totekp
ms.author: kefzhou
manager: akannava
ms.reviewer: akannava, haining, mldocs, jmartens, jasonwhowell
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/29/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 094fd6d8c6c6d647533cf5409d1a85283c71c80e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46953239"
---
# <a name="find-runs-with-the-best-accuracy-and-lowest-duration"></a>Zoeken wordt uitgevoerd met de beste nauwkeurigheid en de kortste duur
Opgegeven meerdere wordt uitgevoerd, is een use-case uitgevoerd met de hoogste nauwkeurigheid vinden. Eén aanpak is het gebruik van de opdrachtregelinterface (CLI) met een [JMESPath](http://jmespath.org/) query. Zie voor meer informatie over het gebruik van JMESPath in de Azure CLI [gebruiken JMESPath-query's met Azure CLI](https://docs.microsoft.com/cli/azure/query-azure-cli?view=azure-cli-latest). In het volgende voorbeeld worden vier wordt uitgevoerd door de waarden van de nauwkeurigheid van 0, 0,98 1 en 1 gemaakt. Wordt uitgevoerd als ze zich in het bereik worden gefilterd `[MaxAccuracy-Threshold, MaxAccuracy]` waar `Threshold = .03`.

## <a name="sample-data"></a>Voorbeeldgegevens
Als u geen bestaande wordt uitgevoerd met een `Accuracy` waarde, worden de volgende stappen wordt uitgevoerd voor het uitvoeren van query's genereren.

Eerst maakt u een Python-bestand in de Azure Machine Learning Workbench, geef deze de naam `log_accuracy.py`, en plak deze in de volgende code:
```python
from azureml.logging import get_azureml_logger

logger = get_azureml_logger()

accuracy_value = 0.5

if len(sys.argv) > 1:
     accuracy_value = float(sys.argv[1])

logger.log("Accuracy", accuracy_value)
```

Maak vervolgens een bestand `run.py`, en plak deze in de volgende code:
```python
import os

accuracy_values = [0, 0.98, 1.0, 1.0]
for value in accuracy_values:
    os.system('az ml experiment submit -c local ./log_accuracy.py {}'.format(value))
```

Ten slotte de CLI via Workbench openen en voer de opdracht `python run.py` vier experimenten verzenden. Nadat het script is voltooid, ziet u vier meer uitvoeringen in de `Run History` tabblad.

## <a name="query-the-run-history"></a>Query uitvoeren op de uitvoeringsgeschiedenis
De eerste opdracht zoekt de nauwkeurigheid van max-waarde.
```powershell
az ml history list --query '@[?Accuracy != null] | max_by(@, &Accuracy).Accuracy'
```

Met behulp van deze waarde voor maximale nauwkeurigheid `1` en een drempelwaarde van `0.03`, de filters van de tweede opdracht wordt uitgevoerd met behulp van `Accuracy` en sorteert deze wordt uitgevoerd door `duration` oplopend.
```powershell
az ml history list --query '@[?Accuracy >= sum(`[1, -0.03]`)] | sort_by(@, &duration)'
```
> [!NOTE]
> Als u een strikte bovengrens controle wilt, is de queryindeling van de ``@[?Accuracy >= sum(`[$max_accuracy_value, -$threshold]`) && Accuracy <= `$max_accuracy_value`]``

Als u PowerShell gebruikt, wordt in de volgende code lokale variabelen gebruikt om op te slaan drempelwaarde en maximale precisie:
```powershell
$threshold = 0.03
$max_accuracy_value = az ml history list --query '@[?Accuracy != null] | max_by(@, &Accuracy).Accuracy'
$find_runs_query = '@[?Accuracy >= sum(`[{0}, -{1}]`)] | sort_by(@, &duration)' -f $max_accuracy_value, $threshold
az ml history list --query $find_runs_query
```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over logboekregistratie, [over het gebruik van de uitvoeringsgeschiedenis en model metrische gegevens in Azure Machine Learning Workbench](how-to-use-run-history-model-metrics.md).    
