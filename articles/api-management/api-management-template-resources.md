---
title: Azure API Management-sjabloonresources | Microsoft Docs
description: Meer informatie over de typen resources beschikbaar voor gebruik in developer portal sjablonen in Azure API Management.
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 51a1b4c6-a9fd-4524-9e0e-03a9800c3e94
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 212e7ea7bb2ffea63c7ba210195df0da38aa8f0a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-api-management-template-resources"></a>Azure API Management-sjabloonresources
Azure API Management biedt de volgende typen resources voor gebruik in het developer portal sjablonen.  
  
-   [Tekenreeksbronnen](#strings)  
  
-   [De glyph-resources](#glyphs)  
  
##  <a name="strings"></a>Tekenreeksbronnen  
 API Management biedt een uitgebreide reeks tekenreeksresources voor gebruik in de portal voor ontwikkelaars. Deze resources worden vertaald in alle talen wordt ondersteund door de API Management. De standaardset van sjablonen maakt gebruik van deze resources voor paginakopteksten, labels en de constante tekenreeksen die worden weergegeven in de portal voor ontwikkelaars. Geef voor het gebruik van een tekenreeksbron in uw sjablonen, het voorvoegsel van de resource-tekenreeks gevolgd door de tekenreeksnaam van de, zoals weergegeven in het volgende voorbeeld.  
  
```  
{% localized "Prefix|Name" %}  
  
```  
  
 Het volgende voorbeeld is van de sjabloon van de lijst Product en geeft weer **producten** boven aan de pagina.  
  
```  
<h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>  
  
```  
  
 Raadpleeg de volgende tabellen voor de tekenreeksbronnen beschikbaar voor gebruik in uw developer portal sjablonen. Naam van de tabel gebruiken als het voorvoegsel voor de tekenreeksresources in de tabel.  
  
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
  
-   [Gebruikersprofiel](#UserProfile)  
  
###  <a name="ApisStrings"></a>ApisStrings  
  
|Naam|Tekst|  
|----------|----------|  
|PageTitleApis|API's|  
  
###  <a name="AppDetailsStrings"></a>AppDetailsStrings  
  
|Naam|Tekst|  
|----------|----------|  
|WebApplicationsDetailsTitle|Voorbeeld van de toepassing|  
|WebApplicationsRequirementsHeader|Vereisten|  
|WebApplicationsScreenshotAlt|schermopname|  
|WebApplicationsScreenshotsHeader|Schermafbeeldingen|  
  
###  <a name="ApplicationListStrings"></a>ApplicationListStrings  
  
|Naam|Tekst|  
|----------|----------|  
|WebDevelopersAppDeleteConfirmation|Weet u zeker dat u toepassing wilt verwijderen?|  
|WebDevelopersAppNotPublished|Niet gepubliceerd|  
|WebDevelopersAppNotSubminted|Niet verzonden|  
|WebDevelopersAppTableCategoryHeader|Category|  
|WebDevelopersAppTableNameHeader|Naam|  
|WebDevelopersAppTableStateHeader|Status|  
|WebDevelopersEditLink|Bewerken|  
|WebDevelopersRegisterAppLink|Toepassing registreren|  
|WebDevelopersRemoveLink|Verwijderen|  
|WebDevelopersSubmitLink|Verzenden|  
|WebDevelopersYourApplicationsHeader|Uw toepassingen|  
  
###  <a name="AppStrings"></a>AppStrings  
  
|Naam|Tekst|  
|----------|----------|  
|WebApplicationsHeader|Toepassingen|  
  
###  <a name="CommonResources"></a>CommonResources  
  
|Naam|Tekst|  
|----------|----------|  
|NoItemsToDisplay|Er zijn geen resultaten gevonden.|  
|GeneralExceptionMessage|Er is niet juist. Het is mogelijk een tijdelijke fout of een fout. Probeer het opnieuw.|  
|GeneralJsonExceptionMessage|Er is niet juist. Het is mogelijk een tijdelijke fout of een fout. Controleer, laad de pagina opnieuw en probeer het opnieuw.|  
|ConfirmationMessageUnsavedChanges|Er zijn enkele niet-opgeslagen wijzigingen. Weet u zeker dat u wilt annuleren en de wijzigingen negeren?|  
|AzureActiveDirectory|Azure Active Directory|  
|HttpLargeRequestMessage|HTTP-aanvraagtekst te groot.|  
  
###  <a name="CommonStrings"></a>CommonStrings  
  
|Naam|Tekst|  
|----------|----------|  
|ButtonLabelCancel|Annuleren|  
|ButtonLabelSave|Opslaan|  
|GeneralExceptionMessage|Er is niet juist. Het is mogelijk een tijdelijke fout of een fout. Probeer het opnieuw.|  
|NoItemsToDisplay|Er zijn geen items om weer te geven.|  
|PagerButtonLabelFirst|Eerste|  
|PagerButtonLabelLast|laatste|  
|PagerButtonLabelNext|Volgende|  
|PagerButtonLabelPrevious|Vorig|  
|PagerLabelPageNOfM|Pagina {0} van {1}|  
|PasswordTooShort|Het wachtwoord is te kort|  
|EmailAsPassword|Gebruik uw e-mailadres niet als uw wachtwoord|  
|PasswordSameAsUserName|Uw wachtwoord mag niet uw gebruikersnaam bevatten.|  
|PasswordTwoCharacterClasses|Klassen voor verschillende tekensets gebruiken|  
|PasswordTooManyRepetitions|Te veel herhalingen|  
|PasswordSequenceFound|Uw wachtwoord bevat reeksen|  
|PagerLabelPageSize|Paginagrootte|  
|CurtainLabelLoading|Laden...|  
|TablePlaceholderNothingToDisplay|Er zijn geen gegevens voor de geselecteerde periode en bereik|  
|ButtonLabelClose|Sluiten|  
  
###  <a name="Documentation"></a>Documentatie  
  
|Naam|Tekst|  
|----------|----------|  
|WebDocumentationInvalidHeaderErrorMessage|Ongeldige header {0}|  
|WebDocumentationInvalidRequestErrorMessage|Ongeldige aanvraag-URL|  
|TextboxLabelAccessToken|Toegangstoken *|  
|DropdownOptionPrimaryKeyFormat|Primaire-{0}|  
|DropdownOptionSecondaryKeyFormat|Secundaire-{0}|  
|WebDocumentationSubscriptionKeyText|De abonnementssleutel van uw|  
|WebDocumentationTemplatesAddHeaders|Vereiste HTTP-headers toevoegen|  
|WebDocumentationTemplatesBasicAuthSample|Basisverificatie-voorbeeld|  
|WebDocumentationTemplatesCurlForBasicAuth|voor basisverificatie gebruik:--gebruiker {username}: {wachtwoord}|  
|WebDocumentationTemplatesCurlValuesForPath|Geef waarden op voor de padparameters (weergegeven als {...}), uw abonnementssleutel en waarden voor queryparameters|  
|WebDocumentationTemplatesDeveloperKey|De abonnementssleutel van uw opgeven|  
|WebDocumentationTemplatesJavaApache|In dit voorbeeld gebruikt de Apache HTTP-client van HTTP-onderdelen (http://hc.apache.org/httpcomponents-client-ga/)|  
|WebDocumentationTemplatesOptionalParams|Waarden opgeven voor optionele parameters, indien nodig|  
|WebDocumentationTemplatesPhpPackage|Dit voorbeeld gebruikt de HTTP_Request2-pakket. (voor meer informatie: http://pear.php.net/package/HTTP_Request2)|  
|WebDocumentationTemplatesPythonValuesForPath|Geef waarden voor padparameters (weergegeven als {...}) en de hoofdtekst indien nodig|  
|WebDocumentationTemplatesRequestBody|Aanvraagtekst opgeven|  
|WebDocumentationTemplatesRequiredParams|Geef waarden op voor de volgende parameters vereist|  
|WebDocumentationTemplatesValuesForPath|Geef waarden voor padparameters (weergegeven als {...})|  
|OAuth2AuthorizationEndpointDescription|Het eindpunt voor autorisatie wordt gebruikt om te communiceren met de resource-eigenaar en het verkrijgen van een machtiging grant.|  
|OAuth2AuthorizationEndpointName|Autorisatie-eindpunt|  
|OAuth2TokenEndpointDescription|Eindpunt van het token wordt gebruikt door de client een toegangstoken verkrijgen in de vorm van de machtiging grant of token worden vernieuwd.|  
|OAuth2TokenEndpointName|-Tokeneindpunt|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_Description|< p\> de client initieert de stroom door de resource-eigenaar gebruikersagent naar het eindpunt voor autorisatie.  De client neemt de client-id, aangevraagde bereik, lokale staat en een omleidings-URI waarvoor de autorisatie-server de gebruikersagent stuurt terug zodra toegang wordt verleend (of geweigerd).     < /p\> < p\> de autorisatie-server verifieert de resource-eigenaar (via de gebruikersagent) en bepaalt of de resource-eigenaar verleent of weigert de aanvraag voor toegang tot de client.     < /p\> < p\> ervan uitgaande dat de resource-eigenaar toegang wordt verleend, de autorisatie-endserver leidt de gebruikersagent terug naar de client met behulp van de omleidings-URI opgegeven eerdere (in de aanvraag of tijdens de clientregistratie).  De omleidings-URI bevat een autorisatiecode en geen lokale status die eerder door de client geleverd.     < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_ErrorDescription|< p\> als de gebruiker de aanvraag voor toegang van weigert als de aanvraag ongeldig is, de client ontvangt met de volgende parameters die u aan bij de omleiding toegevoegd: < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_Name|Autorisatieaanvraag|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_RequestDescription|< p\> de client-app moet de gebruiker verzenden naar het eindpunt voor autorisatie om het OAuth-proces starten.          Op het eindpunt voor autorisatie, de gebruiker wordt geverifieerd en vervolgens verleent of weigert toegang tot de app.     < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_ResponseDescription|< p\> toegang, ervan uitgaande dat de resource-eigenaar verleent autorisatie-endserver leidt de gebruikersagent terug naar de client met behulp van de omleidings-URI opgegeven eerdere (in de aanvraag of tijdens de clientregistratie).  De omleidings-URI bevat een autorisatiecode en geen lokale status die eerder door de client geleverd. < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_Description|< p\> vraagt de client een toegangstoken van de server autorisatie '' s-tokeneindpunt door de autorisatiecode ontvangen in de vorige stap.  Wanneer de aanvraag, verifieert de client met de autorisatie-server.  De client neemt de omleiding die URI gebruikt voor het verkrijgen van de autorisatiecode voor verificatie. < /p\> < p\> de autorisatie-server verifieert de client, valideert de autorisatiecode en zorgt ervoor dat de omleidings-URI ontvangen overeenkomt met de URI die wordt gebruikt voor het omleiden van de client in stap (C).  Als geldig is, reageert de autorisatie-server op een toegangstoken en desgewenst een vernieuwingstoken. < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_ErrorDescription|< p\> als de verificatie van de aanvraag-client is mislukt of ongeldig is, de autorisatie-server reageert met de statuscode van een HTTP-fout 400 (ongeldige aanvraag) (tenzij anders opgegeven) en bevat de volgende parameters met het antwoord. < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_RequestDescription|< p\> maakt de client een aanvraag naar eindpunt van het token door te sturen met de volgende parameters met de vereiste notatie is ' application/x-1-800-www-Dell-form-urlencoded' met een tekencodering van UTF-8 in de HTTP-aanvragen entiteitshoofdtekst. < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_ResponseDescription|< p\> de autorisatie-server een toegangstoken en optionele vernieuwingstoken uitgeeft en vormt van het antwoord door de volgende parameters toe te voegen aan het hoofdgedeelte van het HTTP-antwoord met een statuscode van 200 (OK). < /p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_Description|< p\> de client wordt geverifieerd met de autorisatie-server en aanvragen van een toegangstoken van het eindpunt van het token. < /p\> < p\> de autorisatie-server verifieert de client en als geldig is, geeft u een toegangstoken. < /p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_ErrorDescription|< p\> als de aanvraag is mislukt clientverificatie of ongeldig is in de autorisatie-server reageert met een HTTP-fout 400 (ongeldige aanvraag)-statuscode (tenzij anders opgegeven) en bevat de volgende parameters met het antwoord. < /p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_RequestDescription|< p\> de client een aanvraag indient bij eindpunt van het token door de volgende parameters met de vereiste notatie is ' application/x-1-800-www-Dell-form-urlencoded' met een tekencodering van UTF-8 in de HTTP-aanvragen entiteitshoofdtekst toe te voegen. < /p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_ResponseDescription|< p\> als het toegangsverzoek token geldig en geautoriseerd is, de autorisatie-server een toegangstoken en optionele vernieuwingstoken uitgeeft en vormt van het antwoord door de volgende parameters toe te voegen aan het hoofdgedeelte van het HTTP-antwoord met een statuscode van 200 (OK). < /p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_Description|< p\> de client initieert de stroom door de resource-eigenaar '' s gebruikersagent naar het eindpunt voor autorisatie.  De client neemt de client-id, aangevraagde bereik, lokale staat en een omleidings-URI waarvoor de autorisatie-server de gebruikersagent stuurt terug zodra toegang wordt verleend (of geweigerd). < /p\> < p\> de autorisatie-server verifieert de resource-eigenaar (via de gebruikersagent) en bepaalt of de resource-eigenaar verleent of de client weigert '' s-toegangsaanvraag. < /p\> < p\> ervan uitgaande dat de resource-eigenaar toegang wordt verleend, de autorisatie-endserver leidt de gebruikersagent terug naar de client met behulp van de omleiding URI die eerder is verkregen.  De omleidings-URI bevat het toegangstoken in de URI-fragment. < /p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_ErrorDescription|< p\> als de resource-eigenaar de aanvraag voor toegang tot weigert of als de aanvraag is mislukt om redenen dan een ontbrekende of ongeldige omleiding URI, de autorisatie-server meldt dat de client door de volgende parameters toe te voegen aan het fragmentonderdeel van de omleidings-URI ' application/x-1-800-www-Dell-form-urlencoded'-indeling. < /p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_RequestDescription|< p\> de client-app moet de gebruiker verzenden naar het eindpunt voor autorisatie om het OAuth-proces starten.      Op het eindpunt voor autorisatie, de gebruiker wordt geverifieerd en vervolgens verleent of weigert toegang tot de app. < /p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_ResponseDescription|< p\> als de resource-eigenaar de aanvraag voor toegang tot verleent, de autorisatie-server een toegangstoken problemen en naar de client door de volgende parameters toe te voegen aan het fragmentonderdeel van de omleidings-URI ' application/x-1-800-www-Dell-form-urlencoded'-indeling bezorgd. < /p\>|  
|OAuth2Flow_ObtainAuthorization_AuthorizationCodeGrant_Description|Autorisatiecodestroom is geoptimaliseerd voor clients die geschikt is voor het bewaken van de vertrouwelijkheid van hun referenties (bijvoorbeeld server webtoepassingen geïmplementeerd met behulp van PHP, Java, Python, Ruby, ASP.NET, enz.).|  
|OAuth2Flow_ObtainAuthorization_AuthorizationCodeGrant_Name|Autorisatie Code verlenen|  
|OAuth2Flow_ObtainAuthorization_ClientCredentialsGrant_Description|Clientreferentiestroom is geschikt in gevallen waar de client (uw toepassing) toegang tot beveiligde bronnen onder het beheer worden aangevraagd. De client wordt beschouwd als een resource-eigenaar, zodat geen tussenkomst van de eindgebruiker is vereist.|  
|OAuth2Flow_ObtainAuthorization_ClientCredentialsGrant_Name|Client referenties verlenen|  
|OAuth2Flow_ObtainAuthorization_ImplicitGrant_Description|Impliciete stroom is geoptimaliseerd voor clients waarvoor niet van het onderhouden van de vertrouwelijkheid van hun referenties functioneren van een bepaalde omleidings-URI bekend. Deze clients zijn doorgaans geïmplementeerd in een browser met gebruik van een scripttaal zoals JavaScript.|  
|OAuth2Flow_ObtainAuthorization_ImplicitGrant_Name|Impliciete verlenen|  
|OAuth2Flow_ObtainAuthorization_ResourceOwnerPasswordCredentialsGrant_Description|Resource-eigenaar wachtwoord referentiestroom is geschikt in gevallen waar de resource-eigenaar een vertrouwensrelatie met de client (uw toepassing), zoals het besturingssysteem van het apparaat of een toepassing met bijzondere rechten heeft. Deze stroom is geschikt voor clients die geschikt is voor het verkrijgen van de bron van de eigenaar van referenties (gebruikersnaam en wachtwoord, meestal met behulp van een interactief formulier).|  
|OAuth2Flow_ObtainAuthorization_ResourceOwnerPasswordCredentialsGrant_Name|De referenties van het wachtwoord voor resource-eigenaar verlenen|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_Description|< p\> voorziet de client van de resource-eigenaar met de gebruikersnaam en wachtwoord. < /p\> < p\> vraagt de client een toegangstoken van de server autorisatie '' s-tokeneindpunt door de referenties ontvangen van de resource-eigenaar.  Wanneer de aanvraag, verifieert de client met de autorisatie-server. < /p\> < p\> de autorisatie-server verifieert de client en de referenties van de resource-eigenaar valideert en als geldig is, geeft u een toegangstoken. < /p\>|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_ErrorDescription|< p\> als de aanvraag is mislukt clientverificatie of ongeldig is in de autorisatie-server reageert met een HTTP-fout 400 (ongeldige aanvraag)-statuscode (tenzij anders opgegeven) en bevat de volgende parameters met het antwoord. < /p\>|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_RequestDescription|< p\> de client een aanvraag indient bij eindpunt van het token door de volgende parameters met de vereiste notatie is ' application/x-1-800-www-Dell-form-urlencoded' met een tekencodering van UTF-8 in de HTTP-aanvragen entiteitshoofdtekst toe te voegen. < /p\>|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_ResponseDescription|< p\> als het toegangsverzoek token geldig en geautoriseerd is, de autorisatie-server een toegangstoken en optionele vernieuwingstoken uitgeeft en vormt van het antwoord door de volgende parameters toe te voegen aan het hoofdgedeelte van het HTTP-antwoord met een statuscode van 200 (OK). < /p\>|  
|OAuth2Step_AccessTokenRequest_Name|De tokenaanvraag toegang|  
|OAuth2Step_AuthorizationRequest_Name|Autorisatieaanvraag|  
|OAuth2AccessToken_AuthorizationCodeGrant_TokenResponse|VEREIST. Het toegangstoken dat is uitgegeven door de autorisatie-server.|  
|OAuth2AccessToken_ClientCredentialsGrant_TokenResponse|VEREIST. Het toegangstoken dat is uitgegeven door de autorisatie-server.|  
|OAuth2AccessToken_ImplicitGrant_AuthorizationResponse|VEREIST. Het toegangstoken dat is uitgegeven door de autorisatie-server.|  
|OAuth2AccessToken_ResourceOwnerPasswordCredentialsGrant_TokenResponse|VEREIST. Het toegangstoken dat is uitgegeven door de autorisatie-server.|  
|OAuth2ClientId_AuthorizationCodeGrant_AuthorizationRequest|VEREIST. Client-id.|  
|OAuth2ClientId_AuthorizationCodeGrant_TokenRequest|VEREIST als de client niet is geverifieerd met de autorisatie-server.|  
|OAuth2ClientId_ImplicitGrant_AuthorizationRequest|VEREIST. De client-id.|  
|OAuth2Code_AuthorizationCodeGrant_AuthorizationResponse|VEREIST. De autorisatiecode gegenereerd door de autorisatie-server.|  
|OAuth2Code_AuthorizationCodeGrant_TokenRequest|VEREIST. De autorisatiecode ontvangen van de autorisatie-server.|  
|OAuth2ErrorDescription_AuthorizationCodeGrant_AuthorizationErrorResponse|OPTIONEEL. Leesbare ASCII-tekst bieden aanvullende informatie.|  
|OAuth2ErrorDescription_AuthorizationCodeGrant_TokenErrorResponse|OPTIONEEL. Leesbare ASCII-tekst bieden aanvullende informatie.|  
|OAuth2ErrorDescription_ClientCredentialsGrant_TokenErrorResponse|OPTIONEEL. Leesbare ASCII-tekst bieden aanvullende informatie.|  
|OAuth2ErrorDescription_ImplicitGrant_AuthorizationErrorResponse|OPTIONEEL. Leesbare ASCII-tekst bieden aanvullende informatie.|  
|OAuth2ErrorDescription_ResourceOwnerPasswordCredentialsGrant_TokenErrorResponse|OPTIONEEL. Leesbare ASCII-tekst bieden aanvullende informatie.|  
|OAuth2ErrorUri_AuthorizationCodeGrant_AuthorizationErrorResponse|OPTIONEEL. Een URI die een leesbare webpagina met informatie over de fout te identificeren.|  
|OAuth2ErrorUri_AuthorizationCodeGrant_TokenErrorResponse|OPTIONEEL. Een URI die een leesbare webpagina met informatie over de fout te identificeren.|  
|OAuth2ErrorUri_ClientCredentialsGrant_TokenErrorResponse|OPTIONEEL. Een URI die een leesbare webpagina met informatie over de fout te identificeren.|  
|OAuth2ErrorUri_ImplicitGrant_AuthorizationErrorResponse|OPTIONEEL. Een URI die een leesbare webpagina met informatie over de fout te identificeren.|  
|OAuth2ErrorUri_ResourceOwnerPasswordCredentialsGrant_TokenErrorResponse|OPTIONEEL. Een URI die een leesbare webpagina met informatie over de fout te identificeren.|  
|OAuth2Error_AuthorizationCodeGrant_AuthorizationErrorResponse|VEREIST. Een enkele ASCII-foutcode wordt gegeven van de volgende: invalid_request, unauthorized_client, access_denied, unsupported_response_type, invalid_scope, server_error, temporarily_unavailable.|  
|OAuth2Error_AuthorizationCodeGrant_TokenErrorResponse|VEREIST. Een enkele ASCII-foutcode wordt gegeven van de volgende: invalid_request, invalid_client, invalid_grant, unauthorized_client, unsupported_grant_type, invalid_scope.|  
|OAuth2Error_ClientCredentialsGrant_TokenErrorResponse|VEREIST. Een enkele ASCII-foutcode wordt gegeven van de volgende: invalid_request, invalid_client, invalid_grant, unauthorized_client, unsupported_grant_type, invalid_scope.|  
|OAuth2Error_ImplicitGrant_AuthorizationErrorResponse|VEREIST. Een enkele ASCII-foutcode wordt gegeven van de volgende: invalid_request, unauthorized_client, access_denied, unsupported_response_type, invalid_scope, server_error, temporarily_unavailable.|  
|OAuth2Error_ResourceOwnerPasswordCredentialsGrant_TokenErrorResponse|VEREIST. Een enkele ASCII-foutcode wordt gegeven van de volgende: invalid_request, invalid_client, invalid_grant, unauthorized_client, unsupported_grant_type, invalid_scope.|  
|OAuth2ExpiresIn_AuthorizationCodeGrant_TokenResponse|AANBEVOLEN. De levensduur in seconden van het toegangstoken.|  
|OAuth2ExpiresIn_ClientCredentialsGrant_TokenResponse|AANBEVOLEN. De levensduur in seconden van het toegangstoken.|  
|OAuth2ExpiresIn_ImplicitGrant_AuthorizationResponse|AANBEVOLEN. De levensduur in seconden van het toegangstoken.|  
|OAuth2ExpiresIn_ResourceOwnerPasswordCredentialsGrant_TokenResponse|AANBEVOLEN. De levensduur in seconden van het toegangstoken.|  
|OAuth2GrantType_AuthorizationCodeGrant_TokenRequest|VEREIST. Waarde moet worden ingesteld op 'authorization_code'.|  
|OAuth2GrantType_ClientCredentialsGrant_TokenRequest|VEREIST. Waarde moet worden ingesteld op 'client_credentials'.|  
|OAuth2GrantType_ResourceOwnerPasswordCredentialsGrant_TokenRequest|VEREIST. Waarde moet worden ingesteld op 'password'.|  
|OAuth2Password_ResourceOwnerPasswordCredentialsGrant_TokenRequest|VEREIST. Het wachtwoord van de resource-eigenaar.|  
|OAuth2RedirectUri_AuthorizationCodeGrant_AuthorizationRequest|OPTIONEEL. Het eindpunt omleiding URI moet een absolute URI zijn.|  
|OAuth2RedirectUri_AuthorizationCodeGrant_TokenRequest|VEREIST als de parameter 'redirect_uri' is opgenomen in de autorisatieaanvraag en hun waarden identiek zijn.|  
|OAuth2RedirectUri_ImplicitGrant_AuthorizationRequest|OPTIONEEL. Het eindpunt omleiding URI moet een absolute URI zijn.|  
|OAuth2RefreshToken_AuthorizationCodeGrant_TokenResponse|OPTIONEEL. Het vernieuwingstoken dat kan worden gebruikt voor nieuwe toegangstokens te verkrijgen.|  
|OAuth2RefreshToken_ClientCredentialsGrant_TokenResponse|OPTIONEEL. Het vernieuwingstoken dat kan worden gebruikt voor nieuwe toegangstokens te verkrijgen.|  
|OAuth2RefreshToken_ResourceOwnerPasswordCredentialsGrant_TokenResponse|OPTIONEEL. Het vernieuwingstoken dat kan worden gebruikt voor nieuwe toegangstokens te verkrijgen.|  
|OAuth2ResponseType_AuthorizationCodeGrant_AuthorizationRequest|VEREIST. Waarde moet worden ingesteld op 'code'.|  
|OAuth2ResponseType_ImplicitGrant_AuthorizationRequest|VEREIST. Waarde moet worden ingesteld op 'token'.|  
|OAuth2Scope_AuthorizationCodeGrant_AuthorizationRequest|OPTIONEEL. Het bereik van de aanvraag.|  
|OAuth2Scope_AuthorizationCodeGrant_TokenResponse|OPTIONEEL als identiek is aan het bereik dat is aangevraagd door de client. anders is vereist.|  
|OAuth2Scope_ClientCredentialsGrant_TokenRequest|OPTIONEEL. Het bereik van de aanvraag.|  
|OAuth2Scope_ClientCredentialsGrant_TokenResponse|OPTIONEEL, indien identiek is aan het bereik dat is aangevraagd door de client. anders is vereist.|  
|OAuth2Scope_ImplicitGrant_AuthorizationRequest|OPTIONEEL. Het bereik van de aanvraag.|  
|OAuth2Scope_ImplicitGrant_AuthorizationResponse|OPTIONEEL als identiek is aan het bereik dat is aangevraagd door de client. anders is vereist.|  
|OAuth2Scope_ResourceOwnerPasswordCredentialsGrant_TokenRequest|OPTIONEEL. Het bereik van de aanvraag.|  
|OAuth2Scope_ResourceOwnerPasswordCredentialsGrant_TokenResponse|OPTIONEEL, indien identiek is aan het bereik dat is aangevraagd door de client. anders is vereist.|  
|OAuth2State_AuthorizationCodeGrant_AuthorizationErrorResponse|Als de parameter 'state' aanwezig in de aanvraag van de client-autorisatie is vereist.  De exacte waarde van de client ontvangen.|  
|OAuth2State_AuthorizationCodeGrant_AuthorizationRequest|AANBEVOLEN. Een ondoorzichtige waarde die wordt gebruikt door de client naar de stand tussen de aanvraag en de callback.  De autorisatie-server bevat deze waarde wanneer het omleiden van de gebruikersagent terug naar de client.  De parameter moet worden gebruikt voor het voorkomen van aanvraagvervalsing op meerdere sites.|  
|OAuth2State_AuthorizationCodeGrant_AuthorizationResponse|Als de parameter 'state' aanwezig in de aanvraag van de client-autorisatie is vereist.  De exacte waarde van de client ontvangen.|  
|OAuth2State_ImplicitGrant_AuthorizationErrorResponse|Als de parameter 'state' aanwezig in de aanvraag van de client-autorisatie is vereist.  De exacte waarde van de client ontvangen.|  
|OAuth2State_ImplicitGrant_AuthorizationRequest|AANBEVOLEN. Een ondoorzichtige waarde die wordt gebruikt door de client naar de stand tussen de aanvraag en de callback.  De autorisatie-server bevat deze waarde wanneer het omleiden van de gebruikersagent terug naar de client.  De parameter moet worden gebruikt voor het voorkomen van aanvraagvervalsing op meerdere sites.|  
|OAuth2State_ImplicitGrant_AuthorizationResponse|Als de parameter 'state' aanwezig in de aanvraag van de client-autorisatie is vereist.  De exacte waarde van de client ontvangen.|  
|OAuth2TokenType_AuthorizationCodeGrant_TokenResponse|VEREIST. Het type van het token dat is uitgegeven.|  
|OAuth2TokenType_ClientCredentialsGrant_TokenResponse|VEREIST. Het type van het token dat is uitgegeven.|  
|OAuth2TokenType_ImplicitGrant_AuthorizationResponse|VEREIST. Het type van het token dat is uitgegeven.|  
|OAuth2TokenType_ResourceOwnerPasswordCredentialsGrant_TokenResponse|VEREIST. Het type van het token dat is uitgegeven.|  
|OAuth2UserName_ResourceOwnerPasswordCredentialsGrant_TokenRequest|VEREIST. De gebruikersnaam van de resource-eigenaar.|  
|OAuth2UnsupportedTokenType|Type token {0}' is geen supporetd.|  
|OAuth2InvalidState|Ongeldige reactie van de autorisatie-server|  
|OAuth2GrantType_AuthorizationCode|Autorisatiecode|  
|OAuth2GrantType_Implicit|Impliciete|  
|OAuth2GrantType_ClientCredentials|Clientreferenties|  
|OAuth2GrantType_ResourceOwnerPassword|Wachtwoord voor resource-eigenaar|  
|WebDocumentation302Code|302 gevonden|  
|WebDocumentation400Code|400 (ongeldige aanvraag)|  
|OAuth2SendingMethod_AuthHeader|Autorisatie-header|  
|OAuth2SendingMethod_QueryParam|Query-parameter|  
|OAuth2AuthorizationServerGeneralException|Er is een fout opgetreden bij het verlenen van toegang via {0}|  
|OAuth2AuthorizationServerCommunicationException|Een HTTP-verbinding met de autorisatie-server kan niet worden gemaakt of het onverwacht is gesloten.|  
|WebDocumentationOAuth2GeneralErrorMessage|Er is een onverwachte fout opgetreden.|  
|AuthorizationServerCommunicationException|Autorisatie server communicatie uitzondering is opgetreden. Neem contact op met de beheerder.|  
|TextblockSubscriptionKeyHeaderDescription|Abonnementssleutel dat toegang tot deze API biedt. Gevonden in de < een href ='/ developer'\>profiel < /a\>.|  
|TextblockOAuthHeaderDescription|OAuth 2.0-toegangstoken is verkregen van < ik\>{0} < /i\>. Ondersteunde typen grant: < i\>{1} < /i\>.|  
|TextblockContentTypeHeaderDescription|Het mediatype van de instantie die wordt verzonden naar de API.|  
|ErrorMessageApiNotAccessible|De API die u probeert aan te roepen is op dit moment niet toegankelijk. Neem contact op met de uitgever van de API < een href = "/ problemen"\>hier < /a\>.|  
|ErrorMessageApiTimedout|De API die u probeert aan te roepen duurt langer dan normaal antwoord om terug te gaan. Neem contact op met de uitgever van de API < een href = "/ problemen"\>hier < /a\>.|  
|BadRequestParameterExpected|'parameter '{0}' wordt verwacht'|  
|TooltipTextDoubleClickToSelectAll|Dubbelklik om alles te selecteren.|  
|TooltipTextHideRevealSecret|Weergeven/verbergen|  
|ButtonLinkOpenConsole|Probeer het nu|  
|SectionHeadingRequestBody|Aanvraagtekst|  
|SectionHeadingRequestParameters|Parameters van de aanvraag|  
|SectionHeadingRequestUrl|Aanvraag-URL|  
|SectionHeadingResponse|Antwoord|  
|SectionHeadingRequestHeaders|Aanvraagheaders|  
|FormLabelSubtextOptional|Optioneel|  
|SectionHeadingCodeSamples|Codevoorbeelden|  
|TextblockOpenidConnectHeaderDescription|OpenID Connect-id-token verkregen via < ik\>{0} < /i\>. Ondersteunde typen grant: < i\>{1} < /i\>.|  
  
###  <a name="ErrorPageStrings"></a>ErrorPageStrings  
  
|Naam|Tekst|  
|----------|----------|  
|LinkLabelBack|Terug|  
|LinkLabelHomePage|startpagina|  
|LinkLabelSendUsEmail|Stuur ons een e-mailbericht|  
|PageTitleError|Er is een probleem voor de aangevraagde pagina|  
|TextblockPotentialCauseIntermittentIssue|Kan dit een onregelmatige data access-probleem dat al is.|  
|TextblockPotentialCauseOldLink|De koppeling die u hebt geklikt op kan worden oude en wijst niet naar de juiste locatie meer.|  
|TextblockPotentialCauseTechnicalProblem|Mogelijk zijn er een technisch probleem aan onze kant.|  
|TextblockPotentialSolutionRefresh|Vernieuw de pagina.|  
|TextblockPotentialSolutionStartOver|Beginnen van onze {0}.|  
|TextblockPotentialSolutionTryAgain|Ga {0} en probeer de actie die u opnieuw uitgevoerd.|  
|TextReportProblem|{0} met een beschrijving van wat er mis ging en we kijken deze zo snel kunnen wij.|  
|TitlePotentialCause|Mogelijke oorzaak|  
|TitlePotentialSolution|Het is mogelijk slechts een tijdelijk probleem, enkele dingen om te proberen|  
  
###  <a name="IssuesStrings"></a>IssuesStrings  
  
|Naam|Tekst|  
|----------|----------|  
|WebIssuesIndexTitle|Problemen|  
|WebIssuesNoActiveSubscriptions|U hebt geen actieve abonnementen. U moet zich abonneren voor een product voor het rapporteren van een probleem.|  
|WebIssuesNotSignin|U bent niet aangemeld. Voer de {0} om een probleem melden of opmerkingen posten.|  
|WebIssuesReportIssueButton|Probleem melden|  
|WebIssuesSignIn|aanmelden|  
|WebIssuesStatusReportedBy|Status: {0} &#124; Gemeld door {1}|  
  
###  <a name="NotFoundStrings"></a>NotFoundStrings  
  
|Naam|Tekst|  
|----------|----------|  
|LinkLabelHomePage|startpagina|  
|LinkLabelSendUsEmail|Stuur ons een e-mailbericht|  
|PageTitleNotFound|Sorry, dat de pagina die u zoekt niet vinden|  
|TextblockPotentialCauseMisspelledUrl|U verkeerd gespeld de URL als u hebt getypt in.|  
|TextblockPotentialCauseOldLink|De koppeling die u hebt geklikt op kan worden oude en wijst niet naar de juiste locatie meer.|  
|TextblockPotentialSolutionRetype|Probeer opnieuw de URL te typen.|  
|TextblockPotentialSolutionStartOver|Beginnen van onze {0}.|  
|TextReportProblem|{0} met een beschrijving van wat er mis ging en we kijken deze zo snel kunnen wij.|  
|TitlePotentialCause|Mogelijke oorzaak|  
|TitlePotentialSolution|Mogelijke oplossing|  
  
###  <a name="ProductDetailsStrings"></a>ProductDetailsStrings  
  
|Naam|Tekst|  
|----------|----------|  
|WebProductsAgreement|Als u zich abonneert op {0} Product, ik ga akkoord met de `<a data-toggle='modal' href='#legal-terms'\>Terms of Use</a\>`.|  
|WebProductsLegalTermsLink|Gebruiksvoorwaarden|  
|WebProductsSubscribeButton|Aanmelden|  
|WebProductsUsageLimitsHeader|Limieten voor Resourcegebruik|  
|WebProductsYouAreNotSubscribed|U bent geabonneerd op dit product.|  
|WebProductsYouRequestedSubscription|U hebt aangevraagd abonnement voor dit product.|  
|ErrorYouNeedtoAgreeWithLegalTerms|U moet de gebruiksvoorwaarden accepteren voordat u verder kunt gaan.|  
|ButtonLabelAddSubscription|Abonnement toevoegen|  
|LinkLabelChangeSubscriptionName|wijzigen|  
|ButtonLabelConfirm|Bevestigen|  
|TextblockMultipleSubscriptionsCount|U hebt {0} abonnementen voor dit product:|  
|TextblockSingleSubscriptionsCount|U hebt {0} abonnement voor dit product:|  
|TextblockSingleApisCount|Dit product bevat {0} API:|  
|TextblockMultipleApisCount|Dit product bevat {0} API's:|  
|TextblockHeaderSubscribe|Abonneren op product|  
|TextblockSubscriptionDescription|Een nieuw abonnement worden gemaakt als volgt:|  
|TextblockSubscriptionLimitReached|Abonnementen limiet bereikt.|  
  
###  <a name="ProductsStrings"></a>ProductsStrings  
  
|Naam|Tekst|  
|----------|----------|  
|PageTitleProducts|Producten|  
  
###  <a name="ProviderInfoStrings"></a>ProviderInfoStrings  
  
|Naam|Tekst|  
|----------|----------|  
|TextboxExternalIdentitiesDisabled|Aanmelden is uitgeschakeld door de groep administrators op dit moment.|  
|TextboxExternalIdentitiesSigninInvitation|U kunt ook aanmelden|  
|TextboxExternalIdentitiesSigninInvitationPrimary|Meld u aan met:|  
  
###  <a name="SigninResources"></a>SigninResources  
  
|Naam|Tekst|  
|----------|----------|  
|PrincipalNotFound|Principal is niet gevonden of de handtekening is ongeldig|  
|ErrorSsoAuthenticationFailed|SSO-verificatie is mislukt|  
|ErrorSsoAuthenticationFailedDetailed|Ongeldig token dat is opgegeven of de handtekening kan niet worden geverifieerd.|  
|ErrorSsoTokenInvalid|SSO-token is ongeldig|  
|ValidationErrorSpecificEmailAlreadyExists|E-mailadres {0}' is al geregistreerd|  
|ValidationErrorSpecificEmailInvalid|E-mailadres {0} is ongeldig|  
|ValidationErrorPasswordInvalid|Wachtwoord is ongeldig. Corrigeer de fouten en probeer het opnieuw.|  
|PropertyTooShort|{0} is te kort|  
|WebAuthenticationAddresserEmailInvalidErrorMessage|Ongeldig e-mailadres.|  
|ValidationMessageNewPasswordConfirmationRequired|Nieuw wachtwoord bevestigen|  
|ValidationErrorPasswordConfirmationRequired|Bevestig het wachtwoord is leeg|  
|WebAuthenticationEmailChangeNotice|Wijziging bevestiging per e-mail is onderweg met {0}. Volg de instructies in het bevestigen van je nieuwe e-mailadres. Als het e-mailbericht niet heeft bereikt naar uw postvak in in de volgende minuten, controleert u de map Ongewenste e-mail.|  
|WebAuthenticationEmailChangeNoticeHeader|Uw e-wijzigingsaanvraag is met succes verwerkt|  
|WebAuthenticationEmailChangeNoticeTitle|Wijziging van het e-mailadres aangevraagd|  
|WebAuthenticationEmailHasBeenRevertedNotice|U e-mailadres is al aanwezig. Aanvraag is hersteld|  
|ValidationErrorEmailAlreadyExists|Er bestaat al een e-mailadres|  
|ValidationErrorEmailInvalid|Ongeldig e-mailadres|  
|TextboxLabelEmail|E-mail|  
|ValidationErrorEmailRequired|E-mailadres is vereist.|  
|WebAuthenticationErrorNoticeHeader|Fout|  
|WebAuthenticationFieldLengthErrorMessage|{0} moet een maximale lengte van {1}|  
|TextboxLabelEmailFirstName|Voornaam|  
|ValidationErrorFirstNameRequired|Voornaam is vereist.|  
|ValidationErrorFirstNameInvalid|Ongeldige voornaam|  
|NoticeInvalidInvitationToken|Houd er rekening mee dat bevestiging koppelingen slechts 48 uur geldig zijn. Als u nog steeds binnen deze periode, Controleer of dat de koppeling correct is. Als de koppeling is verlopen, klikt u vervolgens herhaalt u de actie die u probeert om te bevestigen.|  
|NoticeHeaderInvalidInvitationToken|Ongeldige uitnodiging token|  
|NoticeTitleInvalidInvitationToken|Bevestiging fout|  
|WebAuthenticationLastNameInvalidErrorMessage|Ongeldige achternaam|  
|TextboxLabelEmailLastName|Achternaam|  
|ValidationErrorLastNameRequired|Achternaam is vereist.|  
|WebAuthenticationLinkExpiredNotice|Bevestigingskoppeling naar u verzonden is verlopen. `<a href={0}?token={1}>Resend confirmation email.</a\>`|  
|NoticePasswordResetLinkInvalidOrExpired|Uw wachtwoord opnieuw instellen van koppeling is ongeldig of verlopen.|  
|WebAuthenticationLinkExpiredNoticeTitle|Koppeling verzonden|  
|WebAuthenticationNewPasswordLabel|Nieuw wachtwoord|  
|ValidationMessageNewPasswordRequired|Nieuwe wachtwoord is vereist.|  
|TextboxLabelNotificationsSenderEmail|Meldingen afzender e|  
|TextboxLabelOrganizationName|Naam van de organisatie|  
|WebAuthenticationOrganizationRequiredErrorMessage|Naam van de organisatie is leeg|  
|WebAuthenticationPasswordChangedNotice|Uw wachtwoord is bijgewerkt|  
|WebAuthenticationPasswordChangedNoticeTitle|Wachtwoord wilt bijwerken|  
|WebAuthenticationPasswordCompareErrorMessage|Wachtwoorden komen niet overeen|  
|WebAuthenticationPasswordConfirmLabel|Wachtwoord bevestigen|  
|ValidationErrorPasswordInvalidDetailed|Wachtwoord is niet sterk genoeg.|  
|WebAuthenticationPasswordLabel|Wachtwoord|  
|ValidationErrorPasswordRequired|Wachtwoord is vereist.|  
|WebAuthenticationPasswordResetSendNotice|Wijzig de wachtwoordbevestiging e-mail is onderweg met {0}. Volg de instructies in het e-mailbericht om door te gaan uw wachtwoord wijzigen.|  
|WebAuthenticationPasswordResetSendNoticeHeader|Uw aanvraag voor wachtwoordherstel is met succes verwerkt|  
|WebAuthenticationPasswordResetSendNoticeTitle|Wachtwoordherstel aangevraagd|  
|WebAuthenticationRequestNotFoundNotice|Aanvraag is niet gevonden|  
|WebAuthenticationSenderEmailRequiredErrorMessage|Meldingen afzender e-mail is leeg|  
|WebAuthenticationSigninPasswordLabel|Bevestig de wijziging door te voeren van een wachtwoord|  
|WebAuthenticationSignupConfirmNotice|Bevestiging per e-mail is onderweg met {0}. < br /\> Volg de instructies in het e-mailbericht voor het activeren van uw account. < br /\> als het e-mailadres niet in je postvak in binnen enkele minuten plaatsvinden, Controleer of de map Ongewenste e-mail.|  
|WebAuthenticationSignupConfirmNoticeHeader|Uw account is gemaakt|  
|WebAuthenticationSignupConfirmNoticeRepeatHeader|Registratie bevestigingsbericht is opnieuw verzonden|  
|WebAuthenticationSignupConfirmNoticeTitle|Account dat is gemaakt|  
|WebAuthenticationTokenRequiredErrorMessage|Token is leeg|  
|WebAuthenticationUserAlreadyRegisteredNotice|Het lijkt dat een gebruiker met dit e-mailadres is al geregistreerd in het systeem. Als u uw wachtwoord bent vergeten, probeert u teruggezet of neem contact op met het ondersteuningsteam.|  
|WebAuthenticationUserAlreadyRegisteredNoticeHeader|Gebruiker is al geregistreerd|  
|WebAuthenticationUserAlreadyRegisteredNoticeTitle|Al geregistreerd|  
|ButtonLabelChangePassword|Wachtwoord wijzigen|  
|ButtonLabelChangeAccountInfo|Informatie over de account wijzigen|  
|ButtonLabelCloseAccount|Account sluiten|  
|WebAuthenticationInvalidCaptchaErrorMessage|Ingevoerde tekst komt niet overeen met de tekst op de afbeelding. Probeer het opnieuw.|  
|ValidationErrorCredentialsInvalid|E-mailadres of wachtwoord is ongeldig. Corrigeer de fouten en probeer het opnieuw.|  
|WebAuthenticationRequestIsNotValid|Aanvraag is niet geldig|  
|WebAuthenticationUserIsNotConfirm|Bevestig uw inschrijving voordat u probeert aan te melden.|  
|WebAuthenticationInvalidEmailFormated|E-mailadres is ongeldig: {0}|  
|WebAuthenticationUserNotFound|De gebruiker is niet gevonden|  
|WebAuthenticationTenantNotRegistered|Uw account hoort bij een Azure Active Directory-tenant die niet is gemachtigd voor toegang tot deze portal.|  
|WebAuthenticationAuthenticationFailed|Verificatie is mislukt.|  
|WebAuthenticationGooglePlusNotEnabled|Verificatie is mislukt. Als u de toepassing gemachtigd is vervolgens Neem contact op met de beheerder om ervoor te zorgen dat verificatie met Google juist geconfigureerd.|  
|ValidationErrorAllowedTenantIsRequired|Toegestane Tenant is vereist|  
|ValidationErrorTenantIsNotValid|De Azure Active Directory-tenant '{0}' is niet geldig.|  
|WebAuthenticationActiveDirectoryTitle|Azure Active Directory|  
|WebAuthenticationLoginUsingYourProvider|Meld u aan met uw account {0}|  
|WebAuthenticationUserLimitNotice|Deze service heeft het maximum aantal toegestane gebruikers bereikt. Neem `<a href="mailto:{0}"\>contact the administrator</a\>` hun service upgraden en opnieuw inschakelen gebruikersregistratie.|  
|WebAuthenticationUserLimitNoticeHeader|Gebruikersregistratie uitgeschakeld|  
|WebAuthenticationUserLimitNoticeTitle|Gebruikersregistratie uitgeschakeld|  
|WebAuthenticationUserRegistrationDisabledNotice|De registratie van gebruikers is uitgeschakeld door de beheerder. Meld u aan met externe id-provider.|  
|WebAuthenticationUserRegistrationDisabledNoticeHeader|Gebruikersregistratie uitgeschakeld|  
|WebAuthenticationUserRegistrationDisabledNoticeTitle|Gebruikersregistratie uitgeschakeld|  
|WebAuthenticationSignupPendingConfirmationNotice|Voordat het maken van uw account kan worden voltooid, moeten we controleren of uw e-mailadres. Er is een e-mailbericht verzonden naar {0}. Volg de instructies in het e-mailbericht om uw account te activeren. Als het e-mailbericht niet binnen de komende minuten plaatsvinden, controleert u de map Ongewenste e-mail.|  
|WebAuthenticationSignupPendingConfirmationAccountFoundNotice|Er is een niet-bevestigde account aangetroffen voor de e-mailadres {0}. We moeten controleren of uw e-mailadres om het maken van uw account te voltooien. Er is een e-mailbericht verzonden naar {0}. Volg de instructies in het e-mailbericht om uw account te activeren. Als het e-mailbericht niet binnen de komende minuten plaatsvinden, Controleer of de map Ongewenste e-mail|  
|WebAuthenticationSignupConfirmationAlmostDone|Bijna klaar|  
|WebAuthenticationSignupConfirmationEmailSent|Er is een e-mailbericht verzonden naar {0}. Volg de instructies in het e-mailbericht om uw account te activeren. Als het e-mailbericht niet binnen de komende minuten plaatsvinden, controleert u de map Ongewenste e-mail.|  
|WebAuthenticationEmailSentNotificationMessage|E-mail is verzonden op {0}|  
|WebAuthenticationNoAadTenantConfigured|Er is geen Azure Active Directory-tenant is geconfigureerd voor de service.|  
|CheckboxLabelUserRegistrationTermsConsentRequired|Ik ga akkoord met de `<a data-toggle="modal" href="#" data-target="#terms"\>Terms of Use</a\>`.|  
|TextblockUserRegistrationTermsProvided|Raadpleeg`<a data-toggle="modal" href="#" data-target="#terms"\>Terms of Use.</a\>`|  
|DialogHeadingTermsOfUse|Gebruiksvoorwaarden|  
|ValidationMessageConsentNotAccepted|U moet de gebruiksvoorwaarden accepteren voordat u verder kunt gaan.|  
  
###  <a name="SigninStrings"></a>SigninStrings  
  
|Naam|Tekst|  
|----------|----------|  
|WebAuthenticationForgotPassword|Wachtwoord vergeten?|  
|WebAuthenticationIfAdministrator|Als u een beheerder moet u zich aanmeldt `<a href="{0}"\>here</a\>`.|  
|WebAuthenticationNotAMember|Geen lid nog? `<a href="/signup"\>Sign up now</a\>`|  
|WebAuthenticationRemember|Mijn aanmeldingsgegevens onthouden op deze computer|  
|WebAuthenticationSigininWithPassword|Meld u aan met uw gebruikersnaam en wachtwoord|  
|WebAuthenticationSigninTitle|Aanmelden|  
|WebAuthenticationSignUpNow|Nu aanmelden|  
  
###  <a name="SignupStrings"></a>SignupStrings  
  
|Naam|Tekst|  
|----------|----------|  
|PageTitleSignup|Aanmelden|  
|WebAuthenticationAlreadyAMember|Bent u al lid?|  
|WebAuthenticationCreateNewAccount|Maak een nieuwe API Management-account|  
|WebAuthenticationSigninNow|Nu aanmelden|  
|ButtonLabelSignup|Aanmelden|  
  
###  <a name="SubscriptionListStrings"></a>SubscriptionListStrings  
  
|Naam|Tekst|  
|----------|----------|  
|SubscriptionCancelConfirmation|Weet u zeker dat u wilt dit abonnement annuleren?|  
|SubscriptionRenewConfirmation|Weet u zeker dat u dit abonnement te verlengen?|  
|WebDevelopersManageSubscriptions|Abonnementen beheren|  
|WebDevelopersPrimaryKey|Primaire sleutel|  
|WebDevelopersRegenerateLink|Opnieuw genereren|  
|WebDevelopersSecondaryKey|Secundaire sleutel|  
|ButtonLabelShowKey|Weergeven|  
|ButtonLabelRenewSubscription|Verlengen|  
|WebDevelopersSubscriptionReqested|Aangevraagd voor {0}|  
|WebDevelopersSubscriptionRequestedState|Aangevraagd|  
|WebDevelopersSubscriptionTableNameHeader|Naam|  
|WebDevelopersSubscriptionTableStateHeader|Status|  
|WebDevelopersUsageStatisticsLink|Analytics-rapporten|  
|WebDevelopersYourSubscriptions|Uw abonnementen|  
|SubscriptionPropertyLabelRequestedDate|Aangevraagde op|  
|SubscriptionPropertyLabelStartedDate|Begonnen op|  
|PageTitleRenameSubscription|Wijzig de naam van abonnement|  
|SubscriptionPropertyLabelName|De naam van abonnement|  
  
###  <a name="SubscriptionStrings"></a>SubscriptionStrings  
  
|Naam|Tekst|  
|----------|----------|  
|SectionHeadingCloseAccount|Zoek uw account wilt sluiten?|  
|PageTitleDeveloperProfile|Profiel|  
|ButtonLabelHideKey|Verbergen|  
|ButtonLabelRegenerateKey|Opnieuw genereren|  
|InformationMessageKeyWasRegenerated|Weet u zeker dat u wilt deze sleutel opnieuw genereren?|  
|ButtonLabelShowKey|Weergeven|  
  
###  <a name="UpdateProfileStrings"></a>UpdateProfileStrings  
  
|Naam|Tekst|  
|----------|----------|  
|ButtonLabelUpdateProfile|Profiel bijwerken|  
|PageTitleUpdateProfile|Accountgegevens bijwerken|  
  
###  <a name="UserProfile"></a>Gebruikersprofiel  
  
|Naam|Tekst|  
|----------|----------|  
|ButtonLabelChangeAccountInfo|Informatie over de account wijzigen|  
|ButtonLabelChangePassword|Wachtwoord wijzigen|  
|ButtonLabelCloseAccount|Account sluiten|  
|TextboxLabelEmail|E-mail|  
|TextboxLabelEmailFirstName|Voornaam|  
|TextboxLabelEmailLastName|Achternaam|  
|TextboxLabelNotificationsSenderEmail|Meldingen afzender e|  
|TextboxLabelOrganizationName|Naam van de organisatie|  
|SubscriptionStateActive|Actief|  
|SubscriptionStateCancelled|Geannuleerd|  
|SubscriptionStateExpired|Verlopen|  
|SubscriptionStateRejected|Afgewezen|  
|SubscriptionStateRequested|Aangevraagd|  
|SubscriptionStateSuspended|Onderbroken|  
|DefaultSubscriptionNameTemplate|{0} (standaard)|  
|SubscriptionNameTemplate|Ontwikkelaars toegang #{0}|  
|TextboxLabelSubscriptionName|De naam van abonnement|  
|ValidationMessageSubscriptionNameRequired|De abonnementsnaam van het mag niet leeg zijn.|  
|ApiManagementUserLimitReached|Deze service heeft het maximum aantal toegestane gebruikers bereikt. Upgrade naar een hogere prijscategorie.|  
  
##  <a name="glyphs"></a>De glyph-resources  
 Developer-portal API Management-sjablonen kunt de glyphs van [Glyphicons van Bootstrap](http://getbootstrap.com/components/#glyphicons). Deze reeks glyphs bevat meer dan 250 glyphs in lettertype-indeling van de [Glyphicon](http://glyphicons.com/) Halflings ingesteld. Gebruik de volgende syntaxis voor het gebruik van een glyph uit deze set.  
  
```html  
<span class="glyphicon glyphicon-user">  
```  
  
 Zie voor de volledige lijst van glyphs [Glyphicons van Bootstrap](http://getbootstrap.com/components/#glyphicons).

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het werken met sjablonen [het aanpassen van de API Management portal voor ontwikkelaars met behulp van sjablonen](api-management-developer-portal-templates.md).
