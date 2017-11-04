---
title: Platforms en hulpprogramma's voor gegevenswetenschap projecten - Azure in een team | Microsoft Docs
description: Itemizes en worden de gegevens en analyse bronnen beschikbaar zijn voor ondernemingen van het Team gegevens wetenschap proces standaardiseren besproken.
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2017
ms.author: bradsev;
ms.openlocfilehash: 3ec2eaaf4e8d54e7b1ea3d272c47eac96451f317
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="platforms-and-tools-for-data-science-team-projects"></a>Platforms en hulpprogramma's voor gegevenswetenschap projecten in een team

Microsoft biedt een volledige scala aan gegevens en analytics-services en bronnen voor cloud- of on-premises platforms. Ze kunnen worden geïmplementeerd om de uitvoering van uw gegevens wetenschappelijke projecten efficiënter en schaalbare. Richtlijnen voor het implementeren van gegevens wetenschappelijke projecten in een herleidbare teams versie beheerd en gezamenlijke manier wordt geleverd door de [Team gegevens wetenschap proces](overview.md) (TDSP).  Zie voor een overzicht van de rollen personeel en de bijbehorende taken die worden verwerkt door een gegevens wetenschappelijke team standaardiseren op dit proces [Team gegevens wetenschap proces rollen en taken](roles-tasks.md).

De gegevens en analyse services beschikbaar zijn voor gegevens wetenschappelijke teams met behulp van de TDSP zijn onder andere:

- Gegevens wetenschappelijke virtuele Machines (Windows en Linux-CentOS)
- HDInsight Spark-Clusters
- SQL Data Warehouse
- Azure Data Lake
- HDInsight Hive-Clusters
- Azure File Storage
- SQL Server 2016 R-Services

In dit document, we kort wordt beschreven welke resources en bevatten koppelingen naar de zelfstudies en scenario's, de teams TDSP hebt gepubliceerd. Ze kunt u informatie over het gebruik van deze stap voor stap en start u deze voor het bouwen van uw intelligente toepassingen. Meer informatie over deze resources is beschikbaar op de product-pagina's. 

## <a name="data-science-virtual-machine-dsvm"></a>Gegevens wetenschappelijke virtuele Machine (DSVM)

De virtuele machine van de gegevens wetenschappelijke aangeboden op Windows- en Linux door Microsoft, populaire hulpprogramma's voor het model- en -activiteiten van wetenschappelijke gegevens bevat. Het bevat hulpprogramma's zoals:

- Microsoft R Server Developer Edition 
- Anaconda Python-distributie
- Jupyter-notebooks voor Python en R 
- Visual Studio Community Edition met Python en R-hulpprogramma's op Windows / Eclipse op Linux
- Power BI desktop voor Windows
- Developer Edition van SQL Server 2016 op Windows / Postgres op Linux

Dit omvat ook **hulpprogramma's voor ML en AI** , zoals CNTK (een Open Source grondige Learning toolkit van Microsoft), xgboost, mxnet en Vowpal Wabbit.

DSVM is momenteel beschikbaar in **Windows** en **CentOS Linux** besturingssystemen. Kies de grootte van uw DSVM (aantal CPU-kernen) en de hoeveelheid geheugen op basis van de behoeften van de gegevens wetenschap-projecten die u van plan bent om uit te voeren op het. 

Zie voor meer informatie over Windows-editie van DSVM [Microsoft Data wetenschappelijke Virtual Machine](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) op de Azure marketplace. Zie voor de Linux-versie van de DSVM, [Linux gegevens wetenschappelijke virtuele Machine](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/).

Zie voor meer informatie over de algemene gegevens wetenschappelijke taken efficiënt uitvoeren op de DSVM, [tien dingen die u op de virtuele Machine voor gegevenswetenschap doen kunt](../data-science-virtual-machine/vm-do-ten-things.md)


## <a name="azure-hdinsight-spark-clusters"></a>Azure HDInsight Spark-clusters

Apache Spark is een open source parallelle verwerking die ondersteuning biedt voor in-memory verwerking om zo de prestaties van analytische big data-toepassingen. De Spark-verwerkingsengine is gebouwd voor snelheid, gebruiksgemak en geavanceerde analyses. In het geheugen-rekencapaciteiten van Spark maken het een goede keuze voor zich herhalende algoritmen in machine learning en berekeningen van de grafiek. Spark is ook compatibel met Azure Blob storage (WASB), zodat uw bestaande gegevens opgeslagen in Azure, gemakkelijk kunnen worden verwerkt met behulp van Spark.

Wanneer u in HDInsight een Spark-cluster maakt, maakt u rekenresources in Azure, waarbij Spark is geïnstalleerd en geconfigureerd. Het duurt ongeveer 10 minuten voor het maken van een Spark-cluster in HDInsight. Opslaan van de gegevens moeten worden verwerkt in Azure Blob-opslag. Zie voor meer informatie over het gebruik van Azure Blob-opslag met een cluster [HDFS-compatibele Azure Blob storage gebruiken met Hadoop in HDInsight](../../hdinsight/hdinsight-hadoop-use-blob-storage.md).

TDSP-team van Microsoft die beschikbaar heeft gesteld twee end-to-end-scenario's voor het gebruik van Azure HDInsight Spark-Clusters voor het bouwen van oplossingen voor wetenschap, één met behulp van Python en de andere Scala. Voor meer informatie over Azure HDInsight **Spark-Clusters**, Zie [overzicht: Apache Spark in HDInsight Linux](../../hdinsight/spark/apache-spark-overview.md). Voor informatie over het bouwen van een data wetenschappelijke oplossing met **Python** op een Azure HDInsight Spark-Cluster Zie [overzicht van Gegevenswetenschap met Spark op Azure HDInsight](spark-overview.md). Voor informatie over het bouwen van een data wetenschappelijke oplossing met **Scala** op een Azure HDInsight Spark-Cluster Zie [Gegevenswetenschap met behulp van Scala en Spark op Azure](scala-walkthrough.md). 


##  <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse

Azure SQL Data Warehouse kunt u rekenresources geschaald uitbreiden eenvoudig en in seconden, zonder overprovisioning of te veel betaalt. Biedt ook de unieke optie voor het onderbreken van het gebruik van rekenresources, zodat u de vrijheid om de kosten van uw cloud beter te beheren. De mogelijkheid voor het implementeren van schaalbare rekenresources maakt het mogelijk om al uw gegevens in Azure SQL Data Warehouse. Opslagkosten zijn minimaal en u kunt compute alleen uitvoeren op de onderdelen van gegevenssets die u wilt analyseren. 

Zie voor meer informatie over Azure SQL Data Warehouse de [SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse) website. Zie voor meer informatie over het bouwen van geavanceerde analyses van end-to-end-oplossingen met SQL Data Warehouse, [het Team gegevens wetenschappelijke processen in actie: met behulp van SQL Data Warehouse](sqldw-walkthrough.md).


## <a name="azure-data-lake"></a>Azure Data Lake

Er is een Azure data lake als een ondernemingsbrede opslagplaats van elk type gegevens verzameld in één locatie, voordat de formele behoeften of het schema wordt opgelegd. Deze flexibiliteit kan elk type gegevens worden bewaard in een data lake, ongeacht de grootte of de structuur of hoe snel het wordt ingenomen. Organisaties kunnen vervolgens gebruiken voor Hadoop of patronen voor geavanceerde analyses te vinden in deze gegevens meren. Gegevens meren kunnen ook fungeren als een opslagplaats voor lagere kosten gegevens voorbereiden voordat u inrichting van de gegevens en u deze verplaatst naar een datawarehouse.

Zie voor meer informatie over Azure Data Lake [Introducing Azure Data Lake](https://azure.microsoft.com/blog/introducing-azure-data-lake/). Zie voor meer informatie over het bouwen van een oplossing van de wetenschap schaalbare end-to-end-gegevens met Azure Data Lake, [schaalbare Gegevenswetenschap in Azure Data Lake: een end-to-end-overzicht](data-lake-walkthrough.md)


## <a name="azure-hdinsight-hive-hadoop-clusters"></a>Azure HDInsight Hive (Hadoop)-clusters

Apache Hive is een systeem voor het datawarehouse van gegevens voor Hadoop, waardoor gegevenssamenvatting, query's en de analyse van gegevens met behulp van HiveQL, een querytaal die vergelijkbaar is met SQL. Hive kan worden gebruikt om interactief Verken uw gegevens of herbruikbare batch taken verwerkt.

Hive kunt u de projectstructuur van het op grotendeels ongestructureerde gegevens. Nadat u de structuur gedefinieerd, kunt u Hive query die gegevens in een Hadoop-cluster zonder dat wilt gebruiken, of zelfs weet, Java of MapReduce. HiveQL (Hive query language) kunt u query's met die vergelijkbaar met T-SQL zijn-instructies schrijven.

Voor gegevenswetenschappers, kan Hive Python User-Defined functies (UDF's) in de Hive-query's voor het verwerken van records uitgevoerd. Deze mogelijkheid breidt de mogelijkheden van Hive-query's in data-analyse aanzienlijk. In het bijzonder kunt gegevenswetenschappers voor het uitvoeren van schaalbare functie-engineering in talen vooral bekend zijn met zijn: de SQL-achtige HiveQL en Python. 

Zie voor meer informatie over Azure HDInsight Hive-Clusters [Hive en HiveQL met Hadoop in HDInsight gebruiken](../../hdinsight/hadoop/hdinsight-use-hive.md). Zie voor meer informatie over het bouwen van een oplossing van de wetenschap schaalbare end-to-end-gegevens met Azure HDInsight Hive-Clusters, [het Team gegevens wetenschappelijke processen in actie: met behulp van HDInsight Hadoop-clusters](hive-walkthrough.md).


## <a name="azure-file-storage"></a>Azure File Storage 

Azure File Storage is een service die bestandsshares in de cloud met behulp van de standaard Server Message Block (SMB)-Protocol biedt. Zowel SMB 2.1 als SMB 3.0 wordt ondersteund. Met Azure File Storage kunt u oudere toepassingen die afhankelijk zijn van bestandsshares, snel en zonder kostbare regeneraties naar Azure migreren. Toepassingen die worden uitgevoerd op virtuele machines of in cloudservices van Azure, of op on-premises clients, kunnen een bestandsshare in de cloud koppelen, op dezelfde manier zoals een bureaubladtoepassing een gewone SMB-share koppelt. Daarna kan een willekeurig aantal toepassingsonderdelen de File Storage-share tegelijkertijd koppelen en gebruiken.

Met name handig voor gegevens wetenschappelijke projecten is de mogelijkheid een archief voor Azure maken als de project-gegevens delen met uw Projectteamleden. Elk van deze vervolgens heeft toegang tot hetzelfde exemplaar van de gegevens in de Azure file storage. Ze kunnen ook de opslag van deze bestanden gebruiken om te delen functiesets gegenereerd tijdens het uitvoeren van het project. Als het project een client engagement is, kan uw clients een Azure-bestandsopslag onder de project-gegevens en -functies met u te delen hun eigen Azure-abonnement kunnen maken. Op deze manier heeft de client volledig beheer van het project gegevensassets. Zie voor meer informatie over Azure File Storage [aan de slag met Azure File storage in Windows](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-files) en [Azure File Storage gebruiken met Linux](../../storage/files/storage-how-to-use-files-linux.md).


## <a name="sql-server-2016-r-services"></a>SQL Server 2016 R-Services

R-Services (In database) bieden een platform voor het ontwikkelen en implementeren van intelligent toepassingen die u kunnen nieuwe insights onthullen. De uitgebreide en krachtige R taal, met inbegrip van de vele pakketten geleverd door de community R, kunt u modellen maken en de voorspellingen van uw SQL Server-gegevens worden gegenereerd. Omdat de taal R R-Services (In database) geïntegreerd met SQL Server, worden analytics dicht bij de gegevens, zodat de kosten en beveiligingsrisico's die zijn gekoppeld aan het verplaatsen van gegevens bewaard.

R-Services (In database) ondersteuning voor de taal van de open-source R met een uitgebreide set hulpprogramma's voor SQL Server en -technologieën. Ze bieden hogere prestaties, beveiliging, betrouwbaarheid en beheermogelijkheden. U kunt implementeren met behulp van hulpprogramma's voor handige en bekend R-oplossingen. Uw productietoepassingen kunnen aanroepen van de R-runtime en ophalen van voorspellingen en visuele elementen met Transact-SQL. U kunt ook de bibliotheken ScaleR gebruiken voor het verbeteren van de schaal en prestaties van uw R-oplossingen. Zie voor meer informatie [SQL Server-R-Services](https://msdn.microsoft.com/library/mt604845.aspx)

Het TDSP-team van Microsoft heeft twee end-to-end-scenario's die laten zien hoe u oplossingen voor wetenschap in SQL Server 2016 R Services gepubliceerd: één voor programmeurs R en één voor SQL-ontwikkelaars. Voor **R programmeurs**, Zie [Gegevenswetenschap End-to-End-overzicht](https://msdn.microsoft.com/library/mt612857.aspx). Voor **SQL ontwikkelaars**, Zie [In Database Advanced Analytics voor SQL-ontwikkelaars (zelfstudie)](https://msdn.microsoft.com/library/mt683480.aspx).


## <a name="appendix"></a>Bijlage: Hulpprogramma's voor het instellen van gegevens wetenschappelijke projecten

### <a name="install-git-credential-manager-on-windows"></a>Installeer Git Referentiebeheer in Windows

Als u de TDSP volgt op **Windows**, moet u voor het installeren van de **Git referentie Manager (GCM)** om te communiceren met de Git-opslagplaatsen. Als u wilt installeren GCM, moet u eerst installeren **Chocolaty**. Als u wilt Chocolaty en de GCM hebt geïnstalleerd, kunt u de volgende opdrachten uitvoeren in Windows PowerShell als een **beheerder**:  

    iwr https://chocolatey.org/install.ps1 -UseBasicParsing | iex
    choco install git-credential-manager-for-windows -y
    

### <a name="install-git-on-linux-centos-machines"></a>Installeer Git op machines met Linux (CentOS)

Voer de volgende opdracht bash Git installeren op computers met Linux (CentOS):

    sudo yum install git


### <a name="generate-public-ssh-key-on-linux-centos-machines"></a>Openbare SSH-sleutel op Linux (CentOS)-machines genereren

Als u van Linux (CentOS)-machines gebruikmaakt de git-opdrachten uit te voeren, moet u de openbare SSH-sleutel van uw machine toevoegen aan uw server VSTS, zodat deze machine wordt herkend door de server VSTS. Eerst moet u een openbare SSH-sleutel genereren en de sleutel toevoegen aan de openbare SSH-sleutels in uw pagina VSTS beveiliging instelling. 

- Voer de volgende twee opdrachten voor het genereren van de SSH-sleutel: 

        ssh-keygen
        cat .ssh/id_rsa.pub

![](./media/platforms-and-tools/resources-1-generate_ssh.png)

- Kopieer de gehele ssh sleutel inclusief *ssh-rsa*. 
- Aanmelden bij uw VSTS-server. 
- Klik op **< naam van uw\>**  in de rechterbovenhoek van de pagina en klik op **beveiliging**. 
    
    ![](./media/platforms-and-tools/resources-2-user-setting.png)

- Klik op **openbare SSH-sleutels**, en klik op **+ toevoegen**. 

    ![](./media/platforms-and-tools/resources-3-add-ssh.png)

- Plak de ssh sleutel hebt gekopieerd in het tekstvak en op te slaan.


## <a name="next-steps"></a>Volgende stappen

Volledige end-to-end-scenario's die de stappen in het proces voor het demonstreren **specifieke scenario's** worden ook gegeven. Ze worden weergegeven en gekoppeld aan de miniatuur beschrijvingen in de [voorbeeld-scenario's](walkthroughs.md) onderwerp. Ze te laten zien hoe cloud, 's voor on-premises en -services combineren in een werkstroom of pijplijn een intelligente toepassing maken. 

Zie voor voorbeelden van stappen uitvoeren in het Team gegevens wetenschappelijke processen die gebruikmaken van Azure Machine Learning Studio de [met Azure ML](http://aka.ms/datascienceprocess) leertraject.