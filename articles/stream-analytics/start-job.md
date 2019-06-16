---
title: Een Azure Stream Analytics-taak starten
description: In dit artikel wordt beschreven hoe u een Stream Analytics-taak starten.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/03/2019
ms.openlocfilehash: 9bc3e4132919e5fc5baadc78841e66efd3c34bcd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61362262"
---
# <a name="how-to-start-an-azure-stream-analytics-job"></a>Een Azure Stream Analytics-taak starten

U kunt uw Azure Stream Analytics-taak met de Azure portal, Visual Studio en PowerShell starten. Als u een taak start, selecteert u een tijd voor de taak moet beginnen met het maken van uitvoer. Azure-portal, Visual Studio en PowerShell hebben verschillende methoden voor het instellen van de begintijd. Deze methoden worden hieronder beschreven.

## <a name="start-options"></a>Opties voor Start
De volgende drie opties zijn beschikbaar om een taak te starten. Houd er rekening mee dat alle hieronder vermelde tijden opgegeven zijn in die [TIMESTAMP BY](https://docs.microsoft.com/stream-analytics-query/timestamp-by-azure-stream-analytics). Als de TIMESTAMP BY is niet opgegeven, wordt aankomsttijd worden gebruikt.
* **Nu**: Maakt het beginpunt van de uitvoergebeurtenis stream hetzelfde als wanneer de taak wordt gestart. Als een tijdelijke operator wordt gebruikt (bijvoorbeeld tijdvenster, vertraging of JOIN), Azure Stream Analytics ziet automatisch terug op de gegevens in de invoerbron. Bijvoorbeeld, als u een taak start 'Nu' en als uw query een Tumblingvenster van 5 minuten gebruikt, Azure Stream Analytics wordt willen naar gegevens zoeken van 5 minuten geleden is in de invoer.
De eerste uitvoergebeurtenis voor mogelijke zou wel een tijdstempel zijn gelijk aan of groter is dan de huidige tijd en ASA garandeert dat alle invoergebeurtenissen die logisch kunnen bijdragen aan de uitvoer heeft zijn verwerkt voor. Bijvoorbeeld, worden er zijn geen gedeeltelijke samentellingen gegenereerd. Het is altijd het volledige geaggregeerde waarde.

* **Aangepast**: U kunt het beginpunt van de uitvoer. Net als de **nu** optie, Azure Stream Analytics leest de gegevens voordat deze tijd automatisch als een tijdelijke operator wordt gebruikt 

* **Laatste tijd geëindigd**. Deze optie is alleen beschikbaar wanneer de taak eerder is gestart, maar is handmatig gestopt of is mislukt. Wanneer u deze optie kiest gebruikt Azure Stream Analytics de laatste uitvoertijd voor de taak opnieuw te starten, zodat er geen gegevens verloren gegaan zijn. Op dezelfde manier naar de vorige opties leest Azure Stream Analytics automatisch de gegevens voordat deze tijd als een tijdelijke operator wordt gebruikt. Aangezien verschillende invoer partities kunnen verschillende tijd hebt, de vroegste tijd voor het stoppen van alle partities wordt gebruikt, als gevolg hiervan enkele dubbele waarden kunnen worden weergegeven in de uitvoer. Meer informatie over exact-één keer worden verwerkt, zijn beschikbaar op de pagina [gebeurtenis levering garanties](https://docs.microsoft.com/stream-analytics-query/event-delivery-guarantees-azure-stream-analytics).


## <a name="azure-portal"></a>Azure Portal

Ga naar uw taak in Azure portal en selecteer **Start** op de overzichtspagina. Selecteer een **starttijd voor taakuitvoer** en selecteer vervolgens **Start**.

Kies een van de opties voor **starttijd voor taakuitvoer**. De opties zijn *nu*, *aangepaste*, en, als de taak eerder is uitgevoerd, *laatste tijd geëindigd*. Zie hierboven voor meer informatie over deze opties.

## <a name="visual-studio"></a>Visual Studio

Selecteer in de taakweergave, de groene pijl om de taak te starten. Stel de **taak uitvoer startmodus** en selecteer **Start**. Status van de taak wordt gewijzigd in **met**.

Er zijn drie opties voor **taak uitvoer startmodus**: *JobStartTime*, *CustomTime*, en *LastOutputEventTime*. Als deze eigenschap niet aanwezig is, is de standaardinstelling *JobStartTime*. Zie hierboven voor meer informatie over deze opties.


## <a name="powershell"></a>PowerShell

Gebruik de volgende cmdlet om de taak met behulp van PowerShell te starten:

```powershell
Start-AzStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  -Name $jobName `
  -OutputStartMode 'JobStartTime'
```

Er zijn drie opties voor **OutputStartMode**: *JobStartTime*, *CustomTime*, en *LastOutputEventTime*. Als deze eigenschap niet aanwezig is, is de standaardinstelling *JobStartTime*. Zie hierboven voor meer informatie over deze opties.

Voor meer informatie over de `Start-AzStreamAnalyitcsJob` weergave-cmdlet de [Start AzStreamAnalyticsJob verwijzing](/powershell/module/az.streamanalytics/start-azstreamanalyticsjob).

## <a name="next-steps"></a>Volgende stappen

* [Snelstart: Een Stream Analytics-taak maken met behulp van Azure portal](stream-analytics-quick-create-portal.md)
* [Snelstart: Maken van een Stream Analytics-taak met behulp van Azure PowerShell](stream-analytics-quick-create-powershell.md)
* [Snelstart: Een Stream Analytics-taak maken met behulp van de Azure Stream Analytics-hulpprogramma's voor Visual Studio](stream-analytics-quick-create-vs.md)
