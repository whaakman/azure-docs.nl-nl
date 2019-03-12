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
ms.openlocfilehash: 6b48dbfc33890df12209c3a242d812ad2103e07a
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553348"
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
