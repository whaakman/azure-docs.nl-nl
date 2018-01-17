---
title: Azure datacenter integratie Stack - eindpunten publiceren
description: Informatie over het publiceren van Azure-Stack-eindpunten in uw datacenter
services: azure-stack
author: troettinger
ms.service: azure-stack
ms.topic: article
ms.date: 01/16/2018
ms.author: victorh
keywords: 
ms.openlocfilehash: 1cc74cb2214918d6bfd0c0827cf5d9832b84f317
ms.sourcegitcommit: 5108f637c457a276fffcf2b8b332a67774b05981
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="azure-stack-datacenter-integration---publish-endpoints"></a>Azure datacenter integratie Stack - eindpunten publiceren

*Van toepassing op: Azure Stack geïntegreerd systemen*

Azure Stack stelt u verschillende eindpunten (VIP's - virtuele IP-adressen) voor de functies van de infrastructuur. Deze VIP's worden toegewezen vanuit de openbare IP-adresgroep. Elke VIP is beveiligd met een toegangsbeheerlijst (ACL) in de software gedefinieerde netwerklaag. ACL's worden ook gebruikt voor de fysieke switches (blijven en BMC) voor het verder beperken van de oplossing. Een DNS-vermelding wordt voor elk eindpunt dat in de externe DNS-zone die is opgegeven tijdens de implementatie gemaakt.


Het volgende architecturaal diagram toont de verschillende lagen en ACL's:

![Architectuurdiagram](media/azure-stack-integrate-endpoints/Integrate-Endpoints-01.png)

## <a name="ports-and-protocols-inbound"></a>Poorten en protocollen (inkomend)

De infrastructuur VIP's die vereist voor publicatie Stack Azure-eindpunten met externe netwerken zijn worden vermeld in de volgende tabel. De lijst ziet u alle eindpunt, de vereiste poort en protocol. Eindpunten die vereist zijn voor de extra resourceproviders, zoals de resourceprovider voor SQL en andere, vallen in de documentatie van bepaalde resource provider-implementatie.

Interne infrastructuur VIP's worden niet weergegeven omdat ze niet vereist voor publishing Azure-Stack.

> [!NOTE]
> Gebruiker VIP's zijn dynamisch, gedefinieerd door de gebruikers zelf geen controle door de Azure-Stack-operator.


|Eindpunt (VIP)|DNS-hostnaam een record|Protocol|Poorten|
|---------|---------|---------|---------|
|AD FS|Adfs.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Portal (beheerder)|Adminportal.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>12495<br>12499<br>12646<br>12647<br>12648<br>12649<br>12650<br>13001<br>13003<br>13010<br>13011<br>13020<br>13021<br>13026<br>30015|
|Azure Resource Manager (beheerder)|Adminmanagement.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>30024|
|Portal (gebruiker)|Portal.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>12495<br>12649<br>13001<br>13010<br>13011<br>13020<br>13021<br>30015<br>13003|
|Azure Resource Manager (gebruiker)|Management.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>30024|
|Graph|Graph.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Lijst met ingetrokken certificaten|Crl.*&lt;region>.&lt;fqdn>*|HTTP|80|
|DNS|&#42;.*&lt;region>.&lt;fqdn>*|TCP EN UDP|53|
|Sleutelkluis (gebruiker)|*.vault.*&lt;region>.&lt;fqdn>*|TCP|443|
|Sleutelkluis (beheerder)|&#42;.adminvault.*&lt;region>.&lt;fqdn>*|TCP|443|
|Opslagwachtrij|&#42;.queue.*&lt;region>.&lt;fqdn>*|HTTP<br>HTTPS|80<br>443|
|Table Storage|&#42;.table.*&lt;region>.&lt;fqdn>*|HTTP<br>HTTPS|80<br>443|
|Storage Blob|&#42;.blob.*&lt;region>.&lt;fqdn>*|HTTP<br>HTTPS|80<br>443|

## <a name="ports-and-urls-outbound"></a>Poorten en URL's (uitgaand)

Azure-Stack ondersteunt alleen transparentproxy servers. In een implementatie waarbij een transparentproxy uplinks met een traditioneel proxyserver, moet u toestaan de volgende poorten en URL's voor uitgaande communicatie:


|Doel|URL|Protocol|Poorten|
|---------|---------|---------|---------|
|Identiteit|login.windows.net<br>login.microsoftonline.com<br>graph.windows.net|HTTP<br>HTTPS|80<br>443|
|Marketplace-syndicatie|https://management.azure.com<br>https://&#42;.blob.core.windows.net<br>https://*.azureedge.net<br>https://&#42;.microsoftazurestack.com|HTTPS|443|
|Patch & bijwerken|https://&#42;.azureedge.net|HTTPS|443|
|Registratie|https://management.azure.com|HTTPS|443|
|Gebruik|https://&#42;.microsoftazurestack.com<br>https://*.trafficmanager.com|HTTPS|443|

## <a name="firewall-publishing"></a>Publicatie van de firewall

De poorten die worden vermeld in de vorige sectie zijn van toepassing op binnenkomende communicatie bij het publiceren van Azure Stack Services via een firewall.

Het is raadzaam dat u een firewall-apparaat gebruiken voor het beveiligde Azure-Stack. Het is echter niet strikt vereist. Hoewel firewalls bij items zoals gedistribueerde denial-of-service (DDOS) aanvallen en inhoudscontrole helpen kunnen, kunnen ze ook een knelpunt doorvoer voor Azure storage-services zoals blobs, tabellen en wachtrijen geworden.

Op basis van het model identiteit (Azure AD of AD FS), mogelijk of kan niet worden vereist voor het publiceren van het AD FS-eindpunt. Als een niet-verbonden implementatiemodus wordt gebruikt, moet u de AD FS-eindpunt publiceren. (Zie het onderwerp Datacenter integratie identiteit voor meer informatie.)

De Azure Resource Manager (beheerder), beheerdersportal en Sleutelkluis (beheerder) eindpunten vereisen per se geen externe publiceren. Dit is afhankelijk van het scenario. Bijvoorbeeld, als een serviceprovider mogelijk wilt u de kwetsbaarheid te beperken en alleen Azure-Stack van beheren in uw netwerk en niet vanaf het Internet.

Het externe netwerk kan het bestaande bedrijfsnetwerk zijn voor uw organisatie. In een dergelijk scenario moet u deze eindpunten voor het gebruik van Azure-Stack van het bedrijfsnetwerk publiceren.

## <a name="edge-firewall-scenario"></a>Edge-firewall-scenario

Azure-Stack wordt geïmplementeerd achter de edge-router (geleverd door de Internetprovider) rechtstreeks in een edge-implementatie met of zonder een firewall plaatsen.

![Architectuurdiagram van de implementatie van een Azure-Stack-rand](media/azure-stack-integrate-endpoints/Integrate-Endpoints-02.png)

Normaal gesproken zijn openbaar routeerbare IP-adressen opgegeven voor het openbare VIP-adresgroep tijdens de implementatie in een implementatie van de rand. Dit scenario kan een gebruiker om de ervaring voor de volledige zelf gecontroleerde cloud zoals in een openbare cloud zoals Azure.

### <a name="using-nat"></a>Met behulp van NAT

Hoewel niet aanbevolen vanwege de overhead, kunt u Network Address Translation (NAT) gebruiken voor publiceren-eindpunten. Hiervoor moet een NAT-regel per gebruiker VIP met alle poorten die een gebruiker kan gebruiken voor het eindpunt publiceren die volledig door gebruikers worden beheerd.

Andere overweging is Azure biedt geen ondersteuning voor instellen van een VPN-tunnel naar een eindpunt met behulp van NAT in een scenario met hybride cloud met Azure.

## <a name="enterpriseintranetperimeter-network-firewall-scenario"></a>Intranet-Enterprise/perimeternetwerk bevinden firewall scenario

In de implementatie van een intranet-enterprise/perimeter Azure Stack geïmplementeerd buiten een tweede firewall, die meestal deel uitmaakt van een perimeternetwerk (ook wel een DMZ genoemd).

![Azure Stack firewall scenario](media/azure-stack-integrate-endpoints/Integrate-Endpoints-03.png)

Als openbaar routeerbare IP-adressen zijn opgegeven voor het openbare VIP-adresgroep van Azure-Stack, wordt deze adressen logisch deel uitmaken van het perimeternetwerk en regels voor publicatie op de primaire firewall vereisen.

### <a name="using-nat"></a>Met behulp van NAT

Als niet-openbaar routeerbare IP-adressen worden gebruikt voor het openbare VIP-adresgroep van Azure-Stack, wordt op de secundaire firewall NAT gebruikt voor het publiceren van Azure-Stack-eindpunten. In dit scenario moet u de regels voor publicatie op de primaire firewall buiten de rand, en op de secundaire firewall configureren. Houd rekening met de volgende punten als u wilt NAT gebruiken:

- NAT voegt overhead bij het beheren van firewallregels omdat gebruikers hun eigen eindpunten en hun eigen regels voor publicatie in de software gedefinieerde netwerken (SDN)-stack beheren. Gebruikers moeten contact opnemen met de Azure-Stack-operator hun VIP's gepubliceerd ophalen en bijwerken van de lijst met poorten.
- Tijdens het gebruik van NAT beperkt de gebruikerservaring, geeft deze volledig beheer aan de operator over publicatieaanvragen afhandelt.
- Voor hybride cloud scenario's met Azure, kunt u Azure biedt geen ondersteuning voor de instelling van een VPN-tunnel naar een eindpunt met NAT bevinden.


## <a name="next-steps"></a>Volgende stappen

[Stack datacenter integratie van Azure - beveiliging](azure-stack-integrate-security.md)