---
title: Scenario's voor geavanceerde analyses bepalen voor Azure Machine Learning | Microsoft Docs
description: Selecteer de juiste scenario's voor geavanceerde voorspellende analyses met het Team gegevens wetenschap proces doen.
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 53aecc1e-5089-42cf-8d44-77678653f92d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: bradsev
ms.openlocfilehash: 3b6a92f4f4615954902124c59adca25560182de6
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2017
---
# <a name="scenarios-for-advanced-analytics-in-azure-machine-learning"></a>Scenario's voor geavanceerde analyses in Azure Machine Learning
In dit artikel bevat een overzicht van de verschillende gegevensbronnen voorbeeld en doel-scenario's die kunnen worden verwerkt door de [Team gegevens wetenschap proces (TDSP)](overview.md). De TDSP biedt een systematische aanpak voor teams die samenwerken aan intelligente toepassingen bouwen. De scenario's die hier wordt gepresenteerd illustreren beschikbare opties in de werkstroom gegevensverwerking die afhankelijk van de gegevenskenmerken, bronlocaties en doel-opslagplaatsen in Azure zijn.

De **beslissingsstructuur** voor het selecteren van de voorbeeldscenario's die geschikt is voor uw gegevens en het doel is opgenomen in de laatste sectie.

> [!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]
> 
> 

Elk van de volgende secties geeft een voorbeeldscenario. Voor elk scenario, een mogelijke gegevenswetenschap of geavanceerde analyses stroom en de ondersteunende Azure-resources die worden vermeld.

> [!NOTE]
> **Voor alle van de volgende scenario's moet u:**
> <br/>
> 
> * [Een opslagaccount maken](../../storage/common/storage-create-storage-account.md)
>   <br/>
> * [Een Azure Machine Learning-werkruimte maken](../studio/create-workspace.md)
> 
> 

## <a name="smalllocal"></a>Scenario \#1: kleine tot middelgrote in tabelvorm gegevensset in een lokale bestanden
![Kleine tot middelgrote lokale bestanden][1]

#### <a name="additional-azure-resources-none"></a>Extra Azure-resources: geen
1. Aanmelden bij de [Azure Machine Learning Studio](https://studio.azureml.net/).
2. Upload een dataset.
3. Maak een Azure Machine Learning-experiment stroom beginnen met geüploade gegevensset (s).

## <a name="smalllocalprocess"></a>Scenario \#2: kleine tot middelgrote gegevensset van lokale bestanden die moeten worden verwerkt
![Kleine tot middelgrote lokale bestanden met de verwerking][2]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Extra Azure-resources: Azure virtuele Machine (IPython Notebook server)
1. Maak een virtuele Machine van Azure IPython Notebook uitgevoerd.
2. Gegevens uploaden naar een Azure storage-container.
3. Vooraf verwerken en de gegevens in IPython laptop, toegang tot gegevens uit Azure storage-container opschonen.
4. Transformeer gegevens gereinigd, in tabelvorm.
5. Getransformeerde gegevens opslaan in Azure blobs.
6. Aanmelden bij de [Azure Machine Learning Studio](https://studio.azureml.net/).
7. Leest de gegevens uit Azure blobs met de [importgegevens] [ import-data] module.
8. Maak een Azure Machine Learning-experiment stroom vanaf opgenomen gegevensset (s).

## <a name="largelocal"></a>Scenario \#3: grote gegevensset van lokale bestanden, die gericht is op Azure Blobs
![Grote lokale bestanden][3]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Extra Azure-resources: Azure virtuele Machine (IPython Notebook server)
1. Maak een virtuele Machine van Azure IPython Notebook uitgevoerd.
2. Gegevens uploaden naar een Azure storage-container.
3. Vooraf verwerken en de gegevens in IPython laptop, toegang tot gegevens uit Azure blobs opschonen.
4. Transformeer gegevens gereinigd, in tabelvorm, indien nodig.
5. Gegevens verkennen en functies maken indien nodig.
6. Een voorbeeld van kleine tot middelgrote gegevens extraheren.
7. Sla de steekproefgegevens in Azure blobs.
8. Aanmelden bij de [Azure Machine Learning Studio](https://studio.azureml.net/).
9. Leest de gegevens uit Azure blobs met de [importgegevens] [ import-data] module.
10. Azure Machine Learning-experiment stroom vanaf opgenomen kolomovereenkomst bouwen.

## <a name="smalllocaltodb"></a>Scenario \#4: klein middelgrote DataSet van lokale bestanden, die gericht is op SQL Server in een virtuele Machine van Azure
![Kleine tot middelgrote lokale bestanden aan de SQL-database in Azure][4]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Extra Azure-resources: Azure virtuele Machine (SQL Server / IPython Notebook server)
1. Maak een virtuele Machine van Azure SQL-Server + IPython Notebook uitgevoerd.
2. Gegevens uploaden naar een Azure storage-container.
3. Vooraf verwerken en de gegevens in Azure storage-container met IPython Notebook opschonen.
4. Transformeer gegevens gereinigd, in tabelvorm, indien nodig.
5. Gegevens opslaan naar VM-local-bestanden (IPython Notebook wordt uitgevoerd op virtuele machine, lokale stations verwijzen naar VM-schijven).
6. Laden van gegevens naar SQL Server-database op een Azure VM.
   
   Optie \#1: met SQL Server Management Studio.
   
   * Aanmelden bij SQL Server VM
   * Voer SQL Server Management Studio.
   * Database- en doel-tabellen maken.
   * Gebruik een van de bulksgewijs importeren methoden voor het laden van de gegevens van VM-lokale bestanden.
   
   Optie \#2: met behulp van IPython Notebook – niet aanbevolen voor middelgrote en grote gegevenssets
   
   <!-- -->    
   * ODBC-verbindingsreeks gebruiken voor toegang tot SQL Server op virtuele machine.
   * Database- en doel-tabellen maken.
   * Gebruik een van de bulksgewijs importeren methoden voor het laden van de gegevens van VM-lokale bestanden.
7. Gegevens verkennen, functies maken indien nodig. Houd er rekening mee dat de functies niet hoeft te worden gematerialiseerd in de databasetabellen. Opmerking alleen de benodigde query om deze te maken.
8. Bepaal op een steekproefomvang gegevens als nodig is en/of gewenst.
9. Aanmelden bij de [Azure Machine Learning Studio](https://studio.azureml.net/).
10. Lezen van de gegevens rechtstreeks vanuit de SQL Server met de [importgegevens] [ import-data] module. Plak de benodigde query velden extraheert en maakt functies en voorbeelden van gegevens, indien nodig rechtstreeks in de [importgegevens] [ import-data] query.
11. Azure Machine Learning-experiment stroom vanaf opgenomen kolomovereenkomst bouwen.

## <a name="largelocaltodb"></a>Scenario \#5: grote gegevensset in een lokale bestanden doel-SQL-Server in Azure VM
![Grote lokale bestanden aan de SQL-database in Azure][5]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Extra Azure-resources: Azure virtuele Machine (SQL Server / IPython Notebook server)
1. Een Azure-virtuele Machine met SQL Server en IPython Notebook server maken.
2. Gegevens uploaden naar een Azure storage-container.
3. (Optioneel) Vooraf verwerken en gegevens opgeschoond.
   
   a.  Vooraf verwerken en de gegevens in IPython laptop, toegang tot gegevens uit Azure opschonen
   
       blobs.
   
   b.  Transformeer gegevens gereinigd, in tabelvorm, indien nodig.
   
   c.  Gegevens opslaan naar VM-local-bestanden (IPython Notebook wordt uitgevoerd op virtuele machine, lokale stations verwijzen naar VM-schijven).
4. Laden van gegevens naar SQL Server-database op een Azure VM.
   
   a.  Aanmelden bij SQL Server-machine.
   
   b.  Als gegevens niet al opgeslagen, downloaden van gegevensbestanden van Azure
   
       storage container to local-VM folder.
   
   c.  Voer SQL Server Management Studio.
   
   d.  Database- en doel-tabellen maken.
   
   e.  Gebruik een van de bulksgewijs importeren methoden om de gegevens te laden.
   
   f.  Als tabelsamenvoegingen nodig zijn, worden de indexen joins sneller maken.
   
   > [!NOTE]
   > Voor sneller laden van grote hoeveelheden gegevens grootten verdient het aanbeveling dat u gepartitioneerde tabellen maken en grote hoeveelheden gegevens parallel importeren. Zie voor meer informatie [parallelle gegevens importeren in SQL-tabellen gepartitioneerd](parallel-load-sql-partitioned-tables.md).
   > 
   > 
5. Gegevens verkennen, functies maken indien nodig. Houd er rekening mee dat de functies niet hoeft te worden gematerialiseerd in de databasetabellen. Opmerking alleen de benodigde query om deze te maken.
6. Bepaal op een steekproefomvang gegevens als nodig is en/of gewenst.
7. Aanmelden bij de [Azure Machine Learning Studio](https://studio.azureml.net/).
8. Lezen van de gegevens rechtstreeks vanuit de SQL Server met de [importgegevens] [ import-data] module. Plak de benodigde query velden extraheert en maakt functies en voorbeelden van gegevens, indien nodig rechtstreeks in de [importgegevens] [ import-data] query.
9. Eenvoudige Azure Machine Learning-experiment stroom beginnen met geüploade gegevensset

## <a name="largedbtodb"></a>Scenario \#6: grote gegevensset in een SQL Server-database on-premises, die gericht is op SQL Server in een virtuele Machine van Azure
![Grote SQL DB on-premises aan de SQL-database in Azure][6]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Extra Azure-resources: Azure virtuele Machine (SQL Server / IPython Notebook server)
1. Een Azure-virtuele Machine met SQL Server en IPython Notebook server maken.
2. Gebruik een van de gegevens exporteren methoden voor het exporteren van de gegevens uit SQL Server naar dumpbestanden.
   
   > [!NOTE]
   > Als u besluit alle gegevens te verplaatsen uit de on-premises database, een alternatieve (sneller) methode de volledige database verplaatsen naar de SQL Server-exemplaar in Azure. De stappen voor het exporteren van gegevens, database maken en belasting/importeren van gegevens naar de doeldatabase en volgt u de alternatieve methode overslaan.
   > 
   > 
3. Dumpbestanden uploaden naar Azure storage-container.
4. Laden van de gegevens naar een SQL Server-database op een virtuele Machine van Azure.
   
   a.  Meld u aan met de SQL Server VM.
   
   b.  Het downloaden van gegevensbestanden van een Azure storage-container naar de map local-VM.
   
   c.  Voer SQL Server Management Studio.
   
   d.  Database- en doel-tabellen maken.
   
   e.  Gebruik een van de bulksgewijs importeren methoden om de gegevens te laden.
   
   f.  Als tabelsamenvoegingen nodig zijn, worden de indexen joins sneller maken.
   
   > [!NOTE]
   > Importeer gegevens parallel voor sneller laden van grote hoeveelheden gegevens grootte en gepartitioneerde tabellen maken en voor bulkinschrijving. Zie voor meer informatie [parallelle gegevens importeren in SQL-tabellen gepartitioneerd](parallel-load-sql-partitioned-tables.md).
   > 
   > 
5. Gegevens verkennen, functies maken indien nodig. Houd er rekening mee dat de functies niet hoeft te worden gematerialiseerd in de databasetabellen. Opmerking alleen de benodigde query om deze te maken.
6. Bepaal op een steekproefomvang gegevens als nodig is en/of gewenst.
7. Aanmelden bij de [Azure Machine Learning Studio](https://studio.azureml.net/).
8. Lezen van de gegevens rechtstreeks vanuit de SQL Server met de [importgegevens] [ import-data] module. Plak de benodigde query velden extraheert en maakt functies en voorbeelden van gegevens, indien nodig rechtstreeks in de [importgegevens] [ import-data] query.
9. Eenvoudige Azure Machine Learning-experiment stroom met geüploade gegevensset wordt gestart.

### <a name="alternate-method-to-copy-a-full-database-from-an-on-premises--sql-server-to-azure-sql-database"></a>Alternatieve methode volledige database kopiëren vanuit een lokale SQL Server naar Azure SQL Database
![Lokale database loskoppelen en koppelen aan SQL-database in Azure][7]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Extra Azure-resources: Azure virtuele Machine (SQL Server / IPython Notebook server)
Als u wilt repliceren van de volledige SQL Server-database in uw SQL Server VM, moet u kopiëren een database van één locatie of de server naar een andere, ervan uitgaande dat de database kan worden gehouden die tijdelijk offline. Dit doet u in de SQL Server Management Studio Object Explorer of met behulp van de equivalente Transact-SQL-opdrachten.

1. Ontkoppel de database op de bronlocatie. Zie voor meer informatie [loskoppelen van een database](https://technet.microsoft.com/library/ms191491\(v=sql.110\).aspx).
2. Kopieer in Windows Verkenner of Windows-opdrachtprompt venster de losgekoppelde databasebestand of de bestanden en het logboekbestand of de bestanden naar de doellocatie op de SQL Server-virtuele machine in Azure.
3. De gekopieerde bestanden toevoegen aan het doelexemplaar van SQL Server. Zie voor meer informatie [om een Database koppelen](https://technet.microsoft.com/library/ms190209\(v=sql.110\).aspx).

[Verplaatsen van een Database met loskoppelen en koppelt (Transact-SQL)](https://technet.microsoft.com/library/ms187858\(v=sql.110\).aspx)

## <a name="largedbtohive"></a>Scenario \#7: Big data in lokale bestanden doel Hive-database in Azure HDInsight Hadoop-clusters
![BIG data in lokale doel Hive][9]

#### <a name="additional-azure-resources-azure-hdinsight-hadoop-cluster-and-azure-virtual-machine-ipython-notebook-server"></a>Extra Azure-resources: Azure HDInsight Hadoop-Cluster en Azure virtuele Machine (IPython Notebook server)
1. Maak een Azure virtuele Machine waarop IPython Notebook server wordt uitgevoerd.
2. Maak een Azure HDInsight Hadoop-cluster.
3. (Optioneel) Vooraf verwerken en gegevens opgeschoond.
   
   a.  Vooraf verwerken en de gegevens in IPython laptop, toegang tot gegevens uit Azure opschonen
   
       blobs.
   
   b.  Transformeer gegevens gereinigd, in tabelvorm, indien nodig.
   
   c.  Gegevens opslaan naar VM-local-bestanden (IPython Notebook wordt uitgevoerd op virtuele machine, lokale stations verwijzen naar VM-schijven).
4. Gegevens uploaden naar de standaardcontainer van het Hadoop-cluster dat is geselecteerd in stap 2.
5. Laden van gegevens met Hive-database in Azure HDInsight Hadoop-cluster.
   
   a.  Aanmelden met het hoofdknooppunt van het Hadoop-cluster
   
   b.  Open de Hadoop-opdrachtregel.
   
   c.  Voer in de hoofdmap van Hive met opdracht `cd %hive_home%\bin` in Hadoop vanaf de opdrachtregel.
   
   d.  Voer de Hive-query's voor het maken van de database en tabellen en laden van gegevens uit blob storage naar Hive-tabellen.
   
   > [!NOTE]
   > Als de gegevens groot is, kunnen gebruikers de Hive-tabel met partities maken. Vervolgens kunnen gebruikers gebruiken een `for` lus in de Hadoop opdrachtregel hoofdknooppunt van het laden van gegevens in de Hive-tabel is gepartitioneerd door partitie.
   > 
   > 
6. Gegevens verkennen en functies maken indien nodig in de Hadoop-opdrachtregel. Houd er rekening mee dat de functies niet hoeft te worden gematerialiseerd in de databasetabellen. Opmerking alleen de benodigde query om deze te maken.
   
   a.  Aanmelden met het hoofdknooppunt van het Hadoop-cluster
   
   b.  Open de Hadoop-opdrachtregel.
   
   c.  Voer in de hoofdmap van Hive met opdracht `cd %hive_home%\bin` in Hadoop vanaf de opdrachtregel.
   
   d.  Hive-query's in de Hadoop-opdrachtregel op het hoofdknooppunt van het Hadoop-cluster Verken de gegevens en functies maken indien nodig uitvoeren.
7. Als nodig is en/of gewenst, de gegevens in Azure Machine Learning Studio past controleren.
8. Aanmelden bij de [Azure Machine Learning Studio](https://studio.azureml.net/).
9. Lezen van de gegevens rechtstreeks vanuit de `Hive Queries` met behulp van de [importgegevens] [ import-data] module. Plak de benodigde query velden extraheert en maakt functies en voorbeelden van gegevens, indien nodig rechtstreeks in de [importgegevens] [ import-data] query.
10. Eenvoudige Azure Machine Learning-experiment stroom met geüploade gegevensset wordt gestart.

## <a name="decisiontree"></a>Beslissingsstructuur voor de selectie van scenario
- - -
Het volgende diagram ziet u de scenario's die hierboven worden beschreven en het proces van Advanced Analytics en technologie die u hebt gekozen om naar de gespecificeerde scenario's. Denk eraan dat gegevensverwerking, exploratie functie-engineering en steekproeven kunnen duren plaats in een of meer methode/omgeving--op de bronlocatie, tussenliggende, en/of doel omgevingen: en iteratief naar behoefte kunt doorgaan. Het diagram alleen fungeert als een illustratie van enkele mogelijke stromen en biedt geen een uitputtende opsomming.

![Voorbeeldscenario DS proces scenario 's][8]

### <a name="advanced-analytics-in-action-examples"></a>Geavanceerde analyses in actie voorbeelden
Zie voor end-to-end Azure Machine Learning-scenario's die gebruikmaken van het proces van Advanced Analytics en technologie openbare gegevenssets:

* [Procedure voor het wetenschappelijke gegevens in een team in actie: met behulp van SQL Server](sql-walkthrough.md).
* [Procedure voor het wetenschappelijke gegevens in een team in actie: met behulp van HDInsight Hadoop-clusters](hive-walkthrough.md).

[1]: ./media/plan-sample-scenarios/dsp-plan-small-in-aml.png
[2]: ./media/plan-sample-scenarios/dsp-plan-local-with-processing.png
[3]: ./media/plan-sample-scenarios/dsp-plan-local-large.png
[4]: ./media/plan-sample-scenarios/dsp-plan-local-to-db.png
[5]: ./media/plan-sample-scenarios/dsp-plan-large-to-db.png
[6]: ./media/plan-sample-scenarios/dsp-plan-db-to-db.png
[7]: ./media/plan-sample-scenarios/dsp-plan-attach-db.png
[8]: ./media/plan-sample-scenarios/dsp-plan-sample-scenarios.png
[9]: ./media/plan-sample-scenarios/dsp-plan-local-to-hive.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
