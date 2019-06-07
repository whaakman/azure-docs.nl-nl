---
title: 'Azure Status Monitor v2 API-verwijzing: Instrumentatie-engine inschakelen | Microsoft Docs'
description: Status Monitor v2 API-verwijzing. Enable-InstrumentationEngine. Websiteprestaties controleren zonder de website opnieuw te implementeren. Werkt met ASP.NET web-apps die on-premises, in virtuele machines, of op Azure worden gehost.
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
ms.openlocfilehash: 1e30490dbd51f541afd0b7036769cfc638a75877
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66514368"
---
# <a name="status-monitor-v2-api-enable-instrumentationengine-v021-alpha"></a>Status Monitor v2 API: Enable-InstrumentationEngine (v0.2.1-alfa)

Dit artikel wordt beschreven voor een cmdlet die deel uitmaakt van de [Az.ApplicationMonitor PowerShell-module](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> Status Monitor v2 is momenteel in openbare preview.
> Deze preview-versie wordt geleverd zonder een service level agreement, en wordt niet aanbevolen voor productieworkloads. Sommige functies mogelijk niet ondersteund, en sommige mogelijk beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="description"></a>Description

Hiermee kunt de instrumentatie-engine door in te stellen bepaalde registersleutels.
Start IIS opnieuw om de wijzigingen te laten treden.

De engine instrumentatie kunt vormen een aanvulling op gegevens die zijn verzameld door de .NET-SDK's.
Deze verzamelt de gebeurtenissen en -berichten die tijdens de uitvoering van een beheerd proces wordt beschreven. Deze gebeurtenissen en -berichten zijn resultaatcodes afhankelijkheid, HTTP-termen en SQL-opdrachttekst.

De engine instrumentation inschakelen als:
- U hebt al ingeschakeld voor bewaking met de cmdlet Enable maar de instrumentatie-engine niet hebt ingeschakeld.
- U hebt uw app met de .NET-SDK's handmatig geïnstrumenteerd en wilt u meer telemetrie verzamelen.

> [!IMPORTANT] 
> Deze cmdlet is een PowerShell-sessie met beheerdersmachtigingen vereist.

> [!NOTE] 
> - Deze cmdlet is vereist dat u lees en accepteer van onze licentie en privacy-instructie.
> - De engine instrumentation voegt extra overhead en is standaard uitgeschakeld.

## <a name="examples"></a>Voorbeelden

```powershell
PS C:\> Enable-InstrumentationEngine
```

## <a name="parameters"></a>Parameters

### <a name="-acceptlicense"></a>-AcceptLicense
**Optioneel.** Gebruik deze switch te accepteren van de licentie- en privacy-instructie in ' headless '-installaties.

### <a name="-verbose"></a>-Verbose
**Algemene parameter.** Gebruik deze switch naar gedetailleerde logboeken uitvoer.

## <a name="output"></a>Uitvoer


#### <a name="example-output-from-successfully-enabling-the-instrumentation-engine"></a>Van voorbeelduitvoer van de instrumentatie-engine is ingeschakeld

```
Configuring IIS Environment for instrumentation engine...
Configuring registry for instrumentation engine...
```

## <a name="next-steps"></a>Volgende stappen

  Uw telemetrie weergeven:
 - [Verken de metrische gegevens](../../azure-monitor/app/metrics-explorer.md) prestaties controleren en gebruik.
- [Doorzoek gebeurtenissen en logboeken](../../azure-monitor/app/diagnostic-search.md) om problemen te diagnosticeren.
- Gebruik [analytics](../../azure-monitor/app/analytics.md) voor meer geavanceerde query's.
- [Maak dashboards](../../azure-monitor/app/overview-dashboard.md).
 
 Meer telemetrie toevoegen:
 - [Maak webtests](monitor-web-app-availability.md) om ervoor te zorgen dat uw site actief blijft.
- [Voeg telemetrie van de webclient](../../azure-monitor/app/javascript.md) om te zien welke uitzonderingen webpaginacode en om in te schakelen trace-aanroepen.
- [Voeg de Application Insights SDK toe aan uw code](../../azure-monitor/app/asp-net.md) zodat u kunt invoegen van trace en logboekaanroepen.
 
 Doe meer met Status Monitor v2:
 - Gebruik onze handleiding voor [oplossen](status-monitor-v2-troubleshoot.md) Status Monitor v2.
 - [Ophalen van de configuratie](status-monitor-v2-api-get-config.md) om te bevestigen dat de instellingen correct zijn vastgelegd.
 - [De status ophalen](status-monitor-v2-api-get-status.md) te inspecteren bewaking.
