---
title: 'Azure Backup: toepassingsconsistente back-ups van virtuele Linux-machines | Microsoft Docs'
description: Scripts gebruiken voor het garanderen van toepassingsconsistente back-ups naar Azure voor uw virtuele Linux-machines. De scripts alleen van toepassing op Linux VM's in een implementatie van Resource Manager; de scripts niet van toepassing op VM's van Windows of service manager-implementaties. In dit artikel leert u de stappen voor het configureren van de scripts, inclusief het oplossen van problemen.
services: backup
documentationcenter: dev-center-name
author: anuragmehrotra
manager: shivamg
keywords: App-consistente back-up. toepassingsconsistente virtuele machine van Azure back-up. Back-up van Linux-VM. Azure Backup
ms.assetid: bbb99cf2-d8c7-4b3d-8b29-eadc0fed3bef
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 4/12/2017
ms.author: anuragm;markgal
ms.openlocfilehash: 378c65bec8fd1f880ed459e76f5e4b5d85e49d2a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="application-consistent-backup-of-azure-linux-vms-preview"></a>Toepassingsconsistente back-up van Azure Linux VM's (preview)

Dit artikel vertelt over het Linux vooraf script en framework en hoe deze kan worden gebruikt te ondernemen toepassingsconsistente back-ups van Linux virtuele Azure-machines na een script.

> [!Note]
> Het script voor vóór en na-framework wordt alleen ondersteund voor Linux Azure Resource Manager geïmplementeerde virtuele machines. Scripts voor consistentie van de toepassing worden niet ondersteund voor Service Manager geïmplementeerde virtuele machines of virtuele machines van Windows.
>

## <a name="how-the-framework-works"></a>De werking van het framework

Het framework biedt een optie voor het uitvoeren van aangepaste scripts voor vóór en na scripts tijdens het maken van VM-momentopnamen. Vooraf scripts worden uitgevoerd voordat u de VM-momentopname en na scripts worden uitgevoerd onmiddellijk nadat de momentopname van het VM. Dit biedt u de flexibiliteit om te bepalen van uw toepassing en de omgeving tijdens het maken van VM-momentopnamen.

In dit scenario is het belangrijk om ervoor te zorgen toepassingsconsistente back-up van virtuele machine. Het vooraf script kunt aanroepen toepassing systeemeigen API's voor stilleggen de IOs en leegmaken van de inhoud in het geheugen naar de schijf. Dit zorgt ervoor dat de momentopname toepassingsconsistente (dat wil zeggen dat de toepassing wordt geleverd up wanneer de virtuele machine wordt opgestart na herstellen). Na script kan worden gebruikt voor de IOs ontdooien. Dit gebeurt met behulp van de toepassing-systeemeigen API's zodat de toepassing normale bewerkingen post-VM-momentopname hervatten kunt.

## <a name="steps-to-configure-pre-script-and-post-script"></a>Stappen voor het script voor vóór en na configureren

1. Meld u aan als hoofdgebruiker voor Linux VM die u back wilt-up.

2. Download **VMSnapshotScriptPluginConfig.json** van [GitHub](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig), en vervolgens kopiëren naar de **/etc/azure** map op de virtuele machines die u back wilt-up. Maak de **/etc/azure** directory als deze niet al bestaat.

3. Kopieer het script dat vóór en na voor uw toepassing op alle virtuele machines die u back wilt-up. U kunt de scripts kopiëren naar een locatie op de virtuele machine. Zorg ervoor dat het volledige pad van de scriptbestanden in de **VMSnapshotScriptPluginConfig.json** bestand.

4. Zorg ervoor dat de volgende machtigingen voor deze bestanden:

   - **VMSnapshotScriptPluginConfig.json**: machtiging "600." Bijvoorbeeld alleen 'root' gebruiker kan de machtigingen 'lezen' en 'schrijven' moet hebben tot dit bestand en er is geen gebruiker moet beschikken over machtigingen voor ' uitvoeren'.

   - **Vooraf scriptbestand**: machtiging "700."  Alleen 'root' gebruiker moet bijvoorbeeld 'lezen', 'schrijven' en 'uitvoeren' machtigingen voor dit bestand.
  
   - **Na script** machtiging "700." Alleen 'root' gebruiker moet bijvoorbeeld 'lezen', 'schrijven' en 'uitvoeren' machtigingen voor dit bestand.

   > [!Important]
   > Het framework geeft gebruikers veel stroom. Het is belangrijk dat het is veilig en dat alleen 'root' gebruiker toegang tot kritieke bestanden voor JSON en het script heeft.
   > Als de vorige vereisten zijn niet voldaan, wordt het script niet uitgevoerd. Dit resulteert in het bestand system/crash consistente back-up.
   >

5. Configureer **VMSnapshotScriptPluginConfig.json** zoals hier wordt beschreven:
    - **naam invoegtoepassing**: laat dit veld is of uw scripts werkt mogelijk niet zoals verwacht.

    - **preScriptLocation**: Geef het volledige pad van het script dat vooraf op de virtuele machine die u wilt back-up worden gemaakt.

    - **postScriptLocation**: Geef het volledige pad van het script dat volgt op de virtuele machine die u wilt back-up worden gemaakt.

    - **preScriptParams**: bieden de volgende optionele parameters die moeten worden doorgegeven aan de Pre-script. Alle parameters moeten tussen aanhalingstekens, en moeten door komma's gescheiden als er meerdere parameters.

    - **postScriptParams**: bieden de volgende optionele parameters die moeten worden doorgegeven aan de na-script. Alle parameters moeten tussen aanhalingstekens, en moeten door komma's gescheiden als er meerdere parameters.

    - **preScriptNoOfRetries**: het aantal keren dat het script vooraf moet opnieuw worden geprobeerd, als er een fout voordat het wordt beëindigd is. Nul betekent dat slechts één probeer en geen nieuwe poging als er een storing optreedt.

    - **postScriptNoOfRetries**: het aantal keren dat het script voor na moet opnieuw worden geprobeerd, als er een fout voordat het wordt beëindigd is. Nul betekent dat slechts één probeer en geen nieuwe poging als er een storing optreedt.
    
    - **Time-outInSeconden**: Geef afzonderlijke time-outs voor het script voor vóór en na-script.

    - **continueBackupOnFailure**: deze waarde instelt op **true** als u wilt dat Azure Backup terugvallen op een bestand system consistent/crashconsistente back-up als een script vóór of na script mislukt. Als u dit op **false** mislukt de back-up in geval van storing script (behalve wanneer u één schijf VM die op crashconsistente back-up ongeacht deze instelling terugvalt hebt).

    - **fsFreezeEnabled**: Geef op of Linux-fsfreeze moet worden aangeroepen tijdens het maken van de VM-momentopname om ervoor te zorgen consistentie van het bestandssysteem. We raden u deze instelling ingesteld op **true** tenzij uw toepassing een afhankelijkheid heeft van fsfreeze uitschakelen.

6. Het script-framework is nu geconfigureerd. Als de virtuele machine back-up al is geconfigureerd, wordt de volgende back-up roept de scripts en toepassingsconsistente back-up wordt geactiveerd. Als de back-up van de VM niet is geconfigureerd, configureert u deze met behulp van [Back-up van virtuele machines naar een Recovery Services-kluizen van Azure.](https://docs.microsoft.com/azure/backup/backup-azure-vms-first-look-arm)

## <a name="troubleshooting"></a>Problemen oplossen

Zorg ervoor dat u passende logboekregistratie tijdens het schrijven van uw script voor vóór en na toevoegen en bekijk de logboeken van uw script om op te lossen problemen script. Als u nog steeds problemen bij het uitvoeren van scripts hebt, raadpleegt u de volgende tabel voor meer informatie.

| Fout | Foutbericht | Aanbevolen actie |
| ------------------------ | -------------- | ------------------ |
| Pre-ScriptExecutionFailed |Het vooraf script heeft een fout, dus mogelijk toepassingsconsistente back-up niet.   | Bekijk de logboeken is mislukt voor uw script om het probleem te verhelpen.|  
|   Post-ScriptExecutionFailed |    Het script voor na een fout die voor de toepassingsstatus gevolgen mogelijk geretourneerd. |    Bekijk de logboeken is mislukt voor uw script om los het probleem en controleer de toepassingsstatus van de. |
| Pre-ScriptNotFound |  Het Pre-script is niet gevonden op de locatie die opgegeven in de **VMSnapshotScriptPluginConfig.json** config-bestand. |   Zorg ervoor dat dat vooraf script is aanwezig in het pad dat opgegeven in het configuratiebestand om ervoor te zorgen toepassingsconsistente back-up.|
| Post-ScriptNotFound | Het script voor na is niet gevonden op de locatie die opgegeven in de **VMSnapshotScriptPluginConfig.json** config-bestand. |   Zorg ervoor dat dat na script is aanwezig in het pad dat opgegeven in het configuratiebestand om ervoor te zorgen toepassingsconsistente back-up.|
| IncorrectPluginhostFile | De **Pluginhost** bestand, dat wordt geleverd met de extensie VmSnapshotLinux, is beschadigd, dus het script voor vóór en na kunnen niet worden uitgevoerd en de back-up niet toepassingsconsistente. | Verwijder de **VmSnapshotLinux** , en er wordt automatisch opnieuw geïnstalleerd met de volgende back-up van het probleem op te lossen. |
| IncorrectJSONConfigFile | De **VMSnapshotScriptPluginConfig.json** bestand is onjuist, dus vooraf script en na-script kan niet worden uitgevoerd en de back-up niet toepassingsconsistente. | Download het exemplaar van [GitHub](https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig) en opnieuw configureren. |
| InsufficientPermissionforPre-Script | Voor het uitvoeren van scripts 'root' gebruiker moet de eigenaar van het bestand en het bestand moet '700' machtigingen (dat wil zeggen, alleen 'eigenaar' moet 'lezen', 'schrijven' en 'uitvoeringsmachtigingen'). | Controleer of 'root' gebruiker 'eigenaar' van het scriptbestand is en dat 'eigenaar' wordt alleen 'leesmachtigingen heeft', 'schrijven' en 'uitvoeren'. |
| InsufficientPermissionforPost-Script | Voor het uitvoeren van scripts hoofdgebruiker moet de eigenaar van het bestand en het bestand moet '700' machtigingen (dat wil zeggen, alleen 'eigenaar' moet 'lezen', 'schrijven' en 'uitvoeringsmachtigingen'). | Controleer of 'root' gebruiker 'eigenaar' van het scriptbestand is en dat 'eigenaar' wordt alleen 'leesmachtigingen heeft', 'schrijven' en 'uitvoeren'. |
| Pre-ScriptTimeout | De uitvoering van de toepassingsconsistente back-up vooraf script time-out. | Controleer het script en het verhogen van de time-out in de **VMSnapshotScriptPluginConfig.json** -bestand dat zich bevindt op **/etc/azure**. |
| Post-ScriptTimeout | Er is een time-out opgetreden voor de uitvoering van de toepassingsconsistente back-na-script. | Controleer het script en het verhogen van de time-out in de **VMSnapshotScriptPluginConfig.json** -bestand dat zich bevindt op **/etc/azure**. |

## <a name="next-steps"></a>Volgende stappen
[VM-back-up naar een Recovery Services-kluis configureren](https://docs.microsoft.com/azure/backup/backup-azure-arm-vms)
