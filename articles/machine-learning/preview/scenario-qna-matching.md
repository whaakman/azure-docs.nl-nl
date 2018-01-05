---
title: Met Q & een overeenkomend met behulp van Azure Machine Learning Workbench | Microsoft Docs
description: "Hoe verschillende effectieve machine learning-methoden gebruiken om u te overeen met open beëindigd query's naar een reeds bestaande Veelgestelde vragen over/antwoorden paren."
services: machine-learning
documentationcenter: 
author: mezmicrosoft
editor: mezmicrosoft
ms.assetid: 
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: mez
manager: tihazen
ms.openlocfilehash: 33f807a4a0bbc4afd1f2fbe017f8913eccacc34b
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
#  <a name="q--a-matching-using-azure-machine-learning-workbench"></a>Met Q & een overeenkomend met behulp van Azure Machine Learning-workbench
Open beëindigd vragen te beantwoorden, is moeilijk en vaak vereist handmatige inspanning van deskundigen (bestemd). Om te beperken van de vereisten op interne bestemd, maken bedrijven vaak een lijst met veelgestelde vragen (FAQ's) als een manier om gebruikers te helpen. In dit voorbeeld gepresenteerd verschillende effectieve machine learning-methoden zodat deze overeenkomen met open beëindigd query's naar het reeds bestaande Veelgestelde vragen over antwoorden/waardeparen. Dit voorbeeld wordt een eenvoudige manier kunt ontwikkelen-proces voor het bouwen van een dergelijke oplossing met behulp van de Azure Machine Learning-Workbench. 

## <a name="link-to-the-gallery-github-repository"></a>Koppeling naar de galerie GitHub-opslagplaats
[https://github.com/Azure/MachineLearningSamples-QnAMatching](https://github.com/Azure/MachineLearningSamples-QnAMatching)

## <a name="overview"></a>Overzicht

In dit voorbeeld lost het probleem van het toewijzen van gebruiker vragen naar reeds bestaande vraag en antwoord (Q & A) paren als wordt meestal verzorgd door in een lijst met veelgestelde vragen (dat wil zeggen, een FAQ) of in de Q & een paren aanwezig is op websites, zoals [Stack Overloop](https://stackoverflow.com/). Er zijn veel manieren aan een vraag naar het juiste antwoord, zoals het antwoord geeft die het meest overeenkomt met de vraag. Echter, in dit voorbeeld open beëindigd vragen zijn overeen met de eerder gestelde vragen door ervan uitgaande dat elk antwoord in de veelgestelde vragen kunt meerdere semantisch gelijkwaardige vragen beantwoorden.

De belangrijkste stappen vereist voor het leveren van deze oplossing zijn als volgt:

1. Schoon en verwerken van gegevens.
2. Meer informatie over informatieve zinnen, die meerdere word reeksen met meer informatie ziet er in de reeks dan wanneer beschouwd onafhankelijk zijn.
3. Functies extraheren uit tekst.
4. Tekst classificatie modellen trainen en evalueren van de prestaties van het model.


## <a name="prerequisites"></a>Vereisten

De vereisten voor het uitvoeren van dit voorbeeld zijn als volgt:

1. Een [Azure-account](https://azure.microsoft.com/free/) (gratis proefversies beschikbaar zijn).
2. Een geïnstalleerde kopie van [Azure Machine Learning Workbench](./overview-what-is-azure-ml.md) volgende de [installatie snelstartgids](./quickstart-installation.md) het programma te installeren en het maken van een werkruimte.
3. In dit voorbeeld kan worden uitgevoerd op elke compute-context. Het wordt echter aanbevolen uit te voeren op een machine multicore met ten minste 16GB geheugen-en 5GB schijfruimte.

## <a name="create-a-new-workbench-project"></a>Maak een nieuw project in de workbench

Maak een nieuw project in dit voorbeeld als sjabloon gebruiken:
1.  Open Azure Machine Learning Workbench
2.  Op de **projecten** pagina, klikt u op de  **+**  en selecteer **nieuw Project**
3.  In de **nieuw Project maken** deelvenster Vul de informatie voor het nieuwe project
4.  In de **zoeken projectsjablonen** het zoekvak, typ 'Q & A Matching' en selecteer de sjabloon
5.  Klik op **Maken**.

## <a name="data-description"></a>Beschrijving van de gegevens

De gegevensset die wordt gebruikt in dit voorbeeld is een Stack Exchange gegevens Dump opgeslagen op [archive.org](https://archive.org/details/stackexchange). Deze gegevens is een geanonimiseerde dump van alle inhoud hebben bijgedragen van een gebruiker op de [Stack Exchange netwerk](https://stackexchange.com/). Elke site in het netwerk zijn opgemaakt als een afzonderlijk archief die bestaan uit XML-bestanden via ingepakte 7-zip met bzip2 compressie. Elke site-archief bevat berichten, gebruikers, stemmen, opmerkingen, PostHistory en PostLinks. 

### <a name="data-source"></a>Gegevensbron

In dit voorbeeld wordt de [gegevens (10 GB) verzendt](https://archive.org/download/stackexchange/stackoverflow.com-Posts.7z) en [PostLinks gegevens (515 MB)](https://archive.org/download/stackexchange/stackoverflow.com-PostLinks.7z) uit de Stack Overflow-site. Zie voor informatie over het volledige schema, de [Leesmij](https://ia800500.us.archive.org/22/items/stackexchange/readme.txt). 

De `PostTypeId` veld in de gegevens van berichten wordt aangegeven of een post een `Question` of een `Answer`. De `PostLinkTypeId` veld in de PostLinks gegevens geeft aan of twee berichten zijn gekoppeld of dupliceren. Vraag posts bevatten doorgaans enkele codes die sleutelwoorden die een vraag met andere vergelijkbare/duplicaat vragen categoriseren. Er zijn enkele codes met hoge frequentie zoals `javascript`, `java`, `c#`, `php` enz., bestaan uit een groter aantal berichten van de vraag. In dit voorbeeld, een subset van Q & een paren met de `javascript` tag wordt opgehaald.

Additionionally, een vraag post mogelijk te wijten aan meerdere antwoord posts of dubbele vraag advertenties. Kan een lijst met veelgestelde vragen over van deze twee gegevenssets, worden sommige gegevens verzameling criteria beschouwd. De drie sets van verzamelde gegevens worden geselecteerd met behulp van een SQL-script niet in dit voorbeeld opgenomen is. De gegevensbeschrijving van de resulterende is als volgt:

- `Original Questions (Q)`: Vraag posts worden gevraagd en beantwoord op de Stack Overflow-site.
- `Duplications (D)`: Vraag andere vooraf bestaande vragen dubbele berichten (`PostLinkTypeId = 3`), die de oorspronkelijke vragen zijn. Dubbele worden beschouwd als semantisch gelijk is aan de oorspronkelijke vragen in de zin dat het opgegeven op de oorspronkelijke vraag antwoord ook antwoord op de nieuwe dubbele vraag.
- `Answers (A)`: Elke oorspronkelijke vraag en het bijbehorende dubbele kunnen koppelingen naar meer dan één antwoord-berichten. In dit voorbeeld selecteert alleen de antwoord-post dat ofwel wordt geaccepteerd door de auteur van het oorspronkelijke (gefilterd op `AcceptedAnswerId`) of heeft de hoogste score (gerangschikt door `Score`) in de oorspronkelijke vragen. 

De combinatie van deze drie gegevenssets worden gemaakt met Q & A paren waarbij een antwoord (A) is toegewezen aan één oorspronkelijke vraag (Q) en meerdere dubbele vragen (D) zoals wordt geïllustreerd door het volgende diagram voor gegevens:

<table><tr><td><img id ="data_diagram" src="media/scenario-qna-matching/data_diagram.png"></td></tr></table>

### <a name="data-structure"></a>Gegevensstructuur

Het gegevensschema en de directe downloadkoppelingen van de drie gegevenssets vindt u in de volgende tabel:

| Gegevensset | Veld | Type | Beschrijving
| ----------|------------|------------|--------
| [vragen](https://bostondata.blob.core.windows.net/stackoverflow/orig-q.tsv.gz) | Id | Tekenreeks | De unieke vraag-ID (primaire sleutel)
|  | AnswerId | Tekenreeks | De ID uniek antwoord per vraag
|  | text0 | Tekenreeks | De onbewerkte gegevens zoals de titel en de hoofdtekst van de vraag
|  | CreationDate | Timestamp | De tijdstempel van wanneer de vraag is gevraagd
| [duplicaten](https://bostondata.blob.core.windows.net/stackoverflow/dup-q.tsv.gz) | Id | Tekenreeks | De duplicatie van de unieke ID (primaire sleutel)
|  | AnswerId | Tekenreeks | De antwoord-ID die is gekoppeld aan de duplicatie
|  | text0 | Tekenreeks | De onbewerkte gegevens met inbegrip van de duplicatie titel en tekst
|  | CreationDate | Timestamp | De tijdstempel van wanneer de duplicatie is gevraagd
| [antwoorden](https://bostondata.blob.core.windows.net/stackoverflow/ans.tsv.gz)  | Id | Tekenreeks | De unieke antwoord-ID (primaire sleutel)
|  | text0 | Tekenreeks | De onbewerkte gegevens van het antwoord


## <a name="scenario-structure"></a>Scenario-structuur

Het overeenkomende voorbeeld Q & A worden weergegeven via de drie typen bestanden. Het eerste type is een reeks Jupyter-notitieblokken die de stapsgewijze beschrijvingen van de volledige werkstroom weergeven. Het tweede type is een set Python-bestanden bevatten aangepaste Python-modules voor de extractie learning en functie wachtwoordzin op te geven. Deze Python-modules zijn algemene bedienen niet alleen in dit voorbeeld, maar ook andere toepassingen. Het derde type is een set van Python-bestanden afstemmen van hyper-parameters en prestaties van het model met behulp van de Azure Machine Learning-Workbench.

De bestanden in dit voorbeeld zijn als volgt worden ingedeeld.

| Bestandsnaam | Type | Beschrijving
| ----------|------------|--------
| `Image` | Map | De map die wordt gebruikt voor het opslaan van afbeeldingen voor het Leesmij-bestand
| `notebooks` | Map | De map Jupyter-Notebooks
| `modules` | Map | De map voor Python-modules
| `scripts` | Map | De map Python-bestanden
| `notebooks/Part_1_Data_Preparation.ipynb` | Jupyter Notebook | Toegang tot de voorbeeldgegevens, de tekst, vooraf verwerken en training voorbereiden en gegevenssets testen
| `notebooks/Part_2_Phrase_Learning.ipynb` | Jupyter Notebook | Meer informatieve zinnen en tekst in de eigenschappenverzameling van woorden (bogen) verklaringen basisvormen
| `notebooks/Part_3_Model_Training_and_Evaluation.ipynb` | Jupyter Notebook | Uitpakken van functies, tekst classificatie modellen en de prestaties van de evaluatie-model trainen
| `modules/__init__.py` | Python-bestand | De Python package init-bestand
| `modules/phrase_learning.py` | Python-bestand | De Python-modules die worden gebruikt om de onbewerkte gegevens transformeren en te leren van informatieve zinnen
| `modules/feature_extractor.py` | Python-bestand | De Python-modules extraheren functies voor het model trainen
| `scripts/naive_bayes.py` | Python-bestand | De Python af te stemmen hyper-parameters in het model Naive Bayes
| `scripts/random_forest.py` | Python-bestand | De Python af te stemmen hyper-parameters in het model willekeurige Forest
| `README.md` | Markdown-bestand | Het bestand Leesmij markdown

> [!NOTE]
> De reeks notitieblokken is onder Python 3.5 gebouwd.
> 

### <a name="data-ingestion-and-transformation"></a>Gegevensopname en transformatie

De drie gecompileerde gegevenssets worden opgeslagen in een Blob-opslag en worden opgehaald `Part_1_Data_Preparation.ipynb` notebook.  

Voordat u de tekst classificatie modellen trainen, wordt de tekst in de vragen gereinigd en voorverwerkte als u wilt uitsluiten van codefragmenten. Een leren zonder supervisie woordgroep wordt toegepast via het trainingsmateriaal voor meer informatie over informatieve word meerdere reeksen. Deze formules worden weergegeven als één samengestelde woord eenheden in de downstream eigenschappenverzameling van woorden (bogen) featurization die wordt gebruikt door de tekst classificatie-modellen.

De gedetailleerde stapsgewijze beschrijvingen van tekst voorverwerking en woordgroep learning vindt u in de laptops `Part_1_Data_Preparation.ipynb` en `Part_2_Phrase_Learning.ipynb`respectievelijk.

### <a name="modeling"></a>Modelleren

In dit voorbeeld is ontworpen om te beoordelen classificatiemodellen kunnen waarbij elke bestaande Q & een combinatie van een unieke klasse en een subset van de dubbele vragen voor elke set Q & A zijn beschikbaar als nieuwe vragen op basis van de vooraf bestaande Q & een paren door trainingstekst trainingsmateriaal. In het voorbeeld wordt de oorspronkelijke vragen en 75% van de dubbele vragen behouden voor training en de meest recent geboekte 25% van de dubbele vragen zijn ondergebracht-out als evaluatiegegevens.

De classificatie-model maakt gebruik van een ensemble-methode voor het cumuleren van drie base classificaties, met inbegrip van **Naive Bayes**, **ondersteuning Vectormachine**, en **willekeurige Forest**. In elke base classificatie de `AnswerId` wordt gebruikt als het label van de klasse en de weergaven bogen wordt gebruikt als de functies.

Het model trainen-proces wordt geïllustreerd in `Part_3_Model_Training_and_Evaluation.ipynb`.

### <a name="evaluation"></a>Evaluatie

Twee verschillende evaluatie metrische gegevens worden gebruikt om prestaties vast te stellen. 
1. `Average Rank (AR)`: geeft de gemiddelde positie waar het juiste antwoord wordt gevonden in de lijst met opgehaalde Q & A paren (buiten de volledige set van 103 antwoord klassen). 
2. `Top 3 Percentage`: Hiermee wordt aangegeven dat het juiste antwoord kan worden opgehaald in de belangrijkste drie opties in de geretourneerde lijst gerangschikt op het percentage van de test vragen. 

De evaluatie wordt geïllustreerd in `Part_3_Model_Training_and_Evaluation.ipynb`.


## <a name="conclusion"></a>Conclusie

In dit voorbeeld wordt uitgelegd hoe u Well-Known text analytics technieken, zoals woordgroep learning en tekst classificatie gebruiken om te maken van een robuuste model. Deze gepresenteerd ook hoe Azure Machine Learning Workbench kan helpen met interactieve oplossing ontwikkelen en volgen model prestaties. 

Er zijn een aantal belangrijke licht van dit voorbeeld:

- De vraag en antwoord overeenkomende probleem kunnen effectief zijn opgelost met modellen van woordgroep learning- en classificatie.
- Interactieve model ontwikkelen met Azure Machine Learning Workbench en Jupyter-Notebook te demonstreren.
- Azure Machine Learning Workbench beheert de uitvoeringsgeschiedenis en geleerde modellen met registratie van de evaluatie van metrische gegevens voor vergelijkingsdoeleinden. Deze functies kunnen snel hyper parameter afstemmen en kunnen de best presterende modellen identificeren.


## <a name="references"></a>Verwijzingen

Timothy J. Hazen, Fred Richardson, [ _onderwerp modellering van eigen spraak modelleren Multiword woordgroepen met beperkte zinnen-structuur voor verbeterde_](http://people.csail.mit.edu/hazen/publications/Hazen-SLT-2012.pdf). Gesproken technologie Workshop (SLT) 2012 IEEE. IEEE, 2012.

Timothy J. Hazen, [ _MCE Training technieken voor de identificatie van het onderwerp van gesproken Audio documenten_ ](http://ieeexplore.ieee.org/abstract/document/5742980/) IEEE transacties op spraak, Audio en taal verwerken is vol 19, niet. 8, p. 2451-2460 nov 2011.
