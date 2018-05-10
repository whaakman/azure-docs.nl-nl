---
title: Taken in Azure Data Lake Analytics met Azure Portal controleren | Microsoft Docs
description: 'Informatie over het gebruik van de Azure Portal oplossen met Data Lake Analytics-taken. '
services: data-lake-analytics
documentationcenter: ''
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
ms.author: saveenr
ms.openlocfilehash: 14b1f4ec9dff78e4b5d2480755a4b1f2579ec135
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="monitor-jobs-in-azure-data-lake-analytics-using-the-azure-portal"></a>Taken in Azure Data Lake Analytics met Azure Portal controleren

**Voor een overzicht van alle taken**

1. Klik in de Azure-portal op **Microsoft Azure** in de linkerbovenhoek.
2. Klik op de tegel met de naam van uw Data Lake Analytics-account.  De samenvatting taak wordt weergegeven op de **Jobbeheer** tegel.

    ![Beheer van Azure Data Lake Analytics-taak](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-job-management.png)

    De taak Management biedt u een overzicht van de status van de taak. Er is een mislukte taak.
3. Klik op de **Jobbeheer** tegel om te zien van de taken. De taken zijn onderverdeeld in **met**, **in de wachtrij geplaatst**, en **beëindigd**. U ziet de mislukte taak in de **beëindigd** sectie. Het moet eerste certificaat in de lijst zijn. Wanneer u een groot aantal taken hebt, kunt u **Filter** om u te helpen u bij het zoeken van taken.

    ![Azure Data Lake Analytics-filter-taken](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-filter-jobs.png)
4. Klik op de mislukte taak uit de lijst om de taakdetails openen:

    ![Azure Data Lake Analytics taak is mislukt](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job.png)

    U ziet de **opnieuw indienen** knop. Nadat u het probleem hebt opgelost, kunt u de taak opnieuw indienen.
5. Klik op de gemarkeerde onderdeel in de vorige schermafbeelding details van de fout te openen.  U ziet ongeveer als volgt:

    ![Azure Data Lake Analytics taakdetails is mislukt](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job-details.png)

    Het vertelt u dat de bronmap is niet gevonden.
6. Klik op **Script dubbele**.
7. Update de **FROM** pad naar:

    ' / Samples/Data/SearchLog.tsv '
8. Klik op **Taak verzenden**.

## <a name="see-also"></a>Zie ook
* [Overzicht van Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Aan de slag met Azure Data Lake Analytics met Azure PowerShell](data-lake-analytics-get-started-powershell.md)
* [Azure Data Lake Analytics beheren met Azure Portal](data-lake-analytics-manage-use-portal.md)
