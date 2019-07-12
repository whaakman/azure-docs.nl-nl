---
title: Snelstartgids een notebook uitvoert op uw eigen notebook-server
titleSuffix: Azure Machine Learning service
description: Aan de slag met Azure Machine Learning Service. Gebruik uw eigen lokale notebook-server voor het uitproberen van uw werkruimte.  Uw werkruimte is het fundamentele blok in de cloud die u gebruiken om te experimenten, te trainen en implementeren van machine learning-modellen.
ms.service: machine-learning
ms.subservice: core
ms.topic: quickstart
ms.reviewer: sgilley
author: sdgilley
ms.author: sgilley
ms.date: 07/08/2019
ms.custom: seodec18
ms.openlocfilehash: 406797203a99df7e805e08ee7589148599eeffce
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67670717"
---
# <a name="quickstart-use-your-own-notebook-server-to-get-started-with-azure-machine-learning"></a>Quickstart: Gebruik uw eigen notebook-server aan de slag met Azure Machine Learning

Gebruik uw eigen Python-omgeving en de Jupyter-Notebook Server aan de slag met Azure Machine Learning-service.  Zie voor een snelstartgids met geen SDK-installatie, [Quick Start: Gebruik van een cloud-gebaseerde notebookserver aan de slag met Azure Machine Learning](quickstart-run-cloud-notebook.md).

Deze quickstart laat zien hoe u kunt de [werkruimte van Azure Machine Learning-service](concept-azure-machine-learning-architecture.md) voor het bijhouden van uw machine learning-experimenten. U Python-code wordt uitgevoerd die waarden zich in de werkruimte.

Een videoversie van deze quickstart bekijken:

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2G9N6]

Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](https://aka.ms/AMLFree).

## <a name="prerequisites"></a>Vereisten

* Een Python 3.6 notebook-server met de Azure Machine Learning-SDK geïnstalleerd
* Een werkruimte van Azure Machine Learning-service
* A workspace configuration file ( **.azureml/config.json**).

Ophalen van deze vereisten van [maken van een werkruimte van Azure Machine Learning-service](setup-create-workspace.md#sdk).



## <a name="use-the-workspace"></a>De werkruimte gebruiken

Maken van een script of een notebook te starten in dezelfde map als het configuratiebestand van de werkruimte ( **.azureml/config.json**).

### <a name="attach-to-workspace"></a>Aan werkruimte toevoegen

Deze code leest informatie van het configuratiebestand om te koppelen aan uw werkruimte.

```
from azureml.core import Workspace

ws = Workspace.from_config()
```

### <a name="log-values"></a>Log-waarden

Deze code die gebruikmaakt van de basic-API's van de SDK voor het bijhouden van experimenten uitvoeren.

1. Maak een experiment in de werkruimte.
1. Leg één waarde vast in het experiment.
1. Leg een lijst met waarden vast in het experiment.

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=useWs)]

## <a name="view-logged-results"></a>Vastgelegde resultaten weergeven

Wanneer de uitvoering is voltooid, kunt u de experimentele uitvoering weergeven in de Azure-portal. Gebruik de volgende code om een URL naar de resultaten van de laatste uitvoering af te drukken:

```python
print(run.get_portal_url())
```

Deze code retourneert een koppeling die u gebruiken kunt om de vastgelegde waarden in de Azure portal in uw browser weer te geven.

![Vastgelegde waarden in Azure Portal](./media/quickstart-run-local-notebook/logged-values.png)

## <a name="clean-up-resources"></a>Resources opschonen 

>[!IMPORTANT]
>De resources die u hier hebt gemaakt, kunnen worden gebruikt als basisvereisten voor andere Machine Learning-zelfstudies en artikelen met procedures.

Als u niet van plan bent om de resources te gebruiken die u in dit artikel hebt gemaakt, verwijdert u deze om te voorkomen dat er kosten in rekening worden gebracht.

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=delete)]

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u de resources gemaakt die u nodig hebt om mee te experimenteren en om modellen te implementeren. U hebt code in een notebook uitgevoerd en de uitvoeringsgeschiedenis voor de code in uw werkruimte in de cloud onderzocht.

> [!div class="nextstepaction"]
> [Zelfstudie: Een model trainen voor de classificatie van afbeeldingen](tutorial-train-models-with-aml.md)

U kunt ook verkennen [geavanceerdere voorbeelden op GitHub](https://aka.ms/aml-notebooks) of weergeven van de [gebruikershandleiding voor de SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).
