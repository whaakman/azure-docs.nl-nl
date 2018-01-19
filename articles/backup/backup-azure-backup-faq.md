---
title: Veelgestelde vragen over Azure Backup | Microsoft Docs
description: 'Antwoorden op veelgestelde vragen over de functies van Azure Backup, waaronder de Recovery Services-kluis, waarvan een back-up kan worden gemaakt, hoe het werkt, versleuteling en limieten. '
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: back-up en herstel na noodgeval; Backup-service
ms.assetid: 1011bdd6-7a64-434f-abd7-2783436668d7
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/21/2017
ms.author: markgal;arunak;trinadhk;sogup;
ms.openlocfilehash: 66c2f1c5e8ba26d5c50cf60b7f448406814408b0
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="questions-about-the-azure-backup-service"></a>Vragen over de Azure Backup-service
In dit artikel antwoorden op veelgestelde vragen over de Azure Backup-onderdelen. Sommige antwoorden bevatten koppelingen naar artikelen met uitgebreide informatie over het onderwerp. U kunt via **Opmerkingen** (aan de rechterkant) vragen stellen over Azure Backup. De opmerkingen worden onder aan dit artikel weergegeven. Er is een Livefyre-account vereist om opmerkingen te kunnen plaatsen. U kunt ook in het [discussieforum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup) vragen over de Azure Backup-service plaatsen.

Als u kort de secties in dit artikel wilt bekijken, gebruikt u de koppelingen aan de rechterkant, onder **In dit artikel**.


## <a name="recovery-services-vault"></a>Recovery Services-kluis

### <a name="is-there-any-limit-on-the-number-of-vaults-that-can-be-created-in-each-azure-subscription-br"></a>Is er een limiet voor het aantal kluizen dat voor elk Azure-abonnement kan worden gemaakt? <br/>
Ja. Vanaf September 2016, kunt u 25 Recovery Services-kluizen per abonnement. U kunt per abonnement maximaal 25 Recovery Services-kluizen per ondersteunde regio van Azure Backup maken. Als u extra kluizen nodig hebt, maakt u een extra abonnement.

### <a name="are-there-limits-on-the-number-of-serversmachines-that-can-be-registered-against-each-vault-br"></a>Zijn er beperkingen met betrekking tot het aantal servers/machines dat kan worden geregistreerd voor elke kluis? <br/>
Ja, u kunt maximaal vijftig machines per kluis registreren. Voor virtuele machines van Azure IaaS geldt een limiet van 200 virtuele machines per kluis. Als u meer machines wilt registreren, maakt u een nieuwe kluis.

### <a name="if-my-organization-has-one-vault-how-can-i-isolate-one-servers-data-from-another-server-when-restoring-databr"></a>Als mijn organisatie één kluis heeft, hoe kan ik bij het herstellen van gegevens de gegevens van de ene server dan isoleren van de gegevens van een andere server?<br/>
Alle servers die zijn geregistreerd bij dezelfde kluis, kunnen de gegevensback-ups herstellen van andere servers *die gebruikmaken van dezelfde wachtwoordzin*. Als u servers hebt waarvan u de back-upgegevens wilt isoleren van andere servers in uw organisatie, gebruikt u een speciale wachtwoordzin voor die servers. U kunt bijvoorbeeld verschillende wachtwoordzinnen voor de human resource-server, de accountingserver en de opslagserver gebruiken.

### <a name="can-i-migrate-my-backup-data-or-vault-between-subscriptions-br"></a>Kan ik mijn back-upgegevens- of back-upkluis 'migreren' tussen abonnementen? <br/>
Nee. De kluis is gemaakt op abonnementsniveau en kan, zodra deze is gemaakt, niet opnieuw worden toegewezen aan een ander abonnement.

### <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-still-supported-br"></a>Recovery Services-kluizen zijn op basis van Resource Manager. Worden Backup-kluizen nog steeds ondersteund? <br/>
Back-upkluizen zijn geconverteerd naar een Recovery Services-kluizen. Als u de Backup-kluis niet naar een Recovery Services-kluis converteren hebt, is klikt u vervolgens de Backup-kluis geconverteerd naar een Recovery Services-kluis voor u. 

### <a name="can-i-migrate-a-backup-vault-to-a-recovery-services-vault-br"></a>Kan ik een Backup-kluis migreren naar een Recovery Services-kluis? <br/>
Alle Backup-kluizen zijn geconverteerd naar een Recovery Services-kluizen. Als u de Backup-kluis niet naar een Recovery Services-kluis converteren hebt, is klikt u vervolgens de Backup-kluis geconverteerd naar een Recovery Services-kluis voor u.

## <a name="azure-backup-agent"></a>Azure Backup-agent
Een uitgebreide lijst met vragen vindt u in [FAQ on Azure file-folder backup](backup-azure-file-folder-backup-faq.md) (Veelgestelde vragen over het maken van back-ups van Azure-bestandsmappen)

## <a name="azure-vm-backup"></a>Azure VM Backup
Een uitgebreide lijst met vragen vindt u in [Veelgestelde vragen over Azure VM Backup](backup-azure-vm-backup-faq.md)

## <a name="back-up-vmware-servers"></a>Back-ups maken van VMware-servers

### <a name="can-i-back-up-vmware-vcenter-servers-to-azure"></a>Kan ik een back-up maken van VMware vCenter-servers naar Azure?

Ja. U kunt Azure Backup Server gebruiken om back-ups te maken van VMware vCenter en ESXi naar Azure. Zie [Beveiligingsmatrix voor Azure Backup Server](backup-mabs-protection-matrix.md) voor meer informatie over de ondersteunde versies van VMware. Zie [Azure Backup Server gebruiken om back-ups te maken van een VMware-server](backup-azure-backup-server-vmware.md) voor stapsgewijze instructies.


## <a name="azure-backup-server-and-system-center-data-protection-manager"></a>Azure Backup-server en System Center Data Protection Manager
### <a name="can-i-use-azure-backup-server-to-create-a-bare-metal-recovery-bmr-backup-for-a-physical-server-br"></a>Kan ik de Azure Backup-server gebruiken om een BMR-back-up (Bare Metal Recovery) te maken voor een fysieke server? <br/>
Ja.

### <a name="can-i-register-my-dpm-server-to-multiple-vaults-br"></a>Kan ik mijn DPM-server registreren voor meerdere kluizen? <br/>
Nee. Een DPM- of MABS-server kan voor slechts één kluis worden geregistreerd.

### <a name="which-version-of-system-center-data-protection-manager-is-supported-br"></a>Welke versie van System Center Data Protection Manager wordt ondersteund? <br/>
U kunt het beste de [nieuwste](http://aka.ms/azurebackup_agent) versie van de Azure Backup-agent installeren met het nieuwste updatepakket voor System Center Data Protection Manager (DPM). Updatepakket 11 is vanaf augustus 2016 de nieuwste update.

### <a name="i-have-installed-azure-backup-agent-to-protect-my-files-and-folders-can-i-now-install-system-center-dpm-to-work-with-azure-backup-agent-to-protect-on-premises-applicationvm-workloads-to-azure-br"></a>Ik heb de Azure Backup-agent geïnstalleerd om bestanden en mappen te beveiligen. Kan ik System Center DPM nu installeren voor samenwerking met de Azure Backup-agent om workloads van on-premises toepassingen of virtuele machines naar Azure te beveiligen? <br/>
Installeer eerst System Center Data Protection Manager (DPM) en vervolgens de Azure Backup-agent om Azure Backup met DPM te gebruiken. Als u de Azure Backup-onderdelen in deze volgorde installeert, zorgt u ervoor dat de Azure Backup-agent werkt met DPM. Het installeren van de Azure Backup-agent vóór DPM wordt niet geadviseerd of ondersteund.


## <a name="how-azure-backup-works"></a>De werking van Azure Backup
### <a name="if-i-cancel-a-backup-job-once-it-has-started-is-the-transferred-backup-data-deleted-br"></a>Als ik een back-uptaak annuleer nadat deze is gestart, worden de overgedragen back-upgegevens dan verwijderd? <br/>
Nee. Alle gegevens die naar de kluis zijn overgebracht voordat de back-uptaak is geannuleerd, blijven aanwezig in de kluis. Azure Backup gebruikt een controlepuntmechanisme waarmee tijdens de back-up van tijd tot tijd controlepunten worden toegevoegd aan de back-upgegevens. Omdat de back-upgegevens controlepunten bevatten, kan tijdens het volgende back-upproces de integriteit van de bestanden worden gecontroleerd. De volgende back-uptaak is incrementeel ten opzichte van de gegevens waarvan eerder een back-up is gemaakt. Incrementele back-ups dragen alleen nieuwe of gewijzigde gegevens over, zodat de bandbreedte beter wordt benut.

Als u een back-uptaak voor een virtuele Azure-machine annuleert, worden eventuele overgedragen gegevens geannuleerd. De volgende back-uptaak draagt incrementele gegevens over van na de vorige succesvolle back-up-taak.

### <a name="are-there-limits-on-when-or-how-many-times-a-backup-job-can-be-scheduledbr"></a>Gelden er limieten voor het moment waarop een back-uptaak kan worden gepland en voor het aantal keren?<br/>
Ja. U kunt maximaal drie keer per dag back-uptaken uitvoeren op werkstations met Windows Server of Windows. Op System Center DPM kunt u maximum twee back-uptaken per dag uitvoeren. Voor IaaS VM's kunt u maximaal één keer per dat een back-uptaak uitvoeren. U kunt het planningsbeleid voor Windows Server- of Windows-werkstations gebruiken om een dag- of weekplanning te maken. Als u System Center DPM gebruikt, kunt u een dag-, week-, maand- en jaarplanning maken.

### <a name="why-is-the-size-of-the-data-transferred-to-the-recovery-services-vault-smaller-than-the-data-i-backed-upbr"></a>Waarom is de omvang van de gegevens die worden overgebracht naar de Recovery Services-kluis, kleiner dan de hoeveelheid gegevens waarvan ik een back-up heb gemaakt?<br/>
 Alle gegevens waarvan een back-up is gemaakt via Azure Backup-agent, SCDPM of Azure Backup-server, worden gecomprimeerd en versleuteld voordat ze worden overgedragen. Nadat de compressie en codering is toegepast, is de gegevens in de Recovery Services-kluis 30 tot 40% kleiner.

## <a name="what-can-i-back-up"></a>Waar kan ik een back-up van maken?
### <a name="which-operating-systems-do-azure-backup-support-br"></a>Welke besturingssystemen worden ondersteund door Azure Backup? <br/>
Azure Backup ondersteunt de volgende besturingssystemen voor het maken van back-ups van bestanden, mappen en workloadtoepassingen die worden beschermd met behulp van Azure Backup Server en System Center Data Protection Manager (DPM).

| Besturingssysteem | Platform | SKU |
|:--- | --- |:--- |
| Windows 8 en de meest recente SP's |64 bits |Enterprise, Pro |
| Windows 7 en de meest recente SP's |64 bits |Ultimate, Enterprise, Professional, Home Premium Home Basic, Starter |
| Windows 8.1 en de meest recente SP's |64 bits |Enterprise, Pro |
| Windows 10 |64 bits |Enterprise, Pro, Home |
| Windows Server 2016 |64 bits |Standard, Datacenter, Essentials |
| Windows Server 2012 R2 en de meest recente SP's |64 bits |Standard, Datacenter, Foundation |
| Windows Server 2012 en de meest recente SP's |64 bits |Datacenter, Foundation, Standard |
| Windows Storage Server 2016 en de meest recente SP's |64 bits |Standard, Workgroup | 
| Windows Storage Server 2012 R2 en de meest recente SP's |64 bits |Standard, Workgroup |
| Windows Storage Server 2012 en de meest recente SP's |64 bits |Standard, Workgroup |
| Windows Server 2012 R2 en de meest recente SP's |64 bits |Essential |
| Windows Server 2008 R2 SP1 |64 bits |Standard, Enterprise, Datacenter, Foundation |

**Back-up van virtuele Azure-machines:**

* **Linux**: Azure Backup ondersteunt [een lijst met distributies die zijn goedgekeurd door Azure](../virtual-machines/linux/endorsed-distros.md), met uitzondering van Core OS Linux.  Andere Bring-Your-Own-Linux-distributies werken mogelijk ook, mits de VM-agent beschikbaar is op de virtuele machine en ondersteuning voor Python aanwezig is.
* **Windows Server**: versies ouder dan Windows Server 2008 R2 worden niet ondersteund.


### <a name="is-there-a-limit-on-the-size-of-each-data-source-being-backed-up-br"></a>Geldt er een limiet voor de grootte van elke gegevensbron waarvan een back-up wordt gemaakt? <br/>
Er geldt geen limiet voor de hoeveelheid gegevens waarvan u een back-up kunt maken naar een kluis. Azure Backup beperkt de maximumgrootte voor de gegevensbron. Deze limieten zijn echter hoog. Vanaf augustus 2015 geldt er een maximale gegevensbrongrootte voor de ondersteunde besturingssystemen van:

| S.Nee | Besturingssysteem | Maximale grootte van de gegevensbron |
|:---:|:--- |:--- |
| 1 |Windows Server 2012 of hoger |54.400 GB |
| 2 |Windows 8 of hoger |54.400 GB |
| 3 |Windows Server 2008, Windows Server 2008 R2 |1700 GB |
| 4 |Windows 7 |1700 GB |

In de volgende tabel wordt uitgelegd hoe de grootte voor elke gegevensbron wordt bepaald.

| Gegevensbron | Details |
|:---:|:--- |
| Volume |De hoeveelheid gegevens van één volume van een server of clientcomputer waarvan een back-up wordt gemaakt. |
| Virtuele Hyper-V-machine  |De som van de gegevens van alle VHD's van de virtuele machine waarvan een back-up wordt gemaakt. |
| Microsoft SQL Server-database |De grootte van één SQL-database waarvan een back-up wordt gemaakt. |
| Microsoft SharePoint |De som van de inhoud en configuratiedatabases in een SharePoint-farm waarvan een back-up wordt gemaakt. |
| Microsoft Exchange |De som van alle Exchange-databases in een Exchange-server waarvan een back-up wordt gemaakt. |
| BMR/systeemstatus |Elke afzonderlijke kopie van de BMR of systeemstatus van de machine waarvan een back-up wordt gemaakt. |

Voor Azure VM Backup kan elke virtuele machine maximaal 16 gegevensschijven bevatten met een maximale grootte van 1023 GB elk. 

## <a name="retention-policy-and-recovery-points"></a>Retentiebeleid en herstelpunten
### <a name="is-there-a-difference-between-the-retention-policy-for-dpm-and-windows-serverclient-that-is-on-windows-server-without-dpmbr"></a>Is er een verschil tussen het retentie- of bewaarbeleid voor DPM en Windows Server of de Windows-client (dat wil zeggen op Windows Server zonder DPM)?<br/>
Nee, u kunt zowel voor DPM als voor Windows Server of de Windows-client een dagelijks, wekelijks, maandelijks en jaarlijks bewaarbeleid instellen.

### <a name="can-i-configure-my-retention-policies-selectively--ie-configure-weekly-and-daily-but-not-yearly-and-monthlybr"></a>Kan ik mijn bewaarbeleidsregels selectief configureren, oftewel wekelijks en dagelijks configureren, maar niet jaarlijks en maandelijks?<br/>
Ja, de bewaarstructuur van Azure Backup biedt u de volledige flexibiliteit om het bewaarbeleid te configureren overeenkomstig uw vereisten.

### <a name="can-i-schedule-a-backup-at-6pm-and-specify-retention-policies-at-a-different-timebr"></a>Kan ik 'een back-up plannen' om 18:00 uur en voor het retentiebeleid een ander tijdstip opgeven?<br/>
Nee. Het bewaarbeleid kan alleen worden toegepast op back-uppunten. Het bewaarbeleid in de volgende afbeelding is opgegeven voor de back-ups die tussen 12:00 uur en 18:00 uur zijn gemaakt. <br/>

![Back-up en retentie plannen](./media/backup-azure-backup-faq/Schedule.png)
<br/>

### <a name="if-a-backup-is-retained-for-a-long-duration-does-it-take-more-time-to-recover-an-older-data-point-br"></a>Als een back-up langdurig wordt bewaard, duurt het dan langer om een oud gegevenspunt te herstellen? <br/>
Nee, de tijd die nodig is om het oudste of nieuwste punt te herstellen, is hetzelfde. Elk herstelpunt gedraagt zich als een volledig punt.

### <a name="if-each-recovery-point-is-like-a-full-point-does-it-impact-the-total-billable-backup-storagebr"></a>Als elk herstelpunt een volledig punt is, heeft dit dan invloed op de totale factureerbare back-upopslag?<br/>
Producten met een lange bewaartermijn slaan de back-upgegevens doorgaans op als volledige punten. De volledige punten maken *inefficiënt* gebruik van de opslagruimte, maar kunnen wel gemakkelijker en sneller worden hersteld. Incrementele kopieën maken *efficiënt* gebruik van de opslagruimte, maar vereisen dat u een keten van gegevens hersteld, wat van invloed is op de hersteltijd. De opslagarchitectuur van Azure Backup biedt u het beste van beide werelden door de gegevens optimaal op te slaan voor snelle herstelbewerkingen en lage opslagkosten. Deze benadering van gegevensopslag zorgt ervoor dat uw bandbreedte voor inkomend en uitgaand verkeer efficiënt wordt gebruikt. Zowel de hoeveelheid gegevens die moet worden opgeslagen als de benodigde tijd om de gegevens te herstellen, blijft tot een minimum beperkt. Meer informatie over waarom het gebruik van [incrementele back-ups](https://azure.microsoft.com/blog/microsoft-azure-backup-save-on-long-term-storage/) efficiënt is.

### <a name="is-there-a-limit-on-the-number-of-recovery-points-that-can-be-createdbr"></a>Geldt er een limiet voor het aantal herstelpunten dat kan worden gemaakt?<br/>
U kunt maximaal 9999 herstelpunten maken per beveiligd exemplaar. Een beveiligd exemplaar is een computer, een server (fysiek of virtueel) of een werkbelasting die is geconfigureerd voor het opslaan van back-ups van gegevens in Azure. Zie voor meer informatie de uitleg van [Back-up en retentie](./backup-introduction-to-azure-backup.md#backup-and-retention) en [Wat is een beveiligd exemplaar?](./backup-introduction-to-azure-backup.md#what-is-a-protected-instance)

### <a name="how-many-recoveries-can-i-perform-on-the-data-that-is-backed-up-to-azurebr"></a>Hoeveel herstelbewerkingen kan ik uitvoeren voor de gegevens waarvan een back-up naar Azure is gemaakt?<br/>
Er is geen limiet voor het aantal herstelbewerkingen vanuit Azure Backup.

### <a name="when-restoring-data-do-i-pay-for-the-egress-traffic-from-azure-br"></a>Als ik gegevens ga herstellen, betaal ik dan voor het uitgaande verkeer van Azure? <br/>
Nee. Uw herstelbewerkingen zijn gratis en er worden geen kosten in rekening gebracht voor het uitgaande verkeer.

## <a name="azure-backup-encryption"></a>Azure Backup-versleuteling
### <a name="is-the-data-sent-to-azure-encrypted-br"></a>Worden de gegevens die naar Azure worden verzonden, versleuteld? <br/>
Ja. Ja, de gegevens worden versleuteld op de on-premises server-/client-/SCDPM-machine met AES256 en de gegevens worden verzonden via een beveiligde HTTPS-koppeling.

### <a name="is-the-backup-data-on-azure-encrypted-as-wellbr"></a>Worden de back-upgegevens op Azure ook versleuteld?<br/>
Ja. De gegevens die naar Azure worden verzonden, blijven versleuteld (at rest). De back-upgegevens worden nooit door Microsoft ontsleuteld. Azure Backup is afhankelijk van de versleuteling van de virtuele machine bij het maken van een back-up van een virtuele Azure-machine. Als de virtuele machine bijvoorbeeld is versleuteld met Azure Disk Encryption of een andere versleutelingstechnologie, gebruikt Azure Backup die versleuteling om uw gegevens te beveiligen.

### <a name="what-is-the-minimum-length-of-encryption-key-used-to-encrypt-backup-data-br"></a>Wat is de minimale lengte van de versleutelingssleutel die wordt gebruikt om de back-upgegevens te versleutelen? <br/>
Als u gebruikmaakt van een Azure Backup-agent, moet de versleutelingssleutel ten minste 16 tekens bevatten. Voor virtuele Azure-machines is er geen limiet voor de lengte van de sleutels die door Azure KeyVault worden gebruikt. 

### <a name="what-happens-if-i-misplace-the-encryption-key-can-i-recover-the-data-or-can-microsoft-recover-the-data-br"></a>Wat gebeurt er als ik de versleutelingssleutel kwijtraak? Kan ik of Microsoft de gegevens herstellen? <br/>
Alleen de klant beschikt over de sleutel die wordt gebruikt om de back-upgegevens te versleutelen. Microsoft bewaart geen kopie in Azure en heeft geen toegang tot de sleutel. Als de klant de sleutel kwijtraakt, kan Microsoft back-upgegevens niet herstellen.
