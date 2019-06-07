---
title: 'Inleiding tot Azure-wachtrijen: Azure Storage'
description: Inleiding tot Azure-wachtrijen
services: storage
author: mhopkins-msft
ms.service: storage
ms.topic: overview
ms.date: 06/07/2019
ms.author: mhopkins
ms.reviewer: cbrooks
ms.subservice: queues
ms.openlocfilehash: bc3045e3d3b6977555818fcdb3dcaf3246ebd200
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2019
ms.locfileid: "66754806"
---
# <a name="what-are-azure-queues"></a>Wat zijn wachtrijen in Azure?

Azure Queue storage is een service voor het opslaan van grote aantallen berichten. Toegang tot berichten vanaf elke locatie ter wereld via geverifieerde oproepen met HTTP of HTTPS. Een wachtrijbericht mag maximaal 64 KB groot zijn. Een wachtrij kan miljoenen berichten bevatten, tot de totale capaciteitslimiet van een opslagaccount bevatten.

## <a name="common-uses"></a>Veelvoorkomende toepassingen

Veelvoorkomende toepassingen van Queue Storage zijn onder andere:

* Het maken van een voorraad werk dat asynchroon moet worden verwerkt
* Het doorgeven van berichten van een Azure-webrol aan een Azure-werkrol

## <a name="queue-service-concepts"></a>Concepten van Queue-service

De Queue-service bevat de volgende onderdelen:

![Concepten van Queue](./media/storage-queues-introduction/queue1.png)

* **URL-indeling hebben:** Wachtrijen kunnen worden opgevraagd met behulp van de volgende URL-indeling:

    `https://<storage account>.queue.core.windows.net/<queue>`
  
    Met de volgende URL wordt een wachtrij in het diagram opgevraagd:  
  
    `https://myaccount.queue.core.windows.net/images-to-download`

* **Storage-account:** Alle toegang tot Azure Storage vindt plaats via een opslagaccount. Zie [Azure Storage Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) (Schaalbaarheids- en prestatiedoelen in Azure Storage) voor meer informatie over opslagaccountcapaciteit.

* **Wachtrij:** Een wachtrij bevat een set berichten. Naam van de wachtrij **moet** alleen kleine letters bevatten. Zie [Naming Queues and Metadata](https://msdn.microsoft.com/library/azure/dd179349.aspx) (Wachtrijen en metagegevens een naam geven) voor informatie over de naamgeving van wachtrijen.

* **Bericht:** Een bericht in een willekeurige indeling, van maximaal 64 KB. Voordat u versie 2017-07-29 is de toegestane time-to-live zeven dagen. Voor versie 2017-07-29 of hoger, de maximale time-to-live mag bestaan uit een positief getal of -1 die aangeeft dat het bericht niet verloopt. Als deze parameter wordt weggelaten, de standaard time-to-live is zeven dagen.

## <a name="next-steps"></a>Volgende stappen

* [Een opslagaccount maken](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [Aan de slag met wachtrijen met .NET](storage-dotnet-how-to-use-queues.md)
