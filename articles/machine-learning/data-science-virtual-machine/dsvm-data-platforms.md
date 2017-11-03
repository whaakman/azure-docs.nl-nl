---
title: Gegevens platforms voor de gegevens wetenschappelijke virtuele Machine, Azure | Microsoft Docs
description: Gegevens platforms voor gegevens wetenschappelijke virtuele Machine.
keywords: hulpmiddelen voor wetenschappelijke gegevens, gegevens wetenschappelijke virtuele machine, hulpprogramma's voor gegevenswetenschap, gegevenswetenschap linux
services: machine-learning
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
ms.date: 09/11/2017
ms.author: gokuma;bradsev
ms.openlocfilehash: 9b8beb51c555c6125fa3b0abbad892d627a180b9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="data-platforms"></a>Gegevensplatforms

De gegevens wetenschappelijke virtuele Machine (DSVM) kunt u voor het bouwen van uw analyses op basis van een breed scala aan gegevens platforms. Naast de interfaces op externe gegevens platforms biedt de DSVM een lokaal exemplaar snelle ontwikkeling en het maken van een prototype. 

Hier volgen de op de DSVM ondersteund platform hulpprogramma's. 

## <a name="sql-server-2016-developer-edition"></a>Developer Edition van SQL Server 2016

| | |
| ------------- | ------------- |
| Wat is het?   | Een lokale relationele database-exemplaar      |
| Ondersteunde DSVM edities      | Windows      |
| Doorgaans worden gebruikt      | Snelle ontwikkeling lokaal met kleinere gegevensset <br/> R-database uitvoeren   |
| Koppelingen naar voorbeelden      |    Een klein aantal New York City gegevensset in de SQL-database wordt geladen `nyctaxi`. <br/> Jupyter-voorbeeld toont Microsoft R- en analyses in de database kan worden gevonden op:<br/> `~notebooks/SQL_R_Services_End_to_End_Tutorial.ipynb`  |
| Bijbehorende hulpprogramma's op de DSVM       | SQL Server Management Studio <br/> ODBC/JDBC-stuurprogramma 's<br/> pyodbc, RODBC<br />Apache Inzoomen      |

> [!NOTE]
> De ontwikkelaarsversie van SQL Server 2016 kan alleen worden gebruikt voor ontwikkeling en testdoeleinden. U moet een licentie of een van de SQL Server-VM's uit te voeren in de productieomgeving. 


### <a name="setup"></a>Instellen

De database-server is al vooraf is geconfigureerd en de Windows-services met betrekking tot SQL Server (zoals `SQL Server (MSSQLSERVER)`) zijn ingesteld op automatisch wordt uitgevoerd. De alleen handmatige stap moet worden uitgevoerd is het inschakelen In database-analyses met behulp van Microsoft R. U kunt dit doen door met de volgende opdracht als een eenmalige actie in SQL Server Management Studio (SSMS) na logboekregistratie in als de beheerder van de machine een 'nieuwe Query"openen in SSMS, zorg ervoor dat de geselecteerde database `master` en voer vervolgens: 

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 

        (Please replace the %COMPUTERNAME% with your VM name)
       
U kunt om te kunnen uitvoeren van SQL Server Management Studio, 'SQL Server Management Studio' zoeken op de lijst met programma's of Windows Search gebruiken om te zoeken en voer deze uit. Wanneer u wordt gevraagd om referenties, kies Windows-verificatie' en gebruik de computernaam of ```localhost``` in de naam van SQL Server. 

### <a name="how-to-use--run-it"></a>Hoe gebruik / uitvoeren?  

De database-server met het standaardexemplaar van de database wordt standaard automatisch uitgevoerd. U kunt hulpprogramma's zoals SQL Server Management Studio op de virtuele machine gebruiken voor toegang tot de SQL Server-database lokaal. Lokale Administrator-account hebben beheerderstoegang op de database. 

Ook de DSVM wordt geleverd met ODBC-stuurprogramma's en JDBC-stuurprogramma's voor communicatie met SQL Server, Azure SQL-databases en Azure SQL Data Warehouse van toepassingen die zijn geschreven in meerdere talen, waaronder Python, R. 

### <a name="how-is-it-configured--installed-on-the-dsvm"></a>Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM? 

De SQL-Server is geïnstalleerd op de gebruikelijke manier. Kan worden gevonden op `C:\Program Files\Microsoft SQL Server`. De R In database-instantie wordt gevonden op `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES`. De DSVM heeft ook een afzonderlijk zelfstandig R Server-exemplaar geïnstalleerd op `C:\Program Files\Microsoft\R Server\R_SERVER`. Deze twee-R-exemplaren delen de bibliotheken niet.


## <a name="apache-spark-2x-standalone"></a>Apache Spark 2.x (zelfstandig)

| | |
| ------------- | ------------- |
| Wat is het?   | Een zelfstandig (één knooppunt in-process)-exemplaar van het populaire Apache Spark-platform, een systeem voor het snel grootschalige gegevensverwerking en machine learning     |
| Ondersteunde DSVM edities      | Linux <br /> Windows (experimentele)      |
| Doorgaans worden gebruikt      | * Snelle ontwikkeling van Spark/PySpark toepassingen lokaal met kleinere gegevensset en hoger implementeert op grote Spark-clusters zoals Azure HDInsight<br/> * Test Microsoft R Server Spark-Context <br />* SparkML of de Microsoft open-source gebruiken [MMLSpark](https://github.com/Azure/mmlspark) bibliotheek ML-toepassingen te bouwen  |
| Koppelingen naar voorbeelden      |    Jupyter-voorbeeld: <br />&nbsp;&nbsp;* ~/notebooks/SparkML/pySpark <br /> &nbsp;&nbsp;* ~/notebooks/MMLSpark <br /> Microsoft R Server (Spark Context): /dsvm/samples/MRS/MRSSparkContextSample.R |
| Bijbehorende hulpprogramma's op de DSVM       | PySpark, Scala<br/>Jupyter (Spark/PySpark Kernels)<br/>Microsoft R Server, SparkR, Sparklyr <br />Apache Inzoomen      |

### <a name="how-to-use-it"></a>Het gebruik ervan
U kunt Spark uitvoeren door het verzenden van taken op de opdrachtregel met Spark `spark-submit` of `pyspark` opdrachten. U kunt ook een Jupyter-notebook maken door een nieuwe notebook maken met de kernel Spark. 

U kunt Spark van R bibliotheken zoals SparkR, Sparklyr of Microsoft R Server die beschikbaar op de DSVM zijn gebruiken. Zie verwijzingen naar de voorbeelden in de voorgaande tabel. 

> [!NOTE]
> Met Microsoft R Server in de context van DSVM Spark wordt alleen ondersteund voor de editie Ubuntu Linux DSVM. 



### <a name="setup"></a>Instellen
Voordat u in de context van Spark in Microsoft R Server op Ubuntu Linux DSVM editie, moet u een eenmaal setup stap om in te schakelen lokale één knooppunt Hadoop HDFS en Yarn-exemplaar te doen. Hadoop-services zijn standaard geïnstalleerd maar uitgeschakeld op de DSVM. Als u wilt inschakelen, moet u de volgende opdrachten uitvoeren als basis voor het eerst:

    echo -e 'y\n' | ssh-keygen -t rsa -P '' -f ~hadoop/.ssh/id_rsa
    cat ~hadoop/.ssh/id_rsa.pub >> ~hadoop/.ssh/authorized_keys
    chmod 0600 ~hadoop/.ssh/authorized_keys
    chown hadoop:hadoop ~hadoop/.ssh/id_rsa
    chown hadoop:hadoop ~hadoop/.ssh/id_rsa.pub
    chown hadoop:hadoop ~hadoop/.ssh/authorized_keys
    systemctl start hadoop-namenode hadoop-datanode hadoop-yarn

U kunt de Hadoop stoppen gerelateerde services wanneer u deze niet nodig hebt door te voeren ````systemctl stop hadoop-namenode hadoop-datanode hadoop-yarn```` een voorbeeld laat zien hoe te ontwikkelen en testen van MEVR in externe Spark-context (dit is het zelfstandige Spark-exemplaar op de DSVM) is opgegeven en beschikbaar in de `/dsvm/samples/MRS` de map. 


### <a name="how-is-it-configured--installed-on-the-dsvm"></a>Hoe wordt deze geconfigureerd / geïnstalleerd op de DSVM? 
|Platform|Installatielocatie ($SPARK_HOME)|
|:--------|:--------|
|Windows | c:\dsvm\tools\spark-X.X.X-bin-hadoopX.X|
|Linux   | /dsvm/Tools/Spark-X.X.X-bin-hadoopX.X|


Bibliotheken voor toegang tot gegevens uit Azure Blob of Azure Data Lake storage (ADLS) en met behulp van Microsoft MMLSpark machine learning-bibliotheken zijn in $SPARK_HOME/potten vooraf. Deze potten worden automatisch geladen wanneer Spark wordt gestart. Spark maakt standaard gebruik van gegevens op de lokale schijf. Om het Spark-exemplaar op de DSVM voor toegang tot gegevens die zijn opgeslagen op Azure-blob of ADLS moet u maken/configureren de `core-site.xml` bestand op basis van sjabloon is gevonden in $SPARK_HOME/conf/core-site.xml.template (indien er tijdelijke aanduidingen voor Blob en ADLS zijn configuraties) met de juiste referenties naar Azure blob- en Azure Data Lake Storage. U vindt meer gedetailleerde stappen voor het maken van de referenties van de service ADLS [hier](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-authenticate-using-active-directory#create-an-active-directory-application). Als de referenties voor Azure blob of ADLS worden ingevoerd in het bestand core site.xml, kunt u verwijzen naar gegevens die zijn opgeslagen in de bronnen met het voorvoegsel van de URI van wasb: / / of adl: / /. 

