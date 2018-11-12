---
title: 'Azure Backup: Voorbereiden op back-up van virtuele machines'
description: Zorg ervoor dat uw omgeving is voorbereid voor de back-ups van virtuele machines in Azure.
services: backup
author: rayne-wiselman
manager: carmonm
keywords: back-ups; back-ups van;
ms.service: backup
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: raynew
ms.openlocfilehash: 6de0d29895a6d12d3a5aa761c0c4c5148f62dd81
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51256269"
---
# <a name="prepare-to-back-up-azure-vms"></a>Voorbereiden op back-up van virtuele Azure-machines

In dit artikel bevat de stappen voor het voorbereiden van uw omgeving voor de back-up van een Azure Resource Manager geïmplementeerde virtuele machine (VM). De stappen die worden weergegeven in de procedures gebruikt de Azure portal. Wanneer u back-up van een virtuele machine, worden de back-upgegevens of herstelpunten, opgeslagen in een Recovery Services back-upkluis.



Voordat u beveiligen (of een back-up) een Resource Manager geïmplementeerde virtuele machine, zorg ervoor dat deze vereisten zijn:

* Maak of Identificeer een Recovery Services-kluis *in dezelfde regio als uw virtuele machine*.
* Selecteert u een scenario, de back-upbeleid definiëren en definiëren welke items moeten beveiligen.
* Controleer de installatie van een VM-agent (extensie) op de virtuele machine.
* Controleer de netwerkverbinding.
* Voor virtuele Linux-machines als u uw back-omgeving voor toepassingsconsistente back-ups aanpassen wilt, volgt u de [stappen voor het configureren van scripts voorafgaat aan momentopnamen en volgt op momentopnamen](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).

Als deze voorwaarden al in uw omgeving bestaat, gaat u verder met de [maakt u een Back-up van uw virtuele machines](backup-azure-arm-vms.md) artikel. Als u wilt instellen of een van deze vereisten, begeleidt in dit artikel u de stappen.

## <a name="supported-operating-systems-for-backup"></a>Ondersteunde besturingssystemen voor back-up

 * **Linux**: Azure Backup biedt ondersteuning voor [een lijst met distributies die Azure ondersteunt](../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), met uitzondering van CoreOS Linux. Zie voor de lijst met Linux-besturingssystemen die ondersteuning voor het herstellen van bestanden, [bestanden herstellen vanuit back-up van virtuele machine](backup-azure-restore-files-from-vm.md#for-linux-os).

    > [!NOTE]
    > Andere bring-your-own-Linux-distributies mogelijk werken, als de VM-agent beschikbaar op de virtuele machine is en ondersteuning voor Python aanwezig. Deze distributies worden echter niet ondersteund.
    >
 * **Windows Server**, **Windows client**: versies ouder dan Windows Server 2008 R2 of Windows 7, worden niet ondersteund.


## <a name="limitations-when-backing-up-and-restoring-a-vm"></a>Beperkingen bij het back-up en herstellen van een virtuele machine
Voordat u uw omgeving hebt voorbereid, moet u deze beperkingen begrijpt:

* Back-ups van virtuele machines met meer dan 16 gegevensschijven wordt niet ondersteund.
* Back-ups van virtuele Linux-machines versleuteld door middel van Linux Unified sleutel instellen (LUKS)-codering wordt niet ondersteund.
* Wordt niet aanbevolen back-ups van virtuele machines met gedeelde clustervolumes (CSV) of Scale-Out bestandsserver. Als u klaar bent, wordt mislukken van CSV-schrijvers verwacht. Ze nodig hebben met betrekking tot alle virtuele machines die zijn opgenomen in de configuratie van het cluster tijdens de taak van een momentopname. Azure Backup biedt geen ondersteuning voor meerdere VM's.
* Back-upgegevens bevat geen gekoppeld netwerkstations die zijn gekoppeld aan een virtuele machine.
* Een bestaande virtuele machine kan tijdens het herstel niet worden vervangen. Als u probeert te herstellen van de virtuele machine wanneer de virtuele machine bestaat, mislukt de herstelbewerking opnieuw.
* Regio-overschrijdende back-up en herstel worden niet ondersteund.
* Tijdens het configureren van back up, zorg ervoor dat de **Firewalls en virtuele netwerken** opslagaccountinstellingen zodat toegang vanaf alle netwerken.
* Voor de geselecteerde netwerken, na het configureren van de firewall en virtuele-netwerkinstellingen voor uw opslagaccount, selecteer **vertrouwde Microsoft-services voor toegang tot dit storage-account toestaan** als uitzondering aan Azure Backup-service inschakelen toegang tot het netwerk beperkte storage-account. Herstel op itemniveau wordt niet ondersteund voor het netwerk beperkte opslagaccounts.
* U kunt back-up van virtuele machines in alle openbare regio's van Azure. (Zie de [controlelijst](https://azure.microsoft.com/regions/#services) van ondersteunde regio's.) Als de regio die u zoekt momenteel niet ondersteund wordt, wordt deze niet in de vervolgkeuzelijst weergegeven tijdens het maken van de kluis.
* Herstellen van een domeincontroller wordt (DC) virtuele machine die deel uitmaakt van een multi-DC-configuratie ondersteund alleen via PowerShell. Zie voor meer informatie, [een multi-DC-domeincontroller terugzetten](backup-azure-arm-restore-vms.md#restore-domain-controller-vms).
* Momentopname op de schijf Write Accelerator is ingeschakeld, wordt niet ondersteund. Deze beperking blokkeert Azure Backup-service de mogelijkheid om uit te voeren een toepassingsconsistente momentopname van alle schijven van de virtuele machine.
* Herstellen van virtuele machines waarvoor de volgende speciale netwerkconfiguraties wordt alleen via PowerShell ondersteund. Virtuele machines die zijn gemaakt via de werkstroom terugzetten in de gebruikersinterface wordt geen van deze configuraties van het netwerk nadat de herstelbewerking voltooid is. Zie voor meer informatie, [virtuele machines herstellen met speciale netwerkconfiguraties](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations).
  * Virtuele machines in onder de load balancer-configuratie (intern en extern)
  * Virtuele machines met meerdere gereserveerde IP-adressen
  * Virtuele machines met meerdere netwerkadapters

  > [!NOTE]
  > Azure Backup ondersteunt [Standard-SSD Managed Disks](https://azure.microsoft.com/blog/announcing-general-availability-of-standard-ssd-disks-for-azure-virtual-machine-workloads/), een nieuw type duurzame opslag voor Microsoft Azure virtuele machines. Dit wordt ondersteund voor beheerde schijven in [Azure VM Backup-stack V2](backup-upgrade-to-vm-backup-stack-v2.md).

## <a name="create-a-recovery-services-vault-for-a-vm"></a>Een Recovery Services-kluis voor een VM maken
Een Recovery Services-kluis is een entiteit die de back-ups en herstelpunten die zijn gemaakt na verloop van tijd worden opgeslagen. De Recovery Services-kluis bevat ook het back-upbeleid die gekoppeld aan de beveiligde virtuele machines zijn.

Een Recovery Services-kluis maken:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
1. Op de **Hub** in het menu **Bladeren**, en typ vervolgens **herstelservices**. Als u te typen begint, filtert de invoer van de lijst met resources. Selecteer **Recovery Services-kluizen**.

    ![In het vak te typen en "Recovery Services-kluizen" in de resultaten te selecteren](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

    De lijst met Recovery Services-kluizen wordt weergegeven.
1. Op de **Recovery Services-kluizen** in het menu **toevoegen**.

    ![Een Recovery Services-kluis maken, stap 2](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

    De **Recovery Services-kluizen** deelvenster wordt geopend. Deze gevraagd om informatie voor **naam**, **abonnement**, **resourcegroep**, en **locatie**.

    ![Deelvenster "Recovery Services-kluizen"](./media/backup-azure-arm-vms-prepare/rs-vault-attributes.png)
1. Voer bij **Naam** een beschrijvende naam in om de kluis aan te duiden. De naam moet uniek zijn voor het Azure-abonnement. Typ een naam die 2 en 50 tekens bevat. Deze moet beginnen met een letter en mag alleen letters, cijfers en afbreekstreepjes bevatten.
1. Selecteer **abonnement** om te zien van de lijst met beschikbare abonnementen. Als u niet zeker weet welk abonnement u moet gebruiken, gebruikt u de standaard (of voorgestelde) abonnement. Er zijn meerdere opties alleen als uw werk of school-account is gekoppeld aan meerdere Azure-abonnementen.
1. Selecteer **resourcegroep** om te zien van de beschikbare lijst met resourcegroepen, of selecteer **nieuw** om een nieuwe resourcegroep te maken. Zie voor meer informatie over resourcegroepen [overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
1. Selecteer **locatie** om te selecteren van de geografische regio voor de kluis. De kluis *moet* zich in dezelfde regio bevinden als de virtuele machines die u wilt beveiligen.

   > [!IMPORTANT]
   > Als u niet zeker weet van de locatie waar uw VM zich bevindt, sluit het dialoogvenster voor het maken van kluis en gaat u naar de lijst met virtuele machines in de portal. Als u virtuele machines in meerdere regio's hebt, moet u een Recovery Services-kluis in elke regio maken. Maak de kluis op de eerste locatie voordat u verdergaat met de volgende locatie. Er is niet nodig om op te geven van de storage-accounts voor het opslaan van de back-upgegevens. De Recovery Services-kluis en de Azure Backup-service worden verwerkt die automatisch.
   >
   >

1. Selecteer **Maken**. Het kan even duren voordat de Recovery Services-kluis is gemaakt. Houd de statusmeldingen in de rechterbovenhoek van de portal. Nadat de kluis is gemaakt, wordt deze weergegeven in de lijst met Recovery Services-kluizen. Als u uw kluis niet ziet, selecteert u **vernieuwen**.

    ![Lijst met back-upkluizen](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

Nu u uw kluis hebt gemaakt, leert u hoe u de opslagreplicatie instelt.

## <a name="set-storage-replication"></a>Opslagreplicatie instellen
De optie voor opslagreplicatie kunt u kiezen tussen geografisch redundante opslag en lokaal redundante opslag. Uw kluis heeft standaard geografisch redundante opslag. Laat de instelling van de optie als geografisch redundante opslag voor uw primaire back-up. Als u een goedkopere optie die duurzaam wilt, kies lokaal redundante opslag.

De instelling voor opslagreplicatie bewerken:

1. Op de **Recovery Services-kluizen** deelvenster, selecteert u de kluis.
    Wanneer u uw kluis selecteert de **instellingen** deelvenster (met de naam van de kluis aan het begin) en het detailvenster kluis openen.

   ![Kies uw kluis uit de lijst met back-upkluizen](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)

1. Op de **instellingen** deelvenster, het gebruik van de verticale schuifregelaar te Schuif omlaag naar de **beheren** uit en selecteer **back-upinfrastructuur**. In de **algemene** sectie, selecteer **back-upconfiguratie**. Op de **back-upconfiguratie** deelvenster, kiest u de optie voor opslagreplicatie voor uw kluis. Uw kluis heeft standaard geografisch redundante opslag.

   ![Lijst met back-upkluizen](./media/backup-azure-arm-vms-prepare/full-blade.png)

   Als u Azure als een primaire back-upopslag-eindpunt gebruikt, blijven geografisch redundante opslag gebruiken. Als u Azure gebruikt als een niet-primaire back-upopslag-eindpunt, kies lokaal redundante opslag. Meer informatie over opties voor opslag in de [overzicht van Azure Storage-replicatie](../storage/common/storage-redundancy.md).

1. Als u het opslagtype voor de replicatie hebt gewijzigd, selecteert u **opslaan**.

Nadat u de opslagoptie voor uw kluis hebt gekozen, u kunt de virtuele machine koppelen aan de kluis. Voordat u de VM aan de kluis koppelt, moet u eerst de virtuele Azure-machines detecteren en registreren.

## <a name="select-a-backup-goal-set-policy-and-define-items-to-protect"></a>Een back-doel selecteren, beleid instellen en definiëren welke items moeten beveiligen
Voordat u een virtuele machine met een Recovery Services-kluis registreert, moet u het detectieproces voor het identificeren van nieuwe virtuele machines toegevoegd aan het abonnement uitvoeren. Tijdens de detectie zoekt in Azure naar de lijst met virtuele machines in het abonnement. Als nieuwe virtuele machines worden gevonden, wordt de portal geeft de naam van de cloud-service en de gekoppelde regio. In de Azure-portal, de *scenario* is wat u in de Recovery Services-kluis. *Beleid* is de planning voor hoe vaak en wanneer er herstelpunten worden gemaakt. Het beleid bevat ook de bewaartermijn voor de herstelpunten.

1. Als er al een Recovery Services-kluis is geopend, gaat u verder met stap 2. Als u geen Recovery Services-kluis openen, opent u de [Azure-portal](https://portal.azure.com/). Op de **Hub** in het menu **meer services**.

   a. Typ in de lijst met resources **Recovery Services**. Als u te typen begint, wordt de lijst gefilterd door uw invoer. Wanneer de melding **Recovery Services-kluizen**, selecteert u deze.

      ![In het vak te typen en "Recovery Services-kluizen" in de resultaten te selecteren](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

      De lijst met Recovery Services-kluizen wordt weergegeven. Als er geen kluizen in uw abonnement, wordt deze lijst leeg is.

      ![Weergave van de lijst met Recovery Services-kluizen](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

   b. Selecteer in de lijst met Recovery Services-kluizen een kluis.

      De **instellingen** deelvenster en het dashboard van de kluis voor de door u gekozen kluis openen.

      ![Instellingen voor deelvenster en de kluis dashboard](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)
1. Selecteer op de kluis in het menu **back-up**.

   ![Back-knop](./media/backup-azure-arm-vms-prepare/backup-button.png)

   De **back-up** en **back-updoel** deelvensters openen.

1. Op de **back-doel** instellen in het deelvenster **waar wordt uw werkbelasting uitgevoerd?** als **Azure** en **waarvan wilt u back-up maken?** als  **Virtuele machine**. Selecteer vervolgens **OK**.

   ![Deelvensters van de back-ups en back-updoel](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

   Deze stap wordt de VM-extensie bij de kluis geregistreerd. De **back-updoel** deelvenster wordt gesloten, en de **back-upbeleid** deelvenster wordt geopend.

   !["Backup" en "Back-upbeleid" deelvensters](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)
1. Op de **back-upbeleid** deelvenster, selecteert u de back-upbeleid dat u wilt toepassen op de kluis.

   ![Back-upbeleid selecteren](./media/backup-azure-arm-vms-prepare/setting-rs-backup-policy-new.png)

   De details van het standaardbeleid worden onder de vervolgkeuzelijst weergegeven. Als u een nieuw beleid wilt maken, selecteert u **Nieuw maken** in de vervolgkeuzelijst. Zie [Een back-upbeleid definiëren](backup-azure-vms-first-look-arm.md#defining-a-backup-policy) voor instructies over het definiëren van een back-upbeleid.
    Selecteer **OK** het back-upbeleid koppelen aan de kluis.

   De **back-upbeleid** deelvenster wordt gesloten, en de **virtuele machines selecteren** deelvenster wordt geopend.
1. Op de **virtuele machines selecteren** deelvenster, kiest u de virtuele machines om te koppelen aan het opgegeven beleid en selecteer **OK**.

   ![Deelvenster 'Virtuele machines selecteren'](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

   De geselecteerde virtuele machine wordt gevalideerd. Als u de verwachte virtuele machines niet ziet, controleert u dat de virtuele machines zich in dezelfde Azure-regio als de Recovery Services-kluis. Als u de virtuele machines nog steeds niet ziet, controleert u of ze nog niet zijn beveiligd met een andere kluis. Het dashboard van de kluis wordt aangegeven in de regio waar de Recovery Services-kluis bestaat.

1. Nu dat u alle instellingen voor de kluis zijn gedefinieerd op de **back-up** venster **back-up inschakelen**. Deze stap wordt het beleid geïmplementeerd naar de kluis en de virtuele machines. Deze stap is het eerste herstelpunt voor de virtuele machine niet maken.

   ![Knop 'Back-up inschakelen'](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Nadat u de back-up is ingeschakeld, wordt uw back-upbeleid volgens schema uitgevoerd. Als u genereren van een back-uptaak op aanvraag wilt naar de back-up van de virtuele machines ziet nu, [activeren van de back-uptaak](./backup-azure-vms-first-look-arm.md#initial-backup).

Als u problemen met de registratie van de virtuele machine hebt, raadpleegt u de volgende informatie over het installeren van de VM-agent en de netwerkverbinding. Moet u waarschijnlijk niet de volgende informatie als u virtuele machines die zijn gemaakt in Azure beveiligt. Maar als u uw virtuele machines hebt gemigreerd naar Azure, zorg ervoor dat u de VM-agent correct geïnstalleerd en dat uw virtuele machine met het virtuele netwerk communiceren kan.

## <a name="install-the-vm-agent-on-the-virtual-machine"></a>De VM-agent installeren op de virtuele machine
Voor de Backup-extensie om te werken, de Azure [VM-agent](../virtual-machines/extensions/agent-windows.md) moet worden geïnstalleerd op de virtuele machine van Azure. Als uw virtuele machine is gemaakt vanuit de Azure Marketplace, is de VM-agent al aanwezig op de virtuele machine.

De volgende informatie is bedoeld voor situaties waarin u *niet* met behulp van een virtuele machine gemaakt op basis van de Azure Marketplace. **U kunt bijvoorbeeld een virtuele machine gemigreerd uit een on-premises datacenter. In dat geval moet de VM-agent worden geïnstalleerd om de virtuele machine te beschermen.**

**Houd er rekening mee**: nadat de VM-agent is geïnstalleerd, moet u ook Azure PowerShell gebruiken om bij te werken van de eigenschap ProvisionGuestAgent zodat Azure de virtuele machine heeft de agent is geïnstalleerd.

Als u problemen met back-ups van de Azure-VM hebt, gebruikt u de volgende tabel om te controleren of de Azure VM-agent correct is geïnstalleerd op de virtuele machine. De tabel bevat aanvullende informatie over de VM-agent voor Windows en Linux-machines.

| **Bewerking** | **Windows** | **Linux** |
| --- | --- | --- |
| De VM-agent installeren |Download en installeer de [agent-MSI](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). U hebt Administrator-bevoegdheden nodig om de installatie te kunnen uitvoeren. |<li> Installeer de meest recente [Linux-agent](../virtual-machines/extensions/agent-linux.md). U hebt Administrator-bevoegdheden nodig om de installatie te kunnen uitvoeren. U wordt aangeraden de installatie van agent vanuit de opslagplaats van uw distributie. We **wordt niet aanbevolen** installeren Linux VM-agent rechtstreeks vanuit github.  |
| De VM-agent bijwerken |Om de VM-agent bij te werken hoeft u alleen de [binaire bestanden voor de VM-agent](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) opnieuw te installeren. <br>Zorg ervoor dat er geen back-upbewerking wordt uitgevoerd terwijl de VM-agent wordt bijgewerkt. |Volg de instructies voor het [bijwerken van de Linux VM-agent](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Het is raadzaam om de agent vanuit de opslagplaats van uw distributie bijwerken. We **wordt niet aanbevolen** bijwerken Linux VM-agent rechtstreeks vanuit github.<br>Zorg ervoor dat er geen back-upbewerking wordt uitgevoerd terwijl de VM-agent wordt bijgewerkt. |
| De installatie van de VM-agent valideren |<li>Ga naar de map *C:\WindowsAzure\Packages* in de Azure VM. <li>Het bestand WaAppAgent.exe moet hier aanwezig zijn.<li> Klik met de rechtermuisknop op het bestand, ga naar **Eigenschappen** en selecteer vervolgens het tabblad **Details**. In het veld Productversie moet versie 2.6.1198.718 of hoger worden weergegeven. |N/A |

### <a name="backup-extension"></a>Backup-extensie
Nadat de VM-agent is geïnstalleerd op de virtuele machine, wordt in de Azure Backup-service de Backup-extensie in de VM-agent geïnstalleerd. De Backup-service wordt naadloos upgrades en patches van de Backup-extensie.

De Backup-service installeert de back-upextensie, ongeacht of de virtuele machine wordt uitgevoerd of niet. Bij een actieve VM is de kans het grootst dat een toepassingsconsistent herstelpunt wordt verkregen. De Backup-service blijft echter back-up van de virtuele machine, zelfs als deze is uitgeschakeld en de extensie niet kan worden geïnstalleerd. Dit staat bekend als *offline VM*. In dit geval is het herstelpunt *crashconsistent*.

## <a name="establish-network-connectivity"></a>De netwerkverbinding tot stand brengen
Voor het beheren van de momentopnamen van de VM, moet de back-upextensie verbinding met de Azure openbare IP-adressen. Time-out van de virtuele machine HTTP-aanvragen zonder de juiste verbinding met internet, en de back-upbewerking is mislukt. Als uw implementatie heeft toegangsbeperkingen erin--via een netwerkbeveiligingsgroep (NSG), bijvoorbeeld: Kies een van deze opties voor een duidelijk pad voor back-upverkeer:

* [De Azure-datacenter-IP-bereiken van geaccepteerde](https://www.microsoft.com/download/details.aspx?id=41653).
* Implementeer een HTTP-proxy-server voor het routeren van verkeer.

Bij het bepalen van welke optie voor het gebruik, zijn de wisselwerking tussen beheerbaarheid, gedetailleerde controle en kosten.

| Optie | Voordelen | Nadelen |
| --- | --- | --- |
| Whitelist IP-bereiken |Er zijn geen extra kosten.<br><br>Voor toegang in een NSG te openen, gebruikt u de **Set AzureNetworkSecurityRule** cmdlet. |Moeilijk te beheren als de betrokken IP-adresbereiken verloop van tijd veranderen.<br><br>Biedt toegang tot het geheel van Azure en niet alleen opslag. |
| Gebruik een HTTP-proxy |Nauwkeurige controle in de proxy die over de opslag URL's is toegestaan.<br><br>Één punt van internet toegang tot VM's.<br><br>Niet onderhevig aan Azure-IP-adres verandert. |Extra kosten voor het uitvoeren van een virtuele machine met de proxysoftware. |

### <a name="whitelist-the-azure-datacenter-ip-ranges"></a>Het Azure-datacenter geaccepteerde IP-bereiken
Aan lijst met geaccepteerde IP-bereiken met de Azure-datacenter, Zie de [Azure-website](https://www.microsoft.com/download/details.aspx?id=41653) voor meer informatie over de IP-bereiken en de instructies.

U kunt verbindingen naar de opslag van de specifieke regio met behulp van [servicetags](../virtual-network/security-overview.md#service-tags). Zorg ervoor dat de regel waarmee toegang tot het opslagaccount hogere prioriteit dan de regel die Hiermee blokkeert u toegang tot internet heeft.

![NSG met opslag-tags voor een regio](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

De volgende video leidt u door de stapsgewijze procedure servicetags configureren:

>[!VIDEO https://www.youtube.com/embed/1EjLQtbKm1M]

> [!NOTE]
> Zie voor een lijst van storage service-tags en regio's, [servicetags voor opslag](../virtual-network/security-overview.md#service-tags).

### <a name="use-an-http-proxy-for-vm-backups"></a>Een HTTP-proxy gebruiken voor back-ups van virtuele machine
Wanneer u een back-up van een virtuele machine, verzendt de Backup-extensie op de virtuele machine de opdrachten voor het beheer van momentopname naar Azure Storage met behulp van een HTTPS-API. Routeren het verkeer Backup-extensie de HTTP-proxy, omdat dit het enige onderdeel dat is geconfigureerd voor toegang tot het openbare internet.

> [!NOTE]
> Wordt niet aanbevolen specifieke proxysoftware die u moet gebruiken. Zorg ervoor dat u kiest een proxy die compatibel is met de configuratiestappen die volgen.
>
>

Het volgende van de voorbeeldafbeelding toont de drie configuratiestappen die nodig zijn voor het gebruik van een HTTP-proxy:

* De routes van de virtuele machine app afhankelijk van alle HTTP-verkeer voor het openbare internet via de proxy-VM.
* De VM-proxy kunt inkomend verkeer van virtuele machines in het virtuele netwerk.
* De netwerkbeveiligingsgroep met de naam van NSF-vergrendeling moet een regel waarmee het uitgaande internetverkeer van de proxy-VM.

Voor het gebruik van een HTTP-proxy om te communiceren met het openbare internet, moet u de volgende stappen uitvoeren.

> [!NOTE]
> Deze stappen wordt gebruik van specifieke namen en waarden voor dit voorbeeld. Wanneer u invoeren (of plakken) gegevens in uw code, gebruikt u de namen en waarden voor uw implementatie.

#### <a name="step-1-configure-outgoing-network-connections"></a>Stap 1: Configureren van uitgaande netwerkverbindingen
###### <a name="for-windows-machines"></a>Voor Windows-machines
Deze procedure stelt u de proxyserver-configuratie voor het lokale systeemaccount gebruikt.

1. Download [PsExec](https://technet.microsoft.com/sysinternals/bb897553).
1. Open Internet Explorer met de volgende opdracht uit vanaf een opdrachtprompt:

    ```
    psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe"
    ```

1. Ga in Internet Explorer naar **extra** > **Internetopties** > **verbindingen** > **LAN-instellingen**.
1. Controleer of de proxy-instellingen voor het systeem-account. Stel de proxy-IP en poort.
1. Sluit Internet Explorer.

Het volgende script stelt u een brede, door het machine-proxyconfiguratie en gebruikt deze voor elke uitgaande HTTP of HTTPS-verkeer. Als u een proxyserver voor een huidige gebruikersaccount (niet een lokale systeemaccount) hebt ingesteld, moet u dit script gebruiken voor het toepassen op SYSTEMACCOUNT.

```
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver
```

> [!NOTE]
> Als u '(407) proxyverificatie is vereist' in het logboek voor de proxy-server zien, controleert u of de verificatie juist is ingesteld.
>
>

###### <a name="for-linux-machines"></a>Voor Linux-machines
Voeg de volgende regel aan de ```/etc/environment``` bestand:

```
http_proxy=http://<proxy IP>:<proxy port>
```

Voeg de volgende regels aan de ```/etc/waagent.conf``` bestand:

```
HttpProxy.Host=<proxy IP>
HttpProxy.Port=<proxy port>
```

#### <a name="step-2-allow-incoming-connections-on-the-proxy-server"></a>Stap 2: Binnenkomende verbindingen worden toegestaan op de proxyserver
1. Open Windows Firewall op de proxyserver. De eenvoudigste manier om toegang tot de firewall is om te zoeken naar **Windows Firewall met geavanceerde beveiliging**.
1. In de **Windows Firewall met geavanceerde beveiliging** in het dialoogvenster met de rechtermuisknop op **regels voor binnenkomende verbindingen** en selecteer **nieuwe regel**.
1. In de nieuwe Wizard inkomende-regel op de **regeltype** weergeeft, schakelt de **aangepaste** optie en selecteer **volgende**.
1. Op de **programma** weergeeft, schakelt **alle programma's** en selecteer **volgende**.
1. Op de **protocollen en poorten** pagina, voer de volgende gegevens in en selecteer **volgende**:
   * Voor **protocoltype**, selecteer **TCP**.
   * Voor **lokale poort**, selecteer **specifieke poorten**. Geef in het onderstaande vak het aantal van de proxyserver-poort die is geconfigureerd.
   * Voor **externe poort**, selecteer **alle poorten**.

Voor de rest van de wizard, accepteer de standaardinstellingen totdat u aan het einde. Vervolgens geeft deze regel een naam.

#### <a name="step-3-add-an-exception-rule-to-the-nsg"></a>Stap 3: Een uitzonderingsregel toevoegen aan de NSG
De volgende opdracht voegt een uitzondering aan de NSG. Deze uitzondering kan TCP-verkeer van een willekeurige poort op 10.0.0.5 naar een internetadres op poort 80 (HTTP) of 443 (HTTPS). Als u nodig hebt voor een specifieke poort op het openbare internet, moet u die poort toevoegen ```-DestinationPortRange```.

In een Azure PowerShell-opdrachtprompt en voer de volgende opdracht:

```
Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
```

## <a name="questions"></a>Vragen?
Als u vragen hebt of als er een functie die u wilt opnemen, Zie [Stuur ons feedback](https://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Volgende stappen
Nu dat u uw omgeving voor back-ups van uw virtuele machine hebt voorbereid, wordt de volgende logische stap is het maken van een back-up. De planning artikel bevat meer gedetailleerde informatie over back-ups van virtuele machines.

* [Back-up maken van virtuele machines](backup-azure-arm-vms.md)
* [Uw VM-back-upinfrastructuur plannen](backup-azure-vms-introduction.md)
* [Back-ups van virtuele machine beheren](backup-azure-manage-vms.md)
