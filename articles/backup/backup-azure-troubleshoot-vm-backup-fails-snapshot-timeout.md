---
title: 'Azure Backup oplossen: Status van Gast-Agent niet beschikbaar'
description: Problemen, oorzaken en oplossingen van Azure Backup-fouten met betrekking tot agent-extensie en schijven.
services: backup
author: genlin
manager: cshepard
keywords: Azure back-up. VM-agent; Verbinding met het netwerk;
ms.service: backup
ms.topic: troubleshooting
ms.date: 12/03/2018
ms.author: genli
ms.openlocfilehash: 4d090740b75acbe2629ae4f1e13cde8947f190bb
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58286428"
---
# <a name="troubleshoot-azure-backup-failure-issues-with-the-agent-or-extension"></a>Azure Backup oplossen: Problemen met de agent of de extensie

Dit artikel bevat stappen voor probleemoplossing waarmee u kunnen Azure Backup-fouten met betrekking tot communicatie met de VM-agent en de extensie oplossen.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]



## <a name="UserErrorGuestAgentStatusUnavailable-vm-agent-unable-to-communicate-with-azure-backup"></a>UserErrorGuestAgentStatusUnavailable - VM-agent kan niet communiceren met Azure Backup

**Foutcode**: UserErrorGuestAgentStatusUnavailable <br>
**Foutbericht**: VM-Agent kan niet communiceren met Azure Backup<br>

Nadat u hebt geregistreerd en plannen van een virtuele machine voor de Backup-service, start back-up van de taak door te communiceren met de VM-agent op een punt-in-time-momentopname. Een van de volgende voorwaarden mogelijk te voorkomen dat de momentopname die wordt geactiveerd. Wanneer een momentopname is niet geactiveerd, mislukken de back-up. De volgende stappen voor probleemoplossing in de volgorde weergegeven en voer de bewerking vervolgens opnieuw uit:<br>
**1 oorzaak: [De agent op de virtuele machine is geïnstalleerd, maar het is niet meer reageert (voor Windows-VM's)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**2 oorzaak: [De agent is geïnstalleerd in de virtuele machine is verouderd (voor Linux-VM's)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**3 oorzaak: [De status van de momentopname kan niet worden opgehaald, of een momentopname kan niet worden uitgevoerd](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**    
**4 oorzaak: [De back-upextensie om te werken of te laden is mislukt](#the-backup-extension-fails-to-update-or-load)**  
**5 oorzaak: [De virtuele machine heeft geen internettoegang](#the-vm-has-no-internet-access)**

## <a name="guestagentsnapshottaskstatuserror---could-not-communicate-with-the-vm-agent-for-snapshot-status"></a>GuestAgentSnapshotTaskStatusError - kan niet communiceren met de VM-agent voor de status van de momentopname

**Foutcode**: GuestAgentSnapshotTaskStatusError<br>
**Foutbericht**: Kan niet communiceren met de VM-agent voor de status van de momentopname <br>

Nadat u hebt geregistreerd en plannen van een virtuele machine voor de Azure Backup-service, start back-up van de taak door te communiceren met de back-up VM-extensie om een point-in-time-momentopname. Een van de volgende voorwaarden mogelijk te voorkomen dat de momentopname die wordt geactiveerd. Als de momentopname niet is geactiveerd, kan een back-fout optreden. De volgende stappen voor probleemoplossing in de volgorde weergegeven en voer de bewerking vervolgens opnieuw uit:  
**1 oorzaak: [De agent op de virtuele machine is geïnstalleerd, maar het is niet meer reageert (voor Windows-VM's)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**2 oorzaak: [De agent is geïnstalleerd in de virtuele machine is verouderd (voor Linux-VM's)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**3 oorzaak: [De virtuele machine heeft geen internettoegang](#the-vm-has-no-internet-access)**

## <a name="usererrorrpcollectionlimitreached---the-restore-point-collection-max-limit-has-reached"></a>UserErrorRpCollectionLimitReached - de limiet van het herstelpunt verzameling heeft bereikt

**Foutcode**: UserErrorRpCollectionLimitReached <br>
**Foutbericht**: De limiet van het herstelpunt verzameling heeft bereikt. <br>
* Dit probleem kan optreden als er een vergrendeling op de resourcegroep van de recovery-punt te voorkomen dat automatisch opschonen van het herstelpunt.
* Dit probleem kan ook gebeuren als meerdere back-ups per dag worden geactiveerd. Op dit moment is het raadzaam van slechts één back-up per dag als de instant RPs zeven dagen worden bewaard en alleen 18 instant RPs kan worden gekoppeld aan een virtuele machine op een bepaald moment. <br>

Aanbevolen actie:<br>
U lost dit probleem, verwijder de vergrendeling van de resourcegroep van de virtuele machine en probeer het opnieuw als u wilt opschonen te activeren.
> [!NOTE]
> Backup-service maakt een afzonderlijke resourcegroep dan de resourcegroep van de virtuele machine voor het opslaan van de verzameling van herstelpunt. Klanten wordt aangeraden niet aan het vergrendelen van de resourcegroep gemaakt voor gebruik door de Backup-service. De naamgeving indeling van de resourcegroep hebt gemaakt met Backup-service is: AzureBackupRG_`<Geo>`_`<number>` Eg: AzureBackupRG_northeurope_1

**Stap 1: [Verwijder de vergrendeling van de resourcegroep van de restore-punt](#remove_lock_from_the_recovery_point_resource_group)** <br>
**Stap 2: [Herstelpuntverzameling opschonen](#clean_up_restore_point_collection)**<br>

## <a name="usererrorkeyvaultpermissionsnotconfigured---backup-doesnt-have-sufficient-permissions-to-the-key-vault-for-backup-of-encrypted-vms"></a>UserErrorKeyvaultPermissionsNotConfigured - back-up beschikt niet over voldoende machtigingen tot de key vault voor back-ups van versleutelde virtuele machines

**Foutcode**: UserErrorKeyvaultPermissionsNotConfigured <br>
**Foutbericht**: Back-up beschikt niet over voldoende machtigingen tot de key vault voor back-ups van versleutelde virtuele machines. <br>

Back-bewerking te voltooien voor versleutelde virtuele machines, moet machtigingen voor toegang tot de key vault hebben. Dit kan worden gedaan met behulp van de [Azure-portal](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption) of via [PowerShell](https://docs.microsoft.com/azure/backup/backup-azure-vms-automation#enable-protection).

## <a name="ExtensionSnapshotFailedNoNetwork-snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine"></a>ExtensionSnapshotFailedNoNetwork - momentopname is mislukt omdat er geen netwerkverbinding beschikbaar is op de virtuele machine

**Foutcode**: ExtensionSnapshotFailedNoNetwork<br>
**Foutbericht**: Momentopname mislukt omdat er geen netwerkverbinding beschikbaar is op de virtuele machine<br>

Nadat u hebt geregistreerd en plannen van een virtuele machine voor de Azure Backup-service, start back-up van de taak door te communiceren met de back-up VM-extensie om een point-in-time-momentopname. Een van de volgende voorwaarden mogelijk te voorkomen dat de momentopname die wordt geactiveerd. Als de momentopname niet is geactiveerd, kan een back-fout optreden. De volgende stappen voor probleemoplossing in de volgorde weergegeven en voer de bewerking vervolgens opnieuw uit:    
**1 oorzaak: [De status van de momentopname kan niet worden opgehaald, of een momentopname kan niet worden uitgevoerd](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**2 oorzaak: [De back-upextensie om te werken of te laden is mislukt](#the-backup-extension-fails-to-update-or-load)**  
**3 oorzaak: [De virtuele machine heeft geen internettoegang](#the-vm-has-no-internet-access)**

## <a name="ExtentionOperationFailed-vmsnapshot-extension-operation-failed"></a>ExtentionOperationFailedForManagedDisks - bewerking van VMSnapshot-extensie is mislukt

**Foutcode**: ExtentionOperationFailedForManagedDisks <br>
**Foutbericht**: Bewerking van VMSnapshot-extensie is mislukt<br>

Nadat u hebt geregistreerd en plannen van een virtuele machine voor de Azure Backup-service, start back-up van de taak door te communiceren met de back-up VM-extensie om een point-in-time-momentopname. Een van de volgende voorwaarden mogelijk te voorkomen dat de momentopname die wordt geactiveerd. Als de momentopname niet is geactiveerd, kan een back-fout optreden. De volgende stappen voor probleemoplossing in de volgorde weergegeven en voer de bewerking vervolgens opnieuw uit:  
**1 oorzaak: [De status van de momentopname kan niet worden opgehaald, of een momentopname kan niet worden uitgevoerd](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**2 oorzaak: [De back-upextensie om te werken of te laden is mislukt](#the-backup-extension-fails-to-update-or-load)**  
**3 oorzaak: [De agent op de virtuele machine is geïnstalleerd, maar het is niet meer reageert (voor Windows-VM's)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**4 oorzaak: [De agent is geïnstalleerd in de virtuele machine is verouderd (voor Linux-VM's)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**

## <a name="backupoperationfailed--backupoperationfailedv2---backup-fails-with-an-internal-error"></a>BackUpOperationFailed / BackUpOperationFailedV2 - back-up mislukt met een interne fout

**Foutcode**: BackUpOperationFailed / BackUpOperationFailedV2 <br>
**Foutbericht**: Back-up is mislukt met een interne fout: Voer de bewerking over een paar minuten <br>

Nadat u hebt geregistreerd en plannen van een virtuele machine voor de Azure Backup-service, start back-up van de taak door te communiceren met de back-up VM-extensie om een point-in-time-momentopname. Een van de volgende voorwaarden mogelijk te voorkomen dat de momentopname die wordt geactiveerd. Als de momentopname niet is geactiveerd, kan een back-fout optreden. De volgende stappen voor probleemoplossing in de volgorde weergegeven en voer de bewerking vervolgens opnieuw uit:  
**1 oorzaak: [De agent is geïnstalleerd in de virtuele machine, maar niet reageert (voor Windows-VM's)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**2 oorzaak: [De agent is geïnstalleerd in de virtuele machine is verouderd (voor Linux-VM's)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**3 oorzaak: [De status van de momentopname kan niet worden opgehaald, of een momentopname kan niet worden uitgevoerd](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**4 oorzaak: [De back-upextensie om te werken of te laden is mislukt](#the-backup-extension-fails-to-update-or-load)**  
**5 oorzaak: Backup-service heeft geen toegangsmachtiging voor de oude herstelpunten verwijderd vanwege een vergrendeling van de groep resource** <br>
**Oorzaak 6: [De virtuele machine heeft geen internettoegang](#the-vm-has-no-internet-access)**

## <a name="usererrorunsupporteddisksize---currently-azure-backup-does-not-support-disk-sizes-greater-than-4095gb"></a>UserErrorUnsupportedDiskSize - momenteel Azure Backup biedt geen ondersteuning voor schijven groter dan 4095GB

**Foutcode**: UserErrorUnsupportedDiskSize <br>
**Foutbericht**: Azure Backup biedt momenteel geen ondersteuning voor schijven groter dan 4095GB <br>

Uw back-upbewerking kan mislukken wanneer back-ups van virtuele machine met de grootte van de schijf is groter dan 4095GB. Ondersteuning voor grote schijven is binnenkort beschikbaar.  

## <a name="usererrorbackupoperationinprogress---unable-to-initiate-backup-as-another-backup-operation-is-currently-in-progress"></a>UserErrorBackupOperationInProgress - kan geen back-up initiëren omdat er momenteel een andere back-upbewerking wordt uitgevoerd

**Foutcode**: UserErrorBackupOperationInProgress <br>
**Foutbericht**: Kan geen back-up initiëren omdat er momenteel een andere back-upbewerking wordt uitgevoerd<br>

Uw recente back-uptaak is mislukt omdat er een bestaande back-uptaak uitgevoerd wordt. U kunt een nieuwe back-uptaak niet starten, totdat de huidige taak is voltooid. Zorg ervoor dat de back-upbewerking die momenteel bezig is voltooid voordat u activeert of een andere back-upbewerkingen plannen. Om te controleren of de status van de back-uptaken, voer de onderstaande stappen te volgen:

1. Meld u aan bij de Azure-portal en klik op **Alle services**. Typ Recovery Services en klik op **Recovery Services-kluizen**. De lijst met Recovery Services-kluizen wordt weergegeven.
2. Selecteer in de lijst met recovery services-kluizen een kluis waarin de back-up is geconfigureerd.
3. Klik op het menu kluis dashboard **back-uptaken** de back-uptaken wordt weergegeven.

    * Als een back-uptaak uitgevoerd wordt, wacht het voltooid of annuleer de back-uptaak.
        * De back-uptaak Klik met de rechtermuisknop op de back-uptaak annuleren en klikt u op **annuleren** of gebruik [PowerShell](https://docs.microsoft.com/en-us/powershell/module/az.recoveryservices/stop-azrecoveryservicesbackupjob?view=azps-1.4.0).
    * Als u de back-up in een andere kluis hebt geconfigureerd, klikt u vervolgens controleert u of er zijn geen back-taken die worden uitgevoerd in de oude kluis. Als deze bestaat, annuleert u de back-uptaak.
        * De back-uptaak Klik met de rechtermuisknop op de back-uptaak annuleren en klikt u op **annuleren** of gebruik [PowerShell](https://docs.microsoft.com/en-us/powershell/module/az.recoveryservices/stop-azrecoveryservicesbackupjob?view=azps-1.4.0)
4. Voer back-up opnieuw uit.

Als de geplande back-upbewerking langer conflicteert met de volgende back-upconfiguratie duurt Bekijk vervolgens de [Best Practices](backup-azure-vms-introduction.md#best-practices), [back-upprestaties](backup-azure-vms-introduction.md#backup-performance) en [overweging herstellen ](backup-azure-vms-introduction.md#backup-and-restore-considerations).


## <a name="causes-and-solutions"></a>Oorzaken en oplossingen

### <a name="the-vm-has-no-internet-access"></a>De virtuele machine heeft geen internettoegang
De virtuele machine geen toegang tot internet, per de implementatievereiste. Of dat er beperkingen die voorkomen toegang tot de Azure-infrastructuur dat.

Om correct te functioneren, vereist de Backup-extensie een verbinding met Azure openbare IP-adressen. De extensie verzonden opdrachten naar een Azure storage-eindpunt (HTTPs-URL) voor het beheren van de momentopnamen van de virtuele machine. Als de extensie heeft geen toegang tot het openbare internet, wordt uiteindelijk de back-up mislukt.

####  <a name="solution"></a>Oplossing
Raadpleeg voor het oplossen van het netwerkprobleem [netwerkverbinding tot stand brengen](backup-azure-arm-vms-prepare.md#establish-network-connectivity).

### <a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>De agent op de virtuele machine is geïnstalleerd, maar het is niet meer reageert (voor Windows-VM's)

#### <a name="solution"></a>Oplossing
De VM-agent is beschadigd of de service is gestopt. De VM-agent opnieuw te installeren, kunt de meest recente versie. Het helpt ook bij communicatie met de service opnieuw.

1. Bepalen of de Windows Azure Guest Agent-service wordt uitgevoerd in de VM-services (services.msc). Probeer het opnieuw opstarten van de Windows Azure Guest Agent-service en start de back-up.    
2. Als de Windows Azure Guest Agent-service niet worden weergegeven in de services in het Configuratiescherm, gaat u naar **programma's en onderdelen** om te bepalen of de Windows Azure Guest Agent-service is geïnstalleerd.
4. Als de Windows Azure Guest-Agent wordt weergegeven in **programma's en onderdelen**, de Windows Azure Guest-Agent verwijderen.
5. Download en installeer de [meest recente versie van de agent-MSI](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). U moet beheerdersrechten om de installatie te voltooien.
6. Controleer of dat de Windows Azure Guest Agent-services worden weergegeven in de services.
7. Een on-demand back-up uitvoeren:
    * Selecteer in de portal **nu back-up**.

Controleer ook of die [Microsoft .NET 4.5 is geïnstalleerd](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) in de virtuele machine. .NET 4.5 is vereist voor de VM-agent kan communiceren met de service.

### <a name="the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>De agent is geïnstalleerd in de virtuele machine is verouderd (voor Linux-VM's)

#### <a name="solution"></a>Oplossing
Meest-agents of -extensie-gerelateerde fouten voor Linux-VM's worden veroorzaakt door problemen die invloed hebben op een verouderde VM-agent. Om dit probleem wilt oplossen, gaat u als volgt deze algemene richtlijnen:

1. Volg de instructies voor [bijwerken van de Linux-VM-agent](../virtual-machines/linux/update-agent.md).

   > [!NOTE]
   > We *wordt ten zeerste aangeraden* de agent alleen via een distributie-opslagplaats bij te werken. We raden niet downloaden van de agentcode rechtstreeks vanuit GitHub en bijwerkt. Als de meest recente agent voor uw distributie geen distributiepunt beschikbaar is, contact op met ondersteuning voor instructies over hoe u deze installeert is. Om te controleren of de meest recente agent, gaat u naar de [Windows Azure Linux agent](https://github.com/Azure/WALinuxAgent/releases) pagina in de GitHub-opslagplaats.

2. Zorg ervoor dat de Azure-agent wordt uitgevoerd op de virtuele machine met de volgende opdracht: `ps -e`

   Als het proces niet wordt uitgevoerd, start u deze opnieuw met behulp van de volgende opdrachten:

   * Voor Ubuntu: `service walinuxagent start`
   * Voor andere distributies: `service waagent start`

3. [Configureren van de agent van automatisch opnieuw opstarten](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Een nieuwe test back-up uitvoeren. Als de fout zich blijft voordoen, moet u de volgende logboeken verzamelen van de virtuele machine:

   * /var/lib/waagent/*.XML
   * /var/log/waagent.log
   * /var/log/azure/*

Als we uitgebreide logboekregistratie voor waagent nodig hebt, volgt u deze stappen:

1. Zoek de volgende regel in het bestand /etc/waagent.conf: **Uitgebreide logboekregistratie inschakelen (y | n)**
2. Wijzig de **Logs.Verbose** waarde uit de *n* naar *y*.
3. Sla de wijziging en opnieuw opstarten waagent via de stappen die eerder in deze sectie zijn beschreven.

###  <a name="the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>De status van de momentopname kan niet worden opgehaald, of een momentopname kan niet worden uitgevoerd
De virtuele machine back-up, is afhankelijk van de opdracht van een momentopname naar de onderliggende storage-account. Back-up kan mislukken omdat er geen toegang tot het opslagaccount of omdat het uitvoeren van de momentopname-taak wordt vertraagd.

#### <a name="solution"></a>Oplossing
De volgende voorwaarden kunnen leiden tot de taak momentopname is mislukt:

| Oorzaak | Oplossing |
| --- | --- |
| De status van de virtuele machine niet correct gerapporteerd omdat de virtuele machine wordt afgesloten in Remote Desktop Protocol (RDP). | Als u de virtuele machine in RDP afsluit, controleert u de portal om te bepalen of de status van de virtuele machine juist is. Als dit niet juist is, sluit u de virtuele machine in de portal met behulp van de **afsluiten** optie op het dashboard voor VM's. |
| De virtuele machine kan de host of fabric-adres ophalen van DHCP. | DHCP moet zijn ingeschakeld in de Gast voor de IaaS-VM back-up om te werken. Als de virtuele machine de host of fabric-adres niet uit de DHCP-reacties 245 ophalen kan, kan het downloaden of geen extensies worden uitgevoerd. Als u een statisch privé IP-adres nodig hebt, moet u het configureren via de **Azure-portal** of **PowerShell** en zorg ervoor dat de DHCP-optie binnen de virtuele machine is ingeschakeld. [Meer informatie](../virtual-network/virtual-networks-static-private-ip-arm-ps.md#change-the-allocation-method-for-a-private-ip-address-assigned-to-a-network-interface) over het instellen van een statisch IP-adres met PowerShell.

### <a name="the-backup-extension-fails-to-update-or-load"></a>De back-upextensie om te werken of te laden is mislukt
Als extensies kunnen niet worden geladen, wordt back-up mislukt, omdat een momentopname kan niet worden gemaakt.

#### <a name="solution"></a>Oplossing

Verwijder de extensie om af te dwingen de VMSnapshot-extensie om opnieuw te laden. De volgende back-poging wordt opnieuw geladen met de extensie.

De extensie verwijderen:

1. In de [Azure-portal](https://portal.azure.com/), gaat u naar de virtuele machine waarop back-upfouten zich voordoet.
2. Selecteer **instellingen**.
3. Selecteer **Extensies**.
4. Selecteer **Vmsnapshot-extensie**.
5. Selecteer **verwijderen**.

Voor Linux-VM, als de VMSnapshot-extensie niet wordt weergegeven in de Azure-portal [bijwerken van de Azure Linux Agent](../virtual-machines/linux/update-agent.md), en voer de back-up.

Uitvoeren van deze stappen zorgt ervoor dat de uitbreiding moet worden geïnstalleerd tijdens de volgende back-up.

### <a name="remove_lock_from_the_recovery_point_resource_group"></a>Verwijder de vergrendeling van de resourcegroep van recovery point
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Ga naar **optie alle Resources**, selecteer de resourcegroep waarin u restore point verzameling in de volgende indeling AzureBackupRG_`<Geo>`_`<number>`.
3. In de **instellingen** sectie, selecteer **vergrendelingen** om de vergrendelingen weer te geven.
4. Als u wilt de vergrendeling verwijderen, selecteer het beletselteken en klik op **verwijderen**.

    ![Vergrendeling voor verwijderen](./media/backup-azure-arm-vms-prepare/delete-lock.png)

### <a name="clean_up_restore_point_collection"></a> Herstelpuntverzameling opschonen
Nadat de vergrendeling is verwijderd, moeten de herstelpunten worden opgeschoond. Als u wilt opschonen van de herstelpunten, gaat u als volgt een van de methoden:<br>
* [Wijs opschonen terugzetten verzameling door het uitvoeren van ad-hoc back-up](#clean-up-restore-point-collection-by-running-ad-hoc-backup)<br>
* [Opschonen van terugzetten wijst u het verzamelen van Azure portal](#clean-up-restore-point-collection-from-azure-portal)<br>

#### <a name="clean-up-restore-point-collection-by-running-ad-hoc-backup"></a>Wijs opschonen terugzetten verzameling door het uitvoeren van ad-hoc back-up
Na het verwijderen van vergrendeling-trigger voor een ad-hoc/handmatige back-up. Dit zorgt ervoor dat de herstelpunten worden automatisch opgeschoond. Met deze bewerking ad hoc/handmatige mislukken van de eerste keer; verwacht het wordt echter voor zorgen automatisch op te schonen in plaats van handmatig verwijderen van herstelpunten. De volgende geplande back-up moet na opschonen slaagt.

> [!NOTE]
> Automatisch op te schonen gebeurt na een paar uur van de ad-hoc/handmatige back-up activeren. Als uw geplande back-up nog steeds mislukt, wordt geprobeerd handmatig verwijderen van de verzameling van herstelpunt met behulp van de stappen die worden vermeld [hier](#clean-up-restore-point-collection-from-azure-portal).

#### <a name="clean-up-restore-point-collection-from-azure-portal"></a>Opschonen van terugzetten wijst u het verzamelen van Azure portal <br>

Schakelt u de terugzetbewerking handmatig verwijst verzameling die niet zijn uitgeschakeld vanwege de vergrendeling van de resourcegroep, probeert u de volgende stappen uit:
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Op de **Hub** menu, klikt u op **alle resources**, selecteer de resourcegroep met de volgende indeling AzureBackupRG_`<Geo>`_`<number>` waar uw virtuele machine zich bevindt.

    ![Vergrendeling voor verwijderen](./media/backup-azure-arm-vms-prepare/resource-group.png)

3. Klik op de resourcegroep, de **overzicht** blade wordt weergegeven.
4. Selecteer **verborgen typen weergeven** optie om de verborgen resources weer te geven. Selecteer de verzamelingen terugzetten punt met de volgende indeling AzureBackupRG_`<VMName>`_`<number>`.

    ![Vergrendeling voor verwijderen](./media/backup-azure-arm-vms-prepare/restore-point-collection.png)

5. Klik op **verwijderen**, voor het opschonen van de verzameling van herstelpunt.
6. Voer opnieuw de back-upbewerking opnieuw uit.

> [!NOTE]
 >Als de resource (RP-verzameling) grote heeft wordt het aantal herstelpunten, dezelfde vervolgens te verwijderen uit de portal time-out kan en mislukken. Dit is een bekend probleem CRP waar alle herstelpunten zijn niet verwijderd in de aangegeven tijd en de bewerking een time-out optreedt; de delete-bewerking slaagt echter meestal na 2 of 3 nieuwe pogingen.
