---
title: 'Snelstart: De Python-SDK gebruiken om een werkruimte voor de Machine Learning service te maken - Azure Machine Learning'
description: Aan de slag met Azure Machine Learning.  Installeer de Python-SDK en gebruik deze om een werkruimte te maken. Deze werkruimte is het basisblok in de cloud voor het experimenteren, trainen en implementeren van machine learning-modellen met de Azure Machine Learning-service.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: quickstart
ms.reviewer: sgilley
author: hning86
ms.author: haining
ms.date: 09/24/2018
ms.openlocfilehash: ee24c1797d0f52d2529ed583a0cfe90cc9e27035
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "49067753"
---
# <a name="quickstart-use-python-to-get-started-with-azure-machine-learning"></a>Snelstart: Python gebruiken om aan de slag te gaan met Azure Machine Learning

In deze snelstart gebruikt u de Azure Machine Learning-SDK voor Python om een [werkruimte](concept-azure-machine-learning-architecture.md) voor de Machine Learning service te maken. Deze werkruimte is het basisblok in de cloud voor het experimenteren, trainen en implementeren van machine learning-modellen in de cloud met de Azure Machine Learning-service.

In deze zelfstudie gaat u de Python-SDK installeren en:
* Een werkruimte maken in uw Azure-abonnement
* Een configuratiebestand voor die werkruimte maken voor later gebruik in andere notitieblokken en scripts
* Code schrijven om waarden te schrijven in de werkruimte
* De vastgelegde waarden in uw werkruimte weergeven

De werkruimte en het configuratiebestand die u maakt in deze snelstartgids, kunnen worden gebruikt als vereisten voor andere Azure Machine Learning-zelfstudies en artikelen met procedures. Net als bij andere Azure-services zijn er limieten en quota's voor de Azure Machine Learning-service. [Meer informatie over quota's en hoe u meer kunt aanvragen.](how-to-manage-quotas.md)

Om het u gemakkelijk te maken, worden de volgende Azure-resources automatisch toegevoegd aan uw werkruimte wanneer deze regionaal beschikbaar zijn: [containerregister](https://azure.microsoft.com/services/container-registry/), [opslag](https://azure.microsoft.com/services/storage/), [toepassingsinzichten](https://azure.microsoft.com/services/application-insights/) en [sleutelkluis](https://azure.microsoft.com/services/key-vault/).

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.


##  <a name="install-the-sdk"></a>De SDK installeren

**Sla deze sectie over als u gebruikmaakt** van een Data Science Virtual Machine (DSVM) die is gemaakt na 27 september 2018, want deze DSVM's worden geleverd inclusief de Python-SDK.

Voordat u de SDK installeert, raden we u aan om eerst een geïsoleerde omgeving voor Python te maken. Tijdens deze snelstart maakt u gebruik van [Miniconda](https://conda.io/docs/user-guide/install/index.html), maar u kunt ook gebruikmaken van volledig geïnstalleerde [Anaconda](https://www.anaconda.com/) of [Python virtualenv](https://virtualenv.pypa.io/en/stable/).

### <a name="install-miniconda"></a>Miniconda installeren


[Download](https://conda.io/miniconda.html) en installeer Miniconda. Kies de versie Python 3.7 of hoger. Kies niet de versie Python 2.x.

### <a name="create-an-isolated-python-environment"></a>Een geïsoleerde omgeving voor Python maken 

Start een opdrachtregelvenster en maak een nieuwe conda-omgeving met de naam `myenv` met Python 3.6.

```sh
conda create -n myenv -y Python=3.6
```

Activeer de omgeving.

  ```sh
  conda activate myenv
  ```

### <a name="install-the-sdk"></a>De SDK installeren

Installeer de SDK in de geactiveerde conda-omgeving. Deze code installeert de belangrijkste onderdelen van de Azure Machine Learning-SDK en een Jupyter Notebook-server in de `myenv` conda-omgeving.  De installatie duurt **ongeveer 4 minuten**.

```sh
pip install azureml-sdk[notebooks]
```

## <a name="create-a-workspace"></a>Een werkruimte maken

Start Jupyter Notebook door deze opdracht te typen.
```sh
jupyter notebook
```

Maak een nieuw notitieblok met behulp van de `Python 3`-kernel in het browservenster. 

Geef de SDK-versie weer door de volgende Python-code in een notitieblokcel te typen en voer de versie uit.

```python
import azureml.core
print(azureml.core.VERSION)
```

Maak een nieuwe Azure-resourcegroep en een nieuwe werkruimte.

Zoek een waarde voor `<azure-subscription-id>` in de [abonnementenlijst in Azure Portal](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). U kunt elk abonnement gebruiken waarvoor u de rol eigenaar of bijdrager hebt.

```python
from azureml.core import Workspace
ws = Workspace.create(name='myworkspace',
                      subscription_id='<azure-subscription-id>',
                      resource_group='myresourcegroup',
                      create_resource_group=True,
                      location='eastus2' # or other supported Azure region
                     )
```

Als u bovenstaande code uitvoert, wordt een nieuw browservenster geactiveerd waarin u zich kunt aanmelden bij uw Azure-account. Wanneer u zich aanmeldt, wordt het verificatietoken opgeslagen in de lokale cache.

U kunt de details van de werkruimte, met inbegrip van de bijbehorende opslag, containerregister en sleutelkluis, zien door het volgende typen:

```python
ws.get_details()
```

## <a name="write-a-configuration-file"></a>Een configuratiebestand schrijven

Sla de details van uw werkruimte in een configuratiebestand op in de huidige map. Dit bestand heeft 'aml_config \ config.json'.  

U kunt dit configuratiebestand van de werkruimte gebruiken om deze zelfde werkruimte later gemakkelijk met andere notitieblokken en scripts in dezelfde map of in een submap te laden. 

```python
# Create the configuration file.
ws.write_config()

# Use this code to load the workspace from 
# other scripts and notebooks in this directory.
# ws = Workspace.from_config()
```

Door de `write_config()`-API aan te roepen wordt het configuratiebestand in de huidige map gemaakt. Het `config.json`-bestand bevat het volgende:

```json
{
    "subscription_id": "<azure-subscription-id>",
    "resource_group": "myresourcegroup",
    "workspace_name": "myworkspace"
}
```

## <a name="use-the-workspace"></a>De werkruimte gebruiken

Schrijf code die gebruikmaakt van de basis-API's van de SDK om de experimentele uitvoering te volgen.

```python
from azureml.core import Experiment

# create a new experiemnt
exp = Experiment(workspace=ws, name='myexp')

# start a run
run = exp.start_logging()

# log a number
run.log('my magic number', 42)

# log a list (Fibonacci numbers)
run.log_list('my list', [1, 1, 2, 3, 5, 8, 13, 21, 34, 55]) 

# finish the run
run.complete()
```

## <a name="view-logged-results"></a>Vastgelegde resultaten weergeven
Wanneer de uitvoering is voltooid, kunt u de experimentele uitvoering weergeven in Azure Portal. Gebruik de volgende code om een URL naar de resultaten van de laatste uitvoering af te drukken.

```python
print(run.get_portal_url())
```

Gebruik de koppeling om de vastgelegde waarden in Azure Portal weer te geven in uw browser.

![vastgelegde waarden in de portal](./media/quickstart-create-workspace-with-python/logged-values.png)

## <a name="clean-up-resources"></a>Resources opschonen 
>[!IMPORTANT]
>De resources die u hebt gemaakt kunnen worden gebruikt als de vereisten voor andere Azure Machine Learning-zelfstudies en artikelen met procedures.

Als u het gemaakte niet meer gaat gebruiken, verwijdert u de resources die u zojuist in deze snelstart hebt gemaakt, zodat u geen extra kosten hebt.

```python
ws.delete(delete_dependent_resources=True)
```

## <a name="next-steps"></a>Volgende stappen

U hebt nu de nodige resources gemaakt om te gaan experimenteren en modellen te implementeren. U hebt ook code in een notitieblok uitgevoerd en de uitvoeringsgeschiedenis van die code in uw werkruimte in de cloud onderzocht.

U hebt nog enkele pakketten in uw omgeving nodig om te gebruiken voor de Azure Machine Learning-zelfstudies:

1. Sluit het notitieblok in uw browser.
1. Gebruik `Ctrl` + `C` in het opdrachtregelvenster om de notitieblokserver te stoppen.
1. Installeer extra pakketten.

    ```sh
    conda install -y cython matplotlib scikit-learn pandas numpy
    pip install azureml-sdk[automl]
    ```

Wanneer deze pakketten zijn geïnstalleerd, volgt u de zelfstudies om een model te trainen en implementeren.  

> [!div class="nextstepaction"]
> [Zelfstudie: een model voor de classificatie van afbeeldingen trainen](tutorial-train-models-with-aml.md)

U kunt ook [meer geavanceerde voorbeelden op GitHub](https://aka.ms/aml-notebooks) verkennen.
