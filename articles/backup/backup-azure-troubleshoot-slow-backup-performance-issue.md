---
title: Problemen met langzame back-ups van bestanden en mappen in Azure Backup
description: Biedt richtlijnen voor probleemoplossing zodat u kunt de oorzaak van problemen met Azure Backup-prestaties onderzoeken
services: backup
author: saurabhsensharma
manager: saurabhsensharma
ms.service: backup
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: saurabhsensharma
ms.openlocfilehash: 1bc9c7b4f6e2a4f2a7c712d966caac74b73518df
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/04/2019
ms.locfileid: "67565664"
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

## <a name="cause-performance-bottlenecks-on-the-computer"></a>Oorzaak: Knelpunten op de computer
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
| Processor--\%Processor Time (all instances) |• Less than 60% consumed = Healthy</br>• 61% to 90% consumed = Monitor or Caution</br>• 91% to 100% consumed = Critical |

> [!NOTE]
> If you determine that the infrastructure is the culprit, we recommend that you defragment the disks regularly for better performance.
>
>

<a id="cause2"></a>

## Cause: Another process or antivirus software interfering with Azure Backup
We've seen several instances where other processes in the Windows system have negatively affected performance of the Azure Backup agent process. For example, if you use both the Azure Backup agent and another program to back up data, or if antivirus software is running and has a lock on files to be backed up, the multiple locks on files might cause contention. In this situation, the backup might fail, or the job might take longer than expected.

The best recommendation in this scenario is to turn off the other backup program to see whether the backup time for the Azure Backup agent changes. Usually, making sure that multiple backup jobs are not running at the same time is sufficient to prevent them from affecting each other.

For antivirus programs, we recommend that you exclude the following files and locations:

* C:\Program Files\Microsoft Azure Recovery Services Agent\bin\cbengine.exe as a process
* C:\Program Files\Microsoft Azure Recovery Services Agent\ folders
* Scratch location (if you're not using the standard location)

<a id="cause3"></a>

## Cause: Backup agent running on an Azure virtual machine
If you're running the Backup agent on a VM, performance will be slower than when you run it on a physical machine. This is expected due to IOPS limitations.  However, you can optimize the performance by switching the data drives that are being backed up to Azure Premium Storage. We're working on fixing this issue, and the fix will be available in a future release.

<a id="cause4"></a>

## Cause: Backing up a large number (millions) of files
Moving a large volume of data will take longer than moving a smaller volume of data. In some cases, backup time is related to not only the size of the data, but also the number of files or folders. This is especially true when millions of small files (a few bytes to a few kilobytes) are being backed up.

This behavior occurs because while you're backing up the data and moving it to Azure, Azure is simultaneously cataloging your files. In some rare scenarios, the catalog operation might take longer than expected.

The following indicators can help you understand the bottleneck and accordingly work on the next steps:

* **UI is showing progress for the data transfer**. The data is still being transferred. The network bandwidth or the size of data might be causing delays.
* **UI is not showing progress for the data transfer**. Open the logs located at C:\Program Files\Microsoft Azure Recovery Services Agent\Temp, and then check for the FileProvider::EndData entry in the logs. This entry signifies that the data transfer finished and the catalog operation is happening. Don't cancel the backup jobs. Instead, wait a little longer for the catalog operation to finish. If the problem persists, contact [Azure support](https://portal.azure.com/#create/Microsoft.Support).Processor--\`processortijd (alle exemplaren)es and folders in Azure Backup
description: Provides troubleshooting guidance to help you diagnose the cause of Azure Backup performance issues
services: backup
author: saurabhsensharma
manager: saurabhsensharma
ms.service: backup
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: saurabhsensharma
---
# Troubleshoot slow backup of files and folders in Azure Backup
This article provides troubleshooting guidance to help you diagnose the cause of slow backup performance for files and folders when you're using Azure Backup. When you use the Azure Backup agent to back up files, the backup process might take longer than expected. This delay might be caused by one or more of the following:

* [There are performance bottlenecks on the computer that’s being backed up.](#cause1)
* [Another process or antivirus software is interfering with the Azure Backup process.](#cause2)
* [The Backup agent is running on an Azure virtual machine (VM).](#cause3)  
* [You're backing up a large number (millions) of files.](#cause4)

Before you start troubleshooting issues, we recommend that you download and install the [latest Azure Backup agent](https://aka.ms/azurebackup_agent). We make frequent
updates to the Backup agent to fix various issues, add features, and improve performance.

We also strongly recommend that you review the [Azure Backup service FAQ](backup-azure-backup-faq.md) to make sure you're not experiencing any of the common configuration issues.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

<a id="cause1"></a>

## Cause: Performance bottlenecks on the computer
Bottlenecks on the computer that's being backed up can cause delays. For example, the computer's ability to read or write to disk, or available bandwidth to send data over the network, can cause bottlenecks.

Windows provides a built-in tool that's called [Performance Monitor](https://technet.microsoft.com/magazine/2008.08.pulse.aspx) (Perfmon) to detect these bottlenecks.

Here are some performance counters and ranges that can be helpful in diagnosing bottlenecks for optimal backups.

| Counter | Status |
| --- | --- |
| Logical Disk(Physical Disk)--%idle |• 100% idle to 50% idle = Healthy</br>• 49% idle to 20% idle = Warning or Monitor</br>• 19% idle to 0% idle = Critical or Out of Spec |
| Logical Disk(Physical Disk)--%Avg. Disk Sec Read or Write |• 0.001 ms to 0.015 ms  = Healthy</br>• 0.015 ms to 0.025 ms = Warning or Monitor</br>• 0.026 ms or longer = Critical or Out of Spec |
| Logical Disk(Physical Disk)--Current Disk Queue Length (for all instances) |80 requests for more than 6 minutes |
| Memory--Pool Non Paged Bytes |• Less than 60% of pool consumed = Healthy<br>• 61% to 80% of pool consumed = Warning or Monitor</br>• Greater than 80% pool consumed = Critical or Out of Spec |
| Memory--Pool Paged Bytes |• Less than 60% of pool consumed = Healthy</br>• 61% to 80% of pool consumed = Warning or Monitor</br>• Greater than 80% pool consumed = Critical or Out of Spec |
| Memory--Available Megabytes |• 50% of free memory available or more = Healthy</br>• 25% of free memory available = Monitor</br>• 10% of free memory available = Warning</br>• Less than 100 MB or 5% of free memory available = Critical or Out of Spec |
| Processor--\%Processor Time (all instances) |• Minder dan 60% verbruikt in orde =</br>• 61% tot 90% verbruikt = Monitor of waarschuwing</br>• 91 tot 100% verbruikt = kritiek |

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

## <a name="cause-backup-agent-running-on-an-azure-virtual-machine"></a>Oorzaak: Back-agent die wordt uitgevoerd op een Azure-machine
Als u de backup-agent op een virtuele machine uitvoert, worden prestaties langzamer dan wanneer u deze op een fysieke computer uitvoert. Dit is normaal vanwege beperkingen van IOPS.  U kunt echter de prestaties optimaliseren door over te schakelen van de schijven die zijn back-up naar Azure Premium Storage. Er wordt gewerkt aan het oplossen van dit probleem en de oplossing is beschikbaar in een toekomstige release.

<a id="cause4"></a>

## <a name="cause-backing-up-a-large-number-millions-of-files"></a>Oorzaak: Back-ups van een groot aantal (miljoenen) bestanden
Het verplaatsen van een grote hoeveelheid gegevens duurt langer dan een kleinere hoeveelheid gegevens te verplaatsen. In sommige gevallen, is back-uptijd gerelateerd aan niet alleen de grootte van de gegevens, maar ook het aantal bestanden of mappen. Dit is vooral van toepassing wanneer miljoenen kleine bestanden (enkele bytes enkele kilobytes) wordt een back-.

Dit probleem treedt op omdat terwijl u back-ups van de gegevens en verplaatsen naar Azure, Azure uw bestanden tegelijkertijd wordt gecatalogiseerd. In sommige zeldzame gevallen kan duurt de catalogusbewerking langer dan verwacht.

Aan de hand van de volgende indicatoren kunnen u het knelpunt begrijpen en dienovereenkomstig werken op de volgende stappen uit:

* **Gebruikersinterface voortgang wordt weergegeven voor de overdracht van de**. De gegevens worden nog steeds overgebracht. Bandbreedte van het netwerk of de grootte van gegevens mogelijk worden veroorzaakt door vertraging.
* **Gebruikersinterface niet voortgang wordt weergegeven voor de overdracht van de**. Open de logboeken te vinden op C:\Program Files\Microsoft Azure Recovery Services Agent\Temp en vervolgens de vermelding FileProvider::EndData in de logboeken controleren. Deze vermelding geeft aan dat de gegevensoverdracht voltooid en de catalogusbewerking plaatsvindt. Annuleer de back-uptaken niet. In plaats daarvan iets langer wachten voor de catalogusbewerking te voltooien. Als het probleem zich blijft voordoen, neem dan contact op met [ondersteuning van Azure](https://portal.azure.com/#create/Microsoft.Support).
