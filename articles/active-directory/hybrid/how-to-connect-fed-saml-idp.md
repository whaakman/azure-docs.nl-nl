---
title: 'Azure AD Connect: Gebruik een SAML 2.0-id-Provider voor eenmalige aanmelding op | Microsoft Docs'
description: Dit document beschrijft met behulp van een SAML 2.0 compatibele id-provider voor eenmalige aanmelding op.
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
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: e3bd48cf56650e266f5002a179d20177b3127f25
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52426398"
---
#  <a name="use-a-saml-20-identity-provider-idp-for-single-sign-on"></a>Een SAML 2.0-id-Provider (IdP) gebruiken voor eenmalige aanmelding op

Dit document bevat informatie over het gebruik van een SAML 2.0 compatibel SP-Lite-profiel op basis van id-Provider als de gewenste Security Token Service (STS) / id-provider. In dit scenario is nuttig wanneer u een gebruikersmap en een wachtwoord voor het opslaan van on-premises die kunnen worden benaderd met behulp van SAML 2.0 al hebt. Deze bestaande gebruikersmap kan worden gebruikt voor aanmelding bij Office 365 en andere Azure AD-beveiligde bronnen. Het SAML 2.0 SP-Lite-profiel is gebaseerd op de standaard voor federatieve identiteiten met gebruikte Security Assertion Markup Language (SAML) om een aanmelding en kenmerk exchange-framework.

>[!NOTE]
>Zie voor een lijst met 3e partij id-providers die zijn getest voor gebruik met Azure AD de [federatiecompatibiliteitslijst van Azure AD](how-to-connect-fed-compatibility.md)

Microsoft biedt ondersteuning voor deze aanmelding als de integratie van een Microsoft-cloudservice, zoals Office 365, met uw correct geconfigureerde SAML 2.0-profiel op basis van id-provider. SAML 2.0-id-providers zijn producten van derden en daarom Microsoft biedt geen ondersteuning voor de implementatie, configuratie, het oplossen van aanbevolen procedures met betrekking tot deze. Eenmaal correct is geconfigureerd, de integratie met de SAML 2.0-id-provider voor de juiste configuratie kan worden getest met behulp van de Microsoft Connectivity Analyzer Tool, die in meer detail hieronder wordt beschreven. Voor meer informatie over uw SAML 2.0 SP-Lite-profiel op basis van id-provider, vraagt u de organisatie die deze heeft geleverd.

>[!IMPORTANT]
>Alleen een beperkte set clients zijn beschikbaar in dit scenario aanmelding met SAML 2.0-id-providers, wordt dit omvat:

>- Clients op het web zoals Outlook Web Access en SharePoint Online
- E-rich clients die gebruikmaken van basisverificatie en op een ondersteunde methode van de toegang tot Exchange, zoals IMAP, pop-server, ActiveSync, MAPI, enz. (het eindpunt van de Enhanced-clientprotocol is vereist om te worden geïmplementeerd), met inbegrip van:
    - Microsoft Outlook 2010/Outlook 2013/Outlook 2016, Apple iPhone (verschillende iOS-versies)
    - Verschillende Google Android-apparaten
    - Windows Phone 7, Windows Phone 7.8 en Windows Phone 8.0
    - E-mailclient van Windows 8 en Windows 8.1-e-mailclient
    - E-mailclient van Windows 10

Alle andere clients zijn niet beschikbaar in dit scenario aanmelding met uw SAML 2.0-id-Provider. Bijvoorbeeld, kan de bureaubladclient Lync 2010 niet aan te melden bij de service met uw SAML 2.0-identiteitsprovider geconfigureerd voor eenmalige aanmelding.

## <a name="azure-ad-saml-20-protocol-requirements"></a>Vereisten voor Azure AD-SAML 2.0-protocol
Dit document bevat gedetailleerde vereisten op het protocol en de berichtindeling dat uw SAML 2.0-id-provider implementeren moet om te federeren met Azure AD om eenmalige aanmelding in een of meer Microsoft cloud-services (zoals Office 365). De SAML 2.0 relying party (SP-STS) voor een Microsoft cloud-service die wordt gebruikt in dit scenario is Azure AD.

Het is raadzaam dat u ervoor dat uw id-provider SAML 2.0 zorgen Uitvoerberichten als die vergelijkbaar is met de opgegeven voorbeeld traceringen worden mogelijk. Daarnaast gebruik van specifieke kenmerkwaarden van de opgegeven Azure AD-metagegevens waar mogelijk. Als u tevreden met uw Uitvoerberichten bent, kunt u testen met de Microsoft Connectivity Analyzer zoals hieronder wordt beschreven.

De metagegevens van de Azure AD kan worden gedownload vanaf deze URL: [ https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml ](https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml).
Voor klanten in China met behulp van de China-specifieke exemplaar van Office 365, het volgende federation-eindpunt moet worden gebruikt: [ https://nexus.partner.microsoftonline-p.cn/federationmetadata/saml20/federationmetadata.xml ](https://nexus.partner.microsoftonline-p.cn/federationmetadata/saml20/federationmetadata.xml).

## <a name="saml-protocol-requirements"></a>Vereisten voor SAML-protocol
In deze sectie details op hoe de aanvraag en respons bericht-paren worden samengesteld om te helpen u om uw berichten correct.

Azure AD kan worden geconfigureerd om te werken met de id-providers die gebruikmaken van het SAML 2.0 SP Lite-profiel met enkele specifieke vereisten, zoals hieronder vermeld. Met behulp van het voorbeeld van SAML aanvraag- en responsberichten samen met automatische en handmatige testen, kunt u werken interoperabiliteit met Azure AD te bereiken.

## <a name="signature-block-requirements"></a>Vereisten voor handtekening blokkeren
Knooppunt voor de handtekening bevat in het SAML-antwoord-bericht, informatie over de digitale handtekening van het bericht zelf. Het handtekeningblok gelden de volgende vereisten:

1. Het knooppunt assertion zelf moet worden ondertekend.
2.  Het RSA-sha1-algoritme moet worden gebruikt als de DigestMethod. Andere algoritmen digitale handtekening worden niet geaccepteerd.
   `<ds:DigestMethod Algorithm="https://www.w3.org/2000/09/xmldsig#sha1"/>`
3.  U kunt zich ook het XML-document. 
4.  De transformatie-algoritme moet overeenkomen met de waarden in het volgende voorbeeld:    `<ds:Transform Algorithm="https://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
       <ds:Transform Algorithm="https://www.w3.org/2001/10/xml-exc-c14n#"/>`
9.  Het algoritme SignatureMethod moet overeenkomen met het volgende voorbeeld:   `<ds:SignatureMethod Algorithm="https://www.w3.org/2000/09/xmldsig#rsa-sha1"/>`

## <a name="supported-bindings"></a>Ondersteunde bindingen
Bindingen zijn de communicatie met betrekking tot de transport-parameters die vereist zijn. De volgende vereisten van toepassing op de bindingen

1. HTTPS is het vereist transport.
2.  Azure AD is HTTP POST vereist voor het indienen van token tijdens het aanmelden
3.  Azure AD gebruikt HTTP POST voor de verificatieaanvraag naar de id-provider en de OMLEIDINGS-voor het afmeldingsbericht met de id-provider.

## <a name="required-attributes"></a>Vereiste kenmerken
Deze tabel bevat de vereisten voor specifieke kenmerken in het SAML 2.0-bericht.
 
|Kenmerk|Beschrijving|
| ----- | ----- |
|NameID|De waarde van deze verklaring moet gelijk zijn aan de ImmutableID van de Azure AD-gebruiker. Het kan maximaal 64 alfanumerieke tekens zijn. Alle niet-HTML-veilige tekens moeten worden gecodeerd, bijvoorbeeld een teken '+' wordt weergegeven als '.2B'.|
|IDPEmail|De User Principal Name (UPN) in het SAML-antwoord wordt vermeld als een element met de naam IDPEmail van de gebruiker UserPrincipalName (UPN) in Azure AD/Office 365. De UPN is in de e-mailadres. UPN-waarde in Office 365 (Azure Active Directory) voor Windows.|
|Verlener|Vereist om te worden van een URI van de id-provider. Niet opnieuw gebruiken met de uitgever van de voorbeeldberichten. Als u meerdere domeinen voor op het hoogste niveau in uw Azure AD-tenants de uitgever, moet overeenkomen met de opgegeven URI-instelling geconfigureerd per domein.|

>[!IMPORTANT]
>Azure AD ondersteunt momenteel de volgende URI van de NameID-indeling voor SAML-2.0:urn:oasis:names:tc:SAML:2.0:nameid-indeling: permanente.

## <a name="sample-saml-request-and-response-messages"></a>Voorbeeld van SAML-aanvraag en antwoord berichten
Een paar aanvragen en reacties bericht wordt weergegeven voor de uitwisseling van berichten voor aanmelding.
Hier volgt een voorbeeld-aanvraagbericht dat wordt verzonden vanuit Azure AD met een voorbeeld van SAML 2.0-id-provider. Het voorbeeld van SAML 2.0-id-provider is Active Directory Federation Services (AD FS) geconfigureerd voor het gebruik van SAML-P-protocol. Interoperabiliteit testen is ook voltooid met andere id-providers van SAML 2.0.

    `<samlp:AuthnRequest xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion" ID="_7171b0b2-19f2-4ba2-8f94-24b5e56b7f1e" IssueInstant="2014-01-30T16:18:35Z" Version="2.0" AssertionConsumerServiceIndex="0" >
    <saml:Issuer>urn:federation:MicrosoftOnline</saml:Issuer>
    <samlp:NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
    </samlp:AuthnRequest>`

Hieronder volgt een voorbeeld-antwoordbericht dat wordt verzonden door het voorbeeld van SAML 2.0 compatibele id-provider naar Azure AD / Office 365.

    `<samlp:Response ID="_592c022f-e85e-4d23-b55b-9141c95cd2a5" Version="2.0" IssueInstant="2014-01-31T15:36:31.357Z" Destination="https://login.microsoftonline.com/login.srf" Consent="urn:oasis:names:tc:SAML:2.0:consent:unspecified" InResponseTo="_049917a6-1183-42fd-a190-1d2cbaf9b144" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">http://WS2012R2-0.contoso.com/adfs/services/trust</Issuer>
    <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
    </samlp:Status>
    <Assertion ID="_7e3c1bcd-f180-4f78-83e1-7680920793aa" IssueInstant="2014-01-31T15:36:31.279Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
    <Issuer>http://WS2012R2-0.contoso.com/adfs/services/trust</Issuer>
    <ds:Signature xmlns:ds="https://www.w3.org/2000/09/xmldsig#">
      <ds:SignedInfo>
        <ds:CanonicalizationMethod Algorithm="https://www.w3.org/2001/10/xml-exc-c14n#" />
        <ds:SignatureMethod Algorithm="https://www.w3.org/2000/09/xmldsig#rsa-sha1" />
        <ds:Reference URI="#_7e3c1bcd-f180-4f78-83e1-7680920793aa">
          <ds:Transforms>
            <ds:Transform Algorithm="https://www.w3.org/2000/09/xmldsig#enveloped-signature" />
            <ds:Transform Algorithm="https://www.w3.org/2001/10/xml-exc-c14n#" />
          </ds:Transforms>
          <ds:DigestMethod Algorithm="https://www.w3.org/2000/09/xmldsig#sha1" />
          <ds:DigestValue>CBn/5YqbheaJP425c0pHva9PhNY=</ds:DigestValue>
        </ds:Reference>
      </ds:SignedInfo>
      <ds:SignatureValue>TciWMyHW2ZODrh/2xrvp5ggmcHBFEd9vrp6DYXp+hZWJzmXMmzwmwS8KNRJKy8H7XqBsdELA1Msqi8I3TmWdnoIRfM/ZAyUppo8suMu6Zw+boE32hoQRnX9EWN/f0vH6zA/YKTzrjca6JQ8gAV1ErwvRWDpyMcwdYCiWALv9ScbkAcebOE1s1JctZ5RBXggdZWrYi72X+I4i6WgyZcIGai/rZ4v2otoWAEHS0y1yh1qT7NDPpl/McDaTGkNU6C+8VfjD78DrUXEcAfKvPgKlKrOMZnD1lCGsViimGY+LSuIdY45MLmyaa5UT4KWph6dA==</ds:SignatureValue>
      <KeyInfo xmlns="https://www.w3.org/2000/09/xmldsig#">
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

## <a name="configure-your-saml-20-compliant-identity-provider"></a>Uw SAML 2.0-compatibele id-provider configureren
In deze sectie bevat richtlijnen over het configureren van uw SAML 2.0-id-provider om te federeren met Azure AD voor eenmalige aanmelding toegang tot een of meer Microsoft-cloudservices (zoals Office 365) met behulp van het SAML 2.0-protocol. Het SAML 2.0 relying party voor een Microsoft cloud-service die wordt gebruikt in dit scenario is Azure AD.

## <a name="add-azure-ad-metadata"></a>Azure AD-metagegevens toevoegen
Uw SAML 2.0-id-provider moet in overeenstemming zijn met informatie over de Azure AD relying party. Azure AD worden de metagegevens van de op gepubliceerd https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml.

Het is raadzaam dat u de metagegevens van de meest recente Azure AD altijd importeren bij het configureren van uw identiteitsprovider SAML 2.0.

>[!NOTE]
>Azure AD biedt metagegevens niet lezen uit de id-provider.

## <a name="add-azure-ad-as-a-relying-party"></a>Azure AD als een relying party toevoegen
Communicatie tussen uw id-provider van SAML 2.0 en Azure AD, moet u inschakelen. Deze configuratie zijn afhankelijk van uw specifieke id-provider en u moet verwijzen naar de documentatie voor het. U zou de relying party-ID meestal ingesteld op hetzelfde als de id van de entiteit van de metagegevens van de Azure AD.

>[!NOTE]
>Controleer of dat de klok op de server van SAML 2.0-id-provider wordt gesynchroniseerd met een nauwkeurige tijdbronnen. Een onjuiste clock-tijd kan leiden tot federatieve aanmelding mislukken.

## <a name="install-windows-powershell-for-sign-on-with-saml-20-identity-provider"></a>Windows PowerShell voor aanmelding met SAML 2.0-id-provider installeren
Nadat u hebt uw SAML 2.0-id-provider voor gebruik met Azure AD-aanmelding geconfigureerd, wordt de volgende stap is om te downloaden en installeren van de Azure Active Directory-Module voor Windows PowerShell. Na de installatie, gebruikt u deze cmdlets naar uw Azure AD-domeinen configureren als federatieve domeinen.

De Azure Active Directory-Module voor Windows PowerShell is een download voor het beheren van gegevens van uw organisatie in Azure AD. Deze module wordt een set cmdlets in Windows PowerShell; geïnstalleerd u voert deze cmdlets voor het instellen van eenmalige aanmelding toegang tot Azure AD en in inschakelen als u wilt alle cloudservices die u bent geabonneerd op. Zie voor instructies over het downloaden en installeren van de cmdlets [https://technet.microsoft.com/library/jj151815.aspx](httpss://technet.microsoft.com/library/jj151815.aspx)

## <a name="set-up-a-trust-between-your-saml-identity-provider-and-azure-ad"></a>Instellen van een vertrouwensrelatie tussen de SAML-identiteitsprovider en Azure AD
Voordat u Federatie configureert op een Azure AD-domein, heeft dit een aangepast domein geconfigureerd. Het standaarddomein die wordt geleverd door Microsoft kunnen niet worden gefedereerd. Het standaarddomein van Microsoft eindigt op 'onmicrosoft.com'.
U kunt een reeks cmdlets wordt uitgevoerd in de Windows PowerShell-opdrachtregelinterface toe te voegen of te converteren van domeinen voor eenmalige aanmelding.

Elk Azure Active Directory-domein dat u wilt federeren met uw SAML 2.0-id-provider moet toegevoegd als een enkel domein aanmelding of omgezet in een domein zijn eenmalige aanmelding van een standard-domein. Toe te voegen of het converteren van een domein stelt u een vertrouwensrelatie tussen uw id-provider van SAML 2.0 en Azure AD.

De volgende procedure begeleidt u bij het converteren van een bestaande standaard domein naar een federatief domein met behulp van SAML 2.0 SP-Lite. 

>[!NOTE]
>Uw domein kan zich een storing die gevolgen heeft voor gebruikers om 2 uur nadat u deze stap.

## <a name="configuring-a-domain-in-your-azure-ad-directory-for-federation"></a>Configureren van een domein in uw Azure AD-Directory voor Federatie


1. Verbinding maken met uw Azure AD-Directory als tenantbeheerder: Connect MsolService.
2.  Configureer uw gewenste Office 365-domein voor het gebruik van Federatie met SAML 2.0: `$dom = "contoso.com" $BrandName - "Sample SAML 2.0 IDP" $LogOnUrl = "https://WS2012R2-0.contoso.com/passiveLogon" $LogOffUrl = "https://WS2012R2-0.contoso.com/passiveLogOff" $ecpUrl = "https://WS2012R2-0.contoso.com/PAOS" $MyURI = "urn:uri:MySamlp2IDP" $MySigningCert = @" MIIC7jCCAdagAwIBAgIQRrjsbFPaXIlOG3GTv50fkjANBgkqhkiG9w0BAQsFADAzMTEwLwYDVQQDEyh BREZTIFNpZ25pbmcgLSBXUzIwMTJSMi0wLnN3aW5mb3JtZXIuY29tMB4XDTE0MDEyMDE1MTY0MFoXDT E1MDEyMDE1MTY0MFowMzExMC8GA1UEAxMoQURGUyBTaWduaW5nIC0gV1MyMDEyUjItMC5zd2luZm9yb WVyLmNvbTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAKe+rLVmXy1QwCwZwqgbbp1/kupQ VcjKuKLitVDbssFyqbDTjP7WRjlVMWAHBI3kgNT7oE362Gf2WMJFf1b0HcrsgLin7daRXpq4Qi6OA57 sW1YFMj3sqyuTP0eZV3S4+ZbDVob6amsZIdIwxaLP9Zfywg2bLsGnVldB0+XKedZwDbCLCVg+3ZWxd9 T/jV0hpLIIWr+LCOHqq8n8beJvlivgLmDJo8f+EITnAxWcsJUvVai/35AhHCUq9tc9sqMp5PWtabAEM b2AU72/QlX/72D2/NbGQq1BWYbqUpgpCZ2nSgvlWDHlCiUo//UGsvfox01kjTFlmqQInsJVfRxF5AcC AwEAATANBgkqhkiG9w0BAQsFAAOCAQEAi8c6C4zaTEc7aQiUgvnGQgCbMZbhUXXLGRpjvFLKaQzkwa9 eq7WLJibcSNyGXBa/SfT5wJgsm3TPKgSehGAOTirhcqHheZyvBObAScY7GOT+u9pVYp6raFrc7ez3c+ CGHeV/tNvy1hJNs12FYH4X+ZCNFIT9tprieR25NCdi5SWUbPZL0tVzJsHc1y92b2M2FxqRDohxQgJvy JOpcg2mSBzZZIkvDg7gfPSUXHVS1MQs0RHSbwq/XdQocUUhl9/e/YWCbNNxlM84BxFsBUok1dH/gzBy Sx+Fc8zYi7cOq9yaBT3RLT6cGmFGVYZJW4FyhPZOCLVNsLlnPQcX3dDg9A==" "@ $uri = "http://WS2012R2-0.contoso.com/adfs/services/trust" $Protocol = "SAMLP" Set-MsolDomainAuthentication -DomainName $dom -FederationBrandName $dom -Authentication Federated -PassiveLogOnUri $MyURI -ActiveLogOnUri $ecpUrl -SigningCertificate $MySigningCert -IssuerUri $uri -LogOffUri $url -PreferredAuthenticationProtocol $Protocol` 

3.  U kunt de ondertekenen certificaat base64-gecodeerde tekenreeks verkrijgen uit het metagegevensbestand van uw id-provider. Een voorbeeld van deze locatie is opgegeven, maar kan verschillen enigszins afhankelijk van uw implementatie.

    `<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol"> <KeyDescriptor use="signing"> <KeyInfo xmlns="https://www.w3.org/2000/09/xmldsig#"> <X509Data> <X509Certificate>MIIC5jCCAc6gAwIBAgIQLnaxUPzay6ZJsC8HVv/QfTANBgkqhkiG9w0BAQsFADAvMS0wKwYDVQQDEyRBREZTIFNpZ25pbmcgLSBmcy50ZWNobGFiY2VudHJhbC5vcmcwHhcNMTMxMTA0MTgxMzMyWhcNMTQxMTA0MTgxMzMyWjAvMS0wKwYDVQQDEyRBREZTIFNpZ25pbmcgLSBmcy50ZWNobGFiY2VudHJhbC5vcmcwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCwMdVLTr5YTSRp+ccbSpuuFeXMfABD9mVCi2wtkRwC30TIyPdORz642MkurdxdPCWjwgJ0HW6TvXwcO9afH3OC5V//wEGDoNcI8PV4enCzTYFe/h//w51uqyv48Fbb3lEXs+aVl8155OAj2sO9IX64OJWKey82GQWK3g7LfhWWpp17j5bKpSd9DBH5pvrV+Q1ESU3mx71TEOvikHGCZYitEPywNeVMLRKrevdWI3FAhFjcCSO6nWDiMqCqiTDYOURXIcHVYTSof1YotkJ4tG6mP5Kpjzd4VQvnR7Pjb47nhIYG6iZ3mR1F85Ns9+hBWukQWNN2hcD/uGdPXhpdMVpBAgMBAAEwDQYJKoZIhvcNAQELBQADggEBAK7h7jF7wPzhZ1dPl4e+XMAr8I7TNbhgEU3+oxKyW/IioQbvZVw1mYVCbGq9Rsw4KE06eSMybqHln3w5EeBbLS0MEkApqHY+p68iRpguqa+W7UHKXXQVgPMCpqxMFKonX6VlSQOR64FgpBme2uG+LJ8reTgypEKspQIN0WvtPWmiq4zAwBp08hAacgv868c0MM4WbOYU0rzMIR6Q+ceGVRImlCwZ5b7XKp4mJZ9hlaRjeuyVrDuzBkzROSurX1OXoci08yJvhbtiBJLf3uPOJHrhjKRwIt2TnzS9ElgFZlJiDIA26Athe73n43CT0af2IG6yC7e6sK4L3NEXJrwwUZk=</X509Certificate> </X509Data> </KeyInfo> </KeyDescriptor>` 

Zie voor meer informatie over "Set-MsolDomainAuthentication": [ https://technet.microsoft.com/library/dn194112.aspx ](httpss://technet.microsoft.com/library/dn194112.aspx).

>[!NOTE]
>Gebruik moet worden uitgevoerd ' $ecpUrl = "https://WS2012R2-0.contoso.com/PAOS" "alleen als u een extensie ECP voor uw id-provider instellen. Exchange Online clients, met uitzondering van Outlook Web Application (OWA) afhankelijk zijn van een bericht op basis van actieve eindpunt. Als uw SAML 2.0 STS een actieve eindpunt die vergelijkbaar is met de Shibboleth ECP implementatie van een actieve eindpunt implementeert is het mogelijk dat het mogelijk dat deze rich clients om te communiceren met de Exchange Online-service.

Nadat federation is geconfigureerd u wilt terugkeren naar 'niet-gefedereerde' (of 'beheerd'), maar deze wijziging wordt van maximaal twee uur en hiervoor nieuwe willekeurige wachtwoorden voor cloud-gebaseerde aanmelding toewijzen aan elke gebruiker. Overschakelen naar 'beheerd' kan in sommige scenario's vereist een fout in de instellingen opnieuw instellen. Zie voor meer informatie over de conversie van domein: [ https://msdn.microsoft.com/library/windowsazure/dn194122.aspx ](httpss://msdn.microsoft.com/library/windowsazure/dn194122.aspx).

## <a name="provision-user-principals-to-azure-ad--office-365"></a>Inrichten van gebruikersprincipals met Azure AD / Office 365
Voordat u uw gebruikers met Office 365 verifiëren kunt, moet u Azure AD inrichten met gebruikersprincipals zijn die met de verklaring in de SAML 2.0-claim overeenkomen. Als deze gebruikersprincipals zijn niet bekend zijn bij Azure AD van tevoren regelen, worden klikt u vervolgens ze niet gebruikt voor federatieve aanmelding. Azure AD Connect of Windows PowerShell kan worden gebruikt voor het inrichten van gebruikersprincipals.

Azure AD Connect kan worden gebruikt voor het inrichten van beveiligings-principals voor uw domeinen in uw Azure AD-Directory uit de on-premises Active Directory. Zie voor meer informatie, [uw on-premises directory's integreren met Azure Active Directory](whatis-hybrid-identity.md).

Windows PowerShell kan ook worden gebruikt voor het automatiseren van nieuwe gebruikers toevoegen aan Azure AD en om te synchroniseren van wijzigingen van de on-premises directory. De Windows PowerShell-cmdlets wilt gebruiken, moet u het downloaden de [Azure Active Directory-Modules](https://docs.microsoft.com/powershell/azure/install-adv2?view=azureadps-2.0).

Deze procedure laat zien hoe een enkele gebruiker toevoegen aan Azure AD.


1. Verbinding maken met uw Azure AD-Directory als tenantbeheerder: Connect MsolService.
2.  Maak een nieuwe gebruiker-principal: ` New-MsolUser
        -UserPrincipalName elwoodf1@contoso.com
        -ImmutableId ABCDEFG1234567890
        -DisplayName "Elwood Folk"
        -FirstName Elwood 
        -LastName Folk 
        -AlternateEmailAddresses "Elwood.Folk@contoso.com" 
        -LicenseAssignment "samlp2test:ENTERPRISEPACK" 
        -UsageLocation "US" ` 

Voor meer informatie over "New-MsolUser" Afhandeling, [https://technet.microsoft.com/library/dn194096.aspx](httpss://technet.microsoft.com/library/dn194096.aspx)

>[!NOTE]
>De waarde "UserPrinciplName" moet overeenkomen met de waarde die u voor 'IDPEmail' in het SAML 2.0-claim verzendt en de waarde "ImmutableID" moet overeenkomen met de waarde die wordt verzonden in uw bewering 'NameID'.

## <a name="verify-single-sign-on-with-your-saml-20-idp"></a>Controleer of eenmalige aanmelding met uw SAML 2.0 IDP
Als de beheerder voordat u controleren en beheren van eenmalige aanmelding (ook wel identiteitsfederatie genoemd), lees de informatie en volg de stappen in de volgende artikelen voor het instellen van eenmalige aanmelding met uw SAML 2.0 SP-Lite op basis van id-provider:


1.  U hebt gecontroleerd dat de vereisten voor Azure AD SAML 2.0-Protocol
2.  U kunt uw SAML 2.0-id-provider hebt geconfigureerd
3.  Windows PowerShell installeren voor eenmalige aanmelding met SAML 2.0-id-provider
4.  Instellen van een vertrouwensrelatie tussen SAML 2.0-id-provider en Azure AD
5.  De UPN van een bekende testen met Azure Active Directory (Office 365) ingericht via Windows PowerShell of Azure AD Connect.
6.  Configureren van directory-synchronisatie via [Azure AD Connect](whatis-hybrid-identity.md).

Na het instellen van eenmalige aanmelding met uw SAML 2.0 SP-Lite op basis van identiteit Provider, moet u controleren of deze correct werkt.

>[!NOTE]
>Als u een domein, in plaats van toevoegen van een geconverteerd, duurt het tot 24 uur voor het instellen van eenmalige aanmelding.
Voordat u eenmalige aanmelding verifiëren, moet u klaar bent met het instellen van Active Directory-synchronisatie, synchroniseren van uw adreslijsten en uw gesynchroniseerde gebruikers activeren.

### <a name="use-the-tool-to-verify-that-single-sign-on-has-been-set-up-correctly"></a>Het hulpprogramma gebruiken om te controleren dat eenmalige aanmelding is ingesteld juist
Om te controleren dat eenmalige aanmelding is ingesteld correct, kunt u de volgende procedure om te bevestigen dat u zich kunt aanmelden bij de cloudservice met uw bedrijfsreferenties uitvoeren.

Microsoft is een hulpprogramma waarmee u gebruiken kunt voor het testen van uw op basis van SAML 2.0-identiteitsprovider opgegeven. Voordat u het hulpprogramma test uitvoert, moet u een Azure AD-tenant om te federeren met uw id-provider hebt geconfigureerd.

>[!NOTE]
>De Connectivity Analyzer is Internet Explorer 10 of hoger vereist.



1. Download de Connectivity Analyzer uit [ https://testconnectivity.microsoft.com/?tabid=Client ](https://testconnectivity.microsoft.com/?tabid=Client).
2.  Klik op Nu installeren om te beginnen met downloaden en installeren van het hulpprogramma.
3.  Selecteer 'Ik kan geen instellen van Federatie met Office 365, Azure of andere services die gebruikmaken van Azure Active Directory'.
4.  Wanneer het hulpprogramma gedownload en wordt uitgevoerd is, ziet u het venster diagnostische. Het hulpprogramma wordt u stapsgewijs door het testen van de federation-verbinding.
5.  De Connectivity Analyzer wordt geopend uw SAML 2.0 IDP kunt aanmelden, voert u de referenties voor de gebruiker-principal die u wilt testen: ![SAML](./media/how-to-connect-fed-saml-idp/saml1.png)
6.  Op de federatieve aanmelding ruit, moet u een accountnaam en wachtwoord invoeren voor de Azure AD-tenant die is geconfigureerd om te federeren met uw SAML 2.0-id-provider. Het hulpprogramma wordt geprobeerd om aan te melden met behulp van deze referenties en de gedetailleerde resultaten van tests die worden uitgevoerd tijdens de aanmeldingspoging worden geleverd als uitvoer.
![SAML](./media/how-to-connect-fed-saml-idp/saml2.png)
7. Dit venster toont een mislukte resultaten van de tests. Te klikken op controleren gedetailleerde resultaten ziet u informatie over de resultaten voor elke test die werd uitgevoerd. U kunt de resultaten naar de schijf om te kunnen delen ze ook opslaan.
 
>[!NOTE]
>Test de connectiviteit analyzer ook actieve Federatie met behulp van de WS *-gebaseerd en ECP/PAOS protocollen. Als u deze niet gebruikt u de volgende fout kunt negeren: testen van de stroom voor actieve aanmelden met behulp van uw identiteitsprovider actieve Federatie-eindpunt.

### <a name="manually-verify-that-single-sign-on-has-been-set-up-correctly"></a>Handmatig controleren of dat deze eenmalige aanmelding is ingesteld juist
Handmatige verificatie vindt u aanvullende stappen die u nemen kunt om ervoor te zorgen dat uw identiteit SAML 2.0 Provider correct in veel scenario's werkt.
Om te controleren dat eenmalige aanmelding is ingesteld correct, voert u de volgende stappen uit:


1. Op een computer domein aanmelden met uw cloudservice met behulp van dezelfde aanmeldingsnaam die u voor uw bedrijfsreferenties gebruiken.
2.  Klik in het wachtwoord. Als eenmalige aanmelding is ingesteld, het wachtwoord grijs worden weergegeven wordt en u het volgende bericht ziet: ' u nu zijn vereist om aan te melden bij &lt;van uw bedrijf&gt;. "
3.  Klik op de aanmelding bij &lt;van uw bedrijf&gt; koppeling. Als u zich kunt is aanmelden, klikt u vervolgens één aanmelding ingesteld.

## <a name="next-steps"></a>Volgende stappen


- [Active Directory Federation Services-beheer en aanpassingen met Azure AD Connect](how-to-connect-fed-management.md)
- [Compatibiliteitslijst voor Azure AD-federatie](how-to-connect-fed-compatibility.md)
- [Azure AD Connect aangepaste installatie](how-to-connect-install-custom.md)
