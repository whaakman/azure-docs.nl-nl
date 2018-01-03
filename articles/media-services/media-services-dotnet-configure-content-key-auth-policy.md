---
title: Een autorisatiebeleid voor inhoudssleutels configureren met behulp van de Media Services .NET SDK | Microsoft Docs
description: Informatie over het configureren van een verificatiebeleid voor een sleutel die inhoud met behulp van de Media Services .NET SDK.
services: media-services
documentationcenter: 
author: mingfeiy
manager: cfowler
editor: 
ms.assetid: 1a0aedda-5b87-4436-8193-09fc2f14310c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako;mingfeiy
ms.openlocfilehash: 39782bd687c9c1b50699c05e61e57d9c767a8d32
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="dynamic-encryption-configure-a-content-key-authorization-policy"></a>Dynamische versleuteling: een autorisatiebeleid voor inhoudssleutels configureren
[!INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

## <a name="overview"></a>Overzicht
 U kunt Azure Media Services MPEG-DASH-, Smooth Streaming- en HTTP Live Streaming (HLS)-streams beveiligd met de Advanced Encryption Standard (AES) met behulp van 128-bits versleutelingssleutels leveren of [PlayReady DRM (DRM) ](https://www.microsoft.com/playready/overview/). Met Media Services kunt u ook versleuteld met Widevine DRM DASH-streams leveren. Zowel PlayReady als Widevine zijn versleuteld volgens de specificatie met common encryption (ISO/IEC 23001-7 CENC).

Media Services biedt ook een service voor het leveren van sleutel/licentie van waaruit clients AES-sleutels of PlayReady/Widevine-licenties te spelen de versleutelde inhoud kunnen ophalen.

Als u Media Services voor het versleutelen van een asset wilt, moet u een versleutelingssleutel (CommonEncryption of EnvelopeEncryption) koppelen aan de asset. Zie voor meer informatie [ContentKeys maken met .NET](media-services-dotnet-create-contentkey.md). U moet ook autorisatiebeleid voor de sleutel (zoals beschreven in dit artikel) configureren.

Wanneer een stream is aangevraagd door een speler, Media Services maakt gebruik van de opgegeven sleutel voor het versleutelen van uw inhoud dynamisch met behulp van versleuteling AES of DRM. Voor het ontsleutelen van de stroom, vraagt Windows media player de sleutel van de service sleutellevering. De service beoordeelt om te bepalen of de gebruiker is gemachtigd om op te halen van de sleutel, de autorisatie-beleidsregels die u hebt opgegeven voor de sleutel.

Media Services ondersteunt meerdere manieren om gebruikers te verifiëren die sleutels aanvragen. Het autorisatiebeleid voor inhoudssleutels kan een of meer autorisatiebeperkingen hebben. De opties zijn openen of token-beperking. Het beleid token beperkte vergezeld van een token dat is uitgegeven door een beveiligingstokenservice (STS). Media Services ondersteunt tokens in het eenvoudige web token ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2))-indeling en JSON Web Token ([JWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3)) indeling.

Media Services biedt niet STS. U kunt een aangepaste STS maken of gebruiken van Azure Access Control Service voor het probleem van tokens. De STS moeten worden geconfigureerd voor het maken van een token dat is ondertekend met de opgegeven sleutel- en claims die u hebt opgegeven in de configuratie van de tokenbeperking (zoals beschreven in dit artikel). Als het token geldig is en de claims in het token overeenkomen met die zijn geconfigureerd voor de inhoudssleutel, retourneert de sleutellevering van Media Services de versleutelingssleutel voor de client.

Raadpleeg voor meer informatie de volgende artikelen:

- [JWT-token verificatie](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)
- [Azure Media Services OWIN MVC-gebaseerde app integreren met Azure Active Directory en sleutel inhouddistributie op basis van claims JWT beperken](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/)

### <a name="some-considerations-apply"></a>Enkele overwegingen zijn van toepassing
* Wanneer uw Media Services-account is gemaakt, wordt een standaard streaming-eindpunt toegevoegd aan uw account in de status 'Gestopt'. Om te beginnen uw inhoud te streamen en te profiteren van dynamische pakketten en dynamische versleuteling, moet uw streaming-eindpunt in de status 'Actief'. 
* Uw asset moet een set adaptive bitrate MP4s of adaptive bitrate Smooth Streaming-bestanden bevatten. Zie voor meer informatie [een asset coderen](media-services-encode-asset.md).
* Upload en uw assets coderen met de optie AssetCreationOptions.StorageEncrypted.
* Als u meerdere inhoudssleutels die dezelfde beleidsconfiguratie nodig hebt wilt, raden wij aan dat u een enkel verificatiebeleid maken en deze opnieuw te met meerdere inhoudssleutels gebruiken.
* De service sleutellevering caches ContentKeyAuthorizationPolicy en de verwante objecten (beleidsopties en beperkingen) gedurende 15 minuten. U kunt ContentKeyAuthorizationPolicy maken en aangeven dat u wilt gebruiken een token beperking, testen en het beleid vervolgens bijwerken naar de beperking die wordt geopend. Dit proces duurt ongeveer 15 minuten voordat de beleid-switches op de geopende versie van het beleid.
* Als u toevoegt of leveringsbeleid voor uw assets bijwerkt, moet u een bestaande locator verwijderen en een nieuwe locator maken.
* U kunt progressief downloaden op dit moment niet coderen.
* Een Media Services streaming-eindpunt stelt u de waarde van de header CORS 'Access Control-toestaan-oorsprong' voorbereidende reactie als het jokerteken '\*'. Deze waarde werkt goed samen met de meeste spelers, waaronder Azure Media Player, Roku en JWPlayer en anderen. Evenwel sommige spelers die gebruikmaken van dashjs werken niet omdat de referenties modus ingesteld op "bevatten" XMLHttpRequest in hun dashjs het jokerteken mag geen '\*' als de waarde van 'Access Control-toestaan-oorsprong'. Als een oplossing voor deze beperking in dashjs als host van de client uit één domein: kunt Media Services opgeven dat domein in de voorbereidende response-header. Open een ondersteuningsticket via de Azure portal voor assistentie.

## <a name="aes-128-dynamic-encryption"></a>Dynamische AES-128-versleuteling
### <a name="open-restriction"></a>Open beperking
Open beperking betekent dat het systeem biedt de sleutel voor iedereen die een sleutel aanvraag indient. Deze beperking kan handig zijn voor testdoeleinden.

Het volgende voorbeeld maakt u een open autorisatiebeleid en voegt het toe aan de inhoudssleutel:

    static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
    {
        // Create ContentKeyAuthorizationPolicy with Open restrictions
        // and create authorization policy
        IContentKeyAuthorizationPolicy policy = _context.
        ContentKeyAuthorizationPolicies.
        CreateAsync("Open Authorization Policy").Result;
        
        List<ContentKeyAuthorizationPolicyRestriction> restrictions =
            new List<ContentKeyAuthorizationPolicyRestriction>();

        ContentKeyAuthorizationPolicyRestriction restriction =
            new ContentKeyAuthorizationPolicyRestriction
            {
                Name = "HLS Open Authorization Policy",
                KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                Requirements = null // no requirements needed for HLS
            };

        restrictions.Add(restriction);

        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
            "policy", 
            ContentKeyDeliveryType.BaselineHttp, 
            restrictions, 
            "");

        policy.Options.Add(policyOption);

        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
    }


### <a name="token-restriction"></a>Tokenbeperking
Deze sectie wordt beschreven hoe een autorisatiebeleid voor inhoudssleutels maken en deze koppelen aan de inhoudssleutel. Het verificatiebeleid wordt beschreven welke autorisatievereisten moeten worden voldaan om te bepalen of de gebruiker is gemachtigd voor het ontvangen van de sleutel. Bijvoorbeeld, bevat de lijst met sleutels verificatie van de sleutel die het token is ondertekend met?

Voor het configureren van de tokenbeperking-optie, moet u een XML-tekenreeks gebruiken om te beschrijven van autorisatievereisten van het token. De configuratie-XML van de tokenbeperking moet voldoen aan de volgende XML-schema:

#### <a name="token-restriction-schema"></a>Tokenbeperking schema
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" xmlns:xs="http://www.w3.org/2001/XMLSchema">
      <xs:complexType name="TokenClaim">
        <xs:sequence>
          <xs:element name="ClaimType" nillable="true" type="xs:string" />
          <xs:element minOccurs="0" name="ClaimValue" nillable="true" type="xs:string" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenClaim" nillable="true" type="tns:TokenClaim" />
      <xs:complexType name="TokenRestrictionTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AlternateVerificationKeys" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
          <xs:element name="Audience" nillable="true" type="xs:anyURI" />
          <xs:element name="Issuer" nillable="true" type="xs:anyURI" />
          <xs:element name="PrimaryVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
          <xs:element minOccurs="0" name="RequiredClaims" nillable="true" type="tns:ArrayOfTokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenRestrictionTemplate" nillable="true" type="tns:TokenRestrictionTemplate" />
      <xs:complexType name="ArrayOfTokenVerificationKey">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenVerificationKey" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
      <xs:complexType name="TokenVerificationKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
      <xs:complexType name="ArrayOfTokenClaim">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenClaim" nillable="true" type="tns:TokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenClaim" nillable="true" type="tns:ArrayOfTokenClaim" />
      <xs:complexType name="SymmetricVerificationKey">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:TokenVerificationKey">
            <xs:sequence>
              <xs:element name="KeyValue" nillable="true" type="xs:base64Binary" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="SymmetricVerificationKey" nillable="true" type="tns:SymmetricVerificationKey" />
    </xs:schema>

Wanneer u het beleid token beperkt configureert, moet u de verificatie van de primaire sleutel, uitgever en doelgroep parameters opgeven. De verificatie van de primaire sleutel bevat de sleutel die het token is ondertekend met. De certificaatverlener is de STS die de token uitgeeft. De doelgroep (ook wel bereik genoemd) beschrijft de intentie van het token of de resource het token gemachtigd voor toegang tot. De Media Services-service sleutellevering valideert dat deze waarden in het token overeenkomen met de waarden in de sjabloon.

Wanneer u de Media Services SDK voor .NET gebruikt, kunt u de klasse TokenRestrictionTemplate kunt gebruiken om de beperking-token te genereren.
Het volgende voorbeeld wordt een verificatiebeleid met een beperking van de token. In dit voorbeeld moet de client een token met een handtekeningsleutel (VerificationKey), de uitgever van een token en vereiste claims aanwezig.

    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();

        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;

        List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                new List<ContentKeyAuthorizationPolicyRestriction>();

        ContentKeyAuthorizationPolicyRestriction restriction =
                new ContentKeyAuthorizationPolicyRestriction
                {
                    Name = "Token Authorization Policy",
                    KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                    Requirements = tokenTemplateString
                };

        restrictions.Add(restriction);

        //You could have multiple options 
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
                "Token option for HLS",
                ContentKeyDeliveryType.BaselineHttp,
                restrictions,
                null  // no key delivery data is needed for HLS
                );

        policy.Options.Add(policyOption);

        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKey.AuthorizationPolicyId = policy.Id;
        IContentKey updatedKey = contentKey.UpdateAsync().Result;
        Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);

        return tokenTemplateString;
    }

    static private string GenerateTokenRequirements()
    {
        TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);

        template.PrimaryVerificationKey = new SymmetricVerificationKey();
        template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();

        template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);

        return TokenRestrictionTemplateSerializer.Serialize(template);
    }

#### <a name="test-token"></a>Test-token
Als u een test-token op basis van de tokenbeperking die werd gebruikt voor het sleutelautorisatiebeleid, het volgende doen:

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate =
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on the the data in the given TokenRestrictionTemplate.
    // Note, you need to pass the key id Guid because we specified 
    // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
    Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);

    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
    Console.WriteLine();


## <a name="playready-dynamic-encryption"></a>Dynamische PlayReady-versleuteling
U kunt Media Services gebruiken voor het configureren van de rechten en beperkingen die u wilt dat de runtime PlayReady DRM af te dwingen wanneer een gebruiker probeert om af te spelen beveiligde inhoud. 

Wanneer u uw inhoud met PlayReady beveiligt, een van de dingen die u wilt opgeven in het autorisatiebeleid is een XML-tekenreeks die definieert de [PlayReady-licentiesjabloon](media-services-playready-license-template-overview.md). In Media Services SDK voor .NET helpen de klassen PlayReadyLicenseResponseTemplate en PlayReadyLicenseTemplate u bij het definiëren van de sjabloon PlayReady-licenties.

Zie voor informatie over het versleutelen van uw inhoud met PlayReady en Widevine, [gebruik PlayReady en/of Widevine dynamic common encryption](media-services-protect-with-playready-widevine.md).

### <a name="open-restriction"></a>Open beperking
Open beperking betekent dat het systeem biedt de sleutel voor iedereen die een sleutel aanvraag indient. Deze beperking kan handig zijn voor testdoeleinden.

Het volgende voorbeeld maakt u een open autorisatiebeleid en voegt het toe aan de inhoudssleutel:

    static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
    {

        // Create ContentKeyAuthorizationPolicy with Open restrictions 
        // and create authorization policy          

        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Open", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.Open, 
                Requirements = null
            }
        };

        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);

        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;


        contentKeyAuthorizationPolicy.Options.Add(policyOption);

        // Associate the content key authorization policy with the content key.
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;
    }

### <a name="token-restriction"></a>Tokenbeperking
Voor het configureren van de tokenbeperking-optie, moet u een XML-tekenreeks gebruiken om te beschrijven van autorisatievereisten van het token. De configuratie van de tokenbeperking XML aan het XML-schema wordt weergegeven voldoen moet in de '[Token beperking schema](#token-restriction-schema)' sectie.

    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();

        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;

        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Token Authorization Policy", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                Requirements = tokenTemplateString, 
            }
        };

        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);

        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;

        policy.Options.Add(policyOption);

        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKeyAuthorizationPolicy.Options.Add(policyOption);

        // Associate the content key authorization policy with the content key
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;

        return tokenTemplateString;
    }

    static private string GenerateTokenRequirements()
    {

        TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);

        template.PrimaryVerificationKey = new SymmetricVerificationKey();
        template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();


        template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);

        return TokenRestrictionTemplateSerializer.Serialize(template);
    } 

    static private string ConfigurePlayReadyLicenseTemplate()
    {
        // The following code configures PlayReady License Template using .NET classes
        // and returns the XML string.

        //The PlayReadyLicenseResponseTemplate class represents the template for the response sent back to the end user. 
        //It contains a field for a custom data string between the license server and the application 
        //(may be useful for custom app logic) as well as a list of one or more license templates.
        PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();

        // The PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
        // to be returned to the end users. 
        //It contains the data on the content key in the license and any rights or restrictions to be 
        //enforced by the PlayReady DRM runtime when using the content key.
        PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
        //Configure whether the license is persistent (saved in persistent storage on the client) 
        //or non-persistent (only held in memory while the player is using the license).  
        licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;

        // AllowTestDevices controls whether test devices can use the license or not.  
        // If true, the MinimumSecurityLevel property of the license
        // is set to 150.  If false (the default), the MinimumSecurityLevel property of the license is set to 2000.
        licenseTemplate.AllowTestDevices = true;


        // You can also configure the Play Right in the PlayReady license by using the PlayReadyPlayRight class. 
        // It grants the user the ability to play back the content subject to the zero or more restrictions 
        // configured in the license and on the PlayRight itself (for playback specific policy). 
        // Much of the policy on the PlayRight has to do with output restrictions 
        // which control the types of outputs that the content can be played over and 
        // any restrictions that must be put in place when using a given output.
        // For example, if the DigitalVideoOnlyContentRestriction is enabled, 
        //then the DRM runtime will only allow the video to be displayed over digital outputs 
        //(analog video outputs won’t be allowed to pass the content).

        //IMPORTANT: These types of restrictions can be very powerful but can also affect the consumer experience. 
        // If the output protections are configured too restrictive, 
        // the content might be unplayable on some clients. For more information, see the PlayReady Compliance Rules document.

        // For example:
        //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);

        responseTemplate.LicenseTemplates.Add(licenseTemplate);

        return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
    }


Als u een test-token op basis van de tokenbeperking die werd gebruikt voor het sleutelautorisatiebeleid, raadpleegt u de '[Test-token](#test-token)' sectie. 

## <a id="types"></a>Typen die worden gebruikt bij het definiëren van ContentKeyAuthorizationPolicy
### <a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType
    public enum ContentKeyRestrictionType
    {
        Open = 0,
        TokenRestricted = 1,
        IPRestricted = 2,
    }

### <a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType
    public enum ContentKeyDeliveryType
    {
      None = 0,
      PlayReadyLicense = 1,
      BaselineHttp = 2,
      Widevine = 3
    }

### <a id="TokenType"></a>TokenType
    public enum TokenType
    {
        Undefined = 0,
        SWT = 1,
        JWT = 2,
    }

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Volgende stappen
Nu u autorisatiebeleid voor de inhoudssleutel hebben geconfigureerd, gaat u naar [een leveringsbeleid voor Assets configureren](media-services-dotnet-configure-asset-delivery-policy.md).

