---
title: Migreren van Azure Access Control Service (ACS)
description: Opties voor het verplaatsen van apps en services van Azure Access Control Service | Microsoft Azure
services: active-directory
documentationcenter: dev-center-name
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/14/2017
ms.author: dastrock
ms.openlocfilehash: e32cac7feda929a63c4a80fc0078b221117eb2b5
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="migrating-from-azure-access-control-service-acs"></a>Migreren van Azure Access Control Service (ACS)

Microsoft Azure Active Directory-toegangsbeheer (ook wel bekend als Access Control Service of ACS) wordt buiten gebruik gesteld in November 2018.  Toepassingen en services die momenteel wordt gebruikt door ACS moeten volledig migreren naar een ander verificatiemechanisme voor deze datum. Dit document beschrijft de aanbevelingen voor het huidige klanten als ze willen hun gebruik van ACS als vervangen markeren. Als u ACS op dit moment niet worden gebruikt, hoeft u geen geen actie te ondernemen.


## <a name="brief-acs-overview"></a>Beknopt overzicht van ACS

ACS is verificatie met een cloudservice waarmee u een eenvoudige manier voor verificatie en autoriseren van gebruikers toegang te krijgen tot uw webtoepassingen en services terwijl veel functies van verificatie en autorisatie voor moet rekening worden gehouden van uw code. Dit wordt hoofdzakelijk gebruikt door ontwikkelaars en architecten van .NET-clients, ASP.NET-webtoepassingen en WCF-webservices.

De gebruiksvoorbeelden voor ACS kunnen worden onderverdeeld in drie hoofdcategorieën:

- Verificatie van bepaalde Microsoft-cloudservices, zoals Azure Service Bus, Dynamics CRM en andere gebruikers. Clienttoepassingen kunnen tokens verkrijgen van ACS die kan worden gebruikt om deze services verschillende acties uitvoeren te verifiëren.
- Verificatie toevoegen aan webtoepassingen, zowel van de verschillende maat gemaakte & van het vooraf verpakte RAS (zoals Sharepoint). ACS 'passieve' verificatie gebruikt, kunnen webtoepassingen Meld u aan met accounts van Google, Facebook, Yahoo, Microsoft-account (Live ID), Azure Active Directory en AD FS ondersteunen.
- De beveiliging van aangepaste ingebouwde webservices met tokens die zijn uitgegeven door ACS. Met 'active'-verificatie, webservices kunnen ervoor zorgen dat ze alleen toegang toestaan van bekende clients die zijn geverifieerd met ACS.

Elk van deze gebruiksvoorbeelden en hun aanbevolen strategieën in de volgende secties wordt besproken. In de meeste gevallen worden de aanzienlijke codewijzigingen nodig voor het migreren van bestaande apps en services naar nieuwere technologieën. Het is raadzaam dat u begint met plannen en uitvoeren van uw migratie onmiddellijk om te voorkomen dat eventuele voor uitval of uitvaltijd.

> [!WARNING]
> In de meeste gevallen worden de aanzienlijke codewijzigingen nodig voor het migreren van bestaande apps en services naar nieuwere technologieën. Het is raadzaam dat u begint met plannen en uitvoeren van uw migratie onmiddellijk om te voorkomen dat eventuele voor uitval of uitvaltijd.

Qua architectuur is ACS volledig bestaat uit de volgende onderdelen:

- Een beveiligde beveiligingstokenservice (STS) die verificatieaanvragen ontvangt & beveiligingstokens in.
- De klassieke Azure portal, dat wordt gebruikt voor het maken, verwijderen en in-/ uitschakelen ACS-naamruimten.
- Een afzonderlijke ACS management portal, dat wordt gebruikt voor het aanpassen en het gedrag van een ACS-naamruimte configureren.
- Een management-service, die kan worden gebruikt voor het automatiseren van de functies van de portals.
- Een token transformatie regel engine, die kan worden gebruikt voor het definiëren van complexe logica voor het bewerken van de indeling van de uitvoer van tokens die zijn uitgegeven door ACS.

Voor het gebruik van deze onderdelen moet u een of meer ACS **naamruimten**. Een naamruimte is een toegewezen exemplaar van ACS die uw toepassingen en services met communiceren; het is geïsoleerd van alle andere ACS klanten die hun eigen naamruimten gebruiken. Een ACS-naamruimte heeft een specifieke URL, zoals:

```HTTP
https://mynamespace.accesscontrol.windows.net
```

Alle communicatie met de STS- en beheerbewerkingen zijn gedaan bij deze URL, met verschillende paden voor verschillende doeleinden. Monitor om te bepalen als uw toepassingen of services ACS gebruikt, al het verkeer naar `https://{namespace}.accesscontrol.windows.net`.  Al het verkeer naar deze URL wordt verkeer dat wordt verwerkt door ACS en moet worden stopgezet.  De enige uitzondering hierop is al het verkeer naar `https://accounts.accesscontrol.windows.net` -verkeer naar deze URL al wordt verwerkt door een andere service en wordt niet beïnvloed door ACS afschaffing.  Ook moet u ervoor dat u aanmelden bij de klassieke Azure-portal en Controleer voor elke ACS-naamruimten in de abonnementen waarvan u eigenaar.  ACS-naamruimten worden vermeld in de **Active Directory** service onder de **Access Control-naamruimten** tabblad.

Zie voor meer informatie over ACS [dit documentatie op MSDN gearchiveerd](https://msdn.microsoft.com/library/hh147631.aspx).

## <a name="retirement-schedule"></a>De planning buiten gebruik stellen

Vanaf November 2017, worden alle ACS-onderdelen zijn volledig ondersteunde & operationele. De enige beperking is dat [nieuwe ACS naamruimten kan niet worden gemaakt via de klassieke Azure-portal](https://azure.microsoft.com/blog/acs-access-control-service-namespace-creation-restriction/).

De tijdlijn voor de afschaffing van deze onderdelen volgens deze planning:

- **November 2017**: de Azure AD-beheerder problemen in de klassieke Azure portal [is buiten gebruik gesteld](https://blogs.technet.microsoft.com/enterprisemobility/2017/09/18/marching-into-the-future-of-the-azure-ad-admin-experience-retiring-the-azure-classic-portal/). Op dit moment naamruimte management voor ACS beschikbaar op deze nieuwe zijn, toegewezen URL: `http://manage.windowsazure.com?restoreClassic=true`. Dit is kunt u uw bestaande naamruimten weergeven, in-of uitschakelen ze en verwijder deze volledig indien gewenst.
- **2018 april**: beheer van de ACS-naamruimte is niet langer beschikbaar is op deze URL toegewezen. Op dit moment, kan u niet inschakelen/uitschakelen, verwijderen of uw naamruimten ACS inventariseren. De ACS-beheerportal, is echter volledig functioneel is en zich op `https://{namespace}.accesscontrol.windows.net`. Alle andere onderdelen van ACS blijven werken doorgaans ook.
- **November 2018**: alle ACS-onderdelen permanent zijn afgesloten. Dit omvat de ACS-beheerportal, de beheerservice STS en regel-token transformatie-engine. Op dit moment geen verzoeken verzonden naar ACS (te vinden op `{namespace}.accesscontrol.windows.net`) mislukken. U moet hebt gemigreerd alle bestaande apps en services voor andere technologieën, voordat deze periode.


## <a name="migration-strategies"></a>Strategieën voor migratie

De volgende secties beschrijven hoog niveau aanbevelingen voor het migreren van ACS naar andere Microsoft-technologieën.

### <a name="clients-of-microsoft-cloud-services"></a>Clients van Microsoft-cloudservices

Elk van de Microsoft cloud-services die nu door ACS uitgegeven tokens accepteert ondersteunt ten minste één andere vorm van verificatie. Het juiste verificatiemechanisme varieert voor elke service, zodat het is raadzaam verwijst naar de specifieke documentatie van elke service voor de officiële. Elke reeks documentatie is voor het gemak hier opgegeven:

| Service | Richtlijnen |
| ------- | -------- |
| Azure Service Bus | [Migreren naar handtekeningen voor gedeelde toegang](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-migrate-acs-sas) |
| Azure Relay | [Migreren naar handtekeningen voor gedeelde toegang](https://docs.microsoft.com/azure/service-bus-relay/relay-migrate-acs-sas) |
| Azure Cache | [Migreren naar Azure Redis-cache](https://docs.microsoft.com/azure/redis-cache/cache-faq#which-azure-cache-offering-is-right-for-me) |
| Azure Data markt | [Migreren naar de cognitieve Services-API 's](https://docs.microsoft.com/azure/machine-learning/studio/datamarket-deprecation) |
| BizTalk Services | [Migreren naar Azure Logic Apps](https://docs.microsoft.com/azure/machine-learning/studio/datamarket-deprecation) |
| Azure Media Services | [Migreren naar Azure AD-verificatie](https://azure.microsoft.com/blog/azure-media-service-aad-auth-and-acs-deprecation/) |
| Azure Backup | [Upgrade de Azure Backup-Agent](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq) |

<!-- Dynamics CRM: Migrate to new SDK, Dynamics team handling privately -->
<!-- Azure RemoteApp deprecated in favor of Citrix: http://www.zdnet.com/article/microsoft-to-drop-azure-remoteapp-in-favor-of-citrix-remoting-technologies/ -->
<!-- Exchange push notifications are moving, customers don't need to move -->
<!-- Retail federation services are moving, customers don't need to move -->
<!-- Azure StorSimple: TODO -->
<!-- Azure SiteRecovery: TODO -->


### <a name="web-applications-using-passive-authentication"></a>Webtoepassingen die gebruikmaken van de passieve verificatie

Webtoepassingen met ACS voor gebruikersverificatie, ACS opgegeven voor de volgende functies en mogelijkheden voor ontwikkelaars en architecten van webtoepassingen:

- Diepe integratie met Windows Identity Foundation (WIF).
- Federatie met Google, Facebook, Yahoo, Microsoft-account (Live ID), Azure Active Directory en AD FS.
- Ondersteuning voor de volgende verificatieprotocollen voor: OAuth 2.0-concept 13 en Ws-Trust, Ws-Federation.
- Ondersteuning voor de volgende indelingen token: JSON web token (JWT), SAML 1.1 SAML 2.0 en eenvoudige web-token (SWT).
- Een ervaring home realm discovery geïntegreerd in WIF dat gebruikers om op te halen van het type account die ze gebruiken voor het aanmelden is toegestaan. Deze ervaring is gehost door de webtoepassing en volledig worden aangepast.
- Token transformatie die toegestaan uitgebreide aanpassing van de claims die zijn ontvangen door de webtoepassing van ACS, met inbegrip van:
    - claims van de id-providers te doorlopen
    - aanvullende aangepaste claims toe te voegen
    - eenvoudige als dan logica voor het verlenen van claims onder bepaalde omstandigheden

Er is helaas niet één service waarmee al deze mogelijkheden equivalent.  Moet u evalueren welke mogelijkheden van ACS u nodig hebt en kies vervolgens tussen het gebruik van [ **Azure Active Directory** ](https://azure.microsoft.com/develop/identity/signin/) (Azure AD), [ **Azure AD B2C** ](https://azure.microsoft.com/services/active-directory-b2c/), of een andere cloudservice voor verificatie.

#### <a name="migrating-to-azure-active-directory"></a>Migreren naar Azure Active Directory

Een pad in overweging moet nemen is uw apps en services integreren rechtstreeks met Azure Active Directory. Azure AD is de cloud-gebaseerde id-provider voor Microsoft werken & schoolaccounts - is de id-provider voor Office 365, Azure en nog veel meer. Het biedt vergelijkbare federated authentication-mogelijkheden voor ACS, maar biedt geen ondersteuning voor alle ACS-functies. Het primaire voorbeeld is de Federatie met sociale id-providers, zoals Facebook, Google en Yahoo. Uw gebruikers zich aanmelden met deze typen referenties, is Azure AD niet als u. Tevens voert het niet per se de exacte dezelfde als ACS - tijdens zowel ACS verificatieprotocollen-ondersteuning en Azure AD-ondersteuning OAuth, er zijn bijvoorbeeld subtiele verschillen tussen elke implementatie waarvoor u code wijzigen als onderdeel van de migratie.

Azure AD biedt echter verschillende mogelijke voordelen voor klanten van ACS. Deze systeemeigen ondersteunt Microsoft werken & schoolaccounts gehost in de cloud, die vaak worden gebruikt door de ACS-klanten.  Een Azure AD-tenant kan ook federatieve op een of meer exemplaren van on-premises Active Directory via AD FS, zodat uw app om te verifiëren voor zowel cloud-gebaseerde gebruikers als gebruikers on-premises worden gehost.  Het ondersteunt ook het protocol Ws-Federation, waardoor het betrekkelijk eenvoudig te integreren met een webtoepassing met behulp van Windows Identity Foundation (WIF).

De volgende tabel vergelijkt de functies van ACS relevant zijn voor webtoepassingen die in Azure AD. Op een hoog niveau **Azure Active Directory is waarschijnlijk de juiste keuze voor uw migratie als u toestaat dat alleen gebruikers aanmelden met hun werk Microsoft & schoolaccounts**.

| Mogelijkheid | ACS-ondersteuning | Ondersteuning van Azure AD |
| ---------- | ----------- | ---------------- |
| **Typen Accounts** | | |
| Microsoft werkt & schoolaccounts | Ondersteund | Ondersteund |
| Accounts van Windows Server Active Directory en AD FS | Ondersteund via federatie met Azure AD-tenant <br /> Ondersteund via directe Federatie met AD FS | Alleen ondersteund via federatie met Azure AD-tenant | 
| Accounts van andere bedrijfssystemen, identity management | Mogelijk via federatie met Azure AD-tenant <br />Ondersteund via directe federation | Mogelijk via federatie met Azure AD-tenant |
| Microsoft-accounts voor persoonlijk gebruik (Windows Live ID) | Ondersteund | Ondersteund via Azure AD v2.0 OAuth-protocol, maar niet via alle andere protocollen. | 
| Facebook, Google, Yahoo accounts | Ondersteund | Niet ondersteund dan ook. |
| **Protocollen en SDK compatibiliteit** | | |
| Windows Identity Foundation (WIF) | Ondersteund | Ondersteund, maar beperkt instructies beschikbaar. |
| WS-Federation | Ondersteund | Ondersteund |
| OAuth 2.0 | Ondersteuning voor concept 13 | Ondersteuning voor RFC 6749, de meeste moderne-specificatie. |
| WS-Trust | Ondersteund | Niet ondersteund |
| **Token indelingen** | | |
| JSON Web Tokens (JWTs) | Ondersteund In Beta | Ondersteund |
| 1.1 SAML-tokens | Ondersteund | Ondersteund |
| De tokens SAML 2.0 | Ondersteund | Ondersteund |
| Eenvoudige Webtokens (SWT) | Ondersteund | Niet ondersteund |
| **Aanpassingen** | | |
| Aanpasbare thuisrealm detectieaccount/UI verzamelen | Downloadbare code die kan worden opgenomen in apps | Niet ondersteund |
| Aangepaste handtekeningcertificaten-token uploaden | Ondersteund | Ondersteund |
| Claims in tokens aanpassen | Passthrough invoerclaims van id-providers<br />Haal het toegangstoken van de identiteitsprovider als claim<br />Uitvoer claims op basis van waarden van invoerbestand claims uitgeven<br />Uitvoer claims uitgeven aan constante waarden | Kan geen claims van federatieve identiteitsproviders passthrough<br />Kan geen toegangstoken ophalen uit de id-provider als een claim<br />Kan geen uitvoer claims op basis van waarden van invoerbestand claims uitgeven<br />Uitvoer claims met constante waarden kunnen worden verleend.<br />Uitvoer claims op basis van eigenschappen van de gebruikers gesynchroniseerd naar Azure AD kan verlenen |
| **Automatisering** | | |
| Configuratiebeheer/taken automatiseren | Ondersteund via ACS-Management-Service | Ondersteund via Microsoft Graph & Azure AD Graph API. |

Als u besluit dat Azure AD het juiste pad doorsturen voor uw toepassingen en services is, moet u zich bewust van twee methoden voor het integreren van uw app met Azure AD.

Als u Ws-Federation/WIF wilt integreren met Azure AD, wordt aangeraden volgende [beschreven procedure in dit artikel](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-federated-sso-non-gallery). Het artikel verwijst naar Azure AD voor op basis van SAML eenmalige aanmelding, maar werkt voor het configureren van Ws-Federation ook configureren. Na deze benadering is een Azure AD Premium-licentie vereist, maar heeft twee voordelen:

- Krijgt u de volledige flexibiliteit van Azure AD-token aanpassen. Hiermee kunt u voor het aanpassen van de claims die overeenkomen met die zijn uitgegeven door ACS, met name met inbegrip van de gebruiker-ID of naam-id claim uitgegeven door Azure AD. Moet u ervoor te zorgen dat de gebruikers-id uitgegeven door Azure AD overeenkomen met die zijn uitgegeven door ACS, zodat u nog steeds consistent gebruiker-id's voor uw gebruikers ontvangen zelfs na het wijzigen van de technologieën.
- U kunt een token-ondertekening van certificaten die specifiek zijn voor uw toepassing configureren waarvan u bepaalt levensduur.

<!--

Possible nameIdentifiers from ACS (via AAD or ADFS):
- ADFS - Whatever ADFS is configured to send (email, UPN, employeeID, what have you)
- Default from AAD using App Registrations, or Custom Apps before ClaimsIssuance policy: subject/persistent ID
- Default from AAD using Custom apps nowadays: UPN
- Kusto can't tell us distribution, it's redacted

-->

> [!NOTE]
> Gaat met deze methode is een Azure AD Premium-licentie vereist. Als u een ACS-klant bent en u een premium-licentie nodig voor het instellen van eenmalige aanmelding voor een toepassing, aan ons bereiken zodat we graag bieden developer-licenties voor het gebruik.

Een alternatieve methode is het volgen van [dit codevoorbeeld](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation), waardoor enigszins andere instructies over het instellen van Ws-Federation. Dit voorbeeld gebruikt geen WIF, maar in plaats daarvan de middleware ASP.NET 4.5 OWIN. Echter, de instructies voor het app-registratie zijn geldig voor apps met behulp van WIF en hoeven niet een Azure AD Premium-licentie.  Als u deze benadering, heneed om te begrijpen [sleutelrollover in Azure AD-ondertekening](https://docs.microsoft.com/azure/active-directory/develop/active-directory-signing-key-rollover). Deze methode maakt gebruik van de Azure AD globaal ondertekeningssleutel voor het probleem van tokens. Standaard geeft WIF handtekeningsleutels niet automatisch vernieuwd. Wanneer u Azure AD de globale handtekeningsleutels draait, moet uw implementatie WIF worden voorbereid om de wijzigingen te accepteren.

Als u zich kunt integreren met Azure AD via het OpenID Connect of OAuth-protocol, raden wij doet.  We hebben uitgebreide documentatie en richtlijnen voor het Azure AD integreren in uw webtoepassing beschikbaar in onze [Ontwikkelaarshandleiding voor Azure AD](http://aka.ms/aaddev).

<!-- TODO: If customers ask about authZ, let's put a blurb on role claims here -->

#### <a name="migrating-to-azure-ad-b2c"></a>Migreren naar Azure AD B2C

De andere migratiepad in overweging moet nemen is Azure AD B2C.  B2C is een cloud-verificatie service die vergelijkbaar is met ACS, kunnen ontwikkelaars hun management logica voor authenticatie en identiteit op een cloudservice uitbesteden.  Dit is een betaalde service (met vrije & premium lagen) die zijn bestemd voor consumer geconfronteerd met toepassingen die miljoenen actieve gebruikers kunnen hebben.

ACS, een van de meest aantrekkelijk B2C-functies is die is biedt ondersteuning voor verschillende soorten accounts. Met B2C, u kunt aanmelden gebruikers met hun Facebook, Google, Microsoft, LinkedIn, GitHub, Yahoo-accounts en meer. B2C ondersteunt ook 'lokale accounts' of gebruikersnaam en wachtwoorden op dat gebruikers maken specifiek voor uw toepassing. Azure AD B2C biedt ook uitgebreide uitbreidbaarheid die u gebruiken kunt voor het aanpassen van uw aanmelding stromen. Het ondersteunt echter niet, de breedte van de verificatieprotocollen & token indelingen waardoor ACS klanten mogelijk. Deze ook niet worden gebruikt voor aanvullende informatie over de gebruiker van de id-provider, Microsoft opvragen & ophalen van tokens of anderszins.

De volgende tabel vergelijkt de functies van ACS relevant zijn voor webtoepassingen die beschikbaar zijn in Azure AD B2C. Op een hoog niveau **Azure AD B2C is waarschijnlijk de juiste keuze voor uw migratie als uw toepassing consumer geconfronteerd, of als er veel verschillende typen accounts ondersteunt.**

| Mogelijkheid | ACS-ondersteuning | Azure AD B2C-ondersteuning |
| ---------- | ----------- | ---------------- |
| **Typen Accounts** | | |
| Microsoft werkt & schoolaccounts | Ondersteund | Ondersteund via een aangepast beleid  |
| Accounts van Windows Server Active Directory en AD FS | Ondersteund via directe Federatie met AD FS | Ondersteund via SAML-federation gebruik van aangepast beleid |
| Accounts van andere bedrijfssystemen, identity management | Ondersteund via directe federation via Ws-Federation | Ondersteund via SAML-federation gebruik van aangepast beleid |
| Microsoft-accounts voor persoonlijk gebruik (Windows Live ID) | Ondersteund | Ondersteund | 
| Facebook, Google, Yahoo accounts | Ondersteund | Facebook & Google ondersteund standaard, Yahoo ondersteund via OpenID Connect federation gebruik van aangepast beleid |
| **Protocollen en SDK compatibiliteit** | | |
| Windows Identity Foundation (WIF) | Ondersteund | Niet ondersteund. |
| WS-Federation | Ondersteund | Niet ondersteund. |
| OAuth 2.0 | Ondersteuning voor concept 13 | Ondersteuning voor RFC 6749, de meeste moderne-specificatie. |
| WS-Trust | Ondersteund | Niet ondersteund. |
| **Token indelingen** | | |
| JSON Web Tokens (JWTs) | Ondersteund In Beta | Ondersteund |
| 1.1 SAML-tokens | Ondersteund | Niet ondersteund |
| De tokens SAML 2.0 | Ondersteund | Niet ondersteund |
| Eenvoudige Webtokens (SWT) | Ondersteund | Niet ondersteund |
| **Aanpassingen** | | |
| Aanpasbare thuisrealm detectieaccount/UI verzamelen | Downloadbare code die kan worden opgenomen in apps | Volledig aanpasbare UI via aangepaste CSS. |
| Aangepaste handtekeningcertificaten-token uploaden | Ondersteund | Aangepaste handtekeningsleutels, certificaten, ondersteund via een aangepast beleid. |
| Claims in tokens aanpassen | Passthrough invoerclaims van id-providers<br />Haal het toegangstoken van de identiteitsprovider als claim<br />Uitvoer claims op basis van waarden van invoerbestand claims uitgeven<br />Uitvoer claims uitgeven aan constante waarden | Kan passthrough claims van de id-providers. Aangepast beleid dat is vereist voor sommige claims.<br />Kan geen toegangstoken ophalen uit de id-provider als een claim<br />Kunnen uitvoer claims op basis van waarden van invoerclaims via aangepaste beleidsregels voor uitgeven<br />Uitvoer claims met constante waarden via aangepaste beleidsregels kunnen worden verleend. |
| **Automatisering** | | |
| Configuratiebeheer/taken automatiseren | Ondersteund via ACS-Management-Service | Het maken van gebruikers toegestaan via Azure AD Graph API. Kan niet maken B2C-tenants, toepassingen of beleid via een programma. |

Als u besluit dat Azure AD B2C het juiste pad doorsturen voor uw toepassingen en services is, moet u beginnen met de volgende bronnen:

- [Azure AD B2C-documentatie](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview)
- [Azure AD B2C aangepast beleid](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview-custom)
- [Azure AD B2C-prijzen](https://azure.microsoft.com/pricing/details/active-directory-b2c/)


#### <a name="other-migration-options"></a>Andere opties voor de migratie

Als geen van beide Azure AD of Azure AD B2C voldoet aan de behoeften van uw webtoepassing, kunt contact met ons en je kunt het beste pad doorsturen identificeren.

<!--

#### Migrating to Ping Identity or Auth0

In some cases, you may find that neither Azure AD nor Azure AD B2C is sufficient to replace ACS in your web applications without making major code changes.  Some common examples might include:

- web applications using WIF and/or WS-Federation for sign-in with social identity providers such as Google or Facebook.
- web applications performing direct federation to an enterprise IDP over the Ws-Federation protocol.
- web applications that require the access token issued by a social identity provider (such as Google or Facebook) as a claim in the tokens issued by ACS.
- web applications with complex token transformation rules that Azure AD or Azure AD B2C cannot reproduce.

In these cases, you may want to consider migrating your web application to another cloud authentication service. We recommend exploring the following options, as each provides capabilities similar to ACS:

- [Auth0](https://auth0.com/) has created [high-level migration guidance for customers of ACS](https://auth0.com/blog/windows-azure-acs-alternative-replacement/), and provides a feature-by-feature comparison of ACS vs. Auth0.
- Enterprise customers should consider [Ping Identity](https://www.pingidentity.com) as well. Reach out to us and we can connect you with a representative from Ping who is prepared to help identify potential solutions.

Our aim in working with Ping Identity & Auth0 is to ensure that all ACS customers have a path forward for their apps & services that minimizes the amount of work required to move off of ACS.

-->

<!--

## Sharepoint 2010, 2013, 2016

TODO: AAD only, use AAD SAML 1.1 tokens, when we bring it back online.
Other IDPs: use Auth0? https://auth0.com/docs/integrations/sharepoint.

-->

### <a name="web-services-using-active-authentication"></a>Webservices met behulp van active-verificatie

ACS beschikbaar voor web-services die zijn beveiligd met tokens die zijn uitgegeven door ACS, de volgende functies en mogelijkheden:

- Mogelijkheid om te registreren voor een of meer **service-identiteiten** in uw ACS-naamruimte die kan worden gebruikt voor het aanvragen van tokens.
- Ondersteuning voor de OAuth-TERUGLOPEN & OAuth 2.0-concept 13 protocollen voor het aanvragen van tokens, met behulp van de volgende typen referenties:
    - Een eenvoudig wachtwoord voor de service-identiteit gemaakt
    - Een SWT met een symmetrische sleutel of X509 ondertekend certificaat
    - Een SAML-token dat is uitgegeven door een vertrouwde identiteitsprovider (meestal een exemplaar van AD FS)
- Ondersteuning voor de volgende indelingen token: JSON web token (JWT), SAML 1.1 SAML 2.0 en eenvoudige web-token (SWT).
- Eenvoudige token transformatie-regels

Service-identiteiten in ACS worden meestal gebruikt voor het implementeren van server-naar-server (S2S) zoals verificatie.  

#### <a name="migrating-to-azure-active-directory"></a>Migreren naar Azure Active Directory

De aanbeveling voor dit type authenticatiestroom is om te migreren naar [ **Azure Active Directory** ](https://azure.microsoft.com/develop/identity/signin/) (Azure AD). Azure AD is de cloud-gebaseerde id-provider voor Microsoft werken & schoolaccounts - is de id-provider voor Office 365, Azure en nog veel meer. Maar kan ook worden gebruikt voor de server naar server-verificatie met Azure AD-implementatie van de OAuth-client referenties verlenen.  De volgende tabel vergelijkt de mogelijkheden van ACS in server naar server-verificatie met de beschikbare in Azure AD.

| Mogelijkheid | ACS-ondersteuning | Ondersteuning van Azure AD |
| ---------- | ----------- | ---------------- |
| Het registreren van een webservice | Maak een relying party in de ACS-beheerportal. | Een Azure AD-webtoepassing maken in de Azure portal. |
| Het registreren van een client | Maak een service-identiteit in ACS-beheerportal. | Een andere Azure AD-webtoepassing maken in de Azure portal. |
| Protocol dat wordt gebruikt | Protocol OAuth INPAKKEN<br />OAuth 2.0-concept 13 clientreferenties verlenen | OAuth 2.0-clientreferenties verlenen |
| Methoden voor clientverificatie | Eenvoudig wachtwoord<br />Ondertekende SWT<br />SAML-token van federatieve IDP | Eenvoudig wachtwoord<br />Ondertekende JWT |
| Token indelingen | JWT<br />SAML 1.1<br />SAML 2.0<br />SWT<br /> | Alleen JWT |
| Token transformatie | Aangepaste claims toe te voegen<br />Eenvoudige als dan claimuitgifte logica | Aangepaste claims toe te voegen | 
| Configuratiebeheer/taken automatiseren | Ondersteund via ACS-Management-Service | Ondersteund via Microsoft Graph & Azure AD Graph API. |

Raadpleeg de volgende bronnen voor richtlijnen op uitvoering server naar server-scenario's:

- [Service to Service-sectie van de Azure AD-handleiding voor ontwikkelaars](https://aka.ms/aaddev).
- [Voorbeeld van code daemon met behulp van de referenties van de client eenvoudig wachtwoord](https://github.com/Azure-Samples/active-directory-dotnet-daemon)
- [Voorbeeld van code daemon met behulp van de referenties van de certificaat-client](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential)

#### <a name="other-migration-options"></a>Andere opties voor de migratie

Als Azure AD voldoet niet aan de behoeften van uw web-service, laat een opmerking en je kunt het beste plan voor uw specifieke aanvraag identificeren.

<!--

#### Migrating to Ping Identity or Auth0

In some cases, you may find that neither Azure AD's client credentials OAuth grant implementation is not sufficient to replace ACS in your architecture without major code changes.  Some common examples might include:

- server-to-server authentication using token formats other than JWTs.
- server-to-server authentication using an input token provided by an external identity provider.
- server-to-server authentication with token transformation rules that Azure AD cannot reproduce.

In these cases, you may want to consider migrating your web application to another cloud authentication service. We recommend exploring the following options, as each provides capabilities similar to ACS:

- [Auth0](https://auth0.com/) has created [high-level migration guidance for customers of ACS](https://auth0.com/blog/windows-azure-acs-alternative-replacement/), and provides a feature-by-feature comparison of ACS vs. Auth0.
- Enterprise customers should consider [Ping Identity](https://www.pingidentity.com) as well. Reach out to us and we can connect you with a representative from Ping who is prepared to help identify potential solutions.

Our aim in working with Ping Identity & Auth0 is to ensure that all ACS customers have a path forward for their apps & services that minimizes the amount of work required to move off of ACS.

-->

## <a name="questions-concerns--feedback"></a>Vragen, problemen en Feedback

We begrijpen dat veel ACS-klanten een duidelijk migratiepad niet vinden wordt na het lezen van dit artikel en moet mogelijk bepaalde hulp en richtlijnen bij het bepalen van het juiste abonnement. Als u uw migratiescenario's & vragen behandeld wilt, laat u een opmerking op deze pagina.
