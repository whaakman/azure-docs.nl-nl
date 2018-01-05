---
title: 'Handleiding voor de per saldo # Neural Networks specificatietaal | Microsoft Docs'
description: 'De syntaxis voor de per saldo # neural networks specificatietaal, samen met enkele voorbeelden van een aangepaste neurale netwerk wordt getraind model maken in Microsoft Azure ML met Net #'
services: machine-learning
documentationcenter: 
author: jeannt
manager: jhubbard
editor: cgronlun
ms.assetid: cfd1454b-47df-4745-b064-ce5f9b3be303
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 12/18/2017
ms.author: jeannt
ms.openlocfilehash: 15e68e9b4e6432d14c403e3532b934bfad58b35b
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2017
---
# <a name="guide-to-net-neural-network-specification-language-for-azure-machine-learning"></a>Handleiding voor de taal Net #-neural network-specificatie voor Azure Machine Learning
## <a name="overview"></a>Overzicht
NET # is een taal die is ontwikkeld door Microsoft die wordt gebruikt voor het definiëren van neural network-architecturen. U kunt Net # in neural network-modules in Microsoft Azure Machine Learning.

<!-- This function doesn't currentlyappear in the MicrosoftML documentation. If it is added in a future update, we can uncomment this text.

, or in the `rxNeuralNetwork()` function in [MicrosoftML](https://msdn.microsoft.com/microsoft-r/microsoftml/microsoftml). 

-->

In dit artikel leert u basisconcepten die nodig zijn voor het ontwikkelen van een aangepaste neurale netwerk: 

* Neural netwerkvereisten en het definiëren van de primaire onderdelen
* De syntaxis en trefwoorden van de taal Net #-specificatie
* Voorbeelden van aangepaste neural netwerken die zijn gemaakt met Net # 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="neural-network-basics"></a>Basisprincipes van neurale netwerk
Een structuur neurale netwerk bestaat uit ***knooppunten*** die zijn ingedeeld in ***lagen***, en de gewogen ***verbindingen*** (of ***randen***) tussen de knooppunten. De verbindingen zijn gericht en elke verbinding heeft een ***bron*** knooppunt en een ***bestemming*** knooppunt.  

Elke ***trainable laag*** (een verborgen of een laag uitvoer) heeft een of meer ***verbinding bundels***. Een bundel verbinding bestaat uit een bronlaag en een specificatie voor de verbindingen van die bronlaag. De verbindingen in een bepaalde bundel delen dezelfde ***bronlaag*** en dezelfde ***bestemming laag***. In Net #, wordt beschouwd als een bundel verbinding als onderdeel van de bundel bestemming laag.  

NET # ondersteunt verschillende soorten verbinding pakketten, kunt u de invoer manier aanpassen zijn toegewezen aan verborgen lagen en toegewezen aan de uitvoer.   

De standaard- of standard-bundel is een **volledige bundel**, in die op elk knooppunt in de bronlaag is verbonden met elk knooppunt in de doellaag.  

Daarnaast ondersteunt Net # de volgende vier typen bundels geavanceerde verbinding:  

* **Gefilterd bundels**. De gebruiker kan een predikaat definiëren met behulp van de locaties van het bronknooppunt laag en het doelknooppunt voor de laag. Knooppunten zijn verbonden wanneer het predikaat waar is.
* **Convolutional bundels**. De gebruiker kunt kleine groepen van knooppunten definiëren in de bronlaag. Elk knooppunt in de doellaag is verbonden met één groep van knooppunten in de bronlaag.
* **Groeperen van bundels** en **antwoord normalisatie bundels**. Dit zijn vergelijkbaar met convolutional bundels in dat de gebruiker gedefinieerd kleine groepen van knooppunten in de bronlaag. Het verschil is dat het gewicht van de randen in deze pakketten niet trainable zijn. In plaats daarvan wordt een vooraf gedefinieerde functie toegepast op de waarden van het knooppunt bron om te bepalen van de waarde van de doel-knooppunt.  

Net # definiëren met de structuur van een neural netwerk, maakt het mogelijk voor het definiëren van complexe structuren zoals diep neural networks of convoluties van willekeurige dimensies, waarvan bekend is dat het leren op gegevens, zoals afbeelding, audio of video verbeteren.  

## <a name="supported-customizations"></a>Ondersteunde aanpassingen
De architectuur van neural network-modellen die u in Azure Machine Learning maakt kan grote schaal worden aangepast via Net #. U kunt:  

* Verborgen lagen maken en beheren van het aantal knooppunten in elke laag.
* Opgeven hoe lagen moeten met elkaar worden verbonden.
* Speciale connectiviteit structuren, zoals convoluties en gewicht delen bundels definiëren.
* Geef de activering van andere functies.  

Zie voor meer informatie van de syntaxis van de specificatie [structuur specificatie](#Structure-specifications).  

Zie voor voorbeelden van het definiëren van neural networks voor een aantal veelvoorkomende machine learning-taken uit Simplex () naar complexe [voorbeelden](#Examples-of-Net#-usage).  

## <a name="general-requirements"></a>Algemene vereisten
* Moet er precies één uitvoer laag, ten minste één invoer-laag en nul of meer verborgen lagen. 
* Elke laag heeft een vast aantal knooppunten, conceptueel gerangschikt in een rechthoekige matrix met willekeurige dimensies. 
* Invoer lagen geen bijbehorende getraind parameters hebben en het punt waar de gegevens van het netwerk krijgt vertegenwoordigen. 
* Trainable lagen (de lagen verborgen en uitvoer) gekoppelde hebben getraind parameters, gewicht en vooroordelen genoemd. 
* De bron- en doelserver knooppunten moeten zich in verschillende lagen. 
* Verbindingen moet acyclische; Er kan niet met andere woorden, een keten van toonaangevende terug naar het eerste bronknooppunt verbindingen.
* De uitvoer-laag kan niet een bronlaag van een bundel verbinding.  

## <a name="structure-specifications"></a>Structuur specificaties
Een structure-specificatie van het neurale netwerk bestaat uit drie delen: de **constantendeclaratie**, wordt de **laag-declaratie**, wordt de **verbinding declaratie**. Er is ook een optionele **delen declaratie** sectie. De secties kunnen in elke volgorde worden opgegeven.  

## <a name="constant-declaration"></a>Constantendeclaratie
Een constantendeclaratie is optioneel. Het biedt een manier voor het definiëren van waarden elders in de definitie van het neurale netwerk wordt gebruikt. De declaratie-instructie bestaat uit een id die wordt gevolgd door een gelijkteken en een waardenexpressie.   

De volgende instructie definieert u bijvoorbeeld een constante **x**:  

    Const X = 28;  

Als u twee of meer constanten tegelijkertijd definieert, moet u de id-namen en waarden tussen vierkante haakjes en gescheiden door puntkomma's. Bijvoorbeeld:  

    Const { X = 28; Y = 4; }  

De rechterkant van de toewijzingsexpressie voor elke kan een geheel getal, een reëel getal, een Booleaanse waarde (True of False) of een rekenkundige expressie zijn. Bijvoorbeeld:  

    Const { X = 17 * 2; Y = true; }  

## <a name="layer-declaration"></a>Laag-declaratie
De declaratie laag is vereist. Het definieert de grootte en de bron van de laag, met inbegrip van de verbinding bundels en kenmerken. De declaratie-instructie begint met de naam van de laag (invoer, verborgen of uitvoer), gevolgd door de afmetingen van de laag (een tuple van positieve gehele getallen). Bijvoorbeeld:  

    input Data auto;
    hidden Hidden[5,20] from Data all;
    output Result[2] from Hidden all;  

* Het product van de dimensies is het aantal knooppunten in de laag. In dit voorbeeld zijn er twee dimensies [5,20], wat betekent dat er 100 knooppunten zijn in de laag.
* De lagen kunnen worden gedeclareerd in willekeurige volgorde, met één uitzondering: de volgorde waarin ze zijn gedeclareerd als meer dan één invoer laag is gedefinieerd, moet overeenkomen met de volgorde van de functies in de invoergegevens.  

Als u wilt opgeven dat het aantal knooppunten in een laag automatisch worden vastgesteld, gebruikt u de **automatisch** sleutelwoord. De **automatisch** sleutelwoord heeft verschillende effecten, afhankelijk van de laag:  

* In een declaratie invoer laag is het aantal knooppunten in het aantal functies in de invoergegevens.
* In de declaratie van een verborgen laag, het aantal knooppunten is het aantal dat is opgegeven door de waarde van parameter voor **aantal knooppunten dat verborgen**. 
* Het aantal knooppunten is in de declaratie van een uitvoer-laag 2 voor tweeklasse classificatie, 1 voor regressie en gelijk is aan het aantal uitvoerknooppunten voor multiklassen classificatie.   

De volgende netwerkdefinitie kan bijvoorbeeld de grootte van alle lagen automatisch bepaald:  

    input Data auto;
    hidden Hidden auto from Data all;
    output Result auto from Hidden all;  


De declaratie van een laag voor een trainable laag (de lagen verborgen of uitvoer) kunt u eventueel de uitvoer functie (ook wel een functie voor activering genoemd), die een standaardwaarde opnemen **sigmoid** voor classificatie-modellen en  **lineaire** voor regressie-modellen. (Zelfs als u de standaard gebruikt, u kunt expliciet aangeven van de functie activeren, indien gewenst voor de duidelijkheid.)

De volgende uitvoer-functies worden ondersteund:  

* sigmoid
* Lineair
* softmax
* rlinear
* vierkante
* SQRT
* srlinear
* ABS
* TANH 
* brlinear  

De volgende declaratie gebruikt bijvoorbeeld de **softmax** functie:  

    output Result [100] softmax from Hidden all;  

## <a name="connection-declaration"></a>Verbinding declaratie
Direct na het definiëren van de trainable laag, moet u de verbindingen tussen de lagen die u hebt gedefinieerd declareren. De declaratie van de bundel verbinding begint met het sleutelwoord **van**, gevolgd door de naam van de bundel bronlaag en de aard van de bundel verbinding te maken.   

Op dit moment worden verbinding bundels vijf typen ondersteund:  

* **Volledige** bundels, aangegeven door het sleutelwoord **alle**
* **Gefilterd** bundels, aangegeven door het sleutelwoord **waar**, gevolgd door een predikaat expressie
* **Convolutional** bundels, aangegeven door het sleutelwoord **convolve**, gevolgd door de kenmerken convolutiefilter
* **Groeperen van** bundels, aangegeven door de trefwoorden **maximum aantal toepassingen** of **betekent dat de groep van toepassingen**
* **Antwoord normalisatie** bundels, aangegeven door het sleutelwoord **antwoord norm**      

## <a name="full-bundles"></a>Volledige bundels
Een volledige verbinding bundel bevat een verbinding van elk knooppunt in de bronlaag voor elk knooppunt in de doellaag. Dit is het type standaard netwerkverbinding.  

## <a name="filtered-bundles"></a>Gefilterde bundels
Een gefilterde verbinding bundel specificatie bevat een veel-predicaat, syntaxis, uitgedrukt als een C# lambda-expressie. Het volgende voorbeeld definieert twee gefilterde bundels:  

    input Pixels [10, 20];
    hidden ByRow[10, 12] from Pixels where (s,d) => s[0] == d[0];
    hidden ByCol[5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;  

* In het predikaat voor *ByRow*, **s** is een parameter die een index in de rechthoekige matrix van knooppunten van de invoer laag vertegenwoordigt *Pixels*, en **d** is een parameter die een index in de matrix van knooppunten in de verborgen laag vertegenwoordigt *ByRow*. Het type van beide **s** en **d** is een tuple van gehele getallen van twee lengte. Conceptueel gezien **s** bereiken via alle paren van gehele getallen met *0 < = s [0] < 10* en *0 < = s[1] < 20*, en **d**  bereiken via alle paren van gehele getallen, met *0 < = [0] d < 10* en *0 < = d[1] < 12*. 
* Aan de rechterkant van de predikaatexpressie die is is er een voorwaarde. In dit voorbeeld wordt voor elke waarde van **s** en **d** zodanig dat de voorwaarde waar is, er is een rand van het bronknooppunt laag naar het doelknooppunt voor de laag. Dus deze filterexpressie geeft aan dat de bundel een verbinding van het knooppunt dat is gedefinieerd bevat door **s** naar het knooppunt dat is gedefinieerd door **d** in alle gevallen waarbij s [0] gelijk aan [0] d is.  

U kunt eventueel opgeven dat een set van gewicht voor een gefilterde bundel. De waarde voor de **gewichten** kenmerk moet een tuple met drijvende puntwaarden met een lengte die overeenkomt met het aantal verbindingen dat is gedefinieerd door de bundel. Standaard worden gewichten willekeurig gegenereerd.  

Gewichtswaarden zijn gegroepeerd op de index van de doel-knooppunt. Is als het eerste bestemmingsknooppunt is verbonden met K bron knooppunten, de eerste *K* elementen van de **gewichten** tuple zijn het gewicht voor het eerste doelknooppunt in volgorde van de gegevensbron-index. Hetzelfde geldt voor de resterende knooppunten van de bestemming.  

Het is mogelijk gewichten rechtstreeks als constante waarden opgeven. Bijvoorbeeld, als u het gewicht eerder hebt geleerd, kunt u ze als constanten gebruik de volgende syntaxis:

    const Weights_1 = [0.0188045055, 0.130500451, ...]


## <a name="convolutional-bundles"></a>Convolutional bundels
Wanneer de trainingsgegevens een homogene structuur heeft, worden convolutional verbindingen worden vaak gebruikt voor meer informatie over geavanceerde functies van de gegevens. Gegevens bijvoorbeeld: in afbeelding audio- of videobestanden, ruimtelijke of tijdelijke dimensionaliteit kunnen zijn redelijk uniform.  

Convolutional bundels alvast rechthoekige **kernels** die via de dimensies worden geschoven. In wezen elke kernel definieert een reeks gewichten toegepast in de lokale groepen genoemd **kerneltoepassingen**. Elke toepassing kernel komt overeen met een knooppunt in de bronlaag, waarnaar wordt verwezen als de **centrale knooppunt**. Het gewicht van een kernel worden gedeeld door veel verbindingen. In een convolutional bundel elke kernel is rechthoekig en alle kerneltoepassingen zijn dezelfde grootte hebben.  

Convolutional bundels ondersteunen de volgende kenmerken:

**InputShape** definieert de dimensionaliteit van de bronlaag voor de doeleinden van deze convolutional bundel. De waarde moet een tuple van positieve gehele getallen zijn. Het aantal knooppunten in de bronlaag moet gelijk zijn aan het product van de gehele getallen, maar anders hoeft niet overeenkomen met de dimensionaliteit is gedeclareerd voor de bronlaag. De lengte van deze tuple wordt de **ariteit** waarde voor de convolutional bundel. (Meestal ariteit verwijst naar het aantal argumenten of operanden die kan worden uitgevoerd door een functie.)  

Gebruik de kenmerken definiëren van de vorm en de locaties van de kernels **KernelShape**, **Stride**, **opvulling**, **LowerPad**, en  **UpperPad**:   

* **KernelShape**: (vereist) definieert de dimensionaliteit van elke kernel voor de convolutional bundel. De waarde moet een tuple van positieve gehele getallen met een lengte die gelijk is aan de ariteit van de bundel. Elk onderdeel van deze tuple mag niet langer zijn dan het overeenkomstige onderdeel van **InputShape**. 
* **Stride**: (optioneel) definieert de verschuivende stap grootte van de convolutiefilter (één stapgrootte voor elke dimensie), die de afstand tussen de centrale knooppunten. De waarde moet een tuple van positieve gehele getallen met een lengte die de ariteit van de bundel. Elk onderdeel van deze tuple mag niet langer zijn dan het overeenkomstige onderdeel van **KernelShape**. De standaardwaarde is een tuple met alle onderdelen die gelijk zijn aan één. 
* **Delen**: (optioneel) definieert het gewicht voor elke dimensie van de convolutiefilter delen. De waarde kan niet één Booleaanse waarde of een tuple van Booleaanse waarden met een lengte die de ariteit van de bundel. Een enkele Booleaanse waarde is uitgebreid om te worden van een tuple van de juiste lengte met alle onderdelen die gelijk is aan de opgegeven waarde. De standaardwaarde is een tuple die uit alle waar waarden bestaat. 
* **MapCount**: (optioneel) definieert het aantal van de functie voor de convolutional bundel wordt toegewezen. De waarde kan niet een enkel positief geheel getal of een tuple van positieve gehele getallen met een lengte die de ariteit van de bundel. Een enkele geheelgetalwaarde is uitgebreid om te worden van een tuple van de juiste lengte van de eerste onderdelen die gelijk is aan de opgegeven waarde en de resterende onderdelen gelijk is aan één. De standaardwaarde is een. Het totale aantal functie maps is het product van de onderdelen van de tuple. Waarbij van dit totale aantal voor de onderdelen bepaalt hoe de waarden van de kaart functie worden gegroepeerd in de doel-knooppunten. 
* **Gewicht**: (optioneel) definieert het eerste gewicht voor de bundel. De waarde moet een tuple met drijvende puntwaarden met een lengte van het aantal keren kernels het nummer van de gewichten per kernel, zoals verderop in dit artikel is gedefinieerd. De standaardgewichten worden willekeurig gegenereerd.  

Er zijn twee sets van eigenschappen die regelen opvulling, de eigenschappen wordt sluiten elkaar wederzijds uit:

* **Opvulling**: (optioneel) bepaalt of de invoer moet zijn opgevuld met behulp van een **opvulling standaardschema**. De waarde kan één Booleaanse waarde, of het kan ook een tuple van Booleaanse waarden met een lengte die de ariteit van de bundel. Een enkele Booleaanse waarde is uitgebreid om te worden van een tuple van de juiste lengte met alle onderdelen die gelijk is aan de opgegeven waarde. Als de waarde voor een dimensie True is, worden de bron logisch opgevuld in die dimensie met de cellen met nul ter ondersteuning van aanvullende kerneltoepassingen, zodat de centrale knooppunten van het eerste en laatste kernels in die dimensie zijn de eerste en laatste knooppunten dat in dimensie in de bronlaag. Dus het aantal 'dummy' knooppunten in elke dimensie wordt automatisch bepaald, past precies *(InputShape [d] - 1) / Stride [d] + 1* kernels in de bronlaag gevuld. Als de waarde voor een dimensie False is, worden de kernels gedefinieerd, zodat het aantal knooppunten voor elke zijde die worden weggelaten hetzelfde (maximaal een verschil van 1 wordt). De standaardwaarde van dit kenmerk is een tuple met alle onderdelen die gelijk is aan False.
* **UpperPad** en **LowerPad**: (optioneel) Geef meer controle over de hoeveelheid opvulling te gebruiken. **Belangrijk:** deze kenmerken kunnen worden gedefinieerd als en alleen als de **opvulling** eigenschap hierboven is ***niet*** gedefinieerd. De waarden moet geheel getalwaarde tuples met een lengte die de ariteit van de bundel. Als deze kenmerken worden opgegeven, wordt 'dummy' knooppunten worden toegevoegd aan de boven- en ondergrenzen ends van iedere dimensie van de invoer laag. Het aantal knooppunten die zijn toegevoegd aan de boven- en ondergrenzen ends in elke dimensie wordt bepaald door **LowerPad**[i] en **UpperPad**[i] respectievelijk. Om ervoor te zorgen dat kernels alleen naar 'echte' knooppunten en niet 'dummy' knooppunten overeenkomen, moeten de volgende voorwaarden worden voldaan:
  * Elk onderdeel van **LowerPad** moet strikt kleiner zijn dan KernelShape [d] / 2. 
  * Elk onderdeel van **UpperPad** mag niet langer zijn dan KernelShape [d] / 2. 
  * De standaardwaarde van deze kenmerken is een tuple met alle onderdelen die gelijk zijn aan 0. 

De instelling **opvulling** = true kunt zoveel opvulling te houden van het 'midden' van de kernel binnen de 'werkelijke' invoer nodig is. Hiermee wijzigt u de math iets voor het berekenen van de uitvoergrootte van de. In het algemeen de uitvoergrootte van de *D* wordt berekend als *D = (I - K) / S + 1*, waarbij *ik* is de ingevoerde grootte *K* is de grootte kernel *S*  is de stride en  */*  deling van geheel getal (afronden op nul) is. Als u UpperPad instellen = [1, 1], de grootte van de invoer *ik* is in feite 29, en dus *D = (29-5) / 2 + 1 = 13*. Echter, wanneer **opvulling** = true, wordt in wezen *ik* opgehaald tegenaan door *K - 1*; daarom *D = ((28 + 4) - 5) / 2 + 1 = 27 / 2 + 1 = 13 + 1 = 14*. Door op te geven waarden voor **UpperPad** en **LowerPad** dat u veel meer controle over de opvulling dan als u zojuist hebt ingesteld **opvulling** = true.

Zie voor meer informatie over convolutional netwerken en hun toepassingen in deze artikelen:  

* [http://deeplearning.NET/Tutorial/lenet.HTML](http://deeplearning.net/tutorial/lenet.html)
* [http://Research.Microsoft.com/pubs/68920/icdar03.PDF](http://research.microsoft.com/pubs/68920/icdar03.pdf) 
* [http://People.csail.MIT.edu/jvb/Papers/cnn_tutorial.PDF](http://people.csail.mit.edu/jvb/papers/cnn_tutorial.pdf)  

## <a name="pooling-bundles"></a>Bundels groeperen
Een **groeperen bundel** van toepassing is vergelijkbaar met convolutional connectiviteit geometrie maar vooraf gedefinieerde waarden van de bron-functies worden afgeleid van de waarde van de doel-knooppunt wordt gebruikt. Groepering bundels hebben daarom geen trainable status (gewichten of vooroordelen). Groepering bundels ondersteuning voor alle convolutional kenmerken behalve **delen**, **MapCount**, en **gewichten**.  

Normaal gesproken overlappen de kernels samengevat door aangrenzende groepering eenheden niet. Als Stride [d] gelijk aan KernelShape [d] in elke dimensie is, is de laag verkregen de traditionele lokale groepering laag, die meestal in convolutional neural netwerken gebruikt wordt. Elke doelknooppunt berekent het maximum of het gemiddelde van de activiteiten van de kernel in de bronlaag.  

Het volgende voorbeeld wordt een groepering bundel: 

    hidden P1 [5, 12, 12]
      from C1 max pool {
        InputShape  = [ 5, 24, 24];
        KernelShape = [ 1,  2,  2];
        Stride      = [ 1,  2,  2];
      }  

* De ariteit van de bundel is 3 (de lengte van de tuples **InputShape**, **KernelShape**, en **Stride**). 
* Het aantal knooppunten in de bronlaag is *5 * 24 * 24 = 2880*. 
* Dit is een traditionele lokale groepering laag omdat **KernelShape** en **Stride** gelijk zijn. 
* Het aantal knooppunten in de doellaag is *5 * 12 * 12 = 1440*.  

Zie voor meer informatie over groepering lagen deze artikelen:  

* [http://www.cs.Toronto.edu/~hinton/absps/imagenet.PDF](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf) (sectie 3.4)
* [http://CS.Nyu.edu/~koray/publis/lecun-iscas-10.PDF](http://cs.nyu.edu/~koray/publis/lecun-iscas-10.pdf) 
* [http://CS.Nyu.edu/~koray/publis/jarrett-iccv-09.PDF](http://cs.nyu.edu/~koray/publis/jarrett-iccv-09.pdf)

## <a name="response-normalization-bundles"></a>Antwoord normalisatie bundels
**Antwoord normalisatie** is een lokale normalisatie-schema dat is geïntroduceerd door Geoffrey Hinton, et al., in het artikel [ImageNet Classiﬁcation met Deep Convolutional Neural Networks](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf). Antwoord normalisatie wordt gebruikt om u te helpen generaliseren van neural netten. Wanneer een neuron wordt uitgevoerd op een zeer hoge Activeringsniveau, wordt in een lokale antwoord normalisatie-laag het Activeringsniveau van de omringende neurons onderdrukt. Dit wordt gedaan met drie parameters (***α***, ***β***, en ***k***) en een convolutional structuur (of groep vorm). Elke neuron in de doellaag ***y*** komt overeen met een neuron ***x*** in de bronlaag. Het Activeringsniveau van ***y*** wordt bepaald door de volgende formule, waarbij ***f*** is het Activeringsniveau van een neuron en ***Nx*** is de kernel (of de set met de neurons in de groep van ***x***), zoals gedefinieerd door de volgende convolutional structuur:  

![][1]  

Antwoord normalisatie bundels ondersteuning voor alle convolutional kenmerken behalve **delen**, **MapCount**, en **gewichten**.  

* Als de kernel neurons in dezelfde toewijzing als bevat ***x***, het schema van normalisatie wordt aangeduid als **dezelfde normalisatie toewijzen**. Voor het definiëren van dezelfde kaart normalisatie wordt de eerste coördinaat in **InputShape** moet de waarde 1 hebben.
* Als de kernel neurons in dezelfde ruimtelijke positie als bevat ***x***, maar de neurons zijn in andere toewijzingen, de normalisatie-schema heet **meerdere toegewezen normalisatie**. Dit type antwoord normalisatie implementeert een vorm van laterale maar die is geïnspireerd op het type gevonden in de echte neurons concurrentie voor big activering niveaus onder neuron uitvoer berekend op verschillende maps maken. Om te definiëren tussen maps normalisatie, moet de eerste coördinaat een geheel getal groter dan één en niet groter zijn dan het aantal maps en de rest van de coördinaten moet de waarde 1 hebben.  

Omdat een vooraf gedefinieerde functie antwoord normalisatie bundels op bron knooppuntwaarden om te bepalen van de waarde van de doel-knooppunt toepassen, hebben ze geen trainable status (gewichten of vooroordelen).   

**Waarschuwing**: de knooppunten in de doellaag komen overeen met neurons die de centrale knooppunten van de kernels. Bijvoorbeeld, als KernelShape [d] oneven en wordt vervolgens *KernelShape [d] / 2* komt overeen met het centrale kernel-knooppunt. Als *KernelShape [d]* een even getal is, is het centrale knooppunt *KernelShape [d] / 2-1*. Daarom als **opvulling**[d] is False, de eerste en de laatste *KernelShape [d] / 2* knooppunten hebben geen overeenkomstige knooppunten in de doellaag. Om te voorkomen dat deze situatie, definiëren **opvulling** als [true, true,..., true].  

Naast de vier kenmerken die eerder zijn beschreven, ondersteuning antwoord normalisatie bundels ook voor de volgende kenmerken:  

* **Alpha**: (vereist) geeft een drijvende-kommawaarde die overeenkomt met ***α*** in de vorige formule. 
* **Beta**: (vereist) geeft een drijvende-kommawaarde die overeenkomt met ***β*** in de vorige formule. 
* **Offset**: (optioneel) geeft een drijvende-kommawaarde die overeenkomt met ***k*** in de vorige formule. Wordt standaard op 1.  

Het volgende voorbeeld wordt een antwoord normalisatie-bundel met behulp van deze kenmerken gedefinieerd:  

    hidden RN1 [5, 10, 10]
      from P1 response norm {
        InputShape  = [ 5, 12, 12];
        KernelShape = [ 1,  3,  3];
        Alpha = 0.001;
        Beta = 0.75;
      }  

* De bronlaag bevat vijf maps, elk met aof dimensie van 12 x 12 Samentelling in 1440 knooppunten. 
* De waarde van **KernelShape** geeft aan dat dit een dezelfde normalisatie kaartLaag, waarbij de groep een rechthoek 3 x 3 is. 
* De standaardwaarde van **opvulling** is ingesteld op False, de laag bestemming heeft dus alleen 10 knooppunten in elke dimensie. Toevoegen zodat één knooppunt in de doellaag die overeenkomt met elk knooppunt in de bronlaag opvulling = [true, true, true]; en wijzig de grootte van RN1 [5, 12, 12].  

## <a name="share-declaration"></a>Share-declaratie
NET # ondersteunt eventueel meerdere pakketten met gedeelde gewichten definiëren. Het gewicht van elke twee bundels kunnen worden gedeeld als hun structuren hetzelfde zijn. De volgende syntaxis definieert bundels met gedeelde gewichten:  

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

De volgende share-declaratie geeft bijvoorbeeld de namen van de lagen die aangeeft dat de gewichten en vooroordelen worden gedeeld op:  

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

* De invoer functies worden gepartitioneerd in twee gelijke grootte invoer lagen. 
* Verborgen lagen berekent hoger niveau functies op de twee lagen van de invoer. 
* De share-declaratie geeft aan dat *H1* en *H2* op dezelfde manier uit hun respectieve invoer moet worden berekend.  

U kunt ook kan dit worden opgegeven met twee afzonderlijke share-declaraties als volgt:  

    share { Data1 => H1, Data2 => H2 } // share weights  

<!-- -->

    share { 1 => H1, 1 => H2 } // share biases  

U kunt de korte vorm alleen als de lagen één bundel bevatten. In het algemeen is delen mogelijk alleen wanneer de structuur van de relevante identiek zijn is, zodat ze dezelfde grootte hebben, dezelfde convolutional geometrie, enzovoort.  

## <a name="examples-of-net-usage"></a>Voorbeelden van Net # gebruik
Deze sectie vindt enkele voorbeelden van hoe u Net # kunt verborgen lagen toevoegen, het definiëren van de manier waarop verborgen lagen met andere lagen communiceren en convolutional netwerken maken.   

### <a name="define-a-simple-custom-neural-network-hello-world-example"></a>Een eenvoudig aangepaste neurale netwerk definiëren: 'Hallo wereld'-voorbeeld
Dit eenvoudige voorbeeld laat zien hoe een neural network-model met één verborgen laag maken.  

    input Data auto;
    hidden H [200] from Data all;
    output Out [10] sigmoid from H all;  

Het voorbeeld wordt een aantal basisopdrachten als volgt:  

* De eerste regel definieert de invoer-laag (met de naam *gegevens*). Wanneer u gebruikt de **automatisch** sleutelwoord, het neurale netwerk omvat alle kolommen van de functie automatisch in de invoer voorbeelden. 
* De tweede regel maakt de verborgen laag. De naam van de *H* is toegewezen aan de verborgen laag waarvoor 200 knooppunten. Deze laag volledig wordt verbonden met de invoer laag.
* De derde regel definieert de uitvoer-laag (met de naam *O*), die 10 uitvoerknooppunten bevat. Als het neurale netwerk wordt gebruikt voor classificatie, is er één knooppunt van de uitvoer per klasse. Het sleutelwoord **sigmoid** geeft aan dat de uitvoer-functie wordt toegepast op de uitvoer-laag.   

### <a name="define-multiple-hidden-layers-computer-vision-example"></a>Meerdere verborgen lagen definiëren: computer vision-voorbeeld
Het volgende voorbeeld laat zien hoe een enigszins complexere neurale netwerk, met meerdere lagen voor aangepaste verborgen definiëren.  

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

In dit voorbeeld ziet u enkele functies van de taal van de specificatie neural netwerken:  

* De structuur heeft twee invoer lagen, *Pixels* en *metagegevens*.
* De *Pixels* laag is een bronlaag voor twee verbinding-bundels met lagen van de bestemming, *ByRow* en *ByCol*.
* De lagen *verzamelen* en *resultaat* bestemming lagen in meerdere pakketten van de verbinding zijn.
* De laag uitvoer *resultaat*, is een doellaag in twee verbinding bundels; één met de tweede niveau als een laag bestemming verborgen (verzamelen) en de andere met de invoer-laag (MetaData) als een laag bestemming.
* Het verborgen lagen *ByRow* en *ByCol*, gefilterde verbinding opgeven met behulp van predikaat expressies. Preciezer, het knooppunt in *ByRow* op [x, y] is verbonden met de knooppunten in *Pixels* dat zijn de eerste index coördineren gelijk zijn aan het knooppunt van de eerste coördinaat, x. Op deze manier het knooppunt in *ByCol op [x, y] is verbonden met de knooppunten in _Pixels* dat hebben de tweede index coördineren binnen één van het knooppunt van de tweede coördinaat, y.  

### <a name="define-a-convolutional-network-for-multiclass-classification-digit-recognition-example"></a>Definieer een convolutional netwerk voor multiklassen classificatie: cijfer erkenning voorbeeld
De definitie van de volgende netwerk is ontworpen voor het herkennen van cijfers en sommige geavanceerde technieken voor het aanpassen van een neural netwerk worden geïllustreerd.  

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


* De structuur heeft één invoer laag *installatiekopie*.
* Het sleutelwoord **convolve** geeft aan dat de lagen naam *Conv1* en *Conv2* convolutional lagen. Elk van deze laag declaraties wordt gevolgd door een lijst van de kenmerken convolutiefilter.
* De net is een derde verborgen laag *Hid3*, die volledig is verbonden met de tweede verborgen laag *Conv2*.
* De laag uitvoer *cijfer*, is alleen verbonden met de derde verborgen laag *Hid3*. Het sleutelwoord **alle** geeft aan dat de uitvoer-laag volledig is verbonden met *Hid3*.
* De ariteit van de convolutiefilter is drie (de lengte van de tuples **InputShape**, **KernelShape**, **Stride**, en **delen**). 
* Het aantal van de gewichten per kernel *1 + **KernelShape**\[0] * **KernelShape**\[1] * **KernelShape** \[ 2] = 1 + 1 * 5 * 5 = 26. Of 26 * 50 = 1300*.
* U kunt de knooppunten in de verborgen laag als volgt berekenen:
  * **NodeCount**\[0] = (5 - 1) / 1 + 1 = 5.
  * **NodeCount**\[1] = (13-5) / 2 + 1 = 5. 
  * **NodeCount**\[2] (13-5) = / 2 + 1 = 5. 
* Het totale aantal knooppunten kan worden berekend met behulp van de gedeclareerde dimensionaliteit van de laag [50, 5, 5], als volgt:  ***MapCount** * **NodeCount**\[0] * **NodeCount**\[1] * **NodeCount**\[2] = 10 * 5 * 5 * 5*
* Omdat **delen**[d] False is alleen voor *d == 0*, is het aantal kernels  ***MapCount** * **NodeCount** \[0] = 10 * 5 = 50*. 

## <a name="acknowledgements"></a>Bevestigingen
De taal Net # voor het aanpassen van de architectuur van neural networks is ontwikkeld aan Microsoft door Shon Katzenberger (Architect, Machine Learning) en Alexey Kamenev (Software Engineer, Microsoft Research). Het wordt intern gebruikt voor machine learning-projecten en toepassingen, variërend van detectie van de afbeelding tot tekstanalyse. Zie voor meer informatie [Neural netten in Azure ML - Inleiding tot Net #](http://blogs.technet.com/b/machinelearning/archive/2015/02/16/neural-nets-in-azure-ml-introduction-to-net.aspx)

[1]:./media/azure-ml-netsharp-reference-guide/formula_large.gif

