---
title: 'Azure Backup-fout oplossen: Guest Agent Status niet beschikbaar | Microsoft Docs'
description: Symptomen, oorzaken en oplossingen van Azure Backup-fouten die betrekking hebben op de agent, uitbreiding en schijven.
services: backup
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
keywords: Azure back-up. VM-agent; Verbinding met het netwerk;
ms.assetid: 4b02ffa4-c48e-45f6-8363-73d536be4639
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 01/09/2018
ms.author: genli;markgal;sogup;
ms.openlocfilehash: 81678f6a8659ffb763ebfe418098e510c73f6ae0
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="troubleshoot-azure-backup-failure-issues-with-the-agent-or-extension"></a>Azure Backup-fout oplossen: problemen met de agent of de extensie

Dit artikel bevat stappen voor probleemoplossing waarmee u kunnen Azure Backup fouten met betrekking tot communicatie met de VM-agent en de extensie oplossen.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="vm-agent-unable-to-communicate-with-azure-backup"></a>VM-agent kan niet communiceren met Azure Backup

Foutbericht weergegeven: 'VM-Agent kan niet communiceren met Azure Backup'

Nadat u registreren en plannen van een virtuele machine voor de Backup-service, initieert back-up van de taak door de communicatie met de VM-agent de momentopname van een punt in tijd. Een van de volgende voorwaarden kan voorkomen dat de momentopname wordt geactiveerd. Wanneer een momentopname is niet geactiveerd, mislukken de back-up. De volgende stappen voor probleemoplossing in de volgorde en probeer de bewerking vervolgens opnieuw:

**1 oorzaak: [de virtuele machine heeft geen toegang tot internet](#the-vm-has-no-internet-access)**  
**2 oorzaak: [de agent is geïnstalleerd in de virtuele machine, maar het niet-reagerende (voor Windows-VM's)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**3 oorzaak: [de agent is geïnstalleerd in de virtuele machine is verouderd (voor Linux VM's)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**4 oorzaak: [de status van de momentopname kan niet worden opgehaald, of een momentopname kan niet worden gemaakt.](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**    
**5 oorzaak: [de Backup-extensie niet bijwerken of laden](#the-backup-extension-fails-to-update-or-load)**  

## <a name="snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine"></a>Momentopname-bewerking is mislukt omdat de virtuele machine niet is verbonden met het netwerk

Foutbericht weergegeven: 'Een momentopname van de bewerking is mislukt omdat er geen verbinding met het netwerk op de virtuele machine'

Nadat u registreren en plannen van een virtuele machine voor de Azure Backup-service, initieert back-up van de taak door de communicatie met de Backup-extensie van de VM om een punt in tijd momentopname. Een van de volgende voorwaarden kan voorkomen dat de momentopname wordt geactiveerd. Als de momentopname is niet geactiveerd, optreden een back-upfouten. De volgende stappen voor probleemoplossing in de volgorde en probeer de bewerking vervolgens opnieuw:    
**1 oorzaak: [de virtuele machine heeft geen toegang tot internet](#the-vm-has-no-internet-access)**  
**2 oorzaak: [de status van de momentopname kan niet worden opgehaald, of een momentopname kan niet worden gemaakt.](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**3 oorzaak: [de Backup-extensie niet bijwerken of laden](#the-backup-extension-fails-to-update-or-load)**  

## <a name="vmsnapshot-extension-operation-failed"></a>VMSnapshot extensie mislukt

Foutbericht weergegeven: 'VMSnapshot extensie bewerking is mislukt'

Nadat u registreren en plannen van een virtuele machine voor de Azure Backup-service, initieert back-up van de taak door de communicatie met de Backup-extensie van de VM om een punt in tijd momentopname. Een van de volgende voorwaarden kan voorkomen dat de momentopname wordt geactiveerd. Als de momentopname is niet geactiveerd, optreden een back-upfouten. De volgende stappen voor probleemoplossing in de volgorde en probeer de bewerking vervolgens opnieuw:  
**1 oorzaak: [de status van de momentopname kan niet worden opgehaald, of een momentopname kan niet worden gemaakt.](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**2 oorzaak: [de Backup-extensie niet bijwerken of laden](#the-backup-extension-fails-to-update-or-load)**  
**3 oorzaak: [de agent is geïnstalleerd in de virtuele machine, maar het niet-reagerende (voor Windows-VM's)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**4 oorzaak: [de agent is geïnstalleerd in de virtuele machine is verouderd (voor Linux VM's)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**

## <a name="backup-fails-because-the-vm-agent-is-unresponsive"></a>Back-up is mislukt omdat de VM-agent reageert niet

Fout messagae: 'Kan niet aan de bewerking niet uitvoeren omdat de VM-Agent niet reageert'

Nadat u registreren en plannen van een virtuele machine voor de Azure Backup-service, initieert back-up van de taak door de communicatie met de Backup-extensie van de VM om een punt in tijd momentopname. Een van de volgende voorwaarden kan voorkomen dat de momentopname wordt geactiveerd. Als de momentopname is niet geactiveerd, optreden een back-upfouten. De volgende stappen voor probleemoplossing in de volgorde en probeer de bewerking vervolgens opnieuw:  
**1 oorzaak: [de agent is geïnstalleerd in de virtuele machine, maar het niet-reagerende (voor Windows-VM's)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**2 oorzaak: [de agent is geïnstalleerd in de virtuele machine is verouderd (voor Linux VM's)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**3 oorzaak: [de virtuele machine heeft geen toegang tot internet](#the-vm-has-no-internet-access)**  

## <a name="backup-fails-with-an-internal-error"></a>Back-up is mislukt met een interne fout

Foutbericht weergegeven: 'back-up is mislukt met een interne fout: probeer de bewerking over enkele minuten opnieuw'

Nadat u registreren en plannen van een virtuele machine voor de Azure Backup-service, initieert back-up van de taak door de communicatie met de Backup-extensie van de VM om een punt in tijd momentopname. Een van de volgende voorwaarden kan voorkomen dat de momentopname wordt geactiveerd. Als de momentopname is niet geactiveerd, optreden een back-upfouten. De volgende stappen voor probleemoplossing in de volgorde en probeer de bewerking vervolgens opnieuw:  
**1 oorzaak: [de virtuele machine heeft geen toegang tot internet](#the-vm-has-no-internet-access)**  
**2 oorzaak: [de agent is geïnstalleerd in de virtuele machine, maar reageert (voor Windows-VM's)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**3 oorzaak: [de agent is geïnstalleerd in de virtuele machine is verouderd (voor Linux VM's)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**4 oorzaak: [de status van de momentopname kan niet worden opgehaald, of een momentopname kan niet worden gemaakt.](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**5 oorzaak: [de Backup-extensie niet bijwerken of laden](#the-backup-extension-fails-to-update-or-load)**  
**6 oorzaak: [Backup-service is niet gemachtigd om de oude herstelpunten verwijderd vanwege een vergrendeling van de groep resource](#backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock)**

## <a name="causes-and-solutions"></a>Oorzaken en oplossingen

### <a name="the-vm-has-no-internet-access"></a>De virtuele machine heeft geen toegang tot internet
De virtuele machine geen toegang tot internet, per de implementatievereiste. Of het beperkingen die voorkomen toegang tot de Azure-infrastructuur dat hebt.

Voor een juiste werking van de Backup-extensie is verbinding naar Azure openbare IP-adressen vereist. De extensie stuurt opdrachten naar een Azure-opslag-eindpunt (http-URL) voor het beheren van de momentopnamen van de virtuele machine. Als de extensie heeft geen toegang tot het openbare internet, wordt uiteindelijk de back-up mislukt.

####  <a name="solution"></a>Oplossing
U lost het probleem probeert u een van de volgende methoden:

##### <a name="allow-access-to-azure-storage-that-corresponds-to-the-region"></a>Toegang tot Azure-opslag die overeenkomt met de regio toestaan

U kunt [service labels](../virtual-network/security-overview.md#service-tags) verbindingen naar de opslag van de specifieke regio wilt toestaan. Zorg ervoor dat de regel die toegang tot het opslagaccount verleent hogere prioriteit dan de regel die blokken internettoegang heeft. 

![Netwerkbeveiligingsgroep met labels van de opslag voor een regio](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

> [!WARNING]
> Storage-service-tags zijn Preview-versie. Ze zijn alleen beschikbaar in specifieke gebieden. Zie voor een lijst met regio's, [Service-tags voor opslag](../virtual-network/security-overview.md#service-tags).

##### <a name="create-a-path-for-http-traffic"></a>Een pad voor HTTP-verkeer maken

1. Als u beschikken over netwerkbeperkingen (bijvoorbeeld een netwerkbeveiligingsgroep), implementeert u een HTTP-proxyserver om het verkeer te routeren.
2. Als u wilt toegang tot het internet toestaat vanaf de HTTP-proxyserver, regels toevoegen aan de netwerkbeveiligingsgroep als er een.

Zie voor meer informatie over het instellen van een HTTP-proxy voor VM-back-ups, [voorbereiden van uw back-up van virtuele machines in Azure-omgeving](backup-azure-arm-vms-prepare.md#establish-network-connectivity).

Als u Azure beheerd schijven gebruikt, moet u mogelijk een geopende extra poort (poort 8443) op de firewalls.

### <a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>De agent op de virtuele machine is geïnstalleerd, maar het is niet-reagerende (voor Windows-VM's)

#### <a name="solution"></a>Oplossing
De VM-agent is beschadigd of de service is gestopt. De VM-agent opnieuw te installeren, kunt de nieuwste versie. Het helpt tevens communicatie met de service opnieuw starten.

1. Bepalen of de service Windows Guest-Agent wordt uitgevoerd in de VM-services (services.msc). Probeer het opnieuw opstarten van de Gast-Agent voor Windows-service en starten van de back-up.    
2. Als de service Windows Guest Agent niet worden weergegeven in de services in het Configuratiescherm, gaat u naar **programma's en onderdelen** om te bepalen of de service Windows Guest-Agent is geïnstalleerd.
4. Als de Windows Guest-Agent wordt weergegeven in **programma's en onderdelen**, verwijdert u de Windows Guest-Agent.
5. Download en installeer de [meest recente versie van de agent MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). U moet beheerdersrechten om de installatie te voltooien.
6. Controleer of de Gast-Agent voor Windows-services wordt weergegeven in de services.
7. Een back-up van op aanvraag uitvoeren: 
    * Selecteer in de portal **back-up nu**.

Controleer ook of die [Microsoft .NET 4.5 is geïnstalleerd](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) in de virtuele machine. .NET 4.5 is vereist voor de VM-agent te communiceren met de service.

### <a name="the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>De agent is geïnstalleerd in de virtuele machine is verouderd (voor Linux VM's)

#### <a name="solution"></a>Oplossing
Meest agent gerelateerde of extensie-gerelateerde fouten voor virtuele Linux-machines worden veroorzaakt door problemen met een verouderd VM-agent. Ga als volgt deze algemene richtlijnen voor het oplossen van dit probleem:

1. Volg de instructies voor [bijwerken van de Linux VM-agent](../virtual-machines/linux/update-agent.md).

 > [!NOTE]
 > We *aangeraden* dat u de agent beperken tot een opslagplaats distributiepunten bijwerken. We raden niet downloaden van de code van de agent rechtstreeks vanuit GitHub en wordt bijgewerkt. Als u de nieuwste agent voor de distributie is geen distributiepunt beschikbaar is, neem contact op met ondersteuning voor instructies over hoe u deze installeert. Om te controleren of de meest recente agent, gaat u naar de [Windows Azure Linux agent](https://github.com/Azure/WALinuxAgent/releases) pagina in de GitHub-opslagplaats.

2. Zorg ervoor dat de Azure-agent wordt uitgevoerd op de virtuele machine met de volgende opdracht: `ps -e`

 Als het proces wordt uitgevoerd, start u deze opnieuw met behulp van de volgende opdrachten:

 * Voor Ubuntu: `service walinuxagent start`
 * Voor andere distributies: `service waagent start`

3. [Configureren van de agent van automatisch opnieuw opstarten](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Voer een nieuwe test back-up. Als de fout zich blijft voordoen, moet u de volgende logboeken verzamelen van de virtuele machine:

   * /var/lib/waagent/*.XML
   * /var/log/waagent.log
   * / var/aanmelden/azure / *

Als we uitgebreide logboekregistratie is vereist voor waagent, volg deze stappen:

1. Zoek de volgende regel in het bestand /etc/waagent.conf: **uitgebreide logboekregistratie inschakelen (y | n)**
2. Wijzig de **Logs.Verbose** waarde uit de *n* naar *y*.
3. Sla de wijziging op en start waagent via de stappen die eerder in deze sectie worden beschreven.

###  <a name="the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>De status van de momentopname kan niet worden opgehaald, of een momentopname kan niet worden gemaakt.
De virtuele machine back-up is afhankelijk van de opdracht van een momentopname met de onderliggende opslagaccount. Back-up kan mislukken omdat er geen toegang tot het opslagaccount of omdat de uitvoering van de momentopname-taak wordt vertraagd.

#### <a name="solution"></a>Oplossing
De volgende voorwaarden kunnen ertoe leiden dat de momentopname-taak niet worden uitgevoerd:

| Oorzaak | Oplossing |
| --- | --- |
| De status van de virtuele machine is niet juist gerapporteerd, omdat de virtuele machine wordt afgesloten in Remote Desktop Protocol (RDP). | Als u de virtuele machine in RDP afsluit, controleert u de portal om te bepalen of de status van de VM juist is. Als dit niet juist is, sluit u de virtuele machine in de portal met behulp van de **afsluiten** optie op de VM-dashboard. |
| De virtuele machine ontvangen geen adres van de host of fabric van DHCP. | DHCP moet binnen het gastbesturingssysteem voor de back-up van IaaS VM werken zijn ingeschakeld. Als de virtuele machine niet het adres van de host of fabric uit DHCP-antwoord 245, kan het downloaden of uitvoeren van de uitbreidingen. Als u een statisch privé IP-adres nodig hebt, kunt u deze via het platform configureren. De DHCP-optie in de virtuele machine moet naar links worden ingeschakeld. Zie voor meer informatie [instellen van een statische interne persoonlijke IP-adres](../virtual-network/virtual-networks-reserved-private-ip.md). |

### <a name="the-backup-extension-fails-to-update-or-load"></a>De Backup-extensie niet bijwerken of laden
Als de uitbreidingen kunnen niet worden geladen, worden back-up mislukt omdat een momentopname kan niet worden gemaakt.

#### <a name="solution"></a>Oplossing

Verwijder de extensie om af te dwingen de extensie VMSnapshot om opnieuw te laden. De volgende back-poging wordt opnieuw geladen met de extensie.

De extensie verwijderen:

1. In de [Azure-portal](https://portal.azure.com/), gaat u naar de virtuele machine waarop back-up mislukt.
2. Selecteer **instellingen**.
3. Selecteer **extensies**.
4. Selecteer **Vmsnapshot extensie**.
5. Selecteer **verwijderen**.

Uitvoeren van deze stappen zorgt ervoor dat de extensie opnieuw worden geïnstalleerd tijdens de volgende back-up.

### <a name="backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock"></a>De Backup-service is niet gemachtigd om de oude herstelpunten verwijderd vanwege een vergrendeling van de groep resource
Dit probleem is specifiek voor VM's van beheerde waarin de gebruiker Hiermee vergrendelt u de resourcegroep. De Backup-service niet in dit geval oudere herstelpunten te verwijderen. Omdat er een limiet van 18 herstelpunten, wordt de nieuwe back-ups worden niet gestart.

#### <a name="solution"></a>Oplossing

U lost het probleem door de volgende stappen voor het verwijderen van de verzameling van de punt herstellen: <br>
 
1. Verwijder de vergrendeling in de resourcegroep waarin de virtuele machine zich bevindt. 
2. Installeer ARMClient Chocolatey met: <br>
   https://github.com/projectkudu/ARMClient
3. Aanmelden bij ARMClient: <br>
    `.\armclient.exe login`
4. De verzameling van herstelpunt die overeenkomt met de virtuele machine ophalen: <br>
    `.\armclient.exe get https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Compute/restorepointcollections/AzureBackup_<VM-Name>?api-version=2017-03-30`

    Voorbeeld: `.\armclient.exe get https://management.azure.com/subscriptions/f2edfd5d-5496-4683-b94f-b3588c579006/resourceGroups/winvaultrg/providers/Microsoft.Compute/restorepointcollections/AzureBackup_winmanagedvm?api-version=2017-03-30`
5. Verwijder de verzameling van de punt herstellen: <br>
    `.\armclient.exe delete https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Compute/restorepointcollections/AzureBackup_<VM-Name>?api-version=2017-03-30` 
6. De volgende geplande back-up maakt automatisch een punt-verzameling van herstel en nieuwe herstelpunten.

 
Het probleem zal opnieuw optreden als u de resourcegroep opnieuw vergrendelen. 

