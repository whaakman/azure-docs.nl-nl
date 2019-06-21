---
title: Een Azure Active Directory single sign-on-implementatie plannen
description: Handleiding voor het plannen, implementeren en beheren van eenmalige aanmelding in uw organisatie.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 05/22/2019
ms.author: baselden
ms.reviewer: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e5278d504c43688bf064b869982938db52b1b1bf
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67164943"
---
# <a name="plan-a-single-sign-on-deployment"></a>Een eenmalige aanmelding-implementatie plannen

Eenmalige aanmelding (SSO) betekent dat toegang tot alle toepassingen en bronnen van een gebruiker moet slechts eenmaal met behulp van een gebruikersaccount aanmeldt. Met eenmalige aanmelding, gebruikers toegang tot alle benodigde toepassingen zonder te moeten een tweede keer verifiëren.

## <a name="benefits-of-sso"></a>Voordelen van eenmalige aanmelding

Eenmalige aanmelding (SSO) voegt beveiliging en gemak wanneer gebruikers zich bij toepassingen in Azure Active Directory (Azure AD aanmelden). 

Veel organisaties zijn afhankelijk van de software als een service (SaaS)-toepassingen, zoals Office 365, Box en Salesforce, voor productiviteit van eindgebruikers. IT-personeel nodig in het verleden afzonderlijk maken en bijwerken van gebruikersaccounts in elke SaaS-toepassing en de gebruikers die nodig zijn om te onthouden van een wachtwoord voor elk.

De Azure Marketplace bevat meer dan 3000 toepassingen met vooraf geïntegreerde SSO-verbindingen, zodat u eenvoudig kunt ze integreren in uw tenant.

## <a name="licensing"></a>Licentieverlening

- **Azure AD-licenties** -eenmalige aanmelding voor vooraf geïntegreerde SaaS-toepassingen is gratis. Het aantal objecten in uw directory en de functies die u wilt implementeren, kan echter aanvullende licenties vereist. Zie voor een volledige lijst van de licentievereisten [prijzen Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).
- **Toepassingslicenties** -, moet u de juiste licenties voor uw SaaS-toepassingen om te voldoen aan de behoeften van uw bedrijf. Werken met de eigenaar van de toepassing om te bepalen of de gebruikers die zijn toegewezen aan de toepassing de juiste licenties voor hun rollen in de toepassing hebben. Als Azure AD de automatische inrichting op basis van rollen beheert, moeten de rol is toegewezen in Azure AD afgestemd op het aantal licenties die eigendom zijn in de toepassing. Onjuist aantal licenties die eigendom zijn van de toepassing kan leiden tot fouten tijdens het inrichten van/bijwerken van een gebruiker.

## <a name="plan-your-sso-team"></a>Plannen van uw team eenmalige aanmelding

- **De juiste belanghebbenden betrekken** : wanneer technologie mislukken projecten, het is normaal gesproken vanwege niet-overeenkomende verwachtingen op impact, resultaten en verantwoordelijkheden. Om te voorkomen dat deze valkuilen [ervoor te zorgen dat u de juiste belanghebbenden bent aantrekkelijke](https://aka.ms/deploymentplans) en dat belanghebbenden hun rollen begrijpt.
- **De communicatie plannen** -communicatie is essentieel dat u het succes van elke nieuwe service. Proactief te communiceren met uw gebruikers, maar hoe hun ervaringen verandert, wanneer deze wordt gewijzigd en hoe u ondersteuning krijgen wanneer ze problemen ondervinden. Bekijk de opties voor [hoe eindgebruikers krijgen toegang tot hun SSO toepassingen](end-user-experiences.md), en ze moesten maken uw communicatie zodat deze overeenkomt met uw selectie. 

## <a name="plan-your-sso-protocol"></a>Plan uw SSO-protocol

Een SSO-implementatie op basis van de federation-protocollen verbetert de beveiliging, betrouwbaarheid en eindgebruiker ervaringen en is eenvoudiger te implementeren. Veel toepassingen vooraf zijn geïntegreerd in Azure AD met [stap voor stap helpt beschikbaar](../saas-apps/tutorial-list.md). U kunt vinden op onze [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/). Gedetailleerde informatie over elke methode SSO vindt u in het artikel [eenmalige aanmelding voor toepassingen in Azure Active Directory](what-is-single-sign-on.md).

Er zijn twee primaire manieren waarin u uw gebruikers eenmalige aanmelding voor uw apps inschakelen kunt:

- **Met federatieve eenmalige aanmelding** Azure AD verifieert de gebruiker naar de toepassing met behulp van hun Azure AD-account. Deze methode wordt ondersteund voor toepassingen die ondersteuning voor protocollen, zoals SAML 2.0, WS-Federation en OpenID Connect, en is de uitgebreidste modus van eenmalige aanmelding. Het is raadzaam om met behulp van federatieve eenmalige aanmelding met Azure AD wanneer een toepassing wordt ondersteund, in plaats van eenmalige aanmelding op basis van wachtwoorden en AD FS.

- **Met wachtwoord gebaseerde eenmalige aanmelding** gebruikers zich aanmelden bij de toepassing met een gebruikersnaam en wachtwoord van de eerste keer ze het willen openen. Na de eerste aanmelding levert Azure AD de gebruikersnaam en het wachtwoord voor de toepassing. Wachtwoord gebaseerde eenmalige aanmelding kunt u beveiligde toepassingen wachtwoorden worden opgeslagen en opnieuw afspelen met behulp van een uitbreiding van web browser of mobiele app. Deze optie maakt gebruik van het bestaande aanmeldingsproces geleverd door de toepassing, kan een beheerder de wachtwoorden beheren en niet vereisen dat de gebruiker het wachtwoord kennen.

### <a name="considerations-for-federation-based-sso"></a>Overwegingen voor op basis van een federatieve SSO

- **Met behulp van OpenID Connect en OAuth** - als de toepassing waarmee u verbinding ondersteunt, de OIDC-/ OAuth 2.0-methode gebruiken om in te schakelen uw eenmalige aanmelding bij die toepassing. Deze methode is minder configuratie vereist en kunt een rijkere gebruikerservaring. Zie voor meer informatie, [OAuth 2.0](../develop/v2-oauth2-auth-code-flow.md), [OpenID Connect 1.0](../develop/v2-protocols-oidc.md), en [ontwikkelaarsgids van Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide).
- **Eindpunt-configuraties voor SAML gebaseerde SSO** -als u SAML gebruikt, moet uw ontwikkelaars specifieke informatie vóórdat u de toepassing. Zie voor meer informatie, [basic SAML-opties configureren](configure-single-sign-on-portal.md).
- **Certificaatbeheer voor SAML gebaseerde SSO** : wanneer u federatieve SSO inschakelen voor uw toepassing, Azure AD maakt u een certificaat dat standaard drie jaar geldig is. U kunt de vervaldatum voor dit certificaat, indien nodig aanpassen. Zorg ervoor dat er processen in voor het vernieuwen van certificaten voordat ze zijn verlopen. Zie voor meer informatie, [Azure AD beheren van certificaten](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs).

### <a name="considerations-for-password-based-sso"></a>Overwegingen voor eenmalige aanmelding op basis van wachtwoorden

Met behulp van Azure AD voor eenmalige aanmelding op basis van wachtwoorden is vereist voor het implementeren van een browserextensie die veilig worden de referenties ophalen en de aanmeldings-formulieren invullen. Definieer een mechanisme voor het implementeren van de extensie op schaal met [ondersteunde browsers](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction). Een aantal opties:

- [Groepsbeleid voor Internet Explorer](https://azure.microsoft.com/documentation/articles/active-directory-saas-ie-group-policy/)
- [System Center Configuration Manager (SCCM) voor Internet Explorer](https://docs.microsoft.com/sccm/core/clients/deploy/deploy-clients-to-windows-computers)
- [Door gebruiker gestuurd downloaden en de configuratie van Chrome, Firefox, Microsoft Edge of Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

Zie voor meer informatie, [hoe het configureren van wachtwoord één aanmelding](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-password-sso-non-gallery).

#### <a name="capturing-login-forms-metadata-for-applications-that-arent-in-the-gallery"></a>Vastleggen van aanmelding formulieren metagegevens voor toepassingen die zich niet in de galerie

Microsoft biedt ondersteuning voor metagegevens van het vastleggen van een webtoepassing voor wachtwoord vaulting (het vastleggen van de velden gebruikersnaam en wachtwoord). Navigeer naar de aanmeldings-URL tijdens het proces van het configureren van de toepassing om vast te leggen van de metagegevens van de formulieren. Vraag de eigenaar van de toepassing voor de exacte aanmeldings-URL. Deze informatie wordt gebruikt tijdens de aanmelding, Azure AD-referenties toe te wijzen aan de toepassing tijdens de aanmelding.

Zie voor meer informatie, [wat is toegang tot toepassingen en eenmalige aanmelding met Azure AD?: wachtwoord gebaseerde SSO](https://azure.microsoft.com/documentation/articles/active-directory-appssoaccess-whatis/).

#### <a name="indications-that-metadata-in-forms-needs-to-be-recaptured"></a>Vermeldingen die metagegevens in formulieren moet vrijmaken

Wanneer toepassingen de HTML-indeling wijzigen, moet u mogelijk offlinedefragmentatie van de metagegevens om aan te passen om de wijzigingen. Algemene problemen die aangeven dat de HTML-indeling wijzigen heeft zijn onder andere:

- Gebruikers melden dat te klikken op de toepassing "" in de aanmeldingspagina vastloopt
- Gebruikers melden dat de gebruikersnaam of het wachtwoord niet wordt ingevuld

#### <a name="shared-accounts"></a>Gedeelde accounts

Toepassingen met gedeelde accounts zijn niet vanuit het perspectief aanmelden verschilt van een galerie-toepassing die gebruikmaakt van SSO-wachtwoord voor individuele gebruikers. Er zijn echter enkele extra stappen die nodig zijn bij het plannen en configureren van een toepassing die zijn bedoeld voor gebruik van gedeelde accounts:

1. Werken met toepassing zakelijke gebruikers kunnen het volgende te documenteren:
   1. Instellen van gebruikers in de organisatie die gebruik van de toepassing maken gaan
   1. Bestaande set referenties in de toepassing die is gekoppeld aan de set gebruikers 
1. Voor elke combinatie van de gebruiker instellen en referenties, moet u een security group maken in de cloud of on-premises op basis van uw vereisten.
1. De gedeelde referenties opnieuw instellen. Zodra de app is geïmplementeerd in Azure AD, nodig niet personen het wachtwoord van het gedeelde account. Aangezien Azure AD, wordt het wachtwoord opslaan, kunt u instellen dat zeer lange en complexe. 
1. Configureer Automatische rollover van het wachtwoord als de toepassing wordt ondersteund. Op die manier kunnen zelfs de beheerder die de oorspronkelijke installatie hebt wordt het wachtwoord van het gedeelde account weten. 

## <a name="plan-your-authentication-method"></a>De verificatiemethode plannen

De juiste verificatiemethode kiezen is een belangrijke eerste besluit om een Azure AD-oplossing voor hybride identiteit in te stellen. De verificatiemethode die is geconfigureerd met behulp van Azure AD Connect, die ook gebruikers in de cloud bepalingen implementeren.

Als u een verificatiemethode, moet u rekening houden met de tijd, de bestaande infrastructuur, de complexiteit en de kosten van de implementatie van uw keuze. Deze factoren zijn voor elke organisatie en kunnen na verloop van tijd worden gewijzigd. U moet kiezen die het meest overeenkomt met uw specifieke scenario. Zie voor meer informatie, [kiest u de juiste verificatiemethode voor uw Azure Active Directory-oplossing voor hybride identiteit](https://docs.microsoft.com/azure/security/azure-ad-choose-authn).

## <a name="plan-your-security-and-governance"></a>Plan uw beveiliging en governance 

Identiteit is de nieuwe primaire pivot voor aandacht voor beveiliging en investeringen omdat netwerkverbindingen steeds poreuze en minder effectief aan de explosie van BYOD-apparaten geworden en toepassingen in de cloud. 

### <a name="plan-access-reviews"></a>Toegangsbeoordelingen plannen

[Toegangsbeoordelingen](https://docs.microsoft.com/azure/active-directory/governance/create-access-review) kunnen organisaties op efficiënte wijze groepslidmaatschappen beheren, toegang tot bedrijfstoepassingen en roltoewijzingen. U moet van plan bent om te controleren van gebruikerstoegang regelmatig om ervoor te zorgen dat alleen de juiste personen blijven toegang houden.

Sommige van de belangrijkste onderwerpen voor het plannen van tijdens het instellen van toegangsbeoordelingen zijn onder andere:

1. Identificeren van een uitgebracht voor toegangsbeoordelingen die past bij uw zakelijke behoeften. Dit kan zijn zo vaak als één keer per week, maandelijks, jaarlijks, of bij wijze van oefening op aanvraag.

1. Groepen die staan voor de revisoren van de app toegang tot rapporten maken. U moet ervoor zorgen dat belanghebbenden meest bekend zijn met de app en de Doelgebruikers en use-cases deelnemers in uw beoordelingen zijn

1. Een toegangscontrole voltooien bevat Blijf de app-machtigingen voor gebruikers die toegang niet langer nodig hebt. Plan voor het verwerken van aanvragen voor mogelijke ondersteuning van niet-toegestane gebruikers. Een verwijderde gebruiker blijft verwijderd uit Azure AD voor 30 dagen gedurende welke ze kunnen worden hersteld door een beheerder indien nodig. Na dertig dagen wordt die gebruiker definitief verwijderd. Met behulp van de Azure Active Directory-portal, kan een globale beheerder expliciet definitief verwijderen een onlangs verwijderde gebruiker voordat deze periode is bereikt.

### <a name="plan-auditing"></a>Controle van plan bent

Azure AD biedt [rapporten met technische en zakelijke inzichten](https://azure.microsoft.com/documentation/articles/active-directory-view-access-usage-reports/). 

Beveiligings- en activiteitenrapporten zijn beschikbaar. Beveiligingsrapporten weergeven voor gebruikers die zijn gemarkeerd voor risico's en riskante aanmeldingen. Activiteitenrapporten te begrijpen van het gedrag van gebruikers in uw organisatie doordat met gedetailleerde informatie over Aanmeldingsactiviteit en audittrails van alle aanmeldingen. Rapporten kunt u risico's beheren en controleren of de productiviteit kunt verhogen.

| Rapporttype | Toegangsbeoordeling | Beveiligingsrapporten | Rapport |
|-------------|---------------|------------------|----------------|
| Gebruiken om te controleren | Machtigingen van de toepassing en het gebruik. | Eventueel verdachte accounts | Wie heeft er toegang tot de toepassingen |
| Mogelijke acties | Toegang tot; controleren machtigingen intrekken | Intrekken van toegang; afdwingen dat beveiliging opnieuw instellen | Toegang intrekken |

Azure AD worden de meeste controle gegevens 30 dagen bewaard en zorgt ervoor dat de gegevens beschikbaar zijn via de Azure-beheerportal of via een API voor u om te downloaden naar uw analysesystemen.

### <a name="consider-using-microsoft-cloud-application-security"></a>Overweeg het gebruik van Microsoft Cloud Application Security

Microsoft Cloud App Security (MCAS) is een Cloud Access Security Broker (CASB)-oplossing. Dit geeft u inzicht in uw cloud-apps en services, biedt geavanceerde analyses uitvoeren waardoor identificeren en te bestrijden cyberdreigingen en kunt u bepalen hoe uw gegevens wordt verzonden.

MCAS implementeren, kunt u:

- Cloud Discovery gebruiken voor het toewijzen en identificeren van uw cloudomgeving en de cloud-apps die uw organisatie worden gebruikt.
- Goedkeuren en ongedaan maken-goedgekeurde apps in de cloud
- Eenvoudig te implementeren app-connectors die van de provider-API's, voor zichtbaarheid en beheer van apps waarmee u verbinding maken gebruikmaken met gebruik
- Bescherming van de App-beheer voor voorwaardelijke toegang gebruiken om op te halen van realtime zichtbaarheid en controle over de toegang en activiteiten binnen uw cloud-apps
- Kunt u doorlopend controle te instelling en aanpassen, beleid hebben.

Sessiebeheer voor Microsoft Cloud Application Security (MCAS) is beschikbaar voor elke browser op elk belangrijk platform op elk besturingssysteem. Mobiele apps en bureaublad-apps kunnen ook worden geblokkeerd of toegestaan. Door de systeemeigen integratie met Azure AD, alle apps die zijn geconfigureerd met SAML of Open ID Connect-apps met eenmalige aanmelding in Azure AD kunnen worden ondersteund, met inbegrip van [verschillende aanbevolen apps](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad).

Zie voor meer informatie over MCAS de [Microsoft Cloud App Security-overzicht](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security). MCAS is een abonnementservice-op basis van gebruikers. U kunt gedetailleerde licentiegegevens gegevens in de [MCAS licentieverlening gegevensblad](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE2NXYO).

### <a name="use-conditional-access"></a>Gebruik voorwaardelijke toegang

U kunt toegangsbeheer op basis van de criteria voor uw cloud-apps automatiseren met voorwaardelijke toegang.

Beleid voor voorwaardelijke toegang worden afgedwongen nadat de eerste factor-verificatie is voltooid. Voorwaardelijke toegang is daarom niet bedoeld als een eerste regel defense voor scenario's, zoals denial-of-service (DoS) aanvallen, maar signalen van deze gebeurtenissen kunt gebruiken om toegang te bepalen. Het niveau van aanmeldingsrisico bijvoorbeeld kan locatie van de aanvraag, enzovoort worden gebruikt. Zie voor meer informatie over voorwaardelijke toegang [het overzicht](https://docs.microsoft.com/azure/active-directory/conditional-access/plan-conditional-access) en de [implementatieplan](https://docs.microsoft.com/azure/active-directory/conditional-access/plan-conditional-access).

## <a name="azure-sso-technical-requirements"></a>Technische vereisten voor Azure eenmalige aanmelding

De volgende sectie bevat de vereisten voor het configureren van uw specifieke toepassing, met inbegrip van de benodigde environment(s), -eindpunten, claimtoewijzing, vereiste kenmerken, certificaten en protocollen die worden gebruikt. U moet deze informatie om te configureren van eenmalige aanmelding in de [Azure AD-portal](https://portal.azure.com/).

### <a name="authentication-mechanism-details"></a>Verificatiegegevens mechanisme

Microsoft biedt een zelfstudie voor alle vooraf geïntegreerde SaaS-apps, en hoeft u deze informatie. Als de toepassing zich niet in onze toepassingsmarketplace / galerie, moet u mogelijk de volgende soorten gegevens verzamelen:

- **Huidige id-provider de toepassing gebruikt voor eenmalige aanmelding, indien van toepassing** - bijvoorbeeld: AD FS, PingFederate, Okta
- **Protocollen die worden ondersteund door de doeltoepassing** : bijvoorbeeld SAML 2.0, OpenID Connect, OAuth en op formulieren gebaseerde verificatie, WS-Federation, WS-Trust
- **Protocol wordt geconfigureerd met Azure AD** - bijvoorbeeld SAML 2.0 of 1.1, OpenID Connect, OAuth, op basis van formulieren, WS-Federation

### <a name="attribute-requirements"></a>Vereisten voor kenmerk

Er is een vooraf geconfigureerde set kenmerken en kenmerktoewijzingen tussen Azure AD-gebruikersobjecten en gebruikersobjecten elke SaaS-app. Sommige apps beheren andere soorten objecten, zoals groepen. Plannen van de toewijzing van de kenmerken van de gebruiker uit Azure AD aan uw toepassing en [de standaard-kenmerktoewijzingen aanpassen](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes) op basis van uw bedrijf nodig heeft.

### <a name="certificate-requirements"></a>Vereisten voor certificaten

Het certificaat voor de toepassing up-to-date moet zijn, of er is een risico van gebruikers niet kunnen toegang krijgen tot de toepassing. De meeste SaaS-toepassingscertificaten zijn goede gedurende 36 maanden. U kunt deze duur van het certificaat in de toepassingsblade wijzigen. Zorg ervoor dat u de vervaldatum van het document en weet hoe u uw certificaatvernieuwing wilt beheren. 

Er zijn twee manieren om uw certificaten te beheren. 

- **Automatische certificaatrollover** -Microsoft biedt ondersteuning voor [ondertekening van sleutelrollover in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-signing-key-rollover). Dit is de voorkeursmethode voor het beheren van certificaten, ondersteunt niet alle ISV's in dit scenario.

- **Handmatig bijwerken van de** -elke toepassing heeft een eigen certificaat dat is verlopen op basis van hoe deze gedefinieerd. Voordat het certificaat van de toepassing is verlopen, een nieuw certificaat maken en te verzenden naar de ISV. Deze informatie kan worden opgehaald uit de federatiemetagegevens. [Meer informatie over federatiemetagegevens hier.](https://docs.microsoft.com/azure/active-directory/develop/active-directory-federation-metadata)

## <a name="implement-sso"></a>Eenmalige aanmelding implementeren

Gebruik de volgende stappen om te plannen en implementeren van uw oplossing in uw organisatie:

### <a name="user-configuration-for-sso"></a>Configuratie van de gebruiker voor eenmalige aanmelding

- **Identificatie van uw testgebruikers**

   Neem contact op met de app-eigenaar en vraag deze om te maken van een minimum van drie testgebruikers in de toepassing. Zorg ervoor dat de informatie die u als de primaire id gebruiken gaat juist is ingevuld en komt overeen met een kenmerk dat is beschikbaar in Azure AD. In de meeste gevallen wordt dit toewijzen aan de 'NameID' voor SAML-toepassingen. Voor JWT-tokens is de 'preferred_username."
   
   De gebruiker handmatig maken in Azure AD hetzij als een gebruiker op basis van een cloud of synchroniseren van de gebruiker van on-premises met de Azure AD Connect-synchronisatie-engine. Zorg ervoor dat de gegevens komt overeen met de claims worden verzonden naar de toepassing.

- **Eenmalige aanmelding configureren**

   Uit de [lijst met toepassingen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list), zoeken en openen van de zelfstudie eenmalige aanmelding voor uw toepassing en vervolgens van de zelfstudie stappen volgen voor het configureren van uw SaaS-toepassing is.

   Als u uw toepassing niet kunt vinden, Zie [documentatie aangepaste toepassing](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-federated-sso-non-gallery). Dit begeleidt u stapsgewijs door over het toevoegen van een toepassing die niet in de Azure AD-galerie bevindt zich.

   Desgewenst kunt u uitgegeven claims in het SAML-token voor de enterprise-toepassing met [documentatie van de richtlijnen van Microsoft](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping). Zorg ervoor dat deze wordt toegewezen aan wat u verwacht te ontvangen in het SAML-antwoord voor uw toepassing. Als u problemen tijdens de configuratie ondervindt, gebruikt u onze richtlijnen op [hoe u foutopsporing SSO-integratie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging).

Aangepaste toepassing voorbereiden is een functie van Azure AD Premium P1 of P2-licenties.

### <a name="provide-sso-change-communications-to-end-users"></a>Eenmalige aanmelding wijzigen communicatie bieden aan eindgebruikers

Implementeer uw communicatieplan. Zorg ervoor dat laat u uw gebruikers weten dat een wijziging afkomstig is, wanneer deze is aangekomen, wat u moet nu doen en hoe u hulp beroep te doen.

### <a name="verify-end-user-scenarios-for-sso"></a>Controleren of de eindgebruiker scenario's voor eenmalige aanmelding

U kunt de volgende Testscenario's gebruiken voor het uitvoeren van tests op apparaten in Bedrijfseigendom en persoonlijke apparaten om te controleren of de configuraties van de eenmalige aanmelding werkt zoals verwacht. De volgende scenario's wordt ervan uitgegaan dat een gebruiker te naar de URL van een toepassing navigeren is, en gaan via een verificatiestroom gestart door de serviceprovider (SP geïnitieerde auth-stroom).

| Scenario | Verwacht resultaat op SP geïnitieerde verificatiestroom door gebruiker |
|----------|---------------------------------------------------|
| Meld u aan voor de toepassing met Internet Explorer op corpnet. | Geïntegreerde Windows-verificatie (IWA) plaatsvindt zonder aanvullende vragen. |
| Meld u aan voor de toepassing met Internet Explorer tijdens een waarde buiten het bedrijfsnetwerk met nieuwe aanmeldingspoging. | Op basis van formulieren prompt op de AD FS-Server. Gebruiker zich heeft aangemeld en gevraagd voor MFA. |
| Meld u aan bij de toepassing met Internet Explorer tijdens een waarde buiten het bedrijfsnetwerk met een huidige sessie en er nooit MFA uitgevoerd. | Gebruiker ontvangt niet vragen om de eerste factor. Gebruiker ontvangt vragen voor MFA. |
| Meld u aan bij de toepassing met Internet Explorer tijdens een waarde buiten het bedrijfsnetwerk met een huidige sessie en MFA is al in deze sessie is uitgevoerd. | Gebruiker ontvangt niet vragen om de eerste factor. Gebruiker ontvangt geen MFA. Gebruiker SBF in toepassing. |
| Meld u aan bij de toepassing met Chrome/Firefox/Safari terwijl een waarde buiten het bedrijfsnetwerk met een huidige sessie en MFA is al in deze sessie is uitgevoerd. | Gebruiker ontvangt niet vragen om de eerste factor. Gebruiker ontvangt geen MFA. Eenmalige aanmelding van gebruiker in de toepassing. |
| Meld u aan bij in de toepassing met Chrome/Firefox/Safari terwijl een waarde buiten het bedrijfsnetwerk met nieuwe aanmeldingspoging. | Op basis van formulieren prompt op de AD FS-Server. Gebruiker zich heeft aangemeld en gevraagd voor MFA. |
| Meld u aan voor de toepassing met Chrome/Firefox terwijl op bedrijfsnetwerk door de huidige sessie. | Gebruiker ontvangt niet vragen om de eerste factor. Gebruiker ontvangt geen MFA. Eenmalige aanmelding van gebruiker in de toepassing. |
| Aanmelden bij de toepassing met de mobiele app van de toepassing met een nieuwe aanmeldingspoging. | Op basis van formulieren prompt op de AD FS-Server. Gebruiker zich heeft aangemeld en ADAL client vraagt voor MFA. |
| Niet-gemachtigde gebruiker probeert aan te melden bij de toepassing met de aanmeldings-URL. | Op basis van formulieren prompt op de AD FS-Server. Gebruiker niet aan te melden met de eerste factor. |
| Geautoriseerde gebruiker probeert om aan te melden, maar een onjuist wachtwoord invoert. | Gebruiker navigeert naar de URL van toepassing en onjuiste gebruikersnaam en wachtwoord fout ontvangt. |
| Geautoriseerde gebruiker klikt op de koppeling in een e-mailbericht en al is geverifieerd. | Gebruiker klikt op de URL en is aangemeld bij de toepassing zonder aanvullende vragen. |
| Geautoriseerde gebruiker klikt op de koppeling in een e-mailbericht en nog niet is geverifieerd. | Gebruiker klikt op de URL en vragen om te verifiëren met de eerste factor is. |
| Gebruiker zich aanmeldt bij toepassing met de mobiele app toepassing geautoriseerd (SP geïnitieerde) met een nieuwe aanmeldingspoging. | Op basis van formulieren prompt op de AD FS-Server. Gebruiker zich heeft aangemeld en ADAL client vraagt voor MFA. |
| Niet-gemachtigde gebruiker probeert aan te melden bij de toepassing met de aanmeldings-URL (op een Serviceprovider geïnitieerde). | Op basis van formulieren prompt op de AD FS-Server. Gebruiker niet aan te melden met de eerste factor. |
| Geautoriseerde gebruiker probeert om aan te melden, maar een onjuist wachtwoord invoert.| Gebruiker navigeert naar de URL van toepassing en onjuiste gebruikersnaam en wachtwoord fout ontvangt. |
| Geautoriseerde gebruiker meldt zich af en meldt u zich vervolgens. | Als de URL van de afmelding is geconfigureerd, worden gebruiker wordt afgemeld bij alle services en vragen om te verifiëren. |
| Geautoriseerde gebruiker meldt zich af en meldt u zich vervolgens. | Als de URL van de afmelding niet is geconfigureerd, worden automatisch gebruiker aangemeld weer in met behulp van bestaande token van de bestaande Azure AD-browsersessie. |
| Geautoriseerde gebruiker klikt op de koppeling in een e-mailbericht en al is geverifieerd. | Gebruiker klikt op de URL en is aangemeld bij de toepassing zonder aanvullende vragen. |
| Geautoriseerde gebruiker klikt op de koppeling in een e-mailbericht en nog niet is geverifieerd. | Gebruiker klikt op de URL en vragen om te verifiëren met de eerste factor is. |

## <a name="manage-sso"></a>Beheren van eenmalige aanmelding

In deze sectie geeft een overzicht van de vereisten en aanbevelingen voor het beheren van eenmalige aanmelding is.

### <a name="required-administrative-roles"></a>Beheerdersrollen vereist

Gebruik altijd de rol met de minste machtigingen beschikbaar zijn voor het uitvoeren van de vereiste taak in Azure Active Directory. Microsoft raadt aan [bekijken van de verschillende rollen die beschikbaar zijn](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) en kies de juiste is voor het oplossen van uw behoeften voor elke persona voor deze toepassing. Sommige functies mogelijk tijdelijk wordt toegepast en wordt verwijderd nadat de implementatie is voltooid.

| Persona| Rollen | Azure AD-rol (indien nodig) |
|--------|-------|-----------------------------|
| Helpdesk-beheerder helpen | Ondersteuning voor laag 1 | Geen |
| Beheer van identiteit | Configureren en fouten opsporen in wanneer problemen van invloed zijn op Azure AD | Globale beheerder |
| Beheerder van de toepassing | Attestation gebruiker in de toepassing, de configuratie op gebruikers met machtigingen | Geen |
| Infrastructuur voor beheerders | Rollover van de eigenaar van certificaat | Globale beheerder |
| Zakelijke eigenaar/belanghebbende | Attestation gebruiker in de toepassing, de configuratie op gebruikers met machtigingen | Geen |

Wordt u aangeraden [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) (PIM) voor het beheren van uw functies voor aanvullende controle, beheer en toegang voor gebruikers met directory-machtigingen te controleren.

### <a name="sso-certificate-lifecycle-management"></a>Levensduur van eenmalige aanmelding certificaten beheren

Het is belangrijk om te bepalen van de juiste rollen en e-maildistributielijsten belast zijn met het beheer van de levenscyclus van het handtekeningcertificaat tussen Azure AD en de toepassing die wordt geconfigureerd met eenmalige aanmelding. Hier volgen enkele van de belangrijkste functies die we raden u aan in plaats:

- De eigenaar voor het bijwerken van eigenschappen van gebruikers in de toepassing
- Eigenaar op-aanroep voor probleemondersteuning van toepassing
- De distributielijst nauw bewaakte e-mailadres voor de wijzigingsmeldingen certificaatfout

De maximale levensduur van een certificaat dat is drie jaar. Het is raadzaam tot stand brengen van een proces op hoe u een wijziging van het certificaat tussen Azure AD moet verwerken en uw toepassing. Dit kan helpen voorkomen of minimaliseren van een storing vanwege een certificaat is verlopen of certificaatrollover afdwingen.

### <a name="rollback-process"></a>Ongedaan maken

Nadat u het testen op basis van uw testcases hebt voltooid, is het tijd om naar productie met uw toepassing te verplaatsen. Verhogen naar productie betekent dat u uw geplande en geteste configuraties implementeren in uw productietenant en implementeer uw gebruikers. Het is echter belangrijk om te plannen wat te doen als uw distributie niet gaat u als gepland. Als de SSO-configuratie is mislukt tijdens de implementatie, moet u het beperken van een storing voordoet en de gevolgen voor uw gebruikers te begrijpen.

De beschikbaarheid van de ondersteunde verificatiemethoden in de toepassing bepaalt uw strategie voor aanbevolen. Controleer altijd of dat u hebt gedetailleerde documentatie voor app-eigenaren voor het terug te gaan naar de oorspronkelijke status van de aanmeldings-configuratie in het geval uw implementatie wordt uitgevoerd in problemen.

- **Als uw app meerdere id-providers ondersteunt**voor voorbeeld van de LDAP- en AD FS- en Ping, de bestaande configuratie van eenmalige aanmelding tijdens implementatie niet verwijderen. In plaats daarvan uitschakelen tijdens de migratie als u later weer overschakelen. 

- **Als uw app biedt geen ondersteuning voor meerdere id-providers** , maar Hiermee kunnen gebruikers zich aanmelden met formulieren gebaseerde verificatie (gebruikersnaam en wachtwoord), zorg ervoor dat gebruikers kunnen ook worden deze benadering in het geval de nieuwe implementatie van de SSO-configuratie is mislukt.

### <a name="access-management"></a>Toegangsbeheer

Het is raadzaam om een uitgebreide benadering kiezen bij het beheren van toegang tot bronnen. Algemene methoden omvatten met behulp van lokale groepen door te synchroniseren via Azure AD Connect, [dynamische groepen in Azure AD maken op basis van gebruikerskenmerken](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal), of het maken van [selfservicegroepen](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) in Azure AD beheerd door een resource-eigenaar.

### <a name="monitor-security"></a>Controleren van de beveiliging

We raadzaam instellen van een regelmatiger waarin u de verschillende aspecten van SaaS-app-beveiliging controleren en voer eventuele corrigerende acties die vereist zijn.

### <a name="troubleshooting"></a>Problemen oplossen

De volgende koppelingen bieden scenario's voor het oplossen van problemen. U wilt maken van een specifieke handleiding voor uw medewerkers waarin deze scenario's en de stappen om te corrigeren.

#### <a name="consent-issues"></a>Problemen met toestemming

- [Onverwachte toestemming fout](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-unexpected-user-consent-prompt)

- [Fout bij toestemming gebruiker](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-unexpected-user-consent-error)

#### <a name="sign-in-issues"></a>Aanmeldingsproblemen

- [Problemen met aanmelden vanaf een aangepaste portal](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-other-problem-deeplink)

- [Problemen met aanmelden vanaf toegangsvenster](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-other-problem-access-panel)

- [Fout bij de aanmeldingspagina van toepassing](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-application-error)

- [Fout opgetreden bij het in een Microsoft-toepassing](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-first-party-microsoft)

#### <a name="sso-issues-for-applications-listed-in-the-azure-application-gallery"></a>Problemen met eenmalige aanmelding voor toepassingen die worden vermeld in de galerie met Azure

- [Probleem met wachtwoord eenmalige aanmelding voor toepassingen die worden vermeld in de galerie met Azure](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-password-sso-gallery) 

- [Probleem met federatieve eenmalige aanmelding voor toepassingen die worden vermeld in de galerie met Azure](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-federated-sso-gallery)   

#### <a name="sso-issues-for-applications-not-listed-in-the-azure-application-gallery"></a>Problemen met eenmalige aanmelding voor toepassingen die niet wordt vermeld in de galerie met Azure

- [Probleem met wachtwoord eenmalige aanmelding voor toepassingen die niet wordt vermeld in de galerie met Azure](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-password-sso-non-gallery) 

- [Probleem met federatieve eenmalige aanmelding voor toepassingen die niet wordt vermeld in de galerie met Azure](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-federated-sso-non-gallery)

## <a name="next-steps"></a>Volgende stappen

[Fouten opsporen van op SAML gebaseerde SSO](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)

[Claimtoewijzing voor Apps via PowerShell](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping)

[In het SAML-token is uitgegeven claims aanpassen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)

[Protocol voor eenmalige SAML-aanmelding](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)

[SAML-protocol voor eenmalige afmelding](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-out-protocol-reference)

[Azure AD B2B](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) (voor externe gebruikers, zoals partners en leveranciers)

[Azure AD voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)

[Azure Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)

[Toegang via eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

[Zelfstudie over eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)
