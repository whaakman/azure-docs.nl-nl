---
title: 'REST API: Accountbeheerbewerkingen in Azure Data Lake Store | Microsoft Docs'
description: Azure Data Lake Store en WebHDFS REST API gebruiken voor het uitvoeren van accountbeheerbewerkingen in de Data Lake Store
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 57ac6501-cb71-4f75-82c2-acc07c562889
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2018
ms.author: nitinme
ms.openlocfilehash: 5fafde870a01a6ceb5e86f7b00b0ca11b748c68a
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="account-management-operations-on-azure-data-lake-store-using-rest-api"></a>Accountbeheerbewerkingen - Aan de slag met Azure Data Lake Store met REST API's
> [!div class="op_single_selector"]
> * [.NET-SDK](data-lake-store-get-started-net-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

In dit artikel leert u hoe u REST API gebruikt voor het uitvoeren van accountbeheerbewerkingen in Data Lake Store. Voorbeelden van accountbeheerbewerkingen zijn het maken van een Data Lake Store-account, het verwijderen van een Data Lake Store-account enzovoort. Als u wilt weten hoe u REST API gebruikt voor het uitvoeren van bestandssysteembewerkingen in Data Lake Store, raadpleegt u [Bestandssysteembewerkingen - Aan de slag met Azure Data Lake Store met REST API's](data-lake-store-data-operations-rest-api.md).

## <a name="prerequisites"></a>Vereisten
* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **[cURL](http://curl.haxx.se/)**. In dit artikel wordt cURL gebruikt om te laten zien hoe u REST API-aanroepen maakt voor een Data Lake Store-account.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Hoe verifieer ik met Azure Active Directory?
Er zijn twee benaderingen voor verificatie met Azure Active Directory.

* Zie [End-user authentication with Data Lake Store using .NET SDK](data-lake-store-end-user-authenticate-rest-api.md) (Eindgebruikersverificatie met Data Lake Store met behulp van .NET SDK) voor de verificatie van eindgebruikers voor uw toepassing (interactief).
* Zie [Service-to-service authentication with Data Lake Store using .NET SDK](data-lake-store-service-to-service-authenticate-rest-api.md) (Service-naar-serviceverificatie met Data Lake Store met behulp van .NET SDK) voor service-naar-serviceverificatie voor uw toepassing (niet-interactief).


## <a name="create-a-data-lake-store-account"></a>Een Data Lake Store-account maken
Deze bewerking is gebaseerd op de REST-API-aanroep die [hier](https://msdn.microsoft.com/library/mt694078.aspx) wordt gedefinieerd.

Gebruik de volgende cURL-opdracht: Vervang **\<yourstorename>** door de naam van uw Data Lake Store.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

Vervang \<`REDACTED`\> in de bovenstaande opdracht door het verificatietoken dat u eerder hebt opgehaald. De nettolading van de aanvraag voor deze opdracht zit in het bestand **input.json** dat is opgegeven voor de parameter `-d` hierboven. De inhoud van het bestand input.json ziet er ongeveer als uit als het volgende codefragment:

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }    

## <a name="delete-a-data-lake-store-account"></a>Een Data Lake Store-account verwijderen
Deze bewerking is gebaseerd op de REST-API-aanroep die [hier](https://msdn.microsoft.com/library/mt694075.aspx) wordt gedefinieerd.

Gebruik de volgende cURL-opdracht als u een Data Lake Store-account wilt verwijderen. Vervang **\<yourstorename>** door de naam van uw Data Lake Store.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

Als het goed is, wordt ongeveer het volgende codefragment weergegeven:

    HTTP/1.1 200 OK
    ...
    ...

## <a name="next-steps"></a>Volgende stappen
* [Bestandssysteembewerkingen - Aan de slag met Azure Data Lake Store met REST API's](data-lake-store-data-operations-rest-api.md).

## <a name="see-also"></a>Zie ook
* [Naslaginformatie over de REST API voor Data Lake Store](https://docs.microsoft.com/rest/api/datalakestore/)
* [Open Source Big Data-toepassingen die compatibel zijn met Azure Data Lake Store](data-lake-store-compatible-oss-other-applications.md)

