---
title: Azure API Management-sjabloon gegevensmodel verwijzing | Microsoft Docs
description: Meer informatie over de entiteit en type representaties voor algemene items in de gegevensmodellen gebruikt voor de developer portal sjablonen in Azure API Management.
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: b0ad7e15-9519-4517-bb73-32e593ed6380
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: apimpm
ms.openlocfilehash: 92a52910ef8b9f078a8ea6ac409f89719f889b24
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="azure-api-management-template-data-model-reference"></a>Azure API Management data model verwijzing naar de sjabloon
Dit onderwerp beschrijft de entiteit en type representaties voor algemene items in de gegevensmodellen gebruikt voor de developer portal sjablonen in Azure API Management.  
  
 Zie voor meer informatie over het werken met sjablonen [het aanpassen van de API Management portal voor ontwikkelaars met behulp van sjablonen](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  
  
-   [API](#API)  
-   [API-overzicht](#APISummary)  
-   [Toepassing](#Application)  
-   [Bijlage](#Attachment)  
-   [Voorbeeld van code](#Sample)  
-   [Opmerking](#Comment)  
-   [Filteren](#Filtering)  
-   [Koptekst](#Header)  
-   [HTTP-aanvraag](#HTTPRequest)  
-   [HTTP-antwoord](#HTTPResponse)  
-   [Probleem](#Issue)  
-   [Bewerking](#Operation)  
-   [Bewerking menu](#Menu)  
-   [Bewerking menu-item](#MenuItem)  
-   [Paginering](#Paging)  
-   [Parameter](#Parameter)  
-   [Product](#Product)  
-   [Provider](#Provider)  
-   [Weergave](#Representation)  
-   [Abonnement](#Subscription)  
-   [Samenvatting van abonnement](#SubscriptionSummary)  
-   [Gebruikersaccountgegevens](#UserAccountInfo)  
-   [Gebruiker aanmelden](#UseSignIn)  
-   [Meld u aan gebruiker](#UserSignUp)  
  
##  <a name="API"></a>API  
 De `API` entiteit heeft de volgende eigenschappen.  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|id|Tekenreeks|Bron-id. Een unieke identificatie van de API binnen het huidige exemplaar van API Management-service. De waarde is een geldige relatieve URL in de indeling van `apis/{id}` waar `{id}` is een API-id. Deze eigenschap is alleen-lezen.|  
|naam|Tekenreeks|De naam van de API. Mag niet leeg zijn. Maximale lengte is 100 tekens.|  
|description|Tekenreeks|Beschrijving van de API. Mag niet leeg zijn. Kan HTML-opmaak labels bevatten. Maximumlengte is 1000 tekens.|  
|serviceUrl|Tekenreeks|Absolute URL van de back-endservice uitvoering van deze API.|  
|pad|Tekenreeks|Relatieve URL is een unieke id van deze API en alle bijbehorende paden resource binnen het service-exemplaar van API Management. Deze wordt toegevoegd aan de API-eindpunt basis-URL opgegeven tijdens het maken van de service-exemplaar om een openbare URL voor deze API.|  
|Protocollen|matrix van getal|Hierin wordt beschreven op welke protocollen de bewerkingen in deze API kunnen worden aangeroepen. Toegestane waarden zijn `1 - http` en `2 - https`, of beide.|  
|authenticationSettings|[Autorisatie-instellingen voor server-verificatie](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#AuthenticationSettings)|Verzameling van verificatie-instellingen die zijn opgenomen in deze API.|  
|subscriptionKeyParameterNames|object|Optionele eigenschap die kan worden gebruikt om aangepaste namen voor de query en/of koptekst parameters met de abonnementssleutel te geven. Als deze eigenschap aanwezig is moet er ten minste één van de volgende twee eigenschappen bevatten.<br /><br /> `{   "subscriptionKeyParameterNames":   {     "query": “customQueryParameterName",     "header": “customHeaderParameterName"   } }`|  
  
##  <a name="APISummary"></a>API-overzicht  
 De `API summary` entiteit heeft de volgende eigenschappen.  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|id|Tekenreeks|Bron-id. Een unieke identificatie van de API binnen het huidige exemplaar van API Management-service. De waarde is een geldige relatieve URL in de indeling van `apis/{id}` waar `{id}` is een API-id. Deze eigenschap is alleen-lezen.|  
|naam|Tekenreeks|De naam van de API. Mag niet leeg zijn. Maximale lengte is 100 tekens.|  
|description|Tekenreeks|Beschrijving van de API. Mag niet leeg zijn. Kan HTML-opmaak labels bevatten. Maximumlengte is 1000 tekens.|  
  
##  <a name="Application"></a>Toepassing  
 De `application` entiteit heeft de volgende eigenschappen.  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Id|Tekenreeks|De unieke id van de toepassing.|  
|Titel|Tekenreeks|De titel van de toepassing.|  
|Beschrijving|Tekenreeks|De beschrijving van de toepassing.|  
|URL|URI|De URI voor de toepassing.|  
|Versie|Tekenreeks|Versie-informatie voor de toepassing.|  
|Vereisten|Tekenreeks|Een beschrijving van de vereisten voor de toepassing.|  
|Status|nummer|De huidige status van de toepassing.<br /><br /> -0 - geregistreerd<br /><br /> -1 - verzonden<br /><br /> -2 - gepubliceerd<br /><br /> -3 - geweigerd<br /><br /> -4 - niet gepubliceerd|  
|RegistrationDate|Datum en tijd|De datum en tijd die de toepassing is geregistreerd.|  
|Categorie-id|nummer|De categorie van de toepassing (financiën, entertainment, enz.)|  
|DeveloperId|Tekenreeks|De unieke id van de ontwikkelaar die de aanvraag heeft ingediend.|  
|Bijlagen|Verzameling van [bijlage](#Attachment) entiteiten.|Eventuele bijlagen voor de toepassing zoals schermafbeeldingen of pictogrammen.|  
|Pictogram|[Bijlage](#Attachment)|Het pictogram de voor de toepassing.|  
  
##  <a name="Attachment"></a>Bijlage  
 De `attachment` entiteit heeft de volgende eigenschappen.  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Unieke id|Tekenreeks|De unieke id voor de bijlage.|  
|URL|Tekenreeks|De URL van de resource.|  
|Type|Tekenreeks|Het type van de bijlage.|  
|ContentType|Tekenreeks|Het mediatype van de bijlage.|  
  
##  <a name="Sample"></a>Voorbeeld van code  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|titel|Tekenreeks|De naam van de bewerking.|  
|codefragment|Tekenreeks|Deze eigenschap is afgeschaft en mag niet worden gebruikt.|  
|kwast|Tekenreeks|Welke codesyntaxis sjabloon moet worden gebruikt bij het weergeven van de voorbeeldcode van de kleuren. Toegestane waarden zijn `plain`, `php`, `java`, `xml`, `objc`, `python`, `ruby`, en `csharp`.|  
|sjabloon|Tekenreeks|De naam van deze voorbeeldsjabloon code.|  
|hoofdtekst|Tekenreeks|Een tijdelijke aanduiding voor het gedeelte voor het voorbeeld van code van het fragment.|  
|Methode|Tekenreeks|De HTTP-methode van de bewerking.|  
|Schema|Tekenreeks|Het protocol moet worden gebruikt voor de aanvraag opnieuw.|  
|pad|Tekenreeks|Het pad van de bewerking.|  
|query|Tekenreeks|Voorbeeld van de query-tekenreeks met gedefinieerde parameters.|  
|host|Tekenreeks|De URL van de gateway van API Management-service voor de API met deze bewerking.|  
|headers|Verzameling van [Header](#Header) entiteiten.|Headers voor deze bewerking.|  
|parameters|Verzameling van [Parameter](#Parameter) entiteiten.|De parameters die zijn gedefinieerd voor deze bewerking.|  
  
##  <a name="Comment"></a>Opmerking  
 De `API` entiteit heeft de volgende eigenschappen.  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Id|nummer|De id van de opmerking.|  
|CommentText|Tekenreeks|De hoofdtekst van de opmerking. Kan HTML bevatten.|  
|DeveloperCompany|Tekenreeks|De naam van het bedrijf van de ontwikkelaar.|  
|PostedOn|Datum en tijd|De datum en tijdstip waarop die de opmerking is gepost.|  
  
##  <a name="Issue"></a>Probleem  
 De `issue` entiteit heeft de volgende eigenschappen.  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Id|Tekenreeks|De unieke id voor het probleem.|  
|ApiID|Tekenreeks|De id voor de API waarvoor dit probleem is gerapporteerd.|  
|Titel|Tekenreeks|De titel van het probleem.|  
|Beschrijving|Tekenreeks|Beschrijving van het probleem.|  
|SubscriptionDeveloperName|Tekenreeks|De voornaam van de ontwikkelaar die het probleem gerapporteerd.|  
|IssueState|Tekenreeks|De huidige status van het probleem. Mogelijke waarden zijn voorgesteld, Opened, gesloten.|  
|ReportedOn|Datum en tijd|De datum en tijd die het probleem is opgetreden.|  
|Opmerkingen|Verzameling van [Opmerking](#Comment) entiteiten.|Opmerkingen over dit probleem.|  
|Bijlagen|Verzameling van [bijlage](api-management-template-data-model-reference.md#Attachment) entiteiten.|Eventuele bijlagen aan het probleem.|  
|Services|Verzameling van [API](#API) entiteiten.|De API's die zijn geabonneerd op door de gebruiker die het probleem wordt ingediend.|  
  
##  <a name="Filtering"></a>Filteren  
 De `filtering` entiteit heeft de volgende eigenschappen.  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Patroon|Tekenreeks|De huidige zoekterm; of `null` als er geen zoekterm.|  
|Tijdelijke aanduiding voor|Tekenreeks|De tekst die moet worden weergegeven in het zoekvak als er geen zoekterm opgegeven.|  
  
##  <a name="Header"></a>Koptekst  
 Deze sectie beschrijft de `parameter` weergave.  
  
|Eigenschap|Beschrijving|Type|  
|--------------|-----------------|----------|  
|naam|Tekenreeks|Parameternaam.|  
|description|Tekenreeks|Beschrijving van de parameter.|  
|waarde|Tekenreeks|Headerwaarde.|  
|TypeName|Tekenreeks|Het gegevenstype van headerwaarde.|  
|opties|Tekenreeks|Opties.|  
|Vereist|booleaans|Hiermee wordt aangegeven of de header is vereist.|  
|Alleen-lezen|booleaans|Hiermee wordt aangegeven of de header is alleen-lezen.|  
  
##  <a name="HTTPRequest"></a>HTTP-aanvraag  
 Deze sectie beschrijft de `request` weergave.  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|description|Tekenreeks|Beschrijving van de aanvraag opnieuw.|  
|headers|matrix van [Header](#Header) entiteiten.|Aanvraagheaders.|  
|parameters|matrix van [Parameter](#Parameter)|Verzameling parameters van de aanvraag opnieuw.|  
|verklaringen|matrix van [weergave](#Representation)|Verzameling van bewerking aanvraag verklaringen.|  
  
##  <a name="HTTPResponse"></a>HTTP-antwoord  
 Deze sectie beschrijft de `response` weergave.  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|statusCode|Positief geheel getal|Code voor antwoordstatus bewerking.|  
|description|Tekenreeks|Beschrijving van de reactie bewerking.|  
|verklaringen|matrix van [weergave](#Representation)|Verzameling van bewerking antwoord verklaringen.|  
  
##  <a name="Operation"></a>Bewerking  
 De `operation` entiteit heeft de volgende eigenschappen.  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|id|Tekenreeks|Bron-id. Een unieke identificatie van de bewerking binnen het huidige exemplaar van API Management-service. De waarde is een geldige relatieve URL in de indeling van `apis/{aid}/operations/{id}` waar `{aid}` is een API-id en `{id}` is een bewerking-id. Deze eigenschap is alleen-lezen.|  
|naam|Tekenreeks|De naam van de bewerking. Mag niet leeg zijn. Maximale lengte is 100 tekens.|  
|description|Tekenreeks|Beschrijving van de bewerking. Mag niet leeg zijn. Kan HTML-opmaak labels bevatten. Maximumlengte is 1000 tekens.|  
|Schema|Tekenreeks|Hierin wordt beschreven op welke protocollen de bewerkingen in deze API kunnen worden aangeroepen. Toegestane waarden zijn `http`, `https`, of beide `http` en `https`.|  
|uriTemplate|Tekenreeks|Relatieve URL sjabloon identificeren van de doelbron voor deze bewerking. Kan parameters bevatten. Voorbeeld:`customers/{cid}/orders/{oid}/?date={date}`|  
|host|Tekenreeks|De URL van de API Management-gateway die als host fungeert voor de API.|  
|HttpMethod|Tekenreeks|HTTP-bewerkingsmethode.|  
|Aanvraag|[HTTP-aanvraag](#HTTPRequest)|Een entiteit die gegevens voor de aanvraag bevat.|  
|antwoorden|matrix van [HTTP-antwoord](#HTTPResponse)|Matrix van bewerking [HTTP-antwoord](#HTTPResponse) entiteiten.|  
  
##  <a name="Menu"></a>Bewerking menu  
 De `operation menu` entiteit heeft de volgende eigenschappen.  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|apiId|Tekenreeks|De id van de huidige API.|  
|CurrentOperationId|Tekenreeks|De id van de huidige bewerking.|  
|Actie|Tekenreeks|Het Menutype.|  
|MenuItems|Verzameling van [bewerking menuopdracht](#MenuItem) entiteiten.|De bewerkingen voor de huidige API.|  
  
##  <a name="MenuItem"></a>Bewerking menu-item  
 De `operation menu item` entiteit heeft de volgende eigenschappen.  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Id|Tekenreeks|De id van de bewerking.|  
|Titel|Tekenreeks|De beschrijving van de bewerking.|  
|HttpMethod|Tekenreeks|De Http-methode van de bewerking.|  
  
##  <a name="Paging"></a>Paginering  
 De `paging` entiteit heeft de volgende eigenschappen.  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Pagina|nummer|Het huidige paginanummer.|  
|PageSize|nummer|Het maximum aantal resultaten op één pagina moet worden weergegeven.|  
|TotalItemCount|nummer|Het aantal items om weer te geven.|  
|ShowAll|booleaans|Of u wilt weergeven, alle resultaten worden op één pagina.|  
|PageCount|nummer|Het aantal pagina's met resultaten.|  
  
##  <a name="Parameter"></a>Parameter  
 Deze sectie beschrijft de `parameter` weergave.  
  
|Eigenschap|Beschrijving|Type|  
|--------------|-----------------|----------|  
|naam|Tekenreeks|Parameternaam.|  
|description|Tekenreeks|Beschrijving van de parameter.|  
|waarde|Tekenreeks|Waarde voor parameter.|  
|opties|Matrix van tekenreeks|Waarden die zijn gedefinieerd voor parameterwaarden.|  
|Vereist|booleaans|Hiermee geeft u op of de parameter vereist is.|  
|type|nummer|Hiermee wordt aangegeven of deze parameter is een padparameter (1) of een parameter querystring (2).|  
|TypeName|Tekenreeks|Het parametertype.|  
  
##  <a name="Product"></a>Product  
 De `product` entiteit heeft de volgende eigenschappen.  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Id|Tekenreeks|Bron-id. Een unieke identificatie van het product binnen het huidige exemplaar van API Management-service. De waarde is een geldige relatieve URL in de indeling van `products/{pid}` waar `{pid}` is een product-id. Deze eigenschap is alleen-lezen.|  
|Titel|Tekenreeks|De naam van het product. Mag niet leeg zijn. Maximale lengte is 100 tekens.|  
|Beschrijving|Tekenreeks|Beschrijving van het product. Mag niet leeg zijn. Kan HTML-opmaak labels bevatten. Maximumlengte is 1000 tekens.|  
|Voorwaarden|Tekenreeks|Product gebruiksvoorwaarden. Ontwikkelaars wilt abonneren op het product wordt weergegeven en moeten deze voorwaarden accepteren voordat ze het abonnement proces kunnen voltooien.|  
|ProductState|nummer|Hiermee geeft u op of het product of niet is gepubliceerd. Gepubliceerde producten kunnen worden gevonden door ontwikkelaars op de portal voor ontwikkelaars. Niet-gepubliceerde producten zijn alleen zichtbaar voor beheerders.<br /><br /> De toegestane waarden voor de Productstatus zijn:<br /><br /> - `0 - Not Published`<br /><br /> - `1 - Published`<br /><br /> - `2 - Deleted`|  
|AllowMultipleSubscriptions|booleaans|Hiermee geeft u op of een gebruiker meerdere abonnementen voor dit product op hetzelfde moment kan hebben.|  
|MultipleSubscriptionsCount|nummer|Het aantal abonnementen voor dit product door de huidige gebruiker.|  
  
##  <a name="Provider"></a>Provider  
 De `provider` entiteit heeft de volgende eigenschappen.  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Eigenschappen|tekenreeks woordenlijst|Eigenschappen voor deze verificatieprovider.|  
|authenticationType|Tekenreeks|Het providertype. (Azure Active Directory, Facebook-aanmelding, Google-Account, Microsoft-Account, Twitter).|  
|Bijschrift|Tekenreeks|Weergavenaam van de provider.|  
  
##  <a name="Representation"></a>Weergave  
 Deze sectie beschrijft een `representation`.  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|ContentType|Tekenreeks|Hiermee geeft u een geregistreerde of aangepaste inhoudstype voor deze weergave bijvoorbeeld `application/xml`.|  
|voorbeeld|Tekenreeks|Een voorbeeld van de weergave.|  
  
##  <a name="Subscription"></a>Abonnement  
 De `subscription` entiteit heeft de volgende eigenschappen.  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Id|Tekenreeks|Bron-id. Een unieke identificatie van het abonnement binnen het huidige exemplaar van API Management-service. De waarde is een geldige relatieve URL in de indeling van `subscriptions/{sid}` waar `{sid}` is de abonnement-id. Deze eigenschap is alleen-lezen.|  
|Product-id|Tekenreeks|De bron-id voor het product van het product geabonneerd. De waarde is een geldige relatieve URL in de indeling van `products/{pid}` waar `{pid}` is een product-id.|  
|ProductTitle|Tekenreeks|De naam van het product. Mag niet leeg zijn. Maximale lengte is 100 tekens.|  
|ProductDescription|Tekenreeks|Beschrijving van het product. Mag niet leeg zijn. Kan HTML-opmaak labels bevatten. Maximumlengte is 1000 tekens.|  
|ProductDetailsUrl|Tekenreeks|Relatieve URL naar de details van het product.|  
|state|Tekenreeks|De status van het abonnement. Mogelijke statussen zijn:<br /><br /> - `0 - suspended`– het abonnement is geblokkeerd en de abonnee kan API's van het product niet aanroepen.<br /><br /> - `1 - active`– het abonnement actief is.<br /><br /> - `2 - expired`– het abonnement is bereikt de verloopdatum en is gedeactiveerd.<br /><br /> - `3 - submitted`– de Abonnementaanvraag is gemaakt door de ontwikkelaar, maar is nog niet goedgekeurd of afgewezen.<br /><br /> - `4 - rejected`– de Abonnementaanvraag is geweigerd door een beheerder.<br /><br /> - `5 - cancelled`– het abonnement is geannuleerd door de ontwikkelaar of beheerder.|  
|Weergavenaam|Tekenreeks|Weergavenaam van het abonnement.|  
|CreatedDate|Datum/tijd|De datum waarop het abonnement is gemaakt, in de ISO 8601-notatie: `2014-06-24T16:25:00Z`.|  
|CanBeCancelled|booleaans|Hiermee wordt aangegeven of het abonnement kan worden geannuleerd door de huidige gebruiker.|  
|IsAwaitingApproval|booleaans|Hiermee wordt aangegeven of het abonnement wacht op goedkeuring.|  
|Begindatum|Datum/tijd|De begindatum voor het abonnement, in de ISO 8601-notatie: `2014-06-24T16:25:00Z`.|  
|ExpirationDate|Datum/tijd|De vervaldatum voor het abonnement, in de ISO 8601-notatie: `2014-06-24T16:25:00Z`.|  
|NotificationDate|Datum/tijd|De datum van de melding voor het abonnement, in de ISO 8601-notatie: `2014-06-24T16:25:00Z`.|  
|primaryKey|Tekenreeks|De abonnementssleutel van de primaire. Maximale lengte is 256 tekens.|  
|secundaire sleutel|Tekenreeks|De abonnementssleutel secundaire. Maximale lengte is 256 tekens.|  
|CanBeRenewed|booleaans|Hiermee wordt aangegeven of het abonnement kan worden vernieuwd door de huidige gebruiker.|  
|HasExpired|booleaans|Hiermee wordt aangegeven of het abonnement is verlopen.|  
|IsRejected|booleaans|Hiermee wordt aangegeven of de Abonnementaanvraag is geweigerd.|  
|cancelUrl|Tekenreeks|De relatieve Url om het abonnement.|  
|RenewUrl|Tekenreeks|De relatieve Url om het abonnement te verlengen.|  
  
##  <a name="SubscriptionSummary"></a>Samenvatting van abonnement  
 De `subscription summary` entiteit heeft de volgende eigenschappen.  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Id|Tekenreeks|Bron-id. Een unieke identificatie van het abonnement binnen het huidige exemplaar van API Management-service. De waarde is een geldige relatieve URL in de indeling van `subscriptions/{sid}` waar `{sid}` is de abonnement-id. Deze eigenschap is alleen-lezen.|  
|Weergavenaam|Tekenreeks|De weergavenaam van het abonnement|  
  
##  <a name="UserAccountInfo"></a>Gebruikersaccountgegevens  
 De `user account info` entiteit heeft de volgende eigenschappen.  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Voornaam|Tekenreeks|De voornaam. Mag niet leeg zijn. Maximale lengte is 100 tekens.|  
|Achternaam|Tekenreeks|De achternaam. Mag niet leeg zijn. Maximale lengte is 100 tekens.|  
|E-mail|Tekenreeks|E-mailadres. Mag niet leeg zijn en moet uniek zijn binnen het service-exemplaar. Maximale lengte is 254 tekens.|  
|Wachtwoord|Tekenreeks|Het wachtwoord voor gebruikersaccount.|  
|NameIdentifier|Tekenreeks|Account-id dezelfde zijn als de e-mailadres voor de gebruiker.|  
|ProviderName|Tekenreeks|Providernaam van verificatie.|  
|IsBasicAccount|booleaans|De waarde True als dit account is geregistreerd met e-mailadres en wachtwoord; ONWAAR als het account is geregistreerd met een provider.|  
  
##  <a name="UseSignIn"></a>Gebruiker aanmelden  
 De `user sign in` entiteit heeft de volgende eigenschappen.  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|E-mail|Tekenreeks|E-mailadres. Mag niet leeg zijn en moet uniek zijn binnen het service-exemplaar. Maximale lengte is 254 tekens.|  
|Wachtwoord|Tekenreeks|Het wachtwoord voor gebruikersaccount.|  
|ReturnUrl|Tekenreeks|Meld u de URL van de pagina waar de gebruiker heeft geklikt.|  
|RememberMe|booleaans|Hiermee geeft u op of u gegevens van de huidige gebruiker op te slaan.|  
|RegistrationEnabled|booleaans|Hiermee wordt aangegeven of de registratie is ingeschakeld.|  
|DelegationEnabled|booleaans|Hiermee wordt aangegeven of gedelegeerde aanmelding is ingeschakeld.|  
|DelegationUrl|Tekenreeks|De gedelegeerde aanmelden als ingeschakeld-url.|  
|SsoSignUpUrl|Tekenreeks|De eenmalige aanmelding URL voor de gebruiker, indien aanwezig.|  
|AuxServiceUrl|Tekenreeks|Als de huidige gebruiker een beheerder is, is dit een koppeling naar het service-exemplaar in de Azure portal.|  
|Providers|Verzameling van [Provider](#Provider) entiteiten|De verificatieproviders voor deze gebruiker.|  
|UserRegistrationTerms|Tekenreeks|Termen die een gebruiker accepteren moet voordat u zich aanmeldt.|  
|UserRegistrationTermsEnabled|booleaans|Hiermee wordt aangegeven of de voorwaarden zijn ingeschakeld.|  
  
##  <a name="UserSignUp"></a>Meld u aan gebruiker  
 De `user sign up` entiteit heeft de volgende eigenschappen.  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|PasswordConfirm|booleaans|Waarde die wordt gebruikt door de [aanmelding](api-management-page-controls.md#sign-up)aanmelding besturingselement.|  
|Wachtwoord|Tekenreeks|Het wachtwoord voor gebruikersaccount.|  
|PasswordVerdictLevel|nummer|Waarde die wordt gebruikt door de [aanmelding](api-management-page-controls.md#sign-up)aanmelding besturingselement.|  
|UserRegistrationTerms|Tekenreeks|Termen die een gebruiker accepteren moet voordat u zich aanmeldt.|  
|UserRegistrationTermsOptions|nummer|Waarde die wordt gebruikt door de [aanmelding](api-management-page-controls.md#sign-up)aanmelding besturingselement.|  
|ConsentAccepted|booleaans|Waarde die wordt gebruikt door de [aanmelding](api-management-page-controls.md#sign-up)aanmelding besturingselement.|  
|E-mail|Tekenreeks|E-mailadres. Mag niet leeg zijn en moet uniek zijn binnen het service-exemplaar. Maximale lengte is 254 tekens.|  
|Voornaam|Tekenreeks|De voornaam. Mag niet leeg zijn. Maximale lengte is 100 tekens.|  
|Achternaam|Tekenreeks|De achternaam. Mag niet leeg zijn. Maximale lengte is 100 tekens.|  
|UserData|Tekenreeks|Waarde die wordt gebruikt door de [aanmelding](api-management-page-controls.md#sign-up) besturingselement.|  
|NameIdentifier|Tekenreeks|Waarde die wordt gebruikt door de [aanmelding](api-management-page-controls.md#sign-up)aanmelding besturingselement.|  
|ProviderName|Tekenreeks|Providernaam van verificatie.|

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het werken met sjablonen [het aanpassen van de API Management portal voor ontwikkelaars met behulp van sjablonen](api-management-developer-portal-templates.md).
