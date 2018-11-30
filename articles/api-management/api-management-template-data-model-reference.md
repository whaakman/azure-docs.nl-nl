---
title: Gegevens van Azure API Management-sjabloon reference model | Microsoft Docs
description: Meer informatie over de entiteit en type representaties voor algemene artikelen die worden gebruikt in de gegevensmodellen voor de sjablonen voor ontwikkelaarsportals in Azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: b0ad7e15-9519-4517-bb73-32e593ed6380
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: apimpm
ms.openlocfilehash: 8c21ed737cab98c9136e1c1991997ff3931a4c9d
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52447194"
---
# <a name="azure-api-management-template-data-model-reference"></a>Azure API Management-sjabloon naslaginformatie over gegevensmodellen
Dit onderwerp beschrijft de entiteits- en type representaties voor algemene artikelen die worden gebruikt in de gegevensmodellen voor de sjablonen voor ontwikkelaarsportals in Azure API Management.  
  
 Zie voor meer informatie over het werken met sjablonen [over het aanpassen van de API Management-ontwikkelaarsportal met behulp van sjablonen](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

De portal voor ontwikkelaars is niet beschikbaar in de laag verbruik.

## <a name="reference"></a>Referentie

-   [API](#API)  
-   [API-overzicht](#APISummary)  
-   [Toepassing](#Application)  
-   [Bijlage](#Attachment)  
-   [Voorbeeld van code](#Sample)  
-   [Opmerking](#Comment)  
-   [Filteren](#Filtering)  
-   [Header](#Header)  
-   [HTTP-aanvraag](#HTTPRequest)  
-   [HTTP-antwoord](#HTTPResponse)  
-   [Probleem](#Issue)  
-   [Bewerking](#Operation)  
-   [Bewerking menu](#Menu)  
-   [Bewerking menu-item](#MenuItem)  
-   [Wisselgeheugengebruik](#Paging)  
-   [Parameter](#Parameter)  
-   [Product](#Product)  
-   [Provider](#Provider)  
-   [Weergave](#Representation)  
-   [Abonnement](#Subscription)  
-   [Samenvatting abonnement](#SubscriptionSummary)  
-   [Gebruiker-accountgegevens](#UserAccountInfo)  
-   [Aanmelden van gebruikers](#UseSignIn)  
-   [Meld u aan gebruiker](#UserSignUp)  
  
##  <a name="API"></a> API  
 De `API` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|id|tekenreeks|Bron-id. Een unieke identificatie van de API in de huidige service-exemplaar van API Management. De waarde is een geldige relatieve URL in de indeling van `apis/{id}` waar `{id}` is een API-id. Deze eigenschap is alleen-lezen.|  
|naam|tekenreeks|Naam van de API. Mag niet leeg zijn. Maximale lengte is 100 tekens.|  
|description|tekenreeks|Beschrijving van de API. Mag niet leeg zijn. Kan HTML-opmaak tags bevatten. Maximale lengte is 1000 tekens.|  
|serviceUrl|tekenreeks|Absolute URL van de back-endservice uitvoering van deze API.|  
|pad|tekenreeks|Relatieve URL van deze API en alle van de resource-paden in de API Management service-exemplaar een unieke id. Deze wordt toegevoegd aan de API-eindpunt basis-URL opgegeven tijdens het maken van de service-exemplaar om te vormen van een openbare URL voor deze API.|  
|Protocollen|matrix van getal|Hierin wordt beschreven op welke protocollen de bewerkingen in deze API kunnen worden aangeroepen. Toegestane waarden zijn `1 - http` en `2 - https`, of beide.|  
|authenticationSettings|[Autorisatie-server-verificatie-instellingen](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#AuthenticationSettings)|Verzameling van verificatie-instellingen die zijn opgenomen in deze API.|  
|subscriptionKeyParameterNames|object|Optionele eigenschap die kan worden gebruikt om aangepaste namen voor de query en/of koptekst parameters met de abonnementssleutel te geven. Wanneer deze eigenschap aanwezig is, moet het ten minste één van de volgende twee eigenschappen bevatten.<br /><br /> `{   "subscriptionKeyParameterNames":   {     "query": “customQueryParameterName",     "header": “customHeaderParameterName"   } }`|  
  
##  <a name="APISummary"></a> API-overzicht  
 De `API summary` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|id|tekenreeks|Bron-id. Een unieke identificatie van de API in de huidige service-exemplaar van API Management. De waarde is een geldige relatieve URL in de indeling van `apis/{id}` waar `{id}` is een API-id. Deze eigenschap is alleen-lezen.|  
|naam|tekenreeks|Naam van de API. Mag niet leeg zijn. Maximale lengte is 100 tekens.|  
|description|tekenreeks|Beschrijving van de API. Mag niet leeg zijn. Kan HTML-opmaak tags bevatten. Maximale lengte is 1000 tekens.|  
  
##  <a name="Application"></a> Toepassing  
 De `application` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Id|tekenreeks|De unieke id van de toepassing.|  
|Titel|tekenreeks|De titel van de toepassing.|  
|Beschrijving|tekenreeks|De beschrijving van de toepassing.|  
|URL|URI|De URI voor de toepassing.|  
|Versie|tekenreeks|Versie-informatie voor de toepassing.|  
|Vereisten|tekenreeks|Een beschrijving van de vereisten voor de toepassing.|  
|Status|getal|De huidige status van de toepassing.<br /><br /> -0 - geregistreerd<br /><br /> -1 - verzonden<br /><br /> -2 - gepubliceerd<br /><br /> -3 - geweigerd<br /><br /> -4 - niet-gepubliceerde|  
|RegistrationDate|DateTime|De datum en tijd die de toepassing is geregistreerd.|  
|Categorie-id|getal|De categorie van de toepassing (financiën, entertainment, enz.)|  
|DeveloperId|tekenreeks|De unieke id van de ontwikkelaar die de aanvraag heeft ingediend.|  
|Bijlagen|Verzameling van [bijlage](#Attachment) entiteiten.|Eventuele bijlagen voor de toepassing, zoals schermafbeeldingen of pictogrammen.|  
|Pictogram|[Bijlage](#Attachment)|Het pictogram van de voor de toepassing.|  
  
##  <a name="Attachment"></a> Bijlage  
 De `attachment` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Unieke id|tekenreeks|De unieke id voor de bijlage.|  
|URL|tekenreeks|De URL van de resource.|  
|Type|tekenreeks|Het type van de bijlage.|  
|ContentType|tekenreeks|Het mediatype van de bijlage.|  
  
##  <a name="Sample"></a> Voorbeeld van code  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|titel|tekenreeks|De naam van de bewerking.|  
|codefragment|tekenreeks|Deze eigenschap is afgeschaft en mag niet worden gebruikt.|  
|Kwast|tekenreeks|Welke code syntaxiskleuren sjabloon moet worden gebruikt bij het weergeven van de voorbeeldcode. Toegestane waarden zijn `plain`, `php`, `java`, `xml`, `objc`, `python`, `ruby`, en `csharp`.|  
|sjabloon|tekenreeks|De naam van deze voorbeeldsjabloon code.|  
|hoofdtekst|tekenreeks|Een tijdelijke aanduiding voor het code-voorbeeld-gedeelte van het codefragment.|  
|method|tekenreeks|De HTTP-methode van de bewerking.|  
|Schema|tekenreeks|Het protocol moet worden gebruikt voor de aanvraag opnieuw.|  
|pad|tekenreeks|Het pad van de bewerking.|  
|query|tekenreeks|Voorbeeld van de query-tekenreeks met gedefinieerde parameters.|  
|host|tekenreeks|De URL van de gateway van de API Management-service voor de API die met deze bewerking.|  
|Headers|Verzameling van [Header](#Header) entiteiten.|Headers voor deze bewerking.|  
|parameters|Verzameling van [Parameter](#Parameter) entiteiten.|De parameters die zijn gedefinieerd voor deze bewerking.|  
  
##  <a name="Comment"></a> Opmerking  
 De `API` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Id|getal|De id van de opmerking.|  
|CommentText|tekenreeks|De hoofdtekst van de opmerking. HTML-code, omvat mogelijk.|  
|DeveloperCompany|tekenreeks|De naam van het bedrijf van de ontwikkelaar.|  
|PostedOn|DateTime|De datum en tijdstip waarop die de opmerking is gepost.|  
  
##  <a name="Issue"></a> Probleem  
 De `issue` entiteit heeft de volgende eigenschappen.  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Id|tekenreeks|De unieke id voor het probleem.|  
|ApiID|tekenreeks|De id voor de API waarvoor dit probleem is gerapporteerd.|  
|Titel|tekenreeks|De titel van het probleem.|  
|Beschrijving|tekenreeks|Beschrijving van het probleem.|  
|SubscriptionDeveloperName|tekenreeks|De voornaam van de ontwikkelaar die het probleem gerapporteerd.|  
|IssueState|tekenreeks|De huidige status van het probleem. Mogelijke waarden zijn voorgesteld, geopend en gesloten.|  
|ReportedOn|DateTime|De datum en tijd die het probleem is gerapporteerd.|  
|Opmerkingen|Verzameling van [Opmerking](#Comment) entiteiten.|Opmerkingen over dit probleem.|  
|Bijlagen|Verzameling van [bijlage](api-management-template-data-model-reference.md#Attachment) entiteiten.|Eventuele bijlagen aan het probleem.|  
|Services|Verzameling van [API](#API) entiteiten.|De API's die zijn geabonneerd op door de gebruiker die het probleem wordt ingediend.|  
  
##  <a name="Filtering"></a> Filteren  
 De `filtering` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Patroon|tekenreeks|De zoekterm; of `null` als er geen zoekterm.|  
|Tijdelijke aanduiding|tekenreeks|De tekst die moet worden weergegeven in het zoekvak wanneer er geen zoekterm opgegeven.|  
  
##  <a name="Header"></a> Koptekst  
 Deze sectie beschrijft de `parameter` weergave.  
  
|Eigenschap|Beschrijving|Type|  
|--------------|-----------------|----------|  
|naam|tekenreeks|Parameternaam.|  
|description|tekenreeks|Beschrijving van de parameter.|  
|waarde|tekenreeks|Headerwaarde.|  
|typeName|tekenreeks|Het gegevenstype van de header-waarde.|  
|opties|tekenreeks|Opties.|  
|vereist|booleaans|Of de header is vereist.|  
|Alleen-lezen|booleaans|Of de header is alleen-lezen.|  
  
##  <a name="HTTPRequest"></a> HTTP-aanvraag  
 Deze sectie beschrijft de `request` weergave.  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|description|tekenreeks|Beschrijving voor de aanvraag opnieuw.|  
|Headers|matrix van [Header](#Header) entiteiten.|Aanvraagheaders.|  
|parameters|matrix van [Parameter](#Parameter)|Verzameling van de parameters van de aanvraag opnieuw.|  
|opmerkingen|matrix van [weergave](#Representation)|Verzameling van bewerking aanvraag representaties.|  
  
##  <a name="HTTPResponse"></a> HTTP-antwoord  
 Deze sectie beschrijft de `response` weergave.  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|statusCode|positief geheel getal zijn|Bewerking antwoordstatuscode.|  
|description|tekenreeks|Beschrijving van de reactie bewerking.|  
|opmerkingen|matrix van [weergave](#Representation)|Verzameling van bewerking antwoord representaties.|  
  
##  <a name="Operation"></a> Bewerking  
 De `operation` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|id|tekenreeks|Bron-id. Unieke identificatie van de bewerking binnen de huidige service-exemplaar van API Management. De waarde is een geldige relatieve URL in de indeling van `apis/{aid}/operations/{id}` waar `{aid}` is een API-id en `{id}` is een bewerking-id. Deze eigenschap is alleen-lezen.|  
|naam|tekenreeks|Naam van de bewerking. Mag niet leeg zijn. Maximale lengte is 100 tekens.|  
|description|tekenreeks|Beschrijving van de bewerking. Mag niet leeg zijn. Kan HTML-opmaak tags bevatten. Maximale lengte is 1000 tekens.|  
|Schema|tekenreeks|Hierin wordt beschreven op welke protocollen de bewerkingen in deze API kunnen worden aangeroepen. Toegestane waarden zijn `http`, `https`, of beide `http` en `https`.|  
|uriTemplate|tekenreeks|Relatieve URL-sjabloon voor het identificeren van de doelresource voor deze bewerking. Parameters kunnen worden opgenomen. Voorbeeld: `customers/{cid}/orders/{oid}/?date={date}`|  
|host|tekenreeks|De URL van de API Management-gateway die als host fungeert voor de API.|  
|HttpMethod|tekenreeks|HTTP-bewerkingsmethode.|  
|aanvraag|[HTTP-aanvraag](#HTTPRequest)|Een entiteit met details van de aanvraag.|  
|Antwoorden|matrix van [HTTP-antwoord](#HTTPResponse)|Matrix van bewerking [HTTP-antwoord](#HTTPResponse) entiteiten.|  
  
##  <a name="Menu"></a> Bewerking menu  
 De `operation menu` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|apiId|tekenreeks|De id van de huidige API.|  
|CurrentOperationId|tekenreeks|De id van de huidige bewerking.|  
|Bewerking|tekenreeks|Het Menutype.|  
|Vlastnost MenuItems|Verzameling van [bewerking menu-item](#MenuItem) entiteiten.|De bewerkingen voor de huidige API.|  
  
##  <a name="MenuItem"></a> Bewerking menu-item  
 De `operation menu item` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Id|tekenreeks|De id van de bewerking.|  
|Titel|tekenreeks|De beschrijving van de bewerking.|  
|HttpMethod|tekenreeks|De Http-methode van de bewerking.|  
  
##  <a name="Paging"></a> Wisselgeheugengebruik  
 De `paging` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Pagina|getal|Het huidige paginanummer.|  
|PageSize|getal|Het maximum aantal resultaten dat moet worden weergegeven op één pagina.|  
|TotalItemCount|getal|Het aantal items om weer te geven.|  
|ShowAll|booleaans|Of u wilt weergeven, alle resultaten worden op één pagina.|  
|PageCount|getal|Het aantal pagina's van resultaten.|  
  
##  <a name="Parameter"></a> Parameter  
 Deze sectie beschrijft de `parameter` weergave.  
  
|Eigenschap|Beschrijving|Type|  
|--------------|-----------------|----------|  
|naam|tekenreeks|Parameternaam.|  
|description|tekenreeks|Beschrijving van de parameter.|  
|waarde|tekenreeks|Parameterwaarde.|  
|opties|tekenreeksmatrix|Waarden die zijn gedefinieerd voor waarden van queryparameter.|  
|vereist|booleaans|Hiermee geeft u op of parameter vereist of niet is.|  
|type|getal|Deze parameter is of een padparameter (1) of een queryreeksparameter (2).|  
|typeName|tekenreeks|Het parametertype.|  
  
##  <a name="Product"></a> Product  
 De `product` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Id|tekenreeks|Bron-id. Unieke identificatie van het product in de huidige service-exemplaar van API Management. De waarde is een geldige relatieve URL in de indeling van `products/{pid}` waar `{pid}` is een product-id. Deze eigenschap is alleen-lezen.|  
|Titel|tekenreeks|De naam van het product. Mag niet leeg zijn. Maximale lengte is 100 tekens.|  
|Beschrijving|tekenreeks|Beschrijving van het product. Mag niet leeg zijn. Kan HTML-opmaak tags bevatten. Maximale lengte is 1000 tekens.|  
|Voorwaarden|tekenreeks|Product gebruiksvoorwaarden. Ontwikkelaars proberen om u te abonneren op het product worden gepresenteerd en moeten deze voorwaarden accepteren voordat ze het abonnement kunnen voltooien.|  
|ProductState|getal|Hiermee geeft u op of het product is gepubliceerd of niet. Gepubliceerde producten zijn kunnen worden gedetecteerd door ontwikkelaars op de portal voor ontwikkelaars. Niet-gepubliceerde producten zijn alleen zichtbaar voor beheerders.<br /><br /> De toegestane waarden voor de Productstatus van het zijn:<br /><br /> - `0 - Not Published`<br /><br /> - `1 - Published`<br /><br /> - `2 - Deleted`|  
|AllowMultipleSubscriptions|booleaans|Hiermee geeft u op of een gebruiker meerdere abonnementen voor dit product op hetzelfde moment hebben kan.|  
|MultipleSubscriptionsCount|getal|Maximum aantal abonnementen op dit product een gebruiker mag hebben op hetzelfde moment.|  
  
##  <a name="Provider"></a> Provider  
 De `provider` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Eigenschappen|tekenreeks-woordenlijst|Eigenschappen voor deze verificatieprovider.|  
|AuthenticationType|tekenreeks|Het providertype. (Azure Active Directory, Facebook-aanmelding, Google-Account, Microsoft-Account, Twitter).|  
|Bijschrift|tekenreeks|Naam van de provider weergeven.|  
  
##  <a name="Representation"></a> Weergave  
 Deze sectie beschrijft een `representation`.  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|contentType|tekenreeks|Hiermee geeft u een geregistreerde of aangepaste inhoudstype voor deze weergave, bijvoorbeeld `application/xml`.|  
|Voorbeeld|tekenreeks|Een voorbeeld van de weergave.|  
  
##  <a name="Subscription"></a> Abonnement  
 De `subscription` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Id|tekenreeks|Bron-id. Unieke identificatie van het abonnement in het huidige exemplaar van API Management-service. De waarde is een geldige relatieve URL in de indeling van `subscriptions/{sid}` waar `{sid}` is een abonnements-id. Deze eigenschap is alleen-lezen.|  
|Product-id|tekenreeks|De resource-id voor het product van het product geabonneerd. De waarde is een geldige relatieve URL in de indeling van `products/{pid}` waar `{pid}` is een product-id.|  
|ProductTitle|tekenreeks|De naam van het product. Mag niet leeg zijn. Maximale lengte is 100 tekens.|  
|ProductDescription|tekenreeks|Beschrijving van het product. Mag niet leeg zijn. Kan HTML-opmaak tags bevatten. Maximale lengte is 1000 tekens.|  
|ProductDetailsUrl|tekenreeks|Relatieve URL naar de details van het product.|  
|state|tekenreeks|De status van het abonnement. Mogelijke statussen zijn:<br /><br /> - `0 - suspended` : het abonnement is geblokkeerd en API's van het product kan niet worden aangeroepen door de abonnee.<br /><br /> - `1 - active` : het abonnement actief is.<br /><br /> - `2 - expired` : het abonnement is bereikt de vervaldatum en is gedeactiveerd.<br /><br /> - `3 - submitted` – de Abonnementaanvraag is gemaakt door de ontwikkelaar, maar is nog niet is goedgekeurd of afgewezen.<br /><br /> - `4 - rejected` – de Abonnementaanvraag is afgewezen door een beheerder.<br /><br /> - `5 - cancelled` : het abonnement is geannuleerd door de ontwikkelaar of beheerder.|  
|Weergavenaam|tekenreeks|Weergavenaam van het abonnement.|  
|CreatedDate|Datum/tijd|De datum waarop het abonnement is gemaakt, in ISO 8601-notatie: `2014-06-24T16:25:00Z`.|  
|CanBeCancelled|booleaans|U kunt het abonnement of annuleren door de huidige gebruiker.|  
|IsAwaitingApproval|booleaans|Of het abonnement is in afwachting van goedkeuring.|  
|Begindatum|Datum/tijd|De begindatum voor het abonnement, in ISO 8601-notatie: `2014-06-24T16:25:00Z`.|  
|ExpirationDate|Datum/tijd|De vervaldatum voor het abonnement, in ISO 8601-notatie: `2014-06-24T16:25:00Z`.|  
|NotificationDate|Datum/tijd|De datum van de melding voor het abonnement, in ISO 8601-notatie: `2014-06-24T16:25:00Z`.|  
|primaryKey|tekenreeks|De primaire abonnementssleutel. Maximale lengte is 256 tekens.|  
|secundaire sleutel|tekenreeks|De secundaire abonnementssleutel. Maximale lengte is 256 tekens.|  
|CanBeRenewed|booleaans|Of het abonnement kan worden vernieuwd door de huidige gebruiker.|  
|HasExpired|booleaans|Of het abonnement is verlopen.|  
|IsRejected|booleaans|Of de Abonnementaanvraag is geweigerd.|  
|cancelUrl|tekenreeks|De relatieve Url voor het abonnement hebt geannuleerd.|  
|RenewUrl|tekenreeks|De relatieve Url voor het abonnement te verlengen.|  
  
##  <a name="SubscriptionSummary"></a> Samenvatting abonnement  
 De `subscription summary` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Id|tekenreeks|Bron-id. Unieke identificatie van het abonnement in het huidige exemplaar van API Management-service. De waarde is een geldige relatieve URL in de indeling van `subscriptions/{sid}` waar `{sid}` is een abonnements-id. Deze eigenschap is alleen-lezen.|  
|Weergavenaam|tekenreeks|De weergavenaam van het abonnement|  
  
##  <a name="UserAccountInfo"></a> Gebruiker-accountgegevens  
 De `user account info` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|FirstName|tekenreeks|De voornaam. Mag niet leeg zijn. Maximale lengte is 100 tekens.|  
|LastName|tekenreeks|De achternaam. Mag niet leeg zijn. Maximale lengte is 100 tekens.|  
|Email|tekenreeks|E-mailadres. Mag niet leeg zijn en moet uniek zijn binnen het service-exemplaar. Maximale lengte is 254 tekens.|  
|Wachtwoord|tekenreeks|Het wachtwoord voor gebruikersaccount.|  
|NameIdentifier|tekenreeks|Account-id, gelijk zijn aan het e-mailadres voor de gebruiker kan worden gebruikt.|  
|ProviderName|tekenreeks|Providernaam van verificatie.|  
|IsBasicAccount|booleaans|Waar als dit account is geregistreerd met behulp van e-mailadres en wachtwoord; ONWAAR als het account is geregistreerd met behulp van een provider.|  
  
##  <a name="UseSignIn"></a> Aanmelding door een gebruiker in  
 De `user sign in` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Email|tekenreeks|E-mailadres. Mag niet leeg zijn en moet uniek zijn binnen het service-exemplaar. Maximale lengte is 254 tekens.|  
|Wachtwoord|tekenreeks|Het wachtwoord voor gebruikersaccount.|  
|ReturnUrl|tekenreeks|Meld u aan de URL van de pagina waar de gebruiker heeft geklikt.|  
|RememberMe|booleaans|Hiermee geeft u op of u de huidige gebruiker gegevens op te slaan.|  
|RegistrationEnabled|booleaans|Of de registratie is ingeschakeld.|  
|DelegationEnabled|booleaans|Of gedelegeerde aanmelden is ingeschakeld.|  
|DelegationUrl|tekenreeks|De gedelegeerde aanmelding url als ingeschakeld.|  
|SsoSignUpUrl|tekenreeks|De eenmalige aanmelding URL voor de gebruiker, indien aanwezig.|  
|AuxServiceUrl|tekenreeks|Als de huidige gebruiker een beheerder is, is dit een koppeling naar het service-exemplaar in de Azure-portal.|  
|Providers|Verzameling van [Provider](#Provider) entiteiten|De verificatieproviders voor deze gebruiker.|  
|UserRegistrationTerms|tekenreeks|Termen die een gebruiker instemmen moet voordat u zich aanmeldt.|  
|UserRegistrationTermsEnabled|booleaans|Of voorwaarden zijn ingeschakeld.|  
  
##  <a name="UserSignUp"></a> Gebruiker aanmelden  
 De `user sign up` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|PasswordConfirm|booleaans|Waarde die wordt gebruikt door de [aanmelding](api-management-page-controls.md#sign-up)Meld u aan besturingselement.|  
|Wachtwoord|tekenreeks|Het wachtwoord voor gebruikersaccount.|  
|PasswordVerdictLevel|getal|Waarde die wordt gebruikt door de [aanmelding](api-management-page-controls.md#sign-up)Meld u aan besturingselement.|  
|UserRegistrationTerms|tekenreeks|Termen die een gebruiker instemmen moet voordat u zich aanmeldt.|  
|UserRegistrationTermsOptions|getal|Waarde die wordt gebruikt door de [aanmelding](api-management-page-controls.md#sign-up)Meld u aan besturingselement.|  
|ConsentAccepted|booleaans|Waarde die wordt gebruikt door de [aanmelding](api-management-page-controls.md#sign-up)Meld u aan besturingselement.|  
|Email|tekenreeks|E-mailadres. Mag niet leeg zijn en moet uniek zijn binnen het service-exemplaar. Maximale lengte is 254 tekens.|  
|FirstName|tekenreeks|De voornaam. Mag niet leeg zijn. Maximale lengte is 100 tekens.|  
|LastName|tekenreeks|De achternaam. Mag niet leeg zijn. Maximale lengte is 100 tekens.|  
|UserData|tekenreeks|Waarde die wordt gebruikt door de [aanmelding](api-management-page-controls.md#sign-up) besturingselement.|  
|NameIdentifier|tekenreeks|Waarde die wordt gebruikt door de [aanmelding](api-management-page-controls.md#sign-up)Meld u aan besturingselement.|  
|ProviderName|tekenreeks|Providernaam van verificatie.|

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het werken met sjablonen [over het aanpassen van de API Management-ontwikkelaarsportal met behulp van sjablonen](api-management-developer-portal-templates.md).
