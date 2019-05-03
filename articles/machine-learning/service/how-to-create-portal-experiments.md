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
ms.openlocfilehash: 8b6d7f791300a970e71fda4f1d56354a45d07afd
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029893"
---
# <a name="create-and-explore-automated-machine-learning-experiments-in-the-azure-portal-preview"></a>Maken en het verkennen van geautomatiseerde experimenten voor machine learning in Azure portal (Preview)

 In dit artikel leert u hoe u te maken, uitvoeren en geautomatiseerde experimenten voor machine learning in Azure portal zonder één regel code te verkennen. Automatische leerprocessen automatiseert het proces van het selecteren van het beste algoritme gebruiken voor uw specifieke gegevens, zodat u snel een machine learning-model kunt genereren. [Meer informatie over automatische leerprocessen](https://docs.microsoft.com/azure/machine-learning/service/concept-automated-ml).

 Als u liever een meer op basis van code-ervaring, kunt u ook [configureren van uw geautomatiseerde machine learning-experimenten in Python](how-to-configure-auto-train.md) met de [Azure Machine Learning-SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](https://aka.ms/AMLFree).

* Een werkruimte van Azure Machine Learning-service. Zie [maken van een werkruimte van Azure Machine Learning-service](https://docs.microsoft.com/azure/machine-learning/service/setup-create-workspace).

## <a name="get-started"></a>Aan de slag

Ga naar het linkerdeelvenster van uw werkruimte. Selecteer geautomatiseerde Machine Learning in de sectie Authoring (Preview).

![Deelvenster navigatie in de Azure portal](media/how-to-create-portal-experiments/nav-pane.png)

 Als dit de eerste keer is alle experimenten met Machine Learning automatisch doen, kunt u ziet het volgende:

![Landingspagina van Azure portal experiment](media/how-to-create-portal-experiments/landing-page.png)

Anders is, ziet u uw automatische machine learning-dashboard met een overzicht van al uw geautomatiseerde machine learning-experimenten en wordt uitgevoerd, met inbegrip van die worden uitgevoerd met behulp van de SDK. Hier kunt u filteren en verkennen van uw uitvoerbewerkingen per datum, naam experimenteren en status worden uitgevoerd.

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
    Compute-naam| Voer een unieke naam ter identificatie van uw compute-context.
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
    1. Prognose horizon selecteren: Aangeven hoeveel eenheden (minuten/uren/dagen/weken/maanden/jaar), het model toegankelijk voor de toekomst te voorspellen. Het model moet verder in de toekomst te voorspellen hoe minder nauwkeurig verandert. [Meer informatie over prognoses en prognose horizon](https://docs.microsoft.com/azure/machine-learning/service/how-to-auto-train-forecast#configure-experiment).

1. (Optioneel) Geavanceerde instellingen: aanvullende instellingen die u gebruiken kunt voor het beheren van de trainingstaak beter.

    Geavanceerde instellingen|Description
    ------|------
    Primaire metrische gegevens| Belangrijkste metrische gegevens die worden gebruikt voor het scoren van uw model. [Meer informatie over metrische gegevens model](https://docs.microsoft.com/azure/machine-learning/service/how-to-configure-auto-train#explore-model-metrics).
    Afsluitcriteria| Wanneer een van deze criteria wordt voldaan, wordt de trainingstaak beëindigd voordat de volledige voltooid. <br> *Tijd (minuten) voor de taak training*: Hoe lang om toe te staan de trainingstaak om uit te voeren.  <br> *Maximumaantal iteraties*: Maximum aantal pijplijnen (herhalingen) in de trainingstaak te testen. De taak wordt niet meer dan het opgegeven aantal iteraties uitgevoerd. <br> *Metrische gegevens score drempelwaarde*:  Minimale metrische score voor alle pijplijnen. Dit zorgt ervoor dat als u een gedefinieerd doel metrische gegevens die u wilt bereiken hebt, u doen niet meer tijd besteden aan op de trainingstaak dan nodig.
    Voorverwerking| Selecteer de voorverwerking van gedaan door geautomatiseerde machine learning uit te schakelen. Voorverwerking omvat automatisch gegevens opschonen, voorbereiden en transformatie voor het genereren van synthetische functies. [Meer informatie over voorverwerking](#preprocess).
    Validatie| Selecteer een van de validatieopties voor cross-om in de trainingstaak te gebruiken. [Meer informatie over cross-validatie](https://docs.microsoft.com/azure/machine-learning/service/how-to-configure-auto-train#cross-validation-split-options).
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

Bovendien kunt u deze gegevens om te bepalen of u wilt opnemen of uitsluiten van bepaalde kolommen. U kunt het bereik in welke kolommen worden gebruikt in uw geautomatiseerde machine learning-experiment beheren door het omschakelen van de selector voor elke kolom.

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

## <a name="run-experiment"></a>Experiment uit te voeren

Als u wilt het experiment uitvoeren, klikt u op de knop Start.
  
Het experiment voorbereiden proces duurt enkele minuten.

## <a name="view-results"></a>Resultaten weergeven

Zodra de voorbereidingsfase experiment is voltooid, ziet u het scherm met details van uitvoering. Hiermee geeft u een volledige lijst van de modellen die zijn gemaakt. Standaard, het model dat de hoogste beoordeelt op basis van de parameters worden aan de bovenkant van de lijst. Als de trainingstaak om meer modellen probeert, ziet u deze toegevoegd aan de lijst en de grafiek.
Gebruik de grafiek om een snelle vergelijking van de metrische gegevens voor de modellen die tot nu toe zijn geproduceerd.

![Dashboard van de details van uitvoering](media/how-to-create-portal-experiments/run-details.png)

U zijn kunt inzoomen op een van de uitvoer-modellen die wordt geopend de details van dit model met inbegrip van prestaties en distributie grafieken en metrische gegevens. [Meer informatie over grafieken](https://docs.microsoft.com/azure/machine-learning/service/how-to-track-experiments#understanding-automated-ml-charts).

![Details van de herhaling](media/how-to-create-portal-experiments/dashboard.png)

Taken training kan even voor elke pijplijn die wordt uitgevoerd.

## <a name="deploy-model"></a>Model implementeren

Nadat u het beste model bij de hand hebt, is het tijd om het te implementeren als een webservice om te voorspellen op nieuwe gegevens.

Geautomatiseerde ML helpt u bij het implementeren van het model zonder code te schrijven:

1. Selecteer "Model registreren" in het deelvenster Samenvatting uitvoeren aan de rechterkant.

    ![Model registreren](media/how-to-create-portal-experiments/register-model.png)

1. Zodra het model is geregistreerd, kunt u zult kunnen downloaden van het scoring-script moet worden gebruikt tijdens de implementatie.

    ![Scoring-script downloaden](media/how-to-create-portal-experiments/download-scoring-script.png)

1. Zodra u het scoring-script hebt, gaat u naar de pagina 'Modellen' (in het linker navigatiedeelvenster onder **activa**).

    ![Navigatiedeelvenster model](media/how-to-create-portal-experiments/nav-pane-models.png)

1. Schakel het selectievakje in naast het model dat u hebt geregistreerd, en selecteer 'Installatiekopie maken'.

    U kunt het model identificeren door de omschrijving, waaronder de run-ID en iteratie getal, in de volgende indeling: **< Run_ID > _ < Iteration_number > _Model**.

1. Voer een naam voor de installatiekopie en upload het scoring-bestand dat u eerder hebt gedownload. [Meer informatie over de scoring-scripts](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where.md#script).

    U kunt uw eigen scoring-script en Conda-bestand. Als u een bestand Conda geen [Maak uw eigen](tutorial-deploy-models-with-aml.md#create-environment-file) en upload dit samen met eventuele aanvullende bestanden van dat u wilt gebruiken.

    ![De vorm van een installatiekopie maken](media/how-to-create-portal-experiments/create-image.png)

1. Selecteer de knop 'Maken' om te beginnen met het maken van de installatiekopie. Dit duurt enkele minuten om te voltooien, één keer uitgevoerd, ziet u een bericht op de bovenste balk.

1. Ga naar het tabblad 'Installatiekopieën', schakel het selectievakje in naast de installatiekopie die u wilt implementeren en selecteer 'Implementatie maken'.

    ![Afbeelding implementatiescherm maken](media/how-to-create-portal-experiments/images-create-deployment.png)

1. Voer een unieke implementatie-naam.

1. (optioneel) Voer een beschrijving voor de implementatie.

1. Selecteer het type van de compute doel te gebruiken. 

    ![Implementatie-formulier maken](media/how-to-create-portal-experiments/create-deployment.png)

1. Selecteer 'Maken' voor het starten van het implementatieproces, het duurt enkele minuten.

1. Dat is alles. U hebt een operationele webservice voor het genereren van voorspellingen.

## <a name="next-steps"></a>Volgende stappen

* [Het gebruik van een geïmplementeerd model](how-to-consume-web-service.md).
* [Verzamelen van gegevens voor modellen in productie](how-to-enable-data-collection.md).