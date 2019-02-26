---
title: Spoor experimenten en training metrische gegevens
titleSuffix: Azure Machine Learning service
description: U kunt met de service Azure Machine Learning, bijhouden van uw experimenten en controleren van metrische gegevens voor het verbeteren van het proces voor het model maken. Informatie over logboekregistratie toevoegen aan uw trainingsscript, hoe u het experiment verzenden, hoe u de voortgang van een actieve taak controleren en hoe u de resultaten van een uitvoering wilt weergeven.
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
ms.service: machine-learning
ms.subservice: core
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 356326c7270bb18cec1fdd37dfc6f7f2638b3839
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56822037"
---
# <a name="track-experiments-and-training-metrics-in-azure-machine-learning"></a>Bijhouden van experimenten en training metrische gegevens in Azure Machine Learning

U kunt in de Azure Machine Learning-service uw experimenten volgen en controleren van metrische gegevens voor het verbeteren van het proces voor het model maken. In dit artikel leert u over de verschillende manieren om toe te voegen van logboekregistratie voor uw trainingsscript over het verzenden van het experiment met **start_logging** en **ScriptRunConfig**, het controleren van de voortgang van een actieve taak, en hoe u de resultaten van een uitvoering wilt weergeven. 


## <a name="list-of-training-metrics"></a>Overzicht van metrische gegevens voor training 

De volgende metrische gegevens kunnen worden toegevoegd aan een run tijdens het trainen van een experiment. Voor een meer gedetailleerd overzicht van wat er op een uitvoering kunnen worden bijgehouden, raadpleegt u de [uitvoeren van de klasse referentiedocumentatie voor](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py).

|Type| Funkce Pythonu | Opmerkingen|
|----|:----|:----|
|Scalaire waarden |Functie:<br>`run.log(name, value, description='')`<br><br>Voorbeeld:<br>Run.log ("nauwkeurigheid", 0.95) |Meld u een numerieke of tekenreekswaarde voor de uitvoering met de opgegeven naam. Logboekregistratie van een metrische waarde aan een run zorgt ervoor dat deze metrische gegevens worden opgeslagen in de record uitvoeren in het experiment.  U kunt de dezelfde metrische gegevens meerdere keren binnen een uitvoering, het resultaat wordt beschouwd als een vector van deze metrische gegevens vastleggen.|
|Lijsten|Functie:<br>`run.log_list(name, value, description='')`<br><br>Voorbeeld:<br>Run.log_list ("nauwkeurigheden," [0,6, 0,7, 0.87]) | Meld u een lijst met waarden voor de uitvoering met de opgegeven naam.|
|Rij|Functie:<br>' run.log_row (naam, beschrijving = None, ** kwargs)<br>Voorbeeld:<br>Run.log_row ("Y via X", x = 1, y = 0.4) | Met behulp van *log_row* maakt u een metrische waarde met meerdere kolommen zoals beschreven in kwargs. Elke benoemde parameter genereert een kolom met de opgegeven waarde.  *log_row* kan eenmaal worden aangeroepen voor het melden van een willekeurige tuple of meerdere keren in een lus voor het genereren van een volledige tabel.|
|Tabel|Functie:<br>`run.log_table(name, value, description='')`<br><br>Voorbeeld:<br>Run.log_table ("Y via X", {"x": [1, 2, 3], "y": [0,6, 0,7, 0,89]}) | Meld u aan een dictionary-object de uitvoeren met de opgegeven naam. |
|Installatiekopieën|Functie:<br>`run.log_image(name, path=None, plot=None)`<br><br>Voorbeeld:<br>Run.log_image ("ROC" plt) | Meld u aan een installatiekopie van de record uitvoeren. Log_image gebruiken om aan te melden voor een afbeelding of een matplotlib getekend met het uitvoeren.  Deze installatiekopieën worden zichtbaar en vergelijkbare in de record uitvoeren.|
|Een uitvoering taggen|Functie:<br>`run.tag(key, value=None)`<br><br>Voorbeeld:<br>Run.Tag ("ingeschakeld", "Ja") | Tag de uitvoering met een tekenreekssleutel en een optionele tekenreeks-waarde.|
|Bestand of map uploaden|Functie:<br>`run.upload_file(name, path_or_stream)`<br> <br> Voorbeeld:<br>Run.upload_file ("best_model.pkl", ". / model.pkl") | Upload een bestand naar de record uitvoeren. Wordt uitgevoerd automatisch vastleggen bestand in de map met de opgegeven uitvoer, die standaard ". / levert ' voor de meeste typen die worden uitgevoerd.  Gebruik upload_file alleen als aanvullende bestanden moeten worden geüpload of een map met de uitvoer is niet opgegeven. Het is raadzaam toe te voegen `outputs` op de naam, zodat deze wordt het geüpload naar de map voor uitvoer. U kunt alle bestanden die gekoppeld zijn een lijst met deze record door te voeren met de naam `run.get_file_names()`|

> [!NOTE]
> Metrische gegevens voor hoeken, een lijst met rijen en tabellen kunt type hebben: float, geheel getal of tekenreeks.

## <a name="start-logging-metrics"></a>Logboekregistratie van metrische gegevens starten

Als u wilt bijhouden of bewaken van uw experiment, moet u code om te starten wanneer u de uitvoering verzendt logboekregistratie toevoegen. De volgende zijn manieren voor het activeren van het verzenden van de uitvoering:
* __Run.start_logging__ - logboekregistratiefuncties toevoegen aan uw trainingsscript en een interactieve sessie te starten in de opgegeven experiment. **start_logging** maakt u een interactieve uitvoeren voor gebruik in scenario's zoals laptops. Alle metrische gegevens die zijn vastgelegd tijdens de sessie worden toegevoegd aan de uitvoerregistratie in het experiment.
* __ScriptRunConfig__ - logboekregistratiefuncties toevoegen aan uw trainingsscript en laden van de gehele script-map met de uitvoering.  **ScriptRunConfig** is een klasse voor het instellen van configuraties voor het script wordt uitgevoerd. Met deze optie kunt u de code voor bewaking om te worden geïnformeerd over voltooiing of het opvragen van een visual widget voor het bewaken van toevoegen.

## <a name="set-up-the-workspace"></a>Instellen van de werkruimte
Voordat u logboekregistratie en het verzenden van een experiment toevoegt, moet u de werkruimte instellen.

1. Laden van de werkruimte. Voor meer informatie over het instellen van de configuratie van de standaardwerkruimte, volgt u de [snelstartgids](https://docs.microsoft.com/azure/machine-learning/service/quickstart-get-started).

  ```python
  from azureml.core import Experiment, Run, Workspace
  import azureml.core
  
  ws = Workspace(workspace_name = <<workspace_name>>,
               subscription_id = <<subscription_id>>,
               resource_group = <<resource_group>>)
   ```
  
## <a name="option-1-use-startlogging"></a>Optie 1: Gebruik start_logging

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
  # Get an experiment object from Azure Machine Learning
  experiment = Experiment(workspace = ws, name = "train-within-notebook")
  
  # Create a run object in the experiment
  run = experiment.start_logging()# Log the algorithm parameter alpha to the run
  run.log('alpha', 0.03)

  # Create, fit, and test the scikit-learn Ridge regression model
  regression_model = Ridge(alpha=0.03)
  regression_model.fit(data['train']['X'], data['train']['y'])
  preds = regression_model.predict(data['test']['X'])

  # Output the Mean Squared Error to the notebook and to the run
  print('Mean Squared Error is', mean_squared_error(data['test']['y'], preds))
  run.log('mse', mean_squared_error(data['test']['y'], preds))

  # Save the model to the outputs directory for capture
  joblib.dump(value=regression_model, filename='outputs/model.pkl')

  # Take a snapshot of the directory containing this notebook
  run.take_snapshot('./')

  # Complete the run
  run.complete()
  
  ```

Het script wordt beëindigd met ```run.complete()```, die de uitvoering wordt gemarkeerd als voltooid.  Deze functie wordt meestal gebruikt in interactieve scenario's.

## <a name="option-2-use-scriptrunconfig"></a>Optie 2: Use ScriptRunConfig

**ScriptRunConfig** is een klasse voor het instellen van configuraties voor het script wordt uitgevoerd. Met deze optie kunt u de code voor bewaking om te worden geïnformeerd over voltooiing of het opvragen van een visual widget voor het bewaken van toevoegen.

In dit voorbeeld is een vervolg op het basismodel sklearn Ridge van boven. Hiervoor wordt een eenvoudige parameter vegen te vegen via alpha-waarden van het model om vast te leggen metrische gegevens en getrainde modellen in wordt uitgevoerd onder het experimentcanvas. Het voorbeeld lokaal wordt uitgevoerd op basis van een gebruiker beheerde omgeving. 

1. Maken van een trainingsscript `train.py`.

  ```python
  # train.py

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

2. De `train.py` script verwijzingen `mylib.py` waarmee u de lijst met alfanumerieke waarden worden gebruikt in het model ridge ophalen.

  ```python
  # mylib.py
  
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
  
  experiment = Experiment(workspace=ws, name="train-on-local")
  src = ScriptRunConfig(source_directory = './', script = 'train.py', run_config = run_config_user_managed)
  run = experiment.submit(src)
  ```

## <a name="cancel-a-run"></a>Een uitvoering annuleren
Na een uitvoering is ingediend, kunt u deze annuleren, zelfs als u hebt de objectverwijzing verloren als u weet de naam van het experiment wat en id uitvoeren. 

```python
from azureml.core import Experiment
exp = Experiment(ws, "my-experiment-name")

# if you don't know the run id, you can list all runs under an experiment
for r in exp.get_runs():  
    print(r.id, r.get_status())

# if you know the run id, you can "rehydrate" the run
from azureml.core import get_run
r = get_run(experiment=exp, run_id="my_run_id", rehydrate=True)
  
# check the returned run type and status
print(type(r), r.get_status())

# you can cancel a run if it hasn't completed or failed
if r.get_status() not in ['Complete', 'Failed']:
    r.cancel()
```
Houd er rekening mee dat momenteel wordt alleen ScriptRun en PipelineRun typen bieden ondersteuning voor de annuleringsbewerking.

Bovendien kunt u een uitvoeren via de CLI met behulp van de volgende opdracht uit:
```shell
az ml run cancel -r <run_id> -p <project_path>
```


## <a name="view-run-details"></a>Details van de uitvoering weergeven

### <a name="monitor-run-with-jupyter-notebook-widgets"></a>Monitor uitvoeren met Jupyter-notebook widgets
Wanneer u gebruikt de **ScriptRunConfig** methode om in te dienen wordt uitgevoerd, kunt u de voortgang van de uitvoering met een Jupyter-notebook widget bekijken. Net als het indienen van de run, is de widget asynchroon en biedt deze elke 10-15 seconden live updates totdat de taak is voltooid.

1. Bekijk de Jupyter-widget tijdens het wachten op voor het uitvoeren om te voltooien.

  ```python
  from azureml.widgets import RunDetails
  RunDetails(run).show()
  ```

  ![Schermafbeelding van de Jupyter-notebook widget](./media/how-to-track-experiments/widgets.PNG)

2. **[Voor geautomatiseerde machine learning-uitvoeringen]**  Voor toegang tot de grafieken van een vorige uitvoering. Vervang `<<experiment_name>>` met de naam van de juiste experiment:

   ``` 
   from azureml.widgets import RunDetails
   from azureml.core.run import Run

   experiment = Experiment (workspace, <<experiment_name>>)
   run_id = 'autoML_my_runID' #replace with run_ID
   run = Run(experiment, run_id)
   RunDetails(run).show()
   ```

  ![Jupyter notebook widget voor geautomatiseerde Machine Learning](./media/how-to-track-experiments/azure-machine-learning-auto-ml-widget.png)


Om verdere details van een pijplijn-Klik op de pijplijn weer te geven u graag zou willen verkennen in de tabel en de grafieken worden weergegeven in een pop-upvenster vanuit Azure portal.

### <a name="get-log-results-upon-completion"></a>Resultaten van logboeken weergeven bij voltooiing

Model trainen en bewaking optreden op de achtergrond, zodat u andere taken uitvoeren kunt terwijl u wacht. U kunt ook wachten totdat het model opleiding vóór het uitvoeren van meer code is voltooid. Bij het gebruik **ScriptRunConfig**, kunt u ```run.wait_for_completion(show_output = True)``` om weer te geven wanneer het trainen van het model is voltooid. De ```show_output``` vlag biedt uitgebreide uitvoer. 
  
### <a name="query-run-metrics"></a>Uitvoering van metrische gegevens van de query

U kunt de metrische gegevens van het gebruik van een getraind model weergeven ```run.get_metrics()```. U kunt nu alle van de metrische gegevens die zijn geregistreerd in het voorbeeld hierboven om te bepalen van het beste model krijgen.

<a name="view-the-experiment-in-the-web-portal"></a>
## <a name="view-the-experiment-in-the-azure-portal"></a>Het experiment in Azure portal bekijken

Wanneer een experiment is voltooid, kunt u bladeren naar de vastgelegde experiment record worden uitgevoerd. U kunt doen toegang tot de geschiedenis op twee manieren:

* Ophalen van de URL van de uitvoering rechtstreeks ```print(run.get_portal_url())```
* Details van de uitvoering weergeven door in te dienen de naam van de uitvoering (in dit geval ```run```). Op deze manier verwijst u naar de naam van het experiment, -ID, type, status, de pagina met details, een koppeling naar de Azure-portal en een koppeling naar de documentatie.

De koppeling voor de uitvoering biedt u rechtstreeks naar de pagina de details van uitvoering in Azure portal. Hier ziet u alle eigenschappen, bijgehouden metrische gegevens, afbeeldingen en grafieken die zijn vastgelegd in het experiment. In dit geval geregistreerd we MSE en het alpha-waarden.

  ![Details van de uitvoering in Azure portal](./media/how-to-track-experiments/run-details-page-web.PNG)

U kunt ook eventuele uitvoer of de logboeken voor de uitvoering weergeven of downloaden van de momentopname van het experiment dat u hebt verzonden, zodat u de map experiment met anderen kunt delen.

### <a name="viewing-charts-in-run-details"></a>Grafieken weergeven in de details van uitvoering

Er zijn verschillende manieren voor het gebruik van de logboekregistratie van API's gebruiken voor andere recordtypen van metrische gegevens tijdens een uitvoering en deze weergeven als grafieken in Azure portal. 

|Geregistreerde waarde|Voorbeeldcode| Weergeven in portal|
|----|----|----|
|Meld u een matrix met numerieke waarden| `run.log_list(name='Fibonacci', value=[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89])`|één variabele lijndiagram weer te geven|
|Meld u één numerieke waarde met dezelfde metrische naam herhaaldelijk gebruikt (zoals vanuit een for-lus)| `for i in tqdm(range(-10, 10)):    run.log(name='Sigmoid', value=1 / (1 + np.exp(-i))) angle = i / 2.0`| één variabele lijndiagram weer te geven|
|Meld u een rij met 2 numerieke kolommen herhaaldelijk|`run.log_row(name='Cosine Wave', angle=angle, cos=np.cos(angle))   sines['angle'].append(angle)      sines['sine'].append(np.sin(angle))`|Twee variabelen lijndiagram weer te geven|
|Logboektabel met 2 numerieke kolommen|`run.log_table(name='Sine Wave', value=sines)`|Twee variabelen lijndiagram weer te geven|

<a name="auto"></a>
## <a name="understanding-automated-ml-charts"></a>Informatie over geautomatiseerde ML-grafieken

Na een geautomatiseerde ML-taak in een notitieblok in te dienen, een geschiedenis van alle uitvoeringen deze vindt u in uw machine learning-werkruimte-service. 

Meer informatie over:
+ [Grafieken en curven voor modellen voor classificatie](#classification)
+ [Diagrammen en grafieken voor regressiemodellen](#regression)
+ [Model uitgelegd mogelijkheid](#model-explain-ability-and-feature-importance)


### <a name="view-the-run-charts"></a>De uitvoering grafieken bekijken

1. Ga naar uw werkruimte. 

1. Selecteer **experimenten** in het deelvenster uiterst links van uw werkruimte.

  ![Schermopname van het experiment menu](./media/how-to-track-experiments/azure-machine-learning-auto-ml-experiment_menu.PNG)

1. Selecteer het experiment dat u geïnteresseerd bent in.

  ![Lijst van experiment](./media/how-to-track-experiments/azure-machine-learning-auto-ml-experiment_list.PNG)

1. Selecteer het aantal uitvoeren in de tabel.

   ![Experiment uitvoeren](./media/how-to-track-experiments/azure-machine-learning-auto-ml-experiment_run.PNG)

1.  Selecteer in de tabel, het aantal herhalingen voor het model dat u graag verder te verkennen.

   ![Model van experiment](./media/how-to-track-experiments/azure-machine-learning-auto-ml-experiment_model.PNG)



### <a name="classification"></a>Classificatie

Voor elke classificatie-model dat u bouwen met behulp van de geautomatiseerde machine learning-mogelijkheden van Azure Machine Learning, ziet u de volgende grafieken: 
+ [Verwarringsmatrix](#confusion-matrix)
+ [Precisie-/ Oproepdiagram grafiek](#precision-recall-chart)
+ [Ontvanger operationele kenmerken (of ROC)](#ROC)
+ [Lift-curve](#lift-curve)
+ [Winsten curve](#gains-curve)
+ [Kalibreren tekengebied](#calibration-plot)

#### <a name="confusion-matrix"></a>Verwarringsmatrix

Een verwarringsmatrix wordt gebruikt om te beschrijven van de prestaties van een model voor classificatie. Elke rij geeft de exemplaren van de klasse ' True ', en elke kolom vertegenwoordigt de instanties van de voorspelde klasse. De verwarringsmatrix ziet u de labels correct ingedeeld en de onjuist ingedeeld labels voor een bepaald model.

Voor problemen met de classificatie biedt Azure Machine Learning automatisch een verwarringsmatrix voor elk model dat is gebouwd. Voor elke verwarringsmatrix ziet geautomatiseerde ML u de labels correct ingedeeld als groen en onjuist ingedeeld labels als rood. De grootte van de cirkel geeft het aantal steekproeven in die bin. Bovendien vindt u de frequentie-telling van elk label voorspelde en elk label waar in de aangrenzende staafdiagrammen. 

Voorbeeld 1: Een model classificatie met slechte nauwkeurigheid ![een classificeringsmodel met slechte nauwkeurigheid](./media/how-to-track-experiments/azure-machine-learning-auto-ml-confusion_matrix1.PNG)

Voorbeeld 2: Een model classificatie met hoge nauwkeurigheid (ideaal) ![een classificeringsmodel met hoge nauwkeurigheid](./media/how-to-track-experiments/azure-machine-learning-auto-ml-confusion_matrix2.PNG)


#### <a name="precision-recall-chart"></a>Precisie-/ oproepdiagram grafiek

U kunt de precisie-/ oproepdiagram curven voor elk model om te bepalen welk model u al een acceptabele relatie tussen precisie en intrekken voor uw specifieke zakelijke probleem vergelijken met deze grafiek. In deze grafiek worden precisie-/ Oproepdiagram Macro gemiddeld, gemiddelde Micro-precisie-/ Oproepdiagram en de precisie-/ oproepdiagram die zijn gekoppeld aan alle klassen voor een model.

De term die precisie vertegenwoordigt die mogelijkheid voor een classificatie voor alle instanties juist label. Intrekken vertegenwoordigt de mogelijkheid voor een classificatie om alle exemplaren van een bepaald label te vinden. De precisie-/ oproepdiagram curve ziet u de relatie tussen deze twee concepten. Het model zou in het ideale geval zijn 100% nauwkeurigheid en 100% nauwkeurigheid.

Voorbeeld 1: Een model classificatie met lage precisie en lage intrekken ![een classificeringsmodel met lage precisie en lage intrekken](./media/how-to-track-experiments/azure-machine-learning-auto-ml-precision_recall1.PNG)

Voorbeeld 2: Een model classificatie met ~ 100% nauwkeurigheid en ~ 100% intrekken (ideaal) ![een classificatie model hoge precisie en intrekken](./media/how-to-track-experiments/azure-machine-learning-auto-ml-precision_recall2.PNG)

#### <a name="roc"></a>ROC

Ontvanger operationele kenmerken (of ROC) is een diagram van de labels correct ingedeeld versus de onjuist ingedeeld labels voor een bepaald model. De ROC-curve zijn minder informatieve als training modellen op gegevenssets met hoge vertekening, zoals deze niet in de ONWAAR positief labels weergegeven wordt.

Voorbeeld 1: Een model classificatie met lage waarde true labels en hoge false labels ![classificeringsmodel met lage waarde true labels en hoge false labels](./media/how-to-track-experiments/azure-machine-learning-auto-ml-roc1.PNG)

Voorbeeld 2: Een model classificatie met hoge waarde true labels en lage waarde false labels ![een classificatie-model met hoge waarde true labels en lage waarde false labels](./media/how-to-track-experiments/azure-machine-learning-auto-ml-roc2.PNG)

#### <a name="lift-curve"></a>Lift-curve

U kunt de lift van het model automatisch gemaakt met Azure Machine Learning aan de basislijn om te bekijken van de toename van de waarde van dat specifieke model vergelijken.

Lift-grafieken worden gebruikt voor het evalueren van de prestaties van een model voor classificatie. Hier ziet u hoeveel beter kunt u verwachten doen met een model in vergelijking met zonder een model. 

Voorbeeld 1: Model functioneert met slechter dan een willekeurige selectie model ![een classificatie-model dat slechter dan een willekeurige selectie model](./media/how-to-track-experiments/azure-machine-learning-auto-ml-lift_curve1.PNG)

Voorbeeld 2: Model beter presteert dan een willekeurige selectie model ![een classificatie-model dat beter presteert](./media/how-to-track-experiments/azure-machine-learning-auto-ml-lift_curve2.PNG)

#### <a name="gains-curve"></a>Winsten curve

Een diagram kolomopslag evalueert de prestaties van een model classificatie op basis van elk deel van de gegevens. Hier ziet voor elke percentiel van de gegevensset, hoeveel u kunt beter verwachten om uit te voeren vergeleken met een willekeurige selectie-model.

Met de cumulatieve winsten grafiek kunt u de classificatie afsluitdatum met behulp van een percentage dat overeenkomt met een gewenste winst uit het model kiezen. Deze informatie geeft een andere manier om de resultaten in de bijbehorende lift-grafiek kijken.

Voorbeeld 1: Een model classificatie met minimale winst ![een classificatie-model met minimale winst](./media/how-to-track-experiments/azure-machine-learning-auto-ml-gains_curve1.PNG)

Voorbeeld 2: Een model classificatie met aanzienlijke winst ![een classificatie-model met aanzienlijke winst](./media/how-to-track-experiments/azure-machine-learning-auto-ml-gains_curve2.PNG)

#### <a name="calibration-plot"></a>Kalibreren tekengebied

Voor alle problemen met de classificatie, kunt u de regel kalibreren voor micro-gemiddelde macro-gemiddelde en elke klasse in een bepaalde Voorspellend model bekijken. 

Een diagram kalibreren wordt gebruikt om het vertrouwen van een Voorspellend model weer te geven. Dit gebeurt door de relatie tussen de voorspelde kans en de werkelijke kans op, waarbij "kans" vertegenwoordigt de kans dat een bepaalde instantie deel uitmaakt van een label. Een goed geijkte model wordt uitgelijnd met de y = x regel, waar deze redelijk zeker van zijn in de voorspellingen is. Een model te veel confident worden uitgelijnd met de y = 0 regel, waar de voorspelde kans is aanwezig, maar er is geen daadwerkelijke kans.

Voorbeeld 1: Een meer goed geijkte model ![ meer goed geijkte model](./media/how-to-track-experiments/azure-machine-learning-auto-ml-calib_curve1.PNG)

Voorbeeld 2: Een model te veel confident ![een model te veel confident](./media/how-to-track-experiments/azure-machine-learning-auto-ml-calib_curve2.PNG)

### <a name="regression"></a>Regressie
Voor elke regressiemodel u bouwen met behulp van de geautomatiseerde machine learning-mogelijkheden van Azure Machine Learning, ziet u de volgende grafieken: 
+ [Voorspelde vs. De waarde True](#pvt)
+ [Histogram van dit](#histo)

<a name="pvt"></a>

#### <a name="predicted-vs-true"></a>Voorspelde vs. True

Voorspelde vs. Waar wordt de relatie tussen een voorspelde waarde en de waarde voor correlerende waar een probleem met regressie. Deze grafiek kan worden gebruikt voor het meten van prestaties van een model als de dichter bij de y = x regel de voorspelde waarden zijn, hoe beter de nauwkeurigheid van een Voorspellend model.

Na elke uitvoering ziet u een voorspelde versus waar graph voor elke regressiemodel. Als u wilt beveiligen privacy van gegevens, waarden samen worden binned en de grootte van elke bin wordt weergegeven als een staafdiagram in het onderste gedeelte van het grafiekgebied. U kunt het voorspellende model vergelijken met de lichtere schaduw-gebied met fout marges, op basis van de optimale waarde van waar het model moet worden.

Voorbeeld 1: Een regressiemodel met lage nauwkeurigheid in voorspellingen ![een regressiemodel met lage nauwkeurigheid in voorspellingen](./media/how-to-track-experiments/azure-machine-learning-auto-ml-regression1.PNG)

Voorbeeld 2: Een regressiemodel met hoge nauwkeurigheid in de voorspellingen ![een regressiemodel met hoge nauwkeurigheid in de voorspellingen](./media/how-to-track-experiments/azure-machine-learning-auto-ml-regression2.PNG)

<a name="histo"></a>

#### <a name="histogram-of-residuals"></a>Histogram van dit

Een resterende vertegenwoordigt een waargenomen y – de voorspelde y. Om weer te geven een foutmarge met lage afwijking, moet het histogram van dit als een rinkelende bel curve, gecentreerd rond 0 worden vormgegeven. 

Voorbeeld 1: Een regressiemodel met afwijking van de fouten ![SA regressiemodel met afwijking van de fouten](./media/how-to-track-experiments/azure-machine-learning-auto-ml-regression3.PNG)

Voorbeeld 2: Een regressiemodel met gelijkmatige verdeling van fouten ![een regressiemodel met gelijkmatige verdeling van fouten](./media/how-to-track-experiments/azure-machine-learning-auto-ml-regression4.PNG)

### <a name="model-explain-ability-and-feature-importance"></a>Model functie en wordt uitgelegd mogelijkheid urgentie

Urgentie van de functie biedt een score die aangeeft hoe waardevol elke functie is in het samenstellen van een model. U kunt de functie belang score voor het model algehele, evenals per klasse op een Voorspellend model bekijken. U kunt per functie zien hoe het belang worden vergeleken op basis van elke klasse en de algehele.

![Functie uitleg mogelijkheid](./media/how-to-track-experiments/azure-machine-learning-auto-ml-feature_explain1.PNG)

## <a name="example-notebooks"></a>Voorbeeld-laptops
De volgende notebooks illustratie van concepten in dit artikel:
* [How-to-use-azureml/training/Train-within-notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training\train-within-notebook)
* [How-to-use-azureml/training/Train-on-local](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-on-local)
* [How-to-use-azureml/training/Logging-API/Logging-API.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/logging-api)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Volgende stappen

Probeer de volgende stappen voor informatie over het gebruik van de Azure Machine Learning-SDK voor Python:

* Bekijk een voorbeeld van hoe u het beste model registreren en deze implementeren in de zelfstudie [een model van de installatiekopie classificatie met Azure Machine Learning te trainen](tutorial-train-models-with-aml.md).

* Meer informatie over het [PyTorch-modellen met Azure Machine Learning te trainen](how-to-train-pytorch.md).
