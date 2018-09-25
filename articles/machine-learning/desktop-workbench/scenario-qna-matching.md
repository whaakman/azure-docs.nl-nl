---
title: Q & een overeenkomend met behulp van Azure Machine Learning Workbench | Microsoft Docs
description: Het gebruik van verschillende methoden van de kracht van machine learning zodat deze overeenkomt met een open beëindigd query's naar een reeds bestaande Veelgestelde vragen over/antwoorden op beveiligingsvragen paren.
services: machine-learning
documentationcenter: ''
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ROBOTS: NOINDEX
ms.openlocfilehash: e0f6148e1fb28838bf99c63fbfbfbfe8cd127c8c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46973199"
---
#  <a name="q--a-matching-using-azure-machine-learning-workbench"></a>Q & een overeenkomend met behulp van Azure Machine Learning workbench

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 


Open afgelopen vragen te beantwoorden, is het moeilijk, en moet vaak handmatige inspanningen van deskundigen (MKB). Om te beperken op basis van de vraag op interne kleine en middelgrote ondernemingen, maken bedrijven vaak lijsten met Frequently Asked Questions (Veelgestelde vragen) als middel om te helpen van gebruikers. In dit voorbeeld worden de verschillende methoden van effectieve machine learning zodat deze overeenkomt met open afgelopen query's naar het reeds bestaande Veelgestelde vragen over vraag/antwoord paren gepresenteerd. In dit voorbeeld ziet u een eenvoudige ontwikkeling-proces voor het bouwen van zo'n oplossing met behulp van de Azure Machine Learning Workbench. 

## <a name="link-to-the-gallery-github-repository"></a>Koppeling naar de galerie met GitHub-opslagplaats
[https://github.com/Azure/MachineLearningSamples-QnAMatching](https://github.com/Azure/MachineLearningSamples-QnAMatching)

## <a name="overview"></a>Overzicht

In dit voorbeeld, lost het probleem voor het toewijzen van gebruiker vragen naar vooraf bestaande vraag en antwoord (Q & A) paren dat doorgaans wordt aangeboden in een lijst met veelgestelde vragen (dat wil zeggen, een veelgestelde vragen) of in het Q & een paren aanwezig zijn op websites, zoals [Stack Overflow](https://stackoverflow.com/). Er zijn veel manieren waarop een vraag naar het juiste antwoord, zoals het vinden van het antwoord dat is het meest overeenkomt met de vraag. Echter, in dit voorbeeld open afgelopen vragen zijn afgestemd op eerder gestelde vragen door ervan uitgaande dat elk antwoord in de veelgestelde vragen over meerdere semantisch gelijkwaardige vragen kan beantwoorden.

De belangrijkste stappen vereist voor het leveren van deze oplossing zijn als volgt:

1. Opschonen en verwerken van gegevens.
2. Meer informatie over informatieve zinnen die meervoudige woordreeksen met meer informatie wanneer deze wordt bekeken in de reeks dan wanneer onafhankelijk van elkaar worden behandeld.
3. Functies extraheren uit tekst.
4. Trainen van modellen voor tekstclassificatie en modelprestaties evalueren.


## <a name="prerequisites"></a>Vereisten

De vereisten voor het uitvoeren van dit voorbeeld zijn er als volgt uit:

1. Een [Azure-account](https://azure.microsoft.com/free/) (gratis proefversies zijn beschikbaar).
2. Een geïnstalleerde kopie van [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md) volgende de [snel starten-installatiehandleiding](quickstart-installation.md) aan het programma te installeren en een werkruimte maken.
3. In dit voorbeeld kan worden uitgevoerd op een compute-context. Het wordt echter aanbevolen uit te voeren op een computer meerdere kernen met ten minste 16GB geheugen en 5GB schijfruimte.

## <a name="create-a-new-workbench-project"></a>Een nieuw workbench-project maken

Maak een nieuw project met behulp van dit voorbeeld als een sjabloon:
1.  Open Azure Machine Learning Workbench
2.  Op de **projecten** pagina, klikt u op de **+** Meld u aan en selecteer **nieuw Project**
3.  In de **nieuw Project maken** deelvenster, vult u in de gegevens voor het nieuwe project
4.  In de **zoeken naar projectsjablonen** het zoekvak, typ 'Q & A die overeenkomen met' en selecteer de sjabloon
5.  Klik op **Maken**.

## <a name="data-description"></a>Beschrijving van de gegevens

De gegevensset die wordt gebruikt in dit voorbeeld wordt een Stack Exchange gegevens Dump Data-at- [archive.org](https://archive.org/details/stackexchange). Deze gegevens is een anonieme dump van alle inhoud van de gebruiker heeft op de [Stack Exchange netwerk](https://stackexchange.com/). Elke site in het netwerk zijn opgemaakt als een afzonderlijk archief die bestaat uit een XML-bestanden via ingepakte 7-zip-compressie bzip2 gebruiken. Het archief van elke site bevat berichten, gebruikers, stemmen, opmerkingen, PostHistory en PostLinks. 

### <a name="data-source"></a>Gegevensbron

In dit voorbeeld wordt de [gegevens (10 GB) verzendt](https://archive.org/download/stackexchange/stackoverflow.com-Posts.7z) en [PostLinks gegevens (515 MB)](https://archive.org/download/stackexchange/stackoverflow.com-PostLinks.7z) van de Stack Overflow-site. Zie voor informatie over het volledige schema, de [Leesmij.txt](https://ia800500.us.archive.org/22/items/stackexchange/readme.txt). 

De `PostTypeId` veld in de gegevens voor berichten wordt aangegeven of een bericht een `Question` of een `Answer`. De `PostLinkTypeId` veld in de PostLinks gegevens geeft aan of twee berichten zijn gekoppeld of dupliceren. Vraag berichten bevatten doorgaans enkele tags zijn trefwoorden die een vraag met andere vergelijkbare/dubbele vragen categoriseren. Er zijn enkele codes met hoge frequentie, zoals `javascript`, `java`, `c#`, `php` enz., bestaan uit een groter aantal berichten van de vraag. In dit voorbeeld, een subset van Q & een paren met de `javascript` tag wordt opgehaald.

Additionionally, een bericht vraag verband houden met meerdere antwoord-berichten of dubbele vraag berichten. Kan een lijst met veelgestelde vragen over van deze twee gegevenssets, worden sommige gegevens verzameling criteria beschouwd. De drie gecompileerde gegevenssets zijn geselecteerd met behulp van een SQL-script, dat niet is opgenomen in dit voorbeeld. De beschrijving van de resulterende gegevens is als volgt:

- `Original Questions (Q)`: Vraag berichten zijn gesteld en beantwoord op Stack Overflow-site.
- `Duplications (D)`: Vraag andere vooraf bestaande vragen dubbele berichten (`PostLinkTypeId = 3`), die de oorspronkelijke vragen zijn. Dubbele worden beschouwd als semantisch gelijk is aan de oorspronkelijke vragen in de zin dat het antwoord dat is opgegeven op de oorspronkelijke vraag ook antwoorden op de nieuwe dubbele vraag.
- `Answers (A)`: De oorspronkelijke vraag en de dubbele kunnen koppelingen naar meer dan één antwoord-berichten. In dit voorbeeld selecteert alleen de antwoord-bericht dat ofwel wordt geaccepteerd door de auteur van de oorspronkelijke (gefilterd op `AcceptedAnswerId`) of heeft de hoogste score (gerangschikt op `Score`) in de oorspronkelijke vragen. 

De combinatie van deze drie gegevenssets maakt Q & A-paren waarbij een antwoord (A) is toegewezen aan een oorspronkelijke vraag (Q) en meerdere dubbele vragen (D) zoals wordt geïllustreerd door het volgende diagram voor gegevens:

<table><tr><td><img id ="data_diagram" src="media/scenario-qna-matching/data_diagram.png"></td></tr></table>

### <a name="data-structure"></a>Gegevensstructuur

Het gegevensschema en de directe downloadkoppelingen van de drie gegevenssets kan worden gevonden in de volgende tabel:

| Gegevensset | Veld | Type | Beschrijving
| ----------|------------|------------|--------
| [Vragen](https://bostondata.blob.core.windows.net/stackoverflow/orig-q.tsv.gz) | Id | Reeks | De unieke vraag-ID (primaire sleutel)
|  | AnswerId | Reeks | De ID van unieke antwoord per vraag
|  | Text0 | Reeks | De onbewerkte gegevens, waaronder de titel en de hoofdtekst van de vraag
|  | CreationDate | Tijdstempel | De timestamp van wanneer de vraag is gevraagd
| [duplicaten](https://bostondata.blob.core.windows.net/stackoverflow/dup-q.tsv.gz) | Id | Reeks | De unieke duplicatie-ID (primaire sleutel)
|  | AnswerId | Reeks | De antwoord-ID die is gekoppeld aan de mate van duplicatie
|  | Text0 | Reeks | De onbewerkte gegevens met inbegrip van de titel en de hoofdtekst van de mate van duplicatie
|  | CreationDate | Tijdstempel | De timestamp van wanneer de mate van duplicatie is gevraagd
| [antwoorden](https://bostondata.blob.core.windows.net/stackoverflow/ans.tsv.gz)  | Id | Reeks | De unieke antwoord-ID (primaire sleutel)
|  | text0 | Reeks | De onbewerkte gegevens van het antwoord


## <a name="scenario-structure"></a>Scenario-structuur

In het Q & A overeenkomende voorbeeld worden weergegeven via drie soorten bestanden. Het eerste type is een reeks Jupyter-Notebooks waarin de stapsgewijze beschrijvingen van de volledige werkstroom. Het tweede type is een set Python-bestanden bevatten aangepaste Python-modules voor de functie en learning vindt er sleuteltermextractie plaats. Deze Python-modules zijn algemeen genoeg is om niet alleen in dit voorbeeld, maar ook andere toepassingen. Het derde type is een set Python-bestanden voor het afstemmen van hyperparameters en bijhouden van modelprestaties van het met behulp van de Azure Machine Learning Workbench.

De bestanden in dit voorbeeld zijn als volgt ingedeeld.

| Bestandsnaam | Type | Beschrijving
| ----------|------------|--------
| `Image` | Map | De map die wordt gebruikt voor het opslaan van installatiekopieën voor het Leesmij-bestand
| `notebooks` | Map | De map Jupyter-Notebooks
| `modules` | Map | De map van de Python-modules
| `scripts` | Map | De map met bestanden van Python
| `notebooks/Part_1_Data_Preparation.ipynb` | Jupyter Notebook | Toegang tot de voorbeeldgegevens, vooraf verwerken van de tekst en training voorbereiden en gegevenssets testen
| `notebooks/Part_2_Phrase_Learning.ipynb` | Jupyter Notebook | Meer informatieve zinnen en tekst in de eigenschappenverzameling van woorden (bogen) voorstellingen basisvormen
| `notebooks/Part_3_Model_Training_and_Evaluation.ipynb` | Jupyter Notebook | Functies ophalen, het trainen van modellen voor tekstclassificatie en modelprestaties evaluatie
| `modules/__init__.py` | Python-bestand | Het Python-pakket init-bestand
| `modules/phrase_learning.py` | Python-bestand | De Python-modules gebruikt voor het transformeren van de onbewerkte gegevens en meer informatieve zinnen
| `modules/feature_extractor.py` | Python-bestand | De Python-modules ophalen functies voor modeltraining
| `scripts/naive_bayes.py` | Python-bestand | De Python om af te stemmen hyper-parameters in het Naive Bayes-model
| `scripts/random_forest.py` | Python-bestand | De Python om af te stemmen hyper-parameters in het model willekeurige Forest
| `README.md` | Markdown-bestand | Het Leesmij-bestand markdown-bestand

> [!NOTE]
> De reeks laptops is gebouwd onder Python 3.5.
> 

### <a name="data-ingestion-and-transformation"></a>Gegevensopname en -transformatie

De drie gecompileerde gegevenssets worden opgeslagen in een Blob-opslag en worden opgehaald `Part_1_Data_Preparation.ipynb` notebook.  

De tekst in de vragen is voordat de modellen voor tekstclassificatie training, opgeschoond en voorverwerkte als u wilt uitsluiten van codefragmenten. Een leren zonder supervisie woordgroep wordt toegepast via het trainingsmateriaal voor meer informatie over informatieve meervoudige woordreeksen. Deze items worden weergegeven als één samengestelde woord eenheden in de downstream eigenschappenverzameling van woorden (bogen) parametrisatie die worden gebruikt door de modellen voor tekstclassificatie.

De gedetailleerde stapsgewijze beschrijving van de voorverwerking van tekst en woordgroep learning kunnen u vinden in de Notebooks `Part_1_Data_Preparation.ipynb` en `Part_2_Phrase_Learning.ipynb`, respectievelijk.

### <a name="modeling"></a>Modelleren

In dit voorbeeld is ontworpen om modellen te beoordelen nieuwe vragen op basis van de bestaande Q & een paren door trainingstekst classificatie waarbij elke bestaande Q & een paar is een unieke klasse en een subset van de dubbele vragen voor elk paar Q & A zijn beschikbaar als trainingsmateriaal. In het voorbeeld wordt de oorspronkelijke vragen en 75% van de dubbele vragen worden bewaard voor training en de meest recent geboekte 25% van de dubbele vragen zijn die zijn ondergebracht-out als evaluatiegegevens.

De classificeringsmodel gebruikt een methode ensembles te aggregeren drie base classificaties, met inbegrip van **Naive Bayes**, **Support Vector Machine**, en **willekeurige Forest**. In elke basis classificatie, de `AnswerId` wordt gebruikt als het label van de klasse en de weergaven bogen wordt gebruikt als de functies.

Het proces van de training model wordt weergegeven in `Part_3_Model_Training_and_Evaluation.ipynb`.

### <a name="evaluation"></a>Evaluatie

Twee verschillende evaluatie metrische gegevens worden gebruikt om prestaties vast te stellen. 
1. `Average Rank (AR)`: geeft de gemiddelde positie waar het juiste antwoord wordt gevonden in de lijst van de opgehaalde Q & A-paren (van de volledige set 103 antwoord klassen). 
2. `Top 3 Percentage`: Hiermee wordt aangegeven dat het juiste antwoord kan worden opgehaald in de drie belangrijkste keuzes in de geretourneerde lijst gerangschikt op het percentage van de test vragen. 

De evaluatie wordt geïllustreerd in `Part_3_Model_Training_and_Evaluation.ipynb`.


## <a name="conclusion"></a>Conclusie

Dit voorbeeld wordt uitgelegd hoe u bekende text analytics-technieken, zoals woordgroep learning en tekst classificatie, voor het produceren van een robuust model. Ook zien hoe u Azure Machine Learning Workbench kunt met interactieve oplossing ontwikkelen en traceren modelprestaties. 

Er zijn enkele belangrijke kenmerken van het volgende voorbeeld:

- De vraag en antwoord overeenkomende probleem kunnen effectief zijn opgelost met woordgroep learning en tekst classificatie-modellen.
- Om aan te tonen interactieve model ontwikkelen met Azure Machine Learning Workbench en Jupyter-Notebook.
- Azure Machine Learning Workbench beheert de uitvoeringsgeschiedenis en geleerde modellen met het registreren van de evaluatie van metrische gegevens voor vergelijkingsdoeleinden. Deze functies kunnen snel hyperparameters en helpt bij het identificeren van de best presterende modellen.


## <a name="references"></a>Verwijzingen

Timothy J. Hazen, Fred Richardson, [ _onderwerp modellen van de conversatie spraak Multiword zinnen met beperkte zinnen structuur voor het modelleren verbeterd_](http://people.csail.mit.edu/hazen/publications/Hazen-SLT-2012.pdf). Gesproken taal technologie Workshop (SLT), 2012 IEEE. IEEE, 2012.

Timothy J. Hazen, [ _MCE Training technieken voor het onderwerp-id van gesproken Audio documenten_ ](http://ieeexplore.ieee.org/abstract/document/5742980/) IEEE transacties op Audio-, spraak en taal verwerken, vol 19, niet. 8, p. 2451-2460 november 2011.
