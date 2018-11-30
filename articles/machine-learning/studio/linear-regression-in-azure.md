---
title: Lineaire regressie gebruiken in Machine Learning Studio - Azure | Microsoft Docs
description: Een vergelijking van de lineaire regressiemodellen in Excel en in Azure Machine Learning Studio
metakeywords: ''
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=hshapiro, author=heatherbshapiro)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: 417ae6ab-de4f-4bdd-957a-d96133234656
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.openlocfilehash: d7ff9e157f776ebf683846652fe4788fc35e19af
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52311055"
---
# <a name="using-linear-regression-in-azure-machine-learning-studio"></a>Lineaire regressie gebruiken in Azure Machine Learning Studio
> *Kate Baroni* en *Ben Boatman* enterprise oplossingsarchitecten in van de Microsoft Data Insights Center Excellence zijn. In dit artikel beschreven ze hun ervaring migreren van een bestaande regressie analysis-pakket naar een cloud-gebaseerde oplossing met behulp van Azure Machine Learning. 
> 
> 

&nbsp; 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="goal"></a>Doel
Dit project is gestart met twee drie doelen voor ogen: 

1. Predictive analytics gebruiken voor het verbeteren van de nauwkeurigheid van de maandelijkse omzet projecties van onze organisatie 
2. Azure Machine Learning gebruiken om te bevestigen, optimaliseren, uit te voeren, te verhogen en de schaal van onze resultaten. 

Net als veel bedrijven verloopt onze organisatie via een maandelijkse omzet prognoses proces. Onze klein team van bedrijfsanalisten is belast zijn met het gebruik van Azure Machine Learning ter ondersteuning van het proces en de nauwkeurigheid van prognoses verbeteren. Het team dat is besteed aan enkele maanden voor het verzamelen van gegevens uit meerdere bronnen en met de gegevenskenmerken via statistische analyse van id-sleutel kenmerken die relevant zijn voor de verkoopprognoses van services. De volgende stap is om te beginnen met ontwikkelen van prototypen statistische regressiemodellen op de gegevens in Excel. Er is een Excel-regressiemodel dat is de huidige veld en Financiën prognoses processen stapje binnen een paar weken. Dit is het resultaat van de voorspelling basislijn geworden. 

We vervolgens duurde de volgende stap om te verplaatsen van onze voorspellende analyses via Azure Machine Learning om erachter te komen hoe Machine Learning op de voorspellende prestaties kan verbeteren.

## <a name="achieving-predictive-performance-parity"></a>Achieving voorspellende prestaties pariteit
Onze eerste prioriteit is om te realiseren pariteit tussen regressiemodellen Machine Learning en Excel. Gegeven dezelfde gegevens en de splitsing voor trainings- en testdoeleinden gegevens, wilden we bereiken voorspellende prestaties pariteit tussen Excel en Machine Learning. In eerste instantie kan niet. De Excel-gegevensmodel beter dan de Machine Learning-model. De fout is vanwege een gebrek aan begrip van de instelling basis hulpprogramma in Machine Learning. Na een synchronisatie met het productteam van Machine Learning, we ervaring een beter begrip van de base die vereist zijn voor onze gegevenssets instellen en pariteit tussen de twee modellen bereikt. 

### <a name="create-regression-model-in-excel"></a>Regressiemodel maken in Excel
Onze regressie Excel gebruikt de standaard lineair regressiemodel gevonden in de Excel-invoegtoepassing. 

We berekend *Mean Absolute % Error* en gebruikt u deze als de maateenheid voor prestaties voor het model. Dit duurde drie maanden om naar een werkende-model met behulp van Excel. We kunnen veel van de training in de Machine Learning Studio-experiment die uiteindelijk nuttig in vereisten is.

### <a name="create-comparable-experiment-in-azure-machine-learning"></a>Vergelijkbare experiment maken in Azure Machine Learning
We deze stappen voor het maken van onze experiment in Machine Learning Studio hebt gevolgd: 

1. De gegevensset geüpload als een csv-bestand voor Machine Learning Studio (zeer klein bestand)
2. Een nieuw experiment gemaakt en gebruikt de [Select Columns in Dataset] [ select-columns] module te selecteren van de functies van dezelfde gegevens in Excel gebruikt 
3. Gebruikt de [Split Data] [ split] module (met *relatieve expressie* modus) te verdelen van de gegevens in de dezelfde opleiding gegevenssets zoals had gedaan in Excel 
4. Geëxperimenteerd met de [lineaire regressie] [ linear-regression] module (alleen standaardopties), beschreven en de resultaten naar onze Excel-regressiemodel vergeleken

### <a name="review-initial-results"></a>Bekijk de resultaten van de eerste
Op het eerste gezicht de Excel-gegevensmodel duidelijk beter dan het model voor Machine Learning Studio: 

|  | Excel | Studio |
| --- |:---:|:---:|
| Prestaties | | |
| <ul style="list-style-type: none;"><li>Aangepast R-kwadraat</li></ul> |0.96 |N/A |
| <ul style="list-style-type: none;"><li>Correlatiecoëfficiënt <br />Vaststellen</li></ul> |N/A |0.78<br />(nauwkeurigheid laag) |
| Mean Absolute Error |$9.5 MIN. |$ 19.4 MIN. |
| Mean Absolute Error (%) |6.03% |12.2% |

Wanneer we onze proces en de resultaten door de ontwikkelaars en gegevenswetenschappers uitgevoerd op de Machine Learning-team, opgegeven ze snel enkele nuttige tips. 

* Wanneer u gebruikt de [lineaire regressie] [ linear-regression] -module in Machine Learning Studio, twee methoden worden gebruikt:
  * Online verloop afkomst: Mogelijk beter geschikt is voor grotere problemen
  * Gewone kleinste kwadraten: Dit is de methode die de meeste personen denken als ze lineaire regressie hoort. Voor kleine gegevenssets, kan normale kleinste kwadraten een meer optimale keuze zijn.
* Houd rekening met het aanpassen van de parameter L2 regularisatie gewicht om prestaties te verbeteren. Deze standaard is ingesteld op 0,001, maar voor ons kleine gegevensset we hebben het ingesteld op 0,005 om prestaties te verbeteren. 

### <a name="mystery-solved"></a>Mysterie opgelost!
Wanneer we de aanbevelingen hebt toegepast, bereikt we dezelfde basislijnprestaties in Machine Learning Studio als met Excel: 

|  | Excel | Studio (eerste) | Studio met de kleinste kwadraten |
| --- |:---:|:---:|:---:|
| Label waarde |Werkelijke waarden (numeriek) |Dezelfde |Dezelfde |
| Learner |Excel-gegevens > Analysis -> regressie |Lineaire regressie. |Lineaire regressie |
| Opties voor learner |N/A |Standaardwaarden |gewone kleinste kwadraten<br />L2 0,005 = |
| Gegevensset |26 rijen, 3-functies, 1 label. Alle numerieke. |Dezelfde |Dezelfde |
| Splitsen: Train |Excel die zijn getraind op de eerste 18 rijen zijn getest op de laatste 8 rijen. |Dezelfde |Dezelfde |
| Splitsen: Test |Regressieformule toegepast op de laatste 8 rijen in Excel |Dezelfde |Dezelfde |
| **Prestaties** | | | |
| Aangepast R-kwadraat |0.96 |N/A | |
| Correlatiecoëfficiënt |N/A |0.78 |0.952049 |
| Mean Absolute Error |$9.5 MIN. |$ 19.4 MIN. |$9.5 MIN. |
| Mean Absolute Error (%) |<span style="background-color: 00FF00;"> 6.03%</span> |12.2% |<span style="background-color: 00FF00;"> 6.03%</span> |

De Excel-coëfficiënten vergeleken bovendien ook met het gewicht van de functie in het getrainde model van Azure:

|  | Excel coëfficiënten | Gewicht van de functie van Azure |
| --- |:---:|:---:|
| Intercept/afwijking |19470209.88 |19328500 |
| Functie A |0.832653063 |0.834156 |
| Functie B |11071967.08 |11007300 |
| Functie C |25383318.09 |25140800 |

## <a name="next-steps"></a>Volgende stappen
We wilden de Machine Learning-webservice in Excel gebruiken. Onze zakelijke analisten afhankelijk zijn van Excel en we nodig hebben een manier om het aanroepen van de Machine Learning-webservice met een rij met Excel-gegevens en het retourneren van de voorspelde waarde naar Excel. 

Maar we wilden ook ons model optimaliseren met behulp van de opties en de algoritmen die beschikbaar zijn in Machine Learning Studio.

### <a name="integration-with-excel"></a>Integratie met Excel
Onze oplossing is om onze regressiemodel Machine Learning door te maken van een web-service van het getrainde model operationeel te maken. De webservice is gemaakt binnen een paar minuten, en we noemen dat kan rechtstreeks vanuit Excel om de waarde van een voorspelde omzet te retourneren. 

De *Web Services-Dashboard* sectie bevat een downloadbare Excel-werkmap. De werkmap wordt vooraf ingestelde geleverd met de web service-API en schema-informatie ingesloten. Wanneer u klikt op *Excel-werkmap downloaden*, wordt de werkmap geopend en kunt u deze opslaan op uw lokale computer. 

![][1]

Met de werkmap is geopend, kopieert u de vooraf gedefinieerde parameters in de sectie blauw, zoals hieronder weergegeven. Zodra de parameters worden ingevoerd, Excel worden aangeroepen met de Machine Learning-webservice en de voorspelde scored labels worden weergegeven in de sectie met groene voorspelde waarden. De werkmap blijven maken van voorspellingen voor parameters die op basis van het getrainde model voor alle rij items ingevoerd onder Parameters. Zie voor meer informatie over het gebruik van deze functie [gebruiken van een Azure Machine Learning-webservice uit Excel](consuming-from-excel.md). 

![][2]

### <a name="optimization-and-further-experiments"></a>Optimalisatie en verdere experimenten
Nu dat we een basislijn met onze Excel-gegevensmodel hebben gehad, verplaatst we verder naar het optimaliseren van de Machine Learning-Model voor lineaire regressie. We hebben gebruikt de module [Functieselectie op basis van een Filter] [ filter-based-feature-selection] voor het verbeteren van onze selectie van de initiële gegevens-elementen en het heeft ons geholpen een prestatieverbetering van 4.6% bereiken Mean Absolute Error. We gebruiken deze functie die kan ons weken opslaan in de gegevenskenmerken te vinden van de juiste set met functies die moeten worden gebruikt voor modellering doorlopen voor toekomstige projecten. 

Volgende we van plan bent om op te nemen van andere algoritmen, zoals [Bayesiaanse] [ bayesian-linear-regression] of [Boosted Decision Trees] [ boosted-decision-tree-regression] in onze experiment om te vergelijken prestaties. 

Als u experimenteren met regressie wilt, is een goede gegevensset om te proberen de voorbeeldgegevensset energie-efficiëntie regressie, die voorzien van tal van numerieke kenmerken is. De gegevensset wordt geleverd als onderdeel van de voorbeeldgegevenssets in Machine Learning Studio. Om te voorspellen verwarming laden of Load koeling kunt u een verscheidenheid aan learning-modules. Het onderstaande diagram wordt dat een vergelijking van de prestaties van verschillende regressie op basis van de energie-efficiëntie gegevensset voorspellen voor de doelvariabele koeling Load leert: 

| Model | Mean Absolute Error | Root Mean Squared fout | Relative Absolute Error | Fout in ten opzichte van het kwadraat | Correlatiecoëfficiënt |
| --- | --- | --- | --- | --- | --- |
| Boosted-beslisboom |0.930113 |1.4239 |0.106647 |0.021662 |0.978338 |
| Lineaire regressie (met stochastische Gradiëntdaling) |2.035693 |2.98006 |0.233414 |0.094881 |0.905119 |
| Neural Network regressie |1.548195 |2.114617 |0.177517 |0.047774 |0.952226 |
| Lineaire regressie (normale kleinste kwadraten) |1.428273 |1.984461 |0.163767 |0.042074 |0.957926 |

## <a name="key-takeaways"></a>Belangrijkste Takeaways
We veel geleerd door van actieve Excel regressie en Azure Machine Learning-experimenten parallel. Het maken van de basislijn-model in Excel en vergelijken met modellen met behulp van Machine Learning [lineaire regressie] [ linear-regression] ertoe bijgedragen ons informatie over Azure Machine Learning en er mogelijkheden voor verbetering van de gegevens zijn gedetecteerd prestaties van de selectie en het model. 

Er zijn ook gevonden dat u wordt aangeraden om te gebruiken [Functieselectie op basis van een Filter] [ filter-based-feature-selection] voorspelling van toekomstige projecten versnellen. Door toe te passen Functieselectie tot uw gegevens, kunt u een verbeterde model maken in Machine Learning met betere algehele prestaties. 

De mogelijkheid om over te dragen de voorspellende analytische prognose van Machine Learning naar Excel systemically kan een aanzienlijke toename in de mogelijkheid is om resultaten te bieden voor een uitgebreide zakelijke gebruiker publiek. 

## <a name="resources"></a>Resources
Hier volgen enkele resources voor u werken met regressie te helpen: 

* Regressie in Excel. Als u nooit regressie in Excel hebt geprobeerd, met deze zelfstudie kunt eenvoudig kunnen worden gebruikt: [http://www.excel-easy.com/examples/regression.html](http://www.excel-easy.com/examples/regression.html)
* Regressie vs prognose. Tyler Chessman geschreven een blogartikel waarin wordt uitgelegd hoe u uitvoeren op tijden reeks prognoses in Excel, die een goede beginners beschrijving van de lineaire regressie bevat. [http://sqlmag.com/sql-server-analysis-services/understanding-time-series-forecasting-concepts](http://sqlmag.com/sql-server-analysis-services/understanding-time-series-forecasting-concepts) 
* Lineaire regressie van gewone kleinste kwadraten: Fouten, problemen en valkuilen. Voor een inleiding en een beschrijving van de regressie: [http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/ ](http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/)

[1]: ./media/linear-regression-in-azure/machine-learning-linear-regression-in-azure-1.png
[2]: ./media/linear-regression-in-azure/machine-learning-linear-regression-in-azure-2.png


<!-- Module References -->
[bayesian-linear-regression]: https://msdn.microsoft.com/library/azure/ee12de50-2b34-4145-aec0-23e0485da308/
[boosted-decision-tree-regression]: https://msdn.microsoft.com/library/azure/0207d252-6c41-4c77-84c3-73bdf1ac5960/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/

