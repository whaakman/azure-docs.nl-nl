---
title: Gebruik Azure AD-verificatie voor toegang tot Azure Media Services-API met REST | Microsoft Docs
description: Informatie over het openen van Azure Media Services-API met Azure Active Directory-verificatie met behulp van de REST.
services: media-services
documentationcenter: 
author: willzhan
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: willzhan;juliako
ms.openlocfilehash: 1c62857699fb29b3583363e1c6f2dc7874635f40
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-ad-authentication-to-access-the-azure-media-services-api-with-rest"></a>Azure AD-verificatie gebruiken voor toegang tot de Azure Media Services-API met REST

Het Azure Media Services-team heeft Azure Active Directory (Azure AD) authenticatie biedt ondersteuning voor Azure Media Services toegang uitgebracht. Deze ook plannen voor verificatie van Azure Access Control-service voor Media Services toegang afschaffen aangekondigd. Omdat elke Azure-abonnement en elke Media Services-account is gekoppeld aan een Azure AD-tenant, biedt ondersteuning voor Azure AD-verificatie veel voordelen voor beveiliging. Zie de volgende blogberichten en artikelen voor meer informatie over deze wijziging en de migratie (als u Media Services .NET SDK voor uw app gebruiken):

- [Azure Media Services introduceert ondersteuning voor Azure AD en afschaffing van toegangsbeheer verificatie](https://azure.microsoft.com/blog/azure%20media%20service%20aad%20auth%20and%20acs%20deprecation)
- [Toegang tot Azure Media Services-API met behulp van Azure AD-verificatie](media-services-use-aad-auth-to-access-ams-api.md)
- [Azure AD-verificatie gebruiken om Azure Media Services-API toegang met behulp van Microsoft .NET](media-services-dotnet-get-started-with-aad.md)
- [Aan de slag met Azure AD-verificatie met behulp van de Azure-portal](media-services-portal-get-started-with-aad.md)

Sommige klanten nodig voor het ontwikkelen van hun oplossingen Media Services onder de volgende beperkingen:

*   Gebruik van een programmeertaal die geen Microsoft .NET of C# of de runtime-omgeving is niet Windows.
*   Azure AD-bibliotheken, zoals Active Directory Authentication Libraries zijn niet beschikbaar voor de programmeertaal of kunnen niet worden gebruikt voor hun runtimeomgeving.

Sommige klanten hebben toepassingen ontwikkeld met behulp van REST-API voor toegangsbeheer, verificatie en Azure Media Services-toegang. Voor deze klanten moet u een manier om alleen de REST-API voor Azure AD-verificatie en daaropvolgende toegang van Azure Media Services gebruiken. U moet niet vertrouwen op een van de Azure AD-bibliotheken of op de Media Services .NET SDK. In dit artikel we wordt beschreven een oplossing en voorbeeldcode in dit scenario. Omdat de code alle REST-API-aanroepen, met geen afhankelijkheid van een Azure AD of Azure Media Services-bibliotheek, de code eenvoudig naar andere programmeertalen kan worden omgezet.

> [!IMPORTANT]
> Media Services ondersteunt momenteel, het model van Azure Access Control-services-verificatie. Access Control-verificatie wordt echter 1 juni 2018 afgeschaft. Het is raadzaam om te migreren naar het model van Azure AD authentication zo snel mogelijk.


## <a name="design"></a>Ontwerpen

In dit artikel gebruiken we het ontwerp van de volgende verificatie en autorisatie:

*  Autorisatie protocol: OAuth 2.0. OAuth 2.0 is een Webstandaard die betrekking heeft op zowel de verificatie en autorisatie. Dit wordt ondersteund door Google, Microsoft, Facebook en PayPal-nummer. Het is oktober 2012 bekrachtigd. Microsoft ondersteunt goed OAuth 2.0 en OpenID Connect. Beide van deze standaarden worden ondersteund in meerdere services en clientbibliotheken, waaronder Azure Active Directory, Open Web Interface voor .NET (OWIN)-Katana en Azure AD-bibliotheken.
*  Type verlenen: clientreferenties type verlenen. Clientreferenties is een van de vier grant-typen in OAuth 2.0. Dit wordt vaak gebruikt voor toegang tot Azure AD Microsoft Graph API.
*  Verificatiemodus: Service-principal. De verificatiemodus is gebruiker of interactieve verificatie.

Een totaal van vier toepassingen of services, betrokken zijn bij de Azure AD-verificatie en autorisatie stroom voor het gebruik van Media Services. De toepassingen en -services en de stroom worden in de volgende tabel beschreven:

|Toepassingstype |Toepassing |Stroom|
|---|---|---|
|Client | Klant-app of oplossing | Deze app (daadwerkelijk, de proxy) is geregistreerd in de Azure AD-tenant waarin de Azure-abonnement en het serviceaccount van de media bevinden. De service-principal van de geregistreerde app wordt vervolgens met de eigenaar of bijdrager rol in de Access Control (IAM) van het serviceaccount van de media verleend. De service-principal wordt vertegenwoordigd door het app-ID en client clientgeheim. |
|ID-Provider (IDP) | Azure AD als IDP | De geregistreerde app service-principal (client-ID en clientgeheim) is geverifieerd door Azure AD als de IDP. Deze verificatie wordt uitgevoerd, intern en impliciet. De client is geverifieerd als in clientreferentiestroom, in plaats van de gebruiker. |
|Secure Token Service (STS) / OAuth-server | Azure AD als STS | Na verificatie door de IDP (of de OAuth-Server in termen van OAuth 2.0), een toegangstoken of JSON Web Token (JWT) is uitgegeven door Azure AD als STS/OAuth-Server voor toegang tot de bron voor de middelste laag: in ons geval het Media Services REST-API-eindpunt. |
|Resource | Media Services REST-API | Elke Media Services REST API-aanroep is geautoriseerd door een toegangstoken dat is uitgegeven door Azure AD als STS of de OAuth-server. |

Als u de voorbeeldcode uitvoeren en vastleggen van een JWT of een toegangstoken, heeft de JWT de volgende kenmerken:

    aud: "https://rest.media.azure.net",

    iss: "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",

    iat: 1497146280,

    nbf: 1497146280,
    exp: 1497150180,

    aio: "Y2ZgYDjuy7SptPzO/muf+uRu1B+ZDQA=",

    appid: "02ed1e8e-af8b-477e-af3d-7e7219a99ac6",

    appidacr: "1",

    idp: "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",

    oid: "a938cfcc-d3de-479c-b0dd-d4ffe6f50f7c",

    sub: "a938cfcc-d3de-479c-b0dd-d4ffe6f50f7c",

    tid: "72f988bf-86f1-41af-91ab-2d7cd011db47",

Hier volgen de toewijzingen tussen de kenmerken in de JWT en de vier toepassingen of services in de voorgaande tabel:

|Toepassingstype |Toepassing |JWT-kenmerk |
|---|---|---|
|Client |Klant-app of oplossing |toepassings-id: '02ed1e8e-af8b-477e-af3d-7e7219a99ac6'. De client-ID van een toepassing die u bij Azure AD in de volgende sectie registreren wilt. |
|ID-Provider (IDP) | Azure AD als IDP |IDP: 'https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/'.  De GUID is de tenant-ID van Microsoft (microsoft.onmicrosoft.com). Elke tenant heeft zijn eigen, unieke ID. |
|Secure Token Service (STS) / OAuth-server |Azure AD als STS | iss: 'https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/'. De GUID is de tenant-ID van Microsoft (microsoft.onmicrosoft.com). |
|Resource | Media Services REST-API |AUD: 'https://rest.media.azure.net'. De ontvanger of de doelgroep van het toegangstoken. |

## <a name="steps-for-setup"></a>Stappen voor installatie

Om te registreren en een Azure AD-toepassing voor Azure AD-verificatie instellen en ophalen van een toegangstoken voor het aanroepen van het eindpunt van de REST-API van Azure Media Services, kunt u de volgende stappen:

1.  In de [klassieke Azure-portal](http://go.microsoft.com/fwlink/?LinkID=213885), een Azure AD-toepassing (bijvoorbeeld wzmediaservice) registreren bij de Azure AD-tenant (bijvoorbeeld microsoft.onmicrosoft.com). Het maakt niet uit of u geregistreerd als web-app of systeemeigen app. U kunt ook kiezen eventuele aanmeldings-URL en de antwoord-URL (bijvoorbeeld http://wzmediaservice.com voor beide).
2. In de [klassieke Azure-portal](http://go.microsoft.com/fwlink/?LinkID=213885), gaat u naar de **configureren** tabblad van uw toepassing. Opmerking de **client-ID**. Klik vervolgens onder **sleutels**, genereren een **clientsleutel** (clientgeheim). 

    > [!NOTE] 
    > Let op het clientgeheim. Deze worden niet meer weergegeven.
    
3.  In de [Azure-portal](http://ms.portal.azure.com), gaat u naar de Media Services-account. Selecteer de **toegangsbeheer** deelvenster (IAM). Een nieuw lid met de eigenaar of de rol Inzender toevoegt. Zoek op de naam van de toepassing die u in stap 1 (in dit voorbeeld wzmediaservice) geregistreerd voor de principal.

## <a name="info-to-collect"></a>Gegevens verzamelen

Als u wilt voorbereiden REST coderen, verzamelt de volgende gegevenspunten in de code op te nemen:

*   Azure AD als een STS-eindpunt: https://login.microsoftonline.com/microsoft.onmicrosoft.com/oauth2/token. Van dit eindpunt, wordt een toegangstoken JWT aangevraagd. Naast het fungeren als een IDP, fungeert Azure AD ook als een STS. Azure AD geeft een JWT voor toegang tot bedrijfsbronnen (een toegangstoken). Een JWT-token heeft verschillende claims.
*   Azure Media Services REST-API als resource of doelgroep: https://rest.media.azure.net.
*   Client-ID: Zie stap 2 in [stappen voor het installatieprogramma](#steps-for-setup).
*   Clientgeheim: Zie stap 2 in [stappen voor het installatieprogramma](#steps-for-setup).
*   Uw Media Services account REST-API-eindpunt in de volgende indeling:

    https://[media_service_account_name].restv2. [data_center].media.azure.net/API 

    Dit is het eindpunt met welke alle REST API voor Media Services in uw toepassing worden aangeroepen. Bijvoorbeeld: https://willzhanmswjapan.restv2.japanwest.media.azure.net/API.

Vervolgens kunt u deze vijf parameters plaatsen in het bestand web.config of app.config te gebruiken in uw code.

## <a name="sample-code"></a>Voorbeeldcode

U vindt de voorbeeldcode in [Azure AD-verificatie voor Azure Media Services toegang: beide via REST-API](https://github.com/willzhan/WAMSRESTSoln).

De voorbeeldcode bestaat uit twee delen:

*   Een bibliotheek-project voor het DLL-bestand dat de REST-API-code voor Azure AD-verificatie en autorisatie. Er wordt ook een methode voor het maken van de REST API-aanroepen met het Media Services REST-API-eindpunt met het toegangstoken.
*   Een console testclient, die Azure AD authentication initieert en verschillende Media Services REST-API-aanroepen.

Het voorbeeldproject heeft drie functies:

*   Azure AD-verificaties via de clientreferenties verlenen door met alleen de REST-API.
*   Azure Media Services-toegang met alleen de REST-API.
*   Azure Storage-toegang met behulp van alleen de REST-API (zoals gebruikt voor het maken van een Media Services-account met behulp van REST-API).


## <a name="where-is-the-refresh-token"></a>Waar bevindt zich het vernieuwingstoken?

Sommige lezers kunnen vragen: waar zich het vernieuwingstoken? Waarom niet hier een vernieuwingstoken gebruiken?

Het doel van een vernieuwingstoken is niet aan het vernieuwen van een toegangstoken. Het is in plaats daarvan ontworpen tussenkomst van de eindgebruiker verificatie of gebruiker negeren en een geldig toegangstoken nog steeds wanneer een eerder token is verlopen. Een betere naam voor een vernieuwingstoken mogelijk iets zoals 'opnieuw-sign-in-gebruikerstoken overslaan'.

Als u het OAuth 2.0 autorisatie verlenen stroom (gebruikersnaam en wachtwoord, fungeert namens een gebruiker) gebruikt, wordt er een vernieuwingstoken helpt u bij een vernieuwde toegangstoken ophalen zonder tussenkomst van de gebruiker vraagt. Echter, voor OAuth 2.0 clientreferenties verlenen stroom die in dit artikel worden beschreven, de client fungeert voor eigen rekening. U tussenkomst van de gebruiker helemaal niet nodig, en de autorisatie-server niet nodig (en won't) geeft u een vernieuwingstoken. Als u fouten opsporen in de **GetUrlEncodedJWT** methode, merkt u dat de reactie van het eindpunt van het token een toegangstoken, maar er is geen vernieuwingstoken heeft.

## <a name="next-steps"></a>Volgende stappen

Aan de slag met [bestanden uploaden naar uw account](media-services-dotnet-upload-files.md).
