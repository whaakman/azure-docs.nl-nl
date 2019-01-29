---
title: Uw RESTful-services beveiligen met behulp van HTTP-verificatie in Azure Active Directory B2C | Microsoft Docs
description: Uw aangepaste claims-uitwisselingen van REST-API in uw Azure AD B2C beveiligen met behulp van HTTP-basisverificatie.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/25/2017
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 061987105eac976e40a003a8108921ed0008630d
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55169158"
---
# <a name="secure-your-restful-services-by-using-http-basic-authentication"></a>Uw RESTful-services beveiligen met behulp van HTTP-basisverificatie

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In een [gerelateerde Azure AD B2C-artikel](active-directory-b2c-custom-rest-api-netfw.md), maken van een RESTful-service (web-API) die kan worden geïntegreerd met Azure Active Directory B2C (Azure AD B2C) gebruiker reizen zonder verificatie. 

In dit artikel, voegt u HTTP-verificatie toe aan uw RESTful-service zodat alleen geverifieerde gebruikers, met inbegrip van B2C, hebben toegang tot uw API. Met HTTP-basisverificatie, kunt u de referenties van de gebruiker (app-ID en app-geheim) instellen in uw aangepast beleid. 

Zie voor meer informatie, [basisverificatie in ASP.NET web-API](https://docs.microsoft.com/aspnet/web-api/overview/security/basic-authentication).

## <a name="prerequisites"></a>Vereisten
Voer de stappen in de [REST-API integreren claims worden uitgewisseld in uw Azure AD B2C de gebruikersbeleving](active-directory-b2c-custom-rest-api-netfw.md) artikel.

## <a name="step-1-add-authentication-support"></a>Stap 1: Verificatie-ondersteuning toevoegen

### <a name="step-11-add-application-settings-to-your-projects-webconfig-file"></a>Stap 1.1: Voeg toepassingsinstellingen toe aan het web.config-bestand van uw project
1. Open het Visual Studio-project dat u eerder hebt gemaakt. 

2. De volgende toepassingsinstellingen toevoegen aan het bestand web.config in de `appSettings` element:

    ```XML
    <add key="WebApp:ClientId" value="B2CServiceUserAccount" />
    <add key="WebApp:ClientSecret" value="your secret" />
    ```

3. Maak een wachtwoord, en stel vervolgens de `WebApp:ClientSecret` waarde.

    Voer de volgende PowerShell-code voor het genereren van een complex wachtwoord in. U kunt een willekeurige waarde gebruiken.

    ```PowerShell
    $bytes = New-Object Byte[] 32
    $rand = [System.Security.Cryptography.RandomNumberGenerator]::Create()
    $rand.GetBytes($bytes)
    $rand.Dispose()
    [System.Convert]::ToBase64String($bytes)
    ```

### <a name="step-12-install-owin-libraries"></a>Stap 1.2: OWIN-bibliotheken installeren
Toevoegen als u wilt beginnen, de OWIN-middleware NuGet-pakketten aan het project met behulp van Visual Studio Package Manager Console:

```
PM> Install-Package Microsoft.Owin
PM> Install-Package Owin
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

### <a name="step-13-add-an-authentication-middleware-class"></a>Stap 1.3: Voeg een verificatie-middleware-klasse
Voeg de `ClientAuthMiddleware.cs` klasse onder de *App_Start* map. Dit doet u als volgt:

1. Met de rechtermuisknop op de *App_Start* map, selecteer **toevoegen**, en selecteer vervolgens **klasse**.

   ![ClientAuthMiddleware.cs klasse in de map App_Start toevoegen](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-secure-basic-OWIN-startup-auth1.png)

2. In de **naam** in het vak **ClientAuthMiddleware.cs**.

   ![Nieuwe C#-klasse maken](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-secure-basic-OWIN-startup-auth2.png)

3. Open de *App_Start\ClientAuthMiddleware.cs* -bestand en vervang het bestand inhoud met de volgende code:

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

### <a name="step-14-add-an-owin-startup-class"></a>Stap 1.4: Een OWIN-opstartklasse toevoegen
Voeg een OWIN-Opstartklasse met de naam toe `Startup.cs` naar de API. Dit doet u als volgt:
1. Met de rechtermuisknop op het project, selecteer **toevoegen** > **Nieuw Item**, en zoek vervolgens **OWIN**.

   ![Een OWIN-opstartklasse toevoegen](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-secure-basic-OWIN-startup.png)

2. Open de *Startup.cs* -bestand en vervang het bestand inhoud met de volgende code:

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
Open Controllers\IdentityController.cs en voeg de `[Authorize]` code aan de controllerklasse. Deze tag wordt beperkt tot de controller voor gebruikers die voldoen aan de vereiste machtiging.

![De autoriseren-tag toevoegen aan de controller](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-secure-basic-authorize.png)

## <a name="step-2-publish-to-azure"></a>Stap 2: Publiceren naar Azure
Voor het publiceren van uw project in Solution Explorer, met de rechtermuisknop op de **Contoso.AADB2C.API** project en selecteer vervolgens **publiceren**.

## <a name="step-3-add-the-restful-services-app-id-and-app-secret-to-azure-ad-b2c"></a>Stap 3: De RESTful-services-app-ID en app-geheim toevoegen aan Azure AD B2C
Als uw RESTful-service wordt beveiligd door de client-ID (gebruikersnaam) en -geheim, moet u de referenties opslaan in uw Azure AD B2C-tenant. Uw aangepaste beleid bevat de referenties wanneer deze de RESTful-services roept. 

### <a name="step-31-add-a-restful-services-client-id"></a>Stap 3.1: Toevoegen van een RESTful-services-client-ID
1. Selecteer in uw Azure AD B2C-tenant, **B2C-instellingen** > **Identity-Ervaringsframework**.


2. Selecteer **Beleidssleutels** om de sleutels die beschikbaar in uw tenant zijn weer te geven.

3. Selecteer **Toevoegen**.

4. Voor **opties**, selecteer **handmatig**.

5. Voor **naam**, type **B2cRestClientId**.  
    Het voorvoegsel *B2C_1A_* mogelijk automatisch worden toegevoegd.

6. In de **geheim** voert u de app-ID die u eerder hebt gedefinieerd.

7. Voor **sleutelgebruik**, selecteer **handtekening**.

8. Selecteer **Maken**.

9. Bevestig dat u hebt gemaakt de `B2C_1A_B2cRestClientId` sleutel.

### <a name="step-32-add-a-restful-services-client-secret"></a>Stap 3.2: Toevoegen van een clientgeheim RESTful-services
1. Selecteer in uw Azure AD B2C-tenant, **B2C-instellingen** > **Identity-Ervaringsframework**.

2. Selecteer **Beleidssleutels** om de sleutels die beschikbaar zijn in uw tenant weer te geven.

3. Selecteer **Toevoegen**.

4. Voor **opties**, selecteer **handmatig**.

5. Voor **naam**, type **B2cRestClientSecret**.  
    Het voorvoegsel *B2C_1A_* mogelijk automatisch worden toegevoegd.

6. In de **geheim** voert u de appgeheim dat u eerder hebt gedefinieerd.

7. Voor **sleutelgebruik**, selecteer **handtekening**.

8. Selecteer **Maken**.

9. Bevestig dat u hebt gemaakt de `B2C_1A_B2cRestClientSecret` sleutel.

## <a name="step-4-change-the-technical-profile-to-support-basic-authentication-in-your-extension-policy"></a>Stap 4: Het technische profiel ter ondersteuning van basisverificatie wordt gebruikt in de uitbreiding beleid wijzigen
1. Open het beleid extensiebestand (TrustFrameworkExtensions.xml) in uw werkmap.

2. Zoek de `<TechnicalProfile>` knooppunt met `Id="REST-API-SignUp"`.

3. Zoek de `<Metadata>` element.

4. Wijzig de *AuthenticationType* naar *Basic*, als volgt:
    ```xml
    <Item Key="AuthenticationType">Basic</Item>
    ```

5. Onmiddellijk na de afsluitende `<Metadata>` -element, Voeg het volgende XML-fragment toe: 

    ```xml
    <CryptographicKeys>
        <Key Id="BasicAuthenticationUsername" StorageReferenceId="B2C_1A_B2cRestClientId" />
        <Key Id="BasicAuthenticationPassword" StorageReferenceId="B2C_1A_B2cRestClientSecret" />
    </CryptographicKeys>
    ```
    Nadat u het fragment hebt toegevoegd, wordt het technische profiel moet eruitzien als de volgende XML-code:
    
    ![Basisverificatie XML-elementen toe te voegen](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-secure-basic-add-1.png)

## <a name="step-5-upload-the-policy-to-your-tenant"></a>Stap 5: Uploaden van het beleid aan uw tenant

1. In de [Azure-portal](https://portal.azure.com), Ga naar de [context van uw Azure AD B2C-tenant](active-directory-b2c-navigate-to-b2c-context.md), en open vervolgens **Azure AD B2C**.

2. Selecteer **Identity-Ervaringsframework**.

3. Open **alle beleidsregels**.

4. Selecteer **beleid uploaden**.

5. Selecteer de **het beleid overschrijven als deze bestaat** selectievakje.

6. Upload de *TrustFrameworkExtensions.xml* bestand en zorg ervoor dat deze de validatietests doorstaat.

## <a name="step-6-test-the-custom-policy-by-using-run-now"></a>Stap 6: Het aangepaste beleid testen met behulp van nu uitvoeren
1. Open **Azure AD B2C-instellingen**, en selecteer vervolgens **Identity-Ervaringsframework**.

    >[!NOTE]
    >Voer nu vereist dat ten minste één toepassing vooraf op de tenant worden geregistreerd. Zie voor meer informatie over het registreren van toepassingen, de Azure AD B2C [aan de slag](active-directory-b2c-get-started.md) artikel of de [toepassingsregistratie](active-directory-b2c-app-registration.md) artikel.

2. Open **B2C_1A_signup_signin**, de relying party (RP) aangepast beleid u geüpload en selecteer vervolgens **nu uitvoeren**.

3. Het proces testen door te typen **Test** in de **voornaam** vak.  
    Azure AD B2C wordt een foutbericht weergegeven aan de bovenkant van het venster.

    ![Uw identiteit API testen](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-test.png)

4. In de **voornaam** typt u een naam (met uitzondering van 'Test').  
    Azure AD B2C de gebruiker zich aanmeldt en verzendt vervolgens een getal loyaliteit naar uw toepassing. Houd rekening met het nummer in dit voorbeeld:

    ```
    {
      "typ": "JWT",
      "alg": "RS256",
      "kid": "X5eXk4xyojNFum1kl2Ytv8dlNP4-c57dO6QGTVBwaNk"
    }.{
      "exp": 1507125903,
      "nbf": 1507122303,
      "ver": "1.0",
      "iss": "https://contoso.b2clogin.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
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

## <a name="optional-download-the-complete-policy-files-and-code"></a>(Optioneel) De volledige beleidsbestanden en de code downloaden
* Na het voltooien van de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) scenario, het is raadzaam dat u uw scenario bouwen met behulp van uw eigen aangepaste beleidsbestanden. Ter referentie, we hebben opgegeven [beleid voorbeeldbestanden](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw-secure-basic).
* U kunt de code van de volledige downloaden [voorbeeld Visual Studio-oplossing voor verwijzing](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw/Contoso.AADB2C.API).

## <a name="next-steps"></a>Volgende stappen
* [-Clientcertificaten gebruiken voor het beveiligen van uw RESTful-API](active-directory-b2c-custom-rest-api-netfw-secure-cert.md)

