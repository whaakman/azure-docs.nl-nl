---
title: 'Verificatie van de eindgebruiker: REST-API met Data Lake Store met Azure Active Directory | Microsoft Docs'
description: Meer informatie over het bereiken van de eindgebruiker verificatie met Data Lake Store met Azure Active Directory met REST-API
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
ms.openlocfilehash: 5ee13756e4276055a8c9bdd9642d0766c3ca7e5c
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="end-user-authentication-with-data-lake-store-using-rest-api"></a>Verificatie van de eindgebruiker met Data Lake Store met REST-API
> [!div class="op_single_selector"]
> * [Java gebruiken](data-lake-store-end-user-authenticate-java-sdk.md)
> * [.NET-SDK gebruiken](data-lake-store-end-user-authenticate-net-sdk.md)
> * [Python gebruiken](data-lake-store-end-user-authenticate-python.md)
> * [REST-API gebruiken](data-lake-store-end-user-authenticate-rest-api.md)
> 
>  

In dit artikel leert u hoe u met de REST-API door eindgebruikers Authentication uitvoeren met Azure Data Lake Store. Zie voor de verificatie van de service-naar-service met Data Lake Store met REST-API, [authentication Service-naar-service met Data Lake Store met REST-API](data-lake-store-service-to-service-authenticate-rest-api.md).

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **Maak een 'Systeemeigen' Azure Active Directory-toepassing**. U moet voltooid van de stappen in [eindgebruiker verificatie met Data Lake Store met Azure Active Directory](data-lake-store-end-user-authenticate-using-active-directory.md).

* **[cURL](http://curl.haxx.se/)**. In dit artikel wordt cURL gebruikt om te laten zien hoe u REST API-aanroepen maakt voor een Data Lake Store-account.

## <a name="end-user-authentication"></a>Verificatie van de eindgebruiker
Verificatie van de eindgebruiker is de aanbevolen aanpak als u een gebruiker wilt zich aanmelden bij uw toepassing met behulp van Azure AD. De toepassing is toegang kunnen krijgen tot Azure-resources met hetzelfde niveau van toegang als de aangemelde gebruiker. De gebruiker moet hun referenties periodiek in volgorde voor uw toepassing toegankelijk.

Het resultaat dat de aanmelding door eindgebruikers is dat uw toepassing een toegangstoken en een vernieuwingstoken wordt gegeven. Het toegangstoken opgehaald gekoppeld aan elke aanvraag die wijzigingen in Data Lake Store of de Data Lake Analytics en het één uur standaard geldig is. Het vernieuwingstoken dat kan worden gebruikt voor het verkrijgen van een nieuw toegangstoken en is geldig voor maximaal twee weken standaard als u regelmatig gebruikt. U kunt twee verschillende benaderingen gebruiken voor aanmelding door eindgebruikers.

In dit scenario wordt de gebruiker via de toepassing gevraagd om zich te melden en worden alle bewerkingen uitgevoerd in de context van de gebruiker. De volgende stappen uitvoeren:

1. Leid de gebruiker via de toepassing om naar de volgende URL:
   
        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<APPLICATION-ID>&response_type=code&redirect_uri=<REDIRECT-URI>
   
   > [!NOTE]
   > \<REDIRECT-URI> moet zijn gecodeerd om te worden gebruikt in een URL. Dus https://localhost, gebruikt u `https%3A%2F%2Flocalhost`)
   > 
   > 
   
    Voor deze zelfstudie kunt u de waarden van de tijdelijke aanduiding in bovenstaande URL vervangen en deze in de adresbalk van de webbrowser plakken. U wordt omgeleid om u te verifiëren met uw Azure-aanmelding. Wanneer u bent aangemeld, wordt het antwoord weergegeven in de adresbalk van de browser. Het antwoord heeft de volgende indeling:
   
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>

2. Leg de autorisatiecode uit het antwoord vast. Voor deze zelfstudie kunt u de autorisatiecode uit de adresbalk van de webbrowser en op te geven in het bericht aanvraag voor het eindpunt van het token, zoals wordt weergegeven in het volgende fragment:
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<APPLICATION-ID> \
        -F code=<AUTHORIZATION-CODE>
   
   > [!NOTE]
   > In dit geval hoeft de \<REDIRECT-URI> niet te worden gecodeerd.
   > 
   > 

3. Het antwoord is een JSON-object dat een toegangstoken bevat (bijvoorbeeld `"access_token": "<ACCESS_TOKEN>"`) en een vernieuwingstoken (bijvoorbeeld `"refresh_token": "<REFRESH_TOKEN>"`). Uw toepassing gebruikt het toegangstoken om toegang te krijgen tot Azure Data Lake Store en het vernieuwingstoken om een nieuw toegangstoken op te halen wanneer het oude is verlopen.
   
        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before":    "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}

4. Wanneer het toegangstoken is verlopen, kunt u een nieuw toegangstoken met het vernieuwingstoken aanvragen, zoals wordt weergegeven in het volgende fragment:
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
             -F grant_type=refresh_token \
             -F resource=https://management.core.windows.net/ \
             -F client_id=<APPLICATION-ID> \
             -F refresh_token=<REFRESH-TOKEN>

Zie [De stroom voor autorisatiecodetoekenning](https://msdn.microsoft.com/library/azure/dn645542.aspx) voor meer informatie over interactieve gebruikersverificatie.
   
## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe service to service-verificatie gebruiken om te verifiëren met Azure Data Lake Store met REST-API. U kunt nu de volgende artikelen die uitleggen hoe de REST-API gebruiken om te werken met Azure Data Lake Store bekijken.

* [Accountbeheerbewerkingen op Data Lake Store met REST-API](data-lake-store-get-started-rest-api.md)
* [Bewerkingen van de gegevens in Data Lake Store met REST-API](data-lake-store-data-operations-rest-api.md)

