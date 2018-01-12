---
title: Vergelijking van Azure Data Lake Store met Azure Storage-Blob | Microsoft Docs
description: Vergelijking van Azure Data Lake Store met Azure Storage-Blob
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: b199525b-84de-4f79-9eb6-69a613b8b217
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2018
ms.author: nitinme
ms.openlocfilehash: 8881d28e1625664d0a124057821192aabfe1180b
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="comparing-azure-data-lake-store-and-azure-blob-storage"></a>Vergelijking van Azure Data Lake Store en Azure Blob-opslag
De tabel in dit artikel bevat een overzicht van de verschillen tussen Azure Data Lake Store en Azure Blob Storage langs een aantal belangrijke aspecten van big data-verwerking. Azure Blob Storage is een algemeen, schaalbare object store, die is ontworpen voor een groot aantal scenario's voor opslag. Azure Data Lake Store is een verwerkingsservice voor grote hoeveelheden-opslagplaats die is geoptimaliseerd voor big data-analyses werkbelastingen.

|  | Azure Data Lake Store | Azure Blob Storage |
| --- | --- | --- |
| Doel |Geoptimaliseerde opslag voor big data-analyses werkbelastingen |Algemeen object opslaan voor een groot aantal scenario's voor opslag |
| Gebruiksvoorbeelden |Interactieve batch, streaming analytics en machine learning-gegevens, zoals logboekbestanden, IoT-gegevens, klikt u op grote gegevenssets stromen |Elk type tekst of binaire gegevens, zoals toepassing back-end, back-upgegevens, Mediaopslag voor streaming en algemene gegevens van doel |
| Belangrijkste concepten |Data Lake Store-account bevat mappen, die gegevens die zijn opgeslagen als bestanden op zijn beurt bevat |Storage-account heeft containers die op zijn beurt de gegevens in de vorm van BLOB's heeft |
| structuur |Hiërarchisch bestandssysteem |Objectarchief met platte naamruimte |
| API |REST-API via HTTPS |REST-API via HTTP/HTTPS |
| API-serverzijde |[WebHDFS compatibele REST-API](https://msdn.microsoft.com/library/azure/mt693424.aspx) |[Azure Blob Storage REST-API](https://msdn.microsoft.com/library/azure/dd135733.aspx) |
| Hadoop File System-Client |Ja |Ja |
| Gegevensbewerkingen - verificatie |Op basis van [identiteiten met Azure Active Directory](../active-directory/active-directory-authentication-scenarios.md) |Op basis van gedeelde geheimen - [toegangssleutels van Account](../storage/common/storage-create-storage-account.md#manage-your-storage-account) en [gedeelde handtekening toegangstoetsen](../storage/common/storage-dotnet-shared-access-signature-part-1.md). |
| Gegevensbewerkingen - verificatieprotocol |OAuth 2.0. Aanroepen moeten een geldige JWT (JSON Web Token) uitgegeven door Azure Active Directory bevatten |Hash-based Message Authentication Code (HMAC). Aanroepen moeten een Base64-gecodeerd SHA-256-hash bevatten over een deel van de HTTP-aanvraag. |
| Gegevensbewerkingen - autorisatie |POSIX-toegangsbeheerlijsten (ACL's).  ACL's op basis van Azure Active Directory-identiteiten kunnen bestanden en mappen niveau worden ingesteld. |Gebruiken voor autorisatie van account-niveau – [toegangssleutels van Account](../storage/common/storage-create-storage-account.md#manage-your-storage-account)<br>Gebruik voor het account, container of blob-autorisatie - [handtekeningsleutels voor gedeelde toegang](../storage/common/storage-dotnet-shared-access-signature-part-1.md) |
| Gegevensbewerkingen - controle |Beschikbaar. Zie [hier](data-lake-store-diagnostic-logs.md) voor meer informatie. |Beschikbaar |
| Van Versleutelingsgegevens in rust |<ul><li>Transparant, serverzijde</li> <ul><li>Met service beheerd sleutels</li><li>Met de klant beheerde sleutels in Azure KeyVault</li></ul></ul> |<ul><li>Transparant, serverzijde</li> <ul><li>Met service beheerd sleutels</li><li>Met de klant beheerde sleutels in Azure KeyVault (binnenkort)</li></ul><li>Clientversleuteling</li></ul> |
| Beheertaken uit te voeren (bijvoorbeeld-Account maken) |[Toegangsbeheer op basis van rollen](../active-directory/role-based-access-control-what-is.md) (RBAC) opgegeven door de Azure voor accountbeheer |[Toegangsbeheer op basis van rollen](../active-directory/role-based-access-control-what-is.md) (RBAC) opgegeven door de Azure voor accountbeheer |
| SDK's voor ontwikkelaars |.NET, Java, Python, Node.js |.NET, Java, Python, Node.js, C++, Ruby |
| Prestaties van de Workload Analytics |Geoptimaliseerde prestaties voor parallelle analytics werkbelastingen. Hoge doorvoer en IOPS. |Niet is geoptimaliseerd voor analytics werkbelastingen |
| Maximale grootte |Er zijn geen limieten op account grootten, grootte of aantal bestanden |Specifieke limieten beschreven [hier](../azure-subscription-service-limits.md#storage-limits) |
| Geografische redundantie |Lokaal redundante (meerdere kopieën van gegevens in een Azure-regio) |Lokaal redundant (LRS), globaal redundante (GRS), leestoegang globaal redundante (RA-GRS). Zie [hier](../storage/common/storage-redundancy.md) voor meer informatie |
| Servicestatus |Algemeen beschikbaar |Algemeen beschikbaar |
| Regionale beschikbaarheid |Zie [hier](https://azure.microsoft.com/regions/#services) |Zie [hier](https://azure.microsoft.com/regions/#services) |
| Prijs |Zie [prijzen](https://azure.microsoft.com/pricing/details/data-lake-store/) |Zie [prijzen](https://azure.microsoft.com/pricing/details/storage/) |

### <a name="next-steps"></a>Volgende stappen
* [Overzicht van Azure Data Lake Store](data-lake-store-overview.md)
* [Aan de slag met Data Lake Store](data-lake-store-get-started-portal.md)

