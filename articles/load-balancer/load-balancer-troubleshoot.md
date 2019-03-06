---
title: Problemen met Azure Load Balancer oplossen
titlesuffix: Azure Load Balancer
description: Bekende problemen oplossen met Azure Load Balancer
services: load-balancer
documentationcenter: na
author: chadmath
manager: cshepard
ms.custom: seodoc18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/09/2018
ms.author: genli
ms.openlocfilehash: 26f60a6f1796b080df3294737ce93bfb43029bf1
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57439112"
---
# <a name="troubleshoot-azure-load-balancer"></a>Problemen met Azure Load Balancer oplossen

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Deze pagina bevat informatie over probleemoplossing voor veelgestelde vragen voor Azure Load Balancer. Wanneer de connectiviteit van de Load Balancer niet beschikbaar is, zijn de meest voorkomende problemen als volgt: 
- Virtuele machines achter de Load Balancer reageert niet op statuscontroles 
- Virtuele machines achter de Load Balancer reageert niet op het verkeer op de geconfigureerde poort

## <a name="symptom-vms-behind-the-load-balancer-are-not-responding-to-health-probes"></a>Probleem: Virtuele machines achter de Load Balancer reageert niet op statuscontroles
Voor de back-endservers om deel te nemen in de load balancer-set, moeten ze slagen voor de testcontrole. Zie voor meer informatie over statuscontroles [Understanding Load Balancer controleert](load-balancer-custom-probe-overview.md). 

De Load Balancer-back-end-groep virtuele machines reageert niet op de tests vanwege een van de volgende redenen: 
- Back-endpool VM voor Load Balancer is niet in orde 
- Load Balancer back-endpool-VM is niet op de testpoort luisteren 
- Firewall of een netwerkbeveiligingsgroep wordt geblokkeerd door de poort op de Load Balancer-back-end-groep virtuele machines 
- Andere configuraties in Load Balancer

### <a name="cause-1-load-balancer-backend-pool-vm-is-unhealthy"></a>1 oorzaak: Back-endpool VM voor Load Balancer is niet in orde 

**Validatie en oplossing**

U lost dit probleem, meld u aan bij de deelnemende virtuele machines, en controleer of de VM-status in orde is en kunnen reageren op **PsPing** of **TCPing** uit een andere virtuele machine in de groep. Als de virtuele machine beschadigd is of kan niet reageren op de test, moet u het probleem te verhelpen en ophalen van de virtuele machine naar een goede status voordat deze taakverdeling deelnemen kan.

### <a name="cause-2-load-balancer-backend-pool-vm-is-not-listening-on-the-probe-port"></a>2 oorzaak: Load Balancer back-endpool-VM is niet op de testpoort luisteren
Als de virtuele machine in orde is, maar niet op de test reageert, is een mogelijke reden zijn kan dat de testpoort niet geopend is op de deelnemende virtuele machine of de virtuele machine niet luisteren op poort.

**Validatie en oplossing**

1. Meld u aan bij de back-end van de virtuele machine. 
2. Open een opdrachtprompt en voer de volgende opdracht uit om te valideren, er is een toepassing die luistert op de testpoort:   
            netstat - an
3. Als de poortstatus van de niet wordt vermeld als **LISTENING**, de juiste poort configureren. 
4. Of Selecteer een andere poort, die wordt vermeld als **LISTENING**, en load balancer-configuratie dienovereenkomstig bijwerken.              

### <a name="cause-3-firewall-or-a-network-security-group-is-blocking-the-port-on-the-load-balancer-backend-pool-vms"></a>3 oorzaak: Firewall of een netwerkbeveiligingsgroep wordt geblokkeerd door de poort op de load balancer back-endpool VM 's  
Als de testpoort is geblokkeerd door de firewall op de virtuele machine of een of meer beveiligingsgroepen die zijn geconfigureerd op het subnet of op de VM-netwerk, de test om te bereiken, de poort, is niet toegestaan, is de virtuele machine kan niet reageren op de statustest.          

**Validatie en oplossing**

* Als de firewall is ingeschakeld, controleert u of deze is geconfigureerd voor de testpoort toestaan. Als dat niet het geval is, moet u de firewall verkeer op de testpoort toestaan en test opnieuw configureren. 
* Controleer of het binnenkomende of uitgaande verkeer op de testpoort storing heeft in de lijst van netwerkbeveiligingsgroepen. 
* Controleer ook of als een **Alles weigeren** netwerkbeveiligingsgroepen regel op de NIC van de virtuele machine of het subnet dat een hogere prioriteit dan de standaardregel waarmee LB tests & verkeer (netwerkbeveiligingsgroepen moeten toestaan Load Balancer IP-adres van 168.63.129.16). 
* Als een van deze regels de test-verkeer blokkeert zijn, verwijderen en opnieuw configureren van de regels voor de test-verkeer.  
* Test of de virtuele machine heeft nu gestart voor het reageren op de statuscontroles. 

### <a name="cause-4-other-misconfigurations-in-load-balancer"></a>4 oorzaak: Andere configuraties in Load Balancer
Als de voorgaande oorzaken lijken te worden gevalideerd en omgezet correct en de back-end van de virtuele machine nog steeds niet op de statustest en vervolgens handmatig reageert test de verbinding en verzamelen van sommige traceringen voor meer informatie over de verbinding.

**Validatie en oplossing**

* Gebruik **Psping** uit een van de andere VM's binnen het VNet voor het testen van het antwoord van de poort test (voorbeeld:.\psping.exe -t 10.0.0.4:3389) en resultaten opnemen. 
* Gebruik **TCPing** uit een van de andere VM's binnen het VNet voor het testen van het antwoord van de poort test (voorbeeld:.\tcping.exe 10.0.0.4 3389) en resultaten opnemen. 
* Als er geen reactie wordt ontvangen in deze ping-tests, klikt u vervolgens
    - Voer een gelijktijdige Netsh-tracering op de back-endpool voor doel-VM en een andere test-VM in hetzelfde VNet. Nu, voert u een test PsPing gedurende een bepaalde periode, sommige Netwerktraceringen verzamelen en stop vervolgens de test. 
    - Het vastleggen van netwerk analyseren en te zien of er zowel inkomende als uitgaande pakketten met betrekking tot de ping-query. 
        - Als er geen inkomende pakketten zijn waargenomen op de back-endpool-VM, is er mogelijk een netwerkbeveiligingsgroepen of UDR foutieve configuratie van het verkeer blokkeren. 
        - Als er geen uitgaande pakketten zijn waargenomen op de back-endpool-VM, moet de virtuele machine moet worden gecontroleerd voor niet-gerelateerde problemen (bijvoorbeeld de toepassing de testpoort blokkeren). 
    - Controleer of als de testpakketten wordt gedwongen naar een andere bestemming (mogelijk via de UDR-instellingen) voordat de load balancer wordt bereikt. Hierdoor kan het verkeer naar de back-end van de virtuele machine nooit te bereiken. 
* De test-type (bijvoorbeeld HTTP naar TCP) wijzigen en de bijbehorende poort configureren voor netwerkbeveiligingsgroepen ACL's en firewall om te controleren of het probleem met de configuratie van test-antwoord is. Zie voor meer informatie over de configuratie van de health test [Testconfiguratie health Endpoint Load Balancing](https://blogs.msdn.microsoft.com/mast/2016/01/26/endpoint-load-balancing-heath-probe-configuration-details/).

## <a name="symptom-vms-behind-load-balancer-are-not-responding-to-traffic-on-the-configured-data-port"></a>Probleem: Virtuele machines achter Load Balancer reageren niet op verkeer op de geconfigureerde poort

Als een back-endpool VM wordt vermeld als in orde is en reageert op de statuscontroles, maar nog steeds geen deel uitmaakt van de Load Balancing of reageert niet op het gegevensverkeer, kan dit worden veroorzaakt door een van de volgende redenen: 
* Load Balancer-back-end-pool die VM is niet op de gegevenspoort luisteren 
* Netwerkbeveiligingsgroep wordt geblokkeerd door de poort op de Load Balancer back-endpool VM  
* Toegang tot de Load Balancer van dezelfde virtuele machine en NIC 
* Toegang tot de internetgerichte Load Balancer-frontend van de deelnemende Load Balancer back-endpool VM 

### <a name="cause-1-load-balancer-backend-pool-vm-is-not-listening-on-the-data-port"></a>1 oorzaak: Load Balancer back-endpool-VM is niet op de gegevenspoort luisteren 
Als een virtuele machine niet op het gegevensverkeer reageert, kan het zijn omdat de doelpoort niet geopend op de deelnemende virtuele machine is, of de virtuele machine niet bij die poort luistert. 

**Validatie en oplossing**

1. Meld u aan bij de back-end van de virtuele machine. 
2. Open een opdrachtprompt en voer de volgende opdracht uit om te valideren, er is een toepassing die luistert op de gegevenspoort:  netstat - an 
3. Als de poort niet wordt vermeld met de status 'Luistert', de juiste luisterpoort configureren 
4. Als de poort is gemarkeerd als Listening, controleert u de doeltoepassing op deze poort voor mogelijke problemen. 

### <a name="cause-2-network-security-group-is-blocking-the-port-on-the-load-balancer-backend-pool-vm"></a>2 oorzaak: Netwerkbeveiligingsgroep wordt geblokkeerd door de poort op de Load Balancer back-endpool VM  

Als een of meer netwerkbeveiligingsgroepen op het subnet of op de virtuele machine geconfigureerd, wordt geblokkeerd door de bron-IP of de poort, dan is de virtuele machine kan niet reageren.

* Lijst met de netwerkbeveiligingsgroepen die zijn geconfigureerd op de back-end van de virtuele machine. Zie voor meer informatie, [netwerkbeveiligingsgroepen beheren](../virtual-network/manage-network-security-group.md).
* Controleer in de lijst van netwerkbeveiligingsgroepen als:
    - het binnenkomende of uitgaande verkeer op de gegevenspoort heeft storingen. 
    - een **Alles weigeren** regel voor netwerkbeveiligingsgroep op de NIC van de virtuele machine of het subnet dat een hogere prioriteit die de standaardregel waarmee de Load Balancer-tests heeft het netwerk en het verkeer (netwerkbeveiligingsgroepen moeten toestaan Load Balancer IP-adres van 168.63.129.16, dat is testpoort) 
* Als een van de regels voor het verkeer wordt geblokkeerd, verwijderen en opnieuw configureren van deze regels om de gegevensverkeer te staan.  
* Testen, als u de virtuele machine nu om te reageren op de statuscontroles is gestart.

### <a name="cause-3-accessing-the-load-balancer-from-the-same-vm-and-network-interface"></a>3 oorzaak: Toegang tot de Load Balancer vanuit één virtuele machine en het netwerk 

Als uw toepassing die wordt gehost in de back-end van de virtuele machine van een Load Balancer probeert te krijgen tot een andere toepassing die wordt gehost in de dezelfde back-end virtuele machine via de Interface met hetzelfde netwerk, een niet-ondersteund scenario en mislukken. 

**Resolutie** kunt u dit probleem op via een van de volgende manieren oplossen:
* Afzonderlijke back-endpool, virtuele machines per toepassing configureren. 
* De toepassing in twee NIC-VM's configureren, zodat elke toepassing heeft een eigen netwerkinterface en IP-adres gebruikt. 

### <a name="cause-4-accessing-the-internal-load-balancer-frontend-from-the-participating-load-balancer-backend-pool-vm"></a>4 oorzaak: Toegang tot de interne Load Balancer-frontend van de deelnemende Load Balancer back-endpool VM

Als een interne Load Balancer is geconfigureerd in een VNet en een van de cursist back-end-VM's probeert te krijgen tot de interne Load Balancer-frontend, worden de fouten kunnen optreden wanneer de stroom is toegewezen aan de oorspronkelijke virtuele machine. In dit scenario wordt niet ondersteund. Beoordeling [beperkingen](load-balancer-overview.md#limitations) voor gedetailleerde informatie.

**Resolutie** er zijn verschillende manieren voor het deblokkeren van dit scenario, inclusief het gebruik van een proxy. Application Gateway of andere 3e partij proxy's (bijvoorbeeld nginx of haproxy) evalueren. Zie voor meer informatie over Application Gateway [overzicht van Application Gateway](../application-gateway/application-gateway-introduction.md)

## <a name="additional-network-captures"></a>Aanvullende netwerkopnamen
Als u besluit een ondersteuningsaanvraag openen, verzamel dan de volgende informatie voor een snellere oplossing. Kies een eenmalige back-end VM het uitvoeren van de volgende tests:
- Psping uit een van de back-end-VM's binnen het VNet gebruiken voor het testen van het antwoord van de poort test (voorbeeld: psping 10.0.0.4:3389) en resultaten opnemen. 
- Als er geen reactie wordt ontvangen in deze pingtests, moet u een gelijktijdige Netsh-tracering uitvoeren op de back-end van de virtuele machine en de VNet-test-VM terwijl u PsPing uitvoert en stop vervolgens de Netsh-tracering. 
  
## <a name="next-steps"></a>Volgende stappen

Als de voorgaande stappen het probleem niet verhelpen, opent u een [ondersteuningsticket](https://azure.microsoft.com/support/options/).

