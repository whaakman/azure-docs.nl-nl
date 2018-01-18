---
title: 'Azure AD Domain Services: Netwerken richtlijnen | Microsoft Docs'
description: Overwegingen voor Azure Active Directory Domain Services netwerken
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 23a857a5-2720-400a-ab9b-1ba61e7b145a
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: maheshu
ms.openlocfilehash: a6f0089f13de10ba8bc1f9a656a2d21f9c559047
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="networking-considerations-for-azure-ad-domain-services"></a>Overwegingen voor Azure AD Domain Services netwerken
## <a name="how-to-select-an-azure-virtual-network"></a>Het selecteren van een virtuele Azure-netwerk
De volgende richtlijnen kunnen u een virtueel netwerk gebruiken met Azure AD Domain Services selecteren.

### <a name="type-of-azure-virtual-network"></a>Type virtuele Azure-netwerk
* **Virtuele netwerken van Resource Manager**: Azure AD Domain Services kunnen worden ingeschakeld in virtuele netwerken die zijn gemaakt met Azure Resource Manager.
* U kunt Azure AD Domain Services in de klassieke Azure-netwerk niet inschakelen.
* U kunt andere virtuele netwerken kunt verbinden met het virtuele netwerk waarin Azure AD Domain Services is ingeschakeld. Zie voor meer informatie de [netwerkverbinding](active-directory-ds-networking.md#network-connectivity) sectie.

### <a name="azure-region-for-the-virtual-network"></a>Azure-regio voor het virtuele netwerk
* Uw Azure AD Domain Services beheerd domein wordt geïmplementeerd in dezelfde Azure-regio als het virtuele netwerk dat u wilt de service in te schakelen.
* Selecteer een virtueel netwerk in een Azure-regio wordt ondersteund door Azure AD Domain Services.
* Zie de pagina [Azure-services per regio](https://azure.microsoft.com/regions/#services/) om te bekijken in welke Azure-regio's Azure AD Domain Services beschikbaar is.

### <a name="requirements-for-the-virtual-network"></a>Vereisten voor het virtuele netwerk
* **Nabijheid tot de werkbelasting van uw Azure**: Selecteer het virtuele netwerk dat momenteel fungeert als host/host fungeert voor virtuele machines die toegang nodig tot Azure AD Domain Services. Als uw werkbelastingen zijn geïmplementeerd in een ander virtueel netwerk dan het beheerde domein, kunt u ook verbinding maken met de virtuele netwerken.
* **DNS-servers aangepaste/bring your own**: Zorg ervoor dat er geen aangepaste DNS-servers geconfigureerd voor het virtuele netwerk. Een voorbeeld van een aangepaste DNS-server is een exemplaar van Windows Server DNS uitgevoerd op een Windows Server-VM die u hebt geïmplementeerd in het virtuele netwerk. Azure AD Domain Services is niet geïntegreerd met aangepaste DNS-servers binnen het virtuele netwerk is geïmplementeerd.
* **Bestaande domeinen met dezelfde domeinnaam**: Zorg ervoor dat u geen een bestaand domein met dezelfde domeinnaam beschikbaar is op dit virtuele netwerk hebt. Het domein met de naam contoso.com is bijvoorbeeld al beschikbaar op het geselecteerde virtuele netwerk. Later kunt proberen u om in te schakelen van een beheerd domein van Azure AD Domain Services met dezelfde domeinnaam (dat wil zeggen 'contoso.com') op dit virtuele netwerk. U kunt een fout optreedt tijdens het inschakelen van Azure AD Domain Services. Deze fout komt door naamconflicten voor de domeinnaam op dit virtuele netwerk. In dit geval moet u een andere naam gebruiken om het beheerde domein van Azure AD Domain Services in te stellen. U kunt ook de inrichting van het bestaande domein ongedaan maken en doorgaan met het inschakelen van Azure AD Domain Services.

> [!WARNING]
> U kunt Domain Services niet verplaatsen naar een ander virtueel netwerk nadat u de service hebt ingeschakeld.
>
>


## <a name="guidelines-for-choosing-a-subnet"></a>Richtlijnen voor het kiezen van een subnet

![Aanbevolen subnet ontwerp](./media/active-directory-domain-services-design-guide/vnet-subnet-design.png)

* Azure AD Domain Services te implementeren een **scheiden toegewezen subnet** binnen uw virtuele Azure-netwerk.
* Nsg's zijn niet van toepassing met het toegewezen subnet voor uw beheerde domein. Als het nsg's moet u op het toegewezen subnet toepassen, moet u **niet de vereiste poorten voor service blokkeren en beheren van uw domein**.
* Niet te beperken het aantal IP-adressen beschikbaar zijn binnen de toegewezen subnet voor uw beheerde domein. Deze beperking wordt voorkomen dat de service twee domeincontrollers beschikbaar maken voor uw beheerde domein.
* **Schakel Azure AD Domain Services in het gatewaysubnet geen** van het virtuele netwerk.

> [!WARNING]
> Als u koppelt een NSG aan een subnet waarin Azure AD Domain Services is ingeschakeld, u de mogelijkheid van Microsoft-service en beheren van het domein kan verstoren. Bovendien wordt de synchronisatie tussen uw Azure AD-tenant en uw beheerde domein onderbroken. **De SLA niet van toepassing op implementaties waarbij een NSG is toegepast, die blokkeert Azure AD Domain Services uit bijwerken en het beheren van uw domein.**
>
>

## <a name="ports-required-for-azure-ad-domain-services"></a>Poorten die nodig zijn voor Azure AD Domain Services
De volgende poorten zijn vereist voor de service Azure AD Domain Services en onderhouden van uw beheerde domein. Zorg ervoor dat deze poorten worden niet geblokkeerd voor het subnet waarin u uw beheerde domein hebt ingeschakeld.

| Poortnummer | Vereist? | Doel |
| --- | --- | --- |
| 443 | Verplicht |Synchronisatie met uw Azure AD-tenant |
| 5986 | Verplicht | Beheer van uw domein |
| 3389 | Optioneel | Beheer van uw domein |
| 636 | Optioneel | Beveiligde LDAP (LDAPS) toegang tot uw beheerde domein |

**Poort 443 (synchronisatie met Azure AD)**
* Deze wordt gebruikt om uw Azure AD-directory worden gesynchroniseerd met uw beheerde domein.
* Het is verplicht voor toegang tot deze poort in uw NSG. Uw beheerde domein is niet gesynchroniseerd met uw Azure AD-directory zonder toegang tot deze poort. Gebruikers mogelijk niet kunnen aanmelden als wijzigingen hun wachtwoorden niet gesynchroniseerd met uw beheerde domein.
* U kunt binnenkomende toegang beperken tot deze poort voor IP-adressen die horen bij de Azure-IP-adresbereik.

**Poort 5986 (PowerShell voor externe toegang)**
* Deze wordt gebruikt voor het uitvoeren van beheertaken met behulp van PowerShell voor externe toegang op uw beheerde domein.
* Het is verplicht om toegang te verlenen via deze poort in uw NSG. Uw beheerde domein kan niet zonder toegang tot deze poort worden bijgewerkt, geconfigureerde, back-up of bewaakte.
* U kunt binnenkomende toegang beperken tot deze poort voor de volgende bron-IP-adressen: 52.180.183.8, 23.101.0.70, 52.225.184.198, 52.179.126.223, 13.74.249.156, 52.187.117.83, 52.161.13.95, 104.40.156.18, 104.40.87.209, 52.180.179.108, 52.175.18.134, 52.138.68.41, 104.41.159.212, 52.169.218.0, 52.187.120.237, 52.161.110.169, 52.174.189.149, 13.64.151.161
* De domeincontrollers voor uw beheerde domein luisteren meestal niet op deze poort. De service Hiermee opent u deze poort op beheerde domeincontrollers alleen wanneer er een beheer- of -bewerking moet worden uitgevoerd voor het beheerde domein. Als de bewerking is voltooid, wordt de service afgesloten deze poort op de beheerde domeincontrollers.

**Poort 3389 (Remote desktop)**
* Het wordt gebruikt voor extern bureaublad-verbindingen naar de domeincontrollers voor uw beheerde domein.
* Deze poort via uw NSG te openen is optioneel.
* Deze poort ook blijft grotendeels is uitgeschakeld op uw beheerde domein. Dit mechanisme wordt niet gebruikt voortdurend omdat beheer en controle van taken worden uitgevoerd met behulp van PowerShell voor externe toegang. Deze poort wordt alleen gebruikt in het zeldzame geval dat Microsoft extern verbinding maken met uw beheerde domein moet voor geavanceerde probleemoplossing. De poort is gesloten, zodra de probleemoplossing bewerking voltooid is.

**Port 636 (Secure LDAP)**
* Deze wordt gebruikt voor het inschakelen van beveiligde LDAP-toegang tot uw beheerde domein via internet.
* Deze poort via uw NSG te openen is optioneel. Open de poort alleen als u beveiligde LDAP toegang via het internet is ingeschakeld.
* U kunt binnenkomende toegang beperken tot deze poort voor de bron-IP-adressen van waaruit u verbinding maken via een veilige LDAP verwacht.


## <a name="network-security-groups"></a>Netwerkbeveiligingsgroepen
Een [Netwerkbeveiligingsgroep (NSG)](../virtual-network/virtual-networks-nsg.md) bevat een lijst met regels voor lijst ACL (Access Control) toestaan of weigeren van netwerkverkeer naar uw VM-exemplaren in een virtueel netwerk. NSG's kunnen worden gekoppeld aan subnetten of afzonderlijke VM-exemplaren in dat subnet. Als een NSG is gekoppeld aan een subnet, zijn de ACL-regels van toepassing op alle VM-exemplaren in dat subnet. Bovendien het verkeer naar een afzonderlijke virtuele machine kan worden beperkt door een NSG rechtstreeks aan die VM koppelen verdere.

### <a name="sample-nsg-for-virtual-networks-with-azure-ad-domain-services"></a>Voorbeeld NSG voor virtuele netwerken met Azure AD Domain Services
De volgende tabel ziet u een voorbeeld van een NSG die u voor een virtueel netwerk met een beheerd domein van Azure AD Domain Services configureren kunt. Deze regel kunnen binnenkomend verkeer via de vereiste poorten om te controleren of uw beheerde domein blijft een patch uitgevoerd, bijgewerkt en kan worden bewaakt door Microsoft. De 'DenyAll' standaardregel geldt voor alle binnenkomend verkeer van het internet.

Het NSG ziet ook hoe vergrendelen beveiligde LDAP toegang via internet. Deze regel overslaan als u beveiligde LDAP toegang niet hebt ingeschakeld voor uw beheerde domein via internet. Het NSG bevat een reeks regels waarmee binnenkomende LDAPS toegang via TCP-poort 636 alleen uit een opgegeven set van IP-adressen. De regel NSG LDAPS om toegang te verlenen via het internet vanaf de opgegeven IP-adressen heeft een hogere prioriteit dan de DenyAll NSG-regel.

![Voorbeeld NSG LDAPS om toegang te beveiligen via internet](.\media\active-directory-domain-services-alerts\default-nsg.png)

**Meer informatie** - [maken van een Netwerkbeveiligingsgroep](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).


## <a name="network-connectivity"></a>Netwerkverbinding
Een beheerd domein van Azure AD Domain Services kan alleen binnen één virtueel netwerk in Azure worden ingeschakeld.

### <a name="scenarios-for-connecting-azure-networks"></a>Scenario's voor het verbinden van netwerken in Azure
Verbinding maken met virtuele Azure-netwerken voor het gebruik van het beheerde domein in een van de volgende implementatiescenario's:

#### <a name="use-the-managed-domain-in-more-than-one-azure-virtual-network"></a>Gebruik het beheerde domein in meer dan één virtuele Azure-netwerk
U kunt andere virtuele netwerken van Azure verbinden met het Azure-netwerk waarin u Azure AD Domain Services hebt ingeschakeld. Deze verbinding van VPN-/ VNet-peering kunt u het beheerde domein met uw werkbelastingen die zijn geïmplementeerd in een andere virtuele netwerken gebruiken.

![Klassieke virtuele netwerkverbindingen](./media/active-directory-domain-services-design-guide/classic-vnet-connectivity.png)

#### <a name="use-the-managed-domain-in-a-resource-manager-based-virtual-network"></a>Het beheerde domein in een virtueel netwerk met Resource Manager gebaseerde gebruiken
U kunt een Resource Manager gebaseerde virtueel netwerk verbinding maken met de Azure klassiek virtueel netwerk waarin u Azure AD Domain Services hebt ingeschakeld. Deze verbinding kunt u het beheerde domein met uw werkbelastingen die zijn geïmplementeerd in de Resource Manager gebaseerde virtueel netwerk gebruiken.

![Resource Manager klassiek virtueel netwerk verbinding](./media/active-directory-domain-services-design-guide/classic-arm-vnet-connectivity.png)

### <a name="network-connection-options"></a>Opties voor een netwerkverbinding
* **VNet-naar-VNet-verbindingen met virtuele netwerk peering**: peering virtueel netwerk is een mechanisme dat verbinding twee virtuele netwerken in dezelfde regio via het Azure-backbone-netwerk maakt. Als de twee virtuele netwerken gekoppeld zijn, worden deze als één netwerk weergegeven voor alle verbindingsdoeleinden. Ze worden nog steeds beheerd als afzonderlijke resources, maar virtuele machines in deze virtuele netwerken kunnen met elkaar communiceren via privé-IP-adressen.

    ![Verbinding met het virtuele netwerk met behulp van peering](./media/active-directory-domain-services-design-guide/vnet-peering.png)

    [Meer informatie - virtuele netwerk peering](../virtual-network/virtual-network-peering-overview.md)

* **VNet-naar-VNet-verbindingen met behulp van site-naar-site VPN-verbindingen**: een virtueel netwerk verbinden met een ander virtueel netwerk (VNet-naar-VNet) is vergelijkbaar met een virtueel netwerk verbinden met een on-premises locatie. Voor beide connectiviteitstypen wordt een VPN-gateway gebruikt om een beveiligde tunnel met IPsec/IKE te bieden.

    ![Verbinding met het virtuele netwerk met behulp van VPN-Gateway](./media/active-directory-domain-services-design-guide/vnet-connection-vpn-gateway.jpg)

    [Meer informatie - verbinding maken met virtuele netwerken met behulp van VPN-gateway](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)

<br>

## <a name="related-content"></a>Gerelateerde inhoud
* [Virtueel netwerk van Azure-peering](../virtual-network/virtual-network-peering-overview.md)
* [Een VNet-naar-VNet-verbinding voor het klassieke implementatiemodel configureren](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)
* [Beveiligingsgroepen voor Azure-netwerk](../virtual-network/virtual-networks-nsg.md)
* [Een Netwerkbeveiligingsgroep maken](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)
