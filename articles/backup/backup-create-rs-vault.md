---
title: 'Azure Backup: Recovery Services-kluizen maken'
description: het maken van Recovery Services-kluizen waarin de back-ups en herstelpunten
services: backup
author: sogup
manager: vijayts
keywords: Recovery Services-kluis; Azure VM back-up. Azure VM-herstel.
ms.service: backup
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: sogup
ms.openlocfilehash: 9fba7d679b7d0edb3c99207c99b23f9616c6fa0e
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66477582"
---
# <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

Een Recovery Services-kluis is een entiteit die de back-ups en herstelpunten die zijn gemaakt na verloop van tijd worden opgeslagen. De Recovery Services-kluis bevat ook het back-upbeleid die gekoppeld aan de beveiligde virtuele machines zijn.

Een Recovery Services-kluis maken:

1. Meld u aan bij uw abonnement in [Azure Portal](https://portal.azure.com/).

2. Selecteer in het menu links **Alle services**.

    ![Alle services selecteren](./media/backup-create-rs-vault/click-all-services.png)

3. In het dialoogvenster **Alle services** voert u **Recovery Services** in. De lijst met resources gefilterd op basis van uw invoer. Selecteer in de lijst met resources **Recovery Services-kluizen**.

    ![Recovery Services-kluizen invoeren en kiezen](./media/backup-create-rs-vault/all-services.png)

    De lijst met Recovery Services-kluizen in het abonnement wordt weergeven.

4. Op de **Recovery Services-kluizen** dashboard, selecteer **toevoegen**.

    ![Toevoegen van een Recovery Services-kluis](./media/backup-create-rs-vault/add-button-create-vault.png)

    De **Recovery Services-kluis** in het dialoogvenster wordt geopend. Waarden opgeven voor de **naam**, **abonnement**, **resourcegroep**, en **locatie**.

    ![De Recovery Services-kluis configureren](./media/backup-create-rs-vault/create-new-vault-dialog.png)

   - **Naam**: Voer een beschrijvende naam voor het identificeren van de kluis. De naam moet uniek zijn voor het Azure-abonnement. Geef een naam met ten minste twee, maar niet meer dan 50 tekens. De naam moet beginnen met een letter en bestaan alleen uit letters, cijfers en afbreekstreepjes bevatten.
   - **Abonnement**: Kies het abonnement te gebruiken. Als u een lid van slechts één abonnement bent, ziet u die naam. Als u niet zeker weet welk abonnement u moet gebruiken, gebruikt u het standaardabonnement (aanbevolen). Er zijn meerdere opties alleen als uw werk of school-account is gekoppeld aan meer dan één Azure-abonnement.
   - **Resourcegroep**: Gebruik een bestaande resourcegroep of maak een nieuwe. De lijst met beschikbare resourcegroepen in uw abonnement wilt bekijken, selecteert u **gebruik bestaande**, en selecteert u een resource in de vervolgkeuzelijst. Voor het maken van een nieuwe resourcegroep selecteert **nieuw** en voer de naam. Zie voor meer informatie over resourcegroepen [overzicht van Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).
   - **Locatie**: Selecteer de geografische regio voor de kluis. Een kluis ter bescherming van virtuele machines, de kluis maken **moet** zich in dezelfde regio bevinden als de virtuele machines.

      > [!IMPORTANT]
      > Als u niet zeker van de locatie van uw virtuele machine bent, sluit u het dialoogvenster. Ga naar de lijst met virtuele machines in de portal. Als u virtuele machines in meerdere regio's hebt, maakt u een Recovery Services-kluis in elke regio. Maak de kluis in de eerste locatie voordat u de kluis voor een andere locatie maken. Er is niet nodig om op te geven van de storage-accounts voor het opslaan van de back-upgegevens. De Recovery Services-kluis en de Azure Backup-service worden verwerkt die automatisch.
      >
      >

5. Wanneer u klaar bent om de Recovery Services-kluis maken, selecteert u **maken**.

    ![De Recovery Services-kluis maken](./media/backup-create-rs-vault/click-create-button.png)

    Het kan even om te maken van de Recovery Services-kluis. Houd de statusmeldingen in de **meldingen** gebied aan de rechterbovenhoek van de portal. Nadat de kluis is gemaakt, wordt het weergegeven in de lijst met Recovery Services-kluizen. Als u uw kluis niet ziet, selecteert u **vernieuwen**.

     ![Vernieuw de lijst met back-upkluizen](./media/backup-create-rs-vault/refresh-button.png)

## <a name="set-storage-redundancy"></a>Opslagredundantie instellen

Azure Backup verwerkt automatisch de opslag voor de kluis. U moet opgeven hoe die storage wordt gerepliceerd.

1. Klik op de blade **Recovery Services-kluizen** op de nieuwe kluis. Onder de **instellingen** sectie, klikt u op **eigenschappen**.
2. In **eigenschappen**onder **back-upconfiguratie**, klikt u op **Update**.

3. Selecteer het opslagtype voor replicatie en klikt u op **opslaan**.

     ![De opslagconfiguratie voor nieuwe kluis instellen](./media/backup-try-azure-backup-in-10-mins/recovery-services-vault-backup-configuration.png)

   - Het is raadzaam dat als u Azure als een primaire back-upopslag-eindpunt, echter ook doorgaan met de standaard **geografisch redundante** instelling.
   - Als Azure niet uw primaire eindpunt is voor back-upopslag, kiest u **Lokaal redundant**, zodat u de kosten voor Azure-opslag verlaagt.
   - Meer informatie over [geo](../storage/common/storage-redundancy-grs.md) en [lokale](../storage/common/storage-redundancy-lrs.md) redundantie.

> [!NOTE]
> Wijzigen van **type Opslagreplicatie** (lokaal redundant / geografisch redundante) voor een Recovery services-kluis is om te worden uitgevoerd voordat u back-ups configureren in de kluis. Als u back-up configureren, de optie om te wijzigen is uitgeschakeld en u niet wijzigen, de **type Opslagreplicatie**. 

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over](backup-azure-recovery-services-vault-overview.md) Recovery Services-kluizen.
[Meer informatie over](backup-azure-delete-vault.md) verwijderen Recovery Services-kluizen.
