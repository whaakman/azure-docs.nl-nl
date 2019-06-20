---
title: Back-up van bestanden van de Azure-bestandsshares met de Azure Backup-Service
description: In deze zelfstudie wordt uitgelegd hoe u back-up van Azure-bestandsshares.
services: backup
author: dcurwin
ms.author: dacurwin
ms.date: 06/10/2019
ms.topic: tutorial
ms.service: backup
manager: carmonm
ms.openlocfilehash: 474d5454e30c35d3f3ccf4ea994093ef47bd6ceb
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67275990"
---
# <a name="back-up-azure-file-shares"></a>Een back-up maken van Azure-bestandsshares
In dit artikel wordt uitgelegd hoe u met Azure Portal back-ups kunt maken van [Azure-bestandsshares](../storage/files/storage-files-introduction.md) en deze kunt herstellen.

In deze handleiding leert u het volgende:
> [!div class="checklist"]
> * Een Recovery Services-kluis configureren om een back-up te maken van Azure-bestanden
> * Een back-uptaak op aanvraag uitvoeren om een herstelpunt te maken


## <a name="prerequisites"></a>Vereisten
Voordat u een back-up van een Azure-bestandsshare probeert te maken, moet u nagaan of deze zich in een van de [ondersteunde typen opslagaccounts](tutorial-backup-azure-files.md#limitations-for-azure-file-share-backup-during-preview) bevindt. Zodra u dit hebt bevestigd, kunt u uw bestandsshares beveiligen.

## <a name="limitations-for-azure-file-share-backup-during-preview"></a>Beperkingen voor Azure-bestandsshares back-up van de Preview-versie
Back-up voor Azure-bestandsshares is in preview. Azure-bestandsshares worden ondersteund voor de accounts voor algemeen gebruik v1 en v2. De volgende back-upscenario's worden niet ondersteund voor Azure-bestandsshares:
- U kunt Azure-bestandsshares niet beveiligen in opslagaccounts waarvoor virtuele netwerken zijn of een firewall is ingeschakeld.
- Er is geen CLI beschikbaar voor het beveiligen van Azure Files met behulp van Azure Backup.
- Het maximumaantal geplande back-ups per dag is één.
- Het maximumaantal on-demand back-ups per dag is vier.
- Gebruik [resourcevergrendelingen](https://docs.microsoft.com/cli/azure/resource/lock?view=azure-cli-latest) voor het opslagaccount om per ongeluk verwijderen van back-ups uit de Recovery Services-kluis te voorkomen.
- Verwijder geen momentopnamen die met Azure Backup zijn gemaakt. Het verwijderen van momentopnamen kan leiden tot het verlies van herstelpunten en/of herstelfouten.
- Verwijder geen bestandsshares die zijn beveiligd met Azure Backup. Met de huidige oplossing worden alle momentopnamen die zijn gemaakt met Azure Backup, verwijderd zodra de bestandsshare wordt verwijderd. Alle herstelpunten gaan dan dus verloren

Terug voor Azure-bestandsshares in de Storage-Accounts met [zone-redundante opslag](../storage/common/storage-redundancy-zrs.md) replicatie (ZRS) is momenteel alleen beschikbaar in Centraal VS (CUS), East US (EUS), VS-Oost 2 (EUS2), Noord Europa (NE), Zuidoost Azië (SEA), West-Europa (WE) en VS-West 2 (WUS2).

## <a name="configuring-backup-for-an-azure-file-share"></a>Back-up voor Azure-bestandsshare configureren
In deze zelfstudie wordt ervan uitgegaan dat u al een Azure-bestandsshare hebt gemaakt. Ga als volgt te werk om een back-up van uw Azure-bestandsshare te maken:

1. Maak een Recovery Services-kluis in dezelfde regio als waarin de bestandsshare zich bevindt. Als u al een kluis hebt, opent u de overzichtspagina van uw kluis en klikt u op **Back-up**.

    ![Klik op back-up op de pagina overzicht van uw kluis](./media/backup-file-shares/overview-backup-page.png)

2. In de **back-updoel** in het menu van **wat wilt u een back-up?** , kies Azure-bestandsshare.

    ![Kies Azure-bestandsshare als back-updoel](./media/backup-file-shares/choose-azure-fileshare-from-backup-goal.png)

3. Klik op **Back-up** om de Azure-bestandsshare naar de Recovery Services-kluis te configureren.

   ![Klik op Back-up om de Azure-bestandsshare aan de kluis te koppelen](./media/backup-file-shares/set-backup-goal.png)

    Zodra de kluis gekoppeld aan de Azure-bestandsshare is, wordt het menu back-up wordt geopend en u wordt gevraagd om een Storage-account. Het menu geeft alle ondersteunde Opslagaccounts in de regio waarin uw kluis zich bevindt die niet zijn al gekoppeld aan een Recovery Services-kluis.

   ![Selecteer uw opslagaccount](./media/backup-file-shares/list-of-storage-accounts.png)

4. Selecteer een account in de lijst met opslagaccounts en klik op **OK**. Azure zoekt naar het opslagaccount voor bestandsshares waarvan een back-up kan worden gemaakt. Als u uw bestandsshares recentelijk hebt toegevoegd en ze niet in de lijst worden weergegeven, kan het helpen om even te wachten.

   ![Bestandsshares worden gedetecteerd](./media/backup-file-shares/discover-file-shares.png)

5. Uit de **bestandsshares** , selecteert u een of meer van de bestandsshares die u wilt back-up maken en klikt u op **OK**.

6. Nadat u de bestandsshares hebt geselecteerd, schakelt het menu Back-up over naar het **Back-upbeleid**. In dit menu selecteert u een bestaand back-upbeleid of maakt u een nieuwe. Vervolgens klikt u op **Back-up inschakelen**.

   ![Selecteer een beleid voor back-up of een nieuwe maken](./media/backup-file-shares/apply-backup-policy.png)

    Wanneer er een back-upbeleid is ingesteld, wordt er een momentopname van de bestandsshares genomen op het geplande tijdstip en wordt het herstelpunt gedurende de gekozen periode bewaard.

## <a name="create-an-on-demand-backup"></a>Een back-up op aanvraag maken
Na het configureren van het back-upbeleid, moet u een on-demand back-up om te controleren of dat uw gegevens worden beschermd tot en met de volgende geplande back-up maken.


### <a name="to-create-an-on-demand-backup"></a>Een back-up op aanvraag maken

1. Open de Recovery Services-kluis waarin de herstelpunten van de bestandsshares staan en klik op **Back-upitems**. De lijst met typen back-upitems wordt weergegeven.

   ![Lijst met back-Upitems](./media/backup-file-shares/list-of-backup-items.png)

2. Selecteer **Azure Storage (Azure-bestanden)** in de lijst. De lijst met Azure-bestandsshares wordt weergegeven.

   ![Lijst met Azure-bestandsshares](./media/backup-file-shares/list-of-azure-files-backup-items.png)

3. Selecteer de gewenste bestandsshare uit de lijst met Azure-bestandsshares. Het menu Back-upitem wordt geopend voor de geselecteerde bestandsshare.

   ![Menu van de back-upitem voor het geselecteerde bestand delen](./media/backup-file-shares/backup-item-menu.png)

4. Klik in het menu Back-upitem op **Nu back-up maken**. Omdat dit een back-uptaak op aanvraag is, is er geen bewaarbeleid gekoppeld aan het herstelpunt. Het dialoogvenster **Nu back-up maken** wordt geopend. Geef de laatste dag op die u van het herstelpunt wilt bewaren.

   ![Kies de datum voor de bewaarperiode voor herstelpunten](./media/backup-file-shares/backup-now-menu.png)


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u Azure Portal gebruikt voor het volgende:

> [!div class="checklist"]
> * Een Recovery Services-kluis configureren om een back-up te maken van Azure-bestanden
> * Een back-uptaak op aanvraag uitvoeren om een herstelpunt te maken

Doorgaan met het volgende artikel voor het herstellen van een back-up van een Azure-bestandsshare.

> [!div class="nextstepaction"]
> [Herstellen vanuit back-up van Azure-bestandsshares](./backup-azure-files.md#restore-from-backup-of-azure-file-share)
 
