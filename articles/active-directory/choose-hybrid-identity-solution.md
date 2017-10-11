---
title: Een Azure hybride identiteit-oplossing kiezen | Microsoft Docs
description: Basiskennis van de beschikbare hybride identiteitsoplossingen en aanbevelingen voor u de beste beslissing identiteit beheeracties voor uw organisatie kunt ophalen.
keywords: 
author: jeffgilb
manager: femila
ms.reviewer: jsnow
ms.author: jeffgilb
ms.date: 7/5/2017
ms.topic: article
ms.prod: 
ms.service: azure
ms.technology: 
ms.assetid: 
ms.custom: it-pro
ms.openlocfilehash: 5838e3276765f4f074bca2e3cae81b17edfa7c69
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="microsoft-hybrid-identity-solutions"></a>Microsoft-oplossingen voor hybride identiteit
[Microsoft Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) hybride identiteitsoplossingen kunnen u on-premises mapobjecten synchroniseren met Azure AD tijdens nog steeds het beheren van uw gebruikers on-premises. De eerste beslissing die te maken bij het plannen van uw lokale Windows Server Active Directory synchroniseren met Azure AD is of u wilt gebruiken identiteit gesynchroniseerd of federatieve identiteit. Gesynchroniseerde identiteiten en, optioneel wachtwoord-hashes, kunnen uw gebruikers op hetzelfde wachtwoord voor toegang tot zowel on-premises en organisatie-cloudresources. Voor meer geavanceerde scenariovereisten op, zoals het eenmalige aanmelding (SSO) of lokale MFA, moet u Active Directory Federation Services (AD FS) voor het federeren van identiteiten implementeren. 

Er zijn diverse opties beschikbaar voor het configureren van hybride identiteit. Dit artikel bevat informatie over het kiezen van de beste is voor uw organisatie op basis van een eenvoudig te implementeren en uw specifieke identiteits- en toegangsbeheer-beheer moet. Als u welk identiteitsmodel u het beste past bij de behoeften van uw organisatie overweegt, moet u ook nadenken over tijd, de bestaande infrastructuur, de complexiteit en kosten. Deze factoren zijn voor elke organisatie anders en na verloop van tijd kunnen wijzigen. Als uw vereisten veranderen, wel u ook de flexibiliteit om over te schakelen naar een andere identiteit-model.

> [!TIP]
> Deze oplossingen worden geleverd door [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

## <a name="synchronized-identity"></a>Gesynchroniseerde identiteiten 
Gesynchroniseerde identiteit is de eenvoudigste manier om on-premises mapobjecten (gebruikers en groepen) te synchroniseren met Azure AD. 

![Gesynchroniseerde hybride identiteit](./media/choose-hybrid-identity-solution/synchronized-identity.png)

Gesynchroniseerde identiteiten is de eenvoudigste en snelste methode, steeds uw gebruikers nog een afzonderlijk wachtwoord voor cloud-gebaseerde bronnen onderhouden. Om dit te voorkomen, kunt u ook (optioneel) [een hash van gebruikerswachtwoorden synchroniseren](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-synchronization#what-is-password-synchronization) aan uw Azure AD-directory. Synchroniseren van de wachtwoord-hashes kan gebruikers zich aanmelden bij cloud-gebaseerde organisatieresources met dezelfde gebruikersnaam en wachtwoord dat ze lokale gebruiken. Azure AD Connect controleert periodiek uw on-premises directory op wijzigingen en houdt uw Azure AD-directory worden gesynchroniseerd. Wanneer een gebruikerskenmerk of het wachtwoord gewijzigd op lokale Active Directory is, wordt deze automatisch bijgewerkt in Azure AD. 

Voor de meeste organisaties die alleen willen hun gebruikers in staat aan te melden bij Office 365, SaaS-toepassingen en andere Azure AD gebaseerde-resources, is de standaardoptie voor de synchronisatie van wachtwoord wordt aanbevolen. Als dat niet werkt, moet u kiezen tussen Pass through-verificatie en AD FS.

> [!TIP]
> Wachtwoorden van gebruikers worden opgeslagen in de lokale Windows Server Active Directory in de vorm van een hashwaarde die het werkelijke gebruikerswachtwoord vertegenwoordigt. Een hash-waarde is een resultaat van een wiskundige eenrichtingsfunctie (hash-algoritme). Er is geen methode om het resultaat van een eenrichtingsfunctie naar de versie van de tekst zonder opmaak van een wachtwoord terug te zetten. U kunt een wachtwoord-hash niet gebruiken aan te melden bij uw on-premises netwerk. Wanneer u ervoor kiezen wachtwoorden synchroniseren, wordt Azure AD Connect wachtwoord-hashes geëxtraheerd uit de lokale Active Directory en van toepassing is extra beveiliging verwerking naar de wachtwoordhash voordat deze wordt gesynchroniseerd naar Azure AD. Wachtwoordsynchronisatie kan ook worden gebruikt samen met terugschrijven van wachtwoord selfservice wachtwoordherstel in Azure AD inschakelen. U kunt bovendien eenmalige aanmelding (SSO) inschakelen voor gebruikers in domein computers die zijn verbonden met het bedrijfsnetwerk. Met eenmalige aanmelding moeten ingeschakelde gebruikers alleen een gebruikersnaam voor veilige toegang tot cloud-bronnen invoeren. 

## <a name="pass-through-authentication"></a>Pass through-verificatie
[Azure AD Pass-through-verificatie](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication) biedt een eenvoudig wachtwoord validatie-oplossing voor Azure AD gebaseerde services met behulp van uw lokale Active Directory. Als beveiliging en naleving van beleid voor uw organisatie niet is toegestaan voor het verzenden van wachtwoorden van gebruikers, zelfs in een formulier hash en u alleen hoeft te bureaublad SSO-ondersteuning voor apparaten die lid zijn van een domein, is het raadzaam dat u evalueren met behulp van Pass through-verificatie. Pass through-verificatie is niet vereist voor een implementatie in het Perimeternetwerk, waardoor de implementatie-infrastructuur met AD FS in vergelijking eenvoudiger. Wanneer gebruikers zich met behulp van Azure AD aanmelden, valideert deze verificatiemethode rechtstreeks op uw lokale Active Directory-wachtwoorden van gebruikers.

![Pass through-verificatie](./media/choose-hybrid-identity-solution/pass-through-authentication.png)

Met Pass through-verificatie is niet nodig voor een complexe netwerkinfrastructuur en u hoeft niet te on-premises wachtwoorden opslaan in de cloud. In combinatie met eenmalige aanmelding Pass through-verificatie biedt een volledig geïntegreerde ervaring tijdens het aanmelden bij Azure AD of andere cloudservices.

Pass through-verificatie is geconfigureerd met Azure AD Connect, dat gebruikmaakt van een eenvoudige lokale agent die naar aanvragen voor wachtwoord-validatie luistert. De agent kan gemakkelijk worden geïmplementeerd op meerdere machines voor maximale beschikbaarheid en taakverdeling. Aangezien alle communicatie alleen uitgaand zijn, is er geen vereiste voor de connector moet worden geïnstalleerd in een Perimeternetwerk. De server-computer-vereisten voor de connector zijn als volgt:

- Windows Server 2012 R2 of hoger
- Gekoppeld aan een domein in het forest via welke gebruikers worden gevalideerd

> [!NOTE]
> Azure AD Pass-through-verificatie is momenteel in preview en wordt ondersteund voor web browser gebaseerde clients en Office-clients die moderne verificatie ondersteunen. Voor clients die niet worden ondersteund, zoals verouderde clients van Office en Exchange ActiveSync (inclusief systeemeigen e-mailclients op mobiele apparaten), is het raadzaam de equivalente moderne authenticatie gebruiken. Moderne verificatie wordt niet alleen kan Pass through-verificatie, maar er wordt een ook kunt u beleidsregels voor voorwaardelijke toegang moet worden toegepast, zoals multi-factor authentication-server. 

Pass through-verificatie is momenteel niet ondersteund wanneer met behulp van Windows 10-apparaten die lid zijn van Azure AD. Echter, u synchronisatie van wachtwoordhash kunt gebruiken als een automatische terugvallen op ondersteuning voor Windows 10 en de verouderde clients van eerder is vermeld. Tijdens de preview synchronisatie van wachtwoordhash standaard ingeschakeld wanneer Pass through-verificatie is geselecteerd als de optie aanmelden in Azure AD Connect.


## <a name="federated-identity-ad-fs"></a>Federatieve identiteiten (AD FS)
Voor meer controle over hoe gebruikers toegang Office 365 en andere cloudservices tot, kunt u instellen van directorysynchronisatie met het gebruik van eenmalige aanmelding (SSO) [Active Directory Federation Services (AD FS)](https://docs.microsoft.com/windows-server/identity/ad-fs/overview/whats-new-active-directory-federation-services-windows-server-2016). Federatie van uw gebruikers aanmeldingen met AD FS delegeert verificatie naar een lokale server te valideren en gebruikersreferenties. In dit model worden on-premises Active Directory-referenties nooit doorgegeven naar Azure AD.

![Federatieve identiteiten](./media/choose-hybrid-identity-solution/federated-identity.png)

Ook wel identiteitsfederatie genoemd, met deze methode aanmelden zorgt ervoor dat alle gebruikersverificatie gecontroleerde on-premises en beheerders van de strengere mate van toegangsbeheer implementeren. Identiteitsfederatie met AD FS is de meest complexe optie en implementatie van extra servers in uw on-premises omgeving is vereist. Identiteitsfederatie voert u tot het 24 x 7 ondersteuning bieden voor uw Active Directory en AD FS-infrastructuur. Deze hoge mate van ondersteuning is nodig omdat als uw Internet-lokale toegang, domeincontroller of AD FS-servers niet beschikbaar, gebruikers zich niet aanmelden zijn bij cloud services.

> [!TIP]
> Als u Federatie met Active Directory Federation Services (AD FS) gebruiken besluit, kunt u desgewenst instellen Wachtwoordsynchronisatie als een back-up als uw AD FS-infrastructuur is mislukt.


## <a name="common-scenarios-and-recommendations"></a>Algemene scenario's en aanbevelingen
Hier volgen enkele algemene hybride identiteit en toegang management scenario's met betrekking tot welke hybride identiteit optie (of opties) mogelijk geschikt voor elke aanbevelingen.

|Ik wil:|PWS en SSO<sup>1</sup>| PTA en SSO<sup>2</sup> | AD FS<sup>3</sup>|
|-----|-----|-----|-----|
|Nieuwe gebruikers, contactpersonen en groepsaccounts die zijn gemaakt in Mijn lokale Active Directory naar de cloud worden gesynchroniseerd.|![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png)| ![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png) |![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Instellen van mijn tenants voor Office 365 hybride scenario 's|![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png)| ![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png) |![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Mijn gebruikers aanmelden en toegang tot cloudservices met hun on-premises wachtwoord inschakelen|![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png)| ![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png) |![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Eenmalige aanmelding met bedrijfsreferenties implementeren|![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png)| ![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png) |![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Zorg ervoor dat er geen wachtwoord-hashes zijn opgeslagen in de cloud| |![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png)|![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Oplossingen voor on-premises meervoudige verificatie inschakelen| | |![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Smartcardverificatie ondersteuning voor Mijn gebruikers<sup>4</sup>| | |![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png)|
|Wachtwoord verloopt meldingen worden weergegeven in de Office-Portal en op het bureaublad van Windows 10| | |![Aanbevolen](./media/choose-hybrid-identity-solution/ic195031.png)|

> <sup>1</sup> Wachtwoordsynchronisatie met eenmalige aanmelding. 

> <sup>2</sup> Pass through-verificatie en eenmalige aanmelding. 

> <sup>3</sup> federatieve eenmalige aanmelding met AD FS.

> <sup>4</sup> AD FS kan worden geïntegreerd met uw enterprise PKI te kunnen aanmelden met behulp van certificaten. Deze certificaten kunnen worden soft-certificaten die zijn geïmplementeerd via vertrouwde inrichting kanalen zoals MDM of GPO of smartcard-certificaten (inclusief PIV/CAC kaarten) of Hello voor bedrijven (cert-vertrouwensrelatie). Zie voor meer informatie over ondersteuning voor smartcard-verificatie, [deze blog](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/).


## <a name="next-steps"></a>Volgende stappen
[Meer informatie in een omgeving met Azure Proof-of-Concept](https://aka.ms/aad-poc)

[Azure AD Connect installeren](http://go.microsoft.com/fwlink/?LinkId=615771)

[Synchronisatie van hybride identiteit controleren](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health)

