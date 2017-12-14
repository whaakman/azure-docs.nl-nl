---
title: Uw on-premises infrastructuur voor identiteiten in de cloud controleren.
description: Dit is de Azure AD Connect Health-pagina waar wordt beschreven wat dit is en waarvoor u het kunt gebruiken.
services: active-directory
documentationcenter: 
author: karavar
manager: mtillman
editor: curtand
ms.assetid: 82798ea6-5cd3-4f30-93ae-d56536f8d8e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 9919e326613ce81db32f1d72a8cb5e3668992885
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="monitor-your-on-premises-identity-infrastructure-and-synchronization-services-in-the-cloud"></a>Uw on-premises infrastructuur voor identiteiten en synchronisatieservices in de cloud controleren
Azure Active Directory (Azure AD) Connect Health helpt u om inzicht te verkrijgen in uw on-premises infrastructuur voor identiteiten en de synchronisatieservices. Hiermee kunt u een betrouwbare verbinding met Office 365 en Microsoft Online Services onderhouden door bewakingsmogelijkheden te bieden voor uw belangrijkste identiteitsonderdelen, zoals Active Directory Federation Services-servers (AD FS), Azure AD Connect-servers (ook wel Sync Engine genoemd), Active Directory-domeincontrollers, enzovoort. Ook worden de belangrijkste gegevenspunten over deze onderdelen gemakkelijk toegankelijk gemaakt, zodat u statistieken over het gebruik en andere zaken kunt ophalen en u weloverwogen beslissingen kunt nemen.

Deze informatie vindt u in de [portal voor Azure AD Connect Health](https://aka.ms/aadconnecthealth). In de portal voor Azure AD Connect Health kunt u waarschuwingen bekijken, de prestaties bewaken, het gebruik analyseren en nog veel meer. Met Azure AD Connect Health ziet u in één oogopslag de status van uw belangrijkste identiteitsonderdelen.

![Wat is Azure AD Connect Health?](./media/active-directory-aadconnect-health/aadconnecthealth2.png)


De portal biedt één interface waar u een handig overzicht hebt van het alsmaar toenemende aantal functies van Azure AD Connect Health. U krijgt een veel betere en geïntegreerde omgeving zodat uw gebruikers hun werk sneller en handige kunnen verrichten.
## <a name="why-use-azure-ad-connect-health"></a>Waarom het handig is om Azure AD Connect Health te gebruiken
Wanneer u uw on-premises directory's integreert met Azure AD, worden uw gebruikers productiever omdat zij één identiteit hebben voor toegang tot zowel cloud- als on-premises resources. Bij deze integratie moet er echter wel voor worden gezorgd dat deze omgeving stabiel is, zodat gebruikers op een betrouwbare manier toegang hebben tot zowel on-premises als cloudresources vanaf elk apparaat. Azure AD Connect Health maakt het voor u eenvoudiger om uw on-premises identiteitsinfrastructuur te bewaken en er statistieken over te bekijken. Dit is de infrastructuur die wordt gebruikt voor toegang tot Office 365 en andere Azure AD-toepassingen. Het is net zo eenvoudig als het installeren van een agent op een van uw on-premises identiteitsservers.

## <a name="azure-ad-connect-health-for-ad-fsactive-directory-aadconnect-health-adfsmd"></a>[Azure AD Connect Health voor AD FS](active-directory-aadconnect-health-adfs.md)
Azure AD Connect Health voor AD FS biedt ondersteuning voor AD FS 2.0 in Windows Server 2008 R2, Windows Server 2012 en Windows Server 2012 R2. De oplossing biedt ook ondersteuning voor de bewaking van AD FS-proxy- of webtoepassingsproxyservers die verificatie-ondersteuning bieden voor extranettoegang. Bij Azure AD Connect Health kan de Health-agent eenvoudig en voordelig worden geïnstalleerd. AD FS biedt daarbij de volgende reeks kernfuncties:

* Bewaking met waarschuwingen over de status van AD FS- en AD FS-proxyservers
* E-mailmeldingen voor kritieke waarschuwingen
* Trends wat betreft prestatiegegevens, wat nuttig is voor de capaciteitsplanning van AD FS
* Gebruiksanalyse voor AD FS-aanmeldingen met draaigrafieken (apps, gebruikers, netwerklocatie, enzovoort), wat handig is voor beter inzicht in hoe AD FS wordt gebruikt
* Rapporten voor AD FS, zoals 50 belangrijkste gebruikers die aanmeldingspogingen ondernemen met een ongeldige gebruikersnaam/ongeldig wachtwoord, met het laatste IP-adres
  
Lees hier meer over [Azure AD Connect Health gebruiken met AD FS](active-directory-aadconnect-health-adfs.md)

De volgende video biedt een overzicht van Azure AD Connect Health voor AD FS.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD-Connect-Health--Monitor-you-identity-bridge/player]
>

>

## <a name="azure-ad-connect-health-for-syncactive-directory-aadconnect-health-syncmd"></a>[Azure AD Connect Health for Sync](active-directory-aadconnect-health-sync.md)
Met Azure AD Connect Health for Sync wordt informatie gecontroleerd en geboden over de synchronisaties die worden uitgevoerd tussen uw on-premises Active Directory en Azure AD. Azure AD Connect Health for Sync biedt de volgende set kernfuncties:

* Bewaking met waarschuwingen over de status van Azure AD Connect-servers (ook wel Sync Engine genoemd)
* E-mailmeldingen voor kritieke waarschuwingen
* Operationele synchronisatie-inzichten, inclusief latentiegrafieken voor synchronisatiebewerkingen en trends in verschillende bewerkingen zoals toevoegingen, updates en verwijderingen
* In één oogopslag informatie over synchronisatie-eigenschappen en de laatste succesvolle export naar Azure AD
* Voor rapporten over synchronisatiefouten op objectniveau \(is Azure AD Premium niet vereist\)

Lees hier meer over [Azure AD Connect Health gebruiken voor synchronisatie](active-directory-aadconnect-health-sync.md)

De volgende video biedt een overzicht van Azure AD Connect Health for Sync.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-Health-Monitoring-the-sync-engine/player]
>
>

## <a name="azure-ad-connect-health-for-ad-dsactive-directory-aadconnect-health-addsmd"></a>[Azure AD Connect Health voor AD DS](active-directory-aadconnect-health-adds.md)
Azure AD Connect Health voor Active Directory Domain Services biedt bewaking voor domeincontrollers die zijn geïnstalleerd in Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 en Windows Server 2016. Met een Health-agentinstallatie kunt u uw on-premises AD DS-omgeving vanuit de cloud bewaken. Azure AD Connect Health voor AD DS biedt de volgende set kernfuncties:

* Bewakingswaarschuwingen om te detecteren wanneer domeincontrollers niet in orde zijn, samen met e-mailmeldingen voor kritieke waarschuwingen
* Het dashboard voor domeincontrollers, waarmee u snel inzicht krijgt in de (operationele) status van uw domeincontrollers
* Het dashboard voor replicatiestatus, met meest recente replicatiegegevens, samen met koppelingen naar gidsen voor het oplossen van problemen wanneer er fouten worden gedetecteerd
* Snel overal toegang tot grafieken met prestatiegegevens van populaire prestatiemeteritems, vereist voor het oplossen van problemen en bewakingsdoeleinden

Lees hier meer over [Azure AD Connect Health gebruiken met AD DS](active-directory-aadconnect-health-adds.md)

De volgende video biedt een overzicht van Azure AD Connect Health voor AD DS.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD-Connect-Health-monitors-on-premises-AD-Domain-Services/player]
>
>

## <a name="get-started-with-azure-ad-connect-health"></a>Aan de slag met Azure AD Connect Health
Volg de volgende stappen om aan de slag te gaan met Azure AD Connect Health:

1. [Installeer Azure AD Premium](../active-directory-get-started-premium.md) of [gebruik een proefversie](https://azure.microsoft.com/trial/get-started-active-directory/).
2. [Download en installeer Azure AD Connect Health-agents](#download-and-install-azure-ad-connect-health-agent) op uw servers voor identiteiten.
3. Ga naar [https://aka.ms/aadconnecthealth](https://aka.ms/aadconnecthealth) om het Azure AD Connect Health-dashboard te bekijken.

> [!NOTE]
> Houd er rekening mee dat u de Azure AD Connect Health-agents moet installeren op uw doelservers om gegevens te zien in uw Azure AD Connect Health-dashboard.
>
>

## <a name="download-and-install-azure-ad-connect-health-agent"></a>De Azure AD Connect Health-agent downloaden en installeren
* Zorg dat u [voldoet aan de vereisten](active-directory-aadconnect-health-agent-install.md#requirements) voor Azure AD Connect Health.
* Aan de slag met Azure AD Connect Health voor AD FS
    * [Download de Azure AD Connect Health-agent voor AD FS.](http://go.microsoft.com/fwlink/?LinkID=518973)
    * [Zie de installatie-instructies](active-directory-aadconnect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs).
* Aan de slag met Azure AD Connect Health for Sync
    * [Download en installeer de nieuwste versie van Azure AD Connect](http://go.microsoft.com/fwlink/?linkid=615771). De Health for Sync-agent wordt geïnstalleerd als onderdeel van de Azure AD Connect-installatie (versie 1.0.9125.0 of hoger).
* Aan de slag met Azure AD Connect Health voor AD DS
    * [Download de Azure AD Connect Health-agent voor AD DS](http://go.microsoft.com/fwlink/?LinkID=820540).
    * [Zie de installatie-instructies](active-directory-aadconnect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-ds).

## <a name="azure-ad-connect-health-portal"></a>Portal voor Azure AD Connect Health
In de portal voor Azure AD Connect Health kunt u waarschuwingen weergeven, de prestaties controleren en het gebruik analyseren. Via de URL https://aka.ms/aadconnecthealth gaat u naar de hoofdblade van Azure AD Connect Health. Een blade kunt u zien als een venster. Op de hoofdblade ziet u **Snel starten**, services in Azure AD Connect Health en extra configuratie-opties. Zie de volgende schermafbeelding en de korte uitleg die daarna wordt gegeven. Nadat u de agents hebt geïmplementeerd, identificeert de Health-service automatisch de services die met Azure AD Connect Health worden bewaakt.

> [!NOTE]
> Zie de [Veelgestelde vragen over Azure AD Connect](active-directory-aadconnect-health-faq.md) of de [Azure AD-pagina met prijzen](https://aka.ms/aadpricing) voor licentie-informatie.
    
![Portal voor Azure AD Connect Health](./media/active-directory-aadconnect-health/portal4.png)

* **Snel starten**: wanneer u deze optie selecteert, wordt de blade **Snel starten** geopend. U kunt de Azure AD Connect Health-agent downloaden door **Hulpprogramma's downloaden** te selecteren. U kunt ook documentatie bekijken en feedback geven.
* **Active Directory Federation Services**: met deze optie krijgt u alle AD FS-services te zien die momenteel worden bewaakt via Azure AD Connect Health. Wanneer u een exemplaar selecteert, staat er op de blade die wordt geopend informatie over dat service-exemplaar. Deze informatie omvat een overzicht, eigenschappen, waarschuwingen, controle en gebruiksanalyses. Zie [Azure AD Connect Health gebruiken met AD FS](active-directory-aadconnect-health-adfs.md) voor meer informatie over de mogelijkheden.
* **Azure Active Directory Connect (Sync)**: deze optie verwijst u naar de Azure AD Connect-servers die momenteel door Azure AD Connect Health worden bewaakt. Wanneer u het item selecteert, staat op de blade die wordt geopend informatie over uw Azure AD Connect-servers. Zie [Azure AD Connect Health for Sync gebruiken](active-directory-aadconnect-health-sync.md) voor meer informatie over de mogelijkheden.
* **Active Directory Domain Services**: met deze optie ziet u alle AD DS-forests die momenteel door Azure AD Connect Health worden bewaakt. Wanneer u een forest selecteert, staat er op de blade die wordt geopend informatie over die forest. Deze informatie bevat een overzicht van essentiële gegevens, een dashboard voor domeincontrollers, een dashboard voor replicatiestatus, waarschuwingen en bewaking. Zie [Azure AD Connect Health gebruiken met AD DS](active-directory-aadconnect-health-adds.md) voor meer informatie over de mogelijkheden.
* **Configureer**: deze sectie bevat opties voor het uit- of inschakelen van de volgende zaken:

  - Automatische updates zodat de Azure AD Connect Health-agent automatisch wordt bijgewerkt naar de nieuwste versie: dit betekent dat uw systeem automatisch wordt bijgewerkt naar de nieuwste versie van de Azure AD Connect Health-agent zodra deze beschikbaar is. Deze optie is standaard ingeschakeld.
  - Microsoft alleen toegang geven tot de statusgegevens van uw Azure AD-directory voor het oplossen van problemen: dit betekent dat als deze optie is ingeschakeld, Microsoft dezelfde gegevens kan zien als u. Dit kan handig zijn bij het oplossen van problemen en het verlenen van hulp. Deze optie is standaard uitgeschakeld.

## <a name="related-links"></a>Verwante koppelingen
* [De Azure AD Connect Health-agent installeren](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health-bewerkingen](active-directory-aadconnect-health-operations.md)
* [Azure AD Connect Health gebruiken met AD FS](active-directory-aadconnect-health-adfs.md)
* [Azure AD Connect Health for Sync gebruiken](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect Health gebruiken met AD DS](active-directory-aadconnect-health-adds.md)
* [Veelgestelde vragen over Azure AD Connect Health](active-directory-aadconnect-health-faq.md)
* [Versiegeschiedenis van Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)
