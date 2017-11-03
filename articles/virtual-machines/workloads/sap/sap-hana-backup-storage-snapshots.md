---
title: SAP HANA Azure back-up op basis van opslag-momentopnamen | Microsoft Docs
description: Er zijn twee primaire back-mogelijkheden voor SAP HANA op virtuele machines in Azure, in dit artikel bevat informatie over SAP HANA-back-up op basis van opslag-momentopnamen
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ums.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 3/13/2017
ms.author: rclaus
ms.openlocfilehash: f332b8ac091b75a23489ac27f15ad1fd10d24ec6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="sap-hana-backup-based-on-storage-snapshots"></a>Back-up van SAP HANA op basis van opslagmomentopnamen

## <a name="introduction"></a>Inleiding

Dit maakt deel uit van een reeks drie delen met verwante artikelen op SAP HANA back-up. [Back-handleiding voor SAP HANA op Azure Virtual Machines](sap-hana-backup-guide.md) biedt een overzicht en informatie over het aan de slag, en [SAP HANA Azure Backup op bestandsniveau](sap-hana-backup-file-level.md) wordt ingegaan op de optie back-up-bestand.

Wanneer u een VM-back-upfunctie voor een single instance alles in één demo-systeem, moet een rekening houden met een VM-back-in plaats van het beheren van HANA back-ups op het niveau van het besturingssysteem uitvoeren. Een alternatief is voor Azure blob-momentopnamen kopieën van afzonderlijke virtuele schijven die zijn gekoppeld aan een virtuele machine, te maken en de gegevensbestanden HANA bewaren. Maar een kritiek punt app consistentie bij het maken van een VM-momentopname voor back-up of schijf terwijl het systeem actief is en actief is. Zie _SAP HANA gegevensconsistentie bij het maken van opslag-momentopnamen_ in het bijbehorende artikel [back-up-handleiding voor SAP HANA op Azure Virtual Machines](sap-hana-backup-guide.md). SAP HANA bevat een functie die ondersteuning biedt voor dit soort opslag-momentopnamen.

## <a name="sap-hana-snapshots"></a>SAP HANA-momentopnamen

Er is een functie in SAP HANA die ondersteuning biedt voor maken van een momentopname van de opslag. Vanaf December 2016 is er echter een beperking tot één container systemen. Multitenant container configuraties bieden geen ondersteuning voor dit soort databasemomentopname (Zie [momentopname maken van een opslaggroep (SAP HANA Studio)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/a0/3f8f08501e44d89115db3c5aa08e3f/content.htm)).

Dit werkt als volgt:

- Voorbereiden op een momentopname van de opslag door het starten van de SAP HANA-momentopname
- Uitvoeren van de momentopname van de opslag (in Azure blob-momentopname bijvoorbeeld)
- Bevestig de SAP HANA-momentopname

![Deze schermafbeelding ziet u dat de momentopname van een SAP HANA-gegevens kan worden gemaakt via een SQL-instructie](media/sap-hana-backup-storage-snapshots/image011.png)

Deze schermafbeelding ziet u dat de momentopname van een SAP HANA-gegevens kan worden gemaakt via een SQL-instructie.

![De momentopname wordt ook weergegeven in de back-upcatalogus in SAP HANA-Studio](media/sap-hana-backup-storage-snapshots/image012.png)

De momentopname wordt ook weergegeven in de back-upcatalogus in SAP HANA Studio.

![Op schijf, de momentopname wordt weergegeven in de map voor SAP HANA-gegevens](media/sap-hana-backup-storage-snapshots/image013.png)

Op schijf, de momentopname wordt weergegeven in de map voor SAP HANA-gegevens.

Een heeft om ervoor te zorgen dat de consistentie van het bestandssysteem ook voordat u de momentopname van de opslag terwijl SAP HANA in de modus van de voorbereiding van momentopname is is gegarandeerd. Zie _SAP HANA gegevensconsistentie bij het maken van opslag-momentopnamen_ in het bijbehorende artikel [back-up-handleiding voor SAP HANA op Azure Virtual Machines](sap-hana-backup-guide.md).

Als de momentopname van de opslag is voltooid, is het essentieel om te bevestigen dat de SAP HANA-momentopname. Er is een overeenkomstige SQL-instructie om uit te voeren: sluiten MOMENTOPNAME van de gegevens back-up (Zie [gegevens sluiten MOMENTOPNAME instructie BACKUP (back-up en herstel)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/c3/9739966f7f4bd5818769ad4ce6a7f8/content.htm)).

> [!IMPORTANT]
> Bevestig de momentopname HANA. Vanwege &quot;kopiëren bij schrijven&quot; SAP HANA mogelijk extra schijfruimte in momentopname-voorbereiden modus en het is niet mogelijk worden nieuwe back-ups gestart totdat de SAP HANA-momentopname is bevestigd.

## <a name="hana-vm-backup-via-azure-backup-service"></a>HANA VM back-ups via Azure Backup-service

Vanaf December 2016, zullen de back-upagent van de Azure Backup-service is niet beschikbaar voor virtuele Linux-machines. Om ervoor te gebruiken van Azure back-up op het niveau van het bestand of de map, zou een SAP HANA back-upbestanden kopiëren naar een virtuele machine van Windows en gebruik vervolgens de backup-agent. Anders wordt is alleen een volledige Linux VM back-up mogelijk via de Azure Backup-service. Zie [overzicht van de functies in Azure Backup](../../../backup/backup-introduction-to-azure-backup.md) voor meer informatie.

De Azure Backup-service biedt een optie voor back-up en herstellen van een virtuele machine. Meer informatie over deze service en hoe het werkt vindt u in het artikel [plannen van uw back-infrastructuur van de virtuele machine in Azure](../../../backup/backup-azure-vms-introduction.md).

Er zijn twee belangrijke overwegingen volgens dat artikel:

_&quot;Voor virtuele Linux-machines zijn alleen bestandsconsistente back-ups mogelijk, omdat de Linux beschikt niet over een equivalent platform VSS.&quot;_

_&quot;Toepassingen wilt implementeren hun eigen &quot;-reparatietabel&quot; mechanisme op de herstelde gegevens.&quot;_

Daarom heeft een om te controleren of dat SAP HANA is in een consistente status op de schijf wanneer de back-up wordt gestart. Zie _SAP HANA momentopnamen_ eerder in het document is beschreven. Maar er is een mogelijk probleem wanneer SAP HANA in deze momentopname voorbereiding-modus blijft. Zie [momentopname maken van een opslaggroep (SAP HANA Studio)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/a0/3f8f08501e44d89115db3c5aa08e3f/content.htm) voor meer informatie.

Dit artikel staat:

_&quot;Het is raadzaam om te bevestigen of een opslag-momentopnamen zo snel mogelijk afbreken nadat deze is gemaakt. Terwijl de opslag momentopname wordt voorbereid of gemaakt, wordt de momentopname relevante gegevens bevroren. Terwijl de momentopname relevante gegevens bevroren blijft, kunnen nog steeds wijzigingen aangebracht in de database. Dergelijke wijzigingen leidt niet tot de bevroren momentopname relevante gegevens worden gewijzigd. In plaats daarvan worden de wijzigingen worden geschreven naar een positie in het gegevensgebied die gescheiden van de momentopname van de opslag zijn. Wijzigingen zijn ook geschreven naar het logboek. Echter, hoe langer de momentopname relevante gegevens wordt opgeslagen bevroren, hoe meer gegevensvolume kan groeien.&quot;_

Azure back-up zorgt voor de consistentie van het bestandssysteem via Azure VM-extensies. Deze uitbreidingen zijn niet beschikbaar zelfstandige en werkt alleen in combinatie met Azure Backup-service. Het is echter nog steeds een vereiste voor het beheren van een momentopname van een SAP HANA om te garanderen van de consistentie van de app.

Azure Backup bestaat uit twee belangrijke fasen:

- Momentopname
- Gegevens overdragen naar de kluis

Een kan de momentopname SAP HANA Controleer nadat de Azure Backup-service-fase van het maken van een momentopname is voltooid. Het duurt enkele minuten om te zien in de Azure portal.

![Deze afbeelding ziet u deel van de lijst van de back-uptaak van een Azure Backup-service](media/sap-hana-backup-storage-snapshots/image014.png)

Deze afbeelding ziet u deel uitmaakt van de lijst van de back-uptaak van een Azure Backup-service, die werd gebruikt voor het testen van back-up van de HANA VM.

![Als u wilt de taakdetails weergeven, klikt u op de back-uptaak in de Azure portal](media/sap-hana-backup-storage-snapshots/image015.png)

Als u wilt de taakdetails weergeven, klikt u op de back-uptaak in de Azure portal. Hier ziet een twee fasen. Het duurt enkele minuten totdat de momentopname-fase voltooid te zien. De meeste gevallen is besteed in de fase van de gegevensoverdracht.

## <a name="hana-vm-backup-automation-via-azure-backup-service"></a>Back-automatisering HANA VM via Azure Backup-service

Een kan de momentopname SAP HANA handmatig bevestigen zodra de Azure Backup momentopname-fase is voltooid, zoals eerder beschreven, maar het is handig om rekening houden automation omdat een beheerder niet voor de back-uptaak lijst in de Azure-portal controleren mogelijk.

Hier volgt een uitleg hoe deze kan worden bereikt via Azure PowerShell-cmdlets.

![Een Azure Backup-service is gemaakt met de naam hana-backup-kluis](media/sap-hana-backup-storage-snapshots/image016.png)

Een Azure Backup-service is gemaakt met de naam van de &quot;hana-backup-kluis.&quot; De PS-opdracht **Get-AzureRmRecoveryServicesVault-naam hana-backup-kluis** opgehaald van het bijbehorende object. Dit object wordt vervolgens gebruikt om in te stellen van de back-context weergegeven op de volgende afbeelding.

![Een kunt controleren of de back-uptaak momenteel bezig](media/sap-hana-backup-storage-snapshots/image017.png)

Na het instellen van de juiste context kan een controle voor de back-uptaak momenteel bezig en zoekt u de taakdetails. De subtaak-lijst bevat als de momentopname-fase van de Azure back-uptaak is al voltooid:

```
$ars = Get-AzureRmRecoveryServicesVault -Name hana-backup-vault
Set-AzureRmRecoveryServicesVaultContext -Vault $ars
$jid = Get-AzureRmRecoveryServicesBackupJob -Status InProgress | select -ExpandProperty jobid
Get-AzureRmRecoveryServicesBackupJobDetails -Jobid $jid | select -ExpandProperty subtasks
```

![De waarde in een lus pollen totdat deze in voltooid verandert](media/sap-hana-backup-storage-snapshots/image018.png)

Zodra de taakgegevens zijn opgeslagen in een variabele, is gewoon PS syntaxis voor het ophalen van het eerste item in de matrix en de waarde van de status op te halen. Als u het automatiseringsscript, de waarde in een lus pollen totdat deze verandert in &quot;voltooid.&quot;

```
$st = Get-AzureRmRecoveryServicesBackupJobDetails -Jobid $jid | select -ExpandProperty subtasks
$st[0] | select -ExpandProperty status
```

## <a name="hana-license-key-and-vm-restore-via-azure-backup-service"></a>De licentiesleutel HANA en VM herstellen via de Azure Backup-service

De Azure Backup-service is ontworpen om een nieuwe virtuele machine maken tijdens het terugzetten. Er is geen nu te doen een &quot;in-place&quot; terugzetten van een bestaande virtuele machine in Azure.

![Deze afbeelding ziet u de optie voor terugzetten van de Azure-service in de Azure portal](media/sap-hana-backup-storage-snapshots/image019.png)

Deze afbeelding ziet u de optie voor terugzetten van de Azure-service in de Azure portal. Een kunt kiezen tussen het maken van een virtuele machine tijdens het terugzetten of het herstellen van de schijven. Na het terugzetten van de schijven is het nog steeds nodig voor het maken van een nieuwe virtuele machine toe. Wanneer een nieuwe virtuele machine wordt gemaakt in Azure de unieke ID van VM wordt gewijzigd (Zie [toegang tot en met behulp van Azure VM unieke ID](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/)).

![Deze afbeelding ziet u de unieke ID van de virtuele machine van Azure voor en na het herstellen via de Azure Backup-service](media/sap-hana-backup-storage-snapshots/image020.png)

Deze afbeelding ziet u de unieke ID van de virtuele machine van Azure voor en na het herstellen via de Azure Backup-service. De sleutel van de hardware SAP, die wordt gebruikt voor SAP-licentieverlening, met behulp van deze unieke id voor de virtuele machine. Als gevolg hiervan is een nieuwe SAP-licentie moet worden geïnstalleerd nadat een virtuele machine herstellen.

Een nieuwe Azure Backup-functie is opgenomen in de voorbeeldmodus tijdens het maken van deze back-handleiding. Kunt u een bestand niveau te herstellen op basis van de VM-momentopname die is uitgevoerd voor de virtuele machine back-up. Hiermee voorkomt u de noodzaak voor het implementeren van een nieuwe virtuele machine, en daarom de unieke ID voor VM hetzelfde blijft en geen nieuwe sleutel voor SAP HANA-licentie is vereist. Meer documentatie over deze functie wordt worden opgegeven nadat het volledig is getest.

Azure Backup wordt uiteindelijk back-up van afzonderlijke Azure virtuele schijven, plus bestanden en mappen uit in de virtuele machine toestaan. Een belangrijk voordeel van Azure Backup is het beheer van alle back-ups, voor het opslaan van de klant niet hoeft te doen. Als een herstelpunt nodig wordt, wordt Azure Backup selecteert u de juiste back-up moet worden gebruikt.

## <a name="sap-hana-vm-backup-via-manual-disk-snapshot"></a>SAP HANA VM back-ups via handmatige schijf momentopname

In plaats van de Azure Backup-service, kan een een afzonderlijke back-upoplossing configureren door het maken van momentopnamen van de blob van Azure VHD's handmatig via PowerShell. Zie [Using blob momentopnamen met PowerShell](https://blogs.msdn.microsoft.com/cie/2016/05/17/using-blob-snapshots-with-powershell/) voor een beschrijving van de stappen.

Biedt meer flexibiliteit, maar de problemen beschreven eerder in dit document kan niet worden omgezet:

- Een moet nog steeds ervoor zorgen dat de SAP HANA zich in een consistente status
- De besturingssysteemschijf kan niet worden overschreven, zelfs als de virtuele machine vanwege een fout die aangeeft dat een lease bestaat toewijzing ongedaan wordt gemaakt. Deze functie werkt alleen na het verwijderen van de virtuele machine, die tot een nieuwe unieke ID van virtuele machine en de noodzaak leiden kan voor het installeren van een nieuwe SAP-licentie.

![Het is mogelijk te herstellen van de gegevensschijven van een Azure VM](media/sap-hana-backup-storage-snapshots/image021.png)

Het is mogelijk te herstellen van de gegevensschijven van een virtuele machine van Azure, waardoor het probleem van het ophalen van een nieuwe unieke ID voor VM voorkomen en de SAP-licentie daarom ongeldig is gemaakt:

- Twee Azure gegevensschijven zijn gekoppeld aan een VM voor de test en software-RAID boven op ze is gedefinieerd 
- Is bevestigd dat SAP HANA in een consistente status door de functie voor SAP HANA-momentopname
- Bestandssysteem blokkeren (Zie _SAP HANA gegevensconsistentie bij het maken van opslag-momentopnamen_ in het bijbehorende artikel [back-up-handleiding voor SAP HANA op Azure Virtual Machines](sap-hana-backup-guide.md))
- BLOB momentopnamen zijn vanaf beide gegevensschijven
- Bestandssysteem blokkering opheffen
- Bevestiging voor SAP HANA-momentopname
- Voor het herstellen van de gegevensschijven, de virtuele machine is afgesloten en beide schijven losgekoppeld
- Na het loskoppelen van de schijven zijn ze overschreven met de voormalige blob-momentopnamen
- Vervolgens zijn de herstelde virtuele schijven opnieuw gekoppeld aan de virtuele machine
- Na het starten van de virtuele machine alles op de software RAID prima en opnieuw is ingesteld op de tijd van de momentopname blob
- HANA is opnieuw ingesteld op de momentopname HANA

Als deze mogelijk SAP HANA afsluiten voordat de blob-momentopnamen, zou de procedure minder complex zijn. In dat geval kan een de momentopname HANA overslaan en ook het bestand systeem blokkeren als niets in het systeem gebeurt er overslaan. Extra complexiteit wordt geleverd in de afbeelding als hoeft te doen momentopnamen terwijl alles online is. Zie _SAP HANA gegevensconsistentie bij het maken van opslag-momentopnamen_ in het bijbehorende artikel [back-up-handleiding voor SAP HANA op Azure Virtual Machines](sap-hana-backup-guide.md).

## <a name="next-steps"></a>Volgende stappen
* [Back-handleiding voor SAP HANA op Azure Virtual Machines](sap-hana-backup-guide.md) biedt een overzicht en informatie over aan de slag.
* [SAP HANA back-up die is gebaseerd op bestandsniveau](sap-hana-backup-file-level.md) wordt ingegaan op de optie back-up-bestand.
* Zie voor meer informatie over hoe u hoge beschikbaarheid en herstel na noodgevallen van SAP HANA plannen in Azure (grote exemplaren), [SAP HANA (grote exemplaren) hoge beschikbaarheid en herstel na noodgevallen op Azure](hana-overview-high-availability-disaster-recovery.md).
