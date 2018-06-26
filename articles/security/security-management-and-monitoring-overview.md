---
title: Azure-beveiligingsbeheer en controle overzicht | Microsoft Docs
description: Dit artikel bevat een overzicht van de beveiligingsfuncties en -services die Azure biedt om u te helpen bij het beheer en controle van Azure-cloudservices en virtuele machines.
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
ms.openlocfilehash: 9e538ac39af5b6df44860a4a70b0fd1e058c060c
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36752285"
---
# <a name="azure-security-management-and-monitoring-overview"></a>Azure-beveiligingsbeheer en controle-overzicht
Azure biedt beveiligingsmethoden om u te helpen bij het beheren en controleren van de Azure-cloudservices en virtuele machines (VM's). Dit artikel bevat een overzicht van deze core beveiligingsfuncties en -services. Vindt u koppelingen naar artikelen met meer informatie over elk doorgegeven, zodat u meer informatie.

De beveiliging van uw Microsoft-cloudservices is een verbinding en een gedeelde verantwoordelijkheid tussen u en Microsoft. Microsoft is verantwoordelijk voor het Azure-platform en de fysieke beveiliging van de datacenters (met behulp van beveiligingsinstellingen, zoals vergrendelde badge-vermelding deuren fences en schermen). Azure biedt sterk niveaus van cloud-beveiliging tegen de software-laag die voldoet aan de behoeften van de beveiliging, privacy en naleving van zijn klanten.

Eigenaar van uw gegevens en identiteiten, de verantwoordelijkheid voor het beveiligen van ze de beveiliging van uw lokale bronnen en de beveiliging van cloud-onderdelen waarover u controle hebt. Microsoft biedt u beveiligingsmaatregelen en mogelijkheden voor hulp bij het beveiligen van uw gegevens en toepassingen. De mate van verantwoordelijkheid voor beveiliging is gebaseerd op het type van de cloudservice.

Het volgende diagram ziet u het saldo van verantwoordelijkheid tussen Microsoft en de klant.

![Gedeelde verantwoordelijkheid][1]

Zie voor meer informatie over het beveiligingsbeheer van [beveiligingsbeheer in Azure](azure-security-management.md).

## <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer
Op rollen gebaseerde toegangsbeheer (RBAC) biedt gedetailleerde toegangsbeheer voor Azure-resources. Met RBAC kunt u de personen die alleen de mate van toegang dat ze moeten uitvoeren van hun taken verlenen. RBAC ook kunt u ervoor zorgen dat wanneer mensen de organisatie verlaat, zij toegang tot bronnen in de cloud verliezen.

Meer informatie:

* [Active Directory-teamblog van RBAC](https://cloudblogs.microsoft.com/enterprisemobility/?product=azure-active-directory)
* [Op rollen gebaseerde toegangsbeheer van Azure](../role-based-access-control/role-assignments-portal.md)

## <a name="antimalware"></a>Antimalware
U kunt de antimalware-software van toonaangevende leveranciers zoals Microsoft, Symantec, Trend Micro, McAfee en Kaspersky gebruiken met Azure. Deze software kunt u uw virtuele machines beveiligen tegen schadelijke bestanden, adware en andere dreigingen.

Microsoft Antimalware voor Azure Cloud Services en virtuele Machines biedt de mogelijkheid een antimalware-agent voor virtuele machines en PaaS-functies installeren. Op basis van System Center Endpoint Protection, biedt deze functie beproefde lokale beveiligingstechnologie voor de cloud.

We bieden ook diepe integratie voor de Trend [grondige beveiliging](http://www.trendmicro.com/us/enterprise/cloud-solutions/deep-security/) en [SecureCloud](http://www.trendmicro.com/us/enterprise/cloud-solutions/secure-cloud/) producten in de Azure-platform. Uitgebreide beveiliging is virussen te bestrijden en SecureCloud is een oplossing voor versleuteling. Uitgebreide beveiliging wordt geïmplementeerd in virtuele machines via een extensiemodel. Met behulp van de gebruikersinterface van Azure portal en PowerShell, kunt u uitgebreide beveiliging in nieuwe virtuele machines die worden ingezet, of een bestaande virtuele machines die al zijn geïmplementeerd.

Symantec Endpoint Protection (SEP) wordt ook ondersteund in Azure. U kunt opgeven dat u van plan bent te gebruiken SEP op een virtuele machine via de portal integratie. SEP kan worden geïnstalleerd op een nieuwe virtuele machine via de Azure-portal of deze kan worden geïnstalleerd op een bestaande virtuele machine via PowerShell.

Meer informatie:

* [Deploying Antimalware Solutions on Azure Virtual Machines](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/) (Antimalware-oplossingen implementeren op virtuele machines van Azure)
* [Microsoft Antimalware voor Azure-Cloudservices en virtuele Machines](azure-security-antimalware.md)
* [Het installeren en Trend Micro grondige Security configureren als een Service op een virtuele machine van Windows](../virtual-machines/windows/classic/install-trend.md)
* [Het installeren en configureren van Symantec Endpoint Protection op een virtuele machine van Windows](../virtual-machines/windows/classic/install-symantec.md)
* [Nieuwe Antimalware-opties voor het beveiligen van virtuele Machines in Azure](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication
Azure multi-factor Authentication is een authenticatiemethode die het gebruik van meer dan één verificatiemethode vereist. Een kritieke tweede beveiligingslaag wordt toegevoegd aan de gebruikersaanmeldingen en transacties. 

Multi-factor Authentication helpt beveiliging toegang tot gegevens en toepassingen en te voldoen aan de behoeften van de gebruiker voor een eenvoudig proces aanmelden. Sterke verificatie via een bereik van de verificatie-opties (telefoongesprek, tekstbericht of mobiele app-melding of verificatie code) en OATH-tokens van derde levert.

Meer informatie:

* [Multi-Factor Authentication](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
* [Wat is Azure Multi-Factor Authentication?](../active-directory/authentication/multi-factor-authentication.md)
* [De werking van Azure multi-factor Authentication](../active-directory/authentication/concept-mfa-howitworks.md)

## <a name="expressroute"></a>ExpressRoute
Azure ExpressRoute kunt u uw on-premises netwerken in de Microsoft Cloud uitbreiden via een speciale persoonlijke verbinding die wordt gefaciliteerd door een connectiviteitsprovider. Met ExpressRoute kunt kunt u verbindingen met Microsoft cloud-services zoals Azure, Office 365 en CRM Online maken. Connectiviteit mogelijk vanuit:

- Een any-to-any (IP VPN) netwerk.
- Een point-to-point Ethernet-netwerk.
- Een virtuele overlappende verbinding via een connectiviteitsprovider in een CO-locatiefaciliteit. 

ExpressRoute-verbindingen gaat niet via het openbare internet. Ze kunnen meer betrouwbaarheid, sneller en hebben ze lagere latenties en betere beveiliging dan gewone verbindingen via internet bieden.

Meer informatie:

* [Technisch overzicht van ExpressRoute](../expressroute/expressroute-introduction.md)

## <a name="virtual-network-gateways"></a>Virtuele netwerkgateways
VPN-gateways, ook wel Azure virtuele netwerkgateways worden gebruikt om netwerkverkeer tussen virtuele netwerken en lokale locaties te verzenden. Ze worden ook gebruikt om verkeer tussen meerdere virtuele netwerken in Azure (netwerk-netwerk) te verzenden. VPN-gateways bieden beveiligde cross-premises-connectiviteit tussen Azure en uw infrastructuur.

Meer informatie:

* [Over VPN-gateways](../vpn-gateway/vpn-gateway-about-vpngateways.md)
* [Overzicht van Azure-netwerk-beveiliging](security-network-overview.md)

## <a name="privileged-identity-management"></a>Privileged Identity Management
Gebruikers moeten soms bij het uitvoeren van bevoorrechte bewerkingen in Azure-resources of andere SaaS-toepassingen. Dit betekent vaak dat organisaties hem permanente bevoegde toegang in Azure Active Directory (Azure AD). 

Dit is een groeiende beveiligingsrisico voor cloud-gebaseerde bronnen omdat organisaties voldoende kunnen niet controleren wat gebruikers doen met hun bevoorrechte toegang. Als een gebruikersaccount met uitgebreide toegang is aangetast, kan die een inbreuk bovendien de algehele cloud beveiliging van een organisatie beïnvloeden. Azure AD Privileged Identity Management helpt bij het oplossen van dit risico door te verlagen van de blootstellingstijd van bevoegdheden en inzicht in gebruik te verhogen.  

Privileged Identity Management introduceert het concept van een tijdelijke beheerder voor een functie of 'just in time' beheerderstoegang. Dit type beheerder is een gebruiker die moet worden geactiveerd voor deze rol toegewezen voltooid. Het activeringsproces verandert de toewijzing van de gebruiker aan een rol in Azure AD van actief gedurende een opgegeven periode inactief periode.

Meer informatie:

* [Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-configure.md)
* [Aan de slag met Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-getting-started.md)

## <a name="identity-protection"></a>Identiteitsbeveiliging
Azure AD Identity Protection biedt een totaaloverzicht van verdachte activiteiten voor aanmelden en potentiële zwakke plekken in uw bedrijf beter te beschermen. Identity Protection detecteert verdachte activiteiten voor gebruikers en bevoegdheden (admin) identiteiten, op basis van signalen zoals:

- Beveiligingsaanvallen.
- Gelekte aanmeldingsreferenties.
- Aanmeldingen vanaf onbekende locaties en geïnfecteerde apparaten.

Dankzij de meldingen en aanbevolen herstel, helpt Identity Protection risico's in realtime te beperken. Gebruiker risico ernst wordt berekend. U kunt beleid op basis van risico om automatisch beveiliging toepassing toegang tot van toekomstige bedreigingen te configureren.

Meer informatie:

* [Azure Active Directory Identity Protection](../active-directory/active-directory-identityprotection.md)
* [Channel 9: Azure AD en Identity weergeven: Identity Protection Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="security-center"></a>Beveiligingscentrum
Azure Security Center helpt u bij het detecteren, voorkomen van en reageren op bedreigingen. Security Center biedt u grotere zichtbaarheid van en controle over de beveiliging van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen. Helpt het detecteren van bedreigingen die anders onopgemerkt en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen.

Security Center helpt u te optimaliseren en de beveiliging van uw Azure-resources door te controleren:

* Zodat u kunt beleid voor uw Azure-abonnementresources volgens definiëren:
  * Beveiliging van uw bedrijf nodig heeft.
  * Het type toepassingen of de vertrouwelijkheid van de gegevens in elk abonnement.
* Bewaking van de status van uw virtuele Azure-machines, netwerken en toepassingen.
* Een lijst met beveiligingswaarschuwingen, met inbegrip van waarschuwingen van geïntegreerde partneroplossingen met te leveren. Het bevat ook de informatie die u nodig hebt om snel onderzoek te een aanval en aanbevelingen voor het worden opgelost.

Meer informatie:

* [Inleiding tot Azure Security Center](../security-center/security-center-intro.md)

<!--Image references-->
[1]: ./media/security-management-and-monitoring-overview/shared-responsibility.png
