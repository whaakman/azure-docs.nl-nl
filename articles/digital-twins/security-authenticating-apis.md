---
title: Understanding Azure digitale dubbels API-verificatie | Microsoft Docs
description: Gebruik Azure digitale dubbels om verbinding te verifiëren naar API 's
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/02/2018
ms.author: lyrana
ms.openlocfilehash: ef7838c41bb479da273123c2eb3def8e12802390
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49351299"
---
# <a name="connect-and-authenticate-to-apis"></a>Verbinding maken en te verifiëren voor API 's

Azure van digitale dubbels maakt gebruik van Azure Active Directory (Azure AD) om te verifiëren van gebruikers en toepassingen te beschermen. Azure AD biedt ondersteuning voor verificatie voor een verscheidenheid aan moderne architecturen, ze allemaal op basis van de standaardprotocollen OAuth 2.0 of OpenID Connect. Azure AD biedt bovendien ontwikkelaars de mogelijkheid het bouwen van zowel één tenant, line-of-business (LOB)-toepassingen, evenals ontwikkelaars die willen ontwikkelen van toepassingen met meerdere tenants.

Voor een overzicht van Azure AD gaat u naar de [fundamentals pagina](https://docs.microsoft.com/azure/active-directory/fundamentals/index) voor stapsgewijze instructies, concepten en snel aan de slag.

Om een toepassing of service met Azure Active Directory te integreren, moet een ontwikkelaar de toepassing eerst bij Azure Active Directory registreren. Voor gedetailleerde instructies en schermafdrukken de instructies geven [hier](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-add-azure-ad-app)

Dit zijn de [vijf primaire toepassingsscenario](https://docs.microsoft.com/azure/active-directory/develop/v2-app-types) ondersteund door Azure AD:

* Toepassing met één pagina (SPA): een gebruiker moet zich aanmelden bij een toepassing met één pagina die wordt beveiligd door Azure AD.
* Webbrowser voor web-App: een gebruiker moet zich aanmelden bij een webtoepassing die wordt beveiligd door Azure AD.
* Systeemeigen toepassing voor de web-API: een systeemeigen toepassing die wordt uitgevoerd op een telefoon, tablet of PC moet een gebruiker voor resources van een web-API die wordt beveiligd door Azure AD verifiëren.
* Webtoepassing aan web-API: een web-App nodig heeft om op te halen van resources van een web-API is beveiligd door Azure AD.
* Daemon of servertoepassing naar web-API: een daemon-toepassing of een servertoepassing zonder gebruikersinterface web moet de resources ophalen uit een web-API is beveiligd door Azure AD.

De Windows Azure-Verificatiebibliotheek biedt verschillende manieren voor het verkrijgen van Active Directory-tokens. Voor een verdiepen in de bibliotheek, evenals de code voorbeelden Neem eens een kijkje [hier](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki).

## <a name="calling-digital-twins-from-a-middle-tier-web-api"></a>Digitale dubbels aanroepen vanuit een middelste laag web-API

Wanneer digitale dubbels oplossingen ontwikkelaars vaak ontwerpen kiest u een middelste laag-toepassing of de API die vervolgens de digitale dubbels API downstream roept te maken. Gebruikers wordt eerst worden geverifieerd met de middelste laag-toepassing en vervolgens een tokenstroom op-andere gebruikers-of zou worden gebruikt bij het aanroepen van downstream. Ga voor gedetailleerde instructies over het organiseren van de stroom op-andere gebruikers-of naar [deze pagina](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow). U kunt ook voorbeelden van code weergeven [hier](https://azure.microsoft.com/resources/samples/active-directory-dotnet-webapi-onbehalfof/).


## <a name="test-with-the-postman-client"></a>Testen met de Postman-client

Om aan de slag met de digitale Twins-API's kunt u een client, zoals Postman gebruiken als een API-omgeving. Postman kunt u snel maken van complexe HTTP-aanvragen. De onderstaande instructies wordt de nadruk gelegd op het verkrijgen van een Azure AD-token die nodig zijn om aan te roepen digitale dubbels binnen de Postman-gebruikersinterface.


1. Navigeer naar https://www.getpostman.com/ om de app te downloaden
1. Volg de stappen [hier](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad) om een Azure Active Directory-toepassing te maken (of u kunt kiezen om een bestaande registratie opnieuw te gebruiken). 
1. Onder de vereiste machtigingen toevoegen 'Azure digitale dubbels' en selecteer gedelegeerde machtigingen. Vergeet niet om op machtigingen verlenen om te voltooien.
1. Configureren van een antwoord-url met [ https://www.getpostman.com/oauth2/callback ](https://www.getpostman.com/oauth2/callback).
1. Selecteer de **autorisatie tabblad**, klikt u op **OAuth 2.0**, en selecteer **nieuwe Access Token ophalen**.

    |**Veld**  |**Waarde** |
    |---------|---------|
    | Toekenningstype | Impliciet |
    | URL voor terugbellen | [https://www.getpostman.com/oauth2/callback](https://www.getpostman.com/oauth2/callback) |
    | Auth.-URL | [https://login.microsoftonline.com/microsoft.onmicrosoft.com/oauth2/authorize?resource=0b07f429-9f4b-4714-9392-cc5e8e80c8b0](https://login.microsoftonline.com/microsoft.onmicrosoft.com/oauth2/authorize?resource=0b07f429-9f4b-4714-9392-cc5e8e80c8b0)
    | Client-id | De toepassings-Id gebruiken voor de Azure AD-app die is gemaakt of ander doel wordt gebruikt in stap 1 |
    | Bereik | Leeg laten |
    | Status | Leeg laten |
    | Clientverificatie | Als de basisverificatie-header verzenden |

1. Klik op **Token aanvragen**.

    >[!NOTE]
    >Als u ontvangt foutbericht 'OAuth 2 kan niet worden voltooid', probeert u het volgende:
    > * Postman sluit en opent u het opnieuw en probeer het opnieuw.
   
1. Schuif naar beneden en klikt u op **gebruik Token**.

## <a name="next-steps"></a>Volgende stappen

Lees meer over de beveiliging van Azure digitale dubbels [maken en beheren van roltoewijzingen](./security-create-manage-role-assignments.md).
