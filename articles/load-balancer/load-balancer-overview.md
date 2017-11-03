---
title: Overzicht van Azure Load Balancer | Microsoft Docs
description: Overzicht van Azure Load Balancer-functies, architectuur en implementatie. Informatie over de werking van de load balancer en van maken in de cloud.
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
ms.assetid: 0f313dc0-f007-4cee-b2b9-f542357925a3
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: ecf1fc38d2b9fd54fe5b00db616224a0848179fe
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-load-balancer-overview"></a>Overzicht van Azure Load Balancer

Azure Load Balancer zorgt dat uw toepassingen een hoge beschikbaarheid hebben en goede netwerkprestaties leveren. Het is een laag 4 (TCP, UDP) load balancer die binnenkomend verkeer over orde exemplaren van services gedefinieerd in een set met gelijke taakverdeling distribueert.

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Azure Load Balancer kan worden geconfigureerd voor:

* Binnenkomend internetverkeer saldo aan virtuele machines worden geladen. Deze configuratie wordt ook wel [internetgerichte taakverdeling](load-balancer-internet-overview.md).
* Load balance verkeer tussen virtuele machines in een virtueel netwerk, tussen virtuele machines in de cloud-services of tussen virtuele machines in een virtueel netwerk met cross-premises en lokale computers. Deze configuratie wordt ook wel [interne load balancing](load-balancer-internal-overview.md).
* Externe verkeer naar een specifieke virtuele machine doorsturen.

Alle resources in de cloud moeten een openbaar IP-adres aan bereikbaar zijn vanaf Internet. De cloudinfrastructuur in Azure maakt gebruik van niet-routeerbare IP-adressen voor de bronnen. Azure maakt gebruik van NAT (Netwerkadresomzetting) met openbare IP-adressen om te communiceren met Internet.

## <a name="load-balancer-features"></a>Load Balancer-kenmerken

* Hash-distributiepunt

    Azure Load Balancer gebruikmaakt van een distributiepunt op basis van het hash-algoritme. Standaard wordt een 5-tuple hash bestaat uit de bron-IP, bronpoort, doel-IP, doelpoort en protocoltype gebruikt verkeer toewijzen aan beschikbare servers. Het gebruikerspad alleen biedt *binnen* een transportsessie. Pakketten in dezelfde TCP of UDP-sessie wordt omgeleid naar hetzelfde exemplaar achter het eindpunt met gelijke taakverdeling. Wanneer de client wordt gesloten, opent u de verbinding opnieuw of een nieuwe sessie vanaf dezelfde bron-IP start, verandert de bronpoort. Dit kan leiden tot het verkeer naar een ander eindpunt in een ander datacenter.

    Zie voor meer informatie [Load balancer-distributie modus](load-balancer-distribution-mode.md). De volgende afbeelding toont de distributie op basis van een hash:

    ![Hash-distributiepunt](./media/load-balancer-overview/load-balancer-distribution.png)

    Getal - Hash-gebaseerd distributiepunt

* Poort doorsturen

    Azure Load Balancer hebt die u meer controle over hoe binnenkomende communicatie wordt beheerd. Deze communicatie omvat het verkeer van Internet-hosts, virtuele machines in andere cloudservices of virtuele netwerken wordt geïnitialiseerd. Dit besturingselement wordt vertegenwoordigd door een eindpunt (ook wel een invoereindpunt).

    Een invoereindpunt luistert op een openbare poort en stuurt het verkeer naar een interne poort. U kunt dezelfde poorten voor een interne of externe eindpunt toewijzen of een andere poort gebruiken voor deze. U kunt bijvoorbeeld een webserver die is geconfigureerd om te luisteren op poort 81 terwijl de toewijzing van openbaar eindpunt poort 80 is hebben. Het maken van een openbaar eindpunt activeert het maken van een load balancer-exemplaar.

    Wanneer gemaakt met de Azure portal, de portal automatisch eindpunten aan de virtuele machine gemaakt voor de Remote Desktop Protocol (RDP) en het externe verkeer voor Windows PowerShell-sessie. U kunt deze eindpunten gebruiken voor het extern beheren van de virtuele machine via het Internet.

* Automatische herconfiguratie

    Azure Load Balancer opnieuw onmiddellijk als u exemplaren omhoog of omlaag schalen. Deze herconfiguratie gebeurt bijvoorbeeld wanneer u het aantal exemplaren voor web/worker rollen in een cloudservice worden verhoogd of wanneer u aanvullende virtuele machines aan dezelfde set met gelijke taakverdeling toevoegen.

* Servicecontrole

    De status van de verschillende exemplaren van de server kan Azure Load Balancer-test. Wanneer een test niet reageert, stopt de load balancer nieuwe verbindingen verzenden naar de slechte exemplaren. Bestaande verbindingen worden niet beïnvloed.

    Drie soorten tests worden ondersteund:

    + **Test van de Guest-agent (op Platform als een Service alleen virtuele Machines):** de load balancer gebruikmaakt van de guest-agent op de virtuele machine. De gastagent luistert en reageert met een HTTP 200 OK antwoord wanneer het exemplaar in de status gereed (dat wil zeggen het exemplaar is niet in een status zoals gebruik recycling of stoppen). Als de agent niet reageert met een HTTP 200 OK, wordt de load balancer markeert de instantie als niet-reagerende en stopt verkeer kunnen verzenden naar dit exemplaar. De load balancer, blijft het pingen van het exemplaar. Als u de guest-agent reageert met een HTTP 200, wordt de load balancer verkeer verzenden naar dit exemplaar opnieuw. Wanneer u een Webrol, de websitecode van uw doorgaans wordt uitgevoerd in w3wp.exe die niet wordt bewaakt door de Azure fabric of Gast-agent. Dit betekent dat fouten in w3wp.exe (bijv. HTTP 500-antwoorden) wordt niet gerapporteerd aan de guest-agent en de load balancer niet vinden dat exemplaar buiten rotatie duren.
    + **Aangepaste HTTP-test:** deze test overschrijft de standaard-test (guest agent). U kunt deze gebruiken voor het maken van uw eigen aangepaste regels om te bepalen van de status van de rolinstantie. De load balancer wordt regelmatig probe voor het eindpunt (elke 15 seconden, standaard). Het exemplaar wordt beschouwd als in rotatie als deze met een TCP ACK of een HTTP 200 binnen de time-outperiode (standaard 31 seconden reageert). Dit is handig voor het implementeren van uw eigen logica voor het verwijderen van exemplaren van de load balancer worden gedraaid. U kunt bijvoorbeeld configureren dat het exemplaar om de status van een niet-200 retourneren als het exemplaar hoger dan 90% CPU is. Voor web-functies die gebruikmaken van w3wp.exe, ook dat u automatische bewaking van uw website, omdat er fouten in uw websitecode van de status van een niet-200 terug naar de test.
    + **Aangepaste TCP-test:** deze test is afhankelijk van geslaagde TCP-sessie tot stand brengen van een poort gedefinieerde test.

    Zie voor meer informatie de [LoadBalancerProbe schema](https://msdn.microsoft.com/library/azure/jj151530.aspx).

* Bron NAT

    Al het uitgaande verkeer dat afkomstig van uw service is internetverbinding ondergaat bron NAT (snat omzetten) met behulp van de VIP-adres van de hetzelfde als het binnenkomende verkeer. Snat omzetten biedt belangrijke voordelen:

    + Hiermee kunt easy upgrade en herstel na noodgevallen van services, omdat het VIP kan dynamisch worden toegewezen aan een ander exemplaar van de service.
    + Dit maakt het eenvoudiger toegangsbeheer toegangsbeheerlijst (ACL). ACL's die zijn uitgedrukt in termen van VIP's niet wijzigen als services schaal omhoog, omlaag of ophalen opnieuw geïmplementeerd.

    De load balancer-configuratie ondersteunt volledige kegel NAT voor UDP. Volledige kegel NAT is een type NAT waar de poort kan inkomende verbindingen vanaf elke externe host (in reactie op een uitgaande aanvraag).

    Voor elke nieuwe uitgaande verbinding die een virtuele machine wordt gestart, is ook een uitgaande poort toegewezen door de load balancer. De externe host ziet verkeer met een virtueel IP-adres VIP toegewezen poort. Voor scenario's waarvoor een groot aantal uitgaande verbindingen, is het raadzaam te gebruiken [instantieniveau openbare IP-adres](../virtual-network/virtual-networks-instance-level-public-ip.md) adressen, zodat de virtuele machines een speciale uitgaande IP-adres voor snat omzetten hebben. Dit vermindert het risico van uitputting van de poort.

    Zie [uitgaande verbindingen](load-balancer-outbound-connections.md) artikel voor meer informatie over dit onderwerp.

### <a name="support-for-multiple-load-balanced-ip-addresses-for-virtual-machines"></a>Ondersteuning voor meerdere netwerktaakverdeling IP-adressen voor virtuele machines
U kunt meer dan één taakverdeling openbaar IP-adres toewijzen aan een set van virtuele machines. Met deze mogelijkheid kunt u meerdere SSL-websites en/of meerdere SQL Server AlwaysOn-beschikbaarheidsgroep-listeners op dezelfde set van virtuele machines hosten. Zie voor meer informatie [meerdere VIP's per cloudservice](load-balancer-multivip.md).

[!INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="limitations"></a>Beperkingen

Load Balancer back-endpools kunnen VM-SKU behalve basisstaffel bevatten.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [internetgerichte load balancer](load-balancer-internet-overview.md)

- Meer informatie over [interne load balancer-overzicht](load-balancer-internal-overview.md)

- Maak een [internetgerichte load balancer](load-balancer-get-started-internet-portal.md)

- Informatie over een aantal van de andere sleutel [netwerkmogelijkheden](../networking/networking-overview.md) van Azure

