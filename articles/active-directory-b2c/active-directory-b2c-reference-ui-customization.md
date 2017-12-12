---
title: Aanpassingen van de gebruikersinterface (UI) - Azure AD B2C | Microsoft Docs
description: Een onderwerp op functies voor aanpassing van de gebruikersinterface (UI) in Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: mtillman
editor: parakhj
ms.assetid: 99f5a391-5328-471d-a15c-a2fafafe233d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: saeedakhter-msft
ms.openlocfilehash: be3fe7199308606aaab002290319df9c82149433
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-customize-the-azure-ad-b2c-user-interface-ui"></a>Azure Active Directory B2C: De Azure AD B2C-gebruikersinterface (UI) aanpassen

Gebruikerservaring is uitermate belangrijk in een klantgerichte toepassing.  Uw klant base vergroten door gebruikerservaringen met het uiterlijk van uw merk. Azure Active Directory B2C (Azure AD B2C) kunt u aanpassen registreren, aanmelden, profiel bewerken en wachtwoord opnieuw instellen van pagina's met pixel perfect besturingselement.

> [!NOTE]
> De pagina UI aanpassing-functie beschreven in dit artikel geldt niet voor het enige beleid aanmelden, het bijbehorende wachtwoord opnieuw instellen van pagina- en verificatiestappen e-mailberichten.  Deze functies gebruiken de [functie huisstijl](../active-directory/customize-branding.md) in plaats daarvan.
>
> Op dezelfde manier als een gebruiker intiates een beleid van het profiel bewerken *voordat* aangemeld, de gebruiker wordt omgeleid naar een pagina die kan worden aangepast met behulp van de [functie huisstijl](../active-directory/customize-branding.md).

In dit artikel bevat informatie over de volgende onderwerpen:

* De pagina UI aanpassing-functie.
* Een hulpprogramma voor het HTML-inhoud uploaden naar Azure Blob-opslag voor gebruik met de functie UI aanpassing.
* De UI-elementen die worden gebruikt door Azure AD B2C die u kunt aanpassen met behulp van Cascading stylesheets (CSS).
* Aanbevolen procedures bij het uitoefenen van deze functie.

## <a name="the-page-ui-customization-feature"></a>De pagina UI-functie aanpassing

U kunt het uiterlijk van de klant registreren, aanmelden, wachtwoord opnieuw instellen en profiel bewerken's (door het configureren van [beleid](active-directory-b2c-reference-policies.md)). Uw klanten krijgen een naadloze ervaring bij het navigeren tussen de toepassing en pagina's die worden bediend door Azure AD B2C.

In tegenstelling tot andere services waarbij de opties van de gebruikersinterface, Azure AD B2C gebruikmaakt van een eenvoudige en moderne benadering van UI-aanpassing.

Dit is hoe het werkt: Azure AD B2C wordt code wordt uitgevoerd in de browser van uw klant en maakt gebruik van een benadering van moderne aangeroepen [Cross-Origin-Resource delen (CORS)](http://www.w3.org/TR/cors/).  Inhoud wordt geladen tijdens runtime, via een URL die u in een beleid opgeeft. U kunt verschillende URL's voor verschillende pagina's opgeven. Nadat geladen vanuit de URL van uw inhoud wordt samengevoegd met een HTML-fragment ingevoegd vanuit Azure AD B2C, wordt de pagina naar de klant weergegeven. U hoeft te doen is:

1. HTML5 juist opgemaakte inhoud met een lege maken `<div id="api"></div>` element zich ergens in de `<body>`. Dit element aanhalingstekens waar de inhoud van de Azure AD B2C wordt ingevoegd.
1. Host uw inhoud op een HTTPS-eindpunt (met CORS toegestaan). Houd er rekening mee beide ophalen en opties aanvraagmethoden moeten zijn ingeschakeld wanneer u CORS configureert.
1. CSS gebruiken voor de stijl van de UI-elementen die worden ingevoegd door Azure AD B2C.

### <a name="a-basic-example-of-customized-html"></a>Een eenvoudige voorbeeld van aangepaste HTML

Het volgende voorbeeld is de meest eenvoudige HTML-inhoud die u gebruiken kunt voor het testen van deze mogelijkheid. Gebruik de [hulpprogramma helper](active-directory-b2c-reference-ui-customization-helper-tool.md) uploaden en configureren van deze inhoud op uw Azure-blobopslag. Vervolgens kunt u controleren of de knoppen van eenvoudige, niet-vorm van een gestileerde & formuliervelden op elke pagina weergegeven en werken worden.

```HTML
<!DOCTYPE html>
<html>
    <head>
        <title>!Add your title here!</title>
    </head>
    <body>
        <div id="api"></div>   <!-- Leave this element empty because Azure AD B2C will insert content here. -->
    </body>
</html>
```

## <a name="test-out-the-ui-customization-feature"></a>Testen van de UI-functie voor aanpassing

Wilt u de UI-functie voor aanpassing door het gebruik van onze voorbeeld HTML en CSS-inhoud uitproberen?  We bieden u [een helper-hulpprogramma](active-directory-b2c-reference-ui-customization-helper-tool.md) die uploadt en configureert u voorbeelden van uw Azure Blob-opslag.

> [!NOTE]
> U kunt de inhoud van uw gebruikersinterface overal hosten: op webservers CDN, AWS S3, delen bestandssystemen, enzovoort. Als de inhoud wordt gehost op een openbaar HTTPS-eindpunt met CORS ingeschakeld, bent u klaar om te beginnen. We ter illustratie alleen Azure Blob-opslag gebruiken.
>

## <a name="the-ui-fragments-embedded-by-azure-ad-b2c"></a>De UI-fragmenten ingesloten door Azure AD B2C

De volgende secties worden de HTML5-fragmenten die Azure AD B2C, worden in de `<div id="api"></div>` element zich in uw inhoud. **Voeg deze fragmenten geen uw HTML 5-inhoud.** De service Azure AD B2C ingevoegd in tijdens runtime. Deze fragmenten gebruiken als referentiemateriaal bij het ontwerpen van uw eigen Cascading stylesheets (CSS).

### <a name="fragment-inserted-into-the-identity-provider-selection-page"></a>Fragment ingevoegd in de 'provider selectiepagina'

Deze pagina bevat een lijst met de id-providers die de gebruiker uit tijdens het registreren of aanmelden kiezen kan. Deze knoppen bevatten sociale id-providers zoals Facebook en Google + of lokale accounts (op basis van e-mailadres of de gebruiker naam).

```HTML
<div id="api" data-name="IdpSelections">
    <div class="intro">
         <p>Sign up</p>
    </div>

    <div>
        <ul>
            <li>
                <button class="accountButton" id="FacebookExchange">Facebook</button>
            </li>
            <li>
                <button class="accountButton" id="GoogleExchange">Google+</button>
            </li>
            <li>
                <button class="accountButton" id="SignUpWithLogonEmailExchange">Email</button>
            </li>
        </ul>
    </div>
</div>
```

### <a name="fragment-inserted-into-the-local-account-sign-up-page"></a>Fragment ingevoegd in de 'lokale account aanmeldpagina'

Deze pagina bevat een formulier voor het lokale account aanmelding op basis van een e-mailadres of een gebruikersnaam. Het formulier kan andere invoer besturingselementen zoals tekstinvoervak, vermelding wachtwoordvak keuzerondje, één vervolgkeuzelijsten en meervoudige selectie selectievakjes bevatten.

```HTML
<div id="api" data-name="SelfAsserted">
    <div class="intro">
        <p>Create your account by providing the following details</p>
    </div>

    <div id="attributeVerification">
        <div class="errorText" id="passwordEntryMismatch" style="display: none;">The password entry fields do not match. Please enter the same password in both fields and try again.</div>
        <div class="errorText" id="requiredFieldMissing" style="display: none;">A required field is missing. Please fill out all required fields and try again.</div>
        <div class="errorText" id="fieldIncorrect" style="display: none;">One or more fields are filled out incorrectly. Please check your entries and try again.</div>
        <div class="errorText" id="claimVerificationServerError" style="display: none;"></div>
        <div class="attr" id="attributeList">
            <ul>
                <li>
                    <div class="attrEntry validate">
                        <div>
                            <div class="verificationInfoText" id="email_intro" style="display: inline;">Verification is necessary. Please click Send button.</div>
                            <div class="verificationInfoText" id="email_info" style="display:none">Verification code has been sent to your inbox. Please copy it to the input box below.</div>
                            <div class="verificationSuccessText" id="email_success" style="display:none">E-mail address verified. You can now continue.</div>
                            <div class="verificationErrorText" id="email_fail_retry" style="display:none">Incorrect code, try again.</div>
                            <div class="verificationErrorText" id="email_fail_no_retry" style="display:none">Exceeded number of retries you need to send new code.</div>
                            <div class="verificationErrorText" id="email_fail_server" style="display:none">Server error, please try again</div>
                            <div class="verificationErrorText" id="email_incorrect_format" style="display:none">Incorect format.</div>
                        </div>

                    <div class="helpText show">This information is required</div>
                        <label>Email</label>
                        <input id="email" class="textInput" type="text" placeholder="Email" required="" autofocus=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Email address that can be used to contact you.');" class="tiny">What is this?</a>

                    <div class="buttons verify" claim_id="email">
                        <div id="email_ver_wait" class="working" style="display: none;"></div>
                            <label id="email_ver_input_label" for="email_ver_input" style="display: none;">Verification code</label>
                            <input id="email_ver_input" type="text" placeholder="Verification code" style="display:none">
                            <button id="email_ver_but_send" class="sendButton" type="button" style="display: inline;">Send verification code</button>
                            <button id="email_ver_but_verify" class="verifyButton" type="button" style="display:none">Verify code</button>
                            <button id="email_ver_but_resend" class="sendButton" type="button" style="display:none">Send new code</button>
                            <button id="email_ver_but_edit" class="editButton" type="button" style="display:none">Change e-mail</button>
                            <button id="email_ver_but_default" class="defaultButton" type="button" style="display:none">Default</button>
                        </div>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ " ( ) ; .This information is required</div>
                        <label>Enter password</label>
                        <input id="password" class="textInput" type="password" placeholder="Enter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title="8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ &quot; ( ) ; ." required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Enter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"> This information is required</div>
                        <label>Reenter password</label>
                        <input id="reenterPassword" class="textInput" type="password" placeholder="Reenter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title=" " required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Reenter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Name</label>
                        <input id="displayName" class="textInput" type="text" placeholder="Name" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your display name.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Gender</label>
                        <input id="extension_Gender_F" name="extension_Gender" type="radio" value="F" autofocus="">
                        <label for="extension_Gender_F">Female</label>
                        <input id="extension_Gender_M" name="extension_Gender" type="radio" value="M">
                        <label for="extension_Gender_M">Male</label>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Loyalty number</label>
                        <input id="extension_MemNum" class="textInput" type="text" placeholder="Loyalty number"><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Membership number');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>State</label>
                        <select class="dropdown_single" id="state">
                            <option style="display:none" disabled="disabled" value="placeholder" selected="">State</option>
                            <option value="WA">Washington</option>
                            <option value="NY">New York</option>
                            <option value="CA">California</option>
                        </select>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your residential state or province.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Zip code</label>
                        <input id="postalCode" class="textInput" type="text" placeholder="Zip code" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('The postal code of your address.');" class="tiny">What is this?</a>
                    </div>
                </li>
            </ul>
        </div>
        <div class="buttons"> <button id="continue" disabled="">Create</button> <button id="cancel">Cancel</button></div>
    </div>
    <div class="verifying-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="verifying_blurb"></div>
    </div>
</div>
```

### <a name="fragment-inserted-into-the-social-account-sign-up-page"></a>Fragment ingevoegd in de '' sociale account aanmeldpagina'

Deze pagina kan worden weergegeven wanneer u zich aanmeldt met een bestaand account van een identiteitsprovider van sociale zoals Facebook of Google +.  Wordt gebruikt wanneer u aanvullende informatie moet worden verzameld van de eindgebruiker via een aanmeldingsformulier hebt ingevuld. Deze pagina is vergelijkbaar met de lokale account aanmeldpagina (weergegeven in de vorige sectie) met uitzondering van de invoervelden wachtwoord.

### <a name="fragment-inserted-into-the-unified-sign-up-or-sign-in-page"></a>Fragment ingevoegd in de "Unified registreren of aanmelden pagina'

Deze pagina verwerkt zowel registreren en aanmelden aan klanten, die sociale id-providers zoals Facebook of Google + of lokale accounts kunnen gebruiken.

```HTML
<div id="api" data-name="Unified">
        <div class="social" role="form">
               <div class="intro">
                       <h2>Sign in with your social account</h2>
               </div>
               <div class="options">
                       <div><button class="accountButton firstButton" id="MicrosoftAccountExchange" tabindex="1">msa</button></div>
                       <div><button class="accountButton" id="FacebookExchange" tabindex="1">fb</button></div>
               </div>
        </div>
        <div class="divider">
               <h2>OR</h2>
        </div>
        <div class="localAccount" role="form">
               <div class="intro">
                       <h2>Sign in with your existing account</h2>
               </div>
               <div class="error pageLevel" aria-hidden="true" style="display: none;">
                       <p role="alert"></p>
               </div>
               <div class="entry">
                       <div class="entry-item">
                               <label for="logonIdentifier">Email Address</label> 
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="email" id="logonIdentifier" name="LogonIdentifier" pattern="^[a-zA-Z0-9.!#$%&amp;’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$" placeholder="LogonIdentifier" value="" tabindex="1">
                       </div>
                       <div class="entry-item">
                               <div class="password-label"> <label for="password">Password</label><a id="forgotPassword" tabindex="2">Forgot your password?</a></div>
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="password" id="password" name="Password" placeholder="Password" tabindex="1">
                       </div>
                       <div class="working"></div>
                       <div class="buttons"> <button id="next" tabindex="1">Sign in</button> </div>
               </div>
               <div class="divider">
                       <h2>OR</h2>
               </div>
               <div class="create">
                       <p>Don't have an account?<a id="createAccount" tabindex="1">Sign up now</a> </p>
               </div>
        </div>
</div>
```

### <a name="fragment-inserted-into-the-multi-factor-authentication-page"></a>Fragment ingevoegd op de pagina' multi-factor authentication'

Gebruikers kunnen hun telefoonnummers (met behulp van de tekst of stem) verifiëren tijdens het registreren of aanmelden op deze pagina.

```HTML
<div id="api" data-name="Phonefactor">
    <div id="phonefactor_initial">
        <div class="intro">
            <p>Enter a number below that we can send a code via SMS or phone to authenticate you.</p>
        </div>
        <div class="errorText" id="errorMessage" style="display:none"></div>
        <div class="phoneEntry" id="phoneEntry">
            <div class="phoneNumber">
                <select id="countryCode" style="display:inline-block">
                    <option value="+93">Afghanistan (+93)</option>
                    <!-- Not all country codes listed -->
                    <option value="+44">United Kingdom (+44)</option>
                    <option value="+1" selected="">United States (+1)</option>
                    <!-- Not all country codes listed -->
                </select>
            </div>
            <div class="phoneNumber">
                <input type="text" id="localNumber" style="display:inline-block" placeholder="Phone number">
            </div>
        </div>
        <div id="codeVerification" style="display:none">
            <div>
                <p>Enter your verification code below, or <button id="retryCode" class="textButton">send a new code</button></p>
                <input type="text" id="verificationCode" placeholder="Verification code">
            </div>
        </div>
        <div class="buttons">
            <button id="verifyCode" class="sendInitialCodeButton">Send Code</button>
            <button id="verifyPhone" style="display:inline-block">Call Me</button>
            <button id="cancel" style="display:inline-block">Cancel</button>
        </div>
    </div>
    <div class="dialing-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="dialing_blurb"></div><div id="dialing_number"></div>
    </div>
</div>
```

### <a name="fragment-inserted-into-the-error-page"></a>Fragment ingevoegd in de '' foutpagina'

```HTML
<div id="api" class="error-page-content" data-name="GlobalException">
    <h2>Sorry, but we're having trouble signing you in.</h2>
    <div class="error-page-help">We track these errors automatically, but if the problem persists feel free to contact us. In the meantime, please try again.</div>
    <div class="error-page-messagedetails">Your administrator hasn't provided any contact details.</div>
    <div class="error-page-messagedetails">
        <div class="error-page-correlationid">Correlation ID:1c4f0397-c6e4-4afe-bf74-42f488f2f15f</div>
        <div>Timestamp:2015-09-14 23:22:35Z</div>
        <div class="error-page-detail">AADB2C90065: A B2C client-side error 'Access is denied.' has occurred requesting the remote resource.</div>
    </div>
</div>
```

## <a name="localizing-your-html-content"></a>Lokalisatie van uw HTML-inhoud

U kunt uw HTML-inhoud door het inschakelen van lokaliseren ['Aanpassing taal'](active-directory-b2c-reference-language-customization.md).  Azure AD B2C voor het doorsturen van de parameter Open ID Connect inschakelen van deze functie kunt `ui-locales`, naar het eindpunt.  De inhoudsserver kunt u deze parameter gebruiken voor aangepaste HTML-pagina's die een specifieke taal zijn gebonden.

## <a name="things-to-remember-when-building-your-own-content"></a>Om te onthouden tijdens het bouwen van uw eigen inhoud

Als u van plan bent om de pagina UI aanpassing functie te gebruiken, controleert u de volgende aanbevolen procedures:

* Niet kopiëren van de Standaardinhoud van de Azure AD B2C en probeert te wijzigen. Het is raadzaam om uw HTML5-inhoud vanaf het begin samen te stellen en standaardinhoud gebruikt als referentie.
* Uit veiligheidsoverwegingen kunt geen we u alle JavaScript opnemen in uw inhoud. Wat u moet de meeste zijn gebruiksklaar beschikbaar. Als dit niet het geval is, gebruik [User Voice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c) om aan te vragen van de nieuwe functionaliteit.
* Ondersteunde browserversies:
  * Internet Explorer 11, 10, rand
  * Beperkte ondersteuning voor Internet Explorer 9 kunt 8
  * Google Chrome 42.0 en hoger
  * Mozilla Firefox 38.0 en hoger
