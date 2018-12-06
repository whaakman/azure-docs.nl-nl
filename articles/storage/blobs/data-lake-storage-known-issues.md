---
title: Bekende problemen met Azure Data Lake Storage Gen2 | Microsoft Docs
description: Meer informatie over de beperkingen en bekende problemen met Azure Data Lake Storage Gen2
services: storage
author: normesta
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: normesta
ms.openlocfilehash: 83e9dfbe18dd79e8547e6b48daef39a5aed2cced
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52975282"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Bekende problemen met Azure Data Lake Storage Gen2

In dit artikel bevat bekende problemen en tijdelijke beperkingen met Azure Data Lake Storage Gen2.

## <a name="api-interoperability"></a>API-interoperabiliteit

BLOB Storage-API's en Azure Data Lake Gen2 API's zijn niet compatibel met elkaar uit.

Als u hetzelfde hulpprogramma gebruiken om te werken met alle van de inhoud die u naar uw account moet uploadt, klikt u vervolgens niet inschakelt hiërarchische naamruimten op uw Blob storage-account tot deze API's gebruikt in combinatie met elkaar worden. Met behulp van een opslagaccount zonder de hiërarchische naamruimte toegangsbeheerlijsten betekent dat u geen toegang tot specifieke functies van Data Lake Storage Gen2, zoals de directory en bestandssysteem vervolgens hebt.

## <a name="blob-storage-apis"></a>BLOB storage-API 's

BLOB storage-API's zijn nog niet beschikbaar voor Azure Data Lake Storage Gen 2-accounts.

Deze API's zijn uitgeschakeld om te voorkomen dat onbedoelde gegevens toegangsproblemen die optreden kunnen omdat Blob Storage-API's nog niet compatibel met Azure Data Lake Gen2 API's.

Niet-beheerde schijven van de virtuele Machine (VM) is afhankelijk van deze API's, dus als u wilt inschakelen, hiërarchische naamruimten op een storage-account, houd rekening met het plaatsen van niet-beheerde VM-schijven in een opslagaccount waarvoor geen hiërarchische ingeschakelde naamruimten.

## <a name="azure-storage-explorer"></a>Azure Opslagverkenner

Sommige functies in Storage Explorer werken niet nog met Azure Data Lake Storage Gen2 bestandssystemen. Deze beperkingen zijn van toepassing op de [zelfstandige versie](https://azure.microsoft.com/features/storage-explorer/) van Azure Storage Explorer, evenals de versie die wordt weergegeven in de Azure-portal.

## <a name="blob-viewing-tool"></a>Hulpprogramma voor BLOB weergeven

BLOB hulpprogramma bekijken in Azure portal is de enige beperking is ondersteuning voor Azure Data Lake Storage Gen2.

## <a name="third-party-applications"></a>Toepassingen van derden

Toepassingen van derden mogelijk geen ondersteuning voor Azure Data Lake Storage Gen2.

Ondersteuning is bij de keuze van de provider van elke toepassing van derden. Op dit moment Blob storage-API's en Azure Data Lake Storage Gen2 API's voor het beheren van dezelfde inhoud kan niet worden gebruikt. Als we werken om in te schakelen dat interoperabiliteit, is het mogelijk veel hulpprogramma's van derden wordt automatisch ondersteuning voor Azure Data Lake Storage Gen2.

## <a name="azcopy-support"></a>AzCopy-ondersteuning

AzCopy versie 8 biedt geen ondersteuning voor Azure Data Lake Storage Gen2.

In plaats daarvan gebruikt u de meest recente preview-versie van AzCopy ( [AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json) ) als het Azure Data Lake Storage Gen2 eindpunten ondersteunt.

## <a name="oauth-authentication"></a>OAuth-verificatie

Services zoals Azure Databricks, HDInsight en Azure Data Factory integreren niet nog met Azure Active Directory (Azure AD) OAuth bearer-token verificatie.

## <a name="access-control-lists-acl"></a>Toegangsbeheerlijsten (ACL)

Directory- en bestandsniveau toegangsbeheerlijsten (ACL) zijn moeilijk te beheren. Er is geen gebruikersinterface gebaseerde hulpprogramma waarmee u kunt ophalen en instellen van de toegangsbeheerlijsten.

## <a name="azure-event-grid"></a>Azure Event Grid

[Azure Event Grid](https://azure.microsoft.com/services/event-grid/) niet ontvangen van gebeurtenissen van Azure Data Lake Gen2 accounts omdat ze geen nog door deze accounts genereren.  

## <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer

U kunt rollen gebaseerd toegangsbeheer niet toepassen op bestandssysteemobjecten in een Azure Data Lake Storage Gen2-account.

## <a name="sql-data-warehouse-polybase"></a>SQL datawarehouse PolyBase

Wanneer Storage-Firewalls zijn ingeschakeld op een Azure Storage-account, SQL Data Warehouse [Polybase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide?view=sql-server-2017) geen toegang tot deze accounts.

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
