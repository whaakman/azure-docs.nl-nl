---
title: Inleiding op Azure HDInsight R Server | Microsoft Docs
description: Informatie over het R Server op HDInsight gebruiken voor het maken van toepassingen voor big data-analyse.
services: hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 6dc21bf5-4429-435f-a0fb-eea856e0ea96
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: 860099b2634765f3088199c89341f0b48b20b801
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
#<a name="introduction-to-r-server-and-open-source-r-capabilities-on-hdinsight"></a>Inleiding tot R Server en open-source R mogelijkheden in HDInsight

Microsoft R Server is beschikbaar als een Implementatieoptie wanneer u een HDInsight-clusters in Azure maken. Deze nieuwe mogelijkheid biedt gegevenswetenschappers en statistici R programmeurs op aanvraag toegang tot schaalbare, gedistribueerde methoden van analyses in HDInsight.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)]

Clusters kunnen worden aangepast op de juiste wijze aan de projecten en taken bij de hand en vervolgens verwijderd wanneer ze niet meer nodig zijn. Omdat ze onderdeel van Azure HDInsight zijn, is deze clusters worden geleverd met 24/7 ondersteuning op bedrijfsniveau, een SLA van 99,9% beschikbaarheid en de mogelijkheid te integreren met andere onderdelen in het Azure-ecosysteem.

Op HDInsight R Server biedt de nieuwste mogelijkheden voor analyses op basis van R op gegevenssets van vrijwel elke grootte, in Azure Blob of Data Lake storage geladen. Aangezien R Server is gebouwd op open-source R, de R-toepassingen die u bouwt gebruik kunnen maken van de open-source R-pakketten 8000 +. De routines in ScaleR, van Microsoft big analytics gegevenspakket opgenomen met R Server, zijn ook beschikbaar.

Het edge-knooppunt van een cluster is een handige locatie verbinding maken met het cluster en voor het uitvoeren van uw R-scripts. Met een edge-knooppunt hebt u de optie van de parallelized gedistribueerde functies van ScaleR over de kernen van de edge-knooppunt server wordt uitgevoerd. U kunt ook deze uitvoeren op de knooppunten van het cluster met behulp van ScaleR Hadoop kaart verminderen of Spark compute-contexten.

De modellen of voorspellingen waarmee het resultaat van de analyses kan worden gedownload voor gebruik van lokale. Ze kunnen ook worden geoperationaliseerd elders in Azure, met name via [Azure Machine Learning Studio](http://studio.azureml.net) [webservice](../../machine-learning/studio/publish-a-machine-learning-web-service.md).

## <a name="get-started-with-r-on-hdinsight"></a>Aan de slag met R in HDInsight
Als u wilt opnemen R Server in een HDInsight-cluster, moet u het type van de cluster R Server bij het maken van een HDInsight-cluster met de Azure portal. Het type R Server-cluster bevat R Server op de gegevensknooppunten van het cluster en op een edge-knooppunt dat als een zone landingspagina voor analyses op basis van een R Server fungeert. Zie [aan de slag met op HDInsight R Server](r-server-get-started.md) voor een overzicht over het maken van het cluster.

## <a name="learn-about-data-storage-options"></a>Meer informatie over opties voor opslag van gegevens
Standaard opslag voor het bestandssysteem HDFS van HDInsight-clusters kan worden gekoppeld aan een Azure Storage-account of een Azure Data Lake store. Deze koppeling zorgt ervoor dat de gegevens worden geüpload naar het cluster opslag tijdens de analyse persistent wordt gemaakt. Er zijn verschillende hulpmiddelen voor het verwerken van de gegevensoverdracht naar de opslagoptie die u selecteert, met inbegrip van de portal-gebaseerde uploaden faciliteit van het opslagaccount en de [AzCopy](../../storage/common/storage-use-azcopy.md) hulpprogramma.

U hebt de mogelijkheid toegang toe te voegen aan extra Blob en Data lake slaat tijdens het inrichtingsproces ongeacht de primaire opslagoptie gebruikt cluster. Zie [aan de slag met op HDInsight R Server](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-get-started) voor informatie over het toevoegen van toegang tot extra accounts. Zie de aanvullende [Azure Storage-opties voor R Server op HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-storage) artikel voor meer informatie over het gebruik van meerdere opslagaccounts.

U kunt ook [Azure Files](../../storage/files/storage-how-to-use-files-linux.md) als een opslagoptie voor gebruik op de edge-knooppunt. Azure Files kunt u een bestandsshare die is gemaakt in Azure Storage naar het bestandssysteem van Linux te koppelen. Zie voor meer informatie over deze opties voor het opslaan van gegevens voor R Server op HDInsight-cluster [Azure Storage-opties voor clusters op HDInsight R Server](r-server-storage.md).

## <a name="access-r-server-on-the-cluster"></a>-R Server op het cluster
U kunt verbinding maken met R Server op de edge-knooppunt met een browser. Dit is standaard geïnstalleerd tijdens het maken van het cluster. Zie voor meer informatie [aan de slag met op HDInsight R Server](r-server-get-started.md).

U kunt ook verbinding maken met het R-Server vanaf de opdrachtregel met behulp van SSH/PuTTY toegang tot de R-console. 

## <a name="develop-and-run-r-scripts"></a>Ontwikkelen en uitvoeren van scripts R
De R-scripts u maken en uitvoeren kunnen van de 8000 + open-source R-pakketten naast de geparallelliseerde en gedistribueerde routines in de bibliotheek ScaleR gebruiken. In het algemeen een script dat wordt uitgevoerd met R Server op de edge-knooppunt uitgevoerd binnen de R-interpreter op dat knooppunt. De uitzonderingen worden die stappen die nodig zijn om aan te roepen, een functie ScaleR met een compute-context die is ingesteld op Hadoop-kaart (RxHadoopMR) verminderen of Spark (RxSpark). De functie wordt in dit geval wordt uitgevoerd in een gedistribueerde manier over die (taak)-gegevensknooppunten van het cluster die gekoppeld aan de gegevens waarnaar wordt verwezen zijn. Zie voor meer informatie over de verschillende rekenscenario context opties [Compute context opties voor R Server op HDInsight](r-server-compute-contexts.md).

## <a name="operationalize-a-model"></a>Mogelijk een model maken
Wanneer uw gegevens modelleren voltooid is, kunt u het model om te maken van voorspellingen voor nieuwe gegevens vanuit Azure en on-premises operationeel te maken. Dit proces staat bekend als het score berekenen. Score berekenen kan worden gedaan in HDInsight, Azure Machine Learning of on-premises.

### <a name="score-in-hdinsight"></a>Score in HDInsight
Als u wilt beoordelen in HDInsight, een R-functie die het model voor het maken van voorspellingen voor een nieuw gegevensbestand dat u hebt geladen naar uw opslagaccount aanroept te schrijven. Sla de voorspellingen terug naar het opslagaccount. U kunt de routine op aanvraag uitvoeren op de edge-knooppunt van het cluster of met behulp van een geplande taak.  

### <a name="score-in-azure-machine-learning-aml"></a>Score in Azure Machine Learning eigen (AML)
Als u wilt beoordelen via een webservice AML, gebruiken de open-source Azure Machine Learning-R-pakket bekend als [AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) voor het publiceren van uw model als een Azure-web-service. Dit pakket is voor het gemak vooraf geïnstalleerd zijn op de edge-knooppunt. Vervolgens de faciliteiten in Machine Learning gebruiken voor het maken van een gebruikersinterface voor de webservice en vervolgens de webservice aanroepen naar behoefte voor score berekenen.

Als u deze optie kiest, moet u eventuele modelobjecten ScaleR converteren naar modelobjecten equivalent open source-voor gebruik met de webservice. Gebruik ScaleR afdwingen functies, zoals `as.randomForest()` voor ensemble-modellen, voor deze conversie.

### <a name="score-on-premises"></a>Score lokale
Om te beoordelen op lokale na het maken van uw model, kunt u serialiseren van het model in R, downloaden, de serialisatie van het en vervolgens worden gebruikt voor score berekenen voor nieuwe gegevens. U kunt met behulp van de eerder beschreven in aanpak beoordeling van nieuwe gegevens [scores in HDInsight](#scoring-in-hdinsight) of met behulp van [DeployR](https://deployr.revolutionanalytics.com/).

## <a name="maintain-the-cluster"></a>Het cluster onderhouden
### <a name="install-and-maintain-r-packages"></a>Installeren en onderhouden van R-pakketten
De R-pakketten die u gebruikt de meeste zijn vereist op de edge-knooppunt sinds het meeste werk uw R-scripts uitvoeren er. Als u wilt aanvullende R-pakketten op de edge-knooppunt installeert, kunt u de gebruikelijke `install.packages()` methode in R.

Als u zojuist hebt gebruikt routines uit de bibliotheek ScaleR in het cluster, hoeft niet normaal gesproken u voor het installeren van extra R-pakketten op de gegevensknooppunten. U moet echter extra pakketten voor de ondersteuning van het gebruik van **rxExec** of **RxDataStep** worden uitgevoerd op de gegevensknooppunten.

In dergelijke gevallen kunnen de extra pakketten met een scriptactie worden geïnstalleerd nadat u het cluster hebt gemaakt. Zie voor meer informatie [maken van een HDInsight-cluster met R Server](r-server-get-started.md).   

### <a name="change-hadoop-map-reduce-memory-settings"></a>Hadoop-kaart verminderen geheugeninstellingen wijzigen
Een cluster kan worden aangepast voor het wijzigen van de hoeveelheid geheugen die beschikbaar is voor R Server tijdens het uitvoeren van een taak kaart verminderen. Voor het wijzigen van een cluster door de Apache Ambari-gebruikersinterface die beschikbaar is via de Azure portal blade voor uw cluster te gebruiken. Zie voor instructies over toegang tot de Ambari UI voor uw cluster [HDInsight-clusters beheren met de Ambari-Webgebruikersinterface](../hdinsight-hadoop-manage-ambari.md).

Het is ook mogelijk om te wijzigen van de hoeveelheid geheugen die beschikbaar is voor R Server met behulp van Hadoop-switches in de aanroep naar **RxHadoopMR** als volgt:

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>Een cluster schalen
Een bestaand cluster kan worden geschaald omhoog of omlaag via de portal. Door te schalen, krijgt u de extra capaciteit die u voor grotere verwerkingstaken wellicht of u kunt schalen weer een cluster als deze niet actief is. Zie voor instructies over het schalen van een cluster [HDInsight-clusters beheren](../hdinsight-administer-use-portal-linux.md).

### <a name="maintain-the-system"></a>Het systeem onderhouden
Onderhoud OS patches en andere updates toe te passen wordt uitgevoerd op de onderliggende Linux VM's in een HDInsight-cluster buiten kantooruren. Onderhoud wordt gewoonlijk gedaan om 3:30 uur (op basis van de lokale tijd voor de virtuele machine) elke maandag en donderdag. Updates worden zodanig dat ze geen invloed heeft op meer dan een kwartaal van het cluster tegelijkertijd uitgevoerd.  

Aangezien de hoofdknooppunten redundant zijn en niet alle gegevensknooppunten zijn beïnvloed, vertragen alle taken die worden uitgevoerd tijdens deze periode. Ze moeten nog steeds uitgevoerd worden voltooid, maar. Alle aangepaste software- of lokale gegevens die u hebt blijft behouden over deze gebeurtenissen onderhoud tenzij er een onherstelbare fout optreedt waarvoor cluster opnieuw opbouwen.

## <a name="learn-about-ide-options-for-r-server-on-an-hdinsight-cluster"></a>Meer informatie over IDE-opties voor R Server op een HDInsight-cluster
Het Linux-edge-knooppunt van een HDInsight-cluster is de zone landingspagina voor op basis van het R-analyse. Recente versies van HDInsight biedt een standaardinstallatie van RStudio Server op de edge-knooppunt als een browser gebaseerde IDE. Gebruik van RStudio Server als een IDE voor de ontwikkeling en uitvoering van scripts R aanzienlijk meer productief zijn dan het gebruik van alleen de R-console.

Een andere optie voor volledige IDE is een bureaublad IDE installeren en deze gebruiken voor toegang tot het cluster door gebruik te maken van een externe Map verminderen of Spark compute context. Opties zijn onder andere Microsoft [R-Tools voor Visual Studio](https://www.visualstudio.com/features/rtvs-vs.aspx) (RTVS) RStudio, en Walware's op basis van Eclipse [StatET](http://www.walware.de/goto/statet).

Ten slotte kunt u de console R Server op de edge-knooppunt openen door te typen **R** achter de opdrachtprompt Linux na verbinding maakt via SSH of PuTY. Wanneer u de console-interface, is het handiger om te een teksteditor voor het ontwikkelen van R-script uitvoeren in een ander venster en knippen en plakken van secties van het script in de console R zo nodig.

## <a name="learn-about-pricing"></a>Meer informatie over prijzen
De kosten die gekoppeld aan een HDInsight-cluster met R Server zijn zijn manier gestructureerd als de kosten voor de standaard HDInsight-clusters. Ze zijn gebaseerd op de grootte van de onderliggende virtuele machines in de naam, de gegevens en de edge-knooppunten, met de toevoeging van een opwaartse core uur. Zie voor meer informatie over prijzen voor HDInsight [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Volgende stappen
Zie de volgende onderwerpen voor meer informatie over het gebruik van R Server met HDInsight-clusters:

* [Aan de slag met op HDInsight R Server](r-server-get-started.md)
* [Opties voor compute-context voor R Server op HDInsight](r-server-compute-contexts.md)
* [Opties voor Azure-opslag voor R Server op HDInsight](r-server-storage.md)
