---
title: 'Azure Active Directory B2C: Uw RESTful-services beveiligen met behulp van HTTP-basisverificatie'
description: Uw aangepaste REST-API claims kunnen worden uitgewisseld in uw Azure AD B2C beveiligen met behulp van HTTP-basisverificatie
services: active-directory-b2c
documentationcenter: 
author: yoelhor
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 09/25/2017
ms.author: yoelh
ms.openlocfilehash: 0d4594f5e7c0a13d50993dd42d4780c1ba703140
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="secure-your-restful-services-by-using-http-basic-authentication"></a>Uw RESTful-services beveiligen met behulp van HTTP-basisverificatie
In een [bijbehorende Azure AD B2C-artikel](active-directory-b2c-custom-rest-api-netfw.md), hebt u een RESTful-service (web-API) die kan worden geïntegreerd met Azure Active Directory B2C (Azure AD B2C) gebruiker trajecten zonder verificatie. 

In dit artikel u HTTP basic verificatie toevoegen aan uw RESTful-service zodanig dat alleen geverifieerd gebruikers, met inbegrip van B2C, hebben toegang tot uw API. Met HTTP-basisverificatie stelt u de referenties van de gebruiker (app-ID en app-geheim) in het aangepaste beleid. 

Zie voor meer informatie [basisverificatie in ASP.NET web-API](https://docs.microsoft.com/aspnet/web-api/overview/security/basic-authentication).

## <a name="prerequisites"></a>Vereisten
Voer de stappen in de [REST-API integreren claims kunnen worden uitgewisseld in uw Azure AD B2C gebruiker reis](active-directory-b2c-custom-rest-api-netfw.md) artikel.

## <a name="step-1-add-authentication-support"></a>Stap 1: Ondersteuning voor verificatie toevoegen

### <a name="step-11-add-application-settings-to-your-projects-webconfig-file"></a>Stap 1.1: Toepassingsinstellingen toevoegen aan uw project web.config-bestand
1. Open het Visual Studio-project dat u eerder hebt gemaakt. 

2. De volgende toepassingsinstellingen toevoegen aan het bestand web.config in de `appSettings` element:

    ```XML
    <add key="WebApp:ClientId" value="B2CServiceUserAccount" />
    <add key="WebApp:ClientSecret" value="your secret" />
    ```

3. Maak een wachtwoord in en stel de `WebApp:ClientSecret` waarde.

    Als een complex wachtwoord worden gegenereerd, voert u de volgende PowerShell-code. U kunt een willekeurige waarde gebruiken.

    ```PowerShell
    $bytes = New-Object Byte[] 32
    $rand = [System.Security.Cryptography.RandomNumberGenerator]::Create()
    $rand.GetBytes($bytes)
    $rand.Dispose()
    [System.Convert]::ToBase64String($bytes)
    ```

### <a name="step-12-install-owin-libraries"></a>Stap 1.2: Installeer OWIN-bibliotheken
Om te beginnen, moet u de OWIN middleware NuGet pakketten toevoegen aan het project met de Visual Studio Package Manager-Console:

```
PM> Install-Package Microsoft.Owin
PM> Install-Package Owin
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

### <a name="step-13-add-an-authentication-middleware-class"></a>Stap 1.3: Een verificatie middleware klasse toevoegen
Voeg de `ClientAuthMiddleware.cs` klasse onder de *App_Start* map. Dit doet u als volgt:

1. Met de rechtermuisknop op de *App_Start* map, selecteer **toevoegen**, en selecteer vervolgens **klasse**.

   ![ClientAuthMiddleware.cs klasse toevoegen aan de map App_Start](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-secure-basic-OWIN-startup-auth1.png)

2. In de **naam** in het vak **ClientAuthMiddleware.cs**.

   ![Maak nieuwe klasse C#](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-secure-basic-OWIN-startup-auth2.png)

3. Open de *App_Start\ClientAuthMiddleware.cs* bestand en vervang het bestand inhoud met de volgende code:

    ```csharp
    
    using Microsoft.Owin;
    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.Linq;
    using System.Security.Principal;
    using System.Text;
    using System.Threading.Tasks;
    using System.Web;
    
    namespace Contoso.AADB2C.API
    {
        /// <summary>
        /// Class to create a custom owin middleware to check for client authentication
        /// </summary>
        public class ClientAuthMiddleware
        {
            private static readonly string ClientID = ConfigurationManager.AppSettings["WebApp:ClientId"];
            private static readonly string ClientSecret = ConfigurationManager.AppSettings["WebApp:ClientSecret"];
    
            /// <summary>
            /// Gets or sets the next owin middleware
            /// </summary>
            private Func<IDictionary<string, object>, Task> Next { get; set; }
    
            /// <summary>
            /// Initializes a new instance of the <see cref="ClientAuthMiddleware"/> class.
            /// </summary>
            /// <param name="next"></param>
            public ClientAuthMiddleware(Func<IDictionary<string, object>, Task> next)
            {
                this.Next = next;
            }
    
            /// <summary>
            /// Invoke client authentication middleware during each request.
            /// </summary>
            /// <param name="environment">Owin environment</param>
            /// <returns></returns>
            public Task Invoke(IDictionary<string, object> environment)
            {
                // Get wrapper class for the environment
                var context = new OwinContext(environment);
    
                // Check whether the authorization header is available. This contains the credentials.
                var authzValue = context.Request.Headers.Get("Authorization");
                if (string.IsNullOrEmpty(authzValue) || !authzValue.StartsWith("Basic ", StringComparison.OrdinalIgnoreCase))
                {
                    // Process next middleware
                    return Next(environment);
                }
    
                // Get credentials
                var creds = authzValue.Substring("Basic ".Length).Trim();
                string clientId;
                string clientSecret;
    
                if (RetrieveCreds(creds, out clientId, out clientSecret))
                {
                    // Set transaction authenticated as client
                    context.Request.User = new GenericPrincipal(new GenericIdentity(clientId, "client"), new string[] { "client" });
                }
    
                return Next(environment);
            }
    
            /// <summary>
            /// Retrieve credentials from header
            /// </summary>
            /// <param name="credentials">Authorization header</param>
            /// <param name="clientId">Client identifier</param>
            /// <param name="clientSecret">Client secret</param>
            /// <returns>True if valid credentials were presented</returns>
            private bool RetrieveCreds(string credentials, out string clientId, out string clientSecret)
            {
                string pair;
                clientId = clientSecret = string.Empty;
    
                try
                {
                    pair = Encoding.UTF8.GetString(Convert.FromBase64String(credentials));
                }
                catch (FormatException)
                {
                    return false;
                }
                catch (ArgumentException)
                {
                    return false;
                }
    
                var ix = pair.IndexOf(':');
                if (ix == -1)
                {
                    return false;
                }
    
                clientId = pair.Substring(0, ix);
                clientSecret = pair.Substring(ix + 1);
    
                // Return whether credentials are valid
                return (string.Compare(clientId, ClientAuthMiddleware.ClientID) == 0 &&
                    string.Compare(clientSecret, ClientAuthMiddleware.ClientSecret) == 0);
            }
        }
    }
    ```

### <a name="step-14-add-an-owin-startup-class"></a>Stap 1.4: Een OWIN-Opstartklasse toevoegen
Voeg een OWIN-Opstartklasse met de naam `Startup.cs` aan de API. Dit doet u als volgt:
1. Met de rechtermuisknop op het project, selecteert u **toevoegen** > **Nieuw Item**, en zoek vervolgens naar **OWIN**.

   ![Een OWIN-opstartklasse toevoegen](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-secure-basic-OWIN-startup.png)

2. Open de *Startup.cs* bestand en vervang het bestand inhoud met de volgende code:

    ```csharp
    using Microsoft.Owin;
    using Owin;
    
    [assembly: OwinStartup(typeof(Contoso.AADB2C.API.Startup))]
    namespace Contoso.AADB2C.API
    {
        public class Startup
        {
            public void Configuration(IAppBuilder app)
            {
                    app.Use<ClientAuthMiddleware>();
            }
        }
    }
    ```

### <a name="step-15-protect-the-identity-api-class"></a>Stap 1.5: De klasse-id-API beveiligen
Open Controllers\IdentityController.cs en voeg de `[Authorize]` code aan de controllerklasse. Deze tag beperkt toegang tot de controller voor gebruikers die voldoen aan de vereiste machtiging.

![De tag autoriseren aan de controller toevoegen](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-secure-basic-authorize.png)

## <a name="step-2-publish-to-azure"></a>Stap 2: Publiceren naar Azure
Voor het publiceren van uw project in Solution Explorer, met de rechtermuisknop op de **Contoso.AADB2C.API** project en selecteer vervolgens **publiceren**.

## <a name="step-3-add-the-restful-services-app-id-and-app-secret-to-azure-ad-b2c"></a>Stap 3: De RESTful-services-app-ID en app-geheim toevoegen aan Azure AD B2C
Nadat uw RESTful-service wordt beveiligd door de client-ID (gebruikersnaam) en een geheim, moet u de referenties opgeslagen in uw Azure AD B2C-tenant. Het aangepaste beleid biedt de referenties wanneer het RESTful-services aanroept. 

### <a name="step-31-add-a-restful-services-client-id"></a>Stap 3.1: Een RESTful-services-client-ID toevoegen
1. Selecteer in uw Azure AD B2C-tenant, **B2C-instellingen** > **identiteit ervaring Framework**.


2. Selecteer **beleid sleutels** om de sleutels die beschikbaar in uw tenant zijn weer te geven.

3. Selecteer **Toevoegen**.

4. Voor **opties**, selecteer **handmatige**.

5. Voor **naam**, type **B2cRestClientId**.  
    Het voorvoegsel *B2C_1A_* kunnen automatisch worden toegevoegd.

6. In de **geheim** Voer de app-ID die u eerder hebt gedefinieerd.

7. Voor **sleutelgebruik**, selecteer **geheim**.

8. Selecteer **Maken**.

9. Controleer of u hebt gemaakt de `B2C_1A_B2cRestClientId` sleutel.

### <a name="step-32-add-a-restful-services-client-secret"></a>Stap 3.2: Een clientgeheim RESTful-services toevoegen
1. Selecteer in uw Azure AD B2C-tenant, **B2C-instellingen** > **identiteit ervaring Framework**.

2. Selecteer **beleid sleutels** om de sleutels die beschikbaar zijn in uw tenant weer te geven.

3. Selecteer **Toevoegen**.

4. Voor **opties**, selecteer **handmatige**.

5. Voor **naam**, type **B2cRestClientSecret**.  
    Het voorvoegsel *B2C_1A_* kunnen automatisch worden toegevoegd.

6. In de **geheim** Voer het app-geheim dat u eerder hebt gedefinieerd.

7. Voor **sleutelgebruik**, selecteer **geheim**.

8. Selecteer **Maken**.

9. Controleer of u hebt gemaakt de `B2C_1A_B2cRestClientSecret` sleutel.

## <a name="step-4-change-the-technical-profile-to-support-basic-authentication-in-your-extension-policy"></a>Stap 4: Het technische profiel ter ondersteuning van basisverificatie in de uitbreiding beleid wijzigen
1. Open het bestand met de extensie (TrustFrameworkExtensions.xml) in uw werkmap.

2. Zoeken naar de `<TechnicalProfile>` knooppunt met `Id="REST-API-SignUp"`.

3. Zoek de `<Metadata>` element.

4. Wijzig de *AuthenticationType* naar *Basic*als volgt:
    ```xml
    <Item Key="AuthenticationType">Basic</Item>
    ```

5. Direct na de afsluitende `<Metadata>` element, het volgende XML-fragment toevoegen: 

    ```xml
    <CryptographicKeys>
        <Key Id="BasicAuthenticationUsername" StorageReferenceId="B2C_1A_B2cRestClientId" />
        <Key Id="BasicAuthenticationPassword" StorageReferenceId="B2C_1A_B2cRestClientSecret" />
    </CryptographicKeys>
    ```
    Nadat u het codefragment toevoegt, wordt uw technische profiel moet eruitzien als in de volgende XML-code:
    
    ![Basisverificatie XML-elementen toevoegen](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-secure-basic-add-1.png)

## <a name="step-5-upload-the-policy-to-your-tenant"></a>Stap 5: Het beleid uploaden naar uw tenant

1. In de [Azure-portal](https://portal.azure.com), overschakelen naar de [context van uw Azure AD B2C-tenant](active-directory-b2c-navigate-to-b2c-context.md), en open vervolgens **Azure AD B2C**.

2. Selecteer **identiteit ervaring Framework**.

3. Open **alle beleidsregels**.

4. Selecteer **uploaden beleid**.

5. Selecteer de **het beleid overschreven als deze bestaat** selectievakje.

6. Upload de *TrustFrameworkExtensions.xml* bestand en controleer vervolgens dat deze gevalideerd wordt.

## <a name="step-6-test-the-custom-policy-by-using-run-now"></a>Stap 6: Het aangepaste beleid testen met behulp van nu uitvoeren
1. Open **Azure AD B2C-instellingen**, en selecteer vervolgens **identiteit ervaring Framework**.

    >[!NOTE]
    >Uitvoeren is nu vereist ten minste één toepassing om te worden preregistered op de tenant. Zie voor informatie over het registreren van toepassingen, de Azure AD B2C [aan de slag](active-directory-b2c-get-started.md) artikel of de [toepassingsregistratie](active-directory-b2c-app-registration.md) artikel.

2. Open **B2C_1A_signup_signin**, de relying party (RP) aangepast-beleid die u hebt geüpload en selecteer vervolgens **nu uitvoeren**.

3. Het proces testen door te typen **Test** in de **voornaam** vak.  
    Azure AD B2C wordt een foutbericht weergegeven aan de bovenkant van het venster.

    ![Uw identiteit API testen](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-test.png)

4. In de **voornaam** typt u een naam (met uitzondering van 'Test').  
    Azure AD B2C de gebruiker zich aanmeldt en stuurt vervolgens een getal loyaliteit aan uw toepassing. Houd rekening met het nummer in dit voorbeeld:

    ```
    {
      "typ": "JWT",
      "alg": "RS256",
      "kid": "X5eXk4xyojNFum1kl2Ytv8dlNP4-c57dO6QGTVBwaNk"
    }.{
      "exp": 1507125903,
      "nbf": 1507122303,
      "ver": "1.0",
      "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
      "aud": "e1d2612f-c2bc-4599-8e7b-d874eaca1ee1",
      "acr": "b2c_1a_signup_signin",
      "nonce": "defaultNonce",
      "iat": 1507122303,
      "auth_time": 1507122303,
      "loyaltyNumber": "290",
      "given_name": "Emily",
      "emails": ["B2cdemo@outlook.com"]
    }
    ```

## <a name="optional-download-the-complete-policy-files-and-code"></a>(Optioneel) De volledige-beleidsbestanden en code downloaden
* Na het voltooien van de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) scenario, het is raadzaam dat u uw scenario maken met behulp van uw eigen aangepaste beleidsbestanden. We hebben opgegeven voor eigen referentie [beleid voorbeeldbestanden](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw-secure-basic).
* U kunt de code van de volledige downloaden [voorbeeld Visual Studio-oplossing voor verwijzing](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw/Contoso.AADB2C.API).

## <a name="next-steps"></a>Volgende stappen
* [Clientcertificaten gebruiken voor het beveiligen van uw RESTful-API](active-directory-b2c-custom-rest-api-netfw-secure-cert.md)

