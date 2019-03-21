---
title: 'Maken van aangepaste neurale netwerken met Net #'
titleSuffix: Azure Machine Learning Studio
description: 'Syntaxis van de handleiding voor de Net # neurale netwerken specificatietaal. Informatie over het maken van aangepaste neurale netwerk modellen in Azure Machine Learning Studio.'
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: reference
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/01/2018
ms.openlocfilehash: d667dadeb2e7c9d0005ab8d1a565017973038aaa
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57905151"
---
# <a name="guide-to-net-neural-network-specification-language-for-azure-machine-learning-studio"></a>Handleiding voor het Net # neurale-netwerkspecificatie voor Azure Machine Learning Studio

NET # is een door Microsoft ontwikkelde taal die wordt gebruikt om complexe neural network-architecturen zoals deep neural networks of convoluties van willekeurige dimensies te definiëren. U kunt complexe structuren gebruiken om te leren op gegevens, zoals afbeeldingen, video of audio verbeteren.

In deze context kunt u een specificatie Net #-architectuur:

+ Alle neutrale netwerkmodules in Microsoft Azure Machine Learning Studio: [Multiklassen Neural Network](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/multiclass-neural-network), [Two-Class Neural Network](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/two-class-neural-network), en [Neural Network regressie](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/neural-network-regression)
+ Neural network-functies in Microsoft ML Server: [NeuralNet](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/neuralnet) en [rxNeuralNet](https://docs.microsoft.com/machine-learning-server/r-reference/microsoftml/rxneuralnet)voor de taal R en [rx_neural_network](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/rx-neural-network) voor Python.


In dit artikel beschrijft de basisconcepten en de syntaxis die nodig zijn voor het ontwikkelen van een aangepaste neural network met Net #:

+ Neural network-vereisten en het definiëren van de primaire onderdelen
+ De syntaxis en trefwoorden van de Net #-specificatietaal
+ Voorbeelden van aangepaste neurale netwerken die zijn gemaakt met Net #



## <a name="neural-network-basics"></a>Neural network-basisbeginselen

De structuur van een neuraal netwerk bestaat uit knooppunten die zijn ingedeeld in lagen, en gewogen verbindingen (of randen) tussen de knooppunten. De verbindingen zijn gericht en elke verbinding heeft een bronknooppunt en een doelknooppunt.

Elke trainable laag (een verborgen of een uitvoer-laag) heeft een of meer **verbinding bundels**. Een bundel verbinding bestaat uit een bronlaag en een specificatie van de verbindingen van die bronlaag. Alle verbindingen in een bepaalde bundel delen bron- en lagen. In de Net #, wordt beschouwd als een bundel verbinding als behorend tot de doellaag van de bundel.

NET # biedt ondersteuning voor verschillende soorten verbinding bundels, die u kunnen aanpassen van de invoer van de manier waarop worden toegewezen aan verborgen lagen en toegewezen aan de uitvoer.

De standaard- of standard-bundel is een **volledige bundel**, in die op elk knooppunt in de bronlaag is verbonden met elk knooppunt in de doellaag.

Daarnaast ondersteunt Net # de volgende vier soorten geavanceerde verbinding bundels:

+ **Gefilterd bundels**. U kunt een predicaat definiëren met behulp van de locaties van het bronknooppunt laag en het doelknooppunt voor de laag. Knooppunten zijn verbonden wanneer het predikaat True is.

+ **Convolutional bundels**. U kunt kleine groepen van knooppunten definiëren in de bronlaag. Elk knooppunt in de doellaag is verbonden met één groep van knooppunten in de bronlaag.

+ **Groeperen bundels** en **antwoord normalisering bundels**. Dit zijn vergelijkbaar met convolutional bundels in dat de gebruiker gedefinieerd kleine groepen van knooppunten in de bronlaag. Het verschil is dat het gewicht van de randen in deze pakketten niet trainable zijn. In plaats daarvan wordt een vooraf gedefinieerde functie toegepast op de waarden van het knooppunt bron om te bepalen van de waarde van de doel-knooppunt.


## <a name="supported-customizations"></a>Ondersteunde aanpassingen

De architectuur van neural network-modellen die u in Azure Machine Learning Studio maakt kan worden uitgebreid met behulp van Net # zijn aangepast. U kunt:

+ Verborgen lagen maken en beheren van het aantal knooppunten in elke laag.
+ Opgeven hoe lagen moeten met elkaar worden verbonden.
+ Speciaal connectiviteit structuren, zoals convoluties en gewicht delen bundels definiëren.
+ Geef de activering van de verschillende functies.

Zie voor meer informatie over de syntaxis van de specificatie, [structuur specificatie](#Structure-specifications).

Zie voor meer voorbeelden van het definiëren van neurale netwerken voor sommige reguliere machine learning-taken uit Simplex () aan complexe [voorbeelden](#Examples-of-Net#-usage).

## <a name="general-requirements"></a>Algemene vereisten

+ Er zijn moet exact één uitvoer laag, ten minste één invoer-laag en nul of meer verborgen lagen.
+ Elke laag heeft een vast aantal knooppunten, conceptueel gezien gerangschikt in een rechthoekige matrix van willekeurige dimensies.
+ Invoer lagen hebben geen gekoppelde getrainde parameters en het punt waar gegevens van exemplaar van het netwerk krijgt vertegenwoordigen.
+ Trainable lagen (de lagen verborgen en uitvoer) hebt getraind parameters, ook wel het gewicht en vooroordelen gekoppeld.
+ De bron- als doelknooppunten moeten zich in afzonderlijke lagen.
+ Verbindingen moet acyclische; Er kan niet met andere woorden, een keten van toonaangevende terug naar het eerste bronknooppunt-verbindingen.
+ De uitvoer-laag kan niet een bronlaag van een bundel verbinding.

## <a name="structure-specifications"></a>Structuur specificaties

Een specificatie van de structuur neurale netwerk bestaat uit drie secties: de **declaratie van de constante**, wordt de **laag declaratie**, wordt de **verbinding declaratie**. Er is ook een optionele **delen declaratie** sectie. De secties kunnen in willekeurige volgorde worden opgegeven.

## <a name="constant-declaration"></a>Constante declaratie

Een constante verklaring is optioneel. Het biedt een manier voor het definiëren van waarden die elders in de netwerkdefinitie van neurale wordt gebruikt. De declaratie-instructie bestaat uit een id die wordt gevolgd door een gelijkteken en een waarde-expressie.

Bijvoorbeeld, de volgende instructie definieert een constante `x`:

`Const X = 28;`

Als u twee of meer constanten tegelijkertijd definieert, plaatst u de id-namen en waarden tussen accolades en gescheiden door puntkomma's. Bijvoorbeeld:

`Const { X = 28; Y = 4; }`

De rechterkant van de toewijzingsexpressie voor elke kan een geheel getal, een reëel getal, een Booleaanse waarde (waar of ONWAAR) of een rekenkundige expressie zijn. Bijvoorbeeld:

`Const { X = 17 * 2; Y = true; }`

## <a name="layer-declaration"></a>Laag-declaratie

De declaratie van de laag is vereist. Hiermee worden de grootte en de bron van de laag, met inbegrip van de verbinding bundels en kenmerken gedefinieerd. De declaratie-instructie wordt gestart met de naam van de laag (input, verborgen of uitvoer), gevolgd door de afmetingen van de laag (een tuple van positieve gehele getallen). Bijvoorbeeld:

```Net#
input Data auto;
hidden Hidden[5,20] from Data all;
output Result[2] from Hidden all;
```

+ Het product van de dimensies is het aantal knooppunten in de laag. In dit voorbeeld zijn er twee dimensies [5,20], wat betekent dat er 100 knooppunten zijn in de laag.
+ De lagen kunnen worden gedeclareerd in willekeurige volgorde, met één uitzondering: Als meer dan één invoer laag is gedefinieerd, moet de volgorde van de functies in de ingevoerde gegevens overeenkomen met de volgorde waarin ze zijn gedeclareerd.

Als u wilt opgeven dat het aantal knooppunten in een laag automatisch worden bepaald, gebruikt u de `auto` trefwoord. De `auto` sleutelwoord heeft verschillende effecten, afhankelijk van de laag:

+ In de declaratie van een invoer-laag is het aantal knooppunten in het aantal functies in de ingevoerde gegevens.
+ In een verklaring van de verborgen laag, is het aantal knooppunten het getal dat is opgegeven door de waarde van parameter voor **aantal verborgen knooppunten**.
+ In een laag uitvoerdeclaratie is het aantal knooppunten 2 voor classificatie van twee klassen, 1 voor regressie en gelijk is aan het aantal uitvoerknooppunten voor multiklassen classificatie.

De volgende netwerkdefinitie kan bijvoorbeeld de grootte van alle lagen worden automatisch bepaald:

```Net#
input Data auto;
hidden Hidden auto from Data all;
output Result auto from Hidden all;
```

Een verklaring van de laag voor een trainable laag (de uitvoer of verborgen lagen) (optioneel) de uitvoer-functie kan bevatten (ook wel een functie voor activering genoemd), die standaard **sigmoid** voor classificatie-modellen en  **lineaire** voor regressiemodellen. Zelfs als u de standaardwaarde gebruikt, kunt u de functie activeren kunt expliciet aangeven desgewenst voor de duidelijkheid.

De volgende uitvoer-functies worden ondersteund:

+ sigmoid
+ Lineair
+ softmax
+ rlinear
+ square
+ WORTEL
+ srlinear
+ ABS
+ tanh
+ brlinear

De volgende declaratie gebruikt bijvoorbeeld de **softmax** functie:

`output Result [100] softmax from Hidden all;`

## <a name="connection-declaration"></a>De declaratie van de verbinding

Onmiddellijk na het definiëren van de trainable laag, moet u de verbindingen tussen de lagen die u hebt gedefinieerd declareren. De declaratie van de bundel verbinding begint met het sleutelwoord `from`, gevolgd door de naam van de bronlaag van de bundel en het type van de bundel verbinding te maken.

Op dit moment worden verbinding bundels vijf typen ondersteund:

+ **Volledige** bundels, aangegeven door het sleutelwoord `all`
+ **Gefilterd** bundels, aangegeven door het sleutelwoord `where`, gevolgd door een predicaatexpressie
+ **Convolutional** bundels, aangegeven door het sleutelwoord `convolve`, gevolgd door de kenmerken convolutiefilter
+ **Groeperen** bundels, aangegeven door de trefwoorden **max pool** of **betekenen van toepassingen**
+ **Antwoord normalisering** bundels, aangegeven door het sleutelwoord **antwoord norm**

## <a name="full-bundles"></a>Volledige bundels

Een volledige verbinding bundel bevat een verbinding van elk knooppunt in de bronlaag aan elk knooppunt in de doellaag. Dit is het verbindingstype van de standaard-netwerk.

## <a name="filtered-bundles"></a>Gefilterde bundels

De specificatie van een gefilterde verbinding bundel bevat een zeer predicaat, syntactisch, uitgedrukt als een C# lambda-expressie. Het volgende voorbeeld definieert twee gefilterde pakketten:

```Net#
input Pixels [10, 20];
hidden ByRow[10, 12] from Pixels where (s,d) => s[0] == d[0];
hidden ByCol[5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;
```

+ In het predikaat voor `ByRow`, `s` is een parameter voor een index in de matrix rechthoekige knooppunten van de invoer-laag, `Pixels`, en `d` is een parameter voor een index in de matrix van knooppunten in de verborgen laag `ByRow`. Het type van beide `s` en `d` is een tuple met gehele getallen van de lengte van de twee. Conceptueel gezien `s` bereiken via alle paren van gehele getallen met `0 <= s[0] < 10` en `0 <= s[1] < 20`, en `d` bereiken via alle paren van gehele getallen, met `0 <= d[0] < 10` en `0 <= d[1] < 12`.

+ Aan de rechterkant van de predicaatexpressie is er een voorwaarde. In dit voorbeeld, voor elke waarde van `s` en `d` zodanig dat de voorwaarde waar is, er een rand van het bronknooppunt laag naar het doelknooppunt voor de laag is. Dus dit filterexpressie geeft aan dat de bundel een verbinding van het knooppunt dat is gedefinieerd omvat door `s` naar het knooppunt dat is gedefinieerd door `d` in alle gevallen waarin s [0] gelijk aan d [0 is].

U kunt eventueel een set waarden voor een gefilterde bundel opgeven. De waarde voor de **gewichten** kenmerk moet een tuple van zwevende waarden met een lengte die overeenkomt met het aantal verbindingen die zijn gedefinieerd door de bundel. Standaard worden gewichten willekeurig gegenereerd.

Gewichtswaarden zijn gegroepeerd op de index van de doel-knooppunt. Dat wil zeggen, als het eerste bestemmingsknooppunt is verbonden met de bronknooppunten K, de eerste `K` elementen van de **gewichten** tuple zijn het gewicht voor het eerste doelknooppunt in volgorde van de gegevensbron-index. Hetzelfde geldt voor de resterende knooppunten van de bestemming.

Het is mogelijk om op te geven van het gewicht rechtstreeks als constante waarden. Bijvoorbeeld, als u het gewicht eerder hebt geleerd, kunt u ze als constanten met behulp van deze syntaxis:

`const Weights_1 = [0.0188045055, 0.130500451, ...]`

## <a name="convolutional-bundles"></a>Convolutional bundels

Wanneer de trainingsgegevens een homogene structuur heeft, worden vaak convolutional verbindingen gebruikt voor meer informatie over geavanceerde functies van de gegevens. Gegevens bijvoorbeeld: in afbeelding, audio of video, ruimtelijke of tijdelijke dimensionaliteit kunnen zijn redelijk uniform.

Convolutional bundels maken gebruik van rechthoekige **kernels** die via de dimensies worden geschoven. In wezen elke kernel definieert een reeks gewichten toegepast in de lokale groepen, aangeduid als **kerneltoepassingen**. Elke toepassing kernel komt overeen met een knooppunt in de bronlaag, die is aangeduid als de **centraal knooppunt**. Het gewicht van een kernel worden door veel verbindingen gedeeld. In een convolutional bundel elke kernel rechthoekig is en alle kerneltoepassingen even groot zijn.

Convolutional bundels ondersteunen de volgende kenmerken:

**InputShape** definieert de dimensionaliteit van de bronlaag voor het doel van deze convolutional bundel. De waarde moet een tuple van positieve gehele getallen zijn. Het product van de gehele getallen moet gelijk zijn aan het aantal knooppunten in de bronlaag, maar anders hoeft niet overeenkomt met de dimensionaliteit gedeclareerd voor de bronlaag. De lengte van deze tuple wordt de **specifikaci** waarde voor de convolutional bundel. Specifikaci verwijst doorgaans naar het aantal argumenten of operanden met een functie kan duren.

Gebruik de kenmerken definiëren van de shape en locaties van de kernels **KernelShape**, **Stride**, **opvulling**, **LowerPad**, en  **UpperPad**:

+ **KernelShape**: (vereist) definieert de dimensionaliteit van elke kernel voor de convolutional bundel. De waarde moet een tuple van positieve gehele getallen met een lengte die gelijk is aan de specifikaci van de bundel. Elk onderdeel van deze tuple is mag niet langer zijn dan het overeenkomstige onderdeel van **InputShape**.

+ **Stride**: (optioneel) definieert de sliding stap grootte van de convolutiefilter (één stapgrootte voor elke dimensie), die de afstand tussen de centrale knooppunten. De waarde moet een tuple van positieve gehele getallen met een lengte die de specifikaci van de bundel is. Elk onderdeel van deze tuple is mag niet langer zijn dan het overeenkomstige onderdeel van **KernelShape**. De standaardwaarde is een tuple met alle onderdelen die gelijk is aan één.

+ **Delen van**: (optioneel) definieert het gewicht voor elke dimensie van de convolutiefilter delen. De waarde kan zijn één Booleaanse waarde of een tuple van Booleaanse waarden met een lengte die de specifikaci van de bundel is. Één Booleaanse waarde is uitgebreid om te worden van een tuple van de juiste lengte met alle onderdelen die gelijk is aan de opgegeven waarde. De standaardwaarde is een tuple die uit alle waar waarden bestaat.

+ **MapCount**: (optioneel) definieert het nummer van de functie voor de convolutional bundel wordt toegewezen. De waarde kan niet een afzonderlijke positieve gehele getallen of een tuple van positieve gehele getallen met een lengte die de specifikaci van de bundel. Een enkele gehele waarde is uitgebreid om te worden van een tuple van de juiste lengte met de eerste onderdelen die gelijk is aan de opgegeven waarde en de resterende onderdelen gelijk is aan een. De standaardwaarde is een. Het totale aantal toewijzingen van de functie is het product van de onderdelen van de tuple. Het kiezen van deze totale aantal voor de onderdelen bepaalt hoe de waarden van de kaart functie worden gegroepeerd in de doel-knooppunten.

+ **Gewicht**: (optioneel) het eerste gewicht voor de bundel definieert. De waarde moet een tuple van zwevende waarden met een lengte die is het aantal keren dat kernels het nummer van het gewicht per kernel, zoals verderop in dit artikel worden gedefinieerd. De standaardwaarden gebruiken worden willekeurig gegenereerd.

Er zijn twee sets van eigenschappen die regelen opvulling, de eigenschappen worden sluiten elkaar wederzijds uit:

+ **Opvulling**: (optioneel) bepaalt of de invoer moet worden aangevuld met behulp van een **opvulling standaardschema**. De waarde kan één Booleaanse waarde, of een tuple van Booleaanse waarden met een lengte die de specifikaci van de bundel kan zijn.

    Één Booleaanse waarde is uitgebreid om te worden van een tuple van de juiste lengte met alle onderdelen die gelijk is aan de opgegeven waarde.

    Als de waarde voor een dimensie waar is, worden de bron logisch opgevuld in die dimensie met de waarde nul cellen ter ondersteuning van aanvullende kerneltoepassingen, zodat de centrale knooppunten van de eerste en laatste kernels in die dimensie, de eerste en laatste knooppunten in die dimensie in de bronlaag. Dus, het aantal "dummy" knooppunten in elke dimensie wordt automatisch bepaald, precies past `(InputShape[d] - 1) / Stride[d] + 1` kernels in de bronlaag gevuld.

    Als de waarde voor een dimensie ONWAAR is, worden de kernels gedefinieerd, zodat het aantal knooppunten aan beide zijden die worden weggelaten hetzelfde (maximaal een verschil van 1 wordt). De standaardwaarde van dit kenmerk is een tuple met alle onderdelen die gelijk is aan de waarde False.

+ **UpperPad** en **LowerPad**: (optioneel) Geef meer controle over de hoeveelheid opvulling te gebruiken. **Belangrijk:** Deze kenmerken kunnen worden gedefinieerd als en alleen als de **opvulling** eigenschap hierboven is ***niet*** gedefinieerd. De waarden moet tuples integer-waarde met een lengte die de specifikaci van de bundel. Wanneer deze kenmerken zijn opgegeven, wordt "dummy" knooppunten worden toegevoegd aan het einde van het onderste en bovenste van elke dimensie van de invoer-laag. Het aantal knooppunten die zijn toegevoegd aan het einde van het onderste en bovenste in elke dimensie wordt bepaald door **LowerPad**[i] en **UpperPad**[i] respectievelijk.

    Om ervoor te zorgen dat de kernels alleen naar 'echte' knooppunten en niet naar "dummy" knooppunten overeenkomen, moeten de volgende voorwaarden worden voldaan:
  - Elk onderdeel van **LowerPad** moet strikt minder dan `KernelShape[d]/2`.
  - Elk onderdeel van **UpperPad** mag niet langer zijn dan `KernelShape[d]/2`.
  - De standaardwaarde van deze kenmerken is een tuple met alle onderdelen die gelijk zijn aan 0.

    De instelling **opvulling** = true kunt zoveel opvulling als nodig is voor het behouden van de 'center' van de kernel in de 'realtime' invoer. Hiermee wordt een en ander voor het berekenen van de uitvoergrootte van de gewijzigd. Over het algemeen de uitvoergrootte *D* wordt berekend als `D = (I - K) / S + 1`, waarbij `I` is de invoergrootte `K` is de grootte kernel `S` is de stride en `/` delen door geheel getal (afronden op nul is ). Als u UpperPad = [1, 1], de grootte van de invoer `I` is in feite 29, en dus `D = (29 - 5) / 2 + 1 = 13`. Echter, wanneer **opvulling** = true, in feite `I` opgehaald tegenaan omhoog door `K - 1`; daarom `D = ((28 + 4) - 5) / 2 + 1 = 27 / 2 + 1 = 13 + 1 = 14`. Door op te geven waarden voor **UpperPad** en **LowerPad** krijgt u veel meer controle over de opvulling aan dan als u stelt **opvulling** = true.

Zie voor meer informatie over convolutional netwerken en hun toepassingen, de volgende artikelen:

+ [http://deeplearning.net/tutorial/lenet.html](http://deeplearning.net/tutorial/lenet.html)
+ [https://research.microsoft.com/pubs/68920/icdar03.pdf](https://research.microsoft.com/pubs/68920/icdar03.pdf)

## <a name="pooling-bundles"></a>Bundels groeperen

Een **groeperen bundel** geometrie die vergelijkbaar is met convolutional connectiviteit, toepassing, maar hierbij vooraf gedefinieerde functies aan de waarden van de bron voor het afleiden van de waarde van de doel-knooppunt. Groepsgewijze verbindingen bundels hebben daarom geen trainable status (gewicht of vooroordelen). Groepsgewijze verbindingen bundels ondersteuning voor alle convolutional kenmerken, behalve **delen**, **MapCount**, en **gewichten**.

Normaal gesproken overlappen de kernels samengevat per aangrenzende groepsgewijze verbindingen eenheden niet. Als Stride [d] gelijk aan KernelShape [d] in elke dimensie is, is de laag die is verkregen de traditionele lokale groepsgewijze verbindingen laag, die meestal in convolutional neural networks gebruikt wordt. Elk bestemmingsknooppunt berekent het maximum of het gemiddelde van de activiteiten van de kernel in de bronlaag.

Het volgende voorbeeld wordt een groepering bundel:

```Net#
hidden P1 [5, 12, 12]
  from C1 max pool {
  InputShape  = [ 5, 24, 24];
   KernelShape = [ 1,  2,  2];
   Stride      = [ 1,  2,  2];
  }
```

+ De specifikaci van de bundel is 3: dat wil zeggen, de lengte van de tuples `InputShape`, `KernelShape`, en `Stride`.
+ Het aantal knooppunten in de bronlaag is `5 * 24 * 24 = 2880`.
+ Dit is een traditionele lokale groepsgewijze verbindingen layer omdat **KernelShape** en **Stride** gelijk zijn.
+ Het aantal knooppunten in de doellaag is `5 * 12 * 12 = 1440`.

Zie de volgende artikelen voor meer informatie over groepsgewijze verbindingen lagen:

+ [https://www.cs.toronto.edu/~hinton/absps/imagenet.pdf](https://www.cs.toronto.edu/~hinton/absps/imagenet.pdf) (Sectie 3.4)
+ [https://cs.nyu.edu/~koray/publis/lecun-iscas-10.pdf](https://cs.nyu.edu/~koray/publis/lecun-iscas-10.pdf)
+ [https://cs.nyu.edu/~koray/publis/jarrett-iccv-09.pdf](https://cs.nyu.edu/~koray/publis/jarrett-iccv-09.pdf)

## <a name="response-normalization-bundles"></a>Antwoord normalisering bundels

**Antwoord normalisering** is een lokale normalisering schema die is geïntroduceerd door Geoffrey Hinton, et al., in het document [ImageNet classificatie met Deep Convolutional Neural Networks](https://www.cs.toronto.edu/~hinton/absps/imagenet.pdf).

Antwoord normalisering wordt gebruikt om u te helpen generalisatie in neurale netwerken. Wanneer één neuron wordt uitgevoerd op het Activeringsniveau van een zeer hoge, wordt in een lokale antwoord normalisering laag het Activeringsniveau van de omringende neurons onderdrukt. Dit wordt gedaan met behulp van de drie parameters (`α`, `β`, en `k`) en een convolutional structuur (of de groep vorm). Elke neuron in de doellaag **y** komt overeen met een neuron **x** in de bronlaag. Het Activeringsniveau van **y** wordt bepaald door de volgende formule, waarbij `f` is het Activeringsniveau van een neuron en `Nx` is de kernel (of de set met de neurons in de groep van **x**), zoals gedefinieerd door de volgende convolutional structuur:

![formule voor convolutional structuur](./media/azure-ml-netsharp-reference-guide/formula_large.png)

Antwoord normalisering bundels ondersteuning voor alle convolutional kenmerken, behalve **delen**, **MapCount**, en **gewichten**.

+ Als de kernel neurons in de dezelfde map als bevat ***x***, het normalisatie-schema wordt aangeduid als **hetzelfde normalisering toewijzen**. Voor het definiëren van dezelfde kaart normalisering, de eerste coördinaat in **InputShape** moet de waarde 1 hebben.

+ Als de kernel neurons in dezelfde ruimtelijke positie als bevat ***x***, maar de neurons zijn in andere toewijzingen, het schema normalisering heet **over meerdere toewijzingen normalisering**. Dit type antwoord normalisering implementeert een vorm van laterale maar geïnspireerd op het type gevonden in de echte neurons, het maken van de concurrentie voor grote activering niveaus onder neuron uitvoer berekend op de andere kaarten. Om te definiëren via maps normalisering, moet de eerste coördinaat een geheel getal groter dan één en niet langer zijn dan het aantal kaarten, en de rest van de coördinaten moet de waarde 1 hebben.

Omdat een vooraf gedefinieerde functie antwoord normalisering bundels op waarden van de bron-knooppunt om te bepalen van de waarde van de doel-knooppunt toepassen, hebben ze geen trainable status (gewicht of vooroordelen).

> [!NOTE]
> De knooppunten in de doellaag komen overeen met neurons die de centrale knooppunten van de kernels. Bijvoorbeeld, als `KernelShape[d]` is oneven, `KernelShape[d]/2` komt overeen met het centrale kernel-knooppunt. Als `KernelShape[d]` even getal is, is de centrale knooppunt is `KernelShape[d]/2 - 1`. Dus als `Padding[d]` is ONWAAR, de eerste en de laatste `KernelShape[d]/2` knooppunten geen bijbehorende knooppunten in de doellaag. Om te voorkomen dat deze situatie, definiëren **opvulling** als [true, true,..., true].

Naast de vier kenmerken die eerder zijn beschreven, ondersteuning antwoord normalisering bundels ook voor de volgende kenmerken:

+ **Alpha**: (vereist) Hiermee wordt een drijvende-kommawaarde die overeenkomt met `α` in de vorige formule.
+ **Bèta**: (vereist) Hiermee wordt een drijvende-kommawaarde die overeenkomt met `β` in de vorige formule.
+ **Offset**: (optioneel) Hiermee wordt een drijvende-kommawaarde die overeenkomt met `k` in de vorige formule. Wordt standaard op 1.

Het volgende voorbeeld wordt een antwoord normalisering bundel met behulp van deze kenmerken gedefinieerd:

```Net#
hidden RN1 [5, 10, 10]
from P1 response norm {
  InputShape  = [ 5, 12, 12];
  KernelShape = [ 1,  3,  3];
  Alpha = 0.001;
  Beta = 0.75;
  }
```

+ De bronlaag omvat vijf maps, elk met aof-dimensie van 12 x 12, tellen in 1440 knooppunten.
+ De waarde van **KernelShape** geeft aan dat dit een dezelfde normalisering kaartLaag, waarbij de groep een rechthoek 3 x 3 is.
+ De standaardwaarde van **opvulling** is ingesteld op False, waardoor de doellaag slechts 10 knooppunten is in elke dimensie. Toevoegen als u wilt opnemen een knooppunt in de doellaag die overeenkomt met elk knooppunt in de bronlaag, opvulling = [true, true, true]; en wijzig de grootte van RN1 in [5, 12, 12].

## <a name="share-declaration"></a>Verklaring delen

NET # ondersteunt (optioneel) meerdere pakketten met gedeelde gewicht definiëren. Het gewicht van elke twee pakketten kunnen worden gedeeld als hun structuren hetzelfde zijn. De volgende syntaxis definieert bundels met gedeelde gewicht:

```Net#
share-declaration:
  share    {    layer-list    }
  share    {    bundle-list    }
  share    {    bias-list    }

  layer-list:
    layer-name    ,    layer-name
    layer-list    ,    layer-name

  bundle-list:
    bundle-spec    ,    bundle-spec
    bundle-list    ,    bundle-spec

  bundle-spec:
    layer-name    =>     layer-name

  bias-list:
    bias-spec    ,    bias-spec
    bias-list    ,    bias-spec

  bias-spec:
    1    =>    layer-name

  layer-name:
    identifier
```

De volgende share-declaratie benoemt bijvoorbeeld de laagnamen van de die aangeeft dat de gewichten en vooroordelen mag worden gedeeld:

```Net#
Const {
  InputSize = 37;
  HiddenSize = 50;
  }
input {
  Data1 [InputSize];
  Data2 [InputSize];
  }
hidden {
  H1 [HiddenSize] from Data1 all;
  H2 [HiddenSize] from Data2 all;
  }
output Result [2] {
  from H1 all;
  from H2 all;
  }
share { H1, H2 } // share both weights and biases
```

+ De invoerfuncties zijn in twee gelijke grootte invoer lagen gepartitioneerd.
+ Hogere niveau functies op de twee lagen van de invoer moet u de verborgen lagen berekenen.
+ De share-declaratie geeft aan dat *H1* en *H2* op dezelfde manier van hun respectieve invoer moet worden berekend.

U kunt ook kan dit worden opgegeven met twee afzonderlijke share-declaraties als volgt:

```Net#
share { Data1 => H1, Data2 => H2 } // share weights
<!-- -->
    share { 1 => H1, 1 => H2 } // share biases
```

U kunt de verkorte vorm alleen als de lagen één bundel bevatten. In het algemeen is delen mogelijk alleen wanneer de structuur van de relevante identiek zijn is, wat betekent dat ze dezelfde grootte, dezelfde convolutional geometrie, enzovoort hebben.

## <a name="examples-of-net-usage"></a>Voorbeelden van de Net # gebruik

Deze sectie bevat enkele voorbeelden van hoe u Net # kunt verborgen lagen toevoegen, het definiëren van de manier waarop verborgen lagen met andere lagen communiceren en convolutional netwerken bouwen.

### <a name="define-a-simple-custom-neural-network-hello-world-example"></a>Definieer een eenvoudig aangepaste neurale netwerk: 'Hallo wereld'-voorbeeld

Dit eenvoudige voorbeeld ziet u hoe u een neuraal netwerk-model met één verborgen laag maken.

```Net#
input Data auto;
hidden H [200] from Data all;
output Out [10] sigmoid from H all;
```

Het voorbeeld ziet u enkele eenvoudige opdrachten als volgt:

+ De eerste regel wordt gedefinieerd voor de invoer-laag (met de naam `Data`). Wanneer u gebruikt de `auto` trefwoord, het neurale netwerk omvat alle kolommen van de functie automatisch in de invoer voorbeelden.
+ De tweede regel maakt de verborgen laag. De naam van de `H` is toegewezen aan de verborgen laag met 200 knooppunten. Deze laag is volledig wordt verbonden met de invoer-laag.
+ De derde regel definieert de uitvoer-laag (met de naam `Out`), die 10 uitvoerknooppunten bevat. Als het neurale netwerk wordt gebruikt voor classificatie, is er een uitvoer van de knooppunten per klasse. Het sleutelwoord **sigmoid** geeft aan dat de uitvoer-functie wordt toegepast op de uitvoer-laag.

### <a name="define-multiple-hidden-layers-computer-vision-example"></a>Meerdere verborgen lagen definiëren: computer vision-voorbeeld

Het volgende voorbeeld ziet u hoe u een complexere neural network, met meerdere lagen van aangepaste verborgen definiëren.

```Net#
// Define the input layers
input Pixels [10, 20];
input MetaData [7];

// Define the first two hidden layers, using data only from the Pixels input
hidden ByRow [10, 12] from Pixels where (s,d) => s[0] == d[0];
hidden ByCol [5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;

// Define the third hidden layer, which uses as source the hidden layers ByRow and ByCol
hidden Gather [100]
{
from ByRow all;
from ByCol all;
}

// Define the output layer and its sources
output Result [10]
{
from Gather all;
from MetaData all;
}
```

In dit voorbeeld ziet u verschillende functies van de specificatietaal van neurale netwerken:

+ De structuur heeft twee invoer lagen, `Pixels` en `MetaData`.
+ De `Pixels` laag is een bronlaag voor twee verbinding-bundels met doel-lagen, `ByRow` en `ByCol`.
+ De lagen `Gather` en `Result` bestemming lagen in meerdere pakketten van de verbinding.
+ De uitvoer-laag, `Result`, is een doellaag in twee verbinding bundels, één met de tweede niveau verborgen laag `Gather` als een doellaag, en de andere met de invoer laag `MetaData` als een doellaag.
+ De verborgen lagen, `ByRow` en `ByCol`, gefilterde verbinding opgeven met behulp van predikaat expressies. Preciezer nog, het knooppunt in `ByRow` op [x, y] is verbonden met de knooppunten in `Pixels` dat zijn de eerste index coördineren gelijk zijn aan het knooppunt van de eerste coördineren, x. Op dezelfde manier het knooppunt in `ByCol` op [x, y] is verbonden met de knooppunten in `Pixels` dat zijn de tweede index coördineren binnen een van het knooppunt van de tweede coördinaat, y.

### <a name="define-a-convolutional-network-for-multiclass-classification-digit-recognition-example"></a>Definieer een convolutional network voor multiklassen classificatie: cijfer erkenning voorbeeld

De definitie van de volgende network is ontworpen voor het herkennen van getallen en ziet u enkele geavanceerde technieken voor het aanpassen van een neural network.

```Net#
input Image [29, 29];
hidden Conv1 [5, 13, 13] from Image convolve
  {
  InputShape  = [29, 29];
  KernelShape = [ 5,  5];
  Stride      = [ 2,  2];
  MapCount    = 5;
  }
hidden Conv2 [50, 5, 5]
from Conv1 convolve
  {
  InputShape  = [ 5, 13, 13];
  KernelShape = [ 1,  5,  5];
  Stride      = [ 1,  2,  2];
  Sharing     = [false, true, true];
  MapCount    = 10;
  }
hidden Hid3 [100] from Conv2 all;
output Digit [10] from Hid3 all;
```

+ De structuur heeft één invoer-laag, `Image`.
+ Het sleutelwoord `convolve` geeft aan dat de naam van de lagen `Conv1` en `Conv2` convolutional lagen. Elk van deze laag declaraties wordt gevolgd door een lijst van de kenmerken convolutiefilter.
+ De net is een derde verborgen laag `Hid3`, die volledig is verbonden met de tweede verborgen laag `Conv2`.
+ De uitvoer-laag, `Digit`, is alleen verbonden met de derde verborgen laag `Hid3`. Het sleutelwoord `all` geeft aan dat de uitvoer-laag volledig is verbonden met `Hid3`.
+ De specifikaci van de convolutiefilter is drie: de lengte van de tuples `InputShape`, `KernelShape`, `Stride, and `delen '.
+ Het aantal gewicht per kernel is `1 + KernelShape\[0] * KernelShape\[1] * KernelShape\[2] = 1 + 1 * 5 * 5 = 26`. Of `26 * 50 = 1300`.
+ U kunt de knooppunten in de verborgen laag als volgt berekend:

    `NodeCount\[0] = (5 - 1) / 1 + 1 = 5` `NodeCount\[1] = (13 - 5) / 2 + 1 = 5`
    `NodeCount\[2] = (13 - 5) / 2 + 1 = 5`

+ Het totale aantal knooppunten kan worden berekend met behulp van de gedeclareerde dimensionaliteit van de laag [50, 5, 5], als volgt: `MapCount * NodeCount\[0] * NodeCount\[1] * NodeCount\[2] = 10 * 5 * 5 * 5`
+ Omdat `Sharing[d]` False is alleen voor `d == 0`, is het aantal kernels `MapCount * NodeCount\[0] = 10 * 5 = 50`.

## <a name="acknowledgements"></a>Bevestigingen

De Net #-taal voor het aanpassen van de architectuur van neurale netwerken is bij Microsoft ontwikkeld door Shon Katzenberger (Architect, Machine Learning) en Alexey Kamenev (Software Engineer, Microsoft Research). Het wordt intern gebruikt voor machine learning-projecten en toepassingen, variërend van detectie van de installatiekopie met text analytics. Zie voor meer informatie, [Neurale netwerken in Azure Machine Learning studio - Inleiding tot Net #](https://blogs.technet.com/b/machinelearning/archive/2015/02/16/neural-nets-in-azure-ml-introduction-to-net.aspx)
