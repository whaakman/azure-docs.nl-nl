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
ms.openlocfilehash: 2823a33b25812a69ad463433bacd9710655c9176
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67176442"
---
#### <a name="process-automation"></a>Procesautomatisering

| Resource | Maximumaantal |Opmerkingen|
| --- | --- |---|
| Maximum aantal nieuwe taken die elke 30 seconden per Azure Automation-account (nonscheduled taken) kunnen worden verzonden |100 |Wanneer deze limiet is bereikt, mislukken de volgende aanvragen om een taak te maken. De client ontvangt een foutbericht.|
| Maximum aantal gelijktijdige taken die worden uitgevoerd op hetzelfde exemplaar van de tijd per Automation-account (nonscheduled taken) |200 |Wanneer deze limiet is bereikt, mislukken de volgende aanvragen om een taak te maken. De client ontvangt een foutbericht.|
| Maximale opslaggrootte van de metagegevens van de taak voor een rolling periode van 30 dagen | 10 GB (ongeveer 4 miljoen taken)|Wanneer deze limiet is bereikt, mislukken de volgende aanvragen om een taak te maken. |
| Maximale stream limiet|1MB|Één stroom kan niet groter zijn dan 1 MB.|
| Maximumaantal modules die elke 30 seconden per Automation-account kan worden geïmporteerd |5 ||
| Maximale grootte van een module |100 MB ||
| Taak-runtime, gratis laag |500 minuten per abonnement per kalendermaand ||
| Maximale hoeveelheid schijfruimte toegestaan per sandbox<sup>1</sup> |1 GB |Van toepassing op Azure sandboxes geladen.|
| Maximale hoeveelheid geheugen die aan een sandbox<sup>1</sup> |400 MB |Van toepassing op Azure sandboxes geladen.|
| Maximum aantal netwerk sockets toegestaan per sandbox<sup>1</sup> |1000 |Van toepassing op Azure sandboxes geladen.|
| Maximale runtime toegestaan per runbook<sup>1</sup> |3 uur |Van toepassing op Azure sandboxes geladen.|
| Maximum aantal Automation-accounts in een abonnement |Geen limiet ||
| Maximum aantal Hybrid Worker-groepen per Automation-Account|4,000||
|Maximum aantal gelijktijdige taken die kunnen worden uitgevoerd op een enkele Hybrid Runbook Worker|50 ||
| De parameter taakgrootte maximale runbook   | 512 kilobits||
| Maximale runbook-parameters   | 50|Als u de limiet van 50-parameter is bereikt, kunt u een JSON of XML-tekenreeks doorgeven aan een parameter en aan het runbook parseren.|
| Maximale webhook-nettolading |  512 kilobits|
| Maximum aantal dagen dat gegevens worden bewaard|30 dagen|
| Maximale grootte van PowerShell workflow-staat |5 MB| Is van toepassing op PowerShell workflow-runbooks bij het plaatsen van controlepunten werkstroom.|

<sup>1</sup>een sandbox is een gedeelde omgeving die kan worden gebruikt door meerdere taken. Taken die gebruikmaken van de sandbox zijn gebonden aan de resourcebeperkingen van de sandbox.

#### <a name="change-tracking-and-inventory"></a>Wijzigingen bijhouden en Inventaris

De volgende tabel bevat de limieten bijgehouden item per computer voor het bijhouden van wijzigingen.

| **Resource** | **Limiet**| **Opmerkingen** |
|---|---|---|
|File|500||
|Register|250||
|Windows-software|250|Geen software-updates.|
|Linux-pakketten|1,250||
|Services|250||
|Daemon|250||

#### <a name="update-management"></a>Updatebeheer

De volgende tabel bevat de limieten voor updatebeheer.

| **Resource** | **Limiet**| **Opmerkingen** |
|---|---|---|
|Aantal machines per update-implementatie|1000||