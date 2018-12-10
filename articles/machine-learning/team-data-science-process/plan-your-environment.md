---
title: Scenario's identificeren en te plannen van de analytics-proces - Team Data Science Process
description: Identificeer scenario's en plan voor geavanceerde analytische gegevensverwerking op basis van een reeks van belangrijke vragen.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/13/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 5faa7a58a252a5d3b8cc044f9e81a6d7cb2df7d5
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53138038"
---
# <a name="how-to-identify-scenarios-and-plan-for-advanced-analytics-data-processing"></a>Scenario's en plannen identificeren voor geavanceerde analytische gegevensverwerking
Welke resources moet u van plan bent om op te nemen bij het instellen van een omgeving voor het geavanceerde analyseren verwerking van een gegevensset? Dit artikel bevat een reeks vragen die de taken en resources relevante uw scenario identificeren. De volgorde van stappen op hoog niveau voor voorspellende analyses die worden beschreven in [wat is Team Data Science Process (TDSP)?](overview.md). Elk van deze stappen vereist specifieke resources voor de taken die relevant zijn voor uw specifieke scenario. De belangrijkste vragen om te bepalen van uw scenario heeft betrekking op gegevenslogistiek, kenmerken, de kwaliteit van de gegevenssets en de hulpprogramma's en talen die u liever de analyse.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="logistic-questions-data-locations-and-movement"></a>Logistieke vragen: gegevenslocaties en -verplaatsing
De logistieke vragen hebben betrekking op de locatie van de **gegevensbron**, wordt de **doellocatie** in Azure, en vereisten voor het verplaatsen van de gegevens, met inbegrip van de planning, bedrag, en resources die betrokken zijn. De gegevens mogelijk meerdere keren tijdens de analyse worden verplaatst. Een gebruikelijk scenario is het lokale gegevens verplaatsen naar een vorm van opslag van Azure en vervolgens in Machine Learning Studio.

1. **Wat is uw gegevensbron?** Is het lokaal of in de cloud? Bijvoorbeeld:
   
   * De gegevens is openbaar beschikbaar is op een HTTP-adres.
   * De gegevens zich bevinden in een locatie voor het lokale netwerk.
   * De gegevens zich in een SQL Server-database.
   * De gegevens worden opgeslagen in een Azure storage-container
2. **Wat is de Azure-doelhost?** Waar heeft het nodig zijn voor verwerking of modelleren? Bijvoorbeeld:
   
   * Azure Blob Storage
   * SQL Azure-databases
   * SQL Server op virtuele Azure-machine
   * HDInsight (Hadoop op Azure) of Hive-tabellen
   * Azure Machine Learning
   * Koppelbaar Azure virtuele harde schijven.
3. **Hoe gaat u om de gegevens te verplaatsen?** De procedures en informatiebronnen voor opnemen of gegevens laden in tal van verschillende opslag en verwerking van omgevingen worden beschreven in de volgende artikelen:
   
   * [Gegevens laden in opslagomgevingen voor analyses](ingest-data.md)
   * [Uw trainingsgegevens importeren in Azure Machine Learning Studio van verschillende gegevensbronnen](../studio/import-data.md).
4. **Hoeft de gegevens worden regelmatig verplaatst of gewijzigd tijdens de migratie?** Overweeg het gebruik van Azure Data Factory (ADF) wanneer gegevens voortdurend worden gemigreerd moeten, met name als een hybride scenario dat toegang heeft tot zowel on-premises en cloudresources is betrokken, of wanneer de gegevens wordt afgehandeld of moet worden gewijzigd of bedrijfslogica toegevoegd aan deze doorgeeft die wordt gemigreerd. Zie voor meer informatie, [gegevens verplaatsen van een on-premises SQL server naar SQL Azure met Azure Data Factory](move-sql-azure-adf.md)
5. **Hoeveel van de gegevens is om te worden verplaatst naar Azure?** Zeer grote gegevenssets kan groter zijn dan de opslagcapaciteit van bepaalde omgevingen. Zie de beschrijving van de maximale grootte voor Machine Learning Studio in de volgende sectie voor een voorbeeld. In dergelijke gevallen kan een voorbeeld van de gegevens worden gebruikt tijdens de analyse. Zie voor meer informatie over hoe u een gegevensset in verschillende Azure-omgevingen down-sampling, [Sample van gegevens in het Team Data Science Process](sample-data.md).

## <a name="data-characteristics-questions-type-format-and-size"></a>Vragen over de kenmerken van gegevens: type, de indeling en grootte
Deze vragen zijn essentieel voor plannen van uw opslag en verwerking van omgevingen, die elk zijn ook geschikt voor verschillende typen gegevens en die elk bepaalde beperkingen hebben.

1. **Wat zijn de gegevenstypen?** Bijvoorbeeld:
   
   * Numerieke
   * Categorische gegevens
   * Tekenreeksen
   * Binair bestand
2. **Hoe wordt de gegevens opgemaakt?** Bijvoorbeeld:
   
   * Door komma's gescheiden (CSV) of platte bestanden (TSV) door tabs gescheiden
   * Wel of niet gecomprimeerd
   * Azure-blobs
   * Hadoop Hive-tabellen
   * SQL Server-tabellen
3. **Hoe groot zijn uw gegevens?**
   
   * Kleine: minder dan 2 GB
   * Gemiddeld: Groter is dan 2 GB en kleiner dan 10 GB
   * Grote: Groter is dan 10 GB

Neem bijvoorbeeld de Azure Machine Learning Studio-omgeving:

* Zie voor een lijst van de opmaak van gegevens en die worden ondersteund door Azure Machine Learning Studio, [opmaak van gegevens en gegevens die worden ondersteund](../studio/import-data.md#data-formats-and-data-types-supported) sectie.
* Zie voor informatie over de beperkingen van de gegevens voor Azure Machine Learning Studio, de **hoe groot mag de gegevensset zijn voor mijn modules?** sectie van [importeren en exporteren van gegevens voor Machine Learning](../studio/faq.md#machine-learning-studio-questions)

Zie voor informatie over de beperkingen van andere Azure-services die worden gebruikt in de analytics-proces, [Azure-abonnement en Servicelimieten, Quotums en beperkingen](../../azure-subscription-service-limits.md).

## <a name="data-quality-questions-exploration-and-pre-processing"></a>Vragen over de kwaliteit van gegevens: verkennen en de voorverwerking
1. **Wat weet u over uw gegevens?** Verken de gegevens te krijgen van een begrijpen van de algemene kenmerken ervan. Welke patronen of trends deze vertoont, ontbreken wat heeft uitbijters of het aantal waarden. Deze stap is belangrijk voor het bepalen van de omvang van het vooraf verwerken die nodig zijn voor het formuleren van hypothesen die het meest geschikte functies voorstellen of typ van analyse, en voor het opstellen van plannen voor het verzamelen van aanvullende gegevens. Beschrijvende statistieken te berekenen en uitzetten visualisaties zijn nuttige technieken voor inspectie van gegevens. Zie voor meer informatie over het verkennen van een gegevensset in verschillende Azure-omgevingen [gegevens verkennen in Team Data Science Process](explore-data.md).
2. **Vereist de gegevens vooraf verwerken of opschonen?**
   Vooraf verwerken en opschonen van gegevens zijn belangrijke taken die normaal gesproken moeten worden uitgevoerd voordat de gegevensset effectief kan worden gebruikt voor machine learning. Onbewerkte gegevens is vaak veel ruis veroorzaken en onbetrouwbare en mogelijk ontbreken waarden. Met behulp van dergelijke gegevens voor modellen kan misleidend resultaten opleveren. Zie voor een beschrijving [taken het voorbereiden van gegevens voor verbeterde machine learning](prepare-data.md).

## <a name="tools-and-languages-questions"></a>Vragen over hulpprogramma's en talen
Er zijn talloze opties die hier zijn afhankelijk van welke talen en ontwikkelomgevingen of hulpprogramma's u nodig hebt of meest conformable gebruikt.

1. **Welke talen wilt u gebruiken voor analyse?**  
   
   * R
   * Python
   * SQL
2. **Welke hulpprogramma's moet u voor data-analyse gebruiken?**
   
   * [Microsoft Azure Powershell](/powershell/azure/overview) -een scripttaal die is gebruikt voor het beheren van uw Azure-resources in een scripttaal.
   * [Azure Machine Learning Studio](../studio/what-is-ml-studio.md)
   * [Revolution Analytics af](https://www.microsoft.com/sql-server/machinelearningserver)
   * [RStudio](http://www.rstudio.com)
   * [Python Tools for Visual Studio](https://aka.ms/ptvsdocs)
   * [Anaconda](https://www.continuum.io/why-anaconda)
   * [Jupyter-notebooks](http://jupyter.org/)
   * [Microsoft Power BI](https://powerbi.microsoft.com)

## <a name="identify-your-advanced-analytics-scenario"></a>Identificeren van uw scenario voor geavanceerde analyses
Als u de vragen in de vorige sectie hebt beantwoord, bent u klaar om te bepalen welke aanbevolen scenario past bij uw situatie. De voorbeeldscenario's worden beschreven in [scenario's voor geavanceerde analyses in Azure Machine Learning](plan-sample-scenarios.md).

