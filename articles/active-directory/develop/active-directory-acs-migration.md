---
title: Migreren van de service Azure Access Control | Microsoft Docs
description: Opties voor het verplaatsen van apps en services van de Azure Access Control-service
services: active-directory
documentationcenter: dev-center-name
author: dstrockis
manager: mtillman
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/14/2017
ms.author: dastrock
ms.openlocfilehash: f3de9016fe29a51ab2c7fb9e93fcd33af0f0e871
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="migrate-from-the-azure-access-control-service"></a>Migreren van de Azure Access Control-service

Azure Access Control, een service van Azure Active Directory (Azure AD), wordt in November 2018 buiten gebruik worden gesteld. Toepassingen en services die momenteel gebruikmaken van toegangsbeheer moeten volledig zijn gemigreerd naar een ander verificatiemechanisme dan. Dit artikel bevat aanbevelingen voor het huidige klanten, als u van plan bent uw gebruik van toegangsbeheer als vervangen markeren. Als u toegangsbeheer momenteel niet gebruikt, moet u geen geen actie te ondernemen.


## <a name="overview"></a>Overzicht

Toegangsbeheer is een cloudservice voor verificatie met biedt een eenvoudige manier om te verifiëren en autoriseren van gebruikers om toegang te krijgen tot uw webtoepassingen en services. Kunt u veel functies van verificatie en autorisatie voor moet rekening worden gehouden van uw code. Toegangsbeheer wordt voornamelijk gebruikt door ontwikkelaars en architecten van Microsoft .NET-clients, ASP.NET-webtoepassingen en webservices van Windows Communication Foundation (WCF).

Gebruiksvoorbeelden voor toegangsbeheer kunnen worden onderverdeeld in drie hoofdcategorieën:

- Verificatie van bepaalde Microsoft-cloudservices, met inbegrip van Azure Service Bus en Dynamics CRM. Clienttoepassingen verkrijgen tokens bij toegangsbeheer te verifiëren bij deze services verschillende acties uit te voeren.
- Verificatie toevoegen voor webtoepassingen, aangepaste en voorverpakte (zoals SharePoint). Met behulp van toegangsbeheer 'passieve' verificatie webtoepassingen aanmelden met een Microsoft-account (voorheen Live ID) en met accounts van Google, Facebook, Yahoo, Azure AD kunnen ondersteunen en Active Directory Federation Services (AD FS).
- De beveiliging van aangepaste webservices met tokens die zijn uitgegeven door toegangsbeheer. Met behulp van 'active' verificatie kunnen webservices ervoor te zorgen dat ze alleen toegang tot bekende clients die zijn geverifieerd met toegangsbeheer toestaan.

Elk van deze aanvragen en hun aanbevolen migratie strategieën worden beschreven in de volgende secties gebruiken. 

> [!WARNING]
> In de meeste gevallen worden de aanzienlijke codewijzigingen nodig voor het migreren van bestaande apps en services naar nieuwere technologieën. Het is raadzaam dat u meteen beginnen met het plannen en uitvoeren van de migratie om te voorkomen dat alle mogelijke storingen of uitvaltijd.

Toegangsbeheer heeft de volgende onderdelen:

- Een beveiligde token service (STS), waarmee verificatieaanvragen en verstrekt beveiligingstokens in.
- De klassieke Azure portal, waarin u maken, verwijderen, en inschakelen en uitschakelen van toegangsbeheer naamruimten.
- Een afzonderlijke Access Control-beheerportal, waar u aanpassen en Access Control-naamruimten te configureren.
- Een management-service, die u gebruiken kunt voor het automatiseren van de functies van de portals.
- Een token transformatie regel engine, die u gebruiken kunt voor het definiëren van complexe logica voor het bewerken van de indeling van de uitvoer van tokens die toegangsbeheer uitgeeft.

Voor het gebruik van deze onderdelen moet u een of meer Access Control-naamruimten. Een *naamruimte* is een toegewezen exemplaar van toegangsbeheer die uw toepassingen en services met communiceren. Een naamruimte is geïsoleerd van alle andere Access Control-klanten. Andere klanten toegangsbeheer gebruiken hun eigen naamruimten. Een Access Control-naamruimte heeft een toegewezen URL die er als volgt uit:

```HTTP
https://<mynamespace>.accesscontrol.windows.net
```

Alle communicatie met de STS- en beheerbewerkingen zijn gedaan bij deze URL. U kunt verschillende paden gebruiken voor verschillende doeleinden. Monitor om te bepalen of uw toepassingen of services toegangsbeheer gebruiken, al het verkeer naar https://\<naamruimte\>. accesscontrol.windows.net. Al het verkeer naar deze URL wordt verwerkt door de Access Control en moet worden stopgezet. 

De uitzondering hierop is al het verkeer naar https://accounts.accesscontrol.windows.net. Het verkeer naar deze URL al wordt verwerkt door een andere service en wordt niet beïnvloed door de afschaffing van toegangsbeheer. 

U moet ook aanmelden bij de klassieke Azure-portal en Controleer voor elke Access Control-naamruimten in de abonnementen waarvan u eigenaar. Access Control-naamruimten worden vermeld op de **Access Control-naamruimten** tabblad onder de **Active Directory** service.

Zie voor meer informatie over toegangsbeheer [Access Control Service 2.0 (gearchiveerd)](https://msdn.microsoft.com/library/hh147631.aspx).

## <a name="retirement-schedule"></a>De planning buiten gebruik stellen

Vanaf November 2017 zijn alle Access Control-onderdelen volledig ondersteunde en operationeel. De enige beperking is dat u [kan geen nieuwe toegangsbeheer naamruimten via de klassieke Azure portal maken](https://azure.microsoft.com/blog/acs-access-control-service-namespace-creation-restriction/).

Hier wordt het schema voor bestandstypen Access Control-onderdelen:

- **November 2017**: de Azure AD-beheerder optreden in de klassieke Azure portal [is buiten gebruik gesteld](https://blogs.technet.microsoft.com/enterprisemobility/2017/09/18/marching-into-the-future-of-the-azure-ad-admin-experience-retiring-the-azure-classic-portal/). Op dit moment naamruimte management voor toegangsbeheer is beschikbaar op een nieuwe, toegewezen URL: http://manage.windowsazure.com?restoreClassic=true. Gebruik deze URl weergeven van uw bestaande naamruimten, inschakelen en uitschakelen van naamruimten en verwijderen van naamruimten, als u wilt.
- **2018 april**: beheer van de Access Control-naamruimte is niet meer beschikbaar op de URL http://manage.windowsazure.com?restoreClassic=true toegewezen. U kan niet op dit moment uitschakelen of inschakelen, verwijderen of uw naamruimten toegangsbeheer inventariseren. De Access Control-beheerportal zijn echter volledig functionele en zich op https://\<naamruimte\>. accesscontrol.windows.net. Alle andere onderdelen van toegangsbeheer blijven werken normaal.
- **November 2018**: alle Access Control-onderdelen, permanent worden afgesloten. Dit omvat de Access Control-beheerportal, de beheerservice, STS en de token transformatie-engine voor regel. Op dit moment geen verzoeken verzonden naar de Access Control (te vinden op \<naamruimte\>. accesscontrol.windows.net) mislukken. U moet hebt gemigreerd alle bestaande apps en services voor andere technologieën voor deze tijd ook.


## <a name="migration-strategies"></a>Strategieën voor migratie

De volgende secties worden op hoog niveau aanbevelingen voor het migreren van toegangsbeheer voor andere Microsoft-technologieën.

### <a name="clients-of-microsoft-cloud-services"></a>Clients van Microsoft-cloudservices

Ten minste één andere vorm van verificatie biedt ondersteuning voor elke cloudservice van Microsoft die tokens die zijn uitgegeven door toegangsbeheer nu accepteert. Het juiste verificatiemechanisme varieert voor elke service. Het is raadzaam dat u de specifieke documentatie bij elke service voor de officiële verwijzen. Elke reeks documentatie is voor het gemak hier opgegeven:

| Service | Richtlijnen |
| ------- | -------- |
| Azure Service Bus | [Migreren naar handtekeningen voor gedeelde toegang](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-migrate-acs-sas) |
| Azure Service Bus Relay | [Migreren naar handtekeningen voor gedeelde toegang](https://docs.microsoft.com/azure/service-bus-relay/relay-migrate-acs-sas) |
| Azure Managed Cache | [Migreren naar Azure Redis-cache](https://docs.microsoft.com/azure/redis-cache/cache-faq#which-azure-cache-offering-is-right-for-me) |
| Azure DataMarket | [Migreren naar de cognitieve Services-API 's](https://docs.microsoft.com/azure/machine-learning/studio/datamarket-deprecation) |
| BizTalk Services | [Migreren naar de functie Logic Apps van Azure App Service](https://docs.microsoft.com/azure/machine-learning/studio/datamarket-deprecation) |
| Azure Media Services | [Migreren naar Azure AD-verificatie](https://azure.microsoft.com/blog/azure-media-service-aad-auth-and-acs-deprecation/) |
| Azure Backup | [Upgrade de Azure Backup agent](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq) |

<!-- Dynamics CRM: Migrate to new SDK, Dynamics team handling privately -->
<!-- Azure RemoteApp deprecated in favor of Citrix: http://www.zdnet.com/article/microsoft-to-drop-azure-remoteapp-in-favor-of-citrix-remoting-technologies/ -->
<!-- Exchange push notifications are moving, customers don't need to move -->
<!-- Retail federation services are moving, customers don't need to move -->
<!-- Azure StorSimple: TODO -->
<!-- Azure SiteRecovery: TODO -->


### <a name="web-applications-that-use-passive-authentication"></a>Webtoepassingen die passieve verificatie gebruiken

Voor webtoepassingen die toegangsbeheer voor gebruikersverificatie gebruiken, biedt toegangsbeheer op de volgende functies en mogelijkheden voor ontwikkelaars van webtoepassingen en -architecten:

- Diepe integratie met Windows Identity Foundation (WIF).
- Federatie met Google, Facebook, Yahoo, Azure Active Directory en AD FS-accounts en Microsoft-accounts.
- Ondersteuning voor de volgende verificatieprotocollen voor: OAuth 2.0-concept 13, WS-Trust en Web Services Federation (WS-Federation).
- Ondersteuning voor de volgende indelingen token: JSON Web Token (JWT), SAML 1.1 SAML 2.0 en eenvoudige Web Token (SWT).
- Een thuis realm detectie ervaring, geïntegreerd in WIF, waarmee gebruikers om op te halen van het type account die ze gebruiken voor aanmelden. Deze ervaring wordt gehost door de webtoepassing en geheel worden aangepast.
- Token transformatie waarmee uitgebreide aanpassing van de claims die zijn ontvangen door de webtoepassing van toegangsbeheer, met inbegrip van:
    - Claims van id-providers doorgeven.
    - Toevoegen van aanvullende aangepaste claims.
    - Eenvoudige als dan logica voor het verlenen van claims onder bepaalde omstandigheden.

Helaas niet één service biedt al deze mogelijkheden equivalent. Moet u evalueren welke mogelijkheden van toegangsbeheer u nodig hebt en kies vervolgens tussen het gebruik van [Azure Active Directory](https://azure.microsoft.com/develop/identity/signin/), [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) (Azure AD B2C) of een andere cloud-verificatie de service.

#### <a name="migrate-to-azure-active-directory"></a>Migreren naar Azure Active Directory

Een pad in overweging moet nemen is uw apps en services integreren rechtstreeks met Azure AD. Azure AD is de cloud-gebaseerde id-provider voor Microsoft werk of school accounts. Azure AD is de id-provider voor Office 365, Azure en nog veel meer. Het biedt vergelijkbare federated authentication-mogelijkheden voor toegangsbeheer, maar biedt geen ondersteuning voor alle functies voor toegangsbeheer. 

Het primaire voorbeeld is de Federatie met sociale id-providers, zoals Facebook, Google en Yahoo. Uw gebruikers zich aanmelden met deze typen referenties, is Azure AD niet als de oplossing voor u. 

Azure AD ondersteunt niet noodzakelijkerwijs de exacte dezelfde verificatieprotocollen als toegangsbeheer. Bijvoorbeeld: Hoewel zowel toegangsbeheer en Azure AD OAuth ondersteunen, er zijn subtiele verschillen tussen elke implementatie. Verschillende implementaties moeten u code wijzigen als onderdeel van een migratie.

Azure AD biedt echter verschillende mogelijke voordelen voor toegangsbeheer klanten. Deze systeemeigen ondersteunt Microsoft werk- of schoolaccounts gehost in de cloud, die vaak worden gebruikt door de Access Control-klanten. 

Een Azure AD-tenant kan ook op een of meer exemplaren van de lokale Active Directory via AD FS worden gefedereerd. Op deze manier uw app kunt cloud-gebaseerde gebruikers verifiëren en gebruikers die lokale gehost. Het ondersteunt ook het protocol WS-Federation, waardoor het betrekkelijk eenvoudig te integreren met een webtoepassing met behulp van WIF.

De volgende tabel vergelijkt de functies van toegangsbeheer die relevant zijn voor webtoepassingen met de functies die beschikbaar in Azure AD zijn. 

Op een hoog niveau *Azure Active Directory is waarschijnlijk de beste keuze voor uw migratie als u toestaat dat gebruikers zich aanmelden in alleen met hun Microsoft werk- of schoolaccounts*.

| Mogelijkheid | Access Control-ondersteuning | Ondersteuning van Azure AD |
| ---------- | ----------- | ---------------- |
| **Typen accounts** | | |
| Microsoft werk- of schoolaccounts | Ondersteund | Ondersteund |
| Accounts van Windows Server Active Directory en AD FS |-Ondersteund via federatie met Azure AD-tenant <br />-Ondersteund via directe Federatie met AD FS | Alleen ondersteund via federatie met Azure AD-tenant | 
| Accounts van andere bedrijfssystemen, identity management |-Mogelijke via federatie met Azure AD-tenant <br />-Ondersteund via directe federation | Mogelijk via federatie met Azure AD-tenant |
| Microsoft-accounts voor persoonlijk gebruik | Ondersteund | Ondersteund via de Azure AD v2.0 OAuth-protocol, maar niet via alle andere protocollen | 
| Facebook, Google, Yahoo accounts | Ondersteund | Dan ook ondersteund niet |
| **Protocollen en SDK compatibiliteit** | | |
| WIF | Ondersteund | Ondersteunde, maar in beperkte instructies zijn beschikbaar |
| WS-Federation | Ondersteund | Ondersteund |
| OAuth 2.0 | Ondersteuning voor concept 13 | Ondersteuning voor RFC 6749, de meeste moderne specificatie |
| WS-Trust | Ondersteund | Niet ondersteund |
| **Token indelingen** | | |
| JWT | Ondersteund In Beta | Ondersteund |
| SAML 1.1 | Ondersteund | Preview |
| SAML 2.0 | Ondersteund | Ondersteund |
| SWT | Ondersteund | Niet ondersteund |
| **Aanpassingen** | | |
| Aanpasbare thuisrealm gebruikersinterface van de account-detectie-verzamelen | Downloadbare code die kan worden opgenomen in apps | Niet ondersteund |
| Aangepaste certificaten voor token-ondertekening uploaden | Ondersteund | Ondersteund |
| Claims in tokens aanpassen |-Doorgeven invoerclaims van id-providers<br />-Toegangstoken ophalen uit de identiteitsprovider als claim<br />-Uitvoer-claims op basis van waarden van invoerbestand claims uitgeven<br />-Uitvoer claims uitgeven aan constante waarden |-Claims van federatieve identiteitsproviders kan niet doorgeven<br />-Kan geen toegangstoken ophalen uit de identiteitsprovider als claim<br />-Kan geen uitvoer claims op basis van waarden van invoerbestand claims uitgeven<br />-Kan uitvoer claims uitgeven aan constante waarden<br />-Uitvoer claims op basis van eigenschappen van de gebruikers gesynchroniseerd naar Azure AD kunnen worden verleend. |
| **Automatisering** | | |
| Configureren en beheren van taken automatiseren | Ondersteund via Access Control-Management-Service | Ondersteund via Microsoft Graph en Azure AD Graph API |

Als u besluit dat Azure AD het beste migratiepad voor uw toepassingen en services is, moet u zich bewust van twee methoden voor het integreren van uw app met Azure AD.

Als u WS-Federation of WIF wilt integreren met Azure AD, wordt aangeraden hieronder beschreven procedure in [configureren federatieve eenmalige aanmelding voor een toepassing niet galerie](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-federated-sso-non-gallery). Het artikel verwijst naar Azure AD voor op basis van SAML eenmalige aanmelding te configureren, maar werkt ook voor het configureren van WS-Federation. Deze aanpak volgt, is een Azure AD Premium-licentie vereist. Deze methode biedt twee voordelen:

- Krijgt u de volledige flexibiliteit van Azure AD-token aanpassen. U kunt de claims die zijn uitgegeven door Azure AD overeenkomen met de claims die zijn uitgegeven door toegangsbeheer aanpassen. Dit omvat met name de gebruiker-ID of id van de naam van de claim. Zorg ervoor dat de gebruikers-id door Azure AD overeenkomen met die zijn uitgegeven door toegangsbeheer uitgegeven om door te gaan consistent gebruiker-id's voor uw gebruikers ontvangen nadat u technologieën wijzigen.
- U kunt een certificaat voor token-ondertekening die specifiek is voor uw toepassing, en met een levensduur dat u beheert.

<!--

Possible nameIdentifiers from Access Control (via AAD or AD FS):
- AD FS - Whatever AD FS is configured to send (email, UPN, employeeID, what have you)
- Default from AAD using App Registrations, or Custom Apps before ClaimsIssuance policy: subject/persistent ID
- Default from AAD using Custom apps nowadays: UPN
- Kusto can't tell us distribution, it's redacted

-->

> [!NOTE]
> Deze aanpak is een Azure AD Premium-licentie vereist. Als u een Access Control-klant bent en u een premium-licentie nodig voor het instellen van eenmalige aanmelding voor een toepassing, contact met ons opnemen. We zult graag bieden developer-licenties moet worden gebruikt.

Een alternatieve methode is het volgen van [dit codevoorbeeld](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation), waardoor een iets andere instructies voor het instellen van WS-Federation. Dit voorbeeld gebruikt geen WIF, maar in plaats daarvan de middleware ASP.NET 4.5 OWIN. Echter de instructies voor het app-registratie zijn geldig voor apps met behulp van WIF en een Azure AD Premium-licentie niet nodig. 

Als u deze benadering kiest, moet u begrijpen [sleutelrollover in Azure AD-ondertekening](https://docs.microsoft.com/azure/active-directory/develop/active-directory-signing-key-rollover). Deze methode maakt gebruik van de Azure AD globaal ondertekeningssleutel voor het probleem van tokens. Standaard geeft WIF handtekeningsleutels niet automatisch vernieuwd. Wanneer u Azure AD de globale handtekeningsleutels draait, moet uw implementatie WIF worden voorbereid om de wijzigingen te accepteren.

Als u met Azure AD via het OpenID Connect of OAuth-protocol integreren kunt, raden wij doet. We hebben uitgebreide documentatie en richtlijnen over het Azure AD integreren in uw webtoepassing beschikbaar in onze [Ontwikkelaarshandleiding voor Azure AD](http://aka.ms/aaddev).

<!-- TODO: If customers ask about authZ, let's put a blurb on role claims here -->

#### <a name="migrate-to-azure-active-directory-b2c"></a>Migreren naar Azure Active Directory B2C

De andere migratiepad in overweging moet nemen is Azure AD B2C. Azure AD B2C wordt verificatie met een cloudservice waarmee, zoals Access Control ontwikkelaars uitbesteden hun management logica voor authenticatie en identiteit op een cloudservice. Dit is een betaalde service (met gratis en premium niveaus) die is ontworpen voor consumententoepassingen die mogelijk miljoenen actieve gebruikers.

Zoals toegangsbeheer, een van de meest aantrekkelijk functies van Azure AD B2C, is dat deze ondersteuning veel verschillende typen accounts biedt. Met Azure AD B2C, kunt u gebruikers aanmelden met hun Microsoft-account of Facebook, Google, LinkedIn, GitHub of Yahoo accounts en meer. Azure AD B2C ondersteunt ook 'lokale accounts', of de gebruikersnaam en wachtwoorden op dat gebruikers maken specifiek voor uw toepassing. Azure AD B2C biedt ook uitgebreide uitbreidbaarheid die u gebruiken kunt voor het aanpassen van uw stromen aanmelden. 

Echter, de breedte van de verificatieprotocollen biedt geen ondersteuning voor Azure AD B2C en token dat klanten mogelijk toegangsbeheer geformatteerd. U kan geen Azure AD B2C ook gebruiken om het ophalen van tokens en de query voor meer informatie over de gebruiker van de id-provider, Microsoft of anderszins.

De volgende tabel vergelijkt de functies van toegangsbeheer die relevant zijn voor webtoepassingen met degenen die beschikbaar in Azure AD B2C zijn. Op een hoog niveau *Azure AD B2C is waarschijnlijk de juiste keuze voor uw migratie als uw toepassing consumer geconfronteerd, of als er veel verschillende typen accounts ondersteunt.*

| Mogelijkheid | Access Control-ondersteuning | Ondersteuning van Azure AD B2C |
| ---------- | ----------- | ---------------- |
| **Typen accounts** | | |
| Microsoft werk- of schoolaccounts | Ondersteund | Ondersteund via een aangepast beleid  |
| Accounts van Windows Server Active Directory en AD FS | Ondersteund via directe Federatie met AD FS | Ondersteund via SAML-federation met behulp van aangepaste beleid |
| Accounts van andere bedrijfssystemen, identity management | Ondersteund via directe federation via WS-Federation | Ondersteund via SAML-federation met behulp van aangepaste beleid |
| Microsoft-accounts voor persoonlijk gebruik | Ondersteund | Ondersteund | 
| Facebook, Google, Yahoo accounts | Ondersteund | Facebook en Google ondersteund standaard, Yahoo ondersteund via OpenID Connect federation met behulp van aangepaste beleid |
| **Protocollen en SDK compatibiliteit** | | |
| Windows Identity Foundation (WIF) | Ondersteund | Niet ondersteund |
| WS-Federation | Ondersteund | Niet ondersteund |
| OAuth 2.0 | Ondersteuning voor concept 13 | Ondersteuning voor RFC 6749, de meeste moderne specificatie |
| WS-Trust | Ondersteund | Niet ondersteund |
| **Token indelingen** | | |
| JWT | Ondersteund In Beta | Ondersteund |
| SAML 1.1 | Ondersteund | Niet ondersteund |
| SAML 2.0 | Ondersteund | Niet ondersteund |
| SWT | Ondersteund | Niet ondersteund |
| **Aanpassingen** | | |
| Aanpasbare thuisrealm gebruikersinterface van de account-detectie-verzamelen | Downloadbare code die kan worden opgenomen in apps | Volledig aanpasbare UI via aangepaste CSS |
| Aangepaste certificaten voor token-ondertekening uploaden | Ondersteund | Aangepaste handtekeningsleutels, certificaten, ondersteund via een aangepast beleid |
| Claims in tokens aanpassen |-Doorgeven invoerclaims van id-providers<br />-Toegangstoken ophalen uit de identiteitsprovider als claim<br />-Uitvoer-claims op basis van waarden van invoerbestand claims uitgeven<br />-Uitvoer claims uitgeven aan constante waarden |-Claims van id-providers; kunt doorgeven aangepaste beleidsregels die zijn vereist voor sommige claims<br />-Kan geen toegangstoken ophalen uit de identiteitsprovider als claim<br />-Kunnen uitvoer claims op basis van waarden van invoerclaims via aangepaste beleidsregels voor uitgeven<br />-Kan uitvoer claims uitgeven aan constante waarden via aangepaste beleidsregels |
| **Automatisering** | | |
| Configureren en beheren van taken automatiseren | Ondersteund via Access Control-Management-Service |-Het maken van gebruikers toegestaan via Azure AD Graph API<br />-Kan geen maken B2C-tenants, toepassingen of beleidsregels programmatisch |

Als u besluit dat Azure AD B2C het beste migratiepad voor uw toepassingen en services is, kunt u beginnen met de volgende bronnen:

- [Azure AD B2C-documentatie](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview)
- [Azure AD B2C aangepast beleid](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview-custom)
- [Azure AD B2C-prijzen](https://azure.microsoft.com/pricing/details/active-directory-b2c/)


#### <a name="migrate-to-ping-identity-or-auth0"></a>Migreren naar Ping identiteit of Auth0

In sommige gevallen mogelijk vindt u die Azure AD en Azure AD B2C zijn niet voldoende zijn voor toegangsbeheer in uw webtoepassingen vervangen zonder dat u belangrijke code wijzigingen aanbrengt. Sommige algemene voorbeelden hiervan zijn:

- Webtoepassingen die gebruikmaken van WIF of WS-Federation voor aanmelden bij sociale id-providers zoals Google of Facebook.
- Webtoepassingen die direct federation aan een onderneming uitvoeren identificeren provider via het protocol WS-Federation.
- Webtoepassingen waarvoor het toegangstoken dat is uitgegeven door een identiteitsprovider van sociale (zoals Google of Facebook) als een claim in de tokens die zijn uitgegeven door toegangsbeheer.
- Webtoepassingen met complexe token transformatie regels die Azure AD of Azure AD B2C niet reproduceren.
- Multitenant webtoepassingen die gebruikmaken van ACS om Federatie af te veel andere identiteitsproviders centraal te beheren

In dergelijke gevallen wilt u mogelijk moet u uw webtoepassing op een andere cloud authentication service migreren. Het is raadzaam om de volgende opties te verkennen. Elk van de volgende opties bieden mogelijkheden die vergelijkbaar is met toegangsbeheer:



|     |     | 
| --- | --- |
| ![Auth0](./media/active-directory-acs-migration/rsz_auth0.png) | [Auth0](https://auth0.com/acs) is een flexibele cloud identity-service die is gemaakt [op hoog niveau migratie richtlijnen voor klanten van toegangsbeheer](https://auth0.com/acs), en ondersteunt bijna elke functie heeft van ACS. |
| ![Ping](./media/active-directory-acs-migration/rsz_ping.png) | [Ping-identiteit](https://www.pingidentity.com) biedt twee oplossingen die vergelijkbaar is met ACS. PingOne wordt een identiteit met een cloudservice die ondersteuning biedt voor veel van dezelfde functies als ACS en PingFederate is een vergelijkbaar on-premises identity product die meer flexibiliteit biedt. Raadpleeg [Ping de ACS buiten gebruik stellen richtlijnen](https://www.pingidentity.com/company/blog/2017/11/20/migrating_from_microsoft_acs_to_ping_identity.html) voor meer informatie over het gebruik van deze producten.  |

Ons doel voor het werken met Ping identiteits- en Auth0 is om ervoor te zorgen dat alle klanten van toegangsbeheer een migratiepad voor hun apps en services die minimaliseert de hoeveelheid werk dat nodig is voor het verplaatsen van toegangsbeheer.

<!--

## Sharepoint 2010, 2013, 2016

TODO: Azure AD only, use Azure AD SAML 1.1 tokens, when we bring it back online.
Other IDPs: use Auth0? https://auth0.com/docs/integrations/sharepoint.

-->

### <a name="web-services-that-use-active-authentication"></a>Webservices die gebruikmaken van active-verificatie

Voor de webservices die zijn beveiligd met tokens die zijn uitgegeven door toegangsbeheer, biedt Access Control de volgende functies en mogelijkheden:

- Mogelijkheid om te registreren voor een of meer *service-identiteiten* in uw Access Control-naamruimte. Service-identiteiten kunnen worden gebruikt voor het aanvragen van tokens.
- Ondersteuning voor de OAuth-INPAKKEN en OAuth 2.0-concept 13 protocollen voor het aanvragen van tokens, met behulp van de volgende typen referenties:
    - Een eenvoudig wachtwoord dat gemaakt voor de service-identiteit
    - Een SWT met behulp van een symmetrische sleutel of X509 ondertekend certificaat
    - Een SAML-token dat is uitgegeven door een vertrouwde identiteitsprovider (meestal een exemplaar van AD FS)
- Ondersteuning voor de volgende indelingen token: JWT, SAML 1.1 SAML 2.0 en SWT.
- Eenvoudige token transformatie-regels.

Service-identiteiten in de Access Control worden meestal gebruikt voor het implementeren van server naar server-verificatie.  

#### <a name="migrate-to-azure-active-directory"></a>Migreren naar Azure Active Directory

De aanbeveling voor dit type authenticatiestroom is om te migreren naar [Azure Active Directory](https://azure.microsoft.com/develop/identity/signin/). Azure AD is de cloud-gebaseerde id-provider voor Microsoft werk of school accounts. Azure AD is de id-provider voor Office 365, Azure en nog veel meer. 

U kunt ook Azure AD gebruiken voor verificatie van de server naar server met behulp van de Azure AD-implementatie van de OAuth-client referenties verlenen. De volgende tabel vergelijkt de mogelijkheden van toegangsbeheer in server-naar-server-verificatie met degenen die beschikbaar in Azure AD zijn.

| Mogelijkheid | Access Control-ondersteuning | Ondersteuning van Azure AD |
| ---------- | ----------- | ---------------- |
| Het registreren van een webservice | Maken van een relying party in de Access Control-beheerportal | Een Azure AD-webtoepassing maken in de Azure portal |
| Het registreren van een client | Maken van een service-identiteit in de Access Control-beheerportal | Een andere Azure AD-webtoepassing maken in de Azure portal |
| Protocol dat wordt gebruikt |-OAuth WRAP protocol<br />-De referenties van de client OAuth 2.0-concept 13 verlenen | OAuth 2.0-clientreferenties verlenen |
| Methoden voor clientverificatie |-Eenvoudig wachtwoord<br />-Ondertekende SWT<br />-SAML-token van een federatieve identiteiten-provider |-Eenvoudig wachtwoord<br />-Ondertekende JWT |
| Token indelingen |-JWT<br />-SAML 1.1<br />-SAML 2.0<br />-SWT<br /> | Alleen JWT |
| Token transformatie |-Aangepaste claims toevoegen<br />-Eenvoudig als dan claimuitgifte logica | Aangepaste claims toe te voegen | 
| Configureren en beheren van taken automatiseren | Ondersteund via Access Control-Management-Service | Ondersteund via Microsoft Graph en Azure AD Graph API |

Zie de volgende bronnen voor richtlijnen over scenario's voor server-naar-server geïmplementeerd:

- Service to Service-sectie van de [Ontwikkelaarshandleiding voor Azure AD](https://aka.ms/aaddev)
- [Voorbeeld van code daemon met behulp van de referenties van de client eenvoudig wachtwoord](https://github.com/Azure-Samples/active-directory-dotnet-daemon)
- [Voorbeeld van code daemon met behulp van de referenties van de certificaat-client](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential)

#### <a name="migrate-to-ping-identity-or-auth0"></a>Migreren naar Ping identiteit of Auth0

In sommige gevallen kan gebeuren dat de referenties van de Azure AD-client en de OAuth implementatie zijn niet voldoende zijn verlenen voor het vervangen van toegangsbeheer in uw architectuur zonder primaire codewijzigingen. Sommige algemene voorbeelden hiervan zijn:

- Server-naar-server-verificatie met behulp van token indelingen dan JWTs.
- Server-naar-server-verificatie met behulp van een invoer token dat is opgegeven door een externe id-provider.
- Server-naar-server-verificatie met token transformatie-regels die Azure AD niet reproduceren.

In dergelijke gevallen kunt u uw webtoepassing op een andere cloud authentication service migreren. Het is raadzaam om de volgende opties te verkennen. Elk van de volgende opties bieden mogelijkheden die vergelijkbaar is met toegangsbeheer:

|     |     | 
| --- | --- |
| ![Auth0](./media/active-directory-acs-migration/rsz_auth0.png) | [Auth0](https://auth0.com/acs) is een flexibele cloud identity-service die is gemaakt [op hoog niveau migratie richtlijnen voor klanten van toegangsbeheer](https://auth0.com/acs), en ondersteunt bijna elke functie heeft van ACS. |
| ![Ping](./media/active-directory-acs-migration/rsz_ping.png) | [Ping-identiteit](https://www.pingidentity.com) biedt twee oplossingen die vergelijkbaar is met ACS. PingOne wordt een identiteit met een cloudservice die ondersteuning biedt voor veel van dezelfde functies als ACS en PingFederate is een vergelijkbaar on-premises identity product die meer flexibiliteit biedt. Raadpleeg [Ping de ACS buiten gebruik stellen richtlijnen](https://www.pingidentity.com/company/blog/2017/11/20/migrating_from_microsoft_acs_to_ping_identity.html) voor meer informatie over het gebruik van deze producten.  |

Ons doel voor het werken met Ping identiteits- en Auth0 is om ervoor te zorgen dat alle klanten van toegangsbeheer een migratiepad voor hun apps en services die minimaliseert de hoeveelheid werk dat nodig is voor het verplaatsen van toegangsbeheer.

## <a name="questions-concerns-and-feedback"></a>Vragen, problemen en feedback

We begrijpen dat veel klanten voor toegangsbeheer een duidelijk migratiepad niet vinden na het lezen van dit artikel. Mogelijk moet u enkele hulp en richtlijnen bij het bepalen van het juiste abonnement. Als u uw migratiescenario's en vragen behandeld wilt, laat u een opmerking op deze pagina.
