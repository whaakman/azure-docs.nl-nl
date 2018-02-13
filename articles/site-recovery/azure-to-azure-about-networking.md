---
title: Over netwerken in Azure-Azure-noodherstel met Azure Site Recovery | Microsoft Docs
description: Biedt een overzicht van netwerken voor de replicatie van Azure VM's met Azure Site Recovery.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/08/2018
ms.author: sujayt
ms.openlocfilehash: 5ce85761df4e0ad62c22a829f67464a3145fd827
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2018
---
# <a name="about-networking-in-azure-to-azure-replication"></a>Over netwerken in Azure-Azure-replicatie

>[!NOTE]
> Site Recovery replicatie voor virtuele machines in Azure is momenteel in preview.

In dit artikel biedt netwerken richtlijnen wanneer u bent bezig met repliceren en virtuele Azure-machines herstellen van de ene regio naar de andere met behulp van [Azure Site Recovery](site-recovery-overview.md). 

## <a name="before-you-start"></a>Voordat u begint

Meer informatie over hoe Site Recovery biedt voor herstel na noodgevallen voor [dit scenario](azure-to-azure-architecture.md).

## <a name="typical-network-infrastructure"></a>Een typische netwerkinfrastructuur

Het volgende diagram ziet u een typische Azure-omgeving, voor toepassingen die worden uitgevoerd op Azure Virtual machines:

![klant-omgeving](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

Als u Azure ExpressRoute of een VPN-verbinding vanuit uw on-premises netwerk naar Azure, wordt de omgeving ziet er als volgt:

![klant-omgeving](./media/site-recovery-azure-to-azure-architecture/source-environment-expressroute.png)

Netwerken zijn meestal beveiligd met behulp van firewalls en netwerkbeveiligingsgroepen (nsg's). Firewalls URL gebruiken of door op basis van IP verbinding met het netwerk beheren. Nsg's bevatten regels die gebruikmaken van IP-adresbereiken verbinding met het netwerk beheren.

>[!IMPORTANT]
> Met behulp van een geverifieerde proxyserver voor netwerkverbinding besturingselement wordt niet ondersteund door Site Recovery en replicatie kan niet worden ingeschakeld.


## <a name="outbound-connectivity-for-urls"></a>Uitgaande verbinding voor URL 's

Als u een URL gebaseerde, firewall-proxy om te bepalen van de uitgaande verbinding gebruikt, kunt u deze Site Recovery-URL's:


**URL** | **Details**  
--- | ---
*.blob.core.windows.net | Vereist dat gegevens kunnen worden geschreven naar de storage-account van de cache in het brongebied van de virtuele machine.
login.microsoftonline.com | Vereist voor autorisatie en verificatie met de Site Recovery-service-URL's.
*.hypervrecoverymanager.windowsazure.com | Vereist zodat de communicatie van Site Recovery service van de virtuele machine optreden kan.
*.servicebus.windows.net | Vereist zodat de Site Recovery-controle en diagnostische gegevens van de virtuele machine kunnen worden geschreven.

## <a name="outbound-connectivity-for-ip-address-ranges"></a>Uitgaande verbinding voor IP-adresbereiken

Als u een op basis van IP-firewallproxy of NSG-regels gebruikt voor het beheren van uitgaande verbinding, moeten deze IP-adresbereiken worden toegestaan.

- Alle IP-adresbereiken die met de bronlocatie overeenkomen.
    - U kunt downloaden via de [IP-adresbereiken](https://www.microsoft.com/download/confirmation.aspx?id=41653).
    - U moet deze adressen toestaan, zodat gegevens kunnen worden geschreven naar de cache storage-account van de virtuele machine.
- Alle IP-adresbereiken die met Office 365 overeenkomen [authenticatie en identiteit IP V4-eindpunten](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).
    - Als het nieuwe adres worden toegevoegd aan de Office 365-bereiken in de toekomst, moet u nieuwe NSG-regels te maken.
- Site Recovery service-eindpunt IP-adressen. Dit zijn beschikbaar in een [XML-bestand](https://aka.ms/site-recovery-public-ips), en zijn afhankelijk van uw target-locatie.
-  U kunt [downloaden en gebruiken van dit script](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702), automatisch de vereiste regels maken op het NSG. 
- U wordt aangeraden de vereiste NSG-regels maken op een test NSG en controleert u of er geen problemen voordat u de regels voor een productie-NSG maken.
- Voor het maken van het vereiste aantal NSG-regels, zorg ervoor dat uw abonnement wilt plaatsen. Neem contact op met Azure ondersteuning en verhoog de limiet van de regel NSG in uw abonnement.

IP-adresbereiken zijn als volgt:

>
   **doel** | **Site Recovery IP** |  **Site Recovery IP bewaken**
   --- | --- | ---
   Oost-Azië | 52.175.17.132 | 13.94.47.61
   Zuidoost-Azië | 52.187.58.193 | 13.76.179.223
   Centraal-India | 52.172.187.37 | 104.211.98.185
   Zuid-India | 52.172.46.220 | 104.211.224.190
   Noord-centraal VS | 23.96.195.247 | 168.62.249.226
   Noord-Europa | 40.69.212.238 | 52.169.18.8
   West-Europa | 52.166.13.64 | 40.68.93.145
   VS - oost | 13.82.88.226 | 104.45.147.24
   VS - west | 40.83.179.48 | 104.40.26.199
   Zuid-centraal VS | 13.84.148.14 | 104.210.146.250
   VS - midden | 40.69.144.231 | 52.165.34.144
   VS - oost 2 | 52.184.158.163 | 40.79.44.59
   Japan - oost | 52.185.150.140 | 138.91.1.105
   Japan - west | 52.175.146.69 | 138.91.17.38
   Brazilië - zuid | 191.234.185.172 | 23.97.97.36
   Australië - oost | 104.210.113.114 | 191.239.64.144
   Australië - zuidoost | 13.70.159.158 | 191.239.160.45
   Canada - midden | 52.228.36.192 | 40.85.226.62
   Canada - oost | 52.229.125.98 | 40.86.225.142
   West-centraal VS | 52.161.20.168 | 13.78.149.209
   VS - west 2 | 52.183.45.166 | 13.66.228.204
   Verenigd Koninkrijk West | 51.141.3.203 | 51.141.14.113
   Verenigd Koninkrijk Zuid | 51.140.43.158 | 51.140.189.52
   VK, zuid 2 | 13.87.37.4| 13.87.34.139
   VK, noord | 51.142.209.167 | 13.87.102.68
   Korea - centraal | 52.231.28.253 | 52.231.32.85
   Korea - zuid | 52.231.298.185 | 52.231.200.144
   
   
  

## <a name="example-nsg-configuration"></a>Voorbeeldconfiguratie voor NSG

In dit voorbeeld laat zien hoe NSG regels configureren voor een virtuele machine om te repliceren. 

- Als u NSG-regels waarmee uitgaande verbinding gebruikt, gebruikt u regels voor 'Toestaan HTTPS uitgaande' voor alle de vereiste IP-adresbereiken.
- In het voorbeeld wordt ervan uitgegaan dat de locatie van de virtuele machine 'Oost ons' en de doellocatie 'VS-midden is.

### <a name="nsg-rules---east-us"></a>NSG-regels - VS-Oost

1. Maken van regels die met overeenkomen [Oost ons IP-adresbereiken](https://www.microsoft.com/download/confirmation.aspx?id=41653). Dit is vereist zodat gegevens kunnen worden geschreven naar het opslagaccount van de cache van de virtuele machine.
2. Regels maken voor alle IP-adresbereiken die met Office 365 overeenkomen [authenticatie en identiteit IP V4-eindpunten](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).
3. Regels die met de doellocatie overeenkomen te maken:

   **Locatie** | **Site Recovery-IP-adres** |  **Site Recovery bewaking IP-adres**
    --- | --- | ---
   VS - midden | 40.69.144.231 | 52.165.34.144

### <a name="nsg-rules---central-us"></a>NSG-regels - VS-midden 

Deze regels zijn vereist voor replicatie naar de bron regio na een failover van de doelregio kan worden ingeschakeld:

* Regels die met overeenkomen [centraal VS-IP-bereiken](https://www.microsoft.com/download/confirmation.aspx?id=41653). Dit zijn vereist, zodat gegevens kunnen worden geschreven naar het opslagaccount van de cache van de virtuele machine.

* Regels voor alle IP-adresbereiken die met Office 365 overeenkomen [authenticatie en identiteit IP V4-eindpunten](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).

* Regels die met de bronlocatie overeenkomen:
    - VS - oost
    - Site Recovery-IP-adres: 13.82.88.226
    - Site Recovery bewaking van IP-adres: 104.45.147.24


## <a name="expressroutevpn"></a>ExpressRoute/VPN 

Als u een ExpressRoute- of VPN-verbinding tussen on-premises en Azure-locatie hebt, volgt u de richtlijnen in deze sectie.

### <a name="forced-tunneling"></a>Geforceerde tunneling

Normaal gesproken definieert u een standaardroute (0.0.0.0/0) dat ervoor zorgt uitgaand internetverkeer dat stromen via de on-premises locatie. We raden niet dit. Laat het replicatieverkeer en communicatie van Site Recovery-service niet de grens van Azure. De oplossing is het toevoegen van de gebruiker gedefinieerde routes (udr's) voor [deze IP-adresbereiken](#outbound-connectivity-for-azure-site-recovery-ip-ranges) zodat het replicatieverkeer geen lokale gaan.

### <a name="connectivity"></a>Connectiviteit 

Volg deze richtlijnen voor verbindingen tussen de doellocatie en de on-premises-locatie:
- Als uw toepassing moet verbinding maken met de lokale machines of als er clients die verbinding met de toepassing van on-premises via VPN en ExpressRoute maken, zorg ervoor dat er ten minste een [site-naar-site-verbinding](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) tussen het doel-Azure-regio en de on-premises datacentrum.

- Als u verwacht veel verkeer tussen het doel-Azure-regio en de on-premises datacentrum dat, moet u een andere maken [ExpressRoute-verbinding](../expressroute/expressroute-introduction.md) tussen de doel-Azure-regio en de on-premises datacentrum.

- Als u behouden die IP-adressen voor de virtuele machines wilt nadat ze een failover, houd de doelregio site-naar-site en ExpressRoute-verbinding verbroken. Dit is om te controleren of er is geen bereik-conflict tussen de bron-regio IP-adresbereiken en van de doelregio-IP-adresbereiken.

### <a name="expressroute-configuration"></a>De configuratie van ExpressRoute
Volg deze aanbevolen procedures voor de ExpressRoute-configuratie:

- U moet een ExpressRoute-circuit maken in zowel de bron- en regio's. Vervolgens moet u een verbinding maken tussen:
  - Het virtuele netwerk van de bron en het ExpressRoute-circuit.
  - Het virtuele netwerk en het ExpressRoute-circuit.

- U kunt als onderdeel van de standaard ExpressRoute circuits maken in dezelfde geopolitieke regio. Voor het maken van ExpressRoute-circuits in andere geopolitieke regio's, Azure ExpressRoute Premium is vereist, die betrekking heeft op een incrementele kosten. (Als u al van ExpressRoute Premium gebruikmaakt, er is geen extra kosten verbonden.) Zie voor meer informatie de [ExpressRoute-locaties document](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) en [ExpressRoute prijzen](https://azure.microsoft.com/pricing/details/expressroute/).

- Het is raadzaam dat u verschillende IP-adresbereiken in bron en doel-regio's. Het ExpressRoute-circuit niet mogelijk om te verbinden met twee virtuele Azure-netwerken met de dezelfde IP-adresbereiken op hetzelfde moment.

- U kunt virtuele netwerken maken met de dezelfde IP-adresbereiken in beide regio's en maak vervolgens ExpressRoute-circuits in beide regio's. Verbreek de verbinding tussen het circuit het virtuele netwerk van de bron in het geval van een failover en verbinding maken met het circuit in het virtuele netwerk.

 >[!IMPORTANT]
 > Als de primaire regio volledig niet actief is, kan de bewerking van de verbinding verbreken mislukken. Die wordt voorkomen dat het virtuele doelnetwerk ExpressRoute-verbinding ophalen.

## <a name="next-steps"></a>Volgende stappen
Beginnen met het beveiligen van uw werkbelastingen door [virtuele Azure-machines repliceren](site-recovery-azure-to-azure.md).
