---
title: 'Azure Active Directory B2C: Uw RESTful-service beveiligen met behulp van clientcertificaten'
description: Uw aangepaste REST-API claims kunnen worden uitgewisseld in uw Azure AD B2C beveiligen met behulp van clientcertificaten
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
ms.openlocfilehash: 582aadd35821779e307ac285804e3b7fe5c24abd
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="secure-your-restful-service-by-using-client-certificates"></a>Beveiligen van uw RESTful-service met behulp van clientcertificaten
In een verwant artikel u [maken een RESTful-service](active-directory-b2c-custom-rest-api-netfw.md) die communiceert met Azure Active Directory B2C (Azure AD B2C).

In dit artikel leert u het beperken van toegang tot uw Azure-web-app (RESTful-API) met behulp van een clientcertificaat. Dit mechanisme heet TLS wederzijdse verificatie, of *verificatie van clientcertificaten*. Alleen services die de juiste certificaten, zoals Azure AD B2C, hebben toegang tot uw service.

>[!NOTE]
>U kunt ook uw RESTful-service beveiligen met behulp van [HTTP basisverificatie](active-directory-b2c-custom-rest-api-netfw-secure-basic.md). Echter, HTTP-basisverificatie wordt beschouwd als minder veilig via een clientcertificaat. Onze aanbeveling is het beveiligen van de RESTful-service met behulp van verificatie van clientcertificaten, zoals beschreven in dit artikel.

Details van deze procedure:
* Uw web-app instellen voor gebruik van verificatie van clientcertificaten.
* Het certificaat uploaden naar Azure AD B2C beleid sleutels.
* Configureer het aangepaste beleid voor het gebruik van het clientcertificaat.

## <a name="prerequisites"></a>Vereisten
* Voer de stappen in de [REST-API integreren claims kunnen worden uitgewisseld](active-directory-b2c-custom-rest-api-netfw.md) artikel.
* Verkrijgen van een geldig certificaat (een .pfx-bestand met een persoonlijke sleutel).

## <a name="step-1-configure-a-web-app-for-client-certificate-authentication"></a>Stap 1: Een web-app voor verificatie van clientcertificaten configureren
Voor het instellen van **Azure App Service** clientcertificaten vereist, stelt u de web-app `clientCertEnabled` site-instelling *true*. Om deze wijziging aanbrengt, moet u de REST-API. De instelling is beschikbaar via de beheerervaring in de Azure portal. Het vinden van de instelling op van de toepassing van de RESTful **instellingen** menu onder **ontwikkelingsprogramma's**, selecteer **Resource Explorer**.

>[!NOTE]
>Zorg ervoor dat uw Azure App Service-abonnement Standard of hoger is. Zie voor meer informatie [gedetailleerd overzicht van Azure App Service-plannen](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).


Gebruik [Azure Resourceverkenner (Preview)](https://resources.azure.com) om in te stellen de **clientCertEnabled** eigenschap *true*, zoals wordt weergegeven in de volgende afbeelding:

![Instelling clientCertEnabled via Azure Resource Explorer](media/aadb2c-ief-rest-api-netfw-secure-cert/rest-api-netfw-secure-client-cert-resource-explorer.png)

>[!NOTE]
>Voor meer informatie over het instellen de **clientCertEnabled** eigenschap, Zie [wederzijdse verificatie TLS configureren voor web-apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-configure-tls-mutual-auth).

>[!TIP]
>U kunt ook eenvoudiger worden opgesteld van de REST-API-aanroep, kunt u de [ARMClient](https://github.com/projectkudu/ARMClient) hulpprogramma.

## <a name="step-2-upload-your-certificate-to-azure-ad-b2c-policy-keys"></a>Stap 2: Uw certificaat uploaden naar Azure AD B2C beleid sleutels
Nadat u hebt ingesteld `clientCertEnabled` naar *true*, de communicatie met uw RESTful-API is een clientcertificaat vereist. Opslaan van het clientcertificaat in uw Azure AD B2C-tenant verkrijgen en uploaden, het volgende doen: 
1. Selecteer in uw Azure AD B2C-tenant, **B2C-instellingen** > **identiteit ervaring Framework**.

2. Als u wilt weergeven van de sleutels die beschikbaar in uw tenant zijn, selecteer **beleid sleutels**.

3. Selecteer **Toevoegen**.  
    De **maakt u een sleutel** venster wordt geopend.

4. In de **opties** de optie **uploaden**.

5. In de **naam** in het vak **B2cRestClientCertificate**.  
    Het voorvoegsel *B2C_1A_* automatisch is toegevoegd.

6. In de **uploaden bestand** Selecteer pfx-bestand van het certificaat met een persoonlijke sleutel.

7. In de **wachtwoord** typt u het wachtwoord voor het certificaat.

    ![Beleidssleutel uploaden](media/aadb2c-ief-rest-api-netfw-secure-cert/rest-api-netfw-secure-client-cert-upload.png)

7. Selecteer **Maken**.

8. De sleutels die beschikbaar in uw tenant zijn weergeven en controleer of u hebt gemaakt de `B2C_1A_B2cRestClientCertificate` key, selecteer **beleid sleutels**.

## <a name="step-3-change-the-technical-profile"></a>Stap 3: Het technische profiel wijzigen
Ter ondersteuning van verificatie van clientcertificaten in het aangepaste beleid, de technische profiel te wijzigen als volgt:

1. Open in uw werkmap, de *TrustFrameworkExtensions.xml* extensiebestand beleid.

2. Zoeken naar de `<TechnicalProfile>` knooppunt met `Id="REST-API-SignUp"`.

3. Zoek de `<Metadata>` element.

4. Wijzig de *AuthenticationType* naar *ClientCertificate*als volgt:

    ```xml
    <Item Key="AuthenticationType">ClientCertificate</Item>
    ```

5. Direct na de afsluitende `<Metadata>` element, het volgende XML-fragment toevoegen: 

    ```xml
    <CryptographicKeys>
        <Key Id="ClientCertificate" StorageReferenceId="B2C_1A_B2cRestClientCertificate" />
    </CryptographicKeys>
    ```

    Nadat u het codefragment toevoegt, wordt uw technische profiel moet eruitzien als in de volgende XML-code:

    ![XML-elementen van ClientCertificate verificatie instellen](media/aadb2c-ief-rest-api-netfw-secure-cert/rest-api-netfw-secure-client-cert-tech-profile.png)

## <a name="step-4-upload-the-policy-to-your-tenant"></a>Stap 4: Het beleid uploaden naar uw tenant

1. In de [Azure-portal](https://portal.azure.com), overschakelen naar de [context van uw Azure AD B2C-tenant](active-directory-b2c-navigate-to-b2c-context.md), en selecteer vervolgens **Azure AD B2C**.

2. Selecteer **identiteit ervaring Framework**.

3. Selecteer **alle beleidsregels**.

4. Selecteer **uploaden beleid**.

5. Selecteer de **het beleid overschreven als deze bestaat** selectievakje.

6. Upload de *TrustFrameworkExtensions.xml* bestand en controleer vervolgens dat deze gevalideerd wordt.

## <a name="step-5-test-the-custom-policy-by-using-run-now"></a>Stap 5: Het aangepaste beleid testen met behulp van nu uitvoeren
1. Open **Azure AD B2C-instellingen**, en selecteer vervolgens **identiteit ervaring Framework**.

    >[!NOTE]
    >Uitvoeren is nu vereist ten minste één toepassing om te worden preregistered op de tenant. Zie voor informatie over het registreren van toepassingen, de Azure AD B2C [aan de slag](active-directory-b2c-get-started.md) artikel of de [toepassingsregistratie](active-directory-b2c-app-registration.md) artikel.

2. Open **B2C_1A_signup_signin**, de relying party (RP) aangepast-beleid die u hebt geüpload en selecteer vervolgens **nu uitvoeren**.

3. Het proces testen door te typen **Test** in de **voornaam** vak.  
    Azure AD B2C wordt een foutbericht weergegeven aan de bovenkant van het venster.    

    ![Uw identiteit API testen](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-test.png)

4. In de **voornaam** typt u een naam (met uitzondering van 'Test').  
    Azure AD B2C de gebruiker zich aanmeldt en stuurt vervolgens een getal loyaliteit aan uw toepassing. Houd rekening met het nummer in dit voorbeeld JWT:

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

   >[!NOTE]
   >Als u het foutbericht ontvangt *de naam is niet geldig, Geef een geldige naam*, betekent dit dat Azure AD B2C RESTful-service is aangeroepen terwijl het certificaat van de client gepresenteerd. De volgende stap is het certificaat te valideren.

## <a name="step-6-add-certificate-validation"></a>Stap 6: Validatie van het servercertificaat toevoegen
Het clientcertificaat dat Azure AD B2C naar uw RESTful-service verzendt heeft geen validatie ondergaat door het platform Azure Web Apps behalve om te controleren of het certificaat bestaat. Validatie van het certificaat is de verantwoordelijkheid van de web-app. 

In deze sectie voegt u ASP.NET voorbeeldcode die de eigenschappen voor certificaat voor verificatie valideert.

> [!NOTE]
>Zie voor meer informatie over het configureren van Azure App Service voor verificatie van clientcertificaten [wederzijdse verificatie TLS configureren voor web-apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-configure-tls-mutual-auth).

### <a name="61-add-application-settings-to-your-projects-webconfig-file"></a>6.1 toepassingsinstellingen toevoegen aan uw project web.config-bestand
In de Visual Studio-project dat u eerder hebt gemaakt, voegt u de volgende toepassingsinstellingen op de *web.config* bestand na de `appSettings` element:

```XML
<add key="ClientCertificate:Subject" value="CN=Subject name" />
<add key="ClientCertificate:Issuer" value="CN=Issuer name" />
<add key="ClientCertificate:Thumbprint" value="Certificate thumbprint" />
```

Vervang het certificaat **onderwerpnaam**, **certificaatverlener**, en **certificaatvingerafdruk** waarden met de waarden van uw certificaat.

### <a name="62-add-the-isvalidclientcertificate-function"></a>6.2 de IsValidClientCertificate-functie toevoegen
Open de *Controllers\IdentityController.cs* bestand en voeg vervolgens toe aan de `Identity` controller klasse met de volgende functie: 

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

In de bovenstaande voorbeeldcode accepteren we het certificaat als alleen geldig als de volgende voorwaarden wordt voldaan:
* Het certificaat niet is verlopen en voor de huidige tijd op de server actief is.
* De `Subject` naam van het certificaat is gelijk aan de `ClientCertificate:Subject` waarde van toepassingsinstelling.
* De `Issuer` naam van het certificaat is gelijk aan de `ClientCertificate:Issuer` waarde van toepassingsinstelling.
* De `thumbprint` van het certificaat is gelijk aan de `ClientCertificate:Thumbprint` waarde van toepassingsinstelling.

>[!IMPORTANT]
>Afhankelijk van de vertrouwelijkheid van uw service moet u mogelijk meer validaties toevoegen. U wilt testen of het certificaat wordt gekoppeld aan een vertrouwde basis-CA, validatie van de verlener organisatie naam, enzovoort.

### <a name="63-call-the-isvalidclientcertificate-function"></a>6.3 Roep de functie IsValidClientCertificate
Open de *Controllers\IdentityController.cs* bestand en vervolgens aan het begin van de `SignUp()` werkt, Voeg het volgende codefragment toe: 

```csharp
if (IsValidClientCertificate() == false)
{
    return Content(HttpStatusCode.Conflict, new B2CResponseContent("Your client certificate is not valid", HttpStatusCode.Conflict));
}
```

Nadat u het codefragment toevoegen uw `Identity` controller moet eruitzien als in de volgende code:

![Certificaat validatiecode toevoegen](media/aadb2c-ief-rest-api-netfw-secure-cert/rest-api-netfw-secure-client-code.png)

## <a name="step-7-publish-your-project-to-azure-and-test-it"></a>Stap 7: Uw project publiceren naar Azure en testen
1. In **Solution Explorer**, met de rechtermuisknop op de **Contoso.AADB2C.API** project en selecteer vervolgens **publiceren**.

2. Herhaal 'Stap 6' en het aangepaste beleid met de validatie van het certificaat opnieuw te testen. Probeer het uitvoeren van het beleid en zorg ervoor dat alles werkt nadat u de validatie toevoegen.

3. In uw *web.config* bestand, wijzig de waarde van `ClientCertificate:Subject` naar **ongeldig**. Het beleid voor opnieuw uitvoeren en ziet u een foutbericht weergegeven.

4. Wijzig de waarde terug naar **geldig**, en zorg ervoor dat het beleid uw REST API kan aanroepen.

Als u deze stap oplossen moet, raadpleegt u [verzamelen van logboeken met behulp van Application Insights](active-directory-b2c-troubleshoot-custom.md).

## <a name="optional-download-the-complete-policy-files-and-code"></a>(Optioneel) De volledige-beleidsbestanden en code downloaden
* Na het voltooien van de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) scenario, het is raadzaam dat u uw scenario maken met behulp van uw eigen aangepaste beleidsbestanden. We hebben opgegeven voor eigen referentie [beleid voorbeeldbestanden](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw-secure-cert).
* U kunt de code van de volledige downloaden [voorbeeld Visual Studio-oplossing voor verwijzing](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw/Contoso.AADB2C.API). 
