---
title: 'Azure Backup-fout oplossen: Guest Agent Status niet beschikbaar | Microsoft Docs'
description: Symptomen, oorzaken en oplossingen van Azure Backup fouten met betrekking tot agent, uitbreiding, schijven
services: backup
documentationcenter: 
author: genlin
manager: cshepard
editor: 
keywords: Azure back-up. VM-agent; Verbinding met het netwerk;
ms.assetid: 4b02ffa4-c48e-45f6-8363-73d536be4639
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/08/2017
ms.author: genli;markgal;
ms.openlocfilehash: f3195fa83479986a3e605abce618c78bcdb64dac
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-azure-backup-failure-issues-with-agent-andor-extension"></a>Azure Backup-fout oplossen: problemen met de agent en/of extensie

In dit artikel bevat stappen voor probleemoplossing op te lossen back-up-fouten die betrekking hebben op problemen in de communicatie met VM-agent en -extensie.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="vm-agent-unable-to-communicate-with-azure-backup"></a>VM-Agent kan niet communiceren met Azure Backup
Nadat u registreren en plannen van een virtuele machine voor de Azure Backup-service, initieert back-up van de taak door de communicatie met de VM-agent de momentopname van een punt in tijd. Een van de volgende voorwaarden kan verhinderen dat de momentopname wordt geactiveerd, wat op zijn beurt tot het mislukken van de back-up leiden kan. Volg onderstaande stappen in de gegeven volgorde voor probleemoplossing en probeer de bewerking opnieuw.
##### <a name="cause-1-the-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>1 oorzaak: [de virtuele machine geen internettoegang heeft](#the-vm-has-no-internet-access)
##### <a name="cause-2-the-agent-is-installed-in-the-vm-but-is-unresponsive-for-windows-vmsthe-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>2 oorzaak: [de agent is geïnstalleerd in de virtuele machine, maar is niet-reagerende (voor Windows-VM's)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### <a name="cause-3-the-agent-installed-in-the-vm-is-out-of-date-for-linux-vmsthe-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>3 oorzaak: [de agent is geïnstalleerd in de virtuele machine is verouderd (voor Linux VM's)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)
##### <a name="cause-4-the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-takenthe-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>4 oorzaak: [de status van de momentopname kan niet worden opgehaald of een momentopname kan niet worden gemaakt.](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### <a name="cause-5-the-backup-extension-fails-to-update-or-loadthe-backup-extension-fails-to-update-or-load"></a>5 oorzaak: [de Backup-extensie niet bijwerken of laden](#the-backup-extension-fails-to-update-or-load)

## <a name="snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine"></a>Momentopname-bewerking is mislukt, omdat er geen verbinding met het netwerk op de virtuele machine
Nadat u registreren en plannen van een virtuele machine voor de Azure Backup-service, initieert back-up van de taak door de communicatie met de Backup-extensie van de VM om een punt in tijd momentopname. Een van de volgende voorwaarden kan verhinderen dat de momentopname wordt geactiveerd, wat op zijn beurt tot het mislukken van de back-up leiden kan. Volg onderstaande stappen in de gegeven volgorde voor probleemoplossing en probeer de bewerking opnieuw.
##### <a name="cause-1-the-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>1 oorzaak: [de virtuele machine geen internettoegang heeft](#the-vm-has-no-internet-access)
##### <a name="cause-2-the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-takenthe-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>2 oorzaak: [de status van de momentopname kan niet worden opgehaald of een momentopname kan niet worden gemaakt.](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### <a name="cause-3-the-backup-extension-fails-to-update-or-loadthe-backup-extension-fails-to-update-or-load"></a>3 oorzaak: [de Backup-extensie niet bijwerken of laden](#the-backup-extension-fails-to-update-or-load)

## <a name="vmsnapshot-extension-operation-failed"></a>VMSnapshot extensie-bewerking is mislukt.

Nadat u registreren en plannen van een virtuele machine voor de Azure Backup-service, initieert back-up van de taak door de communicatie met de Backup-extensie van de VM om een punt in tijd momentopname. Een van de volgende voorwaarden kan verhinderen dat de momentopname wordt geactiveerd, wat op zijn beurt tot het mislukken van de back-up leiden kan. Volg onderstaande stappen in de gegeven volgorde voor probleemoplossing en probeer de bewerking opnieuw.
##### <a name="cause-1-the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-takenthe-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>1 oorzaak: [de status van de momentopname kan niet worden opgehaald of een momentopname kan niet worden gemaakt.](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### <a name="cause-2-the-backup-extension-fails-to-update-or-loadthe-backup-extension-fails-to-update-or-load"></a>2 oorzaak: [de Backup-extensie niet bijwerken of laden](#the-backup-extension-fails-to-update-or-load)
##### <a name="cause-3-the-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>3 oorzaak: [de virtuele machine geen internettoegang heeft](#the-vm-has-no-internet-access)
##### <a name="cause-4-the-agent-is-installed-in-the-vm-but-is-unresponsive-for-windows-vmsthe-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>4 oorzaak: [de agent is geïnstalleerd in de virtuele machine, maar is niet-reagerende (voor Windows-VM's)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### <a name="cause-5-the-agent-installed-in-the-vm-is-out-of-date-for-linux-vmsthe-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>5 oorzaak: [de agent is geïnstalleerd in de virtuele machine is verouderd (voor Linux VM's)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)

## <a name="unable-to-perform-the-operation-as-the-vm-agent-is-not-responsive"></a>Kan de bewerking niet uitvoeren omdat de VM-Agent niet reageert niet

Nadat u registreren en plannen van een virtuele machine voor de Azure Backup-service, initieert back-up van de taak door de communicatie met de Backup-extensie van de VM om een punt in tijd momentopname. Een van de volgende voorwaarden kan verhinderen dat de momentopname wordt geactiveerd, wat op zijn beurt tot het mislukken van de back-up leiden kan. Volg onderstaande stappen in de gegeven volgorde voor probleemoplossing en probeer de bewerking opnieuw.
##### <a name="cause-1-the-agent-is-installed-in-the-vm-but-is-unresponsive-for-windows-vmsthe-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>1 oorzaak: [de agent is geïnstalleerd in de virtuele machine, maar is niet-reagerende (voor Windows-VM's)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### <a name="cause-2-the-agent-installed-in-the-vm-is-out-of-date-for-linux-vmsthe-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>2 oorzaak: [de agent is geïnstalleerd in de virtuele machine is verouderd (voor Linux VM's)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)
##### <a name="cause-3-the-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>3 oorzaak: [de virtuele machine geen internettoegang heeft](#the-vm-has-no-internet-access)

## <a name="backup-failed-with-an-internal-error---please-retry-the-operation-in-a-few-minutes"></a>Back-up is mislukt met een interne fout: probeer de bewerking over enkele minuten opnieuw

Nadat u registreren en plannen van een virtuele machine voor de Azure Backup-service, initieert back-up van de taak door de communicatie met de Backup-extensie van de VM om een punt in tijd momentopname. Een van de volgende voorwaarden kan verhinderen dat de momentopname wordt geactiveerd, wat op zijn beurt tot het mislukken van de back-up leiden kan. Volg onderstaande stappen in de gegeven volgorde voor probleemoplossing en probeer de bewerking opnieuw.
##### <a name="cause-1-the-vm-has-no-internet-accessthe-vm-has-no-internet-access"></a>1 oorzaak: [de virtuele machine geen internettoegang heeft](#the-vm-has-no-internet-access)
##### <a name="cause-2-the-agent-installed-in-the-vm-but-unresponsive-for-windows-vmsthe-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>2 oorzaak: [de agent is geïnstalleerd in de virtuele machine, maar niet reageert (voor Windows-VM's)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)
##### <a name="cause-3-the-agent-installed-in-the-vm-is-out-of-date-for-linux-vmsthe-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>3 oorzaak: [de agent is geïnstalleerd in de virtuele machine is verouderd (voor Linux VM's)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)
##### <a name="cause-4-the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-takenthe-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>4 oorzaak: [de status van de momentopname kan niet worden opgehaald of een momentopname kan niet worden gemaakt.](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)
##### <a name="cause-5-the-backup-extension-fails-to-update-or-loadthe-backup-extension-fails-to-update-or-load"></a>5 oorzaak: [de Backup-extensie niet bijwerken of laden](#the-backup-extension-fails-to-update-or-load)

## <a name="the-specified-disk-configuration-is-not-supported"></a>De opgegeven schijf-configuratie wordt niet ondersteund.

Momenteel schijfgrootten biedt geen ondersteuning voor Azure Backup [groter zijn dan 1023GB](https://docs.microsoft.com/azure/backup/backup-azure-arm-vms-prepare#limitations-when-backing-up-and-restoring-a-vm). 
- Als er meer dan 1 TB schijven [koppel nieuwe schijven](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal) die minder dan 1 TB zijn <br>
- Kopieer vervolgens de gegevens van de schijf groter is dan 1TB in de zojuist gemaakte een of meer schijven van kleiner is dan 1 TB. <br>
- Zorg ervoor dat alle gegevens zijn gekopieerd en verwijder de schijven die groter is dan 1TB
- Starten van de back-up

## <a name="causes-and-solutions"></a>Oorzaken en oplossingen

### <a name="the-vm-has-no-internet-access"></a>De virtuele machine heeft geen internettoegang
Per de implementatievereiste van de virtuele machine geen internettoegang heeft of beperkingen die voorkomen toegang tot de Azure-infrastructuur dat heeft.

Voor een juiste werking van de Backup-extensie is verbinding naar Azure openbare IP-adressen vereist. De extensie stuurt opdrachten naar een Azure Storage-eindpunt (http-URL) voor het beheren van de momentopnamen van de virtuele machine. Als de extensie geen toegang tot het openbare Internet heeft, wordt uiteindelijk de back-up mislukt.

####  <a name="solution"></a>Oplossing
U lost het probleem probeert u een van de methoden die hier worden vermeld.
##### <a name="allow-access-to-the-azure-datacenter-ip-ranges"></a>Toegang tot de Azure datacenter IP-adresbereiken toestaan

1. Verkrijgen van de [lijst met Azure datacenter IP-adressen](https://www.microsoft.com/download/details.aspx?id=41653) zodat toegang heeft.
2. De IP-adressen deblokkeren door het uitvoeren van de **nieuw NetRoute** cmdlet in de Azure VM in een PowerShell-venster met verhoogde bevoegdheid. De cmdlet uitvoeren als beheerder.
3. Voor toegang tot de IP-adressen, regels toevoegen aan de netwerkbeveiligingsgroep als er een.

##### <a name="create-a-path-for-http-traffic-to-flow"></a>Maken van een pad voor de HTTP-verkeer

1. Als u beschikken over netwerkbeperkingen (bijvoorbeeld een netwerkbeveiligingsgroep), implementeert u een HTTP-proxyserver om het verkeer te routeren.
2. Als u wilt toegang tot het Internet toestaat vanaf de HTTP-proxyserver, regels toevoegen aan de netwerkbeveiligingsgroep als er een.

Zie voor meer informatie over het instellen van een HTTP-proxy voor VM-back-ups, [voorbereiden van uw back-up van virtuele machines in Azure-omgeving](backup-azure-vms-prepare.md#using-an-http-proxy-for-vm-backups).

Als u van schijven worden beheerd gebruikmaakt, moet u een extra poort (8443) geopend op de firewalls.

### <a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>De agent is geïnstalleerd in de virtuele machine, maar niet reageert (voor Windows-VM's)

#### <a name="solution"></a>Oplossing
De VM-Agent is beschadigd of de service is gestopt. De VM-agent opnieuw te installeren kan helpen de nieuwste versie downloaden en opnieuw starten de communicatie.

1. Controleer of Windows Guest Agent-service die wordt uitgevoerd in de services (services.msc) van de virtuele Machine. Probeer het opnieuw opstarten van de Gast-Agent voor Windows-service en starten van de back-up<br>
2. Als dit niet zichtbaar in de services is, controleert u of in programma's en onderdelen of Windows Guest agent-service is geïnstalleerd.
4. Als u zich kunt weergeven in programma's en onderdelen verwijderen van de Windows Guest-Agent.
5. Download en installeer de [meest recente versie van de agent-MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). U hebt beheerdersmachtigingen nodig om de installatie te kunnen uitvoeren.
6. Vervolgens moet u kunnen Windows Guest Agent-services in services weergeven
7. Voer een op-verzoek/ad-hoc back-up door te klikken op 'Nu back-up' in de portal.

Controleer ook de virtuele Machine is  **[.NET 4.5 geïnstalleerd in het systeem](https://docs.microsoft.com/en-us/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed)**. Het is vereist voor de VM-agent te communiceren met de service

### <a name="the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>De agent is geïnstalleerd in de virtuele machine is verouderd (voor Linux VM's)

#### <a name="solution"></a>Oplossing
Meest agent gerelateerde of extensie-gerelateerde fouten voor virtuele Linux-machines worden veroorzaakt door problemen met een verouderd VM-agent. Ga als volgt deze algemene richtlijnen voor het oplossen van dit probleem:

1. Volg de instructies voor [bijwerken van de Linux VM-agent](../virtual-machines/linux/update-agent.md).

 >[!NOTE]
 >We *aangeraden* dat u de agent beperken tot een opslagplaats distributiepunten bijwerken. We raden niet downloaden van de code van de agent rechtstreeks vanuit GitHub en wordt bijgewerkt. Als de nieuwste agent niet beschikbaar voor de distributie is, contact op met ondersteuning van de distributiepunten voor instructies over hoe u deze installeert. Om te controleren of de meest recente agent, gaat u naar de [Windows Azure Linux agent](https://github.com/Azure/WALinuxAgent/releases) pagina in de GitHub-opslagplaats.

2. Zorg ervoor dat de Azure-agent wordt uitgevoerd op de virtuele machine met de volgende opdracht:`ps -e`

 Als het proces wordt niet uitgevoerd, start u deze opnieuw met behulp van de volgende opdrachten:

 * Voor Ubuntu:`service walinuxagent start`
 * Voor andere distributies:`service waagent start`

3. [Configureren van de agent van automatisch opnieuw opstarten](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Voer een nieuwe test back-up. Als de fout zich blijft voordoen, verzamelt u de volgende logboeken van de klant VM:

   * /var/lib/waagent/*.XML
   * /var/log/waagent.log
   * / var/aanmelden/azure / *

Als we uitgebreide logboekregistratie is vereist voor waagent, volg deze stappen:

1. Zoek de volgende regel in het bestand /etc/waagent.conf: **uitgebreide logboekregistratie inschakelen (y | n)**
2. Wijzig de **Logs.Verbose** waarde uit de  *n*  naar *y*.
3. Sla de wijziging op en start waagent de vorige stappen in deze sectie.

### <a name="the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>De status van de momentopname kan niet worden opgehaald of een momentopname kan niet worden gemaakt.
De virtuele machine back-up is afhankelijk van de opdracht van een momentopname met de onderliggende opslagaccount. Back-up kan mislukken omdat deze geen toegang tot het opslagaccount heeft of omdat de uitvoering van de momentopname-taak wordt vertraagd.

#### <a name="solution"></a>Oplossing
Taakfout momentopname kunnen worden veroorzaakt door de volgende voorwaarden:

| Oorzaak | Oplossing |
| --- | --- |
| De virtuele machine heeft de SQL Server back-up is geconfigureerd. | Standaard back-up van de virtuele machine wordt uitgevoerd een VSS volledige back-ups op VM's van Windows. Op virtuele machines die worden uitgevoerd op basis van SQL Server-servers en op welke SQL-Server worden de back-up geconfigureerd, is de momentopname uitvoering vertragingen optreden.<br><br>Als u een back-up mislukt vanwege een probleem met de momentopname ondervindt, stelt u de volgende registersleutel:<br><br>**[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT] 'USEVSSCOPYBACKUP '=' TRUE'** |
| De status van de virtuele machine is niet juist gerapporteerd, omdat de virtuele machine wordt afgesloten in RDP. | Als u de virtuele machine in Remote Desktop Protocol (RDP) afsluit, controleert u de portal om te bepalen of de status van de VM juist is. Als dit niet juist is, sluit u de virtuele machine in de portal met behulp van de **afsluiten** optie op de VM-dashboard. |
| Veel virtuele machines in dezelfde cloudservice zijn geconfigureerd voor back-up op hetzelfde moment. | Het is een best practice verspreiden uit de back-upschema's voor virtuele machines in dezelfde cloudservice. |
| De virtuele machine wordt uitgevoerd op een hoog gebruik van CPU of geheugen. | Als de virtuele machine wordt uitgevoerd op een hoog CPU-gebruik (meer dan 90 procent) of hoog geheugengebruik, wordt de momentopname-taak is in de wachtrij en vertraagd en wordt er uiteindelijk een time-out optreedt. In deze situatie probeert een-op-verzoek back-up. |
| De virtuele machine kan de host/fabric-adres niet ophalen van DHCP. | DHCP moet binnen het gastbesturingssysteem voor de back-up van IaaS VM werken zijn ingeschakeld.  Als de virtuele machine niet het adres van de host/fabric uit DHCP-antwoord 245, kan het downloaden of uitvoeren van de uitbreidingen. Als u een statisch privé IP-adres nodig hebt, moet u deze via het platform configureren. De DHCP-optie in de virtuele machine moet naar links worden ingeschakeld. Zie voor meer informatie [instellen van een statische interne persoonlijke IP-adres](../virtual-network/virtual-networks-reserved-private-ip.md). |

### <a name="the-backup-extension-fails-to-update-or-load"></a>De Backup-extensie niet bijwerken of laden
Als extensies kunnen niet worden geladen, wordt de back-up mislukt omdat een momentopname kan niet worden gemaakt.

#### <a name="solution"></a>Oplossing

**Voor Windows-gasten:** controleren dat de iaasvmprovider-service is ingeschakeld en het opstarttype van *automatische*. Als de service niet op deze manier is geconfigureerd, het inschakelen om te bepalen of de volgende back-up is geslaagd.

**Voor Linux-gasten:** Controleer of de nieuwste versie van VMSnapshot voor Linux (de extensie die wordt gebruikt door de back-up) 1.0.91.0 is.<br>


Als de Backup-extensie wordt nog steeds niet bijwerken of geladen, kunt u de extensie VMSnapshot kan worden geladen door de uitbreiding verwijderen afdwingen. De volgende back-poging laadt u opnieuw de extensie.

Wilt u de uitbreiding verwijderen, het volgende doen:

1. Ga naar de [Azure Portal](https://portal.azure.com/).
2. Ga naar de virtuele machine die u problemen met de back-up kent.
3. Klik op **instellingen**.
4. Klik op **extensies**.
5. Klik op **Vmsnapshot extensie**.
6. Klik op **verwijderen**.

Deze procedure zorgt ervoor dat de extensie opnieuw worden geïnstalleerd tijdens de volgende back-up.

