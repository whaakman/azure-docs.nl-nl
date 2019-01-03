---
title: Bekende problemen met Azure Data Lake Storage Gen2 | Microsoft Docs
description: Meer informatie over de beperkingen en bekende problemen met Azure Data Lake Storage Gen2
services: storage
author: normesta
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: normesta
ms.openlocfilehash: bc6f59922286663435782623a72345c19adb494c
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "53995029"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Bekende problemen met Azure Data Lake Storage Gen2

In dit artikel bevat bekende problemen en tijdelijke beperkingen met Azure Data Lake Storage Gen2.

## <a name="api-interoperability"></a>API-interoperabiliteit

BLOB Storage-API's en Azure Data Lake Gen2 API's zijn niet compatibel met elkaar uit.

Als u hetzelfde hulpprogramma gebruiken om te werken met alle van de inhoud die u naar uw account moet uploadt, klikt u vervolgens niet inschakelt hiërarchische naamruimten op uw Blob storage-account tot deze API's gebruikt in combinatie met elkaar worden. Met behulp van een opslagaccount zonder de hiërarchische naamruimte toegangsbeheerlijsten betekent dat u geen toegang tot specifieke functies van Data Lake Storage Gen2, zoals de directory en bestandssysteem vervolgens hebt.

## <a name="blob-storage-apis"></a>BLOB storage-API 's

BLOB storage-API's zijn nog niet beschikbaar voor Azure Data Lake Storage Gen2 accounts.

Deze API's zijn uitgeschakeld om te voorkomen dat onbedoelde gegevens toegangsproblemen die optreden kunnen omdat Blob Storage-API's nog niet compatibel met Azure Data Lake Gen2 API's.

Als u deze API's gebruikt om gegevens te laden voordat ze zijn uitgeschakeld en u een productie-vereiste hebt voor toegang tot die gegevens, klikt u vervolgens Neem contact op met Microsoft Support met de volgende informatie:

* Abonnements-ID (de GUID, niet de naam)

* Opslagaccounts

* Of u actief in productie wordt beïnvloed, en zo ja, voor welke storage-accounts?

* Zelfs als u niet actief beïnvloed in productie, laat ons weten of moet u deze gegevens moeten worden gekopieerd naar een ander opslagaccount om een bepaalde reden, en indien dit het geval is, waarom?

In de volgende situaties, kunnen we herstellen toegang tot de Blob-API voor een beperkte periode zodat u kunt deze gegevens kopiëren naar een opslagaccount waarvoor geen hiërarchische ingeschakelde naamruimten.

Niet-beheerde schijven van de virtuele Machine (VM) is afhankelijk van het uitgeschakelde Blob Storage-API's, dus als u wilt inschakelen, hiërarchische naamruimten op een storage-account, houd rekening met het plaatsen van niet-beheerde VM-schijven in een opslagaccount waarvoor geen hiërarchische ingeschakelde naamruimten.

## <a name="azure-storage-explorer"></a>Azure Opslagverkenner

Als u wilt weergeven of Data Lake Storage Gen2 accounts beheren met behulp van Azure Storage Explorer, hebt u ten minste versie `1.6.0` van het hulpprogramma die beschikbaar is als een [gratis download](https://azure.microsoft.com/features/storage-explorer/).

Houd er rekening mee dat de versie van Storage Explorer die is ingesloten in de Azure Portal biedt momenteel geen ondersteuning voor bekijken of beheren van Data Lake Storage Gen2 accounts met hiërarchische ingeschakelde naamruimten.

## <a name="blob-viewing-tool"></a>Hulpprogramma voor BLOB weergeven

BLOB hulpprogramma bekijken in Azure portal is de enige beperking is ondersteuning voor Azure Data Lake Storage Gen2.

## <a name="third-party-applications"></a>Toepassingen van derden

Toepassingen van derden mogelijk geen ondersteuning voor Azure Data Lake Storage Gen2.

Ondersteuning is bij de keuze van de provider van elke toepassing van derden. Op dit moment Blob storage-API's en Azure Data Lake Storage Gen2 API's voor het beheren van dezelfde inhoud kan niet worden gebruikt. Als we werken om in te schakelen dat interoperabiliteit, is het mogelijk veel hulpprogramma's van derden wordt automatisch ondersteuning voor Azure Data Lake Storage Gen2.

## <a name="azcopy-support"></a>AzCopy-ondersteuning

AzCopy versie 8 biedt geen ondersteuning voor Azure Data Lake Storage Gen2.

In plaats daarvan gebruikt u de meest recente preview-versie van AzCopy ( [AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json) ) als het Azure Data Lake Storage Gen2 eindpunten ondersteunt.

## <a name="azure-event-grid"></a>Azure Event Grid

[Azure Event Grid](https://azure.microsoft.com/services/event-grid/) niet ontvangen van gebeurtenissen van Azure Data Lake Gen2 accounts omdat ze geen nog door deze accounts genereren.  

## <a name="soft-delete-and-snapshots"></a>Voorlopig verwijderen en momentopnamen

Zijn niet beschikbaar voor Azure Data Lake Storage Gen2 accounts voorlopig verwijderen en momentopnamen.

Alle versiebeheer functies, waaronder [momentopnamen](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob) en [voorlopig verwijderen](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) zijn nog niet beschikbaar voor opslagaccounts waarvoor hiërarchische ingeschakelde naamruimten.

## <a name="object-level-storage-tiers"></a>Object niveau opslaglagen

Object niveau opslaglagen (warm, koud en archief) zijn nog niet beschikbaar voor Azure Data Lake Storage Gen 2-accounts, maar ze zijn beschikbaar voor opslagaccounts waarvoor geen hiërarchische spaties ingeschakeld.

## <a name="azure-blob-storage-lifecycle-management-preview-policies"></a>Azure Blob-opslag lifecycle management (Preview)-beleid

Azure Blob-opslag lifecycle management (Preview)-beleid zijn nog niet beschikbaar voor Azure Data Lake Storage Gen2-accounts.

Deze beleidsregels zijn beschikbaar voor opslagaccounts waarvoor geen hiërarchische spaties ingeschakeld.

## <a name="diagnostic-logs"></a>Diagnostische logboeken

Logboeken met diagnostische gegevens zijn niet beschikbaar voor Azure Data Lake Storage Gen2 accounts.

Om aan te vragen de logboeken met diagnostische gegevens, neem contact op met ondersteuning voor Azure. Geef ze met de accountnaam van uw en de periode waarvoor u Logboeken nodig hebt.
