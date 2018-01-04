---
title: Excel en Hadoop koppelen met Power Query - Azure HDInsight | Microsoft Docs
description: Informatie over het profiteren van business intelligence-onderdelen en het gebruik van Power Query voor Excel voor toegang tot gegevens die zijn opgeslagen in Hadoop op HDInsight.
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 01ad2f90-7520-44d9-8c16-4d936faaff9b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/03/2017
ms.author: jgao
ms.openlocfilehash: 1f5139f3fdb1cc73bb40828021256bed4290e1a2
ms.sourcegitcommit: 4bd369fc472dced985239aef736fece42fecfb3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="connect-excel-to-hadoop-by-using-power-query"></a>Excel en Hadoop koppelen met Power Query
Een belangrijke functie van de Microsoft big data-oplossing is de integratie van Microsoft business intelligence (BI)-onderdelen met Hadoop-clusters in Azure HDInsight. Een voorbeeld van een primaire is de mogelijkheid Excel verbindt met de Azure Storage-account met de gegevens die zijn gekoppeld aan de Hadoop-cluster met behulp van de Microsoft Power Query voor Excel-invoegtoepassing. Dit artikel begeleidt u bij het instellen en gebruiken van Power Query query uitvoeren op gegevens die zijn gekoppeld aan een beheerd met HDInsight Hadoop-cluster.

### <a name="prerequisites"></a>Vereisten
Voordat u dit artikel, hebt u de volgende items:

* **Een HDInsight-cluster**. Zie een configureren [aan de slag met Azure HDInsight] [hdinsight-get-started].
* **Een werkstation** waarop Windows 7, Windows Server 2008 R2 of een nieuwer besturingssysteem wordt uitgevoerd.
* **Office 2016, Office 2013 Professional Plus Office 365 ProPlus, zelfstandige versie van Excel 2013 of Office 2010 Professional Plus**.

## <a name="install-power-query"></a>Power Query installeren
Power Query kunt importeren van gegevens die de uitvoer heeft zijn of die is gegenereerd door een Hadoop-taak uitgevoerd op een HDInsight-cluster.

In Excel 2016 is Power Query geïntegreerd in het lint gegevens onder de sectie transformatie & ophalen. Voor oudere versies van Excel, download u Microsoft Power Query voor Excel uit de [Microsoft Download Center] [ powerquery-download] en te installeren.

## <a name="import-hdinsight-data-into-excel"></a>HDInsight-gegevens importeren in Excel
De Power Query-invoegtoepassing voor Excel kunt u eenvoudig gegevens importeren uit uw HDInsight-cluster in Excel, waarbij BI-hulpprogramma's zoals PowerPivot en Power kaart kunnen worden gebruikt om te controleren, analyseren en de gegevens aanwezig.

**Gegevens importeren uit een HDInsight-cluster**

1. Open Excel.
2. Maak een nieuwe lege werkmap.
3. Voer de volgende stappen uit op basis van de versie van Excel:

    - Excel 2016

        - Klik op de **gegevens** menu, klikt u op **gegevens ophalen** van de **transformeren gegevens & ophalen** lint, klikt u op **van Azure**, en klik vervolgens op **Van Azure HDInsight(HDFS)**.

        ![HDI. PowerQuery.SelectHdiSource](./media/apache-hadoop-connect-excel-power-query/hdi.powerquery.selecthdisource.excel2016.png)

    - Excel 2013/2010

        - Klik op de **Power Query** menu, klikt u op **van Azure**, en klik vervolgens op **van Microsoft Azure HDInsight**.
   
        ![HDI. PowerQuery.SelectHdiSource][image-hdi-powerquery-hdi-source]
       
        **Opmerking:** als er geen de **Power Query** menu, gaat u naar **bestand** > **opties** > **-invoegtoepassingen**, en selecteer **COM-invoegtoepassingen** in de vervolgkeuzelijst **beheren** vak aan de onderkant van de pagina. Selecteer de **gaan...**  knop en controleer of het selectievakje uit voor de Power Query voor Excel-invoegtoepassing is gecontroleerd.
       
        **Opmerking:** Power Query kunt u gegevens uit HDFS importeren door te klikken op **van andere bronnen**.
4. Voor **accountnaam**, voer de naam van de Azure Blob storage-account die is gekoppeld aan het cluster en klik vervolgens op **OK**. Dit account kan worden de [standaard opslagaccount](../hdinsight-administer-use-management-portal.md#find-the-default-storage-account) of een gekoppeld opslagaccount.  De indeling is *https://&lt;StorageAccountName >.blob.core.windows.net/*.
5. Voor **Accountsleutel**, voer de sleutel voor de Blob storage-account en klik vervolgens op **opslaan**. (U moet de account informatie alleen de eerste keer dat u toegang dit archief tot invoeren.)
6. In de **Navigator** in het linkerdeelvenster van de Query-Editor, dubbelklikt u op de naam van de Blob-container. De containernaam is standaard dezelfde naam als de naam van het cluster.
7. Zoek **HiveSampleData.txt** in de **naam** kolom (het mappad is **... / hive/datawarehouse/hivesampletable/**), en klik vervolgens op **binaire** aan de linkerkant van HiveSampleData.txt. HiveSampleData.txt wordt geleverd met het cluster. Desgewenst kunt u uw eigen-bestand.
   
    ![HDI. PowerQuery.ImportData][image-hdi-powerquery-importdata]
8. Als u wilt, kunt u de kolomnamen van de wijzigen. Wanneer u klaar bent, klikt u op **sluit & laden**.  De gegevens zijn geladen met uw werkmap:
   
    ![HDI. PowerQuery.ImportedTable][image-hdi-powerquery-imported-table]

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe gebruik Power Query om gegevens te halen uit HDInsight in Excel. Op deze manier kunt u gegevens ophalen uit HDInsight in Azure SQL Database. Het is ook mogelijk om gegevens te uploaden naar HDInsight. Zie voor meer informatie de volgende artikelen:

* [Hive-gegevens visualiseren met Microsoft Power BI in Azure HDInsight](apache-hadoop-connect-hive-power-bi.md).
* [Interactieve Hive-Query-gegevens visualiseren met Power BI in Azure HDInsight](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Zeppelin gebruiken voor het uitvoeren van Hive-query's in Azure HDInsight ](./../hdinsight-connect-hive-zeppelin.md).
* [Excel verbinden met HDInsight met het Microsoft Hive ODBC-stuurprogramma](apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Verbinding maken met Azure HDInsight en het uitvoeren van Hive-query's met Data Lake Tools voor Visual Studio](apache-hadoop-visual-studio-tools-get-started.md).
* [Azure HDInsight-hulpprogramma gebruiken voor Visual Studio Code](../hdinsight-for-vscode.md).
* [Gegevens uploaden naar HDInsight](./../hdinsight-upload-data.md).

[image-hdi-powerquery-hdi-source]: ./media/apache-hadoop-connect-excel-power-query/hdi.powerquery.selecthdisource.png
[image-hdi-powerquery-importdata]: ./media/apache-hadoop-connect-excel-power-query/hdi.powerquery.importdata.png
[image-hdi-powerquery-imported-table]: ./media/apache-hadoop-connect-excel-power-query/hdi.powerquery.importedtable.PNG

[powerquery-download]: http://go.microsoft.com/fwlink/?LinkID=286689
