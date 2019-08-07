---
title: Firewall tabellen-VMware-oplossing op CloudSimple-Azure
description: Meer informatie over CloudSimple-firewall tabellen en firewall regels voor de privécloud.
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 9d25aa9252f061cee7f4cffdca42f00d84f719a3
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812658"
---
# <a name="firewall-tables-overview"></a>Overzicht van Firewall tabellen

Een firewall tabel bevat regels voor het filteren van netwerk verkeer naar en van de privécloud. U kunt deze Toep assen op een VLAN of subnet. De regels bepalen vervolgens het netwerk verkeer tussen een bron netwerk of IP-adres en een bestemmings netwerk of IP-adres.

## <a name="firewall-rules"></a>Firewallregels

In de volgende tabel worden de para meters in een firewall regel beschreven.

| Eigenschap | Details |
| ---------| --------|
| **Name** | Een naam die een unieke identificatie vormt van de firewall regel en het doel ervan. |
| **prioriteit** | Een getal tussen 100 en 4096, met 100 de hoogste prioriteit. Regels worden in volg orde van prioriteit verwerkt. Wanneer het verkeer bij een regel overeenkomst komt, stopt de verwerking van de regel. Als gevolg hiervan worden regels met een lagere prioriteit die dezelfde kenmerken hebben als regels met hogere prioriteiten, niet verwerkt.  Wees voorzichtig om conflicterende regels te voor komen. |
| **Status bijhouden** | Bijhouden kan stateless (Privécloud, Internet of VPN) of stateful (openbaar IP-adres) zijn.  |
| **Protocol** | De opties zijn onder andere TCP of UDP. Als ICMP vereist is, gebruikt u een. |
| **Richting** | Hiermee wordt aangegeven of de regel van toepassing is op binnenkomend of uitgaand verkeer. |
| **Actie** | Het type verkeer dat in de regel is gedefinieerd, toestaan of weigeren. |
| **Bron** | Een IP-adres, een CIDR-blok (Classless Inter-Domain Routing) (10.0.0.0/24) of een wille keurig.  Als u een bereik, een servicetag of een toepassings beveiligings groep opgeeft, kunt u minder beveiligings regels maken. |
| **Bron poort** | Poort van waaruit het netwerk verkeer afkomstig is.  U kunt een afzonderlijke poort of een bereik van poorten opgeven, zoals 443 of 8000-8080. Als u bereiken opgeeft, hoeft u minder beveiligingsregels te maken. |
| **Destination** | Een IP-adres, een CIDR-blok (Classless Inter-Domain Routing) (10.0.0.0/24) of een wille keurig.  Als u een bereik, een servicetag of een toepassings beveiligings groep opgeeft, kunt u minder beveiligings regels maken.  |
| **Doel poort** | Poort waarnaar het netwerk verkeer loopt.  U kunt een afzonderlijke poort of een bereik van poorten opgeven, zoals 443 of 8000-8080. Als u bereiken opgeeft, hoeft u minder beveiligingsregels te maken.|

### <a name="stateless"></a>Stateless

Een staatloze regel ziet er alleen uit bij afzonderlijke pakketten en filtert deze op basis van de regel.  
Er zijn mogelijk extra regels vereist voor de verkeers stroom in de omgekeerde richting.  Gebruik stateless regels voor verkeer tussen de volgende punten:

* Subnetten van persoonlijke Clouds
* On-premises subnet en een persoonlijk Cloud subnet
* Internet verkeer van de Privécloud

### <a name="stateful"></a>Stateful

 Een stateful regel is op de hoogte van de verbindingen die het passeren. Voor bestaande verbindingen wordt een stroomrecord gemaakt. Communicatie wordt toegestaan of geweigerd op basis van de verbindingsstatus van de stroomrecord.  Gebruik dit regel type voor open bare IP-adressen voor het filteren van verkeer van het internet.

### <a name="default-rules"></a>Standaardregels

De volgende standaard regels worden op elke firewall tabel gemaakt.

|Priority|Name|Status bijhouden|Direction|Verkeers type|Protocol|Source|Bronpoort|Doel|Doelpoort|Action|
|--------|----|--------------|---------|------------|--------|------|-----------|-----------|----------------|------|
|65000|alles-naar-Internet toestaan|Stateful|Uitgaande|Openbaar IP-of Internet verkeer|Alle|Any|Any|Any|Any|Allow|
|65001|weigeren: alle van Internet|Stateful|Inkomend|Openbaar IP-of Internet verkeer|Alle|Any|Any|Any|Any|Weigeren|
|65002|alles-naar-intranet toestaan|Stateless|Uitgaande|Intern of VPN-verkeer in de privécloud|Alle|Any|Any|Any|Any|Allow|
|65003|toestaan-alles-uit-intranet|Stateless|Inkomend|Intern of VPN-verkeer in de privécloud|Alle|Any|Any|Any|Any|Allow|

## <a name="next-steps"></a>Volgende stappen

* [Firewall tabellen en-regels instellen](https://docs.azure.cloudsimple.com/firewall/)