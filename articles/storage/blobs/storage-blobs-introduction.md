---
title: Inleiding tot Azure Blob storage | Microsoft Docs
description: Inleiding tot Azure Blob storage
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/28/2017
ms.author: tamram
ms.openlocfilehash: 0097f1c02b88343a135b6489130a6e0d35cf6fba
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="introduction-to-blob-storage"></a>Inleiding tot Blob-opslag

Azure Blob Storage is een service voor het opslaan van grote hoeveelheden ongestructureerde objectgegevens, zoals tekst of binaire gegevens, die via HTTP of HTTPS overal vandaan kunnen worden opgevraagd. U kunt Blob Storage gebruiken om gegevens openbaar te maken of om toepassingsgegevens privé op te slaan.

Veelvoorkomende toepassingen van Blob Storage zijn onder andere:

* Het rechtstreeks aan een browser leveren van afbeeldingen of documenten
* De opslag van bestanden voor gedistribueerde toegang
* Streaming van video en audio
* De opslag van gegevens voor back-up en herstel, herstel na noodgevallen en archivering
* De opslag van gegevens voor analyse door een on-premises in Azure gehoste service

## <a name="blob-service-concepts"></a>Concepten van Blob service

De Blob service bevat de volgende onderdelen:

![Blobarchitectuur](./media/storage-blobs-introduction/blob1.png)

* **Opslagaccount:** Alle toegang tot Azure Storage vindt plaats via een opslagaccount. Dit opslagaccount mag een **algemeen opslagaccount (v1 of v2)** of **Blob storage-accounts**. Zie [Over Azure Storage-accounts](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) voor meer informatie.

* **Container:** Een container is een groepering van een reeks blobs. Alle blobs moeten zich in een container bevinden. Een account kan een onbeperkt aantal containers bevatten. Een container kan een onbeperkt aantal blobs bevatten. De naam van een container mag alleen kleine letters bevatten.

* **Blob:** Een bestand van willekeurig type en willekeurige grootte. Azure Storage biedt drie typen blobs: blok-blobs, [pagina-blobs](storage-blob-pageblob-overview.md), en toevoeg-blobs.
  
    *Blok-blobs* zijn ideaal voor de opslag van tekst- of binaire bestanden, zoals documenten en mediabestanden. *Toevoeg-blobs* zijn vergelijkbaar met blok-blobs in dat opzicht dat ze bestaan uit blokken, maar ze zijn geoptimaliseerd voor toevoegbewerkingen; ze zijn daarom nuttig voor logboekscenario's. Eén blok-blob kan maximaal 50.000 blokken van maximaal 100 MB bevatten. Dat is in totaal iets meer dan 4.75 TB (100 MB x 50.000). Eén toevoeg-blob kan maximaal 50.000 blokken van maximaal 4 MB bevatten. Dat is in totaal iets meer dan 195 GB (4 MB x 50.000).
  
    *Pagina-blobs* kan maximaal 8 TB groot zijn en zijn efficiënter voor regelmatige lees-en schrijfbewerkingen. In Azure Virtuele Machines worden pagina-blobs gebruikt als besturingssysteem- en gegevensschijven.
  
    Zie [Containers, blobs en metagegevens een naam geven en hiernaar verwijderen](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata) voor meer informatie over de naamgeving van containers en blobs.

## <a name="next-steps"></a>Volgende stappen

* [Een opslagaccount maken](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Aan de slag met Blob storage met .NET](storage-dotnet-how-to-use-blobs.md)
* [Voorbeelden van Azure Storage met .NET](../common/storage-samples-dotnet.md)
* [Azure Storage-voorbeelden u Java gebruikt](../common/storage-samples-java.md)
