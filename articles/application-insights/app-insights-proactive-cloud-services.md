---
title: Waarschuwen bij problemen in Azure Cloud Services met behulp van de Azure Diagnostics-integratie met Azure Application Insights | Microsoft Docs
description: Monitor voor problemen zoals storingen bij het opstarten, crashes en rol lussen in Azure Cloud Services met Azure Application Insights recyclen
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/07/2018
ms.author: harelbr; mbullwin
ms.openlocfilehash: 18817fd84a86a72d379f96973b71658f2cdf4afd
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34851251"
---
# <a name="alert-on-issues-in-azure-cloud-services-using-the-azure-diagnostics-integration-with-azure-application-insights"></a>Waarschuwing bij problemen in Azure Cloud Services met behulp van de Azure diagnostics-integratie met Azure Application Insights

In dit artikel wordt het instellen van regels voor waarschuwingen die op problemen controleren zoals storingen bij het opstarten, crashes en rol lussen in Azure Cloud Services (web- en werkrollen rollen recyclen) worden beschreven.

De methode die wordt beschreven in dit artikel is gebaseerd op de [Azure Diagnostics-integratie met Application Insights](https://azure.microsoft.com/blog/azure-diagnostics-integration-with-application-insights/), en de onlangs uitgebrachte [logboek waarschuwingen voor Application Insights](https://azure.microsoft.com/blog/log-alerts-for-application-insights-preview/) mogelijkheid.

## <a name="define-a-base-query"></a>Definieer een base query

Om te beginnen definieert we een base query waarmee de Windows-gebeurtenislogboek gebeurtenissen uit de Windows Azure-kanaal worden opgehaald die zijn vastgelegd in Application Insights als trace-records.
Deze records kunnen worden gebruikt voor het detecteren van tal van problemen in Azure Cloud Services, zoals startfouten, runtime-fouten en recyclen van lussen.

> [!NOTE]
> De onderstaande base query controleert op problemen in een tijdvenster van 30 minuten en wordt ervan uitgegaan dat een latentie van 10 minuten in het opnemen van de telemetrie-records. Deze standaardinstellingen kunnen worden geconfigureerd, zoals u dat wilt.

```
let window = 30m;
let endTime = ago(10m);
let EventLogs = traces
| where timestamp > endTime - window and timestamp < endTime
| extend channel = tostring(customDimensions.Channel), eventId = tostring(customDimensions.EventId)
| where channel == 'Windows Azure' and isnotempty(eventId)
| where tostring(customDimensions.DeploymentName) !contains 'deployment' // discard records captured from local machines
| project timestamp, channel, eventId, message, cloud_RoleInstance, cloud_RoleName, itemCount;
```

## <a name="check-for-specific-event-ids"></a>Controleren op specifieke gebeurtenis-id 's

Bij het ophalen van de Windows-gebeurtenislogboek gebeurtenissen, kunnen specifieke problemen worden gedetecteerd door te controleren op hun respectieve gebeurtenis-ID en het bericht-eigenschappen (Zie de volgende voorbeelden).
Gecombineerd gewoon de base query hierboven met een van de query's hieronder en gebruikte die gecombineerd query bij het definiëren van de waarschuwingsregel logboek.

> [!NOTE]
> In de onderstaande voorbeelden is wordt een probleem gedetecteerd als meer dan drie gebeurtenissen zijn gevonden tijdens het geanalyseerde tijdvenster. Deze standaardinstelling kan worden geconfigureerd voor het wijzigen van de vertrouwelijkheid van de waarschuwingsregel.

```
// Detect failures in the OnStart method
EventLogs
| where eventId == '2001'
| where message contains '.OnStart()'
| summarize Failures = sum(itemCount) by cloud_RoleInstance, cloud_RoleName
| where Failures > 3
```

```
// Detect failures during runtime
EventLogs
| where eventId == '2001'
| where message contains '.Run()'
| summarize Failures = sum(itemCount) by cloud_RoleInstance, cloud_RoleName
| where Failures > 3
```

```
// Detect failures when running a startup task
EventLogs
| where eventId == '1000'
| summarize Failures = sum(itemCount) by cloud_RoleInstance, cloud_RoleName
| where Failures > 3
```

```
// Detect recycle loops
EventLogs
| where eventId == '1006'
| summarize Failures = sum(itemCount) by cloud_RoleInstance, cloud_RoleName
| where Failures > 3
```

## <a name="create-an-alert"></a>Een waarschuwing maken

In het navigatiemenu binnen uw Application Insights-resource, gaat u naar **waarschuwingen**, en selecteer vervolgens **nieuwe waarschuwingsregel**.

![Schermopname van Create rule](./media/app-insights-proactive-cloud-services/001.png)

In de **maken regel** venster onder de **definiëren meldingsvoorwaarde** sectie, klikt u op **criteria toevoegen**, en selecteer vervolgens **aangepaste logboek zoekactie**.

![Schermafbeelding van de voorwaarde criteria definiëren voor waarschuwing](./media/app-insights-proactive-cloud-services/002.png)

In de **zoekquery** vak, plakt u de gecombineerde query die u hebt voorbereid in de vorige stap.

Ga vervolgens verder naar de **drempelwaarde** vak en stel de waarde op 0. U kunt eventueel aanpassen de **periode** en frequentie **velden**.
Klik op **Gereed**.

![Schermopname van signaal logica query configureren](./media/app-insights-proactive-cloud-services/003.png)

Onder de **definiëren Waarschuwingsdetails** sectie, bieden een **naam** en **beschrijving** naar de waarschuwingsregel en stel de **ernst**.
Zorg er ook voor dat de **regel inschakelen tijdens het maken van** is ingesteld op **Ja**.

![Schermopname Waarschuwingsdetails](./media/app-insights-proactive-cloud-services/004.png)

Onder de **definiëren actiegroep** sectie, kunt u een bestaande **actiegroep** of maak een nieuwe.
U kunt de actiegroep meerdere acties van verschillende typen bevatten.

![Schermopname actiegroep](./media/app-insights-proactive-cloud-services/005.png)

Zodra de groep acties die u hebt gedefinieerd, bevestigt u uw wijzigingen en klikt u op **waarschuwingsregel maken**.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het automatisch te detecteren:

[Fout afwijkingen](app-insights-proactive-failure-diagnostics.md)
[geheugenlekken](app-insights-proactive-potential-memory-leak.md)
[afwijkingen](app-insights-proactive-performance-diagnostics.md)

