---
title: bestand opnemen
description: bestand opnemen
services: batch
author: dlepow
ms.service: batch
ms.topic: include
ms.date: 10/11/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: c98a2146a019817152be9fae76638dbaa4d9de3d
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49458833"
---
| **Resource** | **Standaardlimiet** | **Maximumaantal** |
| --- | --- | --- |
| Batch-accounts per regio per abonnement | 1 - 3 |50 |
| Toegewezen kerngeheugens per Batch-account | 10 - 100 | N.v.t.<sup>1</sup> |
| Kernen per Batch-account met lage prioriteit | 10 - 100 | N.V.T.<sup>2</sup> |
| Actieve taken en taakplanningen<sup>3</sup> per Batch-account | 100 - 300 | 1000<sup>4</sup> |
| Pools per Batch-account | 20 - 100 | 500<sup>4</sup> |

> [!NOTE]
> Standaardlimieten zijn afhankelijk van het type abonnement dat u een Batch-account maken. Kernquota's weergegeven, zijn voor Batch-accounts in de modus voor Batch-service. [De quota in uw Batch-account weergeven](../articles/batch/batch-quota-limit.md#view-batch-quotas). 

<sup>1</sup> het aantal toegewezen kerngeheugens per Batch-account kan worden vergroot, maar het maximum aantal is niet opgegeven. Neem contact op met ondersteuning van Azure wilt discussiëren over meer opties.

<sup>2</sup> het aantal kernen per Batch-account met lage prioriteit kan worden verhoogd, maar het maximum aantal is niet opgegeven. Neem contact op met ondersteuning van Azure wilt discussiëren over meer opties.

<sup>3</sup> voltooide taken en taakplanningen zijn niet beperkt.

<sup>4</sup> Neem contact op met Azure-ondersteuning als u wilt een verhoging boven deze limiet.
