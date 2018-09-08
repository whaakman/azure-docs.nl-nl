---
title: Infrastructuur en connectiviteit voor SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Infrastructuur van de verbinding is vereist voor het gebruik van SAP HANA op Azure (grote instanties) configureren.
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c61dffc6fd9d0c65f5e925daebdf2a02cfb5d6ba
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44161353"
---
# <a name="sap-hana-large-instances-deployment"></a>Implementatie van SAP HANA (grote instanties) 

Na de aankoop van SAP HANA op Azure (grote instanties) tussen u en het team van Microsoft enterprise-account is voltooid, wordt de volgende informatie door Microsoft vereist HANA grote instantie eenheden implementeren:

- Naam van de klant
- Neem contact op met bedrijfsgegevens (met inbegrip van e-mailadres en telefoonnummer)
- Technische contactgegevens (met inbegrip van e-mailadres en telefoonnummer getal)
- Technische contactpersoon netwerkgegevens (met inbegrip van e-mailadres en telefoonnummer getal)
- Implementatie van Azure-regio (VS-West, VS-Oost, Australië-Oost, Australië-Zuidoost, West-Europa en Noord-Europa vanaf juni 2017)
- Controleer of SAP HANA op Azure (grote instanties) SKU (configuratie)
- Zoals beschreven in het overzicht en architectuur voor HANA grote instanties, voor elke Azure-regio worden geïmplementeerd op:
    - Een/29 IP-adresbereik voor ER-P2P-verbindingen die Azure VNets verbinden met HANA grote instanties
    - Een/24 CIDR-blok gebruikt voor de HANA grote instanties Server IP-adresgroep
- Het IP-adresbereik waarden die worden gebruikt in het kenmerk VNet-adresruimte van elke Azure-VNet dat is verbonden met de HANA grote instanties
- Gegevens voor elk van HANA grote instanties system:
  - Gewenste hostnaam - in het ideale geval met volledig gekwalificeerde domeinnaam.
  - Gewenste IP-adres voor de eenheid HANA grote instantie buiten het bereik van de servergroep van IP-adres - Houd er rekening mee dat de eerste 30 IP-adressen in de Server IP-Adresgroepbereik dat zijn gereserveerd voor intern gebruik binnen HANA grote instanties
  - De naam van de SAP HANA SID voor de SAP HANA-instantie (vereist voor het maken van de schijfvolumes die nodig zijn met betrekking tot SAP HANA-). De HANA-SID is vereist voor het maken van de machtigingen voor sidadm op de NFS-volumes zijn ophalen die zijn gekoppeld aan de eenheid HANA grote instantie. Het wordt ook gebruikt als een van de onderdelen van de naam van de volumes op schijven die worden gekoppeld. Als u meer dan één HANA-exemplaar wordt uitgevoerd op de eenheid wilt, moet u meerdere HANA-SID's weergeven. Elk item krijgt een aparte set van volumes die zijn toegewezen.
  - De groeps-id die de gebruiker sidadm in het Linux-besturingssysteem heeft is vereist om de schijfvolumes die nodig zijn met betrekking tot SAP HANA-te maken. De installatie van de SAP HANA wordt meestal de sapsys-groep gemaakt met een groeps-id van 1001. De gebruiker sidadm maakt deel uit van de groep
  - De gebruikersnaam die de gebruiker sidadm in het Linux-besturingssysteem heeft is vereist om de schijfvolumes die nodig zijn met betrekking tot SAP HANA-te maken. Als u meerdere HANA-exemplaren op de eenheid uitvoert, moet u de lijst alle de <sid>adm-gebruikers 
- Azure-abonnement-ID voor de Azure-abonnement met welke SAP HANA op Azure HANA grote instanties gaat direct worden gekoppeld. Abonnements-ID verwijst naar de Azure-abonnement, die wordt met de HANA grote instantie eenheid/eenheden in rekening gebracht.

Nadat u de informatie hebt opgegeven, wordt Microsoft bepalingen van SAP HANA op Azure (grote instanties) en de informatie die nodig zijn voor uw Azure-VNets koppelen aan HANA grote instanties en de toegang tot de eenheden HANA grote instantie wordt geretourneerd.

Voordat u dit artikel leest, raken met [HANA grote instanties algemene termen](hana-know-terms.md) en de [HANA grote instanties SKU's](hana-available-skus.md).

Verbinding maken met de HANA grote instanties nadat deze is geïmplementeerd door de Microsoft kunt u de volgende volgorde:

1. [Azure-VM's verbinden met HANA grote instanties](hana-connect-azure-vm-large-instances.md)
2. [Een VNet verbinden met HANA grote instanties ExpressRoute](hana-connect-vnet-express-route.md)
3. [Aanvullende vereisten (optioneel)](hana-additional-network-requirements.md)

**Volgende stappen**

- Raadpleeg [Azure VM's verbinden met HANA grote instanties](hana-connect-azure-vm-large-instances.md).
- Raadpleeg [verbinden van een VNet met HANA grote instantie ExpressRoute](hana-connect-vnet-express-route.md).