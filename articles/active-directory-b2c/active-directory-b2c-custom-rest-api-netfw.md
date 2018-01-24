---
title: 'Azure Active Directory B2C: REST-API claim uitwisselingen in uw Azure AD B2C gebruiker reis integreren als gebruikersinvoer'
description: REST-API claim uitwisselingen in uw Azure AD B2C gebruiker reis als gebruikersinvoer integreren.
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
ms.date: 09/30/2017
ms.author: yoelh
ms.openlocfilehash: fd9c95ae78590aa772fde10c8c80914c905767a8
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-validation-of-user-input"></a>REST-API claims kunnen worden uitgewisseld in uw Azure AD B2C gebruiker reis integreren als gebruikersinvoer
Met het Framework van de gebruikerservaring identiteit voltooiingscallback die Azure Active Directory B2C (Azure AD B2C), die u kunt integreren met een RESTful-API in het traject van een gebruiker. In dit scenario leert u hoe Azure AD B2C communiceert met .NET Framework RESTful-services (web-API).

## <a name="introduction"></a>Inleiding
Met behulp van Azure AD B2C, kunt u uw eigen zakelijke logica toevoegen aan een gebruiker reis door het aanroepen van uw eigen RESTful-service. Het Framework van de gebruikerservaring identiteit verzendt gegevens naar de RESTful-service in een *invoer claims* verzameling en ontvangt gegevens van RESTful in een *uitvoer claims* verzameling. U kunt met RESTful-service-integratie:

* **Valideren van invoer gebruikersgegevens**: met deze actie voorkomt dat een verkeerd ingedeelde gegevens behouden blijven met Azure AD. Als de waarde van de gebruiker niet geldig is, retourneert de RESTful-service een foutbericht weergegeven dat de gebruiker naar een vermelding in de opdracht geeft. U kunt bijvoorbeeld controleren of het e-mailadres dat is opgegeven door de gebruiker in de database van uw klant bestaat.
* **Overschrijven invoerclaims**: bijvoorbeeld als een gebruiker de voornaam in alleen kleine letters of hoofdletters letters invoert, u kunt de naam opmaken met alleen de eerste letter een hoofdletter.
* **Gebruikersgegevens aanvullen door verdere integreren met zakelijke line-of-business-toepassingen**: uw RESTful-service kan ontvangen van e-mailadres van de gebruiker, query-database van de klant en loyaliteit-nummer van de gebruiker terug naar Azure AD B2C. De retour-claims kunnen worden opgeslagen in een van de gebruiker Azure AD-account, geëvalueerd in de volgende *Indelingsstappen*, of in het toegangstoken is opgenomen.
* **Uitvoeren van aangepaste zakelijke logica**: U kunt pushmeldingen te verzenden, zakelijke databases bijwerken, uitvoeren van het migratieproces van een gebruiker, machtigingen beheren, controleren van de databases en andere acties worden uitgevoerd.

U kunt de integratie met de RESTful-services ontwerpen in de volgende manieren:

* **Technische validatieprofiel**: de aanroep van de RESTful-service binnen het technische profiel van de validatie van het opgegeven profiel voor technische gebeurt. Het profiel van de technische valideert de gegevens van de gebruiker opgegeven voordat de gebruiker reis doorsturen worden verplaatst. U kunt met het technische validatie-profiel:
   * Invoer claims verzenden.
   * Valideer de invoerclaims en genereren van aangepaste foutberichten.
   * Back-uitvoer claims verzenden.

* **Exchange-claims**: dit ontwerp is vergelijkbaar met de technische validatieprofiel, maar dit gebeurt in een stap orchestration. Deze definitie is beperkt tot:
   * Invoer claims verzenden.
   * Back-uitvoer claims verzenden.

## <a name="restful-walkthrough"></a>RESTful-overzicht
In dit scenario maakt ontwikkelen u een .NET Framework-web-API die de gebruikersinvoer valideert en biedt een aantal van de loyaliteit gebruiker. Uw toepassing kan bijvoorbeeld toegang tot verlenen *platina voordelen* op basis van het aantal loyaliteit.

Overzicht:
* Ontwikkel de RESTful-service (.NET Framework-web-API).
* Gebruik de RESTful-service in het traject van de gebruiker.
* Invoer claims verzenden en ze in uw code te lezen.
* Valideer de voornaam van de gebruiker.
* Een getal loyaliteit teruggestuurd. 
* Het aantal loyaliteit toevoegen, naar een JSON Web Token (JWT).

## <a name="prerequisites"></a>Vereisten
Voer de stappen in de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) artikel.

## <a name="step-1-create-an-aspnet-web-api"></a>Stap 1: Een ASP.NET-web-API maken

1. Maak in Visual Studio een project door te selecteren **bestand** > **nieuw** > **Project**.

2. In de **nieuw Project** Selecteer **Visual C#** > **Web** > **ASP.NET-webtoepassing (.NET Framework)**.

3. In de **naam** typt u een naam voor de toepassing (bijvoorbeeld *Contoso.AADB2C.API*), en selecteer vervolgens **OK**.

    ![Nieuwe visual studio-project maken](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-create-project.png)

4. In de **nieuwe ASP.NET-webtoepassing** Selecteer een **Web API** of **Azure API-app** sjabloon.

    ![Web-API-sjabloon selecteren](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-select-web-api.png)

5. Zorg ervoor dat de verificatie is ingesteld op **geen verificatie**.

6. Selecteer **OK** om het project te maken.

## <a name="step-2-prepare-the-rest-api-endpoint"></a>Stap 2: Bereid het REST-API-eindpunt

### <a name="step-21-add-data-models"></a>Stap 2.1: Gegevensmodellen toevoegen
De modellen, zijn de claims van de invoer en uitvoer claims gegevens in uw RESTful-service. Uw code leest de invoergegevens met het deserialiseren van het model invoerclaims vanuit een JSON-tekenreeks naar een C#-object (uw model). De ASP.NET-web-API deserializes automatisch het model van de claims uitvoer terug naar JSON en vervolgens de geserialiseerde gegevens schrijft naar de hoofdtekst van het HTTP-antwoordbericht. 

Maak een model met invoerclaims als volgt:

1. Als Solution Explorer nog niet is geopend, selecteert u **weergave** > **Solution Explorer**. 
2. Klik in Solution Explorer met de rechtermuisknop op de **modellen** map, selecteer **toevoegen**, en selecteer vervolgens **klasse**.

    ![Het model toevoegen](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-add-model.png)

3. Naam van de klasse `InputClaimsModel`, en voeg de volgende eigenschappen voor de `InputClaimsModel` klasse:

    ```csharp
    namespace Contoso.AADB2C.API.Models
    {
        public class InputClaimsModel
        {
            public string email { get; set; }
            public string firstName { get; set; }
            public string lastName { get; set; }
        }
    }
    ```

4. Maak een nieuw model `OutputClaimsModel`, en voeg de volgende eigenschappen voor de `OutputClaimsModel` klasse:

    ```csharp
    namespace Contoso.AADB2C.API.Models
    {
        public class OutputClaimsModel
        {
            public string loyaltyNumber { get; set; }
        }
    }
    ```

5. Maken van een meer model `B2CResponseContent`, waarin u invoer validatiefout berichten genereert. Voeg de volgende eigenschappen voor de `B2CResponseContent` klasse, bieden de ontbrekende verwijzingen en sla het bestand:

    ```csharp
    namespace Contoso.AADB2C.API.Models
    {
        public class B2CResponseContent
        {
            public string version { get; set; }
            public int status { get; set; }
            public string userMessage { get; set; }

            public B2CResponseContent(string message, HttpStatusCode status)
            {
                this.userMessage = message;
                this.status = (int)status;
                this.version = Assembly.GetExecutingAssembly().GetName().Version.ToString();
            }    
        }
    }
    ```

### <a name="step-22-add-a-controller"></a>Step 2.2: Een controller toevoegen
In de web-API, een _controller_ is een object dat HTTP-aanvragen worden verwerkt. De controller retourneert uitvoer claims of, als de eerste naam niet geldig is is, genereert een foutbericht Conflict HTTP.

1. Klik in Solution Explorer met de rechtermuisknop op de **domeincontrollers** map, selecteer **toevoegen**, en selecteer vervolgens **Controller**.

    ![Nieuwe controller toevoegen](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-add-controller-1.png)

2. In de **Add Scaffold** Selecteer **Web API-Controller - leeg**, en selecteer vervolgens **toevoegen**.

    ![Selecteer Web API 2-Controller - leeg](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-add-controller-2.png)

3. In de **Controller toevoegen** venster, de naam van de controller **IdentityController**, en selecteer vervolgens **toevoegen**.

    ![Typ de naam van de domeincontroller](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-add-controller-3.png)

    De steiger maakt een bestand met de naam *IdentityController.cs* in de *domeincontrollers* map.

4. Als de *IdentityController.cs* bestand nog niet open is, dubbelklik erop en vervolgens de code in het bestand vervangen door de volgende code:

    ```csharp
    using Contoso.AADB2C.API.Models;
    using Newtonsoft.Json;
    using System;
    using System.NET;
    using System.Web.Http;

    namespace Contoso.AADB2C.API.Controllers
    {
        public class IdentityController: ApiController
        {
            [HttpPost]
            public IHttpActionResult SignUp()
            {
                // If no data came in, then return
                if (this.Request.Content == null) throw new Exception();

                // Read the input claims from the request body
                string input = Request.Content.ReadAsStringAsync().Result;

                // Check the input content value
                if (string.IsNullOrEmpty(input))
                {
                    return Content(HttpStatusCode.Conflict, new B2CResponseContent("Request content is empty", HttpStatusCode.Conflict));
                }

                // Convert the input string into an InputClaimsModel object
                InputClaimsModel inputClaims = JsonConvert.DeserializeObject(input, typeof(InputClaimsModel)) as InputClaimsModel;

                if (inputClaims == null)
                {
                    return Content(HttpStatusCode.Conflict, new B2CResponseContent("Can not deserialize input claims", HttpStatusCode.Conflict));
                }

                // Run an input validation
                if (inputClaims.firstName.ToLower() == "test")
                {
                    return Content(HttpStatusCode.Conflict, new B2CResponseContent("Test name is not valid, please provide a valid name", HttpStatusCode.Conflict));
                }

                // Create an output claims object and set the loyalty number with a random value
                OutputClaimsModel outputClaims = new OutputClaimsModel();
                outputClaims.loyaltyNumber = new Random().Next(100, 1000).ToString();

                // Return the output claim(s)
                return Ok(outputClaims);
            }
        }
    }
    ```

## <a name="step-3-publish-the-project-to-azure"></a>Stap 3: Het project publiceren naar Azure
1. Klik in Solution Explorer met de rechtermuisknop op de **Contoso.AADB2C.API** project en selecteer vervolgens **publiceren**.

    ![Publiceren naar Microsoft Azure App Service](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-publish-to-azure-1.png)

2. In de **publiceren** Selecteer **Microsoft Azure App Service**, en selecteer vervolgens **publiceren**.

    ![Nieuwe Microsoft Azure App Service maken](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-publish-to-azure-2.png)

    De **Create App Service** venster wordt geopend. In dit maakt u alle benodigde Azure-resources voor het uitvoeren van de ASP.NET-web-app in Azure.

    > [!NOTE]
    >Zie voor meer informatie over het publiceren [een ASP.NET-web-app maken in Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet#publish-to-azure).

3. In de **Web-Appnaam** typt u een unieke app-naam (geldige tekens zijn a-z 0-9 en koppeltekens (-). De URL van de web-app is http://<app_name>.azurewebsites.NET, waarbij *app_naam* is de naam van uw web-app. U kunt de automatisch gegenereerde naam accepteren, die uiteraard uniek is.

    ![Geef eigenschappen voor de App Service](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-publish-to-azure-3.png)

4. Selecteer om te beginnen met het maken van Azure-resources, **maken**.  
    Nadat de ASP.NET-web-app is gemaakt, wordt de wizard publiceert deze naar Azure en start u de app in de standaardbrowser.

6. Kopieer de URL van de web-app.

## <a name="step-4-add-the-new-loyaltynumber-claim-to-the-schema-of-your-trustframeworkextensionsxml-file"></a>Stap 4: De nieuwe toevoegen `loyaltyNumber` claim met het schema van het bestand TrustFrameworkExtensions.xml
De `loyaltyNumber` claim nog niet is gedefinieerd in onze schema. Voeg een definitie binnen de `<BuildingBlocks>` element, dat u aan het begin van vindt de *TrustFrameworkExtensions.xml* bestand.

```xml
<BuildingBlocks>
    <ClaimsSchema>
        <ClaimType Id="loyaltyNumber">
            <DisplayName>loyaltyNumber</DisplayName>
            <DataType>string</DataType>
            <UserHelpText>Customer loyalty number</UserHelpText>
        </ClaimType>
    </ClaimsSchema>
</BuildingBlocks>
```

## <a name="step-5-add-a-claims-provider"></a>Stap 5: Een claimprovider toevoegen 
Elke claimprovider moet een of meer technische profielen vast te stellen de eindpunten en protocollen die nodig zijn om te communiceren met de claimprovider hebben. 

Een claimprovider kan meerdere profielen voor technische om verschillende redenen hebben. Meerdere profielen voor technische kunnen bijvoorbeeld worden gedefinieerd omdat de claimprovider meerdere protocollen ondersteunt eindpunten kunnen verschillende mogelijkheden zijn, of releases claims die kunnen op verschillende niveaus van zekerheid kunnen bevatten. Kan het zijn aanvaardbaar is voor gevoelige claims in één gebruiker reis maar niet in een andere release. 

De volgende XML-fragment bevat een knooppunt van de provider claims met twee technische profielen:

* **TechnicalProfile-Id = "REST-API-SignUp"**: definieert de RESTful-service. 
   * `Proprietary`Als het protocol wordt beschreven voor een op basis van RESTful-provider. 
   * `InputClaims`Hiermee definieert u de claims die naar de REST-service van Azure AD B2C wordt verzonden. 

   In dit voorbeeld wordt de inhoud van de claim `givenName` verzonden naar de REST-service als `firstName`, de inhoud van de claim `surname` verzonden naar de REST-service als `lastName`, en `email` is verzonden. De `OutputClaims` element de claims die worden opgehaald uit de RESTful-service naar Azure AD B2C wordt gedefinieerd.

* **TechnicalProfile-Id = 'LocalAccountSignUpWithLogonEmail'**: een profiel van de technische toegevoegd aan een bestaand profiel voor technische (gedefinieerd in base beleid). Tijdens de registratie reis roept het profiel van de technische het voorgaande technische profiel. Als de RESTful-service retourneert een HTTP-fout 409 (een conflict-fout), wordt het foutbericht wordt weergegeven voor de gebruiker. 

Zoek de `<ClaimsProviders>` knooppunt, en voeg vervolgens de volgende XML-fragment onder de `<ClaimsProviders>` knooppunt:

```xml
<ClaimsProvider>
    <DisplayName>REST APIs</DisplayName>
    <TechnicalProfiles>
    
    <!-- Custom Restful service -->
    <TechnicalProfile Id="REST-API-SignUp">
        <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
        <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
        <Metadata>
        <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
        <Item Key="AuthenticationType">None</Item>
        <Item Key="SendClaimsIn">Body</Item>
        </Metadata>
        <InputClaims>
        <InputClaim ClaimTypeReferenceId="email" />
        <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="firstName" />
        <InputClaim ClaimTypeReferenceId="surname" PartnerClaimType="lastName" />
        </InputClaims>
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="loyaltyNumber" PartnerClaimType="loyaltyNumber" />
        </OutputClaims>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
    </TechnicalProfile>

<!-- Change LocalAccountSignUpWithLogonEmail technical profile to support your validation technical profile -->
    <TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="loyaltyNumber" PartnerClaimType="loyaltyNumber" />
        </OutputClaims>
        <ValidationTechnicalProfiles>
        <ValidationTechnicalProfile ReferenceId="REST-API-SignUp" />
        </ValidationTechnicalProfiles>
    </TechnicalProfile>

    </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="step-6-add-the-loyaltynumber-claim-to-your-relying-party-policy-file-so-the-claim-is-sent-to-your-application"></a>Stap 6: Toevoegen de `loyaltyNumber` claim naar het bestand met beleidsregel van relying party, zodat de claim wordt verzonden naar uw toepassing
Bewerk uw *SignUpOrSignIn.xml* relying party (RP) het bestand en het wijzigen van de TechnicalProfile-Id = 'PolicyProfile'-element om toe te voegen van de volgende: `<OutputClaim ClaimTypeReferenceId="loyaltyNumber" />`.

Nadat u de nieuwe claim toegevoegd, wordt de relying party code ziet er als volgt:

```xml
<RelyingParty>
    <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
    <TechnicalProfile Id="PolicyProfile">
        <DisplayName>PolicyProfile</DisplayName>
        <Protocol Name="OpenIdConnect" />
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        <OutputClaim ClaimTypeReferenceId="email" />
        <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
        <OutputClaim ClaimTypeReferenceId="identityProvider" />
        <OutputClaim ClaimTypeReferenceId="loyaltyNumber" DefaultValue="" />
        </OutputClaims>
        <SubjectNamingInfo ClaimType="sub" />
    </TechnicalProfile>
    </RelyingParty>
</TrustFrameworkPolicy>
```

## <a name="step-7-upload-the-policy-to-your-tenant"></a>Stap 7: Het beleid uploaden naar uw tenant

1. In de [Azure-portal](https://portal.azure.com), overschakelen naar de [context van uw Azure AD B2C-tenant](active-directory-b2c-navigate-to-b2c-context.md), en open vervolgens **Azure AD B2C**.

2. Selecteer **identiteit ervaring Framework**.

3. Open **alle beleidsregels**. 

4. Selecteer **uploaden beleid**.

5. Selecteer de **het beleid overschreven als deze bestaat** selectievakje.

6. Upload het bestand TrustFrameworkExtensions.xml en zorg ervoor dat deze gevalideerd wordt.

7. Herhaal de voorgaande stap met het bestand SignUpOrSignIn.xml.

## <a name="step-8-test-the-custom-policy-by-using-run-now"></a>Stap 8: Het aangepaste beleid testen met behulp van nu uitvoeren
1. Selecteer **Azure AD B2C-instellingen**, en ga vervolgens naar **identiteit ervaring Framework**.

    > [!NOTE]
    > **Nu uitvoeren** vereist ten minste één toepassing om te worden preregistered op de tenant. Zie voor informatie over het registreren van toepassingen, de Azure AD B2C [aan de slag](active-directory-b2c-get-started.md) artikel of de [toepassingsregistratie](active-directory-b2c-app-registration.md) artikel.

2. Open **B2C_1A_signup_signin**, de relying party (RP) aangepast-beleid die u hebt geüpload en selecteer vervolgens **nu uitvoeren**.

    ![Het venster B2C_1A_signup_signin](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-run.png)

3. Het proces testen door te typen **Test** in de **voornaam** vak.  
    Azure AD B2C wordt een foutbericht weergegeven aan de bovenkant van het venster.

    ![Het beleid testen](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-test.png)

4.  In de **voornaam** typt u een naam (met uitzondering van 'Test').  
    Azure AD B2C de gebruiker zich aanmeldt en vervolgens een loyaltyNumber verzendt naar uw toepassing. Noteer het nummer in deze JWT.

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
* Na het voltooien van de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) scenario, het is raadzaam dat u uw scenario maken met behulp van uw eigen aangepaste beleidsbestanden. We hebben opgegeven voor eigen referentie [beleid voorbeeldbestanden](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw).
* U kunt de code van de volledige downloaden [voorbeeld Visual Studio-oplossing voor verwijzing](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw/).
    
## <a name="next-steps"></a>Volgende stappen
* [Beveiligen van uw RESTful-API met basisverificatie (gebruikersnaam en wachtwoord)](active-directory-b2c-custom-rest-api-netfw-secure-basic.md)
* [Beveiligen van uw RESTful-API met clientcertificaten](active-directory-b2c-custom-rest-api-netfw-secure-cert.md)
