---
title: Aanbevolen beveiligingsprocedures voor MFA | Microsoft Docs
description: Dit document bevat de aanbevolen procedures om het gebruik van Azure MFA met Azure-accounts
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: 3be7d968-96bb-4320-8701-869fd04a2595
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: joflore
ms.reviewer: richagi
ms.custom: it-pro
ms.openlocfilehash: 465b6cac8ac04b48d27890893c1a4459e652dc28
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="security-best-practices-for-using-azure-multi-factor-authentication-with-azure-ad-accounts"></a>Aanbevolen beveiligingsprocedures voor het gebruik van Azure multi-factor Authentication met Azure AD-accounts

Verificatie in twee stappen is de beste keuze voor de meeste organisaties die willen hun verificatieproces verbeteren. Azure multi-factor Authentication (MFA) helpt bedrijven die voldoen aan de vereisten voor beveiliging en naleving en tegelijkertijd een eenvoudige aanmeldingservaring voor gebruikers. In dit artikel bevat informatie over een aantal tips waarmee u rekening houden moet bij het plannen van de acceptatie van Azure MFA.

## <a name="deploy-azure-mfa-in-the-cloud"></a>Azure MFA in de cloud implementeren

Er zijn twee manieren Azure MFA inschakelen voor alle gebruikers.

* Licenties voor elke gebruiker (ofwel Azure MFA, Azure AD Premium of Enterprise Mobility + Security) kopen
* Een multi-factor Authentication-Provider en betalen per gebruiker of per authenticatie maken

### <a name="licenses"></a>Licenties
![EMS](./media/multi-factor-authentication-security-best-practices/ems.png)

Als u Azure AD Premium of Enterprise Mobility + Security licenties hebt, hebt u al Azure MFA. Uw organisatie nodig niet voor iets meer uit te breiden de functionaliteit van de verificatie in twee stappen voor alle gebruikers. U hoeft alleen een licentie toewijzen aan een gebruiker en klikt u vervolgens kunt u MFA.

Bij het instellen van multi-factor Authentication, kunt u de volgende tips:

* Maak geen een per authenticatie multi-factor Authentication-Provider. Als u dit doet, kan u betaalt voor aanvragen voor verificatie van gebruikers die al licenties terechtkomen.
* Als er niet voldoende licenties voor alle gebruikers, kunt u een per gebruiker multi-factor Authentication-Provider ten aanzien van de rest van uw organisatie. 
* Azure AD Connect is alleen vereist als u uw on-premises Active Directory-omgeving met een Azure Active directory synchroniseert. Als u een Azure AD-directory niet is gesynchroniseerd met een lokaal exemplaar van Active Directory gebruikt, hoeft u geen Azure AD Connect.

### <a name="multi-factor-auth-provider"></a>Multi-factor Authentication-Provider
![Multi-factor Authentication-Provider](./media/multi-factor-authentication-security-best-practices/authprovider.png)

Als er geen licenties met Azure MFA, kunt u een MFA Auth-Provider maken. 

Bij het maken van de Authentication-Provider, moet u een map selecteren en u kunt de volgende details:

* U hoeft geen Azure AD-adreslijst te maken van een multi-factor Authentication-Provider, maar u krijgt meer functionaliteit met een. De volgende functies zijn ingeschakeld wanneer u de Authentication-Provider aan een Azure Active directory koppelen:  
  * Verificatie in twee stappen uitbreiden naar alle gebruikers  
  * Uw globale beheerders aanvullende functies, zoals de beheerportal, aangepaste begroeting en rapporten bieden.
* Als u uw on-premises Active Directory-omgeving met een Azure Active directory synchroniseert, moet u de DirSync- of AAD Sync. Als u een Azure AD-directory niet is gesynchroniseerd met een lokaal exemplaar van Active Directory gebruikt, hoeft u niet DirSync of AAD Sync.
* Kies het verbruik model die het beste past bij uw bedrijf. Zodra u het gebruiksmodel selecteert, kunt u deze niet wijzigen. De twee modellen zijn:
  * Per verificatie: berekent u voor elke verificatie. Dit model gebruiken als u wilt dat de verificatie in twee stappen voor iedereen die toegang heeft tot een bepaalde app, niet voor specifieke gebruikers.
  * Per ingeschakelde gebruiker: berekent u voor elke gebruiker die u voor Azure MFA inschakelen. Dit model gebruiken als u sommige gebruikers met Azure AD Premium of Enterprise Mobility Suite licenties en andere zonder hebt.

### <a name="supportability"></a>Ondersteuning
Aangezien de meeste gebruikers worden vertrouwd te maken met alleen wachtwoorden gebruiken om te verifiëren, is het belangrijk dat u alleen uw bedrijf op voor alle gebruikers met betrekking tot dit proces. Deze afhankelijkheid kan verminderen de kans dat gebruikers contact opnemen met uw helpdesk voor kleine problemen met MFA. Er zijn echter enkele scenario's tijdelijk uitschakelen MFA waar nodig is. Gebruik de volgende richtlijnen om te begrijpen hoe voor het afhandelen van de scenario's:

* Training van uw technische ondersteuning voor het afhandelen van scenario's waarbij de gebruiker niet aanmelden omdat de mobiele app of de telefoon geen een melding of telefoongesprek ontvangt. Technische ondersteuning kunt [inschakelen eenmalig overslaan](multi-factor-authentication-whats-next.md#one-time-bypass) zodat een gebruiker één keer verifiëren door te 'negeren' verificatie in twee stappen. De bypass is tijdelijk en verloopt na een opgegeven aantal seconden.
* Houd rekening met de [goedgekeurde IP-adressen mogelijkheid](multi-factor-authentication-whats-next.md#trusted-ips) in Azure MFA als een manier om te minimaliseren verificatie in twee stappen. Met deze functie kunnen beheerders van een tenant beheerd of federatieve verificatie in twee stappen voor gebruikers die vanaf een lokaal intranet van het bedrijf aanmelden zich omzeilen. De functies zijn beschikbaar voor Azure AD-tenants die Azure AD Premium of Enterprise Mobility Suite Azure multi-factor Authentication-licenties hebt.

## <a name="best-practices-for-an-on-premises-deployment"></a>Aanbevolen procedures voor een on-premises implementatie
Als uw bedrijf besloten een eigen infrastructuur voor het inschakelen van MFA gebruiken, moet u een Azure multi-factor Authentication-Server on-premises implementeren. De MFA-Server-onderdelen worden weergegeven in het volgende diagram:

![Standaard MFA-Server-onderdelen: console, synchronisatie-engine, -beheerportal, cloudservice](./media/multi-factor-authentication-security-best-practices/server.png) \*niet standaard geïnstalleerd \** geïnstalleerd maar niet standaard is ingeschakeld

Azure multi-factor Authentication-Server kunt cloud bronnen en on-premises resources beveiligen met behulp van de federatieserver. U moet de AD FS hebt en laat het gefedereerd met Azure AD-tenant.
Bij het instellen van multi-factor Authentication-Server, houd rekening met de volgende details:

* Als u met Active Directory Federation Services (AD FS) beveiligen wilt, wordt de eerste verificatiestap is uitgevoerd voor Azure AD-resources on-premises met AD FS. De tweede stap wordt on-premises uitgevoerd door de claim toe te kennen.
* U hoeft niet te installeren van de Azure multi-factor Authentication-Server uw federatieve AD FS-server. De multi-factor Authentication-Adapter voor AD FS moet echter worden geïnstalleerd op een Windows Server 2012 R2 waarop AD FS wordt uitgevoerd. U kunt de server installeren op een andere computer, zolang het is een ondersteunde versie en de AD FS-adapter afzonderlijk installeren op uw federatieve AD FS-server. 
* De installatiewizard van de multi-factor Authentication AD FS-Adapter een beveiligingsgroep met de naam PhoneFactor Admins in uw Active Directory maakt en vervolgens uw AD FS-serviceaccount aan deze groep toegevoegd. Controleer of de groep PhoneFactor Admins inderdaad is gemaakt op uw domeincontroller en of het AD FS-serviceaccount lid is van deze groep. Voeg, indien nodig, het AD FS-serviceaccount handmatig toe aan de groep PhoneFactor Admins in uw domeincontroller.

### <a name="user-portal"></a>Gebruikersportal
De gebruikersportal kunt selfservice mogelijkheden en biedt een volledige set met mogelijkheden voor het beheer van gebruiker. Deze wordt uitgevoerd in een website van Internet Information Server (IIS). Gebruik de volgende richtlijnen voor het configureren van dit onderdeel:

* Gebruik IIS 6 of hoger
* Installeren en registreren van ASP.NET v2.0.507207
* Zorg ervoor dat deze server kan worden geïmplementeerd in een perimeternetwerk

### <a name="app-passwords"></a>Appwachtwoorden
Als uw organisatie is gefedereerd voor eenmalige aanmelding met Azure AD en u wilt gebruikmaken van Azure MFA, vervolgens mee worden van de volgende details:

* Het app-wachtwoord wordt gecontroleerd door Azure AD en wordt daarom overgeslagen federation. Federatie wordt alleen gebruikt bij het instellen van app-wachtwoorden.
* Voor federatieve gebruikers (SSO), worden wachtwoorden opgeslagen in de organisatie-id. Als de gebruiker het bedrijf verlaat, heeft dat gegevens naar de organisatie-id met behulp van DirSync stromen. Account uitschakelen/verwijderen kan tot drie uur duren om te synchroniseren die uitschakelen/verwijderen van app-wachtwoorden in Azure AD uitstelt.
* On-premises instellingen voor toegangsbeheer van client worden niet herkend door het app-wachtwoord.
* Er is geen mogelijkheid lokale verificatie logboekregistratie/controle is beschikbaar voor app-wachtwoorden.
* Bepaalde geavanceerde architectuur ontwerpen mogelijk met een combinatie van organisatie-gebruikersnaam en wachtwoorden app bij het gebruik van verificatie in twee stappen met clients, afhankelijk van waar ze verifiëren. Voor clients die worden geverifieerd bij een on-premises infrastructuur, gebruikt u een organisatie-gebruikersnaam en wachtwoord. Voor clients die worden geverifieerd bij Azure AD, gebruikt u het app-wachtwoord.
* Gebruikers kunnen geen app-wachtwoorden maken standaard. Als u dat gebruikers wilt kunnen maken van app-wachtwoorden, selecteer de **gebruikers toestaan te maken van app-wachtwoorden voor aanmelding bij niet-browsertoepassingen** optie.

## <a name="additional-considerations"></a>Aanvullende overwegingen
Gebruik deze lijst voor aanvullende overwegingen en richtlijnen voor elk onderdeel dat is geïmplementeerd op de lokale:

- Azure Multi-Factor Authentication instellen met [Active Directory Federation Services](multi-factor-authentication-get-started-adfs.md).
- De Azure MFA-server instellen en configureren met [RADIUS-verificatie](multi-factor-authentication-get-started-server-radius.md).
- Installeren en configureren van de Azure MFA-Server met [IIS-verificatie](multi-factor-authentication-get-started-server-iis.md).
- Installeren en configureren van de Azure MFA-Server met [Windows-verificatie](multi-factor-authentication-get-started-server-windows.md).
- Installeren en configureren van de Azure MFA-Server met [LDAP-verificatie](multi-factor-authentication-get-started-server-ldap.md).
- Installeren en configureren van de Azure MFA-Server met [extern bureaublad-Gateway en Azure multi-factor Authentication-Server met behulp van RADIUS](multi-factor-authentication-get-started-server-rdg.md).
- Installeren en configureren van de synchronisatie tussen de Azure MFA-Server en [Windows Server Active Directory](multi-factor-authentication-get-started-server-dirint.md).
- [Implementatie van de webservice voor mobiele apps van de Azure Multi-Factor Authentication-server](multi-factor-authentication-get-started-server-webservice.md).
- [Geavanceerde VPN-configuratie met Azure multi-factor Authentication](multi-factor-authentication-advanced-vpn-configurations.md) voor Cisco ASA, Citrix Netscaler en Juniper/Pulse Secure VPN-apparaten met behulp van de LDAP-Adreslijst of RADIUS.

## <a name="next-steps"></a>Volgende stappen
In dit artikel worden enkele aanbevolen procedures voor Azure MFA gemarkeerd, maar er zijn andere bronnen die u kunt ook tijdens het plannen van uw implementatie MFA gebruiken. De onderstaande lijst bevat enkele belangrijke artikelen die u tijdens dit proces helpen kunnen:

* [Rapporten in Azure multi-factor Authentication](multi-factor-authentication-manage-reports.md)
* [De ervaring in twee stappen verificatie registratie](multi-factor-authentication-end-user-first-time.md)
* [Veelgestelde vragen over Azure multi-factor Authentication](multi-factor-authentication-faq.md)

