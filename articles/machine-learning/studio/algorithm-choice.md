---
title: Algoritmes kiezen
titleSuffix: Azure Machine Learning Studio
description: Klik hier voor meer informatie over het kiezen van Azure Machine Learning Studio-algoritmen voor leren met en zonder supervisie in clustering-, classificatie- of regressiemodel experimenten.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: previous-ms.author=pakalra, previous-author=pakalra
ms.date: 03/04/2019
ms.openlocfilehash: 3bb88f2f9546ec25433061a0704bd144730bd34c
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57894628"
---
# <a name="how-to-choose-algorithms-for-azure-machine-learning-studio"></a>Algoritmen kiezen voor Azure Machine Learning Studio

Het antwoord op de vraag 'Welke machine learning-algoritme moet ik gebruiken?' is altijd "Dat hangt ervan af." Dit is afhankelijk van de grootte, de kwaliteit en de aard van de gegevens. Dit is afhankelijk van wat u wilt doen met het antwoord. Dat hangt ervan af op hoe de berekening van de algoritme is vertaald naar instructies voor de computer die u gebruikt. En dit is afhankelijk van hoeveel tijd die u hebt. Zelfs de meest ervaren gegevenswetenschappers kunnen niet zien welk algoritme beste voordat u ze uitvoert.

Machine Learning Studio biedt geavanceerde algoritmen, zoals schaalbare beslissingsstructuren, Bayesiaanse aanbevelingssystemen, Deep Neural Networks en Decision Jungles die zijn ontwikkeld door Microsoft Research. Ook bevat het schaalbare open-source machine learning-pakketten, zoals Vowpal Wabbit. Machine Learning Studio ondersteunt machine learning-algoritmen voor multiklassen en binaire classificatie, regressie en clusters. Bekijk de volledige lijst van [Machine Learning-Modules](/azure/machine-learning/studio-module-reference/index).
De documentatie biedt informatie over elk algoritme en de parameters voor het optimaliseren van de algoritmen kunt afstemmen.  


## <a name="the-machine-learning-algorithm-cheat-sheet"></a>Tips voor het Machine Learning-algoritme

De **[Microsoft Azure Machine Learning Studio-algoritme Cheat Sheet](algorithm-cheat-sheet.md)** helpt bij het kiezen van het recht voor machine learning-algoritme voor uw predictive analytics-oplossingen uit de Azure Machine Learning Studio-bibliotheek met algoritmen.
In dit artikel leert u hoe u deze overzichtskaart gebruiken.

> [!NOTE]
> Voor het downloaden van het referentiemateriaal voor en in dit artikel volgen, gaat u naar [Machine learning-overzichtskaart algoritmen voor Microsoft Azure Machine Learning Studio](algorithm-cheat-sheet.md).
> 
> 

Deze overzichtskaart is een zeer specifieke doelgroep in gedachten: begin gegevenswetenschapper met Bachelor-niveau van machine learning, probeert te beginnen met een algoritme kiezen in Azure Machine Learning Studio. Dit betekent dat dat het enige generalisaties en oversimplifications maakt, maar u in een veilige richting verwijst. Het betekent ook dat er veel van de algoritmen die hier niet vermeld zijn.

Deze aanbevelingen zijn gecompileerde feedback en tips van veel data scientists en machine learning-experts. Alles wat niet is geaccepteerd, maar we hebben geprobeerd aan onze mening harmoniseren in een ruwe consensus. De meeste van de instructies van geschil beginnen met 'Dat hangt ervan af...'

### <a name="how-to-use-the-cheat-sheet"></a>Het gebruik van het referentiemateriaal voor

Lezen van het pad en de algoritme labels op de grafiek als ' voor  *&lt;pad label&gt;*, gebruikt u  *&lt;algoritme&gt;*. " Bijvoorbeeld, "voor *snelheid*, gebruikt u *twee logistieke regressie klasse*." Soms meer dan één vertakking is van toepassing.
Geen van beide zijn soms een perfecte maat. Ze zijn bedoeld om te worden van de regel van de miniatuur aanbevelingen, dus hoeft hiervoor niets erop vertrouwen dat u precies.
Verschillende gegevenswetenschappers die we hebben het gehad met genoemde die de enige manier om te zoeken, het beste algoritme is om te proberen ze allemaal.

Hier volgt een voorbeeld van de [Azure AI Gallery](https://gallery.azure.ai/) van een experiment dat probeert verschillende algoritmes op basis van dezelfde gegevens en vergelijkt de resultaten: [Vergelijk ROC classificaties: Herkenning letter](https://gallery.azure.ai/Details/a635502fc98b402a890efe21cec65b92).

> [!TIP]
> Zie voor het downloaden van een overzicht van de infographic eenvoudig te begrijpen van de basisprincipes van machine learning voor meer informatie over populaire algoritmen gebruikt om veelvoorkomende machine learning vragen te beantwoorden [Machine learning-basisbeginselen met algoritmevoorbeelden](basics-infographic-with-algorithm-examples.md).

## <a name="flavors-of-machine-learning"></a>Versies van machine learning

### <a name="supervised"></a>Onder supervisie

Leren met supervisie algoritmen voorspellingen op basis van een set voorbeelden. Historische aandelenkoersen kunnen bijvoorbeeld worden gebruikt om wat over de prijzen in de toekomst. Elk voorbeeld dat wordt gebruikt voor de training is gelabeld met de waarde van belang zijn, in dit geval de aandelenkoers. Een algoritme voor leren met supervisie zoekt naar patronen in de waardelabels. Alle gegevens die mogelijk relevant kunnen worden gebruikt, de dag van de week, de seizoen, financiële gegevens van het bedrijf, het type van de branche, de aanwezigheid van de geopolitieke storingen, en elk algoritme voor verschillende soorten patronen. Nadat de algoritme de beste patroon heeft gevonden dat kan, het patroon gebruikt voor het maken van voorspellingen voor niet-gelabelde testgegevens — van morgen prijzen.

Leren met supervisie is een populaire en nuttige van machine learning. Met één uitzondering, alle modules in Azure Machine Learning Studio onder supervisie staan learning-algoritmen. Er zijn verschillende specifieke typen van leren met supervisie die worden weergegeven in Azure Machine Learning Studio: classificatie, regressie en anomaliedetectie.

* **Classificatie**. Wanneer de gegevens worden gebruikt voor het voorspellen van een categorie, leren met supervisie is een afkorting classificatie. Dit is het geval bij het toewijzen van een afbeelding als een afbeelding van een 'cat' of een 'hond'. Wanneer er slechts twee keuzes, heet dit **twee klassen** of **binomiale classificatie**. Wanneer er meer categorieën zijn als bij het voorspellen van de winnaar van het NCAA March Madness-toernooi, dit probleem wordt ook wel **ROC-classificatie**.
* **Regressie**. Als een waarde wordt net als bij aandelenkoersen wordt voorspeld, leren met supervisie regressie genoemd.
* **Detectie van afwijkingen**. Soms is het doel is om te identificeren van de gegevenspunten die gewoon ongebruikelijk zijn. Fraude te detecteren, bijvoorbeeld een zeer ongebruikelijke creditcard bestedingslimiet patronen zijn verdacht zijn gemarkeerd. De mogelijke variaties zijn dus veel en lijkt op de training voorbeelden zo weinig, is niet haalbaar voor meer informatie over welke frauduleuze activiteit. De aanpak die detectie van afwijkingen is om de informatie over welke normale activiteit uitziet (met behulp van een geschiedenis van niet-frauduleuze transacties) identificeren iets dat is heel anders.

### <a name="unsupervised"></a>Niet onder supervisie

Leren met zonder supervisie hebben gegevenspunten geen labels worden gekoppeld. In plaats daarvan is het doel van een algoritme zonder supervisie learning om de gegevens op een bepaalde manier te ordenen of om te beschrijven van de structuur. Dit kan betekenen dat het groeperen in clusters of het vinden van complexe gegevens kijken, zodat deze wordt weergegeven eenvoudiger of meer geordend op verschillende manieren.

### <a name="reinforcement-learning"></a>Bekrachtigingen

In de versterkend leren, haalt de algoritme een actie te kiezen in reactie op elk gegevenspunt. Het learning-algoritme ontvangt ook een signaal beloning korte tijd later, dat aangeeft hoe goed de beslissing is.
Het algoritme wijzigt op basis hiervan de strategie om te realiseren van de hoogste prijs. Er zijn momenteel geen leren met bekrachtigingen beschikt learning-algoritme modules in Azure Machine Learning Studio. Bekrachtigingen is gebruikelijk in robotiek, waarbij de set sensorwaarden, serverlogs, op één punt in tijd een gegevenspunt, en het algoritme moet de volgende actie van de robot kiezen. Het is ook dat een natuurlijke geschikt voor Internet of Things toepassingen.

## <a name="considerations-when-choosing-an-algorithm"></a>Overwegingen bij het kiezen van een algoritme

### <a name="accuracy"></a>Nauwkeurigheid

Aan de meest nauwkeurige antwoord mogelijk niet altijd nodig.
Soms is een benadering voldoende, afhankelijk van wat u wilt gebruiken. Als dit het geval is, kunt u mogelijk de verwerkingstijd aanzienlijk knippen door meer geschatte methoden bevalt. Een ander voordeel van meer geschatte methoden is dat ze op een natuurlijke manier om te voorkomen dat te vaak.

### <a name="training-time"></a>Trainingstijd

Het aantal minuten of uren die nodig zijn voor een model te trainen, verschilt geweldige deals algoritmen. Tijd training is vaak nauw gekoppeld aan nauwkeurigheid: een doorgaans meegestuurd met de andere. Bovendien zijn sommige algoritmen gevoeliger voor het aantal gegevenspunten dan andere.
Als de tijd beperkt is kan het de keuze van de algoritme, station met name wanneer de gegevensset groot is.

### <a name="linearity"></a>Lineariteit

Veel machine learning-algoritmen maken gebruik van lineariteit. Lineaire classificatie algoritmen wordt ervan uitgegaan dat klassen kunnen worden gescheiden door een rechte lijn (of de hogere-dimensionale analoge). Deze omvatten logistieke regressie en ondersteuning voor machines vector (zoals geïmplementeerd in Azure Machine Learning Studio).
Lineaire regressie algoritmen wordt ervan uitgegaan dat gegevenstrends gaat u als een rechte lijn volgt. Deze veronderstellingen niet nadelig voor sommige problemen, maar op andere ze uitvallen nauwkeurigheid.

![Niet-lineaire klasse grens](./media/algorithm-choice/image1.png)

***Niet-lineaire klasse grens*** *-afhankelijk zijn van een lineaire classificatiealgoritme zou leiden tot nauwkeurigheid laag*

![Gegevens met een niet-lineaire trend](./media/algorithm-choice/image2.png)

***Gegevens met een niet-lineaire trend*** *-met behulp van een lineaire regressie-methode genereert veel fouten groter is dan nodig*

Ondanks de gevaren zijn lineaire algoritmen erg populair als een eerste regel van de aanval. Ze zijn meestal algoritmisch eenvoudig en snel te trainen.

### <a name="number-of-parameters"></a>Aantal parameters

Parameters zijn de knoppen die een gegevenswetenschapper om in te schakelen bij het instellen van een algoritme opgehaald. Ze zijn getallen die invloed hebben op van het algoritme gedrag, zoals fouttolerantie of het aantal iteraties of opties tussen varianten van het gedrag van het algoritme. De trainingstijd en de nauwkeurigheid van het algoritme kunnen soms zijn erg gevoelig zijn voor het ophalen van alleen de juiste instellingen. Algoritmen met een groot aantal parameters moeten normaal de meeste vallen en opstaan te vinden van een goede combinatie.

Er is ook een [parameter sweeping](algorithm-parameters-optimize.md) module blok in Azure Machine Learning Studio die probeert automatisch alle combinaties van parameters aan de granulariteit die u kiest. Dit is een uitstekende manier om ervoor te zorgen dat u de parameter-ruimte hebt omspannen, verhoogt de tijd die nodig is voor een model te trainen exponentieel met het aantal parameters.

Het voordeel is dat veel parameters doorgaans met geeft aan dat een algoritme nog meer flexibiliteit heeft. Zeer goed nauwkeurigheid kunt vaak bereiken mits u de juiste combinatie van instellingen voor de stuurprogrammaparameter kunt vinden.

### <a name="number-of-features"></a>Aantal functies

Voor bepaalde typen gegevens, het aantal functies kan erg groot worden vergeleken met het aantal gegevenspunten. Dit is vaak het geval is bij genetica of tekstuele gegevens. Het grote aantal functies kan verwarrend omlaag sommige learning-algoritmen, zodat de training unfeasibly lange tijd. Support Vector Machines zijn bijzonder goed geschikt voor deze aanvraag (Zie hieronder).

### <a name="special-cases"></a>Speciale gevallen

Sommige learning-algoritmen moeten u bepaalde veronderstellingen over de structuur van de gegevens of de gewenste resultaten. Als u een die past bij uw behoeften vinden kunt, geeft u nog meer nuttige resultaten, nauwkeurigere voorspellingen of sneller training tijden.

| **Algoritme** | **nauwkeurigheid** | **Trainingstijd** | **Lineariteit** | **Parameters** | **Opmerkingen** |
| --- |:---:|:---:|:---:|:---:| --- |
| **Classificatie van twee klassen** | | | | | |
| [Logistieke regressie](/azure/machine-learning/studio-module-reference/two-class-logistic-regression) | |● |● |5 | |
| [beslissingsforest met](/azure/machine-learning/studio-module-reference/two-class-decision-forest) |● |○ | |6 | |
| [besluit jungle](/azure/machine-learning/studio-module-reference/two-class-decision-jungle) |● |○ | |6 |Het geheugengebruik van laag |
| [boosted-beslisboom](/azure/machine-learning/studio-module-reference/two-class-boosted-decision-tree) |● |○ | |6 |Het geheugengebruik van grote |
| [neurale netwerk](/azure/machine-learning/studio-module-reference/two-class-neural-network) |● | | |9 |[Aanvullende aanpassing is mogelijk](azure-ml-netsharp-reference-guide.md) |
| [gemiddelde perceptron](/azure/machine-learning/studio-module-reference/two-class-averaged-perceptron) |○ |○ |● |4 | |
| [voor ondersteuningsvectormachines](/azure/machine-learning/studio-module-reference/two-class-support-vector-machine) | |○ |● |5 |Goed voor grote functiesets |
| [lokaal diep voor ondersteuningsvectormachines](/azure/machine-learning/studio-module-reference/two-class-locally-deep-support-vector-machine) |○ | | |8 |Goed voor grote functiesets |
| [De Bayes point machine](/azure/machine-learning/studio-module-reference/two-class-bayes-point-machine) | |○ |● |3 | |
| **Meerdere klasse-classificatie** | | | | | |
| [Logistieke regressie](/azure/machine-learning/studio-module-reference/multiclass-logistic-regression) | |● |● |5 | |
| [beslissingsforest met](/azure/machine-learning/studio-module-reference/multiclass-decision-forest) |● |○ | |6 | |
| [besluit jungle](/azure/machine-learning/studio-module-reference/multiclass-decision-jungle) |● |○ | |6 |Het geheugengebruik van laag |
| [neurale netwerk](/azure/machine-learning/studio-module-reference/multiclass-neural-network) |● | | |9 |[Aanvullende aanpassing is mogelijk](azure-ml-netsharp-reference-guide.md) |
| [one-v-all](/azure/machine-learning/studio-module-reference/one-vs-all-multiclass) |- |- |- |- |Controleer de eigenschappen van de twee klassen methode geselecteerd |
| **Regressie** | | | | | |
| [linear](/azure/machine-learning/studio-module-reference/linear-regression) | |● |● |4 | |
| [Bayesiaanse lineair](/azure/machine-learning/studio-module-reference/bayesian-linear-regression) | |○ |● |2 | |
| [beslissingsforest met](/azure/machine-learning/studio-module-reference/decision-forest-regression) |● |○ | |6 | |
| [boosted-beslisboom](/azure/machine-learning/studio-module-reference/boosted-decision-tree-regression) |● |○ | |5 |Het geheugengebruik van grote |
| [snelle forest kwantiel](/azure/machine-learning/studio-module-reference/fast-forest-quantile-regression) |● |○ | |9 |Distributies in plaats van punt voorspellingen |
| [neurale netwerk](/azure/machine-learning/studio-module-reference/neural-network-regression) |● | | |9 |[Aanvullende aanpassing is mogelijk](azure-ml-netsharp-reference-guide.md) |
| [Poisson](/azure/machine-learning/studio-module-reference/poisson-regression) | | |● |5 |Technisch log-lineaire. Voor het voorspellen van tellingen |
| [Volgnummer](/azure/machine-learning/studio-module-reference/ordinal-regression) | | | |0 |Voor het voorspellen van positie-volgorde worden toegepast |
| **Detectie van afwijkingen** | | | | | |
| [voor ondersteuningsvectormachines](/azure/machine-learning/studio-module-reference/one-class-support-vector-machine) |○ |○ | |2 |Met name geschikt voor grote functiesets |
| [PCA-gebaseerd anomaliedetectie](/azure/machine-learning/studio-module-reference/pca-based-anomaly-detection) | |○ |● |3 | |
| [K-means](/azure/machine-learning/studio-module-reference/k-means-clustering) | |○ |● |4 |Een clustering-algoritme |

**Eigenschappen van de algoritme:**

**●** -ziet u uitstekende nauwkeurigheid, snelle training tijden en het gebruik van lineariteit

**○** -ziet u goede nauwkeurigheid en gemiddeld training tijden

## <a name="algorithm-notes"></a>Opmerkingen bij de algoritme

### <a name="linear-regression"></a>Lineaire regressie

Zoals eerder vermeld [lineaire regressie](/azure/machine-learning/studio-module-reference/linear-regression) geschikt is voor een regel (of vlak of hyperplane) in de gegevensset. Het is een werkpaard, eenvoudig en snel, maar kan het zijn zeer eenvoudig voor bepaalde problemen.

![Gegevens met een lineaire trends](./media/algorithm-choice/image3.png)

***Gegevens met een lineaire trends***

### <a name="logistic-regression"></a>Logistieke regressie

Hoewel deze 'regression' in de naam bevat, logistieke regressie is eigenlijk een krachtig hulpprogramma voor [twee klassen](/azure/machine-learning/studio-module-reference/two-class-logistic-regression) en [multiklasse](/azure/machine-learning/studio-module-reference/multiclass-logistic-regression) classificatie. Het is snel en eenvoudig. Het feit dat wordt gebruikt een van '-gevormde curve in plaats van een rechte lijn maakt het een perfect geschikt voor het delen van gegevens in groepen. Grenzen voor logistieke regressie biedt lineaire klasse, zorg wanneer u gebruikt, er dus dat een lineaire benadering is iets wat die u kunt live met.

![Logistieke regressie tot gegevens van twee klassen met slechts één functie](./media/algorithm-choice/image4.png)

***Een logistic regression tot gegevens van twee klassen met slechts één functie*** *-de grens van de klasse is het punt waarop de logistieke curve is net zo dicht bij beide klassen*

### <a name="trees-forests-and-jungles"></a>Structuren en forests jungles

Besluit forests ([regressie](/azure/machine-learning/studio-module-reference/decision-forest-regression), [twee klassen](/azure/machine-learning/studio-module-reference/two-class-decision-forest), en [multiklasse](/azure/machine-learning/studio-module-reference/multiclass-decision-forest)), decision jungles ([twee klassen](/azure/machine-learning/studio-module-reference/two-class-decision-jungle) en [ multiklasse](/azure/machine-learning/studio-module-reference/multiclass-decision-jungle)), en heeft de klantenopbrengst beslisbomen ([regressie](/azure/machine-learning/studio-module-reference/boosted-decision-tree-regression) en [twee klassen](/azure/machine-learning/studio-module-reference/two-class-boosted-decision-tree)) zijn alle op basis van beslissingsstructuren, een fundamentele machine learning-concept. Er zijn veel varianten van beslissingsstructuren, maar ze allemaal met hetzelfde doen: de functie ruimte onderverdelen in regio's met grotendeels hetzelfde label. Dit kunnen de regio's van dezelfde categorie of van een constante waarde, afhankelijk van of u classificatie- of regressiemodel doen zijn.

![Beslissingsstructuur verdeelt een spatie functie](./media/algorithm-choice/image5.png)

***Een beslissingsstructuur verdeelt een functie-ruimte in de regio's van ongeveer uniform waarden***

Omdat de adresruimte van een functie kan worden onderverdeeld in willekeurig kleine regio's, is het eenvoudig te imagine fijn genoeg delen één gegevenspunt per regio hebben. Dit is een extreme voorbeeld van te. Een groot aantal structuren zijn om te voorkomen dat dit, samengesteld met speciale wiskundige zorg genomen om te controleren of de structuren niet worden gecorreleerd. Het gemiddelde van dit forest' besluit' is een structuur die wordt te voorkomen. Besluit forests kunnen een grote hoeveelheid geheugen gebruiken. Decision jungles zijn een variant die wordt verbruikt minder geheugen ten koste van iets meer trainingstijd in.

Te voorkomen boosted decision trees door te beperken hoe vaak ze kunnen onderverdelen en hoe gegevenspunten zijn toegestaan in elke regio. Het algoritme vormt een reeks structuren, die elk leert om te compenseren voor de fout die door de structuur voordat links. Het resultaat is een zeer nauwkeurige learner die doorgaans gebruik van een grote hoeveelheid geheugen. Bekijk voor de volledige technische beschrijving [Friedman het oorspronkelijke document](https://www-stat.stanford.edu/~jhf/ftp/trebst.pdf).

[Snelle forest kwantiel regressie](/azure/machine-learning/studio-module-reference/fast-forest-quantile-regression) is een variatie van beslissingsstructuren voor de speciaal geval waar u niet alleen de typische (gemiddelde) waarde van de gegevens binnen een regio, maar ook de distributie hiervan in de vorm van quantiles weten.

### <a name="neural-networks-and-perceptrons"></a>Neurale netwerken en perceptrons

Neurale netwerken zijn brein geïnspireerde learning-algoritmen die betrekking hebben op [multiklasse](/azure/machine-learning/studio-module-reference/multiclass-neural-network), [twee klassen](/azure/machine-learning/studio-module-reference/two-class-neural-network), en [regressie](/azure/machine-learning/studio-module-reference/neural-network-regression) problemen. Ze zijn verkrijgbaar in een oneindige verschillende, maar de neurale netwerken in Azure Machine Learning Studio zijn alle van de vorm van gerichte acyclische grafieken. Dit betekent dat dat invoerfuncties worden doorgegeven naar voren (nooit terug) via een reeks lagen voordat het wordt omgezet in de uitvoer. In elke laag worden invoer gewogen in verschillende combinaties, bij elkaar opgeteld en doorgegeven aan de volgende laag. Dankzij deze combinatie van eenvoudige berekeningen resulteert in de mogelijkheid om meer geavanceerde klasse grenzen en gegevens trends schijnbaar Magic-pakket. Netwerken veel lagen van de sortering voeren de 'deep learning' die helpt u bij het zo veel tech reporting en science fiction.

Deze hoge prestaties gratis niet, maar. Neurale netwerken kunnen erg lang duren om te trainen, met name voor grote gegevenssets met veel functies. Ze hebben ook meer parameters dan de meeste algoritmen, wat betekent dat parameter sweeping de trainingstijd geweldige deals uitgebreid.
En voor deze overachievers die willen [geven hun eigen netwerkstructuur](azure-ml-netsharp-reference-guide.md), de mogelijkheden zijn inexhaustible.

![Grenzen geleerd door neurale netwerken](./media/algorithm-choice/image6.png)

***De grenzen geleerd door neurale netwerken zijn complex en onregelmatige***

De [twee klassen gemiddelde perceptron](/azure/machine-learning/studio-module-reference/two-class-averaged-perceptron) neurale netwerken antwoord tot enorm training tijden. Maakt gebruik van een netwerkstructuur waarmee lineaire klassengrenzen. Het is bijna primitieve volgens de normen van vandaag, maar heeft een lange geschiedenis van databases robuuster worden werken en klein genoeg is voor meer informatie over snel.

### <a name="svms"></a>SVMs

Support vector machines (SVMs) vinden de grens die worden gescheiden van klassen door als een breed marge mogelijk. Wanneer de twee klassen kunnen duidelijk worden gescheiden, vinden de algoritmen voor de grens van de beste kunnen. Zoals vermeld in Azure Machine Learning Studio, de [twee klassen SVM](/azure/machine-learning/studio-module-reference/two-class-support-vector-machine) doet dit met een rechte lijn alleen (in SVM spreekt, worden er gebruikt een lineaire kernel).
Omdat deze lineaire aanpassing, is het kunnen vrij snel uitvoeren. Met de functie-intensieve gegevens, zoals tekst of genetische gegevens is waar deze uitkomst. In dergelijke gevallen kunnen SVMs voor het scheiden sneller en met minder te dan de meeste andere algoritmen, naast een kleine hoeveelheid geheugen vereisen.

![Support vector machine klasse grens](./media/algorithm-choice/image7.png)

***De grens van een typische support vector machine klasse maximaliseert de marge scheiden van twee klassen***

Een ander product van Microsoft Research, de [twee klassen lokaal diep SVM](/azure/machine-learning/studio-module-reference/two-class-locally-deep-support-vector-machine) is een niet-lineaire variant van SVM die de meeste van de snelheid en efficiëntie van de lineaire versie behoudt. Dit is ideaal voor gevallen waarin de lineaire methode nauwkeurig genoeg antwoorden geen geeft. De ontwikkelaars bewaard deze snel door het afbreken van het probleem in een aantal kleine lineaire SVM problemen. Lees de [volledige beschrijving](http://proceedings.mlr.press/v28/jose13.html) voor de informatie over hoe ze opgehaald uit deze methode.

Met behulp van een slimme uitbreiding van niet-lineaire SVMs de [één klasse SVM](/azure/machine-learning/studio-module-reference/one-class-support-vector-machine) tekent een grens die nauw geeft een overzicht van de volledige gegevensset. Dit is handig voor detectie van afwijkingen. Nieuwe gegevenspunten die tot nu toe buiten die grens vallen zijn ongebruikelijke opmerkelijk moet.

### <a name="bayesian-methods"></a>Bayesiaanse methoden

Bayesiaanse methoden hebben een zeer wenselijk kwaliteit: ze te voorkomen. Ze doen dit met bepaalde veronderstellingen vooraf over de waarschijnlijke distributie van het antwoord. Een andere bijkomend gevolg van deze benadering is dat er heel weinig parameters. Azure Machine Learning Studio heeft Bayesiaanse algoritmen voor beide classificatie (['Two-class Bayes point machine](/azure/machine-learning/studio-module-reference/two-class-bayes-point-machine)) en regressie ([Bayesiaanse lineaire regressie](/azure/machine-learning/studio-module-reference/bayesian-linear-regression)).
Houd er rekening mee dat deze wordt ervan uitgegaan dat de gegevens kunnen worden gesplitst of met een rechte lijn aanpassen.

Bayes point machines ontwikkeld op een historische Opmerking bij Microsoft Research. Ze hebben wat uitzonderlijk prachtige theoretische werk deze. De geïnteresseerd student wordt omgeleid naar de [oorspronkelijke artikel in JMLR](http://jmlr.org/papers/volume1/herbrich01a/herbrich01a.pdf) en een [inzichtelijke blog door Chris Bishop](https://blogs.technet.com/b/machinelearning/archive/2014/10/30/embracing-uncertainty-probabilistic-inference.aspx).

### <a name="specialized-algorithms"></a>Gespecialiseerde algoritmes
Als u beschikt over een zeer specifieke doel is het mogelijk dat de oplossing. In de verzameling van Azure Machine Learning Studio zijn er algoritmen die zijn gespecialiseerd in:

- rangschikking voorspelling ([ordinaal](/azure/machine-learning/studio-module-reference/ordinal-regression)),
- aantal voorspelling ([Poisson regressie](/azure/machine-learning/studio-module-reference/poisson-regression)),
- detectie van afwijkingen (één op basis van [analyse van de belangrijkste onderdelen](/azure/machine-learning/studio-module-reference/pca-based-anomaly-detection) en één op basis van [ondersteuning vector machines](/azure/machine-learning/studio-module-reference/one-class-support-vector-machine))
- Clustering ([K-means](/azure/machine-learning/studio-module-reference/k-means-clustering))

![PCA-gebaseerd anomaliedetectie](./media/algorithm-choice/image8.png)

***PCA-gebaseerd anomaliedetectie*** *-het overgrote deel van de gegevens in een stereotypical distributie valt; punten die aanzienlijk afwijken van dat distributiepunt verdacht zijn*

![Gegevensset met behulp van K-means gegroepeerd](./media/algorithm-choice/image9.png)

***Een gegevensset worden onderverdeeld in vijf clusters met behulp van K-means***

Er is ook een ensembles [one-v-all multiklassen classificatie](/azure/machine-learning/studio-module-reference/one-vs-all-multiclass), die de N-klasse regressieprobleem opgesplitst in N-1 twee klassen classificatie problemen. De nauwkeurigheid, trainingstijd en lineariteit eigenschappen worden bepaald door de twee klassen classificaties gebruikt.

![Twee klassen classificaties gecombineerd tot een classificatie van drie-klasse](./media/algorithm-choice/image10.png)

***Een combinatie van twee klassen classificaties combineren tot een classificatie van drie-klasse***

Azure Machine Learning Studio bevat ook toegang tot een krachtige machine learning-frameworks onder de titel van [Vowpal Wabbit](/azure/machine-learning/studio-module-reference/train-vowpal-wabbit-version-7-4-model).
VW defies categorisatie hier, omdat deze meer classificatie- en regressiemodellen problemen en zelfs van gedeeltelijk niet-gelabelde gegevens leren kunt. U kunt deze voor het gebruik van een van een aantal learning-algoritmen, verlies van functies en algoritmen voor de optimalisatie configureren. Het is ontworpen vanaf het begin van efficiënte, parallelle en zeer snel. Heel groot functiesets met veel blijkt moeite verwerkt.
Aan de slag en onder leiding van Microsoft Research eigen John Langford, is VW een formule één vermelding in een veld van het aandeel auto-algoritmen. Niet elk probleem VW past, maar als u het geval is, kan het zinvol uw tijdens het leerproces oplopen op de interface zijn. Het is ook beschikbaar als [zelfstandige open-source code](https://github.com/JohnLangford/vowpal_wabbit) in verschillende talen.

## <a name="next-steps"></a>Volgende stappen

* Zie voor het downloaden van een overzicht van de infographic eenvoudig te begrijpen van de basisprincipes van machine learning voor meer informatie over populaire algoritmen gebruikt om veelvoorkomende machine learning vragen te beantwoorden [Machine learning-basisbeginselen met algoritmevoorbeelden](basics-infographic-with-algorithm-examples.md).

* Zie voor een lijst op categorie van alle de machine learning-algoritmen in Machine Learning Studio, [Initialize Model](/azure/machine-learning/studio-module-reference/machine-learning-initialize-model) in de algoritme voor Machine Learning Studio en de Module Help.

* Zie voor een volledige alfabetische lijst van algoritmen en modules in Machine Learning Studio, [alfabetische lijst van Machine Learning Studio-modules](/azure/machine-learning/studio-module-reference/a-z-module-list) in de Module Help en algoritme voor Machine Learning Studio.
