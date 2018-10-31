---
title: 'Azure Backup oplossen: Gast-Agent de Status niet beschikbaar'
description: Problemen, oorzaken en oplossingen van Azure Backup-fouten met betrekking tot agent-extensie en schijven.
services: backup
author: genlin
manager: cshepard
keywords: Azure back-up. VM-agent; Verbinding met het netwerk;
ms.service: backup
ms.topic: troubleshooting
ms.date: 10/30/2018
ms.author: genli
ms.openlocfilehash: 5c37e2e3cabb81ed123146f283c7d568cc58816d
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50242619"
---
# <a name="troubleshoot-azure-backup-failure-issues-with-the-agent-or-extension"></a>Azure Backup oplossen: problemen met de agent of de extensie

Dit artikel bevat stappen voor probleemoplossing waarmee u kunnen Azure Backup-fouten met betrekking tot communicatie met de VM-agent en de extensie oplossen.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="vm-agent-unable-to-communicate-with-azure-backup"></a>VM-agent kan niet communiceren met Azure Backup

Foutbericht weergegeven: "VM-Agent kan niet communiceren met Azure Backup"<br>
Foutcode: "UserErrorGuestAgentStatusUnavailable"

Nadat u hebt geregistreerd en plannen van een virtuele machine voor de Backup-service, start back-up van de taak door te communiceren met de VM-agent op een punt-in-time-momentopname. Een van de volgende voorwaarden mogelijk te voorkomen dat de momentopname die wordt geactiveerd. Wanneer een momentopname is niet geactiveerd, mislukken de back-up. De volgende stappen voor probleemoplossing in de volgorde weergegeven en voer de bewerking vervolgens opnieuw uit:

**1 oorzaak: [de virtuele machine heeft geen internettoegang](#the-vm-has-no-internet-access)**  
**2 oorzaak: [de agent is geïnstalleerd in de virtuele machine, maar het niet-reagerende (voor Windows-VM's)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**3 oorzaak: [de agent is geïnstalleerd in de virtuele machine is verouderd (voor Linux-VM's)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**4 oorzaak: [de status van de momentopname kan niet worden opgehaald, of een momentopname kan niet worden uitgevoerd](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**    
**5 oorzaak: [de Backup-extensie om te werken of te laden is mislukt](#the-backup-extension-fails-to-update-or-load)**  

## <a name="snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine"></a>De momentopnamebewerking is mislukt omdat de virtuele machine is niet met het netwerk verbonden

Foutbericht weergegeven: 'Een momentopname van de bewerking is mislukt omdat er geen netwerkverbinding beschikbaar is op de virtuele machine'<br>
Foutcode: "ExtensionSnapshotFailedNoNetwork"

Nadat u hebt geregistreerd en plannen van een virtuele machine voor de Azure Backup-service, start back-up van de taak door te communiceren met de back-up VM-extensie om een point-in-time-momentopname. Een van de volgende voorwaarden mogelijk te voorkomen dat de momentopname die wordt geactiveerd. Als de momentopname niet is geactiveerd, kan een back-fout optreden. De volgende stappen voor probleemoplossing in de volgorde weergegeven en voer de bewerking vervolgens opnieuw uit:    
**1 oorzaak: [de virtuele machine heeft geen internettoegang](#the-vm-has-no-internet-access)**  
**2 oorzaak: [de status van de momentopname kan niet worden opgehaald, of een momentopname kan niet worden uitgevoerd](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**3 oorzaak: [de Backup-extensie om te werken of te laden is mislukt](#the-backup-extension-fails-to-update-or-load)**  

## <a name="vmsnapshot-extension-operation-failed"></a>Bewerking van VMSnapshot-extensie mislukt

Foutbericht weergegeven: "bewerking van VMSnapshot-extensie mislukt"<br>
Foutcode: "ExtentionOperationFailed"

Nadat u hebt geregistreerd en plannen van een virtuele machine voor de Azure Backup-service, start back-up van de taak door te communiceren met de back-up VM-extensie om een point-in-time-momentopname. Een van de volgende voorwaarden mogelijk te voorkomen dat de momentopname die wordt geactiveerd. Als de momentopname niet is geactiveerd, kan een back-fout optreden. De volgende stappen voor probleemoplossing in de volgorde weergegeven en voer de bewerking vervolgens opnieuw uit:  
**1 oorzaak: [de status van de momentopname kan niet worden opgehaald, of een momentopname kan niet worden uitgevoerd](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**2 oorzaak: [de Backup-extensie om te werken of te laden is mislukt](#the-backup-extension-fails-to-update-or-load)**  
**3 oorzaak: [de agent is geïnstalleerd in de virtuele machine, maar het niet-reagerende (voor Windows-VM's)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**4 oorzaak: [de agent is geïnstalleerd in de virtuele machine is verouderd (voor Linux-VM's)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**

## <a name="backup-fails-because-the-vm-agent-is-unresponsive"></a>Back-up is mislukt omdat de VM-agent niet reageert

Foutmelding: 'Kan niet communiceren met de VM-agent voor de status van de momentopname' <br>
Foutcode: "GuestAgentSnapshotTaskStatusError"

Nadat u hebt geregistreerd en plannen van een virtuele machine voor de Azure Backup-service, start back-up van de taak door te communiceren met de back-up VM-extensie om een point-in-time-momentopname. Een van de volgende voorwaarden mogelijk te voorkomen dat de momentopname die wordt geactiveerd. Als de momentopname niet is geactiveerd, kan een back-fout optreden. De volgende stappen voor probleemoplossing in de volgorde weergegeven en voer de bewerking vervolgens opnieuw uit:  
**1 oorzaak: [de agent is geïnstalleerd in de virtuele machine, maar het niet-reagerende (voor Windows-VM's)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**2 oorzaak: [de agent is geïnstalleerd in de virtuele machine is verouderd (voor Linux-VM's)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**3 oorzaak: [de virtuele machine heeft geen internettoegang](#the-vm-has-no-internet-access)**  

## <a name="backup-fails-with-an-internal-error"></a>Back-up is mislukt met een interne fout

Foutmelding: "back-up is mislukt met een interne fout: Voer de bewerking over een paar minuten" <br>
Foutcode: 'BackUpOperationFailed' / "BackUpOperationFailedV2"

Nadat u hebt geregistreerd en plannen van een virtuele machine voor de Azure Backup-service, start back-up van de taak door te communiceren met de back-up VM-extensie om een point-in-time-momentopname. Een van de volgende voorwaarden mogelijk te voorkomen dat de momentopname die wordt geactiveerd. Als de momentopname niet is geactiveerd, kan een back-fout optreden. De volgende stappen voor probleemoplossing in de volgorde weergegeven en voer de bewerking vervolgens opnieuw uit:  
**1 oorzaak: [de virtuele machine heeft geen internettoegang](#the-vm-has-no-internet-access)**  
**2 oorzaak: [de agent is geïnstalleerd in de virtuele machine, maar niet reageert (voor Windows-VM's)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**3 oorzaak: [de agent is geïnstalleerd in de virtuele machine is verouderd (voor Linux-VM's)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**4 oorzaak: [de status van de momentopname kan niet worden opgehaald, of een momentopname kan niet worden uitgevoerd](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**5 oorzaak: [de Backup-extensie om te werken of te laden is mislukt](#the-backup-extension-fails-to-update-or-load)**  
**6 oorzaak: [Backup-service heeft geen toegangsmachtiging voor de oude herstelpunten verwijderd vanwege een vergrendeling van de groep resource](#backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock)**

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

1. Bepalen of de Windows Guest Agent-service wordt uitgevoerd in de VM-services (services.msc). Probeer het opnieuw opstarten van de Windows Guest Agent-service en start de back-up.    
2. Als de Windows Guest Agent-service niet worden weergegeven in de services in het Configuratiescherm, gaat u naar **programma's en onderdelen** om te bepalen of de Windows Guest Agent-service is geïnstalleerd.
4. Als de Windows Guest-Agent wordt weergegeven in **programma's en onderdelen**, verwijdert u de Windows Guest Agent.
5. Download en installeer de [meest recente versie van de agent-MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). U moet beheerdersrechten om de installatie te voltooien.
6. Controleer of dat de Windows Guest Agent-services worden weergegeven in de services.
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

1. Zoek de volgende regel in het bestand /etc/waagent.conf: **uitgebreide logboekregistratie inschakelen (y | n)**
2. Wijzig de **Logs.Verbose** waarde uit de *n* naar *y*.
3. Sla de wijziging en opnieuw opstarten waagent via de stappen die eerder in deze sectie zijn beschreven.

###  <a name="the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>De status van de momentopname kan niet worden opgehaald, of een momentopname kan niet worden uitgevoerd
De virtuele machine back-up, is afhankelijk van de opdracht van een momentopname naar de onderliggende storage-account. Back-up kan mislukken omdat er geen toegang tot het opslagaccount of omdat het uitvoeren van de momentopname-taak wordt vertraagd.

#### <a name="solution"></a>Oplossing
De volgende voorwaarden kunnen leiden tot de taak momentopname is mislukt:

| Oorzaak | Oplossing |
| --- | --- |
| De status van de virtuele machine niet correct gerapporteerd omdat de virtuele machine wordt afgesloten in Remote Desktop Protocol (RDP). | Als u de virtuele machine in RDP afsluit, controleert u de portal om te bepalen of de status van de virtuele machine juist is. Als dit niet juist is, sluit u de virtuele machine in de portal met behulp van de **afsluiten** optie op het dashboard voor VM's. |
| De virtuele machine kan de host of fabric-adres ophalen van DHCP. | DHCP moet zijn ingeschakeld in de Gast voor de IaaS-VM back-up om te werken. Als de virtuele machine de host of fabric-adres niet uit de DHCP-reacties 245 ophalen kan, kan het downloaden of geen extensies worden uitgevoerd. Als u een statisch privé IP-adres nodig hebt, kunt u deze via het platform configureren. De DHCP-optie binnen de virtuele machine moet naar links worden ingeschakeld. Zie voor meer informatie, [instellen van een statische interne privé IP-adres](../virtual-network/virtual-networks-reserved-private-ip.md). |

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

### <a name="backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock"></a>De Backup-service heeft geen toegangsmachtiging voor de oude herstelpunten verwijderd vanwege een vergrendeling van de groep resource
Dit probleem is specifiek voor beheerde VM's waarin de gebruiker Hiermee vergrendelt u de resourcegroep. In dit geval kan de back-upservice niet oudere herstelpunten verwijderen. Omdat er een limiet van 18 herstelpunten zijn gemaakt, wordt de nieuwe back-ups worden niet gestart.

#### <a name="solution"></a>Oplossing

Los het probleem, verwijder de vergrendeling van de resourcegroep en voer de volgende stappen uit als u wilt verwijderen van de verzameling van herstelpunt: 
 
1. Verwijder de vergrendeling in de resourcegroep waarin de virtuele machine zich bevindt. 
2. Installeer ARMClient Chocolatey met: <br>
   https://github.com/projectkudu/ARMClient
3. Aanmelden bij ARMClient: <br>
    `.\armclient.exe login`
4. Haal de verzameling van herstelpunt dat overeenkomt met de virtuele machine: <br>
    `.\armclient.exe get https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Compute/restorepointcollections/AzureBackup_<VM-Name>?api-version=2017-03-30`

    Voorbeeld: `.\armclient.exe get https://management.azure.com/subscriptions/f2edfd5d-5496-4683-b94f-b3588c579006/resourceGroups/winvaultrg/providers/Microsoft.Compute/restorepointcollections/AzureBackup_winmanagedvm?api-version=2017-03-30`
5. De herstelpuntverzameling verwijderen: <br>
    `.\armclient.exe delete https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Compute/restorepointcollections/AzureBackup_<VM-Name>?api-version=2017-03-30` 
6. De volgende geplande back-up maakt automatisch een herstelpuntverzameling en nieuwe herstelpunten.

Zodra u klaar bent, kunt u het opnieuw plaatsen terug de vergrendeling op de VM-resourcegroep. 
