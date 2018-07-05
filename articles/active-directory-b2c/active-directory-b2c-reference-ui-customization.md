---
title: Aanpassingen van de gebruikersinterface (UI) in Azure Active Directory B2C | Microsoft Docs
description: Een onderwerp op functies voor aanpassing van gebruikersinterface (UI) in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/16/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 385c13194063761d6449fafa49714d8627f6c6fc
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37447050"
---
# <a name="azure-active-directory-b2c-customize-the-azure-ad-b2c-user-interface-ui"></a>Azure Active Directory B2C: De Azure AD B2C-gebruikersinterface (UI) aanpassen

Gebruikerservaring is uitermate belangrijk in een klantgerichte toepassingen.  Breid uw klant basis door gebruikerservaringen met het uiterlijk van uw merk. Azure Active Directory B2C (Azure AD B2C) kunt u aanpassen met het bewerken van profielen voor gebruikersregistratie, aanmelding, en het wachtwoord opnieuw instellen van pagina's met perfecte controle.

> [!NOTE]
> De functie pagina gebruikersinterface aanpassen die worden beschreven in dit artikel is niet van toepassing op de aanmelding alleen beleid, de bijbehorende pagina voor het opnieuw instellen van wachtwoorden en verificatie e-mailberichten.  Deze functies gebruiken de [functie huisstijl van bedrijf](../active-directory/fundamentals/customize-branding.md) in plaats daarvan.
>
> Op dezelfde manier als een gebruiker intiates een bewerkingsprofielbeleid *voordat* aanmeldt, wordt de gebruiker worden omgeleid naar een pagina die kan worden aangepast met behulp van de [functie huisstijl van bedrijf](../active-directory/fundamentals/customize-branding.md).

In dit artikel bevat informatie over de volgende onderwerpen:

* De pagina UI aanpassing-functie.
* Een hulpprogramma voor het HTML-inhoud uploaden naar Azure Blob-opslag voor gebruik met de pagina-functie voor het aanpassen van de gebruikersinterface.
* De UI-elementen die worden gebruikt door Azure AD B2C die u kunt aanpassen met behulp van Cascading Style Sheets (CSS).
* Aanbevolen procedures bij het nakomen van deze functie.

## <a name="the-page-ui-customization-feature"></a>De functie pagina gebruikersinterface aanpassen

U kunt het uiterlijk van de klant gebruikersregistratie, aanmelding bij (Zie de bovenstaande opmerking voor uitzonderingen met betrekking tot huisstijl), wachtwoord opnieuw instellen en bewerken van profielen pagina's (door het configureren van [beleid](active-directory-b2c-reference-policies.md)). Uw klanten genieten van een naadloze ervaring bij het navigeren tussen uw toepassing en pagina's die worden geleverd door Azure AD B2C.

In tegenstelling tot andere services waar opties van de gebruikersinterface, Azure AD B2C maakt gebruik van een eenvoudige en moderne aanpak voor UI-aanpassing.

Hoe het werkt als volgt: Azure AD B2C wordt uitgevoerd de code in de browser van uw klant en maakt gebruik van een moderne manier met de naam [Cross-Origin Resource Sharing (CORS)](http://www.w3.org/TR/cors/).  Inhoud is geladen op het moment van uitvoering, vanuit een URL die u in een beleid opgeeft. U kunt verschillende URL's voor verschillende pagina's opgeven. Nadat de geladen uit de URL van uw inhoud met een HTML-fragment ingevoegd vanuit Azure AD B2C wordt samengevoegd, wordt de pagina wordt weergegeven aan de klant. U hoeft alleen is:

1. HTML5 opgemaakte inhoud met een lege maken `<div id="api"></div>` element zich ergens in de `<body>`. Dit element merken waar de inhoud van de Azure AD B2C wordt ingevoegd.
1. Host uw inhoud op een HTTPS-eindpunt (met CORS toegestaan). Houd er rekening mee beide ophalen en opties aanvraagmethoden moeten zijn ingeschakeld bij het configureren van CORS.
1. CSS gebruiken voor de stijl van de UI-elementen die Azure AD B2C wordt ingevoegd.

### <a name="a-basic-example-of-customized-html"></a>Een eenvoudige voorbeeld van aangepaste HTML

Het volgende voorbeeld is de meest eenvoudige HTML-inhoud die u gebruiken kunt voor het testen van deze mogelijkheid. Gebruik de [hulpprogramma](active-directory-b2c-reference-ui-customization-helper-tool.md) uploaden en configureren van deze inhoud op uw Azure Blob-opslag. Vervolgens kunt u controleren dat de basis-, niet-vorm van een gestileerde knoppen & formuliervelden op elke pagina weergegeven en functioneert worden.

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

## <a name="test-out-the-ui-customization-feature"></a>Testen van de functie van de gebruikersinterface aanpassen

Wilt u voor het uitproberen van de functie van de gebruikersinterface aanpassen met behulp van onze voorbeeld-HTML en CSS-inhoud?  We bieden u [een hulpprogramma](active-directory-b2c-reference-ui-customization-helper-tool.md) die wordt geüpload en voorbeelden voor inhoud op uw Azure Blob-opslag configureert.

> [!NOTE]
> U kunt de inhoud van uw gebruikersinterface overal hosten: op de webservers, CDN, AWS S3, delen bestandssystemen, enzovoort. Als de inhoud wordt gehost op een openbaar beschikbare HTTPS-eindpunt met CORS ingeschakeld, bent u aan de slag. Ter illustratie alleen gebruiken we Azure Blob-opslag.
>

## <a name="the-ui-fragments-embedded-by-azure-ad-b2c"></a>De UI-fragmenten ingesloten door Azure AD B2C

De volgende secties worden de HTML5-fragmenten die Azure AD B2C worden samengevoegd in de `<div id="api"></div>` element zich in uw inhoud. **Voeg geen deze fragmenten in uw HTML-5-inhoud.** De Azure AD B2C-service voegt deze in op het moment van uitvoering. Gebruik deze fragmenten als referentie bij het ontwerpen van uw eigen Cascading Style Sheets (CSS).

### <a name="fragment-inserted-into-the-identity-provider-selection-page"></a>Fragment ingevoegd in de "Identity provider selectie page"

Deze pagina bevat een lijst met id-providers die de gebruiker uit tijdens het registreren of aanmelden kiezen kan. Deze knoppen bevatten sociale id-providers, zoals Facebook en Google + of lokale accounts (op basis van e-mailadres of gebruikersnaam naam).

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

### <a name="fragment-inserted-into-the-local-account-sign-up-page"></a>Fragment ingevoegd in de 'lokale account registratiepagina"

Deze pagina bevat een formulier voor het lokale account Meld u aan op basis van een e-mailadres of de naam van een gebruiker. Het formulier kan verschillende besturingselementen voor tekstinvoer zoals tekstinvoervak, wachtwoordinvoervak, keuzerondje, enkelvoudige selectie vervolgkeuzelijsten en meervoudige selectie selectievakjes bevatten.

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

### <a name="fragment-inserted-into-the-social-account-sign-up-page"></a>Fragment ingevoegd in de 'sociaal account registratiepagina"

Deze pagina kan worden weergegeven bij het aanmelden met een bestaand account van een sociale id-provider zoals Facebook of Google +.  Deze wordt gebruikt wanneer aanvullende informatie moet worden verzameld van een eindgebruiker met behulp van een aanmeldingsformulier hebt ingevuld. Deze pagina is vergelijkbaar met het lokale accountpagina voor het registreren (weergegeven in de vorige sectie) met uitzondering van de velden van de vermelding wachtwoord.

### <a name="fragment-inserted-into-the-unified-sign-up-or-sign-in-page"></a>Fragment ingevoegd in de "Unified registreren of aanmelden pagina"

Deze pagina verwerkt zowel registreren en aanmelden van klanten die op sociale id-providers zoals Facebook of Google + of lokale accounts kunnen gebruiken.

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

Gebruikers kunnen hun telefoonnummers (via SMS of spraak) controleren tijdens het registreren of aanmelden op deze pagina.

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

### <a name="fragment-inserted-into-the-error-page"></a>Fragment op de pagina' fout' ingevoegd

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

Er zijn twee manieren om uw HTML-inhoud te lokaliseren. Één manier is om in te schakelen [aanpassing van taal](active-directory-b2c-reference-language-customization.md). Azure AD B2C voor het doorsturen van de parameter Open ID Connect inschakelen van deze functie kunt `ui-locales`, aan uw eindpunt.  Uw server voor webinhoud kunt u deze parameter gebruiken voor aangepaste HTML-pagina's die specifieke taal.

U kunt ook inhoud ophaalt uit verschillende plaatsen op basis van de landinstelling die wordt gebruikt. U kunt een mapstructuur op de host-inhoud voor specifieke talen instellen in uw eindpunt CORS is ingeschakeld. U moet de juiste is aanroepen als u het jokerteken `{Culture:RFC5646}`.  Bijvoorbeeld, wordt ervan uitgegaan dat dit de URI voor uw aangepaste pagina is:

```
https://wingtiptoysb2c.blob.core.windows.net/{Culture:RFC5646}/wingtip/unified.html
```
U kunt de pagina in laden `fr`. Wanneer de pagina HTML en CSS-inhoud haalt, wordt het binnenhalen van:
```
https://wingtiptoysb2c.blob.core.windows.net/fr/wingtip/unified.html
```

## <a name="things-to-remember-when-building-your-own-content"></a>Om te onthouden bij het bouwen van uw eigen inhoud

Als u van plan bent om de pagina gebruikersinterface aanpassing functie te gebruiken, raadpleegt u de volgende aanbevolen procedures:

* Niet kopiëren van de Standaardinhoud van de Azure AD B2C en proberen om deze te wijzigen. Het is raadzaam om te maken van uw inhoud HTML5 helemaal en standaardinhoud gebruikt als referentie.
* Uit veiligheidsoverwegingen kunt geen we u alle JavaScript opnemen in uw inhoud. De meeste van wat u nodig hebt, moet zijn gebruiksklaar beschikbaar. Als dit niet het geval is, gebruikt u [User Voice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c) om aan te vragen van de nieuwe functionaliteit.
* Ondersteunde browserversies:
  * Internet Explorer 11, 10, Edge
  * Beperkte ondersteuning voor Internet Explorer 9, 8
  * Google Chrome 42.0 en hoger
  * Mozilla Firefox 38.0 en hoger
* Zorg ervoor dat u geen opneemt `<form>` tags in uw HTML-code, zoals dit leiden tot met de POST-bewerkingen die worden gegenereerd door de geïnjecteerde HTML uit Azure AD B2C problemen zal.
