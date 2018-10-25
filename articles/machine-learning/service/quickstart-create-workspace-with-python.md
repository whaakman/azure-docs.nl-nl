---
title: 'Snelstart: De Python-SDK gebruiken om een werkruimte voor de Machine Learning service te maken - Azure Machine Learning'
description: Aan de slag met Azure Machine Learning. Installeer de Python-SDK en gebruik deze om een werkruimte te maken. Deze werkruimte is het basisblok in de cloud dat u gebruikt voor het experimenteren, trainen en implementeren van machine learning-modellen met Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: quickstart
ms.reviewer: sgilley
author: hning86
ms.author: haining
ms.date: 09/24/2018
ms.openlocfilehash: e4624b115143f9f2e6dd77aa8ee79597c86ba31c
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49456171"
---
# <a name="quickstart-use-python-to-get-started-with-azure-machine-learning"></a>Snelstart: Python gebruiken om aan de slag te gaan met Azure Machine Learning

In deze quickstart gebruikt u de Azure Machine Learning-SDK voor Python om een [werkruimte](concept-azure-machine-learning-architecture.md) voor de Machine Learning-service te maken. Deze werkruimte is het basisblok in de cloud dat u gebruikt voor het experimenteren met en trainen en implementeren van machine learning-modellen met Machine Learning.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2G9N6]

In deze zelfstudie gaat u de Python-SDK installeren en:

* Een werkruimte maken in uw Azure-abonnement.
* Een configuratiebestand voor die werkruimte maken voor later gebruik in andere notitieblokken en scripts.
* Code schrijven om waarden te schrijven in de werkruimte
* De vastgelegde waarden in uw werkruimte weergeven

In deze quickstart gaat u een werkruimte en een configuratiebestand maken. U kunt deze gebruiken als basisvereisten voor andere Machine Learning-zelfstudies en artikelen met procedures. Net als bij andere Azure-services gelden er limieten en quota voor Machine Learning. [Meer informatie over quota's en hoe u meer kunt aanvragen.](how-to-manage-quotas.md)

De volgende Azure-resources worden automatisch toegevoegd aan uw werkruimte wanneer deze regionaal beschikbaar zijn:
 
- [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)
- [Azure Storage](https://azure.microsoft.com/services/storage/)
- [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) 
- [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.


## <a name="install-the-sdk"></a>De SDK installeren

*Sla deze sectie over als u een virtuele machine voor datatechnologie gebruikt die is gemaakt na 27 september 2018.* Deze virtuele machines voor datatechnologie worden geleverd met de Python SDK er vooraf op geïnstalleerd.

Voordat u de SDK installeert, raden we u aan om een geïsoleerde omgeving voor Python te maken. Tijdens deze quickstart maakt u gebruik van [Miniconda](https://conda.io/docs/user-guide/install/index.html), maar u kunt ook gebruikmaken van volledig geïnstalleerde [Anaconda](https://www.anaconda.com/) of [Python virtualenv](https://virtualenv.pypa.io/en/stable/).

### <a name="install-miniconda"></a>Miniconda installeren


[Download](https://conda.io/miniconda.html) en installeer Miniconda. Kies de versie Python 3.7 of hoger. Selecteer niet Python 2.x.

### <a name="create-an-isolated-python-environment"></a>Een geïsoleerde omgeving voor Python maken 

Open een opdrachtregelvenster. Maak vervolgens met Python 3.6 een nieuwe conda-omgeving met de naam `myenv`.

```sh
conda create -n myenv -y Python=3.6
```

Activeer de omgeving.

  ```sh
  conda activate myenv
  ```

### <a name="install-the-sdk"></a>De SDK installeren

Installeer de SDK in de geactiveerde conda-omgeving. Met deze code worden de belangrijkste onderdelen van de Machine Learning SDK geïnstalleerd. Er wordt ook een Jupyter Notebook-server in de conda-omgeving `myenv` geïnstalleerd. De installatie duurt **ongeveer vier minuten**.

```sh
pip install azureml-sdk[notebooks]
```

## <a name="create-a-workspace"></a>Een werkruimte maken

Als u Jupyter Notebook wilt starten, voert u de volgende opdracht in.
```sh
jupyter notebook
```

Maak een nieuw notitieblok met behulp van de standaard-`Python 3`-kernel in het browservenster. 

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

Als u bovenstaande code uitvoert, wordt mogelijk een nieuw browservenster geactiveerd waarin u zich kunt aanmelden bij uw Azure-account. Nadat u zich hebt aangemeld, wordt het verificatietoken opgeslagen in de lokale cache.

U kunt de details van de werkruimte, met inbegrip van de bijbehorende opslag, containerregister en sleutelkluis, zien door de volgende code in te voeren.

```python
ws.get_details()
```

## <a name="write-a-configuration-file"></a>Een configuratiebestand schrijven

Sla de details van uw werkruimte in een configuratiebestand op in de huidige map. Dit bestand heeft 'aml_config \ config.json'.  

Dit configuratiebestand van de werkruimte maakt het eenvoudig om dezelfde werkruimte later te laden. U kunt het samen met andere notitieblokken en scripts in dezelfde map of submap laden. 

```python
# Create the configuration file.
ws.write_config()

# Use this code to load the workspace from 
# other scripts and notebooks in this directory.
# ws = Workspace.from_config()
```

Door de `write_config()`-API aan te roepen wordt het configuratiebestand in de huidige map gemaakt. Het bestand `config.json` bevat het volgende script.

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

# create a new experiment
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
Wanneer de uitvoering is voltooid, kunt u de experimentele uitvoering weergeven in de Azure-portal. Gebruik de volgende code om een URL naar de resultaten van de laatste uitvoering af te drukken.

```python
print(run.get_portal_url())
```

Gebruik de koppeling om de vastgelegde waarden in Azure Portal weer te geven in uw browser.

![Vastgelegde waarden in de portal](./media/quickstart-create-workspace-with-python/logged-values.png)

## <a name="clean-up-resources"></a>Resources opschonen 
>[!IMPORTANT]
>De resources die u hebt gemaakt kunnen worden gebruikt als de basisvereisten voor andere Machine Learning-zelfstudies en artikelen met procedures.

Als u niet van plan bent om gebruik te maken van de resources die u hier hebt gemaakt, kunt u ze verwijderen zodat er geen kosten voor in rekening worden gebracht.

```python
ws.delete(delete_dependent_resources=True)
```

## <a name="next-steps"></a>Volgende stappen

U hebt de resources gemaakt die u nodig hebt om mee te experimenteren en om modellen te implementeren. U hebt ook code uitgevoerd in een notitieblok. U hebt ook de uitvoeringsgeschiedenis van die code in uw werkruimte in de cloud onderzocht.

U hebt nog enkele pakketten in uw omgeving nodig om te gebruiken voor de Machine Learning-zelfstudies.

1. Sluit het notitieblok in uw browser.
1. Gebruik `Ctrl` + `C` in het opdrachtregelvenster om de notitieblokserver te stoppen.
1. Installeer extra pakketten.

    ```sh
    conda install -y cython matplotlib scikit-learn pandas numpy
    pip install azureml-sdk[automl]
    ```

Nadat u deze pakketten hebt geïnstalleerd, volgt u de zelfstudies om een model te trainen en implementeren. 

> [!div class="nextstepaction"]
> [Zelfstudie: een model voor de classificatie van afbeeldingen trainen](tutorial-train-models-with-aml.md)

U kunt ook [meer geavanceerde voorbeelden op GitHub](https://aka.ms/aml-notebooks) verkennen.
