---
title: Machine learning-algoritmen kiezen | Microsoft Docs
description: Klik hier voor meer informatie over het Azure Machine Learning-algoritmen voor en zonder supervisie learning in clustering, classificatie of regressie experimenten kiest.
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: a3b23d7f-f083-49c4-b6b1-3911cd69f1b4
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 12/18/2017
ms.author: garye
ms.openlocfilehash: 1b30e4dbf20cac653c323720de779aa5f8edba68
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="how-to-choose-algorithms-for-microsoft-azure-machine-learning"></a>Algoritmen kiezen voor Microsoft Azure Machine Learning
Het antwoord op de vraag 'Welke machine learning-algoritme moet ik gebruiken?' is altijd "Deze afhankelijk is." Dit is afhankelijk van de grootte, kwaliteit en aard van de gegevens. Dit is afhankelijk van wat u wilt doen met het antwoord. Dit is afhankelijk van hoe de berekening van het algoritme is vertaald naar instructies voor de computer die u gebruikt. En dit is afhankelijk van hoeveel tijd die u hebt. Zelfs de meest ervaren gegevenswetenschappers kunnen niet zien welk algoritme best zal uitvoeren voordat u ze.

## <a name="the-machine-learning-algorithm-cheat-sheet"></a>De Machine Learning-algoritme cheats blad
De **Microsoft Azure Machine Learning-algoritme cheats blad** helpt bij het kiezen van de juiste machine learning-algoritme voor uw predictive analytics-oplossingen van de Microsoft Azure Machine Learning-bibliotheek van algoritmen.
Dit artikel begeleidt u bij het gebruik ervan.

> [!NOTE]
> Voor het downloaden van de referentieoverzicht en volg daarna de in dit artikel, gaat u naar [Machine learning algorithm cheat sheet voor Microsoft Azure Machine Learning Studio](algorithm-cheat-sheet.md).
> 
> 

Deze referentieoverzicht heeft een specifieke doelgroep in gedachten: een begin gegevens wetenschappelijk met undergraduate niveau machine learning, probeert te beginnen met een algoritme kiezen in Azure Machine Learning Studio. Dat betekent dat bepaalde generalisaties en oversimplifications maakt, maar het u in een veilige richting distributiepunten. Het betekent ook dat er veel algoritmen hier niet worden vermeld zijn. Wanneer Azure Machine Learning groeit te omvatten een uitgebreidere set van beschikbare methoden, gaan we ze toevoegen.

Deze aanbevelingen zijn gecompileerde feedback en tips van veel gegevenswetenschappers en machine learning-experts. We alles wat niet overeenkomen, maar ik heb geprobeerd te harmoniseren onze adviezen in een ruwe consensus. De meeste instructies van geschil beginnen met "Taak afhankelijk is,..."

### <a name="how-to-use-the-cheat-sheet"></a>Het gebruik van de referentieoverzicht
Lezen van het pad en de algoritme labels in de grafiek als ' voor  *&lt;pad label&gt;*, gebruik  *&lt;algoritme&gt;*. " Bijvoorbeeld ' voor *snelheid*, gebruik *twee logistic regression klasse*. " Soms meer dan een vertakking is van toepassing.
Geen van hen worden soms past perfect. Ze zijn bedoeld als regel van de miniatuur aanbevelingen, dus u hoeft het exacte wordt.
Verschillende gegevenswetenschappers die ik gehad met genoemde die de enige manier om te zoeken de allerbeste algoritme is om te proberen ze allemaal.

Hier volgt een voorbeeld van de [AI-galerie van Azure](http://gallery.cortanaintelligence.com/) van een experiment die probeert verschillende algoritmen tegen dezelfde gegevens en de resultaten vergelijkt: [vergelijken met meerdere klasse classificaties: Letter erkenning](http://gallery.cortanaintelligence.com/Details/a635502fc98b402a890efe21cec65b92).

> [!TIP]
> Zie [Overzichtsdiagram van de mogelijkheden van Azure Machine Learning Studio](studio-overview-diagram.md) als u een diagram wilt downloaden en afdrukken met een overzicht van de mogelijkheden van Machine Learning Studio.
> 
> 

## <a name="flavors-of-machine-learning"></a>Versies van machine learning
### <a name="supervised"></a>Onder supervisie
Leren met supervisie algoritmen voorspellingen op basis van een reeks voorbeelden. Historische aandelenkoersen kunnen bijvoorbeeld worden gebruikt voor gevaar wat op prijzen in de toekomst. Elk voorbeeld gebruikt voor training is gelabeld met de waarde van belang: in dit geval de aandelenkoersen. Een algoritme leren met supervisie zoekt naar patronen in deze waardelabels. Het kan gebruiken informatie die van belang kan zijn: de dag van de week, het seizoen, financiële gegevens van het bedrijf, het type branche, de aanwezigheid van verstoren geopolitieke gebeurtenissen, en elk algoritme eruitziet voor verschillende soorten patronen. Nadat de algoritme de beste patroon heeft gevonden dat mogelijk is, wordt het patroon gebruikt voor het maken van voorspellingen voor zonder label testgegevens: morgen prijzen.

Leren met supervisie is een populair en nuttige van machine learning. Met één uitzondering, worden alle modules in Azure Machine Learning supervisie learning-algoritmen. Er zijn verschillende specifieke leren met supervisie-typen die worden vertegenwoordigd in Azure Machine Learning: classificatie, regressie en afwijkingsdetectie detectie.

* **Classificatie**. Wanneer de gegevens worden gebruikt om te voorspellen van een categorie, leren met supervisie is een afkorting classificatie. Dit is het geval bij het toewijzen van een afbeelding als een afbeelding van een 'cat' of een 'aquaduct'. Wanneer er slechts twee keuzes, deze wordt aangeroepen **tweeklasse** of **binomiale classificatie**. Wanneer er meer categorieën als bij de voorspelling van de toernooi NCAA maart Madness gewonnen, dit probleem wordt ook wel **meerdere klasse classificatie**.
* **Regressie**. Wanneer u een waarde wordt net als bij aandelenkoersen wordt voorspeld, leren met supervisie regressie genoemd.
* **Afwijkingsdetectie**. Soms is het doel het identificeren van gegevenspunten die gewoon ongebruikelijk zijn. Fraude te detecteren, bijvoorbeeld zijn een bestedingslimiet patronen maximaal ongebruikelijke creditcard verdacht. De mogelijke variaties zijn dus talrijke en de voorbeelden training dus enkele, is niet haalbaar voor meer informatie over welke frauduleuze activiteit ziet eruit als. De benadering die afwijkingsdetectie duurt is gewoon meer informatie over welke normale activiteit eruit (met behulp van een niet-frauduleuze transacties geschiedenis) en alles die aanzienlijk verschilt identificeren.

### <a name="unsupervised"></a>Zonder supervisie
Zonder supervisie learning hebben gegevenspunten geen labels worden gekoppeld. In plaats daarvan is het doel van een leeralgoritme zonder supervisie Organiseer de gegevens op een bepaalde manier of om de structuur te beschrijven. Dit kan betekenen dat het groeperen in clusters of het vinden van de verschillende manieren van complexe gegevens bekijkt, zodat het eenvoudiger of meer geordend wordt weergegeven.

### <a name="reinforcement-learning"></a>Versterking learning
In de versterking leren, haalt de algoritme een actie te kiezen in reactie op elk gegevenspunt. Het leeralgoritme ontvangt ook een signaal derden korte tijd later, dat aangeeft hoe goed het besluit is.
De algoritme wijzigt op basis van dit, de strategie met het oog op de hoogste prijs. Er zijn momenteel geen versterking learning-algoritme-modules in Azure Machine Learning. Versterking learning is gemeenschappelijk in robotics, waarbij de set sensormetingen op één punt in tijd is een gegevenspunt en het algoritme moet de volgende actie van de robot kiezen. Het is ook dat een natuurlijke geschikt voor Internet der dingen toepassingen.

## <a name="considerations-when-choosing-an-algorithm"></a>Overwegingen bij het kiezen van een algoritme
### <a name="accuracy"></a>Nauwkeurigheid
Ophalen van de meest nauwkeurige antwoord mogelijk niet altijd nodig.
Een benadering is soms voldoende, afhankelijk van wat u wilt gebruiken. Als dit het geval is, kunt u mogelijk uw verwerkingstijd aanzienlijk knippen door bevalt meer geschatte methoden. Een ander voordeel van methoden met meer bij benadering is dat ze natuurlijk vaak voorkomen [overfitting](https://youtu.be/DQWI1kvmwRg).

### <a name="training-time"></a>Trainingstijd
Het aantal minuten of uren die nodig zijn voor het model trainen is veel van de algoritmen. Vaak sterk training tijd is gekoppeld aan nauwkeurigheid: een doorgaans wordt meegestuurd met de andere. Bovendien zijn sommige algoritmen gevoeliger voor het aantal gegevenspunten dan andere.
Wanneer het tijd is beperkt kan deze de keuze van algoritme, station met name wanneer de gegevensset groot is.

### <a name="linearity"></a>Lineariteit
Veel van de machine learning-algoritmen maken gebruik van lineariteit. Lineaire classificatie algoritmen wordt ervan uitgegaan dat klassen kunnen worden gescheiden door een lineaire (of de hogere-dimensionale analoge). Dit zijn logistic regression en vector machines ondersteunen (zoals geïmplementeerd in Azure Machine Learning).
Lineaire regressie algoritmen wordt ervan uitgegaan dat gegevenstrends een lineaire volgen. Deze veronderstellingen niet nadelig voor sommige problemen, maar ze op andere omlaag nauwkeurigheid meenemen.

![Niet-lineaire klasse grens][1]

***Niet-lineaire klasse grens*** *-afhankelijk te zijn van een algoritme lineaire classificatie zou leiden tot nauwkeurigheid laag*

![Gegevens met een niet-lineaire trend][2]

***Gegevens met een niet-lineaire trend*** *-met een methode lineaire regressie genereert veel fouten groter is dan nodig*

Lineaire algoritmen zijn ondanks hun gevaren zeer populair is een eerste regel van een aanval. Ze zijn meestal algoritmisch eenvoudig en snel te trainen.

### <a name="number-of-parameters"></a>Aantal parameters
Parameters zijn de knoppen die een wetenschappelijk gegevens opgehaald bij het instellen van een algoritme inschakelen. Ze zijn getallen die invloed hebben op het algoritme gedrag, zoals fouttolerantie of het aantal iteraties of opties tussen varianten van het gedrag van de algoritme. De trainingstijd en de nauwkeurigheid van het algoritme kunnen soms worden behoorlijk gevoelig zijn voor het ophalen van alleen de juiste instellingen. Algoritmen met grote aantallen parameters moeten normaal de meeste vallen en opstaan vinden een goede combinatie.

Er is ook een [parameter sweeping](algorithm-parameters-optimize.md) module blok in Azure Machine Learning die probeert automatisch alle parametercombinaties ongeacht samenvattingen die u kiest. Dit is een uitstekende manier om ervoor te zorgen dat u de parameter-ruimte hebt omspannen, verhoogt de benodigde tijd voor het model trainen exponentieel met het aantal parameters.

De opwaartse is dat veel parameters doorgaans met geeft aan dat een algoritme meer flexibiliteit heeft. Zeer goede nauwkeurigheid ontstaan vaak. Opgegeven vindt u de juiste combinatie van instellingen.

### <a name="number-of-features"></a>Aantal functies
Voor bepaalde soorten gegevens, het aantal functies kan erg groot worden vergeleken met het aantal gegevenspunten. Dit is vaak het geval is bij genetica of tekstgegevens. Het grote aantal functies kunt bog omlaag sommige learning-algoritmen waardoor training unfeasibly lange tijd. Ondersteuning Vector Machines zijn bijzonder geschikt voor deze aanvraag (Zie hieronder).

### <a name="special-cases"></a>Bijzondere gevallen
Sommige learning-algoritmen zorg bepaalde veronderstellingen over de structuur van de gegevens of de gewenste resultaten. Als u een die past bij uw behoeften vinden kunt, krijgt deze u meer bruikbare resultaten, meer nauwkeurige prognoses of sneller training.

| **Algoritme** | **Nauwkeurigheid** | **Trainingstijd** | **Lineariteit** | **Parameters** | **Opmerkingen bij de** |
| --- |:---:|:---:|:---:|:---:| --- |
| **Classificatie van de twee-klasse** | | | | | |
| [logistic regression](https://msdn.microsoft.com/library/azure/dn905994.aspx) | |● |● |5 | |
| [besluit forest](https://msdn.microsoft.com/library/azure/dn906008.aspx) |● |○ | |6 | |
| [besluit jungle](https://msdn.microsoft.com/library/azure/dn905976.aspx) |● |○ | |6 |Lage geheugengebruik: |
| [gestimuleerd beslissingsstructuur](https://msdn.microsoft.com/library/azure/dn906025.aspx) |● |○ | |6 |Groot geheugengebruik: |
| [neurale netwerk](https://msdn.microsoft.com/library/azure/dn905947.aspx) |● | | |9 |[Extra aanpassingen zijn mogelijk](http://go.microsoft.com/fwlink/?LinkId=402867) |
| [gemiddelde perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx) |○ |○ |● |4 | |
| [ondersteuning voor vectormachine](https://msdn.microsoft.com/library/azure/dn905835.aspx) | |○ |● |5 |Goede voor grote functiesets |
| [lokaal grondige ondersteuning vectormachine](https://msdn.microsoft.com/library/azure/dn913070.aspx) |○ | | |8 |Goede voor grote functiesets |
| [De Bayes point machine](https://msdn.microsoft.com/library/azure/dn905930.aspx) | |○ |● |3 | |
| **Meerdere klasse-classificatie** | | | | | |
| [logistic regression](https://msdn.microsoft.com/library/azure/dn905853.aspx) | |● |● |5 | |
| [besluit forest](https://msdn.microsoft.com/library/azure/dn906015.aspx) |● |○ | |6 | |
| [besluit jungle](https://msdn.microsoft.com/library/azure/dn905963.aspx) |● |○ | |6 |Lage geheugengebruik: |
| [neurale netwerk](https://msdn.microsoft.com/library/azure/dn906030.aspx) |● | | |9 |[Extra aanpassingen zijn mogelijk](http://go.microsoft.com/fwlink/?LinkId=402867) |
| [One-v-all](https://msdn.microsoft.com/library/azure/dn905887.aspx) |- |- |- |- |Controleer de eigenschappen van de twee klassenmethode geselecteerd |
| **Regressie** | | | | | |
| [Lineair](https://msdn.microsoft.com/library/azure/dn905978.aspx) | |● |● |4 | |
| [Bayesiaanse lineair](https://msdn.microsoft.com/library/azure/dn906022.aspx) | |○ |● |2 | |
| [besluit forest](https://msdn.microsoft.com/library/azure/dn905862.aspx) |● |○ | |6 | |
| [gestimuleerd beslissingsstructuur](https://msdn.microsoft.com/library/azure/dn905801.aspx) |● |○ | |5 |Groot geheugengebruik: |
| [snelle forest kwantiel](https://msdn.microsoft.com/library/azure/dn913093.aspx) |● |○ | |9 |Distributies in plaats van punt voorspellingen |
| [neurale netwerk](https://msdn.microsoft.com/library/azure/dn905924.aspx) |● | | |9 |[Extra aanpassingen zijn mogelijk](http://go.microsoft.com/fwlink/?LinkId=402867) |
| [POISSON](https://msdn.microsoft.com/library/azure/dn905988.aspx) | | |● |5 |Technisch logboek-lineaire. Voor het voorspellen van aantallen |
| [rangtelwoord](https://msdn.microsoft.com/library/azure/dn906029.aspx) | | | |0 |Voor het voorspellen van positie ordenen |
| **Detectie van afwijkingen** | | | | | |
| [ondersteuning voor vectormachine](https://msdn.microsoft.com/library/azure/dn913103.aspx) |○ |○ | |2 |Met name geschikt voor grote functiesets |
| [PCA-gebaseerd anomaliedetectie](https://msdn.microsoft.com/library/azure/dn913102.aspx) | |○ |● |3 | |
| [K-means](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/) | |○ |● |4 |Een clustering-algoritme |

**Algoritme-eigenschappen:**

**●** -uitstekende nauwkeurigheid, snelle training tijden en het gebruik van lineariteit toont

**○** -goede nauwkeurigheid en matig training tijden toont

## <a name="algorithm-notes"></a>Opmerkingen bij de algoritme
### <a name="linear-regression"></a>Lineaire regressie
Zoals eerder vermeld [lineaire regressie](https://msdn.microsoft.com/library/azure/dn905978.aspx) geschikt is voor een regel (of vlak of hyperplane) in de gegevensset. Er is een werkpaard, eenvoudig en snel, maar dit kan zeer eenvoudig voor bepaalde problemen.
Schakel dit selectievakje in voor een [lineaire regressie zelfstudie](linear-regression-in-azure.md).

![Gegevens met een lineaire trend][3]

***Gegevens met een lineaire trend***

### <a name="logistic-regression"></a>Logistic regression
Hoewel er verwarrende 'regression' in de naam bevat, is logistic regression daadwerkelijk een krachtig hulpprogramma voor [tweeklasse](https://msdn.microsoft.com/library/azure/dn905994.aspx) en [multiklasse](https://msdn.microsoft.com/library/azure/dn905853.aspx) classificatie. Het is snel en eenvoudig. Het feit dat wordt gebruikt een van '-gevormde curve in plaats van een lineaire maakt het naadloos voor gegevens verdelen in groepen. Logistic regression biedt lineaire klassengrenzen zorg wanneer u gebruikt, er dus dat een lineaire benadering is iets dat die u kunt leven.

![Logistic regression tweeklasse gegevens met één functie][4]

***Een logistic regression tweeklasse gegevens met één functie*** *-grens van de klasse is het punt waarop de logistic curve is net zo dicht mogelijk bij beide klassen*

### <a name="trees-forests-and-jungles"></a>Structuren en forests jungles
Besluit forests ([regressie](https://msdn.microsoft.com/library/azure/dn905862.aspx), [tweeklasse](https://msdn.microsoft.com/library/azure/dn906008.aspx), en [multiklasse](https://msdn.microsoft.com/library/azure/dn906015.aspx)), besluit jungles ([tweeklasse](https://msdn.microsoft.com/library/azure/dn905976.aspx) en [multiklasse](https://msdn.microsoft.com/library/azure/dn905963.aspx)), en boosted decision trees ([regressie](https://msdn.microsoft.com/library/azure/dn905801.aspx) en [tweeklasse](https://msdn.microsoft.com/library/azure/dn906025.aspx)) zijn alle op basis van beslissingsstructuren, een fundamenteel machine learning-concept. Er zijn veel varianten van beslissingsstructuren, maar ze allemaal dezelfde dingen doen: de functie ruimte onderverdelen in regio's met grotendeels hetzelfde label. Deze kunnen gebieden van consistente categorie of van een constante waarde, afhankelijk van of er zijn geen classificatie- of regressiemodel zijn.

![Beslissingsstructuur verdeelt een spatie functie][5]

***Een beslissingsstructuur verdeelt de ruimte van een functie in regio's van ongeveer uniform waarden***

Omdat een spatie functie kan worden onderverdeeld in willekeurig kleine regio's, is het eenvoudig kunnen voorstellen fijn genoeg delen één gegevenspunt per regio hebben. Dit is een uiterst voorbeeld van te. Om te voorkomen dat dit, zijn een groot aantal structuren speciale wiskundige zorgvuldig genomen samengesteld dat de structuren niet zijn gecorreleerd. Het gemiddelde van deze 'besluit forest' is een structuur die wordt te voorkomen. Besluit forests kunnen veel geheugen gebruiken. Decision jungles zijn een variant die minder geheugen ten koste van de iets meer trainingstijd verbruikt.

Te voorkomen gestimuleerd beslissingsstructuren door te beperken hoe vaak ze kunnen onderverdelen en hoe weinig gegevenspunten zijn toegestaan in elke regio. Het algoritme vormt een reeks van structuren, die elk leert om te compenseren voor de fout die door de structuur voordat achtergelaten. Het resultaat is een zeer nauwkeurig cursist die doorgaans veel geheugen gebruiken. Bekijk voor de volledige technische beschrijving [Friedman van oorspronkelijke papier](http://www-stat.stanford.edu/~jhf/ftp/trebst.pdf).

[Snelle forest kwantiel regressie](https://msdn.microsoft.com/library/azure/dn913093.aspx) is een variatie beslissingsstructuren bijzondere gevallen waarin u wilt weten niet alleen de typische () mediaanwaarde van de gegevens in een regio, maar ook de distributie in de vorm van quantiles.

### <a name="neural-networks-and-perceptrons"></a>Neural networks en perceptrons
Neural networks zijn brain-geïnspireerd learning-algoritmen die betrekking hebben op [multiklasse](https://msdn.microsoft.com/library/azure/dn906030.aspx), [tweeklasse](https://msdn.microsoft.com/library/azure/dn905947.aspx), en [regressie](https://msdn.microsoft.com/library/azure/dn905924.aspx) problemen. Ze worden geleverd in een oneindige RAS, maar de neural netwerken in Azure Machine Learning zijn alle van de vorm van een gerichte acyclische grafieken. Dat betekent dat invoer functies worden doorgegeven doorsturen (nooit terug) via een reeks van lagen voordat het wordt omgezet in de uitvoer. In elke laag zijn ingangen gewogen in verschillende combinaties, opgeteld en doorgegeven aan de volgende laag. Deze combinatie van eenvoudige berekeningen resulteert in de mogelijkheid om te leren van geavanceerde klasse grenzen en gegevens trends schijnbaar door verwerkt Magic-pakket. Veel gelaagd netwerken van de sortering uitvoeren de 'grondige kennis' die helpt u bij het zo veel technische rapportage en sciencefiction.

Deze hoge prestaties niet afkomstig zijn gratis al. Neural networks kunnen lang duren om te trainen, met name voor grote gegevenssets met een groot aantal functies. Ze hebben ook meer parameters dan de meeste algoritmen, wat betekent dat parameter sweeping wordt uitgebreid de trainingstijd veel.
En voor deze overachievers die willen [opgeven van hun eigen netwerkstructuur](http://go.microsoft.com/fwlink/?LinkId=402867), de mogelijkheden zijn inexhaustible.

![Grenzen geleerd door neural networks][6]
***de grenzen van neural networks geleerd complex zijn en onregelmatige***

De [tweeklasse gemiddeld perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx) neural networks antwoord op de training keren enorm is. Een netwerkstructuur waarmee lineaire klassengrenzen wordt gebruikt. Is bijna primitieve door de hedendaagse standaarden, maar er is een lang geschiedenisgegevens krachtig te werken en klein genoeg zijn voor meer informatie over snel.

### <a name="svms"></a>SVMs
Ondersteuning vector machines (SVMs) vinden de grens die klassen door als breed marge mogelijk scheidt. Wanneer twee klassen duidelijk kunnen niet worden gescheiden, vinden de algoritmen voor de beste grens die ze kunnen. Zoals vermeld in Azure Machine Learning de [tweeklasse SVM](https://msdn.microsoft.com/library/azure/dn905835.aspx) doet dit met een lineaire. (In SVM spreekt, worden er gebruikt een lineaire kernel.) Omdat deze lineaire aanpassing maakt, is het vrij snel uitvoeren. Waar deze uitkomst is met de functie intensief gegevens, zoals tekst of Toepassingsgeoriënteerde. In dergelijke gevallen kunnen SVMs afzonderlijke klassen sneller en met minder te dan de meeste andere algoritmen, naast het vereisen van een kleine hoeveelheid geheugen.

![Ondersteuning vector machine klasse grens][7]

***De grens van een typische ondersteuning vector machine klasse maximaliseert de marge tussen twee klassen***

Een ander product van Microsoft Research de [tweeklasse lokaal grondige SVM](https://msdn.microsoft.com/library/azure/dn913070.aspx) is een niet-lineaire variant van SVM die de meeste van de snelheid en efficiëntie van de lineaire versie behoudt. Dit is ideaal voor gevallen waarin de lineaire aanpak nauwkeurig genoeg antwoorden niet geven. De ontwikkelaars bewaard deze snel door het probleem in een aantal kleine lineaire SVM problemen op te splitsen. Lees de [volledige beschrijving](http://research.microsoft.com/um/people/manik/pubs/Jose13.pdf) voor een overzicht van hoe ze opgehaald uit deze methode.

De extensie van niet-lineaire SVMs slimme de [één klasse SVM](https://msdn.microsoft.com/library/azure/dn913103.aspx) een grens die nauw geeft een overzicht van de volledige gegevensset wordt getekend. Dit is handig voor afwijkingsdetectie. Nieuwe gegevenspunten die ver buiten die grens vallen zijn ongebruikelijke opmerkelijk moet.

### <a name="bayesian-methods"></a>Bayesiaanse methoden
Bayesiaanse methoden hebben een zeer wenselijk kwaliteit: ze te voorkomen. Ze doen dit met bepaalde veronderstellingen tevoren over de waarschijnlijke distributie van het antwoord. Een andere bijkomend gevolg van deze benadering is dat ze heel weinig parameters hebben. Azure Machine Learning heeft beide Bayesiaanse algoritmen voor beide classificatie (['Two-class Bayes point machine](https://msdn.microsoft.com/library/azure/dn905930.aspx)) en regressie ([Bayesiaanse lineaire regressie](https://msdn.microsoft.com/library/azure/dn906022.aspx)).
Houd er rekening mee dat deze wordt ervan uitgegaan dat de gegevens kunnen worden gesplitst of aan een lineaire voldoen.

De Bayes point-machines zijn op een historische notitie ontwikkeld door Microsoft Research. Ze hebben sommige uitzonderlijk mooi vormgegeven theoretische werk achterliggende. De geïnteresseerd student wordt omgeleid naar de [oorspronkelijke artikel in JMLR](http://jmlr.org/papers/volume1/herbrich01a/herbrich01a.pdf) en een [begrijpelijke manier mee blog door Chris Bishop](http://blogs.technet.com/b/machinelearning/archive/2014/10/30/embracing-uncertainty-probabilistic-inference.aspx).

### <a name="specialized-algorithms"></a>Gespecialiseerde algoritmen
Als u een zeer specifiek doel hebt u mogelijk mee. Zijn binnen de verzameling Azure Machine Learning-algoritmen die zijn gespecialiseerd in:

- voorspelling rangschikken ([ordinale regressie](https://msdn.microsoft.com/library/azure/dn906029.aspx)),
- voorspelling tellen ([Poisson regressie](https://msdn.microsoft.com/library/azure/dn905988.aspx)),
- afwijkingsdetectie (één op basis van [analyse van de belangrijkste onderdelen](https://msdn.microsoft.com/library/azure/dn913102.aspx) en op basis van een [vectormachine ondersteuning](https://msdn.microsoft.com/library/azure/dn913103.aspx)s)
- Clustering ([K-means](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/))

![PCA-gebaseerd anomaliedetectie][8]

***PCA-gebaseerd anomaliedetectie*** *-de grote meerderheid van de gegevens worden onderverdeeld in een stereotypical verdeling; punten die aanzienlijk afwijken van dit distributiepunt verdacht zijn*

![Gegevensset met K-means gegroepeerd][9]

***Een gegevensset worden onderverdeeld in vijf clusters met K-means***

Er is ook een ensemble [one-v-all multiklassen classificatie](https://msdn.microsoft.com/library/azure/dn905887.aspx), waarmee het probleem van de classificatie N klasse opgesplitst in N-1 tweeklasse classificatie problemen. De nauwkeurigheid en trainingstijd lineariteit eigenschappen worden bepaald door de twee klasse classificaties gebruikt.

![Twee klasse classificaties gecombineerd om een classificatie drie-klasse][10]

***Een combinatie van twee klasse classificaties combineren om een classificatie drie-klasse***

Azure Machine Learning bevat ook toegang tot een krachtige machine learning-framework onder de titel van [Vowpal Wabbit](https://msdn.microsoft.com/library/azure/8383eb49-c0a3-45db-95c8-eb56a1fef5bf).
Een code defies categorisatie hier, omdat deze zowel classificatie en regressie problemen lezen kan en zelfs van gegevens gedeeltelijk zonder label leren kan. U kunt het gebruik van een van een aantal learning-algoritmen, verlies van functies en optimalisatie algoritmen configureren. Is ontworpen compleet efficiënt, parallelle en zeer snel. Heel groot functiesets moeiteloos duidelijk verwerkt.
Gestart en door Microsoft Research van eigen John Langford geleid, is een code een formule één vermelding in een veld van stock auto-algoritmen. Niet elk probleem geschikt is voor een code, maar als u het geval is, wordt de moeite verstuurd het leerproces toe op de interface waard. Het is ook beschikbaar als [zelfstandige open-source code](https://github.com/JohnLangford/vowpal_wabbit) in verschillende talen.

## <a name="more-help-with-algorithms"></a>Meer hulp bij algoritmen
* Zie voor een downloadbare infographic die beschrijving algoritmen en voorbeelden, [downloadbare Infographic: Machine learning-algoritme voorbeelden voor alledaagse](basics-infographic-with-algorithm-examples.md).
* Zie voor een lijst door de categorie van alle de machine learning-algoritmen beschikbaar zijn in Azure Machine Learning Studio [Model initialiseren] [ initialize-model] in de Machine Learning Studio algoritme en de Module Help.
* Zie voor een volledige alfabetische lijst van algoritmen en modules in Azure Machine Learning Studio [lijst A-Z van Machine Learning Studio-modules] [ a-z-list] in de Help van de Module en Machine Learning Studio-algoritme.
* Als u wilt downloaden en afdrukken van een diagram waarin u een van de mogelijkheden van Azure Machine Learning Studio overzicht, Zie [overzichtsdiagram van de mogelijkheden van Azure Machine Learning Studio](studio-overview-diagram.md).


<!-- Reference links -->
[initialize-model]: https://msdn.microsoft.com/library/azure/dn905812.aspx
[a-z-list]: https://msdn.microsoft.com/library/azure/dn906033.aspx

<!-- Media -->

[1]: ./media/algorithm-choice/image1.png
[2]: ./media/algorithm-choice/image2.png
[3]: ./media/algorithm-choice/image3.png
[4]: ./media/algorithm-choice/image4.png
[5]: ./media/algorithm-choice/image5.png
[6]: ./media/algorithm-choice/image6.png
[7]: ./media/algorithm-choice/image7.png
[8]: ./media/algorithm-choice/image8.png
[9]: ./media/algorithm-choice/image9.png
[10]: ./media/algorithm-choice/image10.png
