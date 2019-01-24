---
title: Beveiligen van uw RESTful-service met behulp van clientcertificaten in Azure Active Directory B2C | Microsoft Docs
description: Uw aangepaste claims-uitwisselingen van REST-API in uw Azure AD B2C beveiligen met behulp van clientcertificaten
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/25/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: c0a8ef9600406e94c1077436861d6578847b5ade
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54845027"
---
# <a name="secure-your-restful-service-by-using-client-certificates"></a>Beveiligen van uw RESTful-service met behulp van clientcertificaten

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In een verwant artikel u [maken van een RESTful-service](active-directory-b2c-custom-rest-api-netfw.md) die communiceert met Azure Active Directory B2C (Azure AD B2C).

In dit artikel leert u hoe u toegang tot uw Azure-web-app (RESTful-API) met behulp van een clientcertificaat te beperken. Dit mechanisme heet wederzijdse TLS-verificatie, of *verificatie van clientcertificaten*. Alleen services die u hebt de juiste certificaten, zoals Azure AD B2C, hebben toegang tot uw service.

>[!NOTE]
>U kunt ook uw RESTful-service beveiligen met behulp van [HTTP-basisverificatie](active-directory-b2c-custom-rest-api-netfw-secure-basic.md). Echter, HTTP-verificatie wordt beschouwd als minder veilig via een clientcertificaat. Onze aanbeveling is voor het beveiligen van de RESTful-service met behulp van verificatie van clientcertificaten, zoals beschreven in dit artikel.

Dit artikel wordt beschreven hoe u:
* Uw web-app ingesteld voor het gebruik van verificatie van clientcertificaten.
* Upload het certificaat naar de sleutels voor Azure AD B2C-beleid.
* Configureren van uw aangepaste beleid voor het gebruik van het clientcertificaat.

## <a name="prerequisites"></a>Vereisten
* Voer de stappen in de [REST-API integreren claims worden uitgewisseld](active-directory-b2c-custom-rest-api-netfw.md) artikel.
* Verkrijgen van een geldig certificaat (een pfx-bestand met een persoonlijke sleutel).

## <a name="step-1-configure-a-web-app-for-client-certificate-authentication"></a>Stap 1: Configureren van een web-app voor verificatie van clientcertificaten
Voor het instellen van **Azure App Service** om te vereisen dat clientcertificaten, stelt u de web-app `clientCertEnabled` site-instelling *waar*. Om deze wijziging in de Azure-portal, de webpagina van de app te openen. In het linker navigatiedeelvenster, onder **instellingen** Selecteer **SSL-instellingen**. In de **clientcertificaten** sectie, schakelt u de **binnenkomend clientcertificaat** optie.

>[!NOTE]
>Zorg ervoor dat uw Azure App Service-plan Standard of hoger. Zie voor meer informatie, [gedetailleerd overzicht van Azure App Service-plannen](https://docs.microsoft.com/azure/app-service/overview-hosting-plans).

>[!NOTE]
>Voor meer informatie over het instellen de **clientCertEnabled** eigenschap, Zie [configureren TLS wederzijdse verificatie voor web-apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-configure-tls-mutual-auth).

## <a name="step-2-upload-your-certificate-to-azure-ad-b2c-policy-keys"></a>Stap 2: Upload uw certificaat naar de sleutels voor Azure AD B2C-beleid
Nadat u hebt ingesteld `clientCertEnabled` naar *waar*, de communicatie met uw RESTful-API is een clientcertificaat vereist. Als u wilt downloaden, uploaden en opslaan van het clientcertificaat in uw Azure AD B2C-tenant, het volgende doen: 
1. Selecteer in uw Azure AD B2C-tenant, **B2C-instellingen** > **Identity-Ervaringsframework**.

2. Als u de sleutels die beschikbaar in uw tenant zijn, selecteer **Beleidssleutels**.

3. Selecteer **Toevoegen**.  
    De **maakt u een sleutel** venster wordt geopend.

4. In de **opties** Schakel **uploaden**.

5. In de **naam** in het vak **B2cRestClientCertificate**.  
    Het voorvoegsel *B2C_1A_* wordt automatisch toegevoegd.

6. In de **bestandsupload** vak, selecteert u het pfx-bestand van het certificaat met een persoonlijke sleutel.

7. In de **wachtwoord** van het certificaat wachtwoord typt.

    ![Sleutel voor het uploaden](media/aadb2c-ief-rest-api-netfw-secure-cert/rest-api-netfw-secure-client-cert-upload.png)

7. Selecteer **Maken**.

8. De sleutels die beschikbaar in uw tenant zijn weergeven en Bevestig dat u hebt gemaakt de `B2C_1A_B2cRestClientCertificate` sleutel, selecteer **Beleidssleutels**.

## <a name="step-3-change-the-technical-profile"></a>Stap 3: Het technische profiel wijzigen
Ter ondersteuning van verificatie van clientcertificaten in uw aangepast beleid, het technische profiel te wijzigen door het volgende te doen:

1. Open in uw werkmap en de *TrustFrameworkExtensions.xml* extensiebestand beleid.

2. Zoek de `<TechnicalProfile>` knooppunt met `Id="REST-API-SignUp"`.

3. Zoek de `<Metadata>` element.

4. Wijzig de *AuthenticationType* naar *ClientCertificate*, als volgt:

    ```xml
    <Item Key="AuthenticationType">ClientCertificate</Item>
    ```

5. Onmiddellijk na de afsluitende `<Metadata>` -element, Voeg het volgende XML-fragment toe: 

    ```xml
    <CryptographicKeys>
        <Key Id="ClientCertificate" StorageReferenceId="B2C_1A_B2cRestClientCertificate" />
    </CryptographicKeys>
    ```

    Nadat u het fragment hebt toegevoegd, wordt het technische profiel moet eruitzien als de volgende XML-code:

    ![ClientCertificate verificatie XML-elementen instellen](media/aadb2c-ief-rest-api-netfw-secure-cert/rest-api-netfw-secure-client-cert-tech-profile.png)

## <a name="step-4-upload-the-policy-to-your-tenant"></a>Stap 4: Uploaden van het beleid aan uw tenant

1. In de [Azure-portal](https://portal.azure.com), Ga naar de [context van uw Azure AD B2C-tenant](active-directory-b2c-navigate-to-b2c-context.md), en selecteer vervolgens **Azure AD B2C**.

2. Selecteer **Identity-Ervaringsframework**.

3. Selecteer **alle beleidsregels**.

4. Selecteer **beleid uploaden**.

5. Selecteer de **het beleid overschrijven als deze bestaat** selectievakje.

6. Upload de *TrustFrameworkExtensions.xml* bestand en zorg ervoor dat deze de validatietests doorstaat.

## <a name="step-5-test-the-custom-policy-by-using-run-now"></a>Stap 5: Het aangepaste beleid testen met behulp van nu uitvoeren
1. Open **Azure AD B2C-instellingen**, en selecteer vervolgens **Identity-Ervaringsframework**.

    >[!NOTE]
    >Voer nu vereist dat ten minste één toepassing vooraf op de tenant worden geregistreerd. Zie voor meer informatie over het registreren van toepassingen, de Azure AD B2C [aan de slag](active-directory-b2c-get-started.md) artikel of de [toepassingsregistratie](active-directory-b2c-app-registration.md) artikel.

2. Open **B2C_1A_signup_signin**, de relying party (RP) aangepast beleid u geüpload en selecteer vervolgens **nu uitvoeren**.

3. Het proces testen door te typen **Test** in de **voornaam** vak.  
    Azure AD B2C wordt een foutbericht weergegeven aan de bovenkant van het venster.    

    ![Uw identiteit API testen](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-test.png)

4. In de **voornaam** typt u een naam (met uitzondering van 'Test').  
    Azure AD B2C de gebruiker zich aanmeldt en verzendt vervolgens een getal loyaliteit naar uw toepassing. Houd rekening met het nummer in dit voorbeeld JWT:

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

   >[!NOTE]
   >Als u het foutbericht ontvangt *de naam is niet geldig, Geef een geldige naam*, betekent dit dat Azure AD B2C uw RESTful-service is aangeroepen terwijl het certificaat van de client weergegeven. De volgende stap is om het certificaat te valideren.

## <a name="step-6-add-certificate-validation"></a>Stap 6: Validatie van het servercertificaat toevoegen
Het clientcertificaat dat Azure AD B2C wordt verzonden naar uw RESTful-service heeft geen validatie ondergaan door de Azure App Service-platform, behalve om te controleren of het certificaat bestaat. Validatie van het certificaat is de verantwoordelijkheid van de web-app. 

In deze sectie voegt u ASP.NET-voorbeeldcode die de eigenschappen voor certificaat voor verificatiedoeleinden wordt gebruikt valideert.

> [!NOTE]
>Zie voor meer informatie over het configureren van Azure App Service voor verificatie van clientcertificaten [configureren TLS wederzijdse verificatie voor web-apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-configure-tls-mutual-auth).

### <a name="61-add-application-settings-to-your-projects-webconfig-file"></a>6.1 toepassingsinstellingen aan web.config-bestand van uw project toevoegen
In de Visual Studio-project dat u eerder hebt gemaakt, voeg de volgende toepassingsinstellingen op de *web.config* bestand na de `appSettings` element:

```XML
<add key="ClientCertificate:Subject" value="CN=Subject name" />
<add key="ClientCertificate:Issuer" value="CN=Issuer name" />
<add key="ClientCertificate:Thumbprint" value="Certificate thumbprint" />
```

Vervangen van het certificaat **onderwerpnaam**, **verlenernaam**, en **certificaatvingerafdruk** waarden met de certificaatwaarden van uw.

### <a name="62-add-the-isvalidclientcertificate-function"></a>6.2 de IsValidClientCertificate-functie toevoegen
Open de *Controllers\IdentityController.cs* bestand en vervolgens toevoegen aan de `Identity` controller klasse de volgende functie: 

```csharp
private bool IsValidClientCertificate()
{
    string ClientCertificateSubject = ConfigurationManager.AppSettings["ClientCertificate:Subject"];
    string ClientCertificateIssuer = ConfigurationManager.AppSettings["ClientCertificate:Issuer"];
    string ClientCertificateThumbprint = ConfigurationManager.AppSettings["ClientCertificate:Thumbprint"];

    X509Certificate2 clientCertInRequest = RequestContext.ClientCertificate;
    if (clientCertInRequest == null)
    {
        Trace.WriteLine("Certificate is NULL");
        return false;
    }

    // Basic verification
    if (clientCertInRequest.Verify() == false)
    {
        Trace.TraceError("Basic verification failed");
        return false;
    }

    // 1. Check the time validity of the certificate
    if (DateTime.Compare(DateTime.Now, clientCertInRequest.NotBefore) < 0 ||
        DateTime.Compare(DateTime.Now, clientCertInRequest.NotAfter) > 0)
    {
        Trace.TraceError($"NotBefore '{clientCertInRequest.NotBefore}' or NotAfter '{clientCertInRequest.NotAfter}' not valid");
        return false;
    }

    // 2. Check the subject name of the certificate
    bool foundSubject = false;
    string[] certSubjectData = clientCertInRequest.Subject.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
    foreach (string s in certSubjectData)
    {
        if (String.Compare(s.Trim(), ClientCertificateSubject) == 0)
        {
            foundSubject = true;
            break;
        }
    }

    if (!foundSubject)
    {
        Trace.TraceError($"Subject name '{clientCertInRequest.Subject}' is not valid");
        return false;
    }
    
    // 3. Check the issuer name of the certificate
    bool foundIssuerCN = false;
    string[] certIssuerData = clientCertInRequest.Issuer.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
    foreach (string s in certIssuerData)
    {
        if (String.Compare(s.Trim(), ClientCertificateIssuer) == 0)
        {
            foundIssuerCN = true;
            break;
        }
    }

    if (!foundIssuerCN)
    {
        Trace.TraceError($"Issuer '{clientCertInRequest.Issuer}' is not valid");
        return false;
    }

    // 4. Check the thumbprint of the certificate
    if (String.Compare(clientCertInRequest.Thumbprint.Trim().ToUpper(), ClientCertificateThumbprint) != 0)
    {
        Trace.TraceError($"Thumbprint '{clientCertInRequest.Thumbprint.Trim().ToUpper()}' is not valid");
        return false;
    }

    // 5. If you also want to test whether the certificate chains to a trusted root authority, you can uncomment the following code:
    //
    //X509Chain certChain = new X509Chain();
    //certChain.Build(certificate);
    //bool isValidCertChain = true;
    //foreach (X509ChainElement chElement in certChain.ChainElements)
    //{
    //    if (!chElement.Certificate.Verify())
    //    {
    //        isValidCertChain = false;
    //        break;
    //    }
    //}
    //if (!isValidCertChain) return false;
    return true;
}
```

In de bovenstaande voorbeeldcode accepteren we het certificaat zoals is alleen geldig als de volgende voorwaarden wordt voldaan:
* Het certificaat niet is verlopen en de huidige tijd op de server actief is.
* De `Subject` naam van het certificaat is gelijk aan de `ClientCertificate:Subject` waarde van toepassing.
* De `Issuer` naam van het certificaat is gelijk aan de `ClientCertificate:Issuer` waarde van toepassing.
* De `thumbprint` van het certificaat is gelijk aan de `ClientCertificate:Thumbprint` waarde van toepassing.

>[!IMPORTANT]
>Afhankelijk van de vertrouwelijkheid van uw service moet u mogelijk meer validaties toevoegen. Bijvoorbeeld, als u wilt testen of het certificaat wordt gekoppeld aan een vertrouwde basis-CA, validatie van de verlener organisatie pakketnaam, enzovoort.

### <a name="63-call-the-isvalidclientcertificate-function"></a>6.3 de IsValidClientCertificate functie aanroepen
Open de *Controllers\IdentityController.cs* bestand en vervolgens aan het begin van de `SignUp()` functie, Voeg het volgende codefragment toe: 

```csharp
if (IsValidClientCertificate() == false)
{
    return Content(HttpStatusCode.Conflict, new B2CResponseContent("Your client certificate is not valid", HttpStatusCode.Conflict));
}
```

Nadat u het codefragment toevoegen uw `Identity` controller moet eruitzien als de volgende code:

![Certificaat validatiecode toevoegen](media/aadb2c-ief-rest-api-netfw-secure-cert/rest-api-netfw-secure-client-code.png)

## <a name="step-7-publish-your-project-to-azure-and-test-it"></a>Stap 7: Uw project kunt publiceren naar Azure en kunt u deze testen
1. In **Solution Explorer**, met de rechtermuisknop op de **Contoso.AADB2C.API** project en selecteer vervolgens **publiceren**.

2. Herhaal de 'Stap 6' en het aangepaste beleid met de validatie van het servercertificaat opnieuw testen. Probeer het uitvoeren van het beleid en zorg ervoor dat alles werkt nadat u de validatie hebt toegevoegd.

3. In uw *web.config* bestand, wijzig de waarde van `ClientCertificate:Subject` naar **ongeldig**. Het beleid voor opnieuw uitvoeren en ziet u een foutbericht weergegeven.

4. Wijzig de waarde weer **geldig**, en zorg ervoor dat het beleid voor de REST-API kunt aanroepen.

Als u nodig hebt om op te lossen in deze stap ziet [verzamelen van logboeken met behulp van Application Insights](active-directory-b2c-troubleshoot-custom.md).

## <a name="optional-download-the-complete-policy-files-and-code"></a>(Optioneel) De volledige beleidsbestanden en de code downloaden
* Na het voltooien van de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) scenario, het is raadzaam dat u uw scenario bouwen met behulp van uw eigen aangepaste beleidsbestanden. Ter referentie, we hebben opgegeven [beleid voorbeeldbestanden](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw-secure-cert).
* U kunt de code van de volledige downloaden [voorbeeld Visual Studio-oplossing voor verwijzing](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw/Contoso.AADB2C.API). 
