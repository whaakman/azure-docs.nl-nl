---
title: 'Naslag informatie voor Azure Status Monitor v2 API: Bewaking uitschakelen | Microsoft Docs'
description: Status Monitor v2 API-referentie. Disable-ApplicationInsightsMonitoring. Bewaak de prestaties van de website zonder de website opnieuw te implementeren. Werkt met ASP.NET-Web-apps die on-premises worden gehost, in Vm's of op Azure.
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
ms.openlocfilehash: 368365251ec61464869c6120ded2ec2237acffea
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68326403"
---
# <a name="status-monitor-v2-api-disable-applicationinsightsmonitoring"></a>Status Monitor v2-API: Disable-ApplicationInsightsMonitoring

In dit artikel wordt een cmdlet beschreven die lid is van de [Power shell-module AZ. ApplicationMonitor](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

## <a name="description"></a>Description

Hiermee schakelt u de bewaking op de doel computer uit.
Met deze cmdlet worden bewerkingen verwijderd uit IIS applicationHost. config en worden de register sleutels verwijderd.

> [!IMPORTANT] 
> Voor deze cmdlet is een Power shell-sessie met beheerders machtigingen vereist.

## <a name="examples"></a>Voorbeelden

```powershell
PS C:\> Disable-ApplicationInsightsMonitoring
```

## <a name="parameters"></a>Parameters 

### <a name="-verbose"></a>-Verbose
**Algemene para meter.** Gebruik deze schakel optie om gedetailleerde logboeken weer te geven.

## <a name="output"></a>Output


#### <a name="example-output-from-successfully-disabling-monitoring"></a>Voorbeeld uitvoer van het uitschakelen van de bewaking

```
Initiating Disable Process
Applying transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config'
'C:\Windows\System32\inetsrv\config\applicationHost.config' backed up to 'C:\Windows\System32\inetsrv\config\applicationHost.config.backup-2019-03-26_08-59-00z'
in :1,237
No element in the source document matches '/configuration/location[@path='']/system.webServer/modules/add[@name='ManagedHttpModuleHelper']'
Not executing RemoveAll (transform line 1, 546)
Transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config' was successfully applied. Operation: 'disable'
GAC Module will not be removed, since this operation might cause IIS instabilities
Configuring IIS Environment for codeless attach...
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]
Configuring IIS Environment for instrumentation engine...
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]
Configuring registry for instrumentation engine...
Successfully disabled Application Insights Status Monitor
```


## <a name="next-steps"></a>Volgende stappen

 Meer doen met Status Monitor v2:
 - Gebruik onze hand leiding voor het [oplossen van problemen](status-monitor-v2-troubleshoot.md) status monitor v2.
