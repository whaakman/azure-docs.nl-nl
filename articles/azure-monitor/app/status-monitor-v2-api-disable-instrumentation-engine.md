---
title: 'Status van Azure Monitor v2 API-verwijzing: Instrumentation engine uitschakelen | Microsoft Docs'
description: Status Monitor v2 InstrumentationEngine-API-verwijzing uitschakelen. Websiteprestaties controleren zonder de website opnieuw te implementeren. Werkt met ASP.NET-web-apps die on-premises worden gehost, die in virtuele machines worden gehost en die via Azure worden gehost.
services: application-insights
documentationcenter: .net
author: MS-TimothyMothra
manager: alexklim
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: tilee
ms.openlocfilehash: 7d1b658da59c0e778c82301077ef27bdd9c9e614
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65144894"
---
# <a name="status-monitor-v2-api-disable-instrumentationengine-v021-alpha"></a>Status Monitor v2 API: Disable-InstrumentationEngine (v0.2.1-alfa)

Dit document beschrijft een cmdlet die wordt geleverd als een lid van de [Az.ApplicationMonitor PowerShell-module](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> Status Monitor v2 is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie voor meer informatie, [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

## <a name="description"></a>Description

Deze cmdlet wordt de Engine Instrumentation uitgeschakeld door bepaalde registersleutels te verwijderen.
Start IIS opnieuw om deze wijzigingen te laten treden.

> [!IMPORTANT] 
> Deze cmdlet is een PowerShell-sessie met beheerdersmachtigingen vereist.

## <a name="examples"></a>Voorbeelden

```powershell
PS C:\> Disable-InstrumentationEngine
```

## <a name="parameters"></a>Parameters 

### <a name="-verbose"></a>-Verbose
**Common Parameter.** Gebruik deze switch naar gedetailleerde logboeken uitvoer.

## <a name="output"></a>Uitvoer


#### <a name="example-output-from-successfully-disabling-the-instrumentation-engine"></a>Van voorbeelduitvoer van de instrumentatie-engine is uitschakelen

```
Configuring IIS Environment for instrumentation engine...
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]'
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]'
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]'
Configuring registry for instrumentation engine...
```


## <a name="next-steps"></a>Volgende stappen

 Doe meer met Status Monitor v2:
 - Gebruik onze handleiding voor [oplossen](status-monitor-v2-troubleshoot.md) Status Monitor v2.
