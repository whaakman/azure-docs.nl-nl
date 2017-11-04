---
title: Problemen met trage back-up van bestanden en mappen in Azure Backup | Microsoft Docs
description: Biedt richtlijnen voor probleemoplossing waarmee u de oorzaak van de Azure Backup prestatieproblemen analyseren
services: backup
documentationcenter: 
author: genlin
manager: cshepard
editor: 
ms.assetid: e379180a-db13-4e0c-90e4-28e5dd6f5b14
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: 373a98855886cc7be7518c664f82bb6f92ca86f3
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="troubleshoot-slow-backup-of-files-and-folders-in-azure-backup"></a>Problemen met langzame back-ups van bestanden en mappen in Azure Backup
Dit artikel bevat richtlijnen voor probleemoplossing voor het vaststellen van de oorzaak van trage prestaties van back-up voor bestanden en mappen wanneer u Azure Backup. Wanneer u de Azure Backup agent back-up van bestanden gebruikt, is het back-upproces duurt langer dan verwacht. Deze vertraging kan worden veroorzaakt door een of meer van de volgende opties:

* [Er zijn knelpunten op de computer die wordt back-up.](#cause1)
* [Een ander proces of anti-virussoftware is interactie aangaan met het Azure Backup-proces.](#cause2)
* [De backup-agent wordt uitgevoerd op Azure een virtuele machine (VM).](#cause3)  
* [U een back-up van een groot aantal (miljoenen) bestanden.](#cause4)

Voordat u begint met het oplossen van problemen, het is raadzaam dat u downloadt en installeert de [meest recente Azure backup-agent](http://aka.ms/azurebackup_agent). We aanbrengen regelmatig updates in de backup-agent op verschillende problemen oplossen en de prestaties verbeteren onderdelen toevoegen.

Ook wordt aangeraden u wordt aangeraden de [Veelgestelde vragen over Azure Backup-service](backup-azure-backup-faq.md) om ervoor te zorgen dat u een van de algemene configuratiekwesties niet ondervindt.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

<a id="cause1"></a>

## <a name="cause-performance-bottlenecks-on-the-computer"></a>Oorzaak: Prestatieknelpunten op de computer
Knelpunten op de computer die wordt back-up kunnen leiden tot vertragingen. De computer de mogelijkheid om te lezen of schrijven naar schijf of de beschikbare bandbreedte om gegevens te verzenden via het netwerk, kan bijvoorbeeld knelpunten veroorzaken.

Windows biedt een ingebouwde functie die wordt aangeroepen [Prestatiemeter](https://technet.microsoft.com/magazine/2008.08.pulse.aspx) (Perfmon) voor het detecteren van deze knelpunten.

Hier volgen enkele prestatiemeteritems en de bereiken die handig zijn bij het oplossen van knelpunten voor optimale back-ups zijn.

| Prestatiemeteritems | Status |
| --- | --- |
| Logische schijf (fysieke schijf)--% inactief |• 100% inactief op 50% inactief goed =</br>• 49% 20% inactief inactief = Monitor of waarschuwing</br>• 19% inactief op 0% inactief essentiële of buiten Spec = |
| Logische schijf (fysieke schijf)--% gem. Schijf per seconde lezen of schrijven |• 0,001 ms 0.015 MS goed =</br>• 0.015 ms 0.025 MS = Monitor of waarschuwing</br>• 0.026 ms of meer = essentiële of buiten Spec |
| Logische schijf (fysieke schijf)--huidige wachtrijlengte voor schijf (voor alle exemplaren) |80 aanvragen voor meer dan 6 minuten |
| Geheugen--niet wisselbaar geheugen: Bytes |• Minder dan 60% van de groep verbruikt goed =<br>• 61% tot 80% van de groep verbruikt = Monitor of waarschuwing</br>• Groter zijn dan 80% groep verbruikt essentiële of buiten Spec = |
| Geheugen - wisselbaar geheugen: Bytes |• Minder dan 60% van de groep verbruikt goed =</br>• 61% tot 80% van de groep verbruikt = Monitor of waarschuwing</br>• Groter zijn dan 80% groep verbruikt essentiële of buiten Spec = |
| Geheugen: beschikbare Megabytes |• 50% van het beschikbare geheugen beschikbaar of meer goed =</br>• 25% van het beschikbare geheugen beschikbaar = Monitor</br>• 10% van het beschikbare geheugen beschikbaar = waarschuwing</br>• Minder dan 100 MB of 5% van het beschikbare geheugen beschikbaar essentiële of buiten Spec = |
| Processor--\%processortijd (alle exemplaren) |• Minder dan 60% verbruikt goed =</br>• 61% tot 90% verbruikt = Monitor of waarschuwing</br>• 91 tot 100% verbruikt = kritiek |

> [!NOTE]
> Als u vaststelt dat de infrastructuur de stackpad is, wordt u aangeraden de schijven voor betere prestaties regelmatig te defragmenteren.
>
>

<a id="cause2"></a>

## <a name="cause-another-process-or-antivirus-software-interfering-with-azure-backup"></a>Oorzaak: Een ander proces of antivirussoftware interactie aangaan met Azure Backup
We hebt enkele exemplaren waar andere processen in het Windows-systeem een negatieve invloed is op prestaties van het proces van Azure Backup agent gezien. Als u de Azure Backup agent en een ander programma back-up kunt gebruiken, of als antivirussoftware wordt uitgevoerd en bestanden naar een back-up heeft vergrendeld, het veelvoud wordt vergrendeld op kunnen bestanden conflicten veroorzaken. In dit geval is, mislukken de back-up of de taak duurt langer dan verwacht.

De aanbeveling in dit scenario is het back-upprogramma om te zien of de back-uptijd voor de Azure Backup agent verandert uitschakelen. Om ervoor te zorgen dat meerdere back-uptaken niet worden uitgevoerd op hetzelfde moment is meestal voldoende om te voorkomen dat ze elkaar beïnvloeden.

Antivirusprogramma's, wordt u aangeraden dat u de volgende bestanden en locaties uitsluiten:

* C:\Program Files\Microsoft Azure Recovery Services Agent\bin\cbengine.exe als een proces
* C:\Program Files\Microsoft Azure Recovery Services Agent\ mappen
* Locatie scratchruimte (als u niet de standaard locatie)

<a id="cause3"></a>

## <a name="cause-backup-agent-running-on-an-azure-virtual-machine"></a>Oorzaak: Back-up-agent wordt uitgevoerd op een virtuele machine van Azure
Als u de backup-agent op een virtuele machine uitvoert, worden prestaties langzamer dan wanneer u deze op een fysieke computer uitvoert. Dit is normaal door beperkingen van de IOPS.  U kunt echter de prestaties optimaliseren door het overschakelen van de schijven die zijn wordt een back-up naar Azure Premium-opslag. We proberen op het oplossen van dit probleem en de oplossing is beschikbaar in een toekomstige release.

<a id="cause4"></a>

## <a name="cause-backing-up-a-large-number-millions-of-files"></a>Oorzaak: Back-ups van een groot aantal (miljoenen) bestanden
Het verplaatsen van een grote hoeveelheid gegevens duurt langer dan het verplaatsen van een kleinere hoeveelheid gegevens. In sommige gevallen is back-up gerelateerd aan niet alleen de grootte van de gegevens, maar het aantal bestanden of mappen. Dit geldt vooral als miljoenen kleine bestanden (enkele bytes aan enkele kilobytes) wordt een back-up.

Dit probleem treedt op omdat terwijl u een back-up van de gegevens en verplaatsen naar Azure, Azure uw bestanden tegelijkertijd is gecatalogiseerd. In sommige zeldzame gevallen kan de catalogusbewerking duurt mogelijk langer dan verwacht.

Aan de hand van de volgende indicatoren kunnen u het knelpunt begrijpen en dienovereenkomstig werken op de volgende stappen:

* **Gebruikersinterface voortgang wordt weergegeven voor de gegevensoverdracht**. De gegevens wordt nog steeds overgedragen. De netwerkbandbreedte of de grootte van gegevens kan worden veroorzaakt door vertragingen.
* **Gebruikersinterface niet voortgang wordt weergegeven voor de gegevensoverdracht**. Open de logboeken die zich bevindt op C:\Microsoft Azure Recovery Services Agent\Temp en controleer of de vermelding FileProvider::EndData in de logboeken. Deze vermelding geeft aan dat de gegevensoverdracht voltooid en de catalogusbewerking er gebeurt. De back-uptaken niet worden geannuleerd. In plaats daarvan wachten nog even voor de catalogusbewerking te voltooien. Als het probleem zich blijft voordoen, neem dan contact op met [ondersteuning van Azure](https://portal.azure.com/#create/Microsoft.Support).
