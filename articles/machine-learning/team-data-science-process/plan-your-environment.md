---
title: Scenario's identificeren en te plannen van uw proces analytics - Azure | Microsoft Docs
description: Abonnement voor geavanceerde analyses op basis van een reeks van belangrijke vragen.
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 421520dd-7728-4d29-889c-ebe6a0a6fb07
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: bradsev
ms.openlocfilehash: f4cf702b899b285b18c09d7a5951589d2ae71b7d
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2017
---
# <a name="how-to-identify-scenarios-and-plan-for-advanced-analytics-data-processing"></a>Scenario's en plannen identificeren voor geavanceerde analytische gegevensverwerking
Welke bronnen moet u van plan bent om op te nemen bij het instellen van een omgeving geavanceerde analyses verwerken van een gegevensset doen? Dit artikel bevat een reeks te stellen vragen die helpen identificeren van de taken en bronnen die relevant zijn uw scenario. De volgorde van stappen op hoog niveau voor predictive analytics wordt beschreven in [wat is er Team gegevens wetenschap proces (TDSP)?](overview.md). Elk van deze stappen vereist specifieke resources voor de taken die relevant zijn voor uw specifieke scenario. De belangrijke vragen om te bepalen van uw scenario betrekking hebben op gegevens logistiek, kenmerken, de kwaliteit van de gegevenssets en de hulpprogramma's en talen die u liever de analyse.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="logistic-questions-data-locations-and-movement"></a>Logistic vragen: gegevenslocaties en verkeer
De logistic vragen hebben betrekking op de locatie van de **gegevensbron**, wordt de **doellocatie** in Azure, en de vereisten voor het verplaatsen van de gegevens, inclusief de planning, het bedrag en de resources die zijn betrokken. De gegevens wellicht meerdere keren worden verplaatst tijdens de analyse. Een gebruikelijk scenario is het lokale gegevens verplaatsen naar een vorm van opslag in Azure en vervolgens naar Machine Learning Studio.

1. **Wat is uw gegevensbron?** Is het lokaal of in de cloud? Bijvoorbeeld:
   
   * De gegevens zijn openbaar beschikbaar is op een HTTP-adres.
   * De gegevens zich op een LAN/bestandslocatie.
   * De gegevens zich in een SQL Server-database.
   * De gegevens worden opgeslagen in een Azure storage-container
2. **Wat is de bestemming Azure?** Waar deze moet voor verwerking of modelleren? Bijvoorbeeld:
   
   * Azure Blob Storage
   * Azure SQL-databases
   * SQL Server op virtuele Azure-machine
   * HDInsight (Hadoop op Azure) of Hive-tabellen
   * Azure Machine Learning
   * Koppelbaar Azure virtuele harde schijven.
3. **Hoe gaat u de gegevens worden verplaatst** De procedures en resources beschikbaar voor het opnemen of gegevens laden in tal van andere opslag en verwerking van omgevingen worden beschreven in de volgende artikelen:
   
   * [Gegevens laden in omgevingen met opslag voor analyses](ingest-data.md)
   * [Uw trainingsgegevens importeren in Azure Machine Learning Studio van verschillende gegevensbronnen](../studio/import-data.md).
4. **Heeft de gegevens worden regelmatig verplaatst of gewijzigd tijdens de migratie nodig?** Overweeg het gebruik van Azure Data Factory (ADF) als de gegevens moeten voortdurend worden gemigreerd, met name als een hybride scenario die toegang heeft tot zowel on-premises en cloudresources betrokken is, of de gegevens is transactionele of moet worden gewijzigd of toegevoegd aan uw bedrijfslogica hebben het is in de loop van wordt gemigreerd. Zie voor meer informatie [verplaatsen van gegevens van een lokale SQL server naar SQL Azure met Azure Data Factory](move-sql-azure-adf.md)
5. **Hoeveel van de gegevens wordt verplaatst naar Azure?** Extreem grote gegevenssets overschrijdt misschien de opslagcapaciteit van bepaalde omgevingen. Zie het onderwerp over maximale grootte voor Machine Learning Studio in de volgende sectie voor een voorbeeld. In dergelijke gevallen kan een steekproef van de gegevens worden gebruikt tijdens de analyse. Zie voor gedetailleerde informatie over down-sample gegevensset in verschillende Azure-omgevingen, [voorbeeldgegevens in het Team gegevens wetenschap proces](sample-data.md).

## <a name="data-characteristics-questions-type-format-and-size"></a>Vragen over de kenmerken van gegevens: type, indeling en grootte
Deze vragen zijn de sleutel tot het plannen van uw opslag en omgevingen, die geschikt zijn voor verschillende soorten gegevens en elk met bepaalde beperkingen hebben verwerken.

1. **Wat zijn de gegevenstypen?** Bijvoorbeeld:
   
   * Numerieke
   * Categorische gegevens
   * Tekenreeksen
   * Binair bestand
2. **Hoe wordt uw gegevens opgemaakt?** Bijvoorbeeld:
   
   * Door komma's gescheiden (CSV) of door tabs gescheiden (TSV) platte bestanden
   * Wel of niet gecomprimeerd
   * Azure blobs
   * Hadoop Hive-tabellen
   * SQL Server-tabellen
3. **Hoe groot is uw gegevens?**
   
   * Kleine: minder dan 2 GB
   * Gemiddeld: Groter is dan 2 GB en minder dan 10 GB
   * Grote: Groter zijn dan 10 GB

Neem bijvoorbeeld de Azure Machine Learning Studio-omgeving:

* Zie voor een lijst van de gegevensindelingen en die worden ondersteund door Azure Machine Learning Studio [gegevensindelingen en de ondersteunde gegevenstypen](../studio/import-data.md#data-formats-and-data-types-supported) sectie.
* Zie voor informatie over de beperkingen van de gegevens voor Azure Machine Learning Studio, de **hoe groot mag de gegevensset zijn voor mijn modules?** sectie van [importeren en exporteren van gegevens voor Machine Learning](../studio/faq.md#machine-learning-studio-questions)

Zie voor informatie over de beperkingen van andere Azure-services gebruikt tijdens de analytics, [Azure-abonnement en Service-limieten, quota's en beperkingen](../../azure-subscription-service-limits.md).

## <a name="data-quality-questions-exploration-and-pre-processing"></a>Vragen over de kwaliteit van de gegevens: exploratie en vooraf verwerken
1. **Wat u weten over uw gegevens** Verken de gegevens voor het verkrijgen van een begrijpen van de basiskenmerken. Welke patronen of trends deze vertoont, ontbreken welke uitschieters heeft of hoeveel waarden. Deze stap is belangrijk voor het bepalen van de mate van vooraf verwerken die nodig zijn voor formuleren hypothesen die kunnen de meest geschikte functies aanbevelen of typ van analyse en voor het plannen voor het verzamelen van aanvullende gegevens te formuleren. Beschrijvende statistiek berekenen en uitzetten van visualisaties zijn nuttige technieken voor inspectie van gegevens. Zie voor meer informatie over het verkennen van een gegevensset in verschillende Azure-omgevingen [gegevens in het Team gegevens wetenschap proces](explore-data.md).
2. **Vereist de gegevens vooraf verwerken of reinigen?**
   Vooraf verwerken en het opruimen van gegevens zijn belangrijke taken die normaal gesproken plaatsvinden moeten voordat gegevensset effectief kan worden gebruikt voor machine learning. Onbewerkte gegevens is vaak veel ruis veroorzaken en onbetrouwbare mogelijk ontbreken waarden. Met deze gegevens voor modellering kan misleidende resultaten opleveren. Zie voor een beschrijving [taken voor het voorbereiden van gegevens voor verbeterde machine learning](prepare-data.md).

## <a name="tools-and-languages-questions"></a>Hulpprogramma's en talen vragen
Er zijn veel opties, afhankelijk van welke talen en ontwikkelomgevingen of hulpprogramma's of u kunt met behulp van meest conformable zijn.

1. **Welke talen wilt u gebruiken voor analyse?**  
   
   * R
   * Python
   * SQL
2. **Welke hulpprogramma's moet u voor data-analyse gebruiken?**
   
   * [Microsoft Azure Powershell](/powershell/azure/overview) -een scripttaal die is gebruikt voor het beheren van uw Azure-resources in een scripttaal.
   * [Azure Machine Learning Studio](../studio/what-is-ml-studio.md)
   * [Revolution Analytics](http://www.revolutionanalytics.com/revolution-r-open)
   * [RStudio](http://www.rstudio.com)
   * [Python Tools for Visual Studio](http://microsoft.github.io/PTVS/)
   * [Anaconda](https://www.continuum.io/why-anaconda)
   * [Jupyter-notebooks](http://jupyter.org/)
   * [Microsoft Power BI](http://powerbi.microsoft.com)

## <a name="identify-your-advanced-analytics-scenario"></a>Identificeren van uw scenario geavanceerde analyses
Zodra u de vragen in de vorige sectie hebt beantwoord, bent u klaar om te bepalen welke scenario best past bij uw aanvraag. De voorbeeldscenario's worden beschreven in [scenario's voor geavanceerde analyses in Azure Machine Learning](plan-sample-scenarios.md).

