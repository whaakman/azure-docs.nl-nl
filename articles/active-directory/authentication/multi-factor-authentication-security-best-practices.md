---
title: Richtlijnen voor beveiliging voor Azure multi-factor Authentication
description: Dit document bevat informatie over het gebruik van Azure MFA met Azure-accounts
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: 8b6ed6a7e71da2f302df4b41656c8a63e93be2b0
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39159070"
---
# <a name="security-guidance-for-using-azure-multi-factor-authentication-with-azure-ad-accounts"></a>Beveiligingsrichtlijnen voor het gebruik van Azure multi-factor Authentication met Azure AD-accounts

Verificatie in twee stappen is de beste keuze voor de meeste organisaties die willen hun verificatieproces verbeteren. Azure multi-factor Authentication (MFA) helpt bedrijven voldoen aan hun vereisten voor beveiliging en naleving terwijl er een eenvoudige aanmeldingsprocedure voor hun gebruikers. In dit artikel bevat een aantal tips waarmee u rekening houden moet bij het plannen van de acceptatie van Azure MFA.

## <a name="deploy-azure-mfa-in-the-cloud"></a>Azure MFA in de cloud implementeren

Er zijn twee manieren om [Azure MFA inschakelen voor al uw gebruikers](howto-mfa-getstarted.md).

* Kopen van licenties voor elke gebruiker (de Azure MFA, Azure AD Premium of Enterprise Mobility + Security)
* Maken van een multi-factor Authentication-Provider en betalen per gebruiker of per authenticatie

### <a name="licenses"></a>Licenties
![EMS](./media/multi-factor-authentication-security-best-practices/ems.png)

Als u Azure AD Premium of Enterprise Mobility + Security-licenties hebt, hebt u al Azure MFA. Uw organisatie hoeft niet verder niets meer om uit te breiden de functionaliteit van de verificatie in twee stappen voor alle gebruikers. U hoeft alleen een licentie toewijzen aan een gebruiker en vervolgens u MFA kunt inschakelen.

Bij het instellen van multi-factor Authentication, houd rekening met de volgende tips:

* Maak een per authenticatie multi-factor Authentication-Provider niet. Als u dit doet, kan het uiteindelijke betaalt voor aanvragen voor verificatie van gebruikers die al licenties hebt.
* Als u geen voldoende licenties voor al uw gebruikers hebt, kunt u een per gebruiker multi-factor Authentication-Provider voor de rest van uw organisatie maken. 
* Azure AD Connect is alleen vereist als u uw on-premises Active Directory-omgeving met Azure AD-adreslijst synchroniseert. Als u een Azure AD-directory die niet wordt gesynchroniseerd met een on-premises exemplaar van Active Directory gebruikt, hoeft u geen Azure AD Connect.

### <a name="multi-factor-auth-provider"></a>Multi-factor Authentication-Provider
![Multi-factor Authentication-Provider](./media/multi-factor-authentication-security-best-practices/authprovider.png)

Als u geen licenties met Azure MFA hebt, dan u kunt [een MFA-verificatieprovider maken](concept-mfa-authprovider.md).

Bij het maken van de Authentication-Provider, moet u een map selecteren en houd rekening met de volgende gegevens:

* U hoeft niet de Azure AD-adreslijst te maken van een multi-factor Authentication-Provider, maar krijgt u meer functionaliteit met een. De volgende functies zijn ingeschakeld wanneer u de verificatieprovider aan een Azure AD-directory koppelen:
  * Verificatie in twee stappen uitbreiden naar alle gebruikers
  * Extra functies, zoals de beheerportal, aangepaste begroetingen en rapporten voor uw globale beheerders bieden.
* Als u uw on-premises Active Directory-omgeving met Azure AD-adreslijst synchroniseert, moet u DirSync of AAD Sync. Als u een Azure AD-directory die niet wordt gesynchroniseerd met een on-premises exemplaar van Active Directory gebruikt, hoeft u niet DirSync of AAD Sync.
* Kies het verbruik model die het beste bij uw bedrijf. Wanneer u het gebruiksmodel selecteert, kunt u deze niet wijzigen. De twee modellen zijn:
  * Per verificatie: u in rekening gebracht voor elke verificatie. Dit model gebruiken als u wilt dat de verificatie in twee stappen voor iedereen die toegang heeft tot een bepaalde app, niet voor specifieke gebruikers.
  * Per ingeschakelde gebruiker: u in rekening gebracht voor elke gebruiker die u voor Azure MFA inschakelen. Dit model gebruiken als u een aantal gebruikers met Azure AD Premium of Enterprise Mobility Suite-licenties, en sommige zonder hebt.

### <a name="supportability"></a>Ondersteuning
Omdat de meeste gebruikers gewend zijn om te verifiëren met behulp van alleen wachtwoorden, is het belangrijk dat uw bedrijf awareness voor alle gebruikers met betrekking tot dit proces zorgt. Deze kennis kan verminderen de kans dat gebruikers contact opnemen met de helpdesk voor kleine problemen met betrekking tot MFA. Er zijn echter enkele scenario's tijdelijk uitschakelen MFA waar nodig is. Gebruik de volgende richtlijnen om te begrijpen hoe u deze scenario's:

* De technische ondersteuning voor het afhandelen van scenario's waarin de gebruiker kan niet aanmelden omdat de mobiele app of de telefoon niet een melding of automatische oproep ontvangen is trainen. Technische ondersteuning kunt [inschakelen van een eenmalig overslaan](howto-mfa-mfasettings.md#one-time-bypass) waarmee een gebruiker zich één keer verifiëren door 'overslaan' verificatie in twee stappen. De bypass is tijdelijk en verloopt na een opgegeven aantal seconden.
* Houd rekening met de [goedgekeurde IP-adressen mogelijkheid](howto-mfa-mfasettings.md#trusted-ips) in Azure MFA als een manier om te voorkomen van verificatie in twee stappen. Met deze functie kunnen beheerders van een tenant beheerd of federatieve verificatie in twee stappen voor gebruikers die vanaf het bedrijf lokaal intranet aanmelden zich omzeilen. De functies zijn beschikbaar voor Azure AD-tenants die Azure AD Premium, Enterprise Mobility Suite of Azure multi-factor Authentication-licenties hebt.

## <a name="best-practices-for-an-on-premises-deployment"></a>Aanbevolen procedures voor een on-premises implementatie
Als uw bedrijf besloten gebruikmaken van een eigen infrastructuur voor het inschakelen van MFA, dan u moet [implementeren van een Azure multi-factor Authentication-Server on-premises](howto-mfaserver-deploy.md). De MFA-Server-onderdelen worden weergegeven in het volgende diagram:

![MFA-Server-onderdelen standaard: console, synchronisatie-engine, -beheerportal, cloudservice](./media/multi-factor-authentication-security-best-practices/server.png) \*niet standaard geïnstalleerd \** geïnstalleerd maar niet standaard ingeschakeld.

Azure multi-factor Authentication-Server kunt cloud-resources en on-premises resources beveiligen met behulp van Federatie. U moet AD FS hebt en deze gefedereerd met uw Azure AD-tenant.
Bij het instellen van multi-factor Authentication-Server, houd rekening met de volgende gegevens:

* Als u bij het beveiligen van Azure AD-resources met behulp van Active Directory Federation Services (AD FS), en vervolgens de eerste verificatiestap wordt uitgevoerd on-premises met AD FS. De tweede stap wordt on-premises uitgevoerd door de claim toe te kennen.
* U hoeft niet te installeren van de Azure multi-factor Authentication-Server uw federatieve AD FS-server. De multi-factor Authentication-Adapter voor AD FS moet echter worden geïnstalleerd op een Windows Server 2012 R2 waarop AD FS wordt uitgevoerd. U kunt de server installeren op een andere computer, zolang het is een ondersteunde versie en de AD FS-adapter afzonderlijk installeren op uw federatieve AD FS-server. 
* De wizard multi-factor Authentication AD FS-Adapter installeren maakt een beveiligingsgroep met de naam PhoneFactor Admins in uw Active Directory en voegt vervolgens uw AD FS-serviceaccount toe aan deze groep. Controleer of de groep PhoneFactor Admins inderdaad is gemaakt op uw domeincontroller en of het AD FS-serviceaccount lid is van deze groep. Voeg, indien nodig, het AD FS-serviceaccount handmatig toe aan de groep PhoneFactor Admins in uw domeincontroller.

### <a name="user-portal"></a>Gebruikersportal
De gebruikersportal kunt selfservicemogelijkheden en biedt een volledige set met mogelijkheden voor het beheer van gebruiker. Deze wordt uitgevoerd in een Internet Information Server (IIS)-website. Gebruik de volgende richtlijnen voor het configureren van dit onderdeel:

* Gebruik IIS 6 of hoger
* Installeer en Registreer ASP.NET v2.0.507207
* Zorg ervoor dat deze server kan worden geïmplementeerd in een perimeternetwerk

### <a name="app-passwords"></a>Appwachtwoorden
Als uw organisatie is gefedereerd voor eenmalige aanmelding met Azure AD en u wilt gebruikmaken van Azure MFA, klikt u rekening houden met de volgende gegevens:

* Het app-wachtwoord wordt gecontroleerd door Azure AD en daarom omzeilt federation. Federatie wordt alleen gebruikt bij het instellen van app-wachtwoorden.
* Voor federatieve gebruikers (SSO), worden wachtwoorden opgeslagen in de organisatie-id. Als de gebruiker het bedrijf verlaat, heeft dat informatie naar de organisatie-id met behulp van DirSync stromen. Account uitschakelen/verwijderen kan tot drie uur duren om te synchroniseren die uitschakelen/verwijderen van app-wachtwoorden in Azure AD uitstelt.
* On-premises instellingen voor toegangsbeheer van client worden niet herkend door het app-wachtwoord.
* Geen on-premises verificatie logboekregistratie/controle mogelijkheid is beschikbaar voor app-wachtwoorden.
* Bepaalde geavanceerde ontwerpen van de architectuur mogelijk met behulp van een combinatie van organisatie-gebruikersnaam en wachtwoorden en app-wachtwoorden als u de verificatie in twee stappen voor clients, afhankelijk van waar ze worden geverifieerd. Voor clients die verificatie op basis van een on-premises infrastructuur, gebruikt u een organisatie-gebruikersnaam en wachtwoord. Voor clients die worden geverifieerd bij Azure AD, gebruikt u het app-wachtwoord.
* Gebruikers kunnen geen app-wachtwoorden maken standaard. Als u toestaan dat gebruikers wilt kunnen maken van app-wachtwoorden, selecteer de **gebruikers toestaan te maken van app-wachtwoorden aan te melden bij niet-browsertoepassingen** optie.

## <a name="additional-considerations"></a>Aanvullende overwegingen
Gebruik deze lijst voor aanvullende overwegingen en richtlijnen voor elk onderdeel dat on-premises geïmplementeerd:

- Azure Multi-Factor Authentication instellen met [Active Directory Federation Services](multi-factor-authentication-get-started-adfs.md).
- De Azure MFA-server instellen en configureren met [RADIUS-verificatie](howto-mfaserver-dir-radius.md).
- Instellen en configureren van de Azure MFA-Server met [IIS-verificatie](howto-mfaserver-iis.md).
- Instellen en configureren van de Azure MFA-Server met [Windows-verificatie](howto-mfaserver-windows.md).
- Instellen en configureren van de Azure MFA-Server met [LDAP-verificatie](howto-mfaserver-dir-ldap.md).
- Instellen en configureren van de Azure MFA-Server met [extern bureaublad-Gateway en Azure multi-factor Authentication-Server met behulp van RADIUS](howto-mfaserver-nps-rdg.md).
- Instellen en configureren van de synchronisatie tussen de Azure MFA-Server en [Windows Server Active Directory](howto-mfaserver-dir-ad.md).
- [Implementatie van de webservice voor mobiele apps van de Azure Multi-Factor Authentication-server](howto-mfaserver-deploy-mobileapp.md).
- [Geavanceerde VPN-configuratie met Azure multi-factor Authentication](howto-mfaserver-nps-vpn.md) voor Cisco ASA, Citrix Netscaler en Juniper/Pulse Secure VPN-apparaten met behulp van de LDAP-Adreslijst of RADIUS.

## <a name="next-steps"></a>Volgende stappen
In dit artikel worden enkele aanbevolen procedures voor Azure MFA gemarkeerd, maar er zijn andere bronnen die u ook gebruiken kunt bij het plannen van uw MFA-implementatie. De onderstaande lijst bevat enkele belangrijke artikelen die u tijdens dit proces helpen kunnen:

* [Rapporten in de Azure multi-factor Authentication](howto-mfa-reporting.md)
* [De verificatie in twee stappen verificatie registratie-ervaring](../user-help/multi-factor-authentication-end-user-first-time.md)
* [Veelgestelde vragen over Azure multi-factor Authentication](multi-factor-authentication-faq.md)
