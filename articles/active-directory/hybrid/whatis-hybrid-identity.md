---
title: Active Directory verbinden met Azure Active Directory. | Microsoft Docs
description: Azure AD Connect integreert uw on-premises directory's met Azure Active Directory. Hiermee kunt u een algemene identiteit bieden voor Office 365, Azure en SaaS toepassingen die zijn geïntegreerd met Azure AD.
keywords: inleiding tot Azure AD Connect, overzicht Azure AD Connect, wat is Azure AD Connect, Active Directory installeren
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 59bd209e-30d7-4a89-ae7a-e415969825ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/02/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 2aca42c23cc213d5d7e451105052d5d5d697b77d
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2018
ms.locfileid: "50979468"
---
# <a name="hybrid-identity-and-microsoft-identity-solutions"></a>Hybride identiteit en identiteitsoplossingen van Microsoft
Met hybride identiteitsoplossingen van [Microsoft Azure Active Directory (Azure AD)](../../active-directory/fundamentals/active-directory-whatis.md) kunt u on-premises Directory-objecten synchroniseren met Azure AD terwijl u uw gebruikers on-premises kunt blijven beheren. De eerste beslissing die u dient te nemen als u uw on-premises Windows Server Active Directory wilt synchroniseren met Azure AD, is of u beheerde identiteiten of gefedereerde identiteit wilt gebruiken. 

- **Beheerde identiteiten** - gebruikersaccounts en groepen die worden gesynchroniseerd vanuit een on-premises Active Directory en waarvan de gebruikersverificatie wordt beheerd door Azure.   
- Met **gefedereerde identiteiten** heeft u meer controle over gebruikers door de gebruikersverificatie van Azure te scheiden en de verificatie over te dragen aan een vertrouwde, on-premises identiteitsprovider. 

Voor het configureren van hybride identiteit zijn er diverse opties beschikbaar. Terwijl u overweegt welk identiteitsmodel het beste bij uw organisatie past, moet u ook nadenken over tijd, bestaande infrastructuur, complexiteit en kosten. Deze factoren zijn voor elke organisatie verschillend en kunnen in de loop der tijd veranderen. Als uw vereisten inderdaad veranderen, bent u flexibel genoeg om op een ander identiteitsmodel over te stappen.

## <a name="managed-identity"></a>Beheerde identiteit 

Beheerde identiteit is de eenvoudigste manier om on-premises Directory-objecten (gebruikers en groepen) met Azure AD te synchroniseren. 

![Gesynchroniseerde hybride identiteit](./media/whatis-hybrid-identity/managed.png)

Hoewel beheerde identiteit de makkelijkste en snelste methode is, moeten uw gebruikers nog wel een afzonderlijk wachtwoord hebben voor resources in de cloud. U kunt dit voorkomen door [een gebruikerswachtwoord-hash te synchroniseren](how-to-connect-password-hash-synchronization.md) met uw Azure AD Directory. Het synchroniseren van wachtwoord-hashes stelt gebruikers in staat zich met dezelfde gebruikersnaam en hetzelfde wachtwoord die ze on-premises gebruiken, aan te melden bij organisatieresources in de cloud. Azure AD Connect controleert regelmatig uw on-premises directory op wijzigingen en houdt uw Azure AD Directory gesynchroniseerd. Als een gebruikerskenmerk of -wachtwoord on-premises in Active Directory wordt gewijzigd, wordt het automatisch in Azure AD bijgewerkt. 

Voor de meeste organisaties waarvoor geldt dat ze hun gebruikers moeten laten aanmelden bij Office 365, SaaS-toepassingen en andere Azure-resources, wordt de standaardoptie voor wachtwoordhashsynchronisatie aanbevolen. Als dat niet geschikt is, dient u te kiezen tussen Pass-through-verificatie en AD FS.

> [!TIP]
> Gebruikerswachtwoorden worden opgeslagen in on-premises Windows Server Active Directory en wel in de vorm van een hash waarde die het feitelijke gebruikerswachtwoord voorstelt. Een hash-waarde is het resultaat vaan een eenzijdige wiskundige functie (het hash-algoritme). Er bestaat geen methode het resultaat van een eenzijdige functie terug te draaien naar de versie in tekst zonder opmaak van een wachtwoord. U kunt een wachtwoord-hash niet gebruiken om u aan te melden bij uw on-premises netwerk. Als u ervoor kiest wachtwoorden te synchroniseren, extraheert Azure AD Connect wachtwoord-hashes uit de on-premises Active Directory en past extra beveiligingsverwerking op de wachtwoord-hash toe voordat deze met Azure AD wordt gesynchroniseerd. Wachtwoordhashsynchronisatie kan ook in combinatie met terugschrijven van wachtwoord worden gebruikt om self-service voor wachtwoord opnieuw instellen in te schakelen. Daarnaast kunt u eenmalige aanmelding inschakelen voor gebruikers op computers die zijn toegevoegd aan een domein en verbonden met het bedrijfsnetwerk. Met eenmalige aanmelding hoeven gebruikers die hiervan gebruikmaken, alleen een gebruikersnaam in te voeren voor veilige toegang tot cloudresources. 
>

## <a name="pass-through-authentication"></a>Pass-through-verificatie

[Azure AD-pass-through-verificatie](how-to-connect-pta.md) biedt een eenvoudige valideringsoplossing voor Azure AD-services die gebruikmaken van uw on-premises Active Directory. Als het beveiligings- en nalevingsbeleid van uw organisatie het verzenden van gebruikerswachtwoorden niet toestaat, ook niet in een gehashte vorm, en als u alleen eenmalige aanmelding via het bureaublad hoeft te ondersteunen voor apparaten die zijn toegevoegd aan een domein, wordt het aangeraden evaluatie uit te voeren via pass-through-verificatie. Voor pass-through-verificatie is geen implementatie in de DMS vereist, wat de infrastructuur voor de implementatie vereenvoudigt in vergelijking met AD FS. Als gebruikers zich aanmelden bij Azure AD, worden met deze verificatiemethode de wachtwoorden rechtstreeks gecontroleerd tegen de on-premises Active Directory.

![Pass-through-verificatie](./media/whatis-hybrid-identity/pass-through-authentication.png)

Met pass-through-verificatie is er geen behoefte aan een complexe netwerkinfrastructuur en hoeft u geen on-premises wachtwoorden in de cloud op te slaan. In combinatie met eenmalige aanmelding biedt pass-through-verificatie een werkelijk geïntegreerde ervaring bij het aanmelden bij Azure AD of andere cloudservices.

Pass-through-verificatie wordt geconfigureerd met Azure AD Connect, waarvoor een eenvoudige on-premises agent wordt gebruikt die luistert naar aanvragen voor wachtwoordvalidatie. De agent kan eenvoudig worden geïmplementeerd voor meerdere machines, zodat hoge beschikbaarheid en taakverdeling wordt geboden. Omdat alle communicatie alleen uitgaand is, hoeft er geen connector in een DMS te worden geïnstalleerd. De vereisten van de server voor de connector zijn als volgt:

- Windows Server 2012 R2 of hoger
- Toegevoegd aan een domein in de forest via welke gebruikers worden gevalideerd

## <a name="federated-identity-ad-fs"></a>Federatieve identiteit (AD FS)

Als u meer controle wilt over hoe gebruikers toegang tot Office 365 en andere cloudservices krijgen, kunt u adreslijstsynchronisatie instellen met eenmalige aanmelding met behulp van [Active Directory Federation Services (AD FS)](how-to-connect-fed-whatis.md). Als u de aanmeldingen van uw gebruikers federeert met AD FS, wordt de verificatie gedelegeerd naar een on-premises server die de gebruikersreferenties valideert. In dit model worden on-premises Active Directory-referenties nooit doorgegeven aan Azure AD.

![Federatieve identiteit](./media/whatis-hybrid-identity/federated-identity.png)

Met deze aanmeldingsmethode, ook wel identiteitsfederatie genoemd, wordt gegarandeerd dat alle gebruikersverificatie on-premises wordt afgehandeld. Bovendien kunnen beheerders dan een robuuster niveau van toegangsbeheer implementeren. Identiteitsfederatie met AD FS is de ingewikkeldste optie en hiervoor is de implementatie van extra servers in uw on-premises omgeving vereist. Bij identiteitsfederatie bent u ook verplicht 24/7 ondersteuning te bieden voor uw Active Directory- en AD FS-infrastructuur. Dit hoge niveau van ondersteuning is noodzakelijk, omdat als uw on-premises internettoegang, domeincontroller of AD FS-servers niet beschikbaar zijn, gebruikers zich bij cloudservices kunnen aanmelden.

> [!TIP]
> Als u besluit federatie met Active Directory Federation Services (AD FS) te gebruiken, kunt u optioneel als back-up wachtwoordhashsynchronisatie instellen in geval de AD FS-infrastructuur mislukt.
>

## <a name="common-scenarios-and-recommendations"></a>Veelvoorkomende scenario's en aanbevelingen

Hieronder volgen enkele veelvoorkomende hybride identiteits- en beheerscenario's met aanbevelingen voor de meest geschikte hybride identiteitsoptie(s) voor elk scenario.

|Ik wil graag:|PHS en eenmalige aanmelding<sup>1</sup>| PTA en eenmalige aanmelding<sup>2</sup> | AD FS<sup>3</sup>|
|-----|-----|-----|-----|
|nieuwe accounts van gebruikers, contactpersonen en groepen die in mijn on-premises Active Directory zijn gemaakt, automatisch met de cloud synchroniseren.|![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)| ![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png) |![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)|
|mijn tenant instellen voor hybride Office 365-scenario's|![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)| ![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png) |![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)|
|mijn gebruikers in staat stellen zich aan te melden bij cloudservices met hun on-premises wachtwoord|![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)| ![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png) |![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)|
|eenmalige aanmelding implementeren met bedrijfsreferenties|![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)| ![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png) |![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)|
|ervoor zorgen dat wachtwoord-hashes in de cloud worden opgeslagen| |![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)|![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)|
|oplossingen voor meervoudige verificatie in de cloud mogelijk maken| |![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)|![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)|
|oplossingen voor meervoudige verificatie on-premises mogelijk maken| | |![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)|
|verificatie van smartcards voor mijn gebruikers mogelijk maken<sup>4</sup>| | |![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)|
|meldingen voor verlopen wachtwoorden weergeven in de Office-portal en op het Windows 10-bureaublad| | |![Aanbevolen](./media/whatis-hybrid-identity/ic195031.png)|

> <sup>1</sup> Wachtwoordhashsynchronisatie met eenmalige aanmelding.
>
> <sup>2</sup> Pass-through-verificatie en eenmalige aanmelding. 
>
> <sup>3</sup> Federatieve eenmalige aanmelding met AD FS.
>
> <sup>4</sup> AD FS kan worden geïntegreerd met uw Enterprise PKI om aanmelding met certificaten toe te staan. Deze certificaten kunnen voorlopige certificaten zijn die worden geïmplementeerd via vertrouwde inrichtingskanalen, zoals MDM-, GPO- of smartcardcertificaten (waaronder PIV/CAC-kaarten) of Hello voor Bedrijven (cert-trust). Zie [dit blog](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/) (Engelstalig) voor meer informatie over ondersteuning voor smartcardverificatie.
>

## <a name="what-is-azure-ad-connect"></a>Wat is Azure AD Connect?

Azure AD Connect is het programma van Microsoft dat is ontworpen om te voldoen aan uw doelstellingen voor een hybride identiteit.  Hiermee kunt u uw gebruikers een algemene identiteit bieden voor Office 365, Azure en SaaS toepassingen die zijn geïntegreerd met Azure AD.  Deze biedt de volgende functies:
    
- [Synchronisatie](how-to-connect-sync-whatis.md): dit onderdeel is verantwoordelijk voor het maken van gebruikers, groepen en andere objecten. Het kan ook gebruikt worden om te controleren of de identiteitsinformatie van uw on-premises gebruikers en groepen overeenkomt met de cloud.  Het onderdeel is verantwoordelijk voor het synchroniseren van wachtwoord-hashes met Azure AD.
- [Synchronisatie van wachtwoord-hashes](how-to-connect-password-hash-synchronization.md): een optioneel onderdeel waarmee gebruikers hetzelfde wachtwoord on-premises en in de cloud kunnen gebruiken door een hash van het gebruikerswachtwoord met Azure AD te synchroniseren.
-   [AD FS en integratie van federatie](how-to-connect-fed-whatis.md): een optioneel onderdeel van Azure AD Connect dat kan worden gebruikt om een hybride omgeving te configureren met behulp van een on-premises AD FS-infrastructuur. Het onderdeel biedt ook beheermogelijkheden voor AD FS, zoals het vernieuwen van certificaten en aanvullende implementaties van AD FS-servers.
-   [Pass through-verificatie](how-to-connect-pta.md): met dit optionele onderdeel kunnen gebruikers hetzelfde wachtwoord on-premises en in de cloud gebruiken, zonder dat er een extra infrastructuur van een federatieve omgeving vereist is.
-   [PingFederate en integratie van federatie](how-to-connect-install-custom.md#configuring-federation-with-pingfederate): een andere federatieoptie waarmee u PingFederate kunt gebruiken als uw identiteitsprovider.
-   [Statuscontrole](whatis-hybrid-identity-health.md): Azure AD Connect Health biedt goede controle en een centrale locatie in de Azure-portal om deze activiteit weer te geven. 


![Wat is Azure AD Connect?](./media/whatis-hybrid-identity/arch.png)



## <a name="what-is-azure-ad-connect-health"></a>Wat is Azure AD Connect Health?

Azure Active Directory (Azure AD) Connect Health helpt u om inzicht te verkrijgen in uw on-premises infrastructuur voor identiteiten en de synchronisatieservices. Hiermee kunt u een betrouwbare verbinding met Office 365 en Microsoft Online Services onderhouden door bewakingsmogelijkheden te bieden voor uw belangrijkste identiteitsonderdelen, zoals Active Directory Federation Services-servers (AD FS), Azure AD Connect-servers (ook wel Sync Engine genoemd), Active Directory-domeincontrollers, enzovoort. Ook worden de belangrijkste gegevenspunten over deze onderdelen gemakkelijk toegankelijk gemaakt, zodat u statistieken over het gebruik en andere zaken kunt ophalen en u weloverwogen beslissingen kunt nemen.

Deze informatie vindt u in de [portal voor Azure AD Connect Health](https://aka.ms/aadconnecthealth). In de portal voor Azure AD Connect Health kunt u waarschuwingen bekijken, de prestaties bewaken, het gebruik analyseren en nog veel meer. Met Azure AD Connect Health ziet u in één oogopslag de status van uw belangrijkste identiteitsonderdelen.

![Wat is Azure AD Connect Health?](./media/whatis-hybrid-identity-health/aadconnecthealth2.png)


De portal biedt één interface waar u een handig overzicht hebt van het alsmaar toenemende aantal functies van Azure AD Connect Health. U krijgt een veel betere en geïntegreerde omgeving zodat uw gebruikers hun werk sneller en handige kunnen verrichten.


## <a name="why-use-azure-ad-connect"></a>Waarom Azure AD Connect gebruiken?
Wanneer u uw on-premises directory's integreert met Azure AD, worden uw gebruikers productiever omdat zij één identiteit hebben voor toegang tot zowel resources in de cloud als on-premises. Gebruikers en organisaties kunnen profiteren van het volgende:

* Gebruikers kunnen één identiteit gebruiken voor toegang tot on-premises toepassingen en cloudservices zoals Office 365.
* Eén hulpprogramma voor gemakkelijke implementatie voor synchronisatie en aanmelden.
* Biedt de nieuwste mogelijkheden voor uw scenario's. Azure AD Connect vervangt oudere versies van hulpprogramma's voor identiteitsintegratie zoals DirSync en Azure AD Sync. Zie voor meer informatie [Vergelijking hybride hulpprogramma’s voor identiteitsdirecory-integratie ](plan-hybrid-identity-design-considerations-tools-comparison.md).

## <a name="why-use-azure-ad-connect-health"></a>Waarom het handig is om Azure AD Connect Health te gebruiken
Wanneer u uw on-premises directory's integreert met Azure AD, worden uw gebruikers productiever omdat zij één identiteit hebben voor toegang tot zowel cloud- als on-premises resources. Bij deze integratie moet er echter wel voor worden gezorgd dat deze omgeving stabiel is, zodat gebruikers op een betrouwbare manier toegang hebben tot zowel on-premises als cloudresources vanaf elk apparaat. Azure AD Connect Health maakt het voor u eenvoudiger om uw on-premises identiteitsinfrastructuur te bewaken en er statistieken over te bekijken. Dit is de infrastructuur die wordt gebruikt voor toegang tot Office 365 en andere Azure AD-toepassingen. Het is net zo eenvoudig als het installeren van een agent op een van uw on-premises identiteitsservers.

### <a name="azure-ad-connect-health-for-ad-fshow-to-connect-health-adfsmd"></a>[Azure AD Connect Health voor AD FS](how-to-connect-health-adfs.md)
Azure AD Connect Health voor AD FS biedt ondersteuning voor AD FS 2.0 in Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 en Windows Server 2016. De oplossing biedt ook ondersteuning voor de bewaking van AD FS-proxy- of webtoepassingsproxyservers die verificatie-ondersteuning bieden voor extranettoegang. De Health-agent kan snel en eenvoudig worden geïnstalleerd, waarna u met Azure AD Connect Health voor AD FS beschikt over een set hoofdfuncties:

#### <a name="key-benefits-and-best-practices"></a>Belangrijkste voordelen en aanbevolen procedures

- *Verbeterde beveiliging*
  - [Trends voor vergrendeling van extranet](how-to-connect-health-adfs.md#usage-analytics-for-ad-fs)
  - [Rapport over mislukte aanmeldingen](how-to-connect-health-adfs.md#risky-ip-report-public-preview) 
  - [Krachtige privacybeveiliging](reference-connect-health-user-privacy.md)    
- *Waarschuwingen ontvangen voor alle [kritieke problemen met AD FS-systeem](how-to-connect-health-alert-catalog.md#alerts-for-active-directory-federation-services)*
    - Serverconfiguratie en -beschikbaarheid 
    - [Prestaties en connectiviteit](how-to-connect-health-adfs.md#performance-monitoring-for-ad-fs) 
  - Regelmatig onderhoud    
- *Eenvoudig te implementeren en beheren*
  - Snelle [installatie van agent](how-to-connect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs) 
  - Agent automatisch upgraden naar meest recente versie 
  - Gegevens binnen enkele minuten beschikbaar in de portal    
- *Uitgebreide [gebruiksgegevens](how-to-connect-health-adfs.md#usage-analytics-for-ad-fs)* 
  - Meest gebruikte toepassingen
  - Netwerklocaties en TCP-verbinding
  - Tokenaanvragen per server    
- *Goede gebruikerservaring* 
  - Dashboardstijl van Azure-portal
  - [Waarschuwingen per mail](how-to-connect-health-adfs.md#alerts-for-ad-fs)    

#### <a name="feature-highlight"></a>Uitgelichte functies

*   Bewaking met waarschuwingen over de status van AD FS- en AD FS-proxyservers
*   E-mailmeldingen voor kritieke waarschuwingen
*   Trends wat betreft prestatiegegevens, wat nuttig is voor de capaciteitsplanning van AD FS
*   Gebruiksanalyse voor AD FS-aanmeldingen met draaigrafieken (apps, gebruikers, netwerklocatie, enzovoort)
*   Rapporten voor AD FS, zoals 50 belangrijkste gebruikers die aanmeldingspogingen ondernemen met een ongeldige gebruikersnaam/ongeldig wachtwoord, met het laatste IP-adres
*   Melding van riskante IP voor mislukte AD FS-aanmeldingen

Lees hier meer over [Azure AD Connect Health gebruiken met AD FS](how-to-connect-health-adfs.md)

### <a name="azure-ad-connect-health-for-synchow-to-connect-health-syncmd"></a>[Azure AD Connect Health for Sync](how-to-connect-health-sync.md)
Met Azure AD Connect Health for Sync wordt informatie gecontroleerd en geboden over de synchronisaties die worden uitgevoerd tussen uw on-premises Active Directory en Azure AD. Azure AD Connect Health for Sync biedt de volgende set kernfuncties:

* Bewaking met waarschuwingen over de status van Azure AD Connect-servers (ook wel Sync Engine genoemd)
* E-mailmeldingen voor kritieke waarschuwingen
* Operationele inzichten synchroniseren, inclusief latentiegrafieken voor synchronisatiebewerkingen en trends in verschillende bewerkingen zoals toevoegingen, updates en verwijderingen
* In één oogopslag informatie over synchronisatie-eigenschappen en de laatste succesvolle export naar Azure AD
* Voor rapporten over synchronisatiefouten op objectniveau \(is Azure AD Premium niet vereist\)

Lees hier meer over [Azure AD Connect Health gebruiken voor synchronisatie](how-to-connect-health-sync.md)

### <a name="azure-ad-connect-health-for-ad-dshow-to-connect-health-addsmd"></a>[Azure AD Connect Health voor AD DS](how-to-connect-health-adds.md)
Azure AD Connect Health voor Active Directory Domain Services biedt bewaking voor domeincontrollers die zijn geïnstalleerd in Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 en Windows Server 2016. Met een Health-agentinstallatie kunt u uw on-premises AD DS-omgeving vanuit de cloud bewaken. Azure AD Connect Health voor AD DS biedt de volgende set kernfuncties:

* Bewakingswaarschuwingen om te detecteren wanneer domeincontrollers niet in orde zijn, samen met e-mailmeldingen voor kritieke waarschuwingen
* Het dashboard voor domeincontrollers, waarmee u snel inzicht krijgt in de (operationele) status van uw domeincontrollers
* Het dashboard voor replicatiestatus, met meest recente replicatiegegevens, samen met koppelingen naar gidsen voor het oplossen van problemen wanneer er fouten worden gedetecteerd
* Snel overal toegang tot grafieken met prestatiegegevens van populaire prestatiemeteritems, vereist voor het oplossen van problemen en bewakingsdoeleinden

Lees hier meer over [Azure AD Connect Health gebruiken met AD DS](how-to-connect-health-adds.md)

## <a name="next-steps"></a>Volgende stappen


- [Hardware en vereisten](how-to-connect-install-prerequisites.md) 
- [Snelle instellingen](how-to-connect-install-express.md)
- [Aangepaste instellingen](how-to-connect-install-custom.md)
- [Synchronisatie van wachtwoord-hashes](how-to-connect-password-hash-synchronization.md)|
- [Pass-through-verificatie](how-to-connect-pta.md)
- [Azure AD Connect en federatie](how-to-connect-fed-whatis.md)
- [Azure AD Connect Health-agents installeren](how-to-connect-health-agent-install.md) 
- [Azure AD Connect-synchronisatie](how-to-connect-sync-whatis.md)
- [Versiegeschiedenis](reference-connect-version-history.md)
- [Hulpprogramma's voor vergelijking van adreslijstintegratie](plan-hybrid-identity-design-considerations-tools-comparison.md)
- [Veelgestelde vragen over Azure AD Connect](reference-connect-faq.md)









