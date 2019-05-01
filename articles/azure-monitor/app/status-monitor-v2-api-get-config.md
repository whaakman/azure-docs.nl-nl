---
title: 'Status van Azure Monitor v2 API-verwijzing: Configuratie ophalen | Microsoft Docs'
description: Status Monitor v2 API-verwijzing Get-ApplicationInsightsMonitoringConfig. Websiteprestaties controleren zonder de website opnieuw te implementeren. Werkt met ASP.NET-web-apps die on-premises worden gehost, die in virtuele machines worden gehost en die via Azure worden gehost.
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
ms.openlocfilehash: 0e6b47c9b629aed28fa217cb6299edb57423fc6f
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2019
ms.locfileid: "64870475"
---
# <a name="status-monitor-v2-api-get-applicationinsightsmonitoringconfig-v021-alpha"></a>Status Monitor v2 API: Get-ApplicationInsightsMonitoringConfig (v0.2.1-alpha)

Dit document beschrijft een cmdlet die wordt geleverd als een lid van de [Az.ApplicationMonitor PowerShell-module](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> Status Monitor v2 is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie voor meer informatie, [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

## <a name="description"></a>Description

Ophalen van het configuratiebestand en de waarden in de console afdrukken.

> [!IMPORTANT] 
> Deze cmdlet is een PowerShell-sessie met beheerdersmachtigingen vereist.

## <a name="examples"></a>Voorbeelden

```powershell
PS C:\> Get-ApplicationInsightsMonitoringConfig
```

## <a name="parameters"></a>Parameters 

(Er zijn geen Parameters vereist)

## <a name="output"></a>Uitvoer


#### <a name="example-output-from-reading-the-config-file"></a>Voorbeeld van uitvoer de configuratiebestand te lezen

```
RedfieldConfiguration:
Filters:
0)InstrumentationKey:  AppFilter: WebAppExclude MachineFilter: .*
1)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2 AppFilter: WebAppTwo MachineFilter: .*
2)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault AppFilter: .* MachineFilter: .*
```
