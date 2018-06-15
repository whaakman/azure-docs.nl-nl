---
title: Inleiding op Azure HDInsight R Server | Microsoft Docs
description: Informatie over het R Server op HDInsight gebruiken voor het maken van toepassingen voor big data-analyse.
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
ms.date: 03/23/2018
ms.author: nitinme
ms.openlocfilehash: 19c286db9a8a2aa537badc83d98a1b74b73e9873
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
ms.locfileid: "31414715"
---
# <a name="introduction-to-r-server-and-open-source-r-capabilities-on-hdinsight"></a>Inleiding tot R Server en open-source R mogelijkheden in HDInsight

Microsoft R Server (ook bekend als Microsoft Machine Learning-Server) is beschikbaar als een Implementatieoptie wanneer u een HDInsight-clusters in Azure maken. Het type van het cluster waarmee u deze optie wordt aangeroepen **R Server**. Deze mogelijkheid biedt gegevenswetenschappers en statistici R programmeurs op aanvraag toegang tot schaalbare, gedistribueerde methoden van analyses in HDInsight.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)]

Op HDInsight R Server biedt de nieuwste mogelijkheden voor analyses op basis van R op gegevenssets van vrijwel elke grootte, in Azure Blob of Data Lake storage geladen. Aangezien R Server-cluster is gebouwd op open source-R, de R-toepassingen die u bouwt gebruik kunnen maken van de 8000 + open source R-pakketten. De routines in ScaleR, van Microsoft big analytics gegevenspakket opgenomen met R Server, zijn ook beschikbaar.

Het edge-knooppunt van een cluster is een handige locatie verbinding maken met het cluster en voor het uitvoeren van uw R-scripts. Met een edge-knooppunt hebt u de optie van de parallelized gedistribueerde functies van ScaleR over de kernen van de edge-knooppunt server wordt uitgevoerd. U kunt ook deze uitvoeren op de knooppunten van het cluster met behulp van ScaleR Hadoop kaart verminderen of Spark compute-contexten.

De modellen of voorspellingen die het resultaat van de analyse kunnen worden gedownload voor intern gebruik. Ze kunnen ook worden geoperationaliseerd elders in Azure, met name via [Azure Machine Learning Studio](http://studio.azureml.net) [webservice](../../machine-learning/studio/publish-a-machine-learning-web-service.md).

## <a name="get-started-with-r-server-on-hdinsight"></a>Aan de slag met R Server in HDInsight

Als u wilt een R Server-cluster in Azure HDInsight maakt, selecteert u de **R Server** cluster type bij het maken van een HDInsight-cluster met de Azure portal. Het type R Server-cluster bevat R Server op de gegevensknooppunten van het cluster en op een edge-knooppunt dat als een zone landingspagina voor analyses op basis van een R Server fungeert. Zie [aan de slag met op HDInsight R Server](r-server-get-started.md) voor een overzicht over het maken van het cluster.

## <a name="why-choose-r-server-in-hdinsight"></a>Waarom kiezen R Server in HDInsight?

R Server in HDInsight biedt de mogelijkheid voor het gebruik van R Server op Azure. R Server omvat:

+ **De beste AI vernieuwing van Microsoft en open-source**

  Microsoft wil AI toegankelijk maken voor elke afzonderlijke en de organisatie. R Server bevat een groot aantal uiterst schaalbare, gedistribueerde set algoritmen, zoals [RevoscaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler), [revoscalepy](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/revoscalepy-package), en [microsoftML](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/microsoftml-package) die op de grootte van meer gegevens kunnen werken dan de grootte van fysiek geheugen en wordt uitgevoerd op een groot aantal verschillende platformen in een gedistribueerde manier. Meer informatie over het verzamelen van Microsoft's aangepaste [R-pakketten](https://docs.microsoft.com/machine-learning-server/r-reference/introducing-r-server-r-package-reference) en [Python-pakketten](https://docs.microsoft.com/machine-learning-server/python-reference/introducing-python-package-reference) die deel uitmaakt van het product.
  
  R Server verbinding van deze Microsoft vernieuwingen en die afkomstig zijn van de open-source-community (toolkits R, Python en AI) alle boven op een enkele bedrijfsniveau-platform. Een R- of Python open-source machine learning-pakket kunt side-by-side werken met een eigen innovatie van Microsoft. 

+ **Eenvoudige, veilige en hoge schaalbaarheid uitoefening en beheer**

  Ondernemingen vertrouwen op traditionele uitoefening paradigma's en omgevingen uiteindelijk investeren veel tijd en moeite naar dit gebied. Pijn verwijst vaak wat resulteert in hoge kosten en vertragingen bevatten: de tijd vertaling voor modellen, iteraties bewaring geldig en actueel, regelgevende goedkeuring, machtigingen via uitoefening beheren.

  R Server biedt de beste eersteklas [uitoefening](https://docs.microsoft.com/machine-learning-server/what-is-operationalization) --vanaf het moment dat een machine learning-model is voltooid, duurt een paar muisklikken webservices-API's te genereren. Deze [webservices](https://docs.microsoft.com/machine-learning-server/operationalize/concept-what-are-web-services) worden gehost op een raster server lokaal of in de cloud en kunnen worden geïntegreerd met line-of-business-toepassingen. De mogelijkheid om te implementeren voor een elastische raster kunt u probleemloos schalen met de behoeften van uw bedrijf, zowel voor batch- en realtime score berekenen. Zie voor instructies [operationeel R Server op HDInsight](r-server-operationalize.md).

+ **Grondige ecosysteem betrokkenheid succes van de klant met optimale totale eigendomskosten**

  Personen die u op het traject van hun toepassingen intelligent maken of gewoon willen leren van de nieuwe wereld van AI en machine learning, moeten de juiste resources om te beginnen. Naast deze documentatie Microsoft biedt diverse learning resources en heeft verschillende trainingpartners kunt u mogelijk uitbreiden en snel productief worden ingeschakeld.


## <a name="key-features-of-r-server-on-hdinsight"></a>Belangrijke functies van op HDInsight R Server

De volgende functies zijn opgenomen in op HDInsight R Server.

| Functie-categorie | Beschrijving |
|------------------|-------------|
| R-ingeschakeld | [R-pakketten](https://docs.microsoft.com/machine-learning-server/r-reference/introducing-r-server-r-package-reference) voor oplossingen die zijn geschreven in R, met een open-source verdeling van R en runtime-infrastructuur voor het uitvoeren van script. |
| Python-ingeschakeld | [Python-modules](https://docs.microsoft.com/machine-learning-server/python-reference/introducing-python-package-reference) voor oplossingen die zijn geschreven in Python, met een open-source verdeling van Python en runtime-infrastructuur voor het uitvoeren van script.  
| [Vooraf getraind modellen](https://docs.microsoft.com/machine-learning-server/install/microsoftml-install-pretrained-models) | Visuele analyse en analyse van tekst gevoel gereed om te beoordelen gegevens u bieden. |
| [Implementeren en gebruiken](r-server-operationalize.md) | Uw server operationeel en oplossingen implementeren als een webservice. |
| [Uitvoering op afstand](r-server-hdinsight-manage.md#connect-remotely-to-microsoft-ml-server-or-client) | Start externe sessies op R Server in uw netwerk van uw clientwerkstation. |


## <a name="data-storage-options-for-r-server-on-hdinsight"></a>Gegevens opslagopties voor R Server op HDInsight

Standaard opslag voor het bestandssysteem HDFS van HDInsight-clusters kan worden gekoppeld aan een Azure Storage-account of een Azure Data Lake Store. Deze koppeling zorgt ervoor dat de gegevens worden geüpload naar het cluster opslag tijdens de analyse persistent is gemaakt en de gegevens zijn beschikbaar, zelfs nadat het cluster wordt verwijderd. Er zijn verschillende hulpmiddelen voor het verwerken van de gegevensoverdracht naar de opslagoptie die u selecteert, met inbegrip van de portal-gebaseerde uploaden faciliteit van het opslagaccount en de [AzCopy](../../storage/common/storage-use-azcopy.md) hulpprogramma.

U hebt de mogelijkheid toegang toe te voegen aan extra Blob en Data lake slaat tijdens het inrichtingsproces ongeacht de primaire opslagoptie gebruikt cluster. Zie [aan de slag met op HDInsight R Server](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-get-started) voor informatie over het toevoegen van toegang tot extra accounts. Zie de aanvullende [Azure Storage-opties voor R Server op HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-storage) artikel voor meer informatie over het gebruik van meerdere opslagaccounts.

U kunt ook [Azure Files](../../storage/files/storage-how-to-use-files-linux.md) als een opslagoptie voor gebruik op de edge-knooppunt. Azure Files kunt u een bestandsshare die is gemaakt in Azure Storage naar het bestandssysteem van Linux te koppelen. Zie voor meer informatie over deze opties voor het opslaan van gegevens voor R Server op HDInsight-cluster [Azure Storage-opties voor R Server op HDInsight](r-server-storage.md).

## <a name="access-machine-learning-server-on-the-cluster"></a>-Machine Learning-Server op het cluster

U kunt verbinding maken met Microsoft Machine Learning-Server op de edge-knooppunt met een browser. Dit is standaard geïnstalleerd tijdens het maken van het cluster. Zie voor meer informatie [stared ophalen met op HDInsight R Server](r-server-get-started.md). U kunt ook verbinding maken met de Server ML vanaf de opdrachtregel met behulp van SSH/PuTTY toegang tot de R-console. 

## <a name="develop-and-run-r-scripts"></a>Ontwikkelen en uitvoeren van scripts R

De R-scripts u maken en uitvoeren kunnen van de 8000 + open source R-pakketten naast de geparallelliseerde en gedistribueerde routines in de bibliotheek ScaleR gebruiken. In het algemeen een script dat wordt uitgevoerd met R Server op de edge-knooppunt uitgevoerd binnen de R-interpreter op dat knooppunt. De uitzonderingen worden die stappen die nodig zijn om aan te roepen, een functie ScaleR met een compute-context die is ingesteld op Hadoop-kaart (RxHadoopMR) verminderen of Spark (RxSpark). De functie wordt in dit geval wordt uitgevoerd in een gedistribueerde manier over die (taak)-gegevensknooppunten van het cluster die gekoppeld aan de gegevens waarnaar wordt verwezen zijn. Zie voor meer informatie over de verschillende rekenscenario context opties [Compute context opties voor R Server op HDInsight](r-server-compute-contexts.md).

## <a name="operationalize-a-model"></a>Een model operationeel maken

Wanneer uw gegevens modelleren voltooid is, kunt u het model om te maken van voorspellingen voor nieuwe gegevens vanuit Azure en on-premises operationeel te maken. Dit proces staat bekend als het score berekenen. Score berekenen kan worden gedaan in HDInsight, Azure Machine Learning of on-premises.

### <a name="score-in-hdinsight"></a>Score in HDInsight
Als u wilt beoordelen in HDInsight, een R-functie die het model voor het maken van voorspellingen voor een nieuw gegevensbestand dat u hebt geladen naar uw opslagaccount aanroept te schrijven. Sla de voorspellingen vervolgens terug naar het opslagaccount. U kunt dit routinematig op aanvraag uitvoeren op de edge-knooppunt van het cluster of met behulp van een geplande taak.

### <a name="score-in-azure-machine-learning-aml"></a>Score in Azure Machine Learning eigen (AML)
Als u wilt beoordelen met Azure Machine Learning, gebruiken de open source Azure Machine Learning-R-pakket bekend als [AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) voor het publiceren van uw model als een Azure-web-service. Dit pakket is voor het gemak vooraf geïnstalleerd zijn op de edge-knooppunt. Vervolgens de faciliteiten in Azure Machine Learning gebruiken voor het maken van een gebruikersinterface voor de webservice en vervolgens de webservice aanroepen naar behoefte voor score berekenen.

Als u deze optie kiest, moet u eventuele modelobjecten ScaleR converteren naar modelobjecten equivalent open source-voor gebruik met de webservice. Gebruik ScaleR afdwingen functies, zoals `as.randomForest()` voor ensemble-modellen, voor deze conversie.

### <a name="score-on-premises"></a>Score lokale
Om te beoordelen op lokale na het maken van uw model, kunt u serialiseren van het model in R, downloaden, de serialisatie van het en vervolgens worden gebruikt voor score berekenen voor nieuwe gegevens. U kunt met behulp van de eerder beschreven in aanpak beoordeling van nieuwe gegevens [scores in HDInsight](#scoring-in-hdinsight) of met behulp van [DeployR](https://deployr.revolutionanalytics.com/).

## <a name="maintain-the-cluster"></a>Het cluster onderhouden

### <a name="install-and-maintain-r-packages"></a>Installeren en onderhouden van R-pakketten

De R-pakketten die u gebruikt de meeste zijn vereist op de edge-knooppunt sinds het meeste werk uw R-scripts uitvoeren er. Als u wilt aanvullende R-pakketten op de edge-knooppunt installeert, kunt u de gebruikelijke `install.packages()` methode in R.

Als u zojuist hebt gebruikt routines uit de bibliotheek ScaleR in het cluster, hoeft niet normaal gesproken u voor het installeren van extra R-pakketten op de gegevensknooppunten. U moet echter extra pakketten voor de ondersteuning van het gebruik van **rxExec** of **RxDataStep** worden uitgevoerd op de gegevensknooppunten.

In dergelijke gevallen kunnen de extra pakketten met een scriptactie worden geïnstalleerd nadat u het cluster hebt gemaakt. Zie voor meer informatie [R Server beheren in HDInsight-cluster](r-server-hdinsight-manage.md).

### <a name="change-hadoop-mapreduce-memory-settings"></a>Hadoop-MapReduce-geheugeninstellingen wijzigen

Een cluster kan worden aangepast voor het wijzigen van de hoeveelheid geheugen die beschikbaar is voor R Server wanneer dit een MapReduce-taak wordt uitgevoerd. Voor het wijzigen van een cluster door de Apache Ambari-gebruikersinterface die beschikbaar is via de Azure portal blade voor uw cluster te gebruiken. Zie voor instructies over toegang tot de Ambari UI voor uw cluster [HDInsight-clusters beheren met de Ambari-Webgebruikersinterface](../hdinsight-hadoop-manage-ambari.md).

Het is ook mogelijk om te wijzigen van de hoeveelheid geheugen die beschikbaar is voor R Server met behulp van Hadoop-switches in de aanroep naar **RxHadoopMR** als volgt:

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>Een cluster schalen

Een bestaand R Server-cluster in HDInsight kan worden uitgebreid omhoog of omlaag via de portal. Met omhoog schalen, krijgt u de extra capaciteit die u voor grotere verwerkingstaken wellicht of u kunt schalen weer een cluster als deze niet actief is. Zie voor instructies over het schalen van een cluster [HDInsight-clusters beheren](../hdinsight-administer-use-portal-linux.md).

### <a name="maintain-the-system"></a>Het systeem onderhouden

Onderhoud OS patches en andere updates toe te passen wordt uitgevoerd op de onderliggende Linux VM's in een HDInsight-cluster buiten kantooruren. Onderhoud wordt gewoonlijk gedaan om 3:30 uur (op basis van de lokale tijd voor de virtuele machine) elke maandag en donderdag. Updates worden zodanig dat ze geen invloed heeft op meer dan een kwartaal van het cluster tegelijkertijd uitgevoerd.  

Aangezien de hoofdknooppunten redundant zijn en niet alle gegevensknooppunten zijn beïnvloed, vertragen alle taken die worden uitgevoerd tijdens deze periode. Ze moeten echter nog steeds uitgevoerd worden voltooid. Alle aangepaste software- of lokale gegevens die u hebt blijft behouden over deze gebeurtenissen onderhoud tenzij er een onherstelbare fout optreedt waarvoor cluster opnieuw opbouwen.

## <a name="ide-options-for-r-server-on-an-hdinsight-cluster"></a>IDE-opties voor R Server op een HDInsight-cluster

Het Linux-edge-knooppunt van een HDInsight-cluster is de zone landingspagina voor op basis van het R-analyse. Recente versies van HDInsight biedt een standaardinstallatie van RStudio Server op de edge-knooppunt als een browser gebaseerde IDE. Gebruik van RStudio Server als een IDE voor de ontwikkeling en uitvoering van scripts R aanzienlijk meer productief zijn dan het gebruik van alleen de R-console.

Bovendien kunt u een bureaublad IDE installeren en gebruiken voor toegang tot het cluster door gebruik te maken van een externe MapReduce of Spark compute-context. Opties zijn onder andere Microsoft [R-Tools voor Visual Studio](https://www.visualstudio.com/features/rtvs-vs.aspx) (RTVS) RStudio, en Walware's op basis van Eclipse [StatET](http://www.walware.de/goto/statet).

Ten slotte kunt u het R-console op de edge-knooppunt openen door te typen **R** achter de opdrachtprompt Linux na verbinding maakt via SSH of PuTY. Wanneer u de console-interface, is het handiger om te een teksteditor voor het ontwikkelen van R-script uitvoeren in een ander venster en knippen en plakken van secties van het script in de console R zo nodig.

## <a name="pricing"></a>Prijzen

De prijzen die gekoppeld aan een HDInsight-cluster met R Server zijn op dezelfde manier gestructureerd op de prijzen voor de standaard HDInsight-clusters. Ze zijn gebaseerd op de grootte van de onderliggende virtuele machines in de naam, de gegevens en de edge-knooppunten, met de toevoeging van een opwaartse core uur. Zie voor meer informatie over prijzen voor HDInsight [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie over het gebruik van R Server op HDInsight-clusters:

* [Aan de slag met R Server-cluster in HDInsight](r-server-get-started.md)
* [Opties voor compute-context voor R Server-cluster in HDInsight](r-server-compute-contexts.md)
* [Opties voor Azure Storage voor R Server-cluster in HDInsight](r-server-storage.md)
