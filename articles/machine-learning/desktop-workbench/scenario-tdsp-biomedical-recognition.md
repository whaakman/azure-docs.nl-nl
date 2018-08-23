---
title: Herkenning biomedische entiteit - Team Data Science Process - Azure Machine Learning | Microsoft Docs
description: Een Team Data Science Process project-snelstartgids die gebruikmaakt van diepteleren voor herkenning biomedische entiteit in Azure Machine Learning Workbench.
services: machine-learning
documentationcenter: ''
author: deguhath
ms.author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2017
ms.openlocfilehash: f6ce43c2d290bacee10e102cc6c382981db9917f
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2018
ms.locfileid: "42058136"
---
# <a name="biomedical-entity-recognition-using-team-data-science-process-tdsp-template"></a>Herkenning biomedische entiteit met behulp van Team Data Science Process (TDSP)-sjabloon

Uitpakken van de entiteit is een subtaak van informatie-extractie (ook wel bekend als [herkenning van Named-entiteit (NER)](https://en.wikipedia.org/wiki/Named-entity_recognition), entiteit logische groepen te verdelen en de entiteits-id). Het doel van dit Praktijkscenario is te laten zien hoe u Azure Machine Learning Workbench gebruiken bij het oplossen van een complexe taak van natuurlijke taal verwerken (NLP) zoals entiteiten extraheren uit ongestructureerde tekst:

1. Hoe u met het trainen van een neural woord insluitingen model op een corpus tekst van ongeveer 18 miljoen PubMed samenvattingen met behulp van [Spark Word2Vec implementatie](https://spark.apache.org/docs/latest/mllib-feature-extraction.html#word2vec).
2. Over het bouwen van een grondige lange termijn geheugen (LSTM) terugkerende neural network-model voor het ophalen van de entiteit op een GPU ingeschakeld Azure Data Science Virtual Machine (DS-GPU VM) op Azure.
2. Laten zien dat domeinspecifieke word insluitingen-model kunt leveren betere prestaties dan algemene word insluitingen modellen in de entiteit opname-taak. 
3. Laten zien hoe u trainen en deep learning-modellen met behulp van Azure Machine Learning Workbench.

4. De volgende mogelijkheden in Azure Machine Learning Workbench demonstreren:

    * Instantie van [Team Data Science Process (TDSP) structuur en sjablonen](how-to-use-tdsp-in-azure-ml.md)
    * Geautomatiseerd beheer van de afhankelijkheden van uw project met inbegrip van het downloaden en installeren
    * Uitvoering van de Python-scripts op verschillende rekenomgevingen
    * Geschiedenis bijhouden voor Python-scripts uitvoeren
    * Uitvoering van taken op externe Spark compute-context met HDInsight Spark 2.1-clusters
    * Uitvoering van taken in externe GPU-VM's in Azure
    * Eenvoudig uitoefening van deep learning-modellen als webservices op Azure Container Services (ACS)

## <a name="use-case-overview"></a>Gebruik case-overzicht
Herkenning biomedische named entity's is een belangrijke stap voor complexe biomedische NLP-taken, zoals: 
* De vermeldingen van benoemde entiteiten extraheren dergelijke ziekten, drugs, chemisch en symptomen uit elektronische medische of de gezondheid van records.
* Drug detectie
* Informatie over de interactie tussen verschillende entiteit van het type zoals drug drug interactie, drug ziekten relatie en gen eiwitten relatie.

Onze use-casescenario is gericht op hoe een grote hoeveelheid niet-gestructureerde gegevens corpus zoals Medline PubMed samenvattingen trainen van een woord insluiten model kan worden geanalyseerd. Vervolgens de uitvoer-insluitingen beschouwd als de automatisch gegenereerde functies met het trainen van een neural entiteit-extractor.

Onze resultaten staat dat de training biomedische entiteit extractie model op de domeinspecifieke woord functies voor het insluiten van inhoud voor het model is getraind op de algemene functietype presteert. Het model domeinspecifieke 7012 entiteiten correct (van 9475) kan worden gedetecteerd met F1-score van 0,73 vergeleken met 5274 entiteiten met F1-score van 0,61 voor het algemene model.

De volgende afbeelding toont de architectuur die is gebruikt voor het verwerken van gegevens en het trainen van modellen.

![Architectuur](./media/scenario-tdsp-biomedical-recognition/architecture.png)

## <a name="data-description"></a>Beschrijving van de gegevens

### <a name="1-word2vec-model-training-data"></a>1. Word2Vec model trainen van gegevens
We eerst de onbewerkte MEDLINE abstracte gegevens uit gedownloade [MEDLINE](https://www.nlm.nih.gov/pubs/factsheets/medline.html). De gegevens zijn openbaar beschikbaar in de vorm van XML-bestanden op hun [FTP-server](https://ftp.ncbi.nlm.nih.gov/pubmed/baseline). Er zijn 892 XML-bestanden beschikbaar op de server en bevat de informatie 30.000 artikelen over elk van de XML-bestanden. Meer informatie over de stap van de verzameling gegevens vindt u in de sectie projectstructuur. De velden die aanwezig zijn in elk bestand zijn 
        
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

### <a name="2-lstm-model-training-data"></a>2. LSTM model trainen van gegevens

Het neurale entiteit extractie-model is getraind en geëvalueerd op openbaar beschikbare gegevenssets. Als u een gedetailleerde beschrijving over deze gegevenssets, kan u de volgende bronnen raadplegen:
 * [Biografie van entiteit erkenning taak BioNLP/NLPBA 2004](http://www.nactem.ac.uk/tsujii/GENIA/ERtask/report.html)
 * [BioCreative V CDR taak corpus](http://www.biocreative.org/tasks/biocreative-v/track-3-cdr/)
 * [Semeval 2013 - taak 9.1 (Drug Recognition)](https://www.cs.york.ac.uk/semeval-2013/task9/)

## <a name="link-to-the-azure-gallery-github-repository"></a>Koppeling naar de GitHub-opslagplaats voor Azure-galerie
Hieronder ziet u de koppeling naar de openbare GitHub-opslagplaats van de real-world scenario met de code en gedetailleerdere beschrijving: 

[https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction)


## <a name="prerequisites"></a>Vereisten 

* Een Azure [abonnement](https://azure.microsoft.com/free/)
* Azure Machine Learning Workbench. Zie [installatiehandleiding](../service/quickstart-installation.md). De Azure Machine Learning Workbench kunt op dit moment worden geïnstalleerd op alleen de volgende besturingssystemen: 
    * Windows 10 of Windows Server 2016
    * macOS Sierra (of nieuwer)


### <a name="azure-services"></a>Azure-services
* [HDInsight Spark-cluster](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-jupyter-spark-sql) versie 2.1 van Spark in Linux (HDI 3.6) voor de berekening van de scale-out. Voor het verwerken van het volledige bedrag van MEDLINE samenvattingen hierna besproken, moet u de minimale configuratie van: 
    * Hoofdknooppunt: [D13_V2](https://azure.microsoft.com/pricing/details/hdinsight/) grootte
    * Worker-knooppunten: ten minste 4 van [D12_V2](https://azure.microsoft.com/pricing/details/hdinsight/). In ons werk, hebben we 11 worker-knooppunten van de grootte van de D12_V2 gebruikt.
* [NC6 Data Science Virtual Machine (DSVM)](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-linux-dsvm-intro) voor omhoog berekeningen.

### <a name="python-packages"></a>Python-pakketten

De vereiste afhankelijkheden zijn gedefinieerd in het bestand aml_config/conda_dependencies.yml onder de map met het scenario. De afhankelijkheden die zijn gedefinieerd in dit bestand worden automatisch ingericht voor worden uitgevoerd op basis van docker, virtuele machine en HDI-cluster doelen. Raadpleeg voor meer informatie over de bestandsindeling van het Conda-omgeving [hier](https://conda.io/docs/using/envs.html#create-environment-file-by-hand).

* [TensorFlow](https://www.tensorflow.org/install/)
* [CNTK 2.0](https://docs.microsoft.com/cognitive-toolkit/using-cntk-with-keras)
* [Keras](https://keras.io/#installation)
* NLTK
* Fastparquet

### <a name="basic-instructions-for-azure-machine-learning-aml-workbench"></a>Algemene instructies voor Azure Machine Learning (AML) workbench
* [Overzicht](../service/overview-what-is-azure-ml.md)
* [Installatie](../service/quickstart-installation.md)
* [TDSP gebruiken](how-to-use-tdsp-in-azure-ml.md)
* [Hoe u bestanden lezen en schrijven](how-to-read-write-files.md)
* [Jupyter-Notebooks gebruiken](how-to-use-jupyter-notebooks.md)
* [Het gebruik van GPU](how-to-use-gpu.md)

## <a name="scenario-structure"></a>Scenario-structuur
Voor het scenario gebruiken we de TDSP-structuur en documentatie projectsjablonen (afbeelding 1), waarbij de [TDSP-levenscyclus](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md). Project is gemaakt op basis van de instructies hiervoor vindt u [hier](./how-to-use-tdsp-in-azure-ml.md).


![Projectgegevens in te vullen](./media/scenario-tdsp-biomedical-recognition/instantiation-3.png) 

De stapsgewijze gegevenswetenschapwerkstroom is als volgt:

### <a name="1-data-acquisition-and-understanding"></a>1. Gegevens verzamelen en begrijpen

Zie [gegevens verzamelen en begrijpen](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/blob/master/code/01_data_acquisition_and_understanding/ReadMe.md).

De onbewerkte MEDLINE corpus heeft een totaal van 27 miljoen samenvattingen waar ongeveer 10 miljoen artikelen abstracte veld leeg zijn. Azure HDInsight Spark wordt gebruikt voor het verwerken van big data die kan niet worden geladen in het geheugen van een enkele machine als een [Pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html). De gegevens wordt eerst gedownload naar het Spark-cluster. Klik in de volgende stappen worden uitgevoerd op de [Spark DataFrame](https://spark.apache.org/docs/latest/sql-programming-guide.html): 
* de XML-bestanden met behulp van de XML-Parser Medline parseren
* de abstracte tekst inclusief zin splitsen, tokeniseren en case normalisering voorverwerken.
* artikelen waar abstracte veld is leeg of bevat korte tekst uitsluiten 
* het woord vocabulaire van de training samenvattingen maken
* het woord insluiten neurale model trainen. Zie voor meer informatie, [link naar GitHub-code](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/blob/master/code/01_data_acquisition_and_understanding/ReadMe.md) aan de slag.


Na het parseren van XML-bestanden, heeft gegevens de volgende indeling: 

![Voorbeeld van gegevens](./media/scenario-tdsp-biomedical-recognition/datasample.png)

Het neurale entiteit extractie-model is getraind en geëvalueerd op openbaar beschikbare gegevenssets. Als u een gedetailleerde beschrijving over deze gegevenssets, kan u de volgende bronnen raadplegen:
 * [Biografie van entiteit erkenning taak BioNLP/NLPBA 2004](http://www.nactem.ac.uk/tsujii/GENIA/ERtask/report.html)
 * [BioCreative V CDR taak corpus](http://www.biocreative.org/tasks/biocreative-v/track-3-cdr/)
 * [Semeval 2013 - taak 9.1 (Drug Recognition)](https://www.cs.york.ac.uk/semeval-2013/task9/)

### <a name="2-modeling"></a>2. Modelleren

Zie [modelleren](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling).

Modellering is de fase waarin we laten zien hoe u kunt gebruikmaken van de gegevens in de vorige sectie voor het trainen van uw eigen word insluiten model hebt gedownload en deze gebruiken voor andere downstream-taken. Hoewel we de gegevens PubMed gebruiken, wordt de pijplijn voor het genereren van de insluitingen is algemeen en opnieuw trainen van woordinsluitingen voor een ander domein kan worden gebruikt. Voor insluitingen moet een nauwkeurige weergave van de gegevens, is het essentieel dat de word2vec op een grote hoeveelheid gegevens wordt getraind.
Zodra we de woordinsluitingen gereed hebben, kunnen we een deep neural network-model dat u de geleerde insluitingen gebruikt om te initialiseren van de laag Embedding trainen. We de insluiten laag als niet-trainable markeren, maar dat is niet verplicht. De training van het woord model insluiten zonder supervisie is en die dus we kunnen profiteren van niet-gelabelde teksten. De training van het model van de spraakherkenning entiteit is een taak leren met supervisie echter en de nauwkeurigheid is afhankelijk van de hoeveelheid en de kwaliteit van een handmatig aantekeningen. 


#### <a name="21-feature-generation"></a>2.1. Functie genereren

Zie [generatie functie](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling/01_feature_engineering).

Word2Vec is het woord insluiten zonder supervisie learning-algoritme dat een neuraal netwerk model vanuit een niet-gelabelde training corpus traint. Het genereert een continue vector voor elk woord in het corpus dat de semantische gegevens vertegenwoordigt. Deze modellen zijn eenvoudige neurale netwerken met een enkele verborgen laag. Vectoren-/ woordinsluitingen worden geleerd door backpropagation en stochastische leren met stochastische gradiëntdaling. Er zijn twee soorten word2vec modellen, namelijk de Skip-Gram en de continue-bag-van-woorden (controleren [hier](https://arxiv.org/pdf/1301.3781.pdf) voor meer informatie). Omdat we van de MLlib uitvoering van de word2vec, die ondersteuning biedt voor het overslaan-gram-model, gebruiken we een korte beschrijving van het hier (afbeelding die is overgenomen uit [koppeling](https://ahmedhanibrahim.wordpress.com/2017/04/25/thesis-tutorials-i-understanding-word2vec-for-word-embedding-i/)): 

![Model voor Gram overslaan](./media/scenario-tdsp-biomedical-recognition/skip-gram.png)

Het model worden hiërarchische Softmax en negatieve steekproeven gebruikt om de prestaties te optimaliseren. Hiërarchische SoftMax (H-SoftMax) is een benadering geïnspireerd op binaire structuren. De platte SoftMax laag vervangt H SoftMax in feite door een hiërarchische laag waarvoor de woorden die als verlaat. Dit kan we berekenen van de kans van één woord in een reeks kans berekeningen, Hiermee slaat u ons niet hoeft te berekenen van de duur normalisering via alle woorden ontleden. Aangezien een evenwichtige binaire-structuur een diepte van log2 heeft (| V |) (V is het vocabulaire), we hebben alleen nodig om te evalueren maximaal log2 (| V |) knooppunten om op te halen van de laatste kans op een woord. De kans op een word-w gegeven van de context-c is dan gewoon het product van de waarschijnlijkheid van de juiste nemen en links schakelt respectievelijk die leiden tot de leaf-knooppunt. We bouwen een Huffman-boomstructuur op basis van de frequentie van de woorden in de gegevensset om ervoor te zorgen dat meer frequente woorden kortere voorstellingen krijgen. Zie voor meer informatie, [deze koppeling](http://sebastianruder.com/word-embeddings-softmax/).
Afbeelding die is overgenomen uit [hier](https://ahmedhanibrahim.wordpress.com/2017/04/25/thesis-tutorials-i-understanding-word2vec-for-word-embedding-i/).

##### <a name="visualization"></a>Visualisatie

Zodra we de woordinsluitingen hebben, willen we ze visualiseren en de relatie tussen semantisch gelijksoortige woorden zien. 

![W2V gelijkenis](./media/scenario-tdsp-biomedical-recognition/w2v-sim.png)

We hebben twee verschillende manieren visualiseren van de insluitingen weergegeven. Het eerste item maakt gebruik van een PCA aan het project de hoge dimensionale vector naar een vector 2D-ruimte. Dit leidt tot een aanzienlijk verlies van gegevens en de visualisatie is niet zo nauwkeurig. De tweede is het gebruik van Pso met [t GND](https://distill.pub/2016/misread-tsne/). t-GND is een techniek voor het verminderen van niet-lineaire dimensionaliteit die geschikt zijn voor het insluiten van high-dimensionale gegevens in een ruimte van twee of drie dimensies, die vervolgens kunnen worden gevisualiseerd in een spreidingsdiagram.  Dit bepaalt de elke high-dimensionale object door een dubbele of three dimensional punt op een manier die vergelijkbare objecten zijn gemodelleerd op basis van punten in de buurt en verschillende objecten zijn gemodelleerd op basis van verafgelegen punten. Het werkt in twee delen. Eerst een waarschijnlijkheidsverdeling wordt gemaakt via de paren in de hogere dimensionale ruimte op een manier die vergelijkbare objecten hebben een hoge waarschijnlijkheid wordt verzameld en verschillende punten hebben lage kans van het ophalen van opgehaald. Vervolgens definieert een vergelijkbare waarschijnlijkheidsverdeling over de punten in een laag dimensionale kaart en minimaliseert de KL verschillen tussen de twee distributies met betrekking tot de locatie van punten op de kaart. De locatie van de punten in de laag dimensie wordt verkregen door het minimaliseren van de KL verschillen met behulp van leren met stochastische Gradiëntdaling. Maar t GND mogelijk niet altijd betrouwbare. Implementatiegegevens vindt [hier](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling/01_feature_engineering). 


Zoals in de volgende afbeelding wordt weergegeven, biedt de visualisatie t GND meer scheiding van word vectoren en potentiële clustering patronen. 


* Visualisatie met PCA

![PCA](./media/scenario-tdsp-biomedical-recognition/pca.png)

* Visualisatie met t-GND

![t-SNE](./media/scenario-tdsp-biomedical-recognition/tsne.png)

* Punten die het dichtst bij 'Kanker' (ze zijn alle subtypen van kanker)

![Punten die het dichtst bij kanker](./media/scenario-tdsp-biomedical-recognition/nearesttocancer.png)

#### <a name="22-train-the-neural-entity-extractor"></a>2.2. Het neurale entiteit-extractor trainen

Zie [trainen de extractor neurale entiteit](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling/02_model_creation/ReadMe.md).

De architectuur van de feed-forward neural network lijden heeft een probleem dat ze behandelen van elke invoer en uitvoer als onafhankelijk van de andere invoer en uitvoer. Deze architectuur kan geen reeks-naar-sequence labelen taken, zoals automatische vertalingen en entiteiten extraheren model. Terugkerende neural network-modellen oplossen dit probleem als ze informatie tot nu naar het volgende knooppunt berekend kunnen doorgeven. Deze eigenschap wordt aangeroepen met geheugen in het netwerk omdat het de informatie die eerder berekende gebruiken zoals wordt weergegeven in de volgende afbeelding:

![RNN](./media/scenario-tdsp-biomedical-recognition/rnn-expanded.png)

Vanilla rnn's werkelijk ten koste gaan van de [kleurovergang probleem verdwijnen](https://en.wikipedia.org/wiki/Vanishing_gradient_problem) vanwege die ze geen gebruikmaken van alle gegevens die ze kunnen eerder hebt gezien. Het probleem wordt duidelijk alleen wanneer een grote hoeveelheid context is vereist om een voorspelling te maken. Maar modellen, zoals LSTM doen niet te maken krijgen met een probleem optreedt, wordt in feite ze zijn ontworpen om te onthouden op lange termijn afhankelijkheden. In tegenstelling tot vanille rnn's waarvoor een enkele neural network, hebben de lstm's de interactie tussen de vier neurale netwerken voor elke cel. Raadpleeg voor een gedetailleerde uitleg over de werking van LSTM [dit bericht](http://colah.github.io/posts/2015-08-Understanding-LSTMs/).

![LSTM cel](./media/scenario-tdsp-biomedical-recognition/lstm-cell.png)

Laten we voor het samenstellen van onze eigen LSTM op basis van periodieke neurale netwerk en probeer Entiteitstypen, zoals drug, ziekten en symptoom vermeldingen ophalen uit PubMed gegevens. De eerste stap is het verkrijgen van een grote hoeveelheid gelabelde gegevens en zoals u zou hebben geraden, dat is niet eenvoudig. De meeste van de medische gegevens bevat veel gevoelige informatie over de persoon en kan daarom niet openbaar beschikbaar zijn. We zijn afhankelijk van een combinatie van twee verschillende gegevenssets die openbaar beschikbaar zijn. De eerste gegevensset uit Semeval 2013 - taak 9.1 (Drug Recognition) en de andere is een van de BioCreative V CDR-taak. We zijn combineren en automatische labels van deze twee gegevenssets, zodat we kunnen zowel drugs en ziekten van medische teksten detecteren en evalueren van onze woordinsluitingen. Raadpleeg voor meer informatie voor implementatie, [link naar GitHub-code](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling/02_model_creation).

De model-architectuur die we hebben gebruikt in alle codes en voor de vergelijking wordt hieronder weergegeven. De parameter die voor verschillende gegevenssets is de maximale sequence-lengte (hier 613).

![LSTM model](./media/scenario-tdsp-biomedical-recognition/d-a-d-model.png)

#### <a name="23-model-evaluation"></a>2.3. Model-evaluatie
Zie [Model evaluatie](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling/03_model_evaluation/ReadMe.md).

We gebruiken de evaluatie-script uit de gedeelde taak [biografie-entiteit erkenning taak biografie NLP/NLPBA 2004](http://www.nactem.ac.uk/tsujii/GENIA/ERtask/report.html) de precisie, intrekken en F1 score van het model te evalueren. 

#### <a name="in-domain-versus-generic-word-embedding-models"></a>In het domein ten opzichte van de algemene word modellen insluiten

Hieronder volgt een vergelijking tussen de nauwkeurigheid van de twee typen van de functie: (1) woordinsluitingen getraind op PubMed samenvattingen en (2) woordinsluitingen getraind op Google nieuws. Duidelijk zien we dat het model in het domein het algemene model presteert. Daarom hebben een bepaald woord model insluiten in plaats van met behulp van een algemene is veel meer nuttige. 

* Taak #1: Drugs en ziekten detectie

![Vergelijking van model 1](./media/scenario-tdsp-biomedical-recognition/mc1.png)

We het uitvoeren van de evaluatie van de woordinsluitingen op andere gegevenssets op een soortgelijke wijze en Zie dat model in het domein is altijd beter.

* Taak #2: Eiwitten, cellijn-, Type Legendacel, DNA en RNA detectie

![Vergelijking van model 2](./media/scenario-tdsp-biomedical-recognition/mc2.png)

* Taak #3: Chemisch en ziekten detectie

![Vergelijking van model 3](./media/scenario-tdsp-biomedical-recognition/mc3.png)

* Taak #4: Drugs detectie

![Vergelijking van model 4](./media/scenario-tdsp-biomedical-recognition/mc4.png)

* Taak #5: Genen detectie

![Vergelijking van model 5](./media/scenario-tdsp-biomedical-recognition/mc5.png)

#### <a name="tensorflow-versus-cntk"></a>TensorFlow ten opzichte van CNTK
De gerapporteerde modellen worden getraind met Keras TensorFlow als back-end. Keras met CNTK back-end biedt geen ondersteuning voor 'omgekeerde' op het moment dat dit werk is uitgevoerd. Daarom voor vergelijking, hebben we een Unidirectioneel LSTM-model met de CNTK back-end getraind en vergeleken met een Unidirectioneel LSTM-model met TensorFlow back-end. CNTK 2.0 installeren voor Keras van [hier](https://docs.microsoft.com/cognitive-toolkit/using-cntk-with-keras). 

![Vergelijking van model 6](./media/scenario-tdsp-biomedical-recognition/mc6.png)

We geconcludeerd dat CNTK wordt uitgevoerd als goed als Tensorflow, zowel wat betreft de trainingstijd per epoche (60 seconden voor CNTK en 75 seconden voor Tensorflow) en het aantal test-entiteiten die zijn gedetecteerd. We gebruiken de Unidirectioneel lagen voor evaluatie.


### <a name="3-deployment"></a>3. Implementatie

Zie [implementatie](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/03_deployment).

We implementatie van een webservice op een cluster in de [Azure Container Service (ACS)](https://azure.microsoft.com/services/container-service/). De omgeving bepalingen Docker en Kubernetes in het cluster voor het beheren van de web service-implementatie. U vindt meer informatie over het proces uitoefening [hier](model-management-service-deploy.md ).


## <a name="conclusion"></a>Conclusie

Er is een fout over de details van hoe u kan een word-model insluiten met behulp van Word2Vec algoritme op Spark te trainen en vervolgens de uitgepakte insluitingen als functies gebruiken om een deep neural network voor het ophalen van de entiteit te trainen. We hebben de pijplijn training toegepast op het biomedische domein. De pijplijn is echter algemene moet worden toegepast voor het detecteren van aangepaste Entiteitstypen van andere domeinen. U hoeft alleen voldoende gegevens en kunt u de werkstroom die hier wordt gepresenteerd voor een ander domein gemakkelijk aanpassen.

## <a name="references"></a>Verwijzingen

* Tomas Mikolov, Kai Chen, Greg Corrado en Jeffrey Dean. 2013a. Efficiënte schatting van word voorstellingen in vector ruimte. In de procedures van ICLR.
* Tomas Mikolov, Ilya Sutskever Kai Chen, Greg S Corrado en Jeff Dean. 2013b. Gedistribueerde weergaven van woorden en zinnen en hun compositionality. Pagina in de procedures van NIPS 3111: 3119.
* Billy Chiu, Gamal Crichton, Anna Korhonen en Sampo Pyysalo. 2016. [Het goede Woordinsluitingen Train voor biomedische NLP](http://aclweb.org/anthology/W/W16/W16-2922.pdf), In behandeling van de vijftiende Workshop op biomedische Natural Language Processing 166 – 174 pagina's.
* [Vector representaties van woorden](https://www.tensorflow.org/tutorials/word2vec)
* [Terugkerende Neural Networks](https://www.tensorflow.org/tutorials/recurrent)
* [Problemen met Spark ml Word2Vec](https://intothedepthsofdataengineering.wordpress.com/2017/06/26/problems-encountered-with-spark-ml-word2vec/)
* [Spark Word2Vec: geleerde lessen](https://intothedepthsofdataengineering.wordpress.com/2017/06/26/spark-word2vec-lessons-learned/)

