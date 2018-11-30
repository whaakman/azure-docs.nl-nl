---
title: Sjablonen in Azure API Management | Microsoft Docs
description: Informatie over het aanpassen van de inhoud van ontwikkelaarsportalpagina's met behulp van een set van sjablonen in Azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: e57df269-1019-4b74-b74d-53155b809d59
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: apimpm
ms.openlocfilehash: 1fbafcdab938a0f8653df48631d7733cc58a3668
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52441907"
---
# <a name="page-templates-in-azure-api-management"></a>Sjablonen in Azure API Management
Met Azure API Management biedt u de mogelijkheid om aan te passen van de inhoud van ontwikkelaarsportalpagina's met behulp van een set van sjablonen die hun inhoud hebt geconfigureerd. Met behulp van [DotLiquid](http://dotliquidmarkup.org/) syntaxis en de editor van uw keuze, zoals [DotLiquid voor ontwerpers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), en een opgegeven set gelokaliseerde [tekenreeks resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), en [pagina besturingselementen](api-management-page-controls.md), hebt u geweldige flexibiliteit voor het configureren van de inhoud van de pagina's naar eigen inzicht met behulp van deze sjablonen.  
  
 De sjablonen in deze sectie kunt u de inhoud van het teken in sign up aanpassen en pagina's pagina niet gevonden in de portal voor ontwikkelaars.  
  
-   [Aanmelden](#SignIn)  
  
-   [Aanmelden](#SignUp)  
  
-   [Pagina niet gevonden](#PageNotFound)  
  
> [!NOTE]
>  Standaard-voorbeeldsjablonen zijn opgenomen in de volgende documentatie, maar kunnen worden gewijzigd vanwege de continue verbeteringen. U kunt de live standaardsjablonen weergeven in de portal voor ontwikkelaars door te navigeren naar de gewenste afzonderlijke sjablonen. Zie voor meer informatie over het werken met sjablonen [over het aanpassen van de API Management-ontwikkelaarsportal met behulp van sjablonen](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]
  
##  <a name="SignIn"></a> Aanmelden  
 De **aanmelden** sjabloon kunt u de aanmeldingspagina in de ontwikkelaarsportal aanpassen.  
  
 ![Meld u aan op de pagina](./media/api-management-page-templates/APIM-Sign-In-Page-Developer-Portal-Templates.png "APIM sjablonen voor Ontwikkelaarsportals pagina aanmelden")  
  
### <a name="default-template"></a>Standaardsjabloon  
  
```xml  
<h2 class="text-center">{% localized "SigninStrings|WebAuthenticationSigninTitle" %}</h2>  
{% if registrationEnabled == true %}  
<p class="text-center">{% localized "SigninStrings|WebAuthenticationNotAMember" %}</p>  
{% endif %}  
  
<div class="row center-block ap-idp-container">  
  <div class="col-md-6">  
    {% if registrationEnabled == true %}  
        <p>{% localized "SigninStrings|WebAuthenticationSigininWithPassword" %}</p>  
    <basic-SignIn></basic-SignIn>  
    {% endif %}  
  </div>  
  
    {% if registrationEnabled != true and providers.size == 0 %}  
        {% localized "ProviderInfoStrings|TextboxExternalIdentitiesDisabled" %}  
  {% else %}  
        {% if providers.size > 0 %}  
      <div class="col-md-6">  
            <div class="providers-list">  
                <p class="text-left">  
                {% if registrationEnabled == true %}  
                    {% localized "ProviderInfoStrings|TextboxExternalIdentitiesSigninInvitation" %}  
                {% else %}  
                    {% localized "ProviderInfoStrings|TextboxExternalIdentitiesSigninInvitationPrimary" %}  
                {% endif %}  
                </p>  
        <providers></providers>  
            </div>  
    </div>  
        {% endif %}  
    {% endif %}  
  
  {% if userRegistrationTermsEnabled == true %}  
    <div class="col-md-6">  
        <div id="terms" class="modal" role="dialog" tabindex="-1">  
            <div class="modal-dialog">  
                <div class="modal-content">  
                    <div class="modal-header">  
                        <h4 class="modal-title">{% localized "SigninResources|DialogHeadingTermsOfUse" %}</h4>  
                    </div>  
                    <div class="modal-body break-all">{{userRegistrationTerms}}</div>  
                    <div class="modal-footer">  
                        <button type="button" class="btn btn-default" data-dismiss="modal">{% localized "CommonStrings|ButtonLabelClose" %}</button>  
                    </div>  
                </div>  
            </div>  
        </div>  
        <p>{% localized "SigninResources|TextblockUserRegistrationTermsProvided" %}</p>  
    </div>  
    {% endif %}  
</div>  
```  
  
### <a name="controls"></a>Besturingselementen  
 Deze sjabloon mogelijk gebruikt u de volgende [pagina besturingselementen](api-management-page-controls.md).  
  
-   [Basic-aanmelding](api-management-page-controls.md#basic-signin)  
  
-   [Providers](api-management-page-controls.md#providers)  
  
### <a name="data-model"></a>Gegevensmodel  
 [Aanmelding door een gebruiker in](api-management-template-data-model-reference.md#UseSignIn) entiteit.  
  
### <a name="sample-template-data"></a>Voorbeeldgegevens voor de sjabloon  
  
```json  
{
    "Email": null,
    "Password": null,
    "ReturnUrl": null,
    "RememberMe": false,
    "RegistrationEnabled": true,
    "DelegationEnabled": false,
    "DelegationUrl": null,
    "SsoSignUpUrl": null,
    "AuxServiceUrl": "https://portal.azure.com/#resource/subscriptions/{subscription ID}/resourceGroups/Api-Default-West-US/providers/Microsoft.ApiManagement/service/contoso5",
    "Providers": [  
        {  
            "Properties": {  
                "AuthenticationType": "Aad",  
                "Caption": "Azure Active Directory"  
            },  
            "AuthenticationType": "Aad",  
            "Caption": "Azure Active Directory"  
        }  
        ],
    "UserRegistrationTerms": null,
    "UserRegistrationTermsEnabled": false
}
```  
  
##  <a name="SignUp"></a> Aanmelden  
 De **aanmelden** sjabloon blijft u op de aanmeldpagina voor in de ontwikkelaarsportal aanpassen.  
  
 ![Aanmeldingspagina](./media/api-management-page-templates/APIM-Sign-Up-Page-Developer-Portal-Templates.png "APIM sjablonen voor Ontwikkelaarsportals pagina aanmelden")  
  
### <a name="default-template"></a>Standaardsjabloon  
  
```xml  
<h2 class="text-center">{% localized "SignupStrings|PageTitleSignup" %}</h2>  
<p class="text-center">  
  {% localized "SignupStrings|WebAuthenticationAlreadyAMember" %} <a href="/signin">{% localized "SignupStrings|WebAuthenticationSigninNow" %}</a>  
</p>  
  
<div class="row center-block ap-idp-container">  
  <div class="col-md-6">  
    <p>{% localized "SignupStrings|WebAuthenticationCreateNewAccount" %}</p>  
    <sign-up></sign-up>  
  </div>  
</div>  
```  
  
### <a name="controls"></a>Besturingselementen  
 Deze sjabloon mogelijk gebruikt u de volgende [pagina besturingselementen](api-management-page-controls.md).  
  
-   [Meld u aan](api-management-page-controls.md#sign-up)  
  
### <a name="data-model"></a>Gegevensmodel  
 [Gebruikersregistraties](api-management-template-data-model-reference.md#UserSignUp) entiteit.  
  
### <a name="sample-template-data"></a>Voorbeeldgegevens voor de sjabloon  
  
```json  
{  
    "PasswordConfirm": null,  
    "Password": null,  
    "PasswordVerdictLevel": 0,  
    "UserRegistrationTerms": null,  
    "UserRegistrationTermsOptions": 0,  
    "ConsentAccepted": false,  
    "Email": null,  
    "FirstName": null,  
    "LastName": null,  
    "UserData": null,  
    "NameIdentifier": null,  
    "ProviderName": null  
}  
```  
  
##  <a name="PageNotFound"></a> Pagina niet gevonden  
 De **pagina niet gevonden** sjabloon blijft u op de pagina pagina niet gevonden in de ontwikkelaarsportal aanpassen.  
  
 ![Pagina niet gevonden](./media/api-management-page-templates/APIM-Not-Found-Page-Developer-Portal-Templates.png "APIM niet gevonden pagina Developer Portal-sjablonen")  
  
### <a name="default-template"></a>Standaardsjabloon  
  
```xml  
<h2>{% localized "NotFoundStrings|PageTitleNotFound" %}</h2>  
  
<h3>{% localized "NotFoundStrings|TitlePotentialCause" %}</h3>  
<ul>  
  <li>{% localized "NotFoundStrings|TextblockPotentialCauseOldLink" %}</li>  
  <li>{% localized "NotFoundStrings|TextblockPotentialCauseMisspelledUrl" %}</li>  
</ul>  
  
<h3>{% localized "NotFoundStrings|TitlePotentialSolution" %}</h3>  
<ul>  
  <li>{% localized "NotFoundStrings|TextblockPotentialSolutionRetype" %}</li>  
  <li>  
    {% capture textPotentialSolutionStartOver %}{% localized "NotFoundStrings|TextblockPotentialSolutionStartOver" %}{% endcapture %}  
    {% capture homeLink %}<a href="/">{% localized "NotFoundStrings|LinkLabelHomePage" %}</a>{% endcapture %}  
    {% assign replaceString = '{0}' %}  
  
    {{ textPotentialSolutionStartOver | replace : replaceString, homeLink }}  
  </li>  
</ul>  
  
<p>  
  {% capture textReportProblem %}{% localized "NotFoundStrings|TextReportProblem" %}{% endcapture %}  
  {% capture emailLink %}<a href="mailto:apimgmt@microsoft.com" target="_self" title="API Management Support">{% localized "NotFoundStrings|LinkLabelSendUsEmail" %}</a>{% endcapture %}  
  {% assign replaceString = '{0}' %}  
  
  {{ textReportProblem | replace : replaceString, emailLink }}  
</p>  
```  
  
### <a name="controls"></a>Besturingselementen  
 Deze sjabloon kan geen gebruik [pagina besturingselementen](api-management-page-controls.md).  
  
### <a name="data-model"></a>Gegevensmodel  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|referenceCode|tekenreeks|Code gegenereerd als deze pagina wordt weergegeven als gevolg van een interne fout.|  
|Foutcode|tekenreeks|Code gegenereerd als deze pagina wordt weergegeven als gevolg van een interne fout.|  
|emailBody|tekenreeks|E-instantie die worden gegenereerd als deze pagina wordt weergegeven als gevolg van een interne fout.|  
|requestedUrl|tekenreeks|De URL die wordt gevraagd wanneer de pagina is niet gevonden.|  
|referrerUrl|tekenreeks|De verwijzende site-URL voor de aangevraagde URL.|  
  
### <a name="sample-template-data"></a>Voorbeeldgegevens voor de sjabloon  
  
```json  
{  
    "referenceCode": null,  
    "errorCode": null,  
    "emailBody": null,  
    "requestedUrl": "https://contoso5.portal.azure-api.net:443/NotFoundPage?startEditTemplate=NotFoundPage",  
    "referrerUrl": "https://contoso5.portal.azure-api.net/signup?startEditTemplate=SignUpTemplate"  
}  
```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het werken met sjablonen [over het aanpassen van de API Management-ontwikkelaarsportal met behulp van sjablonen](api-management-developer-portal-templates.md).
