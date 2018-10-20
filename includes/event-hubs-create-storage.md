---
title: bestand opnemen
description: bestand opnemen
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: include
ms.date: 10/16/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: e499a0f7bec47e672c599c729a15cc3e3d04a28a
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49471608"
---
## <a name="create-a-storage-account-for-event-processor-host"></a>Een opslagaccount voor Event Processor Host maken
De Event Processor Host is een intelligente agent die het ontvangen van gebeurtenissen van Event Hubs vereenvoudigt door permanente controlepunten en parallelle ontvangsten te beheren. Voor controlepunten heeft de Event Processor Host een opslagaccount nodig. In het volgende voorbeeld ziet u hoe u een opslagaccount maakt en hoe u de sleutels krijgt voor toegang:

1. In de Azure portal en selecteer **een resource maken** op linksboven in het scherm.

2. Selecteer **Storage** en vervolgens **Storage-account - blob, file, table, queue**.
   
    ![Opslagaccount selecteren](./media/event-hubs-create-storage/create-storage1.png)

3. Op de **storage-account maken** pagina, de volgende stappen uit: 

    1. Voer een naam voor het opslagaccount. 
    2. Kies een Azure-abonnement met de event hub.
    3. Selecteer de resourcegroep waaraan de event hub.
    4. Selecteer een locatie voor het maken van de resource. 
    5. Klik vervolgens op **revisie + maken**.
   
    ![Storage-account maken - pagina](./media/event-hubs-create-storage/create-storage2.png)

4. Op de **bekijken + maken** pagina, controleert u de waarden en selecteer **maken**. 

    ![Instellingen van opslagaccount controleren en maken](./media/event-hubs-create-storage/review-create-storage-account.png)
5. Nadat u ziet de **implementaties voltooid** bericht, selecteer **heb resource** aan de bovenkant van de pagina. U kunt ook de Storage-Account-pagina starten door uw storage-account selecteren in de lijst met resources.  

    ![Selecteer het opslagaccount van de implementatie](./media/event-hubs-create-storage/select-storage-deployment.png) 
7. In de **Essentials** venster **Blobs**. 

    ![Selecteer de service Blobs](./media/event-hubs-create-storage/select-blobs-service.png)
1. Selecteer **+ Container** bovenaan, voer een **naam** voor de container en selecteer **OK**. 

    ![Een blob-container maken](./media/event-hubs-create-storage/create-blob-container.png)
1. Selecteer **toegangssleutels** in het menu aan de linkerkant en kopieer de waarde van **key1**. 

    Sla de volgende waarden in Kladblok of een andere tijdelijke locatie.
    - Naam van het storage-account
    - Toegangssleutel voor het opslagaccount
    - Naam van de container