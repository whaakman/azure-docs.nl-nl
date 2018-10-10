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
ms.devlang: multiple
ms.topic: reference
ms.date: 10/05/2018
ms.reviewer: mbullwin
ms.author: tilee
ms.openlocfilehash: 05958f35f80a53da27e020d367799519ef5a9bd7
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48901580"
---
# <a name="application-insights-for-azure-functions-supported-features"></a>Application Insights voor Azure Functions ondersteunde functies

Hieronder vindt u de momenteel ondersteunde lijst met functies voor de [Application Insights-integratie met Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-monitoring). Handleiding voor Azure-functies voor [aan de slag](https://github.com/Azure/Azure-Functions/wiki/App-Insights).


| Azure Functions                       | V1                | V2 (Ignite 2018)  | 
|-----------------------------------    |---------------    |------------------ |
| **Application Insights SDK voor .NET**         | **2.5.0**             | **2.7.2**                 |
| | | | 
| **Automatische verzameling**              |                   |                   |               
| &bull; Aanvragen                           | Ja               | Ja               | 
| &bull; Uitzonderingen                         | Ja               | Ja               | 
| &bull; Afhankelijkheden               |                   |                   |               
| &mdash; HTTP                              |                   | Ja               | 
| &mdash; Service Bus                        |                   | Ja               | 
| &mdash; Event hub                          |                   | Ja               | 
| &mdash; SQL                               |                   | Ja               | 
| | | | 
| **Ondersteunde functies**                    |                   |                   |               
| &bull; QuickPulse/LiveMetrics                         | Ja               | Ja               | 
| &bull; Steekproeven                           | Ja               | Ja               | 
| &bull; Heartbeats                         |       | Ja               | 
| | | | 
| **Correlatie**                           |                   |                   |               
| &bull; Service Bus                         |                   | Ja               | 
| &bull; Event hub                           |                   | Ja               | 
| | | | 
| **Configureerbare**                  |                   |                   |           
| &bull;Volledig kunnen worden geconfigureerd.<br/>Zie [Azure Functions](https://github.com/Microsoft/ApplicationInsights-aspnetcore/issues/759#issuecomment-426687852) voor instructies.<br/>Zie [Asp.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Custom-Configuration) voor alle opties.               |                   | Ja                   | 
