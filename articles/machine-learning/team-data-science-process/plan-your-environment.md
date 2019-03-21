---
title: Scenario's identificeren en te plannen van de analytics-proces - Team Data Science Process | Azure Machine Learning
description: Identificeer scenario's en plan voor geavanceerde analytische gegevensverwerking op basis van een reeks van belangrijke vragen.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/13/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 0eaa5f24be85eab96f11e0c3915a3f60d79a0551
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57864742"
---
# <a name="how-to-identify-scenarios-and-plan-for-advanced-analytics-data-processing"></a>Scenario's en plannen identificeren voor geavanceerde analytische gegevensverwerking

Welke resources zijn vereist om het maken van een omgeving die u kunt uitvoeren, geavanceerde analyseverwerking op een gegevensset? Dit artikel bevat een reeks vragen stelt om te vragen waarmee u kunt taken en resources relevante uw scenario identificeren.

Zie voor meer informatie over de volgorde van stappen op hoog niveau voor voorspellende analyse, [wat is Team Data Science Process (TDSP)](overview.md). Elke stap vereist specifieke resources voor de taken die relevant zijn voor uw specifieke scenario.

Antwoorden op belangrijke vragen in de volgende gebieden te identificeren van uw scenario:

* gegevenslogistiek
* gegevenskenmerk
* gegevensset kwaliteit
* favoriete hulpprogramma's en talen

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="logistic-questions-data-locations-and-movement"></a>Logistieke vragen: gegevenslocaties en -verplaatsing

De logistieke vragen hebben betrekking op de volgende items:

* locatie van gegevensbron
* doellocatie in Azure
* vereisten voor het verplaatsen van gegevens, inclusief de planning, het bedrag en de betrokken resources

Mogelijk moet u de gegevens meerdere keren verplaatsen tijdens de analyse. Een gebruikelijk scenario is het lokale gegevens verplaatsen naar een vorm van opslag van Azure en vervolgens in Machine Learning Studio.

### <a name="what-is-your-data-source"></a>Wat is uw gegevensbron?

Zijn uw gegevens lokaal of in de cloud? Mogelijke locaties zijn onder andere:

* een openbaar beschikbare HTTP-adres
* een lokale of netwerklocatie bestand
* een SQL Server-database
* een Azure storage-container

### <a name="what-is-the-azure-destination"></a>Wat is de Azure-doelhost?

Waar heeft uw gegevens nodig zijn voor verwerking of model maken? 

* Azure Blob Storage
* SQL Azure-databases
* SQL Server op virtuele Azure-machine
* HDInsight (Hadoop op Azure) of Hive-tabellen
* Azure Machine Learning
* Koppelbaar Azure virtuele harde schijven

### <a name="how-are-you-going-to-move-the-data"></a>Hoe gaat u om de gegevens te verplaatsen?

Voor procedures en bronnen naar opnemen of gegevens laden in tal van verschillende opslag en verwerking van omgevingen, Zie:

* [Gegevens laden in opslagomgevingen voor analyses](ingest-data.md)
* [Uw trainingsgegevens importeren in Azure Machine Learning Studio van verschillende gegevensbronnen](../studio/import-data.md)

### <a name="does-the-data-need-to-be-moved-on-a-regular-schedule-or-modified-during-migration"></a>Hoeft de gegevens worden regelmatig verplaatst of gewijzigd tijdens de migratie?

Overweeg het gebruik van Azure Data Factory (ADF) wanneer gegevens moeten voortdurend worden gemigreerd. ADF kan nuttig zijn voor:

* een hybride scenario dat betrekking heeft op zowel on-premises en cloudbronnen
* een scenario waarin de gegevens wordt afgehandeld, gewijzigd of gewijzigd door zakelijke logica doorgeeft die wordt gemigreerd

Zie voor meer informatie, [gegevens verplaatsen van een on-premises SQL server naar SQL Azure met Azure Data Factory](move-sql-azure-adf.md).

### <a name="how-much-of-the-data-is-to-be-moved-to-azure"></a>Hoeveel van de gegevens is om te worden verplaatst naar Azure?

Zeer grote gegevenssets kan groter zijn dan de opslagcapaciteit van bepaalde omgevingen. Zie de beschrijving van de maximale grootte voor Machine Learning Studio in de volgende sectie voor een voorbeeld. In dergelijke gevallen kunt u een voorbeeld van de gegevens tijdens de analyse. Zie voor meer informatie over hoe u een gegevensset in verschillende Azure-omgevingen down-sampling, [Sample van gegevens in het Team Data Science Process](sample-data.md).

## <a name="data-characteristics-questions-type-format-and-size"></a>Vragen over de kenmerken van gegevens: type, de indeling en grootte

Deze vragen zijn essentieel voor plannen van uw opslag en verwerking van omgevingen. Ze helpt u bij het kiezen van het scenario voor het gegevenstype en eventuele beperkingen begrijpt.

### <a name="what-are-the-data-types"></a>Wat zijn de gegevenstypen?

* Numerieke
* Categorische gegevens
* Tekenreeksen
* Binair bestand

### <a name="how-is-your-data-formatted"></a>Hoe wordt de gegevens opgemaakt?

* Door komma's gescheiden (CSV) of platte bestanden (TSV) door tabs gescheiden
* Wel of niet gecomprimeerd
* Azure-blobs
* Hadoop Hive-tabellen
* SQL Server-tabellen

### <a name="how-large-is-your-data"></a>Hoe groot zijn uw gegevens?

* Kleine: Minder dan 2 GB
* Normaal: Groter dan 2 GB en kleiner dan 10 GB
* Grote: Groter dan 10 GB

Neem bijvoorbeeld de Azure Machine Learning Studio-omgeving:

* Zie voor een lijst van de opmaak van gegevens en die worden ondersteund door Azure Machine Learning Studio, [opmaak van gegevens en gegevens die worden ondersteund](../studio/import-data.md#supported-data-formats-and-data-types) sectie.
* Zie voor informatie over de beperkingen van andere Azure-services die worden gebruikt in de analytics-proces, [Azure-abonnement en Servicelimieten, Quotums en beperkingen](../../azure-subscription-service-limits.md).

## <a name="data-quality-questions-exploration-and-pre-processing"></a>Vragen over de kwaliteit van gegevens: verkennen en de voorverwerking

### <a name="what-do-you-know-about-your-data"></a>Wat weet u over uw gegevens?

Inzicht in de basiskenmerken over uw gegevens:

* Wat u patronen of trends deze vertoont
* Wat heeft uitbijters
* Hoeveel waarden ontbreken

Deze stap is het belangrijk om u te helpen:

* Bepalen hoeveel vóór verwerking is vereist
* Formuleren hypothesen dat suggesties van de meest geschikte functies of het type van de analyse
* Plannen voor het verzamelen van aanvullende gegevens formuleren

Handige technieken voor inspectie van de gegevens zijn onder andere beschrijvende statistiek berekenings- en visualisatie worden uitgezet. Zie voor meer informatie over het verkennen van een gegevensset in verschillende Azure-omgevingen [gegevens verkennen in Team Data Science Process](explore-data.md).

### <a name="does-the-data-require-preprocessing-or-cleaning"></a>Vereist de gegevens voorverwerken of opschonen?

Mogelijk moet u voorverwerken en opschonen van uw gegevens voordat u de gegevensset effectief voor machine learning gebruiken kunt. Onbewerkte gegevens zijn vaak veel ruis veroorzaken en onbetrouwbaar. Er ontbreken waarden. Met behulp van dergelijke gegevens voor modellen kan misleidend resultaten opleveren. Zie voor een beschrijving [taken het voorbereiden van gegevens voor verbeterde machine learning](prepare-data.md).

## <a name="tools-and-languages-questions"></a>Vragen over hulpprogramma's en talen

Er zijn veel verschillende talen, ontwikkelomgevingen en hulpprogramma's. Houd rekening met uw behoeften en voorkeuren.

### <a name="what-languages-do-you-prefer-to-use-for-analysis"></a>Welke talen wilt u gebruiken voor analyse?

* R
* Python
* SQL

### <a name="what-tools-should-you-use-for-data-analysis"></a>Welke hulpprogramma's moet u voor data-analyse gebruiken?

* [Microsoft Azure Powershell](/powershell/azure/overview) -een scripttaal die is gebruikt voor het beheren van uw Azure-resources in een scripttaal
* [Azure Machine Learning Studio](../studio/what-is-ml-studio.md)
* [Revolution Analytics af](https://www.microsoft.com/sql-server/machinelearningserver)
* [RStudio](http://www.rstudio.com)
* [Python Tools for Visual Studio](https://aka.ms/ptvsdocs)
* [Anaconda](https://www.continuum.io/why-anaconda)
* [Jupyter-notebooks](https://jupyter.org/)
* [Microsoft Power BI](https://powerbi.microsoft.com)

## <a name="identify-your-advanced-analytics-scenario"></a>Identificeren van uw scenario voor geavanceerde analyses

Nadat u de vragen in de vorige sectie hebt beantwoord, bent u klaar om te bepalen welke aanbevolen scenario past bij uw situatie. De voorbeeldscenario's worden beschreven in [scenario's voor geavanceerde analyses in Azure Machine Learning](plan-sample-scenarios.md).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Wat is Team Data Science Process (TDSP)?](overview.md)