---
title: Analyse van de klant verloop met behulp van Machine Learning | Microsoft Docs
description: "Casestudy van een geïntegreerde model voor het analyseren en score berekenen voor verloop van de klant te ontwikkelen"
services: machine-learning
documentationcenter: 
author: jeannt
manager: jhubbard
editor: cgronlun
ms.assetid: 1333ffe2-59b8-4f40-9be7-3bf1173fc38d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2017
ms.author: jeannt
ms.openlocfilehash: e0b82fe8e8c8bc4ac9c45370d90fa9330d749878
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="analyzing-customer-churn-by-using-azure-machine-learning"></a>Traject van de klant analyseren met Azure Machine Learning
## <a name="overview"></a>Overzicht
Dit artikel bevat een verwijzing implementatie van een klant verloop analysis project is gebouwd met behulp van Azure Machine Learning. In dit artikel wordt besproken gekoppelde algemene modellen voor het oplossen van het probleem van industriële customer verloop holistische. We ook meet de nauwkeurigheid van modellen die zijn gebouwd met behulp van Machine Learning en beoordelen routebeschrijving voor verdere ontwikkeling.  

### <a name="acknowledgements"></a>Bevestigingen
Dit experiment is ontwikkeld en getest door Serge Berger, Principal gegevens wetenschappelijk bij Microsoft en Roger Barga, voorheen Product Manager van Microsoft Azure Machine Learning. Het team van Azure-documentatie dank erkent hun expertise en ze Bedankt voor het delen van dit document.

> [!NOTE]
> De gegevens die worden gebruikt voor dit experiment is niet openbaar beschikbaar. Zie voor een voorbeeld van het bouwen van een machine learning-model voor verloop analyse: [Retail verloop model sjabloon](https://gallery.cortanaintelligence.com/Collection/Retail-Customer-Churn-Prediction-Template-1) in [AI-galerie van Azure](http://gallery.cortanaintelligence.com/)
> 
> 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="the-problem-of-customer-churn"></a>Het probleem van klanten verloop
Bedrijven in de markt consumer en in alle sectoren voor ondernemingen hebben om te gaan met het verloop. Soms verloop uitzonderlijk veel en beleidsbeslissingen beïnvloedt. De traditionele oplossing is te hoog investeringsneiging churners voorspellen en hun behoeften via een service concierge marketingcampagnes, of door het toepassen van speciale dispensaties. Deze methoden kunnen bedrijfstak bedrijfstak variëren. Ze kunnen zelfs verschillen vanaf een cluster bepaalde consument binnen één industry (bijvoorbeeld telecommunicatie).

De algemene factor is dat bedrijven moeten deze speciale klant bewaren inspanningen minimaliseren. Dus kan een natuurlijke methodologie worden score van elke klant met de waarschijnlijkheid van verloop en los van de bovenste N die zijn. De belangrijkste klanten mogelijk het meest winstgevend die zijn. In meer geavanceerde scenario's een winst-functie gebruikt tijdens de selectie van kandidaten voor speciale dispensatie. Deze overwegingen zijn echter slechts een deel van de complete strategie voor het omgaan met verloop. Bedrijven hebben ook rekening account risico (en bijbehorende risicotolerantie), wordt het niveau en de kosten van de tussenkomst en aannemelijke klant segmentering.  

## <a name="industry-outlook-and-approaches"></a>Bedrijfstak outlook en methoden
Geavanceerde verwerking van verloop is een teken van een goed ontwikkelde industrie. Het klassieke voorbeeld is de branche telecommunicatie waar abonnees bekend is dat ze vaak overschakelen van één provider naar het andere. Deze vrijwillige verloop zijn de belangrijkste zorgen. Bovendien providers aanzienlijke kennis hebben verzameld over *verloop van stuurprogramma's*, zijn de factoren die van station klanten om over te schakelen.

Telefoon- of apparaat keuze is voor het exemplaar een bekende stuurprogramma van het verloop in het bedrijf mobiele telefoon. Als gevolg hiervan is een populair beleid voor de prijs van een toestel subsidize voor nieuwe abonnees en een volledige prijs voor bestaande klanten voor een upgrade. Dit beleid heeft in het verleden hebben geleid tot klanten hopping van één provider naar een andere nieuwe korting te krijgen. Dit heeft providers hun strategieën verfijnen op zijn beurt gevraagd.

Hoge volatiliteit in de offerings telefoon is een factor waardoor snel modellen van het verloop op basis van huidige telefoon-modellen. Bovendien worden mobiele telefoons zijn niet alleen telecommunicatie apparaten, zijn ook wijze instructies (Overweeg de iPhone). Deze sociale variabelen vallen buiten het bereik van reguliere telecommunicatie gegevenssets.

Het resultaat voor modellering is dat u een geluid beleid kan niet ontwerpen gewoon doordat bekende redenen voor het verloop. Een strategie voor een continue modellering, met inbegrip van klassieke modellen die categorische variabelen (zoals beslissingsstructuren kwantificeren) is in feite **verplichte**.

Met behulp van grote gegevenssets op hun klanten, uitvoert organisaties big data-analyses (met name verloop detectie op basis van big data) als een doeltreffende methode om het probleem. U vindt meer informatie over de big data-aanpak voor het probleem van het verloop in de aanbevelingen voor ETL-sectie.  

## <a name="methodology-to-model-customer-churn"></a>Methodologie voor het model klant verloop
Een algemene proces oplossen van problemen voor het oplossen van de klant verloop wordt beschreven in cijfers 1-3:  

1. Een risicomodel kunt u overwegen hoe acties invloed op kans en risico's.
2. Een model tussenkomst kunt u rekening houden met het niveau van tussenkomst kan invloed van de waarschijnlijkheid van verloop en de hoeveelheid klant verlooptijd (CLV).
3. Deze analyse gepaard met een kwalitatieve-analyse wordt doorverwezen naar een proactieve marketingcampagne die gericht is op klantsegmenten voor het leveren van de optimale aanbieding.  

![][1]

Deze aanpak sturen ogende is de beste manier om het verloop behandelen, maar wordt geleverd met complexiteit: we hebben een meerdere model archetype en tracering afhankelijkheden tussen de modellen te ontwikkelen. De interactie tussen modellen kunt worden ingekapseld, zoals wordt weergegeven in het volgende diagram:  

![][2]

*Afbeelding 4: Unified met meerdere modellen archetype*  

Interactie tussen de modellen is sleutel als we een holistische benadering om aan te leveren klant bewaren. Elk model vermindert noodzakelijkerwijs gedurende een bepaalde periode; de architectuur is daarom een impliciete lus (vergelijkbaar met de archetype ingesteld door de standaard heldere DM gegevens voor analysemodel [***3***]).  

De algehele cyclus van de risico-besluit-marketing segmentering/afbreken is nog steeds een algemene structuur, die van toepassing op veel zakelijke problemen is. Verloop analyse is gewoon een sterke vertegenwoordiger van deze groep van problemen, omdat deze de kenmerken van complexe zakelijke problemen die niet in een vereenvoudigde voorspellende oplossing staat vertoont. De sociale aspecten van de moderne aanpak voor het verloop in vooral niet in de aanpak zijn gemarkeerd, maar de sociale aspecten worden ingekapseld in de archetype modelleren als ze in een model zou worden.  

Hier een interessante toevoeging is big data-analyses. De hedendaagse telecommunicatie en retail bedrijven volledig worden gegevens verzameld over hun klanten en we kunnen eenvoudig voorzien dat de noodzaak voor meerdere model connectiviteit een algemene trend, bepaalde trends zoals het Internet der dingen opkomende en alomtegenwoordige zal worden apparaten, waardoor business alvast smart-oplossingen met meerdere lagen.  

 

## <a name="implementing-the-modeling-archetype-in-machine-learning-studio"></a>Implementatie van de archetype modelleren in Machine Learning Studio
Gezien het probleem dat hierboven wordt beschreven, wat is de beste manier om een geïntegreerde model en score berekenen benadering implementeren? In deze sectie wordt gedemonstreerd hoe we dit doen met behulp van Azure Machine Learning Studio.  

De aanpak van meerdere model is een moet bij het ontwerpen van een globale archetype voor verloop. Zelfs het scoreprofiel (voorspellende) deel van de aanpak moet met meerdere modellen.  

Het volgende diagram toont het model dat wordt gemaakt, die de veiligheidsmaatregelen voor vier scoreprofiel algoritmen in Machine Learning Studio te voorspellen verloop. De reden voor het gebruik van een benadering van meerdere model is niet alleen een ensemble-classificatie te maken voor betere nauwkeurigheid, maar ook als bescherming tegen te veel aanpassen en verbeteren prescriptieve Functieselectie.  

![][3]

*Afbeelding 5: Prototype van een benadering modelleren verloop*  

De volgende secties bevatten meer informatie over het prototype score model dat wordt geïmplementeerd met behulp van Machine Learning Studio.  

### <a name="data-selection-and-preparation"></a>Gegevensselectie en voorbereiding
De gegevens worden gebruikt voor het bouwen van modellen en score klanten is verkregen van een verticale CRM-oplossing met de gegevens die verborgen ter bescherming van de privacy van klanten. De gegevens bevat informatie over 8.000 abonnementen in de Verenigde Staten en worden drie bronnen gecombineerd: inrichting gegevens (abonnement metagegevens), gegevens over de activiteit (informatie over het gebruik van het systeem) en klantgegevens voor ondersteuning. De gegevens omvat niet alle zakelijke gerelateerde gegevens over de klanten; bijvoorbeeld, omvat het geen loyaliteit metagegevens of een tegoed scores.  

Ter vereenvoudiging zijn ETL en processen voor opschonen van gegevens buiten het bereik omdat we ervan uitgaan dat het voorbereiden van gegevens is al is gedaan elders.   

Functieselectie voor modellering is gebaseerd op de significantie voorlopige score berekenen van de reeks variabelen, opgenomen in het proces dat gebruikmaakt van de module willekeurige forest. Voor de implementatie in Machine Learning Studio berekend we het gemiddelde, de mediaan en de bereiken voor representatieve functies. Bijvoorbeeld, hebben we aggregaties voor de kwalitatieve gegevens, zoals de minimale en maximale waarden voor gebruikersactiviteit toegevoegd.    

We vastgelegd tijdelijke informatie voor de meest recente zes maanden ook. Gegevens van één jaar geanalyseerd en we tot stand gebracht zelfs als er sprake statistisch aanzienlijke trends was, wordt de gevolgen zijn voor verloop aanzienlijk verminderd na zes maanden.  

Het belangrijkste is dat het hele proces, inclusief ETL, feature selection en modelleren in Machine Learning Studio, met behulp van gegevensbronnen in Microsoft Azure is geïmplementeerd.   

De volgende afbeeldingen laten zien dat de gegevens die is gebruikt.  

![][4]

*Afbeelding 6: Fragment van een gegevensbron (verborgen)*  

![][5]

*Afbeelding 7: Functies opgehaald uit de gegevensbron*
 

> Houd er rekening mee dat deze gegevens persoonlijke zijn en daarom het model en gegevens kunnen niet worden gedeeld.
> Er wordt echter in dit voorbeeld experimenteren voor een vergelijkbaar model met openbaar beschikbare gegevens, de [AI-galerie van Azure](http://gallery.cortanaintelligence.com/): [Telco klant verloop](http://gallery.cortanaintelligence.com/Experiment/31c19425ee874f628c847f7e2d93e383).
> 
> Voor meer informatie over hoe u een verloop Analytics-model met behulp van Cortana Intelligence Suite kunt implementeren, wordt ook aangeraden [in deze video](https://info.microsoft.com/Webinar-Harness-Predictive-Customer-Churn-Model.html) door Senior Program Manager Westerse Hyong Tok. 
> 
> 

### <a name="algorithms-used-in-the-prototype"></a>Die worden gebruikt in het model
We de volgende vier machine learning-algoritmen gebruikt voor het bouwen van de prototype (geen aanpassing):  

1. Logistic regression (LR)
2. Gestimuleerd beslissingsstructuur (BT)
3. Gemiddelde perceptron (AP)
4. Ondersteuning voor vectormachine (SVM)  

Het volgende diagram wordt een deel van het ontwerpoppervlak experiment waarmee wordt aangegeven van de volgorde waarin de modellen zijn gemaakt:  

![][6]  

*Afbeelding 8: Modellen maken in Machine Learning Studio*  

### <a name="scoring-methods"></a>Score berekenen voor methoden
We berekend de vier modellen met behulp van een gegevensset gelabelde training.  

De score gegevensset naar een vergelijkbare model gebouwd met behulp van de desktop editie van SAS Enterprise mijnwerkers 12 ook verzonden. We de nauwkeurigheid van de SAS-model en alle vier Machine Learning Studio modellen gemeten.  

## <a name="results"></a>Resultaten
In deze sectie geven we onze bevindingen over de nauwkeurigheid van de modellen, op basis van de score gegevensset.  

### <a name="accuracy-and-precision-of-scoring"></a>Nauwkeurigheid en precisie van score berekenen
De implementatie in Azure Machine Learning is over het algemeen achter SAS nauwkeurig ongeveer 10-15% (gebied onder Curve of AUC).  

De belangrijkste metrische gegevens in verloop is echter de snelheid misclassification: dat wil zeggen, van de bovenste N churners als voorspelde door de classificatie, wie daadwerkelijk heeft **niet** verloop en nog speciale behandeling ontvangen? Het volgende diagram worden de frequentie van deze misclassification voor alle modellen vergeleken:  

![][7]

*Afbeelding 9: Passau prototype gebied onder curve*

### <a name="using-auc-to-compare-results"></a>Met behulp van AUC om resultaten te vergelijken
Gebied onder Curve (AUC) is een waarde die staat voor een globale meting van *Afscheidbaarheid* tussen de verdeling van scores voor positieve en negatieve populaties. Het is vergelijkbaar met de traditionele ontvanger Operator kenmerk (ROC)-grafiek, maar één belangrijk verschil is dat de metriek AUC vereist niet dat u een waarde voor drempel kiezen. In plaats daarvan de resultaten worden samengevat via **alle** keuzemogelijkheden. Daarentegen is de traditionele ROC-grafiek toont de positieve frequentie op de verticale as en de waarde false positief snelheid op de horizontale as en de classificatie-drempelwaarde varieert.   

AUC wordt doorgaans gebruikt als een maatstaf voor waard voor verschillende algoritmen (of andere systemen) omdat deze modellen moet worden vergeleken met behulp van hun AUC-waarden toestaat. Dit is een populair benadering in sectoren zoals meteorologie en biosciences. Dus vertegenwoordigt AUC een populaire hulpprogramma voor beoordeling van de prestaties van de classificatie.  

### <a name="comparing-misclassification-rates"></a>Misclassification tarieven vergelijken
We vergeleken de tarieven misclassification op de desbetreffende gegevensset met behulp van de CRM-gegevens van ongeveer 8000 abonnementen.  

* De snelheid van de misclassification SAS is 10-15%.
* De snelheid van Machine Learning Studio misclassification is 15-20% voor de top 200 en 300 churners.  

In de branche telecommunicatie is het belangrijk om klanten die het hoogste risico voor het verloop in door het aanbieden van deze een service concierge of andere speciale behandeling hebben. In dat opzicht bereikt de implementatie van de Machine Learning Studio resultaten één lijn met het SAS-model.  

Evenzo, is nauwkeurigheid belangrijker dan de precisie omdat we voornamelijk geïnteresseerd bent in de potentiële churners correct te classificeren.  

Het volgende diagram van Wikipedia (Engelstalig) ziet u de relatie in een afbeelding levendige, gemakkelijk te begrijpen:  

![][8]

*Afbeelding 10: Afweging tussen nauwkeurigheid en juistheid*

### <a name="accuracy-and-precision-results-for-boosted-decision-tree-model"></a>Nauwkeurigheid en juistheid resultaten voor gestimuleerd decision tree model
Het volgende diagram toont de onbewerkte resultaten van score berekenen met behulp van de Machine Learning-model voor het model van de boomstructuur gestimuleerd besluit, die worden de meest nauwkeurige tussen de vier modellen wordt uitgevoerd:  

![][9]

*Afbeelding 11: Gestimuleerd decision tree model kenmerken*

## <a name="performance-comparison"></a>Vergelijking van de prestaties
We vergeleken de snelheid waarmee gegevens is berekend met behulp van de Machine Learning Studio-modellen en een vergelijkbare model dat is gemaakt met behulp van de desktop editie van SAS Enterprise mijnwerkers 12.1.  

De volgende tabel geeft een overzicht van de prestaties van de algoritmen:  

*Tabel 1. Algemene prestaties (nauwkeurigheid) van de algoritmen*

| LR | BT | AP | SVM |
| --- | --- | --- | --- |
| Gemiddelde Model |Het beste Model |Attenderen |Gemiddelde Model |

De modellen in Machine Learning Studio ver-loop SAS met 15-25% voor snelheid van kan worden uitgevoerd, maar de nauwkeurigheid grotendeels op nominaal werd gehost.  

## <a name="discussion-and-recommendations"></a>Discussie en aanbevelingen
In de branche telecommunicatie verschillende procedures hebben voorgedaan analyseren verloop, met inbegrip van:  

* Afgeleid metrische gegevens voor vier fundamentele categorieën:
  * **Entiteit (bijvoorbeeld een abonnement)**. Algemene informatie over het abonnement en/of de klant die het onderwerp van het verloop inrichten.
  * **Activiteit**. Alle informatie over het gebruik mogelijk die is gerelateerd aan de entiteit, bijvoorbeeld het aantal aanmeldingen verkrijgen.
  * **Klantondersteuning**. Verzamelt gegevens uit logboeken van de klant ondersteuning om aan te geven of het abonnement problemen of interactie met de klantondersteuning heeft.
  * **Concurrerende en zakelijke gegevens**. Verkrijgen van mogelijk informatie over de klant (bijvoorbeeld kunnen zijn niet beschikbaar of moeilijk om).
* Belang zijn voor Functieselectie station gebruiken. Dit betekent dat het model van de boomstructuur gestimuleerd besluit altijd een ordertoezegging benadering is.  

Het gebruik van de volgende vier categorieën de illusie die een eenvoudige *deterministische* benadering, op basis van indexen notatie op redelijke factoren per categorie, moet voldoende om te bepalen welke klanten bestaat het risico op verloop. Helaas, hoewel dit begrip aannemelijke lijkt, is een false begrip. De reden is dat verloop een tijdelijke effect is en de factoren die bijdragen voor verloop meestal tijdelijke status zijn. Wat kunt u vandaag de dag van een klant leidt kan afwijken morgen en zeker moeten verschillende zes maanden vanaf nu. Daarom een *probabilistische* model is noodzakelijk.  

Deze belangrijke observatie wordt vaak vergeten in bedrijf, die doorgaans in analytics liever een business intelligence gerichte benadering, voornamelijk omdat deze een eenvoudiger verkopen en admits eenvoudige automation.  

De belofte van analyses met zelfservice met behulp van Machine Learning Studio is echter dat de vier categorieën van informatie, ingedeeld in de afdeling een waardevolle bron voor machine learning over verloop geworden.  

Een andere interessante mogelijkheid binnenkort in Azure Machine Learning is de mogelijkheid een aangepaste module toevoegen aan de opslagplaats van vooraf gedefinieerde modules die al beschikbaar zijn. Deze mogelijkheid wordt in wezen een kans om bibliotheken selecteren en sjablonen maken voor verticale markten maakt. Het is een belangrijk kenmerk van Azure Machine Learning op de markt.  

We hopen dat naar dit onderwerp in de toekomst, met name betrekking hebben op big data-analyses.
  

## <a name="conclusion"></a>Conclusie
Dit artikel beschrijft een praktische strategie om het aanpakken van het algemene probleem van klanten verloop met behulp van een algemene framework. We beschouwd als een prototype voor score berekenen voor modellen en geïmplementeerd met Azure Machine Learning. Ten slotte wordt de nauwkeurigheid en prestaties van de oplossing prototype met betrekking tot vergelijkbare algoritmen in SAS beoordeeld.  

 

## <a name="references"></a>Verwijzingen
[1] predictive Analytics: buiten de voorspellingen, West McKnight informatiebeheer, 2011, p.18-20 juli/augustus.  

[2] Wikipedia artikel: [nauwkeurigheid en juistheid](http://en.wikipedia.org/wiki/Accuracy_and_precision)

[3] [heldere-DM 1.0: stapsgewijze datamining Guide](http://www.the-modeling-agency.com/crisp-dm.pdf)   

[4] [big Data Marketing: uw klanten effectiever benaderen en waarde station](http://www.amazon.com/Big-Data-Marketing-Customers-Effectively/dp/1118733894/ref=sr_1_12?ie=UTF8&qid=1387541531&sr=8-12&keywords=customer+churn)

[5] [Telco verloop model sjabloon](http://gallery.cortanaintelligence.com/Experiment/Telco-Customer-Churn-5) in [AI-galerie van Azure](http://gallery.cortanaintelligence.com/) 
 

## <a name="appendix"></a>Bijlage
![][10]

*Afbeelding 12: Momentopname van een prototype van de verloop-presentatie*

[1]: ./media/azure-ml-customer-churn-scenario/churn-1.png
[2]: ./media/azure-ml-customer-churn-scenario/churn-2.png
[3]: ./media/azure-ml-customer-churn-scenario/churn-3.png
[4]: ./media/azure-ml-customer-churn-scenario/churn-4.png
[5]: ./media/azure-ml-customer-churn-scenario/churn-5.png
[6]: ./media/azure-ml-customer-churn-scenario/churn-6.png
[7]: ./media/azure-ml-customer-churn-scenario/churn-7.png
[8]: ./media/azure-ml-customer-churn-scenario/churn-8.png
[9]: ./media/azure-ml-customer-churn-scenario/churn-9.png
[10]: ./media/azure-ml-customer-churn-scenario/churn-10.png
