---
title: Uw eerste gegevensfactory bouwen (Azure Portal) | Microsoft Docs
description: In deze zelfstudie maakt u een Azure Data Factory-voorbeeldpijplijn met behulp van de Data Factory-editor in Azure Portal.
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: d5b14e9e-e358-45be-943c-5297435d402d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
robots: noindex
ms.openlocfilehash: 19f9686de9face1e53fc84eac23381eadc9fb5cd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-build-your-first-azure-data-factory-using-azure-portal"></a>Zelfstudie: uw eerste Azure-gegevensfactory bouwen met Azure Portal
> [!div class="op_single_selector"]
> * [Overzicht en vereisten](data-factory-build-your-first-pipeline.md)
> * [Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
> * [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Resource Manager-sjabloon](data-factory-build-your-first-pipeline-using-arm.md)
> * [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)


In dit artikel leert u hoe u [Azure Portal](https://portal.azure.com/) gebruikt voor het maken van uw eerste Azure-gegevensfactory. Als u de zelfstudie wilt volgen met andere hulpprogramma's/SDK's, selecteert u een van de opties uit de vervolgkeuzelijst. 

De pijplijn in deze zelfstudie heeft één activiteit: **HDInsight-componentactiviteit**. Deze activiteit voert een Hive-script uit op een Azure HDInsight-cluster dat invoergegevens transformeert om uitvoergegevens te produceren. De pijplijn is gepland on één keer per maand tussen de opgegeven begin- en eindtijd te worden uitgevoerd. 

> [!NOTE]
> Met de gegevenspijplijn in deze zelfstudie worden invoergegevens getransformeerd in uitvoergegevens. Zie [Zelfstudie: gegevens kopiëren van Blob Storage naar SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor informatie over het kopiëren van gegevens met Azure Data Factory.
> 
> Een pijplijn kan meer dan één activiteit hebben. Ook kunt u twee activiteiten koppelen (de ene activiteit na de andere laten uitvoeren) door de uitvoergegevensset van één activiteit in te stellen als invoergegevensset voor een andere activiteit. Zie [Planning en uitvoering in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) voor meer informatie.

## <a name="prerequisites"></a>Vereisten
1. Lees het artikel [Overzicht van de zelfstudie](data-factory-build-your-first-pipeline.md) en voer de **vereiste** stappen uit.
2. Dit artikel biedt geen conceptueel overzicht van de Azure Data Factory-service. Lees voor een gedetailleerd overzicht van de service het artikel [Inleiding tot Azure Data Factory](data-factory-introduction.md).  

## <a name="create-data-factory"></a>Een gegevensfactory maken
Een gegevensfactory kan één of meer pijplijnen hebben. Een pijplijn kan één of meer activiteiten bevatten. Bijvoorbeeld een kopieeractiviteit om gegevens van een bron- naar een doelgegevensopslagplaats te kopiëren en een HDInsight Hive-activiteit om een Hive-script uit te voeren voor het transformeren van invoergegevens naar productuitvoergegevens. U begint in deze stap met het maken van de gegevensfactory.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik op **NIEUW** in het linkermenu en klik vervolgens op **Gegevens en analyses** en **Data Factory**.

   ![Blade maken](./media/data-factory-build-your-first-pipeline-using-editor/create-blade.png)
3. Voer op de blade **Nieuwe gegevensfactory** **GetStartedDF** in als naam.

   ![Blade voor een nieuwe gegevensfactory](./media/data-factory-build-your-first-pipeline-using-editor/new-data-factory-blade.png)

   > [!IMPORTANT]
   > De naam van de Azure-gegevensfactory moet **wereldwijd uniek** zijn. Als u de volgende fout ontvangt: **Naam data factory “GetStartedDF” is niet beschikbaar**. Wijzig de naam van de datafactory (bijvoorbeeld yournameGetStartedDF) en probeer om opnieuw te maken. Raadpleeg het onderwerp [Data Factory - Naamgevingsregels](data-factory-naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
   >
   > De naam van de gegevensfactory wordt in de toekomst mogelijk geregistreerd als **DNS**-naam en wordt daarmee ook voor iedereen zichtbaar.
   >
   >
4. Selecteer het **Azure-abonnement** waarvoor u de gegevensfactory wilt maken.
5. Selecteer een bestaande **resourcegroep** of maak een resourcegroep. Maak voor deze zelfstudie een resourcegroep met de naam **ADFGetStartedRG**.
6. Selecteer de **locatie** voor de gegevensfactory. Alleen regio's die worden ondersteund door de Data Factory-service worden weergegeven in de vervolgkeuzelijst.
7. Selecteer **Vastmaken aan dashboard**. 
8. Klik op de blade **Nieuwe gegevensfactory** op **Maken**.

   > [!IMPORTANT]
   > Als u Data Factory-exemplaren wilt maken, moet u lid zijn van de rol [Inzender Data Factory](../../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) op abonnements-/resourcegroepsniveau.
   >
   >
7. Op het dashboard ziet u de volgende tegel met de status: Gegevensfactory implementeren.    

   ![Status van gegevensfactory maken](./media/data-factory-build-your-first-pipeline-using-editor/creating-data-factory-image.png)
8. Gefeliciteerd. U hebt uw eerste gegevensfactory gemaakt. Nadat de gegevensfactory is gemaakt, ziet u de gegevensfactorypagina. Hierop wordt de inhoud van de gegevensfactory weergegeven.     

    ![Blade Gegevensfactory](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-blade.png)

Voordat u een pijplijn maakt in de gegevensfactory, moet u eerst enkele Data Factory-exemplaren maken. U maakt eerst gekoppelde services om gegevensopslag/berekeningen te koppelen aan uw gegevensopslag, vervolgens definieert u welke invoer- en uitvoergegevenssets de invoer- en uitvoergegevens in de gekoppelde gegevensopslag vertegenwoordigen en daarna maakt u de pijplijn met een activiteit waarvoor gebruik wordt gemaakt van die gegevenssets.

## <a name="create-linked-services"></a>Gekoppelde services maken
In deze stap koppelt u uw Azure Storage-account en een on-demand Azure HDInsight-cluster aan uw gegevensfactory. Het Azure Storage-account bevat de in- en uitvoergegevens van de pijplijn in dit voorbeeld. De gekoppelde HDInsight-service wordt gebruikt om een Hive-script uit te voeren dat is opgegeven in de activiteit van de pijplijn in dit voorbeeld. Geef aan welk(e) [gegevensarchief](data-factory-data-movement-activities.md)/[rekenservices](data-factory-compute-linked-services.md) er in uw scenario worden gebruikt. Koppel die services aan de gegevensfactory door gekoppelde services te maken.  

### <a name="create-azure-storage-linked-service"></a>Een gekoppelde Azure Storage-service maken
In deze stap koppelt u uw Azure Storage-account aan uw gegevensfactory. Voor deze zelfstudie gebruikt u hetzelfde Azure Storage-account om invoer- en uitvoergegevens en het HQL-scriptbestand op te slaan.

1. Klik op de blade **GEGEVENSFACTORY** van **GetStartedDF** op **Maken en implementeren**. U krijgt de Data Factory-editor te zien.

   ![Tegel ontwerpen en implementeren](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-author-deploy.png)
2. Klik op **Nieuwe gegevensopslag** en kies **Azure-opslag**.

   ![Nieuwe gegevensopslag - Azure Storage - menu](./media/data-factory-build-your-first-pipeline-using-editor/new-data-store-azure-storage-menu.png)
3. U ziet het JSON-script voor het maken van een gekoppelde Azure Storage-service in de editor.

   ![Een gekoppelde Azure Storage-service](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
4. Vervang de **accountnaam** door de naam van uw Azure-opslagaccount en vervang de **accountsleutel** door de toegangssleutel van het Azure-opslagaccount. Raadpleeg de informatie over het weergeven, kopiëren en opnieuw genereren van toegangssleutels voor opslag in [Uw opslagaccount beheren](../../storage/common/storage-create-storage-account.md#manage-your-storage-account) als u meer wilt weten over het verkrijgen van een toegangssleutel voor opslag.
5. Klik op de opdrachtbalk op **Implementeren** om de gekoppelde service te implementeren.

    ![Implementatieknop](./media/data-factory-build-your-first-pipeline-using-editor/deploy-button.png)

   Wanneer de gekoppelde service is geïmplementeerd, verdwijnt het venster **Draft-1** en ziet u **AzureStorageLinkedService** in de structuurweergave links.

    ![Een gekoppelde opslagservice in het menu](./media/data-factory-build-your-first-pipeline-using-editor/StorageLinkedServiceInTree.png)    

### <a name="create-azure-hdinsight-linked-service"></a>Een gekoppelde HDInsight-service maken
In deze stap koppelt u een on-demand HDInsight-cluster aan uw gegevensfactory. Het HDInsight-cluster wordt automatisch gemaakt tijdens runtime en wordt verwijderd wanneer het verwerken is voltooid en het cluster gedurende een opgegeven tijdsperiode niet actief is geweest.

1. Klik in de **Data Factory-editor** op **... Meer** en op **Nieuwe berekening**. Selecteer vervolgens **On-demand HDInsight-cluster**.

    ![Nieuwe berekening](./media/data-factory-build-your-first-pipeline-using-editor/new-compute-menu.png)
2. Kopieer het onderstaande codefragment en plak het in het venster **Draft-1**. Het JSON-codefragment bevat de eigenschappen die worden gebruikt voor het maken van het on-demand HDInsight-cluster.

    ```JSON
    {
        "name": "HDInsightOnDemandLinkedService",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.5",
                "clusterSize": 1,
                "timeToLive": "00:05:00",
                "osType": "Linux",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }
    ```

    De volgende tabel bevat beschrijvingen van de JSON-eigenschappen die in het codefragment worden gebruikt:

   | Eigenschap | Beschrijving |
   |:--- |:--- |
   | ClusterSize |Geeft de grootte van het HDInsight-cluster aan. |
   | TimeToLive | Geeft aan hoelang het HDInsight-cluster inactief moet zijn voordat het wordt verwijderd. |
   | linkedServiceName | Geeft het opslagaccount aan dat wordt gebruikt voor het opslaan van de logboeken die door HDInsight worden gegenereerd. |

    Houd rekening met de volgende punten:

   * Met de JSON maakt Data Factory voor u een HDInsight-cluster **op basis van Linux**. Zie [Gekoppelde on-demand HDInsight-service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) voor meer informatie.
   * U kunt **uw eigen HDInsight-cluster** gebruiken in plaats van een on-demand HDInsight-cluster. Zie [Gekoppelde HDInsight-service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) voor meer informatie.
   * Het HDInsight-cluster maakt een **standaardcontainer** in de blobopslag die u hebt opgegeven in de JSON (**linkedServiceName**). HDInsight verwijdert deze container niet wanneer het cluster wordt verwijderd. Dit gedrag is standaard. Met een gekoppelde on-demand HDInsight-service wordt er steeds een HDInsight-cluster gemaakt wanneer er een segment wordt verwerkt, tenzij er een bestaand livecluster is (**timeToLive**). Het cluster wordt verwijderd wanneer het verwerken is voltooid.

       Naarmate er meer segmenten worden verwerkt, verschijnen er meer containers in uw Azure-blobopslag. Als u deze niet nodig hebt voor het oplossen van problemen met taken, kunt u ze verwijderen om de opslagkosten te verlagen. De namen van deze containers worden als volgt opgebouwd: adf**naamvanuwgegevensfactory**-**naamvangekoppeldeservice**-datum-/tijdstempel. Gebruik hulpprogramma's zoals [Microsoft Opslagverkenner](http://storageexplorer.com/) om containers in uw Azure-blobopslag te verwijderen.

     Zie [Gekoppelde on-demand HDInsight-service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) voor meer informatie.
3. Klik op de opdrachtbalk op **Implementeren** om de gekoppelde service te implementeren.

    ![Gekoppelde on-demand HDInsight-service implementeren](./media/data-factory-build-your-first-pipeline-using-editor/ondemand-hdinsight-deploy.png)
4. Controleer of u in de structuurweergave links zowel **AzureStorageLinkedService** als **HDInsightOnDemandLinkedService** ziet.

    ![Structuurweergave met gekoppelde services](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-linked-services.png)

## <a name="create-datasets"></a>Gegevenssets maken
In deze stap maakt u gegevenssets die de invoer- en uitvoergegevens voor Hive-verwerking vertegenwoordigen. Deze gegevenssets verwijzen naar de **AzureStorageLinkedService** die u eerder in deze zelfstudie hebt gemaakt. De gekoppelde service verwijst naar een Azure-opslagaccount en in de gegevenssets vindt u de container, map en bestandsnaam in de opslag van de invoer- en uitvoergegevens.   

### <a name="create-input-dataset"></a>Invoergegevensset maken
1. Klik in de **Data Factory-editor** op **... Meer** op de opdrachtbalk, klik op **Nieuwe gegevensset** en selecteer **Azure-blobopslag**.

    ![Nieuwe gegevensset](./media/data-factory-build-your-first-pipeline-using-editor/new-data-set.png)
2. Kopieer het onderstaande codefragment en plak het in het venster Draft-1. U maakt in het JSON-fragment een gegevensset met de naam **AzureBlobInput** die staat voor invoergegevens van een activiteit in de pijplijn. Daarbij geeft u op dat de invoergegevens zich bevinden in de blobcontainer met de naam **adfgetstarted** en de map met de naam **inputdata**.

    ```JSON
    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "input.log",
                "folderPath": "adfgetstarted/inputdata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }
    ```
    De volgende tabel bevat beschrijvingen van de JSON-eigenschappen die in het codefragment worden gebruikt:

   | Eigenschap | Beschrijving |
   |:--- |:--- |
   | type |De eigenschap type wordt ingesteld op **AzureBlob**, omdat de gegevens zich in een Azure-blobopslag bevinden. |
   | linkedServiceName |Deze eigenschap verwijst naar de **AzureStorageLinkedService** die u eerder hebt gemaakt. |
   | folderPath | Deze eigenschap verwijst naar de blob**container** en de **map** die de blobs voor invoer bevat. | 
   | fileName |Deze eigenschap is optioneel. Als u deze eigenschap niet opgeeft, worden alle bestanden uit folderPath gekozen. In deze zelfstudie wordt alleen de **input.log** verwerkt. |
   | type |Omdat de logboekbestanden tekstbestanden zijn, gebruiken we **TextFormat**. |
   | columnDelimiter |Kolommen in de logboekbestanden worden gescheiden door een **komma (,) (`,`)** |
   | frequency/interval |Als frequency wordt ingesteld op **Month** en het interval **1** is, betekent dit dat de invoersegmenten één keer per maand beschikbaar worden gemaakt. |
   | external | Deze eigenschap wordt ingesteld op **true** als de invoergegevens niet worden gegenereerd door deze pijplijn. In deze zelfstudie wordt het bestand input.log niet gegenereerd door deze pijplijn, daarom hebben we de eigenschap ingesteld op true. |

    Zie het [artikel over Azure Blob-connectoren](data-factory-azure-blob-connector.md#dataset-properties) voor meer informatie over deze JSON-eigenschappen.
3. Klik op de opdrachtbalk op **Implementeren** om de zojuist gemaakte gegevensset te implementeren. U ziet de gegevensset in de structuurweergave links.

### <a name="create-output-dataset"></a>Uitvoergegevensset maken
U maakt nu de uitvoergegevensset die staat voor de uitvoergegevens die worden opgeslagen in de Azure-blobopslag.

1. Klik in de **Data Factory-editor** op **... Meer** op de opdrachtbalk, klik op **Nieuwe gegevensset** en selecteer **Azure-blobopslag**.  
2. Kopieer het onderstaande codefragment en plak het in het venster Draft-1. In het JSON-codefragment maakt u een gegevensset met de naam **AzureBlobOutput** en geeft u op welke gegevensstructuur er door het Hive-script wordt geproduceerd. Bovendien geeft u op dat de resultaten worden opgeslagen in de blobcontainer met de naam **adfgetstarted** en in de map met de naam **partitioneddata**. In het gedeelte **availability** wordt opgegeven dat de uitvoergegevensset op maandelijkse basis wordt geproduceerd.

    ```JSON
    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
          "folderPath": "adfgetstarted/partitioneddata",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "availability": {
          "frequency": "Month",
          "interval": 1
        }
      }
    }
    ```
    Raadpleeg het gedeelte **Invoergegevensset maken** voor een beschrijving van deze eigenschappen. U stelt de externe eigenschap niet in op een uitvoergegevensset, omdat de gegevensset wordt geproduceerd door de Data Factory-service.
3. Klik op de opdrachtbalk op **Implementeren** om de zojuist gemaakte gegevensset te implementeren.
4. Controleer of de gegevensset is gemaakt.

    ![Structuurweergave met gekoppelde services](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-data-set.png)

## <a name="create-pipeline"></a>Pijplijn maken
In deze stap maakt u uw eerste pijplijn met een **HDInsightHive**-activiteit. Het invoersegment wordt elke maand beschikbaar gesteld (frequentie: Maand, interval: 1), evenals het uitvoersegment. Ook de plannereigenschap van de activiteit wordt op maandelijks ingesteld. De instellingen voor de uitvoergegevensset en de activiteitenplanner moeten overeenkomen. Op dit moment wordt de planning gebaseerd op de uitvoergegevensset. Daarom moet u ook een uitvoergegevensset maken als er tijdens de activiteit geen uitvoer wordt geproduceerd. Als er voor de activiteit geen invoer nodig is, kunt u het maken van de invoergegevensset overslaan. De eigenschappen die in de volgende JSON worden gebruikt, worden aan het einde van dit gedeelte beschreven.

1. Klik in de **Data Factory-editor** op **Ellips (...) Meer opdrachten**. Klik vervolgens op **Nieuwe pijplijn**.

    ![Knop Nieuw pijplijn](./media/data-factory-build-your-first-pipeline-using-editor/new-pipeline-button.png)
2. Kopieer het onderstaande codefragment en plak het in het venster Draft-1.

   > [!IMPORTANT]
   > Vervang **storageaccountname** door de naam van uw opslagaccount in de JSON.
   >
   >

    ```JSON
    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "AzureStorageLinkedService",
                        "defines": {
                            "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                            "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AzureBlobInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "policy": {
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "name": "RunSampleHiveActivity",
                    "linkedServiceName": "HDInsightOnDemandLinkedService"
                }
            ],
            "start": "2017-07-01T00:00:00Z",
            "end": "2017-07-02T00:00:00Z",
            "isPaused": false
        }
    }
    ```

    In het JSON-codefragment maakt u een pijplijn die bestaat uit een enkele activiteit waarvoor gebruik wordt gemaakt van Hive om gegevens in een HDInsight-cluster te verwerken.

    Het Hive-scriptbestand **partitionweblogs.hql** wordt opgeslagen in het Azure-opslagaccount (opgegeven door de scriptLinkedService met de naam **AzureStorageLinkedService**) en in de map **script** van de container **adfgetstarted**.

    Het gedeelte **defines** wordt gebruikt om de runtime-instellingen die worden doorgegeven aan het Hive-script op te geven als Hive-configuratiewaarden (bijvoorbeeld ${hiveconf:inputtable}, ${hiveconf:partitionedtable}).

    Met de eigenschappen **start** en **end** van de pijplijn wordt opgegeven in welke periode de pijplijn actief is.

    In de activiteits-JSON geeft u op dat het Hive-script wordt uitgevoerd in de berekening die is opgegeven door de **linkedServiceName** – **HDInsightOnDemandLinkedService**.

   > [!NOTE]
   > Zie 'Pijplijn JSON' in [Pijplijnen en activiteiten in Azure Data Factory](data-factory-create-pipelines.md) voor meer informatie over de JSON-eigenschappen die in het voorbeeld worden gebruikt.
   >
   >
3. Bevestig het volgende:

   1. Het bestand **Input.log** staat in de map **inputdata** van de container **adfgetstarted** in de Azure-blobopslag.
   2. Het bestand **partitionweblogs.hql** staat in de map **script** van de container **adfgetstarted** in de Azure-blobopslag. Voltooi de vereiste stappen in [Overzicht van de zelfstudie](data-factory-build-your-first-pipeline.md) als deze bestanden niet worden weergegeven.
   3. Controleer of u **storageaccountname** hebt vervangen door de naam van uw opslagaccount in de pijplijn-JSON.
4. Klik op de opdrachtbalk op **Implementeren** om de pijplijn te implementeren. Aangezien de tijden voor **start** en **end** in het verleden vallen en **isPaused** is ingesteld op false, wordt de pijplijn (activiteit in de pijplijn) direct na het implementeren uitgevoerd.
5. Controleer of de pijplijn in de structuurweergave wordt weergegeven.

    ![Structuurweergave met de pijplijn](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-pipeline.png)
6. U hebt uw eerste pijplijn gemaakt!

## <a name="monitor-pipeline"></a>De pijplijn bewaken
### <a name="monitor-pipeline-using-diagram-view"></a>De pijplijn bewaken met Diagramweergave
1. Klik op **X** om de blades van de Data Factory-editor te sluiten en om terug te keren naar de Data Factory-blade. Klik op **Diagram**.

    ![Tegel Diagram](./media/data-factory-build-your-first-pipeline-using-editor/diagram-tile.png)
2. In de diagramweergave ziet u een overzicht van de pijplijnen en gegevenssets die voor deze zelfstudie worden gebruikt.

    ![Diagramweergave](./media/data-factory-build-your-first-pipeline-using-editor/diagram-view-2.png)
3. Als u alle activiteiten in de pijplijn wilt weergeven, klikt u met de rechtermuisknop op de pijplijn in het diagram. Klik vervolgens op Pijplijn openen.

    ![Menu Pijplijn openen](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-menu.png)
4. Bevestig dat u de HDInsightHive-activiteit in de pijplijn ziet.

    ![Pijplijnweergave openen](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-view.png)

    Als u naar de vorige weergave wilt terugkeren, klikt u op **Gegevensfactory** in het koppelingenmenu bovenaan.
5. Dubbelklik in de **diagramweergave** op de gegevensset **AzureBlobInput**. Controleer of het segment de status **Gereed** heeft. Het kan enkele minuten duren voordat het segment de status Gereed heeft. Als dit niet is gebeurd nadat u enige tijd hebt gewacht, controleert u of het invoerbestand (input.log) in de juiste container (adfgetstarted) en in de juiste map (inputdata) staat.

   ![Invoersegment met de status Gereed](./media/data-factory-build-your-first-pipeline-using-editor/input-slice-ready.png)
6. Klik op **X** om de blade **AzureBlobInput** te sluiten.
7. Dubbelklik in de **diagramweergave** op de gegevensset **AzureBlobOutput**. U ziet het segment dat momenteel wordt verwerkt.

   ![Gegevensset](./media/data-factory-build-your-first-pipeline-using-editor/dataset-blade.png)
8. Als het verwerken is voltooid, ziet u dat het segment de status **Gereed** heeft.

   ![Gegevensset](./media/data-factory-build-your-first-pipeline-using-editor/dataset-slice-ready.png)  

   > [!IMPORTANT]
   > Het maken van een on-demand HDInsight-cluster duurt normaal gesproken enige tijd (ongeveer 20 minuten). Daarom kunt u ervan uitgaan dat het **ongeveer 30 minuten** duurt voordat het segment in de pijplijn is verwerkt.
   >
   >

9. Wanneer het segment de status **Gereed** heeft, controleert u de map **partitioneddata** in de container **adfgetstarted** in uw blobopslag voor de uitvoergegevens.  

   ![Uitvoergegevens](./media/data-factory-build-your-first-pipeline-using-editor/three-ouptut-files.png)
10. Klik op het segment voor details van het segment in de blade **Gegevenssegment**.

   ![Details gegevenssegment](./media/data-factory-build-your-first-pipeline-using-editor/data-slice-details.png)  
11. Klik in de lijst **Uitvoeringen van activiteit** op een activiteit die wordt uitgevoerd om details van een bepaalde activiteit die wordt uitgevoerd (Hive-activiteit in ons scenario) te bekijken in het venster **Details uitvoering van activiteit**.   

   ![Details uitvoering van activiteit](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-blade.png)    

   In de logboekbestanden ziet u de Hive-query die is uitgevoerd en de statusinformatie. Deze logboeken komen van pas bij het oplossen van problemen.
   Zie [Pijplijnen bewaken en beheren met Azure Portal-blades](data-factory-monitor-manage-pipelines.md) voor meer informatie.

> [!IMPORTANT]
> Het invoerbestand wordt verwijderd zodra het segment is verwerkt. Als u het segment dus opnieuw wilt uitvoeren of als u de zelfstudie opnieuw wilt doorlopen, uploadt u het invoerbestand (input.log) naar de map met invoergegevens van de container adfgetstarted.
>
>

### <a name="monitor-pipeline-using-monitor--manage-app"></a>De pijplijn bewaken met de app Bewaking en beheer
U kunt de toepassing Bewaking en beheer ook gebruiken om uw pijplijnen te bewaken. Zie [Azure Data Factory-pijplijnen bewaken en beheren met de app voor bewaking en beheer](data-factory-monitor-manage-app.md) voor meer informatie over het gebruik van deze toepassing.

1. Klik op de tegel **Bewaking en beheer** op de startpagina van uw gegevensfactory.

    ![De tegel Bewaking en beheer](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-tile.png)
2. De toepassing **Bewaking en beheer** wordt weergegeven. Wijzig **Begintijd** en **Eindtijd** zodanig dat deze overeenkomen met de starttijd en de eindtijd van uw pijplijn. Klik vervolgens op **Toepassen**.

    ![De app Bewaking en beheer](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-app.png)
3. Selecteer een activiteitsvenster in de lijst **Activiteitsvensters** voor details van dit venster.

    ![Details van activiteitsvenster](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-details.png)

## <a name="summary"></a>Samenvatting
In deze zelfstudie hebt u een Azure-gegevensfactory gemaakt voor het verwerken van gegevens. Dit hebt u gedaan door Hive-script uit te voeren op een HDInsight Hadoop-cluster. U hebt in de Azure Portal de Data Factory-editor gebruikt om de volgende stappen uit te voeren:  

1. U hebt een Azure-**gegevensfactory** gemaakt.
2. U hebt twee **gekoppelde services** gemaakt:
   1. Een gekoppelde **Azure Storage**-service om te koppelen aan de Azure-blobopslag die de invoer-/uitvoerbestanden van de gegevensfactory bevat.
   2. Een gekoppelde on-demand **Azure HDInsight**-service om te koppelen aan een on-demand HDInsight Hadoop-cluster van de gegevensfactory. Azure Data Factory maakt op tijd een HDInsight Hadoop-cluster om invoergegevens te verwerken en uitvoergegevens te produceren.
3. U hebt twee **gegevenssets** gemaakt, waarin de invoer- en uitvoergegevens van de HDInsight Hive-activiteit in de pijplijn worden beschreven.
4. U hebt een **pijplijn** gemaakt met een **HDInsight Hive**-activiteit.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u een pijplijn gemaakt met een transformatieactiviteit (HDInsight-activiteit) waarvoor een Hive-script wordt uitgevoerd op een on-demand HDInsight-cluster. Zie [Zelfstudie: gegevens van een Azure-blob kopiëren naar Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor meer informatie over het gebruik van een kopieeractiviteit om gegevens van een Azure-blob te kopiëren naar Azure SQL.

## <a name="see-also"></a>Zie ook
| Onderwerp | Beschrijving |
|:--- |:--- |
| [Pijplijnen](data-factory-create-pipelines.md) |Met behulp van dit artikel krijgt u inzicht in de pijplijnen en activiteiten in Azure Data Factory en in de wijze waarop u deze kunt gebruiken om end-to-end gegevensgestuurde werkstromen te maken voor uw scenario of bedrijf. |
| [Gegevenssets](data-factory-create-datasets.md) |Op basis van dit artikel krijgt u inzicht in de gegevenssets in Azure Data Factory. |
| [Plannen en uitvoeren](data-factory-scheduling-and-execution.md) |In dit artikel wordt uitleg gegeven over de plannings- en uitvoeringsaspecten van het Azure Data Factory-toepassingsmodel. |
| [Pijplijnen bewaken en beheren met de app voor bewaking en beheer](data-factory-monitor-manage-app.md) |In dit artikel wordt beschreven hoe u pijplijnen bewaakt en beheert en hoe u fouten hierin oplost met de app voor bewaking en beheer. |
