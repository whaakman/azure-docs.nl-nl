---
title: Directe Federatie met een id-provider voor B2B - Azure Active Directory instellen | Microsoft Docs
description: Rechtstreeks federeren met een id-provider van SAML of WS-Federation, zodat de gasten kunnen zich aanmelden bij uw Azure AD-apps
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: a4dadc68e78fbaa979751d5bcd04ef481c3ab886
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67544346"
---
# <a name="direct-federation-with-ad-fs-and-third-party-providers-for-guest-users-preview"></a>Directe Federatie met AD FS en providers van derden voor gastgebruikers (preview)
|     |
| --- |
| Direct federation is een openbare preview-functie van Azure Active Directory. Zie [Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.|
|     |

Dit artikel wordt beschreven hoe u directe Federatie met een andere organisatie voor B2B-samenwerking kunt instellen. U kunt direct Federatie met elke organisatie waarvan de id-provider (IdP) het SAML 2.0 of WS-Federation-protocol ondersteunt instellen.
Wanneer u direct Federatie met een partner-id-provider hebt ingesteld, kunnen nieuwe gastgebruikers van dat domein hun eigen id-provider beheerde organisatie-account gebruiken om zich aanmelden bij uw Azure AD-tenant en werk vanaf nu samen met u. Er is niet nodig voor de gastgebruiker te maken van een afzonderlijke Azure AD-account.
> [!NOTE]
> Directe federation gastgebruikers moeten aanmelden met een koppeling met de context van de tenant (bijvoorbeeld `https://myapps.microsoft.com/?tenantid=<tenant id>` of `https://portal.azure.com/<tenant id>`, of in geval van een geverifieerd domein `https://myapps.microsoft.com/\<verified domain>.onmicrosoft.com`). Directe koppelingen naar toepassingen en bronnen ook werken, zolang ze de context van de tenant zijn. Directe federatieve gebruikers zijn momenteel niet aan te melden met behulp van algemene eindpunten waarvoor geen tenant-context. Bijvoorbeeld, met behulp van `https://myapps.microsoft.com`, `https://portal.azure.com`, of `https://teams.microsoft.com` zal leiden tot een fout.
 
## <a name="when-is-a-guest-user-authenticated-with-direct-federation"></a>Wanneer is een gastgebruiker geverifieerd met directe Federatie?
Na het instellen van directe Federatie met een organisatie, wordt u uitnodigen een nieuwe gastgebruikers ook kunnen worden geverifieerd met behulp van directe federation. Het is belangrijk te weten dat het instellen van directe federation de verificatiemethode voor gastgebruikers die een uitnodiging van u al hebt ingewisseld niet wijzigen. Hier volgen enkele voorbeelden:
 - Als gastgebruikers hebben al ingewisseld voor uitnodigingen van u, en u vervolgens direct Federatie met hun organisatie, blijft die gastgebruikers ook kunnen gebruiken dezelfde verificatiemethode die ze gebruikt voordat u direct federation instelt.
 - Als u direct Federatie met een partnerorganisatie instellen en gastgebruikers uitnodigen, en vervolgens later de partnerorganisatie wordt verplaatst naar Azure AD, blijft de gastgebruikers die u hebt al ingewisseld voor uitnodigingen direct Federatie, zo lang de direct gebruiken Federation-beleid in uw tenant bestaat.
 - Als u direct Federatie met een partnerorganisatie verwijdert, worden alle gastgebruikers ook kunnen op dit moment met behulp van directe Federatie kan niet aanmelden.

In een van deze scenario's, kunt u een gastgebruiker verificatiemethode bijwerken door te verwijderen van het gastgebruikersaccount van uw directory en reinviting ze.

Directe federation is gekoppeld aan een domeinnaamruimten, zoals contoso.com en fabrikam.com. Bij het maken van een directe federation-configuratie met AD FS of een IdP van derden koppelen organisaties een of meer domain-naamruimten naar deze id-providers. 

## <a name="end-user-experience"></a>Eindgebruikerservaring 
Met direct federation gastgebruikers Meld u aan bij uw Azure AD-tenant met behulp van hun eigen organisatie-account. Wanneer ze toegang hebben tot gedeelde bronnen en wordt gevraagd voor aanmelden, worden direct federatieve gebruikers worden omgeleid naar de id-provider. Na geslaagde aanmelding, worden ze teruggestuurd naar Azure AD voor toegang tot resources. Directe vernieuwingstokens federatieve gebruikers zijn geldig gedurende 12 uur, de [lengte voor vernieuwingstoken passthrough standaard](../develop/active-directory-configurable-token-lifetimes.md#exceptions) in Azure AD. Als de IdP federatieve eenmalige aanmelding ingeschakeld is, wordt de gebruiker wordt SSO-ervaring en niet zichtbaar voor elke prompt aanmelden na de eerste verificatie.

## <a name="limitations"></a>Beperkingen

### <a name="dns-verified-domains-in-azure-ad"></a>DNS-gecontroleerde domeinen in Azure AD
Directe federation is alleen toegestaan voor domeinen die zijn ***niet*** DNS in Azure AD is geverifieerd. Directe federation is toegestaan voor niet-beheerde (via e-mail geverifieerde of 'viraal') Azure AD-tenants omdat ze niet DNS is geverifieerd.
### <a name="authentication-url"></a>Verificatie-URL
Directe federation is alleen toegestaan voor beleid waarbij de verificatie-URL-domein komt overeen met het doeldomein of waarbij de verificatie-URL is een van deze toegestane id-providers (deze lijst is onderhevig aan wijzigingen):
-   accounts.google.com
-   pingidentity.com
-   login.pingone.com
-   okta.com
-   my.salesforce.com
-   Federation.exostar.com
-   Federation.exostartest.com

Bijvoorbeeld, bij het instellen van directe Federatie voor **fabrikam.com**, de verificatie-URL `https://fabrikam.com/adfs` wordt gevalideerd. Een host in hetzelfde domein wordt ook doorgeven, bijvoorbeeld `https://sts.fabrikam.com/adfs`. Echter, de verificatie-URL `https://fabrikamconglomerate.com/adfs` of `https://fabrikam.com.uk/adfs` voor hetzelfde domein wordt niet doorgeven.

### <a name="signing-certificate-renewal"></a>Ondertekening van certificaat vernieuwen
Als u de metagegevens-URL in de instellingen van de id-provider opgeven, wordt Azure AD het certificaat voor ondertekening automatisch verlengd wanneer deze is verlopen. Echter, als het certificaat voor een bepaalde reden wordt gedraaid voordat u de verlooptijd, of als u een URL-metagegevens niet opgeeft, Azure AD geen worden om het te vernieuwen. In dit geval moet u het certificaat voor ondertekening handmatig bijwerken.
## <a name="frequently-asked-questions"></a>Veelgestelde vragen
### <a name="can-i-set-up-direct-federation-with-an-unmanaged-email-verified-tenant"></a>Kan ik direct Federatie met een niet-beheerde tenant voor (via e-mail geverifieerde) instellen? 
Ja. Als het domein nog niet is geverifieerd en de tenant nog niet ondergaan een [beheerdersovername](../users-groups-roles/domains-admin-takeover.md), kunt u direct federation instellen. Niet-beheerde of via e-mail geverifieerde tenants worden gemaakt wanneer een gebruiker een uitnodiging voor een B2B wordt ingewisseld of voert een self-service-aanmelding voor Azure AD met behulp van een domein dat momenteel niet bestaat. U kunt direct Federatie met deze domeinen instellen. Als u probeert om in te stellen direct Federatie met een DNS-gecontroleerd domein, in Azure portal of via PowerShell, ziet u een fout.
### <a name="if-direct-federation-and-email-one-time-passcode-authentication-are-both-enabled-which-method-takes-precedence"></a>Als directe Federatie en e-mailverificatie voor eenmalige wachtwoordcode beide zijn ingeschakeld, is welke methode u voorrang?
Als directe Federatie met een partnerorganisatie tot stand is gebracht, heeft deze voorrang op e-mailverificatie eenmalige wachtwoordcode voor nieuwe gastgebruikers ook kunnen van deze organisatie. Als een gastgebruiker ingewisseld een uitnodiging met behulp van verificatie eenmalige wachtwoordcode voordat u direct federation instelt, blijven deze gewoon gebruik van verificatie eenmalige wachtwoordcode. 
### <a name="does-direct-federation-address-sign-in-issues-due-to-a-partially-synced-tenancy"></a>Federatieve aanmelding problemen direct vanwege een gedeeltelijk gesynchroniseerde tenants?
Nee, de [e-eenmalige wachtwoordcode](one-time-passcode.md) functie moet worden gebruikt in dit scenario. Een 'gedeeltelijk gesynchroniseerde tenants"verwijst naar een partner Azure AD-tenant waar volledig on-premises gebruikers-id's zijn niet gesynchroniseerd naar de cloud. Een gast waarvan de identiteit nog niet bestaat in de cloud, maar die uw B2B-uitnodiging inwisselen probeert zich niet aanmelden. De functie voor eenmalige wachtwoordcode wordt bevraagd deze Gast aan te melden. De functie voor direct federation bedoeld voor scenario's waarbij de Gast hun eigen id-provider beheerde organisatieaccount heeft, maar de organisatie heeft geen Azure AD-aanwezigheid helemaal.

## <a name="step-1-configure-the-partner-organizations-identity-provider"></a>Stap 1: Configureren van de partnerorganisatie-id-provider
Uw partnerorganisatie moet eerst de id-provider configureren met de vereiste claims en vertrouwensrelaties met relying party's. 

> [!NOTE]
> Ter illustratie van een id-provider voor directe federatie configureren, gebruiken we Active Directory Federation Services (AD FS) als voorbeeld. Zie het artikel [direct Federatie met AD FS configureren](direct-federation-adfs.md), waardoor meer voorbeelden van hoe AD FS configureren als een SAML 2.0 of WS-Federation id-provider in voorbereiding voor directe Federatie.

### <a name="saml-20-configuration"></a>Configuratie van SAML 2.0

Azure AD B2B kan worden geconfigureerd om te federeren met id-providers die gebruikmaken van het SAML-protocol met specifieke vereisten die hieronder worden vermeld. Zie voor meer informatie over het instellen van een vertrouwensrelatie tussen de SAML-identiteitsprovider en Azure AD [SAML 2.0-identiteitsprovider (IdP) gebruiken voor eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-fed-saml-idp).  

> [!NOTE]
> Opmerking het doeldomein voor directe Federatie mag geen DNS geverifieerd in Azure AD. De verificatie-URL-domein moet overeenkomen met het doeldomein of u moet het domein van een toegestane id-provider. Zie de [beperkingen](#limitations) sectie voor meer informatie. 

#### <a name="required-saml-20-attributes-and-claims"></a>Vereiste SAML 2.0-kenmerken als claims
De volgende tabellen tonen de vereisten voor specifieke kenmerken en claims die moeten worden geconfigureerd op de externe id-provider. Als u direct federation instelt, moeten de volgende kenmerken worden ontvangen in het SAML 2.0-antwoord van de id-provider. Deze kenmerken kunnen worden geconfigureerd door een koppeling naar de online service voor beveiligingstokens XML-bestand of door ze handmatig invoeren.

Vereiste kenmerken voor het SAML 2.0-antwoord van de id-provider:

|Kenmerk  |Value  |
|---------|---------|
|AssertionConsumerService     |`https://login.microsoftonline.com/login.srf`         |
|Doelgroep     |`urn:federation:MicrosoftOnline`         |
|Verlener     |De verlener URI van de partner-id-provider, bijvoorbeeld `http://www.example.com/exk10l6w90DHM0yi...`         |


Vereiste claims voor het SAML 2.0-token dat is uitgegeven door de id-provider:

|Kenmerk  |Value  |
|---------|---------|
|NameID-indeling     |`urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`         |
|emailaddress     |`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`         |

### <a name="ws-fed-configuration"></a>WS-Federation-configuratie 
Azure AD B2B kan worden geconfigureerd om te federeren met id-providers die gebruikmaken van het protocol WS-Federation met enkele specifieke vereisten, zoals hieronder vermeld. De twee WS-Federation-providers zijn op dit moment getest voor compatibiliteit met Azure AD zijn AD FS en Shibboleth. Voor meer informatie over het maken van een relying party-vertrouwensrelatie tussen een compatibele WS-Federation-provider met Azure AD raadpleegt u het 'STS Integration document met behulp van WS-protocollen"beschikbaar in de [Azure AD Identity Provider compatibiliteit Docs](https://www.microsoft.com/download/details.aspx?id=56843).

> [!NOTE]
> Het doeldomein voor directe Federatie mag geen DNS-geverifieerd in Azure AD. De verificatie-URL-domein moet overeenkomen met het doeldomein of het domein van een toegestane id-provider. Zie de [beperkingen](#limitations) sectie voor meer informatie. 

#### <a name="required-ws-fed-attributes-and-claims"></a>Vereiste WS-Federation-kenmerken als claims

De volgende tabellen tonen de vereisten voor specifieke kenmerken en claims die moeten worden geconfigureerd op de derde partij WS-Federation-id-provider. Als u direct federation instelt, moeten de volgende kenmerken worden ontvangen in de WS-Federation-bericht van de id-provider. Deze kenmerken kunnen worden geconfigureerd door een koppeling naar de online service voor beveiligingstokens XML-bestand of door ze handmatig invoeren.

Vereiste kenmerken in de WS-Federation-bericht van de id-provider:
 
|Kenmerk  |Value  |
|---------|---------|
|PassiveRequestorEndpoint     |`https://login.microsoftonline.com/login.srf`         |
|Doelgroep     |`urn:federation:MicrosoftOnline`         |
|Verlener     |De verlener URI van de partner-id-provider, bijvoorbeeld `http://www.example.com/exk10l6w90DHM0yi...`         |

Vereiste claims voor de WS-Federation-token dat is uitgegeven door de id-provider:

|Kenmerk  |Value  |
|---------|---------|
|ImmutableID     |`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`         |
|emailaddress     |`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`         |

## <a name="step-2-configure-direct-federation-in-azure-ad"></a>Stap 2: Directe federatie configureren in Azure AD 
Vervolgens configureert u Federatie met de id-provider in stap 1 hebt geconfigureerd in Azure AD. U kunt de Azure AD-portal of PowerShell. Het duurt 5-10 minuten voordat het beleid direct federation wordt van kracht. Gedurende deze tijd niet proberen om in te wisselen een uitnodiging voor de directe federation-domein. De volgende kenmerken zijn vereist:
- Uitgever van de URI van de partner-id-provider
- Passieve verificatie-eindpunt van de partner-id-provider (alleen https wordt ondersteund)
- Certificaat

### <a name="to-configure-direct-federation-in-the-azure-ad-portal"></a>Directe om Federatie te configureren in de Azure AD-portal

1. Ga naar de [Azure Portal](https://portal.azure.com/). Selecteer de knop **Azure Active Directory** in het linkerdeelvenster. 
2. Selecteer **organisatie relaties**.
3. Selecteer **id-providers**, en selecteer vervolgens **nieuwe SAML/WS-Federation IdP**.

    ![Schermafbeelding met de knop voor het toevoegen van een nieuwe SAML of WS-Federation-id-provider](media/direct-federation/new-saml-wsfed-idp.png)

4. Op de **nieuwe SAML/WS-Federation IdP** pagina onder **Identity provider protocol**, selecteer **SAML** of **WS-FEDERATION**.

    ![Schermafbeelding met parseren van de knop op de pagina SAML of WS-Federation-id-provider](media/direct-federation/new-saml-wsfed-idp-parse.png)

5. Geef de domeinnaam van uw partnerorganisatie, is de naam van het doel-domein voor directe Federatie
6. U kunt een bestand met metagegevens om in te vullen metagegevens details uploaden. Als u kiest voor het invoeren van metagegevens handmatig, voert u de volgende informatie:
   - Domeinnaam van de partner-id-provider
   - Entiteit-ID van de partner-id-provider
   - Eindpunt van de passieve aanvrager van partner-id-provider
   - Certificaat
   > [!NOTE]
   > Metagegevens-URL is optioneel, maar het wordt aangeraden deze. Als u de metagegevens-URL opgeeft, kan Azure AD het certificaat voor ondertekening automatisch verlengd wanneer deze is verlopen. Als het certificaat voor een bepaalde reden wordt gedraaid voordat u de verlooptijd of als u een URL-metagegevens niet opgeeft, wordt Azure AD worden om het te vernieuwen. In dit geval moet u het certificaat voor ondertekening handmatig bijwerken.

7. Selecteer **Opslaan**. 

### <a name="to-configure-direct-federation-in-azure-ad-using-powershell"></a>Directe om Federatie te configureren in Azure AD met behulp van PowerShell

1. Installeer de nieuwste versie van de Azure AD PowerShell voor Graph-module ([AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview)). (Als u gedetailleerde stappen nodig, de Snelstartgids voor het toevoegen van een gastgebruiker bevat de sectie [Installeer de meest recente AzureADPreview-module](b2b-quickstart-invite-powershell.md#install-the-latest-azureadpreview-module).) 
2. Voer de volgende opdracht uit: 
   ```powershell
   Connect-AzureAD
   ```
1. Aanmelden bij de prompt aanmelden met het beheerde account voor globale beheerders. 
2. Voer de volgende opdrachten, vervang de waarden van het bestand met federatieve metagegevens. Voor AD FS-Server en Okta, is het bestand federation federationmetadata.xml, bijvoorbeeld: `https://sts.totheclouddemo.com/federationmetadata/2007-06/federationmetadata.xml`. 

   ```powershell
   $federationSettings = New-Object Microsoft.Open.AzureAD.Model.DomainFederationSettings
   $federationSettings.PassiveLogOnUri ="https://sts.totheclouddemo.com/adfs/ls/"
   $federationSettings.LogOffUri = $federationSettings.PassiveLogOnUri
   $federationSettings.IssuerUri = "http://sts.totheclouddemo.com/adfs/services/trust"
   $federationSettings.MetadataExchangeUri="https://sts.totheclouddemo.com/adfs/services/trust/mex"
   $federationSettings.SigningCertificate= <Replace with X509 signing cert’s public key>
   $federationSettings.PreferredAuthenticationProtocol="WsFed" OR "Samlp"
   $domainName = <Replace with domain name>
   New-AzureADExternalDomainFederation -ExternalDomainName $domainName  -FederationSettings $federationSettings
   ```

## <a name="step-3-test-direct-federation-in-azure-ad"></a>Stap 3: Directe federation testen in Azure AD
Nu uw direct federatie-instellingen testen door een nieuwe gebruiker van de B2B-gasten uitnodigen. Zie voor meer informatie, [gebruikers toevoegen Azure AD B2B-samenwerking in Azure portal](add-users-administrator.md).
 
## <a name="how-do-i-edit-a-direct-federation-relationship"></a>Hoe kan ik een directe federatieverbinding bewerken?

1. Ga naar de [Azure Portal](https://portal.azure.com/). Selecteer de knop **Azure Active Directory** in het linkerdeelvenster. 
2. Selecteer **organisatie relaties**.
3. Selecteer **id-providers**
4. Onder **SAML/WS-Federation-id-providers**, selecteert u de provider.
5. Werk de waarden in het detailvenster van id-provider.
6. Selecteer **Opslaan**.


## <a name="how-do-i-remove-direct-federation"></a>Hoe verwijder ik direct federation?
U kunt de directe federation-configuratie verwijderen. Als u dit doet, zich direct federation-gastgebruikers die hun uitnodiging al hebt ingewisseld niet aanmelden. Maar u kunt zodat ze toegang tot uw resources opnieuw door deze worden verwijderd uit de map en reinviting ze. Directe Federatie met een id-provider verwijderen in de Azure AD-portal:

1. Ga naar de [Azure Portal](https://portal.azure.com/). Selecteer de knop **Azure Active Directory** in het linkerdeelvenster. 
2. Selecteer **organisatie relaties**.
3. Selecteer **id-providers**.
4. Selecteer de id-provider, en selecteer vervolgens **verwijderen**. 
5. Selecteer **Ja** om verwijdering te bevestigen. 

Directe Federatie met een id-provider verwijderen met behulp van PowerShell:
1. Installeer de nieuwste versie van de Azure AD PowerShell voor Graph-module ([AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview)).
2. Voer de volgende opdracht uit: 
   ```powershell
   Connect-AzureAD
   ```
3. Aanmelden bij de prompt aanmelden met het beheerde account voor globale beheerders. 
4. Voer de volgende opdracht in:
   ```powershell
   Remove-AzureADExternalDomainFederation -ExternalDomainName  $domainName
   ```
