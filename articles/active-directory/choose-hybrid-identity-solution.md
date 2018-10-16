---
title: Kies een Azure hybride identiteitsoplossing | Microsoft Docs
description: Krijg een basiskennis hebt van de beschikbare, hybride identiteitsoplossingen en aanbevelingen voor u het beste identity governance beslissing voor uw organisatie.
keywords: ''
author: jeffgilb
manager: mtillman
ms.reviewer: jsnow
ms.author: billmath
ms.date: 03/02/2018
ms.topic: article
ms.prod: ''
ms.service: azure
ms.technology: ''
ms.assetid: ''
ms.custom: it-pro
ms.openlocfilehash: 15a28cd5937be103aac888a5fc5485e39854a1b4
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49319945"
---
# <a name="microsoft-hybrid-identity-solutions"></a>Microsoft-oplossingen voor hybride identiteit

[Microsoft Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) hybride identiteitsoplossingen kunnen u on-premises directory-objecten synchroniseren met Azure AD en uw gebruikers on-premises nog steeds beheren. De eerste beslissing bij het plannen van uw on-premises Windows Server Active Directory synchroniseren met Azure AD is of u wilt gebruiken identiteit gesynchroniseerd of federatieve identiteit. Gesynchroniseerde identiteiten en, optioneel, wachtwoord-hashes, zodat uw gebruikers kunnen hetzelfde wachtwoord gebruiken voor toegang tot zowel on-premises en cloud-gebaseerde resources van de organisatie. Voor meer geavanceerde scenariovereisten, zoals eenmalige aanmelding (SSO) of on-premises MFA, moet u Active Directory Federation Services (AD FS) federeren identiteiten implementeren. 

Er zijn verschillende opties beschikbaar zijn voor het configureren van hybride identiteit. Dit artikel bevat informatie over het kiezen van de beste is voor uw organisatie op basis van eenvoudige implementatie en beheer van uw specifieke identiteits- en toegangsbeheer moet. Als u rekening houden met welk identiteitsmodel u het beste past bij de behoeften van uw organisatie, moet u ook om na te denken over tijd, bestaande infrastructuur, complexiteit en kosten. Deze factoren zijn verschillend voor elke organisatie en kunnen na verloop van tijd worden gewijzigd. Echter, als uw behoeften veranderen, u hebt ook de flexibiliteit om over te schakelen naar een andere identiteit-model.

> [!TIP]
> Deze oplossingen worden geleverd door [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).
>

## <a name="synchronized-identity"></a>Gesynchroniseerde identiteiten 

Gesynchroniseerde identiteit is de eenvoudigste manier om on-premises directory-objecten (gebruikers en groepen) synchroniseren met Azure AD. 

![Gesynchroniseerd voor hybride identiteit](./media/choose-hybrid-identity-solution/synchronized-identity.png)

Gesynchroniseerde identiteit is de eenvoudigste en snelste methode, wilt uw gebruikers toch behouden een afzonderlijk wachtwoord voor cloudgebaseerde bronnen. Om dit te voorkomen, kunt u ook (optioneel) [synchroniseren van een hash van gebruikerswachtwoorden](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-synchronization#what-is-password-synchronization) aan Azure Active directory. Synchronisatie van wachtwoord-hashes kan gebruikers zich aanmelden bij cloud-gebaseerde organisatieresources met dezelfde gebruikersnaam en wachtwoord dat ze on-premises gebruiken. Azure AD Connect controleert regelmatig of uw on-premises adreslijst voor wijzigingen en houdt uw Azure AD-directory worden gesynchroniseerd. Wanneer een gebruikerskenmerk of het wachtwoord gewijzigd on-premises Active Directory is, wordt deze automatisch bijgewerkt in Azure AD. 

Voor de meeste organisaties die alleen hun gebruikers zich willen aanmelden bij Office 365, SaaS-toepassingen en andere Azure AD gebaseerde-resources, is de standaardoptie voor de synchronisatie van wachtwoord wordt aanbevolen. Als dat niet voor u werkt, moet u kiezen tussen Pass through-verificatie en AD FS.

> [!TIP]
> Wachtwoorden van gebruikers worden opgeslagen in de on-premises Windows Server Active Directory in de vorm van een hashwaarde die aangeeft van de werkelijke gebruikerswachtwoord. Een hash-waarde is een resultaat van een eenzijdige wiskundige functie (het hash-algoritme). Er is geen methode om terug te zetten, het resultaat van een one-way function naar de versie van de tekst zonder opmaak van een wachtwoord. U kunt een wachtwoord-hash niet gebruiken voor het aanmelden bij uw on-premises netwerk. Wanneer u ervoor kiezen om te synchroniseren van wachtwoorden, wordt Azure AD Connect wachtwoord-hashes wordt geëxtraheerd uit de on-premises Active Directory en extra beveiliging voor de wachtwoord-hash worden verwerkt voordat deze wordt gesynchroniseerd met Azure AD is van toepassing. Wachtwoordsynchronisatie kan ook worden gebruikt samen met het terugschrijven van self-service voor wachtwoord opnieuw instellen in Azure AD inschakelen. Bovendien kunt u eenmalige aanmelding (SSO) inschakelen voor gebruikers op domein computers die zijn verbonden met het bedrijfsnetwerk bevinden. Met eenmalige aanmelding hoeft ingeschakelde gebruikers in te voeren van een gebruikersnaam voor veilige toegang tot cloud-bronnen. 
>

## <a name="pass-through-authentication"></a>Pass Through-verificatie

[Azure AD-Pass through-verificatie](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication) biedt een eenvoudig wachtwoord validatie-oplossing voor Azure op basis van een AD-services met behulp van uw on-premises Active Directory. Als beveiliging en naleving van beleid voor uw organisatie niet toestaan dat wachtwoorden van gebruikers, zelfs in een hash formulier verzenden, en u hoeft alleen ondersteuning voor desktop-SSO voor apparaten toegevoegd aan een domein, is het raadzaam dat u evalueren met behulp van Pass through-verificatie. Pass through-verificatie is niet vereist voor elke implementatie in het Perimeternetwerk, waardoor de implementatie-infrastructuur met AD FS in vergelijking eenvoudiger. Wanneer gebruikers zich aanmelden met behulp van Azure AD, evalueert deze verificatiemethode rechtstreeks in uw on-premises Active Directory-wachtwoorden van gebruikers.

![Pass Through-verificatie](./media/choose-hybrid-identity-solution/pass-through-authentication.png)

Met Pass through-verificatie, hoeft niet voor een complexe netwerkinfrastructuur en hoeft u voor het opslaan van on-premises wachtwoorden in de cloud. In combinatie met eenmalige aanmelding, pass-through-verificatie biedt een volledig geïntegreerde ervaring bij het aanmelden bij Azure AD of andere cloudservices.

Pass through-verificatie is geconfigureerd met Azure AD Connect, dat gebruikmaakt van een eenvoudig on-premises-agent die naar aanvragen voor wachtwoord-validatie luistert. De agent kan eenvoudig worden geïmplementeerd op meerdere machines om te zorgen voor hoge beschikbaarheid en taakverdeling. Aangezien alle communicatie alleen uitgaande zijn, is er geen vereiste voor de connector moet worden geïnstalleerd in een perimeternetwerk bevindt. De server-computer-vereisten voor de connector zijn als volgt:

- Windows Server 2012 R2 of hoger
- Toegevoegd aan een domein in het forest via welke gebruikers worden gevalideerd

## <a name="federated-identity-ad-fs"></a>Federatieve identiteiten (AD FS)

Voor meer controle over hoe gebruikers toegang Office 365 en andere cloudservices tot, kunt u instellen van directorysynchronisatie met het gebruik van eenmalige aanmelding (SSO) [Active Directory Federation Services (AD FS)](https://docs.microsoft.com/windows-server/identity/ad-fs/overview/whats-new-active-directory-federation-services-windows-server). Van uw gebruikers-aanmeldingen met AD FS federeert delegeert verificatie met een on-premises server die gebruikersreferenties worden gevalideerd. In dit model worden on-premises Active Directory-referenties nooit doorgegeven aan Azure AD.

![Federatieve identiteit](./media/choose-hybrid-identity-solution/federated-identity.png)

Ook wel identiteitsfederatie genoemd, met deze aanmeldingsmethode zorgt ervoor dat alle verificatie van de gebruiker beheerde on-premises en kan beheerders voor het implementeren van strengere niveaus van access control. Federatieve identiteit met AD FS is de meest complexe optie en vereist extra beheerservers in uw on-premises-omgeving implementeren. Identiteitsfederatie voert u ook tot het 24 / 7 ondersteuning bieden voor uw Active Directory en AD FS-infrastructuur. Deze hoge mate van ondersteuning is nodig omdat als uw on-premises Internet toegankelijk zijn, de domeincontroller of AD FS-servers niet beschikbaar zijn, gebruikers zich niet cloud services.

> [!TIP]
> Als u besluit het gebruik van Federatie met Active Directory Federation Services (AD FS), kunt u eventueel instellen van Wachtwoordsynchronisatie als een back-up als uw AD FS-infrastructuur is mislukt.
>

## <a name="common-scenarios-and-recommendations"></a>Veelvoorkomende scenario's en aanbevelingen

Hier volgen enkele algemene hybride identiteit en toegang management scenario's met betrekking tot welke hybride identiteit optie (of opties) geschikt voor elk zijn kunnen aanbevelingen.

|Ik wil:|PWS en SSO<sup>1</sup>| PTA en SSO<sup>2</sup> | AD FS<sup>3</sup>|
|-----|-----|-----|-----|
|Nieuwe gebruiker, contactpersonen en groepsaccounts die zijn gemaakt in mijn on-premises Active Directory naar de cloud worden gesynchroniseerd.|![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png)| ![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png) |![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Instellen van mijn tenant voor Office 365 hybride scenario 's|![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png)| ![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png) |![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Mijn gebruikers zich aanmelden en toegang tot cloudservices met behulp van hun on-premises wachtwoord inschakelen|![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png)| ![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png) |![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Implementeren van eenmalige aanmelding met bedrijfsreferenties|![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png)| ![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png) |![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Zorg ervoor dat er geen wachtwoord-hashes worden opgeslagen in de cloud| |![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png)|![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Oplossingen voor on-premises meervoudige verificatie inschakelen| | |![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Ondersteuning voor smartcard-verificatie voor Mijn gebruikers<sup>4</sup>| | |![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Wachtwoord verlopen meldingen worden weergegeven in de Office-Portal en op het bureaublad van Windows 10| | |![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png)|

> <sup>1</sup> Wachtwoordsynchronisatie met eenmalige aanmelding.
>
> <sup>2</sup> Pass through-verificatie en eenmalige aanmelding. 
>
> <sup>3</sup> federatieve eenmalige aanmelding met AD FS.
>
> <sup>4</sup> AD FS kunnen worden geïntegreerd met uw enterprise PKI te kunnen aanmelden met behulp van certificaten. Deze certificaten kunnen voorlopig-certificaten die zijn geïmplementeerd via vertrouwde inrichting kanalen, zoals certificaten voor MDM of GPO of smartcard (inclusief PIV/CAC-kaarten) of Hello voor bedrijven (cert-vertrouwensrelatie) zijn. Zie voor meer informatie over ondersteuning voor smartcard-verificatie, [deze blog](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/).
>

## <a name="next-steps"></a>Volgende stappen

[Meer informatie in een omgeving met Azure Proof of Concept](https://aka.ms/aad-poc)
[Installeer Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771)
[hybride identiteitssynchronisatie bewaken](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health)
