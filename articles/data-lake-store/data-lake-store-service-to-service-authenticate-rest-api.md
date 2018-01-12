---
title: 'Verificatie van de service-naar-service: REST-API met Data Lake Store met Azure Active Directory | Microsoft Docs'
description: Meer informatie over het bereiken van de service to service-verificatie met Data Lake Store met Azure Active Directory met REST-API
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2018
ms.author: nitinme
ms.openlocfilehash: 80934d3e5ded5c01e473f8450a3484d84c46e94e
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="service-to-service-authentication-with-data-lake-store-using-rest-api"></a>Verificatie met Data Lake Store met REST-API-services
> [!div class="op_single_selector"]
> * [Java gebruiken](data-lake-store-service-to-service-authenticate-java.md)
> * [.NET-SDK gebruiken](data-lake-store-service-to-service-authenticate-net-sdk.md)
> * [Python gebruiken](data-lake-store-service-to-service-authenticate-python.md)
> * [REST-API gebruiken](data-lake-store-service-to-service-authenticate-rest-api.md)
> 
> 

In dit artikel leert u hoe u met de REST-API Authentication service-naar-service uitvoeren met Azure Data Lake Store. Zie voor de verificatie van de eindgebruiker met Data Lake Store met REST-API, [eindgebruiker verificatie met Data Lake Store met REST-API](data-lake-store-end-user-authenticate-rest-api.md).

## <a name="prerequisites"></a>Vereisten
* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **Maken van een Azure Active Directory 'Web' toepassing**. U moet voltooid van de stappen in [authentication Service-naar-service met Data Lake Store met Azure Active Directory](data-lake-store-service-to-service-authenticate-using-active-directory.md).

## <a name="service-to-service-authentication"></a>Verificatie van service-tot-service
In dit scenario biedt de toepassing eigen referenties om de bewerkingen te voeren. Hiervoor moet u een POST-aanvraag, zoals weergegeven in het volgende fragment uitgeven: 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

De uitvoer van de aanvraag bevat een verificatietoken (aangeduid met `access-token` in de onderstaande uitvoer) dat u vervolgens gaat met de REST-API-aanroepen doorgeven. Het verificatietoken opslaan in een tekstbestand; u hebt deze bij het aanroepen van de REST aan de Data Lake Store.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

In dit artikel wordt de **niet-interactieve** benadering gebruikt. Zie [Service-naar-service-aanroepen met referenties](https://msdn.microsoft.com/library/azure/dn645543.aspx) voor meer informatie over niet-interactieve (service-naar-service) aanroepen. 

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe service to service-verificatie gebruiken om te verifiëren met Azure Data Lake Store met REST-API. U kunt nu de volgende artikelen die uitleggen hoe de REST-API gebruiken om te werken met Azure Data Lake Store bekijken.

* [Accountbeheerbewerkingen op Data Lake Store met REST-API](data-lake-store-get-started-rest-api.md)
* [Bewerkingen van de gegevens in Data Lake Store met REST-API](data-lake-store-data-operations-rest-api.md)

