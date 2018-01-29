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
ms.date: 1/21/2017
ms.author: markgal;trinadhk;sogup;
ms.openlocfilehash: 568509eba47facfc5966d06dff5a1b32dce1008f
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="prepare-your-environment-to-back-up-resource-manager-deployed-virtual-machines"></a>Uw omgeving voorbereiden op door Resource Manager geïmplementeerde virtuele machines

In dit artikel bevat de stappen voor het voorbereiden van uw omgeving tot back-up van een Azure Resource Manager geïmplementeerde virtuele machine (VM). De stappen in de procedures gebruikt u de Azure-portal.  

De Azure Backup-service heeft twee soorten kluizen voor het beveiligen van uw virtuele machines: back-upkluizen en de Recovery Services-kluizen. Een back-upkluis beveiligt virtuele machines die worden geïmplementeerd via het klassieke implementatiemodel. Een Recovery Services-kluis beschermt *zowel klassieke geïmplementeerd als Resource Manager geïmplementeerde VM's*. Als u wilt een Resource Manager geïmplementeerde VM beveiligen, moet u een Recovery Services-kluis gebruiken.

> [!NOTE]
> Azure heeft twee implementatiemodellen voor het maken en werken met resources: [Resource Manager en classic](../azure-resource-manager/resource-manager-deployment-model.md).

Voordat u kunt beveiligen of back-up van een Resource Manager geïmplementeerde virtuele machine, zorg er dan voor dat deze vereisten bestaan:

* Een Recovery Services-kluis maken (of een bestaande Recovery Services-kluis identificeren) *in dezelfde locatie als uw VM*.
* Selecteer een scenario, het back-upbeleid definiëren en definiëren van items moeten worden beveiligd.
* Controleer de installatie van een VM-agent op de virtuele machine.
* Controleer de netwerkverbinding.
* Voor virtuele Linux-machines, als u uw back-omgeving voor toepassingsconsistente back-ups aanpassen wilt, volgt u de [stappen voor het configureren van scripts die voorafgaat aan momentopnamen en volgt op momentopnamen](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).

Als deze voorwaarden al in uw omgeving bestaat, gaat u verder met de [Back-up van uw virtuele machines](backup-azure-arm-vms.md) artikel. Als u wilt instellen of een van deze vereisten te controleren, begeleidt in dit artikel u de stappen.

## <a name="supported-operating-systems-for-backup"></a>Ondersteunde besturingssystemen voor back-up
 * **Linux**: Azure Backup ondersteunt [een lijst met distributies die Azure onderschrijft](../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), behalve CoreOS Linux. 
 
    > [!NOTE] 
    > Andere bring-your-eigenaar-Linux-distributies mogelijk werkt, zolang de VM-agent beschikbaar op de virtuele machine is en ondersteuning voor Python bestaat. We tekent echter niet de distributies voor back-up.
 * **Windows Server**: versies ouder dan Windows Server 2008 R2 worden niet ondersteund.

## <a name="limitations-when-backing-up-and-restoring-a-vm"></a>Beperkingen bij een back-up en herstellen van een virtuele machine
Voordat u uw omgeving hebt voorbereid, zorg er dan voor dat deze beperkingen begrijpen:

* Back-ups van virtuele machines met meer dan 16 gegevensschijven wordt niet ondersteund.
* Back-ups van virtuele machines met gegevens groter zijn dan 1023 GB schijfgrootten wordt niet ondersteund.

  > [!NOTE]
  > We hebben een private preview ter ondersteuning van back-ups voor virtuele machines met schijven > 1TB. Raadpleeg voor meer informatie, [Private preview voor back-ondersteuning voor grote schijven VM](https://gallery.technet.microsoft.com/Instant-recovery-point-and-25fe398a).
  >

* Back-ups van virtuele machines met een gereserveerd IP-adres en er is geen gedefinieerde eindpunt wordt niet ondersteund.
* Back-ups van virtuele machines die zijn gecodeerd met alleen een BitLocker-versleutelingssleutel (BEK) wordt niet ondersteund. Back-ups van virtuele Linux-machines versleuteld door middel van Linux Unified sleutel Setup (LUKS) codering wordt niet ondersteund.
* Back-ups van virtuele machines met gedeelde clustervolumes (CSV) of Scale-Out File Server-configuratie aanbevolen niet. Ze hebben met betrekking tot alle virtuele machines die zijn opgenomen in de configuratie tijdens de taak van een momentopname van het cluster nodig. Azure Backup biedt geen ondersteuning voor consistentie tussen meerdere VM's. 
* Back-upgegevens bevat geen gekoppeld netwerkstations gekoppeld aan een virtuele machine.
* Een bestaande virtuele machine kan tijdens het herstel niet worden vervangen. Als u probeert te herstellen van de virtuele machine wanneer de virtuele machine bestaat, mislukt de herstelbewerking opnieuw.
* Regio-overschrijdende back-up en herstel worden niet ondersteund.
* Back-up en herstel van virtuele Machines met niet-beheerde schijven in de storage-accounts met netwerk regels toegepast wordt momenteel niet ondersteund. Tijdens het configureren van back-up, zorg ervoor dat de 'Firewalls en virtuele netwerken' instellingen voor het opslagaccount toegang toestaan via "Alle netwerken."
* U kunt back-ups van virtuele machines in alle openbare gebieden van Azure. (Zie de [controlelijst](https://azure.microsoft.com/regions/#services) van ondersteunde regio's.) Als de regio die u zoekt niet vandaag ondersteund wordt, wordt deze niet in de vervolgkeuzelijst weergegeven tijdens het maken van de kluis.
* Herstellen van een domeincontroller wordt (DC) VM die deel uitmaakt van een multi-DC-configuratie alleen ondersteund door PowerShell. Zie voor meer informatie, [een multi-DC-domeincontroller terugzetten](backup-azure-arm-restore-vms.md#restore-domain-controller-vms).
* Herstellen van virtuele machines waarvoor de volgende speciale netwerkconfiguraties wordt alleen ondersteund door PowerShell. Virtuele machines die zijn gemaakt via de werkstroom terugzetten in de gebruikersinterface wordt geen van deze netwerkconfiguraties nadat de herstelbewerking voltooid is. Zie voor meer informatie, [herstellen van virtuele machines met speciale netwerkconfiguraties](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations).
  * Virtuele machines onder een load balancer-configuratie (intern en extern)
  * Virtuele machines met meerdere gereserveerde IP-adressen
  * Virtuele machines met meerdere netwerkadapters

## <a name="create-a-recovery-services-vault-for-a-vm"></a>Een Recovery Services-kluis voor een VM maken
Een Recovery Services-kluis is een entiteit die de back-ups en herstelpunten die zijn gemaakt na verloop van tijd worden opgeslagen. De Recovery Services-kluis bevat ook het back-upbeleid die gekoppeld aan de beveiligde virtuele machines zijn.

Een Recovery Services-kluis maken:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Op de **Hub** selecteert u **Bladeren**, en typ vervolgens **Recovery Services**. Als u te typen begint, filtert uw invoer in de lijst met resources. Selecteer **Recovery Services-kluizen**.

    ![Typen in het vak en 'Recovery Services-kluizen' in de resultaten te selecteren](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

    De lijst met Recovery Services-kluizen wordt weergegeven.
3. Op de **Recovery Services-kluizen** selecteert u **toevoegen**.

    ![Een Recovery Services-kluis maken, stap 2](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

    De **Recovery Services-kluizen** deelvenster wordt geopend. Moet u informatie te verstrekken voor **naam**, **abonnement**, **resourcegroep**, en **locatie**.

    ![Deelvenster 'Recovery Services-kluizen'](./media/backup-azure-arm-vms-prepare/rs-vault-attributes.png)
4. Voer bij **Naam** een beschrijvende naam in om de kluis aan te duiden. De naam moet uniek zijn voor het Azure-abonnement. Typ een naam met 2 tot 50 tekens. Moet beginnen met een letter en mag bevatten alleen letters, cijfers en afbreekstreepjes.
5. Selecteer **abonnement** om te zien van de lijst met beschikbare abonnementen. Als u niet zeker weet welk abonnement u moet gebruiken, gebruikt u de standaard (of voorgestelde) abonnement. Er zijn meerdere opties alleen als uw werk of schoolaccount is gekoppeld aan meerdere Azure-abonnementen.
6. Selecteer **resourcegroep** toevoegen aan de lijst met beschikbare van resourcegroepen, **nieuw** om een nieuwe resourcegroep te maken. Zie voor meer informatie over resourcegroepen [overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
7. Selecteer **locatie** selecteren van de geografische regio voor de kluis. De kluis *moet* zich in dezelfde regio bevinden als de virtuele machines die u wilt beveiligen.

   > [!IMPORTANT]
   > Als u niet zeker weet van de locatie waar uw VM zich bevindt, sluit het dialoogvenster kluis maken en Ga naar de lijst met virtuele machines in de portal. Als u virtuele machines in meerdere regio's hebt, moet u een Recovery Services-kluis maken in elke regio. Maak de kluis op de eerste locatie voordat u verdergaat met de volgende locatie. Er is niet nodig om op te geven van de storage-accounts voor het opslaan van de back-upgegevens. De Recovery Services-kluis en de Azure Backup-service worden verwerkt die automatisch.
   >
   >

8. Selecteer **Maken**. Het kan even duren voordat de Recovery Services-kluis is gemaakt. Controleer de statusmeldingen in de rechterbovenhoek van de portal. Nadat uw kluis is gemaakt, wordt deze weergegeven in de lijst met Recovery Services-kluizen. Als u uw kluis niet ziet, selecteert u **vernieuwen**.

    ![Lijst met back-upkluizen](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

Nu u uw kluis hebt gemaakt, leert u hoe u de opslagreplicatie instelt.

## <a name="set-storage-replication"></a>Replicatie van de opslag instellen
De optie voor opslagreplicatie kunt u kiezen tussen geografisch redundante opslag en lokaal redundante opslag. Uw kluis heeft standaard geografisch redundante opslag. Laat de optie ingesteld op geografisch redundante opslag als dit uw primaire back-up is. Kies lokaal redundante opslag als u een goedkopere optie wilt die minder duurzaam is.

De instelling voor opslagreplicatie bewerken:

1. Op de **Recovery Services-kluizen** deelvenster, selecteer uw kluis.
    Wanneer u uw kluis selecteert de **instellingen** deelvenster (met de naam van de kluis boven) en het detailvenster kluis openen.

   ![Kies uw kluis uit de lijst met back-upkluizen](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)

2. Op de **instellingen** deelvenster de verticale schuifbalk kunt Schuif omlaag naar de **beheren** sectie en selecteer **back-upinfrastructuur**. In de **algemene** sectie **back-upconfiguratie**. Op de **back-upconfiguratie** deelvenster, kies de optie voor opslagreplicatie voor uw kluis. Uw kluis heeft standaard geografisch redundante opslag.

   ![Lijst met back-upkluizen](./media/backup-azure-arm-vms-prepare/full-blade.png)

   Als u van Azure als een eindpunt primaire back-upopslag gebruikmaakt, gaat u verder met geografisch redundante opslag. Als u Azure gebruikt als een niet-primaire back-upopslag eindpunt, kies lokaal redundante opslag. Meer informatie over opties voor opslag in de [overzicht van Azure Storage-replicatie](../storage/common/storage-redundancy.md).

3. Als u het opslagtype voor de replicatie is gewijzigd, selecteert u **opslaan**.
    
Nadat u de opslagoptie voor uw kluis hebt gekozen, bent u klaar om te koppelen van de virtuele machine met de kluis. Voordat u de VM aan de kluis koppelt, moet u eerst de virtuele Azure-machines detecteren en registreren.

## <a name="select-a-backup-goal-set-policy-and-define-items-to-protect"></a>Een back-doel selecteren, beleid instellen en definiëren van items moeten worden beveiligd
Voordat u een virtuele machine met een kluis registreert, voert u het detectieproces om ervoor te zorgen dat de nieuwe virtuele machines die zijn toegevoegd aan het abonnement zijn geïdentificeerd. De query's proces Azure voor de lijst met virtuele machines in het abonnement, samen met informatie zoals de naam van de cloudservice en de regio. 

In de Azure portal *scenario* verwijst naar wat u in de Recovery Services-kluis wordt geplaatst. *Beleid* is de planning voor hoe vaak en wanneer de herstelpunten worden gemaakt. Het beleid bevat ook de bewaartermijn voor de herstelpunten.

1. Als er al een Recovery Services-kluis is geopend, gaat u verder met stap 2. Als u geen een Recovery Services-kluis openen, opent u de [Azure-portal](https://portal.azure.com/). Op de **Hub** selecteert u **meer services**.

   a. Typ in de lijst met resources **Recovery Services**. Als u te typen begint, is uw invoer filtert de lijst. Wanneer er **Recovery Services-kluizen**, selecteert u deze.

      ![Typen in het vak en 'Recovery Services-kluizen' in de resultaten te selecteren](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

      De lijst met Recovery Services-kluizen wordt weergegeven. Als er geen kluizen in uw abonnement, wordt deze lijst leeg is.

      ![Weergave van de lijst met Recovery Services-kluizen](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

   b. Selecteer in de lijst met Recovery Services-kluizen een kluis.

      De **instellingen** deelvenster en het kluisdashboard voor de door u gekozen kluis openen.

      ![Instellingen deelvenster en de kluisreferenties dashboard](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)
2. Selecteer op het dashboard kluismenu **back-up**.

   ![Back-knop](./media/backup-azure-arm-vms-prepare/backup-button.png)

   De **back-up** en **back-updoel** deelvensters openen.

3. Op de **back-updoel** deelvenster ingesteld **waar wordt uw workload uitgevoerd?** naar **Azure** en **wat wilt u back-up maken?** naar  **Virtuele machine**. Selecteer vervolgens **OK**.

   ![Back-up- en back-updoel deelvensters](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

   Deze stap wordt de VM-extensie met de kluis geregistreerd. De **back-updoel** deelvenster wordt gesloten, en de **back-up maken van beleid** deelvenster wordt geopend.

   !["Back-up" en 'Back-up beleid' deelvensters](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)
4. Op de **back-up maken van beleid** deelvenster, selecteert u de back-upbeleid dat u wilt toepassen op de kluis.

   ![Back-upbeleid selecteren](./media/backup-azure-arm-vms-prepare/setting-rs-backup-policy-new.png)

   De details van het standaardbeleid worden onder de vervolgkeuzelijst weergegeven. Als u een nieuw beleid wilt maken, selecteert u **Nieuw maken** in de vervolgkeuzelijst. Zie [Een back-upbeleid definiëren](backup-azure-vms-first-look-arm.md#defining-a-backup-policy) voor instructies over het definiëren van een back-upbeleid.
    Selecteer **OK** het back-upbeleid koppelen aan de kluis.

   De **back-up maken van beleid** deelvenster wordt gesloten, en de **virtuele machines selecteren** deelvenster wordt geopend.
5. Op de **virtuele machines selecteren** deelvenster, kiest u de virtuele machines om te koppelen aan het opgegeven beleid en selecteer **OK**.

   ![Deelvenster 'Virtuele machines selecteren'](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

   De geselecteerde virtuele machine wordt gevalideerd. Als u de virtuele machines die u verwacht te zien niet ziet, controleert u dat ze bestaan in dezelfde Azure-locatie als de Recovery Services-kluis en dat u nog niet is beveiligd in een andere kluis. Het kluisdashboard toont de locatie van de Recovery Services-kluis.

6. Nu dat u alle instellingen voor de kluis hebt gedefinieerd op de **back-** deelvenster **back-up**. Deze stap wordt het beleid geïmplementeerd voor de kluis en de virtuele machines. Deze stap maakt geen het eerste herstelpunt voor de virtuele machine.

   ![Knop 'Back-up inschakelen'](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Nadat u de back-up is ingeschakeld, wordt uw back-upbeleid volgens schema uitgevoerd. Als u genereren van een back-uptaak op aanvraag wilt naar de back-up van de virtuele machines nu Zie [activering van de back-uptaak](./backup-azure-arm-vms.md#triggering-the-backup-job).

Als u problemen met de registratie van de virtuele machine hebt, raadpleegt u de volgende informatie over het installeren van de VM-agent en op de netwerkverbinding. Moet u waarschijnlijk niet de volgende informatie als u virtuele machines die zijn gemaakt in Azure beveiligt. Maar als u uw virtuele machines naar Azure hebt gemigreerd, moet u dat u de VM-agent correct geïnstalleerd en dat de virtuele machine met het virtuele netwerk communiceren kan.

## <a name="install-the-vm-agent-on-the-virtual-machine"></a>De VM-agent installeren op de virtuele machine
Voor de back-up-extensie te kunnen gebruiken, de Azure [VM-agent](../virtual-machines/windows/agent-user-guide.md) moet worden geïnstalleerd op virtuele machine van Azure. Als uw virtuele machine is gemaakt vanuit Azure Marketplace, is de VM-agent al aanwezig op de virtuele machine. 

De volgende informatie is bedoeld voor situaties waarin u *niet* gemaakt met behulp van een virtuele machine uit Azure Marketplace. U kunt bijvoorbeeld een virtuele machine gemigreerd van een on-premises datacenter. In dat geval moet de VM-agent worden geïnstalleerd ter bescherming van de virtuele machine.

Als u problemen back-ups van de virtuele machine in Azure hebt, gebruikt u de volgende tabel om te controleren of de Azure VM-agent correct is geïnstalleerd op de virtuele machine. De tabel bevat aanvullende informatie over de VM-agent voor Windows en Linux-machines.

| **Bewerking** | **Windows** | **Linux** |
| --- | --- | --- |
| De VM-agent installeren |Download en installeer de [agent-MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). U moet administrator-bevoegdheden om de installatie te voltooien. |Installeer de meest recente [Linux-agent](../virtual-machines/linux/agent-user-guide.md). U moet administrator-bevoegdheden om de installatie te voltooien. Het is raadzaam om de installatie van de agent van uw opslagplaats voor distributie. We *wordt niet aanbevolen* installeren van de Linux-VM-agent rechtstreeks vanuit GitHub.  |
| De VM-agent bijwerken |Bijwerken van de VM-agent is net zo eenvoudig als het opnieuw installeren van de [binaire bestanden voor VM-agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). <br><br>Zorg ervoor dat er geen back-upbewerking wordt uitgevoerd terwijl de VM-agent wordt bijgewerkt. |Volg de instructies voor [bijwerken van de Linux VM-agent](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Het is raadzaam om de agent van uw opslagplaats distributiepunten bijwerken. We *wordt niet aanbevolen* bijwerken van de Linux VM-agent rechtstreeks vanuit GitHub.<br><br>Zorg ervoor dat er geen back-upbewerking wordt uitgevoerd terwijl de VM-agent wordt bijgewerkt. |
| De installatie van de VM-agent valideren |1. Blader naar de map C:\WindowsAzure\Packages in de Azure VM. <br><br>2. Het bestand WaAppAgent.exe niet vinden. <br><br>3. Klik met de rechtermuisknop op het bestand, ga naar **Eigenschappen** en selecteer vervolgens het tabblad **Details**. De **productversie** veld moet 2.6.1198.718 of hoger. |N/A |

### <a name="backup-extension"></a>Backup-extensie
Nadat de VM-agent is geïnstalleerd op de virtuele machine, wordt in de Azure Backup-service de Backup-extensie in de VM-agent geïnstalleerd. Upgrades naadloos de Backup-service en -patches van de Backup-extensie.

De Backup-service installeert de Backup-extensie of de VM wordt uitgevoerd of niet. Bij een actieve VM is de kans het grootst dat een toepassingsconsistent herstelpunt wordt verkregen. De Backup-service blijft echter back-up van de virtuele machine zelfs als deze is uitgeschakeld en de extensie kan niet worden geïnstalleerd. Dit staat bekend als *offline VM*. In dit geval is het herstelpunt *crashconsistent*.

## <a name="establish-network-connectivity"></a>Netwerkverbinding tot stand brengen
Voor het beheren van de VM-momentopnamen, moet de Backup-extensie verbinding hebben met de Azure openbare IP-adressen. Zonder de juiste internetverbinding time-out van de virtuele machine HTTP-aanvragen en de back-upbewerking is mislukt. Als uw implementatie beschikt over toegangsbeperkingen--via een netwerkbeveiligingsgroep (NSG), bijvoorbeeld--Kies een van deze opties een duidelijke pad opgeven voor back-verkeer:

* [Geaccepteerde het Azure datacenter IP ranges](http://www.microsoft.com/en-us/download/details.aspx?id=41653).
* Implementeer een HTTP-proxyserver voor het routeren van verkeer.

Wanneer u bepaalt welke optie u bent moet gebruiken, zijn de verschillen tussen beheerbaarheid, gedetailleerde controle en kosten.

| Optie | Voordelen | Nadelen |
| --- | --- | --- |
| Whitelist IP-adresbereiken |Er zijn geen extra kosten.<br><br>Voor toegang tot te openen in een NSG, gebruikt u de **Set AzureNetworkSecurityRule** cmdlet. |Moeilijk te beheren als de betrokken IP-adresbereiken op den duur veranderen.<br><br>Biedt toegang tot het geheel van Azure en niet alleen opslag. |
| Gebruik een HTTP-proxy |Gedetailleerd beheer in de proxy van de opslag URL's is toegestaan.<br><br>Één punt internet toegang tot virtuele machines.<br><br>Ten aanzien van wijzigingen in de Azure-IP-adres. |Extra kosten voor het uitvoeren van een virtuele machine met de proxysoftware. |

### <a name="whitelist-the-azure-datacenter-ip-ranges"></a>De Azure-datacenter geaccepteerde IP-adresbereiken
Geaccepteerde IP-bereiken met de Azure-datacenter, Zie de [Azure-website](http://www.microsoft.com/en-us/download/details.aspx?id=41653) voor meer informatie over de IP-adresbereiken en instructies.

U kunt de verbindingen naar de opslag van de specifieke regio toestaan met behulp van [service labels](../virtual-network/security-overview.md#service-tags). Zorg ervoor dat de regel die toegang tot het opslagaccount verleent hogere prioriteit dan de regel die blokkeert de toegang tot internet heeft. 

![NSG met labels van de opslag voor een regio](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

> [!WARNING]
> Storage-service-tags zijn alleen beschikbaar in specifieke gebieden en zijn Preview-versie. Zie voor een lijst met regio's, [Service-tags voor opslag](../virtual-network/security-overview.md#service-tags).

### <a name="use-an-http-proxy-for-vm-backups"></a>Een HTTP-proxy gebruiken voor VM-ups
Wanneer u een back-up van een VM, verzendt de Backup-extensie op de virtuele machine opdrachten voor het beheer van de momentopname naar Azure Storage via een HTTPS-API. De Backup-extensie-verkeer via de HTTP-proxy routeren omdat dit het enige onderdeel dat is geconfigureerd voor toegang tot het openbare internet.

> [!NOTE]
> Specifieke proxysoftware die u moet gebruiken wordt niet aanbevolen. Zorg ervoor dat u kiest een proxy die compatibel is met de configuratiestappen die vervolgens worden weergegeven.
>
>

De installatiekopie van het volgende voorbeeld ziet u de drie configuratiestappen die nodig zijn voor het gebruik van een HTTP-proxy:

* De app VM routes alle HTTP-verkeer gebonden voor het openbare internet via de proxy-VM.
* De VM-proxy kan binnenkomend verkeer van virtuele machines in het virtuele netwerk.
* De netwerkbeveiligingsgroep NSF-vergrendeling door met de naam moet een regel waarmee uitgaand internetverkeer van de VM-proxy.

Voor het gebruik van een HTTP-proxy om te communiceren met het openbare internet, moet u de volgende stappen uitvoeren.

> [!NOTE]
> Deze stappen worden specifieke namen en waarden voor dit voorbeeld gebruiken. Wanneer u invoeren (of plakt) details in uw code worden de namen en waarden voor uw implementatie gebruiken.

#### <a name="step-1-configure-outgoing-network-connections"></a>Stap 1: Uitgaande netwerkverbindingen configureren
###### <a name="for-windows-machines"></a>Voor Windows-machines
Deze procedure stelt u de proxyserver-configuratie voor het lokale systeemaccount.

1. Download [PsExec](https://technet.microsoft.com/sysinternals/bb897553).
2. Open Internet Explorer door met de volgende opdracht vanaf een opdrachtprompt:

    ```
    psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe"
    ```

3. Ga in Internet Explorer naar **extra** > **Internetopties** > **verbindingen** > **LAN-instellingen**.
4. Controleer of de proxy-instellingen voor het account. Stel de IP-proxy en poort.
5. Sluit Internet Explorer.

Het volgende script stelt u de proxyconfiguratie van een voor alle computers en wordt gebruikt voor uitgaande HTTP of HTTPS-verkeer. Als u een proxyserver voor een huidige gebruikersaccount (niet een lokale systeemaccount) hebt ingesteld, moet u dit script gebruiken voor deze toepassen op SYSTEMACCOUNT.

```
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver
```

> [!NOTE]
> Als u '(407) proxyverificatie is vereist' in het logboek voor de proxy-server constateert, controleert u de verificatie juist is ingesteld.
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

#### <a name="step-2-allow-incoming-connections-on-the-proxy-server"></a>Stap 2: Binnenkomende verbindingen toestaan op de proxyserver
1. Open Windows Firewall op de proxyserver. De eenvoudigste manier om toegang tot de firewall is om te zoeken naar **Windows Firewall met geavanceerde beveiliging**.
2. In de **Windows Firewall met geavanceerde beveiliging** in het dialoogvenster met de rechtermuisknop op **regels voor binnenkomende verbindingen** en selecteer **nieuwe regel**.
3. In de nieuwe Wizard regel voor binnenkomende, op de **regeltype** pagina de **aangepaste** optie en selecteer **volgende**.
4. Op de **programma** pagina **alle programma's** en selecteer **volgende**.
5. Op de **protocollen en poorten** pagina, voer de volgende informatie en selecteert u **volgende**:
   * Voor **protocoltype**, selecteer **TCP**.
   * Voor **lokale poort**, selecteer **bepaalde poorten**. Geef het nummer van de proxypoort die is geconfigureerd in het onderstaande vak.
   * Voor **externe poort**, selecteer **alle poorten**.

Voor de rest van de wizard de standaardinstellingen te accepteren totdat u aan het einde. Vervolgens geeft deze regel een naam. 

#### <a name="step-3-add-an-exception-rule-to-the-nsg"></a>Stap 3: Een uitzonderingsregel toevoegen aan het NSG
De volgende opdracht voegt een uitzondering aan het NSG. Deze uitzondering kan TCP-verkeer vanaf een willekeurige poort op 10.0.0.5 aan een internetadres op poort 80 (HTTP) of 443 (HTTPS). Als u een specifieke poort op het openbare internet vereist, moet u die poort toevoegen ```-DestinationPortRange```.

Voer de volgende opdracht in een Azure PowerShell-opdrachtprompt:

```
Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
```

## <a name="questions"></a>Vragen?
Als u vragen hebt of als er een functie die u wilt zien opgenomen, [Stuur ons feedback](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Volgende stappen
Nu dat u uw omgeving voor back-ups van uw virtuele machine hebt voorbereid, wordt de volgende logische stap is het maken van een back-up. De planning artikel vindt meer gedetailleerde informatie over back-ups van virtuele machines.

* [Back-up van virtuele machines](backup-azure-arm-vms.md)
* [Uw VM-back-infrastructuur plannen](backup-azure-vms-introduction.md)
* [Back-ups van virtuele machine beheren](backup-azure-manage-vms.md)
