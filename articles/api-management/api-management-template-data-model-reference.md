---
title: Azure API Management-sjabloon gegevensmodel verwijzing | Microsoft Docs
description: Meer informatie over de entiteit en type representaties voor algemene items in de gegevensmodellen gebruikt voor de developer portal sjablonen in Azure API Management.
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
ms.openlocfilehash: 0f27b6b529c2591e37d48e3386190077fc8efc32
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/20/2018
ms.locfileid: "29378153"
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
-   [Header](#Header)  
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
-   [Gebruikersaanmelding](#UseSignIn)  
-   [Gebruiker registreren](#UserSignUp)  
  
##  <a name="API"></a> API  
 De `API` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|id|tekenreeks|Bron-id. Een unieke identificatie van de API binnen het huidige exemplaar van API Management-service. De waarde is een geldige relatieve URL in de indeling van `apis/{id}` waar `{id}` is een API-id. Deze eigenschap is alleen-lezen.|  
|naam|tekenreeks|De naam van de API. Mag niet leeg zijn. Maximale lengte is 100 tekens.|  
|description|tekenreeks|Beschrijving van de API. Mag niet leeg zijn. Kan HTML-opmaak labels bevatten. Maximumlengte is 1000 tekens.|  
|serviceUrl|tekenreeks|Absolute URL van de back-endservice uitvoering van deze API.|  
|pad|tekenreeks|Relatieve URL is een unieke id van deze API en alle bijbehorende paden resource binnen het service-exemplaar van API Management. Deze wordt toegevoegd aan de API-eindpunt basis-URL opgegeven tijdens het maken van de service-exemplaar om een openbare URL voor deze API.|  
|Protocollen|matrix van getal|Hierin wordt beschreven op welke protocollen de bewerkingen in deze API kunnen worden aangeroepen. Toegestane waarden zijn `1 - http` en `2 - https`, of beide.|  
|authenticationSettings|[Autorisatie-instellingen voor server-verificatie](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#AuthenticationSettings)|Verzameling van verificatie-instellingen die zijn opgenomen in deze API.|  
|subscriptionKeyParameterNames|object|Optionele eigenschap die kan worden gebruikt om aangepaste namen voor de query en/of koptekst parameters met de abonnementssleutel te geven. Als deze eigenschap aanwezig is, moet er ten minste één van de volgende twee eigenschappen bevatten.<br /><br /> `{   "subscriptionKeyParameterNames":   {     "query": “customQueryParameterName",     "header": “customHeaderParameterName"   } }`|  
  
##  <a name="APISummary"></a> API-overzicht  
 De `API summary` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|id|tekenreeks|Bron-id. Een unieke identificatie van de API binnen het huidige exemplaar van API Management-service. De waarde is een geldige relatieve URL in de indeling van `apis/{id}` waar `{id}` is een API-id. Deze eigenschap is alleen-lezen.|  
|naam|tekenreeks|De naam van de API. Mag niet leeg zijn. Maximale lengte is 100 tekens.|  
|description|tekenreeks|Beschrijving van de API. Mag niet leeg zijn. Kan HTML-opmaak labels bevatten. Maximumlengte is 1000 tekens.|  
  
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
|Status|getal|De huidige status van de toepassing.<br /><br /> -0 - geregistreerd<br /><br /> -1 - verzonden<br /><br /> -2 - gepubliceerd<br /><br /> -3 - geweigerd<br /><br /> -4 - niet gepubliceerd|  
|RegistrationDate|DateTime|De datum en tijd die de toepassing is geregistreerd.|  
|Categorie-id|getal|De categorie van de toepassing (financiën, entertainment, enz.)|  
|DeveloperId|tekenreeks|De unieke id van de ontwikkelaar die de aanvraag heeft ingediend.|  
|Bijlagen|Verzameling van [bijlage](#Attachment) entiteiten.|Eventuele bijlagen voor de toepassing zoals schermafbeeldingen of pictogrammen.|  
|Pictogram|[Bijlage](#Attachment)|Het pictogram de voor de toepassing.|  
  
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
|kwast|tekenreeks|Welke codesyntaxis sjabloon moet worden gebruikt bij het weergeven van de voorbeeldcode van de kleuren. Toegestane waarden zijn `plain`, `php`, `java`, `xml`, `objc`, `python`, `ruby`, en `csharp`.|  
|sjabloon|tekenreeks|De naam van deze voorbeeldsjabloon code.|  
|hoofdtekst|tekenreeks|Een tijdelijke aanduiding voor het gedeelte voor het voorbeeld van code van het fragment.|  
|method|tekenreeks|De HTTP-methode van de bewerking.|  
|Schema|tekenreeks|Het protocol moet worden gebruikt voor de aanvraag opnieuw.|  
|pad|tekenreeks|Het pad van de bewerking.|  
|query|tekenreeks|Voorbeeld van de query-tekenreeks met gedefinieerde parameters.|  
|host|tekenreeks|De URL van de gateway van API Management-service voor de API met deze bewerking.|  
|headers|Verzameling van [Header](#Header) entiteiten.|Headers voor deze bewerking.|  
|parameters|Verzameling van [Parameter](#Parameter) entiteiten.|De parameters die zijn gedefinieerd voor deze bewerking.|  
  
##  <a name="Comment"></a> Opmerking  
 De `API` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Id|getal|De id van de opmerking.|  
|CommentText|tekenreeks|De hoofdtekst van de opmerking. Kan HTML bevatten.|  
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
|IssueState|tekenreeks|De huidige status van het probleem. Mogelijke waarden zijn voorgesteld, Opened, gesloten.|  
|ReportedOn|DateTime|De datum en tijd die het probleem is opgetreden.|  
|Opmerkingen|Verzameling van [Opmerking](#Comment) entiteiten.|Opmerkingen over dit probleem.|  
|Bijlagen|Verzameling van [bijlage](api-management-template-data-model-reference.md#Attachment) entiteiten.|Eventuele bijlagen aan het probleem.|  
|Services|Verzameling van [API](#API) entiteiten.|De API's die zijn geabonneerd op door de gebruiker die het probleem wordt ingediend.|  
  
##  <a name="Filtering"></a> Filteren  
 De `filtering` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Patroon|tekenreeks|De huidige zoekterm; of `null` als er geen zoekterm.|  
|Tijdelijke aanduiding|tekenreeks|De tekst die moet worden weergegeven in het zoekvak als er geen zoekterm opgegeven.|  
  
##  <a name="Header"></a> Koptekst  
 Deze sectie beschrijft de `parameter` weergave.  
  
|Eigenschap|Beschrijving|Type|  
|--------------|-----------------|----------|  
|naam|tekenreeks|Parameternaam.|  
|description|tekenreeks|Beschrijving van de parameter.|  
|waarde|tekenreeks|Headerwaarde.|  
|typeName|tekenreeks|Het gegevenstype van headerwaarde.|  
|opties|tekenreeks|Opties.|  
|Vereist|booleaans|Hiermee wordt aangegeven of de header is vereist.|  
|Alleen-lezen|booleaans|Hiermee wordt aangegeven of de header is alleen-lezen.|  
  
##  <a name="HTTPRequest"></a> HTTP-aanvraag  
 Deze sectie beschrijft de `request` weergave.  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|description|tekenreeks|Beschrijving van de aanvraag opnieuw.|  
|headers|matrix van [Header](#Header) entiteiten.|Aanvraagheaders.|  
|parameters|matrix van [Parameter](#Parameter)|Verzameling parameters van de aanvraag opnieuw.|  
|verklaringen|matrix van [weergave](#Representation)|Verzameling van bewerking aanvraag verklaringen.|  
  
##  <a name="HTTPResponse"></a> HTTP-antwoord  
 Deze sectie beschrijft de `response` weergave.  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|statusCode|Positief geheel getal|Code voor antwoordstatus bewerking.|  
|description|tekenreeks|Beschrijving van de reactie bewerking.|  
|verklaringen|matrix van [weergave](#Representation)|Verzameling van bewerking antwoord verklaringen.|  
  
##  <a name="Operation"></a> bewerking  
 De `operation` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|id|tekenreeks|Bron-id. Een unieke identificatie van de bewerking binnen het huidige exemplaar van API Management-service. De waarde is een geldige relatieve URL in de indeling van `apis/{aid}/operations/{id}` waar `{aid}` is een API-id en `{id}` is een bewerking-id. Deze eigenschap is alleen-lezen.|  
|naam|tekenreeks|De naam van de bewerking. Mag niet leeg zijn. Maximale lengte is 100 tekens.|  
|description|tekenreeks|Beschrijving van de bewerking. Mag niet leeg zijn. Kan HTML-opmaak labels bevatten. Maximumlengte is 1000 tekens.|  
|Schema|tekenreeks|Hierin wordt beschreven op welke protocollen de bewerkingen in deze API kunnen worden aangeroepen. Toegestane waarden zijn `http`, `https`, of beide `http` en `https`.|  
|uriTemplate|tekenreeks|Relatieve URL sjabloon identificeren van de doelbron voor deze bewerking. Kan parameters bevatten. Voorbeeld: `customers/{cid}/orders/{oid}/?date={date}`|  
|host|tekenreeks|De URL van de API Management-gateway die als host fungeert voor de API.|  
|HttpMethod|tekenreeks|HTTP-bewerkingsmethode.|  
|Aanvraag|[HTTP-aanvraag](#HTTPRequest)|Een entiteit die gegevens voor de aanvraag bevat.|  
|antwoorden|matrix van [HTTP-antwoord](#HTTPResponse)|Matrix van bewerking [HTTP-antwoord](#HTTPResponse) entiteiten.|  
  
##  <a name="Menu"></a> Bewerking menu  
 De `operation menu` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|apiId|tekenreeks|De id van de huidige API.|  
|CurrentOperationId|tekenreeks|De id van de huidige bewerking.|  
|Bewerking|tekenreeks|Het Menutype.|  
|MenuItems|Verzameling van [bewerking menuopdracht](#MenuItem) entiteiten.|De bewerkingen voor de huidige API.|  
  
##  <a name="MenuItem"></a> Bewerking menu-item  
 De `operation menu item` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Id|tekenreeks|De id van de bewerking.|  
|Titel|tekenreeks|De beschrijving van de bewerking.|  
|HttpMethod|tekenreeks|De Http-methode van de bewerking.|  
  
##  <a name="Paging"></a> Paginering  
 De `paging` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Pagina|getal|Het huidige paginanummer.|  
|PageSize|getal|Het maximum aantal resultaten op één pagina moet worden weergegeven.|  
|TotalItemCount|getal|Het aantal items om weer te geven.|  
|ShowAll|booleaans|Of u wilt weergeven, alle resultaten worden op één pagina.|  
|PageCount|getal|Het aantal pagina's met resultaten.|  
  
##  <a name="Parameter"></a> Parameter  
 Deze sectie beschrijft de `parameter` weergave.  
  
|Eigenschap|Beschrijving|Type|  
|--------------|-----------------|----------|  
|naam|tekenreeks|Parameternaam.|  
|description|tekenreeks|Beschrijving van de parameter.|  
|waarde|tekenreeks|Waarde voor parameter.|  
|opties|Matrix van tekenreeks|Waarden die zijn gedefinieerd voor parameterwaarden.|  
|Vereist|booleaans|Hiermee geeft u op of de parameter vereist is.|  
|type|getal|Hiermee wordt aangegeven of deze parameter is een padparameter (1) of een parameter querystring (2).|  
|typeName|tekenreeks|Het parametertype.|  
  
##  <a name="Product"></a> Product  
 De `product` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Id|tekenreeks|Bron-id. Een unieke identificatie van het product binnen het huidige exemplaar van API Management-service. De waarde is een geldige relatieve URL in de indeling van `products/{pid}` waar `{pid}` is een product-id. Deze eigenschap is alleen-lezen.|  
|Titel|tekenreeks|De naam van het product. Mag niet leeg zijn. Maximale lengte is 100 tekens.|  
|Beschrijving|tekenreeks|Beschrijving van het product. Mag niet leeg zijn. Kan HTML-opmaak labels bevatten. Maximumlengte is 1000 tekens.|  
|Voorwaarden|tekenreeks|Product gebruiksvoorwaarden. Ontwikkelaars wilt abonneren op het product wordt weergegeven en moeten deze voorwaarden accepteren voordat ze het abonnement proces kunnen voltooien.|  
|ProductState|getal|Hiermee geeft u op of het product of niet is gepubliceerd. Gepubliceerde producten kunnen worden gevonden door ontwikkelaars op de portal voor ontwikkelaars. Niet-gepubliceerde producten zijn alleen zichtbaar voor beheerders.<br /><br /> De toegestane waarden voor de Productstatus zijn:<br /><br /> - `0 - Not Published`<br /><br /> - `1 - Published`<br /><br /> - `2 - Deleted`|  
|AllowMultipleSubscriptions|booleaans|Hiermee geeft u op of een gebruiker meerdere abonnementen voor dit product op hetzelfde moment kan hebben.|  
|MultipleSubscriptionsCount|getal|Maximum aantal abonnementen voor dit product een gebruiker mag hebben op hetzelfde moment.|  
  
##  <a name="Provider"></a> Provider  
 De `provider` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Eigenschappen|tekenreeks woordenlijst|Eigenschappen voor deze verificatieprovider.|  
|authenticationType|tekenreeks|Het providertype. (Azure Active Directory, Facebook-aanmelding, Google-Account, Microsoft-Account, Twitter).|  
|Bijschrift|tekenreeks|Weergavenaam van de provider.|  
  
##  <a name="Representation"></a> Weergave  
 Deze sectie beschrijft een `representation`.  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|ContentType|tekenreeks|Hiermee wordt een geregistreerde of aangepaste inhoudstype voor deze weergave bijvoorbeeld `application/xml`.|  
|voorbeeld|tekenreeks|Een voorbeeld van de weergave.|  
  
##  <a name="Subscription"></a> Abonnement  
 De `subscription` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Id|tekenreeks|Bron-id. Een unieke identificatie van het abonnement binnen het huidige exemplaar van API Management-service. De waarde is een geldige relatieve URL in de indeling van `subscriptions/{sid}` waar `{sid}` is de abonnement-id. Deze eigenschap is alleen-lezen.|  
|Product-id|tekenreeks|De bron-id voor het product van het product geabonneerd. De waarde is een geldige relatieve URL in de indeling van `products/{pid}` waar `{pid}` is een product-id.|  
|ProductTitle|tekenreeks|De naam van het product. Mag niet leeg zijn. Maximale lengte is 100 tekens.|  
|ProductDescription|tekenreeks|Beschrijving van het product. Mag niet leeg zijn. Kan HTML-opmaak labels bevatten. Maximumlengte is 1000 tekens.|  
|ProductDetailsUrl|tekenreeks|Relatieve URL naar de details van het product.|  
|state|tekenreeks|De status van het abonnement. Mogelijke statussen zijn:<br /><br /> - `0 - suspended` – het abonnement is geblokkeerd en de abonnee kan API's van het product niet aanroepen.<br /><br /> - `1 - active` – het abonnement actief is.<br /><br /> - `2 - expired` – het abonnement is bereikt de verloopdatum en is gedeactiveerd.<br /><br /> - `3 - submitted` – de Abonnementaanvraag is gemaakt door de ontwikkelaar, maar is nog niet goedgekeurd of afgewezen.<br /><br /> - `4 - rejected` – de Abonnementaanvraag is geweigerd door een beheerder.<br /><br /> - `5 - cancelled` – het abonnement is geannuleerd door de ontwikkelaar of beheerder.|  
|DisplayName|tekenreeks|Weergavenaam van het abonnement.|  
|CreatedDate|Datum/tijd|De datum waarop het abonnement is gemaakt, in de ISO 8601-notatie: `2014-06-24T16:25:00Z`.|  
|CanBeCancelled|booleaans|Hiermee wordt aangegeven of het abonnement kan worden geannuleerd door de huidige gebruiker.|  
|IsAwaitingApproval|booleaans|Hiermee wordt aangegeven of het abonnement wacht op goedkeuring.|  
|Begindatum|Datum/tijd|De begindatum voor het abonnement, in de ISO 8601-notatie: `2014-06-24T16:25:00Z`.|  
|ExpirationDate|Datum/tijd|De vervaldatum voor het abonnement, in de ISO 8601-notatie: `2014-06-24T16:25:00Z`.|  
|NotificationDate|Datum/tijd|De datum van de melding voor het abonnement, in de ISO 8601-notatie: `2014-06-24T16:25:00Z`.|  
|primaryKey|tekenreeks|De abonnementssleutel van de primaire. Maximale lengte is 256 tekens.|  
|secundaire sleutel|tekenreeks|De abonnementssleutel secundaire. Maximale lengte is 256 tekens.|  
|CanBeRenewed|booleaans|Hiermee wordt aangegeven of het abonnement kan worden vernieuwd door de huidige gebruiker.|  
|HasExpired|booleaans|Hiermee wordt aangegeven of het abonnement is verlopen.|  
|IsRejected|booleaans|Hiermee wordt aangegeven of de Abonnementaanvraag is geweigerd.|  
|cancelUrl|tekenreeks|De relatieve Url om het abonnement.|  
|RenewUrl|tekenreeks|De relatieve Url om het abonnement te verlengen.|  
  
##  <a name="SubscriptionSummary"></a> Samenvatting van abonnement  
 De `subscription summary` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Id|tekenreeks|Bron-id. Een unieke identificatie van het abonnement binnen het huidige exemplaar van API Management-service. De waarde is een geldige relatieve URL in de indeling van `subscriptions/{sid}` waar `{sid}` is de abonnement-id. Deze eigenschap is alleen-lezen.|  
|DisplayName|tekenreeks|De weergavenaam van het abonnement|  
  
##  <a name="UserAccountInfo"></a> Gebruikersaccountgegevens  
 De `user account info` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Voornaam|tekenreeks|De voornaam. Mag niet leeg zijn. Maximale lengte is 100 tekens.|  
|LastName|tekenreeks|De achternaam. Mag niet leeg zijn. Maximale lengte is 100 tekens.|  
|Email|tekenreeks|E-mailadres. Mag niet leeg zijn en moet uniek zijn binnen het service-exemplaar. Maximale lengte is 254 tekens.|  
|Wachtwoord|tekenreeks|Het wachtwoord voor gebruikersaccount.|  
|NameIdentifier|tekenreeks|Account-id dezelfde zijn als de e-mailadres voor de gebruiker.|  
|ProviderName|tekenreeks|Providernaam van verificatie.|  
|IsBasicAccount|booleaans|De waarde True als dit account is geregistreerd met e-mailadres en wachtwoord; ONWAAR als het account is geregistreerd met een provider.|  
  
##  <a name="UseSignIn"></a> Gebruiker aanmelden  
 De `user sign in` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Email|tekenreeks|E-mailadres. Mag niet leeg zijn en moet uniek zijn binnen het service-exemplaar. Maximale lengte is 254 tekens.|  
|Wachtwoord|tekenreeks|Het wachtwoord voor gebruikersaccount.|  
|ReturnUrl|tekenreeks|Meld u de URL van de pagina waar de gebruiker heeft geklikt.|  
|RememberMe|booleaans|Hiermee geeft u op of u gegevens van de huidige gebruiker op te slaan.|  
|RegistrationEnabled|booleaans|Hiermee wordt aangegeven of de registratie is ingeschakeld.|  
|DelegationEnabled|booleaans|Hiermee wordt aangegeven of gedelegeerde aanmelding is ingeschakeld.|  
|DelegationUrl|tekenreeks|De gedelegeerde aanmelden als ingeschakeld-url.|  
|SsoSignUpUrl|tekenreeks|De eenmalige aanmelding URL voor de gebruiker, indien aanwezig.|  
|AuxServiceUrl|tekenreeks|Als de huidige gebruiker een beheerder is, is dit een koppeling naar het service-exemplaar in de Azure portal.|  
|Providers|Verzameling van [Provider](#Provider) entiteiten|De verificatieproviders voor deze gebruiker.|  
|UserRegistrationTerms|tekenreeks|Termen die een gebruiker accepteren moet voordat u zich aanmeldt.|  
|UserRegistrationTermsEnabled|booleaans|Hiermee wordt aangegeven of de voorwaarden zijn ingeschakeld.|  
  
##  <a name="UserSignUp"></a> Meld u aan gebruiker  
 De `user sign up` entiteit heeft de volgende eigenschappen:  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|PasswordConfirm|booleaans|Waarde die wordt gebruikt door de [aanmelding](api-management-page-controls.md#sign-up)aanmelding besturingselement.|  
|Wachtwoord|tekenreeks|Het wachtwoord voor gebruikersaccount.|  
|PasswordVerdictLevel|getal|Waarde die wordt gebruikt door de [aanmelding](api-management-page-controls.md#sign-up)aanmelding besturingselement.|  
|UserRegistrationTerms|tekenreeks|Termen die een gebruiker accepteren moet voordat u zich aanmeldt.|  
|UserRegistrationTermsOptions|getal|Waarde die wordt gebruikt door de [aanmelding](api-management-page-controls.md#sign-up)aanmelding besturingselement.|  
|ConsentAccepted|booleaans|Waarde die wordt gebruikt door de [aanmelding](api-management-page-controls.md#sign-up)aanmelding besturingselement.|  
|Email|tekenreeks|E-mailadres. Mag niet leeg zijn en moet uniek zijn binnen het service-exemplaar. Maximale lengte is 254 tekens.|  
|Voornaam|tekenreeks|De voornaam. Mag niet leeg zijn. Maximale lengte is 100 tekens.|  
|LastName|tekenreeks|De achternaam. Mag niet leeg zijn. Maximale lengte is 100 tekens.|  
|UserData|tekenreeks|Waarde die wordt gebruikt door de [aanmelding](api-management-page-controls.md#sign-up) besturingselement.|  
|NameIdentifier|tekenreeks|Waarde die wordt gebruikt door de [aanmelding](api-management-page-controls.md#sign-up)aanmelding besturingselement.|  
|ProviderName|tekenreeks|Providernaam van verificatie.|

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het werken met sjablonen [het aanpassen van de API Management portal voor ontwikkelaars met behulp van sjablonen](api-management-developer-portal-templates.md).
