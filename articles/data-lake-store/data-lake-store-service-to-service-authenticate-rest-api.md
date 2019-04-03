---
title: 'Service-naar-serviceverificatie: REST-API met Azure Data Lake Storage Gen1 met behulp van Azure Active Directory | Microsoft Docs'
description: Meer informatie over het bereiken van service-naar-serviceverificatie met Azure Data Lake Storage Gen1 met behulp van Azure Active Directory met behulp van REST-API
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: c48f7d7608b2b70f4ae41e2af5792cff72bb0dd2
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58885720"
---
# <a name="service-to-service-authentication-with-azure-data-lake-storage-gen1-using-rest-api"></a>Service-naar-serviceverificatie met Azure Data Lake Storage Gen1 met behulp van REST-API
> [!div class="op_single_selector"]
> * [Java gebruiken](data-lake-store-service-to-service-authenticate-java.md)
> * [.NET SDK gebruiken](data-lake-store-service-to-service-authenticate-net-sdk.md)
> * [Python gebruiken](data-lake-store-service-to-service-authenticate-python.md)
> * [REST API gebruiken](data-lake-store-service-to-service-authenticate-rest-api.md)
> 
> 

In dit artikel leert u over het gebruik van de REST-API-service-naar-serviceverificatie met Azure Data Lake Storage Gen1 doen. Zie voor verificatie van eindgebruikers met Data Lake Storage Gen1 met behulp van REST-API, [eindgebruikersverificatie met Data Lake Storage Gen1 met behulp van REST-API](data-lake-store-end-user-authenticate-rest-api.md).

## <a name="prerequisites"></a>Vereisten
* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **Maken van een Azure Active Directory-toepassing voor 'Web'**. U moet zijn voltooid de stappen in [Service-naar-serviceverificatie met Data Lake Storage Gen1 met behulp van Azure Active Directory](data-lake-store-service-to-service-authenticate-using-active-directory.md).

## <a name="service-to-service-authentication"></a>Verificatie van service-tot-service
In dit scenario biedt de toepassing eigen referenties om uit te voeren van de bewerkingen. Hiervoor moet u een POST-aanvraag, zoals weergegeven in het volgende codefragment uitgeven: 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

De uitvoer van de aanvraag bevat een verificatietoken (aangeduid door `access-token` in de onderstaande uitvoer) dat u vervolgens met de REST API-aanroepen doorgeven. Het verificatietoken opslaan in een tekstbestand; u moet deze bij het maken van de REST-aanroepen naar Data Lake Storage Gen1.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

In dit artikel wordt de **niet-interactieve** benadering gebruikt. Zie [Service-naar-service-aanroepen met referenties](https://msdn.microsoft.com/library/azure/dn645543.aspx) voor meer informatie over niet-interactieve (service-naar-service) aanroepen. 

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u service-naar-serviceverificatie gebruiken om te verifiëren met Data Lake Storage Gen1 met behulp van REST-API. U kunt nu de volgende artikelen die bespreken hoe u de REST-API gebruiken om te werken met Data Lake Storage Gen1 kijken.

* [Accountbeheerbewerkingen in Data Lake Storage Gen1 met behulp van REST-API](data-lake-store-get-started-rest-api.md)
* [Bewerkingen van de gegevens in Data Lake Storage Gen1 met behulp van REST-API](data-lake-store-data-operations-rest-api.md)

