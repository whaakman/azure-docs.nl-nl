---
title: 'REST-API: Accountbeheerbewerkingen in Azure Data Lake Storage Gen1 | Microsoft Docs'
description: Azure Data Lake Storage Gen1 en WebHDFS REST-API gebruiken voor het uitvoeren van accountbeheerbewerkingen in de Gen1 van Data Lake Storage-account
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 57ac6501-cb71-4f75-82c2-acc07c562889
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: 7f22fe7d1c3962e59922bc4e2795ed4f899e3eca
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46121665"
---
# <a name="account-management-operations-on-azure-data-lake-storage-gen1-using-rest-api"></a>Accountbeheerbewerkingen in Azure Data Lake Storage Gen1 met behulp van REST-API
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

In dit artikel leert u hoe u kunt uitvoeren van accountbeheerbewerkingen in Azure Data Lake Storage Gen1 met behulp van de REST-API. Accountbeheerbewerkingen zijn het maken van een Data Lake Storage Gen1 account, het verwijderen van een Data Lake Storage Gen1 account, enzovoort. Zie voor instructies over het uitvoeren van bestandssysteembewerkingen in Data Lake Storage Gen1 met behulp van REST-API, [bestandssysteembewerkingen in Data Lake Storage Gen1 met behulp van REST-API](data-lake-store-data-operations-rest-api.md).

## <a name="prerequisites"></a>Vereisten
* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **[cURL](http://curl.haxx.se/)**. In dit artikel wordt cURL gebruikt om u te laten zien hoe u REST API-aanroepen op basis van een Data Lake Storage Gen1-account.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Hoe verifieer ik met Azure Active Directory?
Er zijn twee benaderingen voor verificatie met Azure Active Directory.

* Zie voor verificatie van eindgebruikers voor uw toepassing (interactief), [eindgebruikersverificatie met Data Lake Storage Gen1 met .NET SDK](data-lake-store-end-user-authenticate-rest-api.md).
* Zie voor service-naar-serviceverificatie voor uw toepassing (niet-interactieve), [Service-naar-serviceverificatie met Data Lake Storage Gen1 met .NET SDK](data-lake-store-service-to-service-authenticate-rest-api.md).


## <a name="create-a-data-lake-storage-gen1-account"></a>Een Data Lake Storage Gen1-account maken
Deze bewerking is gebaseerd op de REST-API-aanroep die [hier](https://docs.microsoft.com/rest/api/datalakestore/accounts/create) wordt gedefinieerd.

Gebruik de volgende cURL-opdracht: Vervang  **\<yourstoragegen1name >** met de naam van uw Data Lake Storage Gen1.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstoragegen1name>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

Vervang \<`REDACTED`\> in de bovenstaande opdracht door het verificatietoken dat u eerder hebt opgehaald. De nettolading van de aanvraag voor deze opdracht zit in het bestand **input.json** dat is opgegeven voor de parameter `-d` hierboven. De inhoud van het bestand input.json ziet er ongeveer als uit als het volgende codefragment:

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }    

## <a name="delete-a-data-lake-storage-gen1-account"></a>Een Data Lake Storage Gen1-account verwijderen
Deze bewerking is gebaseerd op de REST-API-aanroep die [hier](https://docs.microsoft.com/rest/api/datalakestore/accounts/delete) wordt gedefinieerd.

Gebruik de volgende cURL-opdracht om een Data Lake Storage Gen1-account te verwijderen. Vervang  **\<yourstoragegen1name >** met de naam van uw Data Lake Storage Gen1-account.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstoragegen1name>?api-version=2015-10-01-preview

Als het goed is, wordt ongeveer het volgende codefragment weergegeven:

    HTTP/1.1 200 OK
    ...
    ...

## <a name="next-steps"></a>Volgende stappen
* [Bestandssysteembewerkingen in Data Lake Storage Gen1 met behulp van REST-API](data-lake-store-data-operations-rest-api.md).

## <a name="see-also"></a>Zie ook
* [Azure Data Lake Storage Gen1 REST API-verwijzing](https://docs.microsoft.com/rest/api/datalakestore/)
* [Open Source Big Data-toepassingen die compatibel zijn met Azure Data Lake Storage Gen1](data-lake-store-compatible-oss-other-applications.md)

