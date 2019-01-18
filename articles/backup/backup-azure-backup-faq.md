---
title: Veelgestelde vragen over Azure Backup
description: 'Antwoorden op veelgestelde vragen over: Functies van Azure Backup Recovery Services-kluizen, wat back-up kan, hoe het werkt, versleuteling en limieten. '
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 01/08/2019
ms.author: raynew
ms.openlocfilehash: 4e3a79c28fc0e67fbf22e4d0fde3de9528d3edf4
ms.sourcegitcommit: ba9f95cf821c5af8e24425fd8ce6985b998c2982
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54382621"
---
# <a name="azure-backup---frequently-asked-questions"></a>Azure Backup - Veelgestelde vragen
In dit artikel vindt u antwoorden op veelgestelde vragen over de Azure Backup-service.

## <a name="recovery-services-vault"></a>Recovery Services-kluis

### <a name="is-there-any-limit-on-the-number-of-vaults-that-can-be-created-in-each-azure-subscription"></a>Is er een limiet voor het aantal kluizen dat voor elk Azure-abonnement kan worden gemaakt?
Ja. U kunt maximaal 500 Recovery Services-kluizen per ondersteunde regio van Azure Backup per abonnement maken. Als u extra kluizen nodig hebt, maakt u een extra abonnement.

### <a name="are-there-limits-on-the-number-of-serversmachines-that-can-be-registered-against-each-vault"></a>Zijn er beperkingen met betrekking tot het aantal servers/machines dat kan worden geregistreerd voor elke kluis?
U kunt maximaal 1000 Azure Virtual machines per kluis registreren. Als u de Microsoft Azure Backup-Agent gebruikt, kunt u maximaal 50 MAB-agents per kluis registreren. En u kunt de 50 MAB-servers/DPM-servers naar een kluis registreren.


### <a name="if-my-organization-has-one-vault-how-can-i-isolate-data-from-different-servers-in-the-vault-when-restoring-data"></a>Als mijn organisatie één kluis heeft, hoe kan ik isoleren van de gegevens van andere servers in de kluis bij het herstellen van gegevens?

Servergegevens die u wilt herstellen, samen moeten dezelfde wachtwoordzin worden gebruikt bij het instellen van back-up. Als u wilt voor het isoleren van herstel naar een specifieke server of servers, gebruikt u een wachtwoordzin voor die server of alleen servers. U kunt bijvoorbeeld verschillende wachtwoordzinnen voor de human resource-server, de accountingserver en de opslagserver gebruiken.

### <a name="can-i-move-my-vault-between-subscriptions"></a>Kan ik mijn kluis verplaatsen tussen abonnementen?

Nee. De kluis wordt gemaakt op abonnementsniveau en kan niet opnieuw worden toegewezen aan een ander abonnement.

### <a name="can-i-move-backup-data-to-another-vault"></a>Kan ik back-upgegevens verplaatsen naar een andere kluis?

Nee. Back-upgegevens die zijn opgeslagen in een kluis kan niet worden verplaatst naar een andere kluis.

### <a name="can-i-change-from-grs-to-lrs-after-a-backup"></a>Kan ik wijzigen van GRS naar LRS na een back-up?

Nee. Een Recovery Services-kluis kunt opslagopties alleen wijzigen voordat u een back-ups zijn opgeslagen.

### <a name="can-i-do-an-item-level-restore-ilr-for-vms-backed-up-to-a-recovery-services-vault"></a>Kan ik een Item niveau herstellen (ILR) voor virtuele machines een back-up naar een Recovery Services-kluis?
Nee, ILR wordt niet ondersteund.


## <a name="azure-backup-agent"></a>Azure Backup-agent

### <a name="where-can-i-find-common-questions-about-the-azure-backup-agent-for-azure-vm-backup"></a>Waar vind ik Veelgestelde vragen over de Azure backup-agent voor Azure VM backup?

- Voor de agent wordt uitgevoerd op Azure Virtual machines, Lees dit [Veelgestelde vragen over](backup-azure-vm-backup-faq.md).
- Voor de agent die wordt gebruikt voor de back-up van Azure-bestandsmappen, Lees dit [Veelgestelde vragen over](backup-azure-file-folder-backup-faq.md).


## <a name="vmware-and-hyper-v-backup"></a>VMware en Hyper-V-back-up

### <a name="can-i-back-up-vmware-vcenter-servers-to-azure"></a>Kan ik een back-up maken van VMware vCenter-servers naar Azure?

Ja. Azure Backup Server kunt u back-up van VMware vCenter-Server- en ESXi-hosts naar Azure.

- [Meer informatie](backup-mabs-protection-matrix.md) over ondersteunde versies.
- [Volg deze stappen](backup-azure-backup-server-vmware.md) back-up een VMware-server.

### <a name="do-i-need-a-separate-license-to-recover-an-full-on-premises-vmwarehyper-v-cluster"></a>Moet ik een afzonderlijke licentie voor het herstellen van een volledige on-premises VMware/Hyper-V-cluster?

U moet afzonderlijke licenties voor VMware/Hyper-V-beveiliging.

- Als u een System Center-klant bent, gebruikt u System Center Data Protection Manager (DPM) om te beveiligen van virtuele VMware-machines.
- Als u niet een System Center-klant bent, kunt u Azure Backup Server (betalen naar gebruik) om te beveiligen van virtuele VMware-machines.

## <a name="dpm-and-azure-backup-server-backup"></a>DPM en Azure Backup-Server back-up

### <a name="which-dpm-versions-are-supported"></a>Welke DPM-versies worden ondersteund?

Ondersteunde versies van DPM worden samengevat in de [ondersteuningsmatrix](backup-azure-dpm-introduction.md#prerequisites-and-limitations). Het is raadzaam dat u de nieuwste DPM-updates installeren en voer de [meest recente versie](https://aka.ms/azurebackup_agent) van de Azure Backup-agent op de DPM-server.

### <a name="can-i-register-the-server-to-multiple-vaults"></a>Kan ik de server voor meerdere kluizen registreren?

Nee. Een DPM of Azure Backup-server kan voor slechts één kluis worden geregistreerd.



### <a name="can-i-use-azure-backup-server-to-create-a-bare-metal-recovery-bmr-backup-for-a-physical-server-br"></a>Kan ik de Azure Backup-server gebruiken om een BMR-back-up (Bare Metal Recovery) te maken voor een fysieke server? <br/>
Ja.


### <a name="can-i-use-dpm-to-back-up-apps-in-azure-stack"></a>Kan ik DPM back-up-apps in Azure Stack gebruiken?
Nee. U kunt Azure Backup gebruiken voor het beveiligen van Azure Stack, Azure Backup biedt geen ondersteuning voor het gebruik van DPM back-up-apps in Azure Stack.


### <a name="if-ive-installed-azure-backup-agent-to-protect-my-files-and-folders-can-i-install-system-center-dpm-to-back-up-on-premises-workloads-to-azure"></a>Als ik Azure backup-agent voor het beveiligen van bestanden en mappen hebt geïnstalleerd, kan ik System Center DPM back-up on-premises werkbelastingen naar Azure installeren?
Ja. Maar u moet eerst instellen van DPM en installeer vervolgens de Azure Backup-agent.  Installeren van onderdelen in deze volgorde zorgt ervoor dat de Azure Backup agent met DPM werkt. De agent installeren voordat u DPM installeert, wordt niet aangeraden of ondersteund.



## <a name="general-backup"></a>Algemene back-up

### <a name="are-there-limits-on-backup-scheduling"></a>Gelden er limieten voor back-upplanning?
Ja.
- U kunt back-up van Windows Server of Windows machines maximaal drie keer per dag. U kunt het planningsbeleid ingesteld op dagelijks of wekelijks schema's.
- U kunt back-up van DPM maximaal twee keer per dag. U kunt het planningsbeleid voor dagelijkse, wekelijkse, maandelijkse en jaarlijkse instellen.
- U back-up van Azure-VM's één keer per dag.

## <a name="what-operating-systems-are-supported-for-backup"></a>Welke besturingssystemen worden ondersteund voor back-up?

Azure Backup biedt ondersteuning voor deze besturingssystemen voor back-ups van bestanden en mappen en apps die zijn beveiligd door Azure Backup Server en DPM.

**Besturingssysteem**| **SKU** |**Details**
--- | --- | ---
Werkstation | |
Windows 10 64-bits | Enterprise, Pro, Home | Machines moeten worden uitgevoerd, de meest recente servicepacks en updates.
Windows 8.1 64-bits | Enterprise, Pro | Machines moeten worden uitgevoerd, de meest recente servicepacks en updates.
Windows 8 64-bits | Enterprise, Pro | Machines moeten worden uitgevoerd, de meest recente servicepacks en updates.
Windows 7 64-bits | Ultimate, Enterprise, Professional, Home Premium Home Basic, Starter | Machines moeten worden uitgevoerd, de meest recente servicepacks en updates.
Server | |
Windows Server 2016, 64-bits | Standard, Datacenter, Essentials | Dat de meest recente service packs/updates.
Windows Server 2012 R2 64-bits | Standard, Datacenter, Foundation | Met de nieuwste service packs/updates.
Windows Server 2012, 64-bits | Datacenter, Foundation, Standard | Met de nieuwste service packs/updates.
Windows Storage Server 2016, 64-bits | Standard, Workgroup | Met de nieuwste service packs/updates.
Windows Storage Server 2012 R2 64-bits | Standard, Workgroup, Essential | Met de nieuwste service packs/updates.
Windows Storage Server 2012, 64-bits | Standard, Workgroup | Met de nieuwste service packs/updates.
Windows Server 2008 R2 SP1 64-bits | Standard, Enterprise, Datacenter, Foundation | Met de nieuwste updates.
Windows Server 2008 64-bits | Standard, Enterprise, Datacenter | Met de meest recente updates.

Voor back-ups van Azure virtuele machine Linux, Azure Backup ondersteunt [de lijst met distributies die zijn goedgekeurd door Azure](../virtual-machines/linux/endorsed-distros.md), met uitzondering van Core OS Linux- en 32-bits besturingssysteem. Andere bring-your-own Linux-distributies kunnen werken als de VM-agent beschikbaar op de virtuele machine is en ondersteuning voor Python aanwezig.


## <a name="are-there-size-limits-for-data-backup"></a>Zijn er limieten voor back-up van gegevens?

Limieten voor grootte zijn als volgt:


Besturingssysteem/machine | De maximale grootte van de gegevensbron
--- | --- | ---
Windows 8 of hoger | 54.400 GB
Windows 7 |1700 GB
Windows Server 2012 of hoger | 54.400 GB
Windows Server 2008, Windows Server 2008 R2 | 1700 GB
Azure VM | 16 gegevensschijven<br/><br/> Gegevens schijf maximaal 4095 GB

## <a name="how-is-the-data-source-size-determined"></a>Hoe wordt de grootte van de gegevens bepaald?

In de volgende tabel wordt uitgelegd hoe de grootte voor elke gegevensbron wordt bepaald.

**Gegevensbron** | **Details**
--- | ---
Volume |De hoeveelheid gegevens die worden back-ups van één volume VM back-up wordt gemaakt.
SQL Server-database |Grootte van één SQL-databasegrootte back-up wordt gemaakt.
SharePoint | De som van de inhoud en configuratiedatabases in een SharePoint-farm back-up wordt gemaakt.
Exchange |De som van alle Exchange-databases in een Exchange-server back-up wordt gemaakt.
BMR/systeemstatus |Elke afzonderlijke kopie van de BMR of systeemstatus van de machine een back-wordt gemaakt.


### <a name="is-there-a-limit-on-the-amount-of-data-backed-up-using-a-recovery-services-vault"></a>Is er een limiet voor de hoeveelheid gegevens een back-up met behulp van een Recovery Services-kluis?

Er is geen limiet voor de hoeveelheid gegevens die u kunt back-up met behulp van een Recovery Services-kluis.

### <a name="if-i-cancel-a-backup-job-once-it-has-started-is-the-transferred-backup-data-deleted"></a>Als ik een back-uptaak annuleer nadat deze is gestart, worden de overgedragen back-upgegevens dan verwijderd?
Nee. Alle gegevens die naar de kluis zijn overgebracht voordat de back-uptaak is geannuleerd, blijven aanwezig in de kluis. Azure Backup gebruikt een controlepuntmechanisme waarmee tijdens de back-up van tijd tot tijd controlepunten worden toegevoegd aan de back-upgegevens. Omdat de back-upgegevens controlepunten bevatten, kan tijdens het volgende back-upproces de integriteit van de bestanden worden gecontroleerd. De volgende back-uptaak is incrementeel ten opzichte van de gegevens waarvan eerder een back-up is gemaakt. Incrementele back-ups dragen alleen nieuwe of gewijzigde gegevens over, zodat de bandbreedte beter wordt benut.

Als u een back-uptaak voor een virtuele Azure-machine annuleert, worden eventuele overgedragen gegevens geannuleerd. De volgende back-uptaak draagt incrementele gegevens over van na de vorige succesvolle back-up-taak.

## <a name="why-is-the-size-of-the-data-transferred-to-the-recovery-services-vault-smaller-than-the-data-selected-for-backup"></a>Waarom is de grootte van de gegevens overgebracht naar de Recovery Services-kluis die kleiner is dan de gegevens die zijn geselecteerd voor back-up?

 Gegevens back-up van Azure Backup-Agent, DPM, en Azure Backup Server wordt gecomprimeerd en versleuteld voordat ze worden overgedragen. Met compressie en versleuteling wordt toegepast, de gegevens in de kluis is 30-40% kleiner.

### <a name="can-i-delete-individual-files-from-a-recovery-point-in-the-vault"></a>Kan ik afzonderlijke bestanden vanaf een herstelpunt in de kluis verwijderen?
Nee, Azure Backup ondersteunt niet verwijderen of permanent verwijderen van afzonderlijke items vanuit opgeslagen back-ups.


### <a name="if-i-cancel-a-backup-job-after-it-starts-is-the-transferred-backup-data-deleted"></a>Als ik een back-uptaak Annuleer nadat deze is gestart, wordt de overgedragen back-upgegevens dan verwijderd?

Nee. Alle gegevens die zijn overgedragen naar de kluis voordat de back-uptaak is geannuleerd blijft in de kluis.
- Azure Backup gebruikt een controlepuntmechanisme waarmee tijdens de back-up van tijd tot tijd controlepunten worden toegevoegd aan de back-upgegevens.
- Omdat de back-upgegevens controlepunten bevatten, kan tijdens het volgende back-upproces de integriteit van de bestanden worden gecontroleerd.
- De volgende back-uptaak is incrementeel ten opzichte van de gegevens waarvan eerder een back-up is gemaakt. Incrementele back-ups dragen alleen nieuwe of gewijzigde gegevens over, zodat de bandbreedte beter wordt benut.




## <a name="retention-and-recovery"></a>Retentie en het herstel

### <a name="are-the-retention-policies-for-dpm-and-windows-machines-without-dpm-the-same"></a>Het bewaarbeleid voor DPM en Windows-machines zonder DPM zijn hetzelfde?
Ja, hebben ze beide dagelijkse, wekelijkse, maandelijkse en jaarlijkse retentiebeleid.

### <a name="can-i-customize-retention-policies"></a>Kan ik voor het bewaren van aanpassen?
Ja, hebt u het beleid aanpassen. U kunt bijvoorbeeld wekelijks en dagelijks vereisten voor de bewaarperiode, maar niet jaarlijks en maandelijks configureren.

### <a name="can-i-use-different-times-for-backup-scheduling-and-retention-policies"></a>Kan ik verschillende keren gebruiken voor back-upplanning en beleid voor het bewaren?
Nee. Het bewaarbeleid kan alleen worden toegepast op back-uppunten. Deze afbeeldingen ziet u bijvoorbeeld een bewaarbeleid voor back-ups die op 12: 00 uur en 18: 00 uur.

![Back-up en retentie plannen](./media/backup-azure-backup-faq/Schedule.png)


### <a name="if-a-backup-is-kept-for-a-long-time-does-it-take-more-time-to-recover-an-older-data-point-br"></a>Als u een back-up worden bewaard gedurende een lange periode duurt langer voordat een oud gegevenspunt te herstellen? <br/>
Nee, de tijd die nodig is om het oudste of nieuwste punt te herstellen, is hetzelfde. Elk herstelpunt gedraagt zich als een volledig punt.

### <a name="if-each-recovery-point-is-like-a-full-point-does-it-impact-the-total-billable-backup-storage"></a>Als elk herstelpunt een volledig punt is, heeft dit dan invloed op de totale factureerbare back-upopslag?

Producten met een lange bewaartermijn slaan de back-upgegevens doorgaans op als volledige punten.
    - De volledige punten maken *inefficiënt* gebruik van de opslagruimte, maar kunnen wel gemakkelijker en sneller worden hersteld.
    - Incrementele kopieën maken opslag *efficiënte* , maar moet u een keten van gegevens, die van invloed is op de hersteltijd herstellen

De opslagarchitectuur van Azure Backup biedt u het beste van beide werelden door de gegevens optimaal op te slaan voor snelle herstelbewerkingen en lage opslagkosten. Dit zorgt ervoor dat uw bandbreedte voor inkomend en uitgaand verkeer efficiënt wordt gebruikt. De hoeveelheid opslag van gegevens en de tijd die nodig is om de gegevens te herstellen wordt tot een minimum beperkt. Meer informatie over [incrementele back-ups](https://azure.microsoft.com/blog/microsoft-azure-backup-save-on-long-term-storage/).

### <a name="is-there-a-limit-on-the-number-of-recovery-points-that-can-be-created"></a>Geldt er een limiet voor het aantal herstelpunten dat kan worden gemaakt?

U kunt maximaal 9999 herstelpunten maken per beveiligd exemplaar. Een beveiligd exemplaar is een computer, server (fysiek of virtueel) of werkbelasting waarvan een back-ups van Azure.

- Meer informatie over [back-up en retentie](./backup-introduction-to-azure-backup.md#backup-and-retention).
- Meer informatie over [beschermde exemplaren](./backup-introduction-to-azure-backup.md#what-is-a-protected-instance)?

### <a name="how-many-times-can-i-recovery-data-thats-backed-up-to-azure"></a>Hoe vaak kan ik gegevens voor herstel die de back-up naar Azure?
Er is geen limiet voor het aantal herstelbewerkingen vanuit Azure Backup.

### <a name="when-restoring-data-do-i-pay-for-the-egress-traffic-from-azure"></a>Als ik gegevens ga herstellen, betaal ik dan voor het uitgaande verkeer van Azure?
Nee. Recovery gratis is en niet in rekening gebracht voor het uitgaande verkeer.

### <a name="what-happens-when-i-change-my-backup-policy"></a>Wat gebeurt er wanneer ik mijn back-upbeleid wijzigen?

Wanneer een nieuw beleid wordt toegepast, schema en de retentie van het nieuwe beleid gevolgd.

- Als de retentie is uitgebreid, worden de bestaande herstelpunten gemarkeerd voor bewaring conform het nieuwe beleid.
- - Als de retentie is beperkt, worden de bestaande herstelpunten gemarkeerd voor verwijdering in de eerstvolgende opschoontaak, en worden ze vervolgens verwijderd.

## <a name="encryption"></a>Versleuteling

### <a name="is-the-data-sent-to-azure-encrypted"></a>Worden de gegevens die naar Azure worden verzonden, versleuteld?

Ja. Gegevens worden versleuteld op de on-premises virtuele machine met AES256. De gegevens worden verzonden via een beveiligde HTTPS-koppeling. De gegevens worden verzonden in de cloud wordt beveiligd door HTTPS-koppeling tussen service voor opslag maken en herstellen. iSCSI-protocol voor beveiliging van de gegevens tussen machine voor herstel-service en de gebruiker verzonden. Secure tunneling wordt gebruikt voor het beveiligen van het iSCSI-kanaal.

### <a name="is-the-backup-data-on-azure-encrypted-as-well"></a>Worden de back-upgegevens op Azure ook versleuteld?

Ja. De gegevens in Azure is versleuteld in rust.
- Versleuteling-at-rest wordt geleverd met de wachtwoordzin die u opgeeft wanneer back-up op Azure voor on-premises back-up.
- Voor virtuele Azure-machines zijn gegevens versleuteld-at-rest met behulp van Storage Service Encryption (SSE).

De back-upgegevens worden nooit door Microsoft ontsleuteld.


### <a name="what-is-the-minimum-length-of-encryption-the-key-used-to-encrypt-backup-data"></a>Wat is de minimale lengte van de sleutel die wordt gebruikt voor het versleutelen van back-upgegevens voor codering?

Als u gebruikmaakt van een Azure Backup-agent, moet de versleutelingssleutel ten minste 16 tekens bevatten. Voor virtuele Azure-machines is er geen limiet voor de lengte van de sleutels die door Azure KeyVault worden gebruikt.

### <a name="what-happens-if-i-misplace-the-encryption-key-can-i-recover-the-data-can-microsoft-recover-the-data"></a>Wat gebeurt er als ik de versleutelingssleutel kwijtraak? Kan ik de gegevens herstellen? Kan Microsoft de gegevens herstellen?
De sleutel die wordt gebruikt voor het versleutelen van de back-upgegevens is alleen op uw site aanwezig. Microsoft bewaart geen kopie in Azure en heeft geen toegang tot de sleutel. Als u de sleutel misplace, kan Microsoft de back-upgegevens niet herstellen.

## <a name="next-steps"></a>Volgende stappen

Lees de andere veelgestelde vragen over:

- [Veelgestelde vragen over](backup-azure-vm-backup-faq.md) over back-ups van virtuele Azure-machine.
- [Veelgestelde vragen over](backup-azure-file-folder-backup-faq.md) over de Azure backup-agent
