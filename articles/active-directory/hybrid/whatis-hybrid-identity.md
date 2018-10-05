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
ms.date: 09/18/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 40ac3ca92c65607df056b883608dde60d816143e
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47181770"
---
# <a name="hybrid-identity-and-microsofts-identity-solutions"></a>Hybride identiteit en identiteitsoplossingen van Microsoft
Vandaag de dag maken organisaties en bedrijven steeds meer gebruik van een mix van on-premises toepassingen en cloudtoepassingen.  Het ondersteunen van toepassingen en gebruikers die toegang nodig hebben tot deze toepassingen, zowel on-premises als in de cloud, is inmiddels een flinke uitdaging.

De identiteitsoplossingen van Microsoft omvatten mogelijkheden voor zowel on-premises als in de cloud. Er wordt één gebruikersidentiteit gemaakt voor verificatie en autorisatie bij alle resources, ongeacht hun locatie. We noemen dit hybride identiteit.

## <a name="what-is-azure-ad-connect"></a>Wat is Azure AD Connect?

Azure AD Connect is het programma van Microsoft dat is ontworpen om te voldoen aan uw doelstellingen voor een hybride identiteit.  Hiermee kunt u uw gebruikers een algemene identiteit bieden voor Office 365, Azure en SaaS toepassingen die zijn geïntegreerd met Azure AD.  Deze biedt de volgende functies:
    
- [Synchronisatie](how-to-connect-sync-whatis.md): dit onderdeel is verantwoordelijk voor het maken van gebruikers, groepen en andere objecten. Het kan ook gebruikt worden om te controleren of de identiteitsinformatie van uw on-premises gebruikers en groepen overeenkomt met de cloud.  Het onderdeel is verantwoordelijk voor het synchroniseren van wachtwoord-hashes met Azure AD.
-   [AD FS en integratie van federatie](how-to-connect-fed-whatis.md): een optioneel onderdeel van Azure AD Connect dat kan worden gebruikt om een hybride omgeving te configureren met behulp van een on-premises AD FS-infrastructuur. Het onderdeel biedt ook beheermogelijkheden voor AD FS, zoals het vernieuwen van certificaten en aanvullende implementaties van AD FS-servers.
-   [Pass through-verificatie](how-to-connect-pta.md): met dit optionele onderdeel kunnen gebruikers hetzelfde wachtwoord on-premises en in de cloud gebruiken, zonder dat er een extra infrastructuur van een federatieve omgeving vereist is.
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









