---
title: Hoe u uitvoeren geschiedenis en het Model metrische gegevens in Azure Machine Learning Workbench | Microsoft Docs
description: Gids voor het gebruik van de Uitvoeringsgeschiedenis en metrische gegevens over Model functies van Azure Machine Learning Workbench
services: machine-learning
author: rastala
ms.author: roastala
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/07/2017
ms.openlocfilehash: 34fe72087a3de133d65ea4a4737ab5dba45242f4
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35643818"
---
# <a name="how-to-use-run-history-and-model-metrics-in-azure-machine-learning-workbench"></a>Het gebruik Uitvoeringsgeschiedenis en metrische gegevens van Model in Azure Machine Learning Workbench

Azure Machine Learning Workbench biedt ondersteuning voor data science experimenten via de **Run History** en **Model metrische gegevens** functies.
**Uitvoeringsgeschiedenis** biedt een manier om bij te houden van de uitvoer van uw machine learning-experimenten, en klikt u vervolgens kunt filteren en vergelijking van de resultaten ervan.
**Model metrische gegevens** kunnen worden geregistreerd op elk moment uw scripts, bijhouden welke waarden in uw data science-experimenten het belangrijkst zijn.
Dit artikel wordt beschreven hoe u effectief gebruik van deze functies om de snelheid en de kwaliteit van uw data science-experimenten te verhogen.

## <a name="prerequisites"></a>Vereisten
Als u wilt in deze gebruiksaanwijzing kunt doorlopen, moet u naar:
* [Maak en installeer Azure Machine Learning](../service/quickstart-installation.md)
- [Een Project maken](../service/quickstart-installation.md)


## <a name="azure-ml-logging-api-overview"></a>API-overzicht van Azure ML-logboekregistratie
De [API van Azure ML-logboekregistratie](reference-logging-api.md) is beschikbaar via de **azureml.logging** module in Python (die is geïnstalleerd met de Azure ML Workbench.) Na het importeren van deze module kunt u de **get_azureml_logger** methode voor het starten van een **logger** object.
Vervolgens kunt u van het logboek **log** methode voor het opslaan van sleutel/waarde-paren die worden geproduceerd door uw Python-scripts.
Op dit moment logboekregistratie model metrische gegevens van de scalaire en lijst met typen worden ondersteund zoals wordt weergegeven.

```Python
# create a logger instance in already set up environment 
from azureml.logging import get_azureml_logger
logger = get_azureml_logger()

# log scalar (any integer or floating point type is fine)
logger.log("simple value", 7)


# log list
logger.log("all values", [5, 6, 7])
```
Het is eenvoudig te gebruiken van het logboek in uw Azure ML Workbench-projecten en in dit artikel leest u hoe om dit te doen.

## <a name="create-a-project-in-azure-ml-workbench"></a>Een Project maken in Azure ML Workbench
Als u nog een project hebt, kunt u een van de [maken en snelstartgids installeren](../service/quickstart-installation.md) van de **projectdashboard**, kunt u openen de **iris_sklearn.py** script () zoals wordt weergegeven.)

![toegang tot een script op het tabblad bestanden](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-01b.png)

U kunt dit script gebruiken als richtlijn voor de verwachte implementatie van model metrische gegevens in Azure ML logboekregistratie.

## <a name="parameterize-and-log-model-metrics-from-script"></a>Parameteriseren en meld u Model metrische gegevens van Script
In de **iris_sklearn.py** script, het verwachte patroon moet worden geïmporteerd om de logger in Python kan worden teruggebracht naar de volgende regels code voor te bereiden.

```Python
from azureml.logging import get_azureml_logger
run_logger = get_azureml_logger()
```

Nadat u hebt gemaakt, kunt u aanroepen de **log** methode met een naam/waarde-paar.

Wanneer ontwikkeling voltooid is, is het vaak nuttig om te voorzien van scripts, zodat de waarden kunnen worden doorgegeven via de opdrachtregel.
Het onderstaande voorbeeld laat zien hoe om te accepteren van opdrachtregelparameters (indien aanwezig) met behulp van standaard Python-bibliotheken.
Met dit script wordt één parameter voor de Regularisatiesnelheid (*reg*) gebruikt voor het aanpassen van een model classificatie in een poging om de *nauwkeurigheid* zonder te.
Deze variabelen worden vervolgens geregistreerd als *Regularisatiefrequentie* en *nauwkeurigheid* zodat het model met optimale resultaten gemakkelijk kan worden geïdentificeerd.

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

Om optimaal gebruik van deze stappen in uw scripts inschakelen **Run History**.

## <a name="launch-runs-from-project-dashboard"></a>Starten vanuit het projectdashboard wordt uitgevoerd
Retourneren aan de **projectdashboard**, kunt u starten een **bijgehouden uitvoeren** door het selecteren van de **iris_sklearn.py** script en het invoeren van de **regularisatiefrequentie**  parameter in de **argumenten** vak bewerken.

![parameters invoeren en starten van uitvoeringen](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-05.png)

Sinds het starten van bijgehouden wordt uitgevoerd, wordt Azure ML Workbench niet geblokkeerd, kunnen verschillende parallel worden gestart.
De status van elke bijgehouden uitvoering wordt weergegeven in de **deelvenster Jobs** zoals wordt weergegeven.

![tracering wordt uitgevoerd in het deelvenster Taken](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-06.png)

Hiermee kunt optimaal gebruik van resources zonder elke taak wordt uitgevoerd in een serie.

## <a name="view-results-in-run-history"></a>Resultaten weergeven in de Uitvoeringsgeschiedenis
Voortgang en resultaten van bijgehouden uitvoeringen zijn beschikbaar voor analyse in Azure ML Workbench **Run History**.
**Uitvoeringsgeschiedenis** biedt drie verschillende weergaven:
- Dashboard
- Details
- Vergelijking

De **Dashboard** weergave bevat gegevens over alle uitvoeringen van een bepaald script, gerenderd in formulieren voor zowel grafische als in tabelvorm.
De **Details** weergave bevat alle gegevens die zijn gegenereerd op basis van een specifieke uitvoering van een bepaald script, met inbegrip van geregistreerde metrische gegevens en uitvoerbestanden (zoals weergegeven die zichtbaar zijn.) De **vergelijking** weergave kunt u resultaten van twee of drie runs worden bekeken side-by-side, ook met inbegrip van geregistreerde metrische gegevens en uitvoerbestanden.

Bijgehouden in acht uitvoeringen van **iris_sklearn.py**, waarden voor de **regularisatiefrequentie** parameter en **nauwkeurigheid** resultaat zijn geregistreerd om u te laten zien hoe u de uitvoeren als gebruiken Geschiedenis van weergaven.

### <a name="run-history-dashboard"></a>Uitvoeringsgeschiedenisdashboard
De resultaten van alle acht uitvoeringen zijn zichtbaar in de **Run geschiedenis Dashboard**.
Als **iris_sklearn.py** logboeken *Regularisatiefrequentie* en *nauwkeurigheid*, wordt de **Run geschiedenis Dashboard** diagrammen voor deze waarden worden weergegeven Standaard.

![uitvoeringsgeschiedenisdashboard](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-07.png)

De **Run geschiedenis Dashboard** kan worden aangepast zodat vastgelegde waarden worden ook in het raster weergegeven.  Te klikken op de **aanpassen** pictogram wordt weergegeven de **aanpassing van weergave** dialoog, zoals wordt weergegeven.

![uitvoeringsgeschiedenis dashboard raster aanpassen](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-08.png)

Alle waarden die zijn vastgelegd tijdens bijgehouden uitvoeringen zijn beschikbaar om weer te geven en te selecteren **Regularisatiefrequentie** en **nauwkeurigheid** toegevoegd aan het raster.

![vastgelegde waarden in aangepaste raster](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-09.png)

Het is gemakkelijk te vinden van interessante wordt uitgevoerd door de muiswijzer op punten in de grafieken.  In dit geval resulteerde 7 uitvoert in een goede nauwkeurigheid in combinatie met een lage duur.

![zoeken naar een interessante uitvoeren](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-10.png)

Een punt dat is gekoppeld aan 7 worden uitgevoerd in een grafiek of de koppeling tot en met 7 worden uitgevoerd in het raster wordt weergegeven op de **Details uitvoering van geschiedenis**.

### <a name="run-history-details"></a>Geschiedenisdetails van de uitvoering
In deze weergave worden de volledige resultaten van de 7 worden uitgevoerd, samen met alle artefacten die worden geproduceerd door 7 uitvoert weergegeven.

![geschiedenisdetails van de uitvoering](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-11.png)

De **Details uitvoering van geschiedenis** weergave biedt ook de mogelijkheid voor **downloaden** alle bestanden die zijn geschreven naar de **. / levert** map (deze bestanden worden ondersteund door Azure ML Workbench opslag in de cloud voor Run History, namelijk het onderwerp van een ander artikel.)

Ten slotte **Details uitvoering van geschiedenis** biedt een manier om te herstellen van uw project de status op het moment van deze uitvoering.
Te klikken op de **herstellen** knop wordt een dialoogvenster voor het bevestigen, zoals wordt weergegeven.

![herstel uitvoeren bevestigen](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-13.png)

Als bevestigd, worden de bestanden worden overschreven of verwijderd, dus gebruik van deze functie zorgvuldig.

### <a name="run-history-comparison"></a>Vergelijking van de geschiedenis uitvoeren
Selecteer twee of drie wordt uitgevoerd in de **Run geschiedenis Dashboard** en te klikken op **vergelijken** keert u terug naar de **vergelijking van de geschiedenis uitvoeren** weergeven.
U kunt ook klikken **vergelijken** en selecteren van een uitvoeren binnen de **Details uitvoering van geschiedenis** weergave ook keert u terug naar de **vergelijking van de geschiedenis uitvoeren** weergeven.
In beide gevallen moet de **vergelijking van de geschiedenis uitvoeren** weergave biedt een manier om te zien van de geregistreerde resultaten en de artefacten van twee of drie runs naast elkaar.

![vergelijking van de geschiedenis uitvoeren](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-12.png)

In deze weergave is vooral handig voor vergelijking van grafieken, maar in het algemeen alle eigenschappen van uitvoeringen hier kunnen worden vergeleken.

### <a name="command-line-interface"></a>Opdrachtregelinterface
Azure Machine Learning Workbench biedt ook toegang tot de geschiedenis uitvoeren via de **Command Line Interface**.
Toegang krijgen tot de **Command Line Interface**, klikt u op de **Open Command Prompt** menu zoals wordt weergegeven.

![opdrachtprompt openen](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-14.png)

De opdrachten die beschikbaar zijn voor de Run History zijn toegankelijk via `az ml history`, met de online help beschikbaar door toe te voegen de `-h` vlag.
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
Deze opdrachten bieden dezelfde voorzieningen en terug te keren dezelfde gegevens die worden weergegeven de **uitvoeren geschiedenis weergaven**.
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
De lijst met alle uitvoeringen kan ook worden weergegeven in tabelvorm.
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
De **Command Line Interface** is een alternatieve route voor toegang tot de kracht van Azure Machine Learning Workbench.

## <a name="next-steps"></a>Volgende stappen
Deze functies zijn beschikbaar om te helpen met het proces van data science experimenten.
We hopen dat u ze handig vinden en zou aanzienlijk waarderen uw feedback.
Dit is slechts onze eerste implementatie, en we hebben veel verbeteringen gepland.
We hopen dat deze continu te leveren aan de Azure Machine Learning Workbench. 
