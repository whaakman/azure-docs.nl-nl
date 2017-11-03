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
ms.openlocfilehash: a358f94b117c12511028a875e56b5c9dba8d3382
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-troubleshoot-an-abnormal-recurring-job"></a>Problemen met een abnormale terugkerende taak oplossen

In dit document, stellen we het gebruik van [Azure Data Lake Tools voor Visual Studio](http://aka.ms/adltoolsvs) terugkerende taak problemen oplossen. Meer informatie over de pijplijn en terugkerende taken van [hier](https://blogs.msdn.microsoft.com/azuredatalake/2017/09/19/managing-pipeline-recurring-jobs-in-azure-data-lake-analytics-made-easy/).
Terugkerende taken worden meestal dezelfde logica van de query en vergelijkbare invoergegevens delen. Bijvoorbeeld: u hebt een terugkerende taak uitgevoerd op elk etmaal maandag 8 uur telling van de laatste week wekelijkse actieve gebruiker, de scripts voor deze taken één script sjabloon de logica query bevat delen en de invoer voor deze taken zijn de gebruiksgegevens voor vorige week. Delen van dezelfde logica opvragen en vergelijkbare invoercode van meestal betekent prestaties van deze taken is vergelijkbaar en stabiele, als een van uw terugkerende taken plotseling abnormale, mislukt of vertragen veel u uitvoert mogelijk wilt:

1.  Zie de Statistiekenrapporten voor de voorbeelden worden uitgevoerd van de periodieke taak om te zien wat er gebeurd is.
2.  Vergelijk de abnormale taak met een normale om te achterhalen wat is gewijzigd.

**Taak weergeven gerelateerd** in Azure Data Lake Tools voor Visual Studio kunt u de voortgang van de problemen met beide gevallen versnellen.

## <a name="step-1-find-recurring-jobs-and-open-related-job-view"></a>Stap 1: Terugkerende taken vinden en openen van verwante taak weergeven

Gebruik van gerelateerde taak weergave terugkerende taak probleem kan oplossen, moet u eerst de terugkerende taak niet vinden in Visual Studio en open vervolgens de bijbehorende taak weergeven.

### <a name="case-1-you-have-the-url-for-the-recurring-job"></a>Voorbeeld 1: U hebt de URL voor de terugkerende taak

Via **Extra > Data Lake > taakweergave**, kunt u de URL van de taak taak als weergave wilt openen in Visual Studio plakken en via weergave gerelateerde taken te openen taak weergeven gerelateerd.

![Data Lake Analytics Tools weergeven gerelateerde taken](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/view-related-job.png)
 
### <a name="case-2-you-have-the-pipeline-for-the-recurring-job-but-not-the-url"></a>Voorbeeld 2: U hebt de pijplijn voor de periodieke taak, maar niet de URL

In Visual Studio, kunt u Browser via pijplijn openen **Server Explorer > uw Data Lake Analytics-account > pijplijnen** (als u dit knooppunt niet in Server Explorer vinden, kun je het meest recente versie hulpprogramma [hier](http://aka.ms/adltoolsvs)). In de Browser pijplijn alle pijplijnen voor het account ADLA aan de linkerkant worden vermeld, kunt u de pijplijnen om te zoeken naar alle terugkerende taken, klikt u op uitbreiden die problemen heeft, de verwante taakweergave opent rechts.

![Data Lake Analytics Tools weergeven gerelateerde taken](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/pipeline-browser.png)

![Data Lake Analytics Tools weergeven gerelateerde taken](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-view.png)

## <a name="step-2-analyze-statistics-report"></a>Stap 2: Statistieken rapport analyseren

Een samenvatting en een rapport statistieken worden weergegeven boven aan de bijbehorende taak weergeven, waarmee u de potentiële hoofdoorzaak van het abnormale krijgt. 

1.  U moet eerst de abnormale taak niet vinden in het rapport. De X-as laat zien taak verzending via welke u de abnormale taak kunt vinden.
2.  Volg onderstaande procedure voor het controleren van de statistieken en de inzichten van de abnormale en de mogelijke oplossingen ophalen.

![Data Lake Analytics Tools weergeven gerelateerde taken](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-metrics-debugging-flow.png)

## <a name="step-3-compare-the-abnormal-recurring-job-to-a-normal-job"></a>Stap 3: De abnormale terugkerende taak is een normale taak vergelijken

U vindt dat alle terugkerende taken via takenlijst aan de onderkant van gerelateerde taakweergave verzonden. U kunt de taak abnormale door een vorige normale meer inzicht en mogelijke oplossingen vinden in de weergave van de taak Diff vergelijken tot en met de rechtermuisknop te klikken op.

![Data Lake Analytics Tools weergeven gerelateerde taken](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/compare-job.png)

Normaal gesproken moet u aandacht te besteden aan de grote verschillen tussen deze 2 taken als ze zich waarschijnlijk de redenen die prestatieproblemen veroorzaken, en kunt u onderstaande stappen om te doen verdere controle volgen.

![Data Lake Analytics Tools weergeven gerelateerde taken](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-diff-debugging-flow.png)

## <a name="next-steps"></a>Volgende stappen

* [Het fouten opsporen en oplossen van problemen met de scheeftrekken](data-lake-analytics-data-lake-tools-data-skew-solutions.md)
* [Fouten opsporen in U-SQL-taak is mislukt voor de gebruiker gedefinieerde foutcode](data-lake-analytics-debug-u-sql-jobs.md)