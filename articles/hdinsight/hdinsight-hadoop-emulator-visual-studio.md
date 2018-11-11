---
title: Data Lake tools voor Visual Studio met Hortonworks Sandbox - Azure HDInsight
description: Informatie over het gebruik van de Azure Data Lake-tools voor Visual Studio met de Hortonworks sandbox die worden uitgevoerd in een lokale virtuele machine. Met deze hulpprogramma's, kunt u maken en uitvoeren van Hive en Pig-taken op de sandbox, en de taakuitvoer weergeven en de geschiedenis.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: hrasheed
ms.openlocfilehash: e6a3fab0e6b3b092ddb55043882c4d284268abfc
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51006844"
---
# <a name="use-the-azure-data-lake-tools-for-visual-studio-with-the-hortonworks-sandbox"></a>Gebruik de Azure Data Lake-tools voor Visual Studio met de Hortonworks Sandbox

Azure Data Lake bevat hulpprogramma's voor het werken met algemene Hadoop-clusters. Dit document bevat de stappen die nodig zijn voor gebruik van de Data Lake-hulpprogramma's met de Hortonworks Sandbox uitgevoerd op een lokale virtuele machine.

Met behulp van de Hortonworks Sandbox, kunt u werken met Hadoop lokaal op uw ontwikkelomgeving. Nadat u hebt een oplossing ontwikkeld en wilt implementeren op schaal, kunt u vervolgens verplaatsen naar een HDInsight-cluster.

## <a name="prerequisites"></a>Vereisten

* De Hortonworks Sandbox uitgevoerd in een virtuele machine op uw ontwikkelomgeving. Dit document is geschreven en getest met de sandbox die worden uitgevoerd in Oracle VirtualBox. Zie voor meer informatie over het instellen van de sandbox de [aan de slag met de Hortonworks sandbox.](hadoop/apache-hadoop-emulator-get-started.md) het document.

* Visual Studio 2013, Visual Studio 2015 of Visual Studio 2017 (alle edities).

* De [Azure SDK voor .NET](https://azure.microsoft.com/downloads/) 2.7.1 of hoger.

* De [Azure Data Lake tools voor Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).

## <a name="configure-passwords-for-the-sandbox"></a>Wachtwoorden voor de sandbox configureren

Zorg ervoor dat de Hortonworks Sandbox wordt uitgevoerd. Volg de stappen in de [aan de slag in de Hortonworks Sandbox](hadoop/apache-hadoop-emulator-get-started.md#set-sandbox-passwords) document. Deze stappen configureert u het wachtwoord voor de SSH `root` -account en de Ambari `admin` account. Deze wachtwoorden worden gebruikt wanneer u verbinding met de sandbox vanuit Visual Studio maken.

## <a name="connect-the-tools-to-the-sandbox"></a>Verbinding maken met de hulpprogramma's naar de sandbox

1. Open Visual Studio, selecteer **weergave**, en selecteer vervolgens **Server Explorer**.

2. Van **Server Explorer**, met de rechtermuisknop op de **HDInsight** , en selecteert u vervolgens **verbinding maken met HDInsight Emulator**.

    ![Schermafbeelding van de Server Explorer, met verbinding maken met HDInsight Emulator gemarkeerd](./media/hdinsight-hadoop-emulator-visual-studio/connect-emulator.png)

3. Uit de **verbinding maken met HDInsight Emulator** dialoogvenster vak, voer het wachtwoord die u hebt geconfigureerd voor Ambari.

    ![Schermopname van dialoogvenster met wachtwoord tekstvak gemarkeerd](./media/hdinsight-hadoop-emulator-visual-studio/enter-ambari-password.png)

    Selecteer **Volgende** om door te gaan.

4. Gebruik de **wachtwoord** veld dat u het wachtwoord die u hebt geconfigureerd voor de `root` account. Laat de andere velden op de standaardwaarde.

    ![Schermopname van dialoogvenster met wachtwoord tekstvak gemarkeerd](./media/hdinsight-hadoop-emulator-visual-studio/enter-root-password.png)

    Selecteer **Volgende** om door te gaan.

5. Wachttijd voor het valideren van de services te voltooien. In sommige gevallen kan validatie mislukt en vraagt u de configuratie bij te werken. Als validatie mislukt, selecteert u **Update**, en wacht tot de configuratie en controle voor de service om te voltooien.

    ![Schermopname van dialoogvenster met bijwerkknop gemarkeerd](./media/hdinsight-hadoop-emulator-visual-studio/fail-and-update.png)

    > [!NOTE]
    > Het updateproces gebruikt Ambari om te wijzigen van de configuratie van Hortonworks Sandbox wat er wordt verwacht door de Data Lake-tools voor Visual Studio.

6. Nadat de validatie is voltooid, schakelt u **voltooien** configuratie wilt voltooien.
    ![Schermopname van dialoogvenster met de knop Voltooien gemarkeerd](./media/hdinsight-hadoop-emulator-visual-studio/finished-connect.png)

     >[!NOTE]
     > Het kan enkele minuten om te configureren en valideren van de services duren, afhankelijk van de snelheid van uw ontwikkelomgeving en de hoeveelheid geheugen toegewezen aan de virtuele machine.

Nadat u deze stappen te volgen, hebt u nu een **lokale HDInsight-cluster** vermelding in Server Explorer, onder de **HDInsight** sectie.

## <a name="write-a-hive-query"></a>Een Hive-query schrijven

Hive biedt een SQL-achtige querytaal (HiveQL) voor het werken met gestructureerde gegevens. Gebruik de volgende stappen uit om te leren hoe u on-demand-query's uitvoeren op het lokale cluster.

1. In **Server Explorer**, met de rechtermuisknop op de vermelding voor het lokale cluster die u eerder hebt toegevoegd, en selecteer vervolgens **een Hive-Query schrijven**.

    ![Schermopname van de Server Explorer en schrijven naar een Hive-Query is gemarkeerd](./media/hdinsight-hadoop-emulator-visual-studio/write-hive-query.png)

    Er wordt een nieuw queryvenster geopend. Hier kunt u snel schrijven en verzenden van een query met het lokale cluster.

2. Voer in het nieuwe queryvenster de volgende opdracht:

        select count(*) from sample_08;

    De query wilt uitvoeren, selecteert u **indienen** aan de bovenkant van het venster. Laat de andere waarden (**Batch** en servernaam) op de standaardwaarden.

    ![Schermafbeelding van de query-venster met de verzendknop gemarkeerd](./media/hdinsight-hadoop-emulator-visual-studio/submit-hive.png)

    U kunt ook de vervolgkeuzelijst naast gebruiken **indienen** selecteren **Geavanceerd**. Geavanceerde opties kunt u extra opties te bieden bij het verzenden van de taak.

    ![Schermafbeelding van Submit Script-dialoogvenster](./media/hdinsight-hadoop-emulator-visual-studio/advanced-hive.png)

3. Nadat u de query hebt ingediend, wordt de taakstatus wordt weergegeven. Status van de taak geeft informatie weer over de taak terwijl deze wordt verwerkt door Hadoop. **Taakstatus** bevat informatie over de status van de taak. De status wordt regelmatig bijgewerkt of kunt u het vernieuwingspictogram op de status handmatig vernieuwen.

    ![Schermafbeelding van de weergave van de taak in het dialoogvenster met de status van de taak is gemarkeerd](./media/hdinsight-hadoop-emulator-visual-studio/job-state.png)

    Na de **taakstatus** wordt gewijzigd in **voltooid**, een omgeleid Acyclic Graph (DAG) wordt weergegeven. In dit diagram beschrijft het uitvoeringspad die is bepaald door de Tez bij het verwerken van de Hive-query. Tez is de standaard-uitvoeringsengine voor Hive op het lokale cluster.

    > [!NOTE]
    > Tez is ook de standaardwaarde als u van HDInsight op basis van Linux-clusters gebruikmaakt. Het is niet de standaard op HDInsight op basis van Windows. Om deze te gebruiken, u moet de regel toevoegen `set hive.execution.engine = tez;` aan het begin van uw Hive-query.

    Gebruik de **Taakuitvoer** koppeling om de uitvoer weer te geven. In dit geval is het 823, het aantal rijen in de tabel sample_08. U kunt diagnostische gegevens over de taak weergeven met behulp van de **taaklogboek** en **YARN-logboek downloaden** koppelingen.

4. U kunt Hive-taken ook interactief uitvoeren met het wijzigen van de **Batch** veld **interactief**. Selecteer vervolgens **Execute**.

    ![Schermafbeelding van interactieve uitvoeren met de knoppen en gemarkeerd](./media/hdinsight-hadoop-emulator-visual-studio/interactive-query.png)

    Een interactieve query streams het uitvoerlogboek voor gegenereerd tijdens de verwerking naar de **HiveServer2 uitvoer** venster.

    > [!NOTE]
    > De informatie is hetzelfde die beschikbaar is in de **taaklogboek** koppelen nadat een taak is voltooid.

    ![Schermafbeelding van het uitvoerlogboek](./media/hdinsight-hadoop-emulator-visual-studio/hiveserver2-output.png)

## <a name="create-a-hive-project"></a>Een Hive-project maken

U kunt ook een project met meerdere Hive-scripts maken. Een project gebruiken wanneer u met scripts gerelateerde of wilt opslaan van scripts in een versie van besturingssysteem.

1. Selecteer in Visual Studio, **bestand**, **nieuw**, en vervolgens **Project**.

2. Vouw in de lijst met projecten **sjablonen**, vouw **Azure Data Lake**, en selecteer vervolgens **HIVE (HDInsight)**. Selecteer in de lijst met sjablonen **Hive voorbeeld**. Voer een naam en locatie, en selecteer vervolgens **OK**.

    ![Schermopname van nieuw Project-venster met Azure Data Lake, HIVE, Hive-voorbeeld en OK gemarkeerd](./media/hdinsight-hadoop-emulator-visual-studio/new-hive-project.png)

De **Hive voorbeeld** -project bevat twee scripts **WebLogAnalysis.hql** en **SensorDataAnalysis.hql**. U kunt deze scripts indienen met behulp van dezelfde **indienen** knop aan de bovenkant van het venster.

## <a name="create-a-pig-project"></a>Een Pig-project maken

Hoewel Hive een SQL-achtige taal biedt voor het werken met gestructureerde gegevens, wordt Pig werkt door het uitvoeren van transformaties op gegevens. Pig biedt een taal (Pig Latin) waarmee u voor het ontwikkelen van een pijplijn van transformaties. Als u wilt Pig gebruiken met het lokale cluster, de volgende stappen uit:

1. Open Visual Studio en selecteer **bestand**, **nieuw**, en vervolgens **Project**. Vouw in de lijst met projecten **sjablonen**, vouw **Azure Data Lake**, en selecteer vervolgens **Pig (HDInsight)**. Selecteer in de lijst met sjablonen **Pig toepassing**. Voer een naam en locatie, en selecteer vervolgens **OK**.

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

    Hoe u de taken worden uitgevoerd terwijl de Pig met een andere taal dan Hive, consistent tussen beide talen, is via de **indienen** knop. Selecteer de vervolgkeuzelijst naast **indienen** wordt een dialoogvenster Geavanceerd verzenden voor Pig.

    ![Schermafbeelding van Submit Script-dialoogvenster](./media/hdinsight-hadoop-emulator-visual-studio/advanced-pig.png)

3. De taakstatus en uitvoer wordt ook weergegeven, gelijk zijn aan een Hive-query.

    ![Schermafbeelding van een voltooide Pig-taak](./media/hdinsight-hadoop-emulator-visual-studio/completed-pig.png)

## <a name="view-jobs"></a>Taken weergeven

Data Lake-tools kunnen u eenvoudig informatie weergeven over taken die zijn uitgevoerd op Hadoop. Gebruik de volgende stappen uit om te zien van de taken die zijn uitgevoerd op het lokale cluster.

1. Van **Server Explorer**, met de rechtermuisknop op het lokale cluster en selecteer vervolgens **taken weergeven**. Een lijst met taken die zijn verzonden naar het cluster wordt weergegeven.

    ![Schermopname van Server Explorer met de weergave taken die zijn gemarkeerd](./media/hdinsight-hadoop-emulator-visual-studio/view-jobs.png)

2. Selecteer in de lijst met taken, een om de taakdetails weer te geven.

    ![Schermopname van Job Browser, met een van de taken die zijn gemarkeerd](./media/hdinsight-hadoop-emulator-visual-studio/view-job-details.png)

    De weergegeven informatie is vergelijkbaar met wat u ziet na het uitvoeren van een query Hive of Pig, inclusief koppelingen naar de weergave van de uitvoer en logboekgegevens.

3. U kunt ook wijzigen en de taak hier opnieuw indienen.

## <a name="view-hive-databases"></a>Hive-databases weergeven

1. In **Server Explorer**, vouw de **lokale HDInsight-cluster** item, en vouw vervolgens **Hive-Databases**. De **standaard** en **xademo** databases op het lokale cluster worden weergegeven. Uitbreiden van een database, ziet u de tabellen in de database.

    ![Schermafbeelding van de Server Explorer, met databases uitgevouwen](./media/hdinsight-hadoop-emulator-visual-studio/expanded-databases.png)

2. Uitbreiden van een tabel, worden de kolommen voor die tabel weergegeven. Als u wilt snel de gegevens bekijken, met de rechtermuisknop op een tabel en selecteer **View Top 100 rijen**.

    ![Schermopname van Server Explorer met de tabel uitgevouwen en View Top 100 rijen geselecteerd](./media/hdinsight-hadoop-emulator-visual-studio/view-100.png)

### <a name="database-and-table-properties"></a>Eigenschappen van de database en tabel

U kunt de eigenschappen van een database of tabel weergeven. Selecteren **eigenschappen** worden details weergegeven voor het geselecteerde item in het eigenschappenvenster. Bijvoorbeeld, Zie de informatie die wordt weergegeven in de volgende schermafbeelding:

![Schermafbeelding van eigenschappenvenster](./media/hdinsight-hadoop-emulator-visual-studio/properties.png)

### <a name="create-a-table"></a>Een tabel maken

Voor het maken van een tabel met de rechtermuisknop op een database en selecteer vervolgens **Create Table**.

![Schermopname van de Server Explorer met Create Table gemarkeerd](./media/hdinsight-hadoop-emulator-visual-studio/create-table.png)

Vervolgens kunt u de tabel met behulp van een formulier maken. Aan de onderkant van de volgende schermafbeelding ziet u de onbewerkte HiveQL die wordt gebruikt om de tabel te maken.

![Schermopname van het formulier dat wordt gebruikt om een tabel te maken](./media/hdinsight-hadoop-emulator-visual-studio/create-table-form.png)

## <a name="next-steps"></a>Volgende stappen

* [Leren werken met de kabels van de Hortonworks Sandbox](http://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [Hadoop-zelfstudie - aan de slag met HDP](http://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)
