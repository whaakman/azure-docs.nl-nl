---
title: Inleiding tot ML-Services in Azure HDInsight | Microsoft Docs
description: Informatie over het ML-Services op HDInsight gebruiken voor het maken van toepassingen voor big data-analyse.
services: hdinsight
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.assetid: 6dc21bf5-4429-435f-a0fb-eea856e0ea96
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: nitinme
ms.openlocfilehash: 9633a7df1cb72f3e9e5ee79be0c332565e7e8f2a
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37054099"
---
# <a name="introduction-to-ml-services-and-open-source-r-capabilities-on-hdinsight"></a>Inleiding tot ML-Services en open-source R mogelijkheden in HDInsight

> [!NOTE]
> In September 2017 Microsoft R Server werd uitgebracht onder de nieuwe naam van **Microsoft Machine Learning Server** of ML-Server. Als gevolg daarvan kan R Server-cluster in HDInsight heet nu **Machine Learning Services** of **ML Services** -cluster in HDInsight. Zie voor meer informatie over de naamswijziging R Server [Microsoft R Server is nu Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/rebranding-microsoft-r-server#get-support-for-r-server).

Microsoft Machine Learning-Server is beschikbaar als een Implementatieoptie wanneer u een HDInsight-clusters in Azure maken. Het type van het cluster waarmee u deze optie wordt aangeroepen **ML Services**. Deze mogelijkheid biedt gegevenswetenschappers en statistici R programmeurs op aanvraag toegang tot schaalbare, gedistribueerde methoden van analyses in HDInsight.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)]

ML-Services op HDInsight biedt de nieuwste mogelijkheden voor analyses op basis van R op gegevenssets van vrijwel elke grootte, in Azure Blob of Data Lake storage geladen. Omdat het cluster ML-Services is gebouwd op open source-R, de R-toepassingen die u bouwt gebruik kunnen maken van de 8000 + open source R-pakketten. De routines in ScaleR big gegevenspakket analytics van Microsoft zijn ook beschikbaar.

Het edge-knooppunt van een cluster is een handige locatie verbinding maken met het cluster en voor het uitvoeren van uw R-scripts. Met een edge-knooppunt hebt u de optie van de parallelized gedistribueerde functies van ScaleR over de kernen van de edge-knooppunt server wordt uitgevoerd. U kunt ook deze uitvoeren op de knooppunten van het cluster met behulp van ScaleR Hadoop kaart verminderen of Spark compute-contexten.

De modellen of voorspellingen die het resultaat van de analyse kunnen worden gedownload voor intern gebruik. Ze kunnen ook worden geoperationaliseerd elders in Azure, met name via [Azure Machine Learning Studio](http://studio.azureml.net) [webservice](../../machine-learning/studio/publish-a-machine-learning-web-service.md).

## <a name="get-started-with-ml-services-on-hdinsight"></a>Aan de slag met ML-Services op HDInsight

Als u een cluster ML-Services in Azure HDInsight, selecteert de **ML Services** cluster type bij het maken van een HDInsight-cluster met de Azure portal. Het type van de cluster ML-Services bevat ML-Server op de gegevensknooppunten van het cluster en op een edge-knooppunt dat als een zone landingspagina voor analyses op basis van het ML-Services fungeert. Zie [aan de slag met ML-Services op HDInsight](r-server-get-started.md) voor een overzicht over het maken van het cluster.

## <a name="why-choose-ml-services-in-hdinsight"></a>Waarom kiezen ML-Services in HDInsight?

ML-Services in HDInsight biedt de volgende voordelen:

### <a name="ai-innovation-from-microsoft-and-open-source"></a>AI innovatie van Microsoft en open-source

  ML-Services bevat uiterst schaalbare, gedistribueerde set algoritmen zoals [RevoscaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler), [revoscalepy](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/revoscalepy-package), en [microsoftML](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/microsoftml-package) die kunnen werken op de grootte van gegevens groter is dan de de grootte van fysiek geheugen en wordt uitgevoerd op een groot aantal verschillende platformen in een gedistribueerde manier. Meer informatie over het verzamelen van Microsoft's aangepaste [R-pakketten](https://docs.microsoft.com/machine-learning-server/r-reference/introducing-r-server-r-package-reference) en [Python-pakketten](https://docs.microsoft.com/machine-learning-server/python-reference/introducing-python-package-reference) die deel uitmaakt van het product.
  
  ML-Services verbinding van deze Microsoft vernieuwingen en bijdragen afkomstig is van de open source-community (toolkits R, Python en AI) alle boven op een enkele bedrijfsniveau-platform. Een R- of Python open-source machine learning-pakket kunt werken naast een eigen innovatie van Microsoft.

### <a name="simple-secure-and-high-scale-operationalization-and-administration"></a>Eenvoudige, veilige en hoge schaalbaarheid uitoefening en beheer

  Ondernemingen vertrouwen op traditionele paradigma's en omgevingen investeren veel tijd en moeite naar uitoefening. Dit resulteert in hoge kosten en er een vertraging met inbegrip van de vertaling tijd voor modellen, iteraties te houden geldige en huidige, regelgevende goedkeuring en beheer van machtigingen via uitoefening.

  ML-Services biedt enterprise hoogwaardige [uitoefening](https://docs.microsoft.com/machine-learning-server/what-is-operationalization), in dat wanneer een machine learning-model is voltooid, duurt een paar muisklikken webservices-API's te genereren. Deze [webservices](https://docs.microsoft.com/machine-learning-server/operationalize/concept-what-are-web-services) worden gehost op een raster server in de cloud en kan worden geïntegreerd met line-of-business-toepassingen. De mogelijkheid om te implementeren voor een elastische raster kunt u probleemloos schalen met de behoeften van uw bedrijf, zowel voor batch- en realtime score berekenen. Zie voor instructies [operationeel ML-Services op HDInsight](r-server-operationalize.md).

<!---
* **Deep ecosystem engagements to deliver customer success with optimal total cost of ownership**

  Individuals embarking on the journey of making their applications intelligent or simply wanting to learn the new world of AI and machine learning, need the right resources to help them get started. In addition to this documentation, Microsoft provides several learning resources and has engaged several training partners to help you ramp up and become productive quickly.
--->

## <a name="key-features-of-ml-services-on-hdinsight"></a>Belangrijke functies van ML-Services op HDInsight

De volgende functies zijn opgenomen in het ML-Services op HDInsight.

| Functie-categorie | Beschrijving |
|------------------|-------------|
| R-ingeschakeld | [R-pakketten](https://docs.microsoft.com/machine-learning-server/r-reference/introducing-r-server-r-package-reference) voor oplossingen die zijn geschreven in R, met een open-source verdeling van R en runtime-infrastructuur voor het uitvoeren van script. |
| Python-ingeschakeld | [Python-modules](https://docs.microsoft.com/machine-learning-server/python-reference/introducing-python-package-reference) voor oplossingen die zijn geschreven in Python, met een open-source verdeling van Python en runtime-infrastructuur voor het uitvoeren van script.
| [Vooraf getraind modellen](https://docs.microsoft.com/machine-learning-server/install/microsoftml-install-pretrained-models) | Visuele analyse en analyse van tekst gevoel gereed om te beoordelen gegevens u bieden. |
| [Implementeren en gebruiken](r-server-operationalize.md) | Uw server operationeel en oplossingen implementeren als een webservice. |
| [Uitvoering op afstand](r-server-hdinsight-manage.md#connect-remotely-to-microsoft-ml-services) | Externe sessies starten op de cluster ML-Services in uw netwerk van uw clientwerkstation. |


## <a name="data-storage-options-for-ml-services-on-hdinsight"></a>Gegevens opslagopties voor het ML-Services op HDInsight

Standaard opslag voor het bestandssysteem HDFS van HDInsight-clusters kan worden gekoppeld aan een Azure Storage-account of een Azure Data Lake Store. Deze koppeling zorgt ervoor dat de gegevens worden geüpload naar het cluster opslag tijdens de analyse persistent is gemaakt en de gegevens zijn beschikbaar, zelfs nadat het cluster wordt verwijderd. Er zijn verschillende hulpmiddelen voor het verwerken van de gegevensoverdracht naar de opslagoptie die u selecteert, met inbegrip van de portal-gebaseerde uploaden faciliteit van het opslagaccount en de [AzCopy](../../storage/common/storage-use-azcopy.md) hulpprogramma.

U hebt de mogelijkheid toegang tot extra Blob inschakelen en Data lake slaat tijdens het inrichtingsproces ongeacht de primaire opslagoptie gebruikt cluster. Zie [aan de slag met ML-Services op HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-get-started) voor informatie over het toevoegen van toegang tot extra accounts. Zie [Azure Storage-opties voor het ML-Services op HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-storage) artikel voor meer informatie over het gebruik van meerdere opslagaccounts.

U kunt ook [Azure Files](../../storage/files/storage-how-to-use-files-linux.md) als een opslagoptie voor gebruik op de edge-knooppunt. Azure Files kunt u een bestandsshare die is gemaakt in Azure Storage naar het bestandssysteem van Linux te koppelen. Zie voor meer informatie over deze opties voor het opslaan van gegevens voor ML-Services op HDInsight-cluster [Azure Storage-opties voor het ML-Services op HDInsight](r-server-storage.md).

## <a name="access-ml-services-edge-node"></a>Toegang tot Services ML edge-knooppunt

U kunt verbinding maken met Microsoft ML Server op de edge-knooppunt met een browser. Dit is standaard geïnstalleerd tijdens het maken van het cluster. Zie voor meer informatie [stared ophalen met ML-Services op HDInsight](r-server-get-started.md). U kunt ook verbinding maken met het edge-knooppunt van het cluster vanaf de opdrachtregel met behulp van SSH/PuTTY toegang tot de R-console.

## <a name="develop-and-run-r-scripts"></a>Ontwikkelen en uitvoeren van scripts R

De R-scripts u maken en uitvoeren kunnen van de 8000 + open source R-pakketten naast de geparallelliseerde en gedistribueerde routines in de bibliotheek ScaleR gebruiken. In het algemeen een script dat wordt uitgevoerd met ML-Services op de edge-knooppunt uitgevoerd binnen de R-interpreter op dat knooppunt. De uitzonderingen worden die stappen die nodig zijn om aan te roepen, een functie ScaleR met een compute-context die is ingesteld op Hadoop-kaart (RxHadoopMR) verminderen of Spark (RxSpark). De functie wordt in dit geval wordt uitgevoerd in een gedistribueerde manier over die (taak)-gegevensknooppunten van het cluster die gekoppeld aan de gegevens waarnaar wordt verwezen zijn. Zie voor meer informatie over de verschillende rekenscenario context opties [Compute context opties voor het ML-Services op HDInsight](r-server-compute-contexts.md).

## <a name="operationalize-a-model"></a>Een model operationeel maken

Wanneer uw gegevens modelleren voltooid is, kunt u het model om te maken van voorspellingen voor nieuwe gegevens vanuit Azure of on-premises operationeel te maken. Dit proces staat bekend als het score berekenen. Score berekenen kan worden gedaan in HDInsight, Azure Machine Learning of on-premises.

### <a name="score-in-hdinsight"></a>Score in HDInsight

Als u wilt beoordelen in HDInsight, een R-functie die het model voor het maken van voorspellingen voor een nieuw gegevensbestand dat u hebt geladen naar uw opslagaccount aanroept te schrijven. Sla de voorspellingen vervolgens terug naar het opslagaccount. U kunt dit routinematig op aanvraag uitvoeren op de edge-knooppunt van het cluster of met behulp van een geplande taak.

### <a name="score-in-azure-machine-learning-aml"></a>Score in Azure Machine Learning eigen (AML)

Als u wilt beoordelen met Azure Machine Learning, gebruiken de open source Azure Machine Learning-R-pakket bekend als [AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) voor het publiceren van uw model als een Azure-web-service. Dit pakket is voor het gemak vooraf geïnstalleerd zijn op de edge-knooppunt. Vervolgens de faciliteiten in Azure Machine Learning gebruiken voor het maken van een gebruikersinterface voor de webservice en vervolgens de webservice aanroepen naar behoefte voor score berekenen.

Als u deze optie kiest, moet u eventuele modelobjecten ScaleR converteren naar modelobjecten equivalent open source-voor gebruik met de webservice. Gebruik ScaleR afdwingen functies, zoals `as.randomForest()` voor ensemble-modellen, voor deze conversie.

### <a name="score-on-premises"></a>Score lokale

Om te beoordelen op lokale na het maken van uw model, kunt u serialiseren van het model in R, downloaden, de serialisatie van het en vervolgens worden gebruikt voor score berekenen voor nieuwe gegevens. U kunt met behulp van de eerder beschreven in aanpak beoordeling van nieuwe gegevens [scores in HDInsight](#scoring-in-hdinsight) of met behulp van [DeployR](https://deployr.revolutionanalytics.com/).

## <a name="maintain-the-cluster"></a>Het cluster onderhouden

### <a name="install-and-maintain-r-packages"></a>Installeren en onderhouden van R-pakketten

De R-pakketten die u gebruikt de meeste zijn vereist op de edge-knooppunt sinds het meeste werk uw R-scripts uitvoeren er. Als u wilt aanvullende R-pakketten op de edge-knooppunt installeert, kunt u de `install.packages()` methode in R.

Als u zojuist hebt gebruikt routines uit de bibliotheek ScaleR in het cluster, hoeft niet normaal gesproken u voor het installeren van extra R-pakketten op de gegevensknooppunten. U moet echter extra pakketten voor de ondersteuning van het gebruik van **rxExec** of **RxDataStep** worden uitgevoerd op de gegevensknooppunten.

In dergelijke gevallen kunnen de extra pakketten met een scriptactie worden geïnstalleerd nadat u het cluster hebt gemaakt. Zie voor meer informatie [ML Services beheren in HDInsight-cluster](r-server-hdinsight-manage.md).

### <a name="change-hadoop-mapreduce-memory-settings"></a>Hadoop-MapReduce-geheugeninstellingen wijzigen

Een cluster kan worden aangepast voor het wijzigen van de hoeveelheid geheugen die beschikbaar is voor ML-Services wanneer het een MapReduce-taak wordt uitgevoerd. Voor het wijzigen van een cluster door de Apache Ambari-gebruikersinterface die beschikbaar is via de Azure portal blade voor uw cluster te gebruiken. Zie voor instructies over toegang tot de Ambari UI voor uw cluster [HDInsight-clusters beheren met de Ambari-Webgebruikersinterface](../hdinsight-hadoop-manage-ambari.md).

Het is ook mogelijk om te wijzigen van de hoeveelheid geheugen die beschikbaar is voor ML-Services met behulp van Hadoop-switches in de aanroep naar **RxHadoopMR** als volgt:

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>Een cluster schalen

Een bestaand cluster ML-Services op HDInsight kan worden geschaald omhoog of omlaag via de portal. Met omhoog schalen, krijgt u de extra capaciteit die u voor grotere verwerkingstaken wellicht of u kunt schalen weer een cluster als deze niet actief is. Zie voor instructies over het schalen van een cluster [HDInsight-clusters beheren](../hdinsight-administer-use-portal-linux.md).

### <a name="maintain-the-system"></a>Het systeem onderhouden

Onderhoud OS patches en andere updates toe te passen wordt uitgevoerd op de onderliggende Linux VM's in een HDInsight-cluster buiten kantooruren. Onderhoud wordt gewoonlijk gedaan om 3:30 uur (op basis van de lokale tijd voor de virtuele machine) elke maandag en donderdag. Updates worden zodanig dat ze geen invloed heeft op meer dan een kwartaal van het cluster tegelijkertijd uitgevoerd.

Aangezien de hoofdknooppunten redundant zijn en niet alle gegevensknooppunten zijn beïnvloed, vertragen alle taken die worden uitgevoerd tijdens deze periode. Ze moeten echter nog steeds uitgevoerd worden voltooid. Alle aangepaste software- of lokale gegevens die u hebt blijft behouden over deze gebeurtenissen onderhoud tenzij er een onherstelbare fout optreedt waarvoor cluster opnieuw opbouwen.

## <a name="ide-options-for-ml-services-on-hdinsight"></a>IDE-opties voor het ML-Services op HDInsight

Het Linux-edge-knooppunt van een HDInsight-cluster is de zone landingspagina voor op basis van het R-analyse. Recente versies van HDInsight biedt een standaardinstallatie van RStudio Server op de edge-knooppunt als een browser gebaseerde IDE. Gebruik van RStudio Server als een IDE voor de ontwikkeling en uitvoering van scripts R aanzienlijk meer productief zijn dan het gebruik van alleen de R-console.

Bovendien kunt u een bureaublad IDE installeren en gebruiken voor toegang tot het cluster door gebruik te maken van een externe MapReduce of Spark compute-context. Opties zijn onder andere Microsoft [R-Tools voor Visual Studio](https://www.visualstudio.com/features/rtvs-vs.aspx) (RTVS) RStudio, en Walware's op basis van Eclipse [StatET](http://www.walware.de/goto/statet).

Bovendien kunt u het R-console op de edge-knooppunt openen door te typen **R** achter de opdrachtprompt Linux na verbinding maakt via SSH of PuTTY. Wanneer u de console-interface, is het handiger om te een teksteditor voor het ontwikkelen van R-script uitvoeren in een ander venster en knippen en plakken van secties van het script in de console R zo nodig.

## <a name="pricing"></a>Prijzen

De prijzen die gekoppeld aan een ML Services HDInsight-cluster zijn worden op dezelfde manier gestructureerd op de prijzen voor andere typen HDInsight-cluster. Ze zijn gebaseerd op de grootte van de onderliggende virtuele machines in de naam, de gegevens en de edge-knooppunten, met de toevoeging van een opwaartse core uur. Zie voor meer informatie [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie over het gebruik van ML-Services op HDInsight-clusters:

* [Aan de slag met ML Serices-cluster in HDInsight](r-server-get-started.md)
* [COMPUTE context opties voor het cluster in HDInsight ML-Services](r-server-compute-contexts.md)
* [Opties voor opslag voor ML-Services-cluster in HDInsight](r-server-storage.md)