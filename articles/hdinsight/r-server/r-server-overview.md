---
title: Kennismaking met ML-Services op Azure HDInsight
description: Informatie over het gebruik van ML-Services op HDInsight te maken van toepassingen voor big data-analyse.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/27/2018
ms.openlocfilehash: 1f53d87d6de76c0ff6954c9bd38d95e05981d822
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52499468"
---
# <a name="introduction-to-ml-services-and-open-source-r-capabilities-on-hdinsight"></a>Inleiding tot ML-Services en mogelijkheden van de open-source R op HDInsight

> [!NOTE]
> In September 2017, Microsoft R Server werd uitgebracht onder de nieuwe naam van **Microsoft Machine Learning Server** of ML-Server. Als gevolg daarvan kan R Server-cluster in HDInsight heet nu **Machine Learning-Services** of **ML-Services** -cluster in HDInsight. Zie voor meer informatie over de naamswijziging van R Server [Microsoft R Server is nu Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/rebranding-microsoft-r-server#get-support-for-r-server).

Microsoft Machine Learning Server is beschikbaar als een implementatie-opties wanneer u een HDInsight-clusters in Azure maken. Het clustertype waarmee u deze optie wordt aangeroepen **ML-Services**. Deze mogelijkheid biedt gegevenswetenschappers, statistici en R-programmeurs on-demandtoegang tot schaalbare, gedistribueerde analysemethoden in HDInsight.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)]

ML-Services op HDInsight biedt de nieuwste mogelijkheden voor analyses op basis van R op gegevenssets van vrijwel elke grootte, geladen in Azure Blob of Data Lake-opslag. Omdat het cluster ML-Services is gebouwd op open-source R, de toepassingen op basis van R die u bouwt gebruik kunnen maken van de 8000 + open source R-pakketten. De routines in ScaleR van Microsoft big data analytics-pakket zijn ook beschikbaar.

Het edge-knooppunt van een cluster biedt een handige locatie verbinding maken met het cluster en uw R-scripts uit te voeren. Met een edge-knooppunt hebt u de mogelijkheid van het uitvoeren van de functies voor geparallelliseerde gedistribueerde van ScaleR over de kernen van het edge-knooppunt-server. U kunt ze ook uitvoeren op de knooppunten van het cluster met behulp van de ScaleR Hadoop Mapreduce of Apache Spark-compute-context.

De modellen of voorspellingen op die het resultaat van de analyse kunnen worden gedownload voor on-premises gebruik. Ze kunnen ook operationaliseren elders in Azure, met name via [Azure Machine Learning Studio](http://studio.azureml.net) [webservice](../../machine-learning/studio/publish-a-machine-learning-web-service.md).

## <a name="get-started-with-ml-services-on-hdinsight"></a>Aan de slag met ML-Services op HDInsight

Voor het maken van een ML-Services-cluster in Azure HDInsight, selecteer de **ML-Services** clustertype bij het maken van een HDInsight-cluster met behulp van de Azure portal. Het clustertype van de ML-Services bevat ML-Server op de gegevensknooppunten van het cluster en een edge-knooppunt, dat als een zone landingspagina voor analyses op basis van een ML-Services fungeert. Zie [aan de slag met ML-Services op HDInsight](r-server-get-started.md) voor een overzicht over het maken van het cluster.

## <a name="why-choose-ml-services-in-hdinsight"></a>Waarom kiezen voor ML-Services in HDInsight?

ML-Services in HDInsight biedt de volgende voordelen:

### <a name="ai-innovation-from-microsoft-and-open-source"></a>AI-innovaties van Microsoft als open source

  ML-Services bevat een zeer schaalbare, gedistribueerde set algoritmen zoals [RevoscaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler), [revoscalepy](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/revoscalepy-package), en [microsoftML](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/microsoftml-package) die kan worden gebruikt voor gegevens die groter zijn dan de de grootte van fysiek geheugen en wordt uitgevoerd op een groot aantal verschillende platformen in een gedistribueerde manier. Meer informatie over het verzamelen van Microsoft's aangepaste [R-pakketten](https://docs.microsoft.com/machine-learning-server/r-reference/introducing-r-server-r-package-reference) en [Python-pakketten](https://docs.microsoft.com/machine-learning-server/python-reference/introducing-python-package-reference) opgenomen met het product.
  
  ML-Services brug vormt tussen deze Microsoft-innovaties en bijdragen die afkomstig zijn van de open-source-community (R, Python en AI-toolkits) alle boven op een enkele eersteklas platform. Een pakket R of Python, open-source machine learning kunt werken samen met eventuele bedrijfsspecifieke innovatie van Microsoft.

### <a name="simple-secure-and-high-scale-operationalization-and-administration"></a>Eenvoudige, veilige en uiterst schaalbare uitoefening en beheer

  Ondernemingen die afhankelijk zijn van traditionele paradigma's en omgevingen investeren veel tijd en moeite voor uitoefening. Dit resulteert in hoge kosten en vertragingen met inbegrip van de tijd van de vertaling voor modellen, iteraties zodat ze kunnen blijven geldig en goedkeuring van de huidige, regelgeving en beheren van machtigingen via uitoefening.

  ML-Services biedt geavanceerde [uitoefening](https://docs.microsoft.com/machine-learning-server/what-is-operationalization), in dat wanneer een machine learning-model is voltooid, duurt het slechts een paar klikken voor het genereren van webservices API's. Deze [webservices](https://docs.microsoft.com/machine-learning-server/operationalize/concept-what-are-web-services) worden gehost op een server raster in de cloud en kan worden geïntegreerd met line-of-business-toepassingen. De mogelijkheid om te implementeren in een elastische grid kunt u probleemloos schalen met de behoeften van uw bedrijf, zowel voor batch- en realtime scoren. Zie voor instructies [ML-Services operationeel maken in HDInsight](r-server-operationalize.md).

<!---
* **Deep ecosystem engagements to deliver customer success with optimal total cost of ownership**

  Individuals embarking on the journey of making their applications intelligent or simply wanting to learn the new world of AI and machine learning, need the right resources to help them get started. In addition to this documentation, Microsoft provides several learning resources and has engaged several training partners to help you ramp up and become productive quickly.
--->

## <a name="key-features-of-ml-services-on-hdinsight"></a>Belangrijke functies van ML-Services op HDInsight

De volgende functies zijn opgenomen in ML-Services op HDInsight.

| Functie-categorie | Beschrijving |
|------------------|-------------|
| R-ingeschakeld | [R-pakketten](https://docs.microsoft.com/machine-learning-server/r-reference/introducing-r-server-r-package-reference) voor oplossingen die zijn geschreven in R, met een open-source-distributie van R- en runtime-infrastructuur voor het uitvoeren van script. |
| Python-ingeschakeld | [Python-modules](https://docs.microsoft.com/machine-learning-server/python-reference/introducing-python-package-reference) voor oplossingen die zijn geschreven in Python, met een open-source-verdeling van Python en runtime-infrastructuur voor het uitvoeren van script.
| [Vooraf getrainde modellen](https://docs.microsoft.com/machine-learning-server/install/microsoftml-install-pretrained-models) | Voor visuele analyse en tekstsentimentanalyse, klaar om gegevens te beoordelen u bieden. |
| [Implementeren en gebruiken](r-server-operationalize.md) | Operationeel maken van uw server en -oplossingen implementeren als een webservice. |
| [Uitvoering op afstand](r-server-hdinsight-manage.md#connect-remotely-to-microsoft-ml-services) | Externe sessies op de cluster ML-Services in uw netwerk vanaf uw clientwerkstation starten. |


## <a name="data-storage-options-for-ml-services-on-hdinsight"></a>Opties voor gegevensopslag voor ML-Services op HDInsight

Standaardopslag voor het HDFS-bestandssysteem van HDInsight-clusters kan worden gekoppeld aan een Azure Storage-account of een Azure Data Lake Store. Deze koppeling zorgt ervoor dat de gewenste gegevens is geüpload naar het cluster opslag tijdens de analyse persistent wordt gemaakt en de gegevens zijn beschikbaar, zelfs nadat het cluster wordt verwijderd. Er zijn verschillende hulpprogramma's voor het verwerken van de gegevensoverdracht naar de opslagoptie die u selecteert, met inbegrip van de faciliteit portal op basis van het uploaden van het opslagaccount en de [AzCopy](../../storage/common/storage-use-azcopy.md) hulpprogramma.

U hebt de mogelijkheid van het inschakelen van toegang tot extra Blob en Data lake opgeslagen tijdens het inrichtingsproces, ongeacht de primaire opslag-optie gebruikt cluster. Zie [aan de slag met ML-Services op HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-get-started) voor informatie over het toevoegen van toegang tot extra accounts. Zie [Azure Storage-opties voor ML-Services op HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-storage) artikel voor meer informatie over het gebruik van meerdere opslagaccounts.

U kunt ook [Azure Files](../../storage/files/storage-how-to-use-files-linux.md) als een opslagoptie voor gebruik op het edge-knooppunt. Azure Files kunt u een bestandsshare die is gemaakt in Azure Storage op het Linux-bestandssysteem koppelen. Zie voor meer informatie over deze opties voor gegevensopslag voor ML-Services op HDInsight-cluster, [Azure Storage-opties voor ML-Services op HDInsight](r-server-storage.md).

## <a name="access-ml-services-edge-node"></a>Toegang tot Services ML edge-knooppunt

U kunt verbinding maken met Microsoft ML Server op het edge-knooppunt via een browser. Dit is standaard geïnstalleerd tijdens het maken van clusters. Zie voor meer informatie, [stared ophalen met ML-Services op HDInsight](r-server-get-started.md). U kunt ook verbinding maken met het edge-knooppunt van het cluster vanaf de opdrachtregel met behulp van SSH/PuTTY voor toegang tot de R-console.

## <a name="develop-and-run-r-scripts"></a>Ontwikkelen en uitvoeren van R-scripts

De R-scripts u maken en uitvoeren kunnen u elk van de 8000 + open source R-pakketten naast de geparallelliseerde en gedistribueerde routines in de bibliotheek ScaleR. Een script dat wordt uitgevoerd met ML-Services op het edge-knooppunt wordt in het algemeen uitgevoerd binnen de R-interpreter op dat knooppunt. De uitzonderingen zijn de stappen die nodig hebt om aan te roepen een ScaleR-functie met een compute-context is ingesteld op Hadoop Mapreduce (RxHadoopMR) of Spark (RxSpark). De functie wordt in dit geval wordt uitgevoerd in een gedistribueerde gestart op deze (taak)-gegevensknooppunten van het cluster die gekoppeld aan de gegevens waarnaar wordt verwezen zijn. Zie voor meer informatie over de opties voor andere compute-context, [Compute-context, opties voor ML-Services op HDInsight](r-server-compute-contexts.md).

## <a name="operationalize-a-model"></a>Een model operationeel maken

Wanneer de gegevensmodellering voltooid is, kunt u het model voorspellingen voor nieuwe gegevens vanuit Azure of on-premises uitvoeren. Dit proces staat bekend als scoren. Score kan worden gedaan in HDInsight, Azure Machine Learning of on-premises.

### <a name="score-in-hdinsight"></a>Score in HDInsight

Als u wilt beoordelen in HDInsight, een R-functie waarmee het model om voorspellingen voor een nieuw gegevensbestand die u hebt geladen naar uw opslagaccount te maken wordt aangeroepen te schrijven. Sla de voorspellingen vervolgens terug naar het opslagaccount. U kunt deze routine on-demand uitvoeren op het edge-knooppunt van het cluster of met behulp van een geplande taak.

### <a name="score-in-azure-machine-learning-aml"></a>Score in Azure Machine Learning (AML)

Voor het scoren met Azure Machine Learning, gebruikt u het open-source Azure Machine Learning, R-pakket genoemd [AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) voor het publiceren van uw modellen vervolgens als een Azure-web-service. Dit pakket is voor het gemak vooraf worden geïnstalleerd op het edge-knooppunt. Vervolgens de faciliteiten in Azure Machine Learning gebruiken om te maken van een gebruikersinterface voor de webservice en roep vervolgens de web-service zo nodig voor het scoren.

Als u deze optie kiest, moet u alle modelobjecten ScaleR converteren naar gelijkwaardige open-source-modelobjecten voor gebruik met de webservice. Gebruik functies voor ScaleR-afdwingen, zoals `as.randomForest()` voor ensembles op basis van modellen, voor deze conversie.

### <a name="score-on-premises"></a>Score on-premises

Voor het scoren van on-premises na het maken van uw model, kunt u serialiseren van het model in R, downloaden, de serialisatie van het en vervolgens worden gebruikt voor het scoren van nieuwe gegevens. Beoordeling van nieuwe gegevens kunt u met behulp van de eerder beschreven in aanpak [Score in HDInsight](#score-in-hdinsight) of met behulp van [webservices](https://docs.microsoft.com/machine-learning-server/operationalize/concept-what-are-web-services).

## <a name="maintain-the-cluster"></a>Het cluster beheren

### <a name="install-and-maintain-r-packages"></a>Installeren en onderhouden van R-pakketten

De meeste van de R-pakketten die u gebruikt zijn vereist op het edge-knooppunt sinds de meeste stappen voor uw R-scripts die er worden uitgevoerd. Voor het installeren van extra R-pakketten op het edge-knooppunt, kunt u de `install.packages()` methode in R.

Als u alleen van routines uit de ScaleR-bibliotheek in het cluster gebruikmaakt, hoeft niet normaal gesproken u extra R-pakketten installeren op de gegevensknooppunten. U moet echter extra pakketten voor de ondersteuning van het gebruik van **rxExec** of **RxDataStep** kan worden uitgevoerd op de gegevensknooppunten.

In dergelijke gevallen kunnen de extra pakketten met een scriptactie worden geïnstalleerd nadat u het cluster hebt gemaakt. Zie voor meer informatie, [ML-Services beheren in HDInsight-cluster](r-server-hdinsight-manage.md).

### <a name="change-apache-hadoop-mapreduce-memory-settings"></a>Apache Hadoop MapReduce-geheugeninstellingen wijzigen

Een cluster kan worden gewijzigd als u wilt wijzigen van de hoeveelheid geheugen die voor ML-Services beschikbaar is wanneer deze een MapReduce-taak wordt uitgevoerd. Voor het wijzigen van een cluster, gebruikt u de Apache Ambari-Webinterface die is beschikbaar via de blade in Azure portal voor uw cluster. Zie voor instructies over hoe u toegang tot de Ambari UI voor uw cluster [beheren HDInsight-clusters met behulp van de Ambari-Webgebruikersinterface](../hdinsight-hadoop-manage-ambari.md).

Het is ook mogelijk om te wijzigen van de hoeveelheid geheugen die beschikbaar is voor ML-Services met behulp van Hadoop-switches in de aanroep naar **RxHadoopMR** als volgt:

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>Een cluster schalen

Een bestaand cluster ML-Services op HDInsight kan omhoog of omlaag worden geschaald via de portal. Door omhoog te schalen, kunt u de capaciteit die u nodig voor grotere verwerkingstaken hebt mogelijk toegang of u kunt weer een cluster schalen wanneer deze niet actief is. Zie voor instructies over hoe u een cluster kunt schalen [beheren HDInsight-clusters](../hdinsight-administer-use-portal-linux.md).

### <a name="maintain-the-system"></a>Het systeem onderhouden

Onderhoud toepassen van patches voor het besturingssysteem en andere updates wordt uitgevoerd op de onderliggende Linux-VM's in een HDInsight-cluster buiten kantooruren. Onderhoud wordt gewoonlijk gedaan om 03:30 uur (gebaseerd op de lokale tijd voor de virtuele machine) elke maandag en donderdag. Updates worden zodanig dat ze niet van invloed zijn op meer dan een kwartaal van het cluster tegelijk uitgevoerd.

Omdat de hoofdknooppunten redundante zijn en niet alle gegevensknooppunten worden beïnvloed, alle taken die worden uitgevoerd tijdens deze periode mogelijk vertragen. Ze moeten echter nog steeds uitgevoerd worden voltooid. Aangepaste software of lokale gegevens die u hebt behouden over deze onderhoudsgebeurtenissen voor, tenzij het een onherstelbare fout optreedt waarvoor het herbouwen van een cluster.

## <a name="ide-options-for-ml-services-on-hdinsight"></a>IDE-opties voor ML-Services op HDInsight

Het Linux-edge-knooppunt van een HDInsight-cluster is de zone van de landingspagina voor op basis van R-analyse. Recente versies van HDInsight bieden een standaardinstallatie van RStudio Server op het edge-knooppunt als een browser gebaseerde IDE. Gebruik van RStudio Server als een IDE voor de ontwikkeling en uitvoering van R-scripts kunnen productief zijn aanzienlijk meer dan alleen met behulp van de R-console.

Bovendien kunt u een bureaublad-IDE installeren en deze gebruiken voor toegang tot het cluster door gebruik te maken van een externe MapReduce of Spark compute-context. Opties zijn onder andere Microsoft [R Tools voor Visual Studio](https://www.visualstudio.com/features/rtvs-vs.aspx) (RTVS), RStudio, en Walware's op basis van Eclipse [StatET](http://www.walware.de/goto/statet).

Bovendien kunt u de R-console op het edge-knooppunt openen door te typen **R** achter de opdrachtprompt Linux na het maken van een verbinding via SSH of PuTTY. Wanneer u de console-interface gebruikt, is het handig is dat een teksteditor voor het ontwikkelen van R-script uitvoeren in een ander venster en knippen en plakken van secties van het script in de R-console naar behoefte.

## <a name="pricing"></a>Prijzen

De prijzen die gekoppeld aan een cluster met HDInsight voor ML-Services zijn zijn op dezelfde manier gestructureerd op de prijzen voor andere typen van HDInsight-cluster. Ze zijn gebaseerd op de grootte van de onderliggende virtuele machines in de naam, de gegevens en het edge-knooppunten, met de toevoeging van een kern-uur slechts iets hoger ligt. Zie voor meer informatie, [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie over het gebruik van ML-Services op HDInsight-clusters:

* [Aan de slag met ML-Services-cluster in HDInsight](r-server-get-started.md)
* [Opties voor compute-context voor ML Services-cluster in HDInsight](r-server-compute-contexts.md)
* [Opties voor opslag voor ML-Services-cluster in HDInsight](r-server-storage.md)
