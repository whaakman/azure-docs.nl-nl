---
title: Azure API Management-sjabloonresources | Microsoft Docs
description: Meer informatie over de typen resources beschikbaar voor gebruik in sjablonen voor ontwikkelaarsportals in Azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 51a1b4c6-a9fd-4524-9e0e-03a9800c3e94
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: c52565403c7fbc80eacbf65428863db07d634030
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57897279"
---
# <a name="azure-api-management-template-resources"></a>Azure API Management-sjabloon-resources
Met Azure API Management biedt de volgende typen resources voor gebruik in de developer portal-sjablonen.  
  
-   [Tekenreeksresources](#strings)  
  
-   [Glyph resources](#glyphs)  

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]
  
##  <a name="strings"></a> Tekenreeksresources  
 API Management biedt een uitgebreide set met de tekenreeksresources voor gebruik in de portal voor ontwikkelaars. Deze resources worden gelokaliseerd in alle van de talen die worden ondersteund door de API Management. De standaardset van sjablonen maakt gebruik van deze resources voor paginakopteksten, labels en de constante tekenreeksen die worden weergegeven in de portal voor ontwikkelaars. Voor het gebruik van een bron van de tekenreeks in de sjablonen, geef het voorvoegsel van de resource-tekenreeks gevolgd door de naam van de tekenreeks, zoals wordt weergegeven in het volgende voorbeeld.  
  
```  
{% localized "Prefix|Name" %}  
  
```  
  
 Het volgende voorbeeld is van de sjabloon van de lijst met Product en wordt weergegeven **producten** aan de bovenkant van de pagina.  
  
```  
<h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>  
  
```  
  
 Raadpleeg de volgende tabellen voor de tekenreeksresources beschikbaar voor gebruik in uw sjablonen voor ontwikkelaarsportals. Naam van de tabel gebruiken als het voorvoegsel voor de tekenreeksresources in die tabel.  
  
-   [ApisStrings](#ApisStrings)  
  
-   [ApplicationListStrings](#ApplicationListStrings)  
  
-   [AppDetailsStrings](#AppDetailsStrings)  
  
-   [AppStrings](#AppStrings)  
  
-   [CommonResources](#CommonResources)  
  
-   [CommonStrings](#CommonStrings)  
  
-   [Documentatie](#Documentation)  
  
-   [ErrorPageStrings](#ErrorPageStrings)  
  
-   [IssuesStrings](#IssuesStrings)  
  
-   [NotFoundStrings](#NotFoundStrings)  
  
-   [ProductDetailsStrings](#ProductDetailsStrings)  
  
-   [ProductsStrings](#ProductsStrings)  
  
-   [ProviderInfoStrings](#ProviderInfoStrings)  
  
-   [SigninResources](#SigninResources)  
  
-   [SigninStrings](#SigninStrings)  
  
-   [SignupStrings](#SignupStrings)  
  
-   [SubscriptionListStrings](#SubscriptionListStrings)  
  
-   [SubscriptionStrings](#SubscriptionStrings)  
  
-   [UpdateProfileStrings](#UpdateProfileStrings)  
  
-   [UserProfile](#UserProfile)  
  
###  <a name="ApisStrings"></a> ApisStrings  
  
|Name|Tekst|  
|----------|----------|  
|PageTitleApis|API's|  
  
###  <a name="AppDetailsStrings"></a> AppDetailsStrings  
  
|Name|Tekst|  
|----------|----------|  
|WebApplicationsDetailsTitle|Voorbeeld van de toepassing|  
|WebApplicationsRequirementsHeader|Vereisten|  
|WebApplicationsScreenshotAlt|Schermopname|  
|WebApplicationsScreenshotsHeader|Schermopnamen|  
  
###  <a name="ApplicationListStrings"></a> ApplicationListStrings  
  
|Name|Tekst|  
|----------|----------|  
|WebDevelopersAppDeleteConfirmation|Weet u zeker dat u de toepassing wilt verwijderen?|  
|WebDevelopersAppNotPublished|Niet gepubliceerd|  
|WebDevelopersAppNotSubmitted|Niet verzonden|  
|WebDevelopersAppTableCategoryHeader|Categorie|  
|WebDevelopersAppTableNameHeader|Name|  
|WebDevelopersAppTableStateHeader|Status|  
|WebDevelopersEditLink|Bewerken|  
|WebDevelopersRegisterAppLink|Toepassing registreren|  
|WebDevelopersRemoveLink|Verwijderen|  
|WebDevelopersSubmitLink|Verzenden|  
|WebDevelopersYourApplicationsHeader|Uw toepassingen|  
  
###  <a name="AppStrings"></a> AppStrings  
  
|Name|Tekst|  
|----------|----------|  
|WebApplicationsHeader|Applicaties|  
  
###  <a name="CommonResources"></a> CommonResources  
  
|Name|Tekst|  
|----------|----------|  
|NoItemsToDisplay|Er zijn geen resultaten gevonden.|  
|GeneralExceptionMessage|Iets is niet goed. Het is mogelijk een tijdelijke storing of een bug. Probeer het opnieuw.|  
|GeneralJsonExceptionMessage|Iets is niet goed. Het is mogelijk een tijdelijke storing of een bug. Laad de pagina in en probeer het opnieuw.|  
|ConfirmationMessageUnsavedChanges|Er zijn enkele niet-opgeslagen wijzigingen. Weet u zeker dat u wilt annuleren en de wijzigingen negeren?|  
|AzureActiveDirectory|Azure Active Directory|  
|HttpLargeRequestMessage|HTTP-aanvraagcorpus te groot.|  
  
###  <a name="CommonStrings"></a> CommonStrings  
  
|Name|Tekst|  
|----------|----------|  
|ButtonLabelCancel|Annuleren|  
|ButtonLabelSave|Opslaan|  
|GeneralExceptionMessage|Iets is niet goed. Het is mogelijk een tijdelijke storing of een bug. Probeer het opnieuw.|  
|NoItemsToDisplay|Er zijn geen items om weer te geven.|  
|PagerButtonLabelFirst|Eerste|  
|PagerButtonLabelLast|Laatste|  
|PagerButtonLabelNext|Volgende|  
|PagerButtonLabelPrevious|Vorige|  
|PagerLabelPageNOfM|Pagina {0} van {1}|  
|PasswordTooShort|Het wachtwoord is te kort|  
|EmailAsPassword|Gebruik uw e-mailadres niet als wachtwoord|  
|PasswordSameAsUserName|Het wachtwoord mag niet uw gebruikersnaam bevatten|  
|PasswordTwoCharacterClasses|Gebruik andere tekenklassen|  
|PasswordTooManyRepetitions|Te veel herhalingen|  
|PasswordSequenceFound|Het wachtwoord bevat reeksen|  
|PagerLabelPageSize|Paginagrootte|  
|CurtainLabelLoading|Laden...|  
|TablePlaceholderNothingToDisplay|Er zijn geen gegevens voor de geselecteerde periode en het geselecteerde bereik|  
|ButtonLabelClose|Sluiten|  
  
###  <a name="Documentation"></a> Documentatie  
  
|Name|Tekst|  
|----------|----------|  
|WebDocumentationInvalidHeaderErrorMessage|Ongeldige header '{0}'|  
|WebDocumentationInvalidRequestErrorMessage|Ongeldige aanvraag-URL|  
|TextboxLabelAccessToken|Access token *|  
|DropdownOptionPrimaryKeyFormat|Primary-{0}|  
|DropdownOptionSecondaryKeyFormat|Secondary-{0}|  
|WebDocumentationSubscriptionKeyText|Uw abonnementssleutel|  
|WebDocumentationTemplatesAddHeaders|Vereiste HTTP-headers toevoegen|  
|WebDocumentationTemplatesBasicAuthSample|Voorbeeld van Basic Authorization|  
|WebDocumentationTemplatesCurlForBasicAuth|gebruik voor Basic Authorization: --user {username}:{password}|  
|WebDocumentationTemplatesCurlValuesForPath|Waarden voor padparameters (weergegeven als {...}), uw abonnementssleutel en waarden voor queryparameters opgeven|  
|WebDocumentationTemplatesDeveloperKey|Uw abonnementssleutel opgeven|  
|WebDocumentationTemplatesJavaApache|In dit voorbeeld wordt de Apache HTTP-client uit HTTP Components (http://hc.apache.org/httpcomponents-client-ga/)|  
|WebDocumentationTemplatesOptionalParams|Waarden voor optionele parameters naar behoefte opgeven|  
|WebDocumentationTemplatesPhpPackage|In dit voorbeeld wordt het HTTP_Request2-pakket. (voor meer informatie: https://pear.php.net/package/HTTP_Request2)|  
|WebDocumentationTemplatesPythonValuesForPath|Waarden voor padparameters opgeven (weergegeven als {...}) en een corpus aanvragen indien nodig|  
|WebDocumentationTemplatesRequestBody|Aanvraagcorpus opgeven|  
|WebDocumentationTemplatesRequiredParams|Waarden opgeven voor de volgende vereiste parameters|  
|WebDocumentationTemplatesValuesForPath|Waarden voor padparameters opgeven (weergegeven als {...})|  
|OAuth2AuthorizationEndpointDescription|Het autorisatie-eindpunt wordt gebruikt voor interactie met de resource-eigenaar en om een autorisatietoekenning te krijgen.|  
|OAuth2AuthorizationEndpointName|Autorisatie-eindpunt|  
|OAuth2TokenEndpointDescription|Het token-eindpunt wordt door de client gebruikt om een toegangstoken te verkrijgen door de autorisatietoekenning of een vernieuwingstoken te overleggen.|  
|OAuth2TokenEndpointName|Tokeneindpunt|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_Description|< p\> de client initieert de stroom door te leiden van de resource-eigenaar gebruikersagent naar het autorisatie-eindpunt.  De client neemt de client-id, aangevraagde bereik, lokale status en een omleidings-URI waarnaar de autorisatieserver de gebruikersagent wordt verzonden terug wanneer toegang wordt verleend (of te weigeren).     < /p\> < p\> de autorisatieserver verifieert de resource-eigenaar (via de gebruikersagent) en wordt tot stand gebracht of de resource-eigenaar verleent of weigert de aanvraag voor toegang van de client.     < /p\> < p\> ervan uitgaande dat de resource-eigenaar toegang verleent, stuurt de de autorisatieserver de gebruikersagent terug naar de client met behulp van de doorstuur-URI opgegeven eerdere (in de aanvraag of tijdens client registrati op).  De doorstuur-URI bevat een autorisatiecode en een lokale status die eerder door de client.     </p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_ErrorDescription|< p\> als de gebruiker de toegang van weigert als de aanvraag ongeldig is, de client ontvangt een melding met de volgende parameters die u aan bij de omleiding toegevoegd: < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_Name|Autorisatie-aanvraag|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_RequestDescription|< p\> de client-app moet de gebruiker doorsturen naar het autorisatie-eindpunt om te starten van de OAuth-proces.          Op het autorisatie-eindpunt voert de gebruiker wordt geverifieerd en verleent of weigert toegang tot de app.     </p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_ResponseDescription|< p\> ervan uitgaande dat de resource-eigenaar toegang verleent, stuurt de autorisatieserver de gebruikersagent terug naar de client met behulp van de doorstuur-URI opgegeven eerdere (in de aanvraag of tijdens de clientregistratie).  De doorstuur-URI bevat een autorisatiecode en een lokale status die eerder door de client. </p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_Description|< p\> de client vraagt een toegangstoken van de autorisatieserver '' s tokeneindpunt door de autorisatiecode ontvangen in de vorige stap.  Wanneer de aanvraag, verifieert de client met de autorisatie-server.  De client neemt de omleiding die URI gebruikt om op te halen van de autorisatiecode voor verificatie. < /p\> < p\> de autorisatieserver verifieert de client, valideert de autorisatiecode en zorgt ervoor dat de doorstuur-URI is ontvangen, overeenkomt met de URI die wordt gebruikt om te leiden van de client in stap (C).  Als geldig is, wordt de autorisatie-server reageert terug met een toegangstoken en eventueel een vernieuwingstoken. </p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_ErrorDescription|< p\> als de verificatie van de aanvraag-client is mislukt of ongeldig is, de autorisatie-server reageert met een statuscode HTTP 400 (onjuiste aanvraag) (tenzij anders opgegeven) en neemt de volgende parameters op in het antwoord. </p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_RequestDescription|< p\> de client een aanvraag doet bij het tokeneindpunt door te sturen met de volgende parameters met behulp van de vereiste indeling is "application/x-www-form-urlencoded goedkeurt" tekencodering UTF-8 in de HTTP-aanvragen entiteitshoofdtekst. </p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_ResponseDescription|< p\> de autorisatieserver verstrekt een toegangstoken en een optioneel vernieuwingstoken en formuleert een antwoord door de volgende parameters toe te voegen aan de entiteitshoofdtekst van het HTTP-antwoord met een 200 (OK)-statuscode. </p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_Description|< p\> de client een verificatie uitvoert met de autorisatieserver en vraagt een toegangstoken van het token-eindpunt. < /p\> < p\> de autorisatieserver verifieert de client en als geldig is, geeft u een toegangstoken. </p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_ErrorDescription|< p\> als de aanvraag is mislukt-clientverificatie of ongeldig is in de autorisatie-server reageert met een statuscode HTTP 400 (onjuiste aanvraag) (tenzij anders opgegeven) en neemt de volgende parameters op in het antwoord. </p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_RequestDescription|< p\> de client een aanvraag doet bij het tokeneindpunt door toe te voegen met de volgende parameters met behulp van de vereiste indeling is "application/x-www-form-urlencoded goedkeurt" tekencodering UTF-8 in de HTTP-aanvragen entiteitshoofdtekst. </p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_ResponseDescription|< p\> als de aanvraag voor een toegangstoken geldig en geautoriseerd, de autorisatieserver verstrekt een toegangstoken en een optioneel vernieuwingstoken en formuleert een antwoord door de volgende parameters toe te voegen aan de entiteitshoofdtekst van het HTTP-antwoord met een 200  Statuscode (OK). </p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_Description|< p\> de client initieert de stroom door te leiden van de resource-eigenaar '' s gebruikersagent naar het autorisatie-eindpunt.  De client neemt de client-id, aangevraagde bereik, lokale status en een omleidings-URI waarnaar de autorisatieserver de gebruikersagent wordt verzonden terug wanneer toegang wordt verleend (of te weigeren). < /p\> < p\> de autorisatieserver verifieert de resource-eigenaar (via de gebruikersagent) en wordt tot stand gebracht of de resource-eigenaar verleent of de client weigert '' s-toegangsaanvraag. < /p\> < p\> ervan uitgaande dat de resource-eigenaar toegang verleent, stuurt de de autorisatieserver de gebruikersagent terug naar de client gebruik van de URI van de eerder opgegeven omleiding.  De doorstuur-URI bevat het toegangstoken in de URI-fragment. </p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_ErrorDescription|< p\> als de resource-eigenaar de toegang weigert of als de aanvraag om redenen dan een ontbrekende of ongeldige doorstuur-URI mislukt, de autorisatieserver de client informeert door de volgende parameters toe te voegen aan het fragment onderdeel o f de doorstuur-URI met de vereiste indeling is ' application/x-www-form-urlencoded goedkeurt'. </p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_RequestDescription|< p\> de client-app moet de gebruiker doorsturen naar het autorisatie-eindpunt om te starten van de OAuth-proces.      Op het autorisatie-eindpunt voert de gebruiker wordt geverifieerd en verleent of weigert toegang tot de app. </p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_ResponseDescription|< p\> als de resource-eigenaar de aanvraag voor toegang tot verleent, de autorisatieserver een toegangstoken en stuurt dit naar de client door de volgende parameters toe te voegen aan de fragmentcomponent van de doorstuur-URI met behulp van de 'application/x-www -formulier - urlencoded'-indeling. </p\>|  
|OAuth2Flow_ObtainAuthorization_AuthorizationCodeGrant_Description|De autorisatiecodestroom is geoptimaliseerd voor clients die de vertrouwelijkheid van hun referenties kunnen onderhouden (bijv. webservertoepassingen die zijn geïmplementeerd met PHP, Java, Python, Ruby, ASP.NET, enz.).|  
|OAuth2Flow_ObtainAuthorization_AuthorizationCodeGrant_Name|Autorisatiecodetoewijzing|  
|OAuth2Flow_ObtainAuthorization_ClientCredentialsGrant_Description|Clientreferentiestroom is geschikt in gevallen waar de client (uw toepassing) toegang tot de beveiligde resources onder het besturingselement aanvraagt. De client wordt beschouwd als een resource-eigenaar, zodat er geen interactie met de eindgebruiker vereist is.|  
|OAuth2Flow_ObtainAuthorization_ClientCredentialsGrant_Name|Toewijzing clientreferenties|  
|OAuth2Flow_ObtainAuthorization_ImplicitGrant_Description|Impliciete stroom is geoptimaliseerd voor clients waarvoor niet de vertrouwelijkheid van hun referenties bekend is bij het uitvoeren van een bepaalde doorstuur-URI. Deze clients worden doorgaans geïmplementeerd in een browser met behulp van een scripttaal, bijvoorbeeld JavaScript.|  
|OAuth2Flow_ObtainAuthorization_ImplicitGrant_Name|Impliciete toewijzing|  
|OAuth2Flow_ObtainAuthorization_ResourceOwnerPasswordCredentialsGrant_Description|Resource is-eigenaar wachtwoord geschikt in gevallen waarin de resource-eigenaar een vertrouwensrelatie tot stand met de client (uw toepassing), zoals het besturingssysteem van het apparaat of een toepassing met bijzondere rechten heeft. Deze stroom is geschikt voor clients die geschikt is voor het verkrijgen van de resource van de eigenaar van referenties (gebruikersnaam en wachtwoord, meestal met behulp van een interactief formulier).|  
|OAuth2Flow_ObtainAuthorization_ResourceOwnerPasswordCredentialsGrant_Name|Toewijzing van wachtwoordreferenties van resource-eigenaar|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_Description|< p\> voorziet de client van de resource-eigenaar met de gebruikersnaam en wachtwoord. < /p\> < p\> de client vraagt een toegangstoken van de autorisatieserver '' s tokeneindpunt door de referenties ontvangen van de resource-eigenaar.  Wanneer de aanvraag, verifieert de client met de autorisatie-server. < /p\> < p\> de autorisatieserver verifieert de client en valideert de referenties van de resource-eigenaar en als geldig is, geeft u een toegangstoken. </p\>|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_ErrorDescription|< p\> als de aanvraag is mislukt-clientverificatie of ongeldig is in de autorisatie-server reageert met een statuscode HTTP 400 (onjuiste aanvraag) (tenzij anders opgegeven) en neemt de volgende parameters op in het antwoord. </p\>|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_RequestDescription|< p\> de client een aanvraag doet bij het tokeneindpunt door toe te voegen met de volgende parameters met behulp van de vereiste indeling is "application/x-www-form-urlencoded goedkeurt" tekencodering UTF-8 in de HTTP-aanvragen entiteitshoofdtekst. </p\>|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_ResponseDescription|< p\> als de aanvraag voor een toegangstoken geldig en geautoriseerd, de autorisatieserver verstrekt een toegangstoken en een optioneel vernieuwingstoken en formuleert een antwoord door de volgende parameters toe te voegen aan de entiteitshoofdtekst van het HTTP-antwoord met een 20 (0 OK)-statuscode. </p\>|  
|OAuth2Step_AccessTokenRequest_Name|Toegangstokenaanvraag|  
|OAuth2Step_AuthorizationRequest_Name|Autorisatie-aanvraag|  
|OAuth2AccessToken_AuthorizationCodeGrant_TokenResponse|VEREIST. Het toegangstoken dat is uitgegeven door de autorisatie-server.|  
|OAuth2AccessToken_ClientCredentialsGrant_TokenResponse|VEREIST. Het toegangstoken dat is uitgegeven door de autorisatie-server.|  
|OAuth2AccessToken_ImplicitGrant_AuthorizationResponse|VEREIST. Het toegangstoken dat is uitgegeven door de autorisatie-server.|  
|OAuth2AccessToken_ResourceOwnerPasswordCredentialsGrant_TokenResponse|VEREIST. Het toegangstoken dat is uitgegeven door de autorisatie-server.|  
|OAuth2ClientId_AuthorizationCodeGrant_AuthorizationRequest|VEREIST. Client-id.|  
|OAuth2ClientId_AuthorizationCodeGrant_TokenRequest|VEREIST als de client geen autorisatie met de autorisatieserver geeft.|  
|OAuth2ClientId_ImplicitGrant_AuthorizationRequest|VEREIST. De client-id.|  
|OAuth2Code_AuthorizationCodeGrant_AuthorizationResponse|VEREIST. De autorisatiecode gegenereerd door de autorisatie-server.|  
|OAuth2Code_AuthorizationCodeGrant_TokenRequest|VEREIST. De autorisatiecode ontvangen van de autorisatie-server.|  
|OAuth2ErrorDescription_AuthorizationCodeGrant_AuthorizationErrorResponse|OPTIONAL. Mensen leesbare ASCII-tekst met aanvullende informatie.|  
|OAuth2ErrorDescription_AuthorizationCodeGrant_TokenErrorResponse|OPTIONAL. Mensen leesbare ASCII-tekst met aanvullende informatie.|  
|OAuth2ErrorDescription_ClientCredentialsGrant_TokenErrorResponse|OPTIONAL. Mensen leesbare ASCII-tekst met aanvullende informatie.|  
|OAuth2ErrorDescription_ImplicitGrant_AuthorizationErrorResponse|OPTIONAL. Mensen leesbare ASCII-tekst met aanvullende informatie.|  
|OAuth2ErrorDescription_ResourceOwnerPasswordCredentialsGrant_TokenErrorResponse|OPTIONAL. Mensen leesbare ASCII-tekst met aanvullende informatie.|  
|OAuth2ErrorUri_AuthorizationCodeGrant_AuthorizationErrorResponse|OPTIONAL. Een URI die verwijst naar een leesbare webpagina met informatie over de fout.|  
|OAuth2ErrorUri_AuthorizationCodeGrant_TokenErrorResponse|OPTIONAL. Een URI die verwijst naar een leesbare webpagina met informatie over de fout.|  
|OAuth2ErrorUri_ClientCredentialsGrant_TokenErrorResponse|OPTIONAL. Een URI die verwijst naar een leesbare webpagina met informatie over de fout.|  
|OAuth2ErrorUri_ImplicitGrant_AuthorizationErrorResponse|OPTIONAL. Een URI die verwijst naar een leesbare webpagina met informatie over de fout.|  
|OAuth2ErrorUri_ResourceOwnerPasswordCredentialsGrant_TokenErrorResponse|OPTIONAL. Een URI die verwijst naar een leesbare webpagina met informatie over de fout.|  
|OAuth2Error_AuthorizationCodeGrant_AuthorizationErrorResponse|VEREIST. Een enkelvoudige ASCII-foutcode uit de volgende: invalid_request, unauthorized_client, access_denied, unsupported_response_type, invalid_scope, server_error, temporarily_unavailable.|  
|OAuth2Error_AuthorizationCodeGrant_TokenErrorResponse|VEREIST. Een enkelvoudige ASCII-foutcode uit de volgende: invalid_request, invalid_client, invalid_grant, unauthorized_client, unsupported_grant_type, invalid_scope.|  
|OAuth2Error_ClientCredentialsGrant_TokenErrorResponse|VEREIST. Een enkelvoudige ASCII-foutcode uit de volgende: invalid_request, invalid_client, invalid_grant, unauthorized_client, unsupported_grant_type, invalid_scope.|  
|OAuth2Error_ImplicitGrant_AuthorizationErrorResponse|VEREIST. Een enkelvoudige ASCII-foutcode uit de volgende: invalid_request, unauthorized_client, access_denied, unsupported_response_type, invalid_scope, server_error, temporarily_unavailable.|  
|OAuth2Error_ResourceOwnerPasswordCredentialsGrant_TokenErrorResponse|VEREIST. Een enkelvoudige ASCII-foutcode uit de volgende: invalid_request, invalid_client, invalid_grant, unauthorized_client, unsupported_grant_type, invalid_scope.|  
|OAuth2ExpiresIn_AuthorizationCodeGrant_TokenResponse|AANBEVOLEN. De levensduur in seconden van het toegangstoken.|  
|OAuth2ExpiresIn_ClientCredentialsGrant_TokenResponse|AANBEVOLEN. De levensduur in seconden van het toegangstoken.|  
|OAuth2ExpiresIn_ImplicitGrant_AuthorizationResponse|AANBEVOLEN. De levensduur in seconden van het toegangstoken.|  
|OAuth2ExpiresIn_ResourceOwnerPasswordCredentialsGrant_TokenResponse|AANBEVOLEN. De levensduur in seconden van het toegangstoken.|  
|OAuth2GrantType_AuthorizationCodeGrant_TokenRequest|VEREIST. Waarde moet worden ingesteld op 'authorization_code'.|  
|OAuth2GrantType_ClientCredentialsGrant_TokenRequest|VEREIST. Waarde moet worden ingesteld op 'client_credentials'.|  
|OAuth2GrantType_ResourceOwnerPasswordCredentialsGrant_TokenRequest|VEREIST. Waarde moet worden ingesteld op 'wachtwoord'.|  
|OAuth2Password_ResourceOwnerPasswordCredentialsGrant_TokenRequest|VEREIST. Het wachtwoord van de resource-eigenaar.|  
|OAuth2RedirectUri_AuthorizationCodeGrant_AuthorizationRequest|OPTIONAL. URI voor de omleiding van het eindpunt moet een absolute URI zijn.|  
|OAuth2RedirectUri_AuthorizationCodeGrant_TokenRequest|VEREIST als de parameter redirect_uri is opgenomen in de autorisatieaanvraag, en hun waarden MOETEN identiek zijn.|  
|OAuth2RedirectUri_ImplicitGrant_AuthorizationRequest|OPTIONAL. URI voor de omleiding van het eindpunt moet een absolute URI zijn.|  
|OAuth2RefreshToken_AuthorizationCodeGrant_TokenResponse|OPTIONAL. Het vernieuwingstoken dat kan worden gebruikt om nieuwe toegangstokens verkrijgen.|  
|OAuth2RefreshToken_ClientCredentialsGrant_TokenResponse|OPTIONAL. Het vernieuwingstoken dat kan worden gebruikt om nieuwe toegangstokens verkrijgen.|  
|OAuth2RefreshToken_ResourceOwnerPasswordCredentialsGrant_TokenResponse|OPTIONAL. Het vernieuwingstoken dat kan worden gebruikt om nieuwe toegangstokens verkrijgen.|  
|OAuth2ResponseType_AuthorizationCodeGrant_AuthorizationRequest|VEREIST. Waarde moet worden ingesteld op "code".|  
|OAuth2ResponseType_ImplicitGrant_AuthorizationRequest|VEREIST. De waarde moet worden ingesteld op 'token'.|  
|OAuth2Scope_AuthorizationCodeGrant_AuthorizationRequest|OPTIONAL. Het bereik van de aanvraag.|  
|OAuth2Scope_AuthorizationCodeGrant_TokenResponse|OPTIONEEL als deze identiek is aan het bereik dat door de client is aangevraagd; anders VEREIST.|  
|OAuth2Scope_ClientCredentialsGrant_TokenRequest|OPTIONAL. Het bereik van de aanvraag.|  
|OAuth2Scope_ClientCredentialsGrant_TokenResponse|OPTIONEEL als deze identiek is aan het bereik dat door de client is aangevraagd; anders VEREIST.|  
|OAuth2Scope_ImplicitGrant_AuthorizationRequest|OPTIONAL. Het bereik van de aanvraag.|  
|OAuth2Scope_ImplicitGrant_AuthorizationResponse|OPTIONEEL als deze identiek is aan het bereik dat door de client is aangevraagd; anders VEREIST.|  
|OAuth2Scope_ResourceOwnerPasswordCredentialsGrant_TokenRequest|OPTIONAL. Het bereik van de aanvraag.|  
|OAuth2Scope_ResourceOwnerPasswordCredentialsGrant_TokenResponse|OPTIONEEL als deze identiek is aan het bereik dat door de client is aangevraagd; anders VEREIST.|  
|OAuth2State_AuthorizationCodeGrant_AuthorizationErrorResponse|VEREIST als de parameter 'state' aanwezig in de autorisatie-aanvraag van de client is.  De exacte waarde die is ontvangen van de client.|  
|OAuth2State_AuthorizationCodeGrant_AuthorizationRequest|AANBEVOLEN. Een onduidelijke waarde die wordt gebruikt door de client om de status tussen de aanvraag en de callback te behouden.  De autorisatieserver bevat deze waarde bij omleiden van de gebruikersagent terug naar de client.  De parameter moet worden gebruikt voor het voorkomen van aanvraag voor cross-site kunnen worden vervalst.|  
|OAuth2State_AuthorizationCodeGrant_AuthorizationResponse|VEREIST als de parameter 'state' aanwezig in de autorisatie-aanvraag van de client is.  De exacte waarde die is ontvangen van de client.|  
|OAuth2State_ImplicitGrant_AuthorizationErrorResponse|VEREIST als de parameter 'state' aanwezig in de autorisatie-aanvraag van de client is.  De exacte waarde die is ontvangen van de client.|  
|OAuth2State_ImplicitGrant_AuthorizationRequest|AANBEVOLEN. Een onduidelijke waarde die wordt gebruikt door de client om de status tussen de aanvraag en de callback te behouden.  De autorisatieserver bevat deze waarde bij omleiden van de gebruikersagent terug naar de client.  De parameter moet worden gebruikt voor het voorkomen van aanvraag voor cross-site kunnen worden vervalst.|  
|OAuth2State_ImplicitGrant_AuthorizationResponse|VEREIST als de parameter 'state' aanwezig in de autorisatie-aanvraag van de client is.  De exacte waarde die is ontvangen van de client.|  
|OAuth2TokenType_AuthorizationCodeGrant_TokenResponse|VEREIST. Het type van het token dat is uitgegeven.|  
|OAuth2TokenType_ClientCredentialsGrant_TokenResponse|VEREIST. Het type van het token dat is uitgegeven.|  
|OAuth2TokenType_ImplicitGrant_AuthorizationResponse|VEREIST. Het type van het token dat is uitgegeven.|  
|OAuth2TokenType_ResourceOwnerPasswordCredentialsGrant_TokenResponse|VEREIST. Het type van het token dat is uitgegeven.|  
|OAuth2UserName_ResourceOwnerPasswordCredentialsGrant_TokenRequest|VEREIST. De gebruikersnaam van de resource-eigenaar.|  
|OAuth2UnsupportedTokenType|Type token '{0}' wordt niet ondersteund.|  
|OAuth2InvalidState|Ongeldig antwoord van de autorisatieserver|  
|OAuth2GrantType_AuthorizationCode|Autorisatiecode|  
|OAuth2GrantType_Implicit|Impliciet|  
|OAuth2GrantType_ClientCredentials|Clientreferenties|  
|OAuth2GrantType_ResourceOwnerPassword|Wachtwoord van resource-eigenaar|  
|WebDocumentation302Code|302 - Gevonden|  
|WebDocumentation400Code|400 (Ongeldige aanvraag)|  
|OAuth2SendingMethod_AuthHeader|Autorisatieheader|  
|OAuth2SendingMethod_QueryParam|Queryparameter|  
|OAuth2AuthorizationServerGeneralException|Er is een fout opgetreden bij de autorisatietoegang via {0}|  
|OAuth2AuthorizationServerCommunicationException|Er kan geen HTTP-verbinding met de autorisatieserver worden gemaakt of deze is onverwacht verbroken.|  
|WebDocumentationOAuth2GeneralErrorMessage|Er is een onverwachte fout opgetreden.|  
|AuthorizationServerCommunicationException|Autorisatie communicatie-uitzondering opgetreden. Neem contact op met de beheerder.|  
|TextblockSubscriptionKeyHeaderDescription|Abonnementssleutel die toegang tot deze API biedt. Gevonden in uw < een href ='/ developer'\>profiel < /a\>.|  
|TextblockOAuthHeaderDescription|OAuth 2.0-toegangstoken verkregen van < ik\>{0}< /i\>. Toekenningstypen voor ondersteund: < ik\>{1}< /i\>.|  
|TextblockContentTypeHeaderDescription|Mediatype van het corpus dat naar de API is verzonden|  
|ErrorMessageApiNotAccessible|De API die u probeert om aan te roepen is niet toegankelijk is op dit moment. Neem contact op met de API-uitgever < een href = "/ issues"\>hier < /a\>.|  
|ErrorMessageApiTimedout|De API die u probeert om aan te roepen duurt langer dan normaal antwoord om terug te gaan. Neem contact op met de API-uitgever < een href = "/ issues"\>hier < /a\>.|  
|BadRequestParameterExpected|"'{0}' parameter wordt verwacht."|  
|TooltipTextDoubleClickToSelectAll|Dubbelklik om alles te selecteren.|  
|TooltipTextHideRevealSecret|Weergeven/Verbergen|  
|ButtonLinkOpenConsole|Probeer het nu|  
|SectionHeadingRequestBody|Aanvraagbody|  
|SectionHeadingRequestParameters|Aanvraagparameters|  
|SectionHeadingRequestUrl|Aanvraag-URL|  
|SectionHeadingResponse|Antwoord|  
|SectionHeadingRequestHeaders|Aanvraagheaders|  
|FormLabelSubtextOptional|optioneel|  
|SectionHeadingCodeSamples|Codevoorbeelden|  
|TextblockOpenidConnectHeaderDescription|OpenID Connect-id-token verkregen van < ik\>{0}< /i\>. Toekenningstypen voor ondersteund: < ik\>{1}< /i\>.|  
  
###  <a name="ErrorPageStrings"></a> ErrorPageStrings  
  
|Name|Tekst|  
|----------|----------|  
|LinkLabelBack|terug|  
|LinkLabelHomePage|startpagina|  
|LinkLabelSendUsEmail|E-mail verzenden|  
|PageTitleError|Er is een probleem met de aangevraagde pagina​|  
|TextblockPotentialCauseIntermittentIssue|Kan dit een onregelmatig voorkomend gegevenstoegangsprobleem dat al is opgelost.|  
|TextblockPotentialCauseOldLink|Mogelijk is de koppeling waarop u hebt geklikt verouderd en verwijst deze niet meer naar de juiste locatie.|  
|TextblockPotentialCauseTechnicalProblem|Er is mogelijk een technisch probleem aan onze kant.|  
|TextblockPotentialSolutionRefresh|Vernieuw de pagina.|  
|TextblockPotentialSolutionStartOver|Opnieuw beginnen vanaf onze {0}.|  
|TextblockPotentialSolutionTryAgain|Ga {0} en probeer de bewerking die u opnieuw uitgevoerd.|  
|TextReportProblem|{0} een beschrijving van wat is een fout opgetreden en kijken we deze als we kunnen.|  
|TitlePotentialCause|Mogelijke oorzaak|  
|TitlePotentialSolution|Mogelijk is dit een tijdelijk probleem; u kunt het volgende proberen|  
  
###  <a name="IssuesStrings"></a> IssuesStrings  
  
|Name|Tekst|  
|----------|----------|  
|WebIssuesIndexTitle|Problemen|  
|WebIssuesNoActiveSubscriptions|U hebt geen actieve abonnementen. U moet zich abonneren op een product om een probleem te melden.|  
|WebIssuesNotSignin|U bent niet aangemeld. Neem {0} op een probleem melden of een opmerking plaatsen.|  
|WebIssuesReportIssueButton|Probleem melden|  
|WebIssuesSignIn|aanmelden|  
|WebIssuesStatusReportedBy|Status: {0} &#124; gemeld door {1}|  
  
###  <a name="NotFoundStrings"></a> NotFoundStrings  
  
|Name|Tekst|  
|----------|----------|  
|LinkLabelHomePage|startpagina|  
|LinkLabelSendUsEmail|e-mail verzenden|  
|PageTitleNotFound|Helaas, dat de pagina die u zoekt niet vinden|  
|TextblockPotentialCauseMisspelledUrl|U mogelijk verkeerd gespeld de URL als u de gegevens hebt ingevoerd in.|  
|TextblockPotentialCauseOldLink|Mogelijk is de koppeling waarop u hebt geklikt verouderd en verwijst deze niet meer naar de juiste locatie.|  
|TextblockPotentialSolutionRetype|Typ de URL opnieuw.|  
|TextblockPotentialSolutionStartOver|Opnieuw beginnen vanaf onze {0}.|  
|TextReportProblem|{0} een beschrijving van wat is een fout opgetreden en kijken we deze als we kunnen.|  
|TitlePotentialCause|Mogelijke oorzaak|  
|TitlePotentialSolution|Mogelijke oplossing|  
  
###  <a name="ProductDetailsStrings"></a> ProductDetailsStrings  
  
|Name|Tekst|  
|----------|----------|  
|WebProductsAgreement|Abonneer u op {0} Product, ik ga akkoord met de `<a data-toggle='modal' href='#legal-terms'\>Terms of Use</a\>`.|  
|WebProductsLegalTermsLink|Gebruiksvoorwaarden|  
|WebProductsSubscribeButton|Abonneer u nu|  
|WebProductsUsageLimitsHeader|Gebruiksbeperkingen|  
|WebProductsYouAreNotSubscribed|U bent geabonneerd op dit product.|  
|WebProductsYouRequestedSubscription|U hebt een abonnement op dit product aangevraagd.|  
|ErrorYouNeedToAgreeWithLegalTerms|U moet akkoord gaan met de gebruiksvoorwaarden voordat u verdergaat.|  
|ButtonLabelAddSubscription|Abonnement toevoegen|  
|LinkLabelChangeSubscriptionName|wijzigen|  
|ButtonLabelConfirm|Bevestigen|  
|TextblockMultipleSubscriptionsCount|U hebt {0} abonnementen op dit product:|  
|TextblockSingleSubscriptionsCount|U hebt {0} abonnementen op dit product:|  
|TextblockSingleApisCount|Dit product bevat {0} API:|  
|TextblockMultipleApisCount|Dit product bevat {0} API's:|  
|TextblockHeaderSubscribe|Abonneren op product|  
|TextblockSubscriptionDescription|Een nieuw abonnement wordt als volgt gemaakt:|  
|TextblockSubscriptionLimitReached|De abonnementslimiet is bereikt.|  
  
###  <a name="ProductsStrings"></a> ProductsStrings  
  
|Name|Tekst|  
|----------|----------|  
|PageTitleProducts|Producten|  
  
###  <a name="ProviderInfoStrings"></a> ProviderInfoStrings  
  
|Name|Tekst|  
|----------|----------|  
|TextboxExternalIdentitiesDisabled|Op het moment is het aanmelden uitgeschakeld door de beheerders.|  
|TextboxExternalIdentitiesSigninInvitation|U kunt zich ook aanmelden met|  
|TextboxExternalIdentitiesSigninInvitationPrimary|Aanmelden met:|  
  
###  <a name="SigninResources"></a> SigninResources  
  
|Name|Tekst|  
|----------|----------|  
|PrincipalNotFound|Kan de principal niet vinden of de handtekening is ongeldig.|  
|ErrorSsoAuthenticationFailed|De SSO-verificatie is mislukt|  
|ErrorSsoAuthenticationFailedDetailed|Ongeldig token opgegeven of kan de handtekening niet verifiëren.|  
|ErrorSsoTokenInvalid|Het SSO-token is ongeldig.|  
|ValidationErrorSpecificEmailAlreadyExists|E-mailbericht '{0}is al geregistreerd|  
|ValidationErrorSpecificEmailInvalid|E-mailbericht '{0}' is ongeldig|  
|ValidationErrorPasswordInvalid|Wachtwoord is ongeldig. Corrigeer de fouten en probeer het opnieuw.|  
|PropertyTooShort|{0} is te kort|  
|WebAuthenticationAddresserEmailInvalidErrorMessage|Ongeldig e-mailadres.|  
|ValidationMessageNewPasswordConfirmationRequired|Nieuw wachtwoord bevestigen|  
|ValidationErrorPasswordConfirmationRequired|Bevestigingswachtwoord is leeg|  
|WebAuthenticationEmailChangeNotice|Wijziging bevestigingse-mail is onderweg naar {0}. Volg de instructies in deze om uw nieuwe e-mailadres te bevestigen. Als het e-mailbericht niet naar uw postvak in in enkele minuten aankomt, controleert u de map Ongewenste e-mail.|  
|WebAuthenticationEmailChangeNoticeHeader|Uw wijzigingsaanvraag voor e-mail is verwerkt|  
|WebAuthenticationEmailChangeNoticeTitle|Wijziging van e-mail aangevraagd|  
|WebAuthenticationEmailHasBeenRevertedNotice|Er bestaat al een e-mailadres. Aanvraag is teruggedraaid|  
|ValidationErrorEmailAlreadyExists|Het e-mailadres bestaat al|  
|ValidationErrorEmailInvalid|Ongeldig e-mailadres|  
|TextboxLabelEmail|Email|  
|ValidationErrorEmailRequired|Het e-mailadres is vereist.|  
|WebAuthenticationErrorNoticeHeader|Fout|  
|WebAuthenticationFieldLengthErrorMessage|{0} moet een maximale lengte van {1}|  
|TextboxLabelEmailFirstName|Voornaam|  
|ValidationErrorFirstNameRequired|De voornaam is vereist.|  
|ValidationErrorFirstNameInvalid|Ongeldige voornaam|  
|NoticeInvalidInvitationToken|Houd er rekening mee dat bevestigingskoppelingen slechts 48 uur geldig zijn. Als u nog steeds binnen deze periode, Controleer of dat de koppeling correct is. Als de koppeling is verlopen, herhaalt u de actie die u probeert om te bevestigen.|  
|NoticeHeaderInvalidInvitationToken|Het uitnodigingstoken is ongeldig.|  
|NoticeTitleInvalidInvitationToken|Bevestigingsfout|  
|WebAuthenticationLastNameInvalidErrorMessage|Ongeldige achternaam|  
|TextboxLabelEmailLastName|Achternaam|  
|ValidationErrorLastNameRequired|De achternaam is vereist.|  
|WebAuthenticationLinkExpiredNotice|Naar u verzonden bevestigingskoppeling is verlopen. `<a href={0}?token={1}>Resend confirmation email.</a\>`|  
|NoticePasswordResetLinkInvalidOrExpired|De herstelkoppeling voor uw wachtwoord is ongeldig of verlopen.|  
|WebAuthenticationLinkExpiredNoticeTitle|Koppeling verzonden|  
|WebAuthenticationNewPasswordLabel|Nieuw wachtwoord|  
|ValidationMessageNewPasswordRequired|Er is een nieuw wachtwoord vereist.|  
|TextboxLabelNotificationsSenderEmail|E-mailadres van afzender meldingen|  
|TextboxLabelOrganizationName|Naam van de organisatie|  
|WebAuthenticationOrganizationRequiredErrorMessage|De organisatienaam is leeg|  
|WebAuthenticationPasswordChangedNotice|Uw wachtwoord is bijgewerkt|  
|WebAuthenticationPasswordChangedNoticeTitle|Wachtwoord bijgewerkt|  
|WebAuthenticationPasswordCompareErrorMessage|Wachtwoorden komen niet overeen|  
|WebAuthenticationPasswordConfirmLabel|Wachtwoord bevestigen|  
|ValidationErrorPasswordInvalidDetailed|Het wachtwoord is te zwak.|  
|WebAuthenticationPasswordLabel|Wachtwoord|  
|ValidationErrorPasswordRequired|Wachtwoord is vereist.|  
|WebAuthenticationPasswordResetSendNotice|E-mail van wijzigen wachtwoord ter bevestiging is onderweg naar {0}. Volg de instructies in het e-mailbericht om door te gaan uw wachtwoordwijziging.|  
|WebAuthenticationPasswordResetSendNoticeHeader|Uw aanvraag voor het opnieuw instellen van het wachtwoord is verwerkt|  
|WebAuthenticationPasswordResetSendNoticeTitle|Nieuwe instelling van wachtwoord is aangevraagd|  
|WebAuthenticationRequestNotFoundNotice|Kan de aanvraag niet vinden|  
|WebAuthenticationSenderEmailRequiredErrorMessage|E-mailadres van afzender van meldingen is leeg|  
|WebAuthenticationSigninPasswordLabel|Bevestig de wijziging door een wachtwoord in te voeren|  
|WebAuthenticationSignupConfirmNotice|Registratie bevestigingse-mail is onderweg naar {0}. < br /\> Volg de instructies in het e-mailbericht te activeren op uw account. < br /\> als het e-mailadres niet in uw postvak in aankomt binnen enkele minuten, Controleer de map Ongewenste e-mail.|  
|WebAuthenticationSignupConfirmNoticeHeader|Uw account is gemaakt|  
|WebAuthenticationSignupConfirmNoticeRepeatHeader|Het e-mailbericht met de registratiebevestiging is opnieuw verzonden|  
|WebAuthenticationSignupConfirmNoticeTitle|Account gemaakt|  
|WebAuthenticationTokenRequiredErrorMessage|Het token is leeg|  
|WebAuthenticationUserAlreadyRegisteredNotice|Het lijkt dat een gebruiker met dit e-mailadres is al geregistreerd in het systeem. Als u uw wachtwoord vergeet, kunt u teruggezet of neem contact op met ons ondersteuningsteam.|  
|WebAuthenticationUserAlreadyRegisteredNoticeHeader|Gebruiker al geregistreerd|  
|WebAuthenticationUserAlreadyRegisteredNoticeTitle|Al geregistreerd|  
|ButtonLabelChangePassword|Wachtwoord wijzigen|  
|ButtonLabelChangeAccountInfo|Accountinformatie wijzigen|  
|ButtonLabelCloseAccount|Account sluiten|  
|WebAuthenticationInvalidCaptchaErrorMessage|Ingevoerde tekst komt niet overeen met de tekst op de afbeelding. Probeer het opnieuw.|  
|ValidationErrorCredentialsInvalid|E-mailadres of wachtwoord is ongeldig. Corrigeer de fouten en probeer het opnieuw.|  
|WebAuthenticationRequestIsNotValid|De aanvraag is ongeldig|  
|WebAuthenticationUserIsNotConfirm|Bevestig uw registratie voordat u zich aanmeldt.|  
|WebAuthenticationInvalidEmailFormated|E-mailadres is ongeldig: {0}|  
|WebAuthenticationUserNotFound|De gebruiker is niet gevonden|  
|WebAuthenticationTenantNotRegistered|Uw account behoort tot een Azure Active Directory-tenant die is niet gemachtigd voor toegang tot deze portal.|  
|WebAuthenticationAuthenticationFailed|De verificatie is mislukt.|  
|WebAuthenticationGooglePlusNotEnabled|De verificatie is mislukt. Als u de toepassing hebt geverifieerd is vervolgens Neem contact op met de beheerder om ervoor te zorgen dat Google de verificatie correct geconfigureerd.|  
|ValidationErrorAllowedTenantIsRequired|Toegestane tenant is verplicht|  
|ValidationErrorTenantIsNotValid|De Azure Active Directory-tenant '{0}' is niet geldig.|  
|WebAuthenticationActiveDirectoryTitle|Azure Active Directory|  
|WebAuthenticationLoginUsingYourProvider|Meld u aan met uw {0} account|  
|WebAuthenticationUserLimitNotice|Deze service heeft het maximum aantal toegestane gebruikers bereikt. Neem `<a href="mailto:{0}"\>contact the administrator</a\>` upgrade hun service en schakelt u de gebruikersregistratie van de.|  
|WebAuthenticationUserLimitNoticeHeader|Gebruikersregistratie uitgeschakeld|  
|WebAuthenticationUserLimitNoticeTitle|Gebruikersregistratie uitgeschakeld|  
|WebAuthenticationUserRegistrationDisabledNotice|De registratie van gebruikers is uitgeschakeld door de beheerder. Meld u aan met externe id-provider.|  
|WebAuthenticationUserRegistrationDisabledNoticeHeader|Gebruikersregistratie uitgeschakeld|  
|WebAuthenticationUserRegistrationDisabledNoticeTitle|Gebruikersregistratie uitgeschakeld|  
|WebAuthenticationSignupPendingConfirmationNotice|We moeten je e-mailadres verifiëren voordat we het maken van uw account kan worden voltooid. Er is een e-mail verzonden {0}. Volg de instructies in het e-mailbericht om uw account te activeren. Als het e-mailbericht niet binnen enkele minuten, Controleer u de map Ongewenste e-mail.|  
|WebAuthenticationSignupPendingConfirmationAccountFoundNotice|Er is een onbevestigd account aangetroffen voor het e-mailadres {0}. We moeten controleren of uw e-mailadres voor het voltooien van het maken van uw account. Er is een e-mail verzonden {0}. Volg de instructies in het e-mailbericht om uw account te activeren. Als het e-mailbericht niet binnen enkele minuten, Controleer of de map Ongewenste e-mail|  
|WebAuthenticationSignupConfirmationAlmostDone|U bent bijna klaar|  
|WebAuthenticationSignupConfirmationEmailSent|Er is een e-mail verzonden {0}. Volg de instructies in het e-mailbericht om uw account te activeren. Als het e-mailbericht niet binnen enkele minuten, Controleer u de map Ongewenste e-mail.|  
|WebAuthenticationEmailSentNotificationMessage|E-mail is verzonden naar {0}|  
|WebAuthenticationNoAadTenantConfigured|Er is geen Azure Active Directory-tenant geconfigureerd voor deze service.|  
|CheckboxLabelUserRegistrationTermsConsentRequired|Ik ga akkoord met de `<a data-toggle="modal" href="#" data-target="#terms"\>Terms of Use</a\>`.|  
|TextblockUserRegistrationTermsProvided|Raadpleeg `<a data-toggle="modal" href="#" data-target="#terms"\>Terms of Use.</a\>`|  
|DialogHeadingTermsOfUse|Gebruiksvoorwaarden|  
|ValidationMessageConsentNotAccepted|U moet akkoord gaan met de gebruiksvoorwaarden voordat u verdergaat.|  
  
###  <a name="SigninStrings"></a> SigninStrings  
  
|Name|Tekst|  
|----------|----------|  
|WebAuthenticationForgotPassword|Wachtwoord vergeten?|  
|WebAuthenticationIfAdministrator|Als u beheerder bent u moet zich aanmelden `<a href="{0}"\>here</a\>`.|  
|WebAuthenticationNotAMember|Nog geen lid? `<a href="/signup"\>Sign up now</a\>`|  
|WebAuthenticationRemember|Mij onthouden op deze computer|  
|WebAuthenticationSigininWithPassword|Aanmelden met uw gebruikersnaam en wachtwoord|  
|WebAuthenticationSigninTitle|Aanmelden|  
|WebAuthenticationSignUpNow|Nu registreren|  
  
###  <a name="SignupStrings"></a> SignupStrings  
  
|Name|Tekst|  
|----------|----------|  
|PageTitleSignup|Aanmelden|  
|WebAuthenticationAlreadyAMember|Bent u al lid?|  
|WebAuthenticationCreateNewAccount|Een nieuw API Management-account maken|  
|WebAuthenticationSigninNow|Nu aanmelden|  
|ButtonLabelSignup|Aanmelden|  
  
###  <a name="SubscriptionListStrings"></a> SubscriptionListStrings  
  
|Name|Tekst|  
|----------|----------|  
|SubscriptionCancelConfirmation|Weet u zeker dat u dit abonnement wilt annuleren?|  
|SubscriptionRenewConfirmation|Weet u zeker dat u dit abonnement wilt verlengen?|  
|WebDevelopersManageSubscriptions|Abonnementen beheren|  
|WebDevelopersPrimaryKey|Primaire sleutel|  
|WebDevelopersRegenerateLink|Opnieuw genereren|  
|WebDevelopersSecondaryKey|Secundaire sleutel|  
|ButtonLabelShowKey|Weergeven|  
|ButtonLabelRenewSubscription|Verlengen|  
|WebDevelopersSubscriptionRequested|Aangevraagd op {0}|  
|WebDevelopersSubscriptionRequestedState|Aangevraagd|  
|WebDevelopersSubscriptionTableNameHeader|Name|  
|WebDevelopersSubscriptionTableStateHeader|Status|  
|WebDevelopersUsageStatisticsLink|Analytics-rapporten|  
|WebDevelopersYourSubscriptions|Uw abonnementen|  
|SubscriptionPropertyLabelRequestedDate|Aangevraagd op|  
|SubscriptionPropertyLabelStartedDate|Gestart op|  
|PageTitleRenameSubscription|Abonnementsnaam wijzigen|  
|SubscriptionPropertyLabelName|Abonnementsnaam|  
  
###  <a name="SubscriptionStrings"></a> SubscriptionStrings  
  
|Name|Tekst|  
|----------|----------|  
|SectionHeadingCloseAccount|Wilt u uw account sluiten?|  
|PageTitleDeveloperProfile|Profiel|  
|ButtonLabelHideKey|Verbergen|  
|ButtonLabelRegenerateKey|Opnieuw genereren|  
|InformationMessageKeyWasRegenerated|Weet u zeker dat u deze sleutel opnieuw wilt genereren?|  
|ButtonLabelShowKey|Weergeven|  
  
###  <a name="UpdateProfileStrings"></a> UpdateProfileStrings  
  
|Name|Tekst|  
|----------|----------|  
|ButtonLabelUpdateProfile|Profiel bijwerken|  
|PageTitleUpdateProfile|Accountgegevens bijwerken|  
  
###  <a name="UserProfile"></a> Gebruikersprofiel  
  
|Name|Tekst|  
|----------|----------|  
|ButtonLabelChangeAccountInfo|Accountinformatie wijzigen|  
|ButtonLabelChangePassword|Wachtwoord wijzigen|  
|ButtonLabelCloseAccount|Account sluiten|  
|TextboxLabelEmail|Email|  
|TextboxLabelEmailFirstName|Voornaam|  
|TextboxLabelEmailLastName|Achternaam|  
|TextboxLabelNotificationsSenderEmail|E-mailadres van afzender meldingen|  
|TextboxLabelOrganizationName|Naam van de organisatie|  
|SubscriptionStateActive|Actief|  
|SubscriptionStateCancelled|Geannuleerd|  
|SubscriptionStateExpired|Vervallen|  
|SubscriptionStateRejected|Afgewezen|  
|SubscriptionStateRequested|Aangevraagd|  
|SubscriptionStateSuspended|Uitgesteld|  
|DefaultSubscriptionNameTemplate|{0}  (standaard)|  
|SubscriptionNameTemplate|Ontwikkelaars toegang tot #{0}|  
|TextboxLabelSubscriptionName|Abonnementsnaam|  
|ValidationMessageSubscriptionNameRequired|De abonnementsnaam mag niet leeg zijn.|  
|ApiManagementUserLimitReached|Deze service heeft het maximum aantal toegestane gebruikers bereikt. Voer een upgrade uit naar een hogere prijscategorie.|  
  
##  <a name="glyphs"></a> Glyph resources  
 Sjablonen voor ontwikkelaarsportals API Management kunnen gebruiken de statusbalk van [Glyphicons van Bootstrap](https://getbootstrap.com/components/#glyphicons). Deze set symbolen bevat meer dan 250 statusbalk in lettertype van de [Glyphicon](https://glyphicons.com/) Halflings instellen. Gebruik de volgende syntaxis voor het gebruik van een glyph uit deze verzameling.  
  
```html  
<span class="glyphicon glyphicon-user">  
```  
  
 Zie voor de volledige lijst van de statusbalk [Glyphicons van Bootstrap](https://getbootstrap.com/components/#glyphicons).

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het werken met sjablonen [over het aanpassen van de API Management-ontwikkelaarsportal met behulp van sjablonen](api-management-developer-portal-templates.md).
