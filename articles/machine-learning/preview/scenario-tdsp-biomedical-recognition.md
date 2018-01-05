---
title: Biomedische entiteit opname - Team gegevens wetenschappelijke Process - Azure Machine Learning | Microsoft Docs
description: Een Team gegevens wetenschap proces project Quick Start die gebruikmaakt van grondige learning voor biomedische entiteit opname in Azure Machine Learning-Workbench.
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2017
ms.author: bradsev
ms.openlocfilehash: 7de3a30e477fcec66ce703b6c3fec7d17d79d3ab
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="biomedical-entity-recognition-using-team-data-science-process-tdsp-template"></a>Biomedische entiteit erkenning met behulp van de sjabloon Team gegevens wetenschap proces (TDSP)

Uitpakken van de entiteit is een subtaak van informatie extractie (ook wel bekend als [benoemde entiteit erkenning (NER)](https://en.wikipedia.org/wiki/Named-entity_recognition), verdeling in segmenten entiteit en entiteits-id). Het doel van deze Praktijkscenario is het gebruik van Azure Machine Learning Workbench voor het oplossen van een gecompliceerde taak van natuurlijke taal verwerken (NLP) zoals entiteit extractie van tekst ongestructureerde markeren:

1. Hoe een neural woord trainen insluitingen model op een corpus tekst van samenvattingen van punt 18 miljoen PubMed met [Spark Word2Vec implementatie](https://spark.apache.org/docs/latest/mllib-feature-extraction.html#word2vec).
2. Het bouwen van een grondige lang korte geheugen (LSTM) terugkerende neurale netwerk wordt getraind model voor het ophalen van de entiteit op een GPU ingeschakeld Azure gegevens wetenschappelijke virtuele Machine (VM GPU DS) in Azure.
2. Laten zien dat domeinspecifieke word insluitingen model kunt leveren betere prestaties dan algemene word insluitingen modellen in de entiteit opname-taak. 
3. Laten zien hoe trainen en dit te implementeren met behulp van Azure Machine Learning-Workbench grondige learning-modellen.

4. De volgende mogelijkheden in Azure Machine Learning Workbench demonstreren:

    * Instantiëring van [Team gegevens wetenschap proces (TDSP) structuur en sjablonen](how-to-use-tdsp-in-azure-ml.md).
    * Automatisch beheer van de afhankelijkheden van uw project met inbegrip van het downloaden en installeren
    * Uitvoering van scripts op differetn Python compute omgevingen.
    * Wijzigingsoverzicht voor Python-scripts worden uitgevoerd.
    * De uitvoering van taken op de externe Spark berekenen met behulp van clusters in HDInsight Spark 2.1 context.
    * De uitvoering van taken in externe GPU-virtuele machines in Azure.
    * Eenvoudig uitoefening van grondige learning-modellen als webservices op Azure Container Services (ACS).

## <a name="use-case-overview"></a>Gebruik van de case-overzicht
Herkenning biomedische benoemde entiteit is een essentiële stap voor complexe biomedische NLP taken, zoals: 
* De vermeldingen van benoemde eenheden uitpakken dergelijke ziekten, middelen, chemicaliën en symptomen van elektronische medische of health records.
* Medicijn detectie
* Informatie over de interacties tussen verschillende entiteit van het type zoals medicijn medicijn interactie, medicijn ziekte relatie en gen eiwit relatie.

Ons gebruik case-scenario is gericht op hoe een grote hoeveelheid corpus zoals Medline PubMed samenvattingen ongestructureerde gegevens kan worden geanalyseerd om te een woord insluiten van het model te trainen. De uitvoer-insluitingen worden vervolgens beschouwd als de automatisch gegenereerde functies voor het trainen van een neural entiteit zelfstandig uitpakken.

Onze resultaten blijkt dat de training biomedische entiteit extractie model op de domeinspecifieke word insluiten functies het model van het functietype van de algemene getraind presteert. Het model domeinspecifieke kunt 7012 entiteiten correct (buiten 9475) detecteren met F1-score van 0,73 vergeleken met 5274 entiteiten met F1-score van 0,61 voor het algemene model.

De volgende afbeelding toont de architectuur die is gebruikt voor het verwerken van gegevens en modellen trainen.

![Architectuur](./media/scenario-tdsp-biomedical-recognition/architecture.png)

## <a name="data-description"></a>Beschrijving van de gegevens

### <a name="1-word2vec-model-training-data"></a>1. Word2Vec model trainingsgegevens
We eerst de onbewerkte MEDLINE abstracte gegevens uit gedownload [MEDLINE](https://www.nlm.nih.gov/pubs/factsheets/medline.html). De gegevens zijn openbaar beschikbaar in de vorm van XML-bestanden op hun [FTP-server](https://ftp.ncbi.nlm.nih.gov/pubmed/baseline). Er zijn 892 XML-bestanden beschikbaar op de server en elk van de XML-bestanden heeft de gegevens van 30.000 artikelen. Meer informatie over de stap van de verzameling gegevens vindt u in de sectie projectstructuur. De velden in elk bestand aanwezig zijn 
        
        abstract
        affiliation
        authors
        country 
        delete: boolean if False means paper got updated so you might have two XMLs for the same paper.
        file_name   
        issn_linking    
        journal 
        keywords    
        medline_ta: this is abbreviation of the journal nam 
        mesh_terms: list of MeSH terms  
        nlm_unique_id   
        other_id: Other IDs 
        pmc: Pubmed Central ID  
        pmid: Pubmed ID
        pubdate: Publication date
        title

### <a name="2-lstm-model-training-data"></a>2. LSTM model trainingsgegevens

Het uitpakken van neural entiteitsmodel is getraind en geëvalueerd op publiclly beschikbare gegevenssets. Als u een gedetailleerde beschrijving van deze gegevenssets, kan u de volgende bronnen raadplegen:
 * [Biografie entiteit erkenning taak BioNLP/NLPBA 2004](http://www.nactem.ac.uk/tsujii/GENIA/ERtask/report.html)
 * [BioCreative V CDR taak corpus](http://www.biocreative.org/tasks/biocreative-v/track-3-cdr/)
 * [Semeval 2013 - taak 9.1 (medicijn Recognition)](https://www.cs.york.ac.uk/semeval-2013/task9/)

## <a name="link-to-the-azure-gallery-github-repository"></a>Koppeling naar de GitHub-opslagplaats voor Azure-galerie
Hieronder vindt u de koppeling naar de openbare GitHub-opslagplaats van het werkelijke scenario met de code en gedetailleerde beschrijving: 

[https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction)


## <a name="prerequisites"></a>Vereisten 

* Een Azure [abonnement](https://azure.microsoft.com/free/)
* Azure Machine Learning-Workbench. Zie [installatiehandleiding](quickstart-installation.md). De Azure Machine Learning-Workbench kan op dit moment worden geïnstalleerd op alleen de volgende besturingssystemen: 
    * Windows 10 of WindowsServer 2016
    * Mac OS Sierra (of nieuwer)


### <a name="azure-services"></a>Azure-services
* [HDInsight Spark-cluster](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-jupyter-spark-sql) versie 2.1 van Spark op Linux (HDI 3.6) voor uitbreidbare berekeningen. Voor het verwerken van de volledige hoeveelheid MEDLINE samenvattingen hieronder besproken, moet u de minimale configuratie van: 
    * Hoofdknooppunt: [D13_V2](https://azure.microsoft.com/pricing/details/hdinsight/) grootte
    * Worker-knooppunten: ten minste 4 van [D12_V2](https://azure.microsoft.com/pricing/details/hdinsight/). In ons werk hebben we 11 worker-knooppunten met een grootte van D12_V2 gebruikt.
* [NC6 gegevens wetenschappelijke virtuele Machine (DSVM)](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-linux-dsvm-intro) voor scale-up berekeningen.

### <a name="python-packages"></a>Python-pakketten

De vereiste afhankelijkheden zijn gedefinieerd in het bestand aml_config/conda_dependencies.yml in de projectmap scenario. De afhankelijkheden die zijn gedefinieerd in dit bestand automatisch worden ingericht voor uitgevoerd tegen docker, VM en HDI-cluster doelen. Raadpleeg voor meer informatie over de bestandsindeling van Conda omgeving [hier](https://conda.io/docs/using/envs.html#create-environment-file-by-hand).

* [TensorFlow](https://www.tensorflow.org/install/)
* [CNTK 2.0](https://docs.microsoft.com/cognitive-toolkit/using-cntk-with-keras)
* [Keras](https://keras.io/#installation)
* NLTK
* Fastparquet

### <a name="basic-instructions-for-azure-machine-learning-aml-workbench"></a>Basic-instructies voor Azure Machine Learning (AML) workbench
* [Overzicht](overview-what-is-azure-ml.md)
* [Installatie](quickstart-installation.md)
* [Met behulp van TDSP](how-to-use-tdsp-in-azure-ml.md)
* [Hoe bestanden lezen en schrijven](how-to-read-write-files.md)
* [Het gebruik van Jupyter-Notebooks](how-to-use-jupyter-notebooks.md)
* [Het gebruik van GPU](how-to-use-gpu.md)

## <a name="scenario-structure"></a>Scenario-structuur
Voor het scenario, gebruiken we de TDSP structuur en documentatie projectsjablonen (Zie afbeelding 1), dat volgt op de [TDSP lifecycle](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md). Project is gemaakt op basis van instructies [hier](./how-to-use-tdsp-in-azure-ml.md).


![Projectgegevens in te vullen](./media/scenario-tdsp-biomedical-recognition/instantiation-3.png) 

De stapsgewijze wetenschappelijke werkstroom is als volgt:

### <a name="1-data-acquisition-and-understanding"></a>1. Gegevens verzamelen en begrijpen

Zie [gegevens overname en inzicht](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/blob/master/code/01_data_acquisition_and_understanding/ReadMe.md).

De onbewerkte MEDLINE corpus heeft een totaal van 27 miljoen samenvattingen waarin ongeveer 10 miljoen artikelen een leeg abstracte veld hebben. Azure HDInsight Spark is gebruikt voor het verwerken van big data dat kan niet worden geladen in het geheugen van een enkele computer als een [Pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html). De gegevens eerst gedownload naar het Spark-cluster. Klik in de volgende stappen worden uitgevoerd op de [Spark DataFrame](https://spark.apache.org/docs/latest/sql-programming-guide.html): 
* de XML-bestanden met behulp van de XML-Parser Medline parseren
* de abstracte tekst waaronder zin splitsen, tokeniseren en case normalisatie voorverwerken.
* artikelen waar abstracte veld is leeg of korte tekst heeft uitsluiten 
* de woordenlijst word maken vanuit de training samenvattingen
* het woord insluiten neural model trainen. Raadpleeg voor meer informatie [GitHub code koppeling](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/blob/master/code/01_data_acquisition_and_understanding/ReadMe.md) aan de slag.


Na het parseren van XML-bestanden, heeft gegevens de volgende indeling: 

![Steekproef](./media/scenario-tdsp-biomedical-recognition/datasample.png)

Het uitpakken van neural entiteitsmodel is getraind en geëvalueerd op publiclly beschikbare gegevenssets. Als u een gedetailleerde beschrijving van deze gegevenssets, kan u de volgende bronnen raadplegen:
 * [Biografie entiteit erkenning taak BioNLP/NLPBA 2004](http://www.nactem.ac.uk/tsujii/GENIA/ERtask/report.html)
 * [BioCreative V CDR taak corpus](http://www.biocreative.org/tasks/biocreative-v/track-3-cdr/)
 * [Semeval 2013 - taak 9.1 (medicijn Recognition)](https://www.cs.york.ac.uk/semeval-2013/task9/)

### <a name="2-modeling"></a>2. Modelleren

Zie [modelleren](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling).

Modellering is de fase waar we zien hoe u gebruik van de gegevens in de vorige sectie uw eigen word insluiten van het model te trainen gedownload en deze gebruiken voor andere downstream taken. Hoewel we de gegevens PubMed gebruiken, wordt de pijplijn voor het genereren van de insluitingen is algemeen en kan worden gebruikt voor het trainen van word insluitingen voor een ander domein. Voor insluitingen moet een nauwkeurige weergave van de gegevens, is het essentieel dat de word2vec op een grote hoeveelheid gegevens wordt getraind.
Wanneer we de word-insluitingen gereed hebt, wij kunt diep neurale netwerk wordt het model trainen aan die gebruikmaakt van de geleerde insluitingen initialiseren van de laag insluiten. We de insluiten laag als niet-trainable markeren, maar dat is niet verplicht. De training van het woord insluiten model bevindt zich niet onder supervisie en de daarmee kunnen profiteren van zonder label teksten. De training van het model van de erkenning entiteit is een taak leren met supervisie echter en de nauwkeurigheid is afhankelijk van de hoeveelheid en de kwaliteit van een data handmatig aangetekend. 


#### <a name="21-feature-generation"></a>2.1. Functie generatie

Zie [generatie functie](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling/01_feature_engineering).

Word2Vec is het woord zonder supervisie learning-algoritme dat een neurale netwerk wordt getraind model uit een corpus zonder label training traint insluiten. Het genereert een continue vector voor elk woord in de corpus die de semantische gegevens vertegenwoordigt. Deze modellen zijn eenvoudige neural netwerken met één laag verborgen. Het woord vectoren/insluitingen worden geleerd door backpropagation en stochastische kleurovergang daalgradiënt. Er zijn twee soorten word2vec modellen, dat wil zeggen de Skip-Gram en het doorlopende-eigenschappenverzameling-van-woorden (controleren [hier](https://arxiv.org/pdf/1301.3781.pdf) voor meer informatie). We de MLlib uitvoering van de word2vec die ondersteuning biedt voor het model Skip-gram, gebruiken we een korte beschrijving van deze hier (genomen van de installatiekopie [koppeling](https://ahmedhanibrahim.wordpress.com/2017/04/25/thesis-tutorials-i-understanding-word2vec-for-word-embedding-i/)): 

![SKIP-Gram-Model](./media/scenario-tdsp-biomedical-recognition/skip-gram.png)

Het model maakt gebruik van hiërarchische Softmax en negatieve steekproeven om de prestaties te optimaliseren. Hiërarchische SoftMax (H SoftMax) is een benadering die is geïnspireerd op binaire structuren. De platte SoftMax laag vervangt H SoftMax in wezen door een hiërarchische laag waarvoor de woorden is als verlaat. Deze manier kunnen wij om af te breken berekenen van de waarschijnlijkheid van één woord in een reeks kans berekeningen, waarin wordt ons opgeslagen bij de berekening van de dure normalisering voor alle woorden opheft. Aangezien een evenwichtige binaire-structuur een diepte van log2 heeft (| V |) (V is de woordenlijst), moeten we alleen evalueren maximaal log2 (| V |) de knooppunten om op te halen van de laatste kans op een woord. De kans op een word-w gegeven van de context-c is dan gewoon het product van de waarschijnlijkheid van de juiste duurt en links schakelt respectievelijk die leiden tot het leafknooppunt. We kunt een Huffman-boomstructuur op basis van de frequentie van de woorden in de gegevensset om ervoor te zorgen dat vaker woorden korter verklaringen krijgen maken. Raadpleeg voor meer informatie [deze koppeling](http://sebastianruder.com/word-embeddings-softmax/).
Afbeelding die afkomstig zijn uit [hier](https://ahmedhanibrahim.wordpress.com/2017/04/25/thesis-tutorials-i-understanding-word2vec-for-word-embedding-i/).

##### <a name="visualization"></a>Visualisatie

Wanneer de word-insluitingen we hebben, willen we ze visualiseren en de relatie tussen semantisch vergelijkbare woorden zien. 

![W2V overeenkomsten](./media/scenario-tdsp-biomedical-recognition/w2v-sim.png)

We hebben twee verschillende manieren visualiseren de insluitingen weergegeven. Het eerste beheerpunt maakt gebruik van een PCA de hoge dimensionale vector met een 2D-vector ruimte projecteren. Dit leidt tot een aanzienlijk verlies van gegevens en de visualisatie is niet zo nauwkeurig. De tweede is het gebruik van Pso met [t GND](https://distill.pub/2016/misread-tsne/). t GND is een niet-lineaire dimensionaliteit vermindering techniek die geschikt zijn voor het insluiten van hoge-dimensionale gegevens in een ruimte van twee of drie dimensies, die vervolgens spreidingsgrafiek tekent kunnen worden weergegeven.  Deze modellen elk hoge dimensionale object door twee of three dimensional punt in zodanig dat vergelijkbare objecten zijn gemodelleerd door in de buurt punten en verschillende objecten zijn gemodelleerd door verafgelegen punten. Dit werkt in twee delen. Eerst zorgt voor een kansverdeling via de paren in de hogere dimensionale ruimte op een manier die vergelijkbare objecten hebben een hoge mate van waarschijnlijkheid is bezig en verschillende punten hebben lage kans ophalen verzameld. Vervolgens definieert een vergelijkbare kansverdeling via de punten van een lage dimensionale kaart en minimaliseert de divergentie KL tussen de twee distributies met betrekking tot de locatie van punten op de kaart. De locatie van de punten van de lage dimensie wordt verkregen door de KL divergentie met behulp van de kleurovergang Daalgradiënt minimaliseren. Maar t GND mogelijk niet altijd betrouwbare. Implementatie-informatie vindt u [hier](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling/01_feature_engineering). 


Zoals u in de volgende afbeelding, biedt de visualisatie t GND meer scheiding van word vectoren en potentiële clustering patronen. 


* Visualisatie met PCA

![PCA](./media/scenario-tdsp-biomedical-recognition/pca.png)

* Visualisatie met t-GND

![t GND](./media/scenario-tdsp-biomedical-recognition/tsne.png)

* Punten die het dichtst bij 'Kanker' (ze zijn alle subtypen van kanker)

![Punten die het dichtst bij kanker](./media/scenario-tdsp-biomedical-recognition/nearesttocancer.png)

#### <a name="22-train-the-neural-entity-extractor"></a>2.2. De entiteit neural zelfstandig uitpakken trainen

Zie [trainen van de entiteit neural zelfstandig uitpakken](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling/02_model_creation/ReadMe.md).

De feed doorsturen neural netwerkarchitectuur lijden onder blokkering van een probleem dat ze elke invoer behandelen en uitvoer als onafhankelijk van de andere invoer en uitvoer. Deze architectuur kan geen model sequence naar sequence labeling taken zoals vertalen en uitpakken van de entiteit. Modellen terugkerende neurale netwerk wordt oplossen dit probleem als kunnen informatie berekend totdat nu naar het volgende knooppunt doorgeven. Deze eigenschap wordt aangeroepen met geheugen in het netwerk omdat het gebruiken van de eerder berekende informatie zoals weergegeven in de volgende afbeelding:

![RNN](./media/scenario-tdsp-biomedical-recognition/rnn-expanded.png)

Vanilla RNNs daadwerkelijk lijden onder blokkering van de [kleurovergang probleem verdwijnen](https://en.wikipedia.org/wiki/Vanishing_gradient_problem) vanwege die ze geen gebruikmaken van alle gegevens die ze kunnen eerder hebt gezien. Het probleem wordt duidelijk alleen wanneer een grote hoeveelheid context is vereist om een voorspelling te maken. Maar modellen zoals LSTM doen niet te lijden onder blokkering van een dergelijk probleem, in feite ze zijn ontworpen om te onthouden op lange termijn afhankelijkheden. In tegenstelling tot vanille RNNs waarvoor een netwerk met één neural, hebben de LSTMs de interacties tussen vier neural networks voor elke cel. Raadpleeg voor een gedetailleerde uitleg over de werking van LSTM [deze post](http://colah.github.io/posts/2015-08-Understanding-LSTMs/).

![LSTM cel](./media/scenario-tdsp-biomedical-recognition/lstm-cell.png)

Laten we onze eigen LSTM netwerk op basis van periodieke neural samenstellen en probeer het Entiteitstypen zoals medicijn uitpakken, noemt en van symptoom van PubMed gegevens. De eerste stap is het verkrijgen van een grote hoeveelheid gelabelde gegevens en zoals u zou hebben geraden, die niet eenvoudig! De meeste van de medische gegevens bevat veel gevoelige informatie over de persoon en daarom niet openbaar beschikbaar zijn. We zijn afhankelijk van een combinatie van twee verschillende gegevenssets die openbaar beschikbaar zijn. De eerste gegevensset van Semeval 2013 - taak 9.1 (Recognition van) en de andere is uit BioCreative V CDR-taak. We zijn combineren en labels van deze twee gegevenssets zodat we kunnen zowel middelen en worden toegekend van medische teksten detecteren en evalueren van onze insluitingen woord auto. Raadpleeg voor meer informatie voor implementatie, [GitHub code koppeling](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling/02_model_creation).

De model-architectuur die wordt gebruikt tussen de codes en voor vergelijking wordt hieronder weergegeven. De parameter die wordt gewijzigd voor verschillende datasets de maximale reekslengte (hier 613 is).

![LSTM model](./media/scenario-tdsp-biomedical-recognition/d-a-d-model.png)

#### <a name="23-model-evaluation"></a>2.3. Model-evaluatie
Zie [Model evaluatie](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling/03_model_evaluation/ReadMe.md).

We gebruiken de evaluatie-script uit de gedeelde taak [biografie entiteit erkenning taak biografie NLP/NLPBA 2004](http://www.nactem.ac.uk/tsujii/GENIA/ERtask/report.html) evalueren de precisie, intrekken en F1 score van het model. 

#### <a name="in-domain-versus-generic-word-embedding-models"></a>In het domein versus algemene word modellen insluiten

Hieronder volgt een vergelijking tussen de nauwkeurigheid van de twee typen van de functie: (1) word insluitingen getraind op PubMed samenvattingen en (2) word insluitingen op Google nieuws getraind. Duidelijk ziet u dat het model in het domein het algemene model presteert. Daarom is een bepaald woord model insluiten in plaats van een algemene is veel meer nuttig. 

* Taak #1: De middelen en ziekten detectie

![Vergelijking van model 1](./media/scenario-tdsp-biomedical-recognition/mc1.png)

We de evaluatie van de word-insluitingen uitvoeren op andere gegevenssets op soortgelijke wijze en Zie dat model in het domein is altijd beter.

* Taak #2: Eiwitten, cel regel, celtype, DNA en RNA detectie

![Vergelijking model 2](./media/scenario-tdsp-biomedical-recognition/mc2.png)

* Taak #3: Chemicaliën en ziekten detectie

![Vergelijking model 3](./media/scenario-tdsp-biomedical-recognition/mc3.png)

* Taak &#4;: Middelen detectie

![Vergelijking van model 4](./media/scenario-tdsp-biomedical-recognition/mc4.png)

* Taak &#5;: Genen detectie

![Vergelijking model 5](./media/scenario-tdsp-biomedical-recognition/mc5.png)

#### <a name="tensorflow-versus-cntk"></a>TensorFlow versus CNTK
Het gerapporteerde model zijn getraind met Keras TensorFlow als back-end. Keras met CNTK back-end biedt geen ondersteuning voor 'omgekeerde' op het moment dat deze taak is uitgevoerd. Daarom voor vergelijking, hebben we een unidirectionele LSTM model met de back-end CNTK getraind en ten opzichte van een unidirectionele LSTM model met TensorFlow back-end. Installeer CNTK 2.0 voor Keras van [hier](https://docs.microsoft.com/cognitive-toolkit/using-cntk-with-keras). 

![Vergelijking model 6](./media/scenario-tdsp-biomedical-recognition/mc6.png)

We gesloten CNTK beleidregels goed als Tensorflow beide in termen van de trainingstijd per epoche (60 seconden voor CNTK en 75 sec. voor Tensorflow) en het aantal test entiteiten die zijn gedetecteerd. We gebruiken de Unidirectioneel lagen voor evaluatie.


### <a name="3-deployment"></a>3. Implementatie

Zie [implementatie](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/03_deployment).

We hebben geïmplementeerd een webservice op een cluster in de [Azure Container Service (ACS)](https://azure.microsoft.com/services/container-service/). De omgeving uitoefening bepalingen Docker en Kubernetes in het cluster voor het beheren van de web service-implementatie. U vindt meer informatie over het proces uitoefening [hier](model-management-service-deploy.md ).


## <a name="conclusion"></a>Conclusie

Er is een fout over de details van hoe u kan een word-model insluiten met Word2Vec algoritme op Spark trainen en vervolgens de uitgepakte insluitingen gebruiken als functies voor het trainen van een diep neurale netwerk voor extractie entiteit. We hebben de pijplijn training toegepast op het biomedische domein. De pijplijn is echter algemene moet worden toegepast om te detecteren typen aangepaste entiteit van een ander domein. Hoeft u alleen voldoende gegevens en kunt u de werkstroom die hier wordt gepresenteerd voor een ander domein gemakkelijk aanpassen.

## <a name="references"></a>Verwijzingen

* Tomas Mikolov, Kai Chen, Greg Corrado en Jeffrey Dean. 2013a. Efficiënte schatting van word verklaringen in vector ruimte. In de procedures van ICLR.
* Tomas Mikolov, Ilya Sutskever Kai Chen, Greg S Corrado en Jeff Dean. 2013b. Gedistribueerde weergaven van woorden en woordgroepen en hun compositionality. Pagina in de procedure van NIPS 3111 – 3119.
* Billy Chiu, Gamal Crichton, Anna Korhonen en Sampo Pyysalo. 2016. [Hoe goed Word insluitingen voor biomedische NLP Train](http://aclweb.org/anthology/W/W16/W16-2922.pdf), 166 – 174-pagina's In procedures van de 15 Workshop op biomedische verwerking van natuurlijke taal.
* [Vector representaties van woorden](https://www.tensorflow.org/tutorials/word2vec)
* [Terugkerende Neural Networks](https://www.tensorflow.org/tutorials/recurrent)
* [Problemen met Spark ml Word2Vec](https://intothedepthsofdataengineering.wordpress.com/2017/06/26/problems-encountered-with-spark-ml-word2vec/)
* [Spark Word2Vec: uitkomsten](https://intothedepthsofdataengineering.wordpress.com/2017/06/26/spark-word2vec-lessons-learned/)

