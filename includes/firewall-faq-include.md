---
title: bestand opnemen
description: bestand opnemen
services: firewall
author: vhorne
ms.service: ''
ms.topic: include
ms.date: 7/30/2018
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: e23579479c61810d651bebae7b486b53aaaf0d42
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39361467"
---
### <a name="what-is-azure-firewall"></a>Wat is Azure Firewall?

Azure Firewall is een beheerde, cloudgebaseerde netwerkbeveiligingsservice die uw Azure Virtual Network-resources beschermt. Het is een volledig stateful firewall-as-a-service met de ingebouwde hoge beschikbaarheid en cloudschaalbaarheid van de onbeperkte. U kunt beleid voor toepassings- en netwerkconnectiviteit centraal maken, afdwingen en registreren voor abonnementen en virtuele netwerken. Firewall van Azure is momenteel in openbare preview.

### <a name="what-capabilities-are-supported-in-the-azure-firewall-public-preview-release"></a>Welke mogelijkheden worden ondersteund in de openbare preview-versie van Azure-Firewall?  

* Stateful firewall als een Service
* Ingebouwde hoge beschikbaarheid met onbeperkte cloudschaalbaarheid
* Filteren op FQDN 
* Regels voor het filteren van netwerkverkeer
* Ondersteuning voor uitgaande SNAT
* Centraal maken, afdwingen en meld u beleid van toepassing en netwerk connectiviteit tussen Azure-abonnementen en VNETs
* Volledig geïntegreerd met Azure Monitor voor registratie en analyses 

### <a name="how-can-i-join-the-azure-firewall-public-preview"></a>Hoe kan ik deelnemen aan de openbare Preview van Azure-Firewall?

Firewall van Azure is momenteel een beheerde preview-versie die u kunt deelnemen aan met behulp van de Register-AzureRmProviderFeature PowerShell-opdracht, zoals wordt beschreven in de openbare Preview-documentatie van Azure Firewall.

### <a name="what-is-the-pricing-for-azure-firewall"></a>Wat zijn de prijzen voor de Firewall van Azure?

Firewall van Azure heeft een vaste prijs + variabele kosten. De prijzen hieronder zijn en deze worden verder korting van 50% tijdens de openbare preview.

* Vaste kosten: $1.25/firewall/hour
* Variabel tarief: $ 0,03/GB verwerkt door de firewall (inkomend of uitgaand verkeer).

### <a name="what-is-the-typical-deployment-model-for-azure-firewall"></a>Wat is het typische implementatiemodel voor de Firewall van Azure?

Hoewel het mogelijk is het implementeren van Azure-Firewall op een VNET, zou doorgaans klanten implementeren van Azure-Firewall in een centrale VNET en andere vnets koppelen aan deze in een Hub en Spoke-model. Vervolgens stelt u de standaardroute uit de gekoppelde VNETs verwijzen naar dit centrale Firewall VNET.

### <a name="how-can-i-install-the-azure-firewall"></a>Hoe kan ik de Azure-Firewall installeren?

Firewall van Azure kunt instellen via de Azure portal, PowerShell, REST-API of sjablonen. Zie [zelfstudie: implementeer en configureer de Firewall van Azure met behulp van de Azure-portal](../articles/firewall/tutorial-firewall-deploy-portal.md) voor stapsgewijze instructies.

### <a name="what-are-some-azure-firewall-concepts"></a>Wat zijn enkele concepten van de Firewall voor Azure?

Firewall van Azure biedt ondersteuning voor regels en regelverzamelingen. Verzameling van een regel is een reeks regels die de dezelfde volgorde en prioriteit deelt. Regelverzamelingen worden uitgevoerd in volgorde van prioriteit, netwerk-regelverzamelingen hogere prioriteit dan de regelverzamelingen van toepassing zijn, alle regels worden beëindigd.
Er zijn twee soorten regelverzamelingen:

* Regels voor Application: u kunt volledig gekwalificeerde domeinnamen (FQDN's) die kunnen worden benaderd vanaf een subnet configureren. 
* Regels voor: u kunt configureren met bron-adres, protocol, bestemmingspoort en doeladres regels. 

### <a name="does-azure-firewall-support-inbound-traffic-filtering"></a>Biedt Azure Firewall ondersteuning voor binnenkomend verkeer filteren?

Openbare preview van Azure Firewall ondersteunt alleen uitgaande filteren. Beveiliging voor niet-HTTP/S-protocollen Inkomend (ex: RDP, SSH, FTP) voorlopig is gepland voor Azure Firewall voor algemene beschikbaarheid.  
 
### <a name="what-logginganalytics-is-supported-by-the-azure-firewall"></a>Welke logboekregistratie en analyse wordt ondersteund door de Firewall van Azure?

Firewall van Azure is geïntegreerd met Azure Monitor voor het weergeven en analyseren van de Firewall-logboeken. Logboeken kunnen worden verzonden naar Log Analytics, Azure Storage of Event Hub. Ze kunnen worden geanalyseerd in Log Analytics of met verschillende hulpprogramma's zoals Excel en Power BI. Zie [zelfstudie: logboeken van de Firewall van Azure Monitor](../articles/firewall/tutorial-diagnostics.md) voor meer informatie.

### <a name="how-does-azure-firewall-work-relative-to-existing-like-nvas-in-the-marketplace"></a>Hoe werkt Azure Firewall ten opzichte van de bestaande zoals NVA's in de marketplace?

Firewall van Azure is een eenvoudige firewall-service die in bepaalde scenario's van klanten voorzien kan. Verwacht wordt dat u een combinatie van de NVA's van derden en de Firewall van Azure hebt. Samen beter is een prioriteit core.
 
### <a name="what-is-the-difference-between-application-gateway-waf-and-azure-firewall"></a>Wat is het verschil tussen de Application Gateway WAF- en firewallinstellingen van Azure?

De Web Application Firewall (WAF) is een functie van Application Gateway die gecentraliseerde binnenkomende beveiliging van uw webtoepassingen tegen algemene aanvallen en beveiligingsproblemen biedt. Firewall van Azure biedt uitgaande beveiliging op netwerkniveau voor alle poorten en protocollen en beveiliging op netwerkniveau voor toepassingen voor uitgaande HTTP/S. Binnenkomende beveiliging voor niet-HTTP/S-protocollen (bijvoorbeeld RDP, SSH, FTP) is voorlopig gepland voor Azure-Firewall algemene beschikbaarheid.

### <a name="what-is-the-difference-between-network-security-groups-nsg-and-azure-firewall"></a>Wat is het verschil tussen Netwerkbeveiligingsgroep groepen (NSG) en de Firewall van Azure?

De Firewall van de Azure-service vormt een aanvulling op Network Security Group functionaliteit en samen biedt betere ingrijpende netwerkbeveiliging. Nsg's gedistribueerde laag filteren van netwerkverkeer om te beperken van verkeer naar resources in virtuele netwerken in elk abonnement.  Firewall van Azure is een volledig stateful, gecentraliseerde netwerk firewall as-a-service, netwerk- en beveiliging op netwerkniveau biedt voor verschillende abonnementen en virtuele netwerken (VNets). 

### <a name="how-do-i-set-up-azure-firewall-with-my-service-endpoints"></a>Hoe stel ik Azure-Firewall in met de service-eindpunten?

Voor beveiligde toegang tot PaaS-services raden wij Service-eindpunten. Azure Firewall-klanten kunnen kiezen voor service-eindpunten in de Firewall van Azure-subnet in- en uitschakelen op het verbonden knooppunt VNETs voor profiteert van beide functies: beveiliging van de service-eindpunt en centrale logboekregistratie voor al het verkeer.

### <a name="what-are-the-known-service-limits"></a>Wat zijn de bekende servicebeperkingen?

* Firewall van Azure is een dynamische limiet van 1000 TB/firewall/maand. 
* Firewall van Azure die worden uitgevoerd in een centrale VNET is onderworpen aan beperkingen voor VNET-peering: maximum van 50 knooppunt VNETs.  
* Firewall van Azure werkt niet met wereldwijde peering, dus u moet ten minste één firewall implementatie per regio hebben.
* Azure ondersteunt 10k toepassing firewallregels en 10 k-netwerkregels.