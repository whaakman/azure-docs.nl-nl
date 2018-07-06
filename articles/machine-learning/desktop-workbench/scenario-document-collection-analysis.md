---
title: Documenteer verzameling Analysis - Azure | Microsoft Docs
description: Het samenvatten en analyseren van een grote verzameling van documenten, met inbegrip van technieken zoals woordgroep learning onderwerp modelleren en onderwerp model analyse met behulp van Azure ML Workbench.
services: machine-learning
author: kehuan
ms.author: kehuan
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, MicrosoftDocs/mlreview, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: 29f493449d48df26919a98452fa7f832d653d45e
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37861709"
---
# <a name="document-collection-analysis"></a>Documentverzamelingen

In dit scenario ziet u hoe u een overzicht maken en analyseren van een grote verzameling van documenten, met inbegrip van technieken zoals woordgroep learning onderwerp modelleren en onderwerp model analyse met behulp van Azure ML Workbench. Azure Machine Learning Workbench biedt voor eenvoudig schalen voor zeer grote documentverzameling, en biedt mechanismen voor het trainen en modellen in tal van compute-context, variërend van de lokale compute op virtuele Machines voor Datatechnologie met Spark-Cluster af te stemmen. Eenvoudige ontwikkeling wordt geboden via Jupyter-notebooks in Azure Machine Learning Workbench.

## <a name="link-to-the-gallery-github-repository"></a>Koppeling naar de galerie met GitHub-opslagplaats

De openbare GitHub-opslagplaats voor dit scenario praktijk bevat al het materiaal, waaronder voorbeelden van code, die nodig zijn voor dit voorbeeld:

[https://github.com/Azure/MachineLearningSamples-DocumentCollectionAnalysis](https://github.com/Azure/MachineLearningSamples-DocumentCollectionAnalysis)

## <a name="overview"></a>Overzicht

Met een grote hoeveelheid gegevens (met name ongestructureerde tekstgegevens) die elke dag wordt verzameld, is een aanzienlijke uitdaging om te organiseren, zoeken en te begrijpen van grote hoeveelheden teksten. Dit document verzameling analysis scenario ziet u een efficiënte en geautomatiseerde end-to-end-werkstroom voor het analyseren van grote documentverzameling en downstream NLP-taken in te schakelen.

De belangrijkste elementen die worden geleverd door dit scenario zijn:

1. Leren werken met opvallende meerdere woorden woordgroep van documenten.

1. Detectie van onderliggende onderwerpen die zijn gepresenteerd in de documentenverzameling.

1. Documenten die door de lokale distributie.

1. Methoden voor het ordenen, zoeken en samenvatting van documenten op basis van de lokale inhoud wordt gepresenteerd.

De methoden die zijn gepresenteerd in dit scenario kunnen een aantal belangrijke industriële werkbelastingen, zoals de detectie van afwijkingen van onderwerp trends, document verzameling overzichten en vergelijkbare document zoeken inschakelen. Het kan worden toegepast op veel verschillende typen documentenanalyse, zoals government wetgeving, nieuws, recensies, feedback van klanten en wetenschappelijk onderzoek artikelen.

De machine learning technieken /-algoritmen die in dit scenario zijn onder andere:

1. Verwerking van tekst en opschonen

1. Woordgroep Learning

1. Modellering van onderwerp

1. Corpus samenvatting

1. Lokale trends en detectie van afwijkingen

## <a name="prerequisites"></a>Vereisten

De vereisten voor het uitvoeren van dit voorbeeld zijn er als volgt uit:

* Zorg ervoor dat u correct hebt geïnstalleerd [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md) door de [installeren en maken van de snelstartgids](../service/quickstart-installation.md).

* In dit voorbeeld kan worden uitgevoerd op een compute-context. Het wordt echter aanbevolen uit te voeren op een computer meerdere kernen met ten minste 16GB geheugen en 5GB schijfruimte.

## <a name="create-a-new-workbench-project"></a>Een nieuw Workbench-project maken

Maak een nieuw project met behulp van dit voorbeeld als een sjabloon:
1.  Open Azure Machine Learning Workbench
2.  Op de **projecten** pagina, klikt u op de **+** Meld u aan en selecteer **nieuw Project**
3.  In de **nieuw Project maken** deelvenster, vult u in de gegevens voor het nieuwe project
4.  In de **zoeken naar projectsjablonen** het zoekvak, typ 'Documentverzamelingen' en selecteer de sjabloon
5.  Klik op **Maken**.

## <a name="data-description"></a>Beschrijving van de gegevens

De gegevensset die in dit scenario bevat tekst samenvattingen en gekoppelde metagegevens voor elke wetgevende actie op die door het Amerikaanse congres. De gegevens worden verzameld van [GovTrack.us](https://www.govtrack.us/), die de activiteiten van de Verenigde Staten worden bijgehouden en helpt Amerikanen deel te nemen in hun nationale wetgevende proces. De bulksgewijs gegevens kunnen worden gedownload [deze koppeling](https://www.govtrack.us/data/congress/) met behulp van een handmatige script, dat niet is opgenomen in dit scenario. De details van het downloaden van de gegevens kunnen worden gevonden in de [GovTrack API-documentatie](https://www.govtrack.us/developers).

### <a name="data-source"></a>Gegevensbron

In dit scenario is de onbewerkte gegevens verzameld een reeks wetgevende acties die zijn geïntroduceerd door het Amerikaanse congres (voorgestelde facturen en oplossingen) van 1973 juni 2017 (de 93rd naar de 115th congressen). De verzamelde gegevens wordt in JSON-indeling en bevat een grote verscheidenheid aan informatie over de wetgevende acties. Raadpleeg [deze link naar GitHub](https://github.com/unitedstates/congress/wiki/bills) voor een gedetailleerde beschrijving van de gegevensvelden. Voor het doel van de demonstratie binnen dit scenario, zijn alleen een subset van gegevensvelden opgehaald uit de JSON-bestanden. Een vooraf gecompileerde TSV-bestand `CongressionalDataAll_Jun_2017.tsv` met records wetgevende acties vindt u in dit scenario. De TSV-bestand kan automatisch worden gedownload door de notebooks `1_Preprocess_Text.ipynb` onder de notitieblokmap of `preprocessText.py` in het Python-pakket.

### <a name="data-structure"></a>Gegevensstructuur

Er zijn negen gegevensvelden in het gegevensbestand. De veldnamen en de beschrijvingen worden als volgt weergegeven.

| Veldnaam | Type | Beschrijving | Ontbrekende waarde bevatten |
|------------|------|-------------|---------------|
| `ID` | Reeks | De ID van de factuur/resolutie. De indeling van dit veld is [bill_type] [aantal]-[Congress, Shanghai]. Bijvoorbeeld, "hconres1-93" betekent dat het factuurtype is 'hconres' (staat voor House gelijktijdige resolutie, verwijzen naar [dit document](https://github.com/unitedstates/congress/wiki/bills#basic-information)), het nummer van de factuur is ' 1 'en het aantal Congress, Shanghai is ' 93'. | Nee |
| `Text` | Reeks | De inhoud van de factuur/resolutie. | Nee |
| `Date` | Reeks | De datum waarop de factuur/oplossing in eerste instantie voorgesteld. In de indeling van 'jjjj-mm-dd'. | Nee |
| `SponsorName` | Reeks | De naam van de primaire sponsor die de factuur/resolutie voorgesteld. | Ja |
| `Type` | Reeks | Het type van de titel van de primaire sponsor, 'vertegenwoordiger' (medewerker) of 'ver' (senator). | Ja |
| `State` | Reeks | De status van de primaire sponsor. | Ja |
| `District` | Geheel getal | Het aantal district van de primaire sponsor als de titel van de sponsor een vertegenwoordiger is. | Ja |
| `Party` | Reeks | De partij van de primaire sponsor. | Ja |
| `Subjects` | Reeks | De certificaathouder voorwaarden cumulatief toegevoegd door de bibliotheek congres op de factuur. De voorwaarden worden samengevoegd door komma's. Deze voorwaarden zijn geschreven door een persoon in de bibliotheek congres en zijn niet meestal aanwezig wanneer gegevens op de factuur voor het eerst wordt gepubliceerd. Ze kunnen op elk gewenst moment worden toegevoegd. Dus aan het einde van de levensduur van een factuur, sommige onderwerp mogelijk niet relevant meer. | Ja |

## <a name="scenario-structure"></a>Scenario-structuur

Het document verzameling analysis-voorbeeld is onderverdeeld in twee soorten producten. Het eerste type is een reeks iPython-notitieblokken waartoe de stapsgewijze beschrijvingen van de volledige werkstroom weergeven. Het tweede type is een Python-pakket, evenals enkele codevoorbeelden van het gebruik van dit pakket. Het Python-pakket is generiek genoeg is voor het bieden van veel van use cases.

De bestanden in dit voorbeeld zijn als volgt ingedeeld.

| Bestandsnaam | Type | Beschrijving |
|-----------|------|-------------|
| `aml_config` | Map | Azure Machine Learning Workbench configuratiemap, verwijzen naar [deze documentatie](./experimentation-service-configuration-reference.md) voor gedetailleerde experiment uitvoering configuratie |
| `Code` | Map | De code-map die wordt gebruikt om op te slaan van de Python-scripts en Python-pakket |
| `Data` | Map | De map data gebruikt voor het opslaan van tussenliggende bestanden |
| `notebooks` | Map | De map Jupyter-notitieblokken |
| `Code/documentAnalysis/__init__.py` | Python-bestand | Het Python-pakket init-bestand |
| `Code/documentAnalysis/configs.py` | Python-bestand | Het configuratiebestand gebruikt door de documentenanalyse Python-pakket, met inbegrip van vooraf gedefinieerde constanten |
| `Code/documentAnalysis/preprocessText.py` | Python-bestand | Het Python-bestand gebruikt met de voorverwerking van de gegevens voor downstream-taken |
| `Code/documentAnalysis/phraseLearning.py` | Python-bestand | Het Python-bestand gebruikt om te leren van zinnen uit de gegevens en de onbewerkte gegevens te transformeren |
| `Code/documentAnalysis/topicModeling.py` | Python-bestand | Het Python-bestand dat is gebruikt voor het trainen van een model van de onderwerp latente Dirichlet toewijzing (LDA) |
| `Code/step1.py` | Python-bestand | Stap 1 van documentverzamelingen: tekst voorverwerken |
| `Code/step2.py` | Python-bestand | Stap 2 van documentverzamelingen: meer woorden learning |
| `Code/step3.py` | Python-bestand | Stap 3 van documentverzamelingen: trainen en evalueren LDA onderwerp model |
| `Code/runme.py` | Python-bestand | Voorbeeld van alle stappen in één bestand uitvoeren |
| `notebooks/1_Preprocess_Text.ipynb` | iPython Notebook | Voorverwerken tekst en de onbewerkte gegevens te transformeren |
| `notebooks/2_Phrase_Learning.ipynb` | iPython Notebook | Informatie over zinnen van tekstgegevens (na de gegevenstransformatie) |
| `notebooks/3_Topic_Model_Training.ipynb` | iPython Notebook | Train LDA onderwerp model |
| `notebooks/4_Topic_Model_Summarization.ipynb` | iPython Notebook | De inhoud van de documentverzameling op basis van model is getraind onderwerp LDA samenvatten |
| `notebooks/5_Topic_Model_Analysis.ipynb` | iPython Notebook | Analyseren van de lokale inhoud van een verzameling van documenten en lokale informatie met betrekking tot andere metagegevens die zijn gekoppeld aan de documentverzameling correleren |
| `notebooks/6_Interactive_Visualization.ipynb` | iPython Notebook | Interactieve visualisatie van geleerde onderwerp model |
| `notebooks/winprocess.py` | Python-bestand | Het python-script voor multiprocessing die worden gebruikt door notitieblokken |
| `README.md` | Markdown-bestand | Het Leesmij-bestand markdown-bestand |

### <a name="data-ingestion-and-transformation"></a>Gegevensopname en -transformatie

De gecompileerde gegevensset `CongressionalDataAll_Jun_2017.tsv` wordt opgeslagen in Blob-opslag en toegankelijk is zowel van binnen de notebooks en de Python-scripts. Er zijn twee stappen voor gegevensopname en -transformaties: voorverwerking van de gegevens en meer woorden learning.

#### <a name="preprocess-text-data"></a>Tekstgegevens voorverwerken

De voorverwerking stap is van toepassing technieken voor de verwerking van natuurlijke taal voor het opschonen en voorbereiden van de onbewerkte gegevens. Het fungeert als een precursor voor het leren zonder supervisie woordgroep en latente onderwerp modellen. De gedetailleerde stapsgewijze beschrijving vindt u in het notitieblok `1_Preprocess_Text.ipynb`. Er is ook een Python-script `step1.py` komt overeen met dit notitieblok.

In deze stap is het gegevensbestand TSV gedownload van de Azure Blob-opslag en geïmporteerd als een Pandas DataFrame. Elk element van de rij in de DataFrame is een eenmalige samenhangend lange tekenreeks van de tekst of een 'document'. Elk document wordt vervolgens gesplitst in segmenten van de tekst die kunnen worden zinnen, zinnen of subphrases. De splitsing is ontworpen om te voorkomen dat de woordgroep leerproces cross-zin of cross-zin word tekenreeksen te gebruiken bij het leren van zinnen.

Er zijn meerdere functies die zijn gedefinieerd voor de voorverwerking stap in het notitieblok en het Python-pakket. Het merendeel van de taak kan worden bereikt door het aanroepen van deze twee regels van codes.

```python
# Read raw data into a Pandas DataFrame
textDF = getData()

# Write data frame with preprocessed text out to TSV file
cleanedDataFrame = CleanAndSplitText(textDF, saveDF=True)
```

#### <a name="phrase-learning"></a>Woordgroep learning

De woordgroep learning-stap implementeert een eenvoudige framework voor meer sleuteltermen uit een grote verzameling van documenten. Deze wordt beschreven in het document van het recht '[Multiword zinnen modelleren met beperkte zinnen structuur voor verbeterde onderwerp modelleren van de conversatie-spraak](http://people.csail.mit.edu/hazen/publications/Hazen-SLT-2012.pdf)', die oorspronkelijk is opgegeven in de 2012 IEEE-Workshop van gesproken taal -Technologie. De uitvoering van woordgroep learning-stap in detail wordt weergegeven in de iPython Notebook `2_Phrase_Learning.ipynb` en het Python-script `step2.py`.

Deze stap neemt de opgeschoonde tekst als invoer en leert de meest opvallende zinnen die aanwezig zijn in een grote verzameling van documenten. De woordgroep learning is een iteratief proces die kan worden onderverdeeld in drie taken: n-grammen tellen, rangschikking van mogelijke zinnen door de gewogen Pointwise wederzijdse gegevens van hun samenstellende woorden en woordgroep om tekst te herschrijven. Deze drie taken worden na elkaar uitgevoerd in meerdere pogingen tot de opgegeven items hebt geleerd hoe.

In het document analysis Python-pakket, een Python-klasse `PhraseLearner` is gedefinieerd in de `phraseLearning.py` bestand. Hieronder volgt het codefragment gebruikt voor meer informatie over zinnen.

```python
# Instantiate a PhraseLearner and run a configuration
phraseLearner = PhraseLearner(cleanedDataFrame, "CleanedText", numPhrase,
                        maxPhrasePerIter, maxPhraseLength, minInstanceCount)

# The chunks of text in a list
textData = list(phraseLearner.textFrame['LowercaseText'])

# Learn most salient phrases present in a large collection of documents
phraseLearner.RunConfiguration(textData,
            phraseLearner.learnedPhrases,
            addSpace=True,
            writeFile=True,
            num_workers=cpu_count()-1)
```

> [!NOTE]
> De woordgroep learning-stap wordt geïmplementeerd met MP-verwerking. Echter meer CPU-kernen doen **niet** betekent een snellere uitvoeringstijd. In onze tests, is niet de prestaties verbeterd met meer dan acht kernen vanwege de overhead van MP-verwerking. Dit duurde ongeveer twee en een half uur voor meer informatie over 25.000 zinnen op een computer met acht kernen (3,6 GHz).
>

### <a name="topic-modeling"></a>Modellering van onderwerp

Leren werken met een model latente onderwerp gebruik is LDA de derde stap in dit scenario. De [gensim](https://radimrehurek.com/gensim/) Python-pakket in deze stap is vereist voor meer informatie over een [LDA onderwerp model](https://en.wikipedia.org/wiki/Latent_Dirichlet_allocation). De bijbehorende laptop voor deze stap is `3_Topic_Model_Training.ipynb`. U kunt ook verwijzen naar `step3.py` voor het gebruik van het document analysis-pakket.

In deze stap worden de belangrijkste taak is om te leren van een onderwerp LDA model en afstemmen van de hyper-parameters. Er zijn meerdere parameters moeten worden afgestemd op wanneer een LDA-model te trainen, maar de belangrijkste parameter is het aantal onderwerpen. Te weinig onderwerpen mogelijk geen inzicht aan de documentenverzameling; bij het kiezen van te resulteert veel in de 'te veel clustering' van een corpus in veel kleine, maximaal vergelijkbare onderwerpen. Het doel van dit scenario is om te laten zien hoe u het aantal onderwerpen af te stemmen. Azure Machine Learning Workbench biedt de vrijheid om te experimenten met verschillende parameterconfiguratie op verschillende compute-context wordt uitgevoerd.

In het document analysis Python-pakket enkele functies zijn gedefinieerd, zodat de gebruikers het aanbevolen aantal onderwerpen te achterhalen. De eerste benadering bestaat door de samenhang van het onderwerp-model te evalueren. Er zijn vier matrices voor evaluatie ondersteund: `u_mass`, `c_v`, `c_uci`, en `c_npmi`. De details van deze vier metrische gegevens worden besproken in [in dit artikel](http://svn.aksw.org/papers/2015/WSDM_Topic_Evaluation/public.pdf). De tweede methode is het evalueren van de perplexity op een corpus ondergebracht-out.

Een curve van de vorm 'U' wordt voor de evaluatie perplexity verwacht om het aanbevolen aantal onderwerpen erachter te komen. En de positie van de hoek is de beste keuze. Het verdient aanbeveling om te evalueren van meerdere keren met verschillende random seed en ophalen van het gemiddelde. De coherentie evalueren verwacht wordt een "n" vorm te geven, wat betekent dat de samenhang neemt toe met het verhogen van het aantal onderwerpen en vervolgens verlagen. Een voorbeeld van de grafiek van perplexity en `c_v` samenhang als volgt wordt weergegeven.

![Perplexity](./media/scenario-document-collection-analysis/Perplexity_Value.png)

![c_v samenhang](./media/scenario-document-collection-analysis/c_v_Coherence.png)

In dit scenario aanzienlijk de perplexity kan na 200 onderwerpen, terwijl de waarde van de samenhang deze aanzienlijk na ook 200 onderwerpen. Gebaseerd op de grafieken en de keuze voor meer algemene onderwerpen versus voor geclusterde onderwerpen, kiest u 200 onderwerpen moet een goede optie.

U kunt een LDA onderwerp model in één experiment uitvoert, of modellen trainen en evalueren meerdere LDA met verschillende onderwerp aantal configuraties in één experiment uitvoeren trainen. Het wordt aanbevolen om meerdere keren uitvoeren voor één configuratie en haal vervolgens de gemiddelde coherentie en/of perplexity evaluaties. De details van het gebruik van het document analysis-pakket te vinden in `step3.py` bestand. Een voorbeeld-codefragment is als volgt.

```python
topicmodeler = TopicModeler(docs,
        stopWordFile=FUNCTION_WORDS_FILE,
        minWordCount=MIN_WORD_COUNT,
        minDocCount=MIN_DOC_COUNT,
        maxDocFreq=MAX_DOC_FREQ,
        workers=cpu_count()-1,
        numTopics=NUM_TOPICS,
        numIterations=NUM_ITERATIONS,
        passes=NUM_PASSES,
        chunksize=CHUNK_SIZE,
        random_state=RANDOM_STATE,
        test_ratio=test_ratio)

# Train an LDA topic model
lda = topicmodeler.TrainLDA(saveModel=saveModel)

# Evaluate coherence metrics
coherence = topicmodeler.EvaluateCoherence(lda, coherence_types)

# Evaluate perplexity on a held-out corpus
perplex = topicmodeler.EvaluatePerplexity(lda)
```

> [!NOTE]
> De uitvoeringstijd voor een onderwerp LDA model te trainen, is afhankelijk van meerdere factoren, zoals de grootte van het corpus, parameterconfiguratie van hyper-, evenals het aantal kernen op de machine. Met behulp van meerdere CPU-kernen traint een snellere model. Met dezelfde hyper-parameter betekent meer kernen instellen echter minder updates tijdens de training. Het wordt aanbevolen om **ten minste 100 updates voor een geconvergeerd LDA-model te trainen**. De relatie tussen het aantal updates en hyper-parameters wordt besproken in [dit bericht](https://groups.google.com/forum/#!topic/gensim/ojySenxQHi4) en [dit bericht](http://miningthedetails.com/blog/python/lda/GensimLDA/). In onze tests ongeveer 3 uur nodig was voor een model LDA met 200 onderwerpen met behulp van de configuratie van de trein `workers=15`, `passes=10`, `chunksize=1000` op een machine met 16 kernen (2,0 GHz).
>

### <a name="topic-summarization-and-analysis"></a>Onderwerp overzichten en analyses

Het onderwerp overzichten en analyses bestaat uit twee notebooks, terwijl er zijn geen overeenkomende functies in het document analysis-pakket.

In `4_Topic_Model_Summarization.ipynb`, ziet u hoe u een overzicht van de inhoud van het document op basis van een getraind model van LDA onderwerp. De samenvatting wordt toegepast op een LDA onderwerp-model dat u in stap 3 hebt geleerd. Het laat zien hoe u voor het meten van het belang of de kwaliteit van een onderwerp met behulp van onderwerp naar document zuiverheid meting. Deze meting zuiverheid wordt ervan uitgegaan dat latente onderwerpen die de documenten waarin ze worden weergegeven overheersen meer semantisch belangrijk dan latente onderwerpen die zwak zijn verdeeld over veel documenten zijn. Dit concept is geïntroduceerd in het document '[latente onderwerp-modellen bouwen voor Audio Corpus samenvatting](http://people.csail.mit.edu/hazen/publications/Hazen-Interspeech11.pdf). "

Laptop `5_Topic_Model_Analysis.ipynb` laat zien hoe u de lokale inhoud van een verzameling documenten analyseren en lokale informatie met betrekking tot andere metagegevens die zijn gekoppeld aan de documentverzameling correleren. Een aantal grafieken zijn geïntroduceerd in dit notitieblok om de gebruikers beter begrip van het geleerde onderwerp en de documentenverzameling te helpen.

Laptop `6_Interactive_Visualization.ipynb` laat zien hoe u interactief visualiseren geleerde onderwerp model. Het omvat vier interactieve visualisatie-taken.

## <a name="conclusion"></a>Conclusie

In dit scenario praktijk markeert Well-Known text analytics-technieken (in dit geval, woordgroep learning en LDA onderwerp modellering) gebruiken voor het produceren van een robuust model en hoe u Azure Machine Learning Workbench kunt bijhouden modelprestaties en naadloos uitvoeren cursisten op een hoger. In meer detail:

* Gebruik woordgroep learning en onderwerp modellen voor het verwerken van een verzameling van documenten en een robuust model bouwen. Als de verzameling van documenten enorm is, Azure Machine Learning Workbench kunt eenvoudig de schaal vergroten en verkleinen. Bovendien hebben gebruikers de vrijheid om te experimenten uitvoeren op verschillende compute-context eenvoudig uit in Azure Machine Learning Workbench.

* Azure Machine Learning Workbench biedt beide opties voor het uitvoeren van notitieblokken in een stap voor stap manier en Python-script voor een volledige experiment uit te voeren.

* Hyper-parameter afstemmen met behulp van Azure Machine Learning Workbench het aanbevolen aantal onderwerpen die nodig zijn voor meer informatie. Azure Machine Learning Workbench kunt u de prestaties van het model en verschillende cursisten naadloos uitvoeren op een hoger.

* Azure Machine Learning Workbench kunt de uitvoeringsgeschiedenis en geleerde modellen beheren. Hiermee kunt gegevenswetenschappers snel identificeren van de best presterende modellen, en om de scripts en gegevens die worden gebruikt voor het genereren van deze te vinden.

## <a name="references"></a>Verwijzingen

* **Timothy J. Hazen, Fred Richardson**, [ _onderwerp modellen van de conversatie spraak Multiword zinnen met beperkte zinnen structuur voor het modelleren verbeterd_](http://people.csail.mit.edu/hazen/publications/Hazen-SLT-2012.pdf). Gesproken taal technologie Workshop (SLT), 2012 IEEE. IEEE, 2012.

* **Timothy J. Hazen**, [ _latente onderwerp modellen bouwen voor Audio Corpus samenvatting_](http://people.csail.mit.edu/hazen/publications/Hazen-Interspeech11.pdf). 12 jaarlijkse conferentie van de koppeling met de internationale spraak communicatie. 2011.

* **Michael Roder, Andreas beide, Alexander Hinneburg**, [ _verkennen van de ruimte van onderwerp samenhang metingen_](http://svn.aksw.org/papers/2015/WSDM_Topic_Evaluation/public.pdf). De procedures van de achtste ACM internationale conferentie over zoeken op het Web en gegevensanalyse. ACM, 2015.
