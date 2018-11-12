---
title: Informatie over Azure digitale dubbels API-verificatie | Microsoft Docs
description: Gebruikt Azure digitale dubbels om verbinding te verifiëren naar API 's
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/02/2018
ms.author: lyrana
ms.openlocfilehash: f85ab05e785ea559962490b43e75b196d1602159
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51016213"
---
# <a name="connect-and-authenticate-to-apis"></a>Verbinding maken en te verifiëren voor API 's

Azure van digitale dubbels maakt gebruik van Azure Active Directory (Azure AD) om te verifiëren van gebruikers en toepassingen te beschermen. Azure AD ondersteunt verificatie voor een verscheidenheid aan moderne architecturen. Ze allemaal zijn gebaseerd op de standaardprotocollen OAuth 2.0 of OpenID Connect. Ontwikkelaars kunnen bovendien Azure AD gebruiken om één tenant en line-of-business (LOB)-toepassingen te bouwen. Ontwikkelaars kunnen ook Azure AD gebruiken voor het ontwikkelen van toepassingen voor meerdere tenants.

Voor een overzicht van Azure AD, gaat u naar de [fundamentals pagina](https://docs.microsoft.com/azure/active-directory/fundamentals/index) voor stapsgewijze instructies, concepten en snelstartgidsen.

Om een toepassing of service met Azure Active Directory te integreren, moet een ontwikkelaar de toepassing eerst bij Azure Active Directory registreren. Zie voor gedetailleerde instructies en schermafbeeldingen [in deze Quick Start](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-add-azure-ad-app).

[Toepassingsscenario's vijf primaire](https://docs.microsoft.com/azure/active-directory/develop/v2-app-types) worden ondersteund door Azure AD:

* Toepassing met één pagina (SPA): een gebruiker moet zich aanmelden bij een toepassing met één pagina die wordt beveiligd door Azure AD.
* Webbrowser voor web-App: een gebruiker moet zich aanmelden bij een webtoepassing die wordt beveiligd door Azure AD.
* Systeemeigen toepassing voor de web-API: een systeemeigen toepassing die wordt uitgevoerd op een telefoon, tablet of PC moet een gebruiker voor resources van een web-API die wordt beveiligd door Azure AD verifiëren.
* Webtoepassing aan web-API: een web-App nodig heeft om op te halen van resources van een web-API is beveiligd door Azure AD.
* Daemon of servertoepassing naar web-API: een daemon-toepassing of een servertoepassing met niet-Webinterface moet de resources ophalen uit een web-API is beveiligd door Azure AD.

De Windows Azure-Verificatiebibliotheek biedt verschillende manieren voor het verkrijgen van Active Directory-tokens. Zie voor meer informatie over de bibliotheek en codevoorbeelden, [in dit artikel](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki).

## <a name="call-digital-twins-from-a-middle-tier-web-api"></a>Digitale dubbels aanroepen vanuit een middelste laag web-API

Wanneer ontwikkelaars ontwerpen van oplossingen voor digitale Twins, maken ze doorgaans een middelste laag-toepassing of de API. De app of API vervolgens roept de digitale dubbels API downstream. Gebruikers eerst worden geverifieerd met de middelste laag-toepassing en klikt u vervolgens een tokenstroom op-andere gebruikers-of wordt gebruikt om aan te roepen downstream. Zie voor instructies over het organiseren van de stroom op-andere gebruikers-of [deze pagina](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow). U kunt voorbeelden van code ook bekijken op [deze pagina](https://azure.microsoft.com/resources/samples/active-directory-dotnet-webapi-onbehalfof/).


## <a name="test-with-the-postman-client"></a>Testen met de Postman-client

Als u wilt aan de slag met de digitale Twins-API's, kunt u een client, zoals Postman gebruiken als een API-omgeving. Postman kunt u snel maken van complexe HTTP-aanvragen. In de volgende stappen laten zien hoe u een Azure AD-verificatietoken die nodig is om aan te roepen digitale dubbels binnen de Postman-gebruikersinterface.


1. Ga naar https://www.getpostman.com/ om de app te downloaden.
1. Volg de stappen in [in deze snelstartgids](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad) te maken van een Azure AD-toepassing. Of u een bestaande registratie opnieuw kunt gebruiken. 
1. Onder **vereiste machtigingen**, voer 'Azure digitale dubbels' en selecteer **gedelegeerde machtigingen**. Selecteer vervolgens **machtigingen verlenen**.
1. Open het toepassingsmanifest, en stel **oauth2AllowImplicitFlow** op ' True '.
1. Configureren van een antwoord-URL met [ https://www.getpostman.com/oauth2/callback ](https://www.getpostman.com/oauth2/callback).
1. Selecteer de **autorisatie** tabblad **OAuth 2.0**, en selecteer vervolgens **nieuwe Access Token ophalen**.

    |**Veld**  |**Waarde** |
    |---------|---------|
    | Toekenningstype | Impliciet |
    | URL voor terugbellen | [https://www.getpostman.com/oauth2/callback](https://www.getpostman.com/oauth2/callback) |
    | Auth.-URL | https://login.microsoftonline.com/<Your Azure AD Tenant e.g. Contoso>.onmicrosoft.com/oauth2/Authorize?resource=0b07f429-9f4b-4714-9392-cc5e8e80c8b0 |
    | Client-id | Gebruik de toepassings-ID voor de Azure AD-app die is gemaakt of ander doel wordt gebruikt in stap 2. |
    | Bereik | Leeg laten. |
    | Status | Leeg laten. |
    | Clientverificatie | Verzenden als een header basisverificatie. |

1. Selecteer **Token aanvragen**.

    >[!NOTE]
    >Als u ontvangt het foutbericht 'OAuth 2 kan niet worden voltooid', probeert u het volgende:
    > * Postman, sluit en opent u het opnieuw en probeer het opnieuw.
   
1. Schuif omlaag en selecteer **gebruik Token**.

## <a name="next-steps"></a>Volgende stappen

Lees meer over de beveiliging van Azure digitale dubbels [maken en beheren van roltoewijzingen](./security-create-manage-role-assignments.md).
