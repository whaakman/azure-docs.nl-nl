---
title: Aangepaste tests van Load Balancer gebruiken voor het bewaken van de integriteitsstatus van de | Microsoft Docs
description: Informatie over het gebruik van aangepaste tests voor Azure Load Balancer voor het bewaken van instanties achter de Load Balancer
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/20/2018
ms.author: kumud
ms.openlocfilehash: afe46cf9fc710decba4524bd5a0fe1e73804f636
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39344161"
---
# <a name="load-balancer-health-probes"></a>Load Balancer-tests voor status

Azure Load Balancer maakt gebruik van de gezondheid van tests om te bepalen welke instanties van de groep back-end ontvangt nieuwe stromen. U kunt statuscontroles gebruiken voor het detecteren van het uitvallen van een toepassing op een back-end-instantie. U kunt ook een aangepast antwoord op een statustest genereren en gebruiken de statustest voor datatransportbesturing en signaal naar Load Balancer of u wilt doorgaan met het verzenden van nieuwe stromen of stoppen met het nieuwe stromen te verzenden naar een back-end-exemplaar. Dit kan worden gebruikt voor het beheren van de belasting of geplande uitvaltijd.

Wanneer een statustest mislukt, stopt Load Balancer nieuwe stromen te verzenden naar het desbetreffende exemplaar niet in orde. Het gedrag van nieuwe en bestaande stromen is afhankelijk van of een stroom is TCP of UDP als alsmede welke Load Balancer-SKU die u gebruikt.  Beoordeling [test omlaag gedrag voor meer informatie](#probedown).

## <a name="health-probe-types"></a>Health test typen

Statuscontroles kunnen bekijken van een willekeurige poort op een back-end-instantie, met inbegrip van de poort waarop de service wordt geleverd. De statustest biedt ondersteuning voor TCP-listeners of een HTTP-eindpunten. 

Voor het UDP-taakverdeling, moet u een aangepaste test van het statussignaal voor het back-end-exemplaar met behulp van een TCP- of HTTP-statustest genereren.

Bij het gebruik van [HA-poorten in load balancer-regels](load-balancer-ha-ports-overview.md) met [standaardversie van Load Balancer](load-balancer-standard-overview.md), alle poorten worden gelijkmatig verdeeld en een antwoord van de gezondheid van één test moet vergelijkbaar zijn met de status van de volledige-exemplaar.  

U moet geen NAT- of proxy een statustest via het exemplaar waarop de statustest naar een andere instantie in uw VNet ontvangt, omdat dit tot een opeenstapeling van storingen in uw scenario leiden kan.

### <a name="tcp-probe"></a>TPC-test

TCP-tests opnieuw verbinding maken met het uitvoeren van een drie richtingen open TCP-handshake met de gedefinieerde poort.  Dit wordt gevolgd door een vier-manier sluiten TCP-handshake.

De minimale testinterval is 5 seconden en het minimale aantal antwoorden niet in orde is 2.  De totale duur mag niet meer dan 120 seconden.

Een TCP-test mislukt wanneer:
* De TCP listener op het exemplaar reageert helemaal niet tijdens de time-outperiode.  Een test is gemarkeerd omlaag op basis van het aantal mislukte test-aanvragen die zijn geconfigureerd om te gaan onbeantwoorde voordat u markeert de test omlaag.
* De test ontvangt een TCP opnieuw instellen van het exemplaar.

### <a name="http-probe"></a>HTTP-test

HTTP-tests een TCP-verbinding tot stand brengen en uitgeven van een HTTP GET met het opgegeven pad. HTTP-tests ondersteunt relatieve paden voor de HTTP GET. De statustest is gemarkeerd als het exemplaar met een HTTP-statuscode 200 binnen de time-outperiode reageert.  HTTP-status tests poging om te controleren op de testpoort geconfigureerd de gezondheid van elke 15 seconden standaard. De minimale testinterval is 5 seconden. De totale duur mag niet meer dan 120 seconden. 


HTTP-tests kunnen ook zijn nuttig als u wilt om uw eigen logica als u wilt verwijderen van exemplaren van de load balancer-rotatie te implementeren. Bijvoorbeeld, kan u wilt verwijderen van een exemplaar als deze hoger dan 90% CPU is en een niet - 200 HTTP-status retourneren. 

Als u gebruik van Cloudservices en web-functies die gebruikmaken van w3wp.exe hebben, bereikt u ook automatische bewaking van uw website. De status van een niet-200 terug fouten in de websitecode van uw naar de load balancer-test.  De HTTP-test heeft voorrang op de standaard guest agent-test. 

Een HTTP-test mislukt wanneer:
* HTTP-test-eindpunt retourneert een HTTP-responscode dan 200 (bijvoorbeeld, 403, 404 of 500). Hiermee wordt de statustest omlaag onmiddellijk gemarkeerd. 
* HTTP-test eindpunt helemaal niet reageert tijdens het een time-outperiode voor 31 seconde. Afhankelijk van de time-outwaarde die is ingesteld, meerdere aanvragen van de WebTest onbeantwoorde mogelijk gaan voordat de test wordt gemarkeerd als niet wordt uitgevoerd (dat wil zeggen, voordat u SuccessFailCount tests worden verzonden).
* Eindpunt van de HTTP-test de verbinding via een TCP-opnieuw instellen wordt gesloten.

### <a name="guest-agent-probe-classic-only"></a>Test van Gast-agent (alleen klassiek)

Cloudservicerollen (werkrollen en webrollen) gebruiken een gastagent voor het bewaken van test standaard.   U moet rekening houden met deze optie uiterste.  U moet altijd een statustest expliciet met een TCP- of HTTP-test definiëren. De test voor een gast-agent is niet zo effectief expliciet gedefinieerde tests voor de meeste scenario's van toepassing.  

De test voor een gast-agent is een controle van de gastagent binnen de virtuele machine. Vervolgens wordt geluisterd en reageert met een HTTP 200 OK antwoord alleen wanneer het exemplaar in de status gereed heeft is. (Overige statussen zijn bezet, Recycling of stoppen.)

Zie voor meer informatie, [het servicedefinitiebestand (csdef) configureren voor statuscontroles](https://msdn.microsoft.com/library/azure/ee758710.aspx) of [aan de slag met het maken van een openbare load balancer voor cloudservices](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

Als de guest-agent niet reageert met een HTTP 200 OK, markeert de load balancer het exemplaar als reageert niet. Wordt geblokkeerd stromen te verzenden naar dat exemplaar. De load balancer blijft om te controleren of het exemplaar. 

Als de guest-agent met een HTTP-200 reageert, stuurt de load balancer nieuwe stromen opnieuw naar dat exemplaar.

Wanneer u een Webrol, de websitecode meestal wordt uitgevoerd in w3wp.exe, die niet wordt bewaakt door de Azure-infrastructuur of Gast-agent. Fouten in w3wp.exe (bijvoorbeeld 500 HTTP-antwoorden) worden niet gerapporteerd aan de gastagent. Als gevolg daarvan kan neemt de load balancer dat exemplaar uitgeschakeld.

## <a name="probe-health"></a>Status

TCP- en HTTP-statuscontroles worden beschouwd als in orde en markeren van de rolinstantie als in orde wanneer:

* De health-test is geslaagd eerst die de virtuele machine wordt opgestart.
* Het nummer voor SuccessFailCount (zoals eerder beschreven) definieert de waarde van geslaagde tests die nodig zijn voor het markeren van de rolinstantie als in orde. Als een rolinstantie is verwijderd, kan het aantal geslaagde, opeenvolgende tests moet gelijk zijn aan of groter zijn dan de waarde van SuccessFailCount markeren van de rolinstantie als het uitvoeren.

> [!NOTE]
> Als de status van een rolinstantie fluctueert, wordt de load balancer meer wacht voordat de rolinstantie wordt geplaatst in de status in orde. Deze extra wachttijd beschermt de gebruiker en de infrastructuur en is een opzettelijke beleid.

## <a name="probe-count-and-timeout"></a>Aantal en de time-test

Afhankelijk van testgedrag:

* Het aantal geslaagde tests waarmee een instantie moet worden gemarkeerd als maximaal.
* Het aantal mislukte tests die ertoe leiden een exemplaar dat moet worden gemarkeerd als offline.

De waarden voor time-outs en frequentie in SuccessFailCount te bepalen of een exemplaar is bevestigd dat moet worden uitgevoerd of niet wordt uitgevoerd. In de Azure-portal, is de time-out ingesteld op tweemaal de waarde van de frequentie.

Een regel voor taakverdeling is een eenmalige statustest gedefinieerd de respectieve back-endpool.

> [!IMPORTANT]
> Een load balancer-statustest maakt gebruik van het IP-adres 168.63.129.16. Dit openbare IP-adres vergemakkelijkt de communicatie met interne platform bronnen voor de bring-your-own-IP-Azure Virtual Network scenario. De virtuele openbare IP-adres 168.63.129.16 wordt gebruikt in alle regio's en wordt niet gewijzigd. Het is raadzaam dat u dit IP-adres toestaan in elke Azure [beveiligingsgroepen](../virtual-network/security-overview.md) en lokale firewall-beleid. Deze moet niet worden beschouwd als een veiligheidsrisico inhouden omdat alleen het interne Azure-platform kunt een pakket van dat adres van bron. Als u niet toestaat deze IP-adres in uw firewall-beleid dat, onverwacht gedrag optreedt in een verscheidenheid aan scenario's, balanced met inbegrip van uitval van uw load service. U moet uw VNet ook niet configureren met een IP-adresbereik met 168.63.129.16.  Als u meerdere interfaces op de virtuele machine hebt, moet u zorgen dat u de test op de interface die u hebt ontvangen op reageren.  Dit is mogelijk een unieke bron NAT'ing dit adres in de virtuele machine op basis van de per-interface.

## <a name="probedown"></a>Test omlaag gedrag

### <a name="tcp-connections"></a>TCP-verbindingen

Nieuwe TCP-verbindingen worden uitgevoerd naar back-end-exemplaar in orde is en heeft een gast-OS en de toepassing een nieuwe stroom accepteren.

Als de statustest van de instantie van een back-end is mislukt, blijven tot stand gebrachte TCP-verbindingen voor dit exemplaar van de back-end.

Als alle tests voor alle exemplaren in een back-endpool mislukken, worden er geen nieuwe stromen worden verzonden naar de back-endpool. Standard Load Balancer toestaat tot stand gebrachte TCP stromen om door te gaan.  Basic Load Balancer wordt beëindigd als alle bestaande TCP-stromen naar de back-endpool.
 
Aangezien de stroom altijd tussen de client en de VM Gast-OS is, zal een groep met alle tests in een frontend mogelijk niet reageert op TCP-verbindingspogingen openen als er geen exemplaar in orde back-end is voor het ontvangen van de stroom.

### <a name="udp-datagrams"></a>UDP-datagrammen

UDP-datagrammen zullen worden geleverd op in orde back-end-exemplaren.

UDP is zonder verbinding en er is geen flow-status voor UDP bijgehouden. Als de statustest van een back-end-instantie is mislukt, mogelijk bestaande UDP-stromen verplaatsen naar een andere goede instantie in de back-endpool.

Als alle tests voor alle exemplaren in een back-endpool mislukken, worden bestaande UDP-stromen worden beëindigd voor Basic en Standard Load Balancers.

## <a name="monitoring"></a>Bewaking

Alle [Standard Load Balancer](load-balancer-standard-overview.md) wordt aangegeven dat de integriteitsstatus van de test als multi-dimensionale metrische gegevens per exemplaar via Azure Monitor.

Basic Load Balancer wordt aangegeven dat de integriteitsstatus van de test per back-endpool via Log Analytics.  Dit is alleen beschikbaar voor openbare Basic Load Balancers en niet beschikbaar voor interne Basic Load Balancers.  U kunt [melden analytics](load-balancer-monitor-log.md) om te controleren op de status van openbare load balancer-test en count-test. Logboekregistratie kan worden gebruikt met Power BI of Azure Operational Insights voor statistische gegevens over de integriteitsstatus van de load balancer.

## <a name="limitations"></a>Beperkingen

-  HTTP-statustest biedt geen ondersteuning voor TLS (HTTPS).  Gebruik in plaats daarvan een TCP-test op poort 443.

## <a name="next-steps"></a>Volgende stappen

- [Maken van een openbare load balancer in Resource Manager met behulp van PowerShell](load-balancer-get-started-internet-arm-ps.md)
- [REST-API voor statuscontroles](https://docs.microsoft.com/en-us/rest/api/load-balancer/loadbalancerprobes/get)

