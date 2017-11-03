---
title: Azure Site Recovery networking richtlijnen voor het repliceren van virtuele machines van Azure naar Azure | Microsoft Docs
description: Richtlijnen voor het repliceren van virtuele machines van Azure toegang
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/31/2017
ms.author: sujayt
ms.openlocfilehash: 99c08a1efbc610959fb4ba824dcb0601efac5877
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="networking-guidance-for-replicating-azure-virtual-machines"></a>Richtlijnen voor het repliceren van virtuele machines van Azure toegang

>[!NOTE]
> Site Recovery replicatie voor virtuele machines in Azure is momenteel in preview.

In dit artikel wordt de instructies voor de netwerken van Azure Site Recovery wanneer u bent bezig met repliceren en herstellen van virtuele machines in Azure vanaf één regio naar een andere regio. Zie voor meer informatie over de vereisten voor Azure Site Recovery de [vereisten](site-recovery-prereq.md) artikel.

## <a name="site-recovery-architecture"></a>Site Recovery-architectuur

Site Recovery biedt een eenvoudige en eenvoudige manier om de toepassingen die worden uitgevoerd op Azure virtuele machines naar een andere Azure-regio, zodat ze kunnen worden hersteld als er een onderbreking in de primaire regio worden gerepliceerd. Meer informatie over [in dit scenario en de Site Recovery-architectuur](site-recovery-azure-to-azure-architecture.md).

## <a name="your-network-infrastructure"></a>Uw netwerkinfrastructuur

Het volgende diagram illustreert de typische Azure-omgeving voor een toepassing die wordt uitgevoerd op virtuele machines in Azure:

![klant-omgeving](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

Als u van Azure ExpressRoute of een VPN-verbinding van een on-premises netwerk naar Azure gebruikmaakt, wordt de omgeving ziet er als volgt:

![klant-omgeving](./media/site-recovery-azure-to-azure-architecture/source-environment-expressroute.png)

Normaal gesproken beveiligen klanten hun netwerken die gebruikmaken van firewalls en/of netwerkbeveiligingsgroepen (nsg's). De firewalls kunnen whitelisting op op basis van een URL of op basis van IP gebruiken voor het beheren van netwerkverbindingen. Nsg's kunnen regels voor het gebruik van IP-adresbereiken voor verbinding met het netwerk beheren.

>[!IMPORTANT]
> Als u verbinding met het netwerk beheren via een geverifieerde proxyserver, wordt niet ondersteund en replicatie van Site Recovery kan niet worden ingeschakeld.

De volgende secties worden de wijzigingen in uitgaande netwerkconnectiviteit die vereist van Azure virtuele machines voor replicatie van Site Recovery zijn werkt.

## <a name="outbound-connectivity-for-azure-site-recovery-urls"></a>Uitgaande verbinding voor Azure Site Recovery-URL

Als u elke URL gebaseerde, firewall-proxy gebruikt om te bepalen uitgaande verbinding, moet aan de lijst met geaccepteerde dat deze vereist Azure Site Recovery-service-URL's:


**URL** | **Doel**  
--- | ---
*.blob.core.windows.net | Vereist dat gegevens kunnen worden geschreven naar de storage-account van de cache in het brongebied van de virtuele machine.
Login.microsoftonline.com | Vereist voor autorisatie en verificatie met de Site Recovery-service-URL's.
*.hypervrecoverymanager.windowsazure.com | Vereist zodat de communicatie van Site Recovery service van de virtuele machine optreden kan.
*. servicebus.windows.net | Vereist zodat de Site Recovery-controle en diagnostische gegevens van de virtuele machine kunnen worden geschreven.

## <a name="outbound-connectivity-for-azure-site-recovery-ip-ranges"></a>Uitgaande verbinding voor Azure Site Recovery IP-adresbereiken

>[!NOTE]
> Als u wilt de vereiste NSG-regels op de netwerkbeveiligingsgroep automatisch gemaakt, kunt u [downloaden en gebruiken van dit script](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702).

>[!IMPORTANT]
> * U wordt aangeraden dat u de vereiste NSG-regels op een netwerkbeveiligingsgroep voor het testen maken en controleren of er geen problemen voordat u de regels op een netwerkbeveiligingsgroep voor productie maken.
> * Voor het maken van het vereiste aantal NSG-regels, zorg ervoor dat uw abonnement wilt plaatsen. Neem contact op met ondersteuning en verhoog de limiet van de regel NSG in uw abonnement.

Als u een op basis van IP-firewallproxy of het NSG-regels gebruikt voor het beheren van uitgaande verbinding, moeten de volgende IP-adresbereiken wilt plaatsen, afhankelijk van de bron- en doellocaties van de virtuele machines:

- Alle IP-adresbereiken die met de bronlocatie overeenkomen. (U kunt downloaden via de [IP-adresbereiken](https://www.microsoft.com/download/confirmation.aspx?id=41653).) Whitelisting is vereist, zodat gegevens kunnen worden geschreven naar het opslagaccount van de cache van de virtuele machine.

- Alle IP-adresbereiken die met Office 365 overeenkomen [authenticatie en identiteit IP V4-eindpunten](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).

    >[!NOTE]
    > Als het nieuwe IP-adressen ophalen in de toekomst toegevoegd aan Office 365-IP-adresbereiken, moet u nieuwe NSG-regels te maken.

- Site Recovery service-eindpunt IP-adressen ([beschikbaar zijn in een XML-bestand](https://aka.ms/site-recovery-public-ips)), die afhankelijk zijn van de doellocatie van uw:

   **TARGET-locatie** | **Site Recovery-service IP-adressen** |  **Site Recovery IP bewaken**
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

## <a name="sample-nsg-configuration"></a>Voorbeeldconfiguratie voor NSG
Deze sectie worden de stappen voor het NSG-regels zodanig configureren dat replicatie van Site Recovery op een virtuele machine werken kunt. Als u NSG-regels gebruikt voor het beheren van uitgaande verbinding, gebruikt u regels voor 'Toestaan HTTPS uitgaande' voor alle vereiste IP-adresbereiken.

>[!Note]
> Als u wilt de vereiste NSG-regels op de netwerkbeveiligingsgroep automatisch gemaakt, kunt u [downloaden en gebruiken van dit script](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702).

Bijvoorbeeld, als de bronlocatie van de VM is 'Oost ons' en de doellocatie van de replicatie is 'Centraal, VS', de stappen in de volgende twee secties.

>[!IMPORTANT]
> * U wordt aangeraden dat u de vereiste NSG-regels op een netwerkbeveiligingsgroep voor het testen maken en controleren of er geen problemen voordat u de regels op een netwerkbeveiligingsgroep voor productie maken.
> * Voor het maken van het vereiste aantal NSG-regels, zorg ervoor dat uw abonnement wilt plaatsen. Neem contact op met ondersteuning en verhoog de limiet van de regel NSG in uw abonnement.

### <a name="nsg-rules-on-the-east-us-network-security-group"></a>NSG-regels op de netwerkbeveiligingsgroep voor VS-Oost

* Maken van regels die met overeenkomen [Oost, VS IP-bereiken](https://www.microsoft.com/download/confirmation.aspx?id=41653). Dit is vereist zodat gegevens kunnen worden geschreven naar het opslagaccount van de cache van de virtuele machine.

* Regels maken voor alle IP-adresbereiken die met Office 365 overeenkomen [authenticatie en identiteit IP V4-eindpunten](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).

* Regels die met de doellocatie overeenkomen te maken:

   **Locatie** | **Site Recovery-service IP-adressen** |  **Site Recovery IP bewaken**
    --- | --- | ---
   VS - midden | 40.69.144.231 | 52.165.34.144

### <a name="nsg-rules-on-the-central-us-network-security-group"></a>NSG-regels op de netwerkbeveiligingsgroep voor VS-midden

Deze regels zijn vereist voor replicatie naar de bron regio na een failover van de doelregio kan worden ingeschakeld:

* Regels die met overeenkomen [centraal VS-IP-bereiken](https://www.microsoft.com/download/confirmation.aspx?id=41653). Dit zijn vereist, zodat gegevens kunnen worden geschreven naar het opslagaccount van de cache van de virtuele machine.

* Regels voor alle IP-adresbereiken die met Office 365 overeenkomen [authenticatie en identiteit IP V4-eindpunten](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity).

* Regels die met de bronlocatie overeenkomen:

   **Locatie** | **Site Recovery-service IP-adressen** |  **Site Recovery IP bewaken**
    --- | --- | ---
   VS - oost | 13.82.88.226 | 104.45.147.24


## <a name="guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration"></a>Richtlijnen voor het bestaande Azure-naar-on-premises ExpressRoute/VPN-configuratie

Als u een ExpressRoute- of VPN-verbinding tussen on-premises en de bronlocatie in Azure hebt, volgt u de richtlijnen in deze sectie.

### <a name="forced-tunneling-configuration"></a>Geforceerde tunneling configureren

Er is een algemene configuratie van de klant voor het definiëren van een standaardroute (0.0.0.0/0) dat ervoor zorgt uitgaand internetverkeer dat stromen via de on-premises locatie. We raden niet dit. Laat het replicatieverkeer en communicatie van Site Recovery-service niet de grens van Azure. De oplossing is het toevoegen van de gebruiker gedefinieerde routes (udr's) voor [deze IP-adresbereiken](#outbound-connectivity-for-azure-site-recovery-ip-ranges) zodat het replicatieverkeer geen lokale gaan.

### <a name="connectivity-between-the-target-and-on-premises-location"></a>Connectiviteit tussen de doel- en on-premises locatie

Volg deze richtlijnen voor verbindingen tussen de doellocatie en de on-premises-locatie:
- Als uw toepassing moet verbinding maken met de lokale machines of als er clients die verbinding met de toepassing van on-premises via VPN en ExpressRoute maken, zorg ervoor dat er ten minste een [site-naar-site-verbinding](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) tussen het doel-Azure-regio en de on-premises datacentrum.

- Als u verwacht veel verkeer tussen het doel-Azure-regio en de on-premises datacentrum dat, moet u een andere maken [ExpressRoute-verbinding](../expressroute/expressroute-introduction.md) tussen de doel-Azure-regio en de on-premises datacentrum.

- Als u behouden die IP-adressen voor de virtuele machines wilt nadat ze een failover, houd de doelregio site-naar-site en ExpressRoute-verbinding verbroken. Dit is om te controleren of er is geen bereik-conflict tussen de bron-regio IP-adresbereiken en van de doelregio-IP-adresbereiken.

### <a name="best-practices-for-expressroute-configuration"></a>Aanbevolen procedures voor het ExpressRoute-configuratie
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
