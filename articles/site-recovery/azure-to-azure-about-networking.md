---
title: Over netwerken in Azure naar Azure-noodherstel met Azure Site Recovery | Microsoft Docs
description: Bevat een overzicht van netwerken voor virtuele Azure-machines met Azure Site Recovery-replicatie.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: sujayt
ms.openlocfilehash: fb80af9df58fba41e5ece060099eb98e3d2f90fe
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56883144"
---
# <a name="about-networking-in-azure-to-azure-replication"></a>Over netwerken in Azure naar Azure-replicatie



Dit artikel bevat netwerken richtlijnen bij het repliceren en herstellen van Azure-VM's van de ene regio naar een andere, met behulp van [Azure Site Recovery](site-recovery-overview.md).

## <a name="before-you-start"></a>Voordat u begint

Informatie over hoe Site Recovery biedt herstel na noodgevallen voor [in dit scenario](azure-to-azure-architecture.md).

## <a name="typical-network-infrastructure"></a>Een typische netwerkinfrastructuur

Het volgende diagram ziet u een typische Azure-omgeving, voor toepassingen die worden uitgevoerd op Azure Virtual machines:

![klant-omgeving](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

Als u Azure ExpressRoute of een VPN-verbinding van uw on-premises netwerk naar Azure, is de omgeving als volgt:

![klant-omgeving](./media/site-recovery-azure-to-azure-architecture/source-environment-expressroute.png)

Netwerken zijn meestal beveiligd met firewalls en netwerkbeveiligingsgroepen (nsg's). Firewalls gebruiken URL of IP-gebaseerd in de whitelist aan voor het beheren van de verbinding met het netwerk. Nsg's bevatten regels die gebruikmaken van IP-adresbereiken voor het beheren van verbinding met het netwerk.

>[!IMPORTANT]
> Met behulp van een geverifieerde proxyserver om netwerkconnectiviteit te beheren wordt niet ondersteund door Site Recovery en replicatie kan niet worden ingeschakeld.


## <a name="outbound-connectivity-for-urls"></a>Uitgaande connectiviteit voor URL's

Als u van een URL-gebaseerde firewallproxy gebruikmaakt voor het beheren van uitgaande connectiviteit, kunt u deze URL's van Site Recovery:


**URL** | **Details**  
--- | ---
*.blob.core.windows.net | Vereist dat gegevens kunnen worden geschreven naar de cache-opslagaccount in de regio van de gegevensbron van de virtuele machine.
login.microsoftonline.com | Vereist voor autorisatie en verificatie van de Site Recovery-service-URL's.
*.hypervrecoverymanager.windowsazure.com | Vereist zodat de communicatie van Site Recovery-service zich van de virtuele machine voordoen kan.
*.servicebus.windows.net | Vereist zodat de Site Recovery-gegevens voor controle en diagnostische gegevens van de virtuele machine kunnen worden geschreven.

## <a name="outbound-connectivity-for-ip-address-ranges"></a>Uitgaande connectiviteit voor IP-adresbereiken

Als u gebruikmaakt van een IP-gebaseerde firewallproxy of NSG-regels voor het beheren van uitgaande connectiviteit, moeten deze IP-adresbereiken worden toegestaan.

- Alle IP-adresbereiken die met de storage-accounts in de regio van de gegevensbron overeenkomen
    - Maak een [opslag servicetag](../virtual-network/security-overview.md#service-tags) op basis van NSG-regel voor de regio van de gegevensbron.
    - Deze adressen toestaan dat gegevens kunnen worden geschreven naar de cache-opslagaccount van de virtuele machine.
- Maak een [Azure Active Directory (AAD)-servicetag](../virtual-network/security-overview.md#service-tags) op basis van NSG-regel voor het toestaan van toegang tot alle IP-adressen die overeenkomen met AAD
    - Als er nieuwe adressen worden toegevoegd aan de Azure Active Directory (AAD) in de toekomst, moet u nieuwe NSG-regels maken.
- Site Recovery-service-eindpunt IP-adressen - beschikbaar zijn in een [XML-bestand](https://aka.ms/site-recovery-public-ips) en afhankelijk van de doellocatie.
- U wordt aangeraden dat u de vereiste NSG-regels met een NSG-test maken en controleer of er zijn geen problemen voordat u de regels voor een productie-NSG maken.


Site Recovery-IP-adresbereiken zijn als volgt:

   **Target** | **Site Recovery IP** |  **Site Recovery IP bewaken**
   --- | --- | ---
   Azië - oost | 52.175.17.132 | 13.94.47.61
   Azië - zuidoost | 52.187.58.193 | 13.76.179.223
   India - centraal | 52.172.187.37 | 104.211.98.185
   India - zuid | 52.172.46.220 | 104.211.224.190
   US - noord-centraal | 23.96.195.247 | 168.62.249.226
   Europa - noord | 40.69.212.238 | 52.169.18.8
   Europa -west | 52.166.13.64 | 40.68.93.145
   US - oost | 13.82.88.226 | 104.45.147.24
   US - west | 40.83.179.48 | 104.40.26.199
   US - zuid-centraal | 13.84.148.14 | 104.210.146.250
   US - centraal | 40.69.144.231 | 52.165.34.144
   US - oost 2 | 52.184.158.163 | 40.79.44.59
   Japan - oost | 52.185.150.140 | 138.91.1.105
   Japan - west | 52.175.146.69 | 138.91.17.38
   Brazilië - zuid | 191.234.185.172 | 23.97.97.36
   Australië - oost | 104.210.113.114 | 191.239.64.144
   Australië - zuidoost | 13.70.159.158 | 191.239.160.45
   Canada - midden | 52.228.36.192 | 40.85.226.62
   Canada - oost | 52.229.125.98 | 40.86.225.142
   US - west-centraal | 52.161.20.168 | 13.78.149.209
   US - west 2 | 52.183.45.166 | 13.66.228.204
   Verenigd Koninkrijk West | 51.141.3.203 | 51.141.14.113
   Verenigd Koninkrijk Zuid | 51.140.43.158 | 51.140.189.52
   VK, zuid 2 | 13.87.37.4| 13.87.34.139
   VK, noord | 51.142.209.167 | 13.87.102.68
   Korea - centraal | 52.231.28.253 | 52.231.32.85
   Korea - zuid | 52.231.298.185 | 52.231.200.144
   Frankrijk - centraal | 52.143.138.106 | 52.143.136.55
   Frankrijk - zuid | 52.136.139.227 |52.136.136.62
   Australië-centraal| 20.36.34.70 | 20.36.46.142
   Australië - centraal 2| 20.36.69.62 | 20.36.74.130

## <a name="example-nsg-configuration"></a>Voorbeeld van de NSG-configuratie

In dit voorbeeld laat zien hoe het configureren van NSG-regels voor een virtuele machine om te repliceren.

- Als u NSG-regels voor het beheren van uitgaande connectiviteit, gebruikt u 'Toestaan uitgaande HTTPS' regels op poort 443: voor alle de vereiste IP-adresbereiken.
- Het voorbeeld wordt ervan uitgegaan dat de locatie van de virtuele machine is 'Oost ons' en de doellocatie is 'VS-midden'.

### <a name="nsg-rules---east-us"></a>NSG-regels - VS-Oost

1. Maak een regel voor uitgaande HTTPS (443) voor 'Storage.EastUS' op de NSG zoals wordt weergegeven in de onderstaande schermafbeelding.

      ![opslag-tag](./media/azure-to-azure-about-networking/storage-tag.png)

2. Maak een regel voor uitgaande HTTPS (443) voor 'AzureActiveDirectory' op de NSG zoals wordt weergegeven in de onderstaande schermafbeelding.

      ![AAD-tag](./media/azure-to-azure-about-networking/aad-tag.png)

3. Uitgaande HTTPS (443)-regels maken voor de Site Recovery IP-adressen die overeenkomen met de doellocatie:

   **Locatie** | **Site Recovery-IP-adres** |  **Site Recovery bewaking IP-adres**
    --- | --- | ---
   US - centraal | 40.69.144.231 | 52.165.34.144

### <a name="nsg-rules---central-us"></a>NSG-regels - VS centraal

Deze regels zijn vereist voor replicatie kan worden ingeschakeld in de doelregio de bron regio nadat failover is uitgevoerd:

1. Maak een regel voor uitgaande HTTPS (443) voor 'Storage.CentralUS' op de NSG.

2. Maak een regel voor uitgaande HTTPS (443) voor 'AzureActiveDirectory' op de NSG.

3. Uitgaande HTTPS (443)-regels maken voor de Site Recovery IP-adressen die overeenkomen met de bronlocatie:

   **Locatie** | **Site Recovery-IP-adres** |  **Site Recovery bewaking IP-adres**
    --- | --- | ---
   US - centraal | 13.82.88.226 | 104.45.147.24

## <a name="network-virtual-appliance-configuration"></a>Configuratie van het virtuele netwerkapparaat

Als u virtuele netwerkapparaten (NVA's) voor het beheren van uitgaande netwerkverkeer van virtuele machines, het apparaat mogelijk te maken met beperkingen als het replicatieverkeer wordt doorgegeven via de NVA. Het wordt aangeraden om een service-eindpunt van het netwerk maken in uw virtuele netwerk voor 'Opslag', zodat het replicatieverkeer niet naar de NVA.

### <a name="create-network-service-endpoint-for-storage"></a>Service-netwerkeindpunt maken voor opslag
U kunt een service-eindpunt van het netwerk in uw virtuele netwerk maken voor 'Opslag' zodat het replicatieverkeer niet Azure grens verlaat.

- Selecteer uw Azure-netwerk en klik op Service-eindpunten

    ![Storage-eindpunt](./media/azure-to-azure-about-networking/storage-service-endpoint.png)

- Klik op 'Toevoegen' en 'Service-eindpunten toevoegen' tabblad wordt geopend
- Selecteer 'Microsoft.Storage' onder 'Service' en de vereiste subnetten onder 'Subnetten' veld en klikt u op 'Toevoegen'

>[!NOTE]
>Beperk toegang tot het virtuele netwerk niet naar uw storage-accounts die worden gebruikt voor ASR volgt. U moet toegang via 'Alle netwerken' toestaan

### <a name="forced-tunneling"></a>Geforceerde tunneling

U kunt onderdrukken van Azure voor standaardroute systeem voor het adresvoorvoegsel 0.0.0.0/0 vervangen door een [aangepaste route](../virtual-network/virtual-networks-udr-overview.md#custom-routes) en omleiden van verkeer van de virtuele machine naar een on-premises virtueel netwerkapparaat (NVA), maar deze configuratie wordt niet aanbevolen voor Site Recovery replicatie. Als u aangepaste routes, moet u [maken van een service-eindpunt voor virtueel netwerk](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) in uw virtuele netwerk voor 'Opslag' zodat het replicatieverkeer niet de Azure-grens verlaat.

## <a name="next-steps"></a>Volgende stappen
- Beginnen met het beveiligen van uw workloads door [repliceren van virtuele machines van Azure](site-recovery-azure-to-azure.md).
- Meer informatie over [IP-adres retentie](site-recovery-retain-ip-azure-vm-failover.md) voor failover van de virtuele machine van Azure.
- Meer informatie over herstel na noodgevallen van [Azure virtual machines met ExpressRoute](azure-vm-disaster-recovery-with-expressroute.md).
