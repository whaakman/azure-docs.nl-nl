---
title: 'Azure Backup: toepassingsconsistente back-ups van virtuele Linux-machines'
description: Maak toepassingsconsistente back-ups van uw virtuele Linux-machines naar Azure. In dit artikel wordt uitgelegd voor het configureren van de script-framework voor back-up van Linux-VM's geïmplementeerd met Azure. In dit artikel bevat ook informatie voor probleemoplossing.
services: backup
author: anuragmehrotra
manager: shivamg
keywords: App-consistente back-up. App-consistente Azure-VM back-up. Back-up van Linux-VM. Azure Backup
ms.service: backup
ms.topic: conceptual
ms.date: 1/12/2018
ms.author: anuragm
ms.openlocfilehash: a81c0b9c87db85771fcecab87c6b9ac88dcbd472
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53581842"
---
# <a name="application-consistent-backup-of-azure-linux-vms"></a>Toepassingsconsistente back-up van Azure Linux VM 's

Bij het maken van back-momentopnamen van uw virtuele machines, toepassingsconsistentie betekent dat uw toepassingen starten wanneer de virtuele machines worden opgestart nadat het wordt hersteld. Zoals u zien kunt, is consistentie van toepassingen erg belangrijk. Uw virtuele Linux-machines zijn om ervoor te zorgen consistent is, dat kunt u de Linux-Pre-script en post-script framework voor de toepassingsconsistente back-ups maken van toepassing. De Pre-script en post-script framework ondersteunt Linux Azure Resource Manager geïmplementeerde virtuele machines. Scripts voor toepassingsconsistentie bieden geen ondersteuning voor virtuele machines geïmplementeerd met een Service-beheer of Windows virtuele machines.

## <a name="how-the-framework-works"></a>De werking van het framework

Het framework biedt een optie voor het uitvoeren van aangepaste scripts die voorafgaan aan en scripts na tijdens het maken van momentopnamen van de VM. Scripts die voorafgaan aan uitgevoerd net voordat u de VM-momentopname en scripts die volgen op uitgevoerd onmiddellijk nadat u de VM-momentopname. Scripts die voorafgaan aan en scripts die volgen op bieden de flexibiliteit om te bepalen van uw toepassing en de omgeving, tijdens het maken van momentopnamen van de VM.

Scripts die voorafgaan aan aanroepen systeemeigen toepassing API's, welke stilleggen de IOs, en in-memory inhoud leegmaken naar de schijf. Deze acties Zorg ervoor dat de momentopname toepassingsconsistent is. Scripts die volgen op gebruik systeemeigen toepassing API's aan de IOs-, waarmee de toepassing naar de normale bewerkingen hervatten nadat de VM-momentopname ontdooien.

## <a name="steps-to-configure-pre-script-and-post-script"></a>Stappen voor het configureren van Pre-script en post-script

1. Meld u aan als hoofdgebruiker voor de Linux-VM die u back wilt-up.

2. Van [GitHub](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig), download **VMSnapshotScriptPluginConfig.json** en kopieert u het naar de **/etc/azure** map voor alle virtuele machines die u back wilt-up. Als de **/etc/azure** map niet bestaat, maakt u deze.

3. Kopieer de Pre-script en post-script voor uw toepassing op alle virtuele machines die u van plan een back bent-up. U kunt de scripts kopiëren naar een locatie op de virtuele machine. Zorg ervoor dat het volledige pad van de scriptbestanden in de **VMSnapshotScriptPluginConfig.json** bestand.

4. Zorg ervoor dat de volgende machtigingen voor deze bestanden:

   - **VMSnapshotScriptPluginConfig.json**: Machtiging "600." Bijvoorbeeld alleen "root" gebruiker "lezen" en 'schrijven' machtigingen moet hebben tot dit bestand en er is geen gebruiker moet beschikken over machtigingen voor "uitvoeren".

   - **Pre-script bestand**: Machtiging "700."  Bijvoorbeeld alleen "root" gebruiker moet 'lezen', 'schrijven' en 'uitvoeren'-machtigingen voor dit bestand.

   - **Script dat volgt** machtiging "700." Bijvoorbeeld alleen "root" gebruiker moet 'lezen', 'schrijven' en 'uitvoeren'-machtigingen voor dit bestand.

   > [!Important]
   > Het framework biedt gebruikers veel mogelijkheden. Beveiligen van het framework en zorg ervoor dat alleen "root" gebruiker heeft toegang tot kritieke JSON en scriptbestanden.
   > Als niet worden voldaan aan de vereisten, wordt niet het script uitgevoerd, waardoor het in een bestand system crashes en inconsistente back-up.
   >

5. Configureer **VMSnapshotScriptPluginConfig.json** zoals hier wordt beschreven:
    - **naam invoegtoepassing**: Laat dit veld is, of uw scripts werken mogelijk niet zoals verwacht.

    - **preScriptLocation**: Geef het volledige pad van het script dat vooraf op de virtuele machine die u wilt een back-up.

    - **postScriptLocation**: Geef het volledige pad van het script dat volgt op de virtuele machine die u wilt back-up worden gemaakt.

    - **preScriptParams**: Geef de volgende optionele parameters die moeten worden doorgegeven aan het Pre-script. Alle parameters moeten worden opgegeven tussen aanhalingstekens. Als u meerdere parameters gebruikt, scheidt u de parameters met een door komma's.

    - **postScriptParams**: Geef de volgende optionele parameters die moeten worden doorgegeven aan het script dat volgt. Alle parameters moeten worden opgegeven tussen aanhalingstekens. Als u meerdere parameters gebruikt, scheidt u de parameters met een door komma's.

    - **preScriptNoOfRetries**: Stel het aantal keren dat die de Pre-script opnieuw moet worden uitgevoerd als er een fout beëindigd is. Nul betekent dat slechts één probeer en er geen nieuwe als er een storing optreedt.

    - **postScriptNoOfRetries**:  Stel het aantal keren dat die het script dat volgt opnieuw moet worden uitgevoerd als er een fout beëindigd is. Nul betekent dat slechts één probeer en er geen nieuwe als er een storing optreedt.

    - **Time-outInSeconden**: Geef individuele time-outs voor de Pre-script en het script dat volgt (maximumwaarde kan 1800 zijn).

    - **continueBackupOnFailure**: Deze waarde instelt op **waar** als u wilt dat Azure Backup terugvallen op een bestand system consistente/crashconsistente back-up als Pre-script of mislukt na een script. Als u dit op **false** mislukt de back-up in geval van storing script (met uitzondering van wanneer u één schijf-VM die op het crashconsistente back-up, ongeacht deze instelling terugvalt hebt).

    - **fsFreezeEnabled**: Geef op of Linux-fsfreeze moet worden aangeroepen tijdens het maken van de VM-momentopname om te controleren of de consistentie van bestandssysteem. We raden u deze instelling is ingesteld op **waar** , tenzij uw toepassing een afhankelijkheid heeft van fsfreeze uitschakelen.

6. Het script-framework is nu geconfigureerd. Als de back-up van virtuele machine al is geconfigureerd, wordt de volgende back-up roept de scripts en toepassingsconsistente back-up wordt geactiveerd. Als de virtuele machine back-up niet is geconfigureerd, configureert u deze met behulp van [maakt u een Back-up van virtuele Azure-machines naar Recovery Services-kluizen.](https://docs.microsoft.com/azure/backup/backup-azure-vms-first-look-arm)

## <a name="troubleshooting"></a>Problemen oplossen

Zorg ervoor dat u passende logboekregistratie tijdens het schrijven van uw Pre-script en post-script toevoegen, en bekijk de scriptlogboeken van uw voor het oplossen van problemen met script. Als u nog steeds problemen bij het uitvoeren van scripts hebt, raadpleegt u de volgende tabel voor meer informatie.

| Fout | Foutbericht | Aanbevolen actie |
| ------------------------ | -------------- | ------------------ |
| Pre-ScriptExecutionFailed |Het script voorafgaand, wordt er een fout geretourneerd zodat mogelijk toepassingsconsistente back-up niet.   | Bekijk de logboeken van de fout voor het script om het probleem te verhelpen.|  
|   Post-ScriptExecutionFailed |    Het script dat volgt heeft een fout die mogelijk van invloed zijn op de status van toepassing. |    Bekijk de logboeken van de fout voor het script los het probleem en de status van de toepassing controleren. |
| Pre-ScriptNotFound |  Het script voorafgaand is niet gevonden op de locatie die opgegeven in de **VMSnapshotScriptPluginConfig.json** config-bestand. |   Zorg ervoor dat dat Pre-script is aanwezig in het pad dat opgegeven in het configuratiebestand zodat toepassingsconsistente back-up.|
| Post-ScriptNotFound | Het script dat volgt is niet gevonden op de locatie die opgegeven in de **VMSnapshotScriptPluginConfig.json** config-bestand. |   Zorg ervoor dat dit script dat volgt is aanwezig in het pad dat opgegeven in het configuratiebestand zodat toepassingsconsistente back-up.|
| IncorrectPluginhostFile | De **Pluginhost** -bestand, dat wordt geleverd met de extensie VmSnapshotLinux, is beschadigd, zodat het Pre-script en post-script kunnen niet worden uitgevoerd en de back-up niet toepassingsconsistent. | Verwijder de **VmSnapshotLinux** , en er wordt automatisch opnieuw worden geïnstalleerd met de volgende back-up om het probleem te verhelpen. |
| IncorrectJSONConfigFile | De **VMSnapshotScriptPluginConfig.json** bestand is onjuist, dus Pre-script en post-script kan niet worden uitgevoerd en de back-up niet toepassingsconsistent. | Downloaden van het exemplaar van [GitHub](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig) en opnieuw configureren. |
| InsufficientPermissionforPre-Script | Voor het uitvoeren van scripts, "root" gebruiker de eigenaar van het bestand moet zijn en het bestand moet bevatten "700" machtigingen (dat wil zeggen, alleen 'eigenaar' moet 'lezen', 'schrijven' en machtigingen voor "uitvoeren"). | Zorg ervoor dat 'root' gebruiker 'eigenaar' van het scriptbestand is en dat alleen de 'eigenaar' is 'lezen', 'schrijven' en 'uitvoeren'-machtigingen. |
| InsufficientPermissionforPost-Script | Hoofdgebruiker moet voor het uitvoeren van scripts, de eigenaar van het bestand en het bestand moet bevatten "700" machtigingen (dat wil zeggen, alleen 'eigenaar' moet 'lezen', 'schrijven' en machtigingen voor "uitvoeren"). | Zorg ervoor dat 'root' gebruiker 'eigenaar' van het scriptbestand is en dat alleen de 'eigenaar' is 'lezen', 'schrijven' en 'uitvoeren'-machtigingen. |
| Pre-ScriptTimeout | De uitvoering van de toepassingsconsistente back-up Pre-script is een time-out. | Controleer het script en verhoog de time-out in de **VMSnapshotScriptPluginConfig.json** bestand bevindt zich op **/etc/azure**. |
| Post-ScriptTimeout | Er is een time-out opgetreden voor de uitvoering van de toepassingsconsistente back-up na script. | Controleer het script en verhoog de time-out in de **VMSnapshotScriptPluginConfig.json** bestand bevindt zich op **/etc/azure**. |

## <a name="next-steps"></a>Volgende stappen
[Back-up van virtuele machine naar een Recovery Services-kluis configureren](https://docs.microsoft.com/azure/backup/backup-azure-arm-vms)
