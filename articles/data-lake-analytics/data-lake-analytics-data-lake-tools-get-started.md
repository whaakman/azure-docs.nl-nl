---
title: Aan de slag met Azure Data Lake Analytics met Visual Studio
description: Informatie over het installeren van Data Lake-tools voor Visual Studio en het ontwikkelen en testen van U-SQL-scripts.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: ad8a6992-02c7-47d4-a108-62fc5a0777a3
ms.topic: conceptual
ms.date: 08/13/2018
ms.openlocfilehash: c41ac2246386b8838d1310a9cd5c03dd0cf5d8e4
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51244505"
---
# <a name="develop-u-sql-scripts-by-using-data-lake-tools-for-visual-studio"></a>U-SQL-scripts ontwikkelen met Data Lake-tools voor Visual Studio
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Informatie over het gebruik van de Visual Studio voor het maken van Azure Data Lake Analytics-accounts, het definiëren van taken in [U-SQL](data-lake-analytics-u-sql-get-started.md) en het verzenden van taken naar de Data Lake Analytics-service. Zie [Overzicht van Azure Data Lake Analytics](data-lake-analytics-overview.md) voor meer informatie over Data Lake Analytics.

>[!IMPORTANT]
> Microsoft raadt u een upgrade uit te voeren naar Azure Data Lake-tools voor Visual Studio versie 2.3.3000.4 of hoger. De vorige versies zijn niet meer beschikbaar om te downloaden en zijn nu afgeschaft. 
>
>**Wat moet ik doen?**
>
>1. Controleer of u een eerdere versie dan 2.3.3000.4 van Azure Data Lake Tools voor Visual Studio gebruikt. 
>   
>   ![Controleer de hulpprogrammaversie](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-about-data-lake.png)
> 
>2. Als u een eerdere versie dan 2.3.3000.4 gebruikt, werkt u uw Azure Data Lake Tools voor Visual Studio bij via het Downloadcentrum: 
>    - [Voor Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=ADLTools.AzureDataLakeandStreamAnalyticsTools)
>    - [Voor Visual Studio 2013 en 2015](https://www.microsoft.com/en-us/download/details.aspx?id=49504)


## <a name="prerequisites"></a>Vereisten

* **Visual Studio**: alle versies behalve Express worden ondersteund.
    * Visual Studio 2017
    * Visual Studio 2015
    * Visual Studio 2013
* **Microsoft Azure SDK voor .NET** versie 2.7.1 of hoger.  U kunt dit installeren met het [webplatforminstallatieprogramma](https://www.microsoft.com/web/downloads/platform.aspx).
* Een **Data Lake Analytics**-account. Zie [Aan de slag met Azure Data Lake Analytics met Azure Portal](data-lake-analytics-get-started-portal.md) om een account te maken.

## <a name="install-azure-data-lake-tools-for-visual-studio"></a>Azure Data Lake-tools voor Visual Studio installeren

Voor deze zelfstudie moet Data Lake-tools voor Visual Studio zijn geïnstalleerd. Volg de [installatie-instructies](data-lake-analytics-data-lake-tools-install.md).

## <a name="connect-to-an-azure-data-lake-analytics-account"></a>Verbinding maken met een Azure Data Lake Analytics-account

1. Open Visual Studio.

2. Open Server Explorer via **Weergave** > **Server Explorer**.

3. Klik met de rechtermuisknop op **Azure**. Selecteer vervolgens **Verbinden met Microsoft Azure-abonnement** en volg de instructies.

4. In Server Explorer selecteert u **Azure** > **Data Lake Analytics**. U ziet een lijst met uw Data Lake Analytics-accounts.

## <a name="write-your-first-u-sql-script"></a>Uw eerste U-SQL-script schrijven

De volgende tekst is een eenvoudig U-SQL-script. Hiermee wordt een kleine gegevensset gedefinieerd en wordt die als het bestand `/data.csv` gegevensset geschreven naar de gebruikelijke Data Lake Store.

```
USE DATABASE master;
USE SCHEMA dbo;
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
```

## <a name="submit-a-data-lake-analytics-job"></a>Een Data Lake Analytics-taak verzenden

1. Selecteer **Bestand** > **Nieuw** > **Project**.

2. Selecteer het type **U-SQL Project** en klik op **OK**. Visual Studio maakt een oplossing met een **Script.usql**-bestand.

3. Plak het vorige script in het **Script.usql**-venster.

4. In de linkerbovenhoek van het **Script.usql**-venster geeft u het Data Lake Analytics-account op.

    ![U-SQL Visual Studio-project verzenden](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job.png)

5. In de linkerbovenhoek van het **Script.usql**-venster selecteert u **Verzenden**.

6. Als de taak is verzonden, wordt het tabblad **Job view** geopend, waarin de voortgang van de taak wordt weergeven. U moet op de knop **Vernieuwen** klikken om de status van de meest recente taak weer te geven en het scherm te vernieuwen.

    ![Prestatiegrafiek U-SQL Visual Studio Data Lake Analytics-taak](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-performance-graph.png)

   * In **Taakoverzicht** staat een overzicht van de taak.   
   * In **Taakgrafiek** staat een visualisatie van de voortgang van de taak.
   * **Bewerkingen van metagegevens** toont alle acties die zijn uitgevoerd voor de U-SQL-catalogus.
   * **Gegevens** bevat alle invoer en uitvoer.
   * **State History** bevat de tijdlijn en de details van de status.
   * **AU Analysis** toont hoeveel AU's er voor de taak zijn gebruikt en u kunt er simulaties mee onderzoeken van verschillende strategieën voor het toewijzen van AU's.
   * **Diagnostische gegevens** biedt een geavanceerde analyse van de taakuitvoering en de prestatieoptimalisatie.

## <a name="check-job-status"></a>Taakstatus controleren

1. In Server Explorer selecteert u **Azure** > **Data Lake Analytics**.

2. Vouw de Data Lake Analytics-accountnaam uit.

3. Dubbelklik op **Taken**.

4. Selecteer de taak die u eerder hebt verzonden.

## <a name="see-the-job-output"></a>Taakuitvoer bekijken

1. In Server Explorer bladert u naar de taak die u hebt verzonden.

2. Klik op het tabblad **Gegevens**.

3. Op het tabblad **Taakuitvoer** selecteert u het bestand `"/data.csv"`.

## <a name="next-steps"></a>Volgende stappen

* [U-SQL-scripts uitvoeren op uw eigen werkstation voor tests en foutopsporing](data-lake-analytics-data-lake-tools-local-run.md)
* [Fouten in C#-code van U-SQL-taken opsporen met behulp van Azure Data Lake Tools voor Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md)
* [De Azure Data Lake-tools gebruiken voor Visual Studio-code](data-lake-analytics-data-lake-tools-for-vscode.md)
