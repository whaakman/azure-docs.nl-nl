---
title: bestand opnemen
description: bestand opnemen
services: backup
author: markgalioto
ms.service: backup
ms.topic: include
ms.date: 5/14/2018
ms.author: markgal
ms.custom: include file
ms.openlocfilehash: 5590da80a1c217e7902e8e010688e40f5624898c
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34664911"
---
## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken
Een Recovery Services-kluis is een entiteit die de back-ups en herstelpunten die zijn gemaakt na verloop van tijd worden opgeslagen. De Recovery Services-kluis bevat ook het back-upbeleid die gekoppeld aan de beveiligde virtuele machines zijn.

Een Recovery Services-kluis maken:

1. Aanmelden bij uw abonnement in de [Azure-portal](https://portal.azure.com/).
2. Selecteer in het menu links **alle Services**.

    ![De optie alle Services in het hoofdmenu](./media/backup-create-rs-vault/click-all-services.png) <br/>

3. Typ in het dialoogvenster alle services *Recovery Services*. Als u te typen begint, filtert uw invoer in de lijst met resources. Als u wordt weergegeven, selecteert u **Recovery Services-kluizen**.

    ![Typ de Recovery Services in het dialoogvenster met alle services](./media/backup-create-rs-vault/all-services.png) <br/>

    De lijst met Recovery Services-kluizen in het abonnement wordt weergegeven.
4. Op de **Recovery Services-kluizen** selecteert u **toevoegen**.

    ![Een Recovery Services-kluis maken, stap 2](./media/backup-create-rs-vault/add-button-create-vault.png)

    De **Recovery Services-kluizen** menu wordt geopend. Moet u informatie te verstrekken voor **naam**, **abonnement**, **resourcegroep**, en **locatie**.

    ![Deelvenster 'Recovery Services-kluizen'](./media/backup-create-rs-vault/create-new-vault-dialog.png)
5. Voer bij **Naam** een beschrijvende naam in om de kluis aan te duiden. De naam moet uniek zijn voor het Azure-abonnement. Typ een naam die ten minste twee, maar niet meer dan 50 tekens bevat. De naam moet beginnen met een letter en mag bevatten alleen letters, cijfers en afbreekstreepjes.
6. Voor **abonnement**, kies het abonnement dat u wilt gebruiken. Als u een lid van slechts één abonnement, wordt deze naam wordt weergegeven. Als u niet zeker weet welk abonnement u moet gebruiken, gaat u met het standaard (of voorgestelde) abonnement. Er zijn meerdere opties alleen als uw werk of schoolaccount is gekoppeld aan meerdere Azure-abonnementen.
7. Voor **resourcegroep** gebruik van een bestaande resourcegroep of maak een nieuwe. Selecteer de beschikbare lijst met resourcegroepen in uw abonnement wilt bekijken, **gebruik bestaande**, en klik op de vervolgkeuzelijst. U maakt een nieuwe resourcegroep te selecteren **nieuw** en typ de naam. Zie voor meer informatie over resourcegroepen [overzicht van Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md).
8. Voor **locatie** Selecteer de geografische regio voor de kluis. Als u een kluis beveiligen van virtuele machines, de kluis maakt *moet* zich in dezelfde regio bevinden als de virtuele machines.

   > [!IMPORTANT]
   > Als u niet zeker weet van de locatie waar uw VM zich bevindt, sluit het dialoogvenster kluis maken en Ga naar de lijst met virtuele machines in de portal. Als u virtuele machines in meerdere regio's hebt, moet u in elke regio een Recovery Services-kluis maken. Maak de kluis op de eerste locatie voordat u verdergaat met de volgende locatie. Er is niet nodig om op te geven van de storage-accounts voor het opslaan van de back-upgegevens. De Recovery Services-kluis en de Azure Backup-service worden verwerkt die automatisch.
   >
   >

9. Wanneer u klaar bent voor de Recovery Services-kluis maken, klikt u op **maken**.

    ![Lijst met back-upkluizen](./media/backup-create-rs-vault/click-create-button.png)

    Het kan even duren voordat de Recovery Services-kluis is gemaakt. Controleer de statusmeldingen in de sectie meldingen (het gebied rechterbovenhoek van de portal). Nadat uw kluis is gemaakt, wordt deze weergegeven in de lijst met Recovery Services-kluizen. Als u uw kluis niet ziet, klikt u op **vernieuwen**.

     ![Lijst met back-upkluizen](./media/backup-create-rs-vault/refresh-button.png)
