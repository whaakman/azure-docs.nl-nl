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
ms.openlocfilehash: 690e0da160042b7ac432b1a36c024a8a8f439823
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58125203"
---
## <a name="create-a-storage-account-for-event-processor-host"></a>Een opslagaccount voor Event Processor Host maken
De Event Processor Host is een intelligente agent die het ontvangen van gebeurtenissen van Event Hubs vereenvoudigt door permanente controlepunten en parallelle ontvangsten te beheren. Voor controlepunten heeft de Event Processor Host een opslagaccount nodig. In het volgende voorbeeld ziet u hoe u een opslagaccount maakt en hoe u de sleutels krijgt voor toegang:

1. Open de Azure-portal en klik op **Een resource maken** linksboven in het scherm.

2. Selecteer **Storage** en vervolgens **Storage-account - blob, file, table, queue**.
   
    ![Opslagaccount selecteren](./media/event-hubs-create-storage/create-storage1.png)

3. Voer op de pagina **Opslagaccount maken** de volgende stappen uit: 

   1. Voer een naam in voor het opslagaccount. 
   2. Kies een Azure-abonnement dat de Event Hub bevat.
   3. Selecteer de resourcegroep waar de Event Hub deel van uitmaakt.
   4. Selecteer op welke locatie u de resource wilt maken. 
   5. Klik dan op **Controleren + maken**.
   
      ![Opslagaccount maken - Pagina](./media/event-hubs-create-storage/create-storage2.png)

4. Op de pagina **Controleren + maken** controleert u de waarden en selecteert u **Maken**. 

    ![Opslagaccountinstellingen controleren en maken](./media/event-hubs-create-storage/review-create-storage-account.png)
5. Nadat u ziet de **implementaties voltooid** bericht, selecteer **naar de resource gaan** aan de bovenkant van de pagina. U kunt ook de Storage-Account-pagina starten door uw storage-account selecteren in de lijst met resources.  

    ![Selecteer het opslagaccount uit de implementatie](./media/event-hubs-create-storage/select-storage-deployment.png) 
7. Selecteer in het venster **Essentials** de optie **Blobs**. 

    ![Selecteer de service Blobs](./media/event-hubs-create-storage/select-blobs-service.png)
1. Selecteer **+ Container** bovenaan, voer een **naam** voor de container en selecteer **OK**. 

    ![Een blob-container maken](./media/event-hubs-create-storage/create-blob-container.png)
1. Selecteer **toegangssleutels** in het menu aan de linkerkant en kopieer de waarde van **key1**. 

    Sla de volgende waarden in Kladblok of een andere tijdelijke locatie.
    - Naam van het storage-account
    - Toegangssleutel voor het opslagaccount
    - Naam van de container
