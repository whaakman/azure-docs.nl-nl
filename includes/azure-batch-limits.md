---
title: bestand opnemen
description: bestand opnemen
services: batch
author: laurenhughes
ms.service: batch
ms.topic: include
ms.date: 10/11/2018
ms.author: lahugh
ms.custom: include file
ms.openlocfilehash: 9ffb02fce41e8805dfccf1dfd6e982cf107039ec
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66127549"
---
| **Resource** | **Standaardlimiet** | **Maximumlimiet** |
| --- | --- | --- |
| Azure Batch-accounts per regio per abonnement | 1-3 |50 |
| Toegewezen kerngeheugens per Batch-account | 10-100 | N.v.t.<sup>1</sup> |
| Kernen per Batch-account met lage prioriteit | 10-100 | N.V.T.<sup>2</sup> |
| Actieve taken en taakplanningen<sup>3</sup> per Batch-account | 100-300 | 1,000<sup>4</sup> |
| Pools per Batch-account | 20-100 | 500<sup>4</sup> |

> [!NOTE]
> Standaardlimieten zijn afhankelijk van het type abonnement dat u een Batch-account maken. Kernquota's weergegeven, zijn voor Batch-accounts in de modus voor Batch-service. [De quota in uw Batch-account weergeven](../articles/batch/batch-quota-limit.md#view-batch-quotas). 

<sup>1</sup>het aantal toegewezen kerngeheugens per Batch-account kan worden vergroot, maar het maximum aantal is niet opgegeven. Om de toename van de mogelijkheden bespreken, neem contact op met ondersteuning voor Azure.

<sup>2</sup>het aantal kernen per Batch-account met lage prioriteit kan worden verhoogd, maar het maximum aantal is niet opgegeven. Om de toename van de mogelijkheden bespreken, neem contact op met ondersteuning voor Azure.

<sup>3</sup>voltooide taken en taakplanningen zijn niet beperkt.

<sup>4</sup>om aan te vragen een toename van deze limiet overschrijdt, neem contact op met ondersteuning voor Azure.
