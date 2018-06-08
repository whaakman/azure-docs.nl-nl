---
title: bestand opnemen
description: bestand opnemen
services: automation
author: georgewallace
ms.service: automation
ms.topic: include
ms.date: 04/05/2018
ms.author: gwallace
ms.custom: include file
ms.openlocfilehash: 6b6e4afa7c8b18c8ce9af8c6abd371b4321e3343
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34852038"
---
| Resource | Maximumaantal |Opmerkingen|
| --- | --- |---|
| Maximumaantal nieuwe taken die elke 30 seconden per Automation-Account (niet-geplande taken) kunnen worden verzonden |100 |Wanneer deze limiet wordt bereikt, wordt de volgende aanvragen voor het maken van een taak mislukt. De client ontvangt een foutbericht.|
| Maximumaantal gelijktijdige taken die worden uitgevoerd op hetzelfde exemplaar van de tijd per Automation-Account (niet-geplande taken) |200 |Wanneer deze limiet wordt bereikt, wordt de volgende aanvragen voor het maken van een taak mislukt. De client ontvangt een foutbericht.|
| Maximumaantal modules dat elke 30 seconden per Automation-Account kan worden geïmporteerd |5 ||
| Maximale grootte van een Module |100 MB ||
| Taak Run-Time - gratis laag |500 minuten per abonnement per kalendermaand ||
| Maximale hoeveelheid schijfruimte toegestaan per sandbox**<sup>1</sup>** |1 GB |Van toepassing op Azure sandboxes alleen|
| Maximum aantal en de hoeveelheid geheugen aan een sandbox gegeven**<sup>1</sup>** |400 MB |Van toepassing op Azure sandboxes alleen|
| Maximumaantal netwerk sockets toegestaan per sandbox**<sup>1</sup>** |1000 |Van toepassing op Azure sandboxes alleen|
| Maximumaantal Automation-Accounts in een abonnement |Geen limiet ||
|Maximumaantal gelijktijdige taken die worden uitgevoerd op een enkele Hybrid Runbook Worker|50 ||

**<sup>1</sup>**  een sandbox is een gedeelde omgeving die kan worden gebruikt door meerdere jobs, taken met behulp van de sandbox zijn gebonden aan de bronbeperkingen van de sandbox.
