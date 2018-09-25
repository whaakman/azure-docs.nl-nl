---
title: Afstemmen van hyperparameters voor uw model met behulp van Azure Machine Learning
description: Informatie over het afstemmen van de hyperparameters voor uw deep learning / machine learning-model met behulp van Azure Machine Learning-service. Hier ziet u hoe u de parameter zoeken ruimte definiëren, geeft u een primaire metrische waarde om te optimaliseren en voortijdig beëindigen slecht presterend configuraties.
ms.author: swatig
author: swatig007
ms.reviewer: sgilley
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 205a4d454be02d64058e3d0fcffda35df5b831a5
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46971668"
---
# <a name="tune-hyperparameters-for-your-model"></a>Afstemmen van hyperparameters voor uw model

In dit artikel leert u efficiënt afstemmen van hyperparameters voor uw model. Hier ziet u hoe u de parameter zoeken ruimte definiëren, geeft u een primaire metrische waarde om te optimaliseren en voortijdig beëindigen slecht presterend configuraties. U kunt ook de verschillende trainingsuitvoeringen visualiseren en selecteert u het beste configuratie uit te voeren voor uw model.

## <a name="what-are-hyperparameters"></a>Wat zijn hyperparameters?
Hyperparameters zijn aanpasbare parameters gekozen vóór het trainen van een model dat van toepassing is het trainingsproces zelf. Bijvoorbeeld, voordat een training een deep neural network, moet u bepalen het aantal verborgen lagen in het netwerk en het aantal knooppunten in elke laag. Deze waarden blijven doorgaans constant tijdens de training.

In uitgebreide machineleren / machine learning-scenario's modelprestaties is afhankelijk van sterk op basis van de waarden van de hyperparameter geselecteerd. Het doel van hyperparameter verkenning is om te zoeken in configuraties met verschillende hyperparameter te vinden van een configuratie die in de gewenste prestaties resulteert. Het proces voor het verkennen van hyperparameter is meestal desktoptoepassingen handmatige, gezien het feit dat de search-ruimte vast is en evaluatie van elke configuratie kan duur zijn.

Azure Machine Learning kunt u deze verkenning hyperparameter automatiseren op een efficiënte manier, waardoor u veel tijd en resources. Kunt u het bereik van hyperparameter waarden om te verkennen en een maximum aantal training voor deze verkenning wordt uitgevoerd. Het systeem vervolgens automatisch wordt gestart met de andere parameterconfiguraties meerdere gelijktijdige trainingsuitvoeringen en vindt de configuratie die in de beste prestaties resulteert wordt gemeten door een metrische waarde door de gebruiker gekozen. Trainingsuitvoeringen van slecht presterende zijn automatisch vroege beëindigd, verspilling van compute-resources beperken. Deze resources worden in plaats daarvan gebruikt om andere configuraties hyperparameter verkennen.

Als u wilt afstemmen van hyperparameters voor uw model met behulp van Azure Machine Learning-service, moet u als volgt:
* De ruimte van de zoekopdracht hyperparameter definiëren
* Geef een primaire metrische waarde om te optimaliseren
* Een beleid voor vroegtijdige beëindiging opgeven
* Bronnen voor het afstemmen van hyperparameter toewijzen
* Een experiment met de bovenstaande configuratie starten

## <a name="define-the-hyperparameter-search-space"></a>De ruimte van de zoekopdracht hyperparameter definiëren
Azure Machine Learning-service wordt automatisch hyperparameters afstemt door het bereik van waarden die zijn gedefinieerd voor elke hyperparameter verkennen.

### <a name="types-of-hyperparameters"></a>Typen hyperparameters
Elke hyperparameter kan discrete of continue zijn.

#### <a name="discrete-hyperparameters"></a>Discrete hyperparameters 
Discrete hyperparameters kan worden opgegeven als een `choice` tot discrete waarden. `choice` Voer een van beide een of meer door komma's gescheiden waarden, een `range` object of een willekeurige `list` object. Bijvoorbeeld  

```Python
    {    
        "batch_size": choice(16, 32, 64, 128)
        "number_of_hidden_layers": choice(range(1,5))
    }
```

In dit geval batch_size kunnen worden uitgevoerd op een van de waarden [16, 32, 64, 128] en number_of_hidden_layers kunnen worden uitgevoerd op een van de waarden [1, 2, 3, 4].

Geavanceerde discrete hyperparameters kan ook worden opgegeven met behulp van een distributiepunt. De volgende distributies worden ondersteund:
* `quniform(low, high, q)` -Retourneert een waarde, zoals round (uniform (laag, hoog) / q) * q
* `qloguniform(low, high, q)` -Retourneert een waarde, zoals round (exp (uniform (laag, hoog)) / q) * q
* `qnormal(mu, sigma, q)` -Retourneert een waarde, zoals round (normaal (mu, sigma) / q) * q
* `qlognormal(mu, sigma, q)` -Retourneert een waarde, zoals round (exp (normaal (mu, sigma)) / q) * q

#### <a name="continuous-hyperparameters"></a>Continue hyperparameters 
Continue hyperparameters kan worden opgegeven als een distributiepunt via een continue reeks waarden. Ondersteunde distributies opnemen-
* `uniform(low, high)` -Retourneert een gelijkmatig verdeeld over lage en hoge waarde
* `loguniform(low, high)` -Retourneert een waarde getekend op basis van exp (uniform (laag, hoog)), zodat de logaritme van de geretourneerde waarde is gelijkmatig verdeeld.
* `normal(mu, sigma)` -Een werkelijke waarde retourneert die normaal gesproken wordt gedistribueerd met gemiddelde mu en de standaarddeviatie sigma
* `lognormal(mu, sigma)` -Retourneert een waarde getekend op basis van exp (normaal (mu, sigma)), zodat de logaritme van de geretourneerde waarde wordt normaal gesproken gedistribueerd

Hier volgt een voorbeeld van een parameterdefinitie ruimte:

```Python
    {    
        "learning_rate": normal(10, 3),
        "keep_probability": uniform(0.05, 0.1)
    }
```

In dit voorbeeld wordt een spatie zoeken met twee parameters - learning_rate en keep_probability gedefinieerd. learning_rate heeft een normale distributie met een gemiddelde waarde 10 en de standaarddeviatie van 3. keep_probability heeft een uniform distributie met een minimale waarde van 0,05 en een maximumwaarde van 0,1.

### <a name="sampling-the-hyperparameter-space"></a>De ruimte hyperparameter steekproeven
De gebruiker geeft ook de parameter methode te gebruiken via de definitie van de opgegeven hyperparameter-ruimte. Azure Machine Learning-service ondersteunt willekeurige steekproef nemen, raster steekproeven en Bayesiaanse steekproeven.

#### <a name="random-sampling"></a>Steekproeven
In willekeurig Samples hyperparameter waarden willekeurig geselecteerd uit de gedefinieerde search-ruimte. Steekproeven kan de ruimte zoeken om op te nemen van zowel discrete als doorlopende hyperparameters. Bijvoorbeeld

```Python
from azureml.train.hyperdrive import RandomParameterSampling
param_sampling = RandomParameterSampling( {
        "learning_rate": normal(10, 3),
        "keep_probability": uniform(0.05, 0.1),
        "batch_size": choice(16, 32, 64, 128)
    }
)
```

#### <a name="grid-sampling"></a>Raster steekproeven
Raster steekproeven uitvoert een eenvoudige grid zoeken van alle mogelijke waarden in de ruimte gedefinieerde zoekopdracht. Kan alleen worden gebruikt met de opgegeven met behulp van hyperparameters `choice`. De volgende ruimte heeft bijvoorbeeld een aantal voorbeelden van zes-

```Python
from azureml.train.hyperdrive import GridParameterSampling
param_sampling = GridParameterSampling( {
        "num_hidden_layers": choice(1, 2, 3),
        "batch_size": choice(16, 32)
    }
)
```

#### <a name="bayesian-sampling"></a>Bayesiaanse steekproeven
Bayesiaanse steekproeven is gebaseerd op de Bayesiaanse optimalisatie-algoritme en intelligente keuzes maakt op basis van de waarden hyperparameter naar het volgende voorbeeld. Het kiest in dit voorbeeld op basis van hoe de vorige voorbeelden uitgevoerd, zoals dat het nieuwe voorbeeld verbetert de gerapporteerde primaire metriek.

Wanneer u Bayesiaanse steekproeven, heeft het aantal gelijktijdige uitvoeringen een invloed op de effectiviteit van het proces voor afstemmen. Normaal gesproken kan een kleiner aantal gelijktijdige uitvoeringen leiden tot betere steekproeven convergentie. Dit komt doordat de kleinere graad van parallelle uitvoering verhoogt het aantal uitvoeringen die baat bij eerder voltooid wordt uitgevoerd hebben.

Bayesiaanse steekproeven ondersteunt alleen `choice` en `uniform` distributies boven de ruimte zoeken. Bijvoorbeeld 

```Python
from azureml.train.hyperdrive import BayesianParameterSampling
param_sampling = BayesianParameterSampling( {
        "learning_rate": uniform(0.05, 0.1),
        "batch_size": choice(16, 32, 64, 128)
    }
)
```

> [!NOTE]
> Bayesiaanse steekproeven biedt momenteel geen ondersteuning voor vroegtijdige beëindiging beleid (Zie [opgeven van een beleid voor vroegtijdige beëindiging](#specify-an-early-termination-policy)). Als Bayesiaanse parameter steekproeven wordt gebruikt, moet u beleid instellen op `None`. Een beleid voor beëindiging niet op te geven met Bayesiaanse Samples heeft hetzelfde effect.
>
> ```Python
> early_termination_policy = None
> ```

## <a name="specify-a-primary-metric-to-optimize"></a>Geef een primaire metrische waarde om te optimaliseren
Bij het afstemmen van hyperparameters, moet u de primaire metrische gegevens die u wilt dat de hyperparameter afstemmen experiment tot het Optimaliseer opgeven. Elke uitvoering training wordt geëvalueerd voor deze primaire metrische gegevens en slecht presterend wordt uitgevoerd (waarbij de primaire metrische waarde voldoet niet aan criteria die zijn ingesteld door het beleid voor vroegtijdige beëindiging) wordt beëindigd. Naast het opgeven van de primaire naam van de meetwaarde, moet u ook het doel van de optimalisatie - opgeven of u wilt maximaliseren of beperken van de primaire metrische gegevens.
* `primary_metric_name`: De naam van de primaire metrische gegevens om te optimaliseren. De naam van de primaire metriek moet exact overeenkomen met de naam van de metrische gegevens vastgelegd door het trainingsscript. Zie [Meld u metrische gegevens voor het afstemmen van hyperparameter](#log-metrics-for-hyperparameter-tuning).
* `primary_metric_goal`: Het kan zijn `PrimaryMetricGoal.MAXIMIZE` of `PrimaryMetricGoal.MINIMIZE` en bepaalt of de primaire metrische gegevens worden gemaximaliseerd of geminimaliseerd bij het evalueren van de wordt uitgevoerd. 

Bijvoorbeeld:
```Python
primary_metric_name="accuracy",
primary_metric_goal=PrimaryMetricGoal.MAXIMIZE
```
Dit optimaliseert de wordt uitgevoerd voor maximale 'nauwkeurigheid'.

### <a name="log-metrics-for-hyperparameter-tuning"></a>Logboek metrische gegevens voor het afstemmen van hyperparameter
Als u wilt gebruiken in Azure Machine Learning-service voor het afstemmen van hyperparameter, moet het trainingsscript voor uw model relevante metrische gegevens rapporteren wanneer het model wordt uitgevoerd. Geeft de primaire metrische gegevens die ze willen dat de service moet gebruiken voor het evalueren van de prestaties van de uitvoering van de gebruiker en de trainingsscript moet zich aanmelden met deze metriek. Zie [opgeven van een primaire metrische waarde voor het Optimaliseer](#specify-a-primary-metric-to-optimize).

U kunt uw trainingsscript om u te melden met deze metriek bijwerken met behulp van het volgende fragment van de voorbeeld -

```Python
from azureml.core.run import Run
run_logger = Run.get_submitted_run()
run_logger.log("accuracy", float(val_accuracy))
```

In dit voorbeeld wordt de trainingsscript berekent de `val_accuracy` en registreert deze 'nauwkeurig', die wordt gebruikt als de primaire metrische gegevens. Het is aan de ontwikkelaar model om te bepalen hoe vaak deze metrische gegevens rapporteren.

## <a name="specify-an-early-termination-policy"></a>Een beleid voor vroegtijdige beëindiging opgeven
Met Azure Machine Learning-service voor het afstemmen van hyperparameters, slecht presterend wordt uitgevoerd wanneer automatisch vroege worden beëindigd. Dit vermindert verspilling van resources en in plaats daarvan deze resources worden gebruikt voor het verkennen van de andere parameterconfiguraties.

Wanneer u een beleid voor vroegtijdige beëindiging, een gebruiker kan worden gebruikt voor het configureren van de volgende parameters die wanneer een beleid wordt toegepast bepalen:
* `evaluation_interval`: de frequentie voor het toepassen van het beleid. Telkens wanneer de training de primaire metriek scriptlogboeken telt als één interval. Dus een `evaluation_interval` van 1 wordt het beleid wordt toegepast wanneer het trainingsscript de primaire metrische gegevens rapporteert. Een `evaluation_interval` van 2 wordt het beleid toepassen andere wanneer het trainingsscript de primaire metrische gegevens rapporteert. Dit is een optionele parameter en indien niet opgegeven, `evaluation_interval` is standaard ingesteld op 1.
* `delay_evaluation`: de eerste evaluatie van het beleid voor een opgegeven aantal intervallen uitstelt. Dit is een optionele parameter waarmee alle configuraties om uit te voeren voor een initiële minimum aantal intervallen, voortijdige beëindiging van trainingsuitvoeringen voorkomen. Als u opgeeft, is het beleid van toepassing is elke meervoud van evaluation_interval die groter zijn dan of gelijk is aan delay_evaluation.

Azure Machine Learning-service ondersteunt de volgende beleidsregels voor vroegtijdige beëindiging-

### <a name="bandit-policy"></a>Bandieten beleid
Bandieten beleid is een beëindiging-beleid op basis van slack factor/slack bedrag en de evaluatie-interval. Dit beleid wordt voortijdig beëindigd geen runs worden gestart wanneer de primaire metriek valt niet binnen het opgegeven slack factor / slack bedrag met betrekking tot de best presterende training run. Het duurt voordat de volgende configuratieparameters-
* `slack_factor` of `slack_amount`: de toegestane vertraging toegestaan met betrekking tot de best presterende training uitvoeren. `slack_factor` Hiermee geeft u de toegestane vertraging als een verhouding. `slack_amount` Hiermee geeft u de toegestane vertraging als absoluut bedrag, in plaats van een verhouding.

    Bijvoorbeeld, kunt u overwegen een bandieten-beleid wordt toegepast op basis van interval 10. Wordt ervan uitgegaan dat een primaire metric 0,8 met als doel om te maximaliseren van de primaire metrische gegevens het beste uitvoeren van uitvoering op interval 10 gerapporteerd. Als het beleid is opgegeven met een `slack_factor` van 0,2, een training wordt uitgevoerd, waarvan aanbevolen metrische gegevens op basis van interval 10 is kleiner dan 0.66 (0,8 / (1 +`slack_factor`)) wordt beëindigd. Als er in plaats daarvan het beleid is opgegeven met een `slack_amount` van 0,2, een training wordt uitgevoerd, waarvan aanbevolen metrische gegevens op basis van interval 10 is kleiner dan 0,6 (0,8 - `slack_amount`) wordt beëindigd.
* `evaluation_interval`: de frequentie voor het toepassen van het beleid (optionele parameter).
* `delay_evaluation`: de eerste evaluatie van het beleid voor een opgegeven aantal intervallen (optionele parameter) uitstelt.

Bekijk het volgende voorbeeld:

```Python
from azureml.train.hyperdrive import BanditPolicy
early_termination_policy = BanditPolicy(slack_factor = 0.1, evaluation_interval=1, delay_evaluation=5)
```

In dit voorbeeld wordt het beleid voor vroegtijdige beëindiging toegepast op elke interval wanneer metrische gegevens worden gerapporteerd, beginnend bij de evaluatie-interval 5. Elke run waarvan aanbevolen waarde is kleiner is dan (1/(1+0.1) of 91% van de best presterende uitvoeren wordt beëindigd.

### <a name="median-stopping-policy"></a>Mediaan beleid stoppen
Mediaan is stoppen een vroegtijdige beëindiging beleid op basis van het uitvoeren van gemiddelden van primaire metrische gegevens die zijn gerapporteerd door de wordt uitgevoerd. Dit beleid uitgevoerd gemiddelden berekent voor alle trainingsuitvoeringen en wordt uitgevoerd waarvan de prestaties slechter dan de mediaan van de actieve gemiddelden is beëindigd. Dit beleid bevat de volgende configuratieparameters-
* `evaluation_interval`: de frequentie voor het toepassen van het beleid (optionele parameter).
* `delay_evaluation`: de eerste evaluatie van het beleid voor een opgegeven aantal intervallen (optionele parameter) uitstelt.

Bekijk het volgende voorbeeld:

```Python
from azureml.train.hyperdrive import MedianStoppingPolicy
early_termination_policy = MedianStoppingPolicy(evaluation_interval=1, delay_evaluation=5)
```

In dit voorbeeld wordt het beleid voor vroegtijdige beëindiging toegepast op elke interval dat begint bij de evaluatie-interval 5. Een uitvoering worden op basis van interval 5 beëindigd als de beste primaire metriek slechter dan de mediaan van de actieve gemiddelden boven intervallen 1:5 tussen alle trainingsuitvoeringen.

### <a name="truncation-selection-policy"></a>Beleid voor afkapping van selectie
Een bepaald percentage van de laagste uitvoering wordt uitgevoerd op elke evaluatie-interval moet worden afgekapt selectiebeleid wilt annuleren. Uitvoeringen worden vergeleken op basis van hun prestaties op de primaire metrische gegevens en de laagste X % worden beëindigd. Het duurt voordat de volgende configuratieparameters-
* `truncation_percentage`: het percentage van de uitvoering van de laagste uitgevoerd om te beëindigen elke evaluatie-interval. Dit moet een geheel getal tussen 1 en 99.
* `evaluation_interval`: de frequentie voor het toepassen van het beleid (optionele parameter).
* `delay_evaluation`: de eerste evaluatie van het beleid voor een opgegeven aantal intervallen (optionele parameter) uitstelt.

Bekijk het volgende voorbeeld:

```Python
from azureml.train.hyperdrive import TruncationSelectionPolicy
early_termination_policy = TruncationSelectionPolicy(evaluation_interval=1, truncation_percentage=20, delay_evaluation=5)
```

In dit voorbeeld wordt het beleid voor vroegtijdige beëindiging toegepast op elke interval dat begint bij de evaluatie-interval 5. Een uitvoering worden op basis van interval 5, beëindigd als de prestaties met interval 5 in de laagste 20% van de prestaties van alle uitvoeringen op interval van 5.

### <a name="no-termination-policy"></a>Er is geen beleid beëindiging
Als u wilt dat alle trainingsuitvoeringen NoTerminationPolicy gebruiken om uit te voeren om te voltooien. Dit is het effect van een vroegtijdige beëindiging-beleid niet wordt toegepast. Bijvoorbeeld 

```Python
from azureml.train.hyperdrive import NoTerminationPolicy
early_termination_policy = NoTerminationPolicy()
```

### <a name="default-policy"></a>Standaardbeleid
Als er geen beleid is opgegeven, gebruikt het afstemmen van de service hyperparameter mediaan stoppen beleid met `evaluation_interval` 1 en `delay_evaluation` 5 standaard. Dit zijn conservatieve instellingen, die ongeveer 25% - 35% lager zonder verlies van primaire metrische gegevens (op basis van onze evaluatiegegevens) kunnen bieden.

## <a name="allocate-resources-for-hyperparameter-tuning"></a>Bronnen voor het afstemmen van hyperparameter toewijzen
U kunt uw budget resource beheren voor uw hyperparameter experiment afstemmen door het maximum aantal trainingsuitvoeringen en, optioneel, de maximale duur voor uw hyperparameter afstemmen experiment (in minuten) op te geven. 
* `max_total_runs`: Totaal aantal maximum van trainingsuitvoeringen die worden gemaakt. Dit is een bovengrens: Er is mogelijk minder wordt uitgevoerd, bijvoorbeeld, als de ruimte hyperparameter beperkt is en minder voorbeelden. Moet een getal tussen 1 en 1000.
* `max_duration_minutes`: De maximale duur van de hyperparameter afstemmen experiment in minuten. Dit is een optionele parameter, en indien aanwezig, alle uitvoerbewerkingen die kunnen worden uitgevoerd na deze periode automatisch worden geannuleerd.

>[!NOTE] 
>Als beide `max_total_runs` en `max_duration_minutes` zijn opgegeven, wordt de hyperparameter afstemmen experiment is beëindigd na het verstrijken van de eerste dag van deze twee drempelwaarden.

Bovendien kunt u dat het maximum aantal training wordt uitgevoerd om uit te voeren gelijktijdig tijdens uw hyperparameter afstemmen zoeken.
* `max_concurrent_runs`: Dit is het maximum aantal uitvoeringen om uit te voeren gelijktijdig op elk gewenst moment. Als er niets opgegeven, alle `max_total_runs` parallel wordt gestart. Als u opgeeft, moet een getal tussen 1 en 100 liggen.

>[!NOTE] 
>Het aantal gelijktijdige uitvoeringen worden geregeld op de beschikbare resources in de opgegeven compute-doel. Daarom moet u ervoor te zorgen dat de compute-doel de beschikbare resources voor de gewenste gelijktijdigheid.

U kunt resources toewijzen voor het afstemmen van hyperparameter zoals wordt weergegeven in dit voorbeeld:
```Python
max_total_runs=20,
max_concurrent_runs=4
```
Hiermee configureert u de hyperparameter afstemmen experiment voor het gebruik van maximaal 20 totaal aantal uitvoeringen, 4 configuraties tegelijk uitvoeren.

## <a name="configure-your-hyperparameter-tuning-experiment"></a>Uw experiment afstemmen hyperparameter configureren
U kunt uw experiment met behulp van de gedefinieerde hyperpameter zoeken ruimte, het beleid voor vroegtijdige beëindiging, de primaire metrische gegevens en de toewijzing van resources uit de bovenstaande secties afstemmen hyperparameter configureren. Daarnaast moet u bieden een `estimator` die wordt aangeroepen met de sample hyperparameters. De `estimator` beschrijft de trainingsscript die u uitvoert, de resources per taak (één of meerdere gpu) en de compute-doel te gebruiken. Sinds gelijktijdigheid voor uw experiment afstemmen hyperparameter wordt geregeld op de resources die beschikbaar is, moet u ervoor te zorgen dat uw compute-doel moet worden opgegeven in de `estimator` voldoende bronnen voor gewenste gelijktijdigheid heeft. (Zie [koppeling](/how-to-train-ml-models.md) voor meer informatie over loopt).

Hier volgt een voorbeeld van hoe u uw hyperparameter afstemmen experiment - kunt configureren

```Python
from azureml.train.hyperdrive import HyperDriveRunConfig
hyperdrive_run_config = HyperDriveRunConfig(estimator=estimator,
                          hyperparameter_sampling=param_sampling, 
                          policy=early_termination_policy,
                          primary_metric_name="accuracy", 
                          primary_metric_goal=PrimaryMetricGoal.MAXIMIZE,
                          max_total_runs=100,
                          max_concurrent_runs=4)
```

## <a name="submit-your-hyperparameter-tuning-experiment"></a>Uw hyperparameter afstemmen experiment verzenden
Als u uw configuratie afstemmen hyperparameter hebt gedefinieerd, kunt u een experiment verzenden met behulp van deze configuratie:

```Python
from azureml.core.experiment import Experiment
experiment = Experiment(workspace, experiment_name)
hyperdrive_run = experiment.submit(hyperdrive_run_config)
```

waar `experiment_name` is de naam die u wilt toewijzen aan uw hyperparameter experiment, afstemmen en `workspace` is de werkruimte waarin u wilt maken van het experiment (Zie [koppeling](/concept-azure-machine-learning-architecture.md) voor meer informatie over experimenten).

## <a name="visualize-your-hyperparameter-tuning-experiment"></a>Visualiseer uw hyperparameter experiment afstemmen
Azure Machine Learning-SDK biedt een laptop-widget die kan worden gebruikt voor het visualiseren van de voortgang van uw uitvoerbewerkingen training. Het volgende codefragment kan worden gebruikt voor het visualiseren van alle uw hyperparameter afstemmen wordt uitgevoerd op één plek-

```Python
from azureml.train.widgets import RunDetails
RunDetails(hyperdrive_run).show()
```

Hiermee wordt een tabel met details over de trainingsuitvoeringen weergegeven voor elk van de hyperparameter-configuraties. Bijvoorbeeld

![hyperparameter afstemmen tabel](media/how-to-tune-hyperparameters/HyperparameterTuningTable.png)

U kunt ook de prestaties van elk van de wordt uitgevoerd als de voortgang van de training visualiseren. Bijvoorbeeld

![hyperparameter afstemmen tekengebied](media/how-to-tune-hyperparameters/HyperparameterTuningPlot.png)

Ten slotte kunt u de correlatie tussen prestaties en de waarden van afzonderlijke hyperparameters met behulp van een parallelle coördineert tekenen visueel identificeren. Bijvoorbeeld 

![hyperparameter parallelle coördinaten afstemmen](media/how-to-tune-hyperparameters/HyperparameterTuningParallelCoordinates.png)

U kunt ook alle uw hyperparameter wordt uitgevoerd in de Azure-web-portal afstemmen visualiseren. Zie [koppeling](/how-to-track-experiments.md/#view-the-experiment-in-the-web-portal) voor meer informatie over het weergeven van een experiment in de webportal. Bijvoorbeeld:

![hyperparameter afstemmen portal](media/how-to-tune-hyperparameters/HyperparameterTuningPortal.png)

## <a name="find-the-configuration-that-resulted-in-the-best-performance"></a>De configuratie dat heeft geresulteerd in de beste prestaties niet vinden
Nadat alle van de uitvoeringen afstemmen hyperparameter hebt voltooid, kunt u identificeren de best presterende configuratie en de bijbehorende hyperparameter waarden met behulp van het volgende codefragment:

```Python
best_run = hyperdrive_run.get_best_run_by_primary_metric()
best_run_metrics = best_run.get_metrics()
parameter_values = best_run.get_details()['runDefinition']['Arguments']

print('Best Run Id: ', best_run.id)
print('\n Accuracy:', best_run_metrics['accuracy'])
print('\n learning rate:',parameter_values[3])
print('\n keep probability:',parameter_values[5])
print('\n batch size:',parameter_values[7])
```

## <a name="sample-notebook"></a>Voorbeeld-notebook
Raadpleeg 
* `training/03.train-hyperparameter-tune-deploy-with-tensorflow/03.train-hyperparameter-tune-deploy-with-tensorflow.ipynb` voor een zelfstudie over het afstemmen van hyperparameters voor een Tensorflow-model. 

Dit notitieblok ophalen:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Volgende stappen
* [Bijhouden van een experiment](/how-to-track-experiments.md)
* [Een getraind model implementeren](/how-to-deploy-and-where.md)
