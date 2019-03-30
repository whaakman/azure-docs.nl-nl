---
title: Gegevens transformeren met behulp van Databricks in Azure Data Factory | Microsoft Docs
description: Informatie over het gebruik van een oplossingssjabloon om gegevens te transformeren met behulp van een Databricks-notebook in Azure Data Factory.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/10/2018
ms.author: abnarain
ms.reviewer: douglasl
ms.openlocfilehash: 562ce675acc43002ce468d60f8a8c412410be86c
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58630811"
---
# <a name="transform-data-by-using-databricks-in-azure-data-factory"></a>Gegevens transformeren met behulp van Databricks in Azure Data Factory

In deze zelfstudie maakt u een end-to-end-pijplijn met **Lookup**, **kopie**, en **Databricks notebook** activiteiten in Data Factory.

-   **Lookup** of de activiteit GetMetadata wordt gebruikt om te controleren of de brongegevensset is gereed voor downstream gebruik, voordat de kopiëren en analytics-taak wordt geactiveerd.

-   **Kopieeractiviteit** kopieert het bronbestand / gegevensset naar de sink-opslag. De sink-opslag is gekoppeld als DBFS in de Databricks notebook zodat de gegevensset kan rechtstreeks worden gebruikt door Spark.

-   **Databricks notebook-activiteit** activeert de Databricks-notebook die de gegevensset worden getransformeerd en voegt deze toe aan een verwerkte map / SQL DW.

Deze sjabloon om eenvoudig te houden, maakt de sjabloon niet een geplande trigger. U kunt die indien nodig toevoegen.

![1](media/solution-template-Databricks-notebook/Databricks-tutorial-image01.png)

## <a name="prerequisites"></a>Vereisten

1.  Maak een **blob storage-account** en een container met de naam `sinkdata` moet worden gebruikt als **sink**. Maak een notitie van de **opslagaccountnaam**, **containernaam**, en **toegangssleutel**, omdat ze later in de sjabloon wordt verwezen.

2.  Zorg ervoor dat u hebt een **Azure Databricks-werkruimte** of maak een nieuwe.

1.  **Het importeren van de notebook voor ETL**. Importeren van de onderstaande transformatie-notebook geïnstrueerd om de Databricks-werkruimte. (Deze hoeft niet te worden op dezelfde locatie als hieronder, maar vergeet niet dat het pad dat u voor later kiest.) De notebook uit de volgende URL importeren door te voeren van deze URL in het URL-veld: `https://adflabstaging1.blob.core.windows.net/share/Transformations.html`. Selecteer **importeren**.

    ![2](media/solution-template-Databricks-notebook/Databricks-tutorial-image02.png)

    ![3](media/solution-template-Databricks-notebook/Databricks-tutorial-image03.png)  

1.  Nu gaan we werken de **transformatie** laptop met uw **opslag verbindingsgegevens** (de naam en toegangssleutel). Ga naar **opdracht 5** in de geïmporteerde laptop die hierboven, vervang deze door het onderstaande codefragment nadat de gemarkeerde waarden vervangen. Zorg ervoor dat dit account is hetzelfde opslagaccount eerder hebt gemaakt en bevat de `sinkdata` container.

    ```python
    # Supply storageName and accessKey values  
    storageName = "<storage name>"  
    accessKey = "<access key>"  

    try:  
      dbutils.fs.mount(  
        source = "wasbs://sinkdata\@"+storageName+".blob.core.windows.net/",  
        mount_point = "/mnt/Data Factorydata",  
        extra_configs = {"fs.azure.account.key."+storageName+".blob.core.windows.net": accessKey})  

    except Exception as e:  
      # The error message has a long stack track. This code tries to print just the relevant line indicating what failed.

    import re
    result = re.findall(r"\^\s\*Caused by:\s*\S+:\s\*(.*)\$", e.message, flags=re.MULTILINE)
    if result:
      print result[-1] \# Print only the relevant error message
    else:  
      print e \# Otherwise print the whole stack trace.  
    ```

1.  Genereren van een **Databricks-toegangstoken** voor Data Factory voor toegang tot Databricks. **Opslaan van het toegangstoken** voor later gebruik bij het maken van een Databricks gekoppelde service, die er ongeveer als volgt 'dapi32db32cbb4w6eee18b7d87e45exxxxxx'

    ![4](media/solution-template-Databricks-notebook/Databricks-tutorial-image04.png)

    ![5](media/solution-template-Databricks-notebook/Databricks-tutorial-image05.png)

## <a name="create-linked-services-and-datasets"></a>Gekoppelde services en gegevenssets maken

1.  Maak een nieuwe **gekoppelde services** in de gebruikersinterface van Data Factory door te gaan naar *verbindingen gekoppelde services + nieuwe*

    1.  **Bron** – voor toegang tot de brongegevens. U kunt de openbare blob-opslag met de bronbestanden voor dit voorbeeld gebruiken.

        Selecteer **Blob-opslag**, gebruik de onderstaande **SAS-URI** verbinding maken met bronopslag (alleen-lezen-toegang).

        `https://storagewithdata.blob.core.windows.net/?sv=2017-11-09&ss=b&srt=sco&sp=rl&se=2019-12-31T21:40:53Z&st=2018-10-24T13:40:53Z&spr=https&sig=K8nRio7c4xMLnUV0wWVAmqr5H4P3JDwBaG9HCevI7kU%3D`

        ![6](media/solution-template-Databricks-notebook/Databricks-tutorial-image06.png)

    1.  **Sink-** – voor het kopiëren van gegevens in.

        Selecteer een opslag die in de vereiste 1, in de sink gekoppelde service gemaakt.

        ![7](media/solution-template-Databricks-notebook/Databricks-tutorial-image07.png)

    1.  **Databricks** – voor het verbinding maken met Databricks-cluster

        Maak een aan Databricks gekoppelde service met behulp van de toegangssleutel in de vereiste 2.c gegenereerd. Als u hebt een *interactieve cluster*, kunt u die selecteren. (In dit voorbeeld wordt de *nieuwe taakcluster* optie.)

        ![8](media/solution-template-Databricks-notebook/Databricks-tutorial-image08.png)

2.  Maak **gegevenssets**

    1.  Maak **'sourceAvailability_Dataset'** om te controleren of de brongegevens beschikbaar is

    ![9](media/solution-template-Databricks-notebook/Databricks-tutorial-image09.png)

    1.  **Brongegevensset-** voor het kopiëren van de brongegevens (met behulp van de binaire kopie)

    ![10](media/solution-template-Databricks-notebook/Databricks-tutorial-image10.png)

    1.  **Sink-gegevensset** – voor het kopiëren naar het sink / doellocatie

        1.  Linked service - select 'sinkBlob_LS' created in 1.b

        2.  Bestandspad - sinkdata/staged_sink

        ![11](media/solution-template-Databricks-notebook/Databricks-tutorial-image11.png)

## <a name="create-activities"></a>Activiteiten maken

1.  Maak een Lookup-activiteit '**beschikbaarheid vlag**' voor het uitvoeren van een controle op beschikbaarheid van de bron (Lookup- of GetMetadata kan worden gebruikt). Selecteer sourceAvailability_Dataset in 2.a gemaakt.

    ![12](media/solution-template-Databricks-notebook/Databricks-tutorial-image12.png)

1.  Maak een kopieeractiviteit '**-bestand naar blob**' voor het kopiëren van de gegevensset van bron naar een sink. De gegevens zijn in dit geval binaire bestand. Naslaginformatie over de onderstaande schermafbeeldingen voor bron en sink-configuraties in de kopieeractiviteit.

    ![13](media/solution-template-Databricks-notebook/Databricks-tutorial-image13.png)

    ![14](media/solution-template-Databricks-notebook/Databricks-tutorial-image14.png)

1.  Definieer **pipeline-parameters**

    ![15](media/solution-template-Databricks-notebook/Databricks-tutorial-image15.png)

1.  Maak een **Databricks-activiteit**

    Selecteer de gekoppelde service in een vorige stap hebt gemaakt.

    ![16](media/solution-template-Databricks-notebook/Databricks-tutorial-image16.png)

    Configureer de **instellingen**. Maak **Base Parameters** zoals wordt weergegeven in de schermafbeelding en parameters worden doorgegeven aan de Databricks-notebook van Data Factory te maken. Bladeren en **Selecteer** de **pad van het juiste notitieblok** geüpload **vereiste 2**.

    ![17](media/solution-template-Databricks-notebook/Databricks-tutorial-image17.png)

1.  **Voer de pijplijn**. U vindt de koppeling naar de Databricks-logboeken voor meer gedetailleerde Spark-Logboeken.

    ![18](media/solution-template-Databricks-notebook/Databricks-tutorial-image18.png)

    U kunt ook het bestand met behulp van Opslagverkenner controleren. (Voor het correleren met Data Factory-pijplijn wordt uitgevoerd, in dit voorbeeld voegt uitvoering van de ID van data factory naar de map voor uitvoer van de pijplijn. Op deze manier kunt u weer de bestanden bijhouden die is gegenereerd via elke uitvoering.)

![19](media/solution-template-Databricks-notebook/Databricks-tutorial-image19.png)

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Data Factory](introduction.md)
