---
title: Data Lake tools voor Visual Studio met Hortonworks Sandbox - Azure HDInsight | Microsoft Docs
description: Informatie over het gebruik van het Azure Data Lake tools voor Visual Studio met de Hortonworks sandbox uitgevoerd in een lokale virtuele machine. U kunt met deze hulpprogramma's, maken en Hive en Pig-taken uitvoeren op de sandbox, en de taakuitvoer weergeven en de geschiedenis.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: e3434c45-95d1-4b96-ad4c-fb59870e2ff0
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/09/2017
ms.author: larryfr
ms.openlocfilehash: 904bbfcd02984b301d9eb9faaa6203f16139eba5
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2017
---
# <a name="use-the-azure-data-lake-tools-for-visual-studio-with-the-hortonworks-sandbox"></a>Gebruik de Azure Data Lake tools voor Visual Studio met de Hortonworks Sandbox

Azure Data Lake bevat hulpprogramma's voor het werken met algemene Hadoop-clusters. Dit document bevat de stappen die nodig zijn voor het Data Lake tools gebruiken met de Hortonworks Sandbox uitgevoerd op een lokale virtuele machine.

Met behulp van de Hortonworks Sandbox, kunt u werken met Hadoop lokaal op uw ontwikkelomgeving. Nadat u een oplossing hebt ontwikkeld en deze implementeren op grote schaal wilt, kunt u vervolgens verplaatsen naar een HDInsight-cluster.

## <a name="prerequisites"></a>Vereisten

* De Hortonworks Sandbox uitgevoerd in een virtuele machine op uw ontwikkelomgeving. Dit document is geschreven en getest met de sandbox in Oracle VirtualBox uitgevoerd. Zie voor meer informatie over het instellen van de sandbox de [aan de slag met de Hortonworks sandbox.](hadoop/apache-hadoop-emulator-get-started.md) document.

* Visual Studio 2013, Visual Studio 2015 of Visual Studio 2017 (alle versies).

* De [Azure SDK voor .NET](https://azure.microsoft.com/downloads/) 2.7.1 of hoger.

* De [Azure Data Lake tools voor Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).

## <a name="configure-passwords-for-the-sandbox"></a>Wachtwoorden voor de sandbox configureren

Zorg ervoor dat de Hortonworks Sandbox wordt uitgevoerd. Volg de stappen in de [aan de slag in de Hortonworks Sandbox](hadoop/apache-hadoop-emulator-get-started.md#set-sandbox-passwords) document. Deze stappen configureert u het wachtwoord voor de SSH `root` -account en de Ambari `admin` account. Deze wachtwoorden worden gebruikt wanneer u verbinding met de sandbox vanuit Visual Studio maken.

## <a name="connect-the-tools-to-the-sandbox"></a>Verbinding maken met de hulpprogramma's voor de sandbox

1. Open Visual Studio, selecteer **weergave**, en selecteer vervolgens **Server Explorer**.

2. Van **Server Explorer**, met de rechtermuisknop op de **HDInsight** vermelding en selecteer vervolgens **verbinding maken met HDInsight-Emulator**.

    ![Schermopname van Server Explorer, met verbinding maken met HDInsight-Emulator gemarkeerd](./media/hdinsight-hadoop-emulator-visual-studio/connect-emulator.png)

3. Van de **verbinding maken met HDInsight-Emulator** dialoogvenster en voer het wachtwoord dat u hebt geconfigureerd voor Ambari.

    ![Schermopname van dialoogvenster met wachtwoord tekstvak gemarkeerd](./media/hdinsight-hadoop-emulator-visual-studio/enter-ambari-password.png)

    Selecteer **volgende** om door te gaan.

4. Gebruik de **wachtwoord** veld dat u het wachtwoord die u hebt geconfigureerd voor de `root` account. Laat de overige velden op de standaardwaarde.

    ![Schermopname van dialoogvenster met wachtwoord tekstvak gemarkeerd](./media/hdinsight-hadoop-emulator-visual-studio/enter-root-password.png)

    Selecteer **volgende** om door te gaan.

5. Wacht op validatie van de services te voltooien. In sommige gevallen validatie mislukt en vraagt u de configuratie bijwerken. Als validatie mislukt, selecteert u **Update**, en wacht tot de configuratie en verificatie voor de service te voltooien.

    ![Schermopname van dialoogvenster met de knop bijwerken gemarkeerd](./media/hdinsight-hadoop-emulator-visual-studio/fail-and-update.png)

    > [!NOTE]
    > Ambari het updateproces gebruikt voor het wijzigen van de configuratie van de Hortonworks Sandbox wordt verwacht door de Data Lake tools voor Visual Studio.

6. Nadat de validatie is voltooid, selecteert u **voltooien** configuratie wilt voltooien.
    ![Schermopname van dialoogvenster met de knop Voltooien gemarkeerd](./media/hdinsight-hadoop-emulator-visual-studio/finished-connect.png)

     >[!NOTE]
     > Afhankelijk van de snelheid van uw ontwikkelomgeving en de hoeveelheid geheugen toegewezen aan de virtuele machine, het kan enige tijd duren om te configureren en valideren van de services.

Na deze stappen uitvoert, hebt u nu een **lokale HDInsight-cluster** vermelding in Server Explorer, onder de **HDInsight** sectie.

## <a name="write-a-hive-query"></a>Een Hive-query schrijven

Hive biedt een SQL-achtige query language (HiveQL) voor het werken met gestructureerde gegevens. Gebruik de volgende stappen uit voor informatie over het uitvoeren van query's op aanvraag op het lokale cluster.

1. In **Server Explorer**, met de rechtermuisknop op de vermelding voor het lokale cluster die u eerder hebt toegevoegd en selecteer vervolgens **een Hive-Query schrijven**.

    ![Schermopname van Server Explorer en schrijven naar een Hive-Query gemarkeerd](./media/hdinsight-hadoop-emulator-visual-studio/write-hive-query.png)

    Een nieuwe queryvenster wordt weergegeven. Hier kunt u snel schrijven en verzenden van een query met het lokale cluster.

2. Voer de volgende opdracht in het nieuwe queryvenster:

        select count(*) from sample_08;

    De query wilt uitvoeren, selecteert u **indienen** aan de bovenkant van het venster. Laat de andere waarden (**Batch** en servernaam) op de standaardwaarden.

    ![Schermafbeelding van de query-venster met de knop verzenden gemarkeerd](./media/hdinsight-hadoop-emulator-visual-studio/submit-hive.png)

    Ook kunt u de vervolgkeuzelijst naast **indienen** selecteren **Geavanceerd**. Geavanceerde opties kunnen u extra opties te bieden bij het verzenden van de taak.

    ![Schermopname van Submit Script dialoogvenster](./media/hdinsight-hadoop-emulator-visual-studio/advanced-hive.png)

3. Nadat u de query hebt verzonden, wordt de taakstatus wordt weergegeven. Status van de taak geeft informatie weer over de taak terwijl deze wordt verwerkt door Hadoop. **Taakstatus** wordt de status van de taak. De status wordt regelmatig bijgewerkt of kunt u het pictogram Vernieuwen handmatig vernieuwen van de status.

    ![Schermafbeelding van de weergave van de taak in het dialoogvenster met de status van de taak is gemarkeerd](./media/hdinsight-hadoop-emulator-visual-studio/job-state.png)

    Na de **taakstatus** wijzigingen in **voltooid**, een omgeleid acyclische grafiek (DAG) wordt weergegeven. Dit diagram beschrijving van het pad worden uitgevoerd die is bepaald door de Tez bij het verwerken van de Hive-query. Tez is de engine voor het uitvoeren standaard voor Hive op het lokale cluster.

    > [!NOTE]
    > Tez is ook de standaard, wanneer u Linux gebaseerde HDInsight-clusters. Het is niet de standaard op HDInsight op basis van Windows. Om deze te gebruiken, u moet de regel toevoegen `set hive.execution.engine = tez;` aan het begin van uw Hive-query.

    Gebruik de **Taakuitvoer** koppeling om de uitvoer weer te geven. In dit geval is het 823, het aantal rijen in de tabel sample_08. U kunt diagnostische gegevens over de taak weergeven met behulp van de **taaklogboek** en **YARN-logboek downloaden** koppelingen.

4. U kunt ook Hive-taken interactief uitvoeren wijzigen van de **Batch** veld **interactief**. Selecteer vervolgens **Execute**.

    ![Schermopname van interactieve en Execute knoppen gemarkeerd](./media/hdinsight-hadoop-emulator-visual-studio/interactive-query.png)

    Een interactieve query streams het uitvoerlogboek voor gegenereerd tijdens de verwerking naar de **HiveServer2 uitvoer** venster.

    > [!NOTE]
    > De informatie is dezelfde informatie die beschikbaar is via de **taaklogboek** koppelen nadat een taak is voltooid.

    ![Schermopname van het uitvoerlogboek](./media/hdinsight-hadoop-emulator-visual-studio/hiveserver2-output.png)

## <a name="create-a-hive-project"></a>Een Hive-project maken

U kunt ook een project met meerdere Hive-scripts maken. Een project gebruiken wanneer u scripts gerelateerde of opslaan van scripts in een versiebeheersysteem.

1. Selecteer in Visual Studio **bestand**, **nieuw**, en vervolgens **Project**.

2. Vouw in de lijst met projecten **sjablonen**, vouw **Azure Data Lake**, en selecteer vervolgens **HIVE (HDInsight)**. Selecteer in de lijst met sjablonen **Hive-voorbeeldquery**. Voer een naam en locatie en selecteer vervolgens **OK**.

    ![Schermopname van nieuw Project-venster met Azure Data Lake, HIVE, Hive-voorbeeldquery en OK gemarkeerd](./media/hdinsight-hadoop-emulator-visual-studio/new-hive-project.png)

De **Hive-voorbeeldquery** -project bevat twee scripts **WebLogAnalysis.hql** en **SensorDataAnalysis.hql**. U kunt deze scripts verzenden met behulp van dezelfde **indienen** knop aan de bovenkant van het venster.

## <a name="create-a-pig-project"></a>Een Pig-project maken

Terwijl Hive een SQL-achtige taal bevat voor het werken met gestructureerde gegevens, werkt de Pig door transformaties uitvoeren op gegevens. Pig biedt een taal (Pig Latin) waarmee u een pijplijn transformaties ontwikkelen. Als u wilt Pig gebruiken met het lokale cluster, de volgende stappen uit:

1. Open Visual Studio en selecteer **bestand**, **nieuw**, en vervolgens **Project**. Vouw in de lijst met projecten **sjablonen**, vouw **Azure Data Lake**, en selecteer vervolgens **Pig (HDInsight)**. Selecteer in de lijst met sjablonen **Pig toepassing**. Voer een naam en locatie en selecteer vervolgens **OK**.

    ![Schermopname van nieuw Project-venster met Azure Data Lake, Pig, Pig-toepassing en OK gemarkeerd](./media/hdinsight-hadoop-emulator-visual-studio/new-pig.png)

2. Voer de volgende tekst als de inhoud van de **script.pig** -bestand dat is gemaakt met dit project.

        a = LOAD '/demo/data/Website/Website-Logs' AS (
            log_id:int,
            ip_address:chararray,
            date:chararray,
            time:chararray,
            landing_page:chararray,
            source:chararray);
        b = FILTER a BY (log_id > 100);
        c = GROUP b BY ip_address;
        DUMP c;

    Pig gebruikmaakt van een andere taal dan Hive, hoe u de taken uitgevoerd is consistent tussen de beide talen via de **indienen** knop. Als u de vervolgkeuzelijst naast **indienen** wordt een dialoogvenster Geavanceerd verzenden voor Pig.

    ![Schermopname van Submit Script dialoogvenster](./media/hdinsight-hadoop-emulator-visual-studio/advanced-pig.png)

3. De taakstatus en de uitvoer wordt ook weergegeven, hetzelfde zijn als een Hive-query.

    ![Schermafbeelding van een voltooide Pig-taak](./media/hdinsight-hadoop-emulator-visual-studio/completed-pig.png)

## <a name="view-jobs"></a>Taken weergeven

Data Lake tools kunnen u gemakkelijk informatie weergeven over taken die zijn uitgevoerd op Hadoop. Gebruik de volgende stappen uit om de taken die zijn uitgevoerd op het lokale cluster.

1. Van **Server Explorer**, met de rechtermuisknop op het lokale cluster en selecteer vervolgens **taken weergeven**. Een lijst met taken die zijn verzonden naar het cluster wordt weergegeven.

    ![Schermopname van Server Explorer met de weergave taken die zijn gemarkeerd](./media/hdinsight-hadoop-emulator-visual-studio/view-jobs.png)

2. Selecteer in de lijst met taken, een om de taakgegevens weer te geven.

    ![Schermopname van Job Browser met een van de taken die zijn gemarkeerd](./media/hdinsight-hadoop-emulator-visual-studio/view-job-details.png)

    De weergegeven informatie is vergelijkbaar met wat u ziet na het uitvoeren van een query Hive of Pig, met inbegrip van koppelingen voor weergave van de uitvoer en logboekgegevens.

3. U kunt ook wijzigen en de taak hier opnieuw indienen.

## <a name="view-hive-databases"></a>Weergave Hive-databases

1. In **Server Explorer**, vouw de **lokale HDInsight-cluster** -item, en vouw vervolgens **Hive-Databases**. De **standaard** en **xademo** databases op het lokale cluster worden weergegeven. Uitbreiden van een database, ziet u de tabellen in de database.

    ![Schermopname van Server Explorer met uitgebreide databases](./media/hdinsight-hadoop-emulator-visual-studio/expanded-databases.png)

2. Uitbreiden van een tabel, worden de kolommen voor die tabel weergegeven. Met de rechtermuisknop op een tabel om snel de gegevens bekijkt, en selecteer **View Top 100 rijen**.

    ![Schermopname van Server Explorer met tabel uitgebreid en View Top 100 rijen geselecteerd](./media/hdinsight-hadoop-emulator-visual-studio/view-100.png)

### <a name="database-and-table-properties"></a>Eigenschappen van de database en tabel

U kunt de eigenschappen van een database of tabel weergeven. Selecteren **eigenschappen** worden details weergegeven voor het geselecteerde item in het eigenschappenvenster. Bijvoorbeeld, Zie de informatie die wordt weergegeven in de volgende schermafbeelding:

![Schermopname van eigenschappen venster](./media/hdinsight-hadoop-emulator-visual-studio/properties.png)

### <a name="create-a-table"></a>Een tabel maken

Voor het maken van een tabel met de rechtermuisknop op een database en selecteer vervolgens **Create Table**.

![Schermopname van Server Explorer, met Create Table gemarkeerd](./media/hdinsight-hadoop-emulator-visual-studio/create-table.png)

Vervolgens kunt u de tabel die gebruikmaken van een formulier maken. Aan de onderkant van de volgende schermafbeelding ziet u de onbewerkte HiveQL die wordt gebruikt om de tabel te maken.

![Schermopname van het formulier dat wordt gebruikt voor het maken van een tabel](./media/hdinsight-hadoop-emulator-visual-studio/create-table-form.png)

## <a name="next-steps"></a>Volgende stappen

* [De kabels van de Hortonworks Sandbox leren](http://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [Hadoop-zelfstudie - aan de slag met HDP](http://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)
