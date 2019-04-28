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
ms.date: 03/21/2019
ms.custom: seodec18
ms.openlocfilehash: 3afea20fe02eafbf14b5162eef3a198d27140b9e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60753015"
---
# <a name="quickstart-use-your-own-notebook-server-to-get-started-with-azure-machine-learning"></a>Quickstart: Gebruik uw eigen notebook-server aan de slag met Azure Machine Learning

Uw eigen notebook-server gebruiken om code waarmee waarden zich in de [werkruimte van Azure Machine Learning-service](concept-azure-machine-learning-architecture.md). De werkruimte is het basisblok in de cloud dat u gebruikt voor het experimenteren met en trainen en implementeren van machine learning-modellen met Machine Learning.

In deze snelstartgids maakt gebruik van uw eigen Python-omgeving en Jupyter Notebook-Server. Zie voor een snelstartgids met geen SDK-installatie, [Quick Start: Een cloud-gebaseerde notebookserver aan de slag met Azure Machine Learning gebruiken](quickstart-run-cloud-notebook.md) 

Een videoversie van deze quickstart bekijken:

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2G9N6]

Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](https://aka.ms/AMLFree).

## <a name="prerequisites"></a>Vereisten

* Een Python 3.6 notebook-server met de Azure Machine Learning-SDK geïnstalleerd
* Een werkruimte van Azure Machine Learning-service
* A workspace configuration file (**.azureml/config.json** ).

Ophalen van deze vereisten van [maken van een werkruimte van Azure Machine Learning-service](setup-create-workspace.md#portal).


## <a name="use-the-workspace"></a>De werkruimte gebruiken

Maken van een script of start een notitieblok in dezelfde map als het configuratiebestand van uw werkruimte. Deze code die gebruikmaakt van de basic-API's van de SDK voor het bijhouden van experimenten uitvoeren.

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
