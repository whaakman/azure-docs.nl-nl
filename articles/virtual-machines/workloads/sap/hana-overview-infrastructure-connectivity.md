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
ms.openlocfilehash: 1d8bbe2fc218004116177c4c9d95777d9ec57503
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49426046"
---
# <a name="sap-hana-large-instances-deployment"></a>Implementatie van SAP HANA (grote instanties) 

In dit artikel wordt ervan uitgegaan dat u hebt voltooid, de aankoop van SAP HANA op Azure (grote instanties) van Microsoft. Zie voordat het lezen van dit artikel, voor algemene achtergrond [HANA grote instanties algemene termen](hana-know-terms.md) en [HANA grote instanties SKU's](hana-available-skus.md).


Microsoft vereist dat de volgende informatie om te implementeren HANA grote instantie eenheden:

- Naam van de klant.
- Contactgegevens van bedrijf (inclusief e-mailadres en telefoonnummer getal).
- Technische contactgegevens (inclusief e-mailadres en telefoonnummer getal).
- Technische netwerken contactgegevens (met inbegrip van e-mailadres en telefoonnummer getal).
- Implementatie van Azure-regio (bijvoorbeeld VS-West, Australië-Oost en Noord-Europa).
- SAP HANA op Azure (grote instanties) SKU (configuratie).
- Voor elke implementatie van Azure-regio:
    - Een/29 IP-adresbereik voor ER-P2P-verbindingen die verbinding maken van virtuele Azure-netwerken met HANA grote instanties.
    - Een/24 CIDR-blok gebruikt voor de HANA grote instanties server IP-adresgroep.
- IP-adres de waarden voor het gebruikt in het virtuele netwerk adresruimte kenmerk van elk Azure-netwerk dat is verbonden met de HANA grote instanties.
- Gegevens voor elke HANA grote instanties-systeem:
  - Gewenste hostnaam, in het ideale geval met een volledig gekwalificeerde domeinnaam.
  - Gewenste IP-adres voor de eenheid HANA grote instantie buiten het adresbereik van IP-adres van groep van toepassingen. (De eerste 30 IP-adressen in het adresbereik van server IP-adresgroep zijn gereserveerd voor intern gebruik binnen HANA grote instanties).
  - De naam van de SAP HANA SID voor de SAP HANA-instantie (vereist voor het maken van de schijfvolumes die nodig zijn met betrekking tot SAP HANA-). Microsoft moet de HANA-SID voor het maken van de machtigingen voor sidadm op de NFS-volumes. Deze volumes koppelen aan de HANA grote instantie-eenheid. De SID HANA wordt ook gebruikt als een van de onderdelen van de naam van de volumes op schijven die worden gekoppeld. Als u meer dan één HANA-exemplaar wordt uitgevoerd op de eenheid wilt, moet u meerdere HANA-SID's weergeven. Elk item krijgt een aparte set van volumes die zijn toegewezen.
  - In de Linux-besturingssysteem heeft de gebruiker sidadm een id op. Deze ID is vereist om de schijfvolumes die nodig zijn met betrekking tot SAP HANA-te maken. De SAP HANA-installatie van de groep sapsys, wordt meestal wordt gemaakt met een groeps-ID van 1001. De gebruiker sidadm maakt deel uit van die groep.
  - In de Linux-besturingssysteem heeft de gebruiker sidadm een gebruikers-ID. Deze ID is vereist om de schijfvolumes die nodig zijn met betrekking tot SAP HANA-te maken. Als u meerdere HANA-exemplaren op de eenheid uitvoert, worden vermeld alle sidadm-gebruikers. 
- De Azure-abonnement-ID voor de Azure-abonnement met welke SAP HANA op Azure HANA grote instanties gaat direct worden gekoppeld. Abonnements-ID verwijst naar de Azure-abonnement, die wordt met de HANA grote instantie eenheid of eenheden in rekening gebracht.

Nadat u de voorgaande informatie verstrekt, voorziet Microsoft SAP HANA op Azure (grote instanties). Microsoft stuurt u informatie om te koppelen van uw Azure-netwerken aan HANA grote instanties. U kunt ook toegang tot de HANA grote instantie-eenheden.

De volgende procedure gebruiken om verbinding maken met de HANA grote instanties nadat Microsoft deze is geïmplementeerd:

1. [Azure-VM's verbinden met HANA grote instanties](hana-connect-azure-vm-large-instances.md)
2. [Een VNet verbinden met HANA grote instanties ExpressRoute](hana-connect-vnet-express-route.md)
3. [Aanvullende vereisten (optioneel)](hana-additional-network-requirements.md)

