---
title: U-SQL-scripts ontwikkelen met Data Lake-tools voor Visual Studio | Microsoft Docs
description: Informatie over het installeren van Data Lake-tools voor Visual Studio en het ontwikkelen en testen van U-SQL-scripts.
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: ad8a6992-02c7-47d4-a108-62fc5a0777a3
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/28/2017
ms.author: saveenr, yanacai
ms.openlocfilehash: a48ce209bf3d5b7e5060acf2850144df5418828d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="develop-u-sql-scripts-by-using-data-lake-tools-for-visual-studio"></a>U-SQL-scripts ontwikkelen met Data Lake-tools voor Visual Studio
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Informatie over het gebruik van de Visual Studio voor het maken van Azure Data Lake Analytics-accounts, het definiëren van taken in [U-SQL](data-lake-analytics-u-sql-get-started.md) en het verzenden van taken naar de Data Lake Analytics-service. Zie [Overzicht van Azure Data Lake Analytics](data-lake-analytics-overview.md) voor meer informatie over Data Lake Analytics.


## <a name="prerequisites"></a>Vereisten

* **Visual Studio**: alle versies behalve Express worden ondersteund.
    * Visual Studio 2017
    * Visual Studio 2015
    * Visual Studio 2013
* **Microsoft Azure SDK voor .NET** versie 2.7.1 of hoger.  U kunt dit installeren met het [webplatforminstallatieprogramma](http://www.microsoft.com/web/downloads/platform.aspx).
* Een **Data Lake Analytics**-account. Zie [Aan de slag met Azure Data Lake Analytics met Azure Portal](data-lake-analytics-get-started-portal.md) om een account te maken.

## <a name="install-azure-data-lake-tools-for-visual-studio"></a>Azure Data Lake-tools voor Visual Studio installeren 

Download en installeer Azure Data Lake-tools voor Visual Studio [via het Downloadcentrum](http://aka.ms/adltoolsvs). Na installatie moet u zich het volgende realiseren:
* Het knooppunt **Server Explorer** > **Azure** bevat een **Data Lake Analytics**-knooppunt. 
* Het menu **Extra** bevat een **Data Lake**-item.

## <a name="connect-to-an-azure-data-lake-analytics-account"></a>Verbinding maken met een Azure Data Lake Analytics-account

1. Open Visual Studio.
2. Open Server Explorer via **Weergave** > **Server Explorer**.
3. Klik met de rechtermuisknop op **Azure**. Selecteer vervolgens **Verbinden met Microsoft Azure-abonnement** en volg de instructies.
4. In Server Explorer selecteert u **Azure** > **Data Lake Analytics**. U ziet een lijst met uw Data Lake Analytics-accounts.


## <a name="write-your-first-u-sql-script"></a>Uw eerste U-SQL-script schrijven

De volgende tekst is een eenvoudig U-SQL-script. Hiermee wordt een kleine gegevensset gedefinieerd en wordt die als het bestand `/data.csv` gegevensset geschreven naar de gebruikelijke Data Lake Store.

```
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

### <a name="submit-a-data-lake-analytics-job"></a>Een Data Lake Analytics-taak verzenden

1. Selecteer **Bestand** > **Nieuw** > **Project**.

2. Selecteer het type **U-SQL Project** en klik op **OK**. Visual Studio maakt een oplossing met een **Script.usql**-bestand.

3. Plak het vorige script in het **Script.usql**-venster.

4. In de linkerbovenhoek van het **Script.usql**-venster geeft u het Data Lake Analytics-account op.

    ![U-SQL Visual Studio-project verzenden](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job.png)

5. In de linkerbovenhoek van het **Script.usql**-venster selecteert u **Verzenden**.
6. Verifieer het **Analytics-account** en selecteer vervolgens **Verzenden**. Het resultaat van het verzenden is beschikbaar in het resultaatvenster van Data Lake Tools voor Visual Studio wanneer het verzenden is voltooid.

    ![U-SQL Visual Studio-project verzenden](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job-advanced.png)
7. U moet op de knop **Vernieuwen** klikken om de status van de meest recente taak weer te geven en het scherm te vernieuwen. Wanneer de taak is voltooid, worden **Taakgrafiek**, **Bewerkingen van metagegevens**, **Statusgeschiedenis** en **Diagnostische gegevens** weergegeven:

    ![Prestatiegrafiek U-SQL Visual Studio Data Lake Analytics-taak](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-performance-graph.png)

   * In **Taakoverzicht** staat een overzicht van de taak.   
   * **Taakgegevens** bevat meer informatie over de taak, waaronder het script, resources en verticalen.
   * In **Taakgrafiek** staat een visualisatie van de voortgang van de taak.
   * **Bewerkingen van metagegevens** toont alle acties die zijn uitgevoerd voor de U-SQL-catalogus.
   * **Gegevens** bevat alle invoer en uitvoer.
   * **Diagnostische gegevens** biedt een geavanceerde analyse van de taakuitvoering en de prestatieoptimalisatie.

### <a name="to-check-job-state"></a>De taakstatus controleren

1. In Server Explorer selecteert u **Azure** > **Data Lake Analytics**. 
2. Vouw de Data Lake Analytics-accountnaam uit.
3. Dubbelklik op **Taken**.
4. Selecteer de taak die u eerder hebt verzonden.

### <a name="to-see-the-output-of-a-job"></a>De uitvoer van een taak bekijken

1. In Server Explorer bladert u naar de taak die u hebt verzonden.
2. Klik op het tabblad **Gegevens**.
3. Op het tabblad **Taakuitvoer** selecteert u het bestand `"/data.csv"`.

## <a name="next-steps"></a>Volgende stappen

* [U-SQL-scripts uitvoeren op uw eigen werkstation voor tests en foutopsporing](data-lake-analytics-data-lake-tools-local-run.md)
* [Fouten in C#-code van U-SQL-taken opsporen met behulp van Azure Data Lake Tools voor Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md)
* [De Azure Data Lake-tools gebruiken voor Visual Studio-code](data-lake-analytics-data-lake-tools-for-vscode.md)
