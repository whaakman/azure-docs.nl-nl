---
title: Maken en te verkennen, experimenten in de Portal
titleSuffix: Azure Machine Learning service
description: Meer informatie over het maken en beheren van geautomatiseerde machine learning-experimenten in de portal
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: tsikiksr
author: tsikiksr
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 05/02/2019
ms.openlocfilehash: 714283628e1b2ac445d36d0b07fe299b589a1cf0
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2019
ms.locfileid: "67312806"
---
# <a name="create-and-explore-automated-machine-learning-experiments-in-the-azure-portal-preview"></a>Maken en het verkennen van geautomatiseerde experimenten voor machine learning in Azure portal (Preview)

 In dit artikel leert u hoe u te maken, uitvoeren en geautomatiseerde experimenten voor machine learning in Azure portal zonder één regel code te verkennen. Automatische leerprocessen automatiseert het proces van het selecteren van het beste algoritme gebruiken voor uw specifieke gegevens, zodat u snel een machine learning-model kunt genereren. [Meer informatie over automatische leerprocessen](concept-automated-ml.md).

 Als u liever een meer op code gebaseerde ervaring, kunt u ook [configureren van uw geautomatiseerde machine learning-experimenten in Python](how-to-configure-auto-train.md) met de [Azure Machine Learning-SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](https://aka.ms/AMLFree).

* Een werkruimte van Azure Machine Learning-service. Zie [maken van een werkruimte van Azure Machine Learning-service](https://docs.microsoft.com/azure/machine-learning/service/setup-create-workspace).

## <a name="get-started"></a>Aan de slag

Ga naar het linkerdeelvenster van uw werkruimte. Selecteer geautomatiseerde Machine Learning in de sectie Authoring (Preview).

![Deelvenster navigatie in de Azure portal](media/how-to-create-portal-experiments/nav-pane.png)

 Als dit de eerste keer is alle experimenten met Machine Learning automatisch doen, kunt u ziet het volgende:

![Landingspagina van Azure portal experiment](media/how-to-create-portal-experiments/landing-page.png)

Anders is, ziet u uw automatische machine learning-dashboard met een overzicht van al uw geautomatiseerde machine learning-experimenten, met inbegrip van die zijn gemaakt met de SDK. Hier kunt u filteren en verkennen van uw uitvoerbewerkingen per datum, naam experimenteren en status worden uitgevoerd.

![Experiment met Azure portal-dashboard](media/how-to-create-portal-experiments/dashboard.png)

## <a name="create-an-experiment"></a>Een experiment maken

Selecteer de knop Experiment maken voor het vullen van de volgende notatie.

![Formulier-experiment maken](media/how-to-create-portal-experiments/create-exp-name-compute.png)

1. Voer de naam van uw experiment.

1. Selecteer een berekenen voor het profileren van gegevens en de trainingstaak. Een lijst met uw bestaande berekeningen is beschikbaar in de vervolgkeuzelijst. Volg de instructies in stap 3 voor het maken van een nieuwe berekening.

1. Selecteer de knop een nieuwe compute openen maken het onder in het deelvenster en configureren uw compute-context voor dit experiment.

    ![Nieuwe berekening voor experiment maken](media/how-to-create-portal-experiments/create-new-compute.png)

    Veld|Description
    ---|---
    COMPUTE-naam| Voer een unieke naam ter identificatie van uw compute-context.
    Grootte virtuele machine| Selecteer de grootte van de virtuele machine voor Computing.
    Aanvullende instellingen| *Min knooppunt*: Voer het minimum aantal knooppunten voor Computing. Het minimum aantal knooppunten voor AML compute is 0. Om in te schakelen data profileren, moet u 1 of meer knooppunten hebben. <br> *Max knooppunt*: Geef het maximum aantal knooppunten voor Computing. De standaardwaarde is 6 knooppunten voor een AML-Computing.

      Selecteer om te beginnen met het maken van uw nieuwe berekening, **maken**. Dit kan even duren.

      >[!NOTE]
      > De naam van uw compute wordt een waarschuwing als de berekening die u selecteert/maken, *profilering ingeschakeld*. (Zie 7b voor meer informatie over gegevens profilering).

1. Selecteer een opslagaccount voor uw gegevens. Openbare preview biedt alleen ondersteuning voor het uploaden van lokale bestanden en Azure Blob Storage-accounts.

1. Selecteer een opslagcontainer.

1. Selecteer een gegevensbestand in uw opslagcontainer of uploaden van een bestand vanaf uw lokale computer naar de container.

    ![Selecteer het gegevensbestand voor experiment](media/how-to-create-portal-experiments/select-file.png)

1. De tabbladen Preview-versie en het profiel gebruiken om uw gegevens voor dit experiment verder te configureren.

    1. Op het tabblad voorbeeld geven als uw gegevens headers bevat en selecteer de functies (kolommen) voor het gebruik van training de **opgenomen** knoppen in elke kolom functie overschakelen.

        ![Voorbeeld van gegevens](media/how-to-create-portal-experiments/data-preview.png)

    1. Op het tabblad profiel vindt u de [gegevensprofiel](#profile) door de functie, evenals de distributie, type en samenvattende statistieken (gemiddelde, gemiddelde, max/min, enzovoort) van elk.

        ![Tabblad Data-profiel](media/how-to-create-portal-experiments/data-profile.png)

        >[!NOTE]
        > Het volgende foutbericht wordt weergegeven als uw compute-context is **niet** profilering ingeschakeld: *Profileren van gegevens is alleen beschikbaar voor de compute-doelen die al worden uitgevoerd*.

1. Selecteer het taaktype training: classificatie, regressie of prognose.

1. Selecteer de doelkolom. De kolom die u de voorspellingen doen wilt op.

1. Voor het voorspellen van:
    1. Selecteer time-kolom: Deze kolom bevat de tijdgegevens moet worden gebruikt.
    1. Prognose horizon selecteren: Aangeven hoeveel eenheden (minuten/uren/dagen/weken/maanden/jaar), het model toegankelijk voor de toekomst te voorspellen. Het model moet verder in de toekomst te voorspellen hoe minder nauwkeurig verandert. [Meer informatie over prognoses en prognose horizon](how-to-auto-train-forecast.md).

1. (Optioneel) Geavanceerde instellingen: aanvullende instellingen die u gebruiken kunt voor het beheren van de trainingstaak beter.

    Geavanceerde instellingen|Description
    ------|------
    Primaire metrische gegevens| Belangrijkste metrische gegevens die worden gebruikt voor het scoren van uw model. [Meer informatie over metrische gegevens model](how-to-configure-auto-train.md#explore-model-metrics).
    Afsluitcriteria| Wanneer een van deze criteria wordt voldaan, wordt de trainingstaak beëindigd voordat de volledige voltooid. <br> *Tijd (minuten) voor de taak training*: Hoe lang om toe te staan de trainingstaak om uit te voeren.  <br> *Maximumaantal iteraties*: Maximum aantal pijplijnen (herhalingen) in de trainingstaak te testen. De taak wordt niet meer dan het opgegeven aantal iteraties uitgevoerd. <br> *Metrische gegevens score drempelwaarde*:  Minimale metrische score voor alle pijplijnen. Dit zorgt ervoor dat als u een gedefinieerd doel metrische gegevens die u wilt bereiken hebt, u doen niet meer tijd besteden aan op de trainingstaak dan nodig.
    Voorverwerking| Selecteer de voorverwerking van gedaan door geautomatiseerde machine learning uit te schakelen. Voorverwerking omvat automatisch gegevens opschonen, voorbereiden en transformatie voor het genereren van synthetische functies. [Meer informatie over voorverwerking](#preprocess).
    Validatie| Selecteer een van de validatieopties voor cross-om in de trainingstaak te gebruiken. [Meer informatie over cross-validatie](how-to-configure-auto-train.md).
    Gelijktijdigheid| Selecteer de multicore-limieten die u gebruiken wilt bij het gebruik van meerdere kernen compute.
    Geblokkeerde algoritme| Selecteer de algoritmen die u wilt uitsluiten van de trainingstaak.

   ![Formulier voor geavanceerde instellingen](media/how-to-create-portal-experiments/advanced-settings.png)

> [!NOTE]
> Klik voor meer informatie over velden de knopinfo die informatie.

<a name="profile"></a>

### <a name="data-profiling"></a>Profileren van gegevens

U kunt een enorme verscheidenheid aan samenvattende statistieken ophalen voor uw verzameling om te controleren of uw gegevensset gereed is voor ML is. Voor niet-numerieke kolommen bevatten ze alleen elementaire statistieken, zoals min, max en aantal fouten. Voor numerieke kolommen kunt u ook de statistische seconden en de geschatte quantiles bekijken. Specifiek, bevat onze gegevensprofiel:

* **Functie**: naam van de kolom die wordt samengevat.

* **Profiel**: een visualisatie in de regel op basis van het type dat is afgeleid. Bijvoorbeeld, heeft tekenreeksen, Booleaanse waarden en datums waarde tellingen, terwijl decimalen (cijfers) hebben histogrammen benaderd. Hiermee kunt u een snelle inzicht in de distributie van de gegevens krijgen.

* **Typ distributie**: een inline-waarde aantal typen in een kolom. Null-waarden zijn hun eigen type, zodat deze visualisatie handig is voor het opsporen van afwijkende of ontbrekende waarden.

* **Type**: het afgeleide type van de kolom. Mogelijke waarden zijn: tekenreeksen, Booleaanse waarden, datums en decimalen.

* **Min**: de minimale waarde van de kolom. Lege gegevens worden weergegeven voor functies waarvan het type beschikt niet over een inherente volgorde (bijvoorbeeld Booleaanse waarden).

* **Maximale**: de maximumwaarde van de kolom. Zoals 'minuut' weergegeven lege gegevens voor functies met irrelevante typen.

* **Aantal**: het totale aantal ontbrekende en niet-ontbrekende gegevens in de kolom.

* **Ontbrekende aantal**: het aantal vermeldingen in de kolom die geen ontbrekende. Houd er rekening mee dat lege tekenreeksen en fouten worden behandeld als waarden, zodat ze niet dragen bij aan de 'niet ontbrekende aantal'.

* **Quantiles** (met tussenpozen van 0.1, 1, 5, 25, 50, 75, 95, 99 en 99,9%): de geschatte waarden voor elke kwantiel voor een beeld van de verdeling van de gegevens. Lege gegevens worden weergegeven voor functies met irrelevante typen.

* **Betekenen**: het rekenkundige gemiddelde van de kolom. Lege gegevens worden weergegeven voor functies met irrelevante typen.

* **Standaarddeviatie**: de standaardafwijking van de kolom. Lege gegevens worden weergegeven voor functies met irrelevante typen.

* **Afwijking**: de afwijking van de kolom. Lege gegevens worden weergegeven voor functies met irrelevante typen.

* **Asymmetrie**: de asymmetrie van de kolom. Lege gegevens worden weergegeven voor functies met irrelevante typen.

* **Kurtosis**: de kurtosis van de kolom. Lege gegevens worden weergegeven voor functies met irrelevante typen.

<a name="preprocess"></a>

### <a name="advanced-preprocessing"></a>Geavanceerde voorverwerking

Bij het configureren van de experimenten nodig hebben, kunt u de geavanceerde instelling `Preprocess`. Dit betekent dat de volgende gegevens voorverwerken en parametrisatie stappen automatisch worden uitgevoerd.

|Voorverwerking&nbsp;stappen| Description |
| ------------- | ------------- |
|Verwijder hoge kardinaliteit of er zijn geen functies afwijking|Verwijderen uit de trainings- en validatie sets, met inbegrip van functies met alle waarden ontbreken, dezelfde waarde in alle tabelrijen of met zeer hoge kardinaliteit (bijvoorbeeld-hashes, -id's of GUID's).|
|Ontbrekende waarden worden toegerekend|Voor numerieke functies, rekenen met gemiddelde van waarden in de kolom.<br/><br/>Rekenen met de meest voorkomende waarde voor de categorische functies.|
|Extra functies genereren|Voor datum/tijd-functies: Jaar, maand, dag, dag van week, dag van jaar, kwartaal, de Week van het jaar, uur, minuut, seconde.<br/><br/>Voor tekst-functies: De frequentie van de termijn is op basis van unigrams, bi-g en drie-teken-gram.|
|Transformeren en coderen |Numerieke functies met weinig unieke waarden worden getransformeerd in categorische functies.<br/><br/>Een hot-codering wordt van lage kardinaliteit categorische; uitgevoerd voor hoge kardinaliteit, 1-' hot '-hash-codering.|
|Woordinsluitingen|Tekst featurizer die vectoren van tekst tokens converteert naar zin vectoren met behulp van een vooraf getrainde model. Insluiten vector van elk woord in een document wordt samen om te produceren van een document functie vector geaggregeerd.|
|Doel coderingen|Voor categorische functies, wijst elke categorie met gemiddelde doelwaarde regressie problemen, en de kans op klasse voor elke objectklasse voor classificatie problemen. Frequentie op basis van gewicht en k-Vouw kruisvalidatie om te beperken via aanpassing van labels van de toewijzings- en ruis veroorzaakt door sparse gegevenscategorieën wordt toegepast.|
|Doel tekstcodering|Voor tekstinvoer, een gestapeld lineair model met de eigenschappenverzameling van woorden gebruikt voor het genereren van de kans op elke klasse.|
|Gewicht van bewijs (wee)|Berekent de wee als een meting van correlatie van categorische kolommen op de doelkolom. Dit wordt berekend als het logboek van de verhouding van Visual Studio in klasse out van de klasse kansen. Deze stap voert een functie voor numerieke kolom per klasse en hoeven zich niet expliciet worden toegerekend ontbrekende waarden en uitschieter behandeling.|
|Afstand van cluster|Traint k-means clustering-model voor alle numerieke kolommen.  Uitvoer k nieuwe functies, een nieuwe numerieke functies per cluster, met de afstand van elk voorbeeld op de massamiddelpunt van elk cluster.|

## <a name="run-experiment-and-view-results"></a>Experiment uit te voeren en resultaten te bekijken

Als u wilt het experiment uitvoeren, klikt u op Start. Het experiment voorbereiden proces duurt enkele minuten.

### <a name="view-experiment-details"></a>Details van experiment

Zodra de voorbereidingsfase experiment is voltooid, ziet u het scherm details worden uitgevoerd. Dit biedt u een volledige lijst van de modellen die zijn gemaakt. Standaard, het model dat de hoogste beoordeelt op basis van de parameters is aan de bovenkant van de lijst. Als de trainingstaak om meer modellen probeert, worden ze toegevoegd aan de lijst met iteratie en grafiek. Gebruik de grafiek herhaling een snelle vergelijking van de metrische gegevens ophalen voor de modellen die tot nu toe zijn geproduceerd.

Taken training kan even voor elke pijplijn die wordt uitgevoerd.

![Dashboard van de details van uitvoering](media/how-to-create-portal-experiments/run-details.png)

### <a name="view-training-run-details"></a>Details uitvoering van weergave-training

Inzoomen op een van de uitvoer-modellen om te zien uitvoeren-gegevens, zoals metrische gegevens en distributie worden prestatiegrafieken training. [Meer informatie over grafieken](how-to-track-experiments.md#understanding-automated-ml-charts).

![Details van de herhaling](media/how-to-create-portal-experiments/iteration-details.png)

## <a name="deploy-model"></a>Model implementeren

Nadat u het beste model bij de hand hebt, is het tijd om het te implementeren als een webservice om te voorspellen op nieuwe gegevens.

Geautomatiseerde ML helpt u bij het implementeren van het model zonder code te schrijven:

1. U hebt een aantal opties voor implementatie. 
    1. Als u wilt implementeren, het beste model op basis van de metrische criteria u instellen voor het experiment, selecteer **beste Model implementeren** uit de **uitvoeren details** pagina.

        ![Knop model implementeren](media/how-to-create-portal-experiments/deploy-model-button.png)

    1. Als u implementeren van een specifiek model iteratie wilt, Inzoomen op het model op de detailpagina voor het specifieke uitvoeren openen en selecteer **Model implementeren**.

        ![Knop model implementeren](media/how-to-create-portal-experiments/deploy-model-button2.png)

1. Eerste stap is het model te registreren bij de service. Selecteer "Model registreren" en wacht totdat het registratieproces te voltooien.

    ![Blade model implementeren](media/how-to-create-portal-experiments/deploy-model-blade.png)

1. Zodra het model is geregistreerd, moet u mogelijk zijn om te downloaden van het scoring-script (scoring.py) en het script met de omgeving (condaEnv.yml) moet worden gebruikt tijdens de implementatie.

1. Als het scoring-script en het script omgeving zijn gedownload, gaat u naar de **activa** blade van het navigatiedeelvenster links in en selecteer **modellen**.

    ![Navigatie deelvenster modellen](media/how-to-create-portal-experiments/nav-pane-models.png)

1. Selecteer het model dat u hebt geregistreerd, en selecteer 'Installatiekopie maken'.

    U kunt het model identificeren door de omschrijving, waaronder de run-ID, het aantal herhalingen, in de volgende indeling: *< Run_ID > _ < Iteration_number > _Model*

    ![Models: Installatiekopie maken](media/how-to-create-portal-experiments/model-create-image.png)

1. Voer een naam voor de installatiekopie. 
1. Selecteer de **Bladeren** knop naast het vak 'File scoren' voor het uploaden van het scoring-bestand (scoring.py) u eerder hebt gedownload.

1. Selecteer de **Bladeren** knop naast het vak 'Conda File' voor het uploaden van het omgevingsbestand (condaEnv.yml) die u eerder hebt gedownload.

    U kunt uw eigen scoring-script en conda-bestand gebruiken, evenals aanvullende bestanden uploaden. [Meer informatie over de scoring-script](how-to-deploy-and-where.md#script).

      >[!Important]
      > Bestandsnamen moeten worden onder 32 tekens bevatten en moet beginnen en eindigen met alfanumerieke tekens. Streepjes, onderstrepingstekens, punten en alfanumerieke tekens tussen kan worden opgenomen. Spaties zijn niet toegestaan.

    ![Installatiekopie maken](media/how-to-create-portal-experiments/create-image.png)

1. Selecteer de knop 'Maken' om te beginnen met het maken van de installatiekopie. Dit duurt enkele minuten om te voltooien, één keer uitgevoerd, ziet u een bericht op de bovenste balk.
1. Ga naar het tabblad 'Installatiekopieën', schakel het selectievakje in naast de installatiekopie die u wilt implementeren en selecteer 'Implementatie maken'. [Meer informatie over implementaties](how-to-deploy-and-where.md).

    Er zijn 2 opties voor implementatie.
     + Azure Container Instance (ACI) - dit meer wordt gebruikt voor het testen van gebruik in plaats van operationele implementatie op grote schaal. Zorg ervoor dat u de waarden voor ten minste één core voor invullen _CPU-reservecapaciteit_, en ten minste één gigabyte (GB) voor _Geheugenreservecapaciteit_
     + Azure Kubernetes Service (AKS)) - deze optie is voor implementatie op grote schaal. U moet een AKS op basis van compute gereed hebben.

     ![Installatiekopieën: Implementatie maken](media/how-to-create-portal-experiments/images-create-deployment.png)

1. Wanneer u klaar bent, selecteert u **Maken**. Implementeren van het model kan enkele minuten duren voor elke pijplijn die wordt uitgevoerd.

1. Dat is alles. U hebt een operationele webservice voor het genereren van voorspellingen.

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over geautomatiseerde machine learning](concept-automated-ml.md) en Azure Machine Learning.
* [Meer informatie over het gebruik van een webservice](https://docs.microsoft.com/azure/machine-learning/service/how-to-consume-web-service).
