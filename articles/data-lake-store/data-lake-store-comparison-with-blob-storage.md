---
title: Vergelijking van Azure Data Lake Storage Gen1 met Azure Storage-Blob | Microsoft Docs
description: Vergelijking van Azure Data Lake Storage Gen1 met Azure Storage-Blob
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: b199525b-84de-4f79-9eb6-69a613b8b217
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: nitinme
ms.openlocfilehash: 3f37b996caa60a54c18388cb1f65873bf0aa4ed9
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55877691"
---
# <a name="comparing-azure-data-lake-storage-gen1-and-azure-blob-storage"></a>Vergelijking van Azure Data Lake Storage Gen1 en Azure Blob-opslag

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)] 

De tabel in dit artikel bevat een overzicht van de verschillen tussen Azure Data Lake Storage Gen1 en Azure Blob Storage langs een aantal belangrijke aspecten van de verwerking van big data. Azure Blob-opslag is een algemeen gebruik, schaalbare opslag die is ontworpen voor een groot aantal scenario's voor opslag. Azure Data Lake Storage Gen1 is een zeer grote opslagruimte die is geoptimaliseerd voor workloads voor analyse van big data.

|  | Azure Data Lake Storage Gen1 | Azure Blob Storage |
| --- | --- | --- |
| Doel |Geoptimaliseerde opslag voor workloads voor analyse van big data |Algemeen gebruik objectarchief voor een groot aantal scenario's voor opslag, met inbegrip van big data-analyses |
| Gebruiksvoorbeelden |Batch, interactieve, streaming analytics en machine learning-gegevens, zoals logboekbestanden, IoT-gegevens, klikstromen, grote gegevenssets |Elk type tekst of binaire gegevens, zoals toepassing back-end, back-upgegevens en opslag van media voor streaming en algemeen gebruik-gegevens. Bovendien volledige ondersteuning voor workloads voor analyse; batch, interactieve, streaming analytics en machine learning-gegevens, zoals logboekbestanden, IoT-gegevens, klikstromen, grote gegevenssets |
| Belangrijkste concepten |Data Lake Storage Gen1-account bevat mappen die op zijn beurt gegevens die zijn opgeslagen als bestanden bevat |Storage-account heeft containers, die op zijn beurt de gegevens in de vorm van blobs |
| structuur |Hiërarchisch bestandssysteem |Objectarchief met platte-naamruimte |
| API |REST-API via HTTPS |REST-API via HTTP/HTTPS |
| Server-side-API |[WebHDFS compatibele REST-API](https://msdn.microsoft.com/library/azure/mt693424.aspx) |[Azure Blob Storage REST API](https://msdn.microsoft.com/library/azure/dd135733.aspx) |
| Hadoop-bestand System-Client |Ja |Ja |
| Gegevensbewerkingen - verificatie |Op basis van [Azure Active Directory-identiteiten](../active-directory/develop/authentication-scenarios.md) |Op basis van gedeelde geheimen - [toegangssleutels](../storage/common/storage-account-manage.md#access-keys) en [gedeelde toegangssleutels handtekening](../storage/common/storage-dotnet-shared-access-signature-part-1.md). |
| Gegevensbewerkingen - verificatieprotocol |OAuth 2.0. Oproepen moeten bevatten een ongeldig JWT (JSON Web Token) dat is uitgegeven door Azure Active Directory |Hash-based Message Authentication Code (HMAC). Aanroepen, moeten een Base64-gecodeerd SHA-256-hash bevatten over een deel van de HTTP-aanvraag. |
| Gegevensbewerkingen - autorisatie |POSIX-toegangsbeheerlijsten (ACL's).  ACL's op basis van Azure Active Directory-identiteiten kunnen worden ingesteld op het niveau van bestanden en mappen. |Gebruik voor autorisatie accountniveau – [toegangssleutels van Account](../storage/common/storage-account-manage.md#access-keys)<br>Gebruik voor het account, container of blob autorisatie - [handtekeningsleutels voor gedeelde toegang](../storage/common/storage-dotnet-shared-access-signature-part-1.md) |
| Gegevensbewerkingen - controle |Beschikbaar. Zie [hier](data-lake-store-diagnostic-logs.md) voor meer informatie. |Beschikbaar |
| Van Versleutelingsgegevens in rust |<ul><li>Transparant en serverzijde</li> <ul><li>Met de service beheerde sleutels</li><li>Met de klant beheerde sleutels in Azure Key Vault</li></ul></ul> |<ul><li>Transparant en serverzijde</li> <ul><li>Met de service beheerde sleutels</li><li>Met de klant beheerde sleutels in Azure Key Vault (preview)</li></ul><li>Clientversleuteling</li></ul> |
| Bewerkingen (bijvoorbeeld-Account maken) |[Op rollen gebaseerd toegangsbeheer](../role-based-access-control/overview.md) (RBAC) verstrekt door Azure voor accountbeheer |[Op rollen gebaseerd toegangsbeheer](../role-based-access-control/overview.md) (RBAC) verstrekt door Azure voor accountbeheer |
| SDK's voor ontwikkelaars |.NET, Java, Python, Node.js |.NET, Java, Python, Node.js, C++, Ruby, PHP, Go, Android, iOS |
| Prestaties van de werkbelastingen Analytics |Geoptimaliseerde prestaties voor werkbelastingen voor parallelle analyse. Hoge doorvoer en IOPS. |Geoptimaliseerde prestaties voor werkbelastingen voor parallelle analyse. |
| Maximale grootte |Geen limieten voor grootte van accounts, bestanden of het aantal bestanden |Specifieke beperkingen beschreven [hier](../storage/common/storage-scalability-targets.md). Grotere account beperkt beschikbaar contact opnemen met de [ondersteuning voor Azure](https://azure.microsoft.com/support/faq/) |
| Geo-redundantie |Lokaal redundante (meerdere kopieën van gegevens in een Azure-regio) |Lokaal redundant (LRS), zone redundant (ZRS), wereldwijd redundant (GRS), leestoegang wereldwijd redundante (RA-GRS). Zie [hier](../storage/common/storage-redundancy.md) voor meer informatie |
| Servicestatus |Algemeen beschikbaar |Algemeen beschikbaar |
| Regionale beschikbaarheid |Zie [hier](https://azure.microsoft.com/regions/#services) |Beschikbaar in alle Azure-regio 's |
| Prijs |Zie [prijzen](https://azure.microsoft.com/pricing/details/data-lake-store/) |Zie [prijzen](https://azure.microsoft.com/pricing/details/storage/) |


