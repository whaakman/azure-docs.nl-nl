---
title: 'Quickstart: Een Spark-taak uitvoeren op Azure Databricks met Azure portal | Microsoft Docs'
description: De quickstart laat zien hoe er met Azure Portal een Azure Databricks-werkruimte en een Apache Spark-cluster kan worden gemaakt, en hoe een Spark-taak kan worden uitgevoerd.
services: azure-databricks
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 03/09/2018
ms.author: nitinme
ms.custom: mvc
ms.openlocfilehash: 9eff06934eefa44db94de3d01be470ca69a2d88c
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/17/2018
---
# <a name="quickstart-run-a-spark-job-on-azure-databricks-using-the-azure-portal"></a>Quickstart: Een Spark-taak uitvoeren op Azure Databricks met Azure portal

Deze quickstart laat zien hoe een Azure Databricks-werkruimte wordt gemaakt en hoe binnen die werkruimte een Apache Spark-cluster kan worden gemaakt. Tot slot leert u hoe u een Spark-taak op het Databricks-cluster kunt uitvoeren. Zie [Wat is Azure Databricks?](what-is-azure-databricks.md) voor meer informatie over Azure Databricks.

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com).

## <a name="create-a-databricks-workspace"></a>Een Databricks-werkruimte maken

In deze sectie gaat u een Azure Databricks-werkruimte maken met behulp van Azure Portal. 

1. Klik in Azure Portal op **Een resource maken**, klik op **Gegevens en analyses** en klik vervolgens op **Azure Databricks (preview)**. 

    ![Databricks in Azure Portal](./media/quickstart-create-databricks-workspace-portal/azure-databricks-on-portal.png "Databricks in Azure Portal")

2. Onder **Azure Databricks (preview)** klikt u op **Maken**.

3. Geef bij **Azure Databricks Service** de waarden op voor het maken van een Databricks-werkruimte.

    ![Een Azure Databricks-werkruimte maken](./media/quickstart-create-databricks-workspace-portal/create-databricks-workspace.png "Een Azure Databricks-werkruimte maken")

    Geef de volgende waarden op: 
     
    |Eigenschap  |Beschrijving  |
    |---------|---------|
    |**Werkruimtenaam**     | Geef een naam op voor uw Databricks-werkruimte.        |
    |**Abonnement**     | Selecteer uw Azure-abonnement in de vervolgkeuzelijst.        |
    |**Resourcegroep**     | Geef aan of u een nieuwe resourcegroep wilt maken of een bestaande groep wilt gebruiken. Een resourcegroep is een container met gerelateerde resources voor een Azure-oplossing. Zie [Overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) voor meer informatie. |
    |**Locatie**     | Selecteer **VS - oost 2**. Zie [Producten beschikbaar per regio](https://azure.microsoft.com/regions/services/) voor andere beschikbare regio's.        |
    |**Prijscategorie**     |  U kunt kiezen tussen **Standard** en **Premium**. Bekijk de pagina [Prijzen voor Databricks](https://azure.microsoft.com/pricing/details/databricks/) voor meer informatie over deze categorieën.       |

    Selecteer **Vastmaken aan dashboard** en klik op **Maken**.

4. Het duurt enkele minuten om het account te maken. Tijdens het maken van het account wordt rechts in de portal de tegel **Implementatie verzenden voor Azure Databricks** weergegeven. Mogelijk moet u op uw dashboard naar rechts scrollen om de tegel te zien. Bovenaan het scherm wordt ook een voortgangsbalk weergegeven. U kunt beide gebieden bekijken voor de voortgang.

    ![Tegel Databricks-implementatie](./media/quickstart-create-databricks-workspace-portal/databricks-deployment-tile.png "Tegel Databricks-implementatie")

## <a name="create-a-spark-cluster-in-databricks"></a>Een Spark-cluster maken in Databricks

1. Ga in Azure Portal naar de Databricks-werkruimte die u hebt gemaakt en klik op **Werkruimte starten**.

2. U wordt omgeleid naar de Azure Databricks-portal. Klik in de portal op **Cluster**.

    ![Databricks in Azure](./media/quickstart-create-databricks-workspace-portal/databricks-on-azure.png "Databricks in Azure")

3. Op de pagina **Nieuw cluster** geeft u de waarden op waarmee een nieuw cluster wordt gemaakt.

    ![Een Databricks Spark-cluster maken in Azure](./media/quickstart-create-databricks-workspace-portal/create-databricks-spark-cluster.png "Een Databricks Spark-cluster maken in Azure")

    * Voer een naam in voor het cluster.
    * Voor dit artikel maakt u een cluster met een **4.0**-runtime. 
    * Zorg ervoor dat u het selectievakje **Beëindigen na ____ minuten van inactiviteit** inschakelt. Geef een duur (in minuten) op waarna het cluster moet worden beëindigd als het niet wordt gebruikt.
    * Accepteer alle overige standaardwaarden. 
    * Klik op **Cluster maken**. Zodra het cluster wordt uitgevoerd, kunt u notitieblokken koppelen aan het cluster en Spark-taken uitvoeren.

Zie [Een Spark-cluster maken in Azure Databricks](https://docs.azuredatabricks.net/user-guide/clusters/create.html) voor meer informatie over het maken van clusters.

## <a name="run-a-spark-sql-job"></a>Een Spark SQL-taak uitvoeren

Voordat u met deze sectie begint, moet u eerst aan de volgende vereisten voldoen:

* [Maak een Azure Storage-account](../storage/common/storage-create-storage-account.md#create-a-storage-account). 
* Download een voorbeeld-JSON-bestand [vanuit Github](https://github.com/Azure/usql/blob/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json). 
* Upload het voorbeeld-JSON-bestand naar het Azure Storage-account dat u hebt gemaakt. U kunt [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) gebruiken om bestanden te uploaden.

Voer de volgende taken uit om een notitieblok in Databricks te maken. Configureer het notitieblok om de gegevens te lezen uit een Azure Blob-opslagaccount en voer vervolgens een Spark SQL-taak uit op de gegevens.

1. Klik in het linkerdeelvenster op **Werkruimte**. Klik in de vervolgkeuzelijst **Werkruimte** op **Maken** en klik vervolgens op **Notitieblok**.

    ![Een notitieblok maken in Databricks](./media/quickstart-create-databricks-workspace-portal/databricks-create-notebook.png "Een notitieblok maken in Databricks")

2. Voer in het dialoogvenster **Notitieblok maken** een naam in, selecteer **Scala** als taal en selecteer het Spark-cluster dat u eerder hebt gemaakt.

    ![Een notitieblok maken in Databricks](./media/quickstart-create-databricks-workspace-portal/databricks-notebook-details.png "Een notitieblok maken in Databricks")

    Klik op **Create**.

3. In deze stap koppelt u het Storage-account aan het Databricks Apache Spark-cluster. U kunt dat op twee manieren doen. Koppel het Storage-account rechtstreeks aan het Databricks Filesystem (DBFS) of open het Storage-account rechtstreeks vanuit de toepassing die u maakt.  

    > [!IMPORTANT]
    >In dit artikel wordt gebruikgemaakt van de **methode waarbij de opslag aan DBFS wordt gekoppeld**. Hierdoor wordt gegarandeerd dat de gekoppelde opslag aan het bestandssysteem van het cluster zelf wordt gekoppeld. Vandaar dat een toepassing die toegang tot het cluster heeft, ook gebruik kan maken van de gekoppelde opslag. De methode met de rechtstreekse toegang is beperkt tot de toepassing van waaruit u de toegang configureert.
    >
    > Als u de methode met de koppeling wilt gebruiken, moet u een Spark-cluster maken met Databricks-runtimeversie **4.0** (de versie die u in dit artikel hebt gekozen).

    In het volgende codefragment vervangt u `{YOUR CONTAINER NAME}`, `{YOUR STORAGE ACCOUNT NAME}` en `{YOUR STORAGE ACCOUNT ACCESS KEY}` door de desbetreffende waarden voor uw Azure Storage-account. Plak het volgende codefragment in een lege cel en druk op Shift+Enter om de codecel uit te voeren.

    * **Het opslagaccount koppelen aan DBFS (aanbevolen)**. Met dit codefragment wordt het pad voor het Azure Storage-account gekoppeld aan `/mnt/mypath`. Als u later het Azure Storage-account opent, hoeft u het volledige pad niet meer op te geven. U kunt gewoon `/mnt/mypath` gebruiken.

          dbutils.fs.mount(
            source = "wasbs://{YOUR CONTAINER NAME}@{YOUR STORAGE ACCOUNT NAME}.blob.core.windows.net/",
            mountPoint = "/mnt/mypath",
            extraConfigs = Map("fs.azure.account.key.{YOUR STORAGE ACCOUNT NAME}.blob.core.windows.net" -> "{YOUR STORAGE ACCOUNT ACCESS KEY}"))

    * **Het opslagaccount rechtstreeks openen**

          spark.conf.set("fs.azure.account.key.{YOUR STORAGE ACCOUNT NAME}.blob.core.windows.net", "{YOUR STORAGE ACCOUNT ACCESS KEY}")

    Zie [Toegangssleutels beheren voor uw opslagaccount](../storage/common/storage-create-storage-account.md#manage-your-storage-account) voor instructies over het ophalen van de toegangssleutel voor uw opslagaccount.

    > [!NOTE]
    > U kunt ook Azure Data Lake Store met een Spark-cluster in Azure Databricks gebruiken. Zie [Data Lake Store met Azure Databricks gebruiken](https://go.microsoft.com/fwlink/?linkid=864084) voor instructies.

4. Voer een SQL-instructie uit om een tijdelijke tabel te maken met gegevens uit het JSON-voorbeeldgegevensbestand, **small_radio_json.json**. Vervang in het volgende codefragment de tijdelijke aanduidingen voor waarden door de containernaam en de naam van het opslagaccount. Plak het codefragment in een lege codecel in het notitieblok en druk op Shift+Enter. In het codefragment duidt `path` de locatie aan van het voorbeeld-JSON-bestand dat u hebt geüpload naar uw Azure Storage-account.

    ```sql
    %sql 
    DROP TABLE IF EXISTS radio_sample_data
    CREATE TABLE radio_sample_data
    USING json
    OPTIONS (
     path "/mnt/mypath/small_radio_json.json"
    )
    ```

    Zodra de opdracht is voltooid, hebt u alle gegevens van het JSON-bestand als een tabel in het Databricks-cluster.

    De magic-opdracht in de taal `%sql` stelt u in staat u SQL-code uit te voeren vanuit het notitieblok, zelfs als het notitieblok van een ander type is. Zie [Talen combineren in een notitieblok](https://docs.azuredatabricks.net/user-guide/notebooks/index.html#mixing-languages-in-a-notebook) voor meer informatie.

5. Laten we eens een momentopname bekijken van de voorbeeld-JSON-gegevens om een beter begrip te krijgen van de query die wordt uitgevoerd. Plak het volgende codefragment in de codecel en druk op **Shift+Enter**.

    ```sql
    %sql 
    SELECT * from radio_sample_data
    ```

6. U ziet uitvoer in tabelvorm zoals weergegeven in de volgende schermafbeelding (alleen bepaalde kolommen worden weergegeven):

    ![Voorbeeld-JSON-gegevens](./media/quickstart-create-databricks-workspace-portal/databricks-sample-csv-data.png "Voorbeeld JSON-gegevens")

    Naast andere gegevens, worden in de voorbeeldgegevens ook het geslacht van de doelgroep van een radiokanaal vastgelegd (kolomnaam **geslacht**) en of het een gratis of een betaald abonnement is (kolomnaam **niveau**).

7. U gaat nu een visuele weergave van deze gegevens maken om voor elk geslacht te zien kunnen hoeveel gebruikers een gratis account hebben en hoeveel een betaald. Klik onder in de tabel met uitvoer op het pictogram voor het **staafdiagram** en klik vervolgens op **Tekenopties**.

    ![Staafdiagram maken](./media/quickstart-create-databricks-workspace-portal/create-plots-databricks-notebook.png "Staafdiagram maken")

8. In **Tekening aanpassen** sleept en zet u de waarden neer zoals in de schermafbeelding wordt weergegeven.

    ![Staafdiagram aanpassen](./media/quickstart-create-databricks-workspace-portal/databricks-notebook-customize-plot.png "Staafdiagram aanpassen")

    * Stel **Sleutels** in op **geslacht**.
    * Stel **Reeksgroeperingen** in op **niveau**.
    * Stel **Waarden** in op **niveau**.
    * Stel **Aggregatie** in op **AANTAL**.

    Klik op **Toepassen**.

9. De uitvoer toont de visuele weergave zoals de volgende schermafbeelding laat zien:

     ![Staafdiagram aanpassen](./media/quickstart-create-databricks-workspace-portal/databricks-sql-query-output-bar-chart.png "Staafdiagram aanpassen")

## <a name="clean-up-resources"></a>Resources opschonen

Nadat u het artikel hebt doorgenomen, kunt u het cluster beëindigen. Dit doet u door vanuit de Azure Databricks-werkruimte in het linkerdeelvenster **Clusters** te selecteren. Voor het cluster dat u wilt beëindigen, plaatst u de cursor op het weglatingsteken onder de kolom **Acties** en selecteert u het **beëindigingspictogram**.

![Een Databricks-cluster stopzetten](./media/quickstart-create-databricks-workspace-portal/terminate-databricks-cluster.png "Een Databricks-cluster stopzetten")

Als u het cluster niet handmatig beëindigt, stopt het cluster automatisch, op voorwaarde dat het selectievakje **Beëindigen na __ minuten inactiviteit** is ingeschakeld tijdens het maken van het cluster. In dat geval stopt het cluster automatisch als het gedurende de opgegeven tijd inactief is geweest.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een Spark-cluster in Azure Databricks gemaakt en een Spark-taak met gegevens in Azure Storage uitgevoerd. U kunt ook zoeken op [Spark gegevensbronnen](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html) voor meer informatie over het importeren van gegevens uit andere gegevensbronnen in Azure Databricks. Ga naar het volgende artikel voor informatie over het streamen van gegevens naar Azure Databricks met behulp van Event Hubs.

> [!div class="nextstepaction"]
>[Gegevens streamen naar Azure Databricks met behulp van Event Hubs](databricks-stream-from-eventhubs.md)
