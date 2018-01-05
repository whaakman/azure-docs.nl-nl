---
title: Verzameling analyse - Azure-document | Microsoft Docs
description: Het samenvatten en analyseren van een grote verzameling van documenten, met inbegrip van technieken zoals woordgroep learning onderwerp modelleren en onderwerp model analyse met behulp van Azure ML-Workbench.
services: machine-learning
author: kehuan
ms.author: kehuan
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, MicrosoftDocs/mlreview, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: a6034652f27765bb20db4dbbb4c25741b261e50a
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="document-collection-analysis"></a>Document verzameling analyse

Dit scenario laat zien hoe samenvatten en analyseren van een grote verzameling van documenten, met inbegrip van technieken zoals woordgroep learning onderwerp modelleren en onderwerp model analyse met behulp van Azure ML-Workbench. Azure Machine Learning-Workbench biedt voor het eenvoudig scale voor zeer grote documentverzameling en biedt mechanismen om te trainen en -modellen in tal van compute-contexten, variërend van lokale compute aan gegevens wetenschappelijke virtuele Machines om Spark-Cluster te optimaliseren. Eenvoudig ontwikkeling is beschikbaar via Jupyter-notebooks in Azure Machine Learning-Workbench.

## <a name="link-to-the-gallery-github-repository"></a>Koppeling naar de galerie GitHub-opslagplaats

De openbare GitHub-opslagplaats voor dit scenario werkelijkheid bevat alle materialen, inclusief codevoorbeelden, die nodig zijn voor dit voorbeeld:

[https://github.com/Azure/MachineLearningSamples-DocumentCollectionAnalysis](https://github.com/Azure/MachineLearningSamples-DocumentCollectionAnalysis)

## <a name="overview"></a>Overzicht

Bij een grote hoeveelheid gegevens (met name ongestructureerde tekstgegevens) die elke dag wordt verzameld, wordt een belangrijke uitdaging organiseren, doorzoeken en grote hoeveelheden van deze teksten begrijpen. Dit document verzameling analysis scenario ziet u een efficiënte en geautomatiseerde end-to-end-werkstroom voor het analyseren van grote documentverzameling en downstream NLP taken inschakelen.

De belangrijkste elementen geleverd door dit scenario zijn:

1. Learning meest kenmerkende meerdere woorden woordgroep van documenten.

1. Detectie van onderliggende onderwerpen in de documentenverzameling.

1. Documenten die door de verdeling van belang.

1. Methoden voor het ordenen, zoeken en documenten op basis van de lokale inhoud samenvatten presenteren.

De methoden die zijn gepresenteerd in dit scenario kunnen een aantal essentiële industriële werkbelastingen, zoals de detectie van onderwerp trends afwijkingsdetectie document verzameling samenvatting en vergelijkbare document zoeken inschakelen. Deze kan worden toegepast op veel verschillende soorten document analyses, zoals government wetgeving, nieuws, product beoordelingen, feedback van klanten en wetenschappelijke artikelen.

De machine learning technieken /-algoritmen die in dit scenario zijn onder andere:

1. Verwerking van tekst en opschonen

1. Woordgroep Learning

1. Onderwerp-model

1. Samenvatting van corpus

1. Belang trends en afwijkingsdetectie

## <a name="prerequisites"></a>Vereisten

De vereisten voor het uitvoeren van dit voorbeeld zijn als volgt:

* Zorg ervoor dat u goed hebt geïnstalleerd [Azure Machine Learning Workbench](./overview-what-is-azure-ml.md) volgens de [installeren en maak Quick Start](quickstart-installation.md).

* In dit voorbeeld kan worden uitgevoerd op elke compute-context. Het wordt echter aanbevolen uit te voeren op een machine multicore met ten minste 16GB geheugen-en 5GB schijfruimte.

## <a name="create-a-new-workbench-project"></a>Maak een nieuw project in de Workbench

Maak een nieuw project in dit voorbeeld als sjabloon gebruiken:
1.  Open Azure Machine Learning Workbench
2.  Op de **projecten** pagina, klikt u op de  **+**  en selecteer **nieuw Project**
3.  In de **nieuw Project maken** deelvenster Vul de informatie voor het nieuwe project
4.  In de **zoeken projectsjablonen** het zoekvak, typ 'Document verzameling Analysis' en selecteer de sjabloon
5.  Klik op **Maken**.

## <a name="data-description"></a>Beschrijving van de gegevens

De gegevensset die in dit scenario bevat overzichten van de tekst en gekoppelde metagegevens voor elke wettelijke actie op die door het congres ons. De gegevens worden verzameld van [GovTrack.us](https://www.govtrack.us/), die de activiteiten van de Verenigde Staten bijgehouden en helpt Americans deelnemen aan hun nationale wettelijke proces. De bulksgewijs gegevens kunnen worden gedownload [deze koppeling](https://www.govtrack.us/data/congress/) met een handmatige script, dat niet is opgenomen in dit scenario. De details van het downloaden van de gegevens kunnen worden gevonden in de [GovTrack API-documentatie](https://www.govtrack.us/developers/api).

### <a name="data-source"></a>Gegevensbron

In dit scenario is de onbewerkte gegevens verzameld een reeks wettelijke acties die zijn geïntroduceerd door het ons congres (voorgestelde facturen en oplossingen) van 1973 juni 2017 (93rd naar de 115th congressen). De gegevens verzameld in JSON-indeling en bevat een uitgebreide set van informatie over de wettelijke acties. Raadpleeg [deze koppeling GitHub](https://github.com/unitedstates/congress/wiki/bills) voor gedetailleerde beschrijving van de gegevensvelden. Voor het doel van de demonstratie binnen dit scenario, alleen een subset van gegevens zijn opgehaald uit de JSON-bestanden. Een vooraf gecompileerde TSV bestand `CongressionalDataAll_Jun_2017.tsv` met records van deze wettelijke acties vindt u in dit scenario. Het bestand TSV kan automatisch worden gedownload door de laptops `1_Preprocess_Text.ipynb` onder de map notebook of `preprocessText.py` in het Python-pakket.

### <a name="data-structure"></a>Gegevensstructuur

Er zijn negen velden in het gegevensbestand. De veldnamen en de beschrijvingen worden als volgt weergegeven.

| Veldnaam | Type | Beschrijving | Ontbrekende waarde bevatten |
|------------|------|-------------|---------------|
| `ID` | Tekenreeks | De ID van de factuur/resolutie. De indeling van dit veld is [bill_type] [aantal]-[congress]. Bijvoorbeeld 'hconres1-93' betekent dat het factuurtype 'hconres' (staat House gelijktijdige resolutie verwijzen naar [dit document](https://github.com/unitedstates/congress/wiki/bills#basic-information)), het aantal factuur is ' 1 'en het aantal congress is 93'. | Nee |
| `Text` | Tekenreeks | De inhoud van de factuur/resolutie. | Nee |
| `Date` | Tekenreeks | De datum van de factuur/resolutie in eerste instantie voorgesteld. In de indeling van 'jjjj-mm-dd'. | Nee |
| `SponsorName` | Tekenreeks | De naam van de primaire sponsor die de factuur/oplossing wordt voorgesteld. | Ja |
| `Type` | Tekenreeks | Het type van de titel van de primaire sponsor 'rep' (medewerker) of 'afzen' (senator). | Ja |
| `State` | Tekenreeks | De status van de primaire sponsor. | Ja |
| `District` | Geheel getal | Het aantal regionale van de primaire sponsor als de titel van de sponsor een representatief is. | Ja |
| `Party` | Tekenreeks | De partij van de primaire sponsor. | Ja |
| `Subjects` | Tekenreeks | De certificaathouder voorwaarden cumulatief door de Library of Congress aan de factuur toegevoegd. De voorwaarden worden samengevoegd door komma's. Deze voorwaarden zijn geschreven door een persoon in de bibliotheek of Congress en zijn niet meestal aanwezig wanneer informatie over de factuur voor het eerst wordt gepubliceerd. Ze kunnen op elk gewenst moment worden toegevoegd. Dus aan het einde van de levensduur van een factuur sommige onderwerp mogelijk niet relevant zijn meer. | Ja |

## <a name="scenario-structure"></a>Scenario-structuur

Het document verzameling analysis voorbeeld is onderverdeeld in twee soorten producten. Het eerste type is een reeks iPython laptops die de stapsgewijze beschrijvingen van de volledige werkstroom weergeven. Het tweede type is een Python-pakket, evenals enkele codevoorbeelden van het gebruik van dit pakket. Het Python-pakket is algemene veel gebruiksvoorbeelden bedienen.

De bestanden in dit voorbeeld zijn als volgt worden ingedeeld.

| Bestandsnaam | Type | Beschrijving |
|-----------|------|-------------|
| `aml_config` | Map | Azure Machine Learning Workbench configuratiemap, verwijzen naar [deze documentatie](./experimentation-service-configuration-reference.md) voor gedetailleerde experiment uitvoering configuratie |
| `Code` | Map | De code map gebruikt voor het opslaan van de Python-scripts en Python-pakket |
| `Data` | Map | De map data gebruikt voor het opslaan van de tussenliggende bestanden |
| `notebooks` | Map | De map Jupyter-notitieblokken |
| `Code/documentAnalysis/__init__.py` | Python-bestand | De Python package init-bestand |
| `Code/documentAnalysis/configs.py` | Python-bestand | Het configuratiebestand gebruikt door het document analysis Python-pakket, met inbegrip van vooraf gedefinieerde constanten |
| `Code/documentAnalysis/preprocessText.py` | Python-bestand | Het Python-bestand dat is gebruikt voor de gegevens voor downstream taken voorverwerken |
| `Code/documentAnalysis/phraseLearning.py` | Python-bestand | Het Python-bestand gebruikt om te leren zinnen van de gegevens en de onbewerkte gegevens transformeren |
| `Code/documentAnalysis/topicModeling.py` | Python-bestand | Het Python-bestand dat is gebruikt voor een onderwerp latente Dirichlet toewijzing (LDA) model trainen |
| `Code/step1.py` | Python-bestand | Stap 1 van document verzameling analyse: tekst voorverwerken |
| `Code/step2.py` | Python-bestand | Stap 2 van document verzameling analyse: learning woorden |
| `Code/step3.py` | Python-bestand | Stap 3 van document verzameling analyse: trainen en evalueren LDA onderwerp model |
| `Code/runme.py` | Python-bestand | Voorbeeld van alle stappen in één bestand uitvoeren |
| `notebooks/1_Preprocess_Text.ipynb` | iPython Notebook | Voorverwerken tekst en de onbewerkte gegevens transformeren |
| `notebooks/2_Phrase_Learning.ipynb` | iPython Notebook | Meer informatie over zinnen van tekstgegevens (na de gegevenstransformatie) |
| `notebooks/3_Topic_Model_Training.ipynb` | iPython Notebook | Train LDA onderwerp model |
| `notebooks/4_Topic_Model_Summarization.ipynb` | iPython Notebook | De inhoud van de documentenverzameling op basis van model is getraind onderwerp LDA samenvatten |
| `notebooks/5_Topic_Model_Analysis.ipynb` | iPython Notebook | Analyseer de belang inhoud van een verzameling van documenten en belang informatie met betrekking tot andere metagegevens die zijn gekoppeld aan de documentenverzameling correleren |
| `notebooks/6_Interactive_Visualization.ipynb` | iPython Notebook | Interactieve visualisatie van geleerde onderwerp model |
| `notebooks/winprocess.py` | Python-bestand | Het pythonscript voor multiprocessing gebruikt door laptops |
| `README.md` | Markdown-bestand | Het bestand Leesmij markdown |

### <a name="data-ingestion-and-transformation"></a>Gegevensopname en transformatie

De gecompileerde gegevensset `CongressionalDataAll_Jun_2017.tsv` wordt opgeslagen in Blob Storage en toegankelijk is zowel vanuit binnen de laptops en de Python-scripts. Er zijn twee stappen voor gegevensopname en transformatie: voorverwerking van de gegevens en learning woorden.

#### <a name="preprocess-text-data"></a>Tekstgegevens voorverwerken

De voorverwerking stap geldt van natuurlijke taal verwerkingstechnieken opruimen en de onbewerkte gegevens wordt voorbereid. Het fungeert als een precursor voor het leren zonder supervisie woordgroep en latente onderwerp modelleren. De gedetailleerde stapsgewijze beschrijving vindt u in de notebook `1_Preprocess_Text.ipynb`. Er is ook een pythonscript `step1.py` komt overeen met deze laptop.

In deze stap is het gegevensbestand TSV gedownload van de Azure Blob Storage en geïmporteerd als een DataFrame Pandas. Elk element in de rij in de DataFrame is een enkel samenhangender lange tekenreeks of een 'document'. Elk document wordt vervolgens onderverdelen in segmenten van de tekst die waarschijnlijk zinnen, woordgroepen of subphrases. De splitsing is ontworpen om te voorkomen dat de woordgroep leerproces cross-zin of cross-zin woord tekenreeksen te gebruiken bij learning zinnen.

Er zijn meerdere functies die zijn gedefinieerd voor de voorverwerking stap in de notebook en het Python-pakket. Het merendeel van de taak kan worden bereikt door het aanroepen van deze twee regels van codes.

```python
# Read raw data into a Pandas DataFrame
textDF = getData()

# Write data frame with preprocessed text out to TSV file
cleanedDataFrame = CleanAndSplitText(textDF, saveDF=True)
```

#### <a name="phrase-learning"></a>Woordgroep learning

De wachtwoordzin learning stap implementeert een basic framework voor meer informatie over belangrijke zinnen tussen een grote verzameling van documenten. Wordt beschreven in het artikel het recht '[modelleren Multiword woordgroepen met beperkte zinnen structuur voor verbeterde onderwerp modelleren van eigen-spraak](http://people.csail.mit.edu/hazen/publications/Hazen-SLT-2012.pdf)', die oorspronkelijk is opgegeven in de 2012 IEEE Workshop van taal gesproken Technologie. De uitvoering van woordgroep learning stap in detail wordt weergegeven in de iPython Notebook `2_Phrase_Learning.ipynb` en het Python-script `step2.py`.

Deze stap duurt de opgeschoonde tekst als invoer en leert de meest meest kenmerkende zinnen aanwezig is in een grote verzameling van documenten. De wachtwoordzin learning is een iteratief proces die kan worden onderverdeeld in drie taken: tellen n g, potentiële zinnen door de gewogen Pointwise wederzijdse gegevens van hun samenstellende woorden rangschikken en woordgroep op tekst herschrijven. Deze drie taken worden sequentieel uitgevoerd in meerdere pogingen tot de opgegeven zinnen hebt geleerd, zijn.

In het document analysis Python-pakket, een Python-klasse `PhraseLearner` is gedefinieerd in de `phraseLearning.py` bestand. Hieronder ziet u het codefragment gebruikt voor meer informatie over zinnen.

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
> De wachtwoordzin learning stap is met multiprocessing geïmplementeerd. Echter meer CPU-kernen doen **niet** betekent dat een sneller worden uitgevoerd. In onze tests, is niet de prestaties verbeterd met meer dan acht kernen vanwege de overhead van multiprocessing. Het heeft geduurd ongeveer twee en een half uur voor meer informatie over 25.000 zinnen op een computer met acht kernen (3,6 GHz).
>

### <a name="topic-modeling"></a>Onderwerp-model

Een model latente onderwerp gebruik Learning is LDA de derde stap in dit scenario. De [gensim](https://radimrehurek.com/gensim/) Python-pakket in deze stap is vereist voor meer informatie over een [LDA onderwerp model](https://en.wikipedia.org/wiki/Latent_Dirichlet_allocation). De bijbehorende laptop voor deze stap is `3_Topic_Model_Training.ipynb`. U kunt ook verwijzen naar `step3.py` voor het gebruik van het document analysis-pakket.

In deze stap worden de belangrijkste taak is te leren van een onderwerp LDA model en afstemmen van de hyper-parameters. Er zijn meerdere parameters moeten worden geconfigureerd wanneer een LDA-model te trainen, maar de belangrijkste parameter is het aantal onderwerpen. Te weinig onderwerpen mogelijk geen inzicht in de documentenverzameling; bij het kiezen van te leidt veel ertoe dat de 'veel clustering' van een corpus in veel kleine, maximaal vergelijkbaar onderwerpen. Het doel van dit scenario is om het afstemmen van het aantal onderwerpen weer te geven. Azure Machine Learning Workbench biedt de vrijheid experimenten uitvoeren met andere configuratie op andere compute-contexten.

In het document analysis Python-pakket enkele functies zijn gedefinieerd, zodat de gebruikers nagaan wat het beste aantal onderwerpen. De eerste benadering bestaat door de coherentie van het onderwerp-model te evalueren. Er zijn vier matrices voor evaluatie ondersteund: `u_mass`, `c_v`, `c_uci`, en `c_npmi`. De details van de vier metrische gegevens worden besproken in [dit artikel](http://svn.aksw.org/papers/2015/WSDM_Topic_Evaluation/public.pdf). De tweede aanpak is het evalueren van de perplexity op een corpus ondergebracht-out.

Voor de evaluatie perplexity wordt een curve van de vorm 'U' om erachter te komen het aanbevolen aantal onderwerpen verwacht. En de positie van de hoek is de beste keuze. Het wordt aanbevolen als u wilt meerdere keren met verschillende willekeurige seed beoordelen en het gemiddelde. De coherentie evalueren een "n" wordt verwacht shape, wat betekent dat de toeneemt samenhang met de waarde van onderwerpen en vervolgens verkleinen. Een voorbeeld van grafiek van perplexity en `c_v` samenhang als volgt worden weergegeven.

![Perplexity](./media/scenario-document-collection-analysis/Perplexity_Value.png)

![c_v samenhang](./media/scenario-document-collection-analysis/c_v_Coherence.png)

In dit scenario wordt aanzienlijk de perplexity kan na 200 onderwerpen, terwijl de waarde samenhang na 200 onderwerpen ook aanzienlijk beperkt. Op basis van deze grafieken en de wens voor meer algemene onderwerpen versus via geclusterde onderwerpen, kies 200 onderwerpen moet een goede optie.

U kunt een LDA onderwerp model in een experiment uitvoeren, of trainen en evalueren van meerdere LDA modellen met nummer configuraties ander onderwerp in een enkel experiment uitvoeren trainen. Het wordt aanbevolen als u wilt meerdere keren uitvoeren voor één configuratie en vervolgens de gemiddelde samenhang en/of perplexity evaluaties. De details van het gebruik van het pakket van de analyse document vindt u in `step3.py` bestand. Een voorbeeld-codefragment is als volgt.

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
> De uitvoeringstijd voor een onderwerp LDA model trainen is afhankelijk van meerdere factoren zoals de grootte van corpus, hyper parameterconfiguratie, evenals het aantal kernen op de machine. Met meerdere CPU-kernen traint een snellere model. Met dezelfde hyper-parameter betekent meer cores instelling echter minder updates tijdens de training. Het verdient aanbeveling om **ten minste 100 updates voor het trainen van een model voor geconvergeerde LDA**. De relatie tussen een aantal updates en hyper parameters wordt besproken in [deze post](https://groups.google.com/forum/#!topic/gensim/ojySenxQHi4) en [deze post](http://miningthedetails.com/blog/python/lda/GensimLDA/). In onze tests ongeveer 3 uur nodig was voor het trainen van een model LDA met 200 onderwerpen met behulp van de configuratie van `workers=15`, `passes=10`, `chunksize=1000` op een machine met 16 kernen (2,0 GHz).
>

### <a name="topic-summarization-and-analysis"></a>Onderwerp-samenvatting en analyse

De samenvatting van onderwerp en analyse bestaat uit twee laptops, terwijl er zijn geen overeenkomende functies in het document analysis-pakket.

In `4_Topic_Model_Summarization.ipynb`, ziet u hoe de inhoud van het document op basis van model is getraind onderwerp LDA samenvatten. De samenvatting wordt toegepast op een LDA onderwerp model geleerd in stap 3. Er wordt weergegeven hoe meet de belang of de kwaliteit van een onderwerp met onderwerp naar document zuiverheid meting. Deze meting zuiverheid wordt ervan uitgegaan dat latente onderwerpen die een groot gedeelte uit de documenten waarin ze worden weergegeven zijn meer semantisch belangrijk dan latente onderwerpen die zwak zijn verdeeld over veel documenten. Dit concept is geïntroduceerd in het artikel '[latente onderwerp modellering voor Audio Corpus samenvatting](http://people.csail.mit.edu/hazen/publications/Hazen-Interspeech11.pdf). "

Laptop `5_Topic_Model_Analysis.ipynb` laat zien hoe de belang inhoud van een verzameling van documenten te analyseren en correleren van belang informatie met betrekking tot andere metagegevens die zijn gekoppeld aan de documentenverzameling. Enkele waarnemingspunten zijn geïntroduceerd in deze notebook om de gebruikers het geleerde onderwerp en de documentenverzameling beter te begrijpen.

Laptop `6_Interactive_Visualization.ipynb` laat zien hoe geleerde onderwerp model interactief te visualiseren. Dit omvat vier interactieve visualisatie taken.

## <a name="conclusion"></a>Conclusie

Dit scenario werkelijkheid licht Well-Known text analytics technieken (in dit geval woordgroep learning en LDA onderwerp modellering) gebruiken voor het produceren van een robuuste model en hoe Azure Machine Learning-Workbench kan helpen model prestaties bijhouden en naadloos uitvoeren overbrengen met hogere schaal. In meer detail:

* Woordgroep learning en onderwerp modellering gebruiken voor het verwerken van een verzameling van documenten en een robuuste model bouwen. Als de verzameling van documenten grote, Azure Machine Learning Workbench kunnen eenvoudig worden geschaald deze boven- en out. Gebruikers hebben bovendien de vrijheid om uit te voeren experimenten in verschillende compute-context eenvoudig vanuit Azure Machine Learning-Workbench.

* Azure Machine Learning Workbench biedt beide opties notitieblokken in een stap voor stap manier en Python-script uit te voeren van een volledige experiment uitvoeren.

* Hyper-parameter afstemmen met behulp van Azure Machine Learning Workbench vinden het aanbevolen aantal onderwerpen voor meer informatie nodig. Azure Machine Learning Workbench kan helpen de model-prestaties bijhouden en andere overbrengen naadloos worden uitgevoerd op hogere schaal.

* Azure Machine Learning Workbench kan de uitvoeringsgeschiedenis en geleerde modellen beheren. Hiermee kunt gegevenswetenschappers snel de best presterende modellen kan identificeren en om de scripts en gegevens die worden gebruikt voor het genereren van deze te vinden.

## <a name="references"></a>Verwijzingen

* **Timothy J. Hazen, Fred Richardson**, [ _onderwerp modellering van eigen spraak modelleren Multiword woordgroepen met beperkte zinnen-structuur voor verbeterde_](http://people.csail.mit.edu/hazen/publications/Hazen-SLT-2012.pdf). Gesproken technologie Workshop (SLT) 2012 IEEE. IEEE, 2012.

* **Timothy J. Hazen**, [ _latente onderwerp modellering voor Audio Corpus samenvatting_](http://people.csail.mit.edu/hazen/publications/Hazen-Interspeech11.pdf). 12de jaarlijkse conferentie van de communicatierelatie internationale spraak. 2011.

* **Michael Roder, Andreas beide, Alexander Hinneburg**, [ _verkennen van de ruimte van het onderwerp samenhang metingen_](http://svn.aksw.org/papers/2015/WSDM_Topic_Evaluation/public.pdf). De procedures van de achtste ACM internationale conferentie op webpagina's zoeken en gegevensanalyse. ACM, 2015.
