---
title: Voorbereiden van uw omgeving tot back-up van virtuele machines in Azure | Microsoft Docs
description: Zorg ervoor dat uw omgeving is voorbereid voor de back-ups van virtuele machines in Azure
services: backup
documentationcenter: 
author: markgalioto
manager: carmonn
editor: 
keywords: back-ups; een back-up;
ms.assetid: 238ab93b-8acc-4262-81b7-ce930f76a662
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 9/10/2017
ms.author: cwatson
ms.openlocfilehash: 917d211c7a5e3aefda79690d5e718dee0b30a8c7
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2017
---
# <a name="prepare-your-environment-to-back-up-azure-virtual-machines"></a>Uw omgeving voorbereiden op het maken van back-ups van virtuele Azure-machines
> [!div class="op_single_selector"]
> * [Resource Manager-model](backup-azure-arm-vms-prepare.md)
> * [Klassieke model](backup-azure-vms-prepare.md)
>
>

Voordat u kunt back-up Azure een virtuele machine (VM), zijn er drie voorwaarden moeten bestaan.

* U moet een back-upkluis maken of een bestaande back-upkluis identificeren *in dezelfde regio bevinden als uw VM*.
* Geen netwerkverbinding tussen de Azure openbare Internet-adressen en de Azure-opslag-eindpunten.
* Installeer de VM-agent op de virtuele machine.

Als u weet dat deze voorwaarden al bestaat in uw omgeving en gaat u verder met de [Back-up van uw virtuele machines artikel](backup-azure-vms.md). Anders loodst lezen op, in dit artikel u door de stappen voor het voorbereiden van uw back-up van een virtuele machine van Azure-omgeving.

##<a name="supported-operating-system-for-backup"></a>Ondersteund besturingssysteem voor back-up
 * **Linux**: Azure Backup ondersteunt [een lijst met distributies die zijn goedgekeurd door Azure](../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), met uitzondering van Core OS Linux. _Andere Bring-Your-eigenaar-Linux-distributies ook mogelijk werken zolang de VM-agent beschikbaar op de virtuele machine is en ondersteuning voor Python bestaat. We tekent echter niet de distributies voor back-up._
 * **Windows Server**: versies ouder dan Windows Server 2008 R2 worden niet ondersteund.


## <a name="limitations-when-backing-up-and-restoring-a-vm"></a>Beperkingen bij een back-up en herstellen van een virtuele machine
> [!NOTE]
> Azure heeft twee implementatiemodellen voor het maken en werken met resources: [Resource Manager en classic](../azure-resource-manager/resource-manager-deployment-model.md). De volgende lijst bevat de beperkingen bij implementatie in het klassieke model.
>
>

* Back-ups van virtuele machines met meer dan 16 gegevensschijven wordt niet ondersteund.
* Back-ups van virtuele machines met een gereserveerd IP-adres en er is geen gedefinieerde eindpunt wordt niet ondersteund.
* Back-upgegevens bevat geen gekoppeld netwerkstations is gekoppeld aan VM.
* Een bestaande virtuele machine kan tijdens het herstel niet worden vervangen. Eerst de bestaande virtuele machine en alle gekoppelde schijven verwijderen en vervolgens de gegevens terugzetten vanaf back-up.
* Regio-overschrijdende back-up en herstel wordt niet ondersteund.
* Back-ups van virtuele machines met behulp van de Azure Backup-service wordt ondersteund in alle openbare gebieden van Azure (Zie de [controlelijst](https://azure.microsoft.com/regions/#services) van ondersteunde regio's). Als de regio die u zoekt niet vandaag ondersteund wordt, wordt deze niet in de vervolgkeuzelijst weergegeven tijdens het maken van de kluis.
* Back-ups van virtuele machines met behulp van de Azure Backup-service wordt alleen ondersteund voor Selecteer besturingssystemen:
* Herstellen van een domeincontroller wordt (DC) VM die deel uitmaakt van een multi-DC-configuratie alleen ondersteund door PowerShell. Lees meer over [een multi-DC-domeincontroller terugzetten](backup-azure-restore-vms.md#restoring-domain-controller-vms).
* Herstellen van virtuele machines waarvoor de volgende speciale netwerkconfiguraties wordt alleen ondersteund door PowerShell. Virtuele machines die u maakt met behulp van de werkstroom terugzetten in de gebruikersinterface wordt geen van deze netwerkconfiguraties nadat de herstelbewerking voltooid is. Zie voor meer informatie, [herstellen van virtuele machines met speciale netwerkconfiguraties](backup-azure-restore-vms.md#restoring-vms-with-special-network-configurations).
  * Virtuele machines onder een load balancer-configuratie (intern en extern)
  * Virtuele machines met meerdere gereserveerde IP-adressen
  * Virtuele machines met meerdere netwerkadapters

## <a name="create-a-backup-vault-for-a-vm"></a>Een back-upkluis maken voor een virtuele machine
Een back-upkluis is een entiteit waarmee alle back-ups en herstelpunten worden opgeslagen die in de loop van de tijd zijn gemaakt. De back-upkluis bevat ook de back-upbeleid dat wordt toegepast op de virtuele machines back-up wordt gemaakt.

> [!IMPORTANT]
> Vanaf maart 2017 is het niet meer mogelijk om de klassieke portal te gebruiken voor het maken van back-upkluizen. Bestaande back-upkluizen worden nog wel ondersteund en het is mogelijk om [Azure PowerShell te gebruiken voor het maken van back-upkluizen](./backup-client-automation-classic.md#create-a-backup-vault). U wordt echter geadviseerd om voor alle implementaties Recovery Services-kluizen te maken, aangezien toekomstige verbeteringen alleen van toepassing zullen zijn op Recovery Services-kluizen.


Deze afbeelding ziet u de relaties tussen de verschillende entiteiten van de Azure Backup: ![Azure Backup-entiteiten en relaties](./media/backup-azure-vms-prepare/vault-policy-vm.png)



## <a name="network-connectivity"></a>Netwerkverbinding
Om de VM-momentopnamen te beheren, moet de Backup-extensie verbinding hebben met de Azure openbare IP-adressen. Zonder de juiste internetverbinding time-out van de virtuele machine HTTP-aanvragen en de back-upbewerking is mislukt. Als uw implementatie beschikt over toegangsbeperkingen (via een netwerkbeveiligingsgroep (NSG), bijvoorbeeld), kies een van deze opties voor het ontwikkelen van een duidelijke pad voor de back-verkeer:

* [Geaccepteerde het Azure datacenter IP ranges](http://www.microsoft.com/en-us/download/details.aspx?id=41653) -Raadpleeg het artikel voor instructies over hoe naar geaccepteerde de IP-adressen.
* Implementeer een HTTP-proxyserver voor het routeren van verkeer.

Wanneer u beslist welke optie u moet gebruiken, zijn de verschillen tussen beheerbaarheid, gedetailleerde controle en kosten.

| Optie | Voordelen | Nadelen |
| --- | --- | --- |
| Whitelist IP-adresbereiken |Er zijn geen extra kosten.<br><br>Voor toegang tot te openen in een NSG, gebruikt u de <i>Set AzureNetworkSecurityRule</i> cmdlet. |Moeilijk te beheren als de betrokken IP-adresbereiken op den duur veranderen.<br><br>Biedt toegang tot het geheel van Azure en niet alleen de opslag. |
| HTTP-proxy |Gedetailleerd beheer in de proxy van de opslag URL's toegestaan. Voor gedetailleerde controle van de installatie in de proxy https://\*.blob.core.windows.net/\* URL patroon moet wilt plaatsen. Aan de goedgekeurde IP-adressen alleen het storage-account dat wordt gebruikt door de virtuele machine, https://\<storageAccount\>.blob.core.windows.net/\* URL patroon moet wilt plaatsen. <br>Één punt Internet toegang tot virtuele machines.<br>Ten aanzien van wijzigingen in de Azure-IP-adres. |Extra kosten voor het uitvoeren van een virtuele machine met de proxysoftware. |

### <a name="whitelist-the-azure-datacenter-ip-ranges"></a>De Azure-datacenter geaccepteerde IP-adresbereiken
Aan de lijst met geaccepteerde het Azure datacenter IP-adresbereiken, raadpleegt u de [Azure-website](http://www.microsoft.com/en-us/download/details.aspx?id=41653) voor meer informatie over de IP-adresbereiken en instructies.

### <a name="using-an-http-proxy-for-vm-backups"></a>Met behulp van een HTTP-proxy voor VM-back-ups
Wanneer een back-up van een VM, verzendt de Backup-extensie op de virtuele machine opdrachten voor het beheer van de momentopname naar Azure Storage met behulp van een HTTPS-API. De Backup-extensie-verkeer via de HTTP-proxy routeren omdat dit het enige onderdeel dat is geconfigureerd voor toegang tot het openbare Internet.

> [!NOTE]
> Er is geen aanbeveling voor de proxysoftware die moet worden gebruikt. Zorg ervoor dat u kiest een proxy die uitgaande gebruikerspad is en die compatibel is met de onderstaande configuratiestappen. Zorg ervoor dat de procedure van derden mag niet de proxy-instellingen worden gewijzigd
>
>

In onderstaande voorbeeldafbeelding ziet de drie configuratiestappen die nodig zijn voor het gebruik van een HTTP-proxy:

* App VM routeert alle HTTP-verkeer voor het openbare Internet via Proxy VM is gebonden.
* Proxy VM zorgt ervoor dat binnenkomend verkeer van virtuele machines in het virtuele netwerk.
* De Netwerkbeveiligingsgroep (NSG) met de naam NSF-lockdown moet een beveiliging regel waardoor uitgaand internetverkeer van Proxy VM.

![NSG met HTTP-proxy-implementatie-diagram](./media/backup-azure-vms-prepare/nsg-with-http-proxy.png)

Volg deze stappen voor het gebruik van een HTTP-proxy om communicatie met het openbare Internet:

#### <a name="step-1-configure-outgoing-network-connections"></a>Step 1. Uitgaande netwerkverbindingen configureren
###### <a name="for-windows-machines"></a>Voor Windows-machines
Hiermee wordt het instellen van proxyserver-configuratie voor Lokaal systeemaccount.

1. Download [PsExec](https://technet.microsoft.com/sysinternals/bb897553)
2. Voer de volgende opdracht uit met verhoogde bevoegdheid om

     ```
     psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe"
     ```
     Internet explorer-venster wordt geopend.
3. Ga naar Extra -> Internet-opties -> verbindingen LAN-instellingen ->.
4. Controleer de proxy-instellingen voor systeem-account. Stel Proxy IP-adres en poort.
5. Sluit Internet Explorer.

Dit stelt een proxyconfiguratie voor alle computers en wordt gebruikt voor alle uitgaande HTTP/HTTPS-verkeer.

Als u hebt een proxyserver ingesteld op het huidige gebruikersaccount (niet een lokaal systeemaccount), gebruikt u het volgende script toepassen op SYSTEMACCOUNT:

```
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver
```

> [!NOTE]
> Als u '(407) proxyverificatie is vereist' in het logboek voor proxy server ziet, Controleer dat de verificatie van uw correct is ingesteld.
>
>

###### <a name="for-linux-machines"></a>Voor Linux-machines
Voeg de volgende regel om de ```/etc/environment``` bestand:

```
http_proxy=http://<proxy IP>:<proxy port>
```

Voeg de volgende regels voor de ```/etc/waagent.conf``` bestand:

```
HttpProxy.Host=<proxy IP>
HttpProxy.Port=<proxy port>
```

#### <a name="step-2-allow-incoming-connections-on-the-proxy-server"></a>Stap 2. Binnenkomende verbindingen toestaan op de proxy-server:
1. Open Windows Firewall op de proxyserver. De eenvoudigste manier om toegang tot de firewall is om te zoeken naar Windows Firewall met geavanceerde beveiliging.

    ![De Firewall openen](./media/backup-azure-vms-prepare/firewall-01.png)
2. In het dialoogvenster Windows Firewall met de rechtermuisknop op **regels voor binnenkomende verbindingen** en klik op **nieuwe regel...** .

    ![Een nieuwe regel maken](./media/backup-azure-vms-prepare/firewall-02.png)
3. In de **nieuwe Wizard regel voor binnenkomende**, kies de **aangepaste** optie voor de **regeltype** en klik op **volgende**.
4. Op de pagina selecteren de **programma**, kies **alle programma's** en klik op **volgende**.
5. Op de **protocollen en poorten** pagina, voer de volgende informatie en klik op **volgende**:

    ![Een nieuwe regel maken](./media/backup-azure-vms-prepare/firewall-03.png)

   * voor *protocoltype* kiezen *TCP*
   * voor *lokale poort* kiezen *bepaalde poorten*, Geef in het veld hieronder de ```<Proxy Port>``` die is geconfigureerd.
   * voor *externe poort* Selecteer *alle poorten*

     Klik helemaal tot aan het einde voor de rest van de wizard en geef een naam op voor deze regel.

#### <a name="step-3-add-an-exception-rule-to-the-nsg"></a>Stap 3. Een uitzonderingsregel toevoegen aan het NSG:
Voer de volgende opdracht in een Azure PowerShell-opdrachtprompt:

De volgende opdracht voegt een uitzondering aan het NSG. Deze uitzondering kan TCP-verkeer vanaf een willekeurige poort op 10.0.0.5 aan een internetadres op poort 80 (HTTP) of 443 (HTTPS). Als u een specifieke poort in het openbare Internet vereist, moet u om toe te voegen die poort naar de ```-DestinationPortRange``` ook.

```
Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
```

*Zorg ervoor dat u de namen in het voorbeeld door de details voor uw implementatie vervangen.*

## <a name="vm-agent"></a>VM-agent
Voordat u kunt back-up van de virtuele machine van Azure, moet u ervoor zorgen dat de Azure VM-agent correct is geïnstalleerd op de virtuele machine. Omdat de VM-agent een optioneel onderdeel op het moment dat de virtuele machine is gemaakt is, zorg ervoor dat het selectievakje in voor de VM-agent is geselecteerd voordat de virtuele machine is ingericht.

### <a name="manual-installation-and-update"></a>Handmatige installatie en bijwerken
De VM-agent is al aanwezig in virtuele machines die zijn gemaakt van de Azure-galerie. Virtuele machines die worden gemigreerd van lokale datacenters moet echter niet de VM-agent is geïnstalleerd. Voor deze virtuele machines moet de VM-agent expliciet worden geïnstalleerd. 

| **Bewerking** | **Windows** | **Linux** |
| --- | --- | --- |
| De VM-agent installeren |Download en installeer de [agent-MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). U hebt Administrator-bevoegdheden nodig om de installatie te kunnen uitvoeren.<li>[Werk de VM-eigenschap bij](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) om aan te geven dat de agent is geïnstalleerd. |<li> Installeer de meest recente [Linux-agent](../virtual-machines/linux/agent-user-guide.md). U hebt Administrator-bevoegdheden nodig om de installatie te kunnen uitvoeren. Het is raadzaam om de installatie van agent van uw opslagplaats voor distributie. We **wordt niet aanbevolen** installeren Linux VM-agent rechtstreeks vanuit github.  |
| De VM-agent bijwerken |Om de VM-agent bij te werken hoeft u alleen de [binaire bestanden voor de VM-agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) opnieuw te installeren. <br>Zorg ervoor dat er geen back-upbewerking wordt uitgevoerd terwijl de VM-agent wordt bijgewerkt. |Volg de instructies voor het [bijwerken van de Linux VM-agent](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Het is raadzaam om de agent van uw opslagplaats distributiepunten bijwerken. We **wordt niet aanbevolen** bijwerken Linux VM-agent rechtstreeks vanuit github.<br>Zorg ervoor dat er geen back-upbewerking wordt uitgevoerd terwijl de VM-agent wordt bijgewerkt. |
| De installatie van de VM-agent valideren |<li>Ga naar de map *C:\WindowsAzure\Packages* in de Azure VM. <li>Het bestand WaAppAgent.exe moet hier aanwezig zijn.<li> Klik met de rechtermuisknop op het bestand, ga naar **Eigenschappen** en selecteer vervolgens het tabblad **Details**. In het veld Productversie moet versie 2.6.1198.718 of hoger worden weergegeven. |N.v.t. |

Meer informatie over de [VM-agent](https://go.microsoft.com/fwLink/?LinkID=390493&clcid=0x409) en [hoe u deze installeert](https://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/).

### <a name="backup-extension"></a>Backup-extensie
Als u wilt back-up van de virtuele machine, installeert de Azure Backup-service een uitbreiding op de VM-agent. De Azure Backup-service wordt probleemloos bijgewerkt en er wordt zonder tussenkomst van de gebruiker een patch voor de Backup-extensie uitgevoerd.

De Backup-extensie is geïnstalleerd als de virtuele machine wordt uitgevoerd. Een actieve virtuele machine biedt ook de kans het grootst dat een toepassingsconsistente herstelpunt ophalen. Echter, de Azure Backup service back-up van de virtuele machine blijven, zelfs als deze is uitgeschakeld en de extensie kan niet worden geïnstalleerd (aka Offline VM). In dit geval wordt het herstelpunt zich *crashconsistent* zoals hierboven wordt beschreven.

## <a name="questions"></a>Vragen?
Als u vragen hebt of als er een functie is die u graag opgenomen ziet worden, [stuur ons dan uw feedback](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Volgende stappen
Nu dat u uw omgeving voor back-ups van uw virtuele machine hebt voorbereid, wordt de volgende logische stap is het maken van een back-up. De planning artikel vindt meer gedetailleerde informatie over back-ups van virtuele machines.

* [Back-up van virtuele machines](backup-azure-vms.md)
* [Uw VM-back-infrastructuur plannen](backup-azure-vms-introduction.md)
* [Back-ups van virtuele machine beheren](backup-azure-manage-vms.md)
