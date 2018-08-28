---
title: Fouten opsporen in terugkerende taken in Azure Data Lake Analytics
description: Informatie over het gebruik van Azure Data Lake Tools voor Visual Studio fouten opsporen in een abnormale terugkerende taak.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 05/20/2018
ms.openlocfilehash: 33c3b91e7bf9fa64e3ba3f98a9396045753d0c2a
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43045691"
---
# <a name="troubleshoot-an-abnormal-recurring-job"></a>Problemen met een abnormale terugkerende taak oplossen

In dit artikel ziet u hoe u [Azure Data Lake Tools voor Visual Studio](http://aka.ms/adltoolsvs) het oplossen van problemen met terugkerende taken. Meer informatie over de pijplijn en terugkerende taken van de [blog Azure Data Lake en Azure HDInsight](https://blogs.msdn.microsoft.com/azuredatalake/2017/09/19/managing-pipeline-recurring-jobs-in-azure-data-lake-analytics-made-easy/).

Terugkerende taken delen meestal dezelfde querylogica en vergelijkbare invoergegevens. Denk bijvoorbeeld aan dat u hebt een terugkerende taak elk etmaal maandag tot en met 8 uur voor het tellen van wekelijkse actieve gebruiker van de afgelopen week. De scripts voor deze taken delen één script-sjabloon die de querylogica bevat. De invoer voor deze taken zijn gegevens over gebruik voor de afgelopen week. Delen de dezelfde querylogica en vergelijkbare invoer doorgaans betekent dat de prestaties van deze taken vergelijkbaar en stabiel. Als een van uw terugkerende taken plotseling abnormaal, mislukt voert of veel vertraagt, kunt u naar:

- Zie de Statistiekenrapporten voor de eerdere uitvoeringen van de periodieke taak om te zien wat is er gebeurd.
- Vergelijk de abnormale taak met een normale om te achterhalen wat is gewijzigd.

**Gerelateerde taakweergave** in Azure Data Lake Tools voor Visual Studio kunt u de voortgang van de oplossen van problemen met beide gevallen versnellen.

## <a name="step-1-find-recurring-jobs-and-open-related-job-view"></a>Stap 1: Terugkerende taken zoeken en openen van verwante taak weergeven

Als u gerelateerde taakweergave als u wilt een terugkerende taak probleem op te lossen, moet u eerst de periodieke taak vinden in Visual Studio en open vervolgens verwante taak weergeven.

### <a name="case-1-you-have-the-url-for-the-recurring-job"></a>Geval 1: U hebt de URL voor de terugkerende taak

Via **extra** > **Data Lake** > **taakweergave**, kunt u de taak-URL taak als weergave wilt openen in Visual Studio plakken. Selecteer **gerelateerde taken weergeven** om verwante taakweergave te openen.

![Koppeling voor weergave van gerelateerde taken in Data Lake Analytics Tools](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/view-related-job.png)
 
### <a name="case-2-you-have-the-pipeline-for-the-recurring-job-but-not-the-url"></a>Geval 2: U hebt de pijplijn voor de periodieke taak, maar niet de URL

In Visual Studio, kunt u pijplijn Browser via Server Explorer openen > uw Azure Data Lake Analytics-account > **pijplijnen**. (Als u dit knooppunt niet in Server Explorer vinden [download de meest recente invoegtoepassing](http://aka.ms/adltoolsvs).) 

![Het knooppunt pijplijnen selecteren](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/pipeline-browser.png)

Alle pijplijnen voor het Data Lake Analytics-account worden in Pipeline-Browser weergegeven aan de linkerkant. U kunt de pijplijnen voor alle terugkerende taken uitvouwen, en vervolgens de optie die problemen heeft. Gerelateerde taakweergave wordt rechts geopend.

![Een pijplijn te selecteren en gerelateerde taakweergave te openen](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-view.png)

## <a name="step-2-analyze-a-statistics-report"></a>Stap 2: Een rapport statistieken analyseren

Een overzicht en een rapport statistieken worden weergegeven boven aan het bijbehorende taak weergeven. Hier vindt u de potentiële hoofdoorzaak van het probleem. 

1.  In het rapport bevat de x-as de tijd van de verzending van taak. Gebruik dit voor het vinden van de afwijkende taak.
2.  Gebruik het proces in het volgende diagram om te controleren van statistieken en Verkrijg inzichten over het probleem en mogelijke oplossingen.

![Procesdiagram voor het controleren van statistieken](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-metrics-debugging-flow.png)

## <a name="step-3-compare-the-abnormal-job-to-a-normal-job"></a>Stap 3: De taak is abnormaal een normale taak vergelijken

U vindt dat alle terugkerende taken via de lijst aan de onderkant van gerelateerde taakweergave verzonden. Ga voor meer inzichten en mogelijke oplossingen, met de rechtermuisknop op de afwijkende taak. De taak Diff-weergave gebruiken om te vergelijken van de afwijkende taak met een eerdere normale.

![Snelmenu voor het vergelijken van taken](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/compare-job.png)

Let op de grote verschillen tussen deze twee taken. Deze verschillen veroorzaken waarschijnlijk de prestatieproblemen. U kunt verder controleren door de stappen in het volgende diagram te gebruiken:

![Procesdiagram voor het controleren van de verschillen tussen taken](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-diff-debugging-flow.png)

## <a name="next-steps"></a>Volgende stappen

* [Gegevensverschil problemen oplossen](data-lake-analytics-data-lake-tools-data-skew-solutions.md)
* [Fouten opsporen in de gebruiker gedefinieerde C#-code voor mislukte U-SQL-taken](data-lake-analytics-debug-u-sql-jobs.md)
