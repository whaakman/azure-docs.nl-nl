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
ms.openlocfilehash: abe7668ed16ddd5cc1b247207d3e80f7c1118137
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50964541"
---
| Resource | Maximumaantal |Opmerkingen|
| --- | --- |---|
| Maximumaantal nieuwe taken die elke 30 seconden per Automation-Account (niet-geplande taken) kunnen worden verzonden |100 |Wanneer deze limiet wordt bereikt, mislukken de volgende aanvragen om een taak te maken. De client ontvangt een foutbericht.|
| Maximumaantal gelijktijdige taken die worden uitgevoerd op hetzelfde exemplaar van de tijd per Automation-Account (niet-geplande taken) |200 |Wanneer deze limiet wordt bereikt, mislukken de volgende aanvragen om een taak te maken. De client ontvangt een foutbericht.|
| Maximumaantal modules die elke 30 seconden per Automation-Account kan worden geïmporteerd |5 ||
| Maximale grootte van een Module |100 MB ||
| Tijd voor de taak uitvoeren - gratis laag |500 minuten per abonnement per kalendermaand ||
| Maximale hoeveelheid schijfruimte toegestaan per sandbox  **<sup>1</sup>** |1 GB |Is van toepassing op Azure sandboxes geladen|
| Maximale hoeveelheid geheugen die aan een sandbox  **<sup>1</sup>** |400 MB |Is van toepassing op Azure sandboxes geladen|
| Maximumaantal netwerk sockets toegestaan per sandbox  **<sup>1</sup>** |1000 |Is van toepassing op Azure sandboxes geladen|
| Maximale runtime toegestaan per runbook  **<sup>1</sup>** |3 uur |Is van toepassing op Azure sandboxes geladen|
| Maximumaantal Automation-Accounts in een abonnement |Geen limiet ||
|Maximumaantal gelijktijdige taken die worden uitgevoerd op een enkele Hybrid Runbook Worker|50 ||
| Maximale Runbook Job parameters grootte   | 512 kb||
| Maximale Runbook-parameters   | 50|U kunt een JSON of XML-tekenreeks doorgeven aan een parameter en parseren met het runbook als u de parameterlimiet van 50 bereikt|
| Maximumgrootte van webhook-nettolading |  512 kb|
| Maximaal aantal dagen dat gegevens worden bewaard|30 dagen|

**<sup>1</sup>**  een sandbox is een gedeelde omgeving die kan worden gebruikt door meerdere taken, taken met behulp van de sandbox zijn gebonden aan de resourcebeperkingen van de sandbox.
