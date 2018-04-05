---
title: Veelgestelde vragen over het maken van een back-up van Azure-bestanden
description: Dit artikel bevat informatie over het beveiligen van uw Azure-bestandsshares.
services: backup
keywords: Voeg geen sleutelwoorden toe en bewerk ze niet zonder overleg met uw SEO-expert.
author: markgalioto
ms.author: markgal
ms.date: 2/21/2018
ms.topic: tutorial
ms.service: backup
ms.workload: storage-backup-recovery
manager: carmonm
ms.openlocfilehash: 850d4d1e2ef6a13fcd8a072e6da210d558c7769b
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2018
---
# <a name="questions-about-backing-up-azure-files"></a>Vragen over het maken back-ups van Azure-bestanden
In dit artikel vindt u antwoorden op veelgestelde vragen over het maken back-ups van Azure-bestanden. Sommige antwoorden bevatten koppelingen naar artikelen met uitgebreide informatie over het onderwerp. U kunt ook in het [discussieforum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup) vragen over de Azure Backup-service plaatsen.

Als u kort de secties in dit artikel wilt bekijken, gebruikt u de koppelingen aan de rechterkant, onder **In dit artikel**.

## <a name="configuring-the-backup-job-for-azure-files"></a>De back-uptaak configureren voor Azure-bestanden

### <a name="why-cant-i-see-some-of-my-storage-accounts-i-want-to-protect-that-contain-valid-azure-file-shares-br"></a>Waarom zie ik een aantal van de opslagaccounts niet die ik wil beveiligen, hoewel ze geldige Azure-bestandsshares bevatten? <br/>
Tijdens de preview-fase worden niet alle typen opslagaccounts ondersteund bij het maken van back-ups voor Azure-bestandsshares. Raadpleeg [deze](troubleshoot-azure-files.md#preview-boundaries) lijst met ondersteunde opslagaccounts.

### <a name="why-cant-i-see-some-of-my-azure-file-shares-in-the-storage-account-when-im-trying-to-configure-backup-br"></a>Waarom zie ik een aantal van mijn Azure-bestandsshares in het opslagaccount niet als ik mijn back-up configureer? <br/>
Controleer of de Azure-bestandsshare al in dezelfde Recovery Services-kluis wordt beveiligd en of de bestandsshare onlangs wellicht is verwijderd.

### <a name="why-cant-i-protect-file-shares-connected-to-a-sync-group-in-azure-file-sync-br"></a>Waarom kan ik bestandsshares die met een synchronisatiegroep in Azure File Sync zijn verbonden, niet beveiligen? <br/>
De beveiliging van Azure-bestandsshares die zijn verbonden met synchronisatiegroepen, is alleen als beperkte preview beschikbaar. Schrijf naar [ AskAzureBackupTeam@microsoft.com ](email:askazurebackupteam@microsoft.com) en vermeld uw abonnements-ID om toegang aan te vragen. 

### <a name="in-which-geos-can-i-back-up-azure-file-shares-br"></a>In welke geografische gebieden kan ik back-ups maken van Azure-bestandsshares? <br/>
Het maken van back-ups van Azure-bestandsshares is momenteel in de preview-fase en is alleen beschikbaar in de volgende geografische gebieden: 
-   Australië - zuidoost (ASE) 
- Brazilië - zuid (BRS)
- Canada centraal (CNC)
-   Canada - oost (CE)
-   VS - centraal (CUS)
-   Oost-Azië (EA)
-   Oost-Australië (AE) 
-   VS - oost (EUS)
-   VS - oost 2 (EUS2)
-   India - centraal (INC) 
-   VS - noord centraal (NCUS) 
-   Noord-Europa (NE) 
-   VS - zuid-centraal (SCUS) 
-   Zuidoost-Azië (SEA)
-   Verenigd Koninkrijk zuid (UKS) 
-   Verenigd Koninkrijk west (UKW) 
-   West-Europa (WE) 
-   VS - west (WUS)
-   VS - west-centraal (WCUS)
-   VS - west 2 (WUS 2)

Schrijf naar [ AskAzureBackupTeam@microsoft.com ](email:askazurebackupteam@microsoft.com) als u deze functie wilt gebruiken in een specifiek geografisch gebied dat hierboven niet wordt vermeld.

### <a name="how-many-azure-file-shares-can-i-protect-in-a-vaultbr"></a>Hoeveel Azure-bestandsshares kan ik beveiligen in een kluis?<br/>
Met de preview-versie kunt u Azure-bestandsshares van maximaal 25 opslagaccounts per kluis beveiligen. U kunt ook tot 200 Azure-bestandsshares in een enkele kluis beveiligen. 

## <a name="backup"></a>Back-up maken

### <a name="how-many-on-demand-backups-can-i-take-per-file-share-br"></a>Hoeveel back-ups op aanvraag kan ik maken per bestandsshare? <br/>
U kunt op elk moment maximaal 200 momentopnamen voor een bestandsshare hebben. Deze limiet is inclusief momentopnamen die zijn gemaakt met Azure Backup zoals gedefinieerd in uw beleid. Als uw back-ups mislukken nadat de limiet is bereikt, verwijdert u herstelpunten op aanvraag om ervoor te zorgen dat toekomstige back-ups slagen.

### <a name="after-enabling-virtual-networks-on-my-storage-account-the-backup-of-file-shares-in-the-account-started-failing-why"></a>Nadat ik op mijn opslagaccount virtuele netwerken had ingeschakeld, heb ik geen back-ups meer kunnen maken van bestandsshares in het account. Hoe komt dat?
Het maken van back-ups van Azure-bestandsshares biedt geen ondersteuning voor opslagaccounts waarvoor virtuele netwerken zijn ingeschakeld. Schakel virtuele netwerken uit voor de opslagaccounts om geslaagde back-ups te maken. 

## <a name="restore"></a>Herstellen

### <a name="can-i-recover-from-a-deleted-azure-file-share-br"></a>Kan ik gegevens herstellen vanuit een verwijderde Azure-bestandsshare? <br/>
Wanneer een Azure-bestandsshare is verwijderd, krijgt u de lijst met back-ups te zien die ook worden verwijderd, en wordt u gevraagd dit te bevestigen. Een verwijderde Azure-bestandsshare kan niet worden hersteld.

### <a name="can-i-restore-from-backups-if-i-stopped-protection-on-an-azure-file-share-br"></a>Kan ik gegevens herstellen vanuit back-ups als ik ben gestopt met de beveiliging van een Azure-bestandsshare? <br/>
Ja. Als u **Back-upgegevens behouden** hebt gekozen toen u met de beveiliging stopte, kunt u vanaf alle bestaande herstelpunten iets terugzetten.

## <a name="manage-backup"></a>Het maken van back-ups beheren

### <a name="can-i-access-the-snapshots-taken-by-azure-backups-and-mount-it-br"></a>Heb ik toegang tot de momentopnamen die door Azure Backup is gemaakt en deze koppelen? <br/>
Alle momentopnamen die zijn gemaakt met Azure Backup, zijn toegankelijk via Momentopnamen weergeven in de portal, in PowerShell of in CLI. Meer informatie over momentopnamen van Azure-bestandsshares vindt u in [Overzicht van share-momentopnamen voor Azure Files (preview)](../storage/files/storage-snapshots-files.md).

### <a name="what-is-the-maximum-retention-i-can-configure-for-backups-br"></a>Wat is de maximale bewaartermijn die ik voor back-ups kan configureren? <br/>
Bij het maken van back-ups voor Azure-bestandsshares, kunt u gebruikmaken van de mogelijkheid uw dagelijkse back-ups 120 dagen te bewaren.

### <a name="what-happens-when-i-change-the-backup-policy-for-an-azure-file-share-br"></a>Wat gebeurt er wanneer ik het back-upbeleid voor een Azure-bestandsshare wijzig? <br/>
Wanneer er op een of meer bestandsshares een nieuw beleid wordt toegepast, worden het schema en de retentie van het nieuwe beleid gevolgd. Als de retentie is uitgebreid, worden de bestaande herstelpunten gemarkeerd voor bewaring conform het nieuwe beleid. Als de retentie is beperkt, worden de bestaande herstelpunten gemarkeerd voor verwijdering in de eerstvolgende opschoontaak, en worden ze vervolgens verwijderd.

## <a name="see-also"></a>Zie ook
Deze informatie heeft alleen betrekking op het maken van back-ups van Azure-bestanden. Voor meer informatie over andere gebieden van Azure Backup, wordt u aangeraden een aantal van een van deze overige veelgestelde vragen over het maken van back-ups te raadplegen:
-  [Veelgestelde vragen over Recovery Services-kluis](backup-azure-backup-faq.md)
-  [Veelgestelde vragen over Azure VM-back-ups](backup-azure-vm-backup-faq.md)
-  [Veelgestelde vragen over Azure Backup-agent](backup-azure-file-folder-backup-faq.md)