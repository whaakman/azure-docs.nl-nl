---
title: bestand opnemen
description: bestand opnemen
services: batch
author: laurenhughes
ms.service: batch
ms.topic: include
ms.date: 05/28/2019
ms.author: lahugh
ms.custom: include file
ms.openlocfilehash: 22bfc3c86605f4c2eed4c022919b3643f394ea95
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67080902"
---
| **Resource** | **Standaardlimiet** | **Maximumlimiet** |
| --- | --- | --- |
| Azure Batch-accounts per regio per abonnement | 1-3 |50 |
| Toegewezen kerngeheugens per Batch-account | 90-900 | Neem contact op met ondersteuning |
| Kernen per Batch-account met lage prioriteit | 10-100 | Neem contact op met ondersteuning |
| **[Actieve](https://docs.microsoft.com/rest/api/batchservice/job/get#jobstate)**  taken en taakplanningen per Batch-account (**voltooid** taken hebben geen limiet) | 100-300 | 1,000<sup>1</sup> |
| Pools per Batch-account | 20-100 | 500<sup>1</sup> |

> [!NOTE]
> Standaardlimieten zijn afhankelijk van het type abonnement dat u een Batch-account maken. Kernquota's weergegeven, zijn voor Batch-accounts in de modus voor Batch-service. [De quota in uw Batch-account weergeven](../articles/batch/batch-quota-limit.md#view-batch-quotas).

<sup>1</sup>om aan te vragen een toename van deze limiet overschrijdt, neem contact op met ondersteuning voor Azure.
