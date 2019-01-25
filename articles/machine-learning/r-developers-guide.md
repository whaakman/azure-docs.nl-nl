---
title: R developer's guide voor Azure - programmeren R | Microsoft Docs
description: Dit artikel bevat een overzicht van de verschillende manieren om dat gegevenswetenschappers kunnen gebruikmaken van hun bestaande vaardigheden met het R programmeertaal in Azure. Azure biedt veel services die ontwikkelaars R gebruiken kunnen om uit te breiden hun data science-werkbelastingen in de cloud.
services: machine-learning
documentationcenter: ''
author: AnalyticJeremy
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: R
ms.topic: article
ms.date: 09/12/2018
ms.author: jepeach
ms.openlocfilehash: 102191b885d2a4a9234b7783b0a51b09903d3abd
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54807453"
---
# <a name="r-developers-guide-to-azure"></a>R developer's guide voor Azure
<img src="media/r-developers-guide/logo_r.svg" alt="R logo" align="right" width="200" />

Veel gegevensanalisten omgaan met steeds grotere hoeveelheden gegevens zijn op zoek naar manieren om de kracht van cloudcomputing voor hun analyses.  In dit artikel biedt een overzicht van de verschillende manieren waarop u gegevenswetenschappers kunnen gebruikmaken van hun bestaande vaardigheden met de [programmeertaal R](https://www.r-project.org) in Azure.

Microsoft heeft de programmeertaal R als een eersteklas hulpprogramma volledig voor data scientists omarmd.  Door op te geven veel verschillende opties voor R-ontwikkelaars kunnen hun code uitvoeren in Azure, is het inschakelen van het bedrijf gegevenswetenschappers om uit te breiden hun data science-werkbelastingen in de cloud bij het toepassen van grootschalige projecten.

We gaan de verschillende opties en de meest waardevolle scenario's voor elk criterium.

## <a name="azure-services-with-r-language-support"></a>Azure-services met ondersteuning voor R-taal
In dit artikel bevat informatie over de volgende Azure-services die ondersteuning bieden voor de R-taal:

|Service                                                          |Description                                                                       |
|-----------------------------------------------------------------|----------------------------------------------------------------------------------|
|[Virtuele Machine voor Datatechnologie](#data-science-virtual-machine)    |een aangepaste virtuele machine gebruiken als een data science-werkstation of als een aangepaste compute-doel|
|[ML-Services op HDInsight](#ml-services-on-hdinsight)            |cluster-systeem voor het uitvoeren van R-analyses op grote gegevenssets over veel knooppunten   |
|[Azure Databricks](#azure-databricks)                            |Spark samenwerkingsomgeving die ondersteuning biedt voor R en andere talen               |
|[Azure Machine Learning Studio](#azure-machine-learning-studio)  |aangepaste R-scripts uitvoeren in Azure machine learning-experimenten                      |
|[Azure Batch](#azure-batch)                                      |biedt verschillende opties voor het uitvoeren van R-code economisch over veel knooppunten in een cluster|
|[Azure Notebooks](#azure-notebooks)                              |een gratis cloudgebaseerde versie van Jupyter-notebooks                  |
|[Azure SQL Database](#azure-sql-database)                        |R-scripts in de database-engine van SQL Server uitvoeren                            |

## <a name="data-science-virtual-machine"></a>Data Science Virtual Machine
De [Data Science Virtual Machine](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview) (DSVM) is een aangepaste VM-installatiekopie op van Microsoft Azure-cloudplatform speciaal gebouwd voor gegevenswetenschap. Het heeft veel populaire hulpprogramma, met inbegrip van:
* [Microsoft R Open](https://mran.microsoft.com/open/)
* [Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server)
* [RStudio Desktop](https://www.rstudio.com/products/rstudio/#Desktop)
* [RStudio Server](https://www.rstudio.com/products/rstudio/#Server)

De DSVM worden ingericht met Windows of Linux als het besturingssysteem.  U kunt de DSVM op twee verschillende manieren gebruiken: als een interactieve werkstation of als een computerplatform voor een aangepaste-cluster.

### <a name="as-a-workstation"></a>Als een werkstation
Als u aan de slag met R in de cloud wilt, snel en eenvoudig, is dit het beste.  De omgeving niet bekend bij iedereen die heeft samengewerkt met R op een lokale werkstation.  Echter, in plaats van lokale bronnen, de R-omgeving wordt uitgevoerd op een virtuele machine in de cloud.  Als uw gegevens al in Azure opgeslagen worden, heeft dit het voordeel van het toestaan van uw R-scripts om uit te voeren "dichter bij de gegevens." In plaats van als de gegevens worden overgebracht via Internet, zijn de gegevens toegankelijk via de interne netwerk van Azure, waarmee u veel sneller toegangstijden.

De DSVM kan zijn met name handig voor kleine teams van R-ontwikkelaars.  Elke ontwikkelaar kunt in plaats van te investeren in krachtige werkstations voor elke ontwikkelaar en vereisen teamleden om te synchroniseren op welke versies van de verschillende softwarepakketten die ze gebruiken, automatisch een exemplaar van de DSVM wanneer dat nodig is.

### <a name="as-a-compute-platform"></a>Als een computerplatform
Naast het wordt gebruikt als een werkstation, wordt de DSVM ook gebruikt als een elastisch schaalbare computerplatform voor R-projecten.  Met behulp van de <code>[AzureDSVM](https://github.com/Azure/AzureDSVM)</code> R-pakket, kunt u via een programma bepalen het maken en verwijderen van exemplaren van de DSVM.  U kunt de exemplaren in een cluster vormen en implementeren van een gedistribueerde analyses worden uitgevoerd in de cloud.  Dit hele proces kan worden beheerd door de R-code die wordt uitgevoerd op uw lokale werkstation.

Voor meer informatie over de DSVM, raadpleegt u de ["Inleiding tot Azure Data Science Virtual Machine voor Linux en Windows."](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview)

## <a name="ml-services-on-hdinsight"></a>ML Services in HDInsight
[Microsoft ML-Services](https://docs.microsoft.com/azure/hdinsight/r-server/r-server-overview) bieden gegevenswetenschappers, statistici en R-programmeurs on-demandtoegang tot schaalbare, gedistribueerde analysemethoden in HDInsight.  Deze oplossing biedt de nieuwste mogelijkheden voor analyses op basis van R op gegevenssets van vrijwel elke grootte, geladen in Azure Blob of Data Lake-opslag.

Dit is een geavanceerde oplossing waarmee u uw R-code in een cluster schalen.  Door gebruik te maken van de functies van Microsoft
<code>[RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler)</code> pakket, uw R-scripts op HDInsight kunnen uitvoeren functies voor het verwerken van gegevens parallel op veel knooppunten in een cluster.  Hierdoor kan R benut gegevens op een veel grotere schaal dan mogelijk is met één thread R die worden uitgevoerd op een werkstation.

Deze mogelijkheid om te schalen is ML-Services op HDInsight een goede optie voor R-ontwikkelaars met enorme gegevenssets.  Het biedt een flexibele en schaalbare platform voor het uitvoeren van uw R-scripts in de cloud.

Zie voor stapsgewijze instructies over het maken van een ML-Services-cluster, de ['Aan de slag met ML-Services op Azure HDInsight'](https://docs.microsoft.com/azure/hdinsight/r-server/r-server-get-started) artikel.

## <a name="azure-databricks"></a>Azure Databricks
[Azure Databricks](https://azure.microsoft.com/services/databricks/) is een op Apache Spark gebaseerd analyseplatform, geoptimaliseerd voor het Microsoft Azure-platform voor cloudservices.  Databricks is ontwikkeld in samenwerking met de grondleggers van Apache Spark en met Azure geïntegreerd. Hierdoor biedt het installatie met één klik, gestroomlijnde werkstromen en een interactieve werkruimte waarmee gegevenswetenschappers, gegevenstechnici en bedrijfsanalisten samen kunnen werken.

De samenwerking in Databricks is notebook-systeem van het platform ingeschakeld.  Gebruikers kunnen maken, delen en laptops met andere gebruikers van de systemen te bewerken.  Deze laptops toestaan dat gebruikers code te schrijven die op Spark-clusters worden beheerd in de Databricks-omgeving wordt uitgevoerd.  Deze laptops volledig ondersteuning voor R en geeft gebruikers toegang tot Spark via zowel de `SparkR` en `sparklyr` pakketten.

Omdat Databricks is gebaseerd op Spark en de nadruk op samenwerking ligt, wordt het platform wordt vaak gebruikt door teams van gegevenswetenschappers die in complexe analyses van grote gegevenssets samenwerken.  Omdat de notebooks in Databricks bieden ondersteuning voor andere talen naast R, is het vooral handig voor teams waar analisten verschillende talen gebruiken voor hun primaire werk.

Het artikel ['Wat is Azure Databricks?'](https://docs.microsoft.com/azure/azure-databricks/what-is-azure-databricks)
krijgt u meer informatie over het platform en de hulp die u aan de slag.

## <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio
[Azure Machine Learning Studio](https://azure.microsoft.com/services/machine-learning-studio/) is een samenwerkingsverband slepen en neerzetten waarmee u gebruiken kunt om te bouwen, testen en implementeren van voorspellende analyseoplossingen in de cloud.  Hiermee kunt opkomende gegevenswetenschappers maken en implementeren van machine learning-modellen zonder de noodzaak om veel code te schrijven.

ML Studio biedt ondersteuning voor R en Python.  U kunt R met ML Studio op twee manieren gebruiken.

### <a name="custom-r-scripts-in-your-experiments"></a>Aangepaste R-scripts in uw experimenten
U kunt eerst de gegevens manipuleren en machine learning-mogelijkheden van ML Studio door aangepaste R-scripts te schrijven uitbreiden.
Hoewel ML Studio een groot aantal modules bevat voor het voorbereiden en analyseren van gegevens, het mag niet overeenkomen met de mogelijkheden van een goed ontwikkelde taal, zoals R.  De service is daarom ontworpen zodat u kunt het introduceren van uw eigen aangepaste R-scripts in gevallen waarbij de opgegeven modules niet voldoen aan uw behoeften.

Als u wilt gebruikmaken van deze mogelijkheid, slepen en neerzetten een 'R-Script uitvoeren'-module in uw experiment.  De code-editor in het deelvenster 'Eigenschappen' vervolgens gebruiken voor een nieuw R-script schrijven of plakt u een bestaand script.  U kunt verwijzen naar externe R-pakketten in het script.  U kunt het script gebruiken om gegevens te manipuleren of met het trainen van complexe ML-modellen die geen deel uitmaken van de standaard ML Studio-model-bibliotheek.

Voor een uitgebreide inleiding over het gebruik van R in experimenten in ML Studio, bekijkt u de ['Beknopte zelfstudie voor de programmeertaal R voor Azure Machine Learning'.](https://docs.microsoft.com/azure/machine-learning/studio/r-quickstart)

### <a name="create-manage-and-deploy-experiments-from-your-local-r-environment"></a>Maken, beheren en implementeren van experimenten van uw lokale R-omgeving
De andere manier dat u R met ML Studio gebruiken kunt, is met de
<code>[AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html)</code> pakket bewaken en beheren van het proces van experimenteren met de programmeertaal R-omgeving.  Dit pakket wordt onderhouden door Microsoft, kunt u om te uploaden en downloaden van gegevenssets van en naar Azure ML experimenten voor R-functies als Azure ML-webservices, publiceren en voor het uitvoeren van R-gegevens via bestaande webservices en op te halen van de uitvoer status.

Dit pakket maakt het veel eenvoudiger met Azure ML als implementatieplatform voor schaalbare voor uw R-code.  In plaats van te klikken en slepen in de gebruikersinterface, kunt u de volledige implementatie-proces met behulp van programma's die u al kent automatiseren.

## <a name="azure-batch"></a>Azure Batch
Voor grootschalige R-taken, kunt u [Azure Batch](https://azure.microsoft.com/services/batch/).  Deze service biedt taakplanning en verwerkingsbeheer op cloudschaal taak, zodat u uw R-workload via tientallen, honderden of duizenden virtuele machines schalen kunt.  Omdat dit een gegeneraliseerde platform voor cloudcomputing, er een aantal opties voor het uitvoeren van R-taken op Azure Batch.

Een optie is het gebruik van Microsoft <code>[doAzureParallel](https://github.com/Azure/doAzureParallel)</code> pakket.  Deze R-pakket is een parallelle back-end voor de `foreach` pakket.  Hierdoor kan elke herhaling van de `foreach` lus om parallel uit voeren op een knooppunt in de Azure Batch-cluster.  Lees voor een inleiding tot het pakket, de ["doAzureParallel: Profiteer van flexibele compute van Azure rechtstreeks vanuit uw R-sessie"](https://azure.microsoft.com/blog/doazureparallel/) blogbericht.

Er is een andere optie voor het uitvoeren van een R-script in Azure Batch te bundelen van uw code met 'RScript.exe' als een Batch-App in Azure portal.  Voor een gedetailleerde gids, moet u de ['Werklasten op Azure Batch R'.](https://azure.microsoft.com/blog/r-workloads-on-azure-batch/)

Een derde optie is het gebruik de [Azure Distributed Data Engineering Toolkit](https://github.com/Azure/aztk) (AZTK), waarmee u te richten op aanvraag Spark-clusters met behulp van Docker-containers in Azure Batch.  Dit biedt een voordelige manier om Spark-taken uitvoeren in Azure.  Met behulp van [SparklyR met AZTK](https://github.com/Azure/aztk/wiki/SparklyR-on-Azure-with-AZTK), uw R-scripts kunnen worden uitgebreid in de cloud eenvoudig en economisch.

## <a name="azure-notebooks"></a>Azure Notebooks

[Azure-notitieblokken](https://notebooks.azure.com) is een goedkope, lage-eenvoudige methode voor R-ontwikkelaars die liever werken met notitieblokken op hun code naar Azure meenemen.  Het is een gratis service voor iedereen ontwikkelen en uitvoeren van code in hun browser met [Jupyter](https://jupyter.org/), dit is een open source-project waarmee markdown prose zoekfunctionaliteit door te combineren, uitvoerbare code en afbeeldingen op bestaat uit één canvas.

De laag gratis service van Azure-notitieblokken is een beschikbare optie voor projecten met een kleinschalige wanneer het proces van elke laptop van 4GB aan geheugen en 1GB gegevenssets limieten. Als u power reken- en gegevens buiten deze beperkingen, maar kunt u uitvoeren notitieblokken in een Data Science Virtual Machine-instantie. Zie voor meer informatie, [beheren en configureren van Azure-notitieblokken projecten - Compute-laag](/azure/notebooks/configure-manage-azure-notebooks-projects.md#compute-tier).

## <a name="azure-sql-database"></a>Azure SQL Database
[Azure SQL Database](https://azure.microsoft.com/services/sql-database/) is van Microsoft intelligente, volledig beheerde relationele clouddatabaseservice.  Hiermee kunt u de volledige kracht van SQL Server zonder alle moeite van het instellen van de infrastructuur gebruiken.  Dit omvat [Machine Learning-Services](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning?view=sql-server-2017), dit is een van de meest recente toevoegingen aan SQL-Service.

Deze functie biedt een ingesloten, voorspellende analyse en data science-engine dat R-code in een SQL Server-database kan worden uitgevoerd als opgeslagen procedures, als met R-overzichten T-SQL-scripts, of als R-code met T-SQL.  In plaats van het ophalen van gegevens uit de database en laden in de R-omgeving, moet u uw R-code rechtstreeks in de database laden en dat rechts naast de gegevens.

Terwijl u Machine Learning-Services is sinds van deel uit van on-premises SQL Server 2016, is het relatief nieuw zijn voor Azure SQL Database.  Het is momenteel in [beperkte Preview-versie](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services?view=sql-server-2017#azure-sql-database-roadmap) , blijven zich ontwikkelen.


### <a name="next-steps"></a>Volgende stappen
* [Uw R-code uitvoeren op Azure met mrsdeploy](http://blog.revolutionanalytics.com/2017/03/running-your-r-code-azure.html)
* [Machine Learning-Server in de Cloud](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-in-the-cloud)
* [Aanvullende bronnen voor Machine Learning-Server en Microsoft R](https://docs.microsoft.com/machine-learning-server/resources-more)
* [R op Azure](https://github.com/yueguoguo/r-on-azure) : een overzicht van pakketten, hulpprogramma's en voor het gebruik van R met de Azure-casestudy's

---

<sub>Het R-logo is &copy; 2016 de R-Foundation en wordt gebruikt overeenkomstig de voorwaarden van de [licentie van Creative Commons Attribution-ShareAlike 4.0 internationale](https://creativecommons.org/licenses/by-sa/4.0/).</sub>
