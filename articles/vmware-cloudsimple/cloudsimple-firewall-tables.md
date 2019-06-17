---
title: Firewall - oplossing door CloudSimple VMware - Azure-tabellen
description: Meer informatie over CloudSimple privécloud firewall tabellen en firewall-regels.
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 861c2e86d623c46c14366f19457d1f689386a316
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64577343"
---
# <a name="firewall-tables-overview"></a>Overzicht van de firewall-tabellen

Een firewall-tabel geeft een lijst van regels voor het filteren van netwerkverkeer naar en van de privécloud-resources. U kunt deze toepassen op een VLAN of subnet. De regels vervolgens bepalen het netwerkverkeer tussen een bron-netwerk of IP-adres en een doelnetwerk of IP-adres.

## <a name="firewall-rules"></a>Firewall-regels

De volgende tabel beschrijft de parameters in een firewallregel.

| Eigenschap | Details |
| ---------| --------|
| **Naam** | Een naam die een unieke identificatie van de firewallregel en het doel. |
| **prioriteit** | Een getal tussen 100 en 4096, met de hoogste prioriteit 100. Regels worden verwerkt in volgorde van prioriteit. Wanneer verkeer in een overeenkomende regel binnenkomt, stopt de regelverwerking. Als gevolg hiervan worden niet worden regels met een lagere prioriteit die dezelfde kenmerken als regels met een hogere prioriteit hebben verwerkt.  Let erop dat het voorkomen van conflicterende regels. |
| **Status bijhouden** | Tracering kan worden stateless is (Private Cloud, Internet of VPN) of stateful (openbaar IP-adres).  |
| **Protocol** | Opties zijn onder andere een, TCP of UDP. Als u ICMP vereist, gebruiken. |
| **Richting** | Hiermee wordt aangegeven of de regel van toepassing is op binnenkomend of uitgaand verkeer. |
| **Actie** | Toestaan of weigeren voor het type verkeer dat is gedefinieerd in de regel. |
| **Bron** | Een IP-adres, klasseloze routing tussen domeinen (CIDR) blokkeren (bijvoorbeeld 10.0.0.0/24) of een.  Een adresbereik op te geven, kunt een servicetag of toepassingsbeveiligingsgroep u minder beveiligingsregels te maken. |
| **Bronpoort** | Poort van welke netwerk verkeer van afkomstig is.  U kunt een afzonderlijke poort of een bereik van poorten, zoals 443 of 8000-8080. Als u bereiken opgeeft, hoeft u minder beveiligingsregels te maken. |
| **Destination** | Een IP-adres, klasseloze routing tussen domeinen (CIDR) blokkeren (bijvoorbeeld 10.0.0.0/24) of een.  Een adresbereik op te geven, kunt een servicetag of toepassingsbeveiligingsgroep u minder beveiligingsregels te maken.  |
| **Doelpoort** | De poort waarop het netwerkverkeer loopt.  U kunt een afzonderlijke poort of een bereik van poorten, zoals 443 of 8000-8080. Als u bereiken opgeeft, hoeft u minder beveiligingsregels te maken.|

### <a name="stateless"></a>Stateless

Een stateless regel alleen gekeken naar afzonderlijke pakketten en filters toe op basis van de regel.  
Aanvullende regels mogelijk zijn vereist voor de verkeersstroom in omgekeerde richting.  Stateless regels gebruiken voor verkeer tussen de volgende punten:

* Subnetten van Privéclouds
* On-premises subnet en een Private Cloud-subnet
* Internetverkeer van de Privéclouds

### <a name="stateful"></a>Stateful

 Een stateful regel is op de hoogte van de verbindingen die via deze. Voor bestaande verbindingen wordt een stroomrecord gemaakt. Communicatie wordt toegestaan of geweigerd op basis van de verbindingsstatus van de stroomrecord.  Gebruik dit regeltype voor openbare IP-adressen op het filter verkeer vanaf Internet.

### <a name="default-rules"></a>Standaardregels

Volgende standaardregels worden voor elke tabel firewall gemaakt.

|Prioriteit|Name|Status bijhouden|Direction|Type netwerkverkeer|Protocol|source|Bronpoort|Doel|Doelpoort|Bewerking|
|--------|----|--------------|---------|------------|--------|------|-----------|-----------|----------------|------|
|65000|toestaan dat alle-met internet|Stateful|Uitgaande|Openbare IP-adres of internet-verkeer|Alle|Alle|Alle|Alle|Alle|Toestaan|
|65001|DENY-all-van-internet|Stateful|Inkomend|Openbare IP-adres of internet-verkeer|Alle|Alle|Alle|Alle|Alle|Weigeren|
|65002|toestaan-alles-naar-intranet|Stateless|Uitgaande|Interne Privécloud of VPN-verkeer|Alle|Alle|Alle|Alle|Alle|Toestaan|
|65003|toestaan-all-van-intranet|Stateless|Inkomend|Interne Privécloud of VPN-verkeer|Alle|Alle|Alle|Alle|Alle|Toestaan|

## <a name="next-steps"></a>Volgende stappen

* [Firewall-tabellen en regels instellen](https://docs.azure.cloudsimple.com/firewall/)