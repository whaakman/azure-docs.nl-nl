---
title: 'Azure AD Connect: Gebruik een SAML-identiteitsprovider 2.0 voor eenmalige aanmelding in | Microsoft Docs'
description: In dit onderwerp wordt beschreven hoe een compatibele Idp SAML 2.0 voor eenmalige aanmelding op.
services: active-directory
author: billmath
manager: mtillman
ms.custom: it-pro
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 46c65e0efdc91b70c5d0d2afdf83d7205efc8057
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
#  <a name="use-a-saml-20-identity-provider-idp-for-single-sign-on"></a>Gebruik van SAML 2.0-identiteitsprovider (IdP) voor eenmalige aanmelding in

Dit onderwerp bevat informatie over het gebruik van een SAML 2.0 compatibel SP Lite-profiel op basis van id-Provider als de voorkeur Security Token Service (STS) / id-provider. Dit is handig wanneer u hebt al een map van de gebruiker en het wachtwoord voor het opslaan van lokale die kunnen worden geopend met behulp van SAML 2.0. Deze bestaande map van de gebruiker kan worden gebruikt voor aanmelding bij Office 365 en andere Azure AD-beveiligde bronnen. Het SAML 2.0 SP-Lite-profiel is gebaseerd op de gebruikte Security Assertion Markup Language (SAML) federatieve identiteiten standaard een aanmelding en kenmerk exchange kader te bieden.

>[!NOTE]
>Zie voor een lijst van 3e partij Idps die zijn getest voor gebruik met Azure AD de [Azure AD-federatiecompatibiliteitslijst](active-directory-aadconnect-federation-compatibility.md)

Microsoft ondersteunt deze ervaring aanmelding als de integratie van een Microsoft-cloudservice, zoals Office 365, met uw correct geconfigureerde SAML 2.0 profiel op basis van IdP. SAML 2.0 identiteitsproviders zijn producten van derden en daarom Microsoft biedt geen ondersteuning voor de implementatie, configuratie, met betrekking tot deze aanbevolen procedures voor het oplossen van problemen. Eenmaal correct is geconfigureerd, de integratie met het SAML 2.0 id-provider voor de juiste configuratie kan worden getest met behulp van het hulpprogramma Microsoft connectiviteit Analyzer die wordt hieronder in detail beschreven. Vraag de organisatie die deze heeft geleverd voor meer informatie over uw SAML 2.0 SP-Lite profiel op basis van id-provider.

>[!IMPORTANT]
>Alleen een beperkte set clients zijn beschikbaar in dit scenario aanmelding met identiteitsproviders SAML 2.0, wordt dit omvat:

>- Web-gebaseerde clients zoals Outlook Web Access en SharePoint Online
- E-rich clients die gebruikmaken van basisverificatie en een ondersteunde Exchange toegangsmethode zoals IMAP, pop-server, ActiveSync, MAPI, enz. (het verbeterde clientprotocol-eindpunt is vereist om te worden geïmplementeerd), met inbegrip van:
    - Microsoft Outlook 2010/Outlook 2013/Outlook 2016, Apple iPhone (verschillende iOS-versies)
    - Verschillende Google Android-apparaten
    - Windows Phone 7, Windows Phone 7,8 en Windows Phone 8.0
    - E-mailclient van Windows 8 en Windows 8.1-e-mailclient
    - E-mailclient van Windows 10

Alle andere clients zijn niet beschikbaar in dit scenario aanmelding met de id-Provider van SAML 2.0. De bureaubladclient Lync 2010 is bijvoorbeeld niet aanmelden bij de service met uw SAML 2.0-identiteitsprovider geconfigureerd voor eenmalige aanmelding.

## <a name="azure-ad-saml-20-protocol-requirements"></a>Vereisten voor Azure AD-SAML 2.0-protocol
Dit onderwerp bevat gedetailleerde vereisten op het protocol en de berichtindeling dat uw SAML 2.0-id-provider implementeren moet om te federeren met Azure AD inschakelen aanmelding bij een of meer Microsoft-cloudservices (zoals Office 365). De SAML 2.0 relying party (SP-STS) voor een Microsoft-cloudservice in dit scenario gebruikt is Azure AD.

Het is raadzaam dat u zorgen dat uw SAML 2.0-identiteitsprovider Uitvoerberichten worden als vergelijkbaar met de opgegeven voorbeeld traceringen mogelijk. Gebruik tevens specifieke kenmerkwaarden van de opgegeven Azure AD-metagegevens waar mogelijk. Wanneer u tevreden met uw Uitvoerberichten bent, kunt u testen met de Microsoft connectiviteit Analyzer zoals hieronder wordt beschreven.

De metagegevens van de Azure AD kan worden gedownload vanaf deze URL: [https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml](http://https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml).
Voor klanten in China via het China-specifieke exemplaar van Office 365, het volgende federation-eindpunt moet worden gebruikt: [https://nexus.partner.microsoftonline-p.cn/federationmetadata/saml20/federationmetadata.xml](https://nexus.partner.microsoftonline-p.cn/federationmetadata/saml20/federationmetadata.xml).

## <a name="saml-protocol-requirements"></a>Vereisten voor SAML-protocol
Deze sectie details hoe in de aanvraag en -antwoord berichtenparen zijn geplaatst om te helpen u om uw berichten correct.

Azure AD kan worden geconfigureerd om te werken met de id-providers die gebruikmaken van het SAML 2.0 SP Lite-profiel met een aantal specifieke vereisten zoals hieronder weergegeven. Met behulp van de steekproef SAML-aanvraag en antwoord berichten samen met automatische en handmatige testen, kunt u samenwerken om interoperabiliteit met Azure AD.

## <a name="signature-block-requirements"></a>Vereisten voor het blokkeren van handtekeningen
Knooppunt voor de handtekening bevat binnen het SAML-antwoordbericht informatie over de digitale handtekening voor het bericht zelf. Het handtekeningblok gelden de volgende vereisten:

1. De verklaring knooppunt zelf moet zijn ondertekend
2.  De RSA-sha1-algoritme moet worden gebruikt als de DigestMethod. Andere algoritmen digitale handtekening worden niet geaccepteerd.
   `<ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>`
3.  Het XML-document kan zich ook aanmelden. 
4.  De transformatie-algoritme moet overeenkomen met de waarden in het volgende voorbeeld:`<ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
       <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>`
9.  Het algoritme SignatureMethod moet overeenkomen met het volgende voorbeeld:`<ds:SignatureMethod Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>`

## <a name="supported-bindings"></a>Ondersteunde bindingen
Bindingen zijn het transport communicatieparameters die vereist zijn gerelateerd. De volgende vereisten gelden voor de bindingen

1. HTTPS is het vereist transport.
2.  Azure AD is HTTP POST-token te verzenden tijdens de aanmelding vereist
3.  Azure AD gebruikt HTTP POST voor de verificatieaanvraag naar de id-provider en -OMLEIDING voor het bericht afmelden om de id-provider.

## <a name="required-attributes"></a>Vereiste kenmerken
Deze tabel bevat de vereisten voor specifieke kenmerken in het SAML 2.0-bericht.
 
|Kenmerk|Beschrijving|
| ----- | ----- |
|NameID|De waarde van deze verklaring moet hetzelfde zijn als de Azure AD-gebruiker onveranderbare id genoemd. Het kan maximaal 64 alfanumerieke tekens lang zijn. Veilige niet HTML-tekens moeten worden gecodeerd, bijvoorbeeld een teken '+' wordt weergegeven als '.2B'.|
|IDPEmail|De User Principal Name (UPN) wordt vermeld in de SAML-reactie als een element met de naam IDPEmail is dit van de gebruiker UserPrincipalName (UPN) in Azure AD/Office 365. De UPN is in de e-mailadres. De waarde van de UPN in Windows Office 365 (Azure Active Directory).|
|Certificaatverlener|Dit is een URI van de identiteitsprovider vereist. U moet niet opnieuw gebruiken voor de verlener van de voorbeeldberichten. Als er meerdere domeinen van het hoogste niveau in uw Azure AD-tenants de uitgever, moet overeenkomen met de opgegeven URI-instelling geconfigureerd per domein.|

>[!IMPORTANT]
>Azure AD momenteel ondersteunt de volgende NameID indeling URI voor SAML-2.0:urn:oasis:names:tc:SAML:2.0:nameid-indeling: permanente.

## <a name="sample-saml-request-and-response-messages"></a>Voorbeeld SAML-aanvraag en antwoord-berichten
Een combinatie van de bericht-aanvraag en antwoord wordt voor de uitwisseling van berichten voor eenmalige aanmelding weergegeven.
Dit is een voorbeeld-aanvraagbericht dat wordt verzonden vanuit Azure AD voor een voorbeeld SAML 2.0-id-provider. De voorbeeld-SAML 2.0-id-provider is Active Directory Federation Services (AD FS) geconfigureerd voor het gebruik van SAML-P-protocol. Interoperabiliteit testen is ook voltooid met een andere id-providers van SAML 2.0.

    `<samlp:AuthnRequest xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion" ID="_7171b0b2-19f2-4ba2-8f94-24b5e56b7f1e" IssueInstant="2014-01-30T16:18:35Z" Version="2.0" AssertionConsumerServiceIndex="0" >
    <saml:Issuer>urn:federation:MicrosoftOnline</saml:Issuer>
    <samlp:NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
    </samlp:AuthnRequest>`

Dit is een voorbeeld van een antwoordbericht dat wordt verzonden door de identiteitsprovider voorbeeld SAML 2.0 compatibele naar Azure AD / Office 365.

    `<samlp:Response ID="_592c022f-e85e-4d23-b55b-9141c95cd2a5" Version="2.0" IssueInstant="2014-01-31T15:36:31.357Z" Destination="https://login.microsoftonline.com/login.srf" Consent="urn:oasis:names:tc:SAML:2.0:consent:unspecified" InResponseTo="_049917a6-1183-42fd-a190-1d2cbaf9b144" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">http://WS2012R2-0.contoso.com/adfs/services/trust</Issuer>
    <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
    </samlp:Status>
    <Assertion ID="_7e3c1bcd-f180-4f78-83e1-7680920793aa" IssueInstant="2014-01-31T15:36:31.279Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
    <Issuer>http://WS2012R2-0.contoso.com/adfs/services/trust</Issuer>
    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      <ds:SignedInfo>
        <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
        <ds:SignatureMethod Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1" />
        <ds:Reference URI="#_7e3c1bcd-f180-4f78-83e1-7680920793aa">
          <ds:Transforms>
            <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
            <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
          </ds:Transforms>
          <ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1" />
          <ds:DigestValue>CBn/5YqbheaJP425c0pHva9PhNY=</ds:DigestValue>
        </ds:Reference>
      </ds:SignedInfo>
      <ds:SignatureValue>TciWMyHW2ZODrh/2xrvp5ggmcHBFEd9vrp6DYXp+hZWJzmXMmzwmwS8KNRJKy8H7XqBsdELA1Msqi8I3TmWdnoIRfM/ZAyUppo8suMu6Zw+boE32hoQRnX9EWN/f0vH6zA/YKTzrjca6JQ8gAV1ErwvRWDpyMcwdYCiWALv9ScbkAcebOE1s1JctZ5RBXggdZWrYi72X+I4i6WgyZcIGai/rZ4v2otoWAEHS0y1yh1qT7NDPpl/McDaTGkNU6C+8VfjD78DrUXEcAfKvPgKlKrOMZnD1lCGsViimGY+LSuIdY45MLmyaa5UT4KWph6dA==</ds:SignatureValue>
      <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
        <ds:X509Data>
          <ds:X509Certificate>MIIC7jCCAdagAwIBAgIQRrjsbFPaXIlOG3GTv50fkjANBgkqhkiG9w0BAQsFADAzMTEwLwYDVQQDEyhBREZTIFNpZ25pbmcgLSBXUzIwMTJSMi0wLnN3aW5mb3JtZXIuY29tMB4XDTE0MDEyMDE1MTY0MFoXDTE1MDEyMDE1MTY0MFowMzExMC8GA1UEAxMoQURGUyBTaWduaW5nIC0gV1MyMDEyUjItMC5zd2luZm9ybWVyLmNvbTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAKe+rLVmXy1QwCwZwqgbbp1/+3ZWxd9T/jV0hpLIIWr+LCOHqq8n8beJvlivgLmDJo8f+EITnAxWcsJUvVai/35AhHCUq9tc9sqMp5PWtabAEMb2AU72/QlX/72D2/NbGQq1BWYbqUpgpCZ2nSgvlWDHlCiUo//UGsvfox01kjTFlmqQInsJVfRxF5AcCAwEAATANBgkqhkiG9w0BAQsFAAOCAQEAi8c6C4zaTEc7aQiUgvnGQgCbMZbhUXXLGRpjvFLKaQzkwa9eq7WLJibcSNyGXBa/SfT5wJgsm3TPKgSehGAOTirhcqHheZyvBObAScY7GOT+u9pVYp6raFrc7ez3c+CGHeV/tNvy1hJNs12FYH4X+ZCNFIT9tprieR25NCdi5SWUbPZL0tVzJsHc1y92b2M2FxqRDohxQgJvyJOpcg2mSBzZZIkvDg7gfPSUXHVS1MQs0RHSbwq/XdQocUUhl9/e/YWCbNNxlM84BxFsBUok1dH/gzBySx+Fc8zYi7cOq9yaBT3RLT6cGmFGVYZJW4FyhPZOCLVNsLlnPQcX3dDg9A==</ds:X509Certificate>
        </ds:X509Data>
      </KeyInfo>
    </ds:Signature>
    <Subject>
      <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">ABCDEG1234567890</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="_049917a6-1183-42fd-a190-1d2cbaf9b144" NotOnOrAfter="2014-01-31T15:41:31.357Z" Recipient="https://login.microsoftonline.com/login.srf" />
      </SubjectConfirmation>
    </Subject>
    <Conditions NotBefore="2014-01-31T15:36:31.263Z" NotOnOrAfter="2014-01-31T16:36:31.263Z">
      <AudienceRestriction>
        <Audience>urn:federation:MicrosoftOnline</Audience>
      </AudienceRestriction>
    </Conditions>
    <AttributeStatement>
      <Attribute Name="IDPEmail">
        <AttributeValue>administrator@contoso.com</AttributeValue>
      </Attribute>
    </AttributeStatement>
    <AuthnStatement AuthnInstant="2014-01-31T15:36:30.200Z" SessionIndex="_7e3c1bcd-f180-4f78-83e1-7680920793aa">
      <AuthnContext>
        <AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport</AuthnContextClassRef>
      </AuthnContext>
    </AuthnStatement>
    </Assertion>
    </samlp:Response>`

## <a name="configure-your-saml-20-compliant-identity-provider"></a>SAML 2.0 compatibele id-provider configureren
Dit onderwerp bevat richtlijnen voor het configureren van SAML 2.0 id-provider wilt federeren met Azure AD voor één aanmelding toegang tot een of meer Microsoft-cloudservices (zoals Office 365) met behulp van het SAML 2.0-protocol. De SAML 2.0 relying party voor een Microsoft-cloudservice in dit scenario gebruikt is Azure AD.

## <a name="add-azure-ad-metadata"></a>Azure AD-metagegevens toevoegen
Id-provider SAML 2.0 moet in overeenstemming zijn met informatie over de Azure AD relying party. Azure AD publiceert metagegevens op https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml.

Het is raadzaam dat u de meest recente Azure AD-metagegevens altijd importeren bij het configureren van SAML 2.0 id-provider. Houd er rekening mee dat Azure AD niet metagegevens door de identiteitsprovider gelezen.

## <a name="add-azure-ad-as-a-relying-party"></a>Azure AD als een relying party toevoegen
U moet de communicatie tussen uw identiteitsprovider die SAML 2.0 en Azure AD inschakelen. Deze configuratie zijn afhankelijk van uw specifieke id-provider en u moet verwijzen naar de documentatie voor het. Normaal gesproken stelt u de relying party-ID op hetzelfde zijn als de id van de entiteit van de metagegevens van de Azure AD.

>[!NOTE]
>Controleer of dat de klok op de server SAML 2.0-provider is gesynchroniseerd met een nauwkeurige bron. Een onjuiste kloktijd kan leiden tot federatieve aanmeldingen mislukken.

## <a name="install-windows-powershell-for-sign-on-with-saml-20-identity-provider"></a>Installeer Windows PowerShell voor aanmelding met SAML 2.0-id-provider
Nadat u hebt uw SAML 2.0-id-provider voor gebruik met aanmelding van Azure AD geconfigureerd, wordt de volgende stap is het downloaden en installeren van de Azure Active Directory-Module voor Windows PowerShell. Na de installatie configureert u deze cmdlets gebruikt voor het configureren van uw Azure AD-domeinen als federatieve domeinen.

De Azure Active Directory-Module voor Windows PowerShell is een download voor het beheren van gegevens van uw organisatie in Azure AD. Deze module installeert een set cmdlets in Windows PowerShell; uitvoeren van deze cmdlets voor het instellen van eenmalige aanmelding toegang tot Azure AD en daarmee ook voor alle cloudservices die u bent geabonneerd op. Zie voor instructies over het downloaden en installeren van de cmdlets [http://technet.microsoft.com/library/jj151815.aspx](http://technet.microsoft.com/library/jj151815.aspx)

## <a name="set-up-a-trust-between-your-saml-identity-provider-and-azure-ad"></a>Stel een vertrouwensrelatie tussen de SAML-identiteitsprovider en Azure AD
Voordat u federation configureert op een Azure AD-domein, moet een aangepast domein geconfigureerd hebben. Het standaarddomein dat wordt geleverd door Microsoft, kunnen niet worden gefedereerd. Het standaarddomein van Microsoft eindigt op "onmicrosoft.com".
U kunt een reeks cmdlets wordt uitgevoerd in de Windows PowerShell-opdrachtregelinterface toe te voegen of te converteren van domeinen voor eenmalige aanmelding.

Elk Azure Active Directory-domein dat u wilt federeren met uw SAML 2.0-id-provider moet worden toegevoegd als een domein met eenmalige aanmelding of één aanmelding domein van een standaarddomein worden omgezet. Het toevoegen of omzetten van een domein stelt een vertrouwensrelatie tussen uw identiteitsprovider die SAML 2.0 en Azure AD.

De volgende procedure begeleidt u bij het converteren van een bestaand domein standaard een federatieve domein met behulp van SAML 2.0 SP-Lite. Houd er rekening mee dat het domein een storing die van invloed is op gebruikers tot 2 uur nadat u deze stap kan optreden.

## <a name="configuring-a-domain-in-your-azure-ad-directory-for-federation"></a>Configureren van een domein in uw Azure AD-Directory voor Federatie


1. Verbinding maken met uw Azure AD-Directory als tenantbeheerder: verbinding maken met MsolService.
2.  Configureer de gewenste Office 365-domein voor het gebruik van Federatie met SAML 2.0:`$dom = "contoso.com" $BrandName - "Sample SAML 2.0 IDP" $LogOnUrl = "https://WS2012R2-0.contoso.com/passiveLogon" $LogOffUrl = "https://WS2012R2-0.contoso.com/passiveLogOff" $ecpUrl = "https://WS2012R2-0.contoso.com/PAOS" $MyURI = "urn:uri:MySamlp2IDP" $MySigningCert = @" MIIC7jCCAdagAwIBAgIQRrjsbFPaXIlOG3GTv50fkjANBgkqhkiG9w0BAQsFADAzMTEwLwYDVQQDEyh BREZTIFNpZ25pbmcgLSBXUzIwMTJSMi0wLnN3aW5mb3JtZXIuY29tMB4XDTE0MDEyMDE1MTY0MFoXDT E1MDEyMDE1MTY0MFowMzExMC8GA1UEAxMoQURGUyBTaWduaW5nIC0gV1MyMDEyUjItMC5zd2luZm9yb WVyLmNvbTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAKe+rLVmXy1QwCwZwqgbbp1/kupQ VcjKuKLitVDbssFyqbDTjP7WRjlVMWAHBI3kgNT7oE362Gf2WMJFf1b0HcrsgLin7daRXpq4Qi6OA57 sW1YFMj3sqyuTP0eZV3S4+ZbDVob6amsZIdIwxaLP9Zfywg2bLsGnVldB0+XKedZwDbCLCVg+3ZWxd9 T/jV0hpLIIWr+LCOHqq8n8beJvlivgLmDJo8f+EITnAxWcsJUvVai/35AhHCUq9tc9sqMp5PWtabAEM b2AU72/QlX/72D2/NbGQq1BWYbqUpgpCZ2nSgvlWDHlCiUo//UGsvfox01kjTFlmqQInsJVfRxF5AcC AwEAATANBgkqhkiG9w0BAQsFAAOCAQEAi8c6C4zaTEc7aQiUgvnGQgCbMZbhUXXLGRpjvFLKaQzkwa9 eq7WLJibcSNyGXBa/SfT5wJgsm3TPKgSehGAOTirhcqHheZyvBObAScY7GOT+u9pVYp6raFrc7ez3c+ CGHeV/tNvy1hJNs12FYH4X+ZCNFIT9tprieR25NCdi5SWUbPZL0tVzJsHc1y92b2M2FxqRDohxQgJvy JOpcg2mSBzZZIkvDg7gfPSUXHVS1MQs0RHSbwq/XdQocUUhl9/e/YWCbNNxlM84BxFsBUok1dH/gzBy Sx+Fc8zYi7cOq9yaBT3RLT6cGmFGVYZJW4FyhPZOCLVNsLlnPQcX3dDg9A==" "@ $uri = "http://WS2012R2-0.contoso.com/adfs/services/trust" $Protocol = "SAMLP" Set-MsolDomainAuthentication -DomainName $dom -FederationBrandName $dom -Authentication Federated -PassiveLogOnUri $MyURI -ActiveLogOnUri $ecpUrl -SigningCertificate $MySigningCert -IssuerUri $uri -LogOffUri $url -PreferredAuthenticationProtocol $Protocol` 

3.  U kunt de ondertekenen certificaat base64-gecodeerde tekenreeks ophalen uit het bestand IDP-metagegevens. Een voorbeeld van deze locatie is opgegeven, maar kan verschillen enigszins verschillen op basis van uw implementatie.

    `<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol"> <KeyDescriptor use="signing"> <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#"> <X509Data> <X509Certificate>MIIC5jCCAc6gAwIBAgIQLnaxUPzay6ZJsC8HVv/QfTANBgkqhkiG9w0BAQsFADAvMS0wKwYDVQQDEyRBREZTIFNpZ25pbmcgLSBmcy50ZWNobGFiY2VudHJhbC5vcmcwHhcNMTMxMTA0MTgxMzMyWhcNMTQxMTA0MTgxMzMyWjAvMS0wKwYDVQQDEyRBREZTIFNpZ25pbmcgLSBmcy50ZWNobGFiY2VudHJhbC5vcmcwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCwMdVLTr5YTSRp+ccbSpuuFeXMfABD9mVCi2wtkRwC30TIyPdORz642MkurdxdPCWjwgJ0HW6TvXwcO9afH3OC5V//wEGDoNcI8PV4enCzTYFe/h//w51uqyv48Fbb3lEXs+aVl8155OAj2sO9IX64OJWKey82GQWK3g7LfhWWpp17j5bKpSd9DBH5pvrV+Q1ESU3mx71TEOvikHGCZYitEPywNeVMLRKrevdWI3FAhFjcCSO6nWDiMqCqiTDYOURXIcHVYTSof1YotkJ4tG6mP5Kpjzd4VQvnR7Pjb47nhIYG6iZ3mR1F85Ns9+hBWukQWNN2hcD/uGdPXhpdMVpBAgMBAAEwDQYJKoZIhvcNAQELBQADggEBAK7h7jF7wPzhZ1dPl4e+XMAr8I7TNbhgEU3+oxKyW/IioQbvZVw1mYVCbGq9Rsw4KE06eSMybqHln3w5EeBbLS0MEkApqHY+p68iRpguqa+W7UHKXXQVgPMCpqxMFKonX6VlSQOR64FgpBme2uG+LJ8reTgypEKspQIN0WvtPWmiq4zAwBp08hAacgv868c0MM4WbOYU0rzMIR6Q+ceGVRImlCwZ5b7XKp4mJZ9hlaRjeuyVrDuzBkzROSurX1OXoci08yJvhbtiBJLf3uPOJHrhjKRwIt2TnzS9ElgFZlJiDIA26Athe73n43CT0af2IG6yC7e6sK4L3NEXJrwwUZk=</X509Certificate> </X509Data> </KeyInfo> </KeyDescriptor>` 

Zie voor meer informatie over 'Set MsolDomainAuthentication': [http://technet.microsoft.com/library/dn194112.aspx](http://technet.microsoft.com/library/dn194112.aspx).

>[!NOTE]
>Gebruik moet worden uitgevoerd ' $ecpUrl 'https://WS2012R2-0.contoso.com/PAOS' = ' alleen als u een uitbreiding ECP ingesteld voor de id-provider. Exchange Online-clients, met uitzondering van Outlook Web Application (OWA) zijn afhankelijk van een bericht op basis van actieve eindpunt. Als uw SAML 2.0 STS een actieve eindpunt die vergelijkbaar met de Shibboleth ECP uitvoering van een actieve eindpunt implementeert mogelijk voor deze uitgebreide clients om te communiceren met de Exchange Online-service.

Zodra federation is geconfigureerd, kunt u terug naar 'niet-gefedereerde' (of 'beheerde'), maar deze wijziging wordt van maximaal twee uur en nieuwe willekeurige wachtwoorden voor aanmelding bij in de cloud toewijzen aan elke gebruiker is vereist. Overschakelen naar 'beheerde' kan in sommige scenario's vereist een fout in de instellingen opnieuw instellen. Zie voor meer informatie over domein-conversie: [http://msdn.microsoft.com/library/windowsazure/dn194122.aspx](http://msdn.microsoft.com/library/windowsazure/dn194122.aspx).

## <a name="provision-user-principals-to-azure-ad--office-365"></a>Inrichten van de gebruiker principals naar Azure AD / Office 365
Voordat u kunt uw gebruikers op Office 365 verifiëren, moet u Azure AD inrichten met gebruiker principals die met de bevestiging van de claim SAML 2.0 overeenkomen. Als deze gebruiker principals niet bekend zijn bij Azure AD van tevoren kunnen niet vervolgens zij worden gebruikt voor federatieve aanmelding. Azure AD Connect of Windows PowerShell kan worden gebruikt voor het inrichten van principals die gebruiker.

Azure AD Connect kan worden gebruikt voor het inrichten van principals naar uw domeinen in uw Azure AD-Directory van de lokale Active Directory. Voor meer informatie gedetailleerde, Zie [uw on-premises adreslijsten integreren met Azure Active Directory](active-directory-aadconnect.md).

Windows PowerShell kan ook worden gebruikt voor het automatiseren van nieuwe gebruikers toevoegen aan Azure AD en om wijzigingen van de on-premises directory te synchroniseren. Gebruik de Windows PowerShell-cmdlets die u moet downloaden de [Azure Active Directory-Modules](https://docs.microsoft.com/powershell/azure/install-adv2?view=azureadps-2.0).

Deze procedure wordt beschreven hoe een enkele gebruiker toevoegen aan Azure AD.


1. Verbinding maken met uw Azure AD-Directory als tenantbeheerder: verbinding maken met MsolService.
2.  Maak een nieuwe gebruiker-principal:` New-MsolUser
        -UserPrincipalName elwoodf1@contoso.com
        -ImmutableId ABCDEFG1234567890
        -DisplayName "Elwood Folk"
        -FirstName Elwood 
        -LastName Folk 
        -AlternateEmailAddresses "Elwood.Folk@contoso.com" 
        -LicenseAssignment "samlp2test:ENTERPRISEPACK" 
        -UsageLocation "US" ` 

Voor meer informatie over 'New-MsolUser' uitchecken, [http://technet.microsoft.com/library/dn194096.aspx](http://technet.microsoft.com/library/dn194096.aspx)

>[!NOTE]
>De waarde 'UserPrinciplName' moet overeenkomen met de waarde die u in uw claim SAML 2.0 voor 'IDPEmail' verzendt en de waarde "Onveranderbare id genoemd" moet overeenkomen met de waarde in de verklaring 'NameID' verzonden.

## <a name="verify-single-sign-on-with-your-saml-20-idp"></a>Controleer of eenmalige aanmelding met uw SAML 2.0 IDP
Als de beheerder voordat u controleren en beheren van eenmalige aanmelding (ook wel identiteitsfederatie genoemd), lees de informatie en voer de stappen in de volgende artikelen voor het instellen van eenmalige aanmelding met SAML 2.0 SP-Lite gebaseerde id-provider:


1.  U hebt gecontroleerd dat de vereisten voor Azure AD SAML 2.0-Protocol
2.  U hebt geconfigureerd SAML 2.0 id-provider
3.  Windows PowerShell installeren voor eenmalige aanmelding met SAML 2.0-id-provider
4.  Stel een vertrouwensrelatie tussen identiteitsprovider die SAML 2.0 en Azure AD
5.  Een UPN bekende testen met Azure Active Directory (Office 365) ingericht via Windows PowerShell of Azure AD Connect.
6.  Configureren met behulp van directory-synchronisatie [Azure AD Connect](active-directory-aadconnect.md).

Na het instellen van eenmalige aanmelding met je SAML 2.0 SP-Lite op basis van identiteit Provider, moet u controleren of deze correct werkt.

>[!NOTE]
>Als u een domein, in plaats van toevoegen van een geconverteerd, kan het instellen van eenmalige aanmelding tot 24 uur duren.
Voordat u eenmalige aanmelding verifiëren, moet u de installatie van Active Directory-synchronisatie voltooid, Synchroniseer uw mappen en de gesynchroniseerde gebruikers activeren.

### <a name="use-the-tool-to-verify-that-single-sign-on-has-been-set-up-correctly"></a>Het hulpprogramma gebruiken om te controleren dat eenmalige aanmelding is ingesteld correct
Om te controleren dat eenmalige aanmelding heeft is ingesteld, kunt u de volgende procedure om te bevestigen dat u zich kunt aanmelden bij de cloudservice met uw bedrijfsreferenties uitvoeren.

Microsoft is een hulpprogramma dat u gebruiken kunt voor het testen van uw gebaseerde SAML 2.0-identiteitsprovider opgegeven. Voordat u het hulpprogramma test als u een Azure AD-tenant wilt federeren met uw id-provider hebt geconfigureerd.

>[!NOTE]
>De connectiviteit Analyzer vereist Internet Explorer 10 of hoger.



1. Downloaden van de analysefunctie voor connectiviteit van [https://testconnectivity.microsoft.com/?tabid=Client](https://testconnectivity.microsoft.com/?tabid=Client).
2.  Klik op Nu installeren om te beginnen met het downloaden en installeren van het hulpprogramma.
3.  Selecteer 'Ik kan geen instellen Federatie met Office 365, Azure of andere services die gebruikmaken van Azure Active Directory'.
4.  Nadat het hulpprogramma gedownload en wordt uitgevoerd is, ziet u het connectiviteit Diagnostics-venster. Het hulpprogramma wordt u stapsgewijs uw federation-verbinding testen.
5.  De connectiviteit Analyzer uw SAML 2.0 IDP kunt aanmelden, voer de referenties voor de user principal die u wilt testen wordt geopend: ![SAML](media/active-directory-aadconnect-federation-saml-idp/saml1.png)
6.  Tijdens de test aanmelden venster van de federatieve moet u een accountnaam en wachtwoord invoeren voor de Azure AD-tenant die is geconfigureerd om te worden gefedereerd met SAML 2.0 id-provider. Het hulpprogramma probeert aan te melden met behulp van deze referenties en gedetailleerde resultaten van tests die worden uitgevoerd tijdens de poging aanmelden wordt geleverd als uitvoer.
![SAML](media/active-directory-aadconnect-federation-saml-idp/saml2.png)
7. Dit venster ziet u een mislukte resultaat van de testen. Te klikken op controleren de resultaten van de gedetailleerde informatie over de resultaten voor elke test die werd uitgevoerd, weergegeven. U kunt ook de resultaten naar de schijf om te kunnen delen opslaan.
 
>[!NOTE]
>Test de connectiviteit analyzer ook Active Federation met WS *-protocollen op basis en ECP/PAOS. Als u deze niet gebruikt u de volgende fout kunt negeren: de stroom voor actieve aanmelden met behulp van de identiteitsprovider Active federation eindpunt testen.

### <a name="manually-verify-that-single-sign-on-has-been-set-up-correctly"></a>Handmatig te verifiëren dat eenmalige aanmelding is ingesteld correct
Handmatige verificatie biedt extra stappen die u ondernemen kunt om ervoor te zorgen dat uw identiteit SAML 2.0 Provider correct in veel scenario's werkt.
Om te controleren dat eenmalige aanmelding heeft is ingesteld, moet u de volgende stappen uitvoeren:


1. Op een computer lid van een domein, moet u zich aanmelden bij uw cloudservice met dezelfde aanmeldingsnaam die u voor uw zakelijke referenties gebruiken.
2.  Klik in het wachtwoord. Als eenmalige aanmelding is ingesteld, het wachtwoord grijs worden weergegeven wordt en u het volgende bericht ziet: ' nu moet u aanmelden bij <your company>. "
3.  Klik op de aanmeldingspagina op <your company> koppeling. Als u zich kunt aanmelden, is klikt u vervolgens eenmalige aanmelding ingesteld.

## <a name="next-steps"></a>Volgende stappen


- [Active Directory Federation Services-beheer en aanpassingen met Azure AD Connect](active-directory-aadconnect-federation-management.md)
- [Compatibiliteitslijst voor Azure AD-federatie](active-directory-aadconnect-federation-compatibility.md)
- [Azure AD Connect aangepaste installatie](active-directory-aadconnect-get-started-custom.md)
