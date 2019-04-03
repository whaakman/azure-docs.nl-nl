---
title: 'Verificatie van eindgebruikers: REST-API met Azure Data Lake Storage Gen1 met behulp van Azure Active Directory | Microsoft Docs'
description: Meer informatie over het bereiken van eindgebruikersverificatie met Azure Data Lake Storage Gen1 met behulp van Azure Active Directory met behulp van REST-API
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
ms.openlocfilehash: 0ef65c23ee1bf4f064695779b71c8616427da204
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58881440"
---
# <a name="end-user-authentication-with-azure-data-lake-storage-gen1-using-rest-api"></a>Eindgebruikersverificatie met Azure Data Lake Storage Gen1 met behulp van REST-API
> [!div class="op_single_selector"]
> * [Java gebruiken](data-lake-store-end-user-authenticate-java-sdk.md)
> * [.NET SDK gebruiken](data-lake-store-end-user-authenticate-net-sdk.md)
> * [Python gebruiken](data-lake-store-end-user-authenticate-python.md)
> * [REST API gebruiken](data-lake-store-end-user-authenticate-rest-api.md)
> 
>  

In dit artikel leert u over het gebruik van de REST-API voor verificatie van eindgebruikers met Azure Data Lake Storage Gen1 doen. Zie voor service-naar-serviceverificatie met Data Lake Storage Gen1 met behulp van REST-API, [Service-naar-serviceverificatie met Data Lake Storage Gen1 met behulp van REST-API](data-lake-store-service-to-service-authenticate-rest-api.md).

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **Maken van een Azure Active Directory-toepassing voor 'Native'**. U moet zijn voltooid de stappen in [eindgebruikersverificatie met Data Lake Storage Gen1 met behulp van Azure Active Directory](data-lake-store-end-user-authenticate-using-active-directory.md).

* **[cURL](https://curl.haxx.se/)**. In dit artikel wordt cURL gebruikt om u te laten zien hoe u REST API-aanroepen op basis van een Data Lake Storage Gen1-account.

## <a name="end-user-authentication"></a>Verificatie van de eindgebruiker
Verificatie van de eindgebruiker is de aanbevolen aanpak als u wilt dat een gebruiker zich aanmelden bij uw toepassing met behulp van Azure AD. Uw toepassing is toegang kunnen krijgen tot Azure-resources met dezelfde mate van toegang als de gebruiker is aangemeld. De gebruiker moet hun referenties periodiek in volgorde voor uw toepassing om toegang te behouden.

Het resultaat dat de aanmelding door eindgebruikers is dat uw toepassing een toegangstoken en een vernieuwingstoken is opgegeven. Het toegangstoken opgehaald die zijn gekoppeld aan elke aanvraag aan de Data Lake Storage Gen1 of Data Lake Analytics en deze is geldig gedurende één uur standaard. Het vernieuwingstoken dat kan worden gebruikt om een nieuw toegangstoken verkrijgen en is geldig voor maximaal twee weken standaard, als u regelmatig gebruikt. U kunt twee verschillende methoden gebruiken voor aanmelding door eindgebruikers.

In dit scenario wordt de gebruiker via de toepassing gevraagd om zich te melden en worden alle bewerkingen uitgevoerd in de context van de gebruiker. Voer de volgende stappen uit:

1. Leid de gebruiker via de toepassing om naar de volgende URL:

        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<APPLICATION-ID>&response_type=code&redirect_uri=<REDIRECT-URI>

   > [!NOTE]
   > \<REDIRECT-URI> moet zijn gecodeerd om te worden gebruikt in een URL. Ja, voor https://localhost, gebruikt u `https%3A%2F%2Flocalhost`)

    Voor deze zelfstudie kunt u de waarden van de tijdelijke aanduiding in bovenstaande URL vervangen en deze in de adresbalk van de webbrowser plakken. U wordt omgeleid om u te verifiëren met uw Azure-aanmelding. Wanneer u bent aangemeld, wordt het antwoord weergegeven in de adresbalk van de browser. Het antwoord heeft de volgende indeling:

        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>

2. Leg de autorisatiecode uit het antwoord vast. Voor deze zelfstudie kunt u de autorisatiecode uit de adresbalk van de webbrowser en geeft u het in het bericht aanvragen bij het tokeneindpunt kopiëren, zoals wordt weergegeven in het volgende codefragment:

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

3. Het antwoord is een JSON-object dat een toegangstoken bevat (bijvoorbeeld `"access_token": "<ACCESS_TOKEN>"`) en een vernieuwingstoken (bijvoorbeeld `"refresh_token": "<REFRESH_TOKEN>"`). Uw toepassing gebruikt het toegangstoken tijdens toegang tot Azure Data Lake Storage Gen1 en het vernieuwingstoken dat aan een andere toegangstoken wanneer een toegangstoken is verlopen.

        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before":    "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}

4. Wanneer het toegangstoken is verlopen, kunt u een nieuw toegangstoken met het vernieuwingstoken aanvragen, zoals wordt weergegeven in het volgende codefragment:

        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
             -F grant_type=refresh_token \
             -F resource=https://management.core.windows.net/ \
             -F client_id=<APPLICATION-ID> \
             -F refresh_token=<REFRESH-TOKEN>

Zie [De stroom voor autorisatiecodetoekenning](https://msdn.microsoft.com/library/azure/dn645542.aspx) voor meer informatie over interactieve gebruikersverificatie.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u service-naar-serviceverificatie voor verificatie met Azure Data Lake Storage Gen1 met behulp van REST-API. U kunt nu de volgende artikelen die bespreken hoe u de REST-API gebruiken om te werken met Azure Data Lake Storage Gen1 kijken.

* [Accountbeheerbewerkingen in Data Lake Storage Gen1 met behulp van REST-API](data-lake-store-get-started-rest-api.md)
* [Bewerkingen van de gegevens in Data Lake Storage Gen1 met behulp van REST-API](data-lake-store-data-operations-rest-api.md)

