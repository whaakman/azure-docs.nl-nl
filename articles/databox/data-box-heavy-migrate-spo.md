---
title: Azure Data Box zware gebruiken voor het migreren van inhoud van share tot SharePoint Online | Microsoft Docs
description: Gebruik deze handleiding om informatie over het migreren van inhoud delen met Online delen punt met uw Azure Data Box zware
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: tutorial
ms.date: 07/03/2019
ms.author: alkohli
ms.openlocfilehash: d74539ec1de8f503b0d0e423adf6273d1422fed5
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67592336"
---
# <a name="use-the-azure-data-box-heavy-to-migrate-your-file-share-content-to-sharepoint-online"></a>Azure Data Box zware gebruiken voor het migreren van uw bestand delen inhoud tot SharePoint Online

Uw Azure Data Box zware en de SharePoint Migration Tool (SPMT) gebruiken voor het migreren van uw bestandsshare-inhoud eenvoudig tot SharePoint Online en OneDrive. Met behulp van Data Box-zwaar, kunt u de afhankelijkheid verwijderen op de koppeling Wide area network (WAN) de gegevens over te dragen.

De Microsoft Azure Data Box is een service waarmee u een apparaat van de Microsoft Azure portal bestellen. U kunt vervolgens terabytes aan gegevens kopiëren van uw servers op het apparaat. Na het verzenden naar Microsoft, worden uw gegevens gekopieerd naar Azure. Afhankelijk van de grootte van de gegevens die u van plan bent om over te dragen, kunt u kiezen uit:

- [Data Box-schijf](https://docs.microsoft.com/azure/databox/data-box-disk-overview) met 35 TB aan bruikbare capaciteit per order voor kleine tot middelgrote gegevenssets.
- [Data Box](https://docs.microsoft.com/azure/databox/data-box-overview) met 80 TB aan bruikbare capaciteit per apparaat voor middelgrote tot grote gegevenssets.
- [Gegevens in het zware](https://docs.microsoft.com/azure/databox/data-box-heavy-overview) met 770 TB aan bruikbare capaciteit per apparaat voor grote gegevenssets.

In dit artikel is specifiek vertelt over het gebruik van de Data Box-zwaar uw bestand delen inhoud migreren naar SharePoint Online.

## <a name="requirements-and-costs"></a>Vereisten en kosten

### <a name="for-data-box-heavy"></a>Voor de Data Box-zwaar

- Gegevens in het zware is alleen beschikbaar voor Enterprise Agreement (EA), Cloud solution provider (CSP) of Azure sponsorship biedt. Als uw abonnement niet binnen een van de bovenstaande typen valt, neem dan contact op met Microsoft Support voor het upgraden van uw abonnement of Zie [prijzen voor Azure-abonnement](https://azure.microsoft.com/pricing/).
- Er is een vergoeding voor het gebruik van gegevens in het zware. Controleer de [Data Box zware prijzen](https://azure.microsoft.com/pricing/details/databox/heavy/).


### <a name="for-sharepoint-online"></a>Voor SharePoint Online

- Controleer de [minimale vereisten voor de SharePoint Migration Tool (SPMT)](https://docs.microsoft.com/sharepointmigration/how-to-use-the-sharepoint-migration-tool).

## <a name="workflow-overview"></a>Overzicht van de werkstroom

Deze werkstroom moet u stappen moeten worden uitgevoerd op Azure Data Box zware, maar ook op de SharePoint-online.
De volgende stappen zijn gekoppeld aan uw Azure Data Box zware.

1. Volgorde Azure Data Box-zwaar.
2. Ontvangen en instellen van uw apparaat.
3. Kopiëren van gegevens uit uw on-premises-bestand delen naar map voor Azure-bestanden op uw apparaat.
4. Nadat de kopie voltooid is, moet u het apparaat terug aan de hand van de instructies verzenden.
5. Wacht tot de gegevens volledig uploaden naar Azure.

De volgende stappen hebben online betrekking op de netwerkshare.

6. Een virtuele machine maken in Azure portal en de Azure-bestandsshare koppelen erop.
7. Installeer het hulpprogramma SPMT op de virtuele machine van Azure.
8. Voer het hulpprogramma SPMT uit met behulp van de Azure-bestandsshare als de *bron*.
9. De laatste stappen van het hulpprogramma.
10. Controleer en Bevestig uw gegevens.

## <a name="use-data-box-heavy-to-copy-data"></a>Gegevens in het zware gebruiken om gegevens te kopiëren

De volgende stappen om gegevens te kopiëren naar uw Data Box-zwaar.

1. [Uw Data Box-zwaar bestellen](data-box-heavy-deploy-ordered.md).
2. Nadat u uw gegevens in het zware, ontvangen [instellen van de Data Box-zwaar](data-box-heavy-deploy-set-up.md). U bekabelen en configureren van beide knooppunten op uw apparaat.
3. [Gegevens kopiëren naar Azure Data Box zware](data-box-heavy-deploy-copy-data.md). Tijdens het kopiëren, zorg ervoor dat:

    - Gebruik alleen de *AzureFile* map in de Data Box-zwaar om de gegevens te kopiëren. Dit is omdat u wilt dat de gegevens uiteindelijk te maken in een Azure-bestandsshare, niet in blok-blobs of pagina-blobs.
    - Bestanden kopiëren naar een map in *AzureFile* map. Een submap in *AzureFile* map wordt gemaakt van een bestandsshare. Bestanden rechtstreeks naar gekopieerd *AzureFile* map mislukken en worden geüpload als blok-blobs. Dit is de bestandsshare die u op de virtuele machine in de volgende stap wordt gekoppeld.
    - Gegevens kopiëren naar beide knooppunten van uw Data Box-zwaar.
3. Voer [voorbereiding voor verzending](data-box-heavy-deploy-picked-up.md#prepare-to-ship) op uw apparaat. Een geslaagde voorbereiding voor verzending zorgt ervoor dat het uploaden van een geslaagde van bestanden naar Azure.
4. [Het apparaat](data-box-heavy-deploy-picked-up.md#ship-data-box-heavy-back).
5. [Controleer of het uploaden van gegevens naar Azure](data-box-heavy-deploy-picked-up.md#verify-data-upload-to-azure).

## <a name="use-spmt-to-migrate-data"></a>SPMT gebruiken om gegevens te migreren

Nadat u een bevestiging van het team van Azure data dat het kopiëren van gegevens is voltooid ontvangen, kunt u nu doorgaan met het migreren van uw gegevens tot SharePoint Online.

Voor optimale prestaties en connectiviteit, wordt u aangeraden dat u een Azure-virtuele Machine (VM) maken.

1. Meld u aan bij de Azure-portal en vervolgens [maken van een virtuele machine](../virtual-machines/windows/quick-create-portal.md).
2. [De Azure-bestandsshare naar de virtuele machine koppelen](../storage/files/storage-how-to-use-files-windows.md#mount-the-azure-file-share-with-file-explorer).
3. [Download het hulpprogramma voor migratie van SharePoint](https://spmtreleasescus.blob.core.windows.net/install/default.htm) en installeer deze op uw virtuele Azure-machine.
4. Start het hulpprogramma voor migratie van SharePoint. Klik op **aanmelden** en voer uw Office 365-gebruikersnaam en wachtwoord.
5. Wanneer u hierom wordt gevraagd **waar zijn uw gegevens?** , selecteer **bestandsshare**. Geef het pad naar uw Azure-bestandsshare waar uw gegevens zich bevinden.
6. Volg de resterende aanwijzingen normaal, met inbegrip van de doellocatie. Ga voor meer informatie naar [over het gebruik van het hulpprogramma voor migratie van SharePoint](https://docs.microsoft.com/sharepointmigration/how-to-use-the-sharepoint-migration-tool).

> [!IMPORTANT]
> - De snelheid waarmee gegevens worden opgenomen in SharePoint Online wordt beïnvloed door diverse factoren, ongeacht als u al uw gegevens hebt in Azure. Informatie over deze factoren, krijgt u van plan bent en de efficiëntie van uw migratie te maximaliseren.  Ga voor meer informatie naar [SharePoint Online en OneDrive migratie snelheid](/sharepointmigration/sharepoint-online-and-onedrive-migration-speed).
> - Er is een risico dat bestaande machtigingen voor bestanden verloren gaan bij het migreren van de gegevens tot SharePoint Online. U kunt ook bepaalde metagegevens, zoals verliezen *die zijn gemaakt door* en *gewijzigd door*.

## <a name="next-steps"></a>Volgende stappen

[Volgorde van uw Data Box-zwaar](./data-box-heavy-deploy-ordered.md)