---
title: Azure Load Balancer oplossen | Microsoft Docs
description: Bekende problemen oplossen met Azure Load Balancer
services: load-balancer
documentationcenter: na
author: RamanDhillon
manager: timlt
editor: 
ms.assetid: 
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: bc059221656a695bb43af0dca06df941ca77c73d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-azure-load-balancer"></a>Azure Load Balancer oplossen

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Deze pagina bevat informatie over probleemoplossing voor veelgestelde vragen voor Azure Load Balancer. Wanneer de connectiviteit taakverdeler niet beschikbaar is, zijn de meest voorkomende problemen als volgt: 
- Virtuele machines achter de Load Balancer reageert niet op statuscontroles 
- Virtuele machines achter de Load Balancer reageert niet op het verkeer op de geconfigureerde poort

## <a name="symptom-vms-behind-the-load-balancer-are-not-responding-to-health-probes"></a>Symptoom: VM's achter de Load Balancer reageert niet op statuscontroles
Voor de back-endservers om deel te nemen in de load balancer-set moeten ze de controle van de test doorgeven. Zie voor meer informatie over statuscontroles [Understanding Load Balancer-tests](load-balancer-custom-probe-overview.md). 

De Load Balancer-back-endpool virtuele machines reageert niet op de testpakketten vanwege een van de volgende redenen: 
- Back-endpool VM voor Load Balancer is slecht 
- Load Balancer back-endpool die VM wordt niet geluisterd op de testpoort 
- Firewall of een netwerkbeveiligingsgroep is de poort op de Load Balancer-back-endpool VM's blokkeren 
- Overige configuratiefouten in de Load Balancer

### <a name="cause-1-load-balancer-backend-pool-vm-is-unhealthy"></a>1 oorzaak: De back-endpool van de Load Balancer VM is slecht 

**Validatie en oplossing**

U lost dit probleem, zich aanmelden bij de deelnemende virtuele machines, en controleer of de VM-status in orde is en kan reageren op **psping om** of **TCPing** uit een andere virtuele machine in de groep. Als de virtuele machine beschadigd is of niet reageren op de test, moet u het probleem corrigeren en de virtuele machine terug in een gezonde toestand verkrijgen voordat deze taakverdeling deelnemen kan.

### <a name="cause-2-load-balancer-backend-pool-vm-is-not-listening-on-the-probe-port"></a>Oorzaak 2: Back-endpool van de Load Balancer VM niet geluisterd op de testpoort
Als de virtuele machine in orde is, maar niet op de test reageert, is een mogelijke reden zijn kan dat de testpoort niet geopend zijn is op de deelnemen aan de virtuele machine, of de virtuele machine niet beschikbaar op deze poort.

**Validatie en oplossing**

1. Aanmelden bij de back-end VM. 
2. Open een opdrachtprompt en voer de volgende opdracht om te valideren, er is een toepassing die luistert op de testpoort:   
            netstat - an
3. Als de poortstatus van de niet is vermeld als **LISTENING**, de juiste poort configureren. 
4. Selecteer een andere poort, die wordt vermeld als **LISTENING**, en bij te werken load balancer-configuratie dienovereenkomstig.              

###<a name="cause-3-firewall-or-a-network-security-group-is-blocking-the-port-on-the-load-balancer-backend-pool-vms"></a>3 oorzaak: Firewall of een netwerkbeveiligingsgroep wordt geblokkeerd door de poort op de load balancer back-endpool virtuele machines  
Als de testpoort wordt geblokkeerd door de firewall op de virtuele machine of een of meer beveiligingsgroepen die zijn geconfigureerd op het subnet of op de VM-netwerk, de test te bereiken, de poort niet toestaat, is de virtuele machine niet reageren op de health-test.          

**Validatie en oplossing**

* Als de firewall is ingeschakeld, controleert u of deze is geconfigureerd voor het toestaan van de testpoort. Als dat niet het geval is, moet u de firewall configureren voor verkeer op de testpoort en test opnieuw. 
* Controleer of de binnenkomende of uitgaande verkeer op de testpoort storing is in de lijst van netwerkbeveiligingsgroepen. 
* Controleer ook als een **weigeren alle** netwerkbeveiligingsgroepen-regel op de NIC van de virtuele machine of het subnet dat een hogere prioriteit dan de standaardregel waarmee LB tests & verkeer (netwerkbeveiligingsgroepen moeten toestaan Load Balancer IP-adres van 168.63.129.16). 
* Als een van deze regels de test-verkeer blokkeert zijn, verwijderen en opnieuw configureren van de regels voor de test-verkeer.  
* Test de virtuele machine is nu gestart op de statuscontroles reageert. 

### <a name="cause-4-other-misconfigurations-in-load-balancer"></a>4 oorzaak: Andere configuratiefouten in de Load Balancer
Als de voorgaande oorzaken lijken te worden gevalideerd en de juiste manier omgezet en de back-end VM nog steeds niet naar de health test vervolgens handmatig reageert test voor connectiviteit en sommige traceringen voor informatie over de connectiviteit verzamelen.

**Validatie en oplossing**

* Gebruik **psping om** uit een van de andere virtuele machines binnen het VNet voor het testen van het antwoord van de poort test (voorbeeld:.\psping.exe -t 10.0.0.4:3389) en noteer de resultaten. 
* Gebruik **TCPing** van een van de andere virtuele machines binnen het VNet voor het testen van het antwoord van de poort test (voorbeeld:.\tcping.exe 10.0.0.4 3389) en noteer de resultaten. 
* Als er geen reactie wordt ontvangen in de deze ping-tests vervolgens
    - Een gelijktijdige Netsh-trace op de doel-back-endpool VM en een andere test VM uitvoeren vanuit het hetzelfde VNet. Nu, voert u een test psping om enige tijd, sommige netwerktracering verzamelen en vervolgens de test stoppen. 
    - Analyseer het vastleggen van het netwerk en zien of er inkomende en uitgaande pakketten die betrekking hebben op de ping-query. 
        - Als er geen inkomende pakketten zijn waargenomen op de back-endpool VM, is er mogelijk een netwerkbeveiligingsgroepen of UDR onjuiste configuratie van het verkeer blokkeert. 
        - Als er geen uitgaande pakketten worden waargenomen op de back-endpool VM, moet de virtuele machine worden gecontroleerd op niet-verwante problemen (voor eample, toepassing blokkeren van de testpoort). 
    - Controleer of als de testpakketten worden gedwongen naar een andere bestemming (mogelijk via instellingen UDR) voordat de load balancer is bereikt. Hierdoor kan het verkeer naar de back-end VM nooit bereikt. 
* De test-type (bijvoorbeeld HTTP naar TCP) wijzigen en de bijbehorende poort configureren voor netwerkbeveiligingsgroepen ACL's en firewall valideren als het probleem met de configuratie van de test-antwoord is. Zie voor meer informatie over de configuratie van health test [configuratie van health test eindpunt Load Balancing](https://blogs.msdn.microsoft.com/mast/2016/01/26/endpoint-load-balancing-heath-probe-configuration-details/).

## <a name="symptom-vms-behind-load-balancer-are-not-responding-to-traffic-on-the-configured-data-port"></a>Symptoom: VM's achter de Load Balancer reageert niet op verkeer op de geconfigureerde poort

Als u een back-endpool VM wordt vermeld als goed en reageert op de statuscontroles, maar nog steeds niet deelneemt aan de taakverdeling of reageert niet op het gegevensverkeer van, mogelijk vanwege een van de volgende redenen: 
* Load Balancer-back-endpool die VM niet geluisterd op de poort 
* Netwerkbeveiligingsgroep wordt geblokkeerd door de poort op de Load Balancer-back-endpool VM  
* Toegang tot de Load Balancer van hetzelfde VM en NIC 
* Toegang tot het Internet Load Balancer-VIP van de deelnemende Load Balancer back-endpool VM 

### <a name="cause-1-load-balancer-backend-pool-vm-is-not-listening-on-the-data-port"></a>1 oorzaak: Back-endpool van de Load Balancer VM niet geluisterd op de poort 
Als een virtuele machine niet op het gegevensverkeer van reageert, kan zijn omdat de doelpoort niet geopend op de deelnemende VM is of de virtuele machine niet geluisterd op poort. 

**Validatie en oplossing**

1. Aanmelden bij de back-end VM. 
2. Open een opdrachtprompt en voer de volgende opdracht om te valideren, er is een toepassing die luistert op de poort:  
            netstat - an 
3. Als de poort niet wordt vermeld met status 'Luistert', de juiste listener-poort configureren 
4. Als de poort is gemarkeerd als Listening, controleert u de doeltoepassing bij die poort voor mogelijke problemen. 

### <a name="cause-2-network-security-group-is-blocking-the-port-on-the-load-balancer-backend-pool-vm"></a>2 oorzaak: De netwerkbeveiligingsgroep wordt geblokkeerd door de poort op de Load Balancer-back-endpool VM  

Als een of meer netwerkbeveiligingsgroepen geconfigureerd op het subnet of op de virtuele machine, blokkeert de bron-IP- of -poort, dan is de virtuele machine niet kunnen reageren.

* Lijst van de netwerkbeveiligingsgroepen die zijn geconfigureerd op de back-end VM. Zie voor meer informatie:
    -  [Netwerkbeveiligingsgroepen via de Portal beheren](../virtual-network/virtual-network-manage-nsg-arm-portal.md)
    -  [Netwerkbeveiligingsgroepen beheren met behulp van PowerShell](../virtual-network/virtual-network-manage-nsg-arm-ps.md)
* Controleer of in de lijst van netwerkbeveiligingsgroepen:
    - het binnenkomende of uitgaande verkeer op de poort heeft een storing. 
    - een **weigeren alle** netwerk beveiligingsregel-groep op de NIC van de virtuele machine of het subnet dat een hogere prioriteit die de standaardregel waarmee de Load Balancer-tests heeft en er verkeer (netwerkbeveiligingsgroepen moeten toestaan Load Balancer IP-adres van 168.63.129.16, is dit testpoort) 
* Als een van de regels voor het verkeer wordt geblokkeerd, verwijderen en opnieuw configureren van deze regels voor het gegevensverkeer.  
* Als de virtuele machine nu gestart is om te reageren op de statuscontroles testen.

### <a name="cause-3-accessing-the-load-balancer-from-the-same-vm-and-network-interface"></a>3 oorzaak: Toegang tot de Load Balancer van dezelfde virtuele machine en netwerk-interface 

Als uw toepassing die wordt gehost in de back-end VM van een Load Balancer probeert te krijgen tot een andere toepassing die wordt gehost in de dezelfde back-end virtuele machine via de Interface met hetzelfde netwerk, is een niet-ondersteunde scenario en mislukt. 

**Resolutie** kunt u dit probleem op via een van de volgende manieren oplossen:
* Configureer afzonderlijke back-endpool VM's per toepassing. 
* De toepassing in twee NIC-VM's configureren, zodat elke toepassing een eigen netwerkinterface en de IP-adres is gebruikt. 

### <a name="cause-4-accessing-the-internal-load-balancer-vip-from-the-participating-load-balancer-backend-pool-vm"></a>4 oorzaak: Toegang tot de interne Load Balancer-VIP van de deelnemende Load Balancer back-endpool VM

Als een ILB VIP is geconfigureerd in een VNet en een van de deelnemer back-end virtuele machines probeert te openen van de interne Load Balancer-VIP, waardoor de fout. Dit scenario wordt niet ondersteund.
**Resolutie** toepassingsgateway evalueren of andere proxy's (bijvoorbeeld nginx of haproxy) voor de ondersteuning van dit soort scenario. Zie voor meer informatie over Application Gateway [overzicht van Application Gateway](../application-gateway/application-gateway-introduction.md)

## <a name="additional-network-captures"></a>Aanvullende netwerkopnamen
Als u besluit een ondersteuningsaanvraag te openen, verzamelt u de volgende informatie voor een snellere oplossing. Kies een eenmalige back-end VM om uit te voeren van de volgende tests:
- Gebruik psping om een van de back-end virtuele machines binnen het VNet te testen van het antwoord van de poort test (voorbeeld: psping om 10.0.0.4:3389) en noteer de resultaten. 
- TCPing van een van de back-end virtuele machines binnen het VNet gebruiken voor het testen van het antwoord van de poort test (voorbeeld: psping om 10.0.0.4:3389) en noteer de resultaten.
- Als er geen reactie wordt ontvangen in de deze pingtests, moet u een gelijktijdige Netsh-trace op de back-end virtuele machine en de VNet-test VM uitvoeren terwijl u psping om Voer vervolgens de Netsh-tracering stoppen. 
  
## <a name="next-steps"></a>Volgende stappen

Als de voorgaande stappen het probleem niet verhelpen, opent u een [ondersteunen ticket](https://azure.microsoft.com/support/options/).

