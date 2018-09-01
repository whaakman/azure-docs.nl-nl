---
title: Integratie van REST-API claim uitwisselingen in uw Azure Active Directory B2C de gebruikersbeleving | Microsoft Docs
description: Integreer claim worden uitgewisseld REST-API in uw Azure AD B2C de gebruikersbeleving als validatie van de invoer van de gebruiker.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/30/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e3d938c4464fc5141b97f85220bf096920e17d00
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43339590"
---
# <a name="integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-validation-of-user-input"></a>Integreer claims worden uitgewisseld REST-API in uw Azure AD B2C de gebruikersbeleving als validatie van de gebruikersinvoer

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Met de Identiteitservaring-Framework, welke opslagstructuur achter Azure Active Directory B2C (Azure AD B2C), die u kunt integreren met een RESTful-API in een gebruikersbeleving. In dit scenario leert u hoe Azure AD B2C communiceert met .NET Framework RESTful-services (web-API).

## <a name="introduction"></a>Inleiding
Met behulp van Azure AD B2C, kunt u uw eigen bedrijfslogica toevoegen aan een gebruikersbeleving door uw eigen RESTful-service aan te roepen. De Identity-Ervaringsframework verzendt gegevens naar de RESTful-service in een *invoer claims* verzameling en ontvangt gegevens van RESTful in een *uitvoer claims* verzameling. Met integratie van RESTful-service, kunt u het volgende doen:

* **Valideren van de gebruiker ingevoerde gegevens**: deze actie voorkomt dat onjuist gevormde gegevens opslaan op Azure AD. Als de waarde van de gebruiker niet geldig is, retourneert de RESTful-service een foutbericht weergegeven waarin de gebruiker voor een vermelding wordt geïnstrueerd. U kunt bijvoorbeeld controleren of het e-mailadres dat is opgegeven door de gebruiker in de database van uw klant bestaat.
* **Invoerclaims overschrijven**: bijvoorbeeld, als een gebruiker de eerste naam in hoofdletters of hoofdletters letters invoert, u kunt de naam opmaken met alleen de eerste letter in hoofdletters zijn.
* **Gebruikersgegevens verrijken door verder integreren met zakelijke line-of-business-toepassingen**: uw RESTful-service kan ontvangen van e-mailadres van de gebruiker, query uitvoeren op database van de klant en loyaliteit-nummer van de gebruiker terug naar Azure AD B2C. Het rendement claims kunnen worden opgeslagen in een van de gebruiker Azure AD-account, geëvalueerd in de volgende *Indelingsstappen*, of opgenomen in het toegangstoken.
* **Uitvoeren van aangepaste bedrijfslogica**: U kunt pushmeldingen te verzenden, zakelijke databases bijwerken, uitvoeren van het migratieproces van een gebruiker, machtigingen beheren, controleren van databases en andere acties worden uitgevoerd.

U kunt de integratie met de RESTful-services ontwerpen in de volgende manieren:

* **Validatie van technisch profiel**: de aanroep van de RESTful-service gebeurt binnen het technische profiel van de validatie van het opgegeven technisch profiel. Het technische validatieprofiel valideert de gegevens van de gebruiker opgegeven voordat de gebruikersbeleving doorsturen. Met het technische validatieprofiel kunt u het volgende doen:
   * Invoer claims verzenden.
   * Valideer de invoerclaims en genereren van aangepaste foutberichten.
   * Back-uitvoerclaims verzenden.

* **Exchange-claims**: dit ontwerp is vergelijkbaar met het technische validatieprofiel, maar dit gebeurt in een orchestration-stap. Deze definitie is beperkt tot:
   * Invoer claims verzenden.
   * Back-uitvoerclaims verzenden.

## <a name="restful-walkthrough"></a>RESTful-overzicht
In dit scenario maakt ontwikkelen u een .NET Framework-web-API die de invoer van de gebruiker wordt gevalideerd en biedt een aantal van de loyaliteit gebruiker. Bijvoorbeeld: uw toepassing toegang kan verlenen tot *platinum voordelen* gebaseerd op het aantal loyaliteit.

Overzicht:
* Ontwikkelen met de RESTful-service (.NET Framework web-API).
* Gebruik de RESTful-service in de gebruikersbeleving.
* Invoerclaims verzenden en lezen in uw code.
* Valideer de voornaam van de gebruiker.
* Een getal loyaliteit terugsturen. 
* Het aantal loyaliteit naar een JSON Web Token (JWT) toevoegen.

## <a name="prerequisites"></a>Vereisten
Voer de stappen in de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) artikel.

## <a name="step-1-create-an-aspnet-web-api"></a>Stap 1: Een ASP.NET-web-API maken

1. Maak in Visual Studio een project door te selecteren **bestand** > **nieuw** > **Project**.

2. In de **nieuw Project** venster **Visual C#** > **Web** > **ASP.NET-webtoepassing (.NET Framework)**.

3. In de **naam** typt u een naam voor de toepassing (bijvoorbeeld *Contoso.AADB2C.API*), en selecteer vervolgens **OK**.

    ![Nieuwe visual studio-project maken](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-create-project.png)

4. In de **nieuwe ASP.NET-webtoepassing** venster een **Web-API** of **Azure API-app** sjabloon.

    ![Selecteer de sjabloon web-API](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-select-web-api.png)

5. Zorg ervoor dat de verificatie is ingesteld op **geen verificatie**.

6. Selecteer **OK** om het project te maken.

## <a name="step-2-prepare-the-rest-api-endpoint"></a>Stap 2: Bereid de REST API-eindpunt

### <a name="step-21-add-data-models"></a>Stap 2.1: Gegevensmodellen toevoegen
De modellen vertegenwoordigen de invoerclaims en uitvoer claims gegevens in uw RESTful-service. Uw code leest de ingevoerde gegevens met het deserialiseren van het model invoerclaims vanuit een JSON-tekenreeks voor een C#-object (uw model). De ASP.NET-web-API automatisch gedeserialiseerd het model van de claims uitvoer terug naar JSON en vervolgens de geserialiseerde gegevens schrijft naar het hoofdgedeelte van de HTTP-antwoordbericht. 

Maak een model dat invoerclaims vertegenwoordigt door het volgende te doen:

1. Als u Solution Explorer nog niet is geopend, selecteert u **weergave** > **Solution Explorer**. 
2. Klik in Solution Explorer met de rechtermuisknop op de map **Modellen** en selecteer achtereenvolgens **Toevoegen** en **Klasse**.

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

5. Maken van een meer model, `B2CResponseContent`, waarmee u kunt de invoer-validatiefout van de berichten genereren. Voeg de volgende eigenschappen voor de `B2CResponseContent` klasse, de ontbrekende verwijzingen bieden en sla het bestand:

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

### <a name="step-22-add-a-controller"></a>Stap 2.2: Een controller toevoegen
In de web-API, een _controller_ is een object dat HTTP-aanvragen worden verwerkt. De controller retourneert uitvoer claims of, als de eerste naam niet geldig is is, genereert een Conflict HTTP-foutmelding.

1. Klik in Solution Explorer met de rechtermuisknop op de map **Controllers** en selecteer achtereenvolgens **Toevoegen** en **Controller**.

    ![Nieuwe controller toevoegen](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-add-controller-1.png)

2. In de **Add Scaffold** venster **Web API-Controller - leeg**, en selecteer vervolgens **toevoegen**.

    ![Selecteer Web API 2-Controller - leeg](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-add-controller-2.png)

3. In de **Controller toevoegen** venster de naam van de controller **IdentityController**, en selecteer vervolgens **toevoegen**.

    ![Typ de naam van de domeincontroller](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-add-controller-3.png)

    Het hulpprogramma maakt een bestand met de naam *IdentityController.cs* in de *Controllers* map.

4. Als de *IdentityController.cs* bestand nog niet open is, dubbelklikt u op en klikt u vervolgens de code in het bestand vervangen door de volgende code:

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

2. In de **publiceren** venster **Microsoft Azure App Service**, en selecteer vervolgens **publiceren**.

    ![Nieuwe Microsoft Azure App Service maken](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-publish-to-azure-2.png)

    De **Create App Service** venster wordt geopend. In deze maakt u alle benodigde Azure-resources voor het uitvoeren van de ASP.NET-web-app in Azure.

    > [!NOTE]
    >Zie voor meer informatie over hoe u publiceert, [een ASP.NET-web-app maken in Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet#publish-to-azure).

3. In de **Web-Appnaam** typt u een unieke app-naam (geldige tekens zijn a-z, 0-9 en afbreekstreepjes (-). De URL van de web-app is http://<APP_NAME>.azurewebsites.NET, waar *app_name* is de naam van uw web-app. U kunt de automatisch gegenereerde naam accepteren, die uiteraard uniek is.

    ![Geef eigenschappen voor de App Service](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-publish-to-azure-3.png)

4. Als u wilt beginnen met het maken van Azure-resources, selecteer **maken**.  
    Nadat de ASP.NET-web-app is gemaakt, wordt de wizard naar Azure gepubliceerd en start u de app in de standaardbrowser.

6. Kopieer de URL van de web-app.

## <a name="step-4-add-the-new-loyaltynumber-claim-to-the-schema-of-your-trustframeworkextensionsxml-file"></a>Stap 4: De nieuwe toevoegen `loyaltyNumber` claim met het schema van het bestand TrustFrameworkExtensions.xml
De `loyaltyNumber` claim nog niet is gedefinieerd in onze schema. Een definitie in de `<BuildingBlocks>` element, dat u aan het begin van vindt de *TrustFrameworkExtensions.xml* bestand.

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
Elke claimprovider beschikken over een of meer technische profielen die bepalen de eindpunten en protocollen die nodig zijn om te communiceren met de claimprovider. 

Een claimprovider kan meerdere technische profielen om verschillende redenen hebben. Meerdere technische profielen kunnen bijvoorbeeld worden gedefinieerd omdat de claimprovider meerdere protocollen ondersteunt, eindpunten verschillende mogelijkheden hebben kunnen of releases claims die verschillende niveaus van zekerheid hebben kunnen bevatten. Het kan zijn aanvaardbaar is voor het vrijgeven van gevoelige claims in een gebruikersbeleving maar niet in een andere. 

De volgende XML-fragment bevat een claims provider-knooppunt met twee technische profielen:

* **Technische profiel-Id = "REST-API-SignUp"**: uw RESTful-service definieert. 
   * `Proprietary` wordt beschreven als protocol voor een op basis van een RESTful-provider. 
   * `InputClaims` Hiermee definieert u de claims die naar de REST-service van Azure AD B2C worden verzonden. 

   In dit voorbeeld wordt de inhoud van de claim `givenName` verzendt naar de REST-service als `firstName`, de inhoud van de claim `surname` verzendt naar de REST-service als `lastName`, en `email` is verzonden. De `OutputClaims` element wordt gedefinieerd voor de claims die worden opgehaald uit RESTful-service naar Azure AD B2C.

* **Technische profiel-Id = "LocalAccountSignUpWithLogonEmail"**: een technisch validatieprofiel toegevoegd aan een bestaand technische profiel (gedefinieerd in het Basisbeleid). Tijdens de registratie reis roept het technische profiel van de validatie van de voorgaande technisch profiel. Als de RESTful-service retourneert een HTTP-fout 409 (een conflict-fout), wordt het foutbericht wordt weergegeven aan de gebruiker. 

Zoek de `<ClaimsProviders>` knooppunt, en voeg de volgende XML-fragment uit onder de `<ClaimsProviders>` knooppunt:

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

## <a name="step-6-add-the-loyaltynumber-claim-to-your-relying-party-policy-file-so-the-claim-is-sent-to-your-application"></a>Stap 6: Voeg de `loyaltyNumber` claim naar uw relying party beleidsbestand dat u dus de claim worden verzonden naar uw toepassing
Bewerk uw *SignUpOrSignIn.xml* relying party (RP) het bestand en het wijzigen van het technische profiel-Id = 'PolicyProfile'-element om toe te voegen van de volgende: `<OutputClaim ClaimTypeReferenceId="loyaltyNumber" />`.

Nadat u de nieuwe claim toegevoegd, ziet de relying party-code er als volgt uit:

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

## <a name="step-7-upload-the-policy-to-your-tenant"></a>Stap 7: Het beleid voor uploaden naar uw tenant

1. In de [Azure-portal](https://portal.azure.com), Ga naar de [context van uw Azure AD B2C-tenant](active-directory-b2c-navigate-to-b2c-context.md), en open vervolgens **Azure AD B2C**.

2. Selecteer **Identity-Ervaringsframework**.

3. Open **alle beleidsregels**. 

4. Selecteer **beleid uploaden**.

5. Selecteer de **het beleid overschrijven als deze bestaat** selectievakje.

6. Upload het bestand TrustFrameworkExtensions.xml en zorg ervoor dat deze de validatietests doorstaat.

7. Herhaal de vorige stap met het bestand SignUpOrSignIn.xml.

## <a name="step-8-test-the-custom-policy-by-using-run-now"></a>Stap 8: Het aangepaste beleid testen met behulp van nu uitvoeren
1. Selecteer **Azure AD B2C-instellingen**, en ga vervolgens naar **Identity-Ervaringsframework**.

    > [!NOTE]
    > **Nu uitvoeren** vereist ten minste één toepassing vooraf op de tenant worden geregistreerd. Zie voor meer informatie over het registreren van toepassingen, de Azure AD B2C [aan de slag](active-directory-b2c-get-started.md) artikel of de [toepassingsregistratie](active-directory-b2c-app-registration.md) artikel.

2. Open **B2C_1A_signup_signin**, de relying party (RP) aangepast beleid u geüpload en selecteer vervolgens **nu uitvoeren**.

    ![Het venster B2C_1A_signup_signin](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-run.png)

3. Het proces testen door te typen **Test** in de **voornaam** vak.  
    Azure AD B2C wordt een foutbericht weergegeven aan de bovenkant van het venster.

    ![Het beleid testen](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-test.png)

4.  In de **voornaam** typt u een naam (met uitzondering van 'Test').  
    Azure AD B2C de gebruiker zich aanmeldt en verzendt vervolgens een loyaltyNumber naar uw toepassing. Noteer het nummer in deze JWT.

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
* Na het voltooien van de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) scenario, het is raadzaam dat u uw scenario bouwen met behulp van uw eigen aangepaste beleidsbestanden. Ter referentie, we hebben opgegeven [beleid voorbeeldbestanden](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw).
* U kunt de code van de volledige downloaden [voorbeeld Visual Studio-oplossing voor verwijzing](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw/).
    
## <a name="next-steps"></a>Volgende stappen
* [Beveilig uw RESTful API met basisverificatie (gebruikersnaam en wachtwoord)](active-directory-b2c-custom-rest-api-netfw-secure-basic.md)
* [Beveilig uw RESTful API met clientcertificaten](active-directory-b2c-custom-rest-api-netfw-secure-cert.md)
