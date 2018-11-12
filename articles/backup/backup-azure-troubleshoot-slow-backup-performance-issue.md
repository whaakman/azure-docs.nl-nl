---
title: Problemen met langzame back-ups van bestanden en mappen in Azure Backup
description: Biedt richtlijnen voor probleemoplossing zodat u kunt de oorzaak van problemen met Azure Backup-prestaties onderzoeken
services: backup
author: genlin
manager: cshepard
ms.service: backup
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: f24a60ab9bdcf1231085de4edeeb89ce1edf4e80
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51248466"
---
# <a name="troubleshoot-slow-backup-of-files-and-folders-in-azure-backup"></a>Problemen met langzame back-ups van bestanden en mappen in Azure Backup
In dit artikel bevat richtlijnen voor probleemoplossing zodat u de oorzaak van trage back-upprestaties voor bestanden en mappen wanneer u Azure Backup. Wanneer u back-up van bestanden met de Azure backup-agent, kan het back-upproces duurt langer dan verwacht. Deze vertraging kan worden veroorzaakt door een of meer van de volgende opties:

* [Er zijn knelpunten op de computer waarop de back-up.](#cause1)
* [Een ander proces of antivirusprogramma verstoort de Azure Backup-proces.](#cause2)
* [De backup-agent wordt uitgevoerd op een Azure-machine (VM).](#cause3)  
* [U wilt maken van een groot aantal (miljoenen) bestanden.](#cause4)

Voordat u begint met het oplossen van problemen, wordt aangeraden dat u downloadt en installeert de [meest recente Azure backup-agent](https://aka.ms/azurebackup_agent). We aanbrengen regelmatige updates in de backup-agent op verschillende problemen oplossen, functies toevoegen en de prestaties verbeteren.

Ook wordt aangeraden dat u bekijkt de [Veelgestelde vragen over Azure Backup-service](backup-azure-backup-faq.md) om ervoor te zorgen dat u nog niet een van de algemene configuratieproblemen ondervindt.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

<a id="cause1"></a>

## <a name="cause-performance-bottlenecks-on-the-computer"></a>Oorzaak: De knelpunten op de computer
Knelpunten op de computer waarop de back-up kunnen dit vertraging veroorzaken. Bijvoorbeeld, de mogelijkheid van de computer te lezen of schrijven naar schijf of de beschikbare bandbreedte voor het verzenden van gegevens via het netwerk, kan leiden tot knelpunten.

Windows biedt een ingebouwde functie die wordt aangeroepen [Prestatiemeter](https://technet.microsoft.com/magazine/2008.08.pulse.aspx) (Perfmon) voor het detecteren van deze knelpunten.

Hier volgen enkele prestatiemeteritems en bereiken die handig zijn bij het vaststellen van knelpunten voor optimale back-ups zijn.

| Teller | Status |
| --- | --- |
| Logische schijf (fysieke schijf)--% niet-actieve |• 100% inactief tot 50% niet-actieve = in orde</br>• 49% inactief tot 20% niet-actieve = Monitor of waarschuwing</br>• 19 procent actieve op 0% niet-actieve = essentiële of van specificaties |
| Logische schijf (fysieke schijf)--% gem. Schijf Sec lezen of schrijven |• 0,001 ms tot 0.015 ms = in orde</br>• 0.015 ms tot 0.025 ms = Monitor of waarschuwing</br>• 0.026 ms of meer = essentiële of van specificaties |
| Logische schijf (fysieke schijf)--huidige wachtrijlengte voor schijf (voor alle exemplaren) |80 aanvragen voor meer dan 6 minuten |
| Geheugen: niet wisselbaar geheugen: Bytes |• Minder dan 60% van de groep van toepassingen die worden gebruikt in orde =<br>• 61% tot 80% van de groep verbruikt = Monitor of waarschuwing</br>• Groter is dan 80% pool verbruikt = essentiële of van specificaties |
| Geheugen: wisselbaar geheugen: Bytes |• Minder dan 60% van de groep van toepassingen die worden gebruikt in orde =</br>• 61% tot 80% van de groep verbruikt = Monitor of waarschuwing</br>• Groter is dan 80% pool verbruikt = essentiële of van specificaties |
| Geheugen: beschikbare Megabytes |• 50% van het beschikbare geheugen beschikbaar of meer = in orde</br>• 25% van het beschikbare geheugen beschikbaar = van Monitor</br>• 10% van het beschikbare geheugen beschikbaar = waarschuwing</br>• Minder dan 100 MB of 5% van het beschikbare geheugen beschikbaar = essentiële of van specificaties |
| Processor--\%processortijd (alle exemplaren) |• Minder dan 60% verbruikt in orde =</br>• 61% tot 90% verbruikt = Monitor of waarschuwing</br>• 91 tot 100% verbruikt = kritiek |

> [!NOTE]
> Als u vaststelt dat de infrastructuur van het overmatig is, wordt u aangeraden dat u de schijven voor betere prestaties regelmatig defragmenteren.
>
>

<a id="cause2"></a>

## <a name="cause-another-process-or-antivirus-software-interfering-with-azure-backup"></a>Oorzaak: Een ander proces of antivirusprogramma verstoort Azure Backup
We hebben gezien dat meerdere exemplaren waar andere processen in het Windows-systeem negatieve invloed is op prestaties van het proces van Azure backup-agent. Bijvoorbeeld, als u zowel de Azure backup-agent en een ander programma gebruikt voor back-up van gegevens, of als antivirussoftware wordt uitgevoerd en een vergrendeling op bestanden naar de back-up worden gemaakt, wordt de meerdere vergrendeld op bestanden kunnen leiden tot conflicten. In dit geval is, mislukken de back-up of de taak duurt langer dan verwacht.

De aanbevolen procedure in dit scenario is het back-upprogramma om te zien of de back-uptijd voor de Azure backup-agent verandert uitschakelen. Meestal is het voldoende om te voorkomen dat ze die betrekking hebben op elkaar ervoor te zorgen dat meerdere back-uptaken op hetzelfde moment niet worden uitgevoerd.

Antivirusprogramma's, wordt u aangeraden dat u de volgende bestanden en locaties uitsluiten:

* C:\Program Files\Microsoft Azure Recovery Services Agent\bin\cbengine.exe als een proces
* C:\Program Files\Microsoft Azure Recovery Services Agent\ mappen
* Scratchlocatie (als u niet de standaard locatie)

<a id="cause3"></a>

## <a name="cause-backup-agent-running-on-an-azure-virtual-machine"></a>Oorzaak: Back-up-agent die wordt uitgevoerd op een Azure-machine
Als u de backup-agent op een virtuele machine uitvoert, worden prestaties langzamer dan wanneer u deze op een fysieke computer uitvoert. Dit is normaal vanwege beperkingen van IOPS.  U kunt echter de prestaties optimaliseren door over te schakelen van de schijven die zijn back-up naar Azure Premium Storage. Er wordt gewerkt aan het oplossen van dit probleem en de oplossing is beschikbaar in een toekomstige release.

<a id="cause4"></a>

## <a name="cause-backing-up-a-large-number-millions-of-files"></a>Oorzaak: Back-ups van een groot aantal (miljoenen) bestanden
Het verplaatsen van een grote hoeveelheid gegevens duurt langer dan een kleinere hoeveelheid gegevens te verplaatsen. In sommige gevallen, is back-uptijd gerelateerd aan niet alleen de grootte van de gegevens, maar ook het aantal bestanden of mappen. Dit is vooral van toepassing wanneer miljoenen kleine bestanden (enkele bytes enkele kilobytes) wordt een back-.

Dit probleem treedt op omdat terwijl u back-ups van de gegevens en verplaatsen naar Azure, Azure uw bestanden tegelijkertijd wordt gecatalogiseerd. In sommige zeldzame gevallen kan duurt de catalogusbewerking langer dan verwacht.

Aan de hand van de volgende indicatoren kunnen u het knelpunt begrijpen en dienovereenkomstig werken op de volgende stappen uit:

* **Gebruikersinterface voortgang wordt weergegeven voor de overdracht van de**. De gegevens worden nog steeds overgebracht. Bandbreedte van het netwerk of de grootte van gegevens mogelijk worden veroorzaakt door vertraging.
* **Gebruikersinterface niet voortgang wordt weergegeven voor de overdracht van de**. Open de logboeken te vinden op C:\Program Files\Microsoft Azure Recovery Services Agent\Temp en vervolgens de vermelding FileProvider::EndData in de logboeken controleren. Deze vermelding geeft aan dat de gegevensoverdracht voltooid en de catalogusbewerking plaatsvindt. Annuleer de back-uptaken niet. In plaats daarvan iets langer wachten voor de catalogusbewerking te voltooien. Als het probleem zich blijft voordoen, neem dan contact op met [ondersteuning van Azure](https://portal.azure.com/#create/Microsoft.Support).
