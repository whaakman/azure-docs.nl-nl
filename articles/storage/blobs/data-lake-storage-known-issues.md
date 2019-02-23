---
title: Bekende problemen met Azure Data Lake Storage Gen2 | Microsoft Docs
description: Meer informatie over de beperkingen en bekende problemen met Azure Data Lake Storage Gen2
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 02/22/2019
ms.author: normesta
ms.openlocfilehash: abc20d8ef51f94e25d0e46ec199ca715bf6cce22
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56736784"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Bekende problemen met Azure Data Lake Storage Gen2

In dit artikel bevat bekende problemen en tijdelijke beperkingen met Data Lake Storage Gen2.

## <a name="data-lake-storage-gen2-apis"></a>Data Lake Storage Gen2 API 's

API's zijn nog niet beschikbaar voor accounts van Data Lake Storage Gen2.

## <a name="blob-storage-apis"></a>BLOB storage-API 's

BLOB storage-API's zijn nog niet beschikbaar voor accounts van Data Lake Storage Gen2.

Deze API's zijn uitgeschakeld om te voorkomen dat onbedoelde gegevens toegangsproblemen die optreden kunnen omdat Blob Storage-API's nog niet compatibel met Azure Data Lake Gen2 API's.

Als u deze API's gebruikt om gegevens te laden voordat ze zijn uitgeschakeld en u een productie-vereiste hebt voor toegang tot die gegevens, klikt u vervolgens Neem contact op met Microsoft Support met de volgende informatie:

* Abonnements-ID (de GUID, niet de naam)

* Opslagaccounts

* Of u actief in productie wordt beïnvloed, en zo ja, voor welke storage-accounts?

* Zelfs als u niet actief beïnvloed in productie, laat ons weten of moet u deze gegevens moeten worden gekopieerd naar een ander opslagaccount om een bepaalde reden, en indien dit het geval is, waarom?

In de volgende situaties, kunnen we herstellen toegang tot de Blob-API voor een beperkte periode zodat u deze gegevens kunt kopiëren naar een opslagaccount dat u beschikt niet over de hiërarchische naamruimte functie is ingeschakeld.

Niet-beheerde schijven van de virtuele Machine (VM) is afhankelijk van het uitgeschakelde Blob Storage-API's, dus als u wilt inschakelen, een hiërarchische naamruimte op een storage-account, houd rekening met het plaatsen van niet-beheerde VM-schijven in een storage-account dat beschikt niet over de functie voor hiërarchische naamruimte ingeschakeld.

## <a name="api-interoperability"></a>API-interoperabiliteit

BLOB Storage-API's en Azure Data Lake Gen2 API's zijn niet compatibel met elkaar uit.

Als u hulpprogramma's, toepassingen, services of scripts die gebruikmaken van Blob-API's hebt en u wilt gebruiken voor het werken met alle inhoud die u naar uw account uploadt, klikt u vervolgens niet inschakelt een hiërarchische naamruimte op uw Blob storage-account tot de Blob-API's worden compatibel met Azure Data Lake Gen2 API's. Met behulp van een storage-account zonder een hiërarchische naamruimte toegangsbeheerlijsten betekent dat u geen toegang tot specifieke functies van Data Lake Storage Gen2, zoals de directory en bestandssysteem vervolgens hebt.

## <a name="azure-storage-explorer"></a>Azure Opslagverkenner

Als u wilt weergeven of Data Lake Storage Gen2 accounts beheren met behulp van Azure Storage Explorer, hebt u ten minste versie `1.6.0` van het hulpprogramma die beschikbaar is als een [gratis download](https://azure.microsoft.com/features/storage-explorer/).

Houd er rekening mee dat de versie van Storage Explorer die is ingesloten in de Azure Portal biedt momenteel geen ondersteuning voor bekijken of beheren van Data Lake Storage Gen2 accounts met de functie voor hiërarchische naamruimte ingeschakeld.

## <a name="blob-viewing-tool"></a>Hulpprogramma voor BLOB weergeven

BLOB hulpprogramma bekijken in Azure portal is de enige beperking is ondersteuning voor Data Lake Storage Gen2.

## <a name="third-party-applications"></a>Toepassingen van derden

Data Lake Storage Gen2 mogelijk geen ondersteuning voor toepassingen van derden.

Ondersteuning is bij de keuze van de provider van elke toepassing van derden. Op dit moment Blob storage-API's en Data Lake Storage Gen2 API's voor het beheren van dezelfde inhoud kan niet worden gebruikt. Als we werken om in te schakelen dat interoperabiliteit, is het mogelijk dat veel hulpprogramma's van derden Data Lake Storage Gen2 automatisch ondersteunen.

## <a name="azcopy-support"></a>AzCopy-ondersteuning

AzCopy versie 8 biedt geen ondersteuning voor Data Lake Storage Gen2.

In plaats daarvan gebruikt u de meest recente preview-versie van AzCopy ( [AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json) ) als het Data Lake Storage Gen2 eindpunten ondersteunt.

## <a name="azure-event-grid"></a>Azure Event Grid

[Azure Event Grid](https://azure.microsoft.com/services/event-grid/) niet ontvangen van gebeurtenissen van Azure Data Lake Gen2 accounts omdat ze geen nog door deze accounts genereren.  

## <a name="soft-delete-and-snapshots"></a>Voorlopig verwijderen en momentopnamen

Zijn niet beschikbaar voor accounts van Data Lake Storage Gen2 voorlopig verwijderen en momentopnamen.

Alle versiebeheer functies, waaronder [momentopnamen](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob) en [voorlopig verwijderen](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) zijn nog niet beschikbaar voor opslagaccounts waarvoor de hiërarchische naamruimte functie is ingeschakeld.

## <a name="object-level-storage-tiers"></a>Object niveau opslaglagen

Object niveau opslaglagen (warm, koud en archief) zijn nog niet beschikbaar voor Azure Data Lake Storage Gen 2-accounts, maar ze zijn beschikbaar voor opslagaccounts waarvoor geen de hiërarchische naamruimte functie is ingeschakeld.

## <a name="azure-blob-storage-lifecycle-management-policies"></a>Azure Blob-opslag lifecycle management-beleid

Azure Blob-opslag lifecycle management-beleid zijn nog niet beschikbaar voor accounts van Data Lake Storage Gen2.

Deze beleidsregels zijn beschikbaar voor opslagaccounts waarvoor geen de hiërarchische naamruimte functie is ingeschakeld.

## <a name="diagnostic-logs"></a>Diagnostische logboeken

Logboeken met diagnostische gegevens zijn niet beschikbaar voor accounts van Data Lake Storage Gen2.
