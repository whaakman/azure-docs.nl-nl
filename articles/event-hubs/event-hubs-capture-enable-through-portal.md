---
title: Azure Event Hubs Capture inschakelen via de portal | Microsoft Docs
description: Schakel de functie Event Hubs Capture in met behulp van de Azure-portal.
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/28/2017
ms.author: sethm
ms.openlocfilehash: 4a4ab1ee022b6b33d35217df916d01f32e04d3a4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="enable-event-hubs-capture-using-the-azure-portal"></a>Event Hubs Capture inschakelen met behulp van de Azure-portal

Met Azure [Event Hubs Capture][capture-overview] kunt u de gegevensstroom in uw Event Hubs automatisch bezorgen in een [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/)- of [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/)-account van uw keuze.

Wanneer u de gebeurtenishub maakt, kunt u Capture configureren met behulp van de [Azure-portal](https://portal.azure.com). U kunt de gegevens vastleggen in een [Blob Storage](https://azure.microsoft.com/services/storage/blobs/)-container van Azure of in een [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/)-account.

Zie voor meer informatie het [overzicht van Event Hubs Capture][capture-overview].

## <a name="capture-data-to-an-azure-storage-account"></a>Gegevens vastleggen in een Azure Storage-account  

Wanneer u een Event Hub maakt, kunt u Capture inschakelen door op het scherm **Event Hub maken** in de portal te klikken op de knop **Aan**. Vervolgens geeft u een opslagaccount en een container op door in de lijst **Capture-provider** te klikken op **Azure Storage**. Omdat Event Hubs Capture gebruikmaakt van service-naar-serviceverificatie met opslag, hoeft u geen verbindingsreeks voor opslag op te geven. De objectkiezer selecteert automatisch de resource-URI voor uw opslagaccount. Als u Azure Resource Manager gebruikt, moet u deze URI expliciet als een tekenreeks opgeven.

Het standaardtijdvenster is 5 minuten. De minimale waarde is 1, de maximale 15. Het venster **Grootte** heeft een bereik van 10-500 MB.

![][1]

## <a name="capture-data-to-an-azure-data-lake-store-account"></a>Gegevens vastleggen in een Azure Data Lake Store-account

Als u gegevens wilt vastleggen in Azure Data Lake Store, maakt u een Data Lake Store-account en een Event Hub:

### <a name="create-an-azure-data-lake-store-account-and-folders"></a>Een Azure Data Lake Store-account en -mappen maken

1. Maak een Data Lake Store-account volgens de instructies in [Aan de slag met Azure Data Lake Store met Azure Portal](../data-lake-store/data-lake-store-get-started-portal.md).
2. Volg de instructies in de sectie [Machtigingen toewijzen aan Event Hubs](../data-lake-store/data-lake-store-archive-eventhub-capture.md#assign-permissions-to-event-hubs) om een map te maken binnen het Data Lake Store-account waarin u de gegevens uit Event Hubs wilt vastleggen en machtigingen toe te wijzen aan de Event Hubs, zodat deze gegevens naar uw Data Lake Store-account kan schrijven.  

### <a name="create-an-event-hub"></a>Een Event Hub maken

1. De Event Hub moet deel uitmaken van het Azure-abonnement waarin de Azure Data Lake Store-account is opgenomen dat u zojuist hebt gemaakt. Maak de Event Hub door te klikken op de knop **Aan**, onder **Vastleggen** op de pagina **Event Hub maken** van de portal. 
2. Selecteer **Azure Data Lake Store** in de lijst **Capture-provider** op de pagina **Event Hub maken** van de portal.
3. Geef bij **Data Lake Store selecteren** het Data Lake Store-account op dat u eerder hebt gemaakt en voer vervolgens in het veld **Data Lake-pad** het pad in naar de gegevensmap die u hebt gemaakt.

    ![][3]

## <a name="add-or-configure-capture-on-an-existing-event-hub"></a>Capture toevoegen of configureren op een bestaande Event Hub

U kunt Capture configureren op bestaande Event Hubs in Event Hubs-naamruimten. Om Capture in te schakelen op een bestaande Event Hub of de Capture-instellingen te wijzigen, klikt u op de naamruimte om het scherm **Essentials** te laden en vervolgens klikt u op de Event Hub waarvan u de Capture-instelling wilt inschakelen of wijzigen. Ten slotte klikt u op de sectie **Eigenschappen** van de geopende pagina en wijzigt u de Capture-instellingen, zoals weergegeven in de volgende afbeelding:

### <a name="azure-blob-storage"></a>Azure Blob Storage

![][2]

### <a name="azure-data-lake-store"></a>Azure Data Lake Store

![][4]

[1]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture1.png
[2]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture2.png
[3]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture3.png
[4]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture4.png

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over Event Hubs Capture vindt u in het [overzicht van Event Hubs Capture][capture-overview].
- U kunt ook Event Hubs Capture configureren met behulp van Azure Resource Manager-sjablonen. Zie voor meer informatie [Capture inschakelen met behulp van een Azure Resource Manager-sjabloon](event-hubs-resource-manager-namespace-event-hub-enable-capture.md).
- [Aan de slag met Azure Data Lake Store met Azure Portal](../data-lake-store/data-lake-store-get-started-portal.md)

[capture-overview]: event-hubs-capture-overview.md