---
title: 'Houd experimenten en training metrische gegevens: Azure Machine Learning | Microsoft Docs'
description: U kunt met de service Azure Machine Learning, bijhouden van uw experimenten en controleren van metrische gegevens voor het verbeteren van het proces voor het model maken. Informatie over logboekregistratie toevoegen aan uw trainingsscript, hoe u het experiment verzenden, hoe u de voortgang van een actieve taak controleren en hoe u de resultaten van een uitvoering wilt weergeven.
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/24/2018
ms.openlocfilehash: da92f59c4e25ec012cd9ad389c9afac410ba28e1
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219304"
---
# <a name="track-experiments-and-training-metrics-in-azure-machine-learning"></a>Bijhouden van experimenten en training metrische gegevens in Azure Machine Learning

U kunt in de Azure Machine Learning-service uw experimenten volgen en controleren van metrische gegevens voor het verbeteren van het proces voor het model maken. In dit artikel leert u over de verschillende manieren om toe te voegen van logboekregistratie voor uw trainingsscript over het verzenden van het experiment met **start_logging** en **ScriptRunConfig**, het controleren van de voortgang van een actieve taak, en hoe u de resultaten van een uitvoering wilt weergeven. 

>[!NOTE]
> Code in dit artikel is getest met Azure Machine Learning SDK versie 0.1.74 

## <a name="list-of-training-metrics"></a>Overzicht van metrische gegevens voor training 

De volgende metrische gegevens kunnen worden toegevoegd aan een run tijdens het trainen van een experiment. Voor een meer gedetailleerd overzicht van wat er op een uitvoering kunnen worden bijgehouden, raadpleegt u de [SDK-referentiedocumentatie](https://aka.ms/aml-sdk).

|Type| Funkce Pythonu | Voorbeeld | Opmerkingen|
|----|:----|:----|:----|
|Scalaire waarden | `run.log(name, value, description='')`| `run.log("accuracy", 0.95) ` |Meld u een numerieke of tekenreekswaarde voor de uitvoering met de opgegeven naam. Logboekregistratie van een metrische waarde aan een run zorgt ervoor dat deze metrische gegevens worden opgeslagen in de record uitvoeren in het experiment.  U kunt de dezelfde metrische gegevens meerdere keren binnen een uitvoering, het resultaat wordt beschouwd als een vector van deze metrische gegevens vastleggen.|
|Lijsten| `run.log_list(name, value, description='')`| `run.log_list("accuracies", [0.6, 0.7, 0.87])` | Meld u een lijst met waarden voor de uitvoering met de opgegeven naam.|
|Rij| `run.log_row(name, description=None, **kwargs)`| `run.log_row("Y over X", x=1, y=0.4)` | Met behulp van *log_row* maakt u een metrische waarde met meerdere kolommen zoals beschreven in kwargs. Elke benoemde parameter genereert een kolom met de opgegeven waarde.  *log_row* kan eenmaal worden aangeroepen voor het melden van een willekeurige tuple of meerdere keren in een lus voor het genereren van een volledige tabel.|
|Tabel| `run.log_table(name, value, description='')`| `run.log_table("Y over X", {"x":[1, 2, 3], "y":[0.6, 0.7, 0.89]})` | Meld u aan een dictionary-object de uitvoeren met de opgegeven naam. |
|Installatiekopieën| `run.log_image(name, path=None, plot=None)`| `run.log_image("ROC", plt)` | Meld u aan een installatiekopie van de record uitvoeren. Log_image gebruiken om aan te melden voor een afbeelding of een matplotlib getekend met het uitvoeren.  Deze installatiekopieën worden zichtbaar en vergelijkbare in de record uitvoeren.|
|Een uitvoering taggen| `run.tag(key, value=None)`| `run.tag("selected", "yes")` | Tag de uitvoering met een tekenreekssleutel en een optionele tekenreeks-waarde.|
|Bestand of map uploaden|`run.upload_file(name, path_or_stream)`| Run.upload_file ("best_model.pkl", ". / model.pkl") | Upload een bestand naar de record uitvoeren. Wordt uitgevoerd automatisch vastleggen bestand in de map met de opgegeven uitvoer, die standaard ". / levert ' voor de meeste typen die worden uitgevoerd.  Gebruik upload_file alleen als aanvullende bestanden moeten worden geüpload of een map met de uitvoer is niet opgegeven. Het is raadzaam toe te voegen `outputs` op de naam, zodat deze wordt het geüpload naar de map voor uitvoer. U kunt alle bestanden die gekoppeld zijn een lijst met deze record door te voeren met de naam `run.get_file_names()`|

> [!NOTE]
> Metrische gegevens voor hoeken, een lijst met rijen en tabellen kunt type hebben: float, geheel getal of tekenreeks.

## <a name="log-metrics-for-experiments"></a>Logboek metrische gegevens voor experimenten

Als u wilt bijhouden of bewaken van uw experiment, moet u code om te starten wanneer u de uitvoering verzendt logboekregistratie toevoegen. De volgende zijn manieren voor het activeren van het verzenden van de uitvoering:
* __Run.start_logging__ - logboekregistratiefuncties toevoegen aan uw trainingsscript en een interactieve sessie te starten in de opgegeven experiment. **start_logging** maakt u een interactieve uitvoeren voor gebruik in scenario's zoals laptops. Alle metrische gegevens die zijn vastgelegd tijdens de sessie worden toegevoegd aan de uitvoerregistratie in het experiment.
* __ScriptRunConfig__ - logboekregistratiefuncties toevoegen aan uw trainingsscript en laden van de gehele script-map met de uitvoering.  **ScriptRunConfig** is een klasse voor het instellen van configuraties voor het script wordt uitgevoerd. Met deze optie kunt u de code voor bewaking om te worden geïnformeerd over voltooiing of het opvragen van een visual widget voor het bewaken van toevoegen.

## <a name="set-up-the-workspace-and-experiment"></a>Instellen van de werkruimte en het experiment
Voordat u logboekregistratie en het verzenden van een experiment toevoegt, moet u de werkruimte en het experiment instellen.

1. Laden van de werkruimte. Voor meer informatie over het instellen van de configuratie van de standaardwerkruimte, volgt u de [snelstartgids](https://docs.microsoft.com/azure/machine-learning/service/quickstart-get-started).

  ```python
  from azureml.core import Workspace, Run
  import azureml.core
  
  ws = Workspace(workspace_name = <<workspace_name>>,
               subscription_id = <<subscription_id>>,
               resource_group = <<resource_group>>)
   ```

2. Maak het experiment.

  ```python
  from azureml.core import Experiment

  # make up an arbitrary name
  experiment_name = 'train-in-notebook'
  ```
  
## <a name="option-1-use-startlogging"></a>Optie 1: Start_logging gebruiken

**start_logging** maakt u een interactieve uitvoeren voor gebruik in scenario's zoals laptops. Alle metrische gegevens die zijn vastgelegd tijdens de sessie worden toegevoegd aan de uitvoerregistratie in het experiment.

Het volgende voorbeeld traint een eenvoudig model sklearn Ridge lokaal in een lokaal Jupyter-notitieblok. Zie voor meer informatie over het verzenden van experimenten in verschillende omgevingen, [compute-doelen voor modeltraining met Azure Machine Learning-service instellen](https://docs.microsoft.com/azure/machine-learning/service/how-to-set-up-training-targets).

1. Maak een trainingsscript in een lokaal Jupyter-notitieblok. 

  ``` python
  # load diabetes dataset, a well-known small dataset that comes with scikit-learn
  from sklearn.datasets import load_diabetes
  from sklearn.linear_model import Ridge
  from sklearn.metrics import mean_squared_error
  from sklearn.model_selection import train_test_split
  from sklearn.externals import joblib

  X, y = load_diabetes(return_X_y = True)
  columns = ['age', 'gender', 'bmi', 'bp', 's1', 's2', 's3', 's4', 's5', 's6']
  X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
  data = {
      "train":{"X": X_train, "y": y_train},        
      "test":{"X": X_test, "y": y_test}
  }
  reg = Ridge(alpha = 0.03)
  reg.fit(data['train']['X'], data['train']['y'])
  preds = reg.predict(data['test']['X'])
  print('Mean Squared Error is', mean_squared_error(preds, data['test']['y']))
  joblib.dump(value = reg, filename = 'model.pkl');
  ```

2. Experiment bijhouden met behulp van de Azure Machine Learning-service-SDK toevoegen en een persistente model uploaden naar het experiment record worden uitgevoerd. De volgende code wordt toegevoegd tags, Logboeken, en wordt een modelbestand geüpload naar het experiment uitvoeren.

  ```python
  experiment = Experiment(workspace = ws, name = experiment_name)
  run = experiment.start_logging()
  run.tag("Description","My first run!")
  run.log('alpha', 0.03)
  reg = Ridge(alpha = 0.03)
  reg.fit(data['train']['X'], data['train']['y'])
  preds = reg.predict(data['test']['X'])
  run.log('mse', mean_squared_error(preds, data['test']['y']))
  joblib.dump(value = reg, filename = 'model.pkl')
  # Upload file directly to the outputs folder
  run.upload_file(name = 'outputs/model.pkl', path_or_stream = './model.pkl')

  run.complete()
  ```

Het script wordt beëindigd met ```run.complete()```, die de uitvoering wordt gemarkeerd als voltooid.  Dit wordt doorgaans gebruikt in interactieve scenario's.

## <a name="option-2-use-scriptrunconfig"></a>Optie 2: ScriptRunConfig gebruiken

**ScriptRunConfig** is een klasse voor het instellen van configuraties voor het script wordt uitgevoerd. Met deze optie kunt u de code voor bewaking om te worden geïnformeerd over voltooiing of het opvragen van een visual widget voor het bewaken van toevoegen.

In dit voorbeeld is een vervolg op het basismodel sklearn Ridge van boven. Hiervoor wordt een eenvoudige parameter vegen te vegen via alpha-waarden van het model om vast te leggen metrische gegevens en getrainde modellen in wordt uitgevoerd onder het experimentcanvas. Het voorbeeld lokaal wordt uitgevoerd op basis van een gebruiker beheerde omgeving. 

1. Maak een trainingsscript. Dit maakt gebruik van ```%%writefile%%``` de training-code naar de scriptmap als schrijven ```train.py```.

  ```python
  %%writefile $project_folder/train.py

  import os
  from sklearn.datasets import load_diabetes
  from sklearn.linear_model import Ridge
  from sklearn.metrics import mean_squared_error
  from sklearn.model_selection import train_test_split
  from azureml.core.run import Run
  from sklearn.externals import joblib

  import numpy as np

  #os.makedirs('./outputs', exist_ok = True)

  X, y = load_diabetes(return_X_y = True)

  run = Run.get_context()

  X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
  data = {"train": {"X": X_train, "y": y_train},
          "test": {"X": X_test, "y": y_test}}

  # list of numbers from 0.0 to 1.0 with a 0.05 interval
  alphas = mylib.get_alphas()

  for alpha in alphas:
      # Use Ridge algorithm to create a regression model
      reg = Ridge(alpha = alpha)
      reg.fit(data["train"]["X"], data["train"]["y"])

      preds = reg.predict(data["test"]["X"])
      mse = mean_squared_error(preds, data["test"]["y"])
      # log the alpha and mse values
      run.log('alpha', alpha)
      run.log('mse', mse)

      model_file_name = 'ridge_{0:.2f}.pkl'.format(alpha)
      # save model in the outputs folder so it automatically get uploaded
      with open(model_file_name, "wb") as file:
          joblib.dump(value = reg, filename = model_file_name)

      # upload the model file explicitly into artifacts 
      run.upload_file(name = model_file_name, path_or_stream = model_file_name)

      # register the model
      #run.register_model(file_name = model_file_name)

      print('alpha is {0:.2f}, and mse is {1:0.2f}'.format(alpha, mse))
  
  ```

2. De ```train.py``` verwijst naar een script ```mylib.py```. Dit bestand kunt u de lijst met alfanumerieke waarden worden gebruikt in het model ridge ophalen.

  ```python
  %%writefile $script_folder/mylib.py
  import numpy as np

  def get_alphas():
      # list of numbers from 0.0 to 1.0 with a 0.05 interval
      return np.arange(0.0, 1.0, 0.05)
  ```

3. Configureer een beheerde gebruiker lokale omgeving.

  ```python
  from azureml.core.runconfig import RunConfiguration

  # Editing a run configuration property on-fly.
  run_config_user_managed = RunConfiguration()

  run_config_user_managed.environment.python.user_managed_dependencies = True

  # You can choose a specific Python environment by pointing to a Python path 
  #run_config.environment.python.interpreter_path = '/home/user/miniconda3/envs/sdk2/bin/python'
  ```

4. Dien de ```train.py``` script uit te voeren in de gebruiker beheerde omgeving. Deze scriptmap gehele wordt ingediend voor de training, met inbegrip van de ```mylib.py``` bestand.

  ```python
  from azureml.core import ScriptRunConfig

  src = ScriptRunConfig(source_directory = './', script = 'train.py', run_config = run_config_user_managed)
  run = experiment.submit(src)
  ```
  
## <a name="view-run-details"></a>Details van de uitvoering weergeven

### <a name="monitor-run-with-jupyter-notebook-widgets"></a>Monitor uitvoeren met Jupyter-notebook widgets
Wanneer u gebruikt de **ScriptRunConfig** methode om in te dienen wordt uitgevoerd, kunt u de voortgang van de uitvoering met een Jupyter-notebook widget bekijken. Net als het indienen van de run, is de widget asynchroon en biedt deze elke 10-15 seconden live updates totdat de taak is voltooid.

1. Bekijk de Jupyter-widget tijdens het wachten op voor het uitvoeren om te voltooien.

  ```python
  from azureml.train.widgets import RunDetails
  RunDetails(run).show()
  ```

  ![Schermafbeelding van de Jupyter-notebook widget](./media/how-to-track-experiments/widgets.PNG)

### <a name="get-log-results-upon-completion"></a>Resultaten van logboeken weergeven bij voltooiing

Model trainen en bewaking optreden op de achtergrond, zodat u andere taken uitvoeren kunt terwijl u wacht. U kunt ook wachten totdat het model opleiding vóór het uitvoeren van meer code is voltooid. Bij het gebruik **ScriptRunConfig**, kunt u ```run.wait_for_completion(show_output = True)``` om weer te geven wanneer het trainen van het model is voltooid. De ```show_output``` vlag biedt uitgebreide uitvoer. 
  
### <a name="query-run-metrics"></a>Uitvoering van metrische gegevens van de query

U kunt de metrische gegevens van het gebruik van een getraind model weergeven ```run.get_metrics()```. U kunt nu alle van de metrische gegevens die zijn geregistreerd in het voorbeeld hierboven om te bepalen van het beste model krijgen.

<a name='view-the-experiment-in-the-web-portal'/>
## <a name="view-the-experiment-in-the-azure-portal"></a>Het experiment in Azure portal bekijken

Wanneer een experiment is voltooid, kunt u bladeren naar de vastgelegde experiment record worden uitgevoerd. U kunt dit op twee manieren doen:

* Ophalen van de URL van de uitvoering rechtstreeks ```print(run.get_portal_url())```
* Details van de uitvoering weergeven door in te dienen de naam van de uitvoering (in dit geval ```run```). U wijst dit op de naam van het experiment, -ID, type, status, de pagina met details, een koppeling naar de Azure-portal en een koppeling naar de documentatie.

De koppeling voor de uitvoering biedt u rechtstreeks naar de pagina de details van uitvoering in Azure portal. Hier ziet u alle eigenschappen, bijgehouden metrische gegevens, afbeeldingen en grafieken die zijn vastgelegd in het experiment. In dit geval geregistreerd we MSE en het alpha-waarden.

  ![Schermafbeelding van de details van de uitvoering in Azure portal](./media/how-to-track-experiments/run-details-page-web.PNG)

U kunt ook eventuele uitvoer of de logboeken voor de uitvoering weergeven of downloaden van de momentopname van het experiment dat u hebt verzonden, zodat u de map experiment met anderen kunt delen.
### <a name="viewing-charts-in-run-details"></a>Grafieken weergeven in de details van uitvoering

Er zijn verschillende manieren voor het gebruik van de logboekregistratie van API's gebruiken voor andere recordtypen van metrische gegevens tijdens een uitvoering en deze weergeven als grafieken in Azure portal. 

|Geregistreerde waarde|Voorbeeldcode| Weergeven in portal|
|----|----|----|
|Meld u een matrix met numerieke waarden| `run.log_list(name='Fibonacci', value=[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89])`|één variabele lijndiagram weer te geven|
|Meld u één numerieke waarde met dezelfde metrische naam herhaaldelijk gebruikt (zoals vanuit een for-lus)| `for i in tqdm(range(-10, 10)):    run.log(name='Sigmoid', value=1 / (1 + np.exp(-i))) angle = i / 2.0`| Één variabele lijndiagram weer te geven|
|Meld u een rij met 2 numerieke kolommen herhaaldelijk|`run.log_row(name='Cosine Wave', angle=angle, cos=np.cos(angle))   sines['angle'].append(angle)      sines['sine'].append(np.sin(angle))`|Twee variabelen lijndiagram weer te geven|
|Logboektabel met 2 numerieke kolommen|`run.log_table(name='Sine Wave', value=sines)`|Twee variabelen lijndiagram weer te geven|

## <a name="example-notebooks"></a>Voorbeeld-laptops
De volgende notebooks illustratie van concepten in dit artikel:
* [01.Getting-Started/01.Train-within-notebook/01.Train-within-notebook.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/01.train-within-notebook)
* [01.Getting-Started/02.Train-on-local/02.Train-on-local.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local)
* [01.Getting-Started/06.Logging-API/06.Logging-API.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/06.logging-api/06.logging-api.ipynb)

Deze laptops ophalen: [!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Volgende stappen

Probeer de volgende stappen voor informatie over het gebruik van de Azure Machine Learning-SDK voor Python:

* Bekijk een voorbeeld van hoe u het beste model registreren en deze implementeren in de zelfstudie [een model van de installatiekopie classificatie met Azure Machine Learning te trainen](tutorial-train-models-with-aml.md).

* Meer informatie over het [PyTorch-modellen met Azure Machine Learning te trainen](how-to-train-pytorch.md).
