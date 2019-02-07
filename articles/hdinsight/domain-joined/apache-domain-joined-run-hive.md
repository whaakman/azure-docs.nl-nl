---
title: Hive-beleidsregels configureren in HDInsight met Enterprise-beveiligingspakket - Azure
description: Informatie over het configureren van Apache Ranger-beleidsregels voor Hive in een Azure HDInsight-service met Enterprise-beveiligingspakket.
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: mamccrea
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 8effa84c9d7adc14060fb00fae9915a04c1d04cc
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55821215"
---
# <a name="configure-apache-hive-policies-in-hdinsight-with-enterprise-security-package"></a>Apache Hive-beleidsregels configureren in HDInsight met Enterprise Security Package
Informatie over het configureren van Apache Ranger-beleidsregels voor Apache Hive. In dit artikel maakt u twee Ranger-beleidsregels om toegang tot de hivesampletable te beperken. De hivesampletable wordt geleverd met HDInsight-clusters. Nadat u de beleidsregels hebt geconfigureerd, gebruikt u Excel en het ODBC-stuurprogramma om verbinding te maken met Hive-tabellen in HDInsight.

## <a name="prerequisites"></a>Vereisten
* Een HDInsight-cluster met Enterprise-beveiligingspakket. Zie [configureren HDInsight-clusters met ESP](apache-domain-joined-configure.md).
* Een werkstation met Office 2016, Office 2013 Professional Plus, Office 365 Pro Plus, een zelfstandige versie van Excel 2013 of Office 2010 Professional Plus.

## <a name="connect-to-apache-ranger-admin-ui"></a>Verbinding maken met de beheerinterface van Apache Ranger
**Verbinding maken met de beheerinterface van Ranger**

1. Maak vanuit een browser verbinding met de beheerinterface van Ranger. De URL is https://&lt;ClusterName>.azurehdinsight.net/Ranger/.

   > [!NOTE]  
   > Ranger maakt gebruik van andere referenties dan Apache Hadoop-cluster. Gebruiken om te voorkomen dat browsers Hadoop-referenties in de cache, nieuw InPrivate-browservenster verbinding maken met de beheerders-UI van Ranger.

2. Meld u aan met de gebruikersnaam en het wachtwoord van het clusterbeheerdomein:

    ![HDInsight ESP Ranger-startpagina](./media/apache-domain-joined-run-hive/hdinsight-domain-joined-ranger-home-page.png)

    Op dit moment werkt Ranger alleen met Yarn en Hive.

## <a name="create-domain-users"></a>Domeingebruikers maken
Zie [een HDInsight-cluster maken met ESP](apache-domain-joined-configure-using-azure-adds.md#create-a-hdinsight-cluster-with-esp), voor informatie over het maken van hiveruser1 en hiveuser2. U gebruikt de twee gebruikersaccounts in deze zelfstudie.

## <a name="create-ranger-policies"></a>Ranger-beleidsregels maken
In deze sectie maakt u twee Ranger-beleidsregels voor toegang tot de hivesampletable. U geeft de machtiging SELECT op voor verschillende sets kolommen. Beide gebruikers zijn gemaakt met behulp van [een HDInsight-cluster maken met ESP](apache-domain-joined-configure-using-azure-adds.md#create-a-hdinsight-cluster-with-esp). In de volgende sectie test u de twee beleidsregels in Excel.

**Ranger-beleidsregels maken**

1. Open de beheerinterface van Ranger. Zie verbinding maken met Apache Ranger beheerders-UI.
2. Klik op **&lt;ClusterName>_hive** onder **Hive**. Er worden twee vooraf geconfigureerde beleidsregels weergegeven.
3. Klik op **Nieuw beleid toevoegen** en voer de volgende waarden in:

   * Beleidsnaam: read-hivesampletable-all
   * Hive-database: standaard
   * Tabel: hivesampletable
   * Hive-kolom:*
   * Gebruiker selecteren: hiveuser1
   * Machtigingen: SELECT

     ![HDInsight ESP Ranger Hive-beleid configureren](./media/apache-domain-joined-run-hive/hdinsight-domain-joined-configure-ranger-policy.png).

     > [!NOTE]  
     > Als een domeingebruiker niet is ingevuld in Gebruiker selecteren, wacht u even, zodat Ranger met AAD kan synchroniseren.
     >
     >
4. Klik op **Toevoegen** om het beleid op te slaan.
5. Herhaal de laatste twee stappen, zodat u een ander beleid kunt maken met de volgende eigenschappen:

   * Beleidsnaam: read-hivesampletable-devicemake
   * Hive-database: standaard
   * Tabel: hivesampletable
   * Hive-kolom: clientid, devicemake
   * Gebruiker selecteren: hiveuser2
   * Machtigingen: SELECT

## <a name="create-hive-odbc-data-source"></a>Hive ODBC-gegevensbron maken
De instructies vindt u in [Hive ODBC-gegevensbron maken](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).  

 | Eigenschap  |Description |
 | --- | --- |
 | Naam van de gegevensbron | Geef uw gegevensbron een naam |
 | Host | Voer &lt;HDInsightClusterName>.azurehdinsight.net in. Bijvoorbeeld: myHDICluster.azurehdinsight.net |
 | Poort | Gebruik **443**. (Deze poort is gewijzigd van 563 in 443.) |
 | Database | Gebruik **Standaard**. |
 | Type Hive-server | Selecteer **Hive Server 2** |
 | Mechanisme | Selecteer **Azure HDInsight Service** |
 | HTTP-pad | Laat dit leeg. |
 | Gebruikersnaam | Voer hiveuser1@contoso158.onmicrosoft.com in. Werk de domeinnaam als het afwijkt. |
 | Wachtwoord | Voer het wachtwoord van hiveuser1 in. |

Zorg ervoor dat u op **Test** klikt voordat u de gegevensbron opslaat.

## <a name="import-data-into-excel-from-hdinsight"></a>Gegevens in Excel importeren vanuit HDInsight
In de laatste sectie hebt u twee beleidsregels geconfigureerd.  hiveuser1 heeft de machtiging SELECT voor alle kolommen en hiveuser2 heeft de machtiging SELECT voor twee kolommen. In deze sectie imiteert u de twee gebruikers, zodat u gegevens kunt importeren in Excel.

1. Open een nieuwe of bestaande werkmap in Excel.
2. Klik op het tabblad **Gegevens** op de optie **Van andere gegevensbronnen** en klik vervolgens op **Van wizard Gegevensverbinding** om de **Wizard Gegevensverbinding** te starten.

    ![Open de Wizard Gegevensverbinding][img hdi simbahiveodbc.excel.dataconnection]
3. Selecteer **ODBC DSN** als de gegevensbron en klik op **Volgende**.
4. Selecteer uit ODBC-gegevensbronnen de naam van de gegevensbron die u in de vorige stap hebt gemaakt en klik op **Volgende**.
5. Geef het wachtwoord voor het cluster in de wizard opnieuw en klik vervolgens op **OK**. Wacht totdat het dialoogvenster **Database en tabel selecteren** wordt geopend. Dit kan een paar seconden duren.
6. Selecteer **hivesampletable** en klik op **Volgende**.
7. Klik op **Voltooien**.
8. In het dialoogvenster **Gegevens importeren** kunt u de query wijzigen of opgeven. Als u dit wilt doen, klikt u op **Eigenschappen**. Dit kan een paar seconden duren.
9. Klik op het tabblad **Definitie**. De opdrachttekst is:

       SELECT * FROM "HIVE"."default"."hivesampletable"

   Bij de Ranger-beleidsregels hebt u gedefinieerd dat hiveuser1 de machtiging SELECT heeft voor alle kolommen.  Deze query werkt dus met de referenties van hiveuser1, maar deze query werkt niet met de referenties van hiveuser2.

   ![Verbindingseigenschappen][img-hdi-simbahiveodbc-excel-connectionproperties]
10. Klik op **OK** om het dialoogvenster Verbindingseigenschappen te sluiten.
11. Klik op **OK** om het dialoogvenster **Gegevens importeren** te sluiten.  
12. Voer het wachtwoord van hiveuser1 opnieuw in en klik op **OK**. Het duurt een paar seconden voordat de gegevens naar Excel worden geïmporteerd. Wanneer dit is voltooid, worden er 11 kolommen met gegevens weergegeven.

Als u wilt de tweede beleidsregel (read-hivesampletable-devicemake) testen, die u in de laatste sectie hebt gemaakt

1. Voeg een nieuw blad in Excel toe.
2. Voer de vorige procedure uit om de gegevens te importeren.  De enige wijziging die u aanbrengt is het gebruik van referenties van hiveuser2 in plaats van die van hiveuser1. Dit mislukt, omdat hiveuser2 alleen gemachtigd om te zien van twee kolommen is. De volgende fout wordt weergegeven:

        [Microsoft][HiveODBC] (35) Error from Hive: error code: '40000' error message: 'Error while compiling statement: FAILED: HiveAccessControlException Permission denied: user [hiveuser2] does not have [SELECT] privilege on [default/hivesampletable/clientid,country ...]'.
3. Voer dezelfde procedure uit om gegevens te importeren. Gebruik deze keer de referenties van hiveuser2 en wijzig ook de SELECT-instructie van:

        SELECT * FROM "HIVE"."default"."hivesampletable"

    in:

        SELECT clientid, devicemake FROM "HIVE"."default"."hivesampletable"

    Wanneer dit is voltooid, worden er twee kolommen met geïmporteerde gegevens weergegeven.

## <a name="next-steps"></a>Volgende stappen
* Zie voor een HDInsight-cluster configureren met Enterprise-beveiligingspakket, [configureren HDInsight-clusters met ESP](apache-domain-joined-configure.md).
* Zie voor het beheren van een HDInsight-cluster met ESP [beheren HDInsight-clusters met ESP](apache-domain-joined-manage.md).
* Zie voor het uitvoeren van Hive-query's met behulp van SSH op HDInsight-clusters met ESP [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
* Zie voor Hive te verbinden met behulp van Hive JDBC, [verbinding maken met Apache Hive op Azure HDInsight met behulp van het Hive JDBC-stuurprogramma](../hadoop/apache-hadoop-connect-hive-jdbc-driver.md)
* Zie voor Excel verbinden met Hadoop met behulp van Hive ODBC, [Excel verbinding maken met Apache Hadoop met het Microsoft Hive ODBC-station](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md)
* Zie voor Excel verbinden met Hadoop met Power Query, [Excel verbinding maken met Apache Hadoop met Power Query](../hadoop/apache-hadoop-connect-excel-power-query.md)
