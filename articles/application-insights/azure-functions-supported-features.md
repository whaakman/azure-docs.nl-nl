---
title: Ondersteunde functies van Azure Application Insights - Azure Functions | Microsoft Docs
description: Application Insights ondersteunde functies voor Azure Functions
services: application-insights
documentationcenter: .net
author: MS-TimothyMothra
manager: ''
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: reference
ms.date: 10/05/2018
ms.reviewer: mbullwin
ms.author: tilee
ms.openlocfilehash: 9ad0579ff9c25753b1e4816b80948b4d8d1232f7
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52720971"
---
# <a name="application-insights-for-azure-functions-supported-features"></a>Application Insights voor Azure Functions ondersteunde functies

Azure Functions biedt [ingebouwde integratie](https://docs.microsoft.com/azure/azure-functions/functions-monitoring) met Application Insights, die beschikbaar zijn via de ILogger Interface is. Hieronder vindt u de lijst met ondersteunde functies. Handleiding voor Azure-functies voor [aan de slag](https://github.com/Azure/Azure-Functions/wiki/App-Insights).

## <a name="supported-features"></a>Ondersteunde functies

| Azure Functions                       | V1                | V2 (Ignite 2018)  | 
|-----------------------------------    |---------------    |------------------ |
| **Application Insights SDK voor .NET**   | **2.5.0**       | **2.7.2**         |
| | | | 
| **Automatische verzameling**        |                 |                   |               
| &bull; Aanvragen                     | Ja             | Ja               | 
| &bull; Uitzonderingen                   | Ja             | Ja               | 
| &bull; Afhankelijkheden                   |                   |                   |               
| &nbsp;&nbsp;&nbsp;&mdash; HTTP      |                 | Ja               | 
| &nbsp;&nbsp;&nbsp;&mdash; Service Bus|                 | Ja               | 
| &nbsp;&nbsp;&nbsp;&mdash; Event hub  |                 | Ja               | 
| &nbsp;&nbsp;&nbsp;&mdash; SQL       |                 | Ja               | 
| | | | 
| **Ondersteunde functies**                |                   |                   |               
| &bull; QuickPulse/LiveMetrics       | Ja             | Ja               | 
| &bull; Steekproeven                     | Ja             | Ja               | 
| &bull; Heartbeats                   |                 | Ja               | 
| | | | 
| **Correlatie**                       |                   |                   |               
| &bull; Service Bus                     |                   | Ja               | 
| &bull; Event hub                       |                   | Ja               | 
| | | | 
| **Configureerbare**                      |                   |                   |           
| &bull;Volledig kunnen worden geconfigureerd.<br/>Zie [Azure Functions](https://github.com/Microsoft/ApplicationInsights-aspnetcore/issues/759#issuecomment-426687852) voor instructies.<br/>Zie [Asp.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Custom-Configuration) voor alle opties.               |                   | Ja                   | 


## <a name="sampling"></a>Steekproeven

Azure Functions kunt steekproeven standaard in de configuratie. Zie voor meer informatie, [configureren steekproeven](https://docs.microsoft.com/azure/azure-functions/functions-monitoring#configure-sampling).
