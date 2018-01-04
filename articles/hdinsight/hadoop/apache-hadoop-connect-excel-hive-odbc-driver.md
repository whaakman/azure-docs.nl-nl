---
title: Excel en Hadoop koppelen met het Hive ODBC-stuurprogramma - Azure HDInsight | Microsoft Docs
description: Informatie over het instellen en gebruiken van het Microsoft Hive ODBC-stuurprogramma voor Excel query uitvoeren op gegevens in HDInsight-clusters van Microsoft Excel.
keywords: hadoop, excel, hive excel, hive odbc
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
tags: azure-portal
editor: cgronlun
ms.assetid: a7665a14-0211-4521-b3e7-3b26e8029cc0
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/06/2017
ms.author: jgao
ms.openlocfilehash: d169645b9b701e611d27e0d984bf44b7ca85d2ad
ms.sourcegitcommit: 4bd369fc472dced985239aef736fece42fecfb3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="connect-excel-to-hadoop-in-azure-hdinsight-with-the-microsoft-hive-odbc-driver"></a>Excel verbinden met Hadoop in Azure HDInsight met het Microsoft Hive ODBC-stuurprogramma

[!INCLUDE [ODBC-JDBC-selector](../../../includes/hdinsight-selector-odbc-jdbc.md)]

Microsoft Business Intelligence (BI) onderdelen Big Data-oplossing van Microsoft geïntegreerd met Apache Hadoop-clusters die zijn geïmplementeerd door de Azure HDInsight. Een voorbeeld van deze integratie is de mogelijkheid Excel verbindt met het Hive-datawarehouse van een Hadoop-cluster in HDInsight met behulp van het stuurprogramma Microsoft Hive Open Database Connectivity (ODBC).

Het is ook mogelijk om de gegevens die zijn gekoppeld aan een HDInsight-cluster en andere gegevensbronnen, met inbegrip van andere (niet-HDInsight) Hadoop-clusters van Excel met de Microsoft Power Query-invoegtoepassing voor Excel te verbinden. Zie voor meer informatie over het installeren en gebruiken van Power Query [Excel verbinding naar HDInsight met Power Query][hdinsight-power-query].

> [!NOTE]
> Terwijl de stappen in dit artikel kunnen worden gebruikt met een Linux- of Windows gebaseerde HDInsight-cluster, is Windows vereist voor de clientwerkstation.
> 
> 

**Vereisten**:

Voordat u dit artikel, hebt u de volgende items:

* **Een HDInsight-cluster**. Als u wilt maken, Zie [aan de slag met Azure HDInsight](apache-hadoop-linux-tutorial-get-started.md).
* **Een werkstation** met Office 2013 Professional Plus, Office 365 Pro Plus, zelfstandige versie van Excel 2013 of Office 2010 Professional Plus.

## <a name="install-microsoft-hive-odbc-driver"></a>Installeer Microsoft Hive ODBC-stuurprogramma
Download en installeer Microsoft Hive ODBC-stuurprogramma van de [Downloadcentrum][hive-odbc-driver-download].

Dit stuurprogramma kan worden geïnstalleerd op een 32-bits of 64-bits versies van Windows 7, Windows 8, Windows 10, Windows Server 2008 R2 en Windows Server 2012. Het stuurprogramma kan verbinding maken met Azure HDInsight (versie 1.6 of hoger) en Azure HDInsight-Emulator (v.1.0.0.0 en hoger). U moet de versie die overeenkomt met de versie van de toepassing waarbij u gebruikmaakt van het ODBC-stuurprogramma installeren. Het stuurprogramma wordt gebruikt voor deze zelfstudie uit Office Excel.

## <a name="create-hive-odbc-data-source"></a>Hive ODBC-gegevensbron maken
De volgende stappen ziet u het maken van een Hive ODBC-gegevensbron.

1. Druk op de Windows-toets op het startscherm openen vanuit Windows 8 of Windows 10, en typ vervolgens **gegevensbronnen**.
2. Klik op **instellen van ODBC-gegevensbronnen (32-bits)** of **instellen van ODBC-gegevensbronnen (64-bits)** , afhankelijk van uw Office-versie. Als u van Windows 7 gebruikmaakt, kiest u **ODBC-gegevensbronnen (32 bits)** of **ODBC-gegevensbronnen (64 bits)** van **Systeembeheer**. U ziet de **ODBC Data Source Administrator** dialoogvenster.
   
    ![Gegevensbronbeheer OBDC](./media/apache-hadoop-connect-excel-hive-odbc-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png "een met ODBC-gegevensbronbeheer DSN configureren")

3. DNS van de gebruiker, klik op **toevoegen** openen de **nieuwe gegevensbron maken** wizard.
4. Selecteer **stuurprogramma Microsoft Hive ODBC**, en klik vervolgens op **voltooien**. U ziet de **Microsoft Hive ODBC-stuurprogramma DNS-instellingen** dialoogvenster.
5. Typ of selecteer de volgende waarden:
   
   | Eigenschap | Beschrijving |
   | --- | --- |
   |  Naam van de gegevensbron |Geef uw gegevensbron een naam |
   |  Host |Voer &lt;HDInsightClusterName>.azurehdinsight.net in. Bijvoorbeeld: myHDICluster.azurehdinsight.net |
   |  Poort |Gebruik <strong>443</strong>. (Deze poort is gewijzigd van 563 in 443.) |
   |  Database |Gebruik <strong>Standaard</strong>. |
   |  Mechanisme |Selecteer <strong>Azure HDInsight Service</strong> |
   |  Gebruikersnaam |Voer de gebruikersnaam van HDInsight-cluster HTTP. De standaardgebruikersnaam <strong>admin</strong>. |
   |  Wachtwoord |Voer gebruikerswachtwoord HDInsight-cluster. |
   
    </table>
   
    Er zijn een aantal belangrijke parameters moet denken wanneer u klikt op **geavanceerde opties**:
   
   | Parameter | Beschrijving |
   | --- | --- |
   |  Gebruik systeemeigen Query |Wanneer deze optie is geselecteerd, probeert het ODBC-stuurprogramma niet TSQL converteren naar HiveQL. U dient alleen te gebruiken als u 100% zeker dat u verzendt pure HiveQL-instructies zijn. Wanneer u verbinding maakt met SQL Server of Azure SQL Database, laat u het vakje uitgeschakeld. |
   |  Rijen per blok opgehaald |Tijdens ophalen van een groot aantal records kan afstemming van deze parameter worden vereist om ervoor te zorgen optimale prestaties. |
   |  Standaard-tekenreekslengte kolom, binaire kolomlengte, decimaal kolomschaal |De lengte van het gegevenstype en Precision-systemen kunnen beïnvloeden hoe gegevens worden geretourneerd. Deze leiden tot onjuiste gegevens moeten worden opgehaald vanwege verlies van precisie en/of moet worden afgekapt. |

    ![Geavanceerde opties voor](./media/apache-hadoop-connect-excel-hive-odbc-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png "DSN geavanceerde configuratieopties")

1. Klik op **testen** voor het testen van de gegevensbron. Wanneer de gegevensbron correct is geconfigureerd, worden *TESTS voltooid!*.
2. Klik op **OK** om de Test-dialoogvenster te sluiten. De nieuwe gegevensbron worden vermeld op de **ODBC Data Source Administrator**.
3. Klik op **OK** de wizard wilt afsluiten.

## <a name="import-data-into-excel-from-hdinsight"></a>Gegevens in Excel importeren vanuit HDInsight
De volgende stappen beschrijven de manier waarop gegevens importeren uit een Hive-tabel in een Excel-werkmap met behulp van de ODBC-gegevensbron die u hebt gemaakt in de vorige sectie.

1. Open een nieuwe of bestaande werkmap in Excel.
2. Van de **gegevens** tabblad **gegevens ophalen**, klikt u op **van andere bronnen**, en klik vervolgens op **van ODBC** starten de **gegevens Wizard verbinding**.
   
    ![Wizard Gegevensverbinding Open](./media/apache-hadoop-connect-excel-hive-odbc-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png "wizard Gegevensverbinding openen")
4. Selecteer de naam van de gegevensbron die u in de laatste sectie hebt gemaakt en klik vervolgens op **OK**.
5. Voer de naam van de gebruiker Hadoop (de standaardnaam is admin) en het wachtwoord en klik vervolgens op **Connect**.
6. Vouw op de Navigator **HIVE**, vouw **standaard**, klikt u op **hivesampletable**, en klik vervolgens op **Load**. Het duurt een paar seconden voordat de gegevens naar Excel worden geïmporteerd.

    ![HDInsight Hive ODBC navigator](./media/apache-hadoop-connect-excel-hive-odbc-driver/hdinsight.hive.odbc.navigator.png "wizard Gegevensverbinding openen")


## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe gebruikmaken van het stuurprogramma Microsoft Hive ODBC-gegevens ophalen van de HDInsight-Service in Excel. Op deze manier kunt u gegevens ophalen uit de HDInsight-Service in SQL-Database. Het is ook mogelijk om gegevens te uploaden naar HDInsight-Service. Voor meer informatie zie:

* [Hive-gegevens visualiseren met Microsoft Power BI in Azure HDInsight](apache-hadoop-connect-hive-power-bi.md).
* [Interactieve Hive-Query-gegevens visualiseren met Power BI in Azure HDInsight](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Zeppelin gebruiken voor het uitvoeren van Hive-query's in Azure HDInsight ](./../hdinsight-connect-hive-zeppelin.md).
* [Excel en Hadoop koppelen met Power Query](apache-hadoop-connect-excel-power-query.md).
* [Verbinding maken met Azure HDInsight en het uitvoeren van Hive-query's met Data Lake Tools voor Visual Studio](apache-hadoop-visual-studio-tools-get-started.md).
* [Azure HDInsight-hulpprogramma gebruiken voor Visual Studio Code](../hdinsight-for-vscode.md).
* [Gegevens uploaden naar HDInsight](./../hdinsight-upload-data.md).

[hdinsight-use-sqoop]:hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]:hdinsight-use-hive.md
[hdinsight-upload-data]: ../hdinsight-upload-data.md
[hdinsight-power-query]: ../hdinsight-connect-excel-power-query.md
[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698


