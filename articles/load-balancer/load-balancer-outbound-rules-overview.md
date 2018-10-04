---
title: Regels voor uitgaand verkeer in Azure Load Balancer | Microsoft Docs
description: Gebruik regels voor uitgaand verkeer vertalingen uitgaand netwerkverkeer definiëren
services: load-balancer
documentationcenter: na
author: KumudD
manager: jpconnock
tags: azure-resource-manager
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/3/2018
ms.author: kumud
ms.openlocfilehash: 50b567b298406b936a11ad5a8737ce4b1e21c0c7
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48248721"
---
# <a name="load-balancer-outbound-rules"></a>Load Balancer-regels voor uitgaand

Azure Load Balancer biedt uitgaande connectiviteit vanuit een virtueel netwerk naast binnenkomend verkeer.  Regels voor uitgaand verkeer kunnen u eenvoudig het configureren van openbare [Standard Load Balancer](load-balancer-standard-overview.md)van uitgaande netwerkadresomzetting.  Hebt u volledige declaratieve controle over uitgaande verbindingen kunnen worden geschaald en afstemmen van deze mogelijkheid aan uw specifieke behoeften.

![Load Balancer-regels voor uitgaand](media/load-balancer-outbound-rules-overview/load-balancer-outbound-rules.png)

Met regels voor uitgaand verkeer, kunt u de Load Balancer om te gebruiken: 
- uitgaande NAT helemaal definiëren.
- schalen en afstemmen van het gedrag van bestaande uitgaande NAT. 

Regels voor uitgaand verkeer kunnen u zelf regelen:
- welke virtuele machines moet worden omgezet naar dit openbare IP-adressen. 
- hoe [poorten voor uitgaande SNAT](load-balancer-outbound-connections.md#snat) moet worden toegewezen.
- welke protocollen voor omzetting van de uitgaande.
- de duur moet worden gebruikt voor de time-out voor inactiviteit van uitgaande verbindingen.
- Hiermee geeft u op of u voor het verzenden van een TCP-opnieuw instellen op de time-out voor inactiviteit (in openbare preview-versie). 

Regels voor uitgaand verkeer Vouw [scenario 2](load-balancer-outbound-connections.md#lb) in dat wordt beschreven in de [uitgaande verbindingen](load-balancer-outbound-connections.md) artikel en de prioriteit van het scenario blijft-is.

## <a name="outbound-rule"></a>Uitgaande regel

Regels voor uitgaand verkeer, zoals alle Load Balancer-regels, Ga als volgt dezelfde vertrouwde syntaxis als load balancing en binnenkomende NAT-regels:

**frontend** + **parameters** + **back-endpool**

Een uitgaande regel configureert uitgaande NAT voor _alle virtuele machines geïdentificeerd door de back-endpool_ moeten worden omgezet in de _frontend_.  En _parameters_ meer fijnmazige controle bieden over de uitgaande NAT-algoritme.

API-versie '2018-07-01' kan de definitie van een uitgaande regel als volgt zijn gestructureerd:

```json
      "outboundRules": [
        {
          "frontendIPConfigurations": [ list_of_frontend_ip_configuations ],
          "allocatedOutboundPorts": number_of_SNAT_ports,
          "idleTimeoutInMinutes": 4 through 66,
          "enableTcpReset": true | false,
          "protocol": "Tcp" | "Udp" | "All",
          "backendAddressPool": backend_pool_reference,
        }
      ]
```

>[!NOTE]
>De effectieve uitgaande NAT-configuratie is een samenstelling van alle regels voor uitgaand verkeer en load balancer-regels. Regels voor uitgaand verkeer zijn taakverdelingsregels. Beoordeling [uitgaande NAT voor een regel voor taakverdeling uitschakelen](#disablesnat) voor het beheren van de effectieve uitgaande NAT-vertaling wanneer meerdere regels van toepassing op een virtuele machine. U moet [uitschakelen uitgaande SNAT](#disablesnat) bij het definiëren van een uitgaande regel die hetzelfde openbare IP-adres wordt gebruikt als een regel voor taakverdeling.

### <a name="scale"></a> Uitgaande NAT schaal met meerdere IP-adressen

Terwijl een uitgaande regel kan worden gebruikt met slechts één openbare IP-adres, vereenvoudigen uitgaande regels de belasting van de configuratie voor het schalen van uitgaande NAT. U kunt meerdere IP-adressen gebruiken om te plannen voor grootschalige scenario's en kunt u regels voor uitgaand verkeer te beperken door [SNAT bronuitputting](load-balancer-outbound-connections.md#snatexhaust) gevoelig patronen.  

Elke extra IP-adres dat is opgegeven door een front-end biedt 64.000 kortstondige poorten voor Load Balancer om te gebruiken als SNAT poorten. Beschikken over een enkele front load balancing of inkomende NAT-regels, wordt de regel voor uitgaande breidt het begrip van de front-end en kunt meerdere front-ends per regel.  Met meerdere front-ends per regel, het aantal beschikbare poorten met SNAT wordt vermenigvuldigd met de openbare IP-adres en zeer grote scenario's kunnen worden ondersteund.

Bovendien kunt u een [openbaar IP-voorvoegsel](https://aka.ms/lbpublicipprefix) rechtstreeks met een uitgaande regel.  Dit biedt voor eenvoudiger schalen en vereenvoudigde zwarte lijst plaatsen van stromen die afkomstig zijn van uw Azure-implementatie. U kunt een front-end-IP-configuratie in de Load Balancer-resource rechtstreeks verwijzen naar een openbare IP-adresvoorvoegsel configureren.  Hierdoor kunnen Load Balancer exclusieve controle over het openbare IP-voorvoegsel en de regel voor uitgaande automatisch gebruikmaken van alle openbare IP-adressen die zijn opgenomen in het openbare IP-voorvoegsel voor uitgaande verbindingen.  Elk van de IP-adressen binnen het bereik van het openbare IP-adresvoorvoegsel 64.000 kortstondige poorten per IP-adres voor de Load Balancer om te gebruiken als SNAT poorten opgeven.   

U kunt geen afzonderlijke openbare IP-adres-resources gemaakt op basis van het openbare IP-adresvoorvoegsel wanneer u deze optie als de regel voor uitgaande volledige controle over het openbare IP-voorvoegsel moet hebben.  Als u meer goed korrelig controle nodig hebt, kunt u afzonderlijke openbare IP-adresresource maken van het openbare IP-voorvoegsel en meerdere openbare IP-adressen afzonderlijk toewijzen aan de front-end van een regel voor uitgaande.

### <a name="snatports"></a> Poorttoewijzing SNAT afstemmen

U kunt regels voor uitgaand verkeer gebruiken om af te stemmen de [automatische SNAT poorttoewijzing op basis van de grootte van de back-end](load-balancer-outbound-connections.md#preallocatedports) en toewijzen van meer of minder dan de automatische toewijzing van de poorten SNAT biedt.

Gebruik de volgende parameter toe te wijzen 10.000 SNAT poorten per VM (NIC IP-configuratie).
 

          "allocatedOutboundPorts": 10000

Elke openbaar IP-adres van alle front-ends van een regel voor uitgaande draagt bij tot maar liefst 64.000 kortstondige poorten voor ons als SNAT poorten.  Load Balancer wijst SNAT poorten in veelvouden van 8. Als u een waarde niet deelbaar is door 8 opgeeft, wordt de configuratiebewerking is afgewezen.  Als u probeert meer SNAT toewijzen poorten dan beschikbaar zijn op basis van het aantal openbare IP-adressen, is de configuratiebewerking afgewezen.  Bijvoorbeeld, als u toewijst 10.000 poorten per VM en 7 VM's in een back-end pool een enkel openbaar IP-adres wilt delen, de configuratie is afgewezen (7 x 10,0000 SNAT poorten > 64.000 SNAT poorten).  U kunt meer openbare IP-adressen toevoegen aan de front-end van de regel voor uitgaande om in te schakelen van het scenario.

U kunt terugkeren naar [automatische SNAT poorttoewijzing op basis van de grootte van de back-end](load-balancer-outbound-connections.md#preallocatedports) door 0 voor het aantal poorten op te geven.

### <a name="idletimeout"></a> Niet-actieve Controletime-out uitgaande stroom

Regels voor uitgaand verkeer bieden een configuratieparameter om te bepalen van de time-out voor inactiviteit uitgaande stroom en vergelijken met de behoeften van uw toepassing.  Uitgaande niet-actieve time-outs standaard 4 minuten.  De parameter accepteert een waarde tussen 4 en 66 naar specifiek het aantal minuten voor de time-out voor inactiviteit voor stromen die overeenkomt met deze bepaalde regel.

Gebruik de volgende parameter om de uitgaande time-out voor inactiviteit ingesteld op 1 uur:

          "idleTimeoutInMinutes": 60

### <a name="tcprst"></a> <a name="tcpreset"></a> Time-out voor inactiviteit (Preview) inschakelen TCP opnieuw instellen

Het standaardgedrag van de Load Balancer is de stroom op de achtergrond verwijderen wanneer de uitgaande time-out voor inactiviteit is bereikt.  Met de parameter enableTCPReset, kunt u een beter voorspelbare dergelijk toepassingsgedrag mogelijk maken en Hiermee bepaalt u of voor het verzenden van bidirectionele TCP (TCP RST) opnieuw instellen op de time-out van uitgaande time-out voor inactiviteit. 

Gebruik de volgende parameter om in te schakelen TCP opnieuw instellen op een uitgaande regel:

           "enableTcpReset": true

Beoordeling [TCP opnieuw ingesteld voor de time-out voor inactiviteit (Preview)](https://aka.ms/lbtcpreset) voor meer informatie, met inbegrip van beschikbaarheid in regio's.

### <a name="proto"></a> Ondersteuning voor zowel TCP en UDP-transportprotocollen met één regel

Zult u waarschijnlijk 'Alle' voor het protocol-transport van de regel voor uitgaande gebruiken, maar u kunt ook de regel voor uitgaande toepassen op een specifieke transportprotocol ook als er een nodig om dit te doen.

Gebruik de volgende parameter om het protocol ingesteld op TCP en UDP:

          "protocol": "All"

### <a name="disablesnat"></a> Uitgaande NAT voor een regel voor taakverdeling uitschakelen

Zoals eerder gezegd, bieden regels voor taakverdeling automatische programmering van uitgaande NAT. Sommige scenario's zijn echter profiteren of moeten u de automatische programmering uitgaande NAT-apparaat uitschakelen met de load balancer-regel voor het te bepalen of het gedrag te verfijnen.  Regels voor uitgaand verkeer zijn scenario's waarin het is belangrijk om te stoppen van de automatische uitgaande NAT-programmering.

U kunt deze parameter op twee manieren gebruiken:
- Optionele onderdrukking van het gebruik van de inkomende IP-adres voor uitgaande NAT.  Regels voor uitgaand verkeer zijn incrementeel laden van regels voor taakverdeling en de uitgaande regel met deze parameter is ingesteld, wordt besturingselement.
  
- Stem de uitgaande NAT-parameters van een IP-adres dat is gebruikt voor binnenkomende en uitgaande tegelijk.  De automatische uitgaande NAT-programmering moet worden uitgeschakeld om toe te staan een uitgaande regel overnemen.  Bijvoorbeeld, om te wijzigen van de toewijzing van SNAT poorten van een adres ook gebruikt voor inkomende, deze parameter moet worden ingesteld op true.  Als u een uitgaande regel gebruiken om te definiëren de parameters van een IP-adres ook gebruikt voor binnenkomende en uitgaande NAT-programmering van de load balancer-regel niet is vrijgegeven, mislukt de bewerking om een uitgaande regel te configureren.

>[!IMPORTANT]
> Uw virtuele machine geen uitgaande verbinding als u deze parameter op waar instellen en nog geen een uitgaande regel (of [scenario voor openbare IP op exemplaarniveau](load-balancer-outbound-connections.md#ilpip) uitgaande connectiviteit te definiëren.  Enkele bewerkingen van uw virtuele machine of de toepassing mogelijk afhankelijk van de uitgaande verbinding beschikbaar. Zorg ervoor dat u begrijpt de afhankelijkheden van uw scenario en impact van deze wijziging aanbrengt in aanmerking hebben genomen.

U kunt uitgaande SNAT op de load balancer-regel met deze configuratieparameter uitschakelen:

```json
      "loadBalancingRules": [
        {
          "disableOutboundSnat": true
        }
      ]
```

De parameter disableOutboundSNAT wordt standaard ingesteld op false, wat betekent dat de load balancer-regel **heeft** automatische uitgaande NAT opgeven als het spiegelbeeld van de load balancer-configuratie.  

Als u disableOutboundSnat ingesteld op ' True ' op de load balancer-regel, wordt in de load balancer-regel controle over de anders automatische uitgaande NAT-programmering versies.  Uitgaande SNAT als gevolg van de load-balancingregel is uitgeschakeld.

### <a name="reuse-existing-or-define-new-backend-pools"></a>Hergebruik bestaande of nieuwe back-endpools definiëren

Regels voor uitgaand verkeer kunnen niet tot een nieuw concept voor het definiëren van de groep van virtuele machines waarop de regel moet worden toegepast.  In plaats daarvan gebruiken ze het concept van een back-endpool, die ook wordt gebruikt voor de load balancer-regels. U kunt dit gebruiken voor het vereenvoudigen van de configuratie door de definitie van een bestaande back-end-pool opnieuw gebruiken of het maken van een specifiek voor een uitgaande regel.

## <a name="scenarios"></a>Scenario's

### <a name="groom"></a> Let op uitgaande verbindingen met een specifieke set openbare IP-adressen

U kunt een uitgaande regel opschonen uitgaande verbindingen worden afkomstig te zijn van een specifieke set openbare IP-adressen te vereenvoudigen in de whitelist aan scenario's worden weergegeven.  Dit openbare IP-adres van bron kan hetzelfde als die worden gebruikt door een taakverdelingsregel of een andere set openbare IP-adressen dan die worden gebruikt door een regel voor taakverdeling.  

1. Maak [openbaar IP-voorvoegsel](https://aka.ms/lbpublicipprefix) (of openbare IP-adressen van openbare IP-voorvoegsel)
2. Een openbare Standard Load Balancer maken
3. Maken van de front-ends die verwijst naar de openbare IP-voorvoegsel (of openbare IP-adressen) die u wilt gebruiken
4. Een back endpool opnieuw gebruiken of een back endpool maken en plaats de virtuele machines in een back-endpool van de openbare Load Balancer
5. Een uitgaande regel configureren op de openbare Load Balancer voor het programmeren van uitgaande NAT voor deze virtuele machines met behulp van de front-ends
   
Als u niet dat voor de load balancer-regel wenst moet worden gebruikt voor uitgaand, moet u [uitschakelen uitgaande SNAT](#disablesnat) op de load balancer-regel.

### <a name="modifysnat"></a> Poorttoewijzing SNAT wijzigen

U kunt regels voor uitgaand verkeer gebruiken om af te stemmen de [automatische SNAT poorttoewijzing op basis van de grootte van de back-end](load-balancer-outbound-connections.md#preallocatedports).

Hebt u twee virtuele machines delen van een enkel openbaar IP-adres voor de uitgaande NAT, wilt u mogelijk het aantal SNAT-poorten die zijn toegewezen de standaardpoorten 1024 als u SNAT uitputting ondervindt verhogen. Elke openbaar IP-adres kan bijdragen tot maar liefst 64.000 kortstondige poorten genoemd.  Als u een uitgaande regel met een enkel openbaar IP-adres front-end configureert, kunt u een totaal van maar liefst 64.000 SNAT poorten op virtuele machines in de back-endpool kunt distribueren.  Voor twee virtuele machines, kan een maximum van 32.000 SNAT poorten met een uitgaande regel (2 x 32.000 = 64.000) worden toegewezen.

Beoordeling [uitgaande verbindingen](load-balancer-outbound-connections.md) en de informatie over hoe u [SNAT](load-balancer-outbound-connections.md#snat) poorten zijn toegewezen en worden gebruikt.

### <a name="outboundonly"></a> Alleen uitgaande inschakelen

U kunt een openbare Standard Load Balancer gebruiken voor de uitgaande NAT voor een groep virtuele machines. In dit scenario kunt u een uitgaande regel door zelf, zonder eventuele extra regels nodig hebt.

#### <a name="outbound-nat-for-vms-only-no-inbound"></a>Uitgaande NAT voor VM's alleen (geen binnenkomend verkeer)

Een openbare Standard Load Balancer definieert, plaats de virtuele machines in de back-endpool en een uitgaande regel voor uitgaande NAT-programma en Let op de uitgaande verbindingen naar afkomstig zijn van een specifiek openbaar IP-adres configureren. U kunt ook een openbaar IP-voorvoegsel wit-aanbieding van de bron van de uitgaande verbindingen te vereenvoudigen.

1. Maak een openbare Standard Load Balancer.
2. Maak een back-endadresgroep en plaats de virtuele machines in een back-endpool van de openbare Load Balancer.
3. Een uitgaande regel configureren op de openbare Load Balancer voor het programmeren van uitgaande NAT voor deze virtuele machines.

#### <a name="outbound-nat-for-internal-standard-load-balancer-scenarios"></a>Uitgaande NAT voor interne Standard Load Balancer-scenario 's

Wanneer u een interne Standard Load Balancer, is uitgaande NAT niet beschikbaar totdat de uitgaande connectiviteit expliciet is gedeclareerd. U kunt uitgaande connectiviteit maken uitgaande connectiviteit voor virtuele machines achter een interne Standard Load Balancer met deze stappen met behulp van een uitgaande regel definiëren:

1. Maak een openbare Standard Load Balancer.
2. Maak een back-endpool en plaats de virtuele machines in een back-endpool van de openbare Load Balancer naast de interne Load Balancer.
3. Een uitgaande regel configureren op de openbare Load Balancer voor het programmeren van uitgaande NAT voor deze virtuele machines.

#### <a name="enable-both-tcp--udp-protocols-for-outbound-nat-with-a-public-standard-load-balancer"></a>Zowel TCP en UDP-protocollen inschakelen voor de uitgaande NAT met een openbare Standard Load Balancer

- Wanneer u een openbare Standard Load Balancer gebruikt, is de automatische uitgaande NAT programmering opgegeven komt overeen met het protocol-transport van de load balancer-regel.  

   1. Uitgaande SNAT op de load balancer-regel uitschakelen.
   2. Een uitgaande regel configureren op de dezelfde Load Balancer.
   3. De back-endpool al gebruikt door uw VM's gebruiken.
   4. Geef 'protocol': 'All' als onderdeel van de regel voor uitgaande.

- Wanneer alleen binnenkomende NAT-regels worden gebruikt, worden er geen uitgaande NAT wordt geboden.

   1. Virtuele machines in een back endadresgroep plaatsen.
   2. Een of meer front-end IP-configuraties met openbare IP-adressen of openbare IP-adresvoorvoegsel definiëren.
   3. Een uitgaande regel configureren op de dezelfde Load Balancer.
   4. Geef 'protocol': 'All' als onderdeel van de regel voor uitgaande

## <a name="limitations"></a>Beperkingen

- Het maximum aantal bruikbaar kortstondige poorten per frontend-IP-adres is 64.000.
- Het bereik van de configureerbare uitgaande time-out voor inactiviteit is 4 tot en met 66 minuten (240-4000 seconden).
- Load Balancer biedt geen ondersteuning voor ICMP voor uitgaande NAT.
- Portal kan niet worden gebruikt om te configureren of regels voor uitgaand verkeer bekijken.  Gebruik in plaats daarvan sjablonen, REST-API, Az CLI 2.0 of PowerShell.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het gebruik van [Load Balancer voor uitgaande verbindingen](load-balancer-outbound-connections.md).
- Meer informatie over [standaardversie van Load Balancer](load-balancer-standard-overview.md).
- Meer informatie over [in twee richtingen TCP opnieuw instellen op de time-out voor inactiviteit](load-balancer-tcp-reset.md).
- [Configureer regels voor uitgaand verkeer met Azure CLI 2.0](configure-load-balancer-outbound-cli.md).
