---
title: Inleiding tot Azure-wacht rijen-Azure Storage
description: Inleiding tot Azure-wacht rijen
author: mhopkins-msft
ms.author: mhopkins
ms.date: 06/07/2019
ms.service: storage
ms.subservice: queues
ms.topic: overview
ms.reviewer: cbrooks
ms.openlocfilehash: 67e4874fcca93633140b7630ceadd273d1646f86
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68721178"
---
# <a name="what-are-azure-queues"></a>Wat zijn wachtrijen in Azure?

Azure Queue-opslag is een service voor het opslaan van grote aantallen berichten. U opent berichten van overal ter wereld via geverifieerde oproepen met HTTP of HTTPS. Een wachtrij bericht kan Maxi maal 64 KB groot zijn. Een wachtrij kan miljoenen berichten bevatten, tot de totale capaciteits limiet van een opslag account.

## <a name="common-uses"></a>Veelvoorkomende toepassingen

Veelvoorkomende toepassingen van Queue Storage zijn onder andere:

* Het maken van een voorraad werk dat asynchroon moet worden verwerkt
* Het doorgeven van berichten van een Azure-webrol aan een Azure-werkrol

## <a name="queue-service-concepts"></a>Queue-service concepten

De Queue-service bevat de volgende onderdelen:

![Wachtrij concepten](./media/storage-queues-introduction/queue1.png)

* **URL-indeling:** Wacht rijen zijn adresseerbaar met behulp van de volgende URL-indeling:

    `https://<storage account>.queue.core.windows.net/<queue>`
  
    Met de volgende URL wordt een wachtrij in het diagram opgevraagd:  
  
    `https://myaccount.queue.core.windows.net/images-to-download`

* **Opslag account:** Alle toegang tot Azure Storage wordt uitgevoerd via een opslag account. Zie [Azure Storage Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) (Schaalbaarheids- en prestatiedoelen in Azure Storage) voor meer informatie over opslagaccountcapaciteit.

* **Wachtrij** Een wachtrij bevat een reeks berichten. De naam van de wachtrij **mag** alleen kleine letters bevatten. Zie [Naming Queues and Metadata](https://msdn.microsoft.com/library/azure/dd179349.aspx) (Wachtrijen en metagegevens een naam geven) voor informatie over de naamgeving van wachtrijen.

* **Bericht:** Een bericht, in een wille keurige indeling, van Maxi maal 64 KB. Voor versie 2017-07-29 is de Maxi maal toegestane time-to-Live 7 dagen. Voor versie 2017-07-29 of hoger kan het maximum aantal time-to-Live elk positief getal zijn of-1 waarmee wordt aangegeven dat het bericht niet verloopt. Als deze para meter wordt wegge laten, is de standaard time-to-Live zeven dagen.

## <a name="next-steps"></a>Volgende stappen

* [Een opslagaccount maken](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [Aan de slag met wacht rijen met .NET](storage-dotnet-how-to-use-queues.md)
