---
title: 'Azure AD Domain Services: Richtlijnen voor de netwerken | Microsoft Docs'
description: Aandachtspunten voor netwerken voor Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: 23a857a5-2720-400a-ab9b-1ba61e7b145a
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/01/2017
ms.author: ergreenl
ms.openlocfilehash: eb97e709e18daba3722dc43a869ef034dbe573cf
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50157425"
---
# <a name="networking-considerations-for-azure-ad-domain-services"></a>Aandachtspunten voor netwerken voor Azure AD Domain Services
## <a name="how-to-select-an-azure-virtual-network"></a>Een Azure-netwerk selecteren
De volgende richtlijnen waarmee u kunt selecteren van een virtueel netwerk gebruiken met Azure AD Domain Services.

### <a name="type-of-azure-virtual-network"></a>Type van de Azure-netwerk
* **Virtuele netwerken van Resource Manager**: Azure AD Domain Services kunnen worden ingeschakeld in virtuele netwerken die zijn gemaakt met behulp van Azure Resource Manager.
* U kunt Azure AD Domain Services in een klassieke Azure-netwerk niet inschakelen.
* U kunt andere virtuele netwerken verbinden met het virtuele netwerk waarin Azure AD Domain Services is ingeschakeld. Zie voor meer informatie de [netwerkverbinding](active-directory-ds-networking.md#network-connectivity) sectie.

### <a name="azure-region-for-the-virtual-network"></a>Azure-regio voor het virtuele netwerk
* Uw Azure AD Domain Services beheerde domein wordt geïmplementeerd in dezelfde Azure-regio als het virtuele netwerk dat u wilt de service in te schakelen.
* Selecteer een virtueel netwerk in een Azure-regio worden ondersteund door Azure AD Domain Services.
* Zie de pagina [Azure-services per regio](https://azure.microsoft.com/regions/#services/) om te bekijken in welke Azure-regio's Azure AD Domain Services beschikbaar is.

### <a name="requirements-for-the-virtual-network"></a>Vereisten voor het virtuele netwerk
* **Nabijheid van uw Azure-workloads**: Selecteer het virtuele netwerk dat momenteel fungeert als host voor/virtuele machines die toegang nodig tot de Azure AD Domain Services host. Als uw workloads worden geïmplementeerd in een ander virtueel netwerk dan het beheerde domein, kunt u ook om de virtuele netwerken te verbinden.
* **DNS-servers aangepaste/bring your own**: Zorg ervoor dat er geen aangepaste DNS-servers zijn geconfigureerd voor het virtuele netwerk. Een voorbeeld van een aangepaste DNS-server is een exemplaar van Windows Server DNS die worden uitgevoerd op een Windows Server-VM die u hebt geïmplementeerd in het virtuele netwerk. Azure AD Domain Services is niet geïntegreerd met alle aangepaste DNS-servers binnen het virtuele netwerk is geïmplementeerd.
* **Bestaande domeinen met dezelfde domeinnaam**: Zorg ervoor dat u geen bestaand domein met dezelfde domeinnaam beschikbaar is op dit virtuele netwerk. Het domein met de naam contoso.com is bijvoorbeeld al beschikbaar op het geselecteerde virtuele netwerk. Later kunt proberen u om in te schakelen van een Azure AD Domain Services beheerde domein met dezelfde domeinnaam (dat wil zeggen 'contoso.com') op dit virtuele netwerk. U kunt een fout optreedt tijdens het inschakelen van Azure AD Domain Services. Deze fout is ontstaan door naamconflicten voor de domeinnaam op dit virtuele netwerk. In dit geval moet u een andere naam gebruiken om het beheerde domein van Azure AD Domain Services in te stellen. U kunt ook de inrichting van het bestaande domein ongedaan maken en doorgaan met het inschakelen van Azure AD Domain Services.

> [!WARNING]
> U kunt Domain Services niet verplaatsen naar een ander virtueel netwerk wanneer u de service hebt ingeschakeld.
>
>


## <a name="guidelines-for-choosing-a-subnet"></a>Richtlijnen voor het kiezen van een subnet

![Aanbevolen subnet ontwerp](./media/active-directory-domain-services-design-guide/vnet-subnet-design.png)

* Azure AD Domain Services te implementeren een **scheiden toegewezen subnet** binnen uw Azure-netwerk.
* Pas geen nsg's aan het toegewezen subnet voor uw beheerde domein. Als u nsg's op het toegewezen subnet toepassen moet, controleert u of u **niet de vereiste poorten voor de service blokkeren en beheren van uw domein**.
* Niet te beperken het aantal IP-adressen beschikbaar zijn binnen de toegewezen subnet voor uw beheerde domein. Deze beperking voorkomt dat de service zodat twee domeincontrollers beschikbaar zijn voor uw beheerde domein.
* **Schakel Azure AD Domain Services in het gatewaysubnet geen** van uw virtuele netwerk.

> [!WARNING]
> Als u koppelt een NSG aan een subnet waarin Azure AD Domain Services is ingeschakeld, de mogelijkheid van Microsoft-service en het beheren van het domein kan worden onderbroken. Bovendien wordt de synchronisatie tussen uw Azure AD-tenant en uw beheerde domein onderbroken. **De SLA geldt niet voor implementaties waarbij een NSG is toegepast die blokkeert Azure AD Domain Services van bijwerken en beheren van uw domein.**
>
>

## <a name="ports-required-for-azure-ad-domain-services"></a>Poorten die nodig zijn voor Azure AD Domain Services
De volgende poorten zijn vereist voor Azure AD Domain Services naar service en onderhouden van uw beheerde domein. Zorg ervoor dat deze poorten niet worden geblokkeerd voor het subnet waarin u uw beheerde domein hebt ingeschakeld.

| Poortnummer | Vereist? | Doel |
| --- | --- | --- |
| 443 | Verplicht |Synchronisatie met uw Azure AD-tenant |
| 5986 | Verplicht | Beheer van uw domein |
| 3389 | Verplicht | Beheer van uw domein |
| 636 | Optioneel | Beveiligde LDAP (LDAPS) toegang tot uw beheerde domein |

**Poort 443 (synchronisatie met Azure AD)**
* Deze wordt gebruikt om uw Azure AD-directory worden gesynchroniseerd met uw beheerde domein.
* Het is verplicht voor toegang tot deze poort in uw NSG. Geen toegang tot deze poort is uw beheerde domein niet gesynchroniseerd met uw Azure AD-directory. Gebruikers zijn mogelijk niet aanmelden als wijzigingen in hun wachtwoorden worden niet gesynchroniseerd naar uw beheerde domein.
* U kunt binnenkomende toegang beperken tot deze poort voor IP-adressen die behoren tot het Azure-IP-adresbereik. Houd er rekening mee dat het Azure-IP-adresbereik een ander bereik dan het PowerShell-bereik dat wordt weergegeven in de onderstaande regel is.

**Poort 5986 (externe communicatie van PowerShell)**
* Het is gebruikt voor het uitvoeren van beheertaken met behulp van PowerShell voor externe toegang in uw beheerde domein.
* Het is verplicht om toegang te verlenen via deze poort in uw NSG. Zonder toegang tot deze poort worden uw beheerde domein kan niet worden bijgewerkt, geconfigureerde, waarvan een back-up is gemaakt of bewaakte.
* U kunt voor elke nieuwe domeinen of domeinen met een virtueel netwerk van Azure Resource Manager, binnenkomende toegang beperken tot deze poort voor de volgende IP-adressen van bron: 52.180.179.108, 52.180.177.87, 13.75.105.168, 52.175.18.134, 52.138.68.41, 52.138.65.157, 104.41.159.212, 104.45.138.161, 52.169.125.119, 52.169.218.0, 52.187.19.1, 52.187.120.237, 13.78.172.246, 52.161.110.169, 52.174.189.149, 40.68.160.142, 40.83.144.56, 13.64.151.161, 52.180.183.67, 52.180.181.39, 52.175.28.111, 52.175.16.141 , 52.138.70.93, 52.138.64.115, 40.80.146.22, 40.121.211.60, 52.138.143.173, 52.169.87.10, 13.76.171.84, 52.187.169.156, 13.78.174.255, 13.78.191.178, 40.68.163.143, 23.100.14.28, 13.64.188.43, 23.99.93.197
* Voor domeinen met een klassiek virtueel netwerk, kunt u inkomende toegang beperken tot deze poort voor de volgende IP-adressen van bron: 52.180.183.8, 23.101.0.70, 52.225.184.198, 52.179.126.223, 13.74.249.156, 52.187.117.83, 52.161.13.95, 104.40.156.18, 104.40.87.209
* De domeincontrollers voor uw beheerde domein luisteren meestal niet op deze poort. De service wordt deze poort op beheerde domeincontrollers geopend, alleen wanneer een bewerking voor management of onderhoud moet worden uitgevoerd voor het beheerde domein. Zodra de bewerking is voltooid, wordt de service wordt uitgeschakeld deze poort op de beheerde domeincontrollers.

**Poort 3389 (extern bureaublad)**
* Deze wordt gebruikt voor verbindingen met extern bureaublad naar domeincontrollers voor uw beheerde domein.
* U kunt binnenkomende toegang tot de volgende bron-IP-adressen beperken: 207.68.190.32/27, 13.106.78.32/27, 13.106.174.32/27, 13.106.4.96/27
* Deze poort ook blijft grotendeels is uitgeschakeld op uw beheerde domein. Dit mechanisme wordt niet regelmatig gebruikt, omdat beheer en controle van taken worden uitgevoerd met behulp van PowerShell voor externe toegang. Deze poort wordt gebruikt alleen in het zeldzame geval die Microsoft nader moet extern verbinding maken met uw beheerde domein voor geavanceerde probleemoplossing. De poort is gesloten zodra de bewerking voor het oplossen van problemen voltooid is.

**Poort 636 (Secure LDAP)**
* Deze wordt gebruikt voor toegang van secure LDAP tot uw beheerde domein via internet.
* U opent deze poort door de Netwerkbeveiligingsgroep is optioneel. Open de poort alleen als u toegang van secure LDAP via internet ingeschakeld.
* U kunt binnenkomende toegang beperken tot deze poort voor de bron-IP-adressen van waaruit u verbinding maken via secure LDAP verwacht.


## <a name="network-security-groups"></a>Netwerkbeveiligingsgroepen
Een [Netwerkbeveiligingsgroep (NSG)](../virtual-network/virtual-networks-nsg.md) bevat een overzicht van de lijst met ACL (Access Control) regels voor toestaan of weigeren van netwerkverkeer naar uw VM-exemplaren in een Virtueelnetwerk. NSG's kunnen worden gekoppeld aan subnetten of afzonderlijke VM-exemplaren in dat subnet. Als een NSG is gekoppeld aan een subnet, zijn de ACL-regels van toepassing op alle VM-exemplaren in dat subnet. Bovendien verkeer naar een afzonderlijke virtuele machine kan worden beperkt verder door een NSG rechtstreeks naar die virtuele machine koppelen.

### <a name="sample-nsg-for-virtual-networks-with-azure-ad-domain-services"></a>Voorbeeld van een NSG voor virtuele netwerken met Azure AD Domain Services
De volgende tabel ziet u een voorbeeld van een NSG die u voor een virtueel netwerk met een Azure AD Domain Services beheerde domein configureren kunt. Deze regel kunt inkomend verkeer via de vereiste poorten om te controleren of uw beheerde domein blijven gevuld, bijgewerkt en kan worden bewaakt door Microsoft. De standaardregel voor 'DenyAll' is van toepassing op alle andere binnenkomend verkeer van internet.

Bovendien ziet u de NSG ook palekar hoe toegang van secure LDAP via internet. Als u toegang van secure LDAP niet hebt ingeschakeld voor uw beheerde domein via internet, kunt u deze regel overslaan. De NSG bevat een set regels die binnenkomend LDAPS toegang toestaan via TCP-poort 636 alleen uit een opgegeven set van IP-adressen. De NSG-regel om toe te staan van LDAPS toegang via internet vanaf een opgegeven IP-adressen heeft een hogere prioriteit dan de DenyAll NSG-regel.

![Voorbeeld NSG voor het beveiligen van LDAPS toegang via internet](.\media\active-directory-domain-services-alerts\default-nsg.png)

**Meer informatie** - [maken van een Netwerkbeveiligingsgroep](../virtual-network/manage-network-security-group.md).


## <a name="network-connectivity"></a>Verbinding met het netwerk
Een beheerd domein van Azure AD Domain Services kan alleen binnen één virtueel netwerk in Azure worden ingeschakeld.

### <a name="scenarios-for-connecting-azure-networks"></a>Scenario's voor het verbinden van netwerken van Azure
Verbinding maken met virtuele Azure-netwerken voor het gebruik van het beheerde domein in een van de volgende implementatiescenario's:

#### <a name="use-the-managed-domain-in-more-than-one-azure-virtual-network"></a>Gebruik het beheerde domein in meer dan één Azure-netwerk
U kunt andere Azure-netwerken verbinden met de Azure-netwerk waarin u Azure AD Domain Services hebt ingeschakeld. Deze VPN-/ VNet-peeringverbinding kunt u het gebruik van het beheerde domein met uw workloads die zijn geïmplementeerd in een andere virtuele netwerken.

![Verbinding met het klassieke virtuele netwerk](./media/active-directory-domain-services-design-guide/classic-vnet-connectivity.png)

#### <a name="use-the-managed-domain-in-a-resource-manager-based-virtual-network"></a>Het beheerde domein in een virtueel netwerk met Resource Manager gebaseerde gebruiken
U kunt een Resource Manager gebaseerd virtueel netwerk verbinden met het Azure klassieke virtuele netwerk waarin u Azure AD Domain Services hebt ingeschakeld. Deze verbinding kunt u het gebruik van het beheerde domein met uw workloads die zijn geïmplementeerd in het Resource Manager gebaseerde virtuele netwerk.

![Resource Manager kan verbinding met het klassieke virtuele netwerk](./media/active-directory-domain-services-design-guide/classic-arm-vnet-connectivity.png)

### <a name="network-connection-options"></a>Opties voor een netwerkverbinding
* **VNet-naar-VNet-verbindingen met behulp van virtual network-peering**: peering op virtueel netwerk is een mechanisme dat twee virtuele netwerken in dezelfde regio via het Azure-backbone-netwerk verbindt. Als de twee virtuele netwerken gekoppeld zijn, worden deze als één netwerk weergegeven voor alle verbindingsdoeleinden. Ze worden nog steeds beheerd als afzonderlijke resources, maar virtuele machines in deze virtuele netwerken kunnen met elkaar communiceren via privé-IP-adressen.

    ![Verbinding met het virtuele netwerk met behulp van peering](./media/active-directory-domain-services-design-guide/vnet-peering.png)

    [Meer informatie: virtual network-peering](../virtual-network/virtual-network-peering-overview.md)

* **VNet-naar-VNet-verbindingen met behulp van site-naar-site VPN-verbindingen**: een virtueel netwerk verbinden met een ander virtueel netwerk (VNet-naar-VNet) is vergelijkbaar met een virtueel netwerk verbinden met een on-premises locatie. Voor beide connectiviteitstypen wordt een VPN-gateway gebruikt om een beveiligde tunnel met IPsec/IKE te bieden.

    ![Verbinding met het virtuele netwerk met behulp van VPN-Gateway](./media/active-directory-domain-services-design-guide/vnet-connection-vpn-gateway.jpg)

    [Meer informatie - verbinding maken met virtuele netwerken met behulp van VPN-gateway](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)

<br>

## <a name="related-content"></a>Gerelateerde inhoud
* [Peering op virtueel netwerk](../virtual-network/virtual-network-peering-overview.md)
* [Een VNet-naar-VNet-verbinding voor het klassieke implementatiemodel configureren](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)
* [Azure Network Security Groups](../virtual-network/security-overview.md)
* [Een Netwerkbeveiligingsgroep maken](../virtual-network/manage-network-security-group.md)
