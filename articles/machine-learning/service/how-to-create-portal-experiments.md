---
title: Experimenten maken en verkennen in de portal
titleSuffix: Azure Machine Learning service
description: Meer informatie over het maken en beheren van automatische machine learning experimenten in de portal
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: cgronlun
author: tsikiksr
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 05/02/2019
ms.openlocfilehash: 1bfc415b2e4dbc66e2afeae73b78079fb027a60c
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68358841"
---
# <a name="create-and-explore-automated-machine-learning-experiments-in-the-azure-portal-preview"></a>Automatische machine learning experimenten maken en verkennen in de Azure Portal (preview-versie)

 In dit artikel leert u hoe u geautomatiseerde machine learning experimenten kunt maken, uitvoeren en verkennen in de Azure Portal zonder één regel code. Automatische machine learning automatiseert het proces van het selecteren van het beste algoritme dat voor uw specifieke gegevens moet worden gebruikt, zodat u snel een machine learning model kunt genereren. Meer [informatie over automatische machine learning](concept-automated-ml.md).

 Als u de voor keur geeft aan een meer op code gebaseerde ervaring, kunt u ook [uw geautomatiseerde machine learning experimenten in python configureren](how-to-configure-auto-train.md) met de [Azure machine learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](https://aka.ms/AMLFree).

* Een werkruimte van Azure Machine Learning-service. Zie [een Azure machine learning service-werk ruimte maken](https://docs.microsoft.com/azure/machine-learning/service/setup-create-workspace).

## <a name="get-started"></a>Aan de slag

Navigeer naar het linkerdeel venster van uw werk ruimte. Selecteer Automatische Machine Learning in het gedeelte ontwerpen (preview).

![Navigatie deel venster Azure Portal](media/how-to-create-portal-experiments/nav-pane.png)

 Als dit de eerste keer is dat u experimenteert met automatische Machine Learning, ziet u het volgende:

![Landings pagina van Azure Portal experiment](media/how-to-create-portal-experiments/landing-page.png)

Als dat niet het geval is, ziet u uw geautomatiseerde machine learning dash board met een overzicht van al uw geautomatiseerde machine learning experimenten, met inbegrip van de proeven die zijn gemaakt met de SDK. Hier kunt u uw uitvoeringen filteren en verkennen op datum, naam van het experiment en uitvoerings status.

![Dash board Azure Portal-experiment](media/how-to-create-portal-experiments/dashboard.png)

## <a name="create-an-experiment"></a>Een experiment maken

Selecteer de knop experiment maken om het volgende formulier in te vullen.

![Formulier experiment maken](media/how-to-create-portal-experiments/create-exp-name-compute.png)

1. Voer de naam van uw experiment in.

1. Selecteer een compute voor de taak voor gegevens profilering en training. In de vervolg keuzelijst vindt u een lijst met uw bestaande berekeningen. Volg de instructies in stap 3 om een nieuwe Compute te maken.

1. Selecteer de knop een nieuwe berekening maken om het onderstaande deel venster te openen en configureer uw berekenings context voor dit experiment.

    ![Nieuwe reken kracht maken voor experiment](media/how-to-create-portal-experiments/create-new-compute.png)

    Veld|Description
    ---|---
    Compute-naam| Voer een unieke naam in die uw berekenings context identificeert.
    Grootte van de virtuele machine| Selecteer de grootte van de virtuele machine voor de reken kracht.
    Aanvullende instellingen| *Min. knoop punt*: Voer het minimum aantal knoop punten in voor de reken kracht. Het minimum aantal knoop punten voor de AML-Compute is 0. Als u gegevens profilering wilt inschakelen, moet u één of meer knoop punten hebben. <br> *Maximum knoop punt*: Voer het maximum aantal knoop punten in voor de reken kracht. De standaard waarde is 6 knoop punten voor een AML-berekening.

      Selecteer **maken**om het maken van de nieuwe Compute te starten. Dit kan even duren.

      >[!NOTE]
      > De naam van de berekening geeft aan of de compute die u selecteert/maakt, *profile ring is ingeschakeld*. (Zie 7b voor meer informatie over gegevens profilering).

1. Selecteer een opslag account voor uw gegevens. Open bare preview biedt alleen ondersteuning voor het uploaden van lokale bestanden en Azure Blob Storage-accounts.

1. Selecteer een opslag container.

1. Selecteer een gegevens bestand uit uw opslag container of upload een bestand van uw lokale computer naar de container.

    ![Gegevens bestand voor experiment selecteren](media/how-to-create-portal-experiments/select-file.png)

1. Gebruik de tabbladen preview en profile om uw gegevens voor dit experiment verder te configureren.

    1. Geef op het tabblad voor beeld aan of uw gegevens kopteksten bevatten en selecteer de functies (kolommen) voor training met behulp van de **opgenomen** switch knoppen in elke functie kolom.

        ![Voor beeld van gegevens](media/how-to-create-portal-experiments/data-preview.png)

    1. Op het tabblad Profiel kunt u het [gegevens profiel](#profile) weer geven op functie, evenals de verdeling, het type en de samenvattings statistieken (gemiddelde, mediaan, maximum/min, enzovoort) van elk.

        ![Tabblad Gegevens Profiel](media/how-to-create-portal-experiments/data-profile.png)

        >[!NOTE]
        > Het volgende fout bericht wordt weer gegeven als voor uw Compute-context **geen** profile ring is ingeschakeld: *Gegevens profilering is alleen beschikbaar voor Compute-doelen die al worden uitgevoerd*.

1. Selecteer het type trainings taak: classificatie, regressie of prognose.

1. Selecteer de doel kolom. De kolom waarop u de voor spellingen wilt uitvoeren.

1. Voor prognoses:
    1. Selecteer een tijd kolom: Deze kolom bevat de tijd gegevens die moeten worden gebruikt.
    1. Prognose horizon selecteren: Geef aan hoeveel tijds eenheden (minuten/uren/dagen/weken/maanden/jaren) het model op de toekomst kan voors pellen. Verder is het model vereist om in de toekomst te voors pellen, hoe minder nauw keurig wordt. Meer [informatie over prognoses en prognoses horizon](how-to-auto-train-forecast.md).

1. Beschrijving Geavanceerde instellingen: extra instellingen die u kunt gebruiken om de trainings taak beter te beheren.

    Geavanceerde instellingen|Description
    ------|------
    Primaire metriek| De belangrijkste waarde die wordt gebruikt voor het scoren van uw model. Meer [informatie over de metrische gegevens van modellen](how-to-configure-auto-train.md#explore-model-metrics).
    Afsluit criteria| Wanneer aan een van deze criteria wordt voldaan, wordt de trainings taak beëindigd voordat deze volledig is voltooid. <br> *Tijd trainings taak (minuten)* : Hoe lang het duurt om de trainings taak uit te voeren.  <br> *Maximum aantal herhalingen*: Maximum aantal pijp lijnen (iteraties) om in de trainings taak te testen. De taak wordt niet meer uitgevoerd dan het opgegeven aantal iteraties. <br> *Drempel waarde voor metrische Score*:  Minimale metrische score voor alle pijp lijnen. Dit zorgt ervoor dat als u een gedefinieerde doel metriek hebt die u wilt bereiken, u niet meer tijd op de trainings taak brengt dan nodig is.
    Verwerking| Selecteer deze optie om de voor verwerking van automatische machine learning in of uit te scha kelen. Preverwerking omvat het automatisch opschonen van gegevens, voorbereiden en transformeren voor het genereren van synthetische functies. [Meer informatie over voor verwerking](#preprocess).
    Validatie| Selecteer een van de opties voor kruis validatie die u wilt gebruiken in de trainings taak. Meer [informatie over Kruis validatie](how-to-configure-auto-train.md).
    Gelijktijdigheid| Selecteer de limieten voor meerdere kernen die u wilt gebruiken bij het gebruik van multi-core-berekening.
    Geblokkeerd algoritme| Selecteer de algoritmen die u wilt uitsluiten van de trainings taak.

   ![Formulier Geavanceerde instellingen](media/how-to-create-portal-experiments/advanced-settings.png)

> [!NOTE]
> Voor meer informatie over velden, klikt u op de informatie knop info.

<a name="profile"></a>

### <a name="data-profiling"></a>Gegevens profilering

U kunt een groot aantal samenvattings statistieken in uw gegevensset ontvangen om te controleren of uw gegevensset van ML is. Voor niet-numerieke kolommen bevatten ze alleen basis statistieken zoals min, Max en aantal fouten. Voor numerieke kolommen kunt u ook hun statistische momenten en geschatte quantiles bekijken. Het gegevens profiel bevat met name:

* **Functie**: de naam van de kolom die wordt samenvatten.

* **Profiel**: een in-line visualisatie op basis van het type dat is afgeleid. Teken reeksen, Booleaanse waarden en datums hebben bijvoorbeeld een aantal aantallen, terwijl decimalen (cijfers) geschatte histogrammen hebben. Zo kunt u een goed inzicht krijgen in de distributie van de gegevens.

* **Type distributie**: een in-line waarde van het aantal typen in een kolom. Nullen zijn hun eigen type, waardoor deze visualisatie nuttig is voor het detecteren van oneven of ontbrekende waarden.

* **Type**: het type dat is afgeleid van de kolom. Mogelijke waarden zijn: teken reeksen, booleans, datums en decimalen.

* **Min**: de minimum waarde van de kolom. Er worden geen lege items weer gegeven voor functies waarvan het type geen inherente ordening (bijvoorbeeld Boole-waarden) heeft.

* **Max**: de maximum waarde van de kolom. Net als ' min. ' worden geen vermeldingen weer gegeven voor functies met irrelevante typen.

* **Aantal**: het totale aantal ontbrekende en niet-ontbrekende vermeldingen in de kolom.

* **Aantal niet-ontbrekende**items: het aantal vermeldingen in de kolom die niet ontbreken. Houd er rekening mee dat lege teken reeksen en fouten worden beschouwd als waarden, zodat ze niet bijdragen aan het aantal niet-ontbrekende items.

* **Quantiles** (bij 0,1, 1, 5, 25, 50, 75, 95, 99 en 99,9% intervallen): de geschatte waarden bij elke quantile om een idee te geven van de distributie van de gegevens. Lege vermeldingen worden weer gegeven voor functies met irrelevante typen.

* **Gemiddelde**: het reken kundige gemiddelde van de kolom. Lege vermeldingen worden weer gegeven voor functies met irrelevante typen.

* **Standaard afwijking**: de standaard deviatie van de kolom. Lege vermeldingen worden weer gegeven voor functies met irrelevante typen.

* **Afwijking**: de variantie van de kolom. Lege vermeldingen worden weer gegeven voor functies met irrelevante typen.

* **Scheefheid**: de scheefheid van de kolom. Lege vermeldingen worden weer gegeven voor functies met irrelevante typen.

* **Kurtosis**: de kurtosis van de kolom. Lege vermeldingen worden weer gegeven voor functies met irrelevante typen.

<a name="preprocess"></a>

### <a name="advanced-preprocessing"></a>Geavanceerde preverwerking

Wanneer u uw experimenten configureert, kunt u de geavanceerde instelling `Preprocess`inschakelen. Dit betekent dat de volgende stappen voor preprocesing en parametrisatie automatisch worden uitgevoerd.

|&nbsp;Stappen voor voor verwerking| Description |
| ------------- | ------------- |
|Verwijder hoge kardinaliteit of er zijn geen functies afwijking|Verwijder deze uit de trainings-en validatie sets, inclusief functies waarbij alle waarden ontbreken, dezelfde waarde over alle rijen of met extreem hoge kardinaliteit (bijvoorbeeld hashes, Id's of GUID'S).|
|Ontbrekende waarden worden toegerekend|Voor numerieke functies toegerekend met het gemiddelde van de waarden in de kolom.<br/><br/>Voor categorische-functies toegerekend met de meest frequente waarde.|
|Extra functies genereren|Voor DateTime-functies: Jaar, maand, dag, dag van de week, dag van jaar, kwar taal, week van het jaar, uur, minuut, seconde.<br/><br/>Voor tekst functies: De term frequentie op basis van unigrams, bi-gram en Tri-Character-gram.|
|Transformeren en coderen |Numerieke functies met weinig unieke waarden worden omgezet in categorische-functies.<br/><br/>Er wordt één Hot encoding uitgevoerd voor lage kardinaliteit categorische; voor hoge kardinaliteit, een-hot-hash-code ring.|
|Woord insluitingen|Tekst featurizer waarmee vectoren van tekst tokens worden geconverteerd naar sentence vectoren met behulp van een vooraf getraind model. De insluitings vector van elk woord in een document wordt samen geaggregeerd om een document functie Vector te maken.|
|Doel codering|Voor categorische-functies wijst elke categorie toe met de gemiddelde doel waarde voor regressie problemen en op basis van de klasse-kans voor elke klasse voor classificatie problemen. De Kruis validatie op basis van frequentie en k-vouwen wordt toegepast om de toewijzing en het lawaai dat door sparse gegevens categorieën wordt veroorzaakt, te verminderen.|
|Coderen van tekst doel|Voor tekst invoer wordt een gestapeld lineair model met Bag-of-woorden gebruikt om de kans van elke klasse te genereren.|
|Gewicht van bewijs (WoE)|Hiermee wordt WoE berekend als een maat eenheid van de correlatie tussen categorische-kolommen en de doel kolom. Het wordt berekend als het logboek van de verhouding van de in-class versus van de cursus kansen. Met deze stap wordt één numerieke functie kolom per klasse uitgevoerd en wordt de nood zaak om ontbrekende waarden en uitschieter-behandeling expliciet te toegerekend.|
|Cluster afstand|Treinen a k: cluster model op alle numerieke kolommen.  Hiermee worden k nieuwe functies, een nieuwe numerieke functie per cluster, met de afstand van elk voor beeld naar de massa middelpunt van elk cluster uitgevoerd.|

## <a name="run-experiment-and-view-results"></a>Experiment uitvoeren en resultaten weer geven

Klik op Start om het experiment uit te voeren. Het proces voor het voorbereiden van het experiment duurt enkele minuten.

### <a name="view-experiment-details"></a>Details van experiment weer geven

Nadat de voor bereiding van het experiment is voltooid, ziet u het scherm Details uitvoeren. Dit geeft u een volledige lijst van de gemaakte modellen. Standaard beoordeelt het model met het hoogste niveau op basis van uw para meters boven aan de lijst. Wanneer de trainings taak meer modellen probeert, worden deze toegevoegd aan de lijst met herhalingen en de grafiek. Gebruik het iteratie diagram om een snelle vergelijking te krijgen van de metrische gegevens voor de modellen die tot nu toe zijn geproduceerd.

Het kan even duren voordat trainings taken worden uitgevoerd om de uitvoering van elke pijp lijn te volt ooien.

![Dash board Details uitvoeren](media/how-to-create-portal-experiments/run-details.png)

### <a name="view-training-run-details"></a>Details van de cursus uitvoering weer geven

Inzoomen op de uitvoer modellen om details van de trainings uitvoering te bekijken, zoals prestatie gegevens en distributie grafieken. [Meer informatie over grafieken](how-to-understand-automated-ml.md).

![Details van herhaling](media/how-to-create-portal-experiments/iteration-details.png)

## <a name="deploy-model"></a>Model implementeren

Zodra u het beste model bij de hand hebt, is het tijd om het te implementeren als een webservice om nieuwe gegevens te voors pellen.

Automatische ML helpt u bij het implementeren van het model zonder code te schrijven:

1. U hebt een aantal opties voor implementatie. 
    1. Als u het beste model wilt implementeren op basis van de criteria voor metrische gegevens die u voor het experiment hebt ingesteld, selecteert u **beste model implementeren** op de pagina **Details uitvoeren** .

        ![Knop model implementeren](media/how-to-create-portal-experiments/deploy-model-button.png)

    1. Als u een specifieke model herhaling wilt implementeren, zoomt u in op het model om de specifieke uitvoerings detail pagina te openen en selecteert u **model implementeren**.

        ![Knop model implementeren](media/how-to-create-portal-experiments/deploy-model-button2.png)

1. De eerste stap is het registreren van het model in de service. Selecteer model registreren en wacht totdat het registratie proces is voltooid.

    ![Blade model implementeren](media/how-to-create-portal-experiments/deploy-model-blade.png)

1. Zodra het model is geregistreerd, kunt u het Score script (scoring.py) en het omgevings script (condaEnv. yml) downloaden dat moet worden gebruikt tijdens de implementatie.

1. Wanneer het Score script en het omgevings script worden gedownload, gaat u naar de Blade **assets** van het navigatie deel venster links en selecteert u **modellen**.

    ![Modellen van het navigatie deel venster](media/how-to-create-portal-experiments/nav-pane-models.png)

1. Selecteer het model dat u hebt geregistreerd en selecteer installatie kopie maken.

    U kunt het model identificeren aan de hand van de beschrijving, zoals de run-ID, het herhalings nummer, in de volgende indeling: *< Run_ID > _ < Iteration_number > _Model*

    ![Modellen: Installatiekopie maken](media/how-to-create-portal-experiments/model-create-image.png)

1. Voer een naam in voor de installatie kopie. 
1. Selecteer de knop **Bladeren** naast het vak Score bestand om het Score bestand (scoring.py) te uploaden dat u eerder hebt gedownload.

1. Selecteer de knop **Bladeren** naast het vak Conda-bestand om het omgevings bestand (condaEnv. yml) te uploaden dat u eerder hebt gedownload.

    U kunt uw eigen score script-en Conda-bestand gebruiken, maar ook extra bestanden uploaden. Meer [informatie over Score script](how-to-deploy-and-where.md#script).

      >[!Important]
      > Bestands namen moeten minder dan 32 tekens lang zijn en moeten beginnen en eindigen met een alfanumerieke teken reeks. Kan streepjes, onderstrepings tekens, punten en alfanumerieken bevatten. Spaties zijn niet toegestaan.

    ![Installatiekopie maken](media/how-to-create-portal-experiments/create-image.png)

1. Selecteer de knop maken om de installatie kopie te maken. Dit kan een paar minuten duren. Zodra u dit hebt gedaan, wordt er een bericht weer gegeven op de bovenste balk.
1. Ga naar het tabblad ' afbeeldingen ', schakel het selectie vakje in naast de installatie kopie die u wilt implementeren en selecteer implementatie maken. Meer [informatie over implementaties](how-to-deploy-and-where.md).

    Er zijn twee opties voor de implementatie.
     + Azure container instance (ACI): dit wordt gebruikt voor test doeleinden in plaats van operationele implementatie op schaal. Vul de waarden in voor ten minste één kern voor _CPU-reserve capaciteit_en ten minste één gigabyte (GB) voor _gereserveerde geheugen capaciteit_
     + Azure Kubernetes service (AKS)): deze optie is voor implementatie op schaal. U moet een AKS-based Compute Ready hebben.

     ![Installatie kopieën Implementatie maken](media/how-to-create-portal-experiments/images-create-deployment.png)

1. Wanneer u klaar bent, selecteert u **Maken**. Het implementeren van het model kan enkele minuten duren voordat de uitvoering van elke pijp lijn is voltooid.

1. Dat is alles. U hebt een Operational web service voor het genereren van voor spellingen.

## <a name="next-steps"></a>Volgende stappen

* Meer [informatie over automatische machine learning](concept-automated-ml.md) en Azure machine learning.
* Krijg [inzicht in geautomatiseerde machine learning resultaten](how-to-understand-automated-ml.md).
* [Meer informatie over het gebruik van een webservice](https://docs.microsoft.com/azure/machine-learning/service/how-to-consume-web-service).
