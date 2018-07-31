---
title: SAP HANA Azure back-up op basis van opslagmomentopnamen | Microsoft Docs
description: Er zijn twee primaire back-mogelijkheden voor SAP HANA op Azure virtual machines, in dit artikel bevat informatie over SAP HANA-back-up op basis van opslagmomentopnamen
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ums.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/05/2018
ms.author: rclaus
ms.openlocfilehash: c5066d23705ca84febaa0ba527a01259134146c0
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39358978"
---
# <a name="sap-hana-backup-based-on-storage-snapshots"></a>Back-up van SAP HANA op basis van opslagmomentopnamen

## <a name="introduction"></a>Inleiding

Dit maakt deel uit van een driedelige reeks gerelateerde artikelen op SAP HANA back-up. [Back-uphandleiding voor SAP HANA op Azure Virtual Machines](sap-hana-backup-guide.md) bevat een overzicht en de informatie op aan de slag, en [SAP HANA Azure back-up op bestandsniveau](sap-hana-backup-file-level.md) bevat informatie over de bestanden zijn gebaseerd back-upoptie.

Wanneer u een VM-back-upfunctie voor een single instance alles-in-een demo-systeem, moet een kunt u in dat een virtuele machine back-up in plaats van het beheren van HANA back-ups op het niveau van het besturingssysteem. Een alternatief is Azure blob-momentopnamen te maken van kopieën van afzonderlijke virtuele schijven die zijn gekoppeld aan een virtuele machine, en de HANA-gegevensbestanden te houden. Maar een punt kritiek is consistentie van de app bij het maken van een momentopname van een virtuele machine back-up of schijf terwijl het systeem actief is en wordt uitgevoerd. Zie _SAP HANA-gegevensconsistentie bij het nemen van opslagmomentopnamen_ in het bijbehorende artikel [back-uphandleiding voor SAP HANA op Azure Virtual Machines](sap-hana-backup-guide.md). SAP HANA heeft een functie die ondersteuning biedt voor dit soort storage-momentopnamen.

## <a name="sap-hana-snapshots-as-central-part-of-application-consistent-backups"></a>Momentopnamen van de SAP HANA als centraal onderdeel van de toepassing consistente back-ups

Er is een functie in SAP HANA die ondersteuning biedt voor maken van een momentopname van de opslag. Er is een beperking van één container-systemen. Scenario's voor eenmalige SAP HANA MCS met meer dan één tenant bieden geen ondersteuning voor dit type momentopname van de SAP HANA-database (Zie [maken van een momentopname van een Storage (SAP HANA Studio)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/a0/3f8f08501e44d89115db3c5aa08e3f/content.htm)).

Het werkt als volgt:

- Voorbereiden op een momentopname van een storage door het starten van de SAP HANA-momentopname
- Uitvoeren van de storage-momentopname (in Azure blob-momentopname, bijvoorbeeld)
- Controleer of de SAP HANA-momentopname

![Deze schermafbeelding ziet u dat de momentopname van een SAP HANA-gegevens kan worden gemaakt via een SQL-instructie](media/sap-hana-backup-storage-snapshots/image011.png)

Deze schermafbeelding ziet u dat de momentopname van een SAP HANA-gegevens kan worden gemaakt via een SQL-instructie.

![De momentopname wordt ook weergegeven in de back-catalogus in SAP HANA Studio](media/sap-hana-backup-storage-snapshots/image012.png)

De momentopname wordt ook weergegeven in de back-catalogus in SAP HANA Studio.

![Op de schijf wordt de momentopname weergegeven in de map voor SAP HANA-gegevens](media/sap-hana-backup-storage-snapshots/image013.png)

Op de schijf wordt de momentopname weergegeven in de map voor SAP HANA-gegevens.

Een heeft om ervoor te zorgen dat de consistentie van bestandssysteem ook voordat de storage-momentopname worden uitgevoerd terwijl SAP HANA in de modus voor het voorbereiden van momentopname is is gegarandeerd. Zie _SAP HANA-gegevensconsistentie bij het nemen van opslagmomentopnamen_ in het bijbehorende artikel [back-uphandleiding voor SAP HANA op Azure Virtual Machines](sap-hana-backup-guide.md).

Nadat de storage-momentopname is voltooid, is het essentieel is voor het bevestigen van de SAP HANA-momentopname. Er is een bijbehorende SQL-instructie om uit te voeren: back-up gegevens sluiten te maken (Zie [gegevens sluiten MOMENTOPNAME instructie BACKUP (back-up en herstel)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/c3/9739966f7f4bd5818769ad4ce6a7f8/content.htm)).

> [!IMPORTANT]
> Controleer of de HANA-momentopname. Vanwege &quot;kopiëren bij schrijven&quot; SAP HANA mogelijk extra schijfruimte in snapshot-voorbereiden modus en het is niet mogelijk om te starten van nieuwe back-ups totdat de SAP HANA-momentopname is bevestigd.

## <a name="hana-vm-backup-via-azure-backup-service"></a>HANA VM back-ups via Azure Backup-service

De backup-agent van de Azure Backup-service is niet beschikbaar voor virtuele Linux-machines. Bovendien heeft Linux geen vergelijkbare functionaliteit als Windows het met VSS biedt  Om het gebruik van Azure back-up op het niveau van het bestand/map, een back-upbestanden voor SAP HANA kopiëren naar een Windows-VM en gebruik vervolgens de backup-agent. 

Anders wordt is alleen een volledige Linux-VM back-up mogelijk via de Azure Backup-service. Zie [overzicht van de functies in Azure Backup](../../../backup/backup-introduction-to-azure-backup.md) voor meer informatie.

De Azure Backup-service biedt een optie voor het back-up en herstellen van een virtuele machine. Meer informatie over deze service en hoe dit werkt vindt u in het artikel [plannen van uw VM-back-upinfrastructuur in Azure](../../../backup/backup-azure-vms-introduction.md).

Er zijn twee belangrijke overwegingen op basis van dat artikel:

_&quot;Voor virtuele Linux-machines zijn alleen bestandsconsistente back-ups mogelijk, omdat Linux heeft geen een gelijkwaardige platform VSS.&quot;_

_&quot;Toepassingen moeten voor het implementeren van hun eigen &quot;herstellen&quot; mechanisme op de herstelde gegevens.&quot;_

Daarom heeft een om te controleren of dat SAP HANA in een consistente status op de schijf is wanneer de back-up wordt gestart. Zie _SAP HANA-momentopnamen_ eerder in het document wordt beschreven. Maar er is een mogelijk probleem wanneer SAP HANA, in de modus voor momentopname voorbereiden blijft. Zie [maken van een momentopname van een Storage (SAP HANA Studio)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/a0/3f8f08501e44d89115db3c5aa08e3f/content.htm) voor meer informatie.

Dit artikel staat:

_&quot;Het is raadzaam om te bevestigen of zo snel mogelijk een momentopname van een opslagaccount afbreken nadat deze is gemaakt. Terwijl de opslag-momentopname wordt voorbereid of gemaakt, de momentopname van relevante gegevens worden geblokkeerd. Terwijl de momentopname van relevante gegevens bevroren blijft, kunnen nog steeds wijzigingen aangebracht in de database. Dergelijke wijzigingen worden niet de bevroren momentopname relevante gegevens worden gewijzigd. In plaats daarvan worden de wijzigingen naar posities in het gebied van de gegevens die gescheiden van de momentopname opslag zijn geschreven. Wijzigingen worden ook naar het logboek geschreven. Echter, hoe langer de momentopname-relevante gegevens worden bewaard bevroren, hoe het gegevensvolume kan groeien.&quot;_

Azure Backup zorgt dat de consistentie van bestandssysteem via Azure VM-extensies. Deze uitbreidingen zijn niet beschikbaar zelfstandige en werkt alleen in combinatie met Azure Backup-service. Het is echter nog steeds een vereiste voor scripts voor het maken en verwijderen van een SAP HANA-momentopname voor de garantie voor de consistentie van de app.

Azure Backup bestaat uit vier belangrijke fasen:

- Uitvoeren van script voorbereiden - script moet een SAP HANA-momentopname maken
- Momentopname maken
- Uitvoeren van script volgt op momentopnamen - script moet het verwijderen van de SAP HANA die zijn gemaakt door het script voorbereiden
- Gegevens overdragen naar de kluis

Voor meer informatie over waar u deze scripts kopiëren en meer informatie over hoe Azure Backup precies werkt, controleert u de volgende artikelen:

- [Plannen van uw VM-back-upinfrastructuur in Azure](https://docs.microsoft.com/en-us/azure/backup/backup-azure-vms-introduction)
- [Toepassingsconsistente back-up van Azure Linux VM 's](https://docs.microsoft.com/en-us/azure/backup/backup-azure-linux-app-consistent)



Op dit moment, Microsoft niet gepubliceerd scripts en volgt op momentopnamen scripts voorbereiden voor SAP HANA. U als klant- of system integrator moet deze scripts maken en configureren van de procedure op basis van de hierboven vermelde documentatie.


## <a name="restore-from-application-consistent-backup-against-a-vm"></a>Toepassing consistente back-ups op basis van een virtuele machine terugzetten
Het herstelproces van een toepassing consistente back-up gemaakt door Azure backup wordt beschreven in het artikel [bestanden herstellen vanuit back-up van virtuele Azure-machine](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm). 

> [!IMPORTANT]
> In het artikel [bestanden herstellen vanuit back-up van virtuele Azure-machine](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm) is een lijst met uitzonderingen en de stappen die worden vermeld bij het gebruik van sets met stripe. Striped schijven zijn waarschijnlijk de gewone VM-configuratie voor SAP HANA. Daarom is het essentieel is voor het artikel lezen en testen van het herstelproces voor dergelijke gevallen, zoals vermeld in het artikel. 



## <a name="hana-license-key-and-vm-restore-via-azure-backup-service"></a>HANA-licentiesleutel hebt en de VM herstellen via Azure Backup-service

De Azure Backup-service is ontworpen voor het maken van een nieuwe virtuele machine tijdens het terugzetten. Er zijn geen plannen nu te doen een &quot;ter plekke&quot; terugzetten van een bestaande VM in Azure.

![In deze afbeelding ziet u de optie voor terugzetten van de Azure-service in Azure portal](media/sap-hana-backup-storage-snapshots/image019.png)

In deze afbeelding ziet u de optie voor terugzetten van de Azure-service in Azure portal. Een kan kiezen tussen het maken van een virtuele machine tijdens het terugzetten en herstellen van de schijven. Na het herstellen van de schijven, is het nog steeds nodig om een nieuwe virtuele machine daarboven maken. Wanneer een nieuwe virtuele machine wordt gemaakt op Azure de unieke ID van de virtuele machine wordt gewijzigd (Zie [toegang tot en met behulp van Azure VM unieke ID](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/)).

![In deze afbeelding ziet u de unieke ID van de Azure VM vóór en na het herstel via Azure Backup-service](media/sap-hana-backup-storage-snapshots/image020.png)

In deze afbeelding ziet u de unieke ID van de Azure VM vóór en na het herstel via Azure Backup-service. De sleutel van de hardware SAP, die wordt gebruikt voor SAP-licentieverlening, met behulp van deze unieke id voor de virtuele machine. Als gevolg hiervan is een nieuwe SAP-licentie moet worden geïnstalleerd nadat een virtuele machine herstellen.

Een nieuwe Azure Backup-functie is opgegeven in de voorbeeldmodus tijdens het maken van deze handleiding voor back-up. Hierdoor kan een bestand op terugzetten op basis van de VM-momentopname die is uitgevoerd voor de virtuele machine back-up. Hiermee voorkomt u hoeft te implementeren van een nieuwe virtuele machine, en dus de unieke ID van de virtuele machine hetzelfde blijft en geen nieuwe sleutel van de SAP HANA-licentie is vereist. Meer documentatie over deze functie ontvangt nadat deze volledig is getest.

Azure Backup wordt uiteindelijk back-up van afzonderlijke Azure virtuele schijven, plus bestanden en mappen uit in de virtuele machine toestaan. Een belangrijk voordeel van Azure Backup is het beheer van alle back-ups, voor het opslaan van de klant niet hoeft te doen. Als een herstelpunt die nodig zijn wordt, wordt Azure Backup selecteert u de juiste back-up te gebruiken.

## <a name="sap-hana-vm-backup-via-manual-disk-snapshot"></a>SAP HANA-VM back-up via handmatige schijfmomentopname

In plaats van de Azure Backup-service, kan een een afzonderlijke back-upoplossing configureren door het maken van blobmomentopnamen van Azure-VHD's handmatig via PowerShell. Zie [met blob-momentopnamen met PowerShell](https://blogs.msdn.microsoft.com/cie/2016/05/17/using-blob-snapshots-with-powershell/) voor een beschrijving van de stappen.

Dit biedt meer flexibiliteit, maar de problemen beschreven eerder in dit document niet is opgelost:

- Nog steeds moet u ervoor zorgen dat SAP HANA in een consistente status is door het maken van een momentopname van een SAP HANA
- De besturingssysteemschijf kan niet worden overschreven, zelfs als de VM ongedaan is gemaakt vanwege een fout met de mededeling dat een lease bestaat. Deze functie werkt alleen na het verwijderen van de virtuele machine, die tot een nieuwe unieke ID van virtuele machine en de noodzaak leiden kan voor het installeren van een nieuwe SAP-licentie.

![Het is mogelijk om terug te zetten van de gegevensschijven van een Azure VM](media/sap-hana-backup-storage-snapshots/image021.png)

Het is mogelijk om terug te zetten van de gegevensschijven van een Azure-VM, voorkomen van het probleem van het ophalen van een nieuwe unieke ID van virtuele machine en daarom ongeldig de SAP-licentie:

- Voor het testen, twee Azure-gegevensschijven zijn gekoppeld aan een virtuele machine en software-RAID boven op ze is gedefinieerd 
- Het is bevestigd dat SAP HANA in een consistente status door de functie voor SAP HANA-momentopname
- Bestandssysteem stilzetten (Zie _SAP HANA-gegevensconsistentie bij het nemen van opslagmomentopnamen_ in het bijbehorende artikel [back-uphandleiding voor SAP HANA op Azure Virtual Machines](sap-hana-backup-guide.md))
- BLOB-momentopnamen zijn afkomstig uit zowel gegevensschijven
- Bestandssysteem Kolomblokkering opheffen
- Bevestiging van SAP HANA-momentopname
- Voor het herstellen van de gegevensschijven, de virtuele machine is afgesloten en beide schijven losgekoppeld
- Nadat de schijven ontkoppelen, zijn ze overschreven met de oude blobmomentopnamen
- Vervolgens zijn de herstelde virtuele schijven opnieuw gekoppeld aan de virtuele machine
- Na het starten van de virtuele machine, alles op de software RAID goed zijn uitgevoerd en is opnieuw ingesteld op het moment van blob-momentopname
- HANA is opnieuw ingesteld op de HANA-momentopname

Als deze mogelijk om SAP HANA voor de blobmomentopnamen af te sluiten is, zou de procedure minder complex zijn. Een kan zich in dat geval de HANA-momentopname overslaan en, als niets in het systeem gebeurt er ook de blokkering van file system overslaan. Extra complexiteit wordt geleverd in de afbeelding wanneer deze nodig zijn om u te doen momentopnamen terwijl alles online is. Zie _SAP HANA-gegevensconsistentie bij het nemen van opslagmomentopnamen_ in het bijbehorende artikel [back-uphandleiding voor SAP HANA op Azure Virtual Machines](sap-hana-backup-guide.md).

## <a name="next-steps"></a>Volgende stappen
* [Back-uphandleiding voor SAP HANA op Azure Virtual Machines](sap-hana-backup-guide.md) biedt een overzicht en informatie over aan de slag.
* [Back-up van SAP HANA op bestandsniveau op basis van](sap-hana-backup-file-level.md) bevat informatie over de bestanden zijn gebaseerd back-upoptie.
* Zie voor meer informatie over het opzetten van hoge beschikbaarheid en plan voor herstel na noodgevallen van SAP HANA op Azure (grote instanties), [SAP HANA (grote instanties) hoge beschikbaarheid en herstel na noodgeval op Azure](hana-overview-high-availability-disaster-recovery.md).
