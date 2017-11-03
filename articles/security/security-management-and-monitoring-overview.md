---
title: Azure-beveiliging beheren en controleren van overzicht | Microsoft Docs
description: " Azure biedt beveiligingsmethoden om u te helpen bij het beheren en controleren van de Azure-cloudservices en virtuele machines.  Dit artikel bevat een overzicht van deze core beveiligingsfuncties en -services. "
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
ms.date: 11/21/2016
ms.author: terrylan
ms.openlocfilehash: 6787877deabafd0b7308e190cb45b4036049b05b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-security-management-and-monitoring-overview"></a>Azure-beveiliging beheren en controleren van overzicht
Azure biedt beveiligingsmethoden om u te helpen bij het beheren en controleren van de Azure-cloudservices en virtuele machines. Dit artikel bevat een overzicht van deze core beveiligingsfuncties en -services. Vindt u koppelingen naar artikelen met meer informatie over elk doorgegeven, zodat u meer informatie.

De beveiliging van uw Microsoft-cloudservices is een samenwerking en gedeelde verantwoordelijkheid tussen u en Microsoft. Gedeelde verantwoordelijkheid betekent dat Microsoft is verantwoordelijk voor de Microsoft Azure en de fysieke beveiliging van de datacentra (met behulp van beveiligingsinstellingen, zoals vergrendelde badge vermelding deuren fences en schermen). Daarnaast biedt Azure sterk niveaus van cloud-beveiliging tegen de software-laag die voldoet aan de behoeften van de beveiliging, privacy en naleving van zijn veeleisende klanten.

Eigenaar van uw gegevens en identiteiten, de verantwoordelijkheid voor het beveiligen van ze de beveiliging van uw lokale bronnen en de beveiliging van cloud-onderdelen waarover u controle hebt. Microsoft biedt u beveiligingsmaatregelen en mogelijkheden voor hulp bij het beveiligen van uw gegevens en toepassingen. De mate van verantwoordelijkheid voor beveiliging is gebaseerd op het type van de cloudservice.

Het volgende diagram ziet u het saldo van de verantwoordelijkheid voor zowel Microsoft als de klant.

![Gedeelde verantwoordelijkheid][1]

Zie voor meer informatie in het beveiligingsbeheer van, [beveiligingsbeheer in Azure](azure-security-management.md).

Hier volgen de belangrijkste functies in dit artikel aan bod:

* Op rollen gebaseerd toegangsbeheer
* Antimalware
* Multi-Factor Authentication
* ExpressRoute
* Virtuele netwerkgateways
* Beschermde identiteitsbeheer
* Identiteitsbeveiliging
* Security Center

## <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer
Op rollen gebaseerde toegangsbeheer (RBAC) biedt Geavanceerd toegangsbeheer voor Azure-resources. Met RBAC kunt verleent u gebruikers alleen de mate van toegang dat ze moeten uitvoeren van hun taken.  RBAC ook kunt u ervoor zorgen dat wanneer mensen de organisatie verlaten zij toegang tot bronnen in de cloud verliezen.

Meer informatie:

* [Active Directory-teamblog van RBAC](http://i1.blogs.technet.com/b/ad/archive/2015/10/12/azure-rbac-is-ga.aspx)
* [Op rollen gebaseerde toegangsbeheer van Azure](../active-directory/role-based-access-control-configure.md)

## <a name="antimalware"></a>Antimalware
Met Azure, kunt u antimalware-software van toonaangevende leveranciers zoals Microsoft, Symantec, Trend Micro, McAfee en Kaspersky kunt u uw virtuele machines beveiligen tegen schadelijke bestanden, adware en andere dreigingen.

Microsoft Antimalware biedt de mogelijkheid een antimalware-agent voor virtuele machines en PaaS-functies installeren. Op basis van System Center Endpoint Protection, biedt deze functie beproefde lokale beveiligingstechnologie voor de cloud.

We bieden ook diepe integratie voor de Trend [grondige beveiliging](http://www.trendmicro.com/us/enterprise/cloud-solutions/deep-security/)™ en [SecureCloud](http://www.trendmicro.com/us/enterprise/cloud-solutions/secure-cloud/)™ producten in de Azure-platform. DeepSecurity is virussen te bestrijden en SecureCloud is een oplossing voor versleuteling. DeepSecurity wordt geïmplementeerd in virtuele machines met een extensiemodel. De UI-portal en PowerShell gebruikt, kunt u gebruiken DeepSecurity in nieuwe virtuele machines die worden ingezet, of een bestaande virtuele machines die al zijn geïmplementeerd.

Symantec Endpoint Protection (SEP) wordt ook ondersteund in Azure. Klanten kunnen opgeven dat ze van plan bent te gebruiken SEP binnen een virtuele machine via de portal integratie. SEP kan worden geïnstalleerd op een nieuwe virtuele machine via de Azure-portal of kan worden geïnstalleerd op een bestaande virtuele machine met behulp van PowerShell.

Meer informatie:

* [Deploying Antimalware Solutions on Azure Virtual Machines](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/) (Antimalware-oplossingen implementeren op virtuele machines van Azure)
* [Microsoft Antimalware voor Azure-Cloudservices en virtuele Machines](azure-security-antimalware.md)
* [Het installeren en Trend Micro grondige Security configureren als een Service op een virtuele machine van Windows](../virtual-machines/windows/classic/install-trend.md)
* [Het installeren en configureren van Symantec Endpoint Protection op een virtuele machine van Windows](../virtual-machines/windows/classic/install-symantec.md)
* [Nieuwe Antimalware-opties voor het beveiligen van virtuele Machines in Azure – McAfee Endpoint Protection](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication
Azure multi-factor authentication (MFA) is een authenticatiemethode die vereist het gebruik van meer dan één verificatiemethode en een kritieke tweede beveiligingslaag wordt toegevoegd aan de gebruikersaanmeldingen en transacties. MFA helpt beveiliging toegang tot gegevens en toepassingen en te voldoen aan de behoeften van de gebruiker voor een eenvoudig proces aanmelden. Levert sterke verificatie via een aantal opties voor verificatie: telefoonoproep, tekstbericht of mobiele app melding of verificatie van code en van derden OATH-tokens.

Meer informatie:

* [Multi-factor authentication](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
* [Wat is Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)
* [De werking van Azure multi-factor Authentication](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="expressroute"></a>ExpressRoute
Met Microsoft Azure ExpressRoute kunt u uw on-premises netwerken in de Microsoft Cloud uitbreiden via een speciale persoonlijke verbinding die wordt gefaciliteerd door een connectiviteitsprovider. Met ExpressRoute kunt u verbindingen tot stand brengen met Microsoft Cloud-services, zoals Microsoft Azure, Office 365 en CRM Online. Via een connectiviteitsprovider in een co-locatiefaciliteit is connectiviteit mogelijk vanuit een any-to-any (IP VPN) netwerk, een point-to-point Ethernet-netwerk of een virtuele overlappende verbinding. ExpressRoute-verbindingen gaan niet via het openbare internet. Daardoor zijn ExpressRoute-verbindingen betrouwbaarder en sneller en hebben ze lagere latenties en betere beveiliging dan gewone verbindingen via internet.

Meer informatie:

* [Technisch overzicht van ExpressRoute](../expressroute/expressroute-introduction.md)

## <a name="virtual-network-gateways"></a>Virtuele netwerkgateways
VPN-Gateways, ook wel Azure Virtual Network Gateways worden gebruikt om netwerkverkeer tussen virtuele netwerken en lokale locaties te verzenden. Ze worden ook gebruikt om verkeer tussen meerdere virtuele netwerken in Azure (VNet-naar-VNet) te verzenden.  VPN-gateways bieden beveiligde cross-premises-connectiviteit tussen Azure en uw infrastructuur.

Meer informatie:

* [Over VPN-gateways](../vpn-gateway/vpn-gateway-about-vpngateways.md)
* [Overzicht van Azure-netwerk-beveiliging](security-network-overview.md)

## <a name="privileged-identity-management"></a>Privileged Identity Management
Gebruikers moeten soms bij het uitvoeren van bevoorrechte bewerkingen in Azure-resources of andere SaaS-toepassingen. Dit betekent vaak dat organisaties hebben om hem permanente bevoegde toegang in Azure Active Directory (Azure AD). Dit is een groeiende beveiligingsrisico voor cloud-gebaseerde bronnen omdat organisaties voldoende kunnen niet controleren wat gebruikers doen met hun bevoorrechte toegang.
Als een gebruikersaccount met uitgebreide toegang is aangetast, kan die een inbreuk bovendien invloed op uw algehele beveiliging van de cloud. Azure AD Privileged Identity Management helpt bij het oplossen van dit risico door te verlagen van de blootstellingstijd van bevoegdheden en inzicht in gebruik te verhogen.  

Privileged Identity Management introduceert het concept van een tijdelijke beheerder voor een functie of 'just in time' beheerderstoegang, die een gebruiker die moet worden geactiveerd voor deze rol toegewezen voltooid is. Het activeringsproces verandert de toewijzing van de gebruiker aan een rol in Azure AD van actief gedurende een opgegeven periode inactief periode zoals acht uur.

Meer informatie:

* [Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-configure.md)
* [Aan de slag met Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-getting-started.md)

## <a name="identity-protection"></a>Identiteitsbeveiliging
Azure Active Directory (AD) Identity Protection biedt een totaaloverzicht van verdachte activiteiten voor aanmelden en potentiële zwakke plekken in uw bedrijf beter te beschermen. Identity Protection detecteert verdachte activiteiten voor gebruikers en bevoegdheden (admin) identiteiten, op basis van signalen zoals brute-force-aanvallen gelekte referenties en aanmeldingen vanaf onbekende locaties en geïnfecteerde apparaten.

Dankzij de meldingen en aanbevolen herstel, helpt Identity Protection risico's in realtime te beperken. Gebruiker risico ernst wordt berekend en u kunt beleid op basis van risico om automatisch te beschermen toepassing tegen toekomstige toegang configureren.

Meer informatie:

* [Azure Active Directory Identity Protection](../active-directory/active-directory-identityprotection.md)
* [Channel 9: Azure AD en Identity weergeven: Identity Protection Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="security-center"></a>Security Center
Azure Security Center helpt u bij het detecteren, voorkomen van en reageren op bedreigingen en biedt dat u grotere zichtbaarheid van, en controle over, de beveiliging van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bedreigingen te detecteren die anders onopgemerkt zouden blijven, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen.

Security Center helpt u te optimaliseren en de beveiliging van uw Azure-resources door te controleren:

* Zodat u kunt beleid voor uw Azure-abonnementresources afhankelijk van de beveiligingsvereisten van uw bedrijf en het type toepassingen of de vertrouwelijkheid van de gegevens definiëren in elk abonnement.
* Bewaking van de status van uw virtuele Azure-machines, netwerken en toepassingen.
* Het bieden van een lijst met beveiligingswaarschuwingen, met inbegrip van waarschuwingen van geïntegreerde partneroplossingen, samen met de informatie die u nodig hebt om snel onderzoek te en aanbevelingen voor het herstellen van een aanval.

Meer informatie:

* [Inleiding tot Azure Security Center](../security-center/security-center-intro.md)

<!--Image references-->
[1]: ./media/security-management-and-monitoring-overview/shared-responsibility.png
