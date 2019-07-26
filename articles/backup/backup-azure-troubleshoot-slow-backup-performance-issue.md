---
title: Problemen met langzame back-ups van bestanden en mappen in Azure Backup
description: Biedt richt lijnen voor probleem oplossing waarmee u de oorzaak van Azure Backup prestatie problemen kunt vaststellen
author: saurabhsensharma
manager: saurabhsensharma
ms.service: backup
ms.topic: troubleshooting
ms.date: 07/05/2019
ms.author: saurse
ms.openlocfilehash: 3a39d39412c8b64d1851ea0fc9511d116f3b232a
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68465347"
---
# <a name="troubleshoot-slow-backup-of-files-and-folders-in-azure-backup"></a>Problemen met langzame back-ups van bestanden en mappen in Azure Backup
Dit artikel bevat richt lijnen voor probleem oplossing waarmee u de oorzaak van trage back-upprestaties voor bestanden en mappen kunt vaststellen wanneer u Azure Backup gebruikt. Wanneer u de Azure Backup-agent gebruikt voor het maken van back-ups van bestanden, kan het back-upproces langer duren dan verwacht. Deze vertraging kan een of meer van de volgende oorzaken hebben:

* [Er zijn prestatie knelpunten op de computer waarvan een back-up wordt gemaakt.](#cause1)
* [Een ander proces of antivirus software verstoort het Azure Backup proces.](#cause2)
* [De back-upagent wordt uitgevoerd op een virtuele Azure-machine (VM).](#cause3)  
* [U maakt een back-up van een groot aantal (miljoenen) bestanden.](#cause4)

Voordat u problemen gaat oplossen, wordt u aangeraden de [nieuwste Azure backup-agent](https://aka.ms/azurebackup_agent)te downloaden en te installeren. Er worden regel matig updates voor de back-upagent gemaakt om verschillende problemen op te lossen, functies toe te voegen en de prestaties te verbeteren.

We raden u ook ten zeerste aan de [Veelgestelde vragen over de Azure backup-service](backup-azure-backup-faq.md) te controleren om ervoor te zorgen dat u geen veelvoorkomende configuratie problemen ondervindt.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

<a id="cause1"></a>

## <a name="cause-performance-bottlenecks-on-the-computer"></a>Oorzaak: Prestatie knelpunten op de computer
Knel punten op de computer waarvan een back-up wordt gemaakt, kunnen vertragingen veroorzaken. Bijvoorbeeld: de mogelijkheid van de computer om te lezen van of te schrijven naar de schijf of beschik bare band breedte voor het verzenden van gegevens via het netwerk, kan knel punten veroorzaken.

Windows biedt een ingebouwd hulp programma dat [prestatie meter](https://technet.microsoft.com/magazine/2008.08.pulse.aspx) (PerfMon) wordt genoemd om deze knel punten te detecteren.

Hier volgen enkele prestatie meter items en bereiken die nuttig kunnen zijn bij het diagnosticeren van knel punten voor optimale back-ups.

| Teller | Status |
| --- | --- |
| Logische schijf (fysieke schijf)--% inactief |• 100% niet-actief tot 50% inactief = in orde</br>• 49% niet actief tot 20% inactief = waarschuwing of monitor</br>• 19% niet-actief tot 0% inactief = kritiek of van specificatie |
| Logische schijf (fysieke schijf)--% gem. Lees-of schrijf tijd schijf |• 0,001 MS tot 0,015 MS = in orde</br>• 0,015 MS tot 0,025 MS = waarschuwing of monitor</br>• 0,026 MS of langer = kritiek of van de specificatie |
| Logische schijf (fysieke schijf)--huidige wachtrij lengte voor de schijf (voor alle exemplaren) |80 aanvragen langer dan 6 minuten |
| Geheugen--groep niet-wisselbaar bytes |• Minder dan 60% van verbruikte groep = in orde<br>• 61% tot 80% van verbruikte pool = waarschuwing of monitor</br>• Groter dan 80% groep verbruikt = kritiek of van specificatie |
| Geheugen--groeps wisselbaar bytes |• Minder dan 60% van verbruikte groep = in orde</br>• 61% tot 80% van verbruikte pool = waarschuwing of monitor</br>• Groter dan 80% groep verbruikt = kritiek of van specificatie |
| Geheugen-beschik bare mega bytes |• 50% vrije geheugen beschikbaar of meer = in orde</br>• 25% vrije beschik bare geheugen = monitor</br>• 10% van het beschik bare geheugen beschikbaar = waarschuwing</br>• Minder dan 100 MB of 5% van het beschik bare geheugen = kritiek of van de specificatie |
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
* **UI is not showing progress for the data transfer**. Open the logs located at C:\Program Files\Microsoft Azure Recovery Services Agent\Temp, and then check for the FileProvider::EndData entry in the logs. This entry signifies that the data transfer finished and the catalog operation is happening. Don't cancel the backup jobs. Instead, wait a little longer for the catalog operation to finish. If the problem persists, contact [Azure support](https://portal.azure.com/#create/Microsoft.Support).Processor--\`processor tijd (alle exemplaren)es and folders in Azure Backup
description: Provides troubleshooting guidance to help you diagnose the cause of Azure Backup performance issues

author: saurabhsensharma
manager: saurabhsensharma
ms.service: backup
ms.topic: troubleshooting
ms.date: 07/05/2019
ms.author: saurse
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
| Processor--\%Processor Time (all instances) |• Minder dan 60% verbruikt = in orde</br>• 61% tot 90% verbruikt = monitor of waarschuwing</br>• 91% tot 100% verbruikt = kritiek |

> [!NOTE]
> Als u vaststelt dat de infra structuur de culprit is, raden wij u aan de schijven regel matig te defragmenteren voor betere prestaties.
>
>

<a id="cause2"></a>

## <a name="cause-another-process-or-antivirus-software-interfering-with-azure-backup"></a>Oorzaak: Een ander proces of antivirus software die met Azure Backup verstoort
We hebben verschillende instanties gezien waarbij andere processen in het Windows-systeem een negatieve invloed hebben op de prestaties van het proces van de Azure Backup Agent. Als u bijvoorbeeld zowel de Azure Backup Agent als een ander programma gebruikt voor het maken van een back-up van gegevens, of als de antivirus software wordt uitgevoerd en er een vergren deling is ingesteld op bestanden waarvan een back-up moet worden gemaakt, kunnen de meerdere vergren delingen op bestanden leiden tot conflicten. In dit geval kan het maken van de back-up mislukken of kan de taak langer duren dan verwacht.

De aanbevolen aanbeveling in dit scenario is om het andere back-upprogramma uit te scha kelen om te zien of de back-uptijd voor de Azure Backup Agent wordt gewijzigd. Normaal gesp roken zorgt ervoor dat meerdere back-uptaken op hetzelfde moment voldoende zijn om te voor komen dat ze elkaar beïnvloeden.

Voor antivirus Programma's wordt u aangeraden de volgende bestanden en locaties uit te sluiten:

* C:\Program Files\Microsoft Azure Recovery Services Agent\bin\cbengine.exe als een proces
* C:\Program Files\Microsoft Azure Recovery Services agent \ mappen
* Scratch locatie (als u geen gebruik maakt van de standaard locatie)

<a id="cause3"></a>

## <a name="cause-backup-agent-running-on-an-azure-virtual-machine"></a>Oorzaak: Back-upagent die wordt uitgevoerd op een virtuele machine van Azure
Als u de back-upagent op een virtuele machine uitvoert, zijn de prestaties langzamer dan wanneer u deze op een fysieke computer uitvoert. Dit wordt verwacht als gevolg van IOPS-beperkingen.  U kunt de prestaties echter optimaliseren door te scha kelen tussen de gegevens stations waarvan een back-up wordt gemaakt naar Azure Premium Storage. Er wordt gewerkt aan het oplossen van dit probleem en de oplossing is beschikbaar in een toekomstige release.

<a id="cause4"></a>

## <a name="cause-backing-up-a-large-number-millions-of-files"></a>Oorzaak: Back-ups maken van een groot aantal bestanden (miljoenen)
Het verplaatsen van een grote hoeveelheid gegevens duurt langer dan het verplaatsen van een kleiner gegevens volume. In sommige gevallen is de back-uptijd gerelateerd aan niet alleen de grootte van de gegevens, maar ook het aantal bestanden of mappen. Dit geldt met name wanneer een back-up wordt gemaakt van miljoenen kleine bestanden (enkele bytes van een paar kilo bytes).

Dit probleem treedt op omdat u een back-up maakt van de gegevens en deze verplaatst naar Azure, maar Azure uw bestanden tegelijkertijd catalogiseert. In sommige zeldzame scenario's kan de catalogus bewerking langer duren dan verwacht.

De volgende indica toren kunnen u helpen inzicht te krijgen in het knel punt en u kunt de volgende stappen uitvoeren:

* **De gebruikers interface toont de voortgang van de gegevens overdracht**. De gegevens worden nog overgedragen. De netwerk bandbreedte of de omvang van gegevens kan vertragingen veroorzaken.
* **De gebruikers interface geeft geen voortgang weer voor de gegevens overdracht**. Open de logboeken die zich bevinden in C:\Program Files\Microsoft Azure Recovery Services Agent\Temp en controleer vervolgens het item FileProvider:: EndData in de logboeken. Dit item geeft aan dat de gegevens overdracht is voltooid en dat de catalogus bewerking wordt uitgevoerd. Annuleer de back-uptaken niet. Wacht in plaats daarvan iets langer om de catalogus bewerking te volt ooien. Als het probleem zich blijft voordoen, neemt u contact op met de [ondersteuning van Azure](https://portal.azure.com/#create/Microsoft.Support).
