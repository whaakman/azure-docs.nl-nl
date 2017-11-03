---
title: Met behulp van de lineaire regressie in Machine Learning | Microsoft Docs
description: Een vergelijking van de lineaire regressie modellen in Excel en in Azure Machine Learning Studio
metakeywords: 
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 417ae6ab-de4f-4bdd-957a-d96133234656
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: kbaroni;garye
ms.openlocfilehash: 218f2b141e3551180a2152570f99fdb427980dd7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="using-linear-regression-in-azure-machine-learning"></a>Lineaire regressie gebruiken in Azure Machine Learning
> *Kate Baroni* en *Ben Boatman* enterprise oplossingsarchitecten in Microsoft gegevens Insights Center uitmuntende zijn. In dit artikel beschrijft ze hun ervaring een bestaande regressie analysis suite migreren naar een cloud-gebaseerde oplossing met behulp van Azure Machine Learning. 
> 
> 

&nbsp; 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="goal"></a>Doel
Onze project gestart met twee drie doelen voor ogen: 

1. Predictive analytics gebruiken voor het verbeteren van de juistheid van onze organisatie maandelijkse omzet projecties 
2. Gebruik van Azure Machine Learning om te bevestigen, optimaliseren, snelheid, verhogen en de schaal van onze resultaten. 

Net als veel bedrijven doorloopt onze organisatie een maandelijkse inkomsten prognose proces. Onze klein team van analisten is opgedragen met Azure Machine Learning ter ondersteuning van het proces en de nauwkeurigheid van de prognose te verbeteren. Het team besteed aan enkele maanden verzamelen van gegevens uit meerdere bronnen en het uitvoeren van de gegevenskenmerken via statistische analyse sleutelkenmerken relevant zijn voor services verkoopprognoses te identificeren. De volgende stap is om te beginnen met maken van een prototype statistische regressie modellen van de gegevens in Excel. Binnen een paar weken hebben we een Excel-regressiemodel dat is feit dat het huidige veld en Financiën prognose processen. Dit is het resultaat van de voorspelling basislijn geworden. 

We vervolgens duurde de volgende stap om te verplaatsen van onze predictive analytics via Azure Machine Learning om erachter te komen hoe Machine Learning op de voorspellende prestaties kan verbeteren.

## <a name="achieving-predictive-performance-parity"></a>Achieving voorspellende prestatie pariteit
Ons eerste prioriteit is als u de pariteit tussen Machine Learning en Excel regressie-modellen. We wilden gegeven dezelfde gegevens en dezelfde splitsing voor trainings- en testdoeleinden gegevens, als u de voorspellende prestatie pariteit tussen Excel en Machine Learning. In eerste instantie is mislukt. De Excel-gegevensmodel ver-loop van de Machine Learning-model. De fout is vanwege een gebrek aan kennis van de instelling basis hulpprogramma in Machine Learning. Na een synchronisatie met de Machine Learning-productteam we een beter begrip van de instelling is vereist voor onze gegevenssets base ervaring en pariteit tussen de twee modellen bereikt. 

### <a name="create-regression-model-in-excel"></a>Regressiemodel maken in Excel
Onze regressie Excel gebruikt de standaard lineair regressiemodel gevonden in de Excel-invoegtoepassing. 

We berekend *gemiddelde Absolute % fout* en gebruikt deze als de maateenheid voor prestaties voor het model. Het heeft geduurd 3 maanden tot een werkmodel met Excel aankomen. We brengen van veel van de learning in de Machine Learning Studio-experiment die uiteindelijk zeer nuttig om vereisten.

### <a name="create-comparable-experiment-in-azure-machine-learning"></a>Vergelijkbare experiment maken in Azure Machine Learning
We deze stappen voor het maken van onze experiment in Machine Learning Studio gevolgd: 

1. De gegevensset geüpload als een csv-bestand aan Machine Learning Studio (zeer kleine-bestand)
2. Een nieuw experiment gemaakt en gebruikt de [Select Columns in Dataset] [ select-columns] module die u wilt de functies van dezelfde gegevens in Excel gebruikt selecteren 
3. Gebruikt de [Split Data] [ split] module (met *relatieve expressie* modus) om te verdelen van de gegevens in dezelfde training gegevenssets als in Excel is gebeurd 
4. Geëxperimenteerd met de [lineaire regressie] [ linear-regression] -module (alleen standaardopties), gedocumenteerd en ten opzichte van de resultaten uitgevoerd naar onze regressiemodel Excel

### <a name="review-initial-results"></a>De eerste resultaten bekijken
Eerst de Excel-gegevensmodel duidelijk ver-loop van de Machine Learning Studio-model: 

|  | Excel | Studio |
| --- |:---:|:---:|
| Prestaties | | |
| <ul style="list-style-type: none;"><li>Aangepast R-kwadraat</li></ul> |0.96 |N.v.t. |
| <ul style="list-style-type: none;"><li>Coefficient van <br />Bepaling</li></ul> |N.v.t. |0.78<br />(nauwkeurigheid laag) |
| Mean Absolute Error |$9. 5M |$ 19.4 M |
| Mean Absolute Error (%) |6.03% |12.2% |

Wanneer we onze proces en de resultaten door de ontwikkelaars en gegevenswetenschappers uitgevoerd op de Machine Learning-team, ze snel enkele nuttige tips aangeboden. 

* Wanneer u gebruikt de [lineaire regressie] [ linear-regression] module in Machine Learning Studio twee methoden zijn beschikbaar:
  * Online verloop Daalgradiënt: Is mogelijk beter geschikt voor grootschalige problemen
  * Gewone kleinste kwadraten: Dit is de methode die de meeste mensen zien wanneer ze de lineaire regressie hoort. Voor kleine gegevenssets mag gewone kleinste kwadraten een meer beste keuze.
* Houd rekening met aanpassingen van de parameter L2 regularisatie gewicht om prestaties te verbeteren. Deze standaard is ingesteld op 0,001, maar voor onze kleine gegevensset we deze instellen op 0,005 om prestaties te verbeteren. 

### <a name="mystery-solved"></a>Mysterie opgelost!
Wanneer we de aanbevelingen hebt toegepast, wordt de dezelfde basislijn in Machine Learning Studio als met Excel bereikt: 

|  | Excel | Studio (eerste) | Studio met de kleinste kwadraten |
| --- |:---:|:---:|:---:|
| Gelabelde waarde |Werkelijke waarden (numeriek) |Dezelfde |Dezelfde |
| Cursist |Excel-gegevens > Analysis-regressie > |Lineaire regressie. |Lineaire regressie |
| Opties voor cursist |N.v.t. |Standaardwaarden |gewone kleinste kwadraten<br />L2 0,005 = |
| Gegevensset |26 rijen, 3 onderdelen, 1 label. Alle numerieke. |Dezelfde |Dezelfde |
| Gesplitste: Train |Excel getraind op de eerste 18 rijen zijn getest op de laatste 8 rijen. |Dezelfde |Dezelfde |
| Gesplitste: Test |Excel-regressieformule toegepast op de laatste 8 rijen |Dezelfde |Dezelfde |
| **Prestaties** | | | |
| Aangepast R-kwadraat |0.96 |N.v.t. | |
| Determinatiecoëfficiënt |N.v.t. |0.78 |0.952049 |
| Mean Absolute Error |$9. 5M |$ 19.4 M |$9. 5M |
| Mean Absolute Error (%) |<span style="background-color: 00FF00;"> 6.03%</span> |12.2% |<span style="background-color: 00FF00;"> 6.03%</span> |

De Excel-coëfficiënten vergeleken bovendien ook met het gewicht van de functie in het Azure getraind model:

|  | Excel coëfficiënten | Gewicht van de functie van Azure |
| --- |:---:|:---:|
| Intercept/afwijking |19470209.88 |19328500 |
| Functie A |0.832653063 |0.834156 |
| Functie B |11071967.08 |11007300 |
| Functie C |25383318.09 |25140800 |

## <a name="next-steps"></a>Volgende stappen
We wilden gebruiken voor de Machine Learning-webservice in Excel. Onze bedrijfsanalisten afhankelijk zijn van Excel en er een manier om aanroepen van de Machine Learning-webservice met een rij met Excel-gegevens en laat het retourneren van de voorspelde waarde naar Excel nodig. 

Ook wilden we onze model optimaliseren met behulp van de opties en de algoritmen die beschikbaar zijn in Machine Learning Studio.

### <a name="integration-with-excel"></a>Integratie met Excel
De oplossing is naar onze regressiemodel Machine Learning operationeel door het maken van een webservice van het getrainde model. De webservice is gemaakt binnen een paar minuten en we noemen dat kan rechtstreeks vanuit Excel om de waarde van een voorspelde omzet te retourneren. 

De *Web Services Dashboard* sectie bevat een downloadbare Excel-werkmap. De werkmap wordt vooraf ingestelde geleverd met de web service API en schema-informatie ingesloten. Wanneer u klikt op *Excel-werkmap downloaden*, de werkmap wordt geopend en kunt u deze opslaan op uw lokale computer. 

![][1]

Met de werkmap is geopend, kopieert u de vooraf gedefinieerde parameters in de sectie met blauw Parameter zoals hieronder wordt weergegeven. Zodra de parameters zijn ingevoerd, Excel, illustreert de Machine Learning-webservice en de voorspelde scored labels worden weergegeven in de sectie met groene voorspelde waarden. De werkmap blijven maken van voorspellingen voor parameters die op basis van het getrainde model voor alle rij items die zijn ingevoerd onder Parameters. Zie voor meer informatie over het gebruik van deze functie [gebruiken van een Azure Machine Learning-webservice vanuit Excel](consuming-from-excel.md). 

![][2]

### <a name="optimization-and-further-experiments"></a>Optimalisatie en verdere experimenten
Nu dat we hebben een basislijn met onze Excel-gegevensmodel gehad, verplaatst we verder naar onze Machine Learning-Model voor lineaire regressie optimaliseren. We gebruiken de module [Functieselectie op basis van het Filter] [ filter-based-feature-selection] voor het verbeteren van onze selectie van de initiële gegevens elementen en geholpen bij ons een prestatieverbetering van 4.6% bereiken Absolute Error betekenen. We gebruiken deze functie die kan ons weken opslaan in de gegevenskenmerken de juiste set met functies voor modellering vinden doorloopt voor toekomstige projecten. 

We willen naast extra algoritmen zoals opnemen [Bayesiaans] [ bayesian-linear-regression] of [Boosted Decision Trees] [ boosted-decision-tree-regression] bij ons experiment vergelijken de prestaties. 

Als u experimenteren met regressie wilt, is een goede gegevensset om te proberen de voorbeeldgegevensset energie-efficiëntie regressie die veel numerieke kenmerken heeft. De gegevensset wordt geleverd als onderdeel van de voorbeeldgegevenssets in Machine Learning Studio. Om te voorspellen verwarming Load of Load koeling kunt u tal van learning-modules. De onderstaande tabel is dat een vergelijking van de prestaties van andere regressie leert tegen energie-efficiëntie gegevensset voorspellen voor de doelvariabele koeling laden: 

| Model | Mean Absolute Error | Root Mean Squared fout | Relative Absolute Error | Relatieve kwadraat fout | Determinatiecoëfficiënt |
| --- | --- | --- | --- | --- | --- |
| Gestimuleerd beslissingsstructuur |0.930113 |1.4239 |0.106647 |0.021662 |0.978338 |
| Lineaire regressie (kleurovergang Daalgradiënt) |2.035693 |2.98006 |0.233414 |0.094881 |0.905119 |
| Regressie neurale netwerk |1.548195 |2.114617 |0.177517 |0.047774 |0.952226 |
| Lineaire regressie (gewone kleinste kwadraten) |1.428273 |1.984461 |0.163767 |0.042074 |0.957926 |

## <a name="key-takeaways"></a>Sleutel Takeaways
Hebt u geleerd veel door vanaf de actieve Excel regressie en Azure Machine Learning-experimenten parallel. Maken van de basislijn-model in Excel en vergelijken met modellen met Machine Learning [lineaire regressie] [ linear-regression] ertoe bijgedragen ons informatie over Azure Machine Learning, en wij mogelijkheden voor het verbeteren van de gegevens gedetecteerd selectie en model van de prestaties. 

We ook te vinden dat u wordt aangeraden om te gebruiken [Functieselectie op basis van het Filter] [ filter-based-feature-selection] te versnellen toekomstige voorspelling projecten. Door toe te passen Functieselectie tot uw gegevens, kunt u een verbeterde model in Machine Learning met betere prestaties. 

De mogelijkheid om over te dragen de voorspellende analytische prognose van Machine Learning naar Excel systemically kunnen een aanzienlijke toename in de mogelijkheid is om te geven resultaten een publiek grote bedrijven. 

## <a name="resources"></a>Resources
Hier volgen enkele resources voor het werken met regressie: 

* Regressie in Excel. Als u nooit regressie in Excel hebt geprobeerd, deze zelfstudie gemakkelijker: [http://www.excel-easy.com/examples/regression.html](http://www.excel-easy.com/examples/regression.html)
* Regressie vs prognose. Tyler Chessman geschreven een blog artikel waarin wordt uitgelegd hoe uitvoeren op tijden reeks prognose in Excel, die een goede beginnende beschrijving van de lineaire regressie bevat. [http://sqlmag.com/SQL-Server-Analysis-Services/Understanding-Time-Series-forecasting-Concepts](http://sqlmag.com/sql-server-analysis-services/understanding-time-series-forecasting-concepts) 
* Gewone kleinste kwadraten lineaire regressie: Fouten, problemen en eventuele problemen. Voor een inleiding en discussie over regressie: [http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/](http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/)

[1]: ./media/linear-regression-in-azure/machine-learning-linear-regression-in-azure-1.png
[2]: ./media/linear-regression-in-azure/machine-learning-linear-regression-in-azure-2.png


<!-- Module References -->
[bayesian-linear-regression]: https://msdn.microsoft.com/library/azure/ee12de50-2b34-4145-aec0-23e0485da308/
[boosted-decision-tree-regression]: https://msdn.microsoft.com/library/azure/0207d252-6c41-4c77-84c3-73bdf1ac5960/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/

