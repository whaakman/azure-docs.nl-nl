---
title: Problemen met Azure Data Lake Analytics-taken via Azure Portal | Microsoft Docs
description: 'Informatie over het gebruik van de Azure Portal oplossen met Data Lake Analytics-taken. '
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: b7066d81-3142-474f-8a34-32b0b39656dc
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: b9c7453cc0a94f70d0098ed83e5f127832065a62
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-azure-data-lake-analytics-jobs-using-azure-portal"></a>Azure Data Lake Analytics-taken via Azure Portal oplossen
Informatie over het gebruik van de Azure Portal oplossen met Data Lake Analytics-taken.

In deze zelfstudie maakt u een ontbrekende bron bestand probleem instellen, en de Azure Portal gebruiken voor het oplossen van het probleem.

## <a name="submit-a-data-lake-analytics-job"></a>Een Data Lake Analytics-taak verzenden

Verzenden van de volgende U-SQL-taak:

```
@searchlog =
   EXTRACT UserId          int,
           Start           DateTime,
           Region          string,
           Query           string,
           Duration        int?,
           Urls            string,
           ClickedUrls     string
   FROM "/Samples/Data/SearchLog.tsv1"
   USING Extractors.Tsv();

OUTPUT @searchlog   
   TO "/output/SearchLog-from-adls.csv"
   USING Outputters.Csv();
```
    
Het bronbestand gedefinieerd in het script **/Samples/Data/SearchLog.tsv1**, dit moet waar **/Samples/Data/SearchLog.tsv**.


## <a name="troubleshoot-the-job"></a>Problemen met de taak oplossen

**Voor een overzicht van alle taken**

1. Klik in de Azure-portal op **Microsoft Azure** in de linkerbovenhoek.
2. Klik op de tegel met de naam van uw Data Lake Analytics-account.  De samenvatting taak wordt weergegeven op de **Jobbeheer** tegel.

    ![Beheer van Azure Data Lake Analytics-taak](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-job-management.png)

    De taak Management biedt u een overzicht van de status van de taak. Er is een mislukte taak.
3. Klik op de **Jobbeheer** tegel om te zien van de taken. De taken zijn onderverdeeld in **met**, **in de wachtrij geplaatst**, en **beëindigd**. U ziet de mislukte taak in de **beëindigd** sectie. Het moet eerste certificaat in de lijst zijn. Wanneer u een groot aantal taken hebt, kunt u **Filter** om u te helpen u bij het zoeken van taken.

    ![Azure Data Lake Analytics-filter-taken](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-filter-jobs.png)
4. Klik op de mislukte taak uit de lijst om de taakdetails openen in een nieuwe blade:

    ![Azure Data Lake Analytics taak is mislukt](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job.png)

    U ziet de **opnieuw indienen** knop. Nadat u het probleem hebt opgelost, kunt u de taak opnieuw indienen.
5. Klik op de gemarkeerde onderdeel in de vorige schermafbeelding details van de fout te openen.  U ziet ongeveer als volgt:

    ![Azure Data Lake Analytics taakdetails is mislukt](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job-details.png)

    Het vertelt u dat de bronmap is niet gevonden.
6. Klik op **Script dubbele**.
7. Update de **FROM** pad naar het volgende:

    ' / Samples/Data/SearchLog.tsv '
8. Klik op **Taak verzenden**.

## <a name="see-also"></a>Zie ook
* [Overzicht van Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Aan de slag met Azure Data Lake Analytics met Azure PowerShell](data-lake-analytics-get-started-powershell.md)
* [Aan de slag met Azure Data Lake Analytics en U-SQL Visual Studio gebruiken](data-lake-analytics-u-sql-get-started.md)
* [Azure Data Lake Analytics beheren met Azure Portal](data-lake-analytics-manage-use-portal.md)
