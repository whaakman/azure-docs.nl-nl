---
title: Gegevens verzamelen en begrijpen van Team Data Science Process
description: De doelen, taken en producten voor de gegevens verzamelen en begrijpen fase van uw data-science-projecten
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/04/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 77c67c491b3157103b267864417678c83326814c
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53141149"
---
# <a name="data-acquisition-and-understanding-stage-of-the-team-data-science-process"></a>Gegevens verzamelen en begrijpen fase van het Team Data Science Process

In dit artikel bevat een overzicht van de doelen, taken en producten die zijn gekoppeld aan de gegevens verzamelen en begrijpen fase van het Team Data Science Process (TDSP). Deze procedure biedt de levensduur van een aanbevolen die u gebruiken kunt voor het structureren van uw data-science-projecten. De levenscyclus van geeft een overzicht van de belangrijke fasen die projecten doorgaans worden uitgevoerd, vaak iteratief:

   1. **Inzicht in het bedrijf**
   2. **Gegevens verzamelen en begrijpen**
   3. **Modeling**
   4. **Implementatie**
   5. **Aanvaarding van de klant**

Hier volgt een visuele representatie van de TDSP-levenscyclus: 

![TDSP-levenscyclus](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Doelstellingen
* Een schone, hoogwaardige gegevensset waarvan de relatie met de doelvariabelen wordt begrepen produceren. Zoek de gegevensset in de juiste analytics-omgeving, zodat u klaar voor het model bent.
* Ontwikkel een oplossingsarchitectuur van de pijplijn die wordt vernieuwd en worden de gegevens regelmatig beoordeelt.

## <a name="how-to-do-it"></a>Hoe voer ik
Er zijn drie belangrijkste taken in deze fase behandeld:

   * **De gegevens opnemen** in de doelomgeving voor analyse.
   * **Verken de gegevens** om te bepalen of de kwaliteit van de gegevens voldoende zijn om de vraag te beantwoorden. 
   * **Instellen van een pijplijn** gegevens vernieuwd om nieuwe of regelmatig te beoordelen.

### <a name="ingest-the-data"></a>De gegevens opnemen
Instellen van het proces voor de gegevens van de bronlocaties verplaatsen naar de doellocaties waar u analytics-bewerkingen, zoals training en voorspellingen uitgevoerd. Zie voor technische informatie en opties voor het verplaatsen van de gegevens met verschillende Azure-gegevensservices [gegevens laden in opslagomgevingen voor analyses](ingest-data.md). 

### <a name="explore-the-data"></a>De gegevens verkennen
Voordat u uw modellen trainen, moet u voor het ontwikkelen van een goed begrip van de gegevens. Echte gegevenssets zijn vaak ruis, ontbreken waarden of hebben een groot aantal andere verschillen. U kunt overzichten en visualisatie gebruiken om te controleren van de kwaliteit van uw gegevens en informatie die u nodig hebt voor het verwerken van de gegevens voordat deze klaar voor modellen is. Dit proces is vaak iteratieve.

TDSP biedt een geautomatiseerde hulpprogramma, met de naam [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils), om te helpen de gegevens visualiseren en voorbereiden van gegevens beknopte rapporten. Het is raadzaam dat u begint met IDEAR eerst de om gegevens te verkennen om te helpen begrijpen van de initiële gegevens interactief met geen codering ontwikkelen. Vervolgens kunt u aangepaste code voor de gegevens verkennen en visualiseren. Zie voor instructies over het opruimen van de gegevens [taken het voorbereiden van gegevens voor verbeterde machine learning](prepare-data.md).  

Nadat u tevreden met de kwaliteit van de gegevens opgeschoond bent, worden de volgende stap is de patronen die inherent aan de gegevens zijn beter te begrijpen. Zo kunt u kiezen en het ontwikkelen van een juiste Voorspellend model voor het doel. Uiterlijk is voor bewijs van hoe u de gegevens goed verbonden met het doel. Vervolgens kunt u bepalen of er voldoende gegevens om na te gaan met de volgende stappen voor modellering is. Dit proces is, vaak iteratieve. U moet mogelijk nieuwe gegevensbronnen met nauwkeurigere of meer relevante gegevens om te verbeteren van de gegevensset in eerste instantie worden geïdentificeerd in de vorige fase vinden. 

### <a name="set-up-a-data-pipeline"></a>Instellen van een pijplijn
Naast de initiële opname van gegevens en opschonen van de gegevens, moet u meestal voor het instellen van een proces voor het scoren van nieuwe gegevens of de gegevens regelmatig vernieuwt als onderdeel van een doorlopende leerproces. U doen dit door het instellen van een pijplijn of werkstroom. De [gegevens verplaatsen van een on-premises SQL Server-exemplaar naar Azure SQL Database met Azure Data Factory](move-sql-azure-adf.md) artikel geeft een voorbeeld van het instellen van een pijplijn met [Azure Data Factory](https://azure.microsoft.com/services/data-factory/). 

In deze fase moet u een architectuur van de pijplijn-oplossing ontwikkelen. Ontwikkelen van de pijplijn in combinatie met de volgende fase van de data science-project. Afhankelijk van uw bedrijfsbehoeften en de beperkingen van uw bestaande systemen waarin deze oplossing wordt geïntegreerd, kan de pijplijn zijn een van de volgende: 

   * Op basis van batch
   * Streaming of realtime 
   * Een hybride 

## <a name="artifacts"></a>Artefacten
Hier volgen de producten in deze fase:

   * [Kwaliteitsrapport](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/DataSummaryReport.md): dit rapport bevat overzichten van de gegevens, worden de relaties tussen elk kenmerk en het doel, variabele rangorde en nog veel meer. De [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils) hulpprogramma meegeleverd als onderdeel van de TDSP kunt dit rapport op een set van gegevens in tabelvorm, zoals een CSV-bestand of een relationele tabel snel genereren. 
   * **Oplossingsarchitectuur**: de oplossingsarchitectuur kan een diagram of beschrijving van uw gegevens een pijplijn die u gebruikt om uit te voeren scoren of voorspellingen op nieuwe gegevens nadat u een model hebt gemaakt. Het bevat ook de pijplijn opnieuw trainen van uw model op basis van nieuwe gegevens. Store van het document in de [Project](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/Project) directory wanneer u de sjabloon TDSP directory-structuur.
   * **Controlepunt besluit**: voordat u begint met volledige-feature-engineering en modellen bouwen, kunt u het project om te bepalen of de verwachte waarde is voldoende om door te gaan met deze in het kader Evalueer. U kan bijvoorbeeld zijn klaar om door te gaan, moet meer gegevens verzamelen, of het project afbreken als de gegevens niet bestaat als u wilt de vraag worden beantwoord.

## <a name="next-steps"></a>Volgende stappen

Hier vindt u koppelingen naar elke stap in de levenscyclus van de TDSP:

   1. [Inzicht in het bedrijf](lifecycle-business-understanding.md)
   2. [Gegevens verzamelen en begrijpen](lifecycle-data.md)
   3. [Modeling](lifecycle-modeling.md)
   4. [Implementatie](lifecycle-deployment.md)
   5. [Aanvaarding van de klant](lifecycle-acceptance.md)

We bieden volledige end-to-end-scenario's die laten zien van alle de stappen in het proces voor het specifieke scenario's. De [voorbeeld walkthroughs](walkthroughs.md) artikel geeft een lijst van de scenario's met koppelingen en beschrijvingen van miniaturen. De scenario's laten zien hoe u cloud, on-premises hulpprogramma's en services combineren in een werkstroom of een pijplijn te maken van een intelligente toepassingen. 

Zie voor meer voorbeelden van hoe u het uitvoeren van de stappen in TDSPs die gebruikmaken van Azure Machine Learning Studio [de TDSP gebruiken met Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/lifecycle-data).
