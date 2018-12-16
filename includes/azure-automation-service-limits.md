---
title: bestand opnemen
description: bestand opnemen
services: automation
author: georgewallace
ms.service: automation
ms.topic: include
ms.date: 12/13/2018
ms.author: gwallace
ms.custom: include file
ms.openlocfilehash: f3ae2289112948dea7d2649c4fad6b1cafb3804b
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53444081"
---
#### <a name="process-automation"></a>Procesautomatisering

| Resource | Maximumaantal |Opmerkingen|
| --- | --- |---|
| Maximumaantal nieuwe taken die elke 30 seconden per Automation-Account (niet-geplande taken) kunnen worden verzonden |100 |Wanneer deze limiet wordt bereikt, mislukken de volgende aanvragen om een taak te maken. De client ontvangt een foutbericht.|
| Maximumaantal gelijktijdige taken die worden uitgevoerd op hetzelfde exemplaar van de tijd per Automation-Account (niet-geplande taken) |200 |Wanneer deze limiet wordt bereikt, mislukken de volgende aanvragen om een taak te maken. De client ontvangt een foutbericht.|
| Maximale opslaggrootte van de metagegevens van de taak voor een rolling periode van 30 dagen. | 10GB (ongeveer 4 miljoen taken)|Wanneer deze limiet wordt bereikt, mislukken de volgende aanvragen om een taak te maken. |
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
| Grootte van de gebruikersstatus Max PowerShell Workflow |5 MB| Is van toepassing op PowerShell Workflow-runbooks bij het plaatsen van controlepunten werkstroom.|

**<sup>1</sup>**  een sandbox is een gedeelde omgeving die kan worden gebruikt door meerdere taken, taken met behulp van de sandbox zijn gebonden aan de resourcebeperkingen van de sandbox.

#### <a name="change-tracking-and-inventory"></a>Wijzigingen bijhouden en Inventaris

De volgende tabel bevat de limieten bijgehouden item per machine voor het bijhouden.

| **Resource** | **Limiet**| **Opmerkingen** |
|---|---|---|
|File|500||
|Register|250||
|Windows-software|250|Bevat geen software-updates|
|Linux-pakketten|1250||
|Services|250||
|Daemon|250||
