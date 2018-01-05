---
title: Wordt uitgevoerd met de beste nauwkeurigheid en de laagste duur in Azure Machine Learning Workbench zoeken | Microsoft Docs
description: Een end-to-end van een gebruiksvoorbeeld nauwkeurigheid via CLI zoeken met behulp van Azure Machine Learning Workbench
services: machine-learning
author: totekp
ms.author: kefzhou
manager: akannava
ms.reviewer: akannava, haining, mldocs, garyericson, jasonwhowell
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/29/2017
ms.openlocfilehash: 40e066fe602e8c4680043158f1d401a884e07c19
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="find-runs-with-the-best-accuracy-and-lowest-duration"></a>Wordt uitgevoerd met de beste nauwkeurigheid en de duur van de laagste zoeken
Opgegeven meerdere wordt uitgevoerd, is een gebruiksvoorbeeld uitgevoerd met de beste nauwkeurigheid vinden. Een aanpak is het gebruik van de opdrachtregelinterface (CLI) met een [JMESPath](http://jmespath.org/) query. Zie voor meer informatie over het gebruik van JMESPath in de Azure CLI [JMESPath gebruiken query's met Azure CLI 2.0](https://docs.microsoft.com/cli/azure/query-azure-cli?view=azure-cli-latest). In het volgende voorbeeld worden vier wordt uitgevoerd met nauwkeurigheid waarden van 0, 0,98 1 en 1 gemaakt. Wordt uitgevoerd als ze in het bereik worden gefilterd `[MaxAccuracy-Threshold, MaxAccuracy]` waar `Threshold = .03`.

## <a name="sample-data"></a>Voorbeeldgegevens
Als u geen bestaande wordt uitgevoerd met een `Accuracy` waarde, worden de volgende stappen wordt uitgevoerd voor het uitvoeren van query's genereren.

Eerst een Python-bestand maken in de Workbench van Azure Machine Learning, naam `log_accuracy.py`, en plak in de volgende code:
```python
from azureml.logging import get_azureml_logger

logger = get_azureml_logger()

accuracy_value = 0.5

if len(sys.argv) > 1:
     accuracy_value = float(sys.argv[1])

logger.log("Accuracy", accuracy_value)
```

Maak vervolgens een bestand `run.py`, en plak in de volgende code:
```python
import os

accuracy_values = [0, 0.98, 1.0, 1.0]
for value in accuracy_values:
    os.system('az ml experiment submit -c local ./log_accuracy.py {}'.format(value))
```

Ten slotte de CLI via Workbench openen en voer de opdracht `python run.py` vier experimenten verzenden. Nadat het script is voltooid, ziet u vier meer wordt uitgevoerd in de `Run History` tabblad.

## <a name="query-the-run-history"></a>Query uitvoeren op de uitvoeringsgeschiedenis
De eerste opdracht zoekt de nauwkeurigheid van de maximale waarde.
```powershell
az ml history list --query '@[?Accuracy != null] | max_by(@, &Accuracy).Accuracy'
```

Met deze waarde max nauwkeurigheid van `1` en een drempelwaarde van `0.03`, de tweede opdracht filters wordt uitgevoerd met behulp van `Accuracy` en sorteringen wordt uitgevoerd door `duration` oplopende.
```powershell
az ml history list --query '@[?Accuracy >= sum(`[1, -0.03]`)] | sort_by(@, &duration)'
```
> [!NOTE]
> Als u een strikte bovengrens controle wilt, is de queryindeling van de``@[?Accuracy >= sum(`[$max_accuracy_value, -$threshold]`) && Accuracy <= `$max_accuracy_value`]``

Als u PowerShell gebruikt, gebruikt de volgende code lokale variabelen voor het opslaan van de drempel en de maximale nauwkeurigheid:
```powershell
$threshold = 0.03
$max_accuracy_value = az ml history list --query '@[?Accuracy != null] | max_by(@, &Accuracy).Accuracy'
$find_runs_query = '@[?Accuracy >= sum(`[{0}, -{1}]`)] | sort_by(@, &duration)' -f $max_accuracy_value, $threshold
az ml history list --query $find_runs_query
```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over logboekregistratie [het gebruik van uitvoeringsgeschiedenis en model metrische gegevens in Azure Machine Learning Workbench](how-to-use-run-history-model-metrics.md).    
