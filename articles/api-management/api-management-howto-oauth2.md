---
title: Ontwikkelaarsaccounts in Azure API Management met behulp van OAuth 2.0 autoriseren | Microsoft Docs
description: Informatie over het autoriseren van gebruikers met behulp van OAuth 2.0 in API Management.
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 78c48247-64f0-4708-b2d0-98b61a821283
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: a19c453bb3271374b587f3d0b35adad55863b490
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-authorize-developer-accounts-using-oauth-20-in-azure-api-management"></a>Het autoriseren van ontwikkelaarsaccounts met behulp van OAuth 2.0 in Azure API Management
Ondersteuning voor veel API's [OAuth 2.0](http://oauth.net/2/) de API beveiligen en ervoor te zorgen dat alleen geldige gebruikers toegang hebben en ze alleen toegang tot resources waarnaar ze bevoegd bent. Om interactieve Azure API Management-Ontwikkelaarsconsole met deze API's gebruiken, kunt de service u voor het configureren van uw service-exemplaar om te werken met uw OAuth 2.0 ingeschakelde API.

## <a name="prerequisites"></a>Vereisten
Deze handleiding leest u hoe uw API Management-service-exemplaar voor het gebruik van OAuth 2.0-autorisatie voor ontwikkelaarsaccounts configureren, maar wordt niet beschreven hoe u een OAuth 2.0-provider configureren. De configuratie voor elke OAuth 2.0-provider is verschillend zijn, hoewel de stappen vergelijkbaar zijn, en de vereiste stukjes informatie die wordt gebruikt bij het configureren van OAuth 2.0 in uw API Management-service-exemplaar hetzelfde zijn. Dit onderwerp bevat voorbeelden met Azure Active Directory als een OAuth 2.0-provider.

> [!NOTE]
> Zie voor meer informatie over het configureren van OAuth 2.0 met Azure Active Directory de [WebApp-GraphAPI-DotNet] [ WebApp-GraphAPI-DotNet] voorbeeld.
> 
> 

## <a name="step1"></a>Een OAuth 2.0-autorisatie-server configureren in API Management
Als u aan de slag wilt gaan, klikt u op **Publicatieportal** in Azure Portal voor uw API Management-service.

![Publicatieportal][api-management-management-console]

> [!NOTE]
> Als u nog geen service-exemplaar van API Management hebt gemaakt, raadpleegt u [Service-exemplaar van API Management maken][Create an API Management service instance] in de zelfstudie [Aan de slag met Azure API Management][Get started with Azure API Management].
> 
> 

Klik op **beveiliging** van de **API Management** menu aan de linkerkant, klik op **OAuth 2.0**, en klik vervolgens op **toevoegen autorisatie server**.

![OAuth 2.0][api-management-oauth2]

Wanneer u op **toevoegen autorisatie server**, de nieuwe vorm van de autorisatie-server wordt weergegeven.

![Nieuwe server][api-management-oauth2-server-1]

Voer een naam en een optionele beschrijving in de **naam** en **beschrijving** velden. 

> [!NOTE]
> Deze velden worden gebruikt voor het identificeren van de OAuth 2.0-autorisatie-server binnen de huidige service-exemplaar van API Management en hun waarden niet afkomstig zijn van de OAuth 2.0-server.
> 
> 

Voer de **Client de registratie van URL**. Deze pagina is waar gebruikers kunnen maken en beheren van hun account, varieert, afhankelijk van de OAuth 2.0-provider gebruikt. De **Client de registratie van URL** verwijst naar de pagina die gebruikers gebruiken kunnen voor het maken en hun eigen accounts configureren voor OAuth 2.0-providers die ondersteuning bieden voor gebruikersbeheer van accounts. Sommige organisaties niet configureert of deze functionaliteit te gebruiken, zelfs als de provider OAuth 2.0 wordt ondersteund. Als uw OAuth 2.0-provider geen Gebruikersbeheer van accounts die zijn geconfigureerd heeft, voert u een tijdelijke aanduiding voor URL hier, zoals de URL van uw bedrijf of een URL, zoals `https://placeholder.contoso.com`.

De volgende sectie van het formulier bevat de **autorisatiecode verlenen typen**, **autorisatie eindpunt-URL**, en **aanvraag autorisatiemethode** instellingen.

![Nieuwe server][api-management-oauth2-server-2]

Geef de **autorisatiecode verlenen typen** door het controleren van de gewenste typen. **Autorisatiecode** standaard is opgegeven.

Voer de **autorisatie eindpunt-URL**. Voor Azure Active Directory, deze URL zijn vergelijkbaar met de volgende URL waar `<client_id>` is vervangen door de client-id die uw toepassing met het OAuth 2.0-server te identificeren.

`https://login.microsoftonline.com/<client_id>/oauth2/authorize`

De **aanvraag autorisatiemethode** geeft aan hoe de autorisatieaanvraag is verzonden naar de OAuth 2.0-server. Standaard **ophalen** is geselecteerd.

De volgende sectie waar is de **Token eindpunt-URL**, **Client verificatiemethoden**, **toegangstoken verzenden methode**, en **standaardbereik** zijn opgegeven.

![Nieuwe server][api-management-oauth2-server-3]

Voor een Azure Active Directory OAuth 2.0-server de **Token eindpunt-URL** hebben de volgende indeling waar `<APPID>` heeft de indeling van `yourapp.onmicrosoft.com`.

`https://login.microsoftonline.com/<APPID>/oauth2/token`

De standaardinstelling voor **Client verificatiemethoden** is **Basic**, en **toegangstoken verzenden methode** is **autorisatie-header**. Deze waarden zijn geconfigureerd op deze sectie van het formulier, samen met de **standaardbereik**.

De **clientreferenties** sectie bevat de **Client-ID** en **clientgeheim**, die zijn verkregen tijdens het maken en de configuratie van uw OAuth 2.0-server. Eenmaal de **Client-ID** en **clientgeheim** zijn opgegeven, de **redirect_uri** voor de **autorisatiecode** wordt gegenereerd. Deze URI wordt gebruikt voor het configureren van de antwoord-URL in de configuratie van uw OAuth 2.0-server.

![Nieuwe server][api-management-oauth2-server-4]

Als **autorisatiecode verlenen typen** is ingesteld op **Resource eigenaarswachtwoord**, wordt de **wachtwoordreferenties Resource-eigenaar** sectie wordt gebruikt om op te geven die referenties; anders kunt u het leeg laten.

![Nieuwe server][api-management-oauth2-server-5]

Wanneer het formulier voltooid is, klikt u op **opslaan** om op te slaan van de configuratie van de API Management OAuth 2.0-autorisatie-server. Zodra de serverconfiguratie is opgeslagen, kunt u API's voor het gebruik van deze configuratie kunt configureren, zoals wordt weergegeven in de volgende sectie.

## <a name="step2"></a>Een API voor het gebruik van OAuth 2.0-gebruikersautorisatie configureren
Klik op **API's** van de **API Management** menu aan de linkerkant, klik op de naam van de gewenste API, klik op **beveiliging**, en vervolgens het selectievakje in voor **OAuth 2.0**.

![Gebruikersautorisatie][api-management-user-authorization]

Selecteer de gewenste **autorisatie server** in de vervolgkeuzelijst en klik op **opslaan**.

![Gebruikersautorisatie][api-management-user-authorization-save]

## <a name="step3"></a>Het OAuth 2.0-gebruikersautorisatie testen in de Portal voor ontwikkelaars
Zodra u hebt uw OAuth 2.0-autorisatie-server is geconfigureerd en uw API voor het gebruik van die server geconfigureerd, kunt u het testen door te gaan naar de Ontwikkelaarsportal en een API aanroepen.  Klik op **ontwikkelaarsportal** in het menu rechtsboven.

![ontwikkelaarsportal][api-management-developer-portal-menu]

Klik op **API's** in het bovenste menu en selecteer **Echo-API**.

![Echo-API][api-management-apis-echo-api]

> [!NOTE]
> Als er slechts één API is geconfigureerd of zichtbaar is voor uw account, gaat u wanneer u op API's klikt rechtstreeks naar de bewerkingen voor die API.
> 
> 

Selecteer de **GET Resource** bewerking, klikt u op **Console openen**, en selecteer vervolgens **autorisatiecode** in de vervolgkeuzelijst.

![Console openen][api-management-open-console]

Wanneer **autorisatiecode** is geselecteerd, wordt een pop-upvenster weergegeven met de vorm aanmelden van de OAuth 2.0-provider. In dit voorbeeld wordt het formulier aanmelden verstrekt door Azure Active Directory.

> [!NOTE]
> Als u pop-ups uitgeschakeld hebt wordt u gevraagd deze inschakelen door de browser. Nadat u deze wilt inschakelen, selecteert u **autorisatiecode** opnieuw en het formulier worden weergegeven.
> 
> 

![Aanmelden][api-management-oauth2-signin]

Wanneer u bent aangemeld, de **aanvraagheaders** zijn gevuld met een `Authorization : Bearer` de aanvraag machtigt-header.

![Token voor verbindingsverzoeken-header][api-management-request-header-token]

Op dit moment kunt u configureren van de gewenste waarden voor de resterende parameters en dien de aanvraag. 

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het gebruik van OAuth 2.0 en API Management, de volgende video en bijbehorende [artikel](api-management-howto-protect-backend-with-aad.md).

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Protecting-Web-API-Backend-with-Azure-Active-Directory-and-API-Management/player]
> 
> 

[api-management-management-console]: ./media/api-management-howto-oauth2/api-management-management-console.png
[api-management-oauth2]: ./media/api-management-howto-oauth2/api-management-oauth2.png
[api-management-user-authorization]: ./media/api-management-howto-oauth2/api-management-user-authorization.png
[api-management-user-authorization-save]: ./media/api-management-howto-oauth2/api-management-user-authorization-save.png
[api-management-oauth2-signin]: ./media/api-management-howto-oauth2/api-management-oauth2-signin.png
[api-management-request-header-token]: ./media/api-management-howto-oauth2/api-management-request-header-token.png
[api-management-developer-portal-menu]: ./media/api-management-howto-oauth2/api-management-developer-portal-menu.png
[api-management-open-console]: ./media/api-management-howto-oauth2/api-management-open-console.png
[api-management-oauth2-server-1]: ./media/api-management-howto-oauth2/api-management-oauth2-server-1.png
[api-management-oauth2-server-2]: ./media/api-management-howto-oauth2/api-management-oauth2-server-2.png
[api-management-oauth2-server-3]: ./media/api-management-howto-oauth2/api-management-oauth2-server-3.png
[api-management-oauth2-server-4]: ./media/api-management-howto-oauth2/api-management-oauth2-server-4.png
[api-management-oauth2-server-5]: ./media/api-management-howto-oauth2/api-management-oauth2-server-5.png
[api-management-apis-echo-api]: ./media/api-management-howto-oauth2/api-management-apis-echo-api.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

