---
title: Excel verbinden met Apache Hadoop met Power Query - Azure HDInsight
description: Leer hoe u profiteren van business intelligence-onderdelen en Power Query voor Excel gebruiken voor toegang tot gegevens die zijn opgeslagen in Hadoop op HDInsight.
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2018
ms.openlocfilehash: 687a9884c861b4cf72e51f9179e6c0b43968801d
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58202901"
---
# <a name="connect-excel-to-apache-hadoop-by-using-power-query"></a>Excel verbinden met Apache Hadoop met Power Query
Een belangrijke functie van de Microsoft big data-oplossing is de integratie van Microsoft business intelligence (BI)-onderdelen met Apache Hadoop-clusters in Azure HDInsight. Een voorbeeld van een primaire is de mogelijkheid om Excel verbinden met de Azure Storage-account met de gegevens die zijn gekoppeld aan uw Hadoop-cluster met behulp van de Microsoft Power Query voor Excel-invoegtoepassing. Dit artikel helpt u bij het instellen en gebruiken van Power Query om gegevens te doorzoeken die zijn gekoppeld aan een beheerd met HDInsight Hadoop-cluster.

### <a name="prerequisites"></a>Vereisten
Voordat u dit artikel, hebt u de volgende items:

* **Een HDInsight-cluster**. Zie configureren van een [aan de slag met Azure HDInsight] [hdinsight-get-started].
* **Een werkstation** waarop Windows 7, Windows Server 2008 R2 of een nieuwer besturingssysteem wordt uitgevoerd.
* **Office 2016, Office 2013 Professional Plus, Office 365 ProPlus, zelfstandige versie van Excel 2013 of Office 2010 Professional Plus**.

## <a name="install-power-query"></a>Installeren van Power Query
Power Query kunt importeren van gegevens die de uitvoer heeft zijn of die is gegenereerd door een Hadoop-taak die wordt uitgevoerd op een HDInsight-cluster.

In Excel 2016, is Power Query geïntegreerd in het lint gegevens onder de sectie ophalen en transformeren. Download voor oudere versies van Excel, Microsoft Power Query voor Excel uit de [Microsoft Download Center] [ powerquery-download] en installeer deze.

## <a name="import-hdinsight-data-into-excel"></a>HDInsight-gegevens importeren in Excel
De Power Query-invoegtoepassing voor Excel kunt u eenvoudig gegevens importeren uit uw HDInsight-cluster in Excel, waar BI-hulpprogramma's zoals PowerPivot en Power Map kunnen worden gebruikt om te controleren, analyseren en de gegevens te presenteren.

**Gegevens importeren uit een HDInsight-cluster**

1. Open Excel.
2. Maak een nieuwe lege werkmap.
3. Voer de volgende stappen uit op basis van de versie van Excel:

   - Excel 2016

     - Klik op de **gegevens** menu, klikt u op **gegevens ophalen** uit de **gegevens ophalen en transformeren** lint, klikt u op **van Azure**, en klik vervolgens op **Uit Azure HDInsight(HDFS)**.

       ![HDI.PowerQuery.SelectHdiSource](./media/apache-hadoop-connect-excel-power-query/hdi.powerquery.selecthdisource.excel2016.png)

   - Excel 2013/2010

     - Klik op de **Power Query** menu, klikt u op **van Azure**, en klik vervolgens op **van Microsoft Azure HDInsight**.
   
       ![HDI.PowerQuery.SelectHdiSource][image-hdi-powerquery-hdi-source]
       
       **Opmerking:** Als er geen de **Power Query** menu, gaat u naar **bestand** > **opties** > **-invoegtoepassingen**, en selecteer  **COM-invoegtoepassingen** in de vervolgkeuzelijst **beheren** vak aan de onderkant van de pagina. Selecteer de **gaan...**  knop en controleer of het selectievakje in voor de Power Query voor Excel-invoegtoepassing is ingeschakeld.
       
       **Opmerking:** Power Query kunt u gegevens importeren uit HDFS door te klikken op ook **van andere bronnen**.
4. Voor **accountnaam**, voer de naam van de Azure Blob storage-account dat is gekoppeld aan het cluster en klik vervolgens op **OK**. Dit account kan worden het standaardaccount voor opslag of een gekoppelde storage-account.  De indeling is *https://&lt;StorageAccountName >.blob.core.windows.net/*.
5. Voor **Accountsleutel**, voer de sleutel voor de Blob storage-account en klik vervolgens op **opslaan**. (U moet de account informatie alleen de eerste keer dat u toegang dit archief tot invoeren.)
6. In de **Navigator** in het linkerdeelvenster van de Query-Editor, dubbelklikt u op de naam van de Blob storage-container. Standaard is de naam van dezelfde naam als de naam van het cluster.
7. Zoek **HiveSampleData.txt** in de **naam** kolom (pad naar de map is **... / hive/datawarehouse/hivesampletable/**), en klik vervolgens op **binaire** aan de linkerkant van HiveSampleData.txt. HiveSampleData.txt wordt geleverd met alle het cluster. Desgewenst kunt u uw eigen-bestand.
   
    ![HDI.PowerQuery.ImportData][image-hdi-powerquery-importdata]
8. Als u wilt, kunt u de namen van kolommen wijzigen. Wanneer u klaar bent, klikt u op **sluiten en laden**.  De gegevens zijn geladen in uw werkmap:
   
    ![HDI.PowerQuery.ImportedTable][image-hdi-powerquery-imported-table]

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u Power Query voor het ophalen van gegevens uit HDInsight in Excel. Op deze manier kunt u gegevens ophalen uit HDInsight in Azure SQL Database. Het is ook mogelijk om gegevens te uploaden naar HDInsight. Zie de volgende artikelen voor meer informatie:

* [Apache Hive-gegevens visualiseren met Microsoft Power BI in Azure HDInsight](apache-hadoop-connect-hive-power-bi.md).
* [Interactive Query Hive-gegevens visualiseren met Power BI in Azure HDInsight](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Apache Zeppelin gebruiken voor het Apache Hive-query's uitvoeren in Azure HDInsight](./../hdinsight-connect-hive-zeppelin.md).
* [Excel verbinden met HDInsight met het Microsoft Hive ODBC-stuurprogramma](apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Verbinding maken met Azure HDInsight en Apache Hive-query's uitvoeren met behulp van Data Lake Tools voor Visual Studio](apache-hadoop-visual-studio-tools-get-started.md).
* [Azure HDInsight-hulpprogramma gebruiken voor Visual Studio Code](../hdinsight-for-vscode.md).
* [Gegevens uploaden naar HDInsight](./../hdinsight-upload-data.md).

[image-hdi-powerquery-hdi-source]: ./media/apache-hadoop-connect-excel-power-query/hdi.powerquery.selecthdisource.png
[image-hdi-powerquery-importdata]: ./media/apache-hadoop-connect-excel-power-query/hdi.powerquery.importdata.png
[image-hdi-powerquery-imported-table]: ./media/apache-hadoop-connect-excel-power-query/hdi.powerquery.importedtable.PNG

[powerquery-download]: https://go.microsoft.com/fwlink/?LinkID=286689
