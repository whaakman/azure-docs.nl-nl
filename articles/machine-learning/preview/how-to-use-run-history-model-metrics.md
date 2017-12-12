---
title: Hoe uitvoeren als gebruiken geschiedenis en Model metrische gegevens in Azure Machine Learning-Workbench | Microsoft Docs
description: Handleiding voor de met de geschiedenis uitvoeren en metrische gegevens Model functies van Azure Machine Learning Workbench
services: machine-learning
author: jopela
ms.author: jopela
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/07/2017
ms.openlocfilehash: 143cbfa66436796e1b61b100def030dde43faac9
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="how-to-use-run-history-and-model-metrics-in-azure-machine-learning-workbench"></a>Het gebruik Uitvoeringsgeschiedenis en Model metrische gegevens in Azure Machine Learning Workbench

Azure Machine Learning Workbench ondersteunt experimenteren van wetenschappelijke gegevens via de **geschiedenis uitvoeren** en **Model metrische gegevens** functies.
**Uitvoeringsgeschiedenis** biedt een manier voor het bijhouden van de uitvoer van uw machine learning-experimenten, en vervolgens kunt u filteren en vergelijking van de resultaten.
**Metrische gegevens model** kunnen worden geregistreerd vanaf elk punt van uw scripts, bijhouden welke waarden in uw gegevens wetenschappelijke experimenten het belangrijkst zijn.
Dit artikel wordt beschreven hoe u effectief gebruik van deze functies om de snelheid en de kwaliteit van uw gegevens wetenschappelijke experimenten te verhogen.

## <a name="prerequisites"></a>Vereisten
Om de stap in deze handleiding instructies, moet u:
* [Maak en installeer Azure Machine Learning](quickstart-installation.md)
- [Een Project maken](quickstart-installation.md)


## <a name="azure-ml-logging-api-overview"></a>Overzicht van Azure ML logboekregistratie API
De [API van Azure ML-logboekregistratie](reference-logging-api.md) is beschikbaar via de **azureml.logging** -module in Python (die wordt geïnstalleerd met de Azure ML-Workbench.) Na het importeren van deze module kunt u de **get_azureml_logger** methode instantiëren van een **berichtenlogboek** object.
Vervolgens gebruikt u het logboek **logboek** methode voor het opslaan van sleutel-waardeparen die wordt geproduceerd door uw Python-scripts.
Op dit moment logboekregistratie model metrieken van scalaire en typen worden ondersteund zoals wordt weergegeven.

```Python
# create a logger instance in already set up environment 
from azureml.logging import get_azureml_logger
logger = get_azureml_logger()

# log scalar (any integer or floating point type is fine)
logger.log("simple value", 7)


# log list
logger.log("all values", [5, 6, 7])
```
Eenvoudig te gebruiken van het logboek binnen uw projecten voor Azure ML-Workbench is en dit artikel wordt beschreven hoe u dit doet u.

## <a name="create-a-project-in-azure-ml-workbench"></a>Maak een Project in Azure ML-Workbench
Als u een project nog geen hebt, kunt u een van de [maken en installeer Quick Start](quickstart-installation.md) van de **projectdashboard**, u kunt openen de **iris_sklearn.py** script () zoals wordt weergegeven.)

![toegang tot een script vanaf het tabblad bestanden](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-01b.png)

U kunt dit script gebruiken als richtlijn voor verwachte model metriek logboekregistratie in Azure ML-implementatie.

## <a name="parameterize-and-log-model-metrics-from-script"></a>Parameter en meld u Model metrische gegevens uit het Script
In de **iris_sklearn.py** script, het verwachte patroon import en het logboek in Python kan worden teruggebracht naar de volgende regels code constructie.

```Python
from azureml.logging import get_azureml_logger
run_logger = get_azureml_logger()
```

Zodra gemaakt, kunt u aanroept de **logboek** methode met een naam/waarde-paar.

Wanneer ontwikkeling voltooid is, is het vaak nuttig om te voorzien van scripts zodat waarden kunnen worden doorgegeven via de opdrachtregel.
Het onderstaande voorbeeld laat zien hoe opdrachtregelparameters (indien aanwezig) met behulp van Python standaardbibliotheken accepteren.
Dit script gebruikt één parameter voor de frequentie regularisatie (*reg*) gebruikt voor het aanpassen van een classificatie-model in een poging om de *nauwkeurigheid* zonder te.
Deze variabelen worden vervolgens geregistreerd als *regularisatie snelheid* en *nauwkeurigheid* zodat het model met optimale resultaten kunt terugvinden.

```Python
# change regularization rate and you will likely get a different accuracy.
reg = 0.01
# load regularization rate from argument if present
if len(sys.argv) > 1:
    reg = float(sys.argv[1])

print("Regularization rate is {}".format(reg))

# log the regularization rate
run_logger.log("Regularization Rate", reg)

# train a logistic regression model on the training set
clf1 = LogisticRegression(C=1/reg).fit(X_train, Y_train)
print (clf1)

# evaluate the test set
accuracy = clf1.score(X_test, Y_test)
print ("Accuracy is {}".format(accuracy))

# log accuracy
run_logger.log("Accuracy", accuracy)
```

Om optimaal gebruik van deze stappen in uw scripts inschakelen **uitvoeren geschiedenis**.

## <a name="launch-runs-from-project-dashboard"></a>Starten vanuit projectdashboard wordt uitgevoerd
Wordt teruggezonden naar de **projectdashboard**, kunt u starten een **bijgehouden uitvoeren** door te selecteren de **iris_sklearn.py** script en in te voeren de **regularisatie snelheid**  parameter in de **argumenten** het invoervak.

![u parameters kunt invoeren en het starten van wordt uitgevoerd](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-05.png)

Omdat Azure ML-Workbench worden niet geblokkeerd als u starten bijgehouden wordt uitgevoerd, kunnen verschillende parallel worden gestart.
De status van elke bijgehouden uitvoering is zichtbaar in de **deelvenster taken** zoals wordt weergegeven.

![bijhouden wordt uitgevoerd in het deelvenster Taken](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-06.png)

Hierdoor optimaal brongebruik zonder elke taak in een serie uit te voeren.

## <a name="view-results-in-run-history"></a>De resultaten weergeven in de geschiedenis uitvoeren
Voortgang en resultaten van getraceerde wordt uitgevoerd, zijn beschikbaar voor analyse in Azure ML Workbench **uitvoeren geschiedenis**.
**Uitvoeringsgeschiedenis** biedt drie verschillende weergaven:
- Dashboard
- Details
- Vergelijking

De **Dashboard** weergave bevat gegevens over alle sessies van een bepaald script, weergegeven in formulieren voor zowel grafische als in tabelvorm.
De **Details** weergave bevat alle gegevens die zijn gegenereerd op basis van een specifieke uitvoering van een bepaald script, met inbegrip van de aangemelde metrische gegevens en uitvoerbestanden (zoals weergegeven worden uitgezet.) De **vergelijking** stelt resultaten van twee of drie uitgevoerd om te worden bekeken side-by-side, waaronder ook metrische gegevens in het logboek geregistreerd en uitvoerbestanden.

Bijgehouden uitvoert in acht **iris_sklearn.py**, waarden voor de **regularisatie snelheid** parameter en **nauwkeurigheid** resultaat zijn geregistreerd om te zien hoe de uitvoeren als gebruiken Geschiedenis van weergaven.

### <a name="run-history-dashboard"></a>Dashboard van de geschiedenis uitvoeren
De resultaten van alle acht wordt uitgevoerd, zijn zichtbaar in de **uitvoeren geschiedenis Dashboard**.
Als **iris_sklearn.py** logboeken *regularisatie snelheid* en *nauwkeurigheid*, wordt de **uitvoeren geschiedenis Dashboard** diagrammen voor deze waarden worden weergegeven Standaard.

![dashboard van de geschiedenis uitvoeren](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-07.png)

De **uitvoeren geschiedenis Dashboard** kan worden aangepast zodat vastgelegde waarden worden ook in het raster weergegeven.  Te klikken op de **aanpassen** pictogram wordt weergegeven de **weergave aanpassing** dialoog, zoals wordt weergegeven.

![uitvoeringsgeschiedenis dashboard raster aanpassen](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-08.png)

Alle waarden die zijn vastgelegd tijdens bijgehouden wordt uitgevoerd zijn beschikbaar voor weergeven en selecteren **regularisatie snelheid** en **nauwkeurigheid** voegt deze toe aan het raster.

![vastgelegde waarden in aangepaste raster](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-09.png)

Het is gemakkelijk te vinden interessante wordt uitgevoerd door de muiswijzer op punten in de grafieken.  Voer 7 hebben in dit geval een goede nauwkeurigheid samen met een lage duur opgeleverd.

![zoeken naar een interessante uitvoeren](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-10.png)

Te klikken op een punt die zijn gekoppeld aan 7 worden uitgevoerd in een grafiek of de koppeling tot en met 7 worden uitgevoerd in het raster geeft de **Details uitvoering van geschiedenis**.

### <a name="run-history-details"></a>Geschiedenisdetails uitvoeren
In deze weergave worden de volledige resultaten van de 7 uitvoeren samen met alle artefacten die zijn geproduceerd door 7 uitvoeren weergegeven.

![geschiedenisdetails uitvoeren](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-11.png)

De **Details uitvoering van geschiedenis** weergave biedt tevens de mogelijkheid om te **downloaden** bestanden geschreven naar de **. / levert** map (deze bestanden worden ondersteund door Azure ML Workbench cloud-opslag voor de geschiedenis uitvoeren, die het onderwerp van een ander artikel.)

Ten slotte **Details uitvoering van geschiedenis** biedt een manier om te herstellen van uw project de status op het moment van dit wordt uitgevoerd.
Te klikken op de **herstellen** knop een bevestiging dialoog weergegeven, zoals wordt weergegeven.

![herstel uitvoeren bevestigen](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-13.png)

Als bevestigd, worden de bestanden kunnen worden overschreven of verwijderd, dus zorgvuldig gebruikmaken van deze functie.

### <a name="run-history-comparison"></a>Vergelijking van de geschiedenis uitvoeren
Twee of drie selecteren die u uitvoert de **uitvoeren geschiedenis Dashboard** en te klikken op **vergelijken** keert u terug naar de **uitvoeren geschiedenis vergelijking** weergeven.
U kunt ook klikken op **vergelijken** en het selecteren van een uitvoering binnen de **Details uitvoering van geschiedenis** weergave ook keert u terug naar de **uitvoeren geschiedenis vergelijking** weergeven.
In beide gevallen moet de **uitvoeren geschiedenis vergelijking** weergave biedt een manier om te zien van de aangemelde resultaten en de artefacten van twee of drie wordt uitgevoerd naast elkaar.

![vergelijking van de geschiedenis uitvoeren](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-12.png)

Deze weergave is vooral nuttig voor vergelijking van waarnemingspunten, maar in het algemeen alle eigenschappen van wordt uitgevoerd, hier kunnen worden vergeleken.

### <a name="command-line-interface"></a>Opdrachtregelinterface
Azure Machine Learning Workbench biedt ook toegang tot de geschiedenis uitvoeren via de **Command Line Interface**.
Voor toegang tot de **Command Line Interface**, klikt u op de **opdrachtprompt openen** menu zoals wordt weergegeven.

![opdrachtprompt openen](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-14.png)

De opdrachten die beschikbaar zijn voor de geschiedenis uitvoeren toegankelijk zijn `az ml history`, met de online help beschikbaar door toe te voegen de `-h` vlag.
```
$ az ml history -h

Group
    az ml history: View run history of machine learning experiments.

Commands:
    compare : Compare two runs.
    download: Download all the artifacts from a run into the destination path.
    info    : Details about one run.
    last    : Get detail about most recent run.
    list    : List runs.
    promote : Promote Artifacts.
```
Deze opdrachten bieden dezelfde voorzieningen en dezelfde gegevens weergegeven retourneren de **uitvoeren geschiedenis weergaven**.
De resultaten van de laatste uitvoering kunnen bijvoorbeeld worden weergegeven als een JSON-object.
```
$ az ml history last
{
  "Accuracy": 0.6792452830188679,
  "Regularization Rate": 0.078125,
  "attachments": "control_log, control_log.txt, driver_log, driver_log.txt, pid.txt, dataprep/backgroundProcess.log, dataprep/backgroundProcess_Engine.log, dataprep/backgroundProcess_EngineHost.log, dataprep/backgroundProcess_ProjectProvider.log, dataprep/backgroundProcess_Sampling.log, dataprep/backgroundProcess_Telemetry.log, outputs/cm.png, outputs/model.pkl, outputs/sdk_debug.txt, outputs/roc.png",
  "created_utc": "2017-09-08T00:58:01.611105+00:00",
  "description": null,
  "duration": "0:00:04.892389",
  "end_time_utc": "2017-09-08T00:58:07.951403+00:00",
  "experiment_id": "ce92d0a9-3e2c-4d51-85de-93ef22249ce1",
  "heartbeat_enabled": true,
  "hidden": false,
  "name": null,
  "parent_run_id": null,
  "properties": {
    "CommitId": "e77a5f0df2af1a482bbe39b70bfbb16b62228cb3"
  },
  "run_id": "IrisDemo_1504832281116",
  "run_number": 8,
  "script_name": "iris_sklearn.py",
  "start_time_utc": "2017-09-08T00:58:03.059014+00:00",
  "status": "Completed",
  "target": "local",
  "user_id": "e9fafe06-b0e4-4154-8374-aae34f9977b2"
}
```
De lijst met alle sessies kan ook worden weergegeven in tabelvorm.
```
$ az ml history list -o table
  Accuracy    Regularization Rate  Duration        Run_id                  Script_name      Start_time_utc                    Status
----------  ---------------------  --------------  ----------------------  ---------------  --------------------------------  ---------
  0.679245               0.078125  0:00:04.892389  IrisDemo_1504832281116  iris_sklearn.py  2017-09-08T00:58:03.059014+00:00  Completed
  0.679245               0.15625   0:00:04.734956  IrisDemo_1504832255198  iris_sklearn.py  2017-09-08T00:57:38.507196+00:00  Completed
  0.660377               0.3125    0:00:04.913762  IrisDemo_1504832234904  iris_sklearn.py  2017-09-08T00:57:16.849881+00:00  Completed
  0.660377               0.625     0:00:06.107764  IrisDemo_1504832210767  iris_sklearn.py  2017-09-08T00:56:54.602813+00:00  Completed
  0.641509               1.25      0:00:04.742727  IrisDemo_1504832171373  iris_sklearn.py  2017-09-08T00:56:13.879280+00:00  Completed
  0.660377               2.5       0:00:04.915401  IrisDemo_1504832148526  iris_sklearn.py  2017-09-08T00:55:52.937083+00:00  Completed
  0.641509               5         0:00:04.730627  IrisDemo_1504832127172  iris_sklearn.py  2017-09-08T00:55:29.612382+00:00  Completed
  0.641509              10         0:00:06.059082  IrisDemo_1504832109906  iris_sklearn.py  2017-09-08T00:55:14.739806+00:00  Completed

```
De **Command Line Interface** is een alternatief pad voor toegang tot de macht van Azure Machine Learning-Workbench.

## <a name="next-steps"></a>Volgende stappen
Deze functies zijn beschikbaar om te helpen bij het proces van gegevens wetenschappelijke experimenteren.
We hopen dat u ze nuttig vinden en zouden aanzienlijk waarderen uw feedback.
Dit is slechts onze initiële implementatie en hebben we veel verbeteringen die zijn gepland.
We graag continu ze af te leveren de Azure Machine Learning-Workbench. 
