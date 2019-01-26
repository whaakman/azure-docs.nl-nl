---
title: Migreren van de Azure Access controleservice | Microsoft Docs
description: Meer informatie over de opties voor het verplaatsen van apps en services van de Azure Access Control Service (ACS).
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/03/2018
ms.author: celested
ms.reviewer: jlu, annaba, hirsin
ms.openlocfilehash: f472503d43d82209c765a331ba3922a2f25ca327
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55077060"
---
# <a name="how-to-migrate-from-the-azure-access-control-service"></a>Procedure: Migreren vanuit de Azure Access Control Service

Microsoft Azure Access Control Service (ACS), een service van Azure Active Directory (Azure AD), wordt op 7 November 2018 beëindigd. Toepassingen en services die momenteel gebruikmaken van toegangsbeheer moeten volledig zijn gemigreerd naar een ander verificatiemechanisme dan. Dit artikel beschrijft de aanbevelingen voor huidige klanten, als u van plan bent uw gebruik van Access Control afschaffen. Als u toegangsbeheer op dit moment niet gebruikt, moet u geen enkele actie ondernemen.

## <a name="overview"></a>Overzicht

Toegangsbeheer is een cloudservice voor verificatie met een eenvoudige manier om te verifiëren en autoriseren van gebruikers om toegang te krijgen tot uw web-apps en services. Hiermee kunt veel functies van verificatie en autorisatie voor moet rekening worden gehouden van uw code. Access Control wordt voornamelijk gebruikt door ontwikkelaars en architecten van Microsoft .NET-clients, ASP.NET-webtoepassingen en webservices van Windows Communication Foundation (WCF).

Gebruiksvoorbeelden voor toegangsbeheer kunnen worden onderverdeeld in drie hoofdcategorieën:

- Verificatie bij bepaalde Microsoft-cloudservices, met inbegrip van Azure Service Bus- en Dynamics CRM. Clienttoepassingen verkrijgen-tokens van toegangsbeheer voor de verificatie bij deze services verschillende acties uit te voeren.
- Verificatie toevoegen voor webtoepassingen, zowel aangepaste als voorverpakte (zoals SharePoint). Met behulp van toegangsbeheer 'passieve' verificatie webtoepassingen aanmelden met een Microsoft-account (voorheen Live ID) en accounts van Google, Facebook, Yahoo, Azure AD kunnen ondersteunen en Active Directory Federation Services (AD FS).
- Aangepaste web-services beveiligen met behulp van tokens die zijn uitgegeven door Access Control. Met behulp van 'active'-verificatie, webservices ervoor zorgen dat ze alleen toegang tot bekende clients die zijn geverifieerd met toegangsbeheer toestaan.

Elk van deze use cases en hun aanbevolen migratie strategieën worden beschreven in de volgende secties.

> [!WARNING]
> Aanzienlijke codewijzigingen zijn in de meeste gevallen vereist voor het migreren van bestaande apps en services naar nieuwere technologieën. Het is raadzaam dat u direct beginnen met het plannen en uitvoeren van de migratie om te voorkomen dat alle mogelijke storingen of downtime.

Access Control heeft de volgende onderdelen:

- Een beveiligde token service (STS), die verificatieaanvragen ontvangt en verstrekt beveiligingstokens toesturen.
- De klassieke Azure portal, waar u hebt gemaakt, verwijderen, en inschakelen en uitschakelen van de Access Control-naamruimten.
- Een afzonderlijke Access Control-beheerportal, waar u aanpassen en Access Control-naamruimten te configureren.
- Een management-service, die u gebruiken kunt voor het automatiseren van de functies van de portals.
- Een token transformatie regel engine, die u gebruiken kunt voor het definiëren van complexe logica voor het bewerken van de indeling van de uitvoer van tokens die problemen met toegangsbeheer.

Voor het gebruik van deze onderdelen, moet u een of meer Access Control-naamruimten maken. Een *naamruimte* is een toegewezen exemplaar van toegangsbeheer voor uw toepassingen en services communiceert. Een naamruimte is geïsoleerd van alle andere Access Control-klanten. Andere Access Control-klanten hun eigen naamruimten gebruiken. Een naamruimte in de Access Control is een speciale URL er als volgt uitzien:

```HTTP
https://<mynamespace>.accesscontrol.windows.net
```

Alle communicatie met de STS en beheerbewerkingen moeten worden uitgevoerd op de volgende URL. U kunt verschillende paden gebruiken voor verschillende doeleinden. Controleren om te bepalen of uw toepassingen of services toegangsbeheer gebruiken, voor al het verkeer naar https://&lt;naamruimte&gt;. accesscontrol.windows.net. Al het verkeer naar deze URL wordt verwerkt door Access Control en moet worden stopgezet. 

De uitzondering hierop is al het verkeer naar `https://accounts.accesscontrol.windows.net`. Verkeer naar deze URL wordt al verwerkt door een andere service en **is niet** beïnvloed door de afschaffing van de Access Control. 

Zie voor meer informatie over toegangsbeheer [Access Control Service 2.0 (gearchiveerde)](https://msdn.microsoft.com/library/hh147631.aspx).

## <a name="find-out-which-of-your-apps-will-be-impacted"></a>Ontdek welke van uw apps wordt beïnvloed

Volg de stappen in deze sectie om erachter te komen welke van uw apps zullen worden beïnvloed door ACS buiten gebruik stellen.

### <a name="download-and-install-acs-powershell"></a>Downloaden en installeren van ACS-PowerShell

1. Ga naar de PowerShell Gallery en download [Acs.Namespaces](https://www.powershellgallery.com/packages/Acs.Namespaces/1.0.2).
1. De module installeren door te voeren

    ```powershell
    Install-Module -Name Acs.Namespaces
    ```

1. Haal een lijst van alle mogelijke opdrachten door te voeren

    ```powershell
    Get-Command -Module Acs.Namespaces
    ```

    Hulp op een specifieke opdracht uitvoeren:

    ```
     Get-Help [Command-Name] -Full
    ```
    
    waar `[Command-Name]` is de naam van de ACS-opdracht.

### <a name="list-your-acs-namespaces"></a>Lijst van de ACS-naamruimten

1. Verbinding maken met ACS met behulp van de **Connect AcsAccount** cmdlet.
  
    U moet mogelijk uitvoeren `Set-ExecutionPolicy -ExecutionPolicy Bypass` voordat u opdrachten kunt uitvoeren en de beheerder van deze abonnementen worden om de opdrachten uit te voeren.

1. Lijst van uw beschikbare Azure-abonnementen met behulp van de **Get-AcsSubscription** cmdlet.
1. Lijst met uw ACS-naamruimten op met de **Get-AcsNamespace** cmdlet.

### <a name="check-which-applications-will-be-impacted"></a>Controleren welke toepassingen worden beïnvloed

1. Gebruik van de naamruimte van de vorige stap en Ga naar `https://<namespace>.accesscontrol.windows.net`

    Bijvoorbeeld, als een van de naamruimten contoso-test is, gaat u naar `https://contoso-test.accesscontrol.windows.net`

1. Onder **vertrouwensrelaties**, selecteer **Relying party toepassingen** om te zien van de lijst met apps die zullen worden beïnvloed door ACS buiten gebruik stellen.
1. Herhaal stappen 1 en 2 voor alle andere ACS namespace(s) die u hebt.

## <a name="retirement-schedule"></a>De planning buiten gebruik stellen

Vanaf November 2017 zijn alle onderdelen van de Access Control volledig ondersteunde en operationele. De enige beperking is dat u [kan geen nieuwe naamruimten voor toegangsbeheer via de klassieke Azure portal maken](https://azure.microsoft.com/blog/acs-access-control-service-namespace-creation-restriction/).

Hier is de planning voor beëindigde Access Control-onderdelen:

- **November 2017**:  De Azure AD-beheerder-ervaring in de klassieke Azure portal [is buiten gebruik gesteld](https://blogs.technet.microsoft.com/enterprisemobility/2017/09/18/marching-into-the-future-of-the-azure-ad-admin-experience-retiring-the-azure-classic-portal/). Op dit moment, beheer van de naamruimte voor toegangsbeheer is beschikbaar op een nieuwe, specifieke URL: `https://manage.windowsazure.com?restoreClassic=true`. Deze URl gebruiken om te bekijken van uw bestaande naamruimten, inschakelen en uitschakelen van naamruimten en verwijderen van naamruimten, als u wilt.
- **2 april 2018**: De klassieke Azure portal is volledig buiten gebruik gesteld, wat betekent dat beheer van de Access Control-naamruimte is niet langer beschikbaar is via een URL. U kan niet op dit moment uitschakelen of inschakelen, verwijderen of het inventariseren van de Access Control-naamruimten. De Access Control-beheerportal zijn echter volledig functionele en zich op `https://\<namespace\>.accesscontrol.windows.net`. Alle andere onderdelen van Access Control blijft normaal functioneren.
- **7 november 2018**: Alle onderdelen van de Access Control zijn permanent afgesloten. Dit omvat de Access Control-beheerportal, de management-service, STS en het token transformatie regels-engine. Op dit moment aanvragen die worden verzonden naar de Access Control (dat zich bevindt in \<naamruimte\>. accesscontrol.windows.net) mislukken. U moet hebt gemigreerd alle bestaande apps en services met andere technologieën en eerder.

> [!NOTE]
> Een beleid schakelt naamruimten die een token voor een bepaalde tijd niet hebt aangevraagd. Vanaf begin September 2018 is deze periode is momenteel op 14 dagen van inactiviteit, maar dit zal worden ingekort tot 7 dagen van inactiviteit in de komende weken. Hebt u Access Control-naamruimten die momenteel zijn uitgeschakeld, kunt u [downloaden en installeren van ACS PowerShell](#download-and-install-acs-powershell) de namespace(s) opnieuw inschakelen.

## <a name="migration-strategies"></a>Migratiestrategieën

De volgende secties beschrijven op hoog niveau aanbevelingen voor het migreren van toegangsbeheer voor andere Microsoft-technologieën.

### <a name="clients-of-microsoft-cloud-services"></a>Clients van Microsoft-cloudservices

Ten minste één alternatieve vorm van verificatie biedt ondersteuning voor elke cloudservice van Microsoft die tokens die zijn uitgegeven door Access Control nu accepteert. Het juiste verificatiemechanisme varieert voor elke service. Het is raadzaam dat u naar de specifieke documentatie over voor elke service voor de officiële richtlijnen verwijzen. Voor het gemak elke set van documentatie is die hier beschikbaar zijn:

| Service | Richtlijnen |
| ------- | -------- |
| Azure Service Bus | [Migreren naar handtekeningen voor gedeelde toegang](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-migrate-acs-sas) |
| Azure Service Bus Relay | [Migreren naar handtekeningen voor gedeelde toegang](https://docs.microsoft.com/azure/service-bus-relay/relay-migrate-acs-sas) |
| Azure Managed Cache | [Migreren naar Azure Cache voor Redis](https://docs.microsoft.com/azure/azure-cache-for-redis/cache-faq#which-azure-cache-offering-is-right-for-me) |
| Azure DataMarket | [Migreren naar de Cognitive Services API 's](https://docs.microsoft.com/azure/machine-learning/studio/datamarket-deprecation) |
| BizTalk Services | [Migreren naar de functie logische Apps van Azure App Service](https://docs.microsoft.com/azure/machine-learning/studio/datamarket-deprecation) |
| Azure Media Services | [Migreren naar Azure AD-verificatie](https://azure.microsoft.com/blog/azure-media-service-aad-auth-and-acs-deprecation/) |
| Azure Backup | [De Azure Backup-agent bijwerken](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq) |

<!-- Dynamics CRM: Migrate to new SDK, Dynamics team handling privately -->
<!-- Azure RemoteApp deprecated in favor of Citrix: https://www.zdnet.com/article/microsoft-to-drop-azure-remoteapp-in-favor-of-citrix-remoting-technologies/ -->
<!-- Exchange push notifications are moving, customers don't need to move -->
<!-- Retail federation services are moving, customers don't need to move -->
<!-- Azure StorSimple: TODO -->
<!-- Azure SiteRecovery: TODO -->

### <a name="sharepoint-customers"></a>SharePoint-klanten

SharePoint 2013, 2016 en SharePoint Online-klanten hebben lang ACS gebruikt voor verificatiedoeleinden wordt gebruikt in de cloud, on-premises en hybride scenario's. Sommige functies van SharePoint en use-cases worden beïnvloed door ACS buiten gebruik stellen, terwijl andere gebruikers. De onderstaande tabel bevat een overzicht van hulp bij de migratie voor enkele van de meest populaire SharePoint die gebruikmaken van ACS functie:

| Functie | Richtlijnen |
| ------- | -------- |
| Verifiëren van gebruikers van Azure AD | Voorheen Azure AD bieden geen ondersteuning voor SAML 1.1-tokens vereist voor SharePoint voor verificatie en ACS is gebruikt als een intermediair die SharePoint compatibel met Azure AD-token indelingen. U kunt nu [SharePoint rechtstreeks verbinden met Azure AD met behulp van Azure AD App Gallery SharePoint op de lokale app](https://docs.microsoft.com/azure/active-directory/saas-apps/sharepoint-on-premises-tutorial). |
| [App-verificatie & server-naar-server-verificatie in SharePoint on-premises](https://technet.microsoft.com/library/jj219571(v=office.16).aspx) | Niet beïnvloed door ACS buiten gebruik stellen; Er zijn geen wijzigingen nodig. | 
| [Lage vertrouwensrelatie autorisatie voor SharePoint-invoegtoepassingen (provider die wordt gehost en SharePoint die wordt gehost)](https://docs.microsoft.com/sharepoint/dev/sp-add-ins/three-authorization-systems-for-sharepoint-add-ins) | Niet beïnvloed door ACS buiten gebruik stellen; Er zijn geen wijzigingen nodig. |
| [SharePoint cloud hybrid search](https://blogs.msdn.microsoft.com/spses/2015/09/15/cloud-hybrid-search-service-application/) | Niet beïnvloed door ACS buiten gebruik stellen; Er zijn geen wijzigingen nodig. |

### <a name="web-applications-that-use-passive-authentication"></a>Webtoepassingen die gebruikmaken van passieve verificatie

Voor webtoepassingen die gebruikmaken van toegangsbeheer voor gebruikersverificatie, biedt toegangsbeheer de volgende functies en mogelijkheden voor ontwikkelaars van webtoepassingen en -architecten:

- Diepe integratie met Windows Identity Foundation (WIF).
- Federatie met Google, Facebook, Yahoo, Azure Active Directory en AD FS-accounts en Microsoft-accounts.
- Ondersteuning voor de volgende verificatieprotocollen voor: OAuth 2.0-concept 13, WS-Trust en Web Services Federation (WS-Federation).
- Ondersteuning voor de volgende indelingen voor het token: JSON Webtoken (JWT), SAML 1.1, SAML 2.0 en Simple Web Token (SWT).
- Een home realm discovery-ervaring, geïntegreerd in WIF, waarmee gebruikers om op te halen van het type account waarmee ze zich aanmelden. Deze ervaring wordt gehost door de web-App en is volledig aanpasbaar.
- Token transformatie waarmee uitgebreide aanpassingen van de claims die zijn ontvangen door de web-App van Access Control, met inbegrip van:
    - Claims van id-providers passeren.
    - Toevoegen van aanvullende aangepaste claims.
    - Eenvoudige als-dan-logica voor het uitgeven van claims onder bepaalde omstandigheden.

Er is helaas niet één service die al deze equivalente mogelijkheden biedt. U moet evalueren welke mogelijkheden van Access Control u nodig hebt en kies vervolgens tussen het gebruik van [Azure Active Directory](https://azure.microsoft.com/develop/identity/signin/), [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) (Azure AD B2C), of een andere cloud-verificatie de service.

#### <a name="migrate-to-azure-active-directory"></a>Migreren naar Azure Active Directory

Een pad naar het overwegen integreert uw apps en services rechtstreeks met Azure AD. Azure AD is de cloud gebaseerde id-provider voor Microsoft werk- of schoolaccount. Azure AD is de id-provider voor Office 365, Azure en nog veel meer. Het biedt vergelijkbare federated authentication-mogelijkheden voor toegangsbeheer, maar biedt geen ondersteuning voor alle functies van de Access Control. 

Het primaire voorbeeld is een federatie met sociale id-providers, zoals Facebook, Google en Yahoo. Als uw gebruikers zich aanmelden met deze soorten referenties, wordt Azure AD is niet de oplossing voor u. 

Azure AD ook ondersteuning niet per se voor de exacte dezelfde verificatieprotocollen als toegangsbeheer. Bijvoorbeeld: Hoewel zowel Access Control en Azure AD OAuth ondersteunen, er zijn subtiele verschillen tussen elke implementatie. Verschillende implementaties moeten u code wijzigen als onderdeel van een migratie.

Azure AD biedt echter verschillende potentiële voordelen voor klanten van de Access Control. Deze systeemeigen ondersteunt Microsoft werk- of school-accounts die worden gehost in de cloud, die vaak worden gebruikt door klanten van de Access Control. 

Een Azure AD-tenant kan ook worden gefedereerd naar een of meer exemplaren van on-premises Active Directory via AD FS. Op deze manier uw app kunt verifiëren cloudgebruikers en gebruikers die zich on-premises gehost. Het ondersteunt ook het protocol WS-Federation, waardoor het betrekkelijk eenvoudig te integreren met een web-App met behulp van WIF.

De volgende tabel vergelijkt de functies van Access Control die relevant zijn voor webtoepassingen met de functies die beschikbaar in Azure AD zijn. 

Op hoog niveau, *Azure Active Directory is waarschijnlijk de beste keuze voor uw migratie als u toestaan dat gebruikers zich in alleen met hun Microsoft werk- of schoolaccount*.

| Mogelijkheid | Ondersteuning voor het beheer van toegang | Azure AD-ondersteuning |
| ---------- | ----------- | ---------------- |
| **Typen accounts** | | |
| Microsoft werk- of schoolaccount | Ondersteund | Ondersteund |
| Accounts van Windows Server Active Directory en AD FS |-Ondersteund via federatie met een Azure AD-tenant <br />-Ondersteund via directe Federatie met AD FS | Alleen ondersteund via federatie met een Azure AD-tenant | 
| Accounts van andere bedrijfssystemen voor het beheer van identiteit |-Mogelijk via federatie met een Azure AD-tenant <br />-Ondersteund via directe federation | Mogelijk via federatie met een Azure AD-tenant |
| Microsoft-accounts voor persoonlijk gebruik | Ondersteund | Ondersteund via de Azure AD v2.0 OAuth-protocol, maar niet via alle andere protocollen | 
| Facebook, Google, Yahoo-accounts | Ondersteund | Helemaal niet wordt ondersteund |
| **Protocollen en SDK compatibiliteit** | | |
| WIF | Ondersteund | Ondersteunde, maar beperkte instructies zijn beschikbaar |
| Webservices-federatie | Ondersteund | Ondersteund |
| OAuth 2.0 | Ondersteuning voor Draft 13 | Ondersteuning voor RFC 6749, de meeste moderne-specificatie |
| WS-Trust | Ondersteund | Niet ondersteund |
| **Token-indelingen** | | |
| JWT | Ondersteund In de bètafase | Ondersteund |
| SAML 1.1 | Ondersteund | Preview |
| SAML 2.0 | Ondersteund | Ondersteund |
| SWT | Ondersteund | Niet ondersteund |
| **Aanpassingen** | | |
| Aanpasbare thuisdomein detectie/account-opname-UI | Downloadbare code die kan worden opgenomen in apps | Niet ondersteund |
| Aangepaste certificaten voor token-ondertekening uploaden | Ondersteund | Ondersteund |
| Claims in tokens aanpassen |-Passeren invoerclaims van id-providers<br />-Toegangstoken ophalen uit de id-provider als een claim<br />-Uitvoerclaims op basis van waarden van invoer claims uitgeven<br />-Uitvoerclaims uitgeven met constante waarden |-Niet worden doorgegeven via de claims van federatieve id-providers<br />-Kan geen toegangstoken ophalen uit de id-provider als een claim<br />-Kan geen uitvoerclaims op basis van waarden van invoer claims uitgeven<br />-Kunnen uitvoerclaims worden uitgegeven met constante waarden<br />-Uitvoerclaims op basis van eigenschappen van gebruikers die zijn gesynchroniseerd met Azure AD kunnen geven |
| **Automation** | | |
| Configuratie en beheer taken automatiseren | Ondersteund via Access Control Management-Service | Ondersteund via Microsoft Graph en Azure AD Graph API |

Als u besluit dat Azure AD de beste migratiepad voor uw toepassingen en services is, moet u rekening houden met twee manieren om uw app integreren met Azure AD.

Als u WS-Federation of WIF wilt integreren met Azure AD, wordt aangeraden de benadering die worden beschreven in volgende [federatieve eenmalige aanmelding configureren voor een toepassing buiten de galerie](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-federated-sso-non-gallery). Het artikel verwijst naar Azure AD voor SAML gebaseerde eenmalige aanmelding configureren, maar werkt ook voor het configureren van WS-Federation. Deze benadering is een Azure AD Premium-licentie vereist. Deze benadering heeft twee voordelen:

- U krijgt de volledige flexibiliteit van Azure AD-token aanpassen. U kunt de claims die zijn uitgegeven door Azure AD zodat deze overeenkomen met de claims die zijn uitgegeven door Access Control aanpassen. Hierbij wordt met name de gebruikers-ID of naam id claim. Zorg ervoor dat de gebruikers-id's dat is uitgegeven door Azure AD-overeenkomst die zijn uitgegeven door toegangsbeheer om door te gaan voor het ontvangen van consistente gebruikers-id voor uw gebruikers na het wijzigen van technologieën.
- U kunt een certificaat voor token-ondertekening die specifiek is voor uw toepassing, en met een levensduur die u beheert configureren.

> [!NOTE]
> Deze aanpak is een Azure AD Premium-licentie vereist. Als u een Access Control-klant bent en u een premium-licentie nodig voor het instellen van eenmalige aanmelding voor een toepassing, contact met ons opnemen. We zult graag bieden ontwikkelaars licenties te gebruiken.

Een alternatieve methode is om te volgen [dit codevoorbeeld](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation), waardoor enigszins instructies voor het instellen van WS-Federation. Dit codevoorbeeld maakt geen gebruik WIF, maar in plaats daarvan de ASP.NET 4.5 OWIN-middleware. Echter, de instructies voor het app-registratie geldig zijn voor apps met behulp van WIF en vereisen een Azure AD Premium-licentie. 

Als u deze aanpak kiest, moet u begrijpen [ondertekening van sleutelrollover in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-signing-key-rollover). Deze methode maakt gebruik van de Azure AD globaal ondertekeningssleutel voor probleem-tokens. Standaard geeft WIF ondersteuningssleutels niet automatisch vernieuwd. Wanneer Azure AD de globale ondersteuningssleutels draait, moet uw toepassing met WIF worden voorbereid om de wijzigingen te accepteren. Zie voor meer informatie, [belangrijke informatie over het ondertekenen van sleutelrollover in Azure AD](https://msdn.microsoft.com/library/azure/dn641920.aspx).

Als u met Azure AD via de OpenID Connect of OAuth-protocollen integreren kunt, raden wij in dat geval. We hebben uitgebreide documentatie en richtlijnen over hoe u Azure AD integreren in uw web-App die beschikbaar zijn in onze [ontwikkelaarsgids van Azure AD](https://aka.ms/aaddev).

#### <a name="migrate-to-azure-active-directory-b2c"></a>Migreren naar Azure Active Directory B2C

De andere migratiepad rekening houden met is Azure AD B2C. Azure AD B2C is een cloudservice voor verificatie dat, zoals Access Control, kan ontwikkelaars uitbesteden de authenticatie en identiteit management logica in een cloud-service. Het is een betaalde service (met de lagen gratis en premium) die is ontworpen voor consumentgerichte toepassingen die mogelijk tot miljoenen actieve gebruikers.

Net zoals Access Control is een van de meest aantrekkelijke functies van Azure AD B2C dat deze ondersteuning biedt voor veel verschillende soorten accounts. Met Azure AD B2C, kunt u gebruikers aanmelden met hun Microsoft-account of Facebook, Google, LinkedIn, GitHub of Yahoo-accounts en meer. Azure AD B2C ondersteunt ook 'lokale accounts', of gebruikersnaam en wachtwoorden op dat gebruikers maken specifiek voor uw toepassing. Azure AD B2C biedt ook uitgebreide uitbreidbaarheid die u gebruiken kunt voor het aanpassen van uw stromen aanmelden. 

Echter, Azure AD B2C biedt geen ondersteuning voor de breedte van de protocollen voor verificatie en token opgemaakt dat toegangsbeheer toe klanten mogelijk. U niet Azure AD B2C ook gebruiken voor het ophalen van tokens en query's uitvoeren voor meer informatie over de gebruiker van de id-provider, Microsoft of anderszins.

De volgende tabel vergelijkt de functies van Access Control die relevant zijn voor webtoepassingen die die beschikbaar in Azure AD B2C zijn. Op hoog niveau, *Azure AD B2C is waarschijnlijk de juiste keuze voor uw migratie als uw toepassing is consumenten, of als het ondersteunt veel verschillende soorten accounts.*

| Mogelijkheid | Ondersteuning voor het beheer van toegang | Ondersteuning van Azure AD B2C |
| ---------- | ----------- | ---------------- |
| **Typen accounts** | | |
| Microsoft werk- of schoolaccount | Ondersteund | Ondersteund via aangepaste beleidsregels  |
| Accounts van Windows Server Active Directory en AD FS | Ondersteund via directe Federatie met AD FS | Ondersteund via SAML-federation met behulp van aangepaste beleidsregels |
| Accounts van andere bedrijfssystemen voor het beheer van identiteit | Ondersteund via directe federation via WS-Federation | Ondersteund via SAML-federation met behulp van aangepaste beleidsregels |
| Microsoft-accounts voor persoonlijk gebruik | Ondersteund | Ondersteund | 
| Facebook, Google, Yahoo-accounts | Ondersteund | Facebook en Google ondersteund, Yahoo ondersteund via OpenID Connect-Federatie met behulp van aangepaste beleidsregels |
| **Protocollen en SDK compatibiliteit** | | |
| Windows Identity Foundation (WIF) | Ondersteund | Niet ondersteund |
| Webservices-federatie | Ondersteund | Niet ondersteund |
| OAuth 2.0 | Ondersteuning voor Draft 13 | Ondersteuning voor RFC 6749, de meeste moderne-specificatie |
| WS-Trust | Ondersteund | Niet ondersteund |
| **Token-indelingen** | | |
| JWT | Ondersteund In de bètafase | Ondersteund |
| SAML 1.1 | Ondersteund | Niet ondersteund |
| SAML 2.0 | Ondersteund | Niet ondersteund |
| SWT | Ondersteund | Niet ondersteund |
| **Aanpassingen** | | |
| Aanpasbare thuisdomein detectie/account-opname-UI | Downloadbare code die kan worden opgenomen in apps | Volledig aanpasbare gebruikersinterface via aangepaste CSS |
| Aangepaste certificaten voor token-ondertekening uploaden | Ondersteund | Aangepaste ondertekeningssleutels, certificaten, ondersteund via aangepaste beleidsregels |
| Claims in tokens aanpassen |-Passeren invoerclaims van id-providers<br />-Toegangstoken ophalen uit de id-provider als een claim<br />-Uitvoerclaims op basis van waarden van invoer claims uitgeven<br />-Uitvoerclaims uitgeven met constante waarden |-Claims van id-providers, kunnen doorgeven aangepaste beleidsregels die zijn vereist voor sommige claims<br />-Kan geen toegangstoken ophalen uit de id-provider als een claim<br />-Uitvoerclaims op basis van waarden van invoerclaims via aangepaste beleidsregels kunnen geven<br />-Kunnen uitvoerclaims worden uitgegeven met constante waarden via aangepaste beleidsregels |
| **Automation** | | |
| Configuratie en beheer taken automatiseren | Ondersteund via Access Control Management-Service |-Het maken van gebruikers die zijn toegestaan via de Azure AD Graph API<br />-Kan geen maken B2C-tenants, toepassingen of beleid via een programma |

Als u besluit dat Azure AD B2C de beste migratiepad voor uw toepassingen en services is, kunt u beginnen met de volgende bronnen:

- [Documentatie voor Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview)
- [Azure AD B2C aangepast beleid](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview-custom)
- [Prijzen voor Azure AD B2C](https://azure.microsoft.com/pricing/details/active-directory-b2c/)

#### <a name="migrate-to-ping-identity-or-auth0"></a>Migreren naar Ping Identity of Auth0

In sommige gevallen kunt u zien dat Azure AD en Azure AD B2C zijn niet voldoende zijn voor het vervangen van Access Control in uw web-apps zonder dat belangrijke code hoeft te wijzigen. Enkele algemene voorbeelden hiervan zijn:

- Webtoepassingen die gebruikmaken van WIF of WS-Federation voor aanmelding met sociale id-providers, zoals Google of Facebook.
- Web-Apps die direct Federatie met een enterprise-id-provider via het protocol WS-Federation uitvoeren.
- Webtoepassingen waarvoor het toegangstoken dat is uitgegeven door een sociale id-provider (zoals Google of Facebook) als een claim in de tokens die zijn uitgegeven door Access Control.
- Webtoepassingen met complexe token transformatie regels die Azure AD of Azure AD B2C niet reproduceren.
- Webtoepassingen voor meerdere tenants die gebruikmaken van ACS federation op veel verschillende id-providers centraal te beheren

In dergelijke gevallen is het raadzaam om te overwegen voor het migreren van uw webtoepassing met een andere cloud authentication-service. Het is raadzaam om de volgende opties verkennen. Elk van de volgende opties bieden mogelijkheden die vergelijkbaar is met toegangsbeheer:

|     |     | 
| --- | --- |
| ![Auth0](./media/active-directory-acs-migration/rsz_auth0.png) | [Auth0](https://auth0.com/acs) is een flexibele cloud-identiteitsservice die gemaakt [op hoog niveau migratierichtlijn voor klanten van Access Control](https://auth0.com/acs), en biedt ondersteuning voor bijna elke functie die door ACS worden ondersteund. |
| ![Ping](./media/active-directory-acs-migration/rsz_ping.png) | [Ping Identity](https://www.pingidentity.com) biedt twee oplossingen die vergelijkbaar is met ACS. PingOne is een identiteitsservice in de cloud die ondersteuning biedt voor veel van de dezelfde functies als ACS en PingFederate is een vergelijkbare versie van on-premises identity product dat meer flexibiliteit biedt. Raadpleeg [Ping van ACS buiten gebruik stellen richtlijnen](https://www.pingidentity.com/en/company/blog/2017/11/20/migrating_from_microsoft_acs_to_ping_identity.html) voor meer informatie over het gebruik van deze producten. |

Ons doel bij het werken met Ping Identity en Auth0 is om ervoor te zorgen dat alle gebruikers van de Access Control een migratiepad voor hun apps en services die zo weinig mogelijk werk dat nodig is om te verplaatsen van Access Control hebben.

<!--

## SharePoint 2010, 2013, 2016

TODO: Azure AD only, use Azure AD SAML 1.1 tokens, when we bring it back online.
Other IDPs: use Auth0? https://auth0.com/docs/integrations/sharepoint.

-->

### <a name="web-services-that-use-active-authentication"></a>Webservices die gebruikmaken van active-verificatie

Voor web-services die zijn beveiligd met tokens die zijn uitgegeven door Access Control, biedt Access Control de volgende functies en mogelijkheden:

- Mogelijkheid om te registreren voor een of meer *service-identiteiten* in de Access Control-naamruimte. Service-identiteiten kunnen worden gebruikt om aan te vragen van tokens.
- Ondersteuning voor de OAuth verpakken en OAuth 2.0-concept 13 protocollen voor het aanvragen van tokens, met behulp van de volgende typen referenties:
    - Een eenvoudig wachtwoord dat voor de service-identiteit gemaakt
    - Een SWT met behulp van een symmetrische sleutel of X509 ondertekend certificaat
    - Een SAML-token dat is uitgegeven door een vertrouwde id-provider (normaal gesproken een exemplaar van AD FS)
- Ondersteuning voor de volgende indelingen voor het token: JWT, SAML 1.1, SAML 2.0 en SWT.
- Regels voor eenvoudige transformeren.

Service-identiteiten in de Access Control worden meestal gebruikt voor het implementeren van server naar server-verificatie. 

#### <a name="migrate-to-azure-active-directory"></a>Migreren naar Azure Active Directory

Onze aanbeveling voor dit type verificatie-stroom is om te migreren naar [Azure Active Directory](https://azure.microsoft.com/develop/identity/signin/). Azure AD is de cloud gebaseerde id-provider voor Microsoft werk- of schoolaccount. Azure AD is de id-provider voor Office 365, Azure en nog veel meer. 

U kunt ook Azure AD gebruiken voor de server naar server-verificatie met behulp van de Azure AD-implementatie van de OAuth-clientreferenties. De volgende tabel vergelijkt de mogelijkheden van Access Control van server naar server-verificatie met degenen die beschikbaar in Azure AD zijn.

| Mogelijkheid | Ondersteuning voor het beheer van toegang | Azure AD-ondersteuning |
| ---------- | ----------- | ---------------- |
| Informatie over het registreren van een webservice | Maken van een relying party in de Access Control-beheerportal | Een Azure AD-webtoepassing maken in Azure portal |
| Informatie over het registreren van een client | Maken van een service-identiteit in Access Control-beheerportal | Een andere Azure AD-webtoepassing maken in Azure portal |
| Protocol dat wordt gebruikt |-WRAP OAuth-protocol<br />-Referenties van de client OAuth 2.0-concept 13 verlenen | Referenties voor OAuth 2.0-client verlenen |
| Methoden voor clientverificatie |-Eenvoudig wachtwoord<br />-Ondertekende SWT<br />-SAML-token van een federatieve id-provider |-Eenvoudig wachtwoord<br />-JWT handtekening |
| Token-indelingen |- JWT<br />-SAML 1.1<br />-SAML 2.0<br />- SWT<br /> | Alleen JWT |
| Transformeren |-Aangepaste claims toevoegen<br />-Eenvoudige als-dan-claimuitgifte logica | Aangepaste claims toevoegen | 
| Configuratie en beheer taken automatiseren | Ondersteund via Access Control Management-Service | Ondersteund via Microsoft Graph en Azure AD Graph API |

Zie de volgende bronnen voor meer informatie over de uitvoerings-server-naar-server-scenario's:

- Service-naar-Service-sectie van de [ontwikkelaarsgids van Azure AD](https://aka.ms/aaddev)
- [Daemon-voorbeeldcode met behulp van clientreferenties eenvoudig wachtwoord](https://github.com/Azure-Samples/active-directory-dotnet-daemon)
- [Daemon-voorbeeldcode met behulp van de referenties van de certificaat-client](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential)

#### <a name="migrate-to-ping-identity-or-auth0"></a>Migreren naar Ping Identity of Auth0

In sommige gevallen kan het gebeuren dat de referenties van de Azure AD-client en de OAuth implementatie zijn niet voldoende zijn verleent voor het vervangen van Access Control in uw architectuur zonder primaire codewijzigingen. Enkele algemene voorbeelden hiervan zijn:

- Server-naar-server-verificatie met behulp van token indelingen dan JWTs.
- Server-naar-server-verificatie met behulp van een invoer token dat is opgegeven door een externe id-provider.
- Server-naar-server-verificatie met token transformatie regels die Azure AD niet reproduceren.

In dergelijke gevallen kunt u overwegen uw webtoepassing met een andere cloud-verificatieservice migreren. Het is raadzaam om de volgende opties verkennen. Elk van de volgende opties bieden mogelijkheden die vergelijkbaar is met toegangsbeheer:

|     |     | 
| --- | --- |
| ![Auth0](./media/active-directory-acs-migration/rsz_auth0.png) | [Auth0](https://auth0.com/acs) is een flexibele cloud-identiteitsservice die gemaakt [op hoog niveau migratierichtlijn voor klanten van Access Control](https://auth0.com/acs), en biedt ondersteuning voor bijna elke functie die door ACS worden ondersteund. |
| ![Ping](./media/active-directory-acs-migration/rsz_ping.png) | [Ping Identity](https://www.pingidentity.com) biedt twee oplossingen die vergelijkbaar is met ACS. PingOne is een identiteitsservice in de cloud die ondersteuning biedt voor veel van de dezelfde functies als ACS en PingFederate is een vergelijkbare versie van on-premises identity product dat meer flexibiliteit biedt. Raadpleeg [Ping van ACS buiten gebruik stellen richtlijnen](https://www.pingidentity.com/en/company/blog/2017/11/20/migrating_from_microsoft_acs_to_ping_identity.html) voor meer informatie over het gebruik van deze producten. |

Ons doel bij het werken met Ping Identity en Auth0 is om ervoor te zorgen dat alle gebruikers van de Access Control een migratiepad voor hun apps en services die zo weinig mogelijk werk dat nodig is om te verplaatsen van Access Control hebben.

#### <a name="passthrough-authentication"></a>Passthrough-verificatie

Er is geen equivalent Microsoft-technologie voor ACS voor passthrough-verificatie met een willekeurige token transformatie. Als dat nodig van uw klanten, mogelijk Auth0 wie de beste benadering-oplossing is.

## <a name="questions-concerns-and-feedback"></a>Vragen, opmerkingen en feedback

We begrijpen dat veel klanten van de Access Control niet worden gevonden bij een duidelijke migratiepad na het lezen van dit artikel. U moet mogelijk bepaalde hulp of hulp bij het bepalen van het juiste abonnement. Als u wilt om uw migratiescenario's en vragen te bespreken, plaatst u een opmerking op deze pagina.
