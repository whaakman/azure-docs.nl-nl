---
title: 'Azure back-up: Back-ups van virtuele machines voorbereiden | Microsoft Docs'
description: Zorg ervoor dat uw omgeving is voorbereid voor de back-ups van virtuele machines in Azure.
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: back-ups; een back-up;
ms.assetid: e87e8db2-b4d9-40e1-a481-1aa560c03395
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 9/3/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: 9b3584a93766be6052c822f40328169910de26c7
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2017
---
# <a name="prepare-your-environment-to-back-up-resource-manager-deployed-virtual-machines"></a>Uw omgeving voorbereiden op door Resource Manager geïmplementeerde virtuele machines

In dit artikel bevat de stappen voor het voorbereiden van uw omgeving tot back-up van een Resource Manager geïmplementeerde virtuele machine (VM). De stappen in de procedures gebruikt u de Azure-portal.  

De Azure Backup-service heeft twee soorten kluizen (back-up kluizen en recovery services-kluizen) voor het beveiligen van uw virtuele machines. Een back-upkluis beveiligt virtuele machines die zijn geïmplementeerd met behulp van het klassieke implementatiemodel. Een recovery services-kluis beveiligt **zowel klassieke geïmplementeerd of Resource Manager geïmplementeerde VM's**. U moet een Recovery Services-kluis gebruiken om een Resource Manager geïmplementeerde VM te beveiligen.

> [!NOTE]
> Azure heeft twee implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../azure-resource-manager/resource-manager-deployment-model.md). 

Voordat u kunt beveiligen of back-up van een Resource Manager geïmplementeerde virtuele machine (VM), zorg er dan voor dat deze vereisten bestaan:

* Een recovery services-kluis maken (of een bestaande recovery services-kluis identificeren) *in dezelfde locatie als uw VM*.
* Selecteer een scenario, het back-upbeleid definiëren en definiëren van items moeten worden beveiligd.
* Controleer de installatie van de VM-Agent op de virtuele machine.
* Controleer de netwerkverbinding
* Voor virtuele Linux-machines, als u wilt aanpassen van uw back-omgeving voor de toepassing consistente back-ups neemt u Volg de [stappen voor het configureren van scripts die voorafgaat aan momentopnamen en volgt op momentopnamen](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent)

Als u weet dat deze voorwaarden al bestaat in uw omgeving en gaat u verder met de [Back-up van uw virtuele machines artikel](backup-azure-arm-vms.md). Als u wilt instellen of controleren, een van deze vereisten, begeleidt in dit artikel u de stappen voor het voorbereiden van of de vereiste.

##<a name="supported-operating-system-for-backup"></a>Ondersteund besturingssysteem voor back-up
 * **Linux**: Azure Backup ondersteunt [een lijst met distributies die zijn goedgekeurd door Azure](../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), met uitzondering van Core OS Linux. _Andere Bring-Your-eigenaar-Linux-distributies ook mogelijk werken zolang de VM-agent beschikbaar op de virtuele machine is en ondersteuning voor Python bestaat. We tekent echter niet de distributies voor back-up._
 * **Windows Server**: versies ouder dan Windows Server 2008 R2 worden niet ondersteund.

## <a name="limitations-when-backing-up-and-restoring-a-vm"></a>Beperkingen bij een back-up en herstellen van een virtuele machine
Voordat u uw omgeving hebt voorbereid, moet u de beperkingen begrijpen.

* Back-ups van virtuele machines met meer dan 16 gegevensschijven wordt niet ondersteund.
* Back-ups van virtuele machines met gegevens groter zijn dan 1023GB schijfgrootten wordt niet ondersteund.

> [!NOTE]
> We hebben een beperkte preview om back-ups voor virtuele machines met > 1 TB aan onbeheerde schijven te ondersteunen. Raadpleeg voor informatie [Private preview voor back-ondersteuning voor grote schijven VM](https://gallery.technet.microsoft.com/Instant-recovery-point-and-25fe398a)
>
>

* Back-ups van virtuele machines met een gereserveerd IP-adres en er is geen gedefinieerde eindpunt wordt niet ondersteund.
* Back-up van virtuele machines die zijn versleuteld met NET BEK wordt niet ondersteund. Back-up van Linux virtuele machines die zijn versleuteld met behulp van versleuteling LUKS wordt niet ondersteund.
* Back-up van virtuele machines met Cluster Shared Volumes(CSV) of Scale out File Server-configuratie wordt niet aanbevolen omdat ze nodig hebben met betrekking tot alle virtuele machines die zijn opgenomen in de configuratie van het cluster tijdens momentopname taak. Azure Backup biedt geen ondersteuning voor consistentie tussen meerdere VM's. 
* Back-upgegevens bevat geen gekoppeld netwerkstations is gekoppeld aan VM.
* Een bestaande virtuele machine kan tijdens het herstel niet worden vervangen. Als u probeert te herstellen van de virtuele machine wanneer de virtuele machine bestaat, mislukt de herstelbewerking opnieuw.
* Regio-overschrijdende back-up en herstel worden niet ondersteund.
* U kunt back-ups van virtuele machines in alle openbare gebieden van Azure (Zie de [controlelijst](https://azure.microsoft.com/regions/#services) van ondersteunde regio's). Als de regio die u zoekt niet vandaag ondersteund wordt, wordt deze niet in de vervolgkeuzelijst weergegeven tijdens het maken van de kluis.
* Herstellen van een domeincontroller wordt (DC) VM die deel uitmaakt van een multi-DC-configuratie alleen ondersteund door PowerShell. Lees meer over [een multi-DC-domeincontroller terugzetten](backup-azure-arm-restore-vms.md#restore-domain-controller-vms).
* Herstellen van virtuele machines waarvoor de volgende speciale netwerkconfiguraties wordt alleen ondersteund door PowerShell. Virtuele machines die zijn gemaakt met behulp van de werkstroom terugzetten in de gebruikersinterface wordt geen van deze netwerkconfiguraties nadat de herstelbewerking voltooid is. Zie voor meer informatie, [herstellen van virtuele machines met speciale netwerkconfiguraties](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations).

  * Virtuele machines onder een load balancer-configuratie (intern en extern)
  * Virtuele machines met meerdere gereserveerde IP-adressen
  * Virtuele machines met meerdere netwerkadapters

## <a name="create-a-recovery-services-vault-for-a-vm"></a>Een Recovery Services-kluis voor een VM maken
Een recovery services-kluis is een entiteit die de back-ups en herstelpunten die zijn gemaakt na verloop van tijd worden opgeslagen. De recovery services-kluis bevat ook het back-upbeleid gekoppeld aan de beveiligde virtuele machines.

Een Recovery Services-kluis maken:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik in het menu Hub op **Bladeren** en typ in de lijst met resources **Recovery Services**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Klik op **Recovery Services-kluis**.

    ![Klik op de knop Bladeren en typ Recovery Services. Wanneer u de optie Recovery Services-kluis ziet, klikt u op het te openen van de blade Recovery Services-kluis.](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

    De lijst met Recovery Services-kluizen wordt weergegeven.
3. Klik in het menu **Recovery Services-kluizen** op **Toevoegen**.

    ![Een Recovery Services-kluis maken, stap 2](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

    De blade Recovery Services-kluis wordt geopend en u wordt gevraagd een **naam**, **abonnement**, **resourcegroep** en **locatie** in te voeren.

    ![Een Recovery Services-kluis maken, stap 5](./media/backup-azure-arm-vms-prepare/rs-vault-attributes.png)
4. Voer bij **Naam** een beschrijvende naam in om de kluis aan te duiden. De naam moet uniek zijn voor het Azure-abonnement. Typ een naam die tussen 2 en 50 tekens bevat. De naam moet beginnen met een letter en mag alleen letters, cijfers en afbreekstreepjes bevatten.
5. Klik op **Abonnement** om de beschikbare lijst met abonnementen te bekijken. Als u niet zeker weet welk abonnement u moet gebruiken, gebruikt u het standaard- (of voorgestelde) abonnement. Er zijn alleen meerdere mogelijkheden als uw organisatieaccount is gekoppeld aan meerdere Azure-abonnementen.
6. Klik op **Resourcegroep** om de lijst met beschikbare resourcegroepen te bekijken of klik op **Nieuw** om een nieuwe resourcegroep te maken. Zie [Overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) voor meer informatie over resourcegroepen.
7. Klik op **Locatie** om de geografische regio voor de kluis te selecteren. De kluis **moet** zich in dezelfde regio bevinden als de virtuele machines die u wilt beveiligen.

   > [!IMPORTANT]
   > Als u niet zeker weet op welke locatie uw VM zich bevindt, sluit u het dialoogvenster voor het maken van de kluis en gaat u naar de lijst met virtuele machines in de portal. Als u virtuele machines in meerdere regio's hebt, moet u in elke regio een Recovery Services-kluis maken. Maak de kluis op de eerste locatie voordat u verdergaat met de volgende locatie. U hoeft geen opslagaccounts voor het opslaan van de back-upgegevens op te geven: dit wordt automatisch door de Recovery Services-kluis en de Azure Backup-service afgehandeld.
   >
   >

8. Klik op **Create**. Het kan even duren voordat de Recovery Services-kluis is gemaakt. Controleer de statusmeldingen rechtsboven in de portal. Zodra de kluis is gemaakt, wordt deze weergegeven in de lijst met Recovery Services-kluizen. Als u uw kluis niet ziet, klikt u op **vernieuwen** naar

    ![Lijst met back-upkluizen](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

    Nu u uw kluis hebt gemaakt, leert u hoe u de opslagreplicatie instelt.

## <a name="set-storage-replication"></a>Opslagreplicatie instellen
U kunt met de optie voor opslagreplicatie kiezen tussen geografisch redundante opslag en lokaal redundante opslag. Uw kluis heeft standaard geografisch redundante opslag. Laat de optie ingesteld op geografisch redundante opslag als dit uw primaire back-up is. Kies lokaal redundante opslag als u een goedkopere optie wilt die niet zo duurzaam is.

De instelling voor opslagreplicatie bewerken:

1. Op de **Recovery Services-kluizen** blade, selecteer uw kluis.
    Wanneer u klikt op uw kluis uit de blade instellingen (*waarvan de naam van de kluis is aan de bovenkant*) en de kluis beschrijft blade wordt geopend.

    ![Kies uw kluis uit de lijst met back-upkluizen](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)

2. Op de **instellingen** blade de verticale schuifbalk kunt Schuif omlaag naar de **beheren** sectie. Klik op **back-upinfrastructuur** om de blade te openen. In de **algemene** sectie Klik **back-upconfiguratie** om de blade te openen. Kies op de blade **Back-upconfiguratie** de optie voor opslagreplicatie voor uw kluis. Uw kluis heeft standaard geografisch redundante opslag. Als u het opslagtype voor de replicatiegroep wijzigt, klikt u op **opslaan**.

    ![Lijst met back-upkluizen](./media/backup-azure-arm-vms-prepare/full-blade.png)

     Als Azure uw primaire eindpunt is voor back-upopslag, blijf dan geografisch redundante opslag gebruiken. Als u van Azure als het eindpunt van een niet-primaire back-upopslag gebruikmaakt, kiest u lokaal redundante opslag. U vindt meer informatie over de opties voor [geografisch redundante](../storage/common/storage-redundancy.md#geo-redundant-storage) en [lokaal redundante](../storage/common/storage-redundancy.md#locally-redundant-storage) opslag in het [overzicht van Azure Storage-replicatie](../storage/common/storage-redundancy.md).
    Nadat u de opslagoptie voor uw kluis hebt gekozen, bent u klaar om de VM aan de kluis te koppelen. Voordat u de VM aan de kluis koppelt, moet u eerst de virtuele Azure-machines detecteren en registreren.

## <a name="select-a-backup-goal-set-policy-and-define-items-to-protect"></a>Een back-doel selecteren, beleid instellen en definiëren welke items moeten worden beveiligd
Voordat u een VM met een kluis registreert, voert u het detectieproces uit om ervoor te zorgen dat nieuwe virtuele machines die zijn toegevoegd aan het abonnement, worden geïdentificeerd. Er wordt een query uitgevoerd om de virtuele Azure-machines in het abonnement weer te geven, samen met aanvullende informatie zoals de naam van de cloudservice en de regio. In Azure Portal wordt met scenario verwezen naar wat u in de Recovery Services-kluis wilt opnemen. Het beleid is de planning voor hoe vaak en wanneer er herstelpunten worden gemaakt. Het beleid bevat ook de bewaartermijn voor de herstelpunten.

1. Als er al een Recovery Services-kluis is geopend, gaat u verder met stap 2. Als u geen een Recovery Services-kluis openen, opent u de [Azure-portal](https://portal.azure.com/) en klik op het menu Hub op **meer services**.

   * Typ in de lijst met resources **Recovery Services**.
   * Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Wanneer **Recovery Services-kluizen** wordt weergegeven, klikt u erop.

     ![Een Recovery Services-kluis maken, stap 1](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

     De lijst met Recovery Services-kluizen wordt weergegeven. Als er geen kluizen in uw abonnement, wordt deze lijst niet leeg zijn.

    ![Weergave van de lijst met Recovery Services-kluizen](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

   * Selecteer in de lijst met Recovery Services-kluizen een kluis om het dashboard te openen.

     De blade instellingen en het kluisdashboard voor de gekozen kluis wordt geopend.

     ![Blade Kluis openen](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)
2. Klik in het menu van het kluisdashboard op **Back-up** om de blade Back-up te openen.

    ![Blade Back-up openen](./media/backup-azure-arm-vms-prepare/backup-button.png)

    De blades Back-up en Doel van de back-up worden geopend.

    ![Blade Scenario openen](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

3. Stel op de blade back-updoel **waar wordt uw workload uitgevoerd** naar Azure en **wat wilt u back-up maken** aan de virtuele machine, klik vervolgens op **OK**.

    Hiermee wordt de VM-extensie bij de kluis geregistreerd. De blade Doel van de back-up wordt gesloten en de blade **Back-upbeleid** wordt geopend.

    ![Blade Scenario openen](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)
4. Selecteer op de blade Back-upbeleid het back-upbeleid dat u op de kluis wilt toepassen.

    ![Back-upbeleid selecteren](./media/backup-azure-arm-vms-prepare/setting-rs-backup-policy-new.png)

    De details van het standaardbeleid worden onder de vervolgkeuzelijst weergegeven. Als u een nieuw beleid wilt maken, selecteert u **Nieuw maken** in de vervolgkeuzelijst. Zie [Een back-upbeleid definiëren](backup-azure-vms-first-look-arm.md#defining-a-backup-policy) voor instructies over het definiëren van een back-upbeleid.
    Klik op **OK** om het back-upbeleid aan de kluis te koppelen.

    De blade Back-upbeleid wordt gesloten en de blade **Virtuele machines selecteren** wordt geopend.
5. Kies op de blade **Virtuele machines selecteren** de virtuele machines die u aan het opgegeven beleid wilt koppelen en klik op **OK**.

    ![Workload selecteren](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

    De geselecteerde virtuele machine wordt gevalideerd. Als u de virtuele machines die u verwacht te zien niet ziet, controleert u dat ze bestaan in dezelfde Azure-locatie als de Recovery Services-kluis en dat u nog niet is beveiligd in een andere kluis. De locatie van de Recovery Services-kluis wordt weergegeven op het kluisdashboard.

6. Nu u alle instellingen voor de kluis hebt gedefinieerd, klikt u op de blade Back-up op **Back-up inschakelen**. Hierop wordt het beleid voor de kluis en de VM's geïmplementeerd. Hiermee wordt niet het eerste herstelpunt voor de virtuele machine gemaakt.

    ![Back-up inschakelen](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Nadat de back-up is ingeschakeld, wordt het back-upbeleid volgens het schema uitgevoerd. Als u genereren van een back-uptaak op aanvraag wilt naar de back-up van de virtuele machines nu Zie [activering van de back-uptaak](./backup-azure-arm-vms.md#triggering-the-backup-job).

Als u problemen met de registratie van de virtuele machine hebt, raadpleegt u de volgende informatie over het installeren van de VM-Agent en op de netwerkverbinding. Moet u waarschijnlijk niet de volgende informatie als u virtuele machines die zijn gemaakt in Azure beveiligt. Maar als u uw virtuele machines in Azure hebt gemigreerd, vervolgens moet u de VM-agent correct is geïnstalleerd en dat de virtuele machine met het virtuele netwerk communiceren kan.

## <a name="install-the-vm-agent-on-the-virtual-machine"></a>De VM-agent op de virtuele machine installeren
De Azure VM-agent moet worden geïnstalleerd op de virtuele Azure-machine om de Backup-extensie te kunnen gebruiken. Als uw virtuele machine is gemaakt vanuit de Azure-galerie, klikt u vervolgens is de VM-Agent al aanwezig op de virtuele machine. Deze informatie wordt verstrekt voor de situaties waarin u *niet* met behulp van een virtuele machine gemaakt op basis van de Azure-galerie - bijvoorbeeld u een virtuele machine gemigreerd vanuit een on-premises datacenter. In dat geval moet de VM-Agent worden geïnstalleerd ter bescherming van de virtuele machine. Meer informatie over de [VM-Agent](../virtual-machines/windows/classic/agents-and-extensions.md#azure-vm-agents-for-windows-and-linux).

Als u problemen ondervindt bij het maken van een back-up van de Azure VM, controleert u of de Azure VM-agent correct is geïnstalleerd op de virtuele machine (zie de onderstaande tabel). De volgende tabel bevat aanvullende informatie over de VM-agent voor Windows- en Linux-VM's.

| **Bewerking** | **Windows** | **Linux** |
| --- | --- | --- |
| De VM-agent installeren |Download en installeer de [agent-MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). U hebt Administrator-bevoegdheden nodig om de installatie te kunnen uitvoeren. |<li> Installeer de meest recente [Linux-agent](../virtual-machines/linux/agent-user-guide.md). U hebt Administrator-bevoegdheden nodig om de installatie te kunnen uitvoeren. Het is raadzaam om de installatie van agent van uw opslagplaats voor distributie. We **wordt niet aanbevolen** installeren Linux VM-agent rechtstreeks vanuit github.  |
| De VM-agent bijwerken |Om de VM-agent bij te werken hoeft u alleen de [binaire bestanden voor de VM-agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) opnieuw te installeren. <br>Zorg ervoor dat er geen back-upbewerking wordt uitgevoerd terwijl de VM-agent wordt bijgewerkt. |Volg de instructies voor het [bijwerken van de Linux VM-agent](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Het is raadzaam om de agent van uw opslagplaats distributiepunten bijwerken. We **wordt niet aanbevolen** bijwerken Linux VM-agent rechtstreeks vanuit github.<br>Zorg ervoor dat er geen back-upbewerking wordt uitgevoerd terwijl de VM-agent wordt bijgewerkt. |
| De installatie van de VM-agent valideren |<li>Ga naar de map *C:\WindowsAzure\Packages* in de Azure VM. <li>Het bestand WaAppAgent.exe moet hier aanwezig zijn.<li> Klik met de rechtermuisknop op het bestand, ga naar **Eigenschappen** en selecteer vervolgens het tabblad **Details**. In het veld Productversie moet versie 2.6.1198.718 of hoger worden weergegeven. |N.v.t. |

### <a name="backup-extension"></a>Backup-extensie
Nadat de VM-agent op de virtuele machine is geïnstalleerd, installeert de Azure Backup-service de Backup-extensie in de VM-agent. De Azure Backup-service naadloos wordt bijgewerkt en -patches van de Backup-extensie.

De Backup-extensie wordt geïnstalleerd door de Backup-service ongeacht of de VM wordt uitgevoerd of niet. Bij een actieve VM is de kans het grootst dat een toepassingsconsistent herstelpunt wordt verkregen. De Azure Backup-service blijft echter back-ups van de VM maken, zelfs als deze is uitgeschakeld en de extensie niet kan worden geïnstalleerd. Dit wordt Offline-VM genoemd. In dit geval is het herstelpunt *crashconsistent*.

## <a name="network-connectivity"></a>Netwerkverbinding
Om de VM-momentopnamen te beheren, moet de Backup-extensie verbinding hebben met de Azure openbare IP-adressen. Zonder de juiste internetverbinding time-out van de virtuele machine HTTP-aanvragen en de back-upbewerking is mislukt. Als uw implementatie beschikt over toegangsbeperkingen (via een netwerkbeveiligingsgroep (NSG), bijvoorbeeld), kies een van deze opties voor het ontwikkelen van een duidelijke pad voor de back-verkeer:

* [Geaccepteerde het Azure datacenter IP ranges](http://www.microsoft.com/en-us/download/details.aspx?id=41653) -Raadpleeg het artikel voor instructies over hoe naar geaccepteerde de IP-adressen.
* Implementeer een HTTP-proxyserver voor het routeren van verkeer.

Wanneer u beslist welke optie u moet gebruiken, zijn de verschillen tussen beheerbaarheid, gedetailleerde controle en kosten.

| Optie | Voordelen | Nadelen |
| --- | --- | --- |
| Whitelist IP-adresbereiken |Er zijn geen extra kosten.<br><br>Voor toegang tot te openen in een NSG, gebruikt u de <i>Set AzureNetworkSecurityRule</i> cmdlet. |Moeilijk te beheren als de betrokken IP-adresbereiken op den duur veranderen.<br><br>Biedt toegang tot het geheel van Azure en niet alleen de opslag. |
| HTTP-proxy |Gedetailleerd beheer in de proxy van de opslag URL's toegestaan.<br>Één punt Internet toegang tot virtuele machines.<br>Ten aanzien van wijzigingen in de Azure-IP-adres. |Extra kosten voor het uitvoeren van een virtuele machine met de proxysoftware. |

### <a name="whitelist-the-azure-datacenter-ip-ranges"></a>De Azure-datacenter geaccepteerde IP-adresbereiken
* Aan de lijst met geaccepteerde het Azure datacenter IP-adresbereiken, raadpleegt u de [Azure-website](http://www.microsoft.com/en-us/download/details.aspx?id=41653) voor meer informatie over de IP-adresbereiken en instructies.
* U kunt service codes gebruiken om toe te staan van verbindingen met de opslag van het gebruik van de specifieke regio [Service-Tags](../virtual-network/security-overview.md#service-tags). Zorg ervoor dat voor die regel waarmee toegang tot opslagaccount heeft een hogere prioriteit dan de regel voor het blokkeren van toegang tot internet. 

  ![NSG met labels van de opslag voor een regio](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

> [!WARNING]
> Opslag-tags zijn alleen beschikbaar in specifieke gebieden en zijn Preview-versie. Raadpleeg voor een lijst met regio's [Service-tags voor opslag](../virtual-network/security-overview.md#service-tags)

### <a name="using-an-http-proxy-for-vm-backups"></a>Met behulp van een HTTP-proxy voor VM-back-ups
Wanneer een back-up van een VM, verzendt de Backup-extensie op de virtuele machine opdrachten voor het beheer van de momentopname naar Azure Storage met behulp van een HTTPS-API. De Backup-extensie-verkeer via de HTTP-proxy routeren omdat dit het enige onderdeel dat is geconfigureerd voor toegang tot het openbare Internet.

> [!NOTE]
> Er is geen aanbeveling voor de proxysoftware die moet worden gebruikt. Zorg ervoor dat u kiest een proxy die compatibel is met de onderstaande configuratiestappen.
>
>

In onderstaande voorbeeldafbeelding ziet de drie configuratiestappen die nodig zijn voor het gebruik van een HTTP-proxy:

* App VM routeert alle HTTP-verkeer voor het openbare Internet via Proxy VM is gebonden.
* Proxy VM zorgt ervoor dat binnenkomend verkeer van virtuele machines in het virtuele netwerk.
* De Netwerkbeveiligingsgroep (NSG) met de naam NSF-lockdown moet een beveiliging regel waardoor uitgaand internetverkeer van Proxy VM.

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
2. In het dialoogvenster Windows Firewall met de rechtermuisknop op **regels voor binnenkomende verbindingen** en klik op **nieuwe regel...** .
3. In de **nieuwe Wizard regel voor binnenkomende**, kies de **aangepaste** optie voor de **regeltype** en klik op **volgende**.
4. Op de pagina selecteren de **programma**, kies **alle programma's** en klik op **volgende**.
5. Op de **protocollen en poorten** pagina, voer de volgende informatie en klik op **volgende**:

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


*Deze stappen worden specifieke namen en waarden voor dit voorbeeld gebruiken. Gebruik de namen en waarden voor uw implementatie wanneer u invoert, of knippen en plakken van gegevens in uw code.*

Nu u weet dat u verbinding met het netwerk hebt, bent u gereed voor back-up van uw virtuele machine. Zie [Back-up van Resource Manager geïmplementeerde VM's](backup-azure-arm-vms.md).

## <a name="questions"></a>Vragen?
Als u vragen hebt of als er een functie is die u graag opgenomen ziet worden, [stuur ons dan uw feedback](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Volgende stappen
Nu dat u uw omgeving voor back-ups van uw virtuele machine hebt voorbereid, wordt de volgende logische stap is het maken van een back-up. De planning artikel vindt meer gedetailleerde informatie over back-ups van virtuele machines.

* [Back-up van virtuele machines](backup-azure-arm-vms.md)
* [Uw VM-back-infrastructuur plannen](backup-azure-vms-introduction.md)
* [Back-ups van virtuele machine beheren](backup-azure-manage-vms.md)
