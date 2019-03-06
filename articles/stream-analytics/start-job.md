---
title: Een Azure Stream Analytics-taak starten
description: In dit artikel wordt beschreven hoe u een Stream Analytics-taak starten.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/05/2019
ms.openlocfilehash: aa089ed53554ec697bd9430cc95a7cce78aabed2
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57411527"
---
# <a name="how-to-start-an-azure-stream-analytics-job"></a>Een Azure Stream Analytics-taak starten

U kunt uw Azure Stream Analytics-taak met de Azure portal, Visual Studio en PowerShell starten. Als u een taak start, selecteert u een tijd voor de taak moet beginnen met het maken van uitvoer. Azure-portal, Visual Studio en PowerShell hebben verschillende methoden voor het instellen van de begintijd. Deze methoden worden hieronder beschreven.

## <a name="azure-portal"></a>Azure Portal

Ga naar uw taak in Azure portal en selecteer **Start** op de overzichtspagina. Selecteer een **starttijd voor taakuitvoer** en selecteer vervolgens **Start**.

Er zijn drie opties voor **starttijd voor taakuitvoer**: *Nu*, *aangepaste*, en *laatste tijd geëindigd*. Selecteren *nu* Start de taak op de huidige tijd. Selecteren *aangepaste* kunt u een aangepaste tijd instellen in het verleden of in de toekomst bevindt voor de taak om te beginnen. Als u wilt een gestopte taak hervatten zonder gegevens te verliezen, kiest u het. Laatste tijd geëindigd *.

## <a name="visual-studio"></a>Visual Studio

Selecteer in de taakweergave, de groene pijl om de taak te starten. Stel de **taak uitvoer startmodus** en selecteer **Start**. Status van de taak wordt gewijzigd in **met**.

Er zijn drie opties voor **taak uitvoer startmodus**: *JobStartTime*, *CustomTime*, en *LastOutputEventTime*. Als deze eigenschap niet aanwezig is, is de standaardinstelling *JobStartTime*.

*JobStartTime* maakt het beginpunt van de uitvoergebeurtenis stream hetzelfde als wanneer de taak wordt gestart.

*CustomTime* de uitvoer begint bij een aangepaste tijd die is opgegeven in de *OutputStartTime* parameter.

*LastOutputEventTime* maakt het beginpunt van de gebeurtenisstroom uitvoer is hetzelfde is als de laatste keer uitgevoerd.

## <a name="powershell"></a>PowerShell

Gebruik de volgende cmdlet om de taak met behulp van PowerShell te starten:

```powershell
Start-AzStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  -Name $jobName `
  -OutputStartMode 'JobStartTime'
```

Er zijn drie opties voor **OutputStartMode**: *JobStartTime*, *CustomTime*, en *LastOutputEventTime*. Als deze eigenschap niet aanwezig is, is de standaardinstelling *JobStartTime*.

*JobStartTime* maakt het beginpunt van de uitvoergebeurtenis stream hetzelfde als wanneer de taak wordt gestart.

*CustomTime* de uitvoer begint bij een aangepaste tijd die is opgegeven in de *OutputStartTime* parameter.

*LastOutputEventTime* maakt het beginpunt van de gebeurtenisstroom uitvoer is hetzelfde is als de laatste keer uitgevoerd.

Voor meer informatie over de `Start-AzStreamAnalyitcsJob` weergave-cmdlet de [Start AzStreamAnalyticsJob verwijzing](/powershell/module/az.streamanalytics/start-azstreamanalyticsjob.md).

## <a name="next-steps"></a>Volgende stappen

* [Snelstart: Een Stream Analytics-taak maken met behulp van Azure portal](stream-analytics-quick-create-portal.md)
* [Snelstart: Maken van een Stream Analytics-taak met behulp van Azure PowerShell](stream-analytics-quick-create-powershell.md)
* [Snelstart: Een Stream Analytics-taak maken met behulp van de Azure Stream Analytics-hulpprogramma's voor Visual Studio](stream-analytics-quick-create-vs.md)