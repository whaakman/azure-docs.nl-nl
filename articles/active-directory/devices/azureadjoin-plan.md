---
title: Over het plannen van de join-implementatie van Azure Active Directory (Azure AD) | Microsoft Docs
description: Worden de stappen die nodig zijn voor het implementeren van Azure AD gekoppelde apparaten in uw omgeving.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.component: devices
ms.assetid: 81d4461e-21c8-4fdd-9076-0e4991979f62
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/20/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: fdc8b5db9316e463f8ec46ca5e235ea53cf44265
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2018
ms.locfileid: "52275969"
---
# <a name="how-to-plan-your-azure-ad-join-implementation"></a>Hoe: uw Azure AD join-implementatie plannen


Azure AD join kunt u apparaten koppelen rechtstreeks naar Azure AD-Tenant zonder de noodzaak om te koppelen aan on-premises Active Directory bij om uw gebruikers productief als veilig te houden.  

In dit artikel wordt ervan uitgegaan dat u bekend bent met Azure AD hebt en [apparaatstatussen in Azure AD](overview.md). 

 

## <a name="review-your-scenarios"></a>Controleer uw scenario 's 

Azure AD join is van toepassing op de meeste scenario's in de instelling van een organisatie, is er mogelijk bepaalde gebruiksvoorbeelden waar u wilt mogelijk Hybrid Azure AD join in plaats daarvan implementeren. Azure AD join is nu klaar voor ondernemingen voor implementaties op schaal of dezelfde scope. 

 
Houd rekening met Azure AD join op basis van de volgende criteria voor uw organisatie:  

- U van plan bent of het merendeel van uw Windows-apparaten hebben op Windows 10. 

- U van plan bent om te verplaatsen naar een cloud gebaseerde implementatie en beheer van Windows-apparaten. 

- U hebt beperkte on-premises infrastructuur of van plan bent om de voetafdruk van uw on-premises te verkleinen. 

- U hebt een sterke afhankelijkheid niet op Groepsbeleid om apparaten te beheren. 

- U hebt niet de oudere toepassingen die afhankelijk van verificatie van Active Directory-machine zijn. 

- Alle of geselecteerde gebruikers in uw organisatie beschikt niet over op het bedrijfsnetwerk toegang krijgen tot de benodigde apps en resources.  

 

Controleer uw scenario's op basis van de volgende evaluatie  

 

## <a name="assess-infrastructure"></a>Infrastructuur beoordelen  

 

### <a name="users"></a>Gebruikers 

Als uw gebruikers worden gemaakt in on-premises Active Directory, moeten ze worden gesynchroniseerd met Azure AD via Azure AD Connect. Zie voor meer informatie over het synchroniseren van gebruikers, [wat is Azure AD Connect?](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity#what-is-azure-ad-connect) 

Als uw gebruikers rechtstreeks in Azure AD worden gemaakt, is geen aanvullende installatie vereist 

 

Alternatieve aanmeldings-id's worden niet ondersteund op Azure AD gekoppelde apparaten. Gebruikers moeten beschikken over een geldige UPN in Azure AD.  

 

### <a name="identity-infrastructure"></a>Infrastructuur voor identiteiten 

Azure AD join werkt met zowel beheerde als federatieve-omgevingen.  

#### <a name="managed-environment"></a>Beheerde omgeving 

Een beheerde omgeving kan worden geïmplementeerd via Wachtwoordhashsynchronisatie of Pass through-verificatie met naadloze eenmalige aanmelding. <read more here> 

#### <a name="federated-environment"></a>Federatieve omgeving  

Een federatieve omgeving moet een id-provider die ondersteuning biedt voor WS-Trust- en WS-Federation-protocollen. WS-Federation is vereist voor een apparaat toevoegen aan Azure AD en WS-Trust is vereist om aan te melden bij een Azure AD gekoppelde apparaat. Dus als uw id-provider niet te ondersteunen, Azure AD join kan niet werken. 


#### <a name="smartcards-and-certificate-based-authentication"></a>Smartcards en certificaten gebaseerde verificatie 

 

Op dit moment worden verificatie op basis van certificaten en smartcards niet ondersteund in Azure AD, zodat ze niet op Azure AD gekoppelde apparaten werkt 

 

### <a name="devices"></a>Apparaten 

 

#### <a name="supported-devices"></a>Ondersteunde apparaten 

Azure AD join is exclusief voor Windows 10-apparaten. Het is niet van toepassing op eerdere versies van Windows en andere besturingssystemen. Als u nog steeds Windows 7/8.1 hebt, moet u upgraden naar Windows 10 voor het implementeren van Azure AD join 

 

Aanbeveling: Gebruik altijd de nieuwste versie van Windows 10 voor bijgewerkte functies 

 

### <a name="applications--other-resources"></a>Toepassingen en andere bronnen 

U wordt aangeraden dat u uw toepassingen migreren van on-premises naar de cloud voor een betere gebruikerservaring en toegangsbeheer. Echter, Azure AD join kunnen apparaten naadloos toegang bieden tot on-premises en cloudtoepassingen. Zie voor meer informatie, [hoe eenmalige aanmelding voor on-premises resources werkt in Azure AD gekoppelde apparaten](azuread-join-sso.md).  


Hieronder vindt u overwegingen voor het andere type van de toepassingen en bronnen 

 

- **Cloud-gebaseerde toepassingen:** als een toepassing wordt toegevoegd aan Azure AD-app-galerie, gebruikers krijgen eenmalige aanmelding via Azure AD gekoppelde apparaten. Er is geen aanvullende configuratie is vereist 

 

- **On-premises webtoepassingen:** als uw apps worden gebouwd en/of on-premises gehost moet u ze toevoegen aan de vertrouwde sites van uw browser. Hiermee schakelt u geïntegreerde Windows-verificatie om te werken en een niet-prompt SSO-ervaring bieden aan gebruikers. Als u van AD FS gebruikmaakt, Zie [controleren en beheren van eenmalige aanmelding met AD FS](https://docs.microsoft.com/en-us/previous-versions/azure/azure-services/jj151809(v%3dazure.100)) voor meer informatie. Aanbeveling: Houd rekening met hosting in de cloud (bijvoorbeeld Azure) en integreren met Azure AD voor een betere ervaring. 

- **On-premises toepassingen die afhankelijk zijn van verouderde protocollen:** gebruikers krijgen eenmalige aanmelding van Azure AD gekoppelde apparaten als apparaat verbinding met de domeincontroller heeft. Aanbeveling: Azure AD App proxy voor veilige toegang voor deze toepassingen implementeren. Zie voor meer informatie, [waarom Application Proxy is een betere oplossing?](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy#why-is-application-proxy-a-better-solution) 

- **On-premises netwerkshares:** gebruikers krijgen eenmalige aanmelding voor hun netwerkshares uit Azure AD gekoppelde apparaten wanneer het apparaat verbonden met een domeincontroller is. Zie voor meer informatie, [hoe eenmalige aanmelding voor on-premises resources werkt in Azure AD gekoppelde apparaten](azuread-join-sso.md).

 

- **Printers:** implementeren hybride cloud print voor het detecteren van printers uit Azure AD gekoppelde apparaten. Gebruikers kunnen ook ook gebruiken voor de printers UNC-pad naar rechtstreeks toe te voegen. Printers kunnen niet automatisch worden gedetecteerd in een alleen-cloudomgeving. 

 

- **On-premises toepassingen die afhankelijk zijn van computerverificatie:** deze toepassingen worden niet ondersteund in Azure AD gekoppelde apparaten. Aanbeveling: Houd rekening met het buiten gebruik stellen van deze toepassingen en verplaatst naar hun moderne alternatieven.  

 

### <a name="device-management"></a>Apparaatbeheer  

 

Device management voor Azure AD gekoppelde apparaten is voornamelijk via een EMM-platform (bijvoorbeeld Intune) en de MDM-CSP's. Windows 10 heeft ingebouwde MDM-agent die geschikt is voor alle compatibele EMM-oplossingen.  

 

Groepsbeleid worden niet ondersteund op Azure AD gekoppelde apparaten als ze niet zijn verbonden met Active Directory.  

 

MDM-beleid pariteit evalueren met Groepsbeleid met behulp van de [MDM-hulpprogramma voor migratie-analyse [MMAT](https://channel9.msdn.com/Events/Ignite/Microsoft-Ignite-Orlando-2017/THR3002R). Controleer de ondersteunde en niet-ondersteunde beleidsregels om te bepalen van de toepasselijkheid van het gebruik van een EMM-oplossing in plaats van Groepsbeleid. Voor niet-ondersteunde beleidsregels, kunt u overwegen:  

- Het beleid voor niet-ondersteunde nodig zijn voor gebruikers of apparaten Azure AD join wordt geïmplementeerd op? 

- De niet-ondersteunde beleidsregels van toepassing zijn in een cloud gestuurde implementatie? 

 

Als uw EMM-oplossing niet beschikbaar via de app-galerie van Azure AD is, kunt u deze volgens de procedure wordt beschreven in toevoegen [Azure Active Directory-integratie met MDM](https://docs.microsoft.com/windows/client-management/mdm/azure-active-directory-integration-with-mdm).

 

Tot en met CO-beheer, kan SCCM worden gebruikt voor het beheren van bepaalde aspecten van de apparaten terwijl beleidsregels worden geleverd via uw EMM-platform. Microsoft Intune kunt CO-beheer met SCCM. Zie voor meer informatie, [CO-beheer voor Windows 10-apparaten](https://docs.microsoft.com/sccm/core/clients/manage/co-management-overview). Als u een EMM-product dan Intune gebruikt, neem contact op met uw provider EMM op scenario's van toepassing CO-beheer.  

 

Er zijn twee uitgebreide mogelijkheden voor het beheer van Azure AD gekoppelde apparaten: 

 

- **MDM-alleen** -apparaat wordt uitsluitend beheerd door een EMM-provider, zoals Intune. Alle beleidsregels worden geleverd als onderdeel van de MDM-inschrijvingsproces. Voor klanten van Azure AD Premium of EMS is MDM-registratie geautomatiseerd als onderdeel van Azure AD join. 

- **CO-beheer** -apparaat wordt gelijktijdig worden beheerd door de EMM-provider en SCCM. In deze benadering is de SCCM-agent geïnstalleerd op een apparaat met MDM worden beheerd voor het beheren van bepaalde aspecten. 

Aanbeveling: Houd rekening met MDM alleen management voor Azure AD apparaten gekoppelde.  

 

## <a name="configuration"></a>Configuratie 

 

Azure AD join kan worden geconfigureerd in de Azure AD-portal op basis van bepaalde specifieke instellingen. Ga naar `Devices -> Device settings` en configureer de volgende opties:   

- Gebruikers kunnen apparaten koppelen aan Azure AD: Stel dit in op 'Alle' of 'Ingeschakeld' op basis van het bereik van uw implementatie.  

- Extra lokale beheerders op Azure AD gekoppelde apparaten: ingesteld op 'Ingeschakeld' en gebruikers selecteert die u wilt toevoegen aan de lokale beheerdersgroep op alle Azure AD gekoppelde apparaten die zijn geïmplementeerd in uw organisatie. Zie voor meer informatie, [over het beheren van de lokale groep administrators op de Azure AD gekoppelde apparaten](assign-local-admin.md). 

- Multi-factor Authentication worden toegevoegd aan apparaten vereist: ingesteld op 'Ja' als u wilt gebruikers om uit te voeren van MFA dat tijdens het toevoegen van apparaten aan Azure AD. Voor de gebruiker die lid wordt van apparaat naar Azure AD MFA gebruiken, wordt het apparaat een 2e factor. 

Als u inschakelen status roaming naar Azure AD wilt zodat apparaten kunnen hun instellingen worden gesynchroniseerd, Zie [wat enterprise state roaming?](enterprise-state-roaming-overview.md). Het is raadzaam het inschakelen van deze instelling ook voor hybride Azure AD gekoppelde apparaten 

### <a name="deployment-options"></a>Implementatieopties 

 

Azure AD join kan worden geïmplementeerd via drie verschillende manieren:  

- **Selfservice in OOBE-instellingen/** - In de self-service-modus, gebruikers gaan via de Azure AD join procesinstellingen hetzij tijdens de Windows-Out van Box Experience (OOBE) of van Windows.  

- **Windows Autopilot** -Windows Autopilot kunt u vooraf configuratie van apparaten voor een soepeler ervaring in OOBE om uit te voeren van Azure AD join.   

- **Bulkinschrijving** -met bulkinschrijving kunnen een beheerder basis van Azure AD join met behulp van een bulksgewijze inrichting hulpprogramma om apparaten te configureren.  


Hier volgt een vergelijking van deze drie methoden 

 
||Selfservice instellen|Windows Autopilot|Bulkinschrijving|
|---|---|---|---|
|Vereisen dat tussenkomst van de gebruiker om in te stellen|Ja|Ja|Nee|
|IT-inspanning vereisen|Nee|Ja|Ja|
|Van toepassing stromen|OOBE & stellingen|Alleen OOBE|Alleen OOBE|
|Lokale beheerdersrechten voor de primaire gebruiker|Ja, standaard|Configureerbare|Nee|
|Vereisen dat OEM-Apparaatondersteuning|Nee|Ja|Nee|
|Ondersteunde versies|versie 1511 +|1709 +|1703 +|
 
Kies uw implementatiebenadering of benaderingen door de bovenstaande tabel controleren en beoordelen van de volgende overwegingen voor het overstappen op beide benaderingen:  

- Zijn uw technische gebruikers ervaren om te gaan via de instelling zelf? 

    - Self-service kan geschikt is voor deze gebruikers. Houd rekening met Windows Autopilot voor het verbeteren van de gebruikerservaring.  

- Zijn uw gebruikers externe of binnen zakelijke premises? 

    - Selfservice of Autopilot werken het beste voor externe gebruikers voor een probleemloze installatie. 
 
- Hebt u een voorkeur basis van een gebruiker of de configuratie van een beheerder beheerd? 

    - Bulkinschrijving werkt beter voor de beheerder gestuurde implementatie voor het instellen van apparaten voordat via toekent aan gebruikers.     

- U koop apparaten van 1-2 OEM's of hebt u een brede distributie van OEM-apparaten?  

    - Als het kopen van beperkte OEM's die Autopilot worden ook ondersteund, kunt u profiteren van betere integratie met Autopilot. 
 

 


## <a name="next-steps"></a>Volgende stappen

- [Apparaatbeheer](overview.md)

