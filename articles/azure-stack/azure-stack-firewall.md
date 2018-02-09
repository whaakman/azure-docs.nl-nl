---
title: "Azure Stack firewall plannen voor Azure-Stack geïntegreerd systemen | Microsoft Docs"
description: Beschrijft de Azure-Stack firewall overwegingen voor implementaties met meerdere knooppunten verbonden met een Azure-Stack Azure.
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: 919618c0779d47f0add02d5e7d3ab9ab4b5bdd10
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="azure-stack-firewall-integration"></a>Integratie van Azure Stack-firewall
Het verdient aanbeveling dat u een firewall-apparaat gebruiken voor het beveiligde Azure-Stack. Hoewel firewalls bij zaken als gedistribueerde denial-of-service (DDOS) aanvallen, inbraakdetectie en inhoudscontrole helpen kunnen, kunnen ze ook een knelpunt doorvoer voor Azure storage-services zoals blobs, tabellen en wachtrijen geworden.

Op basis van het identiteitsmodel van Azure Active Directory (Azure AD) of Windows Server Active Directory Federation Services (AD FS), mogelijk moet u voor het publiceren van het AD FS-eindpunt. Als een niet-verbonden implementatiemodus wordt gebruikt, moet u de AD FS-eindpunt publiceren. Zie voor meer informatie de [datacenter integratie identiteit artikel](azure-stack-integrate-identity.md).

De Azure Resource Manager (beheerder), beheerdersportal en Sleutelkluis (beheerder) eindpunten vereisen per se geen externe publiceren. Bijvoorbeeld, als een serviceprovider mogelijk wilt u de kwetsbaarheid te beperken en alleen Azure-Stack van beheren in uw netwerk en niet vanaf het internet.

Voor ondernemingen, kan het externe netwerk het bestaande bedrijfsnetwerk zijn. In een dergelijk scenario moet u deze eindpunten voor het gebruik van Azure-Stack van het bedrijfsnetwerk publiceren.

### <a name="network-address-translation"></a>Network Address Translation
NAT (Network Address Translation) is de aanbevolen methode om toe te staan voor de implementatie van virtuele machine (DVM) voor toegang tot externe bronnen en het internet tijdens de implementatie, evenals de Emergency Recovery Console (ERCS) virtuele machines of bevoegde eindpunt (PEP) tijdens registratie en het oplossen van problemen.

NAT kan ook een alternatief voor het openbare IP-adressen op het externe netwerk of openbare VIP's worden. Het is echter niet aanbevolen omdat deze wordt beperkt van de gebruikerservaring van de tenant en verhoogt de complexiteit. De twee opties zijn een 1:1-NAT waarvoor nog steeds vereist een openbaar IP-adres per gebruiker IP-adres op de groep op, of veel: 1 NAT waarvoor een NAT-regel per gebruiker VIP die bevat koppelingen naar alle poorten die een gebruiker kan gebruiken.

Enkele van de nadelen van het gebruik van de NAT voor openbare VIP zijn:
- NAT voegt overhead bij het beheren van firewallregels omdat gebruikers hun eigen eindpunten en hun eigen regels voor publicatie in de software gedefinieerde netwerken (SDN)-stack beheren. Gebruikers moeten contact opnemen met de Azure-Stack-operator hun VIP's gepubliceerd ophalen en bijwerken van de lijst met poorten.
- Tijdens het gebruik van NAT beperkt de gebruikerservaring, geeft deze volledig beheer aan de operator over publicatieaanvragen afhandelt.
- Voor hybride cloud scenario's met Azure, kunt u Azure biedt geen ondersteuning voor de instelling van een VPN-tunnel naar een eindpunt met NAT bevinden.

### <a name="ssl-decryption"></a>SSL-ontsleuteling
Het verdient aanbeveling momenteel SSL ontsleuteling op alle Azure-Stack-verkeer uitschakelen. Als dit wordt ondersteund in toekomstige updates, worden richtlijnen gegeven die over het inschakelen van SSL-ontsleuteling voor Azure-Stack.

## <a name="edge-firewall-scenario"></a>Edge-firewall-scenario
In de implementatie van een edge Azure Stack geïmplementeerd rechtstreeks achter de router van de rand of de firewall. In deze scenario's wordt ondersteund voor de firewall moet boven de rand of als het apparaat van de rand fungeert als deze gelijk Cost Multi Path (ECMP) met BGP of statische routering ondersteunt.

Normaal gesproken zijn openbaar routeerbare IP-adressen opgegeven voor het openbare VIP-adresgroep van het externe netwerk tijdens de implementatie. In een edge-scenario, is het niet raadzaam openbaar routeerbare IP-adressen in een ander netwerk gebruiken om beveiligingsredenen. Dit scenario kan een gebruiker om de ervaring voor de volledige zelf gecontroleerde cloud zoals in een openbare cloud zoals Azure.  

![Voorbeeld van Azure Stack edge firewall](.\media\azure-stack-firewall\edge-firewall-scenario.png)

## <a name="enterprise-intranet-or-perimeter-network-firewall-scenario"></a>Intranet- of perimeternetwerk netwerk firewall bedrijfsscenario
In het intranet of een perimeternetwerk implementatie in een onderneming, Azure Stack geïmplementeerd op een firewall multi ingedeeld of tussen de edge-firewall en de interne, zakelijke netwerkfirewall. Het verkeer wordt dan gedistribueerd tussen de veilige, perimeternetwerk (of DMZ) en niet-beveiligde zones als die hieronder worden beschreven:

- **Beveiligde zone**: dit is het interne netwerk die gebruikmaakt van de interne of zakelijke routeerbare IP-adressen. Beveiligd netwerk kan worden verdeeld, uitgaande toegang tot internet via een NAT hebben op de Firewall en meestal toegankelijk is vanaf een willekeurige plaats in uw datacentrum via het interne netwerk. Alle Azure-Stack-netwerken moeten zich bevinden in de beveiligde zone, met uitzondering van het externe netwerk openbare VIP-adresgroep.
- **Perimeter zone**. Het perimeternetwerk is waar externe of internetgericht toepassingen zoals webservers worden doorgaans geïmplementeerd. Dit wordt meestal bewaakt door een firewall tegen aanvallen zoals DDoS en inbraakdetectie (hacken) terwijl nog steeds opgegeven binnenkomend verkeer vanuit het internet. Alleen externe netwerk openbare vaste IP-adresgroep van Azure-Stack moet zich bevinden in de DMZ zone.
- **Niet-beveiligde zone**. Dit is het externe netwerk, het internet. Deze **is niet** aanbevolen voor het implementeren van Azure-Stack in de niet-beveiligde zone.

![Voorbeeld van Azure Stack perimeter netwerk](.\media\azure-stack-firewall\perimeter-network-scenario.png)

## <a name="learn-more"></a>Meer informatie
Meer informatie over [poorten en protocollen die worden gebruikt door Azure Stack eindpunten](azure-stack-integrate-endpoints.md).

## <a name="next-steps"></a>Volgende stappen
[Azure-Stack PKI-vereisten](azure-stack-pki-certs.md)

