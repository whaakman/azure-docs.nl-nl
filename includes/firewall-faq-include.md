---
title: bestand opnemen
description: bestand opnemen
services: firewall
author: vhorne
ms.service: ''
ms.topic: include
ms.date: 8/13/2018
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: a63a12658bd0a4b4d018d51824af9814691a3cbf
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2018
ms.locfileid: "40183333"
---
### <a name="what-is-azure-firewall"></a>Wat is Azure Firewall?

Azure Firewall is een beheerde, cloudgebaseerde netwerkbeveiligingsservice die uw Azure Virtual Network-resources beschermt. Het is een volledig stateful firewall-as-a-service met de ingebouwde hoge beschikbaarheid en cloudschaalbaarheid van de onbeperkte. U kunt beleid voor toepassings- en netwerkconnectiviteit centraal maken, afdwingen en registreren voor abonnementen en virtuele netwerken. Firewall van Azure is momenteel in openbare preview.

### <a name="which-capabilities-are-supported-in-the-azure-firewall-public-preview-release"></a>Welke mogelijkheden worden ondersteund in de openbare preview-versie van Azure-Firewall?  

* Stateful firewall als een service
* Ingebouwde hoge beschikbaarheid met onbeperkte cloudschaalbaarheid
* Filteren op FQDN 
* Regels voor het filteren van netwerkverkeer
* Ondersteuning voor uitgaande SNAT
* De mogelijkheid voor het centraal maken, afdwingen en meld u beleid van toepassing en netwerk connectiviteit voor Azure-abonnementen en virtuele netwerken
* Volledige integratie met Azure Monitor voor logboekregistratie en analyse 

### <a name="how-can-i-join-the-azure-firewall-public-preview"></a>Hoe kan ik deelnemen aan de openbare Preview van Azure-Firewall?

Firewall van Azure is momenteel een beheerde preview-versie die u kunt deelnemen aan met behulp van de Register-AzureRmProviderFeature PowerShell-opdracht. Met deze opdracht wordt uitgelegd in de documentatie van de Firewall van Azure preview-versie.

### <a name="what-is-the-pricing-for-azure-firewall"></a>Wat zijn de prijzen voor de Firewall van Azure?

Firewall van Azure heeft een vaste en variabele kosten. De prijzen zijn als volgt en meer met 50% korting op tijdens de openbare preview.

* Vaste kosten: $1.25/firewall/hour
* Variabel tarief: $ 0,03/GB verwerkt door de firewall (inkomend of uitgaand verkeer)

### <a name="what-is-the-typical-deployment-model-for-azure-firewall"></a>Wat is het typische implementatiemodel voor de Firewall van Azure?

U kunt de Firewall van Azure implementeren op een virtueel netwerk, maar klanten doorgaans implementeren op een centrale virtueel netwerk en andere virtuele netwerken toe in een hub en spoke-model. Vervolgens stelt u de standaardroute uit de gekoppelde virtuele netwerken om te verwijzen naar dit centrale firewall virtuele netwerk.

### <a name="how-can-i-install-the-azure-firewall"></a>Hoe kan ik de Azure-Firewall installeren?

U kunt Azure Firewall instellen met behulp van de Azure portal, PowerShell, REST-API of met behulp van sjablonen. Zie [zelfstudie: implementeer en configureer de Firewall van Azure met behulp van de Azure-portal](../articles/firewall/tutorial-firewall-deploy-portal.md) voor stapsgewijze instructies.

### <a name="what-are-some-azure-firewall-concepts"></a>Wat zijn enkele concepten Firewall van Azure?

Firewall van Azure biedt ondersteuning voor regels en regelverzamelingen. Verzameling van een regel is een reeks regels die de dezelfde volgorde en prioriteit delen. Regelverzamelingen worden uitgevoerd in de volgorde van hun prioriteit. Netwerk regelverzamelingen hogere prioriteit dan de regelverzamelingen van toepassing zijn, en alle regels worden beëindigd.

Er zijn twee soorten regelverzamelingen:

* *Regels voor Application*: Hiermee kunt u de volledig gekwalificeerde domeinnamen (FQDN's) die kunnen worden benaderd vanaf een subnet configureren. 
* *Regels voor*: Hiermee kunt u regels configureren die bronadressen, protocollen, doelpoorten en doeladressen bevatten. 

### <a name="does-azure-firewall-support-inbound-traffic-filtering"></a>Biedt Azure Firewall ondersteuning voor binnenkomend verkeer filteren?

Openbare preview van Azure Firewall ondersteunt alleen uitgaande filteren. Binnenkomende beveiliging voor niet-HTTP/S-protocollen (bijvoorbeeld RDP, SSH of FTP) is voorlopig gepland voor Azure-Firewall algemene beschikbaarheid.  
 
### <a name="which-logging-and-analytics-services-are-supported-by-the-azure-firewall"></a>Welke services logboekregistratie en analyse worden ondersteund door de Firewall van Azure?

Firewall van Azure is geïntegreerd met Azure Monitor voor het weergeven en analyseren van de firewall-Logboeken. Logboeken kunnen worden verzonden naar Log Analytics, Azure Storage of Event Hubs. Ze kunnen worden geanalyseerd in Log Analytics of met verschillende hulpprogramma's zoals Excel en Power BI. Zie voor meer informatie, [zelfstudie: logboeken van de Firewall van Azure Monitor](../articles/firewall/tutorial-diagnostics.md).

### <a name="how-does-azure-firewall-work-differently-from-existing-services-such-as-nvas-in-the-marketplace"></a>Hoe Azure Firewall werkt anders van bestaande services zoals NVA's in de marketplace?

Firewall van Azure is een eenvoudige firewall-service die in bepaalde scenario's van klanten voorzien kan. Verwacht wordt dat u een combinatie van de NVA's van derden en de Firewall van Azure hebt. Samen beter is een prioriteit core.
 
### <a name="what-is-the-difference-between-application-gateway-waf-and-azure-firewall"></a>Wat is het verschil tussen de Application Gateway WAF- en firewallinstellingen van Azure?

De Web Application Firewall (WAF) is een functie van Application Gateway die gecentraliseerde binnenkomende beveiliging van uw webtoepassingen tegen algemene aanvallen en beveiligingsproblemen biedt. Firewall van Azure biedt uitgaande op netwerkniveau-beveiliging voor alle poorten en protocollen en de beveiliging op toepassingsniveau voor uitgaande HTTP/S. Binnenkomende beveiliging voor niet-HTTP/S-protocollen (bijvoorbeeld RDP, SSH, FTP) is voorlopig gepland voor Azure-Firewall algemene beschikbaarheid.

### <a name="what-is-the-difference-between-network-security-groups-nsgs-and-azure-firewall"></a>Wat is het verschil tussen Netwerkbeveiligingsgroepen (nsg's) en de Firewall van Azure?

De Firewall van de Azure-service is een aanvulling op network security group functionaliteit. Samen bieden ze beter 'verdediging in de diepte' netwerkbeveiliging. Netwerkbeveiligingsgroepen bevatten gedistribueerde laag filteren van netwerkverkeer om te beperken van verkeer naar resources in virtuele netwerken in elk abonnement. Firewall van Azure is een volledig stateful, gecentraliseerde netwerk firewall as-a-service, waarmee de netwerk - en -beveiliging op toepassingsniveau in verschillende abonnementen en virtuele netwerken. 

### <a name="how-do-i-set-up-azure-firewall-with-my-service-endpoints"></a>Hoe stel ik Azure-Firewall in met de service-eindpunten?

Voor beveiligde toegang tot PaaS-services raden wij service-eindpunten. U kunt de service-eindpunten in de Firewall van Azure-subnet inschakelen en uitschakelen verbonden spoke-netwerken. Op deze manier die u profiteren van functies, service-eindpunt beveiligings- en centrale logboekregistratie voor al het verkeer.

### <a name="how-can-i-stop-and-start-azure-firewall"></a>Hoe kan ik stoppen en starten van de Firewall van Azure?

U kunt Azure PowerShell gebruiken *toewijzing* en *toewijzen* methoden.

Bijvoorbeeld:

```azurepowershell
# Stop an exisitng firewall

$azfw = Get-AzureRmFirewall -Name "FW Name” -ResourceGroupName "RG Name"
$azfw.Deallocate()
Set-AzureRmFirewall -AzureFirewall $azfw
```

```azurepowershell
#Start a firewall

$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName "RG Name" -Name "VNet Name"
$publicip = Get-AzureRmPublicIpAddress -Name "Public IP Name" -ResourceGroupName " RG Name"
$azfw.Allocate($vnet,$publicip)
Set-AzureRmFirewall -AzureFirewall $azfw
```

### <a name="what-are-the-known-service-limits"></a>Wat zijn de bekende servicebeperkingen?

* Firewall van Azure is een dynamische limiet van 1000 TB per firewall per maand. 
* Een exemplaar van Azure-Firewall die wordt uitgevoerd in een centrale virtueel netwerk heeft virtuele netwerkpeering beperkingen, met een maximum van 50 virtuele spoke-netwerken.  
* Firewall van Azure werkt niet met wereldwijde peering, dus u moet ten minste één firewall implementatie per regio hebben.
* Firewall van Azure biedt ondersteuning voor 10 k toepassing regels en 10 k-netwerkregels.