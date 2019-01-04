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
ms.openlocfilehash: a0f002266764ace07482023a0412366b90acec63
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53789854"
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
    > Backup-service maakt een afzonderlijke resourcegroep dan de resourcegroep van de virtuele machine voor het opslaan van de verzameling van herstelpunt. Klanten wordt aangeraden niet aan het vergrendelen van de resourcegroep gemaakt voor gebruik door de Backup-service. De naamgeving indeling van de resourcegroep hebt gemaakt met Backup-service is: AzureBackupRG_`<Geo>`_`<number>` bijvoorbeeld: AzureBackupRG_northeurope_1

**Stap 1: [Verwijder de vergrendeling van de resourcegroep van de restore-punt](#remove_lock_from_the_recovery_point_resource_group)** <br>
**Stap 2: [Herstelpuntverzameling opschonen](#clean_up_restore_point_collection)**<br>

## <a name="usererrorkeyvaultpermissionsnotconfigured---backup-doesnt-have-sufficient-permissions-to-the-key-vault-for-backup-of-encrypted-vms"></a>UserErrorKeyvaultPermissionsNotConfigured - back-up beschikt niet over voldoende machtigingen tot de key vault voor back-ups van versleutelde virtuele machines.

**Foutcode**: UserErrorKeyvaultPermissionsNotConfigured <br>
**Foutbericht**: Back-up beschikt niet over voldoende machtigingen tot de key vault voor back-ups van versleutelde virtuele machines. <br>

Back-bewerking te voltooien voor versleutelde virtuele machines, moet machtigingen voor toegang tot de key vault hebben. Dit kan worden gedaan met behulp van de [Azure-portal](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption#provide-permissions-to-backup) of via de [PowerShell](https://docs.microsoft.com/azure/backup/backup-azure-vms-automation#enable-protection)

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
**5 oorzaak: [Backup-service heeft geen toegangsmachtiging voor de oude herstelpunten verwijderd vanwege een vergrendeling van de groep resource](#backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock)** <br>
**Oorzaak 6: [De virtuele machine heeft geen internettoegang](#the-vm-has-no-internet-access)**

## <a name="usererrorunsupporteddisksize---currently-azure-backup-does-not-support-disk-sizes-greater-than-1023gb"></a>UserErrorUnsupportedDiskSize - momenteel Azure Backup biedt geen ondersteuning voor schijven groter dan 1023GB

**Foutcode**: UserErrorUnsupportedDiskSize <br>
**Foutbericht**: Momenteel biedt Azure Backup geen ondersteuning voor schijven groter dan 1023 GB <br>

Uw back-upbewerking kan mislukken wanneer back-ups van virtuele machine met de grootte van de schijf is groter dan 1023GB omdat uw vault niet naar de Azure VM-back-upstack V2 bijgewerkt is. Upgraden naar Azure VM Backup stack V2 biedt ondersteuning voor maximaal 4TB. Bekijk deze [voordelen](backup-upgrade-to-vm-backup-stack-v2.md), [overwegingen met betrekking tot](backup-upgrade-to-vm-backup-stack-v2.md#considerations-before-upgrade), en vervolgens gaat u verder met de upgrade Volg hiervoor de volgende [instructies](backup-upgrade-to-vm-backup-stack-v2.md#upgrade).  

## <a name="usererrorstandardssdnotsupported---currently-azure-backup-does-not-support-standard-ssd-disks"></a>UserErrorStandardSSDNotSupported - momenteel Azure Backup biedt geen ondersteuning voor Standard-SSD-schijven

**Foutcode**: UserErrorStandardSSDNotSupported <br>
**Foutbericht**: Azure Backup biedt momenteel geen ondersteuning voor Standard-SSD-schijven <br>

Azure Backup ondersteunt momenteel de Standard-SSD-schijven alleen voor die zijn bijgewerkt naar de Azure VM Backup-stack V2-kluizen. Bekijk deze [voordelen](backup-upgrade-to-vm-backup-stack-v2.md), [overwegingen met betrekking tot](backup-upgrade-to-vm-backup-stack-v2.md#considerations-before-upgrade), en vervolgens gaat u verder met de upgrade Volg hiervoor de volgende [instructies](backup-upgrade-to-vm-backup-stack-v2.md#upgrade).


## <a name="causes-and-solutions"></a>Oorzaken en oplossingen

### <a name="the-vm-has-no-internet-access"></a>De virtuele machine heeft geen internettoegang
De virtuele machine geen toegang tot internet, per de implementatievereiste. Of dat er beperkingen die voorkomen toegang tot de Azure-infrastructuur dat.

Om correct te functioneren, vereist de Backup-extensie een verbinding met Azure openbare IP-adressen. De extensie verzonden opdrachten naar een Azure storage-eindpunt (HTTPs-URL) voor het beheren van de momentopnamen van de virtuele machine. Als de extensie heeft geen toegang tot het openbare internet, wordt uiteindelijk de back-up mislukt.

Het is mogelijk om een proxyserver om de VM-verkeer te routeren te implementeren.
##### <a name="create-a-path-for-https-traffic"></a>Maak een pad voor HTTPs-verkeer

1. Hebt u netwerkbeperkingen op locatie (bijvoorbeeld een netwerkbeveiligingsgroep), implementeert u een HTTPs-proxy-server om het verkeer te routeren.
2. Voor toegang tot het internet van de HTTPs-proxy-server, regels toevoegen aan de netwerkbeveiligingsgroep, indien aanwezig.

Zie voor meer informatie over het instellen van een HTTPs-proxy voor back-ups van virtuele machine, [uw omgeving voorbereiden op back-up van virtuele machines van Azure](backup-azure-arm-vms-prepare.md#establish-network-connectivity).

De back-ups van virtuele machine of de proxy-server waarmee het verkeer wordt doorgestuurd vereist toegang tot Azure openbare IP-adressen

####  <a name="solution"></a>Oplossing
Los het probleem, probeert u een van de volgende methoden:

##### <a name="allow-access-to-azure-storage-that-corresponds-to-the-region"></a>Toegang tot Azure storage die overeenkomt met de regio

U kunt [servicetags](../virtual-network/security-overview.md#service-tags) om verbindingen naar de opslag van de specifieke regio te staan. Zorg ervoor dat de regel waarmee toegang tot het opslagaccount hogere prioriteit dan de regel die blokken toegang tot internet heeft.

![Netwerkbeveiligingsgroep met opslag-tags voor een regio](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

Bekijk voor meer informatie over de stapsgewijze procedure servicetags configureren, [in deze video](https://youtu.be/1EjLQtbKm1M).

> [!WARNING]
> Opslag servicetags zijn beschikbaar als preview. Ze zijn alleen beschikbaar in bepaalde regio's. Zie voor een lijst met regio's, [servicetags voor opslag](../virtual-network/security-overview.md#service-tags).

Als u Azure Managed Disks gebruikt, moet u mogelijk een extra poort openen (poort 8443) op de firewalls.

Als uw subnet niet beschikt over een route voor uitgaand verkeer van internet, moet u bovendien een service-eindpunt met servicetag "Microsoft.Storage" toevoegen aan uw subnet.

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
   * / var/aanmelden/azure / *

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
| De virtuele machine kan de host of fabric-adres ophalen van DHCP. | DHCP moet zijn ingeschakeld in de Gast voor de IaaS-VM back-up om te werken. Als de virtuele machine de host of fabric-adres niet uit de DHCP-reacties 245 ophalen kan, kan het downloaden of geen extensies worden uitgevoerd. Als u een statisch privé IP-adres nodig hebt, moet u het configureren via de **Azure Portal** of **PowerShell** en zorg ervoor dat de DHCP-optie binnen de virtuele machine is ingeschakeld. Zie voor meer informatie over het instellen van een statisch IP-adres via de PowerShell [klassieke VM](../virtual-network/virtual-networks-reserved-private-ip.md#how-to-add-a-static-internal-ip-to-an-existing-vm) en [Resource Manager-VM](../virtual-network/virtual-networks-static-private-ip-arm-ps.md#change-the-allocation-method-for-a-private-ip-address-assigned-to-a-network-interface).

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
1. Meld u aan bij [Azure Portal](http://portal.azure.com/).
2. Ga naar **optie alle Resources**, selecteer de resourcegroep waarin u restore point verzameling in de volgende indeling AzureBackupRG_`<Geo>`_`<number>`.
3. In de **instellingen** sectie, selecteer **vergrendelingen** om de vergrendelingen weer te geven.
4. Als u wilt de vergrendeling verwijderen, selecteer het beletselteken en klik op **verwijderen**.

    ![Vergrendeling voor verwijderen ](./media/backup-azure-arm-vms-prepare/delete-lock.png)

### <a name="clean_up_restore_point_collection"></a> Herstelpuntverzameling opschonen
Nadat de vergrendeling is verwijderd, moeten de herstelpunten worden opgeschoond. Als u wilt opschonen van de herstelpunten, gaat u als volgt een van de methoden:<br>
* [Opschonen van de verzameling van herstelpunt door actieve ad-hoc back-up](#clean-up-restore-point-collection-by-running-ad-hoc-backup)<br>
* [Opschonen van terugzetten wijst u het verzamelen van Azure portal](#clean-up-restore-point-collection-from-azure-portal)<br>

#### <a name="clean-up-restore-point-collection-by-running-ad-hoc-backup"></a>Opschonen van de verzameling van herstelpunt door actieve ad-hoc back-up
Na het verwijderen van vergrendeling-trigger voor een ad-hoc/handmatige back-up. Dit zorgt ervoor dat de herstelpunten worden automatisch opgeschoond. Met deze bewerking ad-hoc/handmatige mislukken van de eerste keer; verwacht het wordt echter voor zorgen automatisch op te schonen in plaats van handmatig verwijderen van herstelpunten. De volgende geplande back-up moet na opschonen slaagt.

> [!NOTE]
    > Automatisch op te schonen gebeurt na een paar uur van de ad-hoc/handmatige back-up activeren. Als uw geplande back-up nog steeds mislukt, wordt geprobeerd handmatig verwijderen van de verzameling van herstelpunt met behulp van de stappen die worden vermeld [hier](#clean-up-restore-point-collection-from-azure-portal).

#### <a name="clean-up-restore-point-collection-from-azure-portal"></a>Opschonen van terugzetten wijst u het verzamelen van Azure portal <br>

Schakelt u de terugzetbewerking handmatig verwijst verzameling die niet zijn uitgeschakeld vanwege de vergrendeling van de resourcegroep, probeert u de volgende stappen uit:
1. Meld u aan bij [Azure Portal](http://portal.azure.com/).
2. Op de **Hub** menu, klikt u op **alle resources**, selecteer de resourcegroep met de volgende indeling AzureBackupRG_`<Geo>`_`<number>` waar uw virtuele machine zich bevindt.

    ![Vergrendeling voor verwijderen ](./media/backup-azure-arm-vms-prepare/resource-group.png)

3. Klik op de resourcegroep, de **overzicht** blade wordt weergegeven.
4. Selecteer **verborgen typen weergeven** optie om de verborgen resources weer te geven. Selecteer de verzamelingen terugzetten punt met de volgende indeling AzureBackupRG_`<VMName>`_`<number>`.

    ![Vergrendeling voor verwijderen ](./media/backup-azure-arm-vms-prepare/restore-point-collection.png)

5. Klik op **verwijderen**, voor het opschonen van de verzameling van herstelpunt.
6. Voer opnieuw de back-upbewerking opnieuw uit.
