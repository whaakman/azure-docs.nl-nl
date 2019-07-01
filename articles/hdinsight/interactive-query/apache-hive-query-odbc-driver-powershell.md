---
title: Query Apache Hive ODBC-stuurprogramma en PowerShell - Azure HDInsight
description: Gebruik het Microsoft Hive ODBC-stuurprogramma en PowerShell op Apache Hive-query op Azure HDInsight-clusters.
keywords: hive, hive odbc, powershell
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: tutorial
ms.date: 06/27/2019
ms.author: hrasheed
ms.openlocfilehash: b02c865e953861b5ac396538fdd0f0623b0e5428
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67486100"
---
# <a name="tutorial-query-apache-hive-with-odbc-and-powershell"></a>Zelfstudie: Query Apache Hive ODBC-en PowerShell

Microsoft ODBC-stuurprogramma's bieden een flexibele manier om te communiceren met verschillende soorten gegevensbronnen, waaronder Apache Hive. U kunt code schrijven in scripttalen zoals PowerShell die gebruikmaken van het ODBC-stuurprogramma's te openen van een verbinding met uw cluster Hive, doorgeven van een query van uw keuze, en de resultaten weer te geven.

In deze zelfstudie voert u de volgende taken:

> [!div class="checklist"]
> * Download en installeer het stuurprogramma Microsoft Hive ODBC
> * Een Apache Hive ODBC-gegevensbron die is gekoppeld aan uw cluster maken
> * Querygegevens als voorbeeld van het cluster met behulp van PowerShell

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voordat u met deze zelfstudie begint, moet u beschikken over de volgende items:

* Een interactieve Query-cluster in HDInsight. Zie voor het maken van een [aan de slag met Azure HDInsight](../hdinsight-hadoop-provision-linux-clusters.md). Selecteer **Interactive Query** als het cluster.

## <a name="install-microsoft-hive-odbc-driver"></a>Microsoft Hive ODBC-stuurprogramma installeren

Download en installeer de [stuurprogramma Microsoft Hive ODBC](https://go.microsoft.com/fwlink/?LinkID=286698).

## <a name="create-apache-hive-odbc-data-source"></a>Apache Hive ODBC-gegevensbron maken

De volgende stappen laten zien hoe u een Apache Hive ODBC-gegevensbron maken.

1. Vanuit Windows, navigeert u naar **Start** > **Windows Systeembeheer** > **ODBC-gegevensbronnen (32-bit)/(64-bit)** .  Een **ODBC-gegevensbronbeheer** venster wordt geopend.

    ![OBDC gegevensbronbeheer](./media/apache-hive-query-odbc-driver-powershell/hive-odbc-driver-dsn-setup.png "configureren met behulp van ODBC-gegevensbronbeheer DSN")

1. Uit de **gebruiker DSN** tabblad **toevoegen** openen de **nieuwe gegevensbron maken** venster.

1. Selecteer **stuurprogramma Microsoft Hive ODBC**, en selecteer vervolgens **voltooien** openen de **Microsoft Hive ODBC-stuurprogramma DSN Setup** venster.

1. Typ of selecteer de volgende waarden:

   | Eigenschap | Description |
   | --- | --- |
   |  Naam van de gegevensbron |Geef uw gegevensbron een naam |
   |  Host (s) |Voer `CLUSTERNAME.azurehdinsight.net` in. Bijvoorbeeld: `myHDICluster.azurehdinsight.net` |
   |  Poort |Gebruik **443**.|
   |  Database |Gebruik **standaard**. |
   |  Mechanisme |Selecteer **Windows Azure HDInsight Service** |
   |  Gebruikersnaam |HDInsight-cluster HTTP-gebruikersnaam invoeren. De standaardgebruikersnaam **admin**. |
   |  Wachtwoord |Voer gebruikerswachtwoord van HDInsight-cluster. Schakel het selectievakje **wachtwoord opslaan (versleutelde)** .|

1. Optioneel: Selecteer **geavanceerde opties**.  

   | Parameter | Description |
   | --- | --- |
   |  Systeemeigen Query gebruiken |Wanneer deze optie geselecteerd, probeert het ODBC-stuurprogramma niet TSQL converteren naar HiveQL. Gebruik deze optie alleen als u 100% zeker dat u pure HiveQL-instructies wilt verzenden. Bij het verbinden met SQL Server of Azure SQL Database, laat u het vakje uitgeschakeld. |
   |  Rijen per blok wordt opgehaald |Tijdens het ophalen van een groot aantal records, kan afstemmen van deze parameter worden vereist om ervoor te zorgen van optimale prestaties. |
   |  Standaard kolomlengte tekenreeks, lengte van de binaire kolom, decimaal kolomschaal |De lengte van het gegevenstype en Precision-systemen kunnen beïnvloeden hoe gegevens worden geretourneerd. Ze ervoor zorgen dat de onjuiste gegevens moeten worden geretourneerd vanwege verlies van precisie- en moet worden afgekapt. |

    ![Geavanceerde opties](./media/apache-hive-query-odbc-driver-powershell/odbc-data-source-advanced-options.png "DSN geavanceerde configuratieopties")

1. Selecteer **testen** voor het testen van de gegevensbron. Wanneer de gegevensbron correct is geconfigureerd, ziet u de testresultaten **SUCCES**.  

1. Selecteer **OK** om de Test-venster te sluiten.  

1. Selecteer **OK** sluiten de **Microsoft Hive ODBC-stuurprogramma DSN Setup** venster.  

1. Selecteer **OK** sluiten de **ODBC-gegevensbronbeheer** venster.  

## <a name="query-data-with-powershell"></a>Gegevens opvragen met PowerShell

De volgende PowerShell-script is een functie die ODBC query uitvoeren op een Hive-cluster.

```powershell
function Get-ODBC-Data {

   param(
   [string]$query=$(throw 'query is required.'),
   [string]$dsn,  
   [PSCredential] $cred = (Get-Credential)  
   )

   $conn = New-Object System.Data.Odbc.OdbcConnection
   $uname = $cred.UserName

   $pswd = (New-Object System.Net.NetworkCredential -ArgumentList "", $cred.Password).Password
   $conn.ConnectionString = "DSN=$dsn;Uid=$uname;Pwd=$pswd;"
   $conn.open()
   $cmd = New-object System.Data.Odbc.OdbcCommand($query,$conn)

   $ds = New-Object system.Data.DataSet

   (New-Object system.Data.odbc.odbcDataAdapter($cmd)).fill($ds) #| out-null
   $conn.close()
   $ds.Tables
}
```

Het volgende codefragment wordt de functie van de bovenstaande gebruikt voor het uitvoeren van een query op het Interactive Query-cluster dat u aan het begin van de zelfstudie hebt gemaakt. Vervang `DATASOURCENAME` met de **gegevensbronnaam** die u hebt opgegeven op de **Microsoft Hive ODBC-stuurprogramma DSN Setup** scherm. Wanneer u hierom wordt gevraagd om referenties, voer de gebruikersnaam en wachtwoord die u hebt ingevoerd onder **de gebruikersnaam voor clusteraanmelding** en **wachtwoord voor clusteraanmelding** tijdens het maken van het cluster.

```powershell

$dsn = "DATASOURCENAME"

$query = "select count(distinct clientid) AS total_clients from hivesampletable"

Get-ODBC-Data -query $query -dsn $dsn
```

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, verwijdert u de resourcegroep, de HDInsight-cluster en de storage-account. Om dit te doen, selecteert u de resourcegroep waarin het cluster is gemaakt en klikt u op **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u de Microsoft Hive ODBC-stuurprogramma en PowerShell gebruiken voor het ophalen van gegevens uit uw Azure HDInsight Interactive Query-cluster.

> [!div class="nextstepaction"]
> [Excel verbinden met Apache Hive ODBC gebruiken](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md)
