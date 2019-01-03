---
title: Een technische SAML-profiel te definiëren in een aangepast beleid in Azure Active Directory B2C | Microsoft Docs
description: Definieer een technische SAML-profiel in een aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 12/21/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: ff9d51a96335af110aa0377c7db57ae665c44893
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53970522"
---
# <a name="define-a-saml-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Een technische SAML-profiel te definiëren in een aangepast beleid voor Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory (Azure AD) B2C biedt ondersteuning voor de SAML 2.0-id-provider. Dit artikel beschrijft de details van een technisch profiel voor interactie met een claimprovider die ondersteuning biedt voor dit gestandaardiseerde protocol. Met een technische SAML-profiel federeren u met een SAML gebaseerde id-provider, bijvoorbeeld [ADFS](active-directory-b2c-custom-setup-adfs2016-idp.md) en [Salesforce](active-directory-b2c-setup-sf-app-custom.md). Deze federation kan uw gebruikers melden zich aan met hun bestaande sociale of ondernemings-id's.

## <a name="metadata-exchange"></a>Uitwisseling van metagegevens

De metagegevens zijn gegevens die in het SAML-protocol wordt gebruikt om beschikbaar te stellen de configuratie van een SAML-partij, zoals een serviceprovider of de id-provider. Metagegevens van definieert de locatie van de services, zoals certificaten voor aanmelden en afmelden, en aanmeldingsmethode. De id-provider maakt gebruik van de metagegevens te weten hoe u om te communiceren met Azure AD B2C. De metagegevens is geconfigureerd in de XML-indeling en kan worden ondertekend met een digitale handtekening, zodat de andere partij kunt controleren of de integriteit van de metagegevens. Wanneer Azure AD B2C met SAML-identiteitsprovider federeert, fungeert deze als een serviceprovider een SAML-aanvraag initiëren en wachten op een SAML-antwoord. En in sommige gevallen, accepteert ongevraagde SAML-verificatie, die ook wel bekend als id-provider is gestart. 

De metagegevens kan worden geconfigureerd in beide partijen als 'Statische metagegevens' of 'Dynamische metagegevens'. In de statische modus, Kopieer de gehele metagegevens van de ene partij en stel deze in de andere partij. In de dynamische modus stelt u de URL in de metagegevens van de terwijl de andere partij de configuratie van de dynamisch leest. De beginselen zijn hetzelfde, instellen van de metagegevens van het technische profiel van de Azure AD B2C in uw id-provider en de metagegevens van de id-provider instellen in Azure AD B2C.

Elke SAML-identiteitsprovider heeft verschillende stappen voor het beschikbaar stellen en het instellen van de serviceprovider in dit geval Azure AD B2C en de metagegevens van de Azure AD B2C instellen in de id-provider. Bekijk documentatie van uw id-provider voor instructies over om dit te doen.

Het volgende voorbeeld ziet u een URL-adres in de SAML-metagegevens van het technische profiel van een Azure AD B2C:

```
https://your-tenant-name.b2clogin.com/your-tenant-name/your-policy/samlp/metadata?idptp=your-technical-profile
```

Vervang de volgende waarden:

- **uw tenant-naam** met de naam van uw tenant, zoals fabrikam.b2clogin.com.
- **uw beleid** met de naam van uw beleid. Gebruik het beleid dat is waar u het technische SAML-provider-profiel configureren, of een beleidsregel die eigenschappen overneemt van dat beleid.
- **uw op technisch profiel** met de naam van de provider technisch profiel voor uw SAML-identiteit.

## <a name="digital-signing-certificates-exchange"></a>Digitale ondertekening certificaten exchange

Voor het bouwen van een vertrouwensrelatie tussen Azure AD B2C en de SAML-id-provider, moet u een geldige X509 certificaat met de persoonlijke sleutel. U upload het certificaat met de persoonlijke sleutel (.pfx-bestand) naar het sleutelarchief van de Azure AD B2C-beleid. Azure AD B2C handtekening digitale de SAML-aanmelden-aanvraag met behulp van het certificaat dat u opgeeft. 

Het certificaat wordt gebruikt in de volgende manieren:

- Azure AD B2C genereert en ondertekent een SAML-aanvraag met behulp van de Azure AD B2C persoonlijke sleutel van het certificaat. De SAML-aanvraag is verzonden naar de id-provider, die de aanvraag met behulp van Azure AD B2C openbare sleutel van het certificaat worden gevalideerd. Het openbare certificaat van Azure AD B2C is toegankelijk via de metagegevens van het technische profiel. U kunt ook handmatig het cer-bestand uploaden naar uw SAML-identiteitsprovider.
- De id-provider zich de gegevens naar Azure AD B2C verzonden met behulp van de persoonlijke sleutel van de id-provider van het certificaat. Azure AD B2C valideert de gegevens via het openbare certificaat van de id-provider. Elke id-provider heeft verschillende stappen voor de installatie, kijkt u naar de documentatie van uw id-provider voor instructies over om dit te doen. In Azure AD B2C moet uw beleid voor toegang tot de openbare sleutel van het certificaat met behulp van de metagegevens van de id-provider.

Een zelfondertekend certificaat is geschikt voor de meeste scenario's. Voor productieomgevingen, het is raadzaam te gebruiken een X509 certificaat dat is uitgegeven door een certificeringsinstantie. Ook, zoals verderop in dit document wordt beschreven, voor een niet-productieomgeving kunt u uitschakelen voor de SAML-handtekeningcertificaten aan beide zijden.

Het volgende diagram toont de uitwisseling van metagegevens en het certificaat:

![metagegevens en certificaat exchange](media/saml-technical-profile/technical-profile-idp-saml-metadata.png)


## <a name="digital-encryption"></a>Digitale versleuteling

Voor het versleutelen van het SAML-verklaring antwoord, gebruikt de id-provider een openbare sleutel van een versleutelingscertificaat altijd in het technische profiel van een Azure AD B2C. Wanneer Azure AD B2C nodig heeft om de gegevens te ontsleutelen, gebruikt het persoonlijke gedeelte van het versleutelingscertificaat.

Voor het versleutelen van de SAML-antwoord-bevestiging:

1. Upload een geldig X509 certificaat met de persoonlijke sleutel (.pfx-bestand) aan het sleutelarchief van de Azure AD B2C-beleid.
2. Voeg een **CryptographicKey** element met een id van `SamlAssertionDecryption` aan het technische profiel **CryptographicKeys** verzameling. Stel de **StorageReferenceId** op de naam van de beleidssleutel die u in stap 1 hebt gemaakt.
3. De metagegevens van het technische profiel instellen **WantsEncryptedAssertions** naar `true`.
4. Werk de id-provider met de metagegevens van de nieuwe Azure AD B2C technisch profiel. U ziet de **KeyDescriptor** met de **gebruiken** eigenschap ingesteld op `encryption` met de openbare sleutel van uw certificaat.

Het volgende voorbeeld ziet u het gedeelte Azure AD B2C technisch profiel versleuteling van de metagegevens:

```XML
<KeyDescriptor use="encryption">
  <KeyInfo xmlns="https://www.w3.org/2000/09/xmldsig#">
    <X509Data>
      <X509Certificate>valid certificate</X509Certificate>
    </X509Data>
  </KeyInfo>
</KeyDescriptor>
```

## <a name="identity-provider-initiated-flow"></a>Identity provider gestart stroom

In een eenmalige aanmelding-sessie (ongevraagde aanvraag) gestart door de id-provider is een ongevraagde SAML-antwoord verzonden naar de serviceprovider in dit geval het technische profiel van een Azure AD B2C. In deze stroom de gebruiker gaat niet via de web-App eerst, maar wordt omgeleid naar de id-provider. Wanneer de aanvraag wordt verzonden, wordt een verificatiepagina aan de gebruiker opgegeven door de id-provider. De gebruiker heeft de aanmelding is voltooid en vervolgens de aanvraag wordt omgeleid naar Azure AD B2C met een SAML-antwoord de asserties bevat. Azure AD B2C leest de asserties en problemen met een nieuwe SAML-token en vervolgens wordt de gebruiker omgeleid naar de relying party-toepassing. De omleidingen moeten worden uitgevoerd door de **AssertionConsumerService** van element **locatie** eigenschap.


![SAML-IDP gestart](media/saml-technical-profile/technical-profile-idp-saml-idp-initiated.png) 

Houd rekening met de vereisten voor het volgende bij het maken van een id-provider stroom gestart:

- De eerste indelingsstap moet dat één claims exchange die naar een technische SAML-profiel verwijst.
- Het technische profiel moet beschikken over een item met de naam metagegevens **IdpInitiatedProfileEnabled** ingesteld op `true`.
- Het beleid voor relying party moet een SAML relying party.
- Het beleid voor relying party moet beschikken over een item met de naam metagegevens **IdpInitiatedProfileEnabled** ingesteld op `true`.
- Het ongevraagde antwoord moet worden verzonden naar de `/your-tenant/your-policy/samlp/sso/assertionconsumer` eindpunt. Een relaystatus opgenomen in het antwoord wordt doorgestuurd naar de relying party. Vervang de volgende waarden: **uw tenant** met de naam van uw tenant. **uw beleid** met de naam van uw relying party-beleid.
    
## <a name="protocol"></a>Protocol

De **naam** kenmerk van het Protocol-element moet worden ingesteld op `SAML2`. 

## <a name="output-claims"></a>Gebruikersclaims
 
De **OutputClaims** element bevat een lijst met claims die zijn geretourneerd door de SAML-identiteitsprovider onder de `AttributeStatement` sectie. Mogelijk moet u de naam van de claim die is gedefinieerd in uw beleid aan de naam die is gedefinieerd in de id-provider toewijzen. U kunt ook bevatten claims die niet zijn geretourneerd door de id-provider, zolang u de `DefaultValue` kenmerk.
 
Lezen van het SAML-verklaring **NamedId** in **onderwerp** als een genormaliseerde claim, stelt u de claim **PartnerClaimType** naar `assertionSubjectName`. Zorg ervoor dat de **NameId** is de eerste waarde in de verklaring XML. Wanneer u meer dan één assertion definieert, kiest de Azure AD B2C de onderwerpwaarde van de laatste verklaring.
 
De **OutputClaimsTransformations** element kan bevatten een verzameling van **OutputClaimsTransformation** elementen die worden gebruikt voor het wijzigen van de uitvoerclaims of nieuwe labels te genereren.
 
Het volgende voorbeeld ziet u de claims die wordt geretourneerd door de Facebook-id-provider:

- De **socialIdpUserId** claim is toegewezen aan de **assertionSubjectName** claim.
- De **first_name** claim is toegewezen aan de **givenName** claim.
- De **achternaam** claim is toegewezen aan de **achternaam** claim.
- De **displayName** claim zonder naam toewijzingen.
- De **e** claim zonder naam toewijzingen.
 
Het technische profiel retourneert ook claims die niet zijn geretourneerd door de id-provider: 
 
- De **identityProvider** claim met de naam van de id-provider.
- De **authenticationSource** claim met een standaardwaarde van **socialIdpAuthentication**.
 
```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="assertionSubjectName" />
  <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="first_name" />
  <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="last_name" />
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
  <OutputClaim ClaimTypeReferenceId="email"  />
  <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="contoso.com" />
  <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
</OutputClaims>
```

## <a name="metadata"></a>Metagegevens

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| PartnerEntity | Ja | URL van de metagegevens van de SAML-identiteitsprovider. Kopieer de metagegevens van de id-provider en toe te voegen in het CDATA-element `<![CDATA[Your IDP metadata]]>` |
| WantsSignedRequests | Nee | Geeft aan of het technische profiel vereist dat alle van de uitgaande verificatieaanvragen moeten worden ondertekend. Mogelijke waarden: `true` of `false`. De standaardwaarde is `true`. Wanneer de waarde is ingesteld op `true`, wordt de **SamlMessageSigning** cryptografische sleutel moet worden opgegeven en alle uitgaande verificatieaanvragen zijn ondertekend. Als de waarde is ingesteld op `false`, wordt de **SigAlg** en **handtekening** parameters (querytekenreeks of parameter plaatsen) worden weggelaten uit de aanvraag. Deze metagegevens bepaalt ook de metagegevens van de **AuthnRequestsSigned** kenmerk, die wordt uitgevoerd in de metagegevens van de Azure AD B2C technisch profiel dat wordt gedeeld met de id-provider. Azure AD B2C niet ondertekenen van de aanvraag als **WantsSignedRequests** in het technische profiel metagegevens is ingesteld op `false` en de metagegevens van de id-provider **WantAuthnRequestsSigned** is ingesteld op `false` of niet opgegeven. |
| XmlSignatureAlgorithm | Nee | De methode die Azure AD B2C gebruikt voor het ondertekenen van de SAML-aanvraag. Deze metagegevens bepaalt de waarde van de **SigAlg** parameter (querytekenreeks of parameter plaatsen) in de SAML-aanvraag. Mogelijke waarden: `Sha256`, `Sha384`, `Sha512`, of `Sha1`. Zorg ervoor dat u de handtekeningalgoritme configureren aan beide zijden met dezelfde waarde. Gebruik alleen de algoritme die ondersteuning biedt voor uw certificaat. | 
| WantsSignedAssertions | Nee | Geeft aan of het technische profiel is vereist dat alle inkomende bevestigingen zijn ondertekend. Mogelijke waarden: `true` of `false`. De standaardwaarde is `true`. Als de waarde is ingesteld op `true`, alle asserties sectie `saml:Assertion` verzonden door de identiteit van de Azure AD B2C-provider moet worden ondertekend. Als de waarde is ingesteld op `false`, de id-provider mag niet de asserties ondertekenen, maar zelfs als dit het geval, Azure AD B2C de handtekening niet gevalideerd. Deze metagegevens bepaalt ook de markering voor metagegevens **WantsAssertionsSigned**, die wordt uitgevoerd in de metagegevens van de Azure AD B2C technisch profiel dat wordt gedeeld met de id-provider. Als u de validatie van asserties uitschakelt, ook kunt u de validatie van de handtekening antwoord uitschakelen (Zie voor meer informatie, **ResponsesSigned**). |
| ResponsesSigned | Nee | Mogelijke waarden: `true` of `false`. De standaardwaarde is `true`. Als de waarde is ingesteld op `false`, de id-provider mag niet het SAML-antwoord ondertekenen, maar zelfs als dit het geval, Azure AD B2C de handtekening niet gevalideerd. Als de waarde is ingesteld op `true`, het SAML-antwoord dat is verzonden door de id-provider naar Azure AD B2C is ondertekend en moet worden gevalideerd. Als u de validatie van SAML-antwoord uitschakelt, ook kunt u de validatie van de handtekening assertion uitschakelen (Zie voor meer informatie, **WantsSignedAssertions**). |
| WantsEncryptedAssertions | Nee | Geeft aan of het technische profiel is vereist dat alle inkomende bevestigingen moeten worden versleuteld. Mogelijke waarden: `true` of `false`. De standaardwaarde is `false`. Als de waarde is ingesteld op `true`, asserties verzonden door de id-provider naar Azure AD B2C moeten worden ondertekend en de **SamlAssertionDecryption** cryptografische sleutel moet worden opgegeven. Als de waarde is ingesteld op `true`, de metagegevens van de Azure AD B2C technisch profiel bevat de **versleuteling** sectie. De id-provider leest de metagegevens en versleutelt de SAML-verklaring antwoord met de openbare sleutel die is opgegeven in de metagegevens van het technische profiel van de Azure AD B2C. Als u de asserties-versleuteling inschakelt, moet u ook mogelijk om uit te schakelen de handtekeningvalidatie van het antwoord (Zie voor meer informatie, **ResponsesSigned**). | 
| IdpInitiatedProfileEnabled | Nee | Geeft aan of het sessieprofiel van een voor eenmalige aanmelding die is gestart door een SAML-profiel-id-provider is ingeschakeld. Mogelijke waarden: `true` of `false`. De standaardwaarde is `false`. In de stroom gestart door de id-provider, de gebruiker extern is geverifieerd en een ongevraagde reactie wordt verzonden naar Azure AD B2C, die vervolgens het token verbruikt, indelingsstappen wordt uitgevoerd en vervolgens een antwoord naar de relying party-toepassing stuurt. |
| NameIdPolicyFormat | Nee | Hiermee geeft u beperkingen op de naam-id moet worden gebruikt voor het aangevraagde onderwerp. Als u dit weglaat, kan elk type ondersteund door de id-provider voor het aangevraagde onderwerp-id kan worden gebruikt. Bijvoorbeeld `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`. **NameIdPolicyFormat** kan worden gebruikt met **NameIdPolicyAllowCreate**. Bekijk uw id-provider-documentatie voor informatie over welke naam ID beleid worden ondersteund. |
| NameIdPolicyAllowCreate | Nee | Bij het gebruik van **NameIdPolicyFormat**, u kunt ook opgeven de `AllowCreate` eigenschap van **NameIDPolicy**. De waarde van deze metagegevens is `true` of `false` om aan te geven of de id-provider is toegestaan tot een nieuw account maken tijdens de stroom aanmelden. Bekijk documentatie van uw id-provider voor instructies over om dit te doen. |
| AuthenticationRequestExtensions | Nee | Optionele protocol bericht extensie-elementen die worden overeengekomen tussen Azure AD BC en de id-provider. De extensie wordt weergegeven in de XML-indeling. Toevoegen van de XML-gegevens in het element CDATA `<![CDATA[Your IDP metadata]]>`. Raadpleeg de documentatie van uw id-provider om te zien of het element extensions wordt ondersteund. |
| IncludeAuthnContextClassReferences | Nee | Hiermee geeft u een of meer URI verwijzingen authentication context klassen te identificeren. Bijvoorbeeld, als u wilt toestaan dat een gebruiker zich aanmelden met alleen gebruikersnaam en wachtwoord, de waarde ingesteld op `urn:oasis:names:tc:SAML:2.0:ac:classes:Password`. Als u wilt dat aanmelding via gebruikersnaam en wachtwoord via een beveiligde sessie (SSL/TLS), geef `PasswordProtectedTransport`. Documentatie van uw id-provider voor instructies bekijken over de **AuthnContextClassRef** URI's die worden ondersteund. |
| IncludeKeyInfo | Nee | Geeft aan of de SAML-aanvraag voor verificatie de openbare sleutel van het certificaat bevat als de binding is ingesteld op `HTTP-POST`. Mogelijke waarden: `true` of `false`. |

## <a name="cryptographic-keys"></a>Cryptografische sleutels

De **CryptographicKeys** element bevat de volgende kenmerken:

| Kenmerk |Vereist | Description |
| --------- | ----------- | ----------- |
| SamlMessageSigning |Ja | De X509 certificaat (RSA-sleutelpaar) moet worden gebruikt voor het ondertekenen van SAML-berichten. Azure AD B2C gebruikt deze sleutel om te ondertekenen van de aanvragen en verzend dit naar de id-provider. |
| SamlAssertionDecryption |Ja | De X509 certificaat (RSA-sleutelpaar) moet worden gebruikt voor het ontsleutelen van SAML-berichten. Dit certificaat moet worden opgegeven door de id-provider. Azure AD B2C gebruikt dit certificaat om de gegevens die is verzonden door de id-provider te ontsleutelen. |
| MetadataSigning |Nee | De X509 certificaat (RSA-sleutelpaar) moet worden gebruikt om te ondertekenen van SAML-metagegevens. Azure AD B2C gebruikt deze sleutel voor het ondertekenen van de metagegevens.  |

## <a name="examples"></a>Voorbeelden

- [AD FS toevoegen als een SAML-id-provider met behulp van aangepaste beleidsregels](active-directory-b2c-custom-setup-adfs2016-idp.md)
- [Meld u aan met behulp van Salesforce-accounts via SAML](active-directory-b2c-setup-sf-app-custom.md)













