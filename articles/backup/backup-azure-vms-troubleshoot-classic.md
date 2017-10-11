---
title: Azure Backup-fouten in de klassieke portal oplossen | Microsoft Docs
description: Problemen oplossen met Azure back-up en herstel van Azure virtuele machines in de klassieke portal.
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 117201fb-c0cd-4be4-b47f-abd88fe914cf
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/23/2017
ms.author: trinadhk;markgal;
ms.openlocfilehash: 284a1b64fbb15d0aa800182c6671d447e191b76a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-azure-virtual-machine-backup"></a>Problemen oplossen met back-ups van virtuele Azure-machines
> [!div class="op_single_selector"]
> * [Recovery services-kluis](backup-azure-vms-troubleshoot.md)
> * [Back-upkluis](backup-azure-vms-troubleshoot-classic.md)
>
>

U kunt er zijn fouten opgetreden tijdens het gebruik van Azure Backup met informatie die worden vermeld in de onderstaande tabel kunt oplossen.

## <a name="discovery"></a>Detectie
| Back-upbewerking | Details van fouten | Tijdelijke oplossing |
| --- | --- | --- |
| Detectie |Kan niet detecteren van nieuwe items - Microsoft Azure Backup aangetroffen en interne fout. Wacht een paar minuten en probeer het opnieuw. |Probeer het detectieproces na 15 minuten. |
| Detectie |Kan niet detecteren van nieuwe items – wordt detectie van een andere bewerking al uitgevoerd. Wacht totdat de huidige bewerking voor de detectie is voltooid. |Geen |

## <a name="register"></a>Registreren
| Back-upbewerking | Details van fouten | Tijdelijke oplossing |
| --- | --- | --- |
| Registreren |Aantal gegevensschijven gekoppeld aan de virtuele machine de ondersteunde limiet overschreden: Neem loskoppelen van een aantal gegevensschijven op deze virtuele machine en probeer het opnieuw. Azure backup ondersteunt maximaal 16 gegevensschijven gekoppeld aan een virtuele machine van Azure voor back-up |Geen |
| Registreren |Microsoft Azure Backup heeft een interne fout - wacht een paar minuten en probeer het vervolgens opnieuw. Neem contact op met Microsoft Ondersteuning als het probleem zich blijft voordoen. |U kunt deze fout als gevolg van een van de volgende niet-ondersteunde configuratie van de virtuele machine op Premium LRS ophalen. <br> Premium-opslag virtuele machines kan worden back-up met behulp van de recovery services-kluis. [Meer informatie](backup-introduction-to-azure-backup.md#using-premium-storage-vms-with-azure-backup) |
| Registreren |Registratie is mislukt met de time-out voor de bewerking van Agent installeren |Controleer of de versie van het besturingssysteem van de virtuele machine wordt ondersteund. |
| Registreren |Opdracht is mislukt: een andere bewerking wordt uitgevoerd voor dit item. Wacht totdat de vorige bewerking is voltooid |Geen |
| Registreren |Virtuele machines met virtuele harde schijven die zijn opgeslagen op Premium-opslag worden niet ondersteund voor back-up |Geen |
| Registreren |Agent van de virtuele machine is niet aanwezig op de virtuele machine: Installeer de vereiste vereiste VM-agent en start de bewerking opnieuw. |[Lees meer](#vm-agent) over de installatie van de VM-agent en het valideren van de installatie van de VM-agent. |

## <a name="backup"></a>Back-up
| Back-upbewerking | Details van fouten | Tijdelijke oplossing |
| --- | --- | --- |
| Back-up |Kan niet communiceren met de VM-agent om de status van de momentopname op te halen. Er is een time-out opgetreden voor de momentopname VM sub-taak. -Zie de gids voor probleemoplossing over het oplossen van dit. |Deze fout wordt gegenereerd als er een probleem met de VM-Agent of netwerktoegang tot de Azure-infrastructuur is geblokkeerd op een bepaalde manier. Meer informatie over [problemen momentopname maken van VM-foutopsporing](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md). <br> Als de VM-agent is niet wordt veroorzaakt door problemen, start u de virtuele machine. Een onjuiste status voor de virtuele machine kan soms problemen veroorzaken en opnieuw starten van de virtuele machine herstelt u deze 'slechte staat' |
| Back-up |Back-up is mislukt met een interne fout: probeer de bewerking over enkele minuten opnieuw. Als het probleem zich blijft voordoen, neem dan contact op met Microsoft Support |Controleer of er een tijdelijk probleem bij het openen van de VM-opslag. Controleer of [Azure Status](https://azure.microsoft.com/en-us/status/) om te zien of er een probleem continu betrekking hebben op de compute-/ opslag/network in de regio. Maak opnieuw die het back-post-probleem is verholpen. |
| Back-up |Kan de bewerking niet uitvoeren omdat de VM niet meer bestaat. |Back-up kan niet worden uitgevoerd omdat de VM die is geconfigureerd voor back-up is verwijderd. Stop verdere back-ups door te gaan naar de weergave van beveiligde items, beveiligde item selecteren en klik op beveiliging stoppen. U kunt gegevens behouden door back-up behouden gegevensoptie te selecteren. U kunt later beveiliging hervatten voor deze virtuele machine door te klikken op beveiligingsconfiguratie van geregistreerde Items weergeven |
| Back-up |Extensie voor het geselecteerde item - VM-Agent is mislukt voor het installeren van de Azure Recovery Services is een vereiste voor Azure Recovery Services-extensie. Installeer de Azure VM-agent en start de bewerking van de registratie opnieuw |<ol> <li>Controleer of de VM-agent correct is geïnstalleerd. <li>Zorg ervoor dat de vlag op de VM-configuratie correct is ingesteld.</ol> [Lees meer](#validating-vm-agent-installation) over de installatie van de VM-agent en het valideren van de installatie van de VM-agent. |
| Back-up |Opdracht uitvoering is mislukt: een andere bewerking wordt momenteel uitgevoerd voor dit item. Wacht totdat de vorige bewerking is voltooid en probeer het vervolgens opnieuw |Een bestaande back-up- of hersteltaak voor de virtuele machine wordt uitgevoerd en een nieuwe taak kan niet worden gestart terwijl de bestaande taak wordt uitgevoerd. |
| Back-up |Installatie van de uitbreiding is mislukt met de fout 'COM + is kan niet communiceren met de Microsoft Distributed Transaction Coordinator |Dit betekent meestal dat de COM +-service niet wordt uitgevoerd. Neem contact op met Microsoft ondersteuning voor hulp bij het oplossen van dit probleem. |
| Back-up |Momentopname-bewerking is mislukt met de VSS-fout voor bewerking 'dit station is vergrendeld door BitLocker-stationsversleuteling. U moet deze schijf van het Configuratiescherm ontgrendelen. |BitLocker uitschakelen voor alle stations op de virtuele machine en bekijk of de VSS-probleem is opgelost |
| Back-up |Virtuele machines met virtuele harde schijven die zijn opgeslagen op Premium-opslag worden niet ondersteund voor back-up |Geen |
| Back-up |Azure virtuele Machine is niet gevonden. |Dit gebeurt wanneer de primaire virtuele machine is verwijderd, maar blijft de back-upbeleid zoekt u naar een virtuele machine back-up uitvoeren. Deze fout oplossen: <ol><li>Maak de virtuele machine met dezelfde naam en hetzelfde Resourcegroepnaam [cloudservicenaam] <br>(OR) <li> Schakel de beveiliging voor deze virtuele machine zodat de volgende back-ups worden niet ophalen geactiveerd. </ol> |
| Back-up |Agent van de virtuele machine is niet aanwezig op de virtuele machine: Installeer de vereiste vereiste VM-agent en start de bewerking opnieuw. |[Lees meer](#vm-agent) over de installatie van de VM-agent en het valideren van de installatie van de VM-agent. |

## <a name="jobs"></a>Taken
| Bewerking | Details van fouten | Tijdelijke oplossing |
| --- | --- | --- |
| Taak annuleren |Annulering wordt niet ondersteund voor dit taaktype: wacht totdat de taak is voltooid. |Geen |
| Taak annuleren |De taak is niet in een status Annuleerbare - wacht totdat de taak is voltooid. <br>OF<br> De geselecteerde taak is niet in een status Annuleerbare - wacht tot de taak te voltooien. |De taak is bijna voltooid naar alle waarschijnlijkheid; Wacht totdat de taak is voltooid |
| Taak annuleren |De taak annuleren niet omdat deze niet uitgevoerd is-deze annulering wordt alleen ondersteund voor taken die uitgevoerd worden. Annuleer van poging op een onderhanden taak. |Dit gebeurt als gevolg van een tijdelijke status. Wacht een paar minuten en probeer de annuleringsbewerking |
| Taak annuleren |Annuleer de taak - is mislukt. Wacht totdat de taak is voltooid. |Geen |

## <a name="restore"></a>Herstellen
| Bewerking | Details van fouten | Tijdelijke oplossing |
| --- | --- | --- |
| Herstellen |Terugzetten is mislukt vanwege een interne Cloud fout |<ol><li>Cloudservice die u probeert te herstellen is geconfigureerd met DNS-instellingen. U kunt controleren <br>$deployment = get-AzureDeployment - ServiceName "ServiceName"-sleuf 'Productie' Get-AzureDns - DnsSettings $deployment. DnsSettings<br>Als er is een adres dat is geconfigureerd, betekent dit dat de DNS-instellingen zijn geconfigureerd.<br> <li>Cloudservice waarnaar u wilt herstellen, is geconfigureerd met ReservedIP en bestaande virtuele machines in de cloudservice wordt gestopt.<br>U kunt controleren dat een cloudservice is gereserveerde IP-adres via de volgende powershell-cmdlets:<br>$deployment = get-AzureDeployment - ServiceName "servicename"-sleuf 'Productie' $dep. ReservedIPName <br><li>U probeert een virtuele machine te herstellen met de volgende speciale netwerkconfiguraties in dezelfde cloudservice. <br>-Virtuele machines onder load balancer-configuratie (intern en extern)<br>-Virtuele machines met meerdere gereserveerde IP 's<br>-Virtuele machines met meerdere NIC 's<br>Selecteer een nieuwe cloudservice in de gebruikersinterface of Raadpleeg [terugzetten overwegingen](backup-azure-restore-vms.md#restoring-vms-with-special-network-configurations) voor virtuele machines met speciale netwerkconfiguraties</ol> |
| Herstellen |De geselecteerde DNS-naam is al in gebruik: Geef een andere DNS-naam en probeer het opnieuw. |De DNS-naam verwijst naar de naam van de cloudservice (meestal eindigend. cloudapp.net). Dit moet uniek zijn. Als deze fout optreedt, moet u een andere VM-naam kiezen tijdens het terugzetten. <br><br> Deze fout wordt alleen weergegeven op gebruikers van de Azure-portal. De herstelbewerking via PowerShell is gelukt, omdat deze alleen worden hersteld van de schijven en de virtuele machine niet maken. De fout wordt worden geconfronteerd wanneer de virtuele machine expliciet door u is gemaakt nadat de herstelbewerking van de schijf. |
| Herstellen |De configuratie van de opgegeven virtuele netwerk is niet correct: Geef een ander virtueel netwerk-configuratie en probeer het opnieuw. |Geen |
| Herstellen |De opgegeven cloudservice maakt gebruik van een gereserveerd IP-adres die komt niet overeen met de configuratie van de virtuele machine wordt hersteld - Geef een andere cloudservice die geen gebruik van gereserveerde IP-adres maakt, of kies een ander herstelpunt van herstellen. |Geen |
| Herstellen |Cloudservice is bereikt voor het aantal invoer eindpunten: Voer de bewerking opnieuw uit door op te geven van een andere cloudservice of met behulp van een bestaand eindpunt. |Geen |
| Herstellen |Back-kluis en doel storage-account zijn in twee verschillende regio's: Zorg ervoor dat het storage-account opgegeven in restore-bewerking in dezelfde Azure-regio als de back-upkluis. |Geen |
| Herstellen |Storage-Account opgegeven voor de herstelbewerking niet is ondersteund - alleen Basic/Standard storage-accounts met lokaal redundante of geografisch redundante replicatie-instellingen worden ondersteund. Selecteer een ondersteunde storage-account |Geen |
| Herstellen |Type Opslagaccount dat is opgegeven voor de herstelbewerking is niet online - Zorg ervoor dat het storage-account opgegeven in restore-bewerking online is |Dit kan gebeuren vanwege een tijdelijke fout in Azure Storage of als gevolg van een storing. Kies een ander opslagaccount. |
| Herstellen |Resourcegroep quotum is bereikt - Verwijder een aantal resourcegroepen van Azure portal of neem contact op met ondersteuning van Azure naar de limieten verhogen. |Geen |
| Herstellen |Geselecteerde subnet bestaat niet: Selecteer een subnet dat bestaat |Geen |

## <a name="policy"></a>Beleid
| Bewerking | Details van fouten | Tijdelijke oplossing |
| --- | --- | --- |
| Beleid maken |Maken van het beleid - Verminder de keuzes bewaarperiode om door te gaan met de configuratie van beleid is mislukt. |Geen |

## <a name="vm-agent"></a>VM-Agent
### <a name="setting-up-the-vm-agent"></a>Instellen van de VM-Agent
Normaal gesproken is de VM-Agent al aanwezig in virtuele machines die zijn gemaakt van de Azure-galerie. Virtuele machines die worden gemigreerd van lokale datacenters moet echter niet de VM-Agent is geïnstalleerd. Voor deze virtuele machines moet de VM-Agent expliciet worden geïnstalleerd. Lees meer over [de VM-agent installeren op een bestaande VM](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx).

Voor VM's van Windows:

* Download en installeer de [agent-MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). U hebt Administrator-bevoegdheden nodig om de installatie te kunnen uitvoeren.
* [Werk de VM-eigenschap bij](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) om aan te geven dat de agent is geïnstalleerd.

Voor virtuele Linux-machines:

* Installeer de meest recente [Linux-agent](https://github.com/Azure/WALinuxAgent) vanuit github.
* [Werk de VM-eigenschap bij](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) om aan te geven dat de agent is geïnstalleerd.

### <a name="updating-the-vm-agent"></a>De VM-agent bijwerken
Voor VM's van Windows:

* Om de VM-agent bij te werken hoeft u alleen de [binaire bestanden voor de VM-agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) opnieuw te installeren. Echter, moet u ervoor zorgen dat er geen back-upbewerking wordt uitgevoerd terwijl de VM-Agent wordt bijgewerkt.

Voor virtuele Linux-machines:

* Volg de instructies op [Linux VM-Agent bijwerken](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="validating-vm-agent-installation"></a>Installatie van de VM-Agent valideren
Het controleren van de versie van het VM-Agent op VM's van Windows:

1. Meld u aan bij de virtuele machine van Azure en navigeer naar de map *C:\WindowsAzure\Packages*. Het bestand WaAppAgent.exe moet hier aanwezig zijn.
2. Klik met de rechtermuisknop op het bestand, ga naar **Eigenschappen** en selecteer vervolgens het tabblad **Details**. Het veld productversie moet 2.6.1198.718 of hoger
