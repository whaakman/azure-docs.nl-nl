---
title: On-premises AD FS-apps naar Azure migreren. | Microsoft Docs
description: Dit document is bedoeld ter ondersteuning van organisaties bij het migreren van on-premises toepassingen naar Azure AD, waarbij de focus op federatieve SaaS-toepassingen ligt.
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/02/2018
ms.author: billmath
ms.openlocfilehash: f1256e2fefe1ad3d37403f37804888ab9dcf6e99
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="migrate-ad-fs-on-premises-apps-to-azure"></a>On-premises AD FS-apps naar Azure migreren 

Dit document is bedoeld ter ondersteuning van organisaties bij het migreren van on-premises toepassingen naar Azure AD.  De focus ligt hierbij op federatieve SaaS-toepassingen.  In dit document wordt geen stapsgewijze begeleiding gegeven.  Het biedt wel theoretische richtlijnen voor de migratie door de vertaling van on-premises configuraties naar Azure AD inzichtelijk te maken. Het bevat ook informatie die in de meestvoorkomende scenario's is vereist.

## <a name="introduction"></a>Inleiding

Als u een on-premises map met gebruikersaccounts hebt, dan is de kans groot dat u minimaal een of twee apps hebt.  En deze apps worden geconfigureerd voor gebruikers zodat ze ze kunnen openen. Dat doen ze door zich aan te melden met die identiteiten.

En net als vele andere organisatie bent u waarschijnlijk al bezig over te stappen op cloudtoepassingen en -identiteiten.  Mogelijk maakt u al gebruik van Office 365 en Azure AD Connect.  En hebt u SaaS-cloudtoepassingen ingesteld voor enkele belangrijke werkbelastingen, maar nog niet voor alle.  

Veel organisaties beschikken over SaaS- of line-of-business (LoB)-apps die rechtstreeks zijn gefedereerd met een on-premises aanmeldingsservice, zoals Active Directory Federation Service (AD FS), in combinatie met Office 365- en Azure AD-apps.  In deze migratiehandleiding wordt beschreven waarom en hoe u on-premises toepassingen naar Azure AD migreert.

>[!NOTE]
>De handleiding biedt uitgebreide informatie over de configuratie en migratie van SaaS-apps, met belangrijke informatie over aangepaste LoB-apps.  Meer gedetailleerde richtlijnen voor LoB-apps staat voor later gepland.

Afbeelding 1: Apps die rechtstreeks on-premises zijn verbonden ![on-premises](media/migrate-adfs-apps-to-azure/migrate1.png)

Afbeelding 2: Apps die via Azure AD zijn gefedereerd ![Azure](media/migrate-adfs-apps-to-azure/migrate2.png)

## <a name="why-migrate-apps-to-azure-ad"></a>Waarom apps naar Azure AD migreren?

Voor organisaties die al gebruikmaken van AD FS, Ping of een andere on-premises verificatieprovider, biedt het migreren van apps naar Azure AD de volgende voordelen:

**Veiliger toegang**
- Gedetailleerde besturingselementen voor toegang per toepassing, waaronder Multi-Factor Authentication (MFA), met behulp van [Voorwaardelijke toegang voor Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).  Het beleid kan worden toegepast op SaaS- en aangepaste apps en wel op dezelfde manier als u voor Office 365 zou doen
- Voor de detectie van bedreigingen en het beveiligen van de aanmelding op basis van machine learning en heuristieken waarmee risicovol verkeer wordt geïdentificeerd, kunt u met [Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) profiteren van de ingebouwde mogelijkheden van Azure AD (die continu ontwikkeld worden)

**B2B-samenwerking van Azure AD**
- Zodra het aanmelden bij SaaS-apps op Azure AD is gebaseerd, kunt u uw partners toegang geven aan cloudresources met [B2B-samenwerking van Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).

**Eenvoudiger beheer met en aanvullende mogelijkheden van Azure AD**
- Azure AD als id-provider voor SaaS-apps biedt ondersteuning voor aanvullende mogelijkheden, zoals certificaten voor token-ondertekening per toepassing, [configureerbare vervaldatums van certificaten](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs) en [geautomatiseerd inrichten](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning) van gebruikersaccounts (in belangrijke galerie-apps) op basis van Azure AD-identiteiten

**Behoud de voordelen van een on-premises id-provider**
- Ook met de extra voordelen van Azure AD kunt u uw on-premises oplossing voor verificatie behouden, zodat de voordelen van on-premises oplossingen als Multi-Factor Authentication (MFA), logboekregistratie en controles ongewijzigd blijven 

**Sneller buiten gebruik stellen van de on-premises id-provider**
- Organisaties die het on-premises verificatieproduct buiten bedrijf willen stellen, kunnen dankzij het migreren van apps naar Azure AD hun overgang vlotter laten verlopen door een deel van het werk over te dragen 

## <a name="mapping-types-of-apps-on-premises-to-types-of-apps-in-azure-ad"></a>App-typen on-premises toewijzen aan app-typen in Azure AD
De meeste apps kunnen in enkele categorieën worden ondergebracht op basis van het type aanmelding dat ze gebruiken.  Deze categorieën bepalen hoe de app in Azure AD wordt gerepresenteerd.

Kortweg geldt dat SAML 2.0-toepassingen kunnen worden geïntegreerd met Azure AD via de Azure AD-toepassingsgalerie of als niet-galerietoepassingen.  Apps die gebruikmaken van OAuth 2.0 of OpenID Connect, kunnen net zoals 'app-registraties' worden geïntegreerd met Azure AD.  Lees verder voor meer informatie.

### <a name="federated-saas-apps-vs-custom-lob-apps"></a>Federatieve SaaS-apps versus aangepaste LoB-apps
Federatieve apps omvatten apps die in de volgende categorieën voorkomen.

- SaaS-apps 
    - Als uw gebruikers zich aanmelden bij SaaS-apps als Salesforce, ServiceNow of Workday en u een integratie uitvoert met een on-premises id-provider als AD FS of Ping, maakt u gebruik van federatieve aanmelding voor SaaS-apps.
    - Over het algemeen maken apps gebruik van het SAML 2.0-protocol voor federatieve aanmelding.
    - Toepassingen in deze categorie kunnen worden geïntegreerd met Azure AD als bedrijfstoepassingen, ofwel vanuit de galerie, ofwel als niet-galerietoepassingen.
- Aangepaste LoB-toepassingen
    - Dit verwijst naar niet-SaaS-apps, die intern zijn ontwikkeld door uw organisatie of beschikbaar zijn als een standaard verpakt product dat is geïnstalleerd in uw datacentrum.  Dit omvat SharePoint-apps en apps die zijn gebouwd onder Windows Identity Foundation (WIF).
    - Apps kunnen gebruikmaken van SAML 2.0, WS-Federation, OAuth of OpenID Connect voor federatieve aanmelding
    - Aangepaste apps die gebruikmaken van Oauth 2.0, OpenID Connect of WS-Federation kunnen worden geïntegreerd met Azure AD als app-registraties; aangepaste apps die gebruikmaken van SAML 2.0 of WS-Federation kunnen worden geïntegreerd als niet-galerietoepassingen binnen bedrijfstoepassingen

### <a name="non-federated-apps"></a>Niet-federatieve apps
Daarnaast kunnen niet-federatieve apps worden geïntegreerd met Azure AD met behulp van de toepassingsproxy van Azure AD en aanverwante mogelijkheden.  Volg de koppelingen voor meer informatie over deze mogelijkheden:
- Apps die rechtstreeks gebruikmaken van Windows Integrated Auth (WIA) naar Active Directory
    - Deze apps kunnen worden geïntegreerd met Azure AD via de [toepassingsproxy van Azure AD](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal)
- Apps die via een agent integreren met uw provider voor eenmalige aanmelding en headers gebruiken voor verificatie
    - On-premises apps die bij aanmelden en autorisatie op basis van headers gebruikmaken van een geïnstalleerde agent, kunnen worden geconfigureerd voor Azure AD-aanmelding met behulp van de toepassingsproxy van Azure AD met [Ping-toegang voor Azure AD](https://blogs.technet.microsoft.com/enterprisemobility/2017/06/15/ping-access-for-azure-ad-is-now-generally-available-ga/)

## <a name="translating-on-premises-federated-apps-to-azure-ad"></a>Federatieve, on-premises apps vertalen naar Azure AD 
Gelukkig werken AD FS en Azure AD op soortgelijke wijze, dus de concepten betreffende het configureren van vertrouwensrelaties, URL's voor aan- en afmelden, en id's zijn op beide producten van toepassing.  Er moeten echter nog enkele kleine verschillen worden uitgelegd voordat u de overgang uit gaat voeren.

In de tabel staan enkele belangrijke ideeën vermeld die gelden voor zowel AD FS, Azure AD als SaaS-apps en waarmee u de vertaalslag beter kunt uitvoeren. 

### <a name="representing-the-app-in-azure-ad-or-ad-fs"></a>De app representeren in Azure AD of AD FS
Voor de migratie begint, moet eerst worden vastgesteld hoe de app on-premises moet worden geconfigureerd en hoe de configuratie aan Azure AD wordt toegewezen.  Wat volgt is een toewijzing van configuratie-elementen van de Relying Party van AD FS aan de overeenkomstige elementen in Azure AD.  
- AD FS-term: Relying Party of vertrouwensrelatie van Relying Party
- Azure AD-term: bedrijfstoepassing of app-registratie (afhankelijk van het type app)

|Configuratie-element van app|Beschrijving|Binnen AD FS-configuratie|Overeenkomstige locatie in Azure AD-configuratie|SAML-tokenelement|
|-----|-----|-----|-----|-----|
|Aanmeldings-URL van app|URL van de aanmeldingspagina van deze toepassing. Hier initieert de gebruiker een aanmelding in de app in een door SP geïnitieerde SAML-stroom.|N.v.t.|In Azure AD wordt binnen Azure Portal de aanmeldings-URL als de aanmeldings-URL geconfigureerd in de eigenschappen voor eenmalige aanmelding van de toepassing.</br></br>(Mogelijk moet u op Geavanceerde URL-instellingen weergeven klikken om de aanmeldings-URL te zien.)||
|Antwoord-URL van app|URL van de app vanuit het perspectief van de id-provider.  Hierheen worden de gebruiker en het token verzonden zodra de gebruiker zich bij de id-provider heeft aangemeld.</br></br>  Dit wordt ook wel het consumenteneindpunt van SAML-bewerking genoemd.|Deze wordt gevonden in de vertrouwensrelatie van de Relying Party van AD FS voor de app.  Klik met de rechtermuisknop op de Relying Party en kies Eigenschappen > tabblad Eindpunten.|In Azure AD wordt binnen Azure Portal de antwoord-URL als de antwoord-URL geconfigureerd in de eigenschappen voor eenmalige aanmelding van de toepassing.</br></br>(Mogelijk moet u op Geavanceerde URL-instellingen weergeven klikken om de antwoord-URL te zien.)|Wordt toegewezen aan het doelelement in het SAML-token.</br></br>  Voorbeeldwaarde: https://contoso.my.salesforce.com|
|Afmeldings-URL van app|URL waarheen aanvragen voor 'sign out cleanup' worden verzonden als een gebruiker zich bij een app afmeldt, waarna wordt afgemeld bij alle overige apps waar de id-provider de gebruiker heeft aangemeld.|Aangetroffen in AD FS-beheer onder: vertrouwensrelaties van Relying Party.  Klik met de rechtermuisknop op de Relying Party en klik op Eigenschappen > tabblad Eindpunten|N.v.t.: Azure AD biedt geen ondersteuning voor eenmalig afmelden, dat wil zeggen afmelden bij alle apps.  De gebruik wordt gewoon afgemeld bij Azure AD zelf.|N.v.t.|
|App-id|Id van de app vanuit het oogpunt van de id-provider. De waarde van de aanmeldings-URL wordt vaak (maar niet altijd) voor de id gebruikt</br></br>  In de app wordt deze id soms de entiteit-id genoemd.|In AD FS is dit de Relying Party-id: klik met de rechtermuisknop op de vertrouwensrelatie van de Relying Party en klik op Eigenschappen > tabblad Id's|In Azure AD wordt de id in Azure Portal in de eigenschappen voor eenmalige aanmelding van de toepassing als id onder Domein en URL's geconfigureerd (mogelijk moet u het selectievakje Geavanceerde URL-instellingen weergeven inschakelen)|Komt overeen met het element Doelgroep in het SAML-token|
|Federatieve metagegevens van app|Locatie van de federatieve metagegevens van de app.  Gebruikt door de id-provider om bepaalde configuratie-instellingen automatisch bij te werken, zoals eindpunten of versleutelingscertificaten.|De URL voor de federatieve metagegevens van de app wordt gevonden in de vertrouwensrelatie van de Relying Party van AD FS voor de app.  Klik met de rechtermuisknop op de vertrouwensrelatie en klik op Eigenschappen > tabblad Controle.|N.v.t.: Azure biedt geen rechtstreekse ondersteuning voor het gebruik van federatieve metagegevens van de app|N.v.t.|
|Gebruikers-id/NameID|Kenmerk dat wordt gebruikt om voor de app op unieke wijze de gebruikers-id aan te geven vanuit Azure AD of AD FS.</br></br>  Dit is gewoonlijk de user principal name of het e-mailadres van de gebruiker.|In AD FS wordt deze gevonden als een claimregel op de Relying Party.  In de meeste gevallen geeft de claimregel een claim uit met een type dat eindigt op 'nameidentifier'|In Azure AD kan de gebruikers-id worden gevonden in Azure Portal in de eigenschappen voor eenmalige aanmelding van de toepassing, onder de kop Gebruikerskenmerken.</br></br>Standaard wordt de user principal name gebruikt.|Wordt vanaf de id-provider naar de app gecommuniceerd als het element NameID in het SAML-token.|
|Andere claims die naar de app worden verzonden|Naast de gebruikers-id/NameID worden er gewoonlijk andere claimgegevens vanaf de id-provider naar de app verzonden, bijvoorbeeld voornaam, achternaam, e-mailadres en groepen waarvan de gebruiker lid is|In AD FS wordt deze gevonden als overige claimregels op de Relying Party.|In Azure AD wordt deze gevonden in Azure Portal in de eigenschappen voor eenmalige aanmelding van de toepassing, onder de kop Gebruikerskenmerken; klik op Weergeven en bewerk alle andere gebruikerskenmerken.|| 

### <a name="representing-azure-ad-as-an-identity-provider-idp-in-a-saas-app"></a>Azure AD representeren als een id-provider in een SaaS-app
Als onderdeel van de migratie dient de app te worden geconfigureerd zodat deze wijst naar Azure AD (versus de on-premises id-provider).  Deze sectie richt zich hoofdzakelijk op SaaS-apps die het SAML-protocol gebruiken, en niet op aangepaste of LoB-apps. De concepten die hier worden beschreven gelden ook voor aangepaste of LoB-apps. 

Op hoog niveau zijn er enkele belangrijke dingen waarom een SaaS-app naar Azure AD moet wijzen
- Identity Provider Issuer: https&#58;//sts.windows.net/{tenant-id}/
- Aanmeldings-URL van de id-provider: https&#58;//login.microsoftonline.com/{tenant-id}/saml2
- Afmeldings-URL van de id-provider: https&#58;//login.microsoftonline.com/{tenant-id}/saml2 
- Locatie federatieve metagegevens: https&#58;//login.windows.net/{tenant-id} <tenant-id>/federationmetadata/2007-06/federationmetadata.xml?appid={<application-id} 

waarin {tenant-id} wordt vervangen door uw tenant-id, aangetroffen als Map-id in Azure Portal onder Azure Active Directory > Eigenschappen, en {application-id} wordt vervangen door de id van de toepassing die als Toepassing-id wordt gevonden in Eigenschappen van de toepassing

In de tabel worden in meer detail de belangrijkste configuratie-elementen van de id-provider beschreven voor het configureren van SSO-instellingen in de app, alsmede de waarden of locaties ervan binnen AD FS en Azure AD.  Het referentiekader van de tabel is de SaaS-app. Deze moet weten hoe verificatieaanvragen moeten worden verzonden en hoe de ontvangen tokens moeten worden gevalideerd.

|Configuratie-element|Beschrijving|AD FS|Azure AD|
|---|---|---|---|
|IdP </br>aanmelden </br>URL|Aanmeldings-URL van de id-provider vanuit het oogpunt van de app (waarnaar de gebruiker wordt omgeleid om zich aan te melden).|De aanmeldings-URL van AD FS is de federatieve AD FS-servicenaam, gevolgd door '/adfs/ls/', bijvoorbeeld: https&#58;//fs.contoso.com/adfs/ls/|De overeenkomstige waarde voor Azure AD volgt dit patroon, waarin {tenant-id} wordt vervangen door uw tenant-id, aangetroffen als Map-id in Azure Portal onder Azure Active Directory > Eigenschappen.</br></br>Voor apps die gebruikmaken van het SAML-P-protocol: https&#58;//login.microsoftonline.com</br>/{tenant-id}/saml2 </br></br>Voor apps die gebruikmaken van het WS-Federation-protocol: https&#58;//login.microsoftonline.com</br>/{tenant-id}/wsfed|
|IdP </br>Afmelden </br>URL|Afmeldings-URL van de id-provider vanuit het oogpunt van de app (waarnaar de gebruiker wordt omgeleid als deze zich wil 'afmelden' bij de app).|Voor AD FS is de afmeldings-URL dezelfde URL als de aanmeldings-URL of dezelfde URL waaraan 'wa=wsignout1.0' is toegevoegd, bijvoorbeeld https&#58;//fs.contoso.com/adfs/ls /?wa=wsignout1.0|De overeenkomstige waarde voor Azure AD is afhankelijk van het al dan niet ondersteunen van SAML 2.0-afmelding door de app.</br></br>Als de app SAML-afmelding ondersteunt, volgt de waarde het patroon, waarbij de waarde voor {tenant-id} wordt vervangen door de tenant-id, aangetroffen als Map-id in Azure Portal onder Azure Active Directory > Eigenschappen. https&#58;//login.microsoftonline.com</br>/{tenant-id}/saml2</br></br>Als de app geen SAML-afmelding ondersteunt: https&#58;//login.microsoftonline.com</br>/common /wsfederation?wa=wsignout1.0|
|Token </br>Ondertekenen </br>Certificaat|Certificaat waarvan de persoonlijke sleutel door de id-provider wordt gebruikt om uitgegeven tokens te ondertekenen.  Verifieert of het token afkomstig is van dezelfde id-provider (de app is geconfigureerd om deze te vertrouwen).|Het AD FS-certificaat voor token-ondertekening wordt gevonden in AD FS-beheer onder Certificaten.|In Azure AD kan het certificaat voor token-ondertekening worden gevonden in Azure Portal in de eigenschappen voor eenmalige aanmelding van de toepassing, onder de kop SAML-handtekeningcertificaat, waar u het certificaat kunt downloaden en vervolgens naar de app kunt uploaden.</br></br>  Als de toepassing meer dan één certificaat bevat, kunnen alle certificaten worden gevonden in het XML-bestand met federatieve metagegevens.|
|Id/ </br>'verlener'|Verlener van de id-provider vanuit het oogpunt van de app (ook wel Verlener of Verlener-id genoemd)</br></br>In het SAML-token wordt de waarde weergegeven als het element Verlener|De id voor AD FS is gewoonlijk de federatieve AD FS-service-id in AD FS-beheer onder Service > Eigenschappen van de Federation Service bewerken.  Bijvoorbeeld: http&#58;//fs.contoso.com/adfs/services/trust|De overeenkomstige waarde voor Azure AD volgt dit patroon, waarin de waarde voor {tenant-id} wordt vervangen door de tenant-id, aangetroffen als Map-id in Azure Portal onder Azure Active Directory > Eigenschappen.  https&#58;//sts.windows.net/{tenant-id}/|
|IdP </br>Federatie </br>Metagegevens|Locatie van de openbaar beschikbare federatieve metagegevens van de id-provider.  (Federatieve metagegevens worden door sommige apps gebruikt als een alternatief voor het individueel configureren van URL's (in plaats van door de beheerder), de id en het certificaat voor token-ondertekening)|U vindt de URL voor de federatieve AD FS-metagegevens in AD FS-beheer onder Service > Eindpunten > Metagegevens > Type: Federatieve metagegevens, bijvoorbeeld: https&#58;//fs.contoso.com/ FederationMetadata/2007-06/</br>FederationMetadata.xml|De overeenkomstige waarde voor Azure AD volgt het patroon https&#58;//login.microsoftonline.com</br>/{TenantDomainName}/FederationMetadata/2007-06/</br>FederationMetadata.xml waarbij de waarde voor {TenantDomainName} door de naam van uw tenant in de indeling contoso.onmicrosoft.com </br></br>[Meer informatie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-federation-metadata) over federatieve metagegevens in Azure AD.

## <a name="migrating-saas-apps"></a>SaaS-apps migreren
Het migreren van SaaS-apps vanuit AD FS of een andere id-provider naar Azure AD is tegenwoordig een handmatig proces. Zie de lijst met zelfstudies over [Integratie van de SaaS-toepassingen met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) voor richtlijnen voor apps.

In de zelfstudies over integratie wordt aangenomen dat u een integratie uitvoert zonder voorafgaande beperkingen.  U dient op de hoogte te zijn van enkele belangrijke concepten voordat u uw apps gaat plannen, beoordelen, configureren en cutover-migratie gaat uitvoeren.  
- Hoewel sommige apps makkelijk kunnen worden gemigreerd, kan voor apps met meer complexe vereisten (zoals aangepaste claims) aanvullende configuratie in Azure AD en/of Azure AD Connect zijn vereist
- In de meestvoorkomende scenario's zijn alleen de claim NameId en andere veelvoorkomende claims voor gebruikers-id's voor een app vereist. Om te bepalen of er nog meer claims worden vereist, onderzoekt u welke claims u vanuit AD FS of uw externe id-provider uitgeeft
- Zodra u hebt vastgesteld dat er aanvullende claims zijn vereist, dient u te controleren of deze beschikbaar zijn in Azure AD.  Controleer Azure AD Connect-synchronisatieconfiguratie zodat er een vereist kenmerk, bijvoorbeeld samAccountName, met Azure AD wordt gesynchroniseerd
- Als de kenmerken in Azure AD beschikbaar zijn, voegt u claimuitgifteregels toe in Azure AD om deze kenmerken als claims in uitgegeven tokens op te nemen.  Dit wordt uitgevoerd in de eigenschappen voor eenmalige aanmelding van de app in Azure AD.

### <a name="assessing-what-can-be-migrated"></a>Vaststellen wat er kan worden gemigreerd
SAML 2.0-toepassingen kunnen worden geïntegreerd met Azure AD via de Azure AD-toepassingsgalerie of als niet-galerietoepassingen.  

Voor sommige configuraties zijn aanvullende stappen vereist voor de configuratie in Azure AD, waarvan sommige momenteel niet worden ondersteund.  Om te bepalen wat er kan worden verplaatst, kijkt u naar de huidige configuratie van elk van de apps, met name de volgende:
- Geconfigureerde claimregels (transformatieregels voor uitgifte)
- Indeling en kenmerk van NameID van SAML
- Uitgegeven SAML-tokenversies
- Andere configuraties, zoals de autorisatieregels voor uitgifte of toegangsbeheerbeleid en regels voor Multi Factor Authentication (aanvullende verificatieregels)

#### <a name="what-can-be-migrated-today"></a>Wat er momenteel kan worden gemigreerd
Apps die momenteel gemakkelijk kunnen worden gemigreerd zijn onder meer SAML 2.0-apps die gebruikmaken van de standaardset configuratie-elementen en claims.  Deze apps kunnen bestaan uit
- user principal name
- e-mailadres
- Voornaam
- Achternaam
- Alternatief kenmerk, zoals NameID van SAML, waaronder het Azure AD-e-mailkenmerk, e-mailvoorvoegsel, werknemer-id, extensiekenmerken 1- 15 of on-premises SamAccountName (zie [De claim NameIdentifier bewerken)](./develop/active-directory-saml-claims-customization.md)
- Aangepaste claims (zie de documenten [hier](active-directory-claims-mapping.md) en [hier](./develop/active-directory-saml-claims-customization.md) voor informatie over ondersteunde claimtoewijzingen)

Naast aangepaste claims en NameID-elementen, zijn er nog configuraties waarvoor aanvullende configuratiestappen zijn vereist in Azure AD als onderdeel van de migratie. Deze zijn:
- Aangepaste autorisatie van MFA-regels in AD FS (geconfigureerd met behulp van de functie [Voorwaardelijke toegang voor Azure AD](active-directory-conditional-access-azure-portal.md))
- Apps met meerdere SAML-eindpunten kunnen worden geconfigureerd in Azure AD via PowerShell (deze mogelijkheid is niet beschikbaar in de portal)
- WS-Federation-apps, zoals SharePoint-apps waarvoor SAML 1.1-versietokens zijn vereist, moeten handmatig worden geconfigureerd via PowerShell

#### <a name="appsconfigurations-not-supported-in-azure-ad-today"></a>Apps/configuraties die momenteel niet in Azure AD worden ondersteund
Apps waarvoor de volgende mogelijkheden zijn vereist, kunnen momenteel niet worden gemigreerd.  Geef feedback in de sectie Opmerkingen als u apps hebt waarvoor deze functies zijn vereist.
- Protocolmogelijkheden
    - Ondersteuning voor eenmalige SAML-afmelding (SLO) van alle aangemelde apps
    - Ondersteuning voor WS-Trust ActAs-patroon
    - SAML-artefactomzetting 
    - Handtekeningverificatie van ondertekende SAML-aanvragen (let erop dat ondertekende aanvragen worden geaccepteerd, maar dat de handtekening niet is geverifieerd)
 - Tokenmogelijkheden 
     - SAML-tokenversleuteling 
     - SAML-versie 1.1-tokens binnen SAML-protocolantwoorden 
- Claims in tokenmogelijkheden
    - Uitgifte van on-premises groepsnamen als claim
    - Claims van archieven anders dan Azure AD
    - Transformatieregels voor uitgifte van complexe claims (zie [dit document](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) en [dit document](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization) voor informatie over ondersteunde claimtoewijzingen)
    - Mapextensies uitgeven als claims
    - Aangepaste specificatie van indeling van NameID
    - Uitgifte van meerwaardige kenmerken

>[!NOTE]
>Azure AD is in ontwikkeling en er worden aanvullende mogelijkheden op dit gebied toegevoegd. Dit document wordt regelmatig bijgewerkt. 

### <a name="configuring-azure-ad"></a>Azure AD configureren    
#### <a name="configure-single-sign-on-sso-settings-for-the-saas-app"></a>Instellingen voor eenmalige aanmelding (SSO) configureren voor de SaaS-app

In Azure AD wordt configuratie van eenmalige SAML-aanmelding (zoals door de app vereist) uitgevoerd binnen de eigenschappen voor eenmalige aanmelding van de app onder Gebruikerskenmerken, zoal hieronder weergegeven:

![](media/migrate-adfs-apps-to-azure/migrate3.png)
- Klik op View and edit all other user attribute om de kenmerken te zien die als claims in het beveiligingstoken moeten worden verzonden

![](media/migrate-adfs-apps-to-azure/migrate4.png)
- Klik op een bepaalde kenmerkrij om deze te bewerken of klik op Kenmerk toevoegen om een nieuw kenmerk toe te voegen. 
![](media/migrate-adfs-apps-to-azure/migrate5.png)

#### <a name="assign-users-to-the-app"></a>Gebruikers aan de app toewijzen
Uw gebruikers moeten vanuit Azure AD toegang hebben zodat ze zich vanuit Azure AD bij een bepaalde SaaS-app kunnen aanmelden.

Als u gebruikers via de Azure AD-portal wilt toewijzen, gaat u in de portal naar het scherm van de SaaS-app en klikt u op de zijbalk op Gebruikers en groepen. Als u een groep of een gebruiker wilt toevoegen, klikt u boven aan het scherm op Gebruiker toevoegen. 

![](media/migrate-adfs-apps-to-azure/migrate6.png) 

![](media/migrate-adfs-apps-to-azure/migrate7.png) Voor controle op de toegang dient de gebruiker bij aanmelden de betreffende SaaS-app te kunnen zien in zijn of haar [toegangspaneel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction). Deze kan worden gevonden op http://myapps.microsoft.com. Bijvoorbeeld: de gebruiker is toegang verleend tot zowel Salesforce als ServiceNow.

![](media/migrate-adfs-apps-to-azure/migrate8.png)

### <a name="configuring-the-saas-app"></a>Saas-app configureren
Het cutover-proces van on-premises federatie naar Azure AD is afhankelijk van het feit of de SaaS-app waarmee u werkt meerdere id-providers ondersteunt.  Hier volgen enkele vragen betreffende ondersteuning voor meerdere id-providers:

   **V: Wat betekent het dat een app meerdere id-providers ondersteunt?**
    
   A: In SaaS-apps die ondersteuning bieden voor meerdere id-providers, kunt u alle gegevens invoeren over de nieuwe id-provider (in dit geval Azure AD) voordat u de nieuwe aanmeldingsmethode wilt wijzigen.  Zodra de configuratie gereed is, kunt u de authenticatieconfiguratie van de app naar Azure AD laten wijzen.

   V: Waarom maakt het uit of de SaaS-app meerder id-providers ondersteunt?

   A: Als er geen ondersteuning is voor meerdere id-providers, heeft de beheerder een korte periode nodig voor bijvoorbeeld service of onderhoud waarin Azure AD wordt geconfigureerd als de nieuwe id-provider van de app. Tijdens deze periode dienen de gebruikers op de hoogte te worden gesteld dat ze zich niet bij hun account kunnen aanmelden.

   Als een app meerdere id-providers ondersteunt, kan de configuratie van de extra id-provider van te voren worden uitgevoerd, dus de beheerder kan gewoon naar een ander id-provider overschakelen bij cutover van Azure.

   Als de app meerdere id-providers ondersteunt en u meerdere id-providers tegelijkertijd verificatie bij aanmelden wilt laten afhandelen, kan de gebruiker kiezen welke id-provider moet worden geverifieerd op de aanmeldingspagina.

#### <a name="example-multiple-idp-support"></a>Voorbeeld: ondersteuning van meerdere id-providers
In Salesforce bijvoorbeeld kan de configuratie van de id-provider worden gevonden onder Instellingen > Bedrijfsinstellingen > Mijn domein > Authenticatieconfiguratie.

![](media/migrate-adfs-apps-to-azure/migrate9.png)

Vanwege de configuratie die eerder is gemaakt onder Identiteit > Instellingen voor eenmalige aanmelding, moet u uw id-provider voor authenticatieconfiguratie kunnen wijzigen van bijvoorbeeld AD FS naar Azure AD. 

![](media/migrate-adfs-apps-to-azure/migrate10.png)

### <a name="optional-configure-user-provisioning-in-azure-ad"></a>Optioneel: inrichten van gebruikers configureren in Azure AD
Als u Azure AD rechtstreeks gebruikers wilt laten inrichten voor een bepaalde SaaS-app, raadpleegt u dit document over het beheren van het inrichten van gebruikersaccounts voor bedrijfs-apps in Azure AD.

## <a name="next-steps"></a>Volgende stappen

- [Toepassingen beheren met Azure Active Directory](active-directory-enable-sso-scenario.md)
- [Toegang tot apps beheren](active-directory-managing-access-to-apps.md)
- [Azure AD Connect Federation](active-directory-aadconnectfed-whatis.md)