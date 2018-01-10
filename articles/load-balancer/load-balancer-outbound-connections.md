---
title: Inzicht in uitgaande verbindingen in Azure | Microsoft Docs
description: Dit artikel wordt uitgelegd hoe de Azure VM's om te communiceren met het openbare internetservices kunt.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: 
ms.assetid: 5f666f2a-3a63-405a-abcd-b2e34d40e001
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: b8e225ba4374c73dbabac3dddab9ba37fa798a5a
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="understanding-outbound-connections-in-azure"></a>Uitleg over uitgaande verbindingen in Azure

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Een virtuele Machine (VM) in Azure kunnen communiceren met de eindpunten buiten Azure in openbare IP-adresruimte. Wanneer een virtuele machine een uitgaande stroom naar een bestemming in openbare IP-adresruimte initieert, wordt Azure de VM privé IP-adres wordt toegewezen aan een openbaar IP-adres en return gegevensverkeer te bereiken van de virtuele machine.

Azure biedt drie verschillende methoden om uitgaande verbinding bereiken. Elke methode heeft een eigen mogelijkheden en beperkingen. Controleer elke methode zorgvuldig om te kiezen welke voldoet aan uw behoeften.

| Scenario | Methode | Opmerking |
| --- | --- | --- |
| Standalone VM (Er is geen load balancer, geen Instance Level Public IP-adres) |Standaard snat omzetten |Azure koppelt een openbare IP-adres voor snat omzetten |
| Taakverdeling VM (geen Instance Level Public IP-adres op virtuele machine) |Snat omzetten met behulp van de load balancer |Azure maakt gebruik van een openbaar IP-adres van de Load Balancer voor snat omzetten |
| Virtuele machine met Instance Level Public IP-adres (met of zonder load balancer) |Snat omzetten wordt niet gebruikt. |Azure maakt gebruik van het openbare IP-adres toegewezen aan de virtuele machine |

Als u niet dat een virtuele machine wilt te communiceren met eindpunten buiten Azure in openbare IP-adresruimte, kunt u Netwerkbeveiligingsgroep groepen (NSG) gebruiken om toegang te blokkeren. Met behulp van de nsg's wordt besproken in nader [openbare connectiviteit voorkomen](#preventing-public-connectivity).

## <a name="standalone-vm-with-no-instance-level-public-ip-address"></a>Standalone VM zonder Instance Level Public IP-adres

In dit scenario wordt de virtuele machine maakt geen deel uit van een pool Load Balancer van Azure en heeft geen een Instance Level Public IP (ILPIP)-adres toegewezen. Wanneer de virtuele machine een uitgaande stroom maakt, vertaalt Azure het persoonlijke bron-IP-adres van de uitgaande stroom aan een openbare IP-adres. Het openbare IP-adres voor deze uitgaande stroom geldt kan niet worden geconfigureerd en niet ten opzichte van het abonnement openbare IP-resource limiet. Azure gebruikt bron Network Address Translation (snat omzetten) om deze functie niet uitvoeren. Kortstondige poorten van het openbare IP-adres worden gebruikt om te onderscheiden van afzonderlijke stromen afkomstig is van het door de virtuele machine. Kortstondige poorten snat omzetten dynamisch toegewezen wanneer stromen worden gemaakt. In deze context worden de kortstondige poorten gebruikt voor snat omzetten aangeduid als snat omzetten poorten.

Snat omzetten poorten zijn een eindige resource die kan worden verbruikt. Het is belangrijk te weten hoe ze worden verbruikt. Per overdracht voor een enkel IP-poort van één snat omzetten verbruikt. Voor meerdere stromen naar hetzelfde doel-IP-adres en poort verbruikt elke flow één poort snat omzetten. Dit zorgt ervoor dat de stromen uniek wanneer afkomstig is van hetzelfde openbare IP-adres op hetzelfde doel-IP-adres en poort zijn. Meerdere stromen, elk op een andere bestemming IP-adres en poort, delen één poort snat omzetten. De doel-IP-adres en poort maakt de stromen uniek.

U kunt [Log Analytics voor Load Balancer](load-balancer-monitor-log.md) en [waarschuwing gebeurtenislogboeken om te controleren op snat omzetten poort uitputting berichten](load-balancer-monitor-log.md#alert-event-log) status van uitgaande verbindingen worden gecontroleerd. Wanneer snat omzetten poort resources zijn uitgeput, mislukken uitgaande stromen tot snat omzetten poorten zijn vrijgegeven door bestaande stromen. Load Balancer gebruikmaakt van een niet-actieve time-out van 4 minuten voor het vrijmaken van poorten snat omzetten.  Bekijk [VM met een Instance Level Public IP-adres (met of zonder Load Balancer)](#vm-with-an-instance-level-public-ip-address-with-or-without-load-balancer) de volgende sectie ook als [beheren snat omzetten uitputting](#snatexhaust).

## <a name="load-balanced-vm-with-no-instance-level-public-ip-address"></a>Taakverdeling met virtuele machine geen Instance Level Public IP-adres

In dit scenario is de virtuele machine deel uitmaakt van een Load Balancer van Azure-toepassingen.  De virtuele machine heeft geen een openbaar IP-adres toegewezen. De Load Balancer-bron moet worden geconfigureerd met een load balancer-regel om een koppeling tussen het openbare IP-frontend met de back-endpool te maken. Als u deze configuratie niet uitvoert, is het gedrag zoals beschreven in de voorgaande sectie voor [Standalone VM met geen Instance Level Public IP](load-balancer-outbound-connections.md#standalone-vm-with-no-instance-level-public-ip-address).

Wanneer de VM Netwerktaakverdeling een uitgaande stroom maakt, vertaalt Azure het persoonlijke bron-IP-adres van de uitgaande stroom voor het openbare IP-adres van de openbare Load Balancer-frontend. Azure gebruikt bron Network Address Translation (snat omzetten) om deze functie niet uitvoeren. Kortstondige poorten van het openbare IP-adres van de Load Balancer worden gebruikt om te onderscheiden van afzonderlijke stromen afkomstig is van het door de virtuele machine. Kortstondige poorten snat omzetten dynamisch toegewezen wanneer uitgaande stromen worden gemaakt. In deze context worden de kortstondige poorten gebruikt voor snat omzetten aangeduid als snat omzetten poorten.

Snat omzetten poorten zijn een eindige resource die kan worden verbruikt. Het is belangrijk te weten hoe ze worden verbruikt. Één snat omzetten poort wordt verbruikt per overdracht voor een enkel IP-adres en poort. Voor meerdere stromen naar hetzelfde doel-IP-adres en poort verbruikt elke flow één poort snat omzetten. Dit zorgt ervoor dat de stromen uniek wanneer afkomstig is van hetzelfde openbare IP-adres op hetzelfde doel-IP-adres en poort zijn. Meerdere stromen, elk op een andere bestemming IP-adres en poort, delen één poort snat omzetten. De doel-IP-adres en poort maakt de stromen uniek.

U kunt [Log Analytics voor Load Balancer](load-balancer-monitor-log.md) en [waarschuwing gebeurtenislogboeken om te controleren op snat omzetten poort uitputting berichten](load-balancer-monitor-log.md#alert-event-log) status van uitgaande verbindingen worden gecontroleerd. Wanneer snat omzetten poort resources zijn uitgeput, mislukken uitgaande stromen tot snat omzetten poorten zijn vrijgegeven door bestaande stromen. Load Balancer gebruikmaakt van een niet-actieve time-out van 4 minuten voor het vrijmaken van poorten snat omzetten.  Raadpleeg de volgende sectie, evenals [beheren snat omzetten uitputting](#snatexhaust).

## <a name="vm-with-an-instance-level-public-ip-address-with-or-without-load-balancer"></a>Virtuele machine met een Instance Level Public IP-adres (met of zonder Load Balancer)

In dit scenario heeft de virtuele machine een exemplaar niveau openbare IP (ILPIP) toegewezen. Het maakt niet uit of de virtuele machine verdeeld wordt, of niet. Wanneer een ILPIP wordt gebruikt, wordt de bron Network Address Translation (snat omzetten) niet gebruikt. De virtuele machine maakt gebruik van de ILPIP voor alle uitgaande stromen. Als uw toepassing veel uitgaande stromen initieert en u snat omzetten uitputting ondervindt, kunt u overwegen een ILPIP te verhelpen snat omzetten beperkingen toe te wijzen.

## <a name="discovering-the-public-ip-used-by-a-given-vm"></a>Het openbare IP-adres gebruikt door een bepaalde virtuele machine detecteren

Er zijn veel manieren om te bepalen van de bron van openbare IP-adres van een uitgaande verbinding. OpenDNS biedt een service waarmee u het openbare IP-adres van uw virtuele machine kunt weergeven. Met de opdracht nslookup, kunt u een DNS-query voor de naam myip.opendns.com verzenden naar de OpenDNS-resolver. De service retourneert het bron-IP-adres dat is gebruikt voor het verzenden van de query. Als u de volgende query uit uw virtuele machine uitvoert, is het antwoord het openbare IP-adres gebruikt voor die VM.

    nslookup myip.opendns.com resolver1.opendns.com

## <a name="preventing-public-connectivity"></a>Zo wordt voorkomen dat openbare-connectiviteit

Soms wordt byserverservice voor een virtuele machine moet worden toegestaan voor het maken van een uitgaande stroom of kan er een vereiste voor het beheren van welke doelen kunnen worden bereikt met uitgaande stromen of welke bestemmingen inkomende stromen kunnen beginnen. In dit geval gebruikt u [Netwerkbeveiligingsgroep groepen (NSG)](../virtual-network/virtual-networks-nsg.md) voor het beheren van de doelen die de virtuele machine kan ook bereiken als welke openbare bestemming inkomende stromen kunnen initiëren. Wanneer u een NSG aan een VM taakverdeling toepassen, moet u aandacht te besteden aan de [standaard labels](../virtual-network/virtual-networks-nsg.md#default-tags) en [regels standaard](../virtual-network/virtual-networks-nsg.md#default-rules).

U moet ervoor zorgen dat de virtuele machine van Azure Load Balancer health test aanvragen kan ontvangen. Als een NSG health test aanvragen van het label van de standaard AZURE_LOADBALANCER blokkeert, uw VM health test mislukt en de virtuele machine niet actief is gemarkeerd. Load Balancer reageert nieuwe stromen verzenden naar die virtuele machine.

## <a name="snatexhaust"></a>Het beheren van uitputting van de snat omzetten

Kortstondige poorten gebruikt voor snat omzetten zijn een onuitputtelijk resource, zoals beschreven in [Standalone VM zonder Instance Level Public IP-adres](#standalone-vm-with-no-instance-level-public-ip-address) en [taakverdeling met virtuele machine geen Instance Level Public IP-adres](#standalone-vm-with-no-instance-level-public-ip-address).

Als u weet tot stand brengen veel uitgaande TCP of UDP-verbindingen met dezelfde IP-adres en poort, zien uitgaande verbindingen mislukken of door ondersteuning wordt aangeraden dat u put snat omzetten poorten zijn, hebt u verschillende algemene risicobeperking mogelijkheden.  Bekijk deze opties en bepaal wat wordt aanbevolen voor uw scenario.  Het is mogelijk een of meer kunnen helpen bij dit scenario.

### <a name="modify-application-to-reuse-connections"></a>Toepassing hergebruiken verbindingen wijzigen 
U kunt de vraag naar kortstondige poorten gebruikt voor snat omzetten door verbindingen in uw toepassing opnieuw te verminderen.  Dit geldt met name voor protocollen HTTP/1.1 waarbij opnieuw verbinding wilt gebruiken, is de standaardwaarde.  En andere protocollen die via HTTP als transportmechanisme gebruiken (dat wil zeggen REST) op zijn beurt kunnen profiteren.  Hergebruik is altijd beter dan afzonderlijke, atomic TCP-verbindingen voor elke aanvraag en resulteert in meer zodat, zeer efficiënte TCP transacties.

### <a name="modify-application-to-use-connection-pooling"></a>Toepassing gebruiken verbindingsgroepering wijzigen
U kunt gebruikmaken van een groepsgewijze schema in uw toepassing, waarin aanvragen intern zijn verdeeld over een vaste set van (elke hergebruiken indien mogelijk)-verbindingen.  Deze beperkingen voor het aantal tijdelijke poorten in gebruik en maakt u een beter voorspelbaar omgeving.  Dit kunt ook de doorvoer van aanvragen door meerdere gelijktijdige bewerkingen toe wanneer één verbinding is geblokkeerd voor het antwoord van een bewerking te verhogen.  Verbindingsgroepering bestaat al in het kader die u gebruikt voor het ontwikkelen van uw toepassing of de configuratie-instellingen voor uw toepassing.  U kunt dit combineren met het hergebruik van verbindingen en uw meerdere aanvragen een vaste, voorspelbare aantal poorten naar het hetzelfde IP-adres en poort gebruiken terwijl het ook profiteren van zeer efficiënt gebruik van TCP-transacties, waardoor latentie en resource gebruik.

### <a name="modify-application-to-use-less-aggressive-retry-logic"></a>Wijzigen van de toepassing minder agressieve Pogingslogica gebruiken
Als kortstondige poorten gebruikt voor snat omzetten zijn uitgeput of toepassingsfouten optreden, pogingen agressieve of brute force zonder decay en backoff logica oorzaak bronuitputting of behouden blijven. Aanvraag voor tijdelijke poorten kunt u met behulp van een minder agressieve Pogingslogica verminderen.   Kortstondige poorten hebben een 4 minuten inactiviteit (niet-aanpasbare) en als de pogingen te agressief, de uitputting heeft geen mogelijkheid om op te ruimen op zichzelf.  Overwegen hoe en hoe vaak uw toepassing opnieuw transacties probeert is daarom essentieel overweging van het ontwerp.

### <a name="assign-an-instance-level-public-ip-to-each-vm"></a>Een openbaar IP op exemplaarniveau toewijzen aan elke virtuele machine
Hiermee kunt u uw scenario naar [instantieniveau openbare IP-adres aan een VM](#vm-with-an-instance-level-public-ip-address-with-or-without-load-balancer).  Alle kortstondige poorten van het openbare IP-adres gebruikt voor elke virtuele machine zijn beschikbaar voor de virtuele machine (in plaats van scenario's waarbij kortstondige poorten van een openbare IP-adres worden gedeeld met alle van de VM gekoppeld aan de betreffende back-endpool).  Er zijn verschillen rekening moet houden, zoals extra kosten van IP-adressen en de mogelijke invloed op door een groot aantal afzonderlijke IP-adressen.

## <a name="limitations"></a>Beperkingen

Als [meerdere (openbare) IP-adressen zijn gekoppeld aan een load balancer](load-balancer-multivip-overview.md), een van deze openbare IP-adressen zijn geschikt is voor uitgaande stromen.

Azure maakt gebruik van een algoritme om te bepalen het aantal beschikbare poorten voor snat omzetten op basis van de grootte van de pool.  Dit kan op dit moment niet worden geconfigureerd.

Uitgaande verbindingen hebben een niet-actieve time-out van 4 minuten.  Dit is geen aanpasbare.

Het is belangrijk te weten dat het aantal beschikbare poorten voor snat omzetten niet rechtstreeks aan het aantal verbindingen wordt omgezet. Raadpleeg de voorgaande secties voor specifieke informatie over wanneer en hoe snat omzetten poorten zijn toegewezen en [deze onuitputtelijk resource beheren](#snatexhaust).
