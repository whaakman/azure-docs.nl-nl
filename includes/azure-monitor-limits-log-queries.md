---
title: bestand opnemen
description: bestand opnemen
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 07/22/2019
ms.author: bwren
ms.custom: include file
ms.openlocfilehash: ed840352096f1a1739bc8f655be42096456d3c33
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68405668"
---
| Limiet | Description |
|:---|:---|
| Querytaal | Azure Monitor gebruikt dezelfde [Kusto-query taal](/azure/kusto/query/) als Azure Data Explorer. Zie [Azure monitor taal verschillen in de logboek query](../articles/azure-monitor/log-query/data-explorer-difference.md) voor KQL-taal elementen die niet worden ondersteund in azure monitor. |
| Azure-regio's | Logboek query's kunnen overmatige overhead ondervinden wanneer gegevens Log Analytics werk ruimten in meerdere Azure-regio's. Zie [query limieten](../articles/azure-monitor/log-query/scope.md#query-limits) voor meer informatie. |
