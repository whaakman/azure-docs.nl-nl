---
title: Beheer van Azure-beveiliging en controle-overzicht | Microsoft Docs
description: Dit artikel bevat een overzicht van de beveiligingsfuncties en -services die Azure biedt om te helpen bij het beheer en bewaking van Azure-cloudservices en virtuele machines.
services: security
documentationcenter: na
author: TerryLanfear
manager: StevenPo
editor: TomSh
ms.assetid: 5cf2827b-6cd3-434d-9100-d7411f7ed424
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/30/2018
ms.author: terrylan
ms.openlocfilehash: 7dfc4329b338a65169c81521360264753bafa9d6
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50239913"
---
# <a name="azure-security-management-and-monitoring-overview"></a>Beheer van Azure-beveiliging en controle-overzicht

Azure biedt beveiligingsmethoden om u te helpen bij het beheer en controle van Azure-cloudservices en virtuele machines (VM's). Dit artikel bevat een overzicht van deze core beveiligingsfuncties en -services. Vindt u koppelingen naar artikelen met details van elk geven, zodat u kunt meer informatie.

De beveiliging van uw Microsoft-cloudservices is een partnerschap en een gedeelde verantwoordelijkheid tussen u en Microsoft. Microsoft is verantwoordelijk voor het Azure-platform en de fysieke beveiliging van de datacenters (met behulp van beveiligingsmaatregelen zoals vergrendelde badge-vermelding deuren fences en beveiligingen). Azure biedt sterke niveaus van cloudbeveiliging op het niveau van de software die voldoet aan de behoeften voor beveiliging, privacy en naleving van zijn klanten.

U bent eigenaar van uw gegevens en identiteiten, de verantwoordelijkheid voor het beveiligen van deze, de beveiliging van uw on-premises resources en de beveiliging van cloudonderdelen die u hebt de controle. Microsoft biedt u besturingselementen voor de beveiliging en mogelijkheden om te helpen uw gegevens en toepassingen te beschermen. De mate van de verantwoordelijkheid voor beveiliging is gebaseerd op het type van de service in de cloud.

Het volgende diagram ziet u het saldo van verantwoordelijkheid tussen Microsoft en de klant.

![Gedeelde verantwoordelijkheid][1]

Zie voor meer informatie over het beveiligingsbeheer van [beveiligingsbeheer in Azure](azure-security-management.md).

## <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer

Rollen gebaseerd toegangsbeheer (RBAC) biedt gedetailleerde toegangsbeheer voor Azure-resources. Met behulp van RBAC, kunt u mensen alleen de mate van toegang die ze nodig hebben om hun werk te verlenen. RBAC ook kunt u ervoor zorgen dat wanneer mensen de organisatie verlaten, ze geen toegang meer tot resources in de cloud.

Meer informatie:

* [Active Directory-teamblog op RBAC](https://cloudblogs.microsoft.com/enterprisemobility/?product=azure-active-directory)
* [Azure Role-Based Access Control](../role-based-access-control/role-assignments-portal.md)

## <a name="antimalware"></a>Antimalware

Met Azure, kunt u antimalwaresoftware van belangrijke leveranciers van beveiligingsoplossingen, zoals Microsoft, Symantec, Trend Micro, McAfee en Kaspersky. Deze software beschermt uw virtuele machines tegen schadelijke bestanden, adware en andere dreigingen.

Microsoft Antimalware voor Azure Cloud Services en virtuele Machines biedt u de mogelijkheid voor het installeren van een antimalware-agent voor virtuele machines en PaaS-rollen. Op basis van System Center Endpoint Protection, zorgt deze functie beproefde on-premises beveiligingstechnologie in de cloud.

We bieden ook diepe integratie voor de Trend [Deep Security](http://www.trendmicro.com/us/enterprise/cloud-solutions/deep-security/) en [SecureCloud](http://www.trendmicro.com/us/enterprise/cloud-solutions/secure-cloud/) producten in de Azure-platform. Deep Security is een antivirusoplossing en SecureCloud is een oplossing voor versleuteling. Deep Security wordt geïmplementeerd in virtuele machines via een extensiemodel. Met behulp van de gebruikersinterface van Azure portal en PowerShell, kunt u het gebruik van Deep Security in nieuwe virtuele machines die zijn opvoert, of een bestaande virtuele machines die al zijn geïmplementeerd.

Symantec Endpoint Protection (SEP) wordt ook ondersteund op Azure. Via integratie met de portal, kunt u opgeven dat u van plan bent SEP gebruiken op een virtuele machine. SEP kan worden geïnstalleerd op een nieuwe virtuele machine via de Azure-portal of deze kan worden geïnstalleerd op een bestaande virtuele machine via PowerShell.

Meer informatie:

* [Deploying Antimalware Solutions on Azure Virtual Machines](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/) (Antimalware-oplossingen implementeren op virtuele machines van Azure)
* [Microsoft Antimalware voor Azure-Cloudservices en virtuele Machines](azure-security-antimalware.md)
* [Installeren en configureren van Trend Micro Deep Security als een Service op een Windows-VM](../virtual-machines/windows/classic/install-trend.md)
* [Installeren en configureren van Symantec Endpoint Protection op een Windows-VM](../virtual-machines/windows/classic/install-symantec.md)
* [Nieuwe opties voor anti-malware voor het beveiligen van virtuele Machines van Azure](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Azure multi-factor Authentication is een verificatiemethode waarvoor het gebruik van meer dan één verificatiemethode. Een essentiële tweede beveiligingslaag wordt toegevoegd aan gebruikersaanmeldingen en transacties. 

Multi-factor Authentication helpt beschermen toegang tot gegevens en toepassingen aan de wensen van gebruikers voor een eenvoudige aanmeldprocedure. Het biedt een robuuste verificatie met een scala van verificatieopties (telefoonoproep, SMS-bericht of mobiele app-melding of verifiëren code) en het OATH-tokens van derden.

Meer informatie:

* [Multi-Factor Authentication](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
* [Wat is Azure Multi-Factor Authentication?](../active-directory/authentication/multi-factor-authentication.md)
* [De werking van Azure multi-factor Authentication](../active-directory/authentication/concept-mfa-howitworks.md)

## <a name="expressroute"></a>ExpressRoute

Azure ExpressRoute kunt u uw on-premises netwerken in de Microsoft Cloud uitbreiden via een speciale persoonlijke verbinding die mogelijk door een connectiviteitsprovider gemaakt wordt. Met ExpressRoute kunt kunt u verbindingen met Microsoft-cloudservices zoals Azure, Office 365 en CRM Online maken. Connectiviteit mogelijk vanuit:

* Een any-to-any (IP VPN) netwerk.
* Een point-to-point Ethernet-netwerk.
* Een virtuele overlappende verbinding via een connectiviteitsprovider in een CO-locatiefaciliteit. 

ExpressRoute-verbindingen lopen niet via het openbare internet. Bieden meer betrouwbaarheid, hogere snelheden, kortere wachttijden en hogere beveiliging dan gebruikelijke verbindingen via internet.

Meer informatie:

* [Technisch overzicht van ExpressRoute](../expressroute/expressroute-introduction.md)

## <a name="virtual-network-gateways"></a>Virtuele netwerkgateways

VPN-gateways, ook wel genoemd virtuele netwerkgateways van Azure, worden gebruikt voor het verzenden van netwerkverkeer tussen virtuele netwerken en on-premises locaties. Ze worden ook gebruikt voor het verzenden van verkeer tussen meerdere virtuele netwerken in Azure (network-netwerk). VPN-gateways bieden beveiligde cross-premises-connectiviteit tussen Azure en uw infrastructuur.

Meer informatie:

* [Over VPN-gateways](../vpn-gateway/vpn-gateway-about-vpngateways.md)
* [Overzicht van de beveiliging van Azure-netwerk](security-network-overview.md)

## <a name="privileged-identity-management"></a>Privileged Identity Management

Soms moeten gebruikers bevoorrechte bewerkingen in de Azure-resources of andere SaaS-toepassingen uitvoeren. Dit betekent vaak dat organisaties zodat ze permanente bevoorrechte toegang in Azure Active Directory (Azure AD). 

Dit is een groeiende beveiligingsrisico voor de cloud gehoste bronnen omdat organisaties voldoende kunnen niet controleren wat gebruikers doen met hun bevoegde toegang. Als een gebruikersaccount met bevoegde toegang is geknoeid, kan dat een schending bovendien de algehele cloud-beveiliging van een organisatie beïnvloeden. Azure AD Privileged Identity Management helpt bij het oplossen van dit risico door te verlagen van de blootstellingstijd van bevoegdheden en inzicht in gebruik te verhogen.  

Privileged Identity Management introduceert het concept van een tijdelijke beheerder voor een functie of 'just-in-time' beheerderstoegang. Dit type beheerder is een gebruiker die moet een activeringsproces voltooien voor die rol is toegewezen. Het activeringsproces wordt de toewijzing van de gebruiker aan een rol in Azure AD van actief gedurende een opgegeven periode niet-actieve periode.

Meer informatie:

* [Azure AD Privileged Identity Management](../active-directory/privileged-identity-management/pim-configure.md)
* [Aan de slag met Azure AD Privileged Identity Management](../active-directory/privileged-identity-management/pim-getting-started.md)

## <a name="identity-protection"></a>Identiteitsbeveiliging

Azure AD Identity Protection biedt een geconsolideerde weergave van verdachte activiteiten voor aanmelden en mogelijke beveiligingsproblemen voor beveiliging van uw bedrijf. Identity Protection detecteert verdachte activiteiten voor gebruikers en identiteiten van bevoegdheden (beheerder), op basis van signalen:

* Beveiligingsaanvallen.
* De referenties zijn gelekt.
* Aanmeldingen vanaf onbekende locaties en geïnfecteerde apparaten.

Identity Protection helpt door te geven meldingen en aanbevolen herstel, risico's in realtime te beperken. Ernst van het risico wordt berekend. U kunt beleid op basis van risico om automatisch toegang tot tegen toekomstige bedreigingen beschermen-toepassing configureren.

Meer informatie:

* [Azure Active Directory Identity Protection](../active-directory/active-directory-identityprotection.md)
* [Channel 9: Azure AD en Identity weergeven: Identity Protection Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="security-center"></a>Security Center

Azure Security Center helpt u bij het voorkomen, detecteren en direct reageren op bedreigingen. Security Center biedt u meer inzicht in, en controle over de beveiliging van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen. Het helpt bedreigingen detecteren die anders onopgemerkt en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen.

Security Center helpt u te optimaliseren en de beveiliging van uw Azure-resources door te controleren:

* Hierdoor kunt u beleid voor de resources van uw Azure-abonnement volgens definiëren:
  * Beveiliging van uw bedrijf nodig heeft.
  * Het type toepassingen of de gevoeligheid van de gegevens in elk abonnement.
* Bewaking van de status van uw Azure virtual machines, netwerken en toepassingen.
* Biedt een lijst met beveiligingswaarschuwingen, inclusief waarschuwingen van geïntegreerde partneroplossingen. Het biedt ook de informatie die u nodig hebt om snel onderzoek te een aanval en aanbevelingen voor het worden opgelost.

Meer informatie:

* [Inleiding tot Azure Security Center](../security-center/security-center-intro.md)
* [Verbetering van uw beveiligde score in Azure Security Center](../security-center/security-center-secure-score.md)

## <a name="intelligent-security-graph"></a>Intelligent Security Graph

Intelligent Security Graph biedt realtime threat protection in Microsoft-producten en services. Maakt gebruik van geavanceerde analyses die een enorme hoeveelheid intelligentie en beveiliging gegevens van bedreigingen voor inzichten aan de beveiliging van de organisatie kunnen versterken koppelen. Microsoft gebruikt geavanceerde analyses, verwerken van meer dan 450 miljard verificaties per maand, 400 miljard e-mailberichten voor malware en phishing scannen en één miljard apparaten bijwerken, uitgebreidere inzichten leveren. Deze inzichten kunnen helpen uw organisatie detecteren en snel reageren op aanvallen.

* [Intelligent Security Graph](https://www.microsoft.com/security/intelligence)

<!--Image references-->
[1]: ./media/security-management-and-monitoring-overview/shared-responsibility.png

