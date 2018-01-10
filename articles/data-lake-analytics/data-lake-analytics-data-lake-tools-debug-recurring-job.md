---
title: Problemen met een abnormale terugkerende taak oplossen | Microsoft Docs
description: Informatie over het gebruik van Azure Data Lake Tools voor Visual Studio fouten opsporen in een abnormale terugkerende taak.
services: data-lake-analytics
documentationcenter: 
author: yanancai
manager: 
editor: 
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/27/2017
ms.author: yanacai
ms.openlocfilehash: 9b60c861810d6577b33aa0cdf14f26dc2cfc0e4d
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="troubleshoot-an-abnormal-recurring-job"></a>Problemen met een abnormale terugkerende taak oplossen

Dit artikel laat zien hoe u [Azure Data Lake Tools voor Visual Studio](http://aka.ms/adltoolsvs) oplossen van problemen met terugkerende taken. Meer informatie over de pijplijn en terugkerende taken van de [Azure Data Lake en Azure HDInsight blog](https://blogs.msdn.microsoft.com/azuredatalake/2017/09/19/managing-pipeline-recurring-jobs-in-azure-data-lake-analytics-made-easy/).

Terugkerende taken worden meestal dezelfde logica van de query en vergelijkbare invoergegevens delen. Stel bijvoorbeeld dat u hebt een terugkerende taak elk etmaal maandag 8 uur uitgevoerd voor het tellen van afgelopen week wekelijkse actieve gebruiker. De scripts voor deze taken delen één script sjabloon de logica van de query bevat. De invoer voor deze taken zijn de gebruiksgegevens voor vorige week. Delen de dezelfde query-logica en vergelijkbare invoer meestal betekent dat de prestaties van deze taken vergelijkbare en stabiele. Als een van uw terugkerende taken plotseling abnormaal, mislukt voert of veel trager, u mogelijk wilt:

- Zie de Statistiekenrapporten voor de vorige wordt uitgevoerd van de periodieke taak om te zien wat er gebeurd is.
- Vergelijk de abnormale taak met een normale om te achterhalen wat is gewijzigd.

**Taak weergeven gerelateerd** in Azure Data Lake Tools voor Visual Studio kunt u de voortgang van de problemen met beide gevallen versnellen.

## <a name="step-1-find-recurring-jobs-and-open-related-job-view"></a>Stap 1: Terugkerende taken vinden en openen van verwante taak weergeven

Verwante taak als weergave wilt gebruiken een terugkerende taak probleem op te lossen, moet u eerst de terugkerende taak niet vinden in Visual Studio en open vervolgens gerelateerde taak weergeven.

### <a name="case-1-you-have-the-url-for-the-recurring-job"></a>Voorbeeld 1: U hebt de URL voor de terugkerende taak

Via **extra** > **Data Lake** > **taakweergave**, kunt u de URL van de taak taak als weergave wilt openen in Visual Studio plakken. Selecteer **gerelateerde taken weergeven** om verwante taakweergave te openen.

![Koppeling voor weergave van gerelateerde taken in Data Lake Analytics-Tools](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/view-related-job.png)
 
### <a name="case-2-you-have-the-pipeline-for-the-recurring-job-but-not-the-url"></a>Voorbeeld 2: U hebt de pijplijn voor de periodieke taak, maar niet de URL

In Visual Studio, kunt u pijplijn Browser via Server Explorer openen > uw Azure Data Lake Analytics-account > **pijplijnen**. (Als u dit knooppunt niet in Server Explorer vinden [download de meest recente invoegtoepassing](http://aka.ms/adltoolsvs).) 

![Het knooppunt pijplijnen selecteren](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/pipeline-browser.png)

In de Browser pijplijn staan alle pijplijnen voor het Data Lake Analytics-account aan de linkerkant. U kunt uitvouwen pijplijnen om te zoeken naar alle terugkerende taken, en selecteer vervolgens een die problemen heeft. Verwante taakweergave geopend rechts.

![Een pijplijn selecteren en openen van verwante taak weergeven](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-view.png)

## <a name="step-2-analyze-a-statistics-report"></a>Stap 2: Een rapport statistieken analyseren

Een samenvatting en een rapport statistieken worden weergegeven boven aan de bijbehorende taak weergeven. U vindt er, de potentiële hoofdoorzaak van het probleem. 

1.  In het rapport wordt de x-as de tijd van de verzending van taak. Gebruik dit voor de abnormale taak vinden.
2.  Gebruik het proces in het volgende diagram statistieken controleren en inzichten over het probleem en mogelijke oplossingen verkrijgen.

![Procesdiagram voor het controleren van statistieken](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-metrics-debugging-flow.png)

## <a name="step-3-compare-the-abnormal-job-to-a-normal-job"></a>Stap 3: De taak is een normale taak abnormale vergelijken

U vindt dat alle terugkerende taken via de lijst onderaan gerelateerde taakweergave verzonden. Ga voor meer inzicht en mogelijke oplossingen, met de rechtermuisknop op de abnormale taak. De taak Diff-weergave gebruiken om de abnormale taak door een vorige normale vergelijken.

![Snelmenu voor het vergelijken van taken](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/compare-job.png)

Let op de grote verschillen tussen deze twee taken. Deze verschillen veroorzaken waarschijnlijk de prestatieproblemen. Gebruik de stappen in het volgende diagram verdere controleren:

![Procesdiagram voor het controleren van de verschillen tussen taken](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-diff-debugging-flow.png)

## <a name="next-steps"></a>Volgende stappen

* [Gegevens tijdverschil problemen oplossen](data-lake-analytics-data-lake-tools-data-skew-solutions.md)
* [Foutopsporing van de gebruiker gedefinieerde C#-code voor mislukte U-SQL-taken](data-lake-analytics-debug-u-sql-jobs.md)