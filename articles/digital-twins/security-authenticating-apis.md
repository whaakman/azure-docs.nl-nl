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
ms.openlocfilehash: 1d5b1869428cec6bf80b8518485f685e38ad5997
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324086"
---
# <a name="connect-and-authenticate-to-apis"></a>Verbinding maken en te verifiëren voor API 's

Azure van digitale dubbels maakt gebruik van Azure Active Directory (Azure AD) om te verifiëren van gebruikers en toepassingen te beschermen.

Als u niet bekend met Azure AD bent, meer informatie is beschikbaar in de [Ontwikkelaarshandleiding voor](https://docs.microsoft.com/azure/active-directory/develop/azure-ad-developers-guide). De Windows Azure-Verificatiebibliotheek biedt verschillende manieren voor het verkrijgen van Active Directory-tokens. Raadpleeg voor een verdiepen in de bibliotheek, [hier](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki).

In dit artikel biedt een overzicht van de twee scenario's: een productiescenario met een middelste laag API en verificatie in de clienttoepassing Postman voor het snel opstarten en testen.

## <a name="authentication-in-production"></a>Verificatie in productie

Oplossingen voor ontwikkelaars zijn twee manieren om te verbinden met digitale dubbels.  Oplossingen voor ontwikkelaars kunnen verbinding maken met Azure digitale Twins in de volgende manieren:

* Ze kunnen een clienttoepassing of een middelste laag-API maken. Client-apps vereisen dat gebruikers om te verifiëren en gebruik vervolgens de [OAuth 2.0 namens](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow) security stroom een downstream-API aan te roepen.
* Maak of maak gebruik van een bestaande Azure AD-toepassing. Raadpleeg de documentatie [hier](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad).
    1. Geef de **aanmeldings- en omleidings-URI's** (indien nodig).
    1. In de toepassing het manifest set `oauth2AllowImplicitFlow` op ' True '.
    1. In **vereiste machtigingen**, digitale dubbels toevoegen door te zoeken naar "Azure Digital dubbels." Selecteer **overgedragen machtigingen voor lezen/schrijven toegang** en klikt u op de **machtigingen verlenen** knop.

Ga voor gedetailleerde instructies over het organiseren van de stroom op-andere gebruikers-of naar [deze pagina](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow). U kunt ook voorbeelden van code weergeven [hier](https://azure.microsoft.com/resources/samples/active-directory-dotnet-webapi-onbehalfof/).

## <a name="test-with-the-postman-client"></a>Testen met de Postman-client

1. Volg de eerste stappen hierboven om een Azure Active Directory-toepassing maken (of wijzigen). Vervolgens stelt u `oauth2AllowImplicitFlow` aan waar in de app-manifest en machtigingen verlenen voor "Azure Digital dubbels."
1. Een antwoord-url ingesteld op [ https://www.getpostman.com/oauth2/callback ](https://www.getpostman.com/oauth2/callback).
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

    >[!TIP]
    >Als u ontvangt foutbericht 'OAuth 2 kan niet worden voltooid', probeert u een van de volgende:
    > * Postman sluit en opent u het opnieuw en probeer het opnieuw.
    > * De geheime sleutel in uw app verwijderen, opnieuw een nieuwe en huurder de waarde in het bovenstaande formulier.

1. Schuif naar beneden en klikt u op **gebruik Token**.

## <a name="next-steps"></a>Volgende stappen

Lees meer over de beveiliging van Azure digitale dubbels [maken en beheren van roltoewijzingen](./security-create-manage-role-assignments.md).
