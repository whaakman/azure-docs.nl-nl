---
title: Ontwikkelaarsaccounts authoriseren met OAuth 2.0 in Azure API Management | Microsoft Docs
description: Informatie over het autoriseren van gebruikers met behulp van OAuth 2.0 in API Management.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2018
ms.author: apimpm
ms.openlocfilehash: b195271edeea6cd5ea527454ad1615ac85a32138
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55746724"
---
# <a name="how-to-authorize-developer-accounts-using-oauth-20-in-azure-api-management"></a>Hoe ontwikkelaarsaccounts authoriseren met OAuth 2.0 in Azure API Management

Veel API's ondersteunen [OAuth 2.0](https://oauth.net/2/) voor het beveiligen van de API en zorg ervoor dat alleen geldige gebruikers toegang hebben, en ze alleen toegang tot bronnen die ze hebben bent recht. Om te kunnen gebruiken Azure API Management interactieve Developer-Console met deze API's, kunt de service u uw service-exemplaar om te werken met het OAuth 2.0 ingeschakelde API configureren.

## <a name="prerequisites"> </a>Vereisten

Deze handleiding laat zien hoe het configureren van uw API Management service-exemplaar voor het gebruik van OAuth 2.0-autorisatie voor accounts voor ontwikkelaars u, maar biedt niet laten zien hoe u een OAuth 2.0-provider configureren. De configuratie voor elke OAuth 2.0-provider is verschillend zijn, hoewel de stappen vergelijkbaar zijn, en de vereiste stukjes informatie die wordt gebruikt bij het configureren van OAuth 2.0 in uw API Management service-exemplaar hetzelfde zijn. Dit onderwerp bevat voorbeelden van het gebruik van Azure Active Directory als een OAuth 2.0-provider.

> [!NOTE]
> Zie voor meer informatie over het configureren van OAuth 2.0 met behulp van Azure Active Directory, de [WebApp-GraphAPI-DotNet] [ WebApp-GraphAPI-DotNet] voorbeeld.

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="step1"> </a>Configureren van een OAuth 2.0-autorisatie-server in API Management

> [!NOTE]
> Als u een exemplaar van API Management-service nog niet hebt gemaakt, raadpleegt u [API Management service-exemplaar maken][Create an API Management service instance].

1. Klik op het tabblad OAuth 2.0 in het menu aan de linkerkant en klik op **+ toevoegen**.

    ![OAuth 2.0-menu](./media/api-management-howto-oauth2/oauth-01.png)

2. Voer een naam en een optionele beschrijving in de **naam** en **beschrijving** velden.

    > [!NOTE]
    > Deze velden worden gebruikt voor identificatie van de OAuth 2.0-autorisatie-server in de huidige service-exemplaar van API Management en hun waarden niet afkomstig zijn van de OAuth 2.0-server.

3. Voer de **URL voor de clientregistratiepgina**. Deze pagina is waar gebruikers kunnen maken en hun accounts beheren en is afhankelijk van de OAuth 2.0-provider gebruikt. De **URL voor de clientregistratiepgina** verwijst naar de pagina waarop gebruikers gebruiken kunnen om te maken en configureren van hun eigen accounts voor OAuth 2.0-providers die ondersteuning bieden voor gebruikersbeheer van accounts. Sommige organisaties niet configureert of deze functionaliteit gebruiken, zelfs als de OAuth 2.0-provider wordt ondersteund. Als uw OAuth 2.0-provider heeft geen Gebruikersbeheer van accounts die zijn geconfigureerd, voert u een tijdelijke aanduiding hier de URL, zoals de URL van uw bedrijf of een URL, zoals `https://placeholder.contoso.com`.

    ![Nieuwe OAuth 2.0-server](./media/api-management-howto-oauth2/oauth-02.png)

4. De volgende sectie van het formulier bevat de **toekenningstypen**, **autorisatie-eindpunt-URL**, en **aanvraagmethode voor autorisatie** instellingen.

    Geef de **toekenningstypen** door het controleren van de gewenste typen. **Autorisatiecode** standaard is opgegeven.

    Voer de **autorisatie-eindpunt-URL**. Voor Azure Active Directory, wordt deze URL zijn die vergelijkbaar is met de volgende URL, waarbij `<client_id>` wordt vervangen door de client-id die uw toepassing met de OAuth 2.0-server identificeert.

    `https://login.microsoftonline.com/<client_id>/oauth2/authorize`

    De **aanvraagmethode voor autorisatie** geeft aan hoe de autorisatieaanvraag wordt verzonden naar de OAuth 2.0-server. Standaard **ophalen** is geselecteerd.

5. Vervolgens **Token-eindpunt-URL**, **methoden voor clientverificatie**, **toegangstoken verzendmethode** en **standaardbereik** moeten worden opgegeven.

    ![Nieuwe OAuth 2.0-server](./media/api-management-howto-oauth2/oauth-03.png)

    Voor een Azure Active Directory OAuth 2.0-server de **Token-eindpunt-URL** heeft de volgende indeling hebben: waar `<TenantID>` heeft de indeling van `yourapp.onmicrosoft.com`.

    `https://login.microsoftonline.com/<TenantID>/oauth2/token`

    De standaardinstelling voor **methoden voor clientverificatie** is **Basic**, en **toegangstoken verzendmethode** is **autorisatie-header**. Deze waarden worden geconfigureerd op deze sectie van het formulier, samen met de **standaardbereik**.

6. De **clientreferenties** sectie bevat de **Client-ID** en **clientgeheim**, die zijn verkregen tijdens het proces voor het maken en de configuratie van de OAuth 2.0-server . Zodra de **Client-ID** en **clientgeheim** zijn opgegeven, de **redirect_uri** voor de **autorisatiecode** wordt gegenereerd. Deze URI wordt gebruikt voor het configureren van de antwoord-URL in de configuratie van uw OAuth 2.0-server.

    ![Nieuwe OAuth 2.0-server](./media/api-management-howto-oauth2/oauth-04.png)

    Als **toekenningstypen** is ingesteld op **wachtwoord van Resource-eigenaar**, wordt de **wachtwoordreferenties van Resource-eigenaar** sectie is gebruikt om op te geven deze referenties, anders u kunt deze leeg laten.

    Nadat het formulier voltooid is, klikt u op **maken** om op te slaan van de configuratie van de API Management OAuth 2.0-autorisatie-server. Wanneer de serverconfiguratie is opgeslagen, kunt u API's voor het gebruik van deze configuratie kunt configureren, zoals wordt weergegeven in de volgende sectie.

## <a name="step2"> </a>Een API voor het gebruik van gebruikersautorisatie OAuth 2.0 configureren

1. Klik op **API's** uit de **API Management** menu aan de linkerkant.

    ![OAuth 2.0-API 's](./media/api-management-howto-oauth2/oauth-05.png)

2. Klik op de naam van de API en klik op de gewenste **instellingen**. Schuif naar de **Security** uit en schakel het selectievakje voor **OAuth 2.0**.

    ![OAuth 2.0-instellingen](./media/api-management-howto-oauth2/oauth-06.png)

3. Selecteer de gewenste **autorisatieserver** uit de vervolgkeuzelijst en klik op **opslaan**.

    ![OAuth 2.0-instellingen](./media/api-management-howto-oauth2/oauth-07.png)

## <a name="step3"> </a>De gebruikersautorisatie OAuth 2.0 testen in de Portal voor ontwikkelaars

Nadat u hebt uw OAuth 2.0-autorisatie-server geconfigureerd en uw API voor het gebruik van die server geconfigureerd, kunt u deze testen door te gaan naar de Portal voor ontwikkelaars en een API wordt aangeroepen.  Klik op **ontwikkelaarsportal** in het bovenste menu van uw Azure API Management-instantie **overzicht** pagina.

![ontwikkelaarsportal][api-management-developer-portal-menu]

Klik op **API's** in het bovenste menu en selecteer **Echo-API**.

![Echo-API][api-management-apis-echo-api]

> [!NOTE]
> Als er slechts één API is geconfigureerd of zichtbaar is voor uw account, gaat u wanneer u op API's klikt rechtstreeks naar de bewerkingen voor die API.

Selecteer de **GET Resource** bewerking, klikt u op **Console openen**, en selecteer vervolgens **autorisatiecode** in de vervolgkeuzelijst.

![Console openen][api-management-open-console]

Wanneer **autorisatiecode** is geselecteerd, wordt een pop-upvenster weergegeven met het aanmeldingsformulier van de OAuth 2.0-provider. In dit voorbeeld wordt het aanmeldingsformulier verstrekt door Azure Active Directory.

> [!NOTE]
> Hebt u pop-ups uitgeschakeld wordt u gevraagd deze inschakelen door de browser. Nadat u deze wilt inschakelen, selecteert u **autorisatiecode** opnieuw en wordt het aanmeldingsformulier weergegeven.

![Aanmelden][api-management-oauth2-signin]

Nadat u bent aangemeld, de **aanvraagheaders** worden ingevuld met een `Authorization : Bearer` -header die de aanvraag machtigt.

![Token voor verbindingsverzoeken koptekst][api-management-request-header-token]

U kunt op dit moment Configureer de gewenste waarden voor de resterende parameters en dien de aanvraag.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van OAuth 2.0 en API Management, de volgende video- en bijbehorende [artikel](api-management-howto-protect-backend-with-aad.md).

[api-management-oauth2-signin]: ./media/api-management-howto-oauth2/api-management-oauth2-signin.png
[api-management-request-header-token]: ./media/api-management-howto-oauth2/api-management-request-header-token.png
[api-management-developer-portal-menu]: ./media/api-management-howto-oauth2/api-management-developer-portal-menu.png
[api-management-open-console]: ./media/api-management-howto-oauth2/api-management-open-console.png
[api-management-apis-echo-api]: ./media/api-management-howto-oauth2/api-management-apis-echo-api.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: get-started-create-service-instance.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: get-started-create-service-instance.md

[https://oauth.net/2/]: https://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

