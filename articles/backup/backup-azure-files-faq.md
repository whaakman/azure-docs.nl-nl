---
title: Veelgestelde vragen over het maken van een back-up van Azure-bestanden
description: Dit artikel bevat informatie over het beveiligen van uw Azure-bestanden in Azure. Dit uittreksel wordt bij de zoekresultaten weergegeven.
services: backup
keywords: Voeg geen sleutelwoorden toe en bewerk ze niet zonder overleg met uw SEO-expert.
author: markgalioto
ms.author: markgal
ms.date: 2/21/2018
ms.topic: tutorial
ms.service: backup
ms.workload: storage-backup-recovery
manager: carmonm
ms.openlocfilehash: 3d09914c93d0f48b8f6bed405202682aaf925a5f
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/24/2018
---
# <a name="questions-about-backing-up-azure-files"></a>Vragen over het maken back-ups van Azure-bestanden
In dit artikel vindt u antwoorden op veelgestelde vragen over het maken back-ups van Azure-bestanden. Sommige antwoorden bevatten koppelingen naar artikelen met uitgebreide informatie over het onderwerp. U kunt ook in het [discussieforum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup) vragen over de Azure Backup-service plaatsen.

Als u kort de secties in dit artikel wilt bekijken, gebruikt u de koppelingen aan de rechterkant, onder **In dit artikel**.

## <a name="configuring-the-backup-job-for-azure-files"></a>De back-uptaak configureren voor Azure-bestanden

### <a name="why-cant-i-see-some-of-my-storage-accounts-i-want-to-protect-that-contain-valid-file-shares-br"></a>Waarom zie ik een aantal van de opslagaccounts die geldige bestandsshares bevatten en die ik wil beveiligen niet? <br/>
Tijdens de preview-fase wordt bij het maken van back-ups voor Azure-bestandsshares geen ondersteuning geboden voor alle typen opslagaccounts. Raadpleeg [deze](troubleshoot-azure-files.md#preview-boundaries) lijst met ondersteunde opslagaccounts.

### <a name="why-cant-i-see-some-of-my-file-shares-in-the-storage-account-when-im-trying-to-configure-backup-br"></a>Waarom zie ik een aantal van mijn bestandsshares in het opslagaccount niet als ik mijn back-up configureer? <br/>
Controleer of de bestandsshare al in dezelfde Recovery Services-kluis wordt beveiligd en of de bestandsshare onlangs wellicht is verwijderd.

### <a name="why-cant-i-protect-file-shares-connected-to-a-sync-group-in-azure-files-sync-br"></a>Waarom kan ik bestandsshares die met een synchronisatiegroep in Azure File Sync zijn verbonden niet beveiligen? <br/>
Het beveiliging van Azure-bestandsshares die zijn verbonden met synchronisatiegroepen in Azure Policy, zijn als beperkte preview-versie beschikbaar. Schrijf naar [ AskAzureBackupTeam@microsoft.com ](email:askazurebackupteam@microsoft.com) en vermeld uw abonnements-ID om toegang aan te vragen. 

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

### <a name="how-many-file-shares-can-i-protect-in-a-vaultbr"></a>Hoeveel bestandsshares kan ik beveiligen in een kluis?<br/>
Met de preview-versie kunt u bestandsshares van maximaal 25 opslagaccounts per kluis beveiligen. U kunt ook tot 200 bestandsshares in een enkele kluis beveiligen. 

## <a name="backup"></a>Back-up maken

### <a name="how-many-on-demand-backups-can-i-take-per-file-share-br"></a>Hoeveel back-ups op aanvraag kan ik maken per bestandsshare? <br/>
U kunt op elk moment maximaal 200 momentopnamen voor een bestandsshare hebben. Deze limiet is inclusief momentopnamen die zijn gemaakt met Azure Backup zoals gedefinieerd in uw beleid. Als uw back-ups mislukken nadat de limiet is bereikt, verwijdert u herstelpunten op aanvraag om ervoor te zorgen dat toekomstige back-ups slagen.

### <a name="after-enabling-virtual-networks-on-my-storage-account-the-backup-of-file-shares-in-the-account-started-failing-why"></a>Nadat ik op mijn opslagaccount virtuele netwerken had ingeschakeld, heb ik geen back-ups meer kunnen maken van bestandsshares in het account. Hoe komt dat?
Het maken van back-ups van Azure-bestanden biedt geen ondersteuning voor opslagaccounts waarvoor virtuele netwerken zijn ingeschakeld. Schakel virtuele netwerken uit voor de opslagaccounts om geslaagde back-ups te maken. 

## <a name="restore"></a>Herstellen

### <a name="can-i-recover-from-a-deleted-file-share-br"></a>Kan ik iets terugzetten vanuit een verwijderde bestandsshare? <br/>
Wanneer een bestandsshare is verwijderd, krijgt u de lijst met back-ups te zien die ook worden verwijderd, en wordt u gevraagd dit te bevestigen. Een verwijderde bestandsshare kan niet worden teruggezet.

### <a name="can-i-restore-from-backups-if-i-stopped-protection-on-a-file-share-br"></a>Kan ik iets terugzetten vanuit back-ups als ik ben gestopt met de beveiliging van een bestandsshare? <br/>
Ja. Als u **Back-upgegevens behouden** hebt gekozen toen u met de beveiliging stopte, kunt u vanaf alle bestaande herstelpunten iets terugzetten.

## <a name="manage-backup"></a>Het maken van back-ups beheren

### <a name="can-i-access-the-snapshots-taken-by-azure-backups-and-mount-it-br"></a>Heb ik toegang tot de momentopnamen die door Azure Backup is gemaakt en deze koppelen? <br/>
Alle momentopnamen die zijn gemaakt met Azure Backup, zijn toegankelijk via Momentopnamen weergeven in de portal, in PowerShell of in CLI. U kunt ze koppelen door [deze](../storage/files/storage-how-to-use-files-snapshots.md#mount-a-file-share) procedure te volgen.

### <a name="what-is-the-maximum-retention-i-can-configure-for-backups-br"></a>Wat is de maximale bewaartermijn die ik voor back-ups kan configureren? <br/>
Bij het maken van back-ups voor Azure-bestandsshares, kunt u gebruikmaken van de mogelijkheid uw dagelijkse back-ups 120 dagen te bewaren.

### <a name="what-happens-when-i-change-the-backup-policy-for-a-file-share-br"></a>Wat gebeurt er wanneer ik het back-upbeleid voor een bestandsshare wijzig? <br/>
Wanneer er op een of meer bestandsshares een nieuw beleid wordt toegepast, worden het schema en de retentie van het nieuwe beleid gevolgd. Als de retentie is uitgebreid, worden de bestaande herstelpunten gemarkeerd voor bewaring conform het nieuwe beleid. Als de retentie is beperkt, worden de bestaande herstelpunten gemarkeerd voor verwijdering in de eerstvolgende opschoontaak, en worden ze vervolgens verwijderd.


## <a name="see-also"></a>Zie ook
Deze informatie heeft alleen betrekking op het maken van back-ups van Azure-bestanden. Voor meer informatie over andere gebieden van Azure Backup, wordt u aangeraden een aantal van een van deze overige veelgestelde vragen over het maken van back-ups te raadplegen:
-  [Veelgestelde vragen over Recovery Services-kluis](backup-azure-backup-faq.md)
-  [Veelgestelde vragen over Azure VM-back-ups](backup-azure-vm-backup-faq.md)
-  [Veelgestelde vragen over Azure Backup-agent](backup-azure-file-folder-backup-faq.md)
